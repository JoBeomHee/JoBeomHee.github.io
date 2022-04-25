---
title: Python gRPC RPC 패턴
author: BeomBeomJoJo
date: 2022-03-30 12:33:00 +0800
date: 2022-03-30 12:33:00 +0800
categories: [Python, gRPC]
tags: [Python, gRPC]
math: true
mermaid: true
---

{% include adsense.html %}

## **소개**
* gRPC 패턴은 크게 4가지가 있습니다.
* 단일 RPC, 서버 스트리밍, 클라이언트 스트리밍, 양방향 스트리밍 크게 4가지의 기본 통신 패턴이 있습니다.
* 이중 첫 번째 패턴인 **단일 RPC** 패턴에 대하여 학습 진행합니다.

<br/>

## **단순 RPC(단일 RPC)**
* 단순 RPC에서는 클라이언트가 서버의 원격 기능을 호출하고자 단일 요청을 서버로 보내고 상태에 대한 세부 정보 및 후행 메타데이터와 함께 단일 응답을 받습니다.
* 해당 패턴은 가장 기초가 되는 패턴입니다.
* 예제 코드를 통해서 살펴 보도록 하겠습니다.

<br/>

## **시나리오**
* 시나리오는 다음과 같습니다.
* 온라인 판매 애플리케이션에 대한 OrderManagement 서비스를 구성한다고 가정합니다.
* 이 서비스의 일부로 구현해야 하는 메서드는 클라이언트가 주문 ID를 제공해 기존 주문을 조회할 수 있는 getOrder 메서드 입니다.

<br/>

## **OrderManagement 서비스 정의**
* order.proto 파일을 생성하여 아래와 같이 서비스를 정의합니다.

```
syntax = "proto3";

import "google/protobuf/wrappers.proto";

package ecommerce;

service OrderManagement {
    rpc getOrder (google.protobuf.StringValue) returns (Order);
}

message Order {
    string id = 1;
    repeated string items = 2;
    string description = 3;
    float price = 4;
    string destination = 5;
}
```
* 위에서 Message 선언에 repeated 키워드가 있습니다.
* repeated 는 메시지에서 0을 포함해 한 번 이상 반복하는 필드를 나타내는데 사용합니다. 여기서 하나의 주문 메시지에는 **여러 아이템이 있을 수 있습니다.**

<br/>

## **파이썬 gRPC 모듈 설치**
* 파이썬 코드로 gRPC 코드 작성 전에 gRPC 관렴 모듈들이 우선 설치 되어야 합니다.
* 모듈 설치하는 명령어는 아래와 같습니다.

```console
python -m pip install --upgrade pip

python -m pip install --upgrade pip
python -m pip install grpcio
python -m pip install grpcio-tools
```

<br/>

## **Code Generation**
* 다음으로 앞서 생성한 order.proto 파일을 파이썬 코드로 사용할 수 있게 Generate 해야 합니다.
* Generation 하는 명령어는 아래와 같습니다.

```console
python -m grpc_tools.protoc -I. --python_out=. --grpc_python_out=. ./order.proto
```

* 위 명령어를 실행하게 되면, order_pb2.py, order_pb2_grpc.py 2개의 파일이 생성됩니다.

<br/>

## **Server 구현**
* 생성된 order_pb2.py, order_pb2_grpc.py 파일을 토대로 gRPC Server 를 Python 코드를 이용하여 구현 합니다.

```python
from concurrent import futures
import time

import uuid
from google.protobuf import wrappers_pb2

import grpc
import order_pb2
import order_pb2_grpc

class OrderManagementServicer(order_pb2_grpc.OrderManagementServicer):
    
    def __init__(self):
        self.orderDict = {}
        
        self.orderDict['101'] = order_pb2.Order(id='101', 
                                                price=1000, 
                                                items=['Item - A', 'Item - B'], 
                                                description='Sample order description.',
                                                destination='Seoul')
        
    
    def getOrder(self, request, context):
        order = self.orderDict.get(request.value)
        
        if order is not None: 
            print(f'{order} 주문이 성공적으로 이루어졌습니다.')
            return order
        else: 
            # Error handling 
            print('Order not found ' + request.value)
            context.set_code(grpc.StatusCode.NOT_FOUND)
            context.set_details('Order : ', request.value, ' Not Found.')
            return order_management_pb2.Order()

# Server gRPC 생성
server = grpc.server(futures.ThreadPoolExecutor(max_workers=10))
order_pb2_grpc.add_OrderManagementServicer_to_server(OrderManagementServicer(), server)
print('서버 시작. 포트 열림 50051.')
server.add_insecure_port('[::]:50051')
server.start()
server.wait_for_termination()
```

<br/>

## **Client 구현**
* 생성된 order_pb2.py, order_pb2_grpc.py 파일을 토대로 gRPC Client 를 Python 코드를 이용하여 구현 합니다.

```python
from google.protobuf import wrappers_pb2
import grpc
import order_pb2
import order_pb2_grpc

import time

def run():
    ip = 'localhost'
    port = 50051
    channel = grpc.insecure_channel(f'{ip}:{port}')
    
    # 스텁 생성
    stub = order_pb2_grpc.OrderManagementStub(channel)
    
    order1 = order_pb2.Order(items=['Item - A', 'Item - B', 'Item - C'],
                             price=1000,
                             description='This is a Sample order - 1 : description.', 
                             destination='Seoul, Guro')
    
    order = stub.getOrder(order_pb2.Order(id='101'))
    print("Order service response", order)
    
    
if __name__ == '__main__':
    run()
```

<br/>

## **실행 결과**
* 실행 결과, Order 주문이 정상적으로 된 것을 확인할 수 있습니다.

```console
Server 결과
C:\Users\Desktop\gRPC\gRPC\gRPC 단순 RPC 예제>python server.py
서버 시작. 포트 열림 50051.
id: "101"
items: "Item - A"
items: "Item - B"
description: "Sample order description."
price: 1000.0
destination: "Seoul"
 주문이 성공적으로 이루어졌습니다.

Client 결과
Order service response id: "101"
items: "Item - A"
items: "Item - B"
description: "Sample order description."
price: 1000.0
destination: "Seoul"
```

{% include adsense.html %}
---
title: gRPC 프로토콜 버퍼 기초 - 메시지 정의 2
author: BeomBeomJoJo
date: 2022-04-16 13:33:00 +0800
date: 2022-04-16 13:33:00 +0800
categories: [gRPC, protobuf]
tags: [gRPC, protobuf]
math: true
mermaid: true
---

{% include adsense.html %}

## **참고**
* [참고 사이트](https://developers.google.com/protocol-buffers/docs/proto3)

<br/>

## **스칼라 값 타입**
* gRPC protobuf 에는 message 정의 시, 스칼라 값 타입을 가지고 메시지를 정의합니다.
* 스칼라 값 타입의 종류는 다음과 같습니다.
  * double
  * float
  * int32 : 변수 길이 인코딩시 사용, 음수에는 비효율적임
  * int64 : 변수 길이 인코딩시  사용, 음수에는 비효율적임
  * uint32 : 변수 길이 인코딩시 사용
  * uint63 : 변수 길이 인코딩시 사용
  * sint32 : 변수 길이 인코딩시 사용, 부호있는 값
  * sint64 : 변수 길이 인코딩시 사용. 부호 있는 값
  * bool : 일반적으로 true 나 false로 표시되는 두 가지 가능한 값을 나타내는 값 
  * enum : 이름이 있는 값의 집합을 나타내는 값
  * string : 문자열은 항상 UTF-8 인코딩이거나 7비트 ASCII 이어야 함
  * bytes : 임의의 바이트 배열을 포함

<br/>

## **기본값**
* 메시지가 구문 분석될 때 인코딩된 메시지에 특정 단일 요소가 포함되어 있지 않으면 구문 분석된 객체의 해당 필드가 해당 필드의 기본값으로 설정됩니다.
* 이러한 기본값은 유형별로 차이가 있습니다.
  * 문자열의 경우 기본값은 빈 문자열입니다.
  * 바이트의 경우 기본값은 빈 바이트입니다.
  * bool의 경우 기본값은 false 입니다.
  * 숫자 유형의 경우 기본값은 0 입니다.
  * enums 의 경우 기본값은 **처음으로 정의된 enum 값** 이며 0이어야 합니다.
  * 메시지 필드의 경우 필드가 설정되지 않습ㄴ디ㅏ. 정확한 값은 언어에 따라 다릅니다.

<br/>

## **Enumerations**
* 메시지 유형을 정의할 때 해당 필드 중 하나에 미리 정의된 값 목록 중 하나만 포함되도록 할 수 있습니다.
* 예를 들더, 말뭉치가 될 수 있는 Corpus 각각에 대한 필드를 추가하려고 가정합니다.
* 가능한 각 값에 대한 상수를 사용하여 메시지 정의에 추가하여 매우 간단하게 이 작업을 수행할 수 있습니다.

```protobuf
message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
  enum Corpus {
    UNIVERSAL = 0;
    WEB = 1;
    IMAGES = 2;
    LOCAL = 3;
    NEWS = 4;
    PRODUCTS = 5;
    VIDEO = 6;
  }
  Corpus corpus = 4;
}
```
* 위에서 보다시피 Corpus 열거형의 첫 번째 상수는 0 에 매핑됩니다.
* **모든 열거형의 정의에서 첫 번째 요소로 0에 매핑되는 상수가 포함되어야 합니다.**

<br/>

## **Reserved Values(예약 된 값)**
* 어떤 필드를 제거하거나 주석 처리하고 나중에 사용자가 필드 번호를 재사용 할 수 있습니다.
* 이런 경우 데이터 손상, 개인 정보 보호 버그 등의 문제가 생길 수 있습니다.
* 이런 일이 발생하지 않도록 하는 하나의 방법은 삭제할 필드 번호를 `reserved` 하는 것 입니다.
* 즉, 필드를 삭제한 후 그 필드의 Field Number 를 재사용할 경우 update 가 늦은 Client 에서 장애가 발생할 수 있는 상황을 미연에 방지할 수 있습니다.

```protobuf
enum Foo {
  reserved 2, 15, 9 to 11, 40 to max;
  reserved "FOO", "BAR";
}
```

<br/>

## **다른 메시지 유형 사용**
* 다른 메시지 유형을 필드 유형으로 사용할 수 있습니다.
* 예를 들어, Result 각 메시지에 메시지를 포함하고 싶다고 가정해 보겠습니다.
* 이렇게 하려면 동일한 메시지 유형을 `SearchResponse` 정의한 다음 다음 유형의 필드를 지정할 수 있습니다.

```proto
message SearchResponse{
    repeated Result results = 1;
}

message Result{
    string url = 1;
    string title = 2;
    repeated string snippets = 3;
}
```
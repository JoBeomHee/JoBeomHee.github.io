---
title: gRPC 프로토콜 버퍼 기초 - 메시지 정의
author: BeomBeomJoJo
date: 2022-04-16 12:33:00 +0800
date: 2022-04-16 12:33:00 +0800
categories: [gRPC, protobuf]
tags: [gRPC, protobuf]
math: true
mermaid: true
---

## **참고**
* [참고 사이트](https://developers.google.com/protocol-buffers/docs/proto3)

<br/>

## **소개**
* gRPC 통신을 하기 위해서는 프로토콜 버퍼라는 파일에 서비스 및 메시지 정보들을 정의하고 사용해야 합니다.
* 프로토콜 버퍼를 작성하는 가이드에 대해서 공부해서 정리합니다.

<br/>

## **메시지 유형 정의**
* 먼저 간단한 얘를 살펴 보도록 하겠습니다.
* 각 검색 요청에 쿼리 문자열, 관심 있는 특정 결과 페이지 및 페이지당 결과 수가 있는 검색 요청 메시지 형식을 정의하려고 한다고 가정하겠습니다.
* 다음 `.proto` 은 메시지 유형을 정의 하는데 사용하는 파일 확장자 명입니다.

```protobuf
syntax = "proto3";

message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}
```

* 위에서 첫 번째 줄은 `proto3`  구문을 사용하고 있음을 지정합니다. 이 작업을 수행하지 않으면 프로토콜 버퍼 컴파일러는 사용자가 `proto2` 를 사용한다고 가정합니다. 해당 구문은 항상 첫 번째 줄에 작성해야 합니다.
* `SearchRequest` 메시지 정의는 이 유형의 메시지에 포함할 각 데이터 조각에 대해 하나씩 세 개의 필드(이름/값 쌍) 을 지정합니다. 각 필드에는 이름과 유형이 있습니다.

<br/>

### **필드 유형 지정**
* 위의 예제에서 모든 필드는 **스칼라 유형** 입니다.
* 두 개의 정수(page_number 및 result_per_page) 와 문자열(query) 입니다.
* 열거형 타입 및 기타 메시지 유형을 포함하여 필드에 대한 복항 유형도 지정할 수 있지만, 해당 부분은 다음 포스팅에서 다루도록 하겠습니다.

<br/>

### **필드 번호 할당**
* 위해서 보면 `string query = 1;` 이와 같이 메시지 정의의 각 필드마다 **고유 번호** 가 있습니다.
* 이러한 필드 번호는 **메시지 바이너리 형식** 에서 필드를 식별하는 데 사용되며 메시지 유형이 사용 중이면 변경해서는 안됩니다.
* `1에서 15 사이의 필드 번호는 필드 번호와 필드 유형을 포함하여 인코딩 하는 데 1바이트가 소요됩니다.`
* 16에서 2047 사이의 필드 번호는 2 바이트를 사용합니다.
* 따라서, 매우 자주 발생하는 메시지 요소에 대해서 1에서 15까지의 숫자를 사용해야 합니다.

<br/>

### **필드 규칙 지정**
* 메시지 필드는 다음 중 하나 일 수 있습니다.
  * `singular` : 잘 구성된 메시지는 이 필드 중 0개 또는 1개를 가질 수 있습니다. 그리고 이것은 proto3 구문의 기본 필드 규칙입니다.
  * `repeated` : 이 필드는 올바른 형식의 메시지에서 여러 번 반복될 수 있습니다. 반복되는 값의 순서는 유지됩니다.
* proto3 에서 `repeated` 스칼라 숫자 유형의 필드는 packed 기본적으로 인코딩을 사용합니다.

<br/>

### **더 많은 메시지 유형 추가**
* 단일 `.proto` 파일에 여러 메시지 유형을 정의할 수 있습니다.
* 이것은 여러 관련 메시지를 정의하는 경우에 유용합니다.
* 예를 들어, 메시지 유형에 해당하는 응답 메시지 형식을 정의하려는 `SearchResponse` 경우 동일한 형식에 추가할 수 있습니다.

```protobuf
message SearchRequest {
  string query = 1;
  int32 page_number = 2;
  int32 result_per_page = 3;
}

message SearchResponse {
 ...
}
```

<br/>

### **주석 추가**
* 프로토콜 버퍼 파일에 `//` 와 `/* ... */` 구문을 사용하여 주석을 사용할 수 있습니다.

```protobuf
/* SearchRequest represents a search query, with pagination options to
 * indicate which results to include in the response. */

message SearchRequest {
  string query = 1;
  int32 page_number = 2;  // Which page number do we want?
  int32 result_per_page = 3;  // Number of results to return per page.
}
```
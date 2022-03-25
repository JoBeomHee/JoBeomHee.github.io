---
title: VS Code | Auto Markdown Toc Extension(목차) 소개 및 사용 방법
author: BeomBeomJoJo
date: 2022-03-25 11:33:00 +0800
categories: [VSCode]
tags: [VSCode]
math: true
mermaid: true
---

# VS Code | Auto Markdown Toc Extension(목차) 소개 및 사용 방법

# **참조**
* [Auto Markdown Toc](https://marketplace.visualstudio.com/items?itemName=huntertran.auto-markdown-toc)      

<br/>

# **정의** 
* Markdown 파일에서 헤드 라인의 목차를 생성 합니다.

<br/>

# **Auto Markdown Toc 설치 방법**
* https://marketplace.visualstudio.com/items?itemName=huntertran.auto-markdown-toc

![1](https://user-images.githubusercontent.com/22911504/160086167-fb4c92f5-8131-4e79-8471-8fd16e1a108c.png)

<br/>

# **목차 삽입 예제**
 
## **1. 목차 삽입 예제 코드**
* **목차 삽입 예제**로 사용 될 코드는 아래와 같습니다. 
```md
## TOC 데모

## 제목 1
* 제목 1의 내용입니다.
### 부제목 1.1
* 부제목 1.1의 내용입니다.
## 제목 2
* 제목2의 내용입니다.
### 부제목 2.1
* 부제목 2.1의 내용입니다.
### 부제목 2.2
* 부제목 2.2의 내용입니다.
## 제목 3
* 제목 3의 내용입니다.
### 부제목 3.1
* 부제목 3.1의 내용입니다.
#### 부제목 3.1.1
* 부제목 3.1.1의 내용입니다.
```

<br/>
 
## **2. Auto TOC 생성할 위치에 마우스 우클릭**
* **Auto Markdown TOC : Insert/Update** 속성 선택 합니다. 

![2](https://user-images.githubusercontent.com/22911504/160086181-20fdfa72-6651-444b-b780-0cf764e7d5c2.png)

<br/>

## **3. 목차 생성 완료**
* **header** 구성에 맞게 자동으로 목차가 생성 되었습니다.
```md
<!-- TOC -->

- [TOC 데모]
- [제목 1]
    - [부제목 1.1]
- [제목 2]
    - [부제목 2.1]
    - [부제목 2.2]
- [제목 3]
    - [부제목 3.1]
        - [부제목 3.1.1]

<!-- /TOC -->

<br/>

## TOC 데모

## 제목 1
* 제목 1의 내용입니다.
### 부제목 1.1
* 부제목 1.1의 내용입니다.
## 제목 2
* 제목2의 내용입니다.
### 부제목 2.1
* 부제목 2.1의 내용입니다.
### 부제목 2.2
* 부제목 2.2의 내용입니다.
## 제목 3
* 제목 3의 내용입니다.
### 부제목 3.1
* 부제목 3.1의 내용입니다.
#### 부제목 3.1.1
* 부제목 3.1.1의 내용입니다.

```

# 헤더 번호 섹션 삽입 예제   
## **1. 헤더 번호 섹션 삽입 예제 코드**
* **헤더 번호 섹션 삽입 예제** 로 사용될 코드는 아래와 같습니다. 
```md
## Header 번호 목차 TOC 데모

## 제목 1
* 제목 1의 내용입니다.
### 부제목 1.1
* 부제목 1.1의 내용입니다.
## 제목 2
* 제목2의 내용입니다.
### 부제목 2.1
* 부제목 2.1의 내용입니다.
### 부제목 2.2
* 부제목 2.2의 내용입니다.
## 제목 3
* 제목 3의 내용입니다.
### 부제목 3.1
* 부제목 3.1의 내용입니다.
#### 부제목 3.1.1
* 부제목 3.1.1의 내용입니다.
```

<br/>

## **2. TOC 주석 [+depthFrom+], [+orderedList:true+] 속성 설정 하기**  
* 헤더 번호 목차를 만들 위치에 **[+depthFrom+]** 과 **[+orderedList:true+]** 속성 설정 합니다.
* **[+depthFrom+]** 과 **[+orderedList:true+]** 설정하지 않으면, **[-헤더 번호 생성되지 않습니다.-]**
* **세부 속성 :** https://marketplace.visualstudio.com/items?itemName=huntertran.auto-markdown-toc

![3](https://user-images.githubusercontent.com/22911504/160086195-d32d72d8-4f1c-4dd6-b1fb-d99182196dff.png)

<br/>

## **3. 헤더 번호 생성 완료**
* 아래와 같이 **목차** 앞에 Header 번호가 생성 됩니다. 
```markdown
<!-- TOC depthfrom:2 orderedlist:true -->

- [1. Header 번호 목차 TOC 데모](#1-header-%EB%B2%88%ED%98%B8-%EB%AA%A9%EC%B0%A8-toc-%EB%8D%B0%EB%AA%A8)
- [2. 제목 1](#2-%EC%A0%9C%EB%AA%A9-1)
    - [2.1. 부제목 1.1](#21-%EB%B6%80%EC%A0%9C%EB%AA%A9-11)
- [3. 제목 2](#3-%EC%A0%9C%EB%AA%A9-2)
    - [3.1. 부제목 2.1](#31-%EB%B6%80%EC%A0%9C%EB%AA%A9-21)
    - [3.2. 부제목 2.2](#32-%EB%B6%80%EC%A0%9C%EB%AA%A9-22)
- [4. 제목 3](#4-%EC%A0%9C%EB%AA%A9-3)
    - [4.1. 부제목 3.1](#41-%EB%B6%80%EC%A0%9C%EB%AA%A9-31)
        - [4.1.1. 부제목 3.1.1](#411-%EB%B6%80%EC%A0%9C%EB%AA%A9-311)

<!-- /TOC -->

<br/>

## Header 번호 목차 TOC 데모

## 제목 1
* 제목 1의 내용입니다.
### 부제목 1.1
* 부제목 1.1의 내용입니다.
## 제목 2
* 제목2의 내용입니다.
### 부제목 2.1
* 부제목 2.1의 내용입니다.
### 부제목 2.2
* 부제목 2.2의 내용입니다.
## 제목 3
* 제목 3의 내용입니다.
### 부제목 3.1
* 부제목 3.1의 내용입니다.
#### 부제목 3.1.1
* 부제목 3.1.1의 내용입니다.
```
---
title: File system
description: 181210 Today I learned file system
categories:
 - TIL
tags: [file system, 파일 시스템]
---

`TIL 카테고리의 글은 그날 배운 것을 정리하는 목적으로 포스팅합니다. 내용이 잘못되었다면 댓글로 피드백 부탁드립니다.`

## file

> 저장의 최소 단위. 바이트의 연속적인 연결

파일은 바이트의 연속적인 연결이기 때문에 구조가 없음. 

### 파일의 속성

- 이름 
- 식별자
- 유형
- 위치
- 크기
- 보호 (권한)
- 시간, 날짜 사용자 식별

### 파일 연산

- 생성 
  - 공간 할당, 파일과 관련된 정보를 디렉토리에 추가
- 쓰기
- 읽기
- 파일 내 위치 변경 
- 삭제
  - 내용 삭제, 공간 회수
- 자르기
  - 내용은 삭제되지만 공간은 회수되지 않음.
- 열기
- 닫기

#### open-file-table

 최초 파일 접근 후 (open)재접근에 대한 낭비를 줄이기 위해 모든 열린 파일에 대한 정보가 저장됨.

매번 디렉토리를 검색하는 비용을 줄임.

#### open-file-count

파일에 대해 몇개의 프로세스가 접근했는지 나타냄.

#### open file locking

- `sheard lock` 여러 프로세스가 잠금을 할 수 있음.
- `exclusive lock` 한번에 한 프로세스만 잠금을 하는 것

### 파일 접근 방법

- `sequential access`
  - 순차적으로 접근
  - 가장 일반적임
- `direct access`
  - 직접 접근. 파일은 고정 길이의 논리 레코드의 집합으로 정의되어야함. 
  - 레코드의 번호로 접근.
- 기타 접근 방법
  - 파일에 대한 인덱스`index`를 통해 작동할 데이터의 위치를 신속하게 찾아냄.

## 디렉토리 

디렉토리는 파일 시스템에 포함되어 있으며 디렉토리 안에 있는 파일의 정보들을 가지고 있음.

### 디렉토리 구조

- **Single-Level Directory**
  - 한개의 폴더에 한개의 파일만 존재하는 것.
  - 각 파일의 이름은 고유해야한다.
  - 그룹핑도 하지 못함.
  - ![single-level-dir](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter11/11_10_TwoLevelStructure.jpg)
- **Two-Level Directory**
  - 마스터 파일 디렉토리 밑에 유저 파일 디렉토리가 있는 구조
  - 각 유저당 동일한 이름의 파일을 가지고 있음
  - path 개념이 생김.
  - ![two-level-dir](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter11/11_10_TwoLevelStructure.jpg)

-  **Tree-Structure Directories**

  - 우리가 아는 트리 구조의 디렉토리

  - 디렉토리 밑에 하위 디렉토리를 생성하는 구조

  - 운영체제는 하위 디렉토리를 하나의 파일로 봄.

  - ![tree-structure dir](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter11/11_11_TreeStructure.jpg)



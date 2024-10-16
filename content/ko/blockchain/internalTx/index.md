---
title: '일반 거래와 내부 거래(Internal Tx)'
author: '임훈'
date: 2024-10-17T17:30:13+09:00
category: ['POSTS']
tags: ['Blockchain', 'Ethereum']
og_image: "/images/gamer.png" 
keywords: ['Blockchain', 'Ethereum']
---

## 일반 거래란 무엇인가요?

이더리움 블록체인 에서의 일반적인 거래는 두 지갑 주소 간의 이더(ETH) 또는 토큰 의 간단한 전송을 말합니다.

이러한 거래는 사용자가 시작하고 블록체인 에 직접 기록되므로 쉽게 추적할 수 있습니다.

### 특징

* 사용자가 시작함: 일반적인 거래는 일반적으로 개인이나 단체가 한 주소에서 다른 주소로 ETH 또는 토큰을 보내면서 시작됩니다.

* 블록체인에 기록됨: 모든 일반적인 거래는 이더리움 블록체인에 영구적으로 기록되어 투명하고 변경 불가능한 기록을 제공합니다.

* 거래 수수료: 사용자는 거래를 처리하고 확인하기 위해 채굴자에게 가스 수수료를 지불해야 합니다. 수수료는 네트워크 혼잡과 거래의 복잡성에 따라 다릅니다.

* 간단한 구조: 일반적인 거래에는 보내는 사람과 받는 사람의 주소, 이체 금액, 가스 한도 등의 기본 정보가 포함됩니다.

* 가시성: 이러한 거래는 Etherscan에 있는 모든 사람이 볼 수 있으므로 사용자는 이체를 추적하고 잔액을 확인할 수 있습니다.

### 일반적인 거래는 어떻게 이루어지나요?

* 거래 생성: 사용자는 수신자 주소와 지갑 앱에서 보낼 이더(ETH) 또는 토큰의 양을 지정합니다.

* 거래 서명: 거래는 발신자의 개인 키로 서명되어 진위 여부를 검증합니다.

* 거래 브로드캐스팅: 서명된 거래는 검증을 위해 이더리움 네트워크로 전송됩니다.

* 거래 풀: 거래는 채굴자들이 다음 블록에 포함할 거래를 선택하는 메모풀 에 들어갑니다.

* 채굴 및 확인: 채굴자는 암호화 퍼즐을 풀어 거래가 포함된 블록을 블록체인에 추가합니다.

* 확정성: 거래가 확인되면 블록체인에 기록되어 수신자의 잔액이 업데이트됩니다.

* 가스 요금: 보내는 사람은 거래 복잡성과 네트워크 혼잡도에 따라 가스 요금을 지불합니다.

### 일반 거래에 대한 사용 사례

* P2P 전송: 개인 간에 ETH나 토큰을 보내는 것.

* 상품 및 서비스에 대한 지불: 암호화폐를 사용하여 구매합니다.

* 자금 지갑: 거래 또는 투자 목적으로 다른 지갑으로 자산을 이전하는 것입니다.

### 일반 거래를 보려면 어떻게 해야 하나요?

1. Etherscan 방문 : etherscan.io 로 가세요 .

2. 주소 검색 : 검색창에 지갑 주소를 입력하세요.

3. 주소 페이지에 접속하세요 . 검색 결과에서 주소를 클릭하세요.

4. 거래 탭으로 이동합니다 . "거래" 탭을 클릭하면 모든 일반 거래 목록이 표시됩니다.

5. 검토 세부 정보 : 각 거래에는 거래 해시, 보낸 사람 및 받는 사람 주소, 값, 거래 수수료 및 타임스탬프가 표시됩니다.

## 내부 거래란 무엇인가요?

내부 거래는 "메시지 호출" 또는 "계약 상호작용"이라고도 하며, 이더리움 블록체인의 스마트 계약 내에서 발생합니다. 

지갑 주소 간에 이더나 토큰을 직접 전송하는 일반적인 거래와 달리, 내부 거래는 스마트 계약 내에서 코드를 실행한 결과로 발생합니다.

### 특징

* 스마트 계약에 의해 트리거됨: 스마트 계약이 다른 스마트 계약을 호출하거나 실행 중에 계약에 Ether를 보낼 때 내부 거래가 생성됩니다 .

* 블록체인에 직접 기록되지 않음: 내부 거래는 정상적인 거래가 시작된 시점까지 추적이 가능하지만, 자체 거래 해시가 없으며 일반적인 거래처럼 블록체인에 명시적으로 기록되지 않습니다.

* 복잡한 논리: 내부 거래에는 토큰 전송, 다중 서명 작업, 계약 조건에 따라 트리거되는 자동화 기능 등 복잡한 논리가 포함되는 경우가 많습니다.

* Etherscan에서의 가시성: 주요 거래 목록에는 표시되지 않지만 내부 거래는 특정 계약 또는 지갑의 "내부 거래" 탭에서 Etherscan에서 볼 수 있습니다.

* 사례 사용: 내부 거래는 일반적으로 분산 애플리케이션(dApp) , 분산 금융(DeFi) 프로토콜에서 사용되며 복잡한 계약 상호작용을 실행하는 데 사용됩니다.

### 내부 거래는 어떻게 이루어지나요?

* 이벤트 발생: 스마트 계약이 실행되면 내부 거래가 시작되며, 이는 대개 정상적인 거래에 대한 응답으로 시작됩니다.

* 계약 상호작용: 스마트 계약은 실행되는 동안 다른 계약을 호출하거나 논리의 일부로 Ether를 주소로 보낼 수 있습니다.

* 별도의 거래 해시 없음: 내부 거래에는 고유한 거래 해시가 없으며 원래의 일반 거래에 연결됩니다.

* 실행 로직: 내부 전송은 스마트 계약 코드의 일부로 발생하며, 여기에는 토큰 전송이나 다중 서명 승인과 같은 복잡한 작업이 포함될 수 있습니다.

* 가시성: 주요 거래 목록에서는 보이지 않지만 내부 거래는 관련 계약 또는 지갑과 관련된 Etherscan의 "내부 거래" 탭에서 볼 수 있습니다.

### 내부 거래에 대한 사용 사례

* 분산형 금융(DeFi): 프로토콜 내에서 대출, 차용 및 유동성 공급을 자동화합니다.

* 토큰 스왑: 분산형 거래소(DEX)의 유동성 풀 간 자산 이전을 관리합니다.

* 자동화된 시장 조작자(AMM): 내부적인 움직임을 통해 공급과 수요에 따라 토큰 가격을 조정합니다.

* 크라우드 펀딩 및 ICO: 기금 모금 이벤트 중에 자금 분배 및 토큰 할당을 투명하게 처리합니다.

* 게임과 NFT: 플레이어 간에 게임 내 자산과 NFT를 쉽게 전송할 수 있습니다.

### 내부 거래를 보는 방법?

1. Etherscan 방문 : etherscan.io 로 가세요 .

2. 주소 또는 거래 검색 : 검색 창에 지갑 주소나 거래 해시를 입력하세요.

3. 주소 또는 거래 페이지에 접속하세요 . 해당 결과를 클릭하면 전용 페이지가 열립니다.

4. "내부 거래" 탭으로 이동합니다 . 주소 또는 거래 페이지에서 "내부 거래" 탭을 찾으세요.

5. 검토 세부 정보 : 보낸 사람, 받는 사람, 가치 및 관련된 일반 거래와 같은 세부 정보를 포함한 내부 거래 목록을 확인합니다.

## 일반 거래 vs 내부 거래

다음 표는 Etherscan의 일반 트랜잭션과 내부 트랜잭션의 차이점을 나타냅니다.

| 구분         | 일반 거래                                       | 내부 거래                                           |
| ------------ | ----------------------------------------------- | --------------------------------------------------- |
| **정의**     | 지갑 간 ETH나 토큰을 직접 전송합니다.             | 스마트 계약 실행 중 발생하는 전송.                    |
| **개시**     | 사용자 작업(예: ETH 보내기)에 의해 트리거됩니다.  | 스마트 계약 실행이나 상호작용을 통해 발생합니다.       |
| **시계**     | 블록체인과 Etherscan에서 쉽게 볼 수 있습니다.     | 독립된 항목으로 직접 기록되지 않음; 별도 탭에서 확인 가능. |
| **거래 해시**| 각 거래에는 고유한 거래 해시가 있습니다.         | 고유한 거래 해시가 없으며, 정상적인 거래를 시작하는 데 연결됩니다. |
| **가스 요금**| 발신자가 가스 요금을 지불해야 합니다.           | 일반적으로 초기 거래의 가스 요금에 포함됩니다.         |
| **사용 사례**| 피어투피어(Peer-to-Peer) 전송, 지불 및 토큰 전송. | DeFi 운영, 토큰 스왑, 게임 거래 및 계약 상호 작용.    |
| **복잡성**   | 일반적으로 단순합니다.                         | 복잡한 논리와 자동화된 프로세스가 포함될 수 있습니다.   |
| **기록**     | 이더리움 블록체인에 직접 기록됩니다.             | 스마트 계약 실행 중의 맥락에서 기록됩니다.             |

*Reference*

<https://www.geeksforgeeks.org/normal-transactions-vs-internal-transactions-in-etherscan/>
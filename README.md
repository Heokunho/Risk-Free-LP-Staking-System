# Risk-Free LP Staking System

◎ What is LP Staking?

● LP(Liquidity Pool)
- 사용자가 중앙의 권한없이 암호 화폐 사이의 이동이 가능하도록 하는 스마트 계약에 의해 관리되는 풀

※ 본래 암호 화폐 사이의 이동은 꽤나 번거롭습니다.

● LP Staking
- Token 보유자가 시장에 유동성(Liquidity)을 제공하도록 장려하는 방법

● 흐름
1. Token 보유자가 유동성(Liquidity)을 제공하면 그 대가로 LP Token을 받습니다. 
2. LP Staking을 통해 유동성(Liquidity) 공급자는 LP Token을 Staking하고,
3. Token 보유자가 위험을 완화하고 이익을 높일 수 있도록 보상으로 ‘이자’를 받을 수 있습니다.

◎ Initial Thoghts

Token에 속한 암호 화폐(Crypto Currency)의 가격이 하락한다면, 결국 Pool에 들어간 총 자산이 감소하기 때문에 받을 수 있는 LP Token도 감소합니다. 이는 받는 ‘이자’의 감소로 이어집니다.

암호 화폐의 가격 변동에 의한 손익을 0에 수렴하게 할 수 있다면, 위험 부담 없이 ‘이자’만을 받을 수 있습니다.

Binance 선물 거래의 Short(공매도) 기능을 활용

※ 본인이 매수한 암호 화폐의 가격이 상승하면 이득을 보는 것이 일반적입니다.  하지만 Binance는 선물 거래가 가능합니다. 따라서 두 가지 방식으로 이득을 볼 수 있습니다.
자신의 포지션이 Long(공매수)이라면 암호 화폐의 가격이 상승하면 이득이 됩니다.
자신의 포지션이 Short(공매도)이라면 암호 화폐의 가격이 하락하면 이득이 됩니다.

2번, 즉 Short(공매도) 기능을 활용하여, Token에 속한 암호 화폐를 공매도 한다면 Token 측에서 암호 화폐의 가격 하락으로 인한 손해만큼 Binance 측에서 이득을 얻습니다. 일정한 주기로 Binance 측에서 취한 이득을 Token 측으로 다시 송금해준다면 암호 화폐의 가격 변동에 의한 손익을 0에 수렴하게 할 수 있습니다.

◎ Dev Problem & Solution

● LP Staking을 하는 사이트에서 따로 API를 제공하지 않습니다. /n
-> Token에 속한 암호 화폐 관련 정보를 해당 사이트에서 Crawling 하면 됩니다.

● BeautifulSoup를 사용하면 실시간 data를 Crawling 할 수 없습니다.
-> Selenium과 Chrome Driver를 사용하면 됩니다.

● Selenium과 Chrome Driver를 통해 Crawling 을 할 시에, 매번 새로운 크롬을 열어서 사용하기 때문에
   LP Staking에 필요한 크롬 확장 프로그램을 매번 다시 깔아줘야 하는 등의 문제가 발생합니다.
-> 크롬을 미리 열어 두고, 이를 이용해 Crawling을 하면 됩니다.

◎ Development Process

● 필요한 정보 Crawling

● Binance API를 통해 암호 화폐의 현재 가격 가져오기

● 두 정보를 비교하여 Binance에서 암호 화폐 개수를 맞춰주기
- Token의 암호 화폐의 개수가 더 많다면, Binance에서 Short(공매도)을 통해 조정
- Token의 암호 화폐의 개수가 더 적다면, Binance에서 Long(공매수)을 통해 조정

● 역대 차트 데이터 분석을 토대로 다음 가격 변동을 예측하고, 변동 폭과 반비례하여 거래 주기를 산정
- SVM(Support Vector Machine) 모델을 활용하여 기계학습

※ 반비례하는 이유?
가격 변동 폭이 크면 거래가 활발하다는 뜻, 즉 우리도 더 빠르게 거래해서 개수를 맞춰주어야 합니다.
따라서 거래 주기가 짧아져야 합니다.

● 거래 시, 파일에 거래 시간 및 암호 화폐의 개수, 총 자산을 기록 후 LINE으로 공지

● 매일 자정, 그 날의 총 자산의 평균, 누적 수익을 파일에 저장 후 LINE으로 공지

◎ Exception

● 서버 시간 불일치 -> 윈도우 서버 동기화 주기 설정을 통해 해결

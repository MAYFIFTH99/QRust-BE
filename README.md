<div align="center">
  <h1>📷 QRust (QR 피싱 탐지 서비스) 📸</h1>
  <p>
		<b>AI 기반 QR 코드 탐지 & 신뢰할 수 있는 QR 코드 생성</b>
	</p>
	<br>
</div>

## 📜 목차

| 순번 | 항목 |
| :-: | :-: |
| 1 | 팀원 소개 |
| 2 | 서비스 개요 |
| 3 | 서비스 소개 |
| 4 | 기능 플로우차트 |
| 5 | 시스템 아키텍처 |
| 6 | UI / UX |
| 7 | ERD |

</br>

## ✨ 팀원 소개

| _이름_ | 김준형 | 성민석 | 노경민 | 
|:-----:|:----:|:-----:|:----:|
| ___역할___ | BE | BE | FE / AI |
| ___Github___ | <a href="https://github.com/JHZLO"><img src="https://avatars.githubusercontent.com/u/105791673?v=4" width="64" height="64"></a> | <a href="https://github.com/MAYFIFTH99"><img src="https://avatars.githubusercontent.com/u/149806254?v=4" width="64" height="64"></a> | <a href="https://github.com/rohkyoungmin"><img src="https://avatars.githubusercontent.com/u/192883674?v=4" width="64" height="64"></a> |

</br>

## 💡 서비스 개요

기술의 발전과 함께 보안 위협 또한 급증하고 있다.

최근 QR 코드의 상용화로 인해 사용자들은 URL 링크를 일일이 검색하지 않아도 카메라 앱을 통해 간편하게 접속할 수 있게 되었다. 이는 사용자뿐만 아니라 관리자에게도 편리함을 제공한다. 온라인에서는 단순히 URL 링크를 전송하면 되었지만, 오프라인에서는 복잡하게 사이트 접속 방법을 안내해야 했다. 그러나 QR 코드가 도입되면서 이제는 QR 코드만 제공하면 사용자들이 쉽게 접속할 수 있어 이러한 번거로움이 해소되었다.

하지만 이러한 편리함을 악용한 보안 위협이 증가하고 있다. 해커들은 QR 코드 스티커 위에 악성 링크가 포함된 QR 코드를 덧붙이는 등의 방식으로 일명 **큐싱(Qsing)** 이라는 공격으로 피싱 공격을 시도하고 있다. 이로 인해 사용자가 악성 사이트에 접속해 개인 정보가 유출되거나 보안 사고가 발생하는 사례가 급증하고 있다.

따라서 이러한 보안 위협을 방지하기 위해 **QR 코드 피싱 탐지** 시스템을 개발하고자 한다.

</br>

## 📢 서비스 소개

### 1️⃣ 배경 및 문제점
![image](https://github.com/user-attachments/assets/09d1bfe9-93a1-4c60-845b-c81e94089848)

QR코드는 기존의 바코드와는 달리 별도의 리더기로 인식하지 않아도 된다는 장점을 가지고 있습니다. 단순히 휴대폰만 보유하고 있으면 언제든 어디서든 누구나 쉽게 인식할 수 있고 생성할 수 있습니다.

이러한 특징 때문에 최근 4년동안 대략 433%나 증가했다는 것을 확인할 수 있습니다.

특히, 코로나 19의 영향으로 인해 비접촉 서비스의 필요성이 증가하면서 QR 코드 사용이 급격히 늘었습니다.

![image](https://github.com/user-attachments/assets/991bcdff-18fc-4fcb-ad4b-d01dfb81191f)
QR코드의 사용량이 증가함에 따라 이러한 편의성의 허점을 노리고 QR코드를 통한 피싱, 일명 큐싱의 사례가 증가하고 있습니다.

우리가 흔히 이용하는 공유 자전거의 QR코드에 피싱사이트를 유도하는 QR코드를 덧붙이거나, 주차 딱지인척 가장한 큐싱으로 피해를 입히고 있습니다.

QR코드는 URL과 달리 주소도 알 수 없어 스캔하기 전까지는 출처의 불분명함을 따지기가 어렵습니다. 또한 범죄 기법이 점점 정교해지면서 **사용자의 인식 강화만으로는 피싱 위협에 효과적으로 대처하기 어려운 상황입니다.**

따라서 이에 대한 기술적인 대응이 시급한 상황입니다.

![image](https://github.com/user-attachments/assets/c851ae85-3e64-475d-8c57-dbe5550139d1)

![image](https://github.com/user-attachments/assets/c5d24384-4bd2-49b5-becd-53aadaf19899)
시중에 나와있는 QR 피싱 방지 서비스들의 한계점을 다음과 같이 분석해보았습니다.

1. **큐싱에 대한 탐지가 미흡하다.**
   
   ⇒ 일부 앱에서는 단순히 피싱 URL만 분석해주지, 사용자가 카메라로 직접 큐알코드에 들이댔을 때 큐싱의 여부를 판별하는 기능은 미흡할뿐더러 제대로 서비스화로 이루어지지 않았습니다.

3. **사용자의 번거로움**

   ⇒  장점은 간편함과 단순함입니다. 사용자가 언제 어디서나 쉽게 QR코드를 인식하고 원하는 작업을 빠르게 수행할 수 있다는 점이 QR코드의 핵심 이점입니다. 그러나 **플러스 친구**나 **마이케이티앱**의 경우, 사용자가 QR코드를 인식하고자 할 때, ‘복잡한 절차나 추가적인 조작이 필요하다는 점에서 사용자의 번거로움을 초래합니다.
   
   ⇒ 결국 QR코드의 **핵심 이점인 간편함과 즉각성**이 사라지게 됩니다.

3. **사용자 입장에서 체감되는 효용이 낮다.**

   ⇒ 결국, 피싱 탐지 앱은 사용자가 실제로 위협을 인식하기 전까지는 필요성을 체감하기 어렵습니다. 또한 보안 위협에 대한 인식이 낮은 사용자들은 피싱 탐지 앱 자체에 관심을 가지지 않습니다.  따라서 결국 피싱 탐지를 목적으로 하는 서비스들은 자연스럽게 사용자들로부터 외면을 당하기 일쑤입니다.

### 2️⃣ END USER
![image](https://github.com/user-attachments/assets/7c4b74d9-8943-4623-9d0d-aa7286fcb47b)

### 3️⃣ 앱 기획
![image](https://github.com/user-attachments/assets/7478b5c5-9857-4f48-a0a6-e156d043e571)

### 4️⃣ 주요 기능
> 출처가 불분명한 QR 코드 탐지

![image](https://github.com/user-attachments/assets/9eb0942d-42eb-4440-b4b8-4d4acf69e583)

> QRust 앱을 통해 생성한 QR 코드 탐지

![image](https://github.com/user-attachments/assets/d9158aa9-a79e-4f34-b3ef-cef74037475c)

### 5️⃣ 기대 효과
![image](https://github.com/user-attachments/assets/2b0f4ed8-9df1-4a06-a688-bea31ee194dd)

![image](https://github.com/user-attachments/assets/70b260df-7516-4152-91d3-b7b8518e9496)

저희 앱이 활성화되면서 " **기업과 개인 간의 신뢰할 수 있는 QR코드 생태계 구축** " 을 목표로 하고 있으며, 이렇게 된다면 사용자는 QR코드를 스캔할 때 별도의 분석 과정 없이 **안전하고 빠르게 접근**할 수 있습니다. 

또한 사용자들은 QR코드를 스캔하는 데에 대한 심리적 불안감이 줄어들고, 기업 입장에서도 보안 강화 및 브랜드 신뢰도 상승 효과를 기대할 수 있습니다.

결국, 저희 앱은 **사용자에게 보안성과 편의성을 동시에 제공**하면서 **QR코드 기반의 안전한 디지털 환경**을 구축하는 데 기여할 것입니다.

</br>

## 🚀 기능 플로우차트

> IA

![image](https://github.com/user-attachments/assets/58e3ceb6-d24b-4af2-b8c1-c5ca8b43f120)

> QR 피싱 탐지 플로우

![image](https://github.com/user-attachments/assets/5d48be28-b9d5-41d7-8e56-917f989f38e6)

</br>

## ⚒️ 시스템 아키텍처

![image](https://github.com/user-attachments/assets/0319d938-4200-4e8b-97ad-1c42c7d7ae66)

</br>

## 🪄 UI / UX

![image](https://github.com/user-attachments/assets/2176e35f-758d-49be-a47a-74d8e921e07e)

</br>

## 📑 ERD

![image](https://github.com/user-attachments/assets/ef2ec694-6a48-41fe-80db-de703d62da5b)



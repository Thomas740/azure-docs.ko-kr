---
title: Azure Cosmos DB 요금 청구 이해
description: 이 문서에서는 몇 가지 예제를 사용하여 Azure Cosmos DB 요금 청구를 이해하는 방법을 설명합니다.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: a097539e51aa2a2130dead236d553d60f2ebb89d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65965669"
---
# <a name="understand-your-azure-cosmos-db-bill"></a>Azure Cosmos DB 요금 청구 이해

완전 관리형 클라우드 기본 데이터베이스 서비스인 Azure Cosmos DB는 프로비전된 처리량 및 사용된 스토리지에 대해서만 요금을 청구하여 청구 과정을 간소화합니다. 추가 라이선스 요금, 하드웨어, 유틸리티 비용 또는 온-프레미스 또는 IaaS 호스팅 대안에 비해 시설 비용 없는 합니다. 다중 지역 Azure Cosmos DB 기능을 고려할 때 데이터베이스 서비스는 기존 온-프레미스 또는 IaaS 솔루션에 비해 비용에서을 상당히 절감을 제공 합니다.

Azure Cosmos DB을 사용하면 프로비전된 처리량 및 사용된 스토리지를 기준으로 시간당 요금이 청구됩니다. 프로비전된 처리량의 경우, 표준 공용 가격 책정이 사용된다고 가정하면 청구 단위는 100RU/초로 시간당 $0.008가 부과됩니다. [가격 책정 페이지](https://azure.microsoft.com/pricing/details/cosmos-db/)를 참조하세요. 사용된 스토리지의 경우, 월간 1GB의 스토리지에 대해 $0.25가 청구됩니다. [가격 책정 페이지](https://azure.microsoft.com/pricing/details/cosmos-db/)를 참조하세요. 

이 문서에서는 몇 가지 예외를 사용하여 월별 청구서에 표시되는 세부 항목을 이해하는 데 도움을 줍니다. 예제에 표시되는 수치는 Azure Cosmos 컨테이너가 다른 양의 처리량이 프로비전되었거나, 다중 지역에 걸쳐 있거나, 한 달 중 다른 기간에 실행된 경우 달라질 수 있습니다.

## <a name="billing-examples"></a>청구 예제

### <a name="billing-example---throughput-on-a-container-full-month"></a>청구 예제 - 컨테이너에 대한 처리량(전체 월)

* 컨테이너에서 1,000RU/초의 처리량을 구성하며, 24시간 * 30일(해당 월에 포함된 일 수) = 총 720시간 동안 유지된다고 가정합니다.  

* 1,000RU/초는 컨테이너가 존재하는 매시간 동안 100RU/초 기준으로 10개 단위에 해당합니다(즉, 1,000/100 = 10). 

* 시간당 10개 단위에 $0.008(시간당 100RU/초)를 곱하면 시간당 $0.08가 됩니다. 

* 시간당 $0.08에 월별 시간 수를 곱하면 $0.08 * 24시간 * 30일 = 월별 $57.60가 됩니다.  

* 월간 총 청구 요금은 7,200개 단위(100RU 기준), 즉 $57.60에 해당합니다.

### <a name="billing-example---throughput-on-a-container-partial-month"></a>청구 예제 - 컨테이너에 대한 처리량(부분 월)

* 프로비전된 처리량이 2,500RU/초인 컨테이너를 만든다고 가정합니다. 이 컨테이너는 월 중에 24시간 동안 라이브됩니다(예를 들어 만들고 24시간 후에 삭제함).  

* 이 경우 청구서에 600개 단위가 표시됩니다(2,500RU/초 / 100RU/초/단위 * 24시간). 비용은 $4.80(600개 단위 * $0.008/단위)입니다.

* 월간 총 청구 금액은 $4.80입니다.

### <a name="billing-rate-if-storage-size-changes"></a>스토리지 크기가 변경될 경우의 청구 요금

Storage 용량은 한 달 동안 저장된 최대 시간당 데이터 양(GB)을 단위로 요금이 청구됩니다. 예를 들어 한 달 동안 15일은 100GB의 스토리지를 사용하고 나머지 15일은 50GB의 스토리지를 사용하는 경우 이 달에는 75GB의 스토리지에 해당하는 요금이 청구됩니다.

### <a name="billing-rate-when-container-or-a-set-of-containers-are-active-for-less-than-an-hour"></a>컨테이너 또는 컨테이너 세트가 1시간 이내로 활성 상태인 경우의 청구 요금

사용량이나 컨테이너 또는 데이터베이스 활성 시간이 1시간 미만인지에 관계없이 컨테이너 또는 데이터베이스가 존재하는 시간마다 균일한 요금이 청구됩니다. 예를 들어, 컨테이너 또는 데이터베이스를 하나 만들고 5분 후 삭제하는 경우 청구서에는 1시간이 포함됩니다.

### <a name="billing-rate-when-throughput-on-a-container-or-database-scales-updown"></a>컨테이너 또는 데이터베이스에 대한 처리량이 확장/축소될 경우의 청구 요금

오전 9:30에 프로비전된 처리량을 400RU/초에서 1,000RU/초로 늘린 다음 오전 10:45에 프로비전된 처리량을 다시 400RU/초로 줄인 경우 1,000RU/초 2시간에 대한 요금이 청구됩니다. 

오전 9:30에 컨테이너 또는 컨테이너 집합에 대해 프로비전된 처리량을 100K RU/초에서 200K RU/초로 늘린 다음 오전 10:45에 프로비전된 처리량을 다시 100K RU/초로 줄인 경우 200K RU/초, 2시간에 대한 요금이 청구됩니다.

### <a name="billing-example-multiple-containers-each-with-dedicated-provisioned-throughput-mode"></a>청구 예제: 각각에 프로비전된 전용 처리량 모드가 적용되는 여러 컨테이너

* 프로비전된 처리량이 각각 500RU/초 및 700RU/초인 두 개의 컨테이너가 있는 미국 동부 2에서 Azure Cosmos 계정을 만드는 경우 총 프로비전된 처리량은 1,200RU/초가 됩니다.  

* 요금은 1,200/100 * $0.008 = $0.096/시간으로 청구됩니다. 

* 처리량 요구 사항이 변경되어 각 컨테이너의 용량을 500RU/초만큼 늘리고 20,000RU/초를 사용하여 새 무제한 컨테이너도 만드는 경우 전체 프로비전된 용량은 22,200RU/초(1,000RU/초 + 1,200RU/초 + 20,000RU/초)가 됩니다.  

* 그러면 청구 금액은 $0.008 x 222 = $1.776/시로 변경됩니다. 

* 720시간(24시간 * 30일)이 있는 월에서 500시간 동안 프로비전된 처리량이 1,200RU/초였고 나머지 220시간 동안 프로비전된 처리량이 22,200RU/초였다면 월별 청구서에 다음 금액이 표시됩니다. 500 x $0.096/시 + 220 x $1.776/시 = $438.72/월

![전용 처리량 청구 예제](./media/understand-your-bill/bill-example1.png)

### <a name="billing-example-containers-with-shared-throughput-mode"></a>청구 예제: 공유 처리량 모드를 사용하는 컨테이너

* 프로비전된 처리량이 각각 50K RU/초 및 70K RU/초인 2개의 Azure Cosmos 데이터베이스(컨테이너 세트가 데이터베이스 수준에서 처리량을 공유함)를 사용하여 미국 동부 2 지역에 Azure Cosmos 계정을 만들 경우 프로비전된 총 처리량은 120K RU/초가 됩니다.  

* 요금은 1,200 * $0.008 = $9.60/시간으로 청구됩니다. 

* 처리량을 변경해야 하며 각 데이터베이스의 프로비전된 처리량을 데이터베이스별로 10K RU/초씩 늘렸으며, 전용 처리량 모드가 15K RU/초인 첫 번째 데이터베이스에 대한 새 컨테이너를 공유 처리량 데이터베이스에 추가할 경우 프로비전된 전체 용량은 155K RU/초(60K RU/초 + 80K RU/초 + 15K RU/초)가 됩니다.  

* 그러면 청구 금액은 다음과 같이 변경됩니다. 1,550 * $0.008 = $12.40/시간  

* 720시간이 있는 월에서 300시간 동안 프로비전된 처리량이 120K RU/초였고 나머지 420시간 동안 프로비전된 처리량이 155K RU/초였다면 월별 청구서에 다음 금액이 표시됩니다. 300 x $9.60/시 + 420 x $12.40/시 = $2,880 + $5,208 = $8,088/월 

![공유 처리량 청구 예제](./media/understand-your-bill/bill-example2.png)

## <a name="billing-examples-with-geo-replication-and-multi-master"></a>지역 복제 및 다중 마스터를 사용하는 청구 예제  

언제든지 전 세계의 Azure 지역을 Azure Cosmos DB 데이터베이스 계정에 추가/제거할 수 있습니다. 다양한 Azure Cosmos DB 데이터베이스 및 컨테이너에 대해 구성한 처리량은 Azure Cosmos 데이터베이스 계정과 연결된 각 Azure 지역에서 예약됩니다. Azure Cosmos 데이터베이스 계정 내의 모든 데이터베이스 및 컨테이너에서 구성된 프로비전된 처리량 합계(시간 기준으로 프로비전됨)가 T이고 데이터베이스 계정과 연결된 Azure 지역 수가 N이면 Azure Cosmos 데이터베이스 계정에 대한 지정된 시간 동안의 총 프로비전된 처리량은 (a) 단일 쓰기 지역으로 구성된 경우 T x N RU/초이고 (b) 쓰기를 처리할 수 있는 모든 지역으로 구성된 경우 T x (N+1) RU/초입니다. 프로비전된 처리량(단일 쓰기 지역) 비용은 100RU/초당 $0.008/시간이며, 여러 쓰기 가능 지역(다중 마스터 구성)으로 프로비전된 처리량 비용은 100RU/초당 $0.016/시간입니다([가격 책정 페이지](https://azure.microsoft.com/pricing/details/cosmos-db/) 참조). 단일 쓰기 지역이든, 다중 쓰기 지역이든 관계없이 Azure Cosmos DB에서는 모든 지역의 데이터를 읽을 수 있습니다.

### <a name="billing-example-multi-region-azure-cosmos-account-single-region-writes"></a>청구 예제: 다중 지역 Azure Cosmos 계정, 단일 지역 쓰기

Azure Cosmos 컨테이너가 미국 서부에 있다고 가정해 보겠습니다. 10K RU/초의 처리량으로 컨테이너를 만들고, 이번달에 1TB의 데이터를 저장합니다. Azure Cosmos 계정에 미국 동부, 북유럽 및 동아시아의 3개 지역을 추가하고 각 지역의 스토리지와 처리량이 같다고 가정해 봅니다. 총 월간 청구 금액은 1개월을 30일로 가정했을 때 다음과 같습니다. 

|**항목** |**사용량(월)** |**비용** |**월간 비용** |
|---------|---------|---------|-------|
|미국 서부의 컨테이너에 대한 처리량 청구      | 10K RU/초 * 24 * 30    |시간 기준 100RU/초당 $0.008   |$576|
|3개 추가 지역인 미국 동부, 북유럽 및 동아시아에 대한 처리량 청구       | 3 * 10K RU/초 * 24 * 30    |시간 기준 100RU/초당 $0.008  |$1,728|
|미국 서부의 컨테이너에 대한 스토리지 청구      | 250GB    |$0.25/GB  |$62.50|
|3개 추가 지역인 미국 동부, 북유럽 및 동아시아에 대한 스토리지 청구      | 3 * 250GB    |$0.25/GB  |$187.50|
|**합계**     |     |  |**$2,554**|

*또한 매월 미국 서부의 컨테이너에서 100GB 데이터를 송신하여 미국 동부, 북유럽 및 동아시아로 데이터를 복제한다고 가정해 봅니다. 데이터 전송 요금을 기준으로 송신에 대한 요금이 청구됩니다.*

### <a name="billing-example-multi-region-azure-cosmos-account-multi-region-writes"></a>청구 예제: 다중 지역 Azure Cosmos 계정, 다중 지역 쓰기

미국 서부에서 Azure Cosmos 컨테이너를 만든다고 가정해 보겠습니다. 10K RU/초의 처리량으로 컨테이너를 만들고, 이번달에 1TB의 데이터를 저장합니다. 각각 스토리지와 처리량이 동일한 3개 지역(미국 동부, 북유럽 및 동아시아)을 추가하고, Azure Cosmos 계정과 연결된 모든 지역의 컨테이너에 쓰려고 한다고 가정합니다. 총 월간 청구 금액은 1개월을 30일로 가정했을 때 다음과 같습니다.

|**항목** |**사용량(월)**|**비용** |**월간 비용** |
|---------|---------|---------|-------|
|미국 서부의 컨테이너에 대한 처리량 청구(모든 지역에 쓰기 가능)       | 10K RU/초 * 24 * 30    |시간 기준 100RU/초당 $0.016    |$1,152 |
|3개 추가 지역인 미국 동부, 북유럽 및 동아시아에 대한 처리량 청구(모든 지역에 쓰기 가능)        | (3 + 1) * 10K RU/초 * 24 * 30    |시간 기준 100RU/초당 $0.016   |$4,608 |
|미국 서부의 컨테이너에 대한 스토리지 청구      | 250GB    |$0.25/GB  |$62.50|
|3개 추가 지역인 미국 동부, 북유럽 및 동아시아에 대한 스토리지 청구      | 3 * 250GB    |$0.25/GB  |$187.50|
|**합계**     |     |  |**$6,010**|

*또한 매월 미국 서부의 컨테이너에서 100GB 데이터를 송신하여 미국 동부, 북유럽 및 동아시아로 데이터를 복제한다고 가정해 봅니다. 데이터 전송 요금을 기준으로 송신에 대한 요금이 청구됩니다.*

### <a name="billing-example-azure-cosmos-account-with-multi-master-database-level-throughput-including-dedicated-throughput-mode-for-some-containers"></a>청구 예제: 일부 컨테이너에 대한 전용 처리량 모드를 포함하여 다중 마스터, 데이터베이스 수준 처리량이 있는 Azure Cosmos 계정

모든 지역이 쓰기 가능한(다중 마스터 구성) 다중 지역 Azure Cosmos 계정이 있는 다음 예제를 가정해봅니다. 간단히 하기 위해 스토리지 크기는 달라지지 않고 일정하게 유지된다고 가정하며, 여기서는 예제를 보다 간단히 나타내기 위해 생략합니다. 월간 프로비전된 처리량은 다음과 같이 달라집니다(30일 또는 720시간으로 가정). 

[0-100시간]:  

* 모든 지역이 쓰기 가능한 세 개의 지역 Azure Cosmos 계정(미국 서부, 미국 동부, 북유럽)을 만들었습니다. 

* 공유 처리량 10K RU/초를 갖는 데이터베이스(D1)를 만들었습니다. 

* 공유 처리량 30K RU/초를 갖는 데이터베이스(D2)를 만들었습니다.  

* 전용 처리량 20K RU/초를 갖는 컨테이너(C1)를 만들었습니다. 

[101-200시간]:  

* 데이터베이스(D1)를 50K RU/초로 확장했습니다. 

* 데이터베이스(D2)를 70K RU/초로 확장했습니다.  

* 컨테이너(C1)를 삭제했습니다.  

[201-300시간]:  

* 전용 처리량 20K RU/초를 갖는 컨테이너(C1)를 다시 만들었습니다. 

[301-400시간]:  

* Azure Cosmos 계정에서 지역 중 하나를 제거했습니다(쓰기 가능 지역 수는 이제 2개임). 

* 데이터베이스(D1)를 10K RU/초로 축소했습니다. 

* 데이터베이스(D2)를 80K RU/초로 확장했습니다.  

* 컨테이너(C1)를 다시 삭제했습니다. 

[401-500시간]:  

* 데이터베이스(D2)를 10K RU/초로 축소했습니다.  

* 전용 처리량 20K RU/초를 갖는 컨테이너(C1)를 다시 만들었습니다. 

[501-700시간]:  

* 데이터베이스(D1)를 20K RU/초로 확장했습니다.  

* 데이터베이스(D2)를 100K RU/초로 확장했습니다.  

* 컨테이너(C1)를 다시 삭제했습니다.  

[701-720시간]:  

* 데이터베이스(D2)를 50K RU/초로 축소했습니다.  

해당 월의 720시간 동안 프로비전된 총 처리량의 변화량은 아래 그림과 같습니다. 

![실제 예제](./media/understand-your-bill/bill-example3.png)

월간 총 청구 금액(한달을 30일/720시간으로 가정)은 다음과 같이 계산됩니다.

|**시간**  |**RU/초** |**항목** |**사용량(시간)** |**비용** |
|---------|---------|---------|-------|-------|
|[0-100] |D1:10K <br/>D2:30K <br/>C1:20K |미국 서부의 컨테이너에 대한 처리량 청구(모든 지역에 쓰기 가능)  | `D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br/>`D2: 30 K RU/sec/100 * $0.016 * 100 hours = $480` <br/>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$960  |
| | |2개의 추가 지역인 미국 동부, 북유럽(모든 지역이 쓰기 가능)에 대한 처리량 요금  |`(2 + 1) * (60 K RU/sec /100 * $0.016) * 100 hours = $2,880`  |$2,880  |
|[101-200] |D1:50K <br/>D2:70K <br/>C1: -- |미국 서부의 컨테이너에 대한 처리량 청구(모든 지역에 쓰기 가능)  |`D1: 50 K RU/sec/100 * $0.016 * 100 hours = $800` <br/>`D2: 70 K RU/sec/100 * $0.016 * 100 hours = $1,120` |$1920  |
| | |2개의 추가 지역인 미국 동부, 북유럽(모든 지역이 쓰기 가능)에 대한 처리량 요금  |`(2 + 1) * (120 K RU/sec /100 * $0.016) * 100 hours = $5,760`  |$5,760  |
|[201-300]  |D1:50K <br/>D2:70K <br/>C1:20K |미국 서부의 컨테이너에 대한 처리량 청구(모든 지역에 쓰기 가능)  |`D1: 50 K RU/sec/100 * $0.016 * 100 hours = $800` <br/>`D2: 70 K RU/sec/100 * $0.016 * 100 hours = $1,120` <br/>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$2,240  |
| | |2개의 추가 지역인 미국 동부, 북유럽(모든 지역이 쓰기 가능)에 대한 처리량 요금  |`(2 + 1) * (140 K RU/sec /100 * $0.016-) * 100 hours = $6,720` |$6,720 |
|[301-400] |D1:10K <br/>D2:80K <br/>C1: -- |미국 서부의 컨테이너에 대한 처리량 청구(모든 지역에 쓰기 가능)  |`D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br/>`D2: 80 K RU/sec/100 * $0.016 * 100 hours = $1,280`  |$1,440   |
| | |2개의 추가 지역인 미국 동부, 북유럽(모든 지역이 쓰기 가능)에 대한 처리량 요금  |`(1 + 1) * (90 K RU/sec /100 * $0.016) * 100 hours = $2,880`  |$2,880  |
|[401-500] |D1:10K <br>D2:10K <br>C1:20K |미국 서부의 컨테이너에 대한 처리량 청구(모든 지역에 쓰기 가능)  |`D1: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br>`D2: 10K RU/sec/100 * $0.016 * 100 hours = $160` <br>`C1: 20 K RU/sec/100 *$0.016 * 100 hours = $320` |$640  |
| | |2개의 추가 지역인 미국 동부, 북유럽(모든 지역이 쓰기 가능)에 대한 처리량 요금  |`(1 + 1) * (40 K RU/sec /100 * $0.016) * 100 hours = $1,280`  |$1,280  |
|[501-700] |D1:20K <br>D2:100K <br>C1: -- |미국 서부의 컨테이너에 대한 처리량 청구(모든 지역에 쓰기 가능)  |`D1: 20 K RU/sec/100 * $0.016 * 200 hours = $640` <br>`D2: 100 K RU/sec/100 * $0.016 * 200 hours = $3,200` |$3,840  |
| | |2개의 추가 지역인 미국 동부, 북유럽(모든 지역이 쓰기 가능)에 대한 처리량 요금  |`(1 + 1) * (120 K RU/sec /100 * $0.016) * 200 hours = $1,280`  |$7,680  |
|[701-720] |D1:20K <br/>D2:50K <br/>C1: -- |미국 서부의 컨테이너에 대한 처리량 청구(모든 지역에 쓰기 가능)  |`D1: 20 K RU/sec/100 *$0.016 * 20 hours = $64` <br/>`D2: 50 K RU/sec/100 *$0.016 * 20 hours = $160` |$224  |
| | |2개의 추가 지역인 미국 동부, 북유럽(모든 지역이 쓰기 가능)에 대한 처리량 요금  |`(1 + 1) * (70 K RU/sec /100 * $0.016) * 20 hours = $448`  |$224  |
|| |**월간 총 비용**  | |**$38,688**   |

## <a name="proactively-estimating-your-monthly-bill"></a>사전에 월별 청구 금액 예측  

해당 월이 끝나기 전에 청구 금액을 미리 예상해보는 또 다른 예를 살펴보겠습니다. 다음과 같이 청구 금액을 예측할 수 있습니다.

|**스토리지 비용** | |
|----|----|
|평균 레코드 크기(KB) |1 |
|레코드 수  |100,000,000  |
|총 스토리지(GB)  |100 |
|GB당 월별 비용  |$0.25  |
|스토리지에 대한 월별 예상 비용   |$25.00  |

<br>

|**처리량 비용** | | | |
|----|----|----|----|
|작업 유형| 요청/초| 평균 RU/요청| 필요한 RU|
|쓰기| 100 | 5 | 500|
|읽기| 400| 1| 400|

총 RU/초: 500 + 400 = 900 시간당 비용: 900/100 * $0.008 = $0.072 처리량에 대한 월간 예상 비용(1달을 31일로 가정) 처리량: $0.072 * 24 * 31 = $53.57

**월간 총 비용**

월간 총 비용 = 스토리지에 대한 월간 비용 + 처리량에 대한 월간 비용 월간 총 비용 = $25.00 + $53.57 = $78.57

*가격 책정은 지역에 따라 달라질 수 있습니다. 최신 가격 책정에 대해서는 [가격 책정 페이지](https://azure.microsoft.com/pricing/details/cosmos-db/)를 참조하세요.*

## <a name="billing-with-azure-cosmos-db-reserved-capacity"></a>Azure Cosmos DB 예약 용량에 따른 요금 청구

Azure Cosmos DB 예약 용량을 사용하여 모든 Azure 지역의 모든 Azure Cosmos DB 데이터베이스 및 컨테이너(API 또는 데이터 모델에 관계없음)에 적용할 수 있는 프로비전된 처리량(예약 용량 또는 예약)을 미리 구입할 수 있습니다. 프로비전된 처리량 가격은 지역에 따라 다르므로, 예약된 용량을 할인된 가격으로 구매한 금액 크레딧(각 지역의 각 가격으로 프로비전된 처리량에서 얻을 수 있는)으로 생각하면 도움이 됩니다. 예를 들어, 50K RU/초가 프로비전된 단일 컨테이너가 있고 전역적으로 복제된 2개의 지역(미국 동부 및 일본 동부)이 있는 Azure Cosmos 계정이 있다고 가정해 보겠습니다. 종량제 옵션을 선택하는 경우 다음 비용을 지불하게 됩니다.  

* 미국 동부: 해당 지역에서 100RU/초당 $0.008 요금으로 50K RU/초에 대해 부과 

* 일본 동부: 해당 지역에서 100RU/초당 $0.009 요금으로 50K RU/초에 대해 부과

총 청구 금액(예약 용량 없음)은 다음과 같습니다(1달을 30일 또는 720시간으로 가정). 

|**지역**| **100RU/초당 시간 기준 가격**|**단위(RU/초)**|**청구된 금액(시간당)**| **청구된 금액(월별)**|
|----|----|----|----|----|
|미국 동부|$0.008 |50K|$4|$2,880 |
|일본 동부|$0.009 |50K| $4.50 |$3,240 |
|합계|||$8.50|$6,120 |

대신 예약 용량을 구입했다고 가정해봅니다. 1년 동안 $56,064(20% 할인) 또는 시간당 $6.40로 100K RU/초의 예약 용량을 구입할 수 있습니다. [가격 책정 페이지](https://azure.microsoft.com/pricing/details/cosmos-db/)에서 예약 용량 가격 책정을 참조하세요.  

* 처리량 비용(종량제): 100,000RU/초/100 * $0.008/시 * 1년간 8760시간 = $70,080 

* 처리량 비용(예약 용량) $70,080에서 20% 할인된 금액 = $56,064 

결과적으로 구입한 내역은 시간당 $6.40의 가격으로 미국 동부의 품목 가격을 사용하는 100K RU/초에 대해 시간당 $8의 크레딧이 됩니다. 그런 다음, 해당 구독에 대해 설정된 지역별 정가로 모든 전역 Azure 지역에서 프로비전된 처리량 용량에 대해 시간당으로 이 선불 처리량 예약에서 차감할 수 있습니다. 미국 동부 및 일본 동부에서 각각 50K RU/초를 프로비전하는 이 예에서는 시간당 프로비전된 처리량에서 $8.00의 가치를 얻을 수 있으며, 시간당(또는 $360/월) $0.50의 초과분에 대한 요금이 청구됩니다. 

|**지역**| **100RU/초당 시간 기준 가격**|**단위(RU/초)**| **청구된 금액(시간당)**| **청구된 금액(월별)**|
|----|----|----|----|----|
|미국 동부|$0.008 |50K|$4|$2,880 |
|일본 동부|$0.009 |50K| $4.50 |$3,240 |
|||종량제|$8.50|$6120|
|구매한 예약 용량|$0.0064(20% 할인) |100RU/초 또는 미리 구매한 $8 용량 |-$8|-$5,760 |
|순 청구|||$0.50 |$360 |

## <a name="next-steps"></a>다음 단계

이제 다음 문서를 사용하여 Azure Cosmos DB의 비용 최적화에 대해 자세히 알아볼 수 있습니다.

* [Cosmos DB 가격 책정 모델이 고객에게 비용 효율적인 방식](total-cost-ownership.md)에 대해 자세히 알아보기
* [개발 및 테스트용으로 최적화](optimize-dev-test.md)에 대한 자세한 정보
* [처리량 비용 최적화](optimize-cost-throughput.md)에 대한 자세한 정보
* [스토리지 비용 최적화](optimize-cost-storage.md)에 대한 자세한 정보
* [읽기 및 쓰기 비용 최적화](optimize-cost-reads-writes.md)에 대한 자세한 정보
* [쿼리 비용 최적화](optimize-cost-queries.md)에 대한 자세한 정보
* [다중 지역 Azure Cosmos 계정 비용 최적화](optimize-cost-regions.md)에 대한 자세한 정보

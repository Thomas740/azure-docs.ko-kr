---
title: Azure Cosmos DB에서 다양한 일관성 수준의 가용성 및 성능 절충
description: Azure Cosmos DB에서 다양한 일관성 수준의 가용성 및 성능 절충
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 09777a9980e4576a5d00123516e33696e845dcac
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65990216"
---
# <a name="consistency-availability-and-performance-tradeoffs"></a>일관성, 가용성 및 성능의 장단점 

고가용성, 낮은 대기 시간 또는 둘 다를 위해 복제에 의존하는 분산 데이터베이스는 절충을 해야 합니다. 읽기 일관성과 가용성, 대기 시간 및 처리량의 절충이 필요합니다.

Azure Cosmos DB는 선택 사항 스펙트럼으로 데이터 일관성에 접근합니다. 이 접근 방식에는 두 가지의 극단적인 강력한 최종 일관성보다 많은 옵션이 포함됩니다. 일관성 스펙트럼의 잘 정의된 5가지 모델 중에서 선택할 수 있습니다. 해당 모델은 가장 강력한 모델부터 가장 약한 모델 순으로 다음과 같습니다.

- *강력*
- *제한된 부실*
- *세션*
- *일관적인 접두사*
- *최종*

각 모델 가용성 및 성능 절충을 제공 하 고 포괄적인 Sla로 지원 됩니다.

## <a name="consistency-levels-and-latency"></a>일관성 수준 및 대기 시간

모든 일관성 수준에 대한 읽기 대기 시간은 항상 99번째 백분위 수에서 10밀리초 미만을 보장합니다. 이 읽기 대기 시간은 SLA로 지원됩니다. 50번째 백분위 수의 평균 읽기 대기 시간은 일반적으로 2밀리초 이하입니다. 여러 Azure 지역에 걸쳐 있으며 강력한 일관성을 사용하여 구성된 Azure Cosmos 계정은 이 보장의 예외입니다.

모든 일관성 수준에 대 한 쓰기 대기 시간을 항상 10 밀리초 미만 99 번째 백분위 수입니다. 이 쓰기 대기 시간은 SLA를 통해 지원됩니다. 50번째 백분위 수의 평균 쓰기 대기 시간은 일반적으로 5밀리초 이하입니다.

둘 이상의 영역을 사용 하 여 강력한 일관성으로 구성 하는 Azure Cosmos 계정에 대 한 쓰기 대기 시간 간에 두 개의 먼 지역에서 백분위 99의 10 밀리초의 미만 두 번 RTT (왕복 시간) 되도록 보장 됩니다.

정확한 RTT 대기 시간은 광속 거리와 Azure 네트워킹 토폴로지의 함수입니다. Azure 네트워킹은 두 Azure 지역 간의 RTT에 대해 대기 시간 SLA를 제공하지 않습니다. Azure Cosmos 계정의 경우 Azure Portal에 복제 대기 시간이 표시됩니다. Azure Cosmos 계정과 연결 된 다양 한 지역 간 복제 대기 시간을 모니터링 하려면 Azure portal (메트릭 블레이드로 이동)를 사용할 수 있습니다.

## <a name="consistency-levels-and-throughput"></a>일관성 수준 및 처리량

- 동일한 요청 단위 수의 경우 세션, 일관된 접두사 및 최종 일관성 수준은 강력하고 제한된 부실에 비해 약 2배의 읽기 처리량을 제공합니다.

- 지정된 쓰기 작업 유형(예: 삽입, 바꾸기, upsert, 삭제 등)의 경우 요청 단위에 대한 쓰기 처리량은 모든 일관성 수준에서 동일합니다.

## <a id="rto"></a>일관성 수준 및 데이터 내구성

전 세계적으로 분산된 데이터베이스 환경에서 지역 전체 가동 중단이 발생할 경우, 일관성 수준과 데이터 내구성 사이에 직접적인 관계가 있습니다. 비즈니스 연속성 계획을 개발할 때는 중단 이벤트가 발생한 후 애플리케이션이 완전히 복구되기까지 허용되는 최대 시간을 이해해야 합니다. 완벽 하 게 복구 하려면 응용 프로그램에 필요한 시간 이라고 **복구 시간 목표** (**RTO**). 또한 중단 이벤트가 발생한 후 복구될 때 애플리케이션에서 손실을 허용할 수 있는 최근 데이터 업데이트의 최대 기간도 이해해야 합니다. 손실을 허용할 수 있는 업데이트의 기간 이라고 **복구 지점 목표** (**RPO**).

아래 표에서 지역 와이드 중단이 있는 경우 일관성 모델 및 데이터 지 속성 간의 관계를 정의 합니다. 분산 시스템에서는 강력한 일관성을 사용 하더라도는 하지는 RPO 및 RTO CAP 정리로 인해 0을 사용 하 여 분산된 된 데이터베이스를 가질 수는 것이 반드시 합니다. 이유에 대 한 자세한 내용은 참조 하세요 [Azure Cosmos DB의 일관성 수준](consistency-levels.md)합니다.

|**지역**|**복제 모드**|**일관성 수준**|**RPO**|**RTO**|
|---------|---------|---------|---------|---------|
|1|단일 또는 다중 마스터|일관성 수준|< 240분|< 1주|
|>1|단일 마스터|세션, 일관된 접두사, 최종|< 15분|< 15분|
|>1|단일 마스터|제한된 부실|*K* & *T*|< 15분|
|>1|단일 마스터|강력|0|< 15분|
|>1|다중 마스터|세션, 일관된 접두사, 최종|< 15분|0|
|>1|다중 마스터|제한된 부실|*K* & *T*|0|

*K* 개수 = *"K"* 항목의 버전 (예: 업데이트) 합니다.

*T* = 시간 간격 *"T"* 마지막 업데이트 이후입니다.

## <a name="next-steps"></a>다음 단계

전역 분산 및 분산 시스템의 일반적인 일관성 절충에 대해 알아봅니다. 다음 문서를 참조하세요.

- [최신 분산 데이터베이스 시스템 디자인의 일관성 절충](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [고가용성](high-availability.md)
- [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)

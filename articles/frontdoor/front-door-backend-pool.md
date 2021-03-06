---
title: Azure 프런트 도어 서비스 백 엔드 및 백 엔드 풀 | Microsoft Docs
description: 이 문서에서는 어떤 백 엔드 및 백 엔드 풀은 도어 구성 앞에 설명 합니다.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 543e237a4a8390a8ebf74d0eb2a1f4be41dcd911
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60193716"
---
# <a name="backends-and-backend-pools-in-azure-front-door-service"></a>Azure Front Door Service의 백 엔드 및 백 엔드 풀
이 문서에서는 Azure 프런트 도어 서비스를 사용 하 여 앱 배포를 매핑하는 방법에 대 한 개념을 설명 합니다. 또한 앱 백 엔드 주위 프런트 도어 구성에서 다양 한 용어를 설명합니다.

## <a name="backends"></a>백 엔드
백 엔드를 영역에는 앱의 배포 인스턴스 하는 것과 같습니다. 첫 번째 관문 서비스 지역 Azure 지역에만 제한 되지 않습니다 있도록 Azure 및 비 Azure 백 엔드 모두를 지원 합니다. 또한 온-프레미스 데이터 센터 또는 다른 클라우드에서 앱 인스턴스 수 있습니다.

첫 번째 관문 서비스 백 엔드 호스트 이름 또는 클라이언트 요청을 처리할 수 있는 앱의 공용 IP를 참조 하십시오. 백 엔드 데이터베이스 계층, 저장소 계층 및 등 프로그램 혼동 해서는 안 됩니다. 백 엔드 앱 백 엔드의 공용 끝점으로 보아야 합니다. 첫 번째 관문 백 엔드 풀에서 백 엔드를 추가 하면 다음 추가 해야 합니다.

- **백 엔드 호스트 유형**합니다. 추가 하려는 리소스의 형식입니다. 첫 번째 관문 서비스에 앱 백 엔드 app service, 클라우드 서비스 또는 저장소에서의 자동 검색을 지원합니다. Azure 또는 비 Azure 백 엔드도에서 다른 리소스를 선택 **사용자 지정 호스트**합니다.

    >[!IMPORTANT]
    >구성 하는 동안 Api 백 엔드를 프런트 도어 환경에서 액세스할 수 없는 경우 유효성을 검사 하지 않습니다. 첫 번째 관문 백 엔드에 연결할 수 있는지 확인 합니다.

- **구독 및 백 엔드 호스트 이름**합니다. 선택 하지 않은 경우 **사용자 지정 호스트** 백 엔드 호스트 형식에 대 한 UI에 적절 한 구독 및 해당 백 엔드 호스트 이름을 선택 하 여 백 엔드를 선택 합니다.

- **백 엔드 호스트 헤더**합니다. 각 요청에 대 한 백 엔드에 전송 하는 호스트 헤더 값입니다. 자세한 내용은 [백 엔드 호스트 헤더](#hostheader)합니다.

- **우선 순위**. 모든 트래픽에 대해 기본 서비스 백 엔드를 사용 하려는 경우에 다른 백 엔드에 우선 순위를 할당 합니다. 또한 기본 또는 백업 백 엔드를 사용할 수 없는 경우 backup을 제공 합니다. 자세한 내용은 [우선 순위](front-door-routing-methods.md#priority)합니다.

- **가중치**합니다. 균일 하 게 또는 가중치 계수에 따라 여러 백 엔드에 트래픽을 분산 하기 위해 다른 백 가중치를 할당 합니다. 자세한 내용은 [가중치](front-door-routing-methods.md#weighted)합니다.

### <a name = "hostheader"></a>백 엔드 호스트 헤더

대상된 리소스를 검색 하는 데 사용 하는 백 엔드는 호스트 헤더 필드를 포함 하는 백 엔드에 첫 번째 관문에서 전달 된 요청입니다. 이 필드의 값은 일반적으로 백 엔드 URI에서 제공되며 호스트 및 포트를 포함합니다.

예를 들어 요청 www\.contoso.com 호스트 헤더 www 해야\.contoso.com입니다. Azure 포털을 사용 하 여 백 엔드를 구성 하는 경우이 필드에 대 한 기본값은 백 엔드의 호스트 이름입니다. 백 엔드는 Azure portal에서 contoso westus.azurewebsites.net 백 엔드 호스트 헤더에 대 한 필드도 값 contoso westus.azurewebsites.net 됩니다. 그러나이 필드를 명시적으로 설정 하지 않고 Azure Resource Manager 템플릿 또는 다른 메서드를 사용 하면 프런트 도어 서비스는 들어오는 호스트 이름을 보내지 값으로 호스트 헤더에 대 한 합니다. Www에 대 한 요청이 수행 된 경우\.contoso.com 및 백 엔드는 빈 헤더 필드를 있는 contoso westus.azurewebsites.net 이면 프런트 도어 서비스 호스트 헤더를 설정 하는 www로\.contoso.com입니다.

대부분의 앱 백 엔드 (Azure Web Apps, Blob storage 및 Cloud Services)는 호스트 헤더를 백 엔드의 도메인과 일치 해야 합니다. 그러나 백 엔드에 라우팅하는 프런트 엔드 호스트 www와 같은 다른 호스트 이름을 사용할지\.contoso.azurefd.net 합니다.

백 엔드 백 엔드 호스트 이름과 일치 하도록 호스트 헤더에 필요한 경우 호스트 이름을 백 엔드에 백 엔드 호스트 헤더를 포함 해야 합니다.

#### <a name="configuring-the-backend-host-header-for-the-backend"></a>백 엔드에 대한 백 엔드 호스트 헤더 구성

구성 하는 **백 엔드 호스트 헤더** 백 엔드의 백 엔드 풀 섹션에서 필드:

1. 첫 번째 관문 리소스를 열고 구성 하려면 백 엔드를 사용 하 여 백 엔드 풀을 선택 합니다.

2. 아직 완료 하지 않은 또는 기존 구성을 편집 하는 경우 백 엔드를 추가 합니다.

3. 사용자 지정 값으로 백 엔드 호스트 헤더 필드를 설정 하거나 비워 둡니다. 들어오는 요청에 대 한 호스트는 호스트 헤더 값으로 사용 됩니다.

## <a name="backend-pools"></a>백 엔드 풀
앞에 도어 서비스 백 엔드 풀을 해당 앱에 대 한 유사한 트래픽을 수신 하는 백 엔드의 집합을 나타냅니다. 즉, 동일한 트래픽을 수신 하 고 예상 되는 동작을 사용 하 여 응답 하는 전 세계에서 앱 인스턴스의 논리적 그룹화 됩니다. 이러한 백 엔드는 여러 지역 간에 또는 동일한 지역 내에 배포 됩니다. 모든 백 엔드 배포 모드 능동/능동 또는 능동/수동 구성으로 정의 될 수 있습니다.

백 엔드 풀 상태 프로브를 통해 다른 백 엔드 계산 방법을 정의 합니다. 또한 간에 부하 분산이 발생 하는 방법을 정의 합니다.

### <a name="health-probes"></a>상태 프로브
첫 번째 관문 서비스 각각에 구성 된 백 엔드에 정기적으로 HTTP/HTTPS 프로브 요청을 보냅니다. 프로브 요청 최종 사용자 요청을 분산 근접 및 로드할 각 백 엔드의 상태를 확인 합니다. 백 엔드 풀에 대 한 상태 프로브 설정 앱 백 엔드의 상태를 폴링할 하는 방법을 정의 합니다. 다음 설정은에 사용할 수 있는 부하 분산 구성:

- **경로**. 백 엔드 풀의 모든 백 엔드에 대 한 프로브 요청에 사용 되는 URL입니다. 예를 들어 백 중 하나는 contoso westus.azurewebsites.net /probe/test.aspx를 설정 하 고 경로 다음 프런트 도어 서비스 환경에는 프로토콜이 HTTP로 설정 된 경우 상태 프로브에 요청을 보냅니다 http\:/ / contoso-westus.azurewebsites.net/probe/test.aspx 합니다.

- **프로토콜**. HTTP 또는 HTTPS 프로토콜을 사용 하 여 백 프런트 도어 서비스에서 상태 프로브 요청을 보낼 것인지를 정의 합니다.

- **간격 (초)** 합니다. 상태 프로브 빈도는 프로브를 보내는 프런트 도어 환경의 각 간격에 백 엔드를 정의 합니다.

    >[!NOTE]
    >더 빠른 장애 조치에 대 한 낮은 값으로 간격을 설정 합니다. 낮을수록 값이 고, 상태 프로브 볼륨이 높을수록 백 수신 합니다. 각 백 엔드에 대 한 수신는 간격을 전역적으로 90 프런트 도어 환경 또는 Pop를 사용 하 여 30 초로 설정 된 경우에 예를 들어, 초당 3 ~ 5 프로브 요청 합니다.

자세한 내용은 [상태 프로브](front-door-health-probes.md)합니다.

### <a name="load-balancing-settings"></a>부하 분산 설정
백 엔드 풀에 대 한 부하 분산 설정 상태 프로브 평가 하는 방법을 정의 합니다. 이러한 설정은 백 엔드 정상 또는 비정상 인지를 결정 합니다. 또한 체크 인할 백 엔드 풀에서 다른 백 엔드 간에 트래픽 부하 분산 하는 방법입니다. 다음 설정은에 사용할 수 있는 부하 분산 구성:

- **샘플 크기**합니다. 백 엔드 상태 평가 대해 고려할 필요 상태 프로브의 얼마나 많은 샘플을 식별 합니다.

- **성공적으로 샘플 크기**합니다. 앞서 언급 한 샘플 크기 정상 백 엔드를 호출 하는 데 필요한 성공한 샘플 수를 정의 합니다. 예를 들어, 첫 번째 관문 상태 프로브 간격은 30 초 이며, 샘플 크기는 5 및 성공적인 샘플 크기는 3을 가정 합니다. 상태 평가 될 때마다 백 엔드에 대 한 프로브를 통해 살펴봅니다 마지막 다섯 개 샘플 150 초 (5 x 30) 이상. 3 개 이상 성공한 프로브로 정상 백 엔드를 선언 해야 합니다.

- **대기 시간 민감도 (추가 대기 시간)** 합니다. 대기 시간 측정 민감도 범위 내에서 백 엔드에 요청을 보내거나 가장 가까운 백 엔드 요청을 전달 하는 출발점이 것인지를 정의 합니다.

자세한 내용은 [최소 대기 시간 기반 라우팅 방법을](front-door-routing-methods.md#latency)합니다.

## <a name="next-steps"></a>다음 단계

- [첫 번째 관문 프로필 만들기](quickstart-create-front-door.md)
- [첫 번째 관문의 작동 원리](front-door-routing-architecture.md)
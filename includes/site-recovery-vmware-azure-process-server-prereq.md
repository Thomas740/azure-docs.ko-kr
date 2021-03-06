---
author: rayne-wiselman
ms.service: site-recovery
ms.topic: include
ms.date: 10/26/2018
ms.author: raynew
ms.openlocfilehash: 96cba4e077be8b7658c270b09b177a845e16c8b0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67182729"
---
이 문서에서는 다음을 가정합니다.

1. 온-프레미스 네트워크와 Azure Virtual Network 간의 **사이트 간 VPN** 또는 **Express 경로**가 이미 설정되었다고 가정합니다.
2. 사용자 계정은 가상 머신을 장애 조치할 Azure 구독에서 새 가상 머신을 만들 수 있는 권한을 보유합니다.
3. 구독에는 최소 4개의 사용 가능한 코어가 포함되어 새로운 프로세스 서버 가상 컴퓨터를 스핀업합니다.
4. **구성 서버 암호**를 사용할 수 있습니다.

> [!TIP]
> 가상 머신을 장애 조치할 Azure Virtual Network에서 구성 서버(온-프레미스 실행)의 포트 443에 연결할 수 있는지 확인합니다.

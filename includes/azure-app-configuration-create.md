---
title: 포함 파일
description: 포함 파일
services: azure-app-configuration
author: yegu
ms.service: azure-app-configuration
ms.topic: include
ms.date: 01/22/2019
ms.author: yegu
ms.custom: include file
ms.openlocfilehash: c98a17be394887ef4e008b079467c85d4ded7e09
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393327"
---
1. 새 App Configuration 저장소를 만들려면 [Azure Portal](https://portal.azure.com)에 로그인합니다. 창의 왼쪽 위 모서리에서 **+리소스 만들기**를 선택합니다. **Marketplace 검색** 상자에서 **App Configuration**을 입력하고 Enter를 선택합니다.

    ![App Configuration 검색](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-new.png)

1. 검색 결과에서 **App Configuration**, **만들기**를 차례로 선택합니다.

1. **App Configuration** > **만들기** 창에서 다음 설정을 입력합니다.

    | 설정 | 제안 값 | 설명 |
    |---|---|---|
    | **리소스 이름** | 전역적으로 고유한 이름 | App Configuration 저장소 리소스에 사용할 고유한 리소스 이름을 입력합니다. 이름은 1~63자의 문자열로, 숫자, 영문자 및 `-` 문자만 포함할 수 있습니다. 이름은 `-` 문자로 시작하거나 끝날 수 없고 연속된 `-` 문자는 유효하지 않습니다.  |
    | **구독** | 사용자의 구독 | App Configuration을 테스트하는 데 사용할 Azure 구독을 선택합니다. 계정에 구독이 하나만 있으면 해당 구독이 자동으로 선택되며 **구독** 목록은 표시되지 않습니다. |
    | **리소스 그룹** | *AppConfigTestResources* | App Configuration 저장소 리소스에 대한 리소스 그룹을 선택하거나 만듭니다. 이 그룹은 리소스 그룹을 삭제하여 여러 리소스를 동시에 삭제할 수 있도록 구성하는 데 유용합니다. 자세한 내용은 [리소스 그룹을 사용하여 Azure 리소스 관리](/azure/azure-resource-manager/resource-group-overview)를 참조하세요. |
    | **위치**: | *미국 중부* | **위치**를 사용하여 앱 구성 저장소를 호스트할 지리적 위치를 지정합니다. 최상의 성능을 얻으려면 애플리케이션의 다른 구성 요소와 동일한 지역에 리소스를 만듭니다. |

    ![App Configuration 저장소 리소스 만들기](../articles/azure-app-configuration/media/quickstarts/azure-app-configuration-create.png)

1. **만들기**를 선택합니다. 배포를 완료하는 데 몇 분 정도 걸릴 수 있습니다.

1. 배포가 완료되면 **설정** > **액세스 키**를 차례로 선택합니다. 기본 읽기 전용 또는 기본 읽기-쓰기 키 연결 문자열 중 하나를 적어 두세요. 이 연결 문자열은 나중에 사용자가 만든 App Configuration 저장소와 통신하도록 애플리케이션을 구성하는 데 사용합니다.

---
title: 개발자를 위한 Azure DevTest Labs 사용 | Microsoft Docs
description: 개발자 시나리오에 Azure DevTest Labs를 사용하는 방법을 알아봅니다.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 22e070e5-3d1a-49fe-9d4c-5e07cb0b7fe2
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 5a293946e4672e7737f912f42511ad0907ba4a81
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61294002"
---
# <a name="use-azure-devtest-labs-for-developers"></a>개발자를 위한 Azure DevTest Labs 사용
Azure DevTest Labs를 사용하여 여러 주요 시나리오를 구현할 수 있지만 기본 시나리오 중 하나는 DevTest Labs를 사용하여 개발자를 위한 개발 컴퓨터를 호스트하는 것입니다. 이 시나리오에서는 DevTest Labs는 다음과 같은 이점을 제공합니다.

- 개발자는 필요에 따라 개발 컴퓨터를 신속하게 프로비전할 수 있습니다.
- 개발자는 필요할 때마다 개발 컴퓨터를 쉽게 사용자 지정할 수 있습니다.
- 관리자는 다음을 확인하여 비용을 제어할 수 있습니다.
  - 개발자는 개발에 필요한 것보다 더 많은 VM을 가져올 수 없습니다.
  - VM은 사용되지 않을 때 종료됩니다. 

![학습에 DevTest Labs 사용](./media/devtest-lab-developer-lab/devtest-lab-developer-lab.png)

이 문서에서는 개발자 요구 사항을 충족하는 데 사용할 수 있는 여러 Azure DevTest Labs 기능과 랩을 설정하는 데 수행할 수 있는 자세한 단계에 대해 학습합니다.

## <a name="implementing-developer-environments-with-azure-devtest-labs"></a>Azure DevTest Labs를 사용하여 개발자 환경 구현
1. **랩 만들기** 
   
    랩은 Azure DevTest Labs의 시작 지점입니다. 랩을 만든 후 랩에 사용자(개발자)를 추가하고, 비용을 제어하는 정책을 설정하고, 신속하게 만들 수 있는 VM 이미지를 정의하는 등의 작업을 수행할 수 있습니다.  
   
    다음 표에 있는 링크를 클릭하면 자세히 알아볼 수 있습니다.
   
   | Task | 학습 내용 |
   | --- | --- |
   | [Azure DevTest Labs에서 랩 만들기](devtest-lab-create-lab.md) |Azure Portal의 Azure DevTest Labs에서 랩을 만드는 방법을 알아봅니다. |
2. **바로 사용할 수 있는 마켓플레이스 이미지 및 사용자 지정 이미지를 사용하여 몇 분 만에 VM 만들기** 
   
    Azure Marketplace의 다양한 이미지에서 바로 사용할 수 있는 이미지를 선택하고 랩에서 사용하도록 할 수 있습니다. 바로 사용할 수 있는 이미지가 요구 사항을 충족하지 않을 경우 Azure Marketplace에서 바로 사용할 수 있는 이미지를 사용하여 랩 VM을 만들고, 필요한 모든 소프트웨어를 설치하고, 랩의 사용자 지정 이미지로 VM을 지정함으로써 사용자 지정 이미지를 만들 수 있습니다.

    사용자 지정 이미지를 사용하는 경우 이미지 팩터리를 사용하여 이미지를 만들고 배포하는 것이 좋습니다. 이미지 팩터리는 구성된 이미지를 자동으로 정기적으로 작성 및 배포하는 "코드로 구성" 솔루션입니다. 이를 사용하면 VM을 기본 OS로 만든 후에 시스템을 수동으로 구성하는 데 필요한 시간이 절감됩니다.
  
    다음 표에 있는 링크를 클릭하면 자세히 알아볼 수 있습니다.
   
   | Task | 학습 내용 |
   | --- | --- |
   | [Azure Marketplace 이미지 구성](devtest-lab-configure-marketplace-images.md) |개발자용으로 원하는 이미지만 선택할 수 있도록 Azure Marketplace 이미지를 허용 목록에 추가할 수 있는 방법을 알아봅니다.|
   | [사용자 지정 이미지 만들기](devtest-lab-create-template.md) |개발자가 사용자 지정 이미지를 사용하여 신속하게 VM을 만들 수 있도록 필요한 소프트웨어를 미리 설치하여 사용자 지정 이미지를 만듭니다.|
   | [이미지 팩터리에 대한 자세한 정보](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/) |이미지 팩터리를 설정 및 사용하는 방법을 설명하는 비디오를 시청하세요.|

3. **개발자 컴퓨터에 대한 재사용 가능 템플릿 만들기** 
   
    Azure DevTest Labs의 수식은 VM을 만드는 데 사용되는 기본 속성 값의 목록입니다. 이미지, VM 크기(CPU와 RAM의 조합) 및 가상 네트워크를 선택하여 랩에서 수식을 만들 수 있습니다. 각 개발자는 랩에서 수식을 보고 수식을 사용하여 VM을 만들 수 있습니다. 
   
    다음 표에 있는 링크를 클릭하면 자세히 알아볼 수 있습니다.
   
   | Task | 학습 내용 |
   | --- | --- |
   | [VM을 만드는 DevTest Labs 수식 관리](devtest-lab-manage-formulas.md) |이미지, VM 크기(CPU와 RAM의 조합) 및 가상 네트워크를 선택하여 수식을 만들 수 있는 방법을 알아봅니다.|

4. **아티팩트를 만들어 유연한 VM 사용자 지정 사용**

   VM이 프로비전된 후 애플리케이션을 배포하고 구성하기 위해 아티팩트가 사용됩니다. 아티팩트는 다음과 같을 수 있습니다.

   - VM에 설치하려는 도구(예: 에이전트, Fiddler 및 Visual Studio)
   - VM에서 실행하려는 작업(예: 리포지토리 복제)
   - 테스트하려는 애플리케이션

   많은 아티팩트는 이미 기본 제공되어 있습니다. 특정 요구에 맞게 추가로 사용자 지정하려는 경우 사용자 고유의 사용자 지정 아티팩트를 만들 수 있습니다.

   다음 표에 있는 링크를 클릭하면 자세히 알아볼 수 있습니다.
   
   | Task | 학습 내용 |
   | --- | --- |
   | [DevTest Lab VM에 대한 사용자 지정 아티팩트 만들기](devtest-lab-artifact-author.md) |랩에서 가상 머신에 대한 사용자 고유의 사용자 지정 아티팩트를 만듭니다.|
   | [Azure DevTest Labs에서 사용하기 위한 사용자 지정 아티팩트 및 Azure Resource Manager 템플릿을 저장할 Git 리포지토리 추가](devtest-lab-add-artifact-repo.md) |사용자 지정 아티팩트를 자체의 프라이빗 Git 리포지토리에 저장하는 방법을 알아봅니다.|

5. **비용 제어**
   
    Azure DevTest Labs를 통해 랩에서 개발자가 만들 수 있는 VM의 최대 수를 지정하는 정책을 랩에 설정할 수 있습니다. 
   
    개발자 팀에 설정된 작업 일정이 있고 하루 중 특정 시간에 모든 VM을 중지했다가 다음 날 자동으로 다시 시작하도록 하려면 랩에서 자동 종료 및 자동 시작 정책을 설정하여 쉽게 수행할 수 있습니다. 
   
    마지막으로 앱 개발이 완료되면 단일 PowerShell 스크립트를 실행하여 모든 VM을 한 번에 삭제할 수 있습니다. 
   
    다음 표에 있는 링크를 클릭하면 자세히 알아볼 수 있습니다.
   
   | Task | 학습 내용 |
   | --- | --- |
   | [랩 정책 정의](devtest-lab-set-lab-policy.md) |랩에 정책을 설정하여 비용을 제어합니다. |
   | [PowerShell 스크립트를 사용하여 모든 랩 VM 삭제](devtest-lab-faq.md#how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab) |개발이 완료되면 한 번에 모든 랩을 삭제합니다.|

1. **VM에 가상 네트워크 추가** 
   
    DevTest Labs는 랩이 생성될 때마다 새 VNET(가상 네트워크)를 만듭니다. 예를 들어 ExpressRoute 또는 사이트 간 VPN을 사용하여 사용자 고유의 VNET을 구성한 경우 VM을 만들 때 사용할 수 있도록 랩의 가상 네트워크 설정에 이 VNET을 추가할 수 있습니다.

    또한 VM이 만들어질 때 VM을 도메인에 가입하는 Azure Active Directory 도메인 가입 아티팩트를 사용할 수 있습니다. 
   
    다음 표에 있는 링크를 클릭하면 자세히 알아볼 수 있습니다.
   
   | Task | 학습 내용 |
   | --- | --- |
   | [Azure DevTest Labs에서 가상 네트워크 구성](devtest-lab-configure-vnet.md) |Azure Portal을 사용하여 Azure DevTest Labs에서 가상 네트워크를 구성하는 방법을 알아봅니다.|

6. **각 개발자와 랩 공유**
   
    개발자와 공유하는 링크를 사용하여 랩에 직접 액세스할 수 있습니다. [Microsoft 계정](devtest-lab-faq.md#what-is-a-microsoft-account)이 있는 개발자는 Azure 계정이 없어도 됩니다. 개발자는 다른 개발자가 만든 VM을 볼 수 없습니다.  
   
    다음 표에 있는 링크를 클릭하면 자세히 알아볼 수 있습니다.
   
   | Task | 학습 내용 |
   | --- | --- |
   | [Azure DevTest Labs에서 랩에 개발자 추가](devtest-lab-add-devtest-user.md) |랩에 개발자를 추가하려면 Azure Portal을 사용합니다.|
   | [PowerShell 스크립트를 사용하여 랩에 개발자 추가](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell) |PowerShell을 사용하여 랩에 개발자를 자동으로 추가합니다. |
   | [랩에 대한 링크 가져오기](devtest-lab-faq.md#how-do-i-share-a-direct-link-to-my-lab) |개발자가 하이퍼링크를 통해 랩에 직접 액세스하는 방법을 알아봅니다.|

7. **추가 팀을 위한 랩 생성 자동화** 
   
    Resource Manager 템플릿을 만들어 동일한 랩을 반복해서 다시 만드는 데 사용하면 사용자 지정 설정을 포함하여 랩 만들기를 자동화할 수 있습니다. 
   
    다음 표에 있는 링크를 클릭하면 자세히 알아볼 수 있습니다.
   
   | Task | 학습 내용 |
   | --- | --- |
   | [Resource Manager 템플릿을 사용하여 랩 만들기](devtest-lab-faq.md#how-do-i-create-a-lab-from-a-resource-manager-template) |Resource Manager 템플릿을 사용하여 Azure DevTest Labs에서 랩을 만듭니다. |

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]


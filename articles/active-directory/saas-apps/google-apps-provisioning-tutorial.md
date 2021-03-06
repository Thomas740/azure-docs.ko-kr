---
title: '자습서: Azure Active Directory로 자동 사용자 프로비전을 위한 G Suite 구성 | Microsoft Docs'
description: 사용자 계정을 Azure AD에서 G Suite로 자동으로 프로비전 및 프로비전 해제하는 방법을 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 6dbd50b5-589f-4132-b9eb-a53a318a64e5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ea1f4d4a6b60961515826a1ba7409bf149b318e8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60277175"
---
# <a name="tutorial-configure-g-suite-for-automatic-user-provisioning"></a>자습서: 자동 사용자 프로비전을 위한 G Suite 구성

이 자습서의 목적은 사용자 계정을 Azure AD(Azure Active Directory)에서 G Suite로 자동으로 프로비전하고 프로비전 해제하는 방법을 보여 주는 것입니다.

> [!NOTE]
> 이 자습서에서는 Azure AD 사용자 프로비저닝 서비스에 기반하여 구축된 커넥터에 대해 설명합니다. 이 서비스의 기능, 작동 방법 및 질문과 대답에 대한 중요한 내용은 [Azure Active Directory를 사용하여 SaaS 애플리케이션의 사용자를 자동으로 프로비저닝 및 프로비저닝 해제](../manage-apps/user-provisioning.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

G Suite와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- G Suite Single Sign-on이 설정된 구독
- Google Apps 구독 또는 Google Cloud Platform 구독.

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [1개월 평가판을 얻을](https://azure.microsoft.com/pricing/free-trial/) 수 있습니다.

## <a name="assign-users-to-g-suite"></a>G Suite에 사용자 할당

Azure Active Directory는 "할당"이라는 개념을 사용하여 어떤 사용자가 선택한 앱에 대한 액세스를 받아야 하는지를 판단합니다. 자동 사용자 계정 프로비전의 컨텍스트에서는 Azure AD의 애플리케이션에 "할당된" 사용자 및 그룹만 동기화됩니다.

프로비전 서비스를 구성 및 사용하도록 설정하기 전에 Azure AD에서 앱에 액세스해야 하는 사용자 또는 그룹을 결정해야 합니다. 결정했으면 [엔터프라이즈 앱에 사용자 또는 그룹 할당](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)의 지침에 따라 사용자를 앱에 할당할 수 있습니다.

> [!IMPORTANT]
> 프로비전 구성을 테스트하기 위해 단일 Azure AD 사용자를 G Suite에 할당하는 것이 좋습니다. 추가 사용자 및 그룹을 나중에 할당할 수 있습니다.
> 
> 사용자를 G Suite에 할당할 때 할당 대화 상자에서 **사용자** 또는 **그룹** 역할을 선택합니다. **기본 액세스** 역할은 프로비전에 작동하지 않습니다.

## <a name="enable-automated-user-provisioning"></a>자동 사용자 프로비전 사용

이 섹션은 Azure AD를 G Suite의 사용자 계정 프로비전 API에 연결하는 과정을 안내합니다. 또한 Azure AD의 사용자 및 그룹 할당을 기반으로 G Suite에 할당된 사용자 계정을 만들고 업데이트하며 사용하지 않도록 프로비전 서비스를 구성할 수도 있습니다.

>[!TIP]
>[Azure Portal](https://portal.azure.com)의 지침에 따라 G Suite에 SAML 기반 Single Sign-On을 사용하도록 선택할 수도 있습니다. Single Sign-On은 자동 프로비전과 별개로 구성할 수 있습니다. 하지만 이 두 가지 기능은 서로 보완적입니다.

### <a name="configure-automatic-user-account-provisioning"></a>자동 사용자 계정 프로비전 구성

> [!NOTE]
> G Suite로 사용자 프로비전을 자동화하는 다른 가능한 옵션은 [GADS(Google Apps Directory Sync)](https://support.google.com/a/answer/106368?hl=en)를 사용하는 것입니다. GADS는 G Suite에 온-프레미스 Active Directory ID를 프로비전하는 것입니다. 반대로 이 자습서의 솔루션은 Azure Active Directory(클라우드) 사용자 및 메일 사용이 가능한 그룹을 G Suite에 프로비전합니다. 

1. 관리자 계정을 사용하여 [Google Apps 관리 콘솔](https://admin.google.com/)에 로그인하고 **보안**을 선택합니다. 링크가 보이지 않으면 화면 아래쪽에 있는 **기타 컨트롤** 메뉴에 숨겨져 있을 수 있습니다.

    ![[보안]을 선택합니다.][10]

1. **보안** 페이지에서 **API 참조**를 선택합니다.

    ![[API 참조]를 선택합니다.][15]

1. **API 액세스 사용**을 선택합니다.

    ![[API 참조]를 선택합니다.][16]

   > [!IMPORTANT]
   > G Suite로 프로비전하려는 모든 사용자에 대해 Azure Active Directory에서 해당 사용자 이름을 사용자 지정 도메인에 연결 *해야* 합니다. 예를 들면 bob@contoso.onmicrosoft.com과 같은 사용자 이름은 G Suite에서 허용되지 않습니다. 반면에 bob@contoso.com은 허용됩니다. Azure AD에서 해당 속성을 편집하여 기존 사용자의 도메인을 변경할 수 있습니다. Azure Active Directory 및 G Suite 모두에 대한 사용자 지정 도메인을 설정하는 방법에 대한 지침이 다음 단계에 포함되어 있습니다.

1. Azure Active Directory에 사용자 지정 도메인 이름을 아직 추가하지 않은 경우에는 다음 단계를 수행합니다.
  
    a. [Azure Portal](https://portal.azure.com)의 왼쪽 탐색 창에서 **Active Directory**를 선택합니다. 디렉터리 목록에서 해당 디렉터리를 선택합니다.

    b. 왼쪽 탐색 창에서 **도메인 이름**을 선택하고 **추가**를 선택합니다.

    ![도메인](./media/google-apps-provisioning-tutorial/domain_1.png)

    ![도메인 추가](./media/google-apps-provisioning-tutorial/domain_2.png)

    다. **도메인 이름** 필드에 사용자 도메인 이름을 입력합니다. 이 도메인 이름은 G Suite에 사용하려는 도메인 이름과 같아야 합니다. 그런 다음 **도메인 추가** 단추를 선택합니다.

    ![도메인 이름](./media/google-apps-provisioning-tutorial/domain_3.png)

    d. **다음**을 선택하여 확인 페이지로 이동합니다. 이 도메인을 소유했는지 확인하려면 이 페이지에 제공된 값에 따라 도메인의 DNS 레코드를 편집합니다. **레코드 종류** 옵션에 대해 선택한 항목에 따라 **MX 레코드** 또는 **TXT 레코드**를 사용하여 확인하도록 선택할 수 있습니다.

    Azure AD에서 도메인 이름을 확인하는 방법에 대한 포괄적인 지침은 [Azure AD에 고유한 도메인 이름 추가](https://go.microsoft.com/fwLink/?LinkID=278919&clcid=0x409)를 참조하세요.

    ![도메인](./media/google-apps-provisioning-tutorial/domain_4.png)

    e. 디렉터리에 추가하려는 모든 도메인에 대해 앞의 단계를 반복합니다.

    > [!NOTE]
    > 사용자 프로비저닝을 위해서는 사용자 지정 도메인이 원본 Azure AD의 도메인 이름과 일치해야 합니다. 일치하지 않으면 특성 매핑 사용자 지정을 구현하여 이 문제를 해결할 수 있습니다.

1. Azure AD에서 모든 도메인을 확인했으므로 Google Apps에서 다시 확인해야 합니다. Google에 아직 등록되지 않은 각 도메인에 대해 다음 단계를 수행합니다.

    a. [Google Apps 관리 콘솔](https://admin.google.com/)에서 **도메인**을 선택합니다.

    ![도메인 선택][20]

    b. **Add a domain or a domain alias(도메인 또는 도메인 별칭 추가)** 를 선택합니다.

    ![새 도메인 추가][21]

    다. **다른 도메인 추가**를 선택하고 추가하려는 도메인 이름을 입력합니다.

    ![사용자의 도메인 이름을 입력합니다.][22]

    d. **Continue and verify domain ownership(계속해서 도메인 소유권 확인)** 을 선택합니다. 그런 다음 도메인 이름을 소유하고 있는지 확인하는 단계를 따릅니다. Google에서 도메인을 확인하는 방법에 대한 포괄적인 지침에 대해서는 [Google Apps에서 사이트 소유권 확인](https://support.google.com/webmasters/answer/35179)(영문)을 참조하세요.

    e. Google Apps에 추가하려는 모든 추가 도메인에 대해 앞의 단계를 반복합니다.

    > [!WARNING]
    > G Suite 테넌트의 주 도메인을 변경하고 Azure AD로 Single Sign-On을 이미 구성한 경우 2단계: Single Sign-On 사용의 3번 항목을 반복해야 합니다.

1. [Google Apps 관리 콘솔](https://admin.google.com/)에서 **관리자 역할**을 선택합니다.

    ![Google Apps 선택][26]

1. 사용자 프로비전을 관리하는 데 사용할 관리자 계정을 결정합니다. 해당 계정의 **관리자 역할**에서 해당 역할에 대한 **권한**을 편집합니다. 이 계정을 프로비전에 사용할 수 있도록 모든 **관리자 API 권한**이 사용하도록 설정되어 있는지 확인합니다.

    ![Google Apps 선택][27]

    > [!NOTE]
    > 프로덕션 환경을 구성하는 경우 특별히 이 단계를 위해 G Suite에 관리자 계정을 만드는 것이 가장 좋습니다. 이러한 계정에는 필요한 API 권한이 있는 계정과 연결된 관리자 역할이 있어야 합니다.

1. [Azure Portal](https://portal.azure.com)에서 **Azure Active Directory** > **엔터프라이즈 앱** > **모든 애플리케이션** 섹션으로 이동합니다.

1. 이미 G Suite에 Single Sign-On을 구성한 경우 검색 필드를 사용하여 G Suite의 인스턴스를 검색합니다. 그렇지 않은 경우 **추가**를 선택하고 애플리케이션 갤러리에서 **G Suite** 또는 **Google Apps**를 검색합니다. 검색 결과에서 앱을 선택하고 애플리케이션 목록에 추가합니다.

1. G Suite 인스턴스를 선택한 다음, **프로비전** 탭을 선택합니다.

1. **프로비전 모드**를 **자동**으로 설정합니다. 

    ![프로비전](./media/google-apps-provisioning-tutorial/provisioning.png)

1. **관리자 자격 증명** 섹션에서 **권한 부여**를 선택합니다. 그러면 새 브라우저 창에서 Google 권한 부여 대화 상자가 열립니다.

1. Azure Active Directory에 G Suite 테넌트를 변경할 권한을 제공할 것인지 확인합니다. **수락**을 선택합니다.

    ![사용 권한을 확인합니다.][28]

1. Azure Portal에서 **연결 테스트**를 선택하여 Azure AD가 앱에 연결할 수 있는지 확인합니다. 연결에 실패하면 G Suite 계정에 팀 관리자 권한이 있는지 확인합니다. 그런 다음 **권한 부여** 단계를 다시 시도합니다.

1. 프로비전 오류 알림을 받을 개인 또는 그룹의 이메일 주소를 **알림 메일** 필드에 입력합니다. 그런 다음 확인란을 선택합니다.

1. **저장**을 선택합니다.

1. **매핑** 섹션에서 **Synchronize Azure Active Directory Users to Google Apps(Azure Active Directory 사용자를 Google Apps에 동기화)** 를 선택합니다.

1. **특성 매핑** 섹션에서 Azure AD에서 G Suite로 동기화되는 사용자 특성을 검토합니다. **일치** 속성인 특성은 업데이트 작업 시 G Suite의 사용자 계정을 일치시키는 데 사용됩니다. 변경 내용을 커밋하려면 **저장**을 선택합니다.

1. G Suite에 대한 Azure AD 프로비전 서비스를 사용하도록 설정하려면 **설정**에서 **프로비전 상태**를 **켜기**로 변경합니다.

1. **저장**을 선택합니다.

이 프로세스는 사용자 및 그룹 섹션에서 G Suite에 할당된 사용자 또는 그룹의 초기 동기화를 시작합니다. 초기 동기화는 서비스가 실행되는 동안 약 40분마다 발생하는 차후 동기화보다 더 많은 시간이 걸립니다. **동기화 세부 정보** 섹션을 사용하여 진행 상태를 모니터링하고 링크를 통해 프로비전 작업 로그를 확인할 수 있습니다. 이러한 로그는 앱에서 프로비저닝 서비스가 수행하는 모든 조치를 설명합니다.

Azure AD 프로비저닝 로그를 읽는 방법에 대한 자세한 내용은 [자동 사용자 계정 프로비저닝에 대한 보고](../manage-apps/check-status-user-account-provisioning.md)를 참조하세요.

## <a name="additional-resources"></a>추가 리소스

* [엔터프라이즈 앱에 대한 사용자 계정 프로비전 관리](tutorial-list.md)
* [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)
* [Single Sign-on 구성](google-apps-tutorial.md)

<!--Image references-->

[10]: ./media/google-apps-provisioning-tutorial/gapps-security.png
[15]: ./media/google-apps-provisioning-tutorial/gapps-api.png
[16]: ./media/google-apps-provisioning-tutorial/gapps-api-enabled.png
[20]: ./media/google-apps-provisioning-tutorial/gapps-domains.png
[21]: ./media/google-apps-provisioning-tutorial/gapps-add-domain.png
[22]: ./media/google-apps-provisioning-tutorial/gapps-add-another.png
[24]: ./media/google-apps-provisioning-tutorial/gapps-provisioning.png
[25]: ./media/google-apps-provisioning-tutorial/gapps-provisioning-auth.png
[26]: ./media/google-apps-provisioning-tutorial/gapps-admin.png
[27]: ./media/google-apps-provisioning-tutorial/gapps-admin-privileges.png
[28]: ./media/google-apps-provisioning-tutorial/gapps-auth.png

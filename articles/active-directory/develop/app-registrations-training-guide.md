---
title: Azure 포털 교육 가이드-Azure에에서 앱 등록
description: 디바이스 코드 부여를 사용하여 브라우저 없는 포함된 인증 흐름을 빌드하세요.
services: active-directory
documentationcenter: ''
author: archieag
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: aragra
ms.reviewer: lenalepa, keyam
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 118c6ecb16d325a384246a0b3d9e685f6f6f04ee
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64870119"
---
# <a name="training-guide-app-registrations-in-the-azure-portal"></a>학습 가이드: Azure portal에서 앱 등록  

새 다양 한 향상 된 기능을 찾을 수 있습니다 [앱 등록](https://go.microsoft.com/fwlink/?linkid=2083908) Azure portal에서 발생 합니다. 레거시 경험을 사용 하 여 보다 친숙 한 경우 새 환경을 사용 하 여 시작 하는 데이 학습 가이드를 사용 합니다.

## <a name="key-changes"></a>키 변경

- 앱 등록에는 두 가지 제한 되지 않습니다.는 **웹 앱/a p I** 또는 **네이티브** 앱. 각 리디렉션 Uri를 등록 하 여 이러한 모든 동일한 앱 등록을 사용할 수 있습니다.
- 레거시 환경 지원 앱에 로그인 하는 조직 (Azure AD) 계정만 합니다. 앱은 단일 테 넌 트 (지원 앱에 등록 된 디렉터리에서만 조직 계정)으로 등록 된 다중 테 넌 트 (모든 조직 계정 지원)를 수정할 수 없습니다. 새로운 환경의 사용 하면 앱 수 이러한 옵션을 모두 지원 뿐만 아니라 세 번째 옵션을 등록할 수 있습니다: 모든 조직 계정 뿐 아니라 개인 Microsoft 계정입니다.
- 레거시 경험 조직 계정을 사용 하 여 Azure portal에 로그인 할 때 사용할 수 있었습니다. 새 환경 디렉터리와 연결 되어 있지 않은 개인 Microsoft 계정을 사용할 수 있습니다.

## <a name="list-of-applications"></a>애플리케이션 목록

- 레거시 앱을 통해 등록 된 응용 프로그램을 표시 하는 새 앱은 앱 뿐만 아니라 Azure 포털 (Azure AD 계정에 로그인 하는 앱)에 등록 환경을 통해 등록 합니다 [응용 프로그램 등록 포털](https://apps.dev.microsoft.com/) (Azure AD에 로그인 하는 앱과 개인용 Microsoft 계정).
- 새 앱 목록에 없는 **응용 프로그램 종류** 열 (단일 앱을 등록 하는 여러 유형이 될 수 있음) 하므로 두 개의 추가 열 및:는 **에서 만든** 열 및 **인증서 & 비밀** 앱에서 등록 된 자격 증명 (현재, 곧 만료 되거나 만료 된) 상태를 표시 하는 열입니다.

## <a name="new-app-registration"></a>새 앱 등록

레거시 환경에서 앱을 등록 해야 제공: **이름을**, **응용 프로그램 종류**, 및 **Sign on URL/리디렉션 URI**합니다. 생성 된 앱에는 앱에 등록 된 디렉터리에서 조직 계정 에서만 지원 의미 하는 Azure AD만 단일 테 넌 트 응용 프로그램 이었습니다.

새 환경에서 제공 해야 합니다는 **이름** 앱에 대 한 선택 합니다 **지원 되는 계정 유형**합니다. 선택적으로 제공할 수는 **리디렉션 URI**합니다. 리디렉션 URI를 제공 하는 경우에 웹/공용 (모바일 및 데스크톱) 인지를 지정 해야 합니다. 환경을 새 앱 등록을 사용 하 여 앱을 등록 하는 방법에 대 한 자세한 내용은 [이 빠른 시작](quickstart-register-app.md)합니다.

## <a name="the-legacy-properties-page"></a>레거시 속성 페이지

레거시 경험 했습니다를 **속성** 새로운 환경을 없는 페이지입니다. 합니다 **속성** 블레이드에서 다음 필드를 했습니다. **이름을**, **개체 ID**, **응용 프로그램 ID**를 **앱 ID URI**를 **로고**를 **홈 페이지 URL** 를 **로그 아웃 URL**를 **서비스 약관 URL**를 **개인정보 처리 방침 URL**를 **응용 프로그램 종류**, 및  **다중 테 넌 트입니다.**

새 환경에서 동일한 기능을 찾을 수 있는 다음과 같습니다.

- **이름을**, **로고**를 **홈페이지 URL**를 **서비스 약관 URL**, 및 **개인정보 처리 방침 URL** 은 앱의  **브랜딩** 페이지입니다.
- **개체 ID** 하 고 **(클라이언트) 응용 프로그램 ID** 켜져 합니다 **개요** 페이지입니다.
- 에 의해 제어 기능을 **다중 테 넌 트** 레거시 환경에서 설정/해제로 대체 되었습니다 **지원 되는 계정 유형** 에 **인증** 페이지입니다. 다중 테 넌 트 지원 되는 계정 유형 옵션에 매핑하는 방법에 대 한 자세한 내용은 참조 하세요. [이 빠른 시작](quickstart-modify-supported-accounts.md)합니다.
- **로그 아웃 URL** 이제 켜져 합니다 **인증** 페이지입니다.
- **응용 프로그램 유형을** 는 더 이상 유효한 필드입니다. 대신, 리디렉션 Uri (에서 찾을 수 있는 합니다 **인증** 페이지) 앱 유형을 지원 되는지 확인 합니다.
- **앱 ID URI** 라고 **응용 프로그램 ID URI** 에서 찾을 수 있습니다 합니다 **API를 노출** 블레이드입니다. 레거시 환경에서이 속성 자동-등록 된 형식: `https://{tenantdomain}/{appID}` (예를 들어 `https://microsoft.onmicrosoft.com/aeb4be67-a634-4f20-9a46-e0d4d4f1f96d`). 새 형식에서으로 자동으로 생성 됩니다 `api://{appID}`, 하지만 명시적으로 저장 해야 합니다.

## <a name="reply-urlsredirect-urls"></a>회신 Url/리디렉션 Url

레거시 환경에서 앱 했습니다를 **회신 Url** 페이지입니다. 새로운 환경에 회신 Url을 앱에서 찾을 수 있습니다 **인증** 섹션입니다. 또한 이러한 이라고 **리디렉션 Uri**합니다. 또한 리디렉션에 대 한 형식 Uri 변경 되었습니다. 앱 유형 (웹 또는 공용)으로 연결 되도록 필요한 경우 또한 보안상의 이유로 와일드 카드 및 http:// 스키마는 지원 되지 않습니다 (제외 http://localhost) 합니다.

## <a name="keyscertificates--secrets"></a>키/인증서 및 암호

레거시 환경에서 앱 했습니다 **키** 페이지입니다. 새 환경에서 해당로 바뀌었습니다 **인증서 및 비밀**합니다. 또한 **공개 키** 이라고 **인증서** 하 고 **암호** 이라고 **클라이언트 비밀**합니다.

## <a name="required-permissionsapi-permissions"></a>필요한 사용 권한/API 사용 권한

- 레거시 환경에서 앱 했습니다를 **필요한 권한** 페이지입니다. 새 환경에서 해당로 바뀌었습니다 **API 사용 권한**합니다.
- 레거시 환경에서 API를 선택할 때 Microsoft Api 또는 테 넌 트에서 서비스 주체를 통해 검색 작은 목록에서 선택할 수 있습니다. 새 환경에서 여러 탭에서 선택할 수 있습니다. **Microsoft Api**하십시오 **내 조직에서 사용 하는 Api**, 또는 **내 Api**합니다. 에 있는 검색 표시줄 **내 조직의 Api** 테 넌 트에서 서비스 주체를 통해 검색 탭을 사용 합니다. 

   > [!NOTE]
   > 응용 프로그램 테 넌 트와 연결 되지 경우이 탭이 나타나지 않습니다. 새 환경을 사용 하 여 사용 권한을 요청 하는 방법에 대 한 자세한 내용은 참조 하세요. [이 빠른 시작](quickstart-configure-app-access-web-apis.md)합니다.

- 레거시 경험 했습니다를 **권한 부여** 맨 위에 있는 단추를 **사용 권한을 요청한** 페이지입니다. 새 환경, 즉를 **동의 부여할** 섹션을 **관리자 동의 부여** 앱의 단추 **API 사용 권한** 섹션입니다. 또한 일부의 차이점이 같은 방법으로 단추 함수:
   - 레거시 경험을 논리는 로그인된 한 사용자 및 요청 된 권한에 따라 달라 집니다. 논리는 다음과 같습니다.
      - 사용자 동의 가능 권한을 요청한 되 고 로그인 하 고 사용자가 관리자, 사용자가 요청한 사용 권한에 대 한 사용자 동의 부여할 수 있습니다.
      - 관리자 동의가 필요한 권한을 하나 이상 요청 되 고 로그인 하 고 사용자가 관리자, 사용자 동의 부여 하는 동안 오류가 발생 합니다.
      - 로그인된 한 사용자의 관리자 인 경우 요청 된 모든 사용 권한은 관리자 동의 허가 되었습니다.
   - 새 환경에서 관리자만 동의 부여할 수 있습니다. 관리자가 선택 하면 합니다 **관리자 동의 부여** 단추 관리자 동의 요청된 된 모든 권한이 부여 됩니다.

## <a name="deleting-an-app-registration"></a>앱 등록 삭제

레거시 환경에서 앱은 단일 테 넌 트를 삭제할 수 해야 했습니다. 삭제 단추는 다중 테 넌 트 앱에 대 한 비활성화 되었습니다. 새 환경에서 앱의 모든 상태를 삭제할 수는 있지만 작업을 확인 해야 합니다. 앱 등록을 삭제 하는 방법에 대 한 자세한 내용은 참조 하세요. [이 빠른 시작](quickstart-remove-app.md)합니다.

## <a name="application-manifest"></a>애플리케이션 매니페스트

기존 및 신규 환경을 매니페스트 편집기에서 json 형식에 대해 서로 다른 버전을 사용 합니다. 자세한 내용은 [애플리케이션 매니페스트](reference-app-manifest.md)를 참조하세요.

## <a name="new-ui"></a>새 UI

이전에 설정할 수는 매니페스트 편집기 또는 API를 사용 하 여 또는 존재 하지는 속성에 대 한 새로운 UI 있습니다.

- **암시적 권한 부여 흐름** (하도록 합니다 (oauth2allowimplicitflow)에서 확인할 수 있습니다 합니다 **인증** 페이지입니다. 달리 레거시 경험을 하도록 설정할 수 있습니다 **액세스 토큰** 하거나 **id 토큰**, 또는 둘 다.
- **이 API에 정의 된 범위** (oauth2Permissions) 및 **클라이언트 응용 프로그램 권한이** (preAuthorizedApplications)를 통해 구성할 수 있습니다 합니다 **API를 노출** 페이지입니다. Web API 사용 권한/범위를 노출 하는 앱을 구성 하는 방법에 대 한 자세한 내용은 참조 하세요. [이 빠른 시작](quickstart-configure-app-expose-web-apis.md)합니다.
- **게시자 도메인** (에 사용자에 게 표시 되는 합니다 [응용 프로그램의 동의 확인 프롬프트](application-consent-experience.md))에서 찾을 수 있습니다 합니다 **브랜드 블레이드에서** 페이지. 게시자 도메인을 구성 하는 방법에 대 한 자세한 내용은 참조 하세요. [이 방법은](howto-configure-publisher-domain.md)합니다.

## <a name="limitations"></a>제한 사항

새 환경에 다음과 같은 제한 사항이 있습니다.

- 새 환경은 현재 사용할 수 없는 경우 Azure AD B2C 테 넌 트에서
- 클라이언트 암호 (앱 암호)의 형식은 레거시 경험 다르므로 하며 CLI 중단 합니다.
- 지원 되는 계정에 대 한 값을 변경 UI에서 지원 되지 않습니다. 단일 테 넌 트 및 다중 테 넌 트의 Azure AD 간의 전환 하는 경우가 아니면 응용 프로그램 매니페스트를 사용 해야 합니다.

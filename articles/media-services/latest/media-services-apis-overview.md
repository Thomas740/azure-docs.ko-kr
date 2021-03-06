---
title: V3 Api-Azure 사용 하 여 개발 | Microsoft Docs
description: 이 문서에서는 Media Services v3을 사용 하 여 개발 하는 경우 엔터티 및 Api에 적용 되는 규칙을 설명 합니다.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/02/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: ca4e343ea4774bbe4ff992ad671575b150b3c045
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65890974"
---
# <a name="developing-with-media-services-v3-apis"></a>V3 Api를 Media Services를 사용 하 여 개발

개발자인 경우 Media Services [REST API](https://aka.ms/ams-v3-rest-ref) 또는 REST API와 상호 작용할 수 있도록 하는 클라이언트 라이브러리를 사용하여 사용자 지정 미디어 워크플로를 손쉽게 만들고, 관리하고 유지할 수 있습니다. [Media Services v3](https://aka.ms/ams-v3-rest-sdk) API는 OpenAPI 사양(이전 명칭 Swagger)에 기반을 두고 있습니다.

이 문서에서는 Media Services v3을 사용 하 여 개발 하는 경우 엔터티 및 Api에 적용 되는 규칙을 설명 합니다.

## <a name="accessing-the-azure-media-services-api"></a>Azure Media Services API 액세스

Media Services 리소스 및 Media Services API에 액세스할 수 있는 권한을 부여하려면 먼저 인증을 거쳐야 합니다. Media Services에서 지 원하는 [Azure Active Directory (Azure AD)-기반](../../active-directory/fundamentals/active-directory-whatis.md) 인증 합니다. 두 일반적인 인증 옵션은
 
* **서비스 주체 인증** -서비스를 인증 하는 데 사용 (예: 웹 앱, 함수 앱, 논리 앱, API 및 마이크로 서비스). 이 인증 방법을 일반적으로 사용하는 애플리케이션은 디먼 서비스, 중간 계층 서비스 또는 예약된 작업을 실행하는 앱입니다. 예를 들어 웹용 응용 프로그램에 있는 해야 서비스 주체로 Media Services에 연결 하는 중간 계층.
* **사용자 인증** -Media Services 리소스와 상호 작용 하는 앱 사용 하는 사용자를 인증 하는 데 사용 합니다. 대화형 애플리케이션은 먼저 사용자에게 사용자의 자격 증명을 묻는 메시지를 표시합니다. 예제는 권한 있는 사용자가 인코딩 작업 또는 라이브 스트리밍을 모니터링하기 위해 사용한 관리 콘솔 앱입니다.

Media Services API에 필요한 사용자 또는 응용 프로그램은 REST API 요청 Media Services 계정 리소스에 액세스할 수 있으며 사용을 **참여자** 또는 **소유자** 역할입니다. API를 사용 하 여 액세스할 수 있습니다는 **판독기** 역할에만 **가져오기** 하거나 **목록**   작업을 사용할 수 있습니다. 자세한 내용은 [Media Services 계정에 대 한 역할 기반 액세스 제어](rbac-overview.md)입니다.

서비스 주체를 만드는 대신 Azure 리소스에 대 한 관리 되는 id를 사용 하 여 Azure Resource Manager를 통해 Media Services API에 액세스 하는 것이 좋습니다. Azure 리소스에 대 한 관리 되는 id에 대 한 자세한 내용은 참조 하세요 [Azure 리소스에 대 한 관리 되는 id 란](../../active-directory/managed-identities-azure-resources/overview.md)합니다.

### <a name="azure-ad-service-principal"></a>Azure AD 서비스 주체 

Azure AD 응용 프로그램 및 서비스 주체를 만드는 경우 응용 프로그램은 자체 테 넌 트입니다. 응용 프로그램을 만든 후 앱을 제공 **참여자** 하거나 **소유자** Media Services 계정에 대 한 역할 액세스 합니다. 

내용은 Azure AD 응용 프로그램을 만들 수 있는 권한이 있는지 확실 하지 않은 경우 [필요한 권한](../../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)합니다.

다음 그림에서는 숫자 순서로 요청 흐름을을 나타냅니다.

![중간 계층 앱](./media/use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

1. 중간 계층 앱에는 다음 매개 변수가 있는 Azure AD 액세스 토큰을 요청 합니다.  

   * Azure AD 테넌트 엔드포인트.
   * Media Services 리소스 URI.
   * REST Media Services의 리소스 URI.
   * Azure AD 애플리케이션 값: 클라이언트 ID 및 클라이언트 암호.
   
   필요한 모든 값을 가져오려면를 참조 하세요. [Azure CLI를 사용 하 여 Azure Media Services API 액세스](access-api-cli-how-to.md)

2. Azure AD 액세스 토큰이 중간 계층으로 전송됩니다.
4. 중간 계층은 Azure AD 토큰과 함께 Azure Media REST API로 요청을 보냅니다.
5. 중간 계층은 Media Services에서 데이터를 다시 가져옵니다.

### <a name="samples"></a>샘플

Azure AD 서비스 주체를 사용 하 여 연결 하는 방법을 보여주는 다음 샘플을 참조 하세요.

* [REST를 사용 하 여 연결](media-rest-apis-with-postman.md)  
* [Java로 연결](configure-connect-java-howto.md)
* [.NET으로 연결](configure-connect-dotnet-howto.md)
* [Node.js로 연결](configure-connect-nodejs-howto.md)
* [Python으로 연결](configure-connect-python-howto.md)

## <a name="naming-conventions"></a>명명 규칙

Azure Media Services v3 리소스 이름(예: 자산, 작업, 변환)은 Azure Resource Manager 명명 제약 조건에 따라 달라집니다. Azure Resource Manager에 따라 리소스 이름은 항상 고유합니다. 따라서 리소스 이름에 대해 고유 식별자 문자열(예: GUID)을 사용할 수 있습니다. 

Media Services 리소스 이름에는 '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', 작은 따옴표 또는 제어 문자가 포함될 수 없습니다. 다른 문자를 모두 허용합니다. 리소스 이름의 최대 길이는 260자입니다. 

Azure Resource Manager 이름 지정에 대한 자세한 내용은 [명명 요구 사항](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) 및 [명명 규칙](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions)을 참조하세요.

## <a name="long-running-operations"></a>장기 실행 작업

작업 표시 `x-ms-long-running-operation` Azure Media services에서 [swagger 파일](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/streamingservice.json) 긴 작업을 실행 합니다. 

Azure 비동기 작업을 추적 하는 방법에 대 한 자세한 내용은 참조 하세요 [비동기 작업](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations#monitor-status-of-operation)합니다.

Media Services에는 다음과 같은 장기 실행 작업에 있습니다.

* LiveEvent 만들기
* LiveEvent 업데이트
* LiveEvent 삭제
* LiveEvent 시작
* LiveEvent 중지
* LiveEvent 다시 설정
* LiveOutput 만들기
* LiveOutput 삭제
* StreamingEndpoint 만들기
* StreamingEndpoint 업데이트
* StreamingEndpoint 삭제
* StreamingEndpoint 시작
* StreamingEndpoint 중지
* StreamingEndpoint 크기 조정


## <a name="sdks"></a>SDK

> [!NOTE]
> Azure Media Services v3 SDK는 스레드로부터 안전을 보장하지 않습니다. 다중 스레드 애플리케이션을 개발하는 경우 클라이언트를 보호하는 고유한 스레드 동기화 논리를 추가하거나 스레드별로 새 AzureMediaServicesClient 개체를 사용해야 합니다. 코드에서 클라이언트에 제공하는 선택적 개체(예: .NET의 HttpClient 인스턴스) 때문에 다중 스레드 문제가 도입될 수 있다는 점에도 주의해야 합니다.

|SDK)|참조|
|---|---|
|[.NET SDK](https://aka.ms/ams-v3-dotnet-sdk)|[.NET 참조](https://aka.ms/ams-v3-dotnet-ref)|
|[Java SDK](https://aka.ms/ams-v3-java-sdk)|[Java 참조](https://aka.ms/ams-v3-java-ref)|
|[Python SDK](https://aka.ms/ams-v3-python-sdk)|[Python 예제](https://aka.ms/ams-v3-python-ref)|
|[Node.js SDK](https://aka.ms/ams-v3-nodejs-sdk) |[Node.js 예제](https://aka.ms/ams-v3-nodejs-ref)| 
|[SDK로 이동](https://aka.ms/ams-v3-go-sdk) |[참조로 이동](https://aka.ms/ams-v3-go-ref)|
|[Ruby SDK](https://aka.ms/ams-v3-ruby-sdk)||

### <a name="see-also"></a>참고 항목

- [미디어 서비스 이벤트를 포함하는 EventGrid .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.EventGrid/)
- [Media Services 이벤트의 정의](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/eventgrid/data-plane/Microsoft.Media/stable/2018-01-01/MediaServices.json)

## <a name="azure-media-services-explorer"></a>Azure Media Services 탐색기

[Azure Media Services 탐색기](https://github.com/Azure/Azure-Media-Services-Explorer)(AMSE)는 Media Services에 대해 알아보고자 하는 Windows 고객이 사용할 수 있는 도구입니다. AMSE는 Media Services에서 VOD 및 실시간 콘텐츠의 업로드, 다운로드, 인코딩, 스트리밍을 수행하는 Winforms/C# 애플리케이션입니다. AMSE 도구는 코드를 작성하지 않고 Media Services를 테스트하려는 고객을 위한 도구입니다. AMSE 코드는 Media Services를 사용하여 개발하려는 고객을 위한 리소스로 제공됩니다.

AMSE는 오픈 소스 프로젝트이며, 커뮤니티에서 지원을 제공합니다(문제는 https://github.com/Azure/Azure-Media-Services-Explorer/issues) 에 보고할 수 있음). 이 프로젝트에는 [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/)(Microsoft 오픈 소스 준수 사항)이 적용됩니다. 자세한 내용은 [Code of Conduct FAQ(준수 사항 FAQ)](https://opensource.microsoft.com/codeofconduct/faq/)를 참조하거나 opencode@microsoft.com에 추가 질문 또는 의견을 알려주세요.

## <a name="filtering-ordering-paging-of-media-services-entities"></a>Media Services 엔터티 필터링, 순서 지정, 페이징

참조 [필터링, 정렬, 페이징 Azure Media Services 엔터티](entities-overview.md)

## <a name="ask-questions-give-feedback-get-updates"></a>질문, 피드백 제공, 업데이트 받기

[Azure Media Services 커뮤니티](media-services-community.md) 문서를 체크 아웃하여 다양한 방법으로 질문을 하고, 피드백을 제공하고, Media Services에 대한 업데이트를 가져올 수 있습니다.

## <a name="see-also"></a>참고 항목

[Azure CLI](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

## <a name="next-steps"></a>다음 단계

* [Java 사용 하 여 Media Services에 연결](configure-connect-java-howto.md)
* [.NET으로 Media Services에 연결](configure-connect-dotnet-howto.md)
* [Node.js 사용 하 여 Media Services에 연결](configure-connect-nodejs-howto.md)
* [Python 사용 하 여 Media Services에 연결](configure-connect-python-howto.md)

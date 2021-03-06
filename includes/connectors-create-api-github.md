---
title: 포함 파일
description: 포함 파일
services: logic-apps
author: MandiOhlinger
ms.service: logic-apps
ms.topic: include
ms.date: 03/02/2018
ms.author: mandia
ms.custom: include file
ms.openlocfilehash: 11280e1678f52ede928cb2a85ea83add222e15fa
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67182466"
---
1. [Azure Portal](https://portal.azure.com)에서 빈 논리 앱을 만듭니다. 

2. 논리 앱 디자이너에서 필터로 "github"를 입력합니다. 

3. GitHub 커넥터 및 사용하려는 트리거를 선택합니다.

   ![GitHub 커넥터 및 트리거 선택](./media/connectors-create-api-github/github-connector.png)

   > [!NOTE]
   > 모든 논리 앱 워크플로에서 트리거를 시작해야 합니다. 논리 워크플로에서 트리거를 이미 시작한 경우에만 작업을 선택할 수 있습니다. 

4. 이전에 연결을 만들지 않은 경우 **로그인**을 선택하여 메시지가 표시될 때 GitHub 자격 증명을 제공할 수 있습니다.  

   ![GitHub 자격 증명으로 로그인](./media/connectors-create-api-github/github-connector-sign-in-credentials.png)

   논리 앱에서 이러한 자격 증명을 사용하여 GitHub 계정에 대한 데이터 연결 및 액세스 권한을 부여합니다. 

5. GitHub 사용자 이름과 암호를 제공한 다음, 권한 부여를 확인합니다.

   ![자격 증명 제공 및 권한 부여 확인](./media/connectors-create-api-github/github-connector-authorize.png)   

   이제 Azure Portal에 연결되어 사용할 준비가 되었습니다.

6. 논리 앱 워크플로를 계속 정의합니다.

   ![논리 앱 워크플로에 더 많은 작업 추가](./media/connectors-create-api-github/github-connector-logic-app.png)


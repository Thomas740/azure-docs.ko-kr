---
title: 포함 파일
description: 포함 파일
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 09/07/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: b2bce9788006a564def9bd8c1375a85dc4184b67
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66814749"
---
## <a name="create-a-device-identity"></a>디바이스 ID 만들기

이 섹션에서는 Azure CLI를 사용하여 이 자습서의 디바이스 ID를 만듭니다. Azure CLI에 미리 설치 되는 [Azure Cloud Shell](~/articles/cloud-shell/overview.md), 할 수 있습니다 [Azure CLI를 로컬로 설치](/cli/azure/install-azure-cli)합니다. 디바이스 ID는 대/소문자를 구분합니다.

1. Azure CLI를 사용하여 IoT 확장을 설치하는 명령줄 환경에서 다음 명령을 실행합니다.

    ```cmd/sh
    az extension add --name azure-cli-iot-ext
    ```

1. Azure CLI를 로컬로 실행하는 경우 다음 명령을 사용하여 Azure 계정에 로그인합니다(Cloud Shell을 사용하는 경우 자동으로 로그인되므로 이 명령을 실행할 필요가 없음).

    ```cmd/sh
    az login
    ```

1. 마지막으로 `myDeviceId`라는 새 디바이스 ID를 만들고 다음 명령으로 디바이스 연결 문자열을 검색합니다.

    ```cmd/sh
    az iot hub device-identity create --device-id myDeviceId --hub-name {Your IoT Hub name}
    az iot hub device-identity show-connection-string --device-id myDeviceId --hub-name {Your IoT Hub name} -o table
    ```

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

결과에서 디바이스 연결 문자열을 기록해 둡니다. 이 디바이스 연결 문자열은 디바이스 앱이 디바이스로 IoT Hub에 연결하는 데 사용됩니다.

<!-- images and links -->

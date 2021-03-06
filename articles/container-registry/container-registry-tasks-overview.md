---
title: 빌드 및 Azure 컨테이너 레지스트리 작업 (ACR 작업)를 사용 하 여 컨테이너 이미지를 패치 자동화
description: 보안, 자동화 된 컨테이너 이미지 빌드, 관리 및 패치를 클라우드에서 제공 하는 Azure Container Registry의 기능 모음인 ACR 작업에 대해 소개 합니다.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/12/2019
ms.author: danlep
ms.openlocfilehash: 5089650996693b81e548bba8d89c0de29a8afd93
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147978"
---
# <a name="automate-container-image-builds-and-maintenance-with-acr-tasks"></a>컨테이너 이미지 빌드 및 ACR 작업을 사용 하 여 유지 관리를 자동화 합니다.

컨테이너는 새 수준의 가상화를 제공하여 인프라 및 운영 요구 사항에서 애플리케이션 및 개발자 종속성을 격리합니다. 남아 것 인데,이 응용 프로그램 가상화 관리 되 고 컨테이너 수명 주기 동안 패치는 어떻게 해결 해야 합니다.

## <a name="what-is-acr-tasks"></a>ACR 작업이란?

**ACR 작업**은 Azure Container Registry 내의 기능 모음입니다. Linux, Windows 및 ARM용 클라우드 기반 컨테이너 이미지 빌드를 제공하며 Docker 컨테이너용 [OS 및 프레임워크 패치](#automate-os-and-framework-patching)를 자동화할 수 있습니다. ACR 작업은 주문형 컨테이너 이미지 빌드를 사용하여 “내부 루프” 개발 주기를 클라우드로 확장할 뿐 아니라 소스 코드 커밋 시 또는 컨테이너의 기본 이미지가 업데이트될 때 자동화된 빌드를 가능하게 합니다. 기본 이미지 업데이트 트리거를 사용하면 OS 및 애플리케이션 프레임워크 패치 워크플로를 자동화하여 변경할 수 없는 컨테이너의 보안 주체를 준수하면서 보안 환경을 유지 관리할 수 있습니다.

다음 네 가지 방법으로 ACR 작업을 사용하여 컨테이너 이미지를 빌드하고 테스트합니다.

* [빠른 작업](#quick-task): 로컬 Docker 엔진을 설치하지 않고도 Azure에서 요청 시 컨테이너 이미지를 빌드하고 푸시합니다. 클라우드의 `docker build`, `docker push`를 생각하면 됩니다. 로컬 소스 코드 또는 Git 리포지토리에서 빌드합니다.
* [소스 코드 커밋 시 빌드](#automatic-build-on-source-code-commit): 코드가 Git 리포지토리에 커밋될 때 컨테이너 이미지 빌드를 자동으로 트리거합니다.
* [기본 이미지 업데이트 시 빌드](#automate-os-and-framework-patching): 이미지의 기본 이미지가 업데이트되었을 때 컨테이너 이미지 빌드를 트리거합니다.
* [다중 단계 작업](#multi-step-tasks): 이미지를 빌드하고, 컨테이너를 명령으로 실행하고, 이미지를 레지스트리로 푸시하는 다단계 작업을 정의합니다. ACR 작업은 요청 시 태스크 실행을 지원 하 고 병렬 이미지 빌드, 테스트 및 푸시 작업의이 기능입니다.

## <a name="quick-task"></a>빠른 작업

내부 루프 개발 주기는 원본 제어에 커밋하기 전에 애플리케이션 코드 작성, 빌드 및 테스트를 반복하는 프로세스로, 실제로 컨테이너 수명 주기 관리의 시작입니다.

첫 번째 코드 줄을 커밋하기 전에, ACR 작업의 빠른 [작업](container-registry-tutorial-quick-task.md) 기능은 컨테이너 이미지 빌드를 Azure에 오프로드하여 통합 개발 환경을 제공할 수 있습니다. 빠른 작업을 사용하면 코드를 커밋하기 전에 자동화된 빌드 정의를 확인하고 잠재적인 문제점을 발견할 수 있습니다.

친숙한 `docker build` 형식을 사용하여 Azure CLI의 [az acr build][az-acr-build] 명령에서 *컨텍스트*(빌드할 파일 집합)를 사용하고, 이를 ACR 작업으로 보내고, 기본적으로 완료되면 빌드된 이미지를 해당 레지스트리에 푸시합니다.

에 대 한 빠른 시작을 참조 하세요 [빌드하고 컨테이너 이미지 실행](container-registry-quickstart-task-cli.md) Azure Container Registry의 합니다.  

다음 표에서는 ACR 작업에 지원되는 컨텍스트 위치의 몇 가지 예를 보여 줍니다.

| 컨텍스트 위치 | 설명 | 예 |
| ---------------- | ----------- | ------- |
| 로컬 파일 시스템 | 로컬 파일 시스템의 디렉터리 내에 있는 파일. | `/home/user/projects/myapp` |
| GitHub 마스터 분기 | GitHub 리포지토리의 마스터(또는 다른 기본) 분기 내에 있는 파일.  | `https://github.com/gituser/myapp-repo.git` |
| GitHub 분기 | GitHub 리포지토리의 특정 분기.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| GitHub 하위 폴더 | GitHub 리포지토리의 하위 폴더 내에 있는 파일. 분기 및 하위 폴더를 지정 하는 조합을 보여 줍니다. | `https://github.com/gituser/myapp-repo.git#mybranch:myfolder` |
| 원격 Tarball | 원격 웹 서버의 압축된 아카이브에 있는 파일. | `http://remoteserver/myapp.tar.gz` |

ACR 작업은 기본 컨테이너 수명 주기로 설계되었습니다. 예를 들어, ACR 작업을 CI/CD 솔루션에 통합합니다. CI/CD 솔루션은 [서비스 주체][az-login-service-principal]로 [az login][az-login]을 실행한 다음, [az acr build][az-acr-build] 명령으로 이미지 빌드를 시작할 수 있습니다.

첫 번째 ACR 작업 자습서인 [Azure Container Registry 작업을 사용하여 클라우드에서 컨테이너 이미지 빌드](container-registry-tutorial-quick-task.md)에서 빠른 작업을 사용하는 방법을 알아보세요.

## <a name="automatic-build-on-source-code-commit"></a>소스 코드 커밋 시 자동 빌드

코드가 Git 리포지토리에 커밋될 때 컨테이너 이미지 빌드를 자동으로 트리거하려면 ACR 작업을 사용합니다. [az acr task][az-acr-task] Azure CLI 명령으로 구성할 수 있는 빌드 작업을 사용하면 Git 리포지토리를 지정하고 필요에 따라 분기와 Dockerfile을 지정할 수 있습니다. 팀에서 리포지토리에 코드를 커밋하면 ACR 작업에서 만든 웹후크에서 리포지토리에 정의된 컨테이너 이미지 빌드를 트리거합니다.

> [!IMPORTANT]
> 이전에 미리 보기 중에 `az acr build-task` 명령을 사용하여 작업을 만든 경우 [az acr task][az-acr-task] 명령을 사용하여 해당 작업을 다시 만들어야 합니다.

두 번째 ACR 작업 자습서인 [Azure Container Registry 작업을 사용하여 컨테이너 이미지 빌드 자동화](container-registry-tutorial-build-task.md)에서 소스 코드 커밋 시 빌드를 트리거하는 방법을 알아보세요.

## <a name="automate-os-and-framework-patching"></a>OS 및 프레임워크 패치 자동화

컨테이너 빌드 워크플로를 실제로 향상시키는 ACR 작업의 기능은 기본 이미지에 대한 업데이트를 검색하는 기능에서 제공됩니다. 업데이트 된 기본 이미지가 레지스트리에 푸시 됨 또는 Docker 허브에서와 같은 공용 리포지토리에 기본 이미지가 업데이트 될 때 ACR 작업에 따라 모든 응용 프로그램 이미지를 자동으로 빌드할 수 있습니다.

컨테이너 이미지는 *기본* 이미지와 *애플리케이션* 이미지로 크게 분류할 수 있습니다. 기본 이미지에는 일반적으로 애플리케이션이 빌드되는 운영 체제 및 애플리케이션 프레임워크가 다른 사용자 지정 항목과 함께 포함됩니다. 이러한 기본 이미지 자체는 일반적으로 [Alpine Linux][base-alpine], [Windows][base-windows], [.NET][base-dotnet], [Node.js][base-node] 등의 공용 업스트림 이미지를 기반으로 합니다. 일부 애플리케이션 이미지에서는 일반적인 기본 이미지를 공유할 수 있습니다.

중요한 OS 보안 패치와 같이 업스트림 유지 관리자가 OS 또는 응용 프로그램 프레임워크 이미지를 업데이트하는 경우, 중요한 수정 프로그램이 포함되도록 기본 이미지도 업데이트해야 합니다. 그러면 각 애플리케이션 이미지도 이제 기본 이미지에 포함된 이러한 업스트림 수정 프로그램을 포함하도록 다시 빌드해야 합니다.

ACR 작업은 컨테이너 이미지를 빌드할 때 기본 이미지 종속성을 동적으로 검색하므로 응용 프로그램 이미지의 기본 이미지가 업데이트되는 시기를 검색할 수 있습니다. 미리 구성된 하나의 [빌드 작업](container-registry-tutorial-base-image-update.md#create-a-task)을 사용하면 ACR 작업에서 **모든 애플리케이션 이미지를 자동으로 다시 빌드합니다**. 이 자동 검색 및 다시 빌드를 통해 ACR 작업에서 업데이트된 기본 이미지를 참조하는 각 애플리케이션 이미지를 빠짐없이 수동으로 추적하고 업데이트하는 데 필요한 시간과 노력을 절약할 수 있습니다.

세 번째 ACR 작업 자습서인 [Azure Container Registry 작업을 사용하여 기본 이미지 업데이트 시 이미지 빌드 자동화](container-registry-tutorial-base-image-update.md)에서 OS 및 프레임워크 패치에 대해 알아보세요.

> [!NOTE]
> 현재 기본 이미지는 모두 기본 및 응용 프로그램 이미지는 동일한 Azure container registry에 또는 기본 공용 Docker 허브 또는 Microsoft Container Registry 리포지토리에 상주 하는 경우에 트리거 빌드를 업데이트 합니다.

## <a name="multi-step-tasks"></a>다단계 작업

다중 단계 작업 단계 기반 작업 정의와 구축, 테스트 및 클라우드의 패치 컨테이너 이미지에 대 한 실행을 제공 합니다. 작업 단계는 개별 컨테이너 이미지 빌드 및 푸시 작업을 정의합니다. 해당 실행 환경으로 컨테이너를 사용하여 각 단계로 하나 이상의 컨테이너 실행을 정의할 수도 있습니다.

예를 들어, 다음을 자동화하는 다단계 작업을 만들 수 있습니다.

1. 웹 애플리케이션 이미지 빌드
1. 웹 애플리케이션 컨테이너 실행
1. 웹 애플리케이션 테스트 이미지 빌드
1. 실행 중인 애플리케이션 컨테이너에 대해 테스트를 수행하는 웹 애플리케이션 테스트 컨테이너 실행
1. 테스트에 통과하면 Helm 차트 보관 패키지 빌드
1. 새 Helm 차트 보관 패키지를 사용하여 `helm upgrade` 수행

다단계 작업을 사용하면 단계 간 종속성 지원을 통해 이미지 빌드, 실행 및 테스트를 보다 복잡한 단계로 분할할 수 있습니다. ACR 작업의 다단계 작업을 통해 이미지 빌드, 테스트, OS 및 프레임워크 패치 워크플로를 보다 세부적으로 제어할 수 있습니다.

[ACR 작업에서 다단계 빌드, 테스트 및 패치 작업 실행](container-registry-tasks-multi-step.md)에서 다단계 작업에 대해 알아보세요.

## <a name="view-task-logs"></a>작업 로그 보기

각 작업 실행 작업 단계를 성공적으로 실행 여부를 확인 하려면 검사할 수 있는 로그 출력을 생성 합니다. 사용 하는 경우는 [az acr 빌드](/cli/azure/acr#az-acr-build), [실행 하는 az acr](/cli/azure/acr#az-acr-run), 또는 [az acr 태스크](/cli/azure/acr/task#az-acr-task-run) 작업을 트리거하는 명령을, 작업 실행에 대 한 로그 출력은 콘솔로 스트리밍되 및 나중에 저장 검색 합니다. 작업에 대 한 로그를 Azure portal에서 실행 하거나 사용 하 여 보기를 [az acr 작업 로그](/cli/azure/acr/task#az-acr-task-logs) 명령입니다.

2019 년 7 월부터, 데이터 및 로그 레지스트리에 작업 실행에 대 한 기본적으로 30 일 동안 보존 되며 자동으로 제거 합니다. 실행 되는 작업에 대 한 데이터를 보관 하려는 경우 사용 하 여 보관 하도록 설정 합니다 [az acr 작업 업데이트 실행](/cli/azure/acr/task#az-acr-task-update-run) 명령입니다. 다음 예제에서는 태스크 실행에 대 한 보관 *cf11* 레지스트리에서 *myregistry*합니다.

```azurecli
az acr task update-run --registry myregistry --run-id cf11 --no-archive false
```

## <a name="next-steps"></a>다음 단계

OS 및 프레임 워크는 클라우드에서 컨테이너 이미지를 작성 하 여 패치를 자동화 하는 준비 된 경우 3 부작 확인해 [ACR 작업 자습서 시리즈](container-registry-tutorial-quick-task.md)합니다.

선택적으로 Azure 컨테이너 레지스트리와 작동할 [Visual Studio Code용 Docker 확장](https://code.visualstudio.com/docs/azure/docker)과 [Azure 계정](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) 확장을 설정합니다. Visual Studio Code 내에서 Azure 컨테이너 레지스트리에 이미지를 밀어넣고 끌어오거나, ACR Tasks를 실행합니다.

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-task]: /cli/azure/acr
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png

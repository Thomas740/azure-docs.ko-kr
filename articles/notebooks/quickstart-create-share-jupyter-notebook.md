---
title: Azure에서 Jupyter Notebook 만들기 및 공유
description: Azure Notebooks에서 Jupyter Notebook을 만들고 실행한 다음, 해당 Notebook을 다른 사용자와 공유합니다.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 5add60ad-0b4b-4fd5-adf5-eb50ce072d00
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 42f5d3abda715d83d58b0d341aed784de639e460
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66751790"
---
# <a name="quickstart-create-and-share-a-notebook"></a>빠른 시작: Notebook 만들기 및 공유

1. [Azure Notebooks](https://notebooks.azure.com)로 이동하여 로그인합니다. (자세한 내용은 [빠른 시작 - Azure Notebooks에 로그인](quickstart-sign-in-azure-notebooks.md)을 참조하세요.)

1. 공개 프로필 페이지 맨 위에 있는 **내 프로젝트**를 선택합니다.

    ![브라우저 창 맨 위에 있는 내 프로젝트 링크](media/quickstarts/my-projects-link.png)

1. **내 프로젝트** 페이지에서 **+ 새 프로젝트**(바로 가기 키: n)를 선택합니다. 브라우저 창이 좁은 경우 이 단추가 **+** 기호로만 표시될 수 있습니다.

    ![내 프로젝트 페이지의 새 프로젝트 명령](media/quickstarts/new-project-command.png)

1. **새 프로젝트 만들기** 팝업이 나타나면 다음 세부 정보를 입력하거나 설정한 다음, **만들기**를 선택합니다.

   - **프로젝트 이름**: Hello World in Python
   - **프로젝트 ID**: hello-world-python
   - **공용 프로젝트**: (지워짐)
   - **README.md 만들기**: (지워짐)

     ![세부 내용이 채워진 새 프로젝트 팝업](media/quickstarts/new-project-popup.png)

1. 잠시 후 Azure Notebooks는 새 프로젝트로 이동합니다. **+ 새로 만들기** 드롭다운 목록 ( **+** 만 나타날 수 있음)을 선택하여 Notebook에 이 프로젝트를 추가한 다음, **Notebook**을 선택합니다.

    [![](media/quickstarts/empty-project-new-notebook-button.png "빈 새 프로젝트 및 Notebook 명령 추가")](media/quickstarts/empty-project-new-notebook-button.png#lightbox)

1. **새 Notebook 만들기** 팝업이 나타나면 *HelloWorldInPython.ipynb* ( *.ipynb*은 IronPython (Jupyter) Notebook을 의미함) 같은 Notebook 파일 이름을 입력한 다음, **Python 3.6** 언어 (*커널*이라고도 함)를 선택합니다.

    ![새 Notebook 만들기 팝업](media/quickstarts/new-notebook-popup.png)

1. **새로 만들기**를 선택하여 Notebook 만들기를 마치면 프로젝트 파일 목록이 나타납니다.

    ![프로젝트의 파일 목록에 나타나는 새 Notebook](media/quickstarts/new-notebook-created.png)

## <a name="run-the-notebook"></a>노트북 실행

1. 새 Notebook을 선택하여 편집기에서 실행합니다. 선택한 커널(이 예의 경우 Python 3.6)은 이 Notebook에서 자동으로 활성화됩니다.

    ![Azure Notebooks에서 새 Notebook 보기](media/quickstarts/create-notebook-first-open.png)

1. 기본적으로 Notebook에는 하나의 빈 코드 셀이 있습니다. 셀 형식을 **Markdown**으로 바꾸려면 셀 형식 드롭다운을 사용하여 **Markdown**을 선택합니다.

    ![새 notebook에서 셀 형식 변경](media/quickstarts/create-notebook-cell-type.png)

1. 셀에 다음 제목 텍스트를 입력하거나 붙여넣습니다.

    ```markdown
    # Hello World in Python
    ```

1. Markdown을 편집하는 중이므로 텍스트는 "#" 헤더로 나타납니다. Markdown을 HTML로 렌더링하려면 **실행** 단추를 선택합니다. 나중에 Azure Notebooks는 자동으로 새 코드 셀을 만듭니다.

    ![셀 및 렌더링된 Markdown에 대한 실행 단추](media/quickstarts/run-cell-markdown-render.png)

1. 코드 셀에 다음 Python 코드를 입력합니다.

    ```python
    from datetime import datetime

    now = datetime.now()
    msg = "Hello, Azure Notebooks! Today is %s"  % now.strftime("%A, %d %B, %Y")
    print(msg)
    ```

1. **실행**(바로 가기 키: Shift+Enter)를 선택하여 이 코드를 실행합니다. 셀 아래에 다음 텍스트와 같은 출력이 표시됩니다.

    ```output
    Hello, Azure Notebooks! Today is Thursday, 15 November, 2018
    ```

1. 저장 아이콘을 선택하여 작업을 저장합니다.

    ![Jupyter Notebook 도구 모음의 저장 아이콘](media/quickstarts/hello-results-save-icon.png)

1. **파일** > **닫고 중지** 메뉴 명령을 선택하여 서버를 중지하고 브라우저 창을 닫습니다.

## <a name="share-the-notebook"></a>Notebook 공유

Notebook을 공유하려면 필요한 경우 프로젝트 페이지로 다시 전환하여 Notebook 파일을 마우스 오른쪽 단추로 클릭하고 **링크 복사**(바로 가기 키: y)를 선택합니다. 그다음 이 링크를 해당 메시지(이메일, IM 등)에 붙여넣습니다.

프로젝트 페이지에서 **공유** 메뉴를 사용하여 링크를 가져오거나 링크가 포함된 이메일 메시지를 만들고 HTML 및 Markdown embed 태그를 가져올 수 있습니다.

![프로젝트 공유 명령](media/quickstarts/share-project-command.png)

## <a name="next-steps"></a>다음 단계

> [!div class="nextstepaction"]
> [자습서: 선형 회귀를 위해 Jupyter 노트 실행 만들기](tutorial-create-run-jupyter-notebook.md)

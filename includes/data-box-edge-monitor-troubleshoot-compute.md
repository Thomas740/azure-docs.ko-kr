---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 7058d7f46373f8adaacbcbf90e5ea591a15f8f37
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67182661"
---
장치의 데이터 가장자리가 상자의 계산 역할이 구성 된 docker의 하위 집합 명령을 모니터링 하거나 모듈을 해결할 수 있습니다. 사용 가능한 명령 목록을 보려면 [PowerShell 인터페이스에 연결](#connect-to-the-powershell-interface) 사용 하는 `dkrdbe` 함수입니다.

```powershell
[10.100.10.10]: PS>dkrdbe -?
Usage: dkrdbe COMMAND

Commands:
   image [prune]
   images
   inspect
   login
   logout
   logs
   port
   ps
   pull
   start
   stats
   stop
   system [df]
   top

[10.100.10.10]: PS>
```
다음 표에서에 대 한 간략 한 설명에 대 한 사용 가능한 명령 `dkrdbe`:

|command  |설명 |
|---------|---------|
|`image`     | 이미지 관리       |
|`images`     | 이미지 나열         |
|`inspect`     | Docker 개체에 하위 수준 정보를 반환 합니다.         |
|`login`     | Docker 레지스트리에 로그인         |
|`logout`     | Docker 레지스트리에서 로그 아웃         |
|`logs`     | 컨테이너의 로그를 인출 합니다.        |
|`port`     | 포트 매핑을 나열 또는 컨테이너에 대 한 특정 매핑        |
|`ps`     | 컨테이너 나열        |
|`pull`     | 이미지 또는 레지스트리에서 리포지토리 끌어오기         |
|`start`     | 하나 이상의 중지 된 컨테이너를 시작 합니다.         |
|`stats`     | 라이브 스트림 컨테이너 리소스 사용 통계를 표시 합니다.         |
|`stop`     | 하나 이상의 실행 중인 컨테이너를 중지 합니다.        |
|`system`     | Docker 관리         |
|`top`     | 컨테이너의 실행 중인 프로세스를 표시 합니다.         |

사용 가능한 모든 명령에 대 한 도움말을 보려면 사용 하 여 `dkrdbe <command-name> --help`입니다.

예를 들어의 사용량을 파악할는 `port` 명령에 입력 합니다.

```powershell
[10.100.10.10]: P> dkrdbe port --help

Usage:  dkr port CONTAINER [PRIVATE_PORT[/PROTO]]

List port mappings or a specific mapping for the container
[10.100.10.10]: P> dkrdbe login --help

Usage:  docker login [OPTIONS] [SERVER]

Log in to a Docker registry.
If no server is specified, the default is defined by the daemon.

Options:
  -p, --password string   Password
      --password-stdin    Take the password from stdin
  -u, --username string   Username
[10.100.10.10]: PS>
```

사용 가능한 명령에 대 한는 `dkrdbe` 함수 일반적인 docker 명령에 사용 되는 것과 동일한 매개 변수를 사용 합니다. 옵션 및 docker 명령을 사용 하 여 사용 된 매개 변수를 [Docker 명령줄을 사용 하 여](https://docs.docker.com/engine/reference/commandline/docker/)입니다.

### <a name="to-check-if-the-module-deployed-successfully"></a>모듈이 성공적으로 배포 하는 경우를 확인 하려면

모듈은 비즈니스 논리를 구현 하는 컨테이너를 계산 합니다. 계산 모듈을 성공적으로 배포 하는 경우를 확인 하려면 실행을 `ps` 명령 및 컨테이너 (계산 모듈에 해당)가 실행 중인지 확인 합니다.

일시 중지 하는 등 모든 컨테이너의 목록을 가져오려면 실행 하는 `ps -a` 명령입니다.

```powershell
[10.100.10.10]: P> dkrdbe ps -a
CONTAINER ID        IMAGE                                                COMMAND                   CREATED             STATUS              PORTS                                                                  NAMES
d99e2f91d9a8        edgecompute.azurecr.io/filemovemodule2:0.0.1-amd64   "dotnet FileMoveModuâ€¦"    2 days ago          Up 2 days                                                                                  movefile
0a06f6d605e9        edgecompute.azurecr.io/filemovemodule2:0.0.1-amd64   "dotnet FileMoveModuâ€¦"    2 days ago          Up 2 days                                                                                  filemove
2f8a36e629db        mcr.microsoft.com/azureiotedge-hub:1.0               "/bin/sh -c 'echo \"$â€¦"   2 days ago          Up 2 days           0.0.0.0:443->443/tcp, 0.0.0.0:5671->5671/tcp, 0.0.0.0:8883->8883/tcp   edgeHub
acce59f70d60        mcr.microsoft.com/azureiotedge-agent:1.0             "/bin/sh -c 'echo \"$â€¦"   2 days ago          Up 2 days                                                                                  edgeAgent
[10.100.10.10]: PS>
```

만드는 컨테이너 이미지 또는 이미지를 풀링 하는 동안 오류가 발생 한 경우 실행 `logs edgeAgent`합니다.  `EdgeAgent` 다른 컨테이너를 프로 비전 하는 일을 담당 하는 IoT Edge 런타임 컨테이너입니다.

때문에 `logs edgeAgent` 옵션을 사용 하는 최근 오류를 참조 하는 좋은 방법은 모든 로그를 덤프 `--tail 20`합니다.


```powershell
[10.100.10.10]: PS>dkrdbe logs edgeAgent --tail 20
2019-02-28 23:38:23.464 +00:00 [DBG] [Microsoft.Azure.Devices.Edge.Util.Uds.HttpUdsMessageHandler] - Connected socket /var/run/iotedge/mgmt.sock
2019-02-28 23:38:23.464 +00:00 [DBG] [Microsoft.Azure.Devices.Edge.Util.Uds.HttpUdsMessageHandler] - Sending request http://mgmt.sock/modules?api-version=2018-06-28
2019-02-28 23:38:23.464 +00:00 [DBG] [Microsoft.Azure.Devices.Edge.Agent.Core.Agent] - Getting edge agent config...
2019-02-28 23:38:23.464 +00:00 [DBG] [Microsoft.Azure.Devices.Edge.Agent.Core.Agent] - Obtained edge agent config
2019-02-28 23:38:23.469 +00:00 [DBG] [Microsoft.Azure.Devices.Edge.Agent.Edgelet.ModuleManagementHttpClient] - Received a valid Http response from unix:///var/run/iotedge/mgmt.soc
k for List modules
--------------------CUT---------------------
--------------------CUT---------------------
08:28.1007774+00:00","restartCount":0,"lastRestartTimeUtc":"2019-02-26T20:08:28.1007774+00:00","runtimeStatus":"running","version":"1.0","status":"running","restartPolicy":"always
","type":"docker","settings":{"image":"edgecompute.azurecr.io/filemovemodule2:0.0.1-amd64","imageHash":"sha256:47778be0602fb077d7bc2aaae9b0760fbfc7c058bf4df192f207ad6cbb96f7cc","c
reateOptions":"{\"HostConfig\":{\"Binds\":[\"/home/hcsshares/share4-dl460:/home/input\",\"/home/hcsshares/share4-iot:/home/output\"]}}"},"env":{}}
2019-02-28 23:38:28.480 +00:00 [DBG] [Microsoft.Azure.Devices.Edge.Agent.Core.Planners.HealthRestartPlanner] - HealthRestartPlanner created Plan, with 0 command(s).
```

### <a name="to-get-container-logs"></a>컨테이너 로그를 가져오려면

특정 컨테이너에 대 한 로그를 가져오려면 먼저 컨테이너를 나열 하 고 관심 있는 컨테이너에 대 한 로그를 가져옵니다.

1. [PowerShell 인터페이스를 연결할](#connect-to-the-powershell-interface)합니다.
2. 실행 중인 컨테이너 목록을 가져오려면 실행 된 `ps` 명령입니다.

    ```powershell
    [10.100.10.10]: P> dkrdbe ps
    CONTAINER ID        IMAGE                                                COMMAND                   CREATED             STATUS              PORTS                                                                  NAMES
    d99e2f91d9a8        edgecompute.azurecr.io/filemovemodule2:0.0.1-amd64   "dotnet FileMoveModuâ€¦"    2 days ago          Up 2 days                                                                                  movefile
    0a06f6d605e9        edgecompute.azurecr.io/filemovemodule2:0.0.1-amd64   "dotnet FileMoveModuâ€¦"    2 days ago          Up 2 days                                                                                  filemove
    2f8a36e629db        mcr.microsoft.com/azureiotedge-hub:1.0               "/bin/sh -c 'echo \"$â€¦"   2 days ago          Up 2 days           0.0.0.0:443->443/tcp, 0.0.0.0:5671->5671/tcp, 0.0.0.0:8883->8883/tcp   edgeHub
    acce59f70d60        mcr.microsoft.com/azureiotedge-agent:1.0             "/bin/sh -c 'echo \"$â€¦"   2 days ago          Up 2 days                                                                                  edgeAgent
    ```

3. 에 대 한 로그를 해야 하는 컨테이너에 대 한 컨테이너 ID를 적어둡니다.

4. 특정 컨테이너에 대 한 로그를 가져오려면 실행 된 `logs` 컨테이너 ID를 제공 하는 명령

    ```powershell
    [10.100.10.10]: PS>dkrdbe logs d99e2f91d9a8
    02/26/2019 18:21:45: Info: Opening module client connection.
    02/26/2019 18:21:46: Info: Initializing with input: /home/input, output: /home/output.
    02/26/2019 18:21:46: Info: IoT Hub module client initialized.
    02/26/2019 18:22:24: Info: Received message: 1, SequenceNumber: 0 CorrelationId: , MessageId: 081886a07e694c4c8f245a80b96a252a Body: [{"ChangeType":"Created","ShareRelativeFilePath":"\\__Microsoft Data Box Edge__\\Upload\\Errors.xml","ShareName":"share4-dl460"}]
    02/26/2019 18:22:24: Info: Moving input file: /home/input/__Microsoft Data Box Edge__/Upload/Errors.xml to /home/output/__Microsoft Data Box Edge__/Upload/Errors.xml
    02/26/2019 18:22:24: Info: Processed event.
    02/26/2019 18:23:38: Info: Received message: 2, SequenceNumber: 0 CorrelationId: , MessageId: 30714d005eb048e7a4e7e3c22048cf20 Body: [{"ChangeType":"Created","ShareRelativeFilePath":"\\f [10]","ShareName":"share4-dl460"}]
    02/26/2019 18:23:38: Info: Moving input file: /home/input/f [10] to /home/output/f [10]
    02/26/2019 18:23:38: Info: Processed event.
    ```

### <a name="to-monitor-the-usage-statistics-of-the-device"></a>장치의 사용 통계를 모니터링 하려면

장치의 메모리, CPU 사용량 및 IO를 모니터링 하려면 사용 된 `stats` 명령입니다.

1. [PowerShell 인터페이스를 연결할](#connect-to-the-powershell-interface)합니다.
2. 실행 된 `stats` 라이브 스트림을 사용 하지 않도록 설정 하 고 첫 번째 결과 끌어오기 위해 명령입니다.

   ```powershell
   dkrdbe stats --no-stream
   ```

   다음 예제에서는이 cmdlet의 사용법을 보여 줍니다.

    ```
    [10.100.10.10]: P> dkrdbe stats --no-stream
    CONTAINER ID        NAME          CPU %         MEM USAGE / LIMIT     MEM %         NET I/O             BLOCK I/O           PIDS
    d99e2f91d9a8        movefile      0.0           24.4MiB / 62.89GiB    0.04%         751kB / 497kB       299kB / 0B          14
    0a06f6d605e9        filemove      0.00%         24.11MiB / 62.89GiB   0.04%         679kB / 481kB       49.5MB / 0B         14
    2f8a36e629db        edgeHub       0.18%         173.8MiB / 62.89GiB   0.27%         4.58MB / 5.49MB     25.7MB / 2.19MB     241
    acce59f70d60        edgeAgent     0.00%         35.55MiB / 62.89GiB   0.06%         2.23MB / 2.31MB     55.7MB / 332kB      14
    [10.100.10.10]: PS>
    ```


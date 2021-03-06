---
title: Azure Data Factory를 사용하여 HDFS에서 데이터 복사 | Microsoft Docs
description: Azure Data Factory 파이프라인의 복사 작업을 사용하여 클라우드 또는 온-프레미스 HDFS 원본에서 지원되는 싱크 데이터 저장소로 데이터를 복사하는 방법에 대해 알아봅니다.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/29/2019
ms.author: jingwang
ms.openlocfilehash: c528f37c8970380678a318ec2d63babd37f89501
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65228041"
---
# <a name="copy-data-from-hdfs-using-azure-data-factory"></a>Azure Data Factory를 사용하여 HDFS에서 데이터 복사
> [!div class="op_single_selector" title1="사용 하는 Data Factory 서비스 버전을 선택 합니다."]
> * [버전 1](v1/data-factory-hdfs-connector.md)
> * [현재 버전](connector-hdfs.md)

이 문서에는 HDFS 서버에서 데이터를 복사 하는 방법을 설명 합니다. Azure Data Factory에 대해 자세히 알아보려면 [소개 문서](introduction.md)를 참조하세요.

## <a name="supported-capabilities"></a>지원되는 기능

이 HDFS 커넥터는 다음 작업에 대 한 지원 됩니다.

- [복사 활동](copy-activity-overview.md) 사용 하 여 [원본/싱크 매트릭스를 지원 합니다.](copy-activity-overview.md)
- [조회 작업](control-flow-lookup-activity.md)

특히 이 HDFS 커넥터는 다음을 지원합니다.

- **Windows**(Kerberos) 또는 **Anonymous** 인증을 사용하여 파일을 복사합니다.
- **webhdfs** 프로토콜 또는 **기본 제공 DistCp** 지원을 사용하여 파일을 복사합니다.
- 파일을 있는 그대로 복사하거나 [지원되는 파일 형식 및 압축 코덱](supported-file-formats-and-compression-codecs.md)을 사용하여 파일을 붙여넣거나 생성합니다.

## <a name="prerequisites"></a>필수 조건

공개적으로 액세스할 수 없는 HDFS 간 데이터를 복사하려면 자체 호스팅 Integration Runtime을 설정해야 합니다. 자세한 내용은 [자체 호스팅 통합 런타임](concepts-integration-runtime.md) 문서를 참조하세요.

> [!NOTE]
> Integration Runtime에서 Hadoop 클러스터의 **모든** [이름 노드 서버]:[이름 노드 포트] 및 [데이터 노드 서버]:[데이터 노드 포트]에 액세스할 수 있는지 확인합니다. 기본 [이름 노드 포트]는 50070이며 기본 [데이터 노드 포트]는 50075입니다.

## <a name="getting-started"></a>시작

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

다음 섹션에서는 HDFS에 한정된 Data Factory 엔터티를 정의하는 데 사용되는 속성에 대해 자세히 설명합니다.

## <a name="linked-service-properties"></a>연결된 서비스 속성

HDFS 연결된 서비스에 다음 속성이 지원됩니다.

| 자산 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | type 속성을 다음으로 설정해야 합니다. **HDFS**. | 예. |
| url |HDFS에 대한 URL |예 |
| authenticationType | 허용되는 값은 다음과 같습니다. **익명**, 또는 **Windows**. <br><br> HDFS 커넥터에 **Kerberos 인증**을 사용하려면 [이 섹션](#use-kerberos-authentication-for-hdfs-connector)을 참조하여 온-프레미스 환경을 적절히 설정합니다. |예 |
| userName |Windows 인증에 대한 사용자 이름. Kerberos 인증의 경우 `<username>@<domain>.com`을 지정합니다. |예(Windows 인증에 대한) |
| password |Windows 인증에 대한 암호. 이 필드를 SecureString으로 표시하여 Data Factory에 안전하게 저장하거나 [Azure Key Vault에 저장되는 비밀을 참조](store-credentials-in-key-vault.md)합니다. |예(Windows 인증에 대한) |
| connectVia | 데이터 저장소에 연결하는 데 사용할 [Integration Runtime](concepts-integration-runtime.md)입니다. 자체 호스팅 Integration Runtime 또는 Azure Integration Runtime을 사용할 수 있습니다(데이터 저장소를 공개적으로 액세스할 수 있는 경우). 지정하지 않으면 기본 Azure Integration Runtime을 사용합니다. |아닙니다. |

**예제: 익명 인증 사용**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Anonymous",
            "userName": "hadoop"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**예제: Windows 인증 사용**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Windows",
            "userName": "<username>@<domain>.com (for Kerberos auth)",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>데이터 세트 속성

데이터 세트 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [데이터 세트](concepts-datasets-linked-services.md) 문서를 참조하세요. 

- 에 대 한 **Parquet 및 구분 기호로 분리 된 텍스트 형식**를 참조 하세요 [Parquet 및 구분 기호로 분리 된 텍스트 데이터 집합 형식](#parquet-and-delimited-text-format-dataset) 섹션입니다.
- 와 같은 다른 형식에 대 한 **ORC/Avro/JSON/이진 파일 형식**를 참조 [다른 형식으로 데이터 집합을](#other-format-dataset) 섹션입니다.

### <a name="parquet-and-delimited-text-format-dataset"></a>Parquet 및 구분 기호로 분리 된 텍스트 데이터 집합 형식

HDFS에서 데이터를 복사할 **Parquet 또는 구분 기호로 분리 된 텍스트 형식**를 참조 [Parquet 형식](format-parquet.md) 및 [구분 기호로 분리 된 텍스트 형식으로](format-delimited-text.md) 형식 기반의 데이터 집합에 대 한 문서 및 지원 설정. HDFS에서 다음 속성이 지원 됩니다 `location` 형식 기반의 데이터 집합의 설정:

| 자산   | 설명                                                  | 필수 |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | Type 속성은 아래 `location` 데이터 집합에서으로 설정 되어 있어야 **HdfsLocation**합니다. | 예.      |
| folderPath | 폴더 경로입니다. 와일드 카드 필터 폴더로 사용 하려는 경우이 설정은 건너뛰고 활동 원본 설정에서 지정 합니다. | 아닙니다.       |
| fileName   | 지정 된 folderPath에서 파일 이름입니다. 와일드 카드를 사용 하 여 파일을 필터링 하려는 경우이 설정은 건너뛰고 활동 원본 설정에서 지정 합니다. | 아닙니다.       |

> [!NOTE]
> **FileShare** 다음 섹션에 언급 된 Parquet/텍스트 형식 사용 하 여 데이터 집합 형식으로 계속 지원-이전 버전과 호환성에 대 한 복사/조회 작업입니다. 앞으로이 새 모델을 사용 하도록 제안 된 및 UI를 작성 하는 ADF 이러한 새 형식 생성로 전환 되었습니다.

**예제:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "HdfsLocation",
                "folderPath": "root/folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>다른 형식으로 데이터 집합

HDFS에서 데이터를 복사 **ORC/Avro/JSON/이진 형식**, 다음 속성이 지원 됩니다.

| 자산 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 데이터 세트의 type 속성을 다음으로 설정해야 합니다. **FileShare** |예 |
| folderPath | 파일의 경로입니다. 와일드카드 필터가 지원되며, 허용되는 와일드카드는 `*`(0개 이상의 문자 일치) 및 `?`(0-1개의 문자 일치)입니다. 실제 파일 이름에 와일드카드 또는 이 이스케이프 문자가 있는 경우 `^`을 사용하여 이스케이프합니다. <br/><br/>예: rootfolder/subfolder/(더 많은 예제는 [폴더 및 파일 필터 예제](#folder-and-file-filter-examples) 참조) |예 |
| fileName |  지정된 "folderPath" 아래의 파일에 대한 **이름 또는 와일드 카드 필터**입니다. 이 속성의 값을 지정하지 않으면 데이터 세트는 폴더에 있는 모든 파일을 가리킵니다. <br/><br/>필터에 허용되는 와일드카드는 `*`(문자 0자 이상 일치) 및 `?`(문자 0자 또는 1자 일치)입니다.<br/>- 예 1: `"fileName": "*.csv"`<br/>- 예 2: `"fileName": "???20180427.txt"`<br/>실제 폴더 이름에 와일드카드 또는 이 이스케이프 문자가 있는 경우 `^`을 사용하여 이스케이프합니다. |아닙니다. |
| modifiedDatetimeStart | 특성에 기반한 파일 필터링: 마지막으로 수정한 날짜 마지막 수정 시간이 `modifiedDatetimeStart`와 `modifiedDatetimeEnd` 사이의 시간 범위 내에 있으면 파일이 선택됩니다. 시간은 UTC 표준 시간대에 "2018-12-01T05:00:00Z" 형식으로 적용됩니다. <br/><br/> 대용량 파일에서에서 필터 파일 수행 하려는 경우이 설정을 사용 하 여 데이터 이동의 전반적인 성능을 영향을 알아야 합니다. <br/><br/> 속성에는 데이터 집합에 없는 파일 특성 필터를 적용할 것을 의미 하는 NULL 일 수 있습니다.  `modifiedDatetimeStart`에 datetime 값이 있지만 `modifiedDatetimeEnd`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 크거나 같은 파일이 선택됩니다.  `modifiedDatetimeEnd`에 datetime 값이 있지만 `modifiedDatetimeStart`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 작은 파일이 선택됩니다.| 아닙니다. |
| modifiedDatetimeEnd | 특성에 기반한 파일 필터링: 마지막으로 수정한 날짜 마지막 수정 시간이 `modifiedDatetimeStart`와 `modifiedDatetimeEnd` 사이의 시간 범위 내에 있으면 파일이 선택됩니다. 시간은 UTC 표준 시간대에 "2018-12-01T05:00:00Z" 형식으로 적용됩니다. <br/><br/> 대용량 파일에서에서 필터 파일 수행 하려는 경우이 설정을 사용 하 여 데이터 이동의 전반적인 성능을 영향을 알아야 합니다. <br/><br/> 속성에는 데이터 집합에 없는 파일 특성 필터를 적용할 것을 의미 하는 NULL 일 수 있습니다.  `modifiedDatetimeStart`에 datetime 값이 있지만 `modifiedDatetimeEnd`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 크거나 같은 파일이 선택됩니다.  `modifiedDatetimeEnd`에 datetime 값이 있지만 `modifiedDatetimeStart`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 작은 파일이 선택됩니다.| 아닙니다. |
| format | 파일 기반 저장소(이진 복사) 간에 **파일을 있는 그대로 복사**하려는 경우 입력 및 출력 데이터 세트 정의 둘 다에서 형식 섹션을 건너뜁니다.<br/><br/>특정 형식의 파일을 구문 분석하려는 경우, 지원되는 파일 형식 유형은 **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**입니다. 이 값 중 하나로 서식에서 **type** 속성을 설정합니다. 자세한 내용은 [텍스트 형식](supported-file-formats-and-compression-codecs.md#text-format), [Json 형식](supported-file-formats-and-compression-codecs.md#json-format), [Avro 형식](supported-file-formats-and-compression-codecs.md#avro-format), [Orc 형식](supported-file-formats-and-compression-codecs.md#orc-format) 및 [Parquet 형식](supported-file-formats-and-compression-codecs.md#parquet-format) 섹션을 참조하세요. |아니요(이진 복사 시나리오에만 해당) |
| compression | 데이터에 대한 압축 유형 및 수준을 지정합니다. 자세한 내용은 [지원되는 파일 형식 및 압축 코덱](supported-file-formats-and-compression-codecs.md#compression-support)을 참조하세요.<br/>지원되는 형식은 **GZip**, **Deflate**, **BZip2** 및 **ZipDeflate**입니다.<br/>지원되는 수준은 **최적** 및 **가장 빠름**입니다. |아닙니다. |

>[!TIP]
>폴더 아래에서 모든 파일을 복사하려면 **folderPath**만을 지정합니다.<br>지정된 이름의 단일 파일을 복사하려면 폴더 부분으로 **folderPath** 및 파일 이름으로 **fileName**을 지정합니다.<br>폴더 아래에서 파일의 하위 집합을 복사하려면 폴더 부분으로 **folderPath** 및 와일드카드 필터로 **fileName**을 지정합니다.

**예제:**

```json
{
    "name": "HDFSDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>복사 작업 속성

작업 정의에 사용할 수 있는 섹션 및 속성의 전체 목록은 [파이프라인](concepts-pipelines-activities.md) 문서를 참조하세요. 이 섹션에서는 HDFS 원본에서 지원하는 속성의 목록을 제공합니다.

### <a name="hdfs-as-source"></a>원본으로 HDFS

- 복사본에 대 한 **Parquet 및 구분 기호로 분리 된 텍스트 형식**를 참조 하세요 [Parquet 및 구분 기호로 분리 된 텍스트 형식으로 원본](#parquet-and-delimited-text-format-source) 섹션.
- 와 같은 다른 형식에서 복사본에 대 한 **ORC/Avro/JSON/이진 파일 형식**를 참조 [다른 형식으로 원본](#other-format-source) 섹션입니다.

#### <a name="parquet-and-delimited-text-format-source"></a>Parquet 및 구분 기호로 분리 된 텍스트 형식으로 원본

HDFS에서 데이터를 복사할 **Parquet 또는 구분 기호로 분리 된 텍스트 형식**를 참조 [Parquet 형식](format-parquet.md) 및 [구분 기호로 분리 된 텍스트 형식으로](format-delimited-text.md) 형식 기반의 복사 활동 원본에 대 한 문서 및 지원 되는 설정입니다. HDFS에서 다음 속성이 지원 됩니다 `storeSettings` 형식 기반의 복사 원본에 설정 합니다.

| 자산                 | 설명                                                  | 필수                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | 아래에 있는 type 속성은 `storeSettings` 으로 설정 되어 있어야 **HdfsReadSetting**합니다. | 예.                                           |
| recursive                | 하위 폴더 또는 지정된 폴더에서만 데이터를 재귀적으로 읽을지 여부를 나타냅니다. recursive를 true로 설정하고 싱크가 파일 기반 저장소인 경우 빈 폴더 또는 하위 폴더가 싱크에 복사되거나 만들어지지 않습니다. 허용되는 값은 **true**(기본값) 및 **false**입니다. | 아닙니다.                                            |
| wildcardFolderPath       | 소스 폴더를 필터링 하려면 와일드 카드 문자를 사용 하 여 폴더 경로입니다. <br>허용되는 와일드카드는 `*`(0개 이상의 문자 일치) 및 `?`(0-1개의 문자 일치)입니다. 실제 폴더 이름에 와일드카드 또는 이 이스케이프 문자가 있는 경우 `^`을 사용하여 이스케이프합니다. <br>더 많은 예는 [폴더 및 파일 필터 예제](#folder-and-file-filter-examples)를 참조하세요. | 아닙니다.                                            |
| wildcardFileName         | 필터 소스 파일에 지정 된 folderPath/wildcardFolderPath에서 와일드 카드 문자를 사용 하 여 파일 이름입니다. <br>허용되는 와일드카드는 `*`(0개 이상의 문자 일치) 및 `?`(0-1개의 문자 일치)입니다. 실제 폴더 이름에 와일드카드 또는 이 이스케이프 문자가 있는 경우 `^`을 사용하여 이스케이프합니다.  더 많은 예는 [폴더 및 파일 필터 예제](#folder-and-file-filter-examples)를 참조하세요. | 경우에 예 `fileName` 데이터 집합에 지정 되지 않은 |
| modifiedDatetimeStart    | 특성에 기반한 파일 필터링: 마지막으로 수정한 날짜 마지막 수정 시간이 `modifiedDatetimeStart`와 `modifiedDatetimeEnd` 사이의 시간 범위 내에 있으면 파일이 선택됩니다. 시간은 UTC 표준 시간대에 "2018-12-01T05:00:00Z" 형식으로 적용됩니다. <br> 속성은 NULL일 수 있습니다. 이 경우 파일 특성 필터가 데이터 세트에 적용되지 않습니다.  `modifiedDatetimeStart`에 datetime 값이 있지만 `modifiedDatetimeEnd`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 크거나 같은 파일이 선택됩니다.  `modifiedDatetimeEnd`에 datetime 값이 있지만 `modifiedDatetimeStart`가 NULL이면, 마지막으로 수정된 특성이 datetime 값보다 작은 파일이 선택됩니다. | 아닙니다.                                            |
| modifiedDatetimeEnd      | 위와 동일합니다.                                               | 아닙니다.                                            |
| maxConcurrentConnections | Storage 저장소에 동시에 연결할 연결 횟수입니다. 데이터 저장소에 대 한 동시 연결을 제한 하려는 경우에 지정 합니다. | 아닙니다.                                            |

> [!NOTE]
> Parquet/구분 기호로 분리 된 텍스트 형식에 대 한 **FileSystemSource** 다음 섹션에 언급 된 형식 복사 활동 원본으로 계속 지원 됩니다-이전 버전과 호환성입니다. 앞으로이 새 모델을 사용 하도록 제안 된 및 UI를 작성 하는 ADF 이러한 새 형식 생성로 전환 되었습니다.

**예제:**

```json
"activities":[
    {
        "name": "CopyFromHDFS",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "HdfsReadSetting",
                    "recursive": true
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>다른 형식으로 원본

HDFS에서 데이터를 복사할 **ORC/Avro/JSON/이진 파일 형식**를 복사 작업에서 다음 속성이 지원 됩니다 **원본** 섹션:

| 자산 | 설명 | 필수 |
|:--- |:--- |:--- |
| type | 복사 작업 원본의 type 속성을 다음으로 설정해야 합니다. **HdfsSource** |예. |
| recursive | 하위 폴더에서 또는 지정된 폴더에서만 데이터를 재귀적으로 읽을지 여부를 나타냅니다. recursive가 true로 설정되고 싱크가 파일 기반 저장소인 경우 싱크에서 빈 폴더/하위 폴더가 복사/생성되지 않습니다.<br/>허용되는 값은 **true**(기본값), **false**입니다. | 아닙니다. |
| distcpSettings | HDFS DistCp를 사용하는 경우 속성 그룹입니다. | 아닙니다. |
| resourceManagerEndpoint | Yarn 리소스 관리자 끝점 | 예(DistCp를 사용하는 경우) |
| tempScriptPath | 임시 DistCp 명령 스크립트를 저장하는 데 사용되는 폴더 경로입니다. 스크립트 파일이 Data Factory에 의해 생성되고 복사 작업을 완료한 후에 제거됩니다. | 예(DistCp를 사용하는 경우) |
| distcpOptions | DistCp 명령에 제공된 추가 옵션입니다. | 아닙니다. |
| maxConcurrentConnections | Storage 저장소에 동시에 연결할 연결 횟수입니다. 데이터 저장소에 대 한 동시 연결을 제한 하려는 경우에 지정 합니다. | 아닙니다. |

**예제: DistCp를 사용 하 여 복사 작업에서 HDFS 원본**

```json
"source": {
    "type": "HdfsSource",
    "distcpSettings": {
        "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
        "tempScriptPath": "/usr/hadoop/tempscript",
        "distcpOptions": "-m 100"
    }
}
```

다음 섹션에서 DistCp를 사용하여 HDFS에서 효율적으로 데이터를 복사하는 방법에 대해 알아봅니다.

### <a name="folder-and-file-filter-examples"></a>폴더 및 파일 필터 예제

이 섹션에서는 와일드카드 필터가 있는 폴더 경로 및 파일 이름의 결과 동작에 대해 설명합니다.

| folderPath | fileName             | recursive | 원본 폴더 구조 및 필터 결과(**굵게** 표시된 파일이 검색됨) |
| :--------- | :------------------- | :-------- | :----------------------------------------------------------- |
| `Folder*`  | (비어 있음, 기본값 사용) | false     | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*`  | (비어 있음, 기본값 사용) | true      | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*`  | `*.csv`              | false     | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.csv<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |
| `Folder*`  | `*.csv`              | true      | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**File1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**File5.csv**<br/>AnotherFolderB<br/>&nbsp;&nbsp;&nbsp;&nbsp;File6.csv |

## <a name="use-distcp-to-copy-data-from-hdfs"></a>DistCp를 사용하여 HDFS에서 데이터 복사

[DistCp](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html)는 Hadoop 클러스터에 분산된 복사를 수행하는 Hadoop 네이티브 명령줄 도구입니다. Distcp 명령을 실행하는 경우 먼저 복사될 모든 파일을 나열하고, Hadoop 클러스터에 여러 맵 작업을 만들며 각 맵 작업은 원본에서 싱크로 이진 복사를 수행합니다.

복사 작업은 DistCp를 사용하여 파일을 있는 그대로 Azure Blob([준비된 복사](copy-activity-performance.md) 또는 Azure Data Lake Store 포함)으로 복사를 지원하며 이 경우 자체 호스팅 Integration Runtime에서 실행하는 대신 클러스터의 기능을 완벽하게 활용할 수 있습니다. 클러스터가 매우 강력한 경우에 특히 더 나은 복사 처리량을 제공합니다. Azure Data Factory의 구성에 따라 복사 작업은 자동으로 distcp 명령을 생성하고, Hadoop 클러스터에 제출하고, 복사 상태를 모니터링합니다.

### <a name="prerequisites"></a>필수 조건

DistCp를 사용하여 HDFS에서 Azure Blob(준비된 복사 포함) 또는 Azure Data Lake Store로 파일을 있는 그대로 복사하려면 Hadoop 클러스터가 아래 요구 사항을 충족하는지 확인합니다.

1. MapReduce 및 Yarn 서비스가 활성화됩니다.
2. Yarn 버전은 2.5 이상입니다.
3. HDFS 서버는 대상 데이터 저장소 - Azure Blob 또는 Azure Data Lake Store와 통합됩니다.

    - Azure Blob 파일 시스템은 Hadoop 2.7부터 기본적으로 지원됩니다. Hadoop env config에서 jar 경로를 지정하기만 하면 됩니다.
    - Azure Data Lake Store 파일 시스템은 Hadoop 3.0.0-alpha1부터 패키지됩니다. Hadoop 클러스터가 해당 버전보다 낮은 경우 [여기](https://hadoop.apache.org/releases.html)에서 ADLS 관련 jar 패키지(azure-datalake-store.jar)를 클러스터로 수동으로 가져오고, Hadoop env config에서 jar 경로를 지정해야 합니다.

4. HDFS에서 임시 폴더를 준비합니다. 이 임시 폴더는 KB 수준 공간을 차지할 수 있도록 DistCp 셸 스크립트를 저장하는 데 사용됩니다.
5. HDFS 연결된 서비스에 제공된 사용자 계정이 a) Yarn에서 애플리케이션을 제출하고, b) 위의 임시 폴더 아래에 하위 폴더, 읽기/쓰기 파일을 만드는 권한을 갖도록 합니다.

### <a name="configurations"></a>구성

DistCp를 참조 하세요. 관련 구성 및 예제의 [원본으로 HDFS](#hdfs-as-source) 섹션입니다.

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>HDFS 커넥터에 Kerberos 인증 사용

HDFS 커넥터에 Kerberos 인증을 사용하도록 온-프레미스 환경을 설정하는 옵션은 두 가지가 있습니다. 이 중 더 잘 맞는 옵션을 선택할 수 있습니다.
* 옵션 1: [Kerberos 영역에 자체 호스팅 통합 런타임 머신 가입](#kerberos-join-realm)
* 옵션 2: [Windows 도메인과 Kerberos 영역 사이에 상호 트러스트를 사용하도록 설정](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>옵션 1: Kerberos 영역에 자체 호스팅 통합 런타임 머신 가입

#### <a name="requirements"></a>요구 사항

* 자체 호스팅 Integration Runtime 컴퓨터가 Kerberos 영역에 가입해야 하고 Windows 도메인에는 가입할 수 없습니다.

#### <a name="how-to-configure"></a>구성 방법

**자체 호스팅 Integration Runtime 컴퓨터에서:**

1.  **Ksetup** 유틸리티를 실행하여 Kerberos KDC 서버 및 영역을 구성합니다.

    Kerberos 영역은 Windows 도메인과 다르기 때문에 컴퓨터를 작업 그룹의 구성원으로 구성해야 합니다. 그러려면 다음과 같이 Kerberos 영역을 설정하고 KDC 서버를 추가합니다. *REALM.COM*은 고유한 각 영역으로 필요에 맞게 바꿉니다.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    이러한 2개 명령을 실행한 후 컴퓨터를 **다시 시작**합니다.

2.  **Ksetup** 명령을 사용하여 구성을 확인합니다. 출력은 다음과 같아야 합니다.

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**Azure Data Factory에서:**

* HDFS 데이터 원본에 연결하는 Kerberos 주체 이름 및 암호와 **Windows 인증**을 함께 사용하여 HDFS 커넥터를 구성합니다. 구성 세부 정보에서 [HDFS 연결된 서비스 속성](#linked-service-properties)을 확인합니다.

### <a name="kerberos-mutual-trust"></a>옵션 2: Windows 도메인과 Kerberos 영역 사이에 상호 트러스트를 사용하도록 설정

#### <a name="requirements"></a>요구 사항

*   자체 호스팅 Integration Runtime 컴퓨터는 Windows 도메인에 가입해야 합니다.
*   도메인 컨트롤러의 설정을 업데이트하는 권한이 있어야 합니다.

#### <a name="how-to-configure"></a>구성 방법

> [!NOTE]
> 다음 자습서에서 REALM.COM 및 AD.COM을 고유한 각 영역과 도메인 컨트롤러로 필요에 맞게 바꿉니다.

**KDC 서버에서:**

1. 다음 구성 템플릿을 참조하여 **krb5.conf** 파일에서 KDC가 Windows 도메인을 신뢰하도록 KDC 구성을 편집합니다. 기본적으로 이 구성은 **/etc/krb5.conf**에 있습니다.

           [logging]
            default = FILE:/var/log/krb5libs.log
            kdc = FILE:/var/log/krb5kdc.log
            admin_server = FILE:/var/log/kadmind.log
            
           [libdefaults]
            default_realm = REALM.COM
            dns_lookup_realm = false
            dns_lookup_kdc = false
            ticket_lifetime = 24h
            renew_lifetime = 7d
            forwardable = true
            
           [realms]
            REALM.COM = {
             kdc = node.REALM.COM
             admin_server = node.REALM.COM
            }
           AD.COM = {
            kdc = windc.ad.com
            admin_server = windc.ad.com
           }
            
           [domain_realm]
            .REALM.COM = REALM.COM
            REALM.COM = REALM.COM
            .ad.com = AD.COM
            ad.com = AD.COM
            
           [capaths]
            AD.COM = {
             REALM.COM = .
            }

   **다시 시작** 구성 후에 KDC 서비스입니다.

2. 명명 된 보안 주체를 준비 **krbtgt/REALM.COM\@AD.COM** 다음 명령 사용 하 여 KDC 서버에서:

           Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3. **hadoop.security.auth_to_local** HDFS 서비스 구성 파일에서 `RULE:[1:$1@$0](.*\@AD.COM)s/\@.*//`를 추가합니다.

**도메인 컨트롤러에서:**

1.  다음 **Ksetup** 명령을 실행하여 영역 항목을 추가합니다.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Windows 도메인에서 Kerberos 영역으로의 트러스트를 설정합니다. [password]는 보안 주체에 대 한 암호 **krbtgt/REALM.COM\@AD.COM**합니다.

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  Kerberos에서 사용되는 암호화 알고리즘을 선택합니다.

    1. 서버 관리자 > 그룹 정책 관리 > 도메인 > 그룹 정책 개체 > 기본 또는 활성 도메인 정책으로 이동하고 편집을 클릭합니다.

    2. **그룹 정책 관리 편집기** 팝업 창에서 컴퓨터 구성 > 정책 > Windows 설정 > 보안 설정 > 로컬 정책 > 보안 옵션으로 이동하고 **네트워크 보안: Kerberos에 허용된 암호화 유형 구성**을 구성합니다.

    3. KDC에 연결할 때 사용할 암호화 알고리즘을 선택합니다. 일반적으로 간단히 모든 옵션을 선택할 수 있습니다.

        ![Kerberos의 암호화 유형 구성](media/connector-hdfs/config-encryption-types-for-kerberos.png)

    4. **Ksetup** 명령을 사용하여 특정 영역에서 사용할 암호화 알고리즘을 지정합니다.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Windows 도메인에서 Kerberos 주체를 사용하려면 도메인 계정 및 Kerberos 주체 간의 매핑을 만듭니다.

    1. 관리 도구 > **Active Directory 사용자 및 컴퓨터**를 시작합니다.

    2. **보기** > **고급 기능**을 클릭하여 고급 기능을 구성합니다.

    3. 매핑을 만들려는 계정을 찾고 마우스 오른쪽 단추를 클릭하여 **이름 매핑**을 본 후 **Kerberos 이름** 탭을 클릭합니다.

    4. 영역의 주체를 추가합니다.

        ![보안 ID 매핑](media/connector-hdfs/map-security-identity.png)

**자체 호스팅 Integration Runtime 컴퓨터에서:**

* 다음 **Ksetup** 명령을 실행하여 영역 항목을 추가합니다.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**Azure Data Factory에서:**

* HDFS 데이터 원본에 연결하는 도메인 계정 또는 Kerberos 주체와 **Windows 인증**을 함께 사용하여 HDFS 커넥터를 구성합니다. 구성 세부 정보에서 [HDFS 연결된 서비스 속성](#linked-service-properties)을 확인합니다.


## <a name="next-steps"></a>다음 단계
Azure Data Factory에서 복사 작업의 원본 및 싱크로 지원되는 데이터 저장소 목록은 [지원되는 데이터 저장소](copy-activity-overview.md#supported-data-stores-and-formats)를 참조하세요.

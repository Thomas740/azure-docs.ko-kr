---
title: Azure CLI를 사용 하 여 Azure HDInsight 클러스터 관리
description: Azure CLI를 사용 하 여 Azure HDInsight 클러스터를 관리 하는 방법을 알아봅니다. 클러스터 형식에는 Apache Hadoop, Spark, HBase, Storm, Kafka, 대화형 쿼리, ML 서비스 포함 됩니다.
ms.reviewer: jasonh
author: tylerfox
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: tyfox
ms.openlocfilehash: 5ae97b17d06fa0a9934a58ac662ef12116cce4f6
ms.sourcegitcommit: e5dcf12763af358f24e73b9f89ff4088ac63c6cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67137396"
---
# <a name="manage-azure-hdinsight-clusters-using-azure-cli"></a>Azure CLI를 사용 하 여 Azure HDInsight 클러스터 관리

[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

사용 방법 알아보기 [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) Azure HDInsight 클러스터를 관리 합니다. Azure CLI(명령줄 인터페이스)는 Azure 리소스를 관리하기 위한 Microsoft의 플랫폼 간 명령줄 환경입니다.

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

## <a name="prerequisites"></a>필수 조건

* Azure CLI. Azure CLI를 설치하지 않은 경우 [Azure CLI 설치](https://docs.microsoft.com/cli/azure/install-azure-cli)에 나온 단계를 참조하세요.

* HDInsight의 Apache Hadoop 클러스터. [Linux에서 HDInsight 시작](hadoop/apache-hadoop-linux-tutorial-get-started.md)을 참조하세요.

## <a name="connect-to-azure"></a>Azure에 연결

Azure 구독에 로그인합니다. Azure Cloud Shell을 사용하려는 경우 코드 블록의 오른쪽 위 모서리에 있는 **사용해보기**를 선택하기만 하면 됩니다. 그렇지 않은 경우 아래 명령을 입력합니다.

```azurecli-interactive
az login

# If you have multiple subscriptions, set the one to use
# az account set --subscription "SUBSCRIPTIONID"
```

## <a name="list-clusters"></a>클러스터 나열

사용 하 여 [az hdinsight 목록](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-list) 목록 클러스터에 있습니다. 대체 하 여 아래 명령을 편집 `RESOURCE_GROUP_NAME` 리소스 그룹의 이름을 사용 하 여 다음 명령을 입력 합니다.

```azurecli-interactive
# List all clusters in the current subscription
az hdinsight list

# List only cluster name and its resource group
az hdinsight list --query "[].{Cluster:name, ResourceGroup:resourceGroup}" --output table

# List all cluster for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME

# List all cluster names for your resource group
az hdinsight list --resource-group RESOURCE_GROUP_NAME --query "[].{clusterName:name}" --output table
```

## <a name="show-cluster"></a>클러스터 표시

사용 하 여 [az hdinsight 표시](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-show) 지정된 된 클러스터에 대 한 정보를 표시 합니다. 대체 하 여 아래 명령을 편집할 `RESOURCE_GROUP_NAME`, 및 `CLUSTER_NAME` 관련 정보를 사용 하 여 다음 명령을 입력 합니다.

```azurecli-interactive
az hdinsight show --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

## <a name="delete-clusters"></a>클러스터 삭제

사용 하 여 [az hdinsight 삭제](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-delete) 지정된 된 클러스터를 삭제 합니다. 대체 하 여 아래 명령을 편집할 `RESOURCE_GROUP_NAME`, 및 `CLUSTER_NAME` 관련 정보를 사용 하 여 다음 명령을 입력 합니다.

```azurecli-interactive
az hdinsight delete --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME
```

또한 클러스터를 포함하는 리소스 그룹을 삭제하여 클러스터를 삭제할 수도 있습니다. Note: 기본 저장소 계정을 비롯 하 여 그룹의 모든 리소스가 삭제 됩니다.

```azurecli-interactive
az group delete --name RESOURCE_GROUP_NAME
```

## <a name="scale-clusters"></a>클러스터 크기 조정

사용 하 여 [az hdinsight의 크기를 조정](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) 를 지정한 크기에 지정된 된 HDInsight 클러스터를 조정 합니다. 명령을 편집 하 여 아래 바꿔 `RESOURCE_GROUP_NAME`, 및 `CLUSTER_NAME` 관련 정보를 사용 하 여 합니다. 대체 `TARGET_INSTANCE_COUNT` 원하는 클러스터에 대 한 작업자 노드의 수입니다. 클러스터 크기 조정 하는 방법에 대 한 자세한 내용은 참조 하세요. [확장 HDInsight 클러스터](./hdinsight-scaling-best-practices.md)합니다. 다음 명령을 입력합니다.

```azurecli-interactive
az hdinsight resize --resource-group RESOURCE_GROUP_NAME --name CLUSTER_NAME --target-instance-count TARGET_INSTANCE_COUNT
```

## <a name="next-steps"></a>다음 단계

이 문서에서는 HDInsight 클러스터 관리 작업을 수행하는 여러 방법에 대해 알아보았습니다. 자세한 내용은 다음 문서를 참조하세요.

* [Azure Portal을 사용하여 HDInsight의 Apache Hadoop 클러스터 관리](hdinsight-administer-use-portal-linux.md)
* [Azure PowerShell을 사용 하 여 HDInsight 관리](hdinsight-administer-use-powershell.md)
* [Azure HDInsight 시작](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure CLI 시작](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)

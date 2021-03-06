---
title: Azure PowerShell을 사용 하 여 웹 응용 프로그램 방화벽 v2 사용자 지정 규칙 구성
description: Azure PowerShell을 사용 하 여 WAF v2 사용자 지정 규칙을 구성 하는 방법에 알아봅니다.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/18/2019
ms.author: victorh
ms.openlocfilehash: f4d2fd7342e0efe95a1bc69e0dba77692053cf14
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164745"
---
# <a name="configure-web-application-firewall-v2--with-a-custom-rule-using-azure-powershell"></a>Azure PowerShell을 사용 하 여 사용자 지정 규칙을 사용 하 여 웹 응용 프로그램 방화벽 v2 구성

<!--- If you make any changes to the PowerShell in this article, also make the change in the corresponding Sample file: azure-docs-powershell-samples/application-gateway/waf-rules/waf-custom-rules.ps1 --->

사용자 지정 규칙을 사용 하면 웹 응용 프로그램 방화벽 (WAF) v2를 통과 하는 각 요청에 대 한 평가 사용자 고유의 규칙을 만들 수 있습니다. 이러한 규칙 관리 되는 규칙 집합에서 규칙의 나머지 부분 보다 더 높은 우선 순위를 보유합니다. 사용자 지정 규칙 (허용 또는 차단) 하는 작업, 일치 조건 및 전체를 사용자 지정할 수 있도록 운영자 경우

이 문서에서는 사용자 지정 규칙을 사용 하는 Application Gateway WAF v2를 만듭니다. 요청 헤더에 사용자 에이전트 *evilbot*이 포함되어 있으면 사용자 지정 규칙이 트래픽을 차단합니다.

더 많은 사용자 지정 규칙 예제를 보려면 [사용자 지정 웹 응용 프로그램 방화벽 규칙 만들기 및 사용](create-custom-waf-rules.md)

원할 경우 실행이 문서는 Azure PowerShell에서 복사, 붙여넣기 및 실행 하는 하나의 연속 스크립트 참조 [Azure 응용 프로그램 게이트웨이 PowerShell 샘플](powershell-samples.md)합니다.

## <a name="prerequisites"></a>필수 조건

### <a name="azure-powershell-module"></a>Azure PowerShell 모듈

Azure PowerShell을 로컬로 설치하고 사용하도록 선택하는 경우 이 스크립트를 실행하려면 Azure PowerShell 모듈 버전 2.1.0 이상이 필요합니다.

1. 버전을 확인하려면 `Get-Module -ListAvailable Az`을 실행합니다. 업그레이드해야 하는 경우 [Azure PowerShell 모듈 설치](/powershell/azure/install-az-ps)를 참조하세요.
2. `Connect-AzAccount`를 실행하여 Azure와 연결합니다.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="example-script"></a>예제 스크립트

### <a name="set-up-variables"></a>변수 설정

```azurepowershell
$rgname = "CustomRulesTest"

$location = "East US"

$appgwName = "WAFCustomRules"
```

### <a name="create-a-resource-group"></a>리소스 그룹 만들기

```azurepowershell
$resourceGroup = New-AzResourceGroup -Name $rgname -Location $location
```

### <a name="create-a-vnet"></a>VNet 만들기

```azurepowershell
$sub1 = New-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -AddressPrefix "10.0.0.0/24"

$sub2 = New-AzVirtualNetworkSubnetConfig -Name "backendSubnet" -AddressPrefix "10.0.1.0/24"

$vnet = New-AzvirtualNetwork -Name "Vnet1" -ResourceGroupName $rgname -Location $location `
  -AddressPrefix "10.0.0.0/16" -Subnet @($sub1, $sub2)
```

### <a name="create-a-static-public-vip"></a>정적 공용 VIP 만들기

```azurepowershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $rgname -name "AppGwIP" `
  -location $location -AllocationMethod Static -Sku Standard
```

### <a name="create-pool-and-frontend-port"></a>풀 및 프런트 엔드 포트 만들기

```azurepowershell
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name "appgwSubnet" -VirtualNetwork $vnet

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "AppGwIpConfig" -Subnet $gwSubnet

$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "fipconfig" -PublicIPAddress $publicip

$pool = New-AzApplicationGatewayBackendAddressPool -Name "pool1" `
  -BackendIPAddresses testbackend1.westus.cloudapp.azure.com, testbackend2.westus.cloudapp.azure.com

$fp01 = New-AzApplicationGatewayFrontendPort -Name "port1" -Port 80
```

### <a name="create-a-listener-http-setting-rule-and-autoscale"></a>수신기, http 설정, 규칙 및 자동 크기 조정 만들기

```azurepowershell
$listener01 = New-AzApplicationGatewayHttpListener -Name "listener1" -Protocol Http `
  -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01

$poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name "setting1" -Port 80 `
  -Protocol Http -CookieBasedAffinity Disabled

$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType basic `
  -BackendHttpSettings $poolSetting01 -HttpListener $listener01 -BackendAddressPool $pool

$autoscaleConfig = New-AzApplicationGatewayAutoscaleConfiguration -MinCapacity 3

$sku = New-AzApplicationGatewaySku -Name WAF_v2 -Tier WAF_v2
```

### <a name="create-the-custom-rule-and-apply-it-to-waf-policy"></a>사용자 지정 규칙을 만들고 WAF 정책 적용

```azurepowershell
$variable = New-AzApplicationGatewayFirewallMatchVariable -VariableName RequestHeaders -Selector User-Agent

$condition = New-AzApplicationGatewayFirewallCondition -MatchVariable $variable -Operator Contains -MatchValue "evilbot" -Transform Lowercase -NegationCondition $False  

$rule = New-AzApplicationGatewayFirewallCustomRule -Name blockEvilBot -Priority 2 -RuleType MatchRule -MatchCondition $condition -Action Block

$wafPolicy = New-AzApplicationGatewayFirewallPolicy -Name wafPolicy -ResourceGroup $rgname -Location $location -CustomRule $rule

$wafConfig = New-AzApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

### <a name="create-the-application-gateway"></a>Application Gateway 만들기

```azurepowershell
$appgw = New-AzApplicationGateway -Name $appgwName -ResourceGroupName $rgname `
   -Location $location -BackendAddressPools $pool `
   -BackendHttpSettingsCollection  $poolSetting01 `
   -GatewayIpConfigurations $gipconfig -FrontendIpConfigurations $fipconfig01 `
   -FrontendPorts $fp01 -HttpListeners $listener01 `
   -RequestRoutingRules $rule01 -Sku $sku -AutoscaleConfiguration $autoscaleConfig `
   -WebApplicationFirewallConfig $wafConfig `
   -FirewallPolicy $wafPolicy
```

## <a name="next-steps"></a>다음 단계

[웹 응용 프로그램 방화벽에 자세히 알아보기](waf-overview.md)
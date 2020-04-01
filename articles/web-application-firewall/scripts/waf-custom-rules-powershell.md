---
title: Azure PowerShell Script Örneği - WAF özel kuralları oluşturun
description: Azure PowerShell Script Örneği - Uygulama Ağ Geçidi özel kurallarında Web Uygulaması Güvenlik Duvarı Oluşturma
author: vhorne
ms.service: web-application-firewall
services: web-application-firewall
ms.topic: sample
ms.date: 09/30/2019
ms.author: victorh
ms.openlocfilehash: 950f71c284268a9aa2773eb57213e266622d85bd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73501578"
---
# <a name="create-waf-custom-rules-with-azure-powershell"></a>Azure PowerShell ile WAF özel kuralları oluşturun

Bu komut dosyası, özel kurallar kullanan bir Uygulama Ağ Geçidi Web Uygulaması Güvenlik Duvarı oluşturur. İstek üstbilgisi Kullanıcı Temsilcisi *evilbot*içeriyorsa özel kural trafiği engeller.

## <a name="prerequisites"></a>Ön koşullar

### <a name="azure-powershell-module"></a>Azure PowerShell modülü

Azure PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu komut dosyası azure PowerShell modülü sürümü 2.1.0 veya daha sonra gerektirir.

1. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps).
2. Azure ile bağlantı oluşturmak `Connect-AzAccount`için çalıştırın.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/application-gateway/waf-rules/waf-custom-rules.ps1 "Custom WAF rules")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Kaynak grubunu, uygulama ağ geçidini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name CustomRulesTest
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Yeni-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Alt ağ yapılandırmasını oluşturur. |
| [Yeni-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Alt ağ yapılandırmalarıyla kullanarak sanal ağı oluşturur. |
| [Yeni-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Uygulama ağ geçidi için genel IP adresini oluşturur. |
| [Yeni-AzApplicationGatewayIPConfiguration](/powershell/module/az.network/new-azapplicationgatewayipconfiguration) | Bir alt ağı uygulama ağ geçidiyle ilişkilendiren yapılandırmayı oluşturur. |
| [Yeni-AzApplicationGatewayFrontendIPConfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig) | Uygulama ağ geçidine genel bir IP adresi atayan yapılandırmayı oluşturur. |
| [Yeni-AzApplicationGatewayFrontendPort](/powershell/module/az.network/new-azapplicationgatewayfrontendport) | Uygulama ağ geçidine erişmek için kullanılacak bir bağlantı noktası atar. |
| [Yeni-AzApplicationGatewayBackendAddressPool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool) | Uygulama ağ geçidi için bir arka uç havuzu oluşturur. |
| [Yeni-AzApplicationGatewayBackendHttpAyarlar](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsetting) | Arka uç havuzu için ayarları yapılandırır. |
| [Yeni-AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener) | Dinleyici oluşturur. |
| [Yeni-AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule) | Yönlendirme kuralı oluşturur. |
| [Yeni-AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku) | Uygulama ağ geçidi için katman ve kapasiteyi belirtin. |
| [Yeni-AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway) | Uygulama ağ geçidi oluşturur. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. |
|[Yeni-AzApplicationGatewayAutoscaleConfiguration](/powershell/module/az.network/New-AzApplicationGatewayAutoscaleConfiguration)|Uygulama Ağ Geçidi için otomatik ölçeklendirme yapılandırması oluşturur.|
|[Yeni-AzApplicationGatewayFirewallMatchVariable](/powershell/module/az.network/New-AzApplicationGatewayFirewallMatchVariable)|Güvenlik duvarı durumu için bir eşleşme değişkeni oluşturur.|
|[Yeni-AzApplicationGatewayFirewallCondition](/powershell/module/az.network/New-AzApplicationGatewayFirewallCondition)|Özel kural için bir eşleşme koşulu oluşturur.|
|[Yeni-AzApplicationGatewayFirewallCustomRule](/powershell/module/az.network/New-AzApplicationGatewayFirewallCustomRule)|Uygulama ağ geçidi güvenlik duvarı ilkesi için yeni bir özel kural oluşturur.|
|[Yeni-AzApplicationGatewayFirewallPolicy](/powershell/module/az.network/New-AzApplicationGatewayFirewallPolicy)|Bir uygulama ağ geçidi güvenlik duvarı ilkesi oluşturur.|
|[Yeni-AzApplicationGatewayWebApplicationFirewallConfiguration](/powershell/module/az.network/New-AzApplicationGatewayWebApplicationFirewallConfiguration)|Bir uygulama ağ geçidi için WAF yapılandırması oluşturur.|

## <a name="next-steps"></a>Sonraki adımlar

- WAF özel kuralları hakkında daha fazla bilgi [için, Web Uygulaması Güvenlik Duvarı için Özel kurallara](../ag/custom-waf-rules-overview.md) bakın
- Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

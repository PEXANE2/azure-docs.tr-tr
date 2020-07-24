---
title: Azure PowerShell Betiği Örneği - Web trafiğini yönetme | Microsoft Docs
description: Azure PowerShell Betik Örneği - Bir uygulama ağ geçidi ve sanal makine ölçek kümesi ile web trafiğini yönetin.
services: application-gateway
documentationcenter: networking
author: vhorne
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/29/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: b0ad5043255c1c8a804245e8e944a8513a958819
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87004826"
---
# <a name="manage-web-traffic-with-azure-powershell"></a>Azure PowerShell ile web trafiğini yönetme

Bu betik, arka uç sunucuları için bir sanal makine ölçek kümesi kullanan bir uygulama ağ geçidi oluşturur. Ardından uygulama ağ geçidi web trafiğini yönetmek üzere yapılandırılabilir. Betiği çalıştırdıktan sonra, genel IP adresini kullanarak uygulama ağ geçidini test edebilirsiniz.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/application-gateway/create-vmss/create-vmss.ps1 "Create application gateway")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Kaynak grubunu, uygulama ağ geçidini ve ilgili tüm kaynakları kaldırmak için aşağıdaki komutu çalıştırın.

```powershell
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik, dağıtımı oluşturmak için aşağıdaki komutları kullanır. Tablodaki her öğe, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) | Alt ağ yapılandırmasını oluşturur. |
| [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) | Alt ağ yapılandırmalarıyla kullanarak sanal ağı oluşturur. |
| [New-Azpublicıpaddress](/powershell/module/az.network/new-azpublicipaddress) | Uygulama ağ geçidi için genel IP adresini oluşturur. |
| [New-Azapplicationgatewayıp yapılandırması](/powershell/module/az.network/new-azapplicationgatewayipconfiguration) | Bir alt ağı uygulama ağ geçidiyle ilişkilendiren yapılandırmayı oluşturur. |
| [New-Azapplicationgatewayfrontendıpconfig](/powershell/module/az.network/new-azapplicationgatewayfrontendipconfig) | Uygulama ağ geçidine genel bir IP adresi atayan yapılandırmayı oluşturur. |
| [New-AzApplicationGatewayFrontendPort](/powershell/module/az.network/new-azapplicationgatewayfrontendport) | Uygulama ağ geçidine erişmek için kullanılacak bir bağlantı noktası atar. |
| [New-Azapplicationgatewaybackendadddresspool](/powershell/module/az.network/new-azapplicationgatewaybackendaddresspool) | Uygulama ağ geçidi için bir arka uç havuzu oluşturur. |
| [New-AzApplicationGatewayBackendHttpSettings](/powershell/module/az.network/new-azapplicationgatewaybackendhttpsetting) | Arka uç havuzu için ayarları yapılandırır. |
| [New-AzApplicationGatewayHttpListener](/powershell/module/az.network/new-azapplicationgatewayhttplistener) | Dinleyici oluşturur. |
| [New-AzApplicationGatewayRequestRoutingRule](/powershell/module/az.network/new-azapplicationgatewayrequestroutingrule) | Yönlendirme kuralı oluşturur. |
| [New-AzApplicationGatewaySku](/powershell/module/az.network/new-azapplicationgatewaysku) | Uygulama ağ geçidi için katman ve kapasiteyi belirtin. |
| [New-AzApplicationGateway](/powershell/module/az.network/new-azapplicationgateway) | Uygulama ağ geçidi oluşturur. |
| [Set-AzVmssStorageProfile](/powershell/module/az.compute/set-azvmssstorageprofile) | Ölçek kümesi için bir depolama profili oluşturun. |
| [Set-AzVmssOsProfile](/powershell/module/az.compute/set-azvmssosprofile) | Ölçek kümesi için işletim sistemini tanımlayın. |
| [Add-Azvmssnetworkınterfaceconfiguration](/powershell/module/az.compute/add-azvmssnetworkinterfaceconfiguration) | Ölçek kümesi için ağ arabirimini tanımlayın. |
| [New-AzVmss](/powershell/module/az.compute/new-azvm) | Sanal makine ölçek kümesi oluşturun. |
| [Get-Azpublicıpaddress](/powershell/module/az.network/get-azpublicipaddress) | Uygulama ağ geçidinin genel IP adresini alır. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Kaynak grubunu ve grubun içerdiği tüm kaynakları kaldırır. | 

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/).

Ek uygulama ağ geçidi PowerShell betiği örnekleri, [Azure Application Gateway belgeleri](../powershell-samples.md) içinde bulunabilir.

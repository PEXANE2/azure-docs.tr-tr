---
title: Powershell'de ağ güvenliği grubu kuralı ekleme
description: Azure PowerShell Komut Dosyası Örneği - Belirli bir bağlantı noktasında gelen trafiğine izin vermek için bir ağ güvenlik grubu ekler.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 11/28/2017
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 33ee8028031bf94d69d11b08c347f84db94494a3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75610429"
---
# <a name="add-an-inbound-network-security-group-rule"></a>Gelen ağ güvenlik grubu kuralı ekleme

Bu örnek komut dosyası, bağlantı noktası 8081'de gelen trafiğe izin vermek için bir ağ güvenlik grubu kuralı oluşturur.  Komut dosyası ağ güvenlik grubunu alır, yeni bir ağ güvenliği yapılandırma kuralı oluşturur ve ağ güvenlik grubunu güncelleştirir. Parametreleri gereken şekilde özelleştirin.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview)bulunan yönergeleri kullanarak Azure PowerShell'i yükleyin. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/add-inbound-nsg-rule/add-inbound-nsg-rule.ps1 "Update the RDP port range values")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Al-AzResource](/powershell/module/az.resources/get-azresource) | `Microsoft.Network/networkSecurityGroups` kaynağını alır. |
|[Get-AzNetworkSecurityGroup](/powershell/module/az.network/get-aznetworksecuritygroup)| Ağ güvenlik grubunu ada göre alır.|
|[Add-AzNetworkSecurityRuleConfig](/powershell/module/az.network/add-aznetworksecurityruleconfig)| Ağ güvenlik grubuna ağ güvenliği kuralı yapılandırması ekler. |
|[Set-AzNetworkSecurityGroup](/powershell/module/az.network/set-aznetworksecuritygroup)| Ağ güvenlik grubu için hedef durumu ayarlar.|

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/overview).

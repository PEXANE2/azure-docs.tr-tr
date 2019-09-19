---
title: Azure 'da özel uç noktalar için ağ ilkelerini devre dışı bırakma
description: Özel uç noktalar için ağ ilkelerini devre dışı bırakmayı öğrenin.
services: private-link
author: KumudD
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: b510b697fbf8b53e9e55e96f60b27967f90893f1
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71104615"
---
# <a name="disable-network-policies-for-private-endpoints"></a>Özel uç noktalar için ağ ilkelerini devre dışı bırak

Ağ güvenlik grupları (NSG) gibi ağ ilkeleri özel uç noktalar için desteklenmez. Belirli bir alt ağda özel bir uç nokta dağıtmak için bu alt ağda açık devre dışı bırakma ayarı gereklidir. Bu ayar yalnızca özel uç nokta için geçerlidir. Alt ağdaki diğer kaynaklar için, erişim ağ güvenlik grupları (NSG) güvenlik kuralları tanımına göre denetlenir. 
 
Özel bir uç nokta oluşturmak için Portal kullanıldığında, bu ayar oluşturma sürecinin bir parçası olarak otomatik olarak devre dışıdır. Diğer istemcileri kullanarak dağıtım, bu ayarı değiştirmek için ek bir adım gerektirir. Cloud Shell 'i Azure portal veya Azure PowerShell, Azure CLı 'nin yerel yüklemelerinden veya Azure Resource Manager şablonları kullanarak ayarı devre dışı bırakabilirsiniz.  
 
Aşağıdaki örneklerde, *myresourcegroup*adlı bir `PrivateEndpointNetworkPolicies` kaynak grubunda barındırılan bir *varsayılan* alt ağ ile *myVirtualNetwork* adlı bir sanal ağ için nasıl devre dışı bırakılacağı açıklanır.

## <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma
Bu bölüm, Azure PowerShell kullanarak alt ağ özel uç nokta ilkelerinin nasıl devre dışı bırakılacağını açıklar.

```azurepowershell
$virtualNetwork= Get-AzVirtualNetwork `
  -Name "myVirtualNetwork" ` 
  -ResourceGroupName "myResourceGroup"  
   
($virtualNetwork ` 
  | Select -ExpandProperty subnets ` 
  | Where-Object  {$_.Name -eq 'default'} ).PrivateEndpointNetworkPolicies = "Disabled" 
 
$virtualNetwork | Set-AzVirtualNetwork 
```
## <a name="using-azure-cli"></a>Azure CLI’yı kullanma
Bu bölümde, Azure CLı kullanılarak alt ağ özel uç nokta ilkelerinin nasıl devre dışı bırakılacağı açıklanır.
```azurecli
az network vnet subnet update \ 
  --name default \ 
  --resource-group myResourceGroup \ 
  --vnet-name myVirtualNetwork \ 
  --disable-private-endpoint-network-policies true
```
## <a name="using-a-template"></a>Şablon kullanma
Bu bölümde, Azure Resource Manager şablonu kullanılarak alt ağ özel uç nokta ilkelerinin nasıl devre dışı bırakılacağı açıklanmaktadır.
```json
{ 
          "name": "myVirtualNetwork", 
          "type": "Microsoft.Network/virtualNetworks", 
          "apiVersion": "2019-04-01", 
          "location": "WestUS", 
          "properties": { 
                "addressSpace": { 
                     "addressPrefixes": [ 
                          "10.0.0.0/16" 
                        ] 
                  }, 
                  "subnets": [ 
                         { 
                                "name": "default", 
                                "properties": { 
                                    "addressPrefix": "10.0.0.0/24", 
                                    "privateEndpointNetworkPolicies": "Disabled" 
                                 } 
                         } 
                  ] 
          } 
} 
```
## <a name="next-steps"></a>Sonraki adımlar
- [Azure özel uç noktası](private-endpoint-overview.md) hakkında daha fazla bilgi edinin
 

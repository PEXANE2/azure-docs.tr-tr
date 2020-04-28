---
title: 'Azure özel bağlantı hizmeti kaynak IP adresi için ağ ilkelerini devre dışı bırak '
description: Azure özel bağlantısı için ağ ilkelerini devre dışı bırakmayı öğrenin
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 4c6bd64d141341e0b7fa5641e04320a95d7951bb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75453002"
---
# <a name="disable-network-policies-for-private-link-service-source-ip"></a>Özel bağlantı hizmeti kaynak IP 'si için ağ ilkelerini devre dışı bırak

Özel bağlantı hizmetiniz için bir kaynak IP adresi seçmek üzere alt ağda bir açık devre dışı bırakma ayarı `privateLinkServiceNetworkPolicies` gereklidir. Bu ayar yalnızca özel bağlantı hizmetinin kaynak IP 'si olarak seçtiğiniz özel IP adresi için geçerlidir. Alt ağdaki diğer kaynaklar için, erişim ağ güvenlik grupları (NSG) güvenlik kuralları tanımına göre denetlenir. 
 
Herhangi bir Azure istemcisi (PowerShell, CLı veya şablon) kullanırken, bu özelliği değiştirmek için ek bir adım gerekir. Azure portal, Cloud Shell 'i kullanarak veya Azure PowerShell, Azure CLı 'nin yerel yüklemelerinden veya Azure Resource Manager şablonları kullanarak ilkeyi devre dışı bırakabilirsiniz.  
 
*MyVirtualNetwork* adlı bir sanal ağ için özel bağlantı hizmeti ağ Ilkelerini, *myresourcegroup*adlı bir kaynak grubunda barındırılan bir *varsayılan* alt ağ ile devre dışı bırakmak için aşağıdaki adımları izleyin. 

## <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma
Bu bölüm, Azure PowerShell kullanarak alt ağ özel uç nokta ilkelerinin nasıl devre dışı bırakılacağını açıklar.

```azurepowershell
$virtualNetwork= Get-AzVirtualNetwork `
  -Name "myVirtualNetwork" ` 
  -ResourceGroupName "myResourceGroup"  
   
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq 'default'} ).privateLinkServiceNetworkPolicies = "Disabled"  
 
$virtualNetwork | Set-AzVirtualNetwork 
```
## <a name="using-azure-cli"></a>Azure CLI’yı kullanma
Bu bölümde, Azure CLı kullanılarak alt ağ özel uç nokta ilkelerinin nasıl devre dışı bırakılacağı açıklanır.
```azurecli
az network vnet subnet update \ 
  --name default \ 
  --resource-group myResourceGroup \ 
  --vnet-name myVirtualNetwork \ 
  --disable-private-link-service-network-policies true 
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
                        "privateLinkServiceNetworkPolicies": "Disabled" 
                    } 
                } 
        ] 
    } 
} 
 
```
## <a name="next-steps"></a>Sonraki adımlar
- [Azure özel uç noktası](private-endpoint-overview.md) hakkında daha fazla bilgi edinin
 

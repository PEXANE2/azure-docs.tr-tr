---
title: 'Azure Private Link hizmet kaynağı IP adresi için ağ ilkelerini devre dışı '
description: Azure özel Bağlantısı için ağ ilkelerini nasıl devre dışı dışı kaçıklayabilme yi öğrenin
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: 4c6bd64d141341e0b7fa5641e04320a95d7951bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75453002"
---
# <a name="disable-network-policies-for-private-link-service-source-ip"></a>Private Link hizmet kaynağı IP için ağ ilkelerini devre dışı

Private Link hizmetiniz için bir kaynak IP adresi seçmek için `privateLinkServiceNetworkPolicies` alt ağda açık bir devre dışı alma ayarı gereklidir. Bu ayar yalnızca Özel Bağlantı hizmetinin kaynak IP'si olarak seçtiğiniz özel IP adresi için geçerlidir. Alt ağdaki diğer kaynaklar için erişim, Ağ Güvenlik Grupları (NSG) güvenlik kuralları tanımına göre denetlenir. 
 
Herhangi bir Azure istemcisi (PowerShell, CLI veya şablonlar) kullanırken, bu özelliği değiştirmek için ek bir adım gerekir. Azure portalındaki bulut kabuğunu veya Azure PowerShell, Azure CLI'nin yerel yüklemelerini kullanarak ilkeyi devre dışı kılabilir veya Azure Kaynak Yöneticisi şablonlarını kullanabilirsiniz.  
 
*myResourceGroup*adlı bir kaynak grubunda barındırılan *varsayılan* bir alt ağ ile *myVirtualNetwork* adlı sanal bir ağ için özel bağlantı hizmeti ağı ilkelerini devre dışı bırakabilmek için aşağıdaki adımları izleyin. 

## <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma
Bu bölümde, Azure PowerShell kullanarak özel uç nokta ilkelerinin nasıl devre dışı atılabildiği açıklanmaktadır.

```azurepowershell
$virtualNetwork= Get-AzVirtualNetwork `
  -Name "myVirtualNetwork" ` 
  -ResourceGroupName "myResourceGroup"  
   
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq 'default'} ).privateLinkServiceNetworkPolicies = "Disabled"  
 
$virtualNetwork | Set-AzVirtualNetwork 
```
## <a name="using-azure-cli"></a>Azure CLI’yı kullanma
Bu bölümde, Azure CLI kullanarak alt nokta ilkeleri nasıl devre dışı kığına açıklanmaktadır.
```azurecli
az network vnet subnet update \ 
  --name default \ 
  --resource-group myResourceGroup \ 
  --vnet-name myVirtualNetwork \ 
  --disable-private-link-service-network-policies true 
```
## <a name="using-a-template"></a>Şablon kullanma
Bu bölümde, Azure Kaynak Yöneticisi Şablonu'nun uyrması kullanılarak özel uç nokta ilkeleri nasıl devre dışı kağıda çıkarılabildiği açıklanmaktadır.
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
- [Azure Özel Bitiş Noktası](private-endpoint-overview.md) hakkında daha fazla bilgi edinin
 

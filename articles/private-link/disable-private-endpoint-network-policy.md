---
title: Azure'da özel uç noktalar için ağ ilkelerini devre dışı
description: Özel uç noktalar için ağ ilkelerini nasıl devre dışı kağıda tizleöğreneceğini öğrenin.
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: allensu
ms.openlocfilehash: b5ab62e7ab57d32a11a45713519633034deb6a5b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75453010"
---
# <a name="disable-network-policies-for-private-endpoints"></a>Özel uç noktalar için ağ ilkelerini devre dışı

Ağ güvenlik grupları (NSG) gibi ağ ilkeleri özel uç noktalar için desteklenmez. Belirli bir alt ağa Özel Bitiş Noktası dağıtmak için, bu alt ağda açık bir devre dışı alma ayarı gereklidir. Bu ayar yalnızca Özel Bitiş Noktası için geçerlidir. Alt ağdaki diğer kaynaklar için erişim, Ağ Güvenlik Grupları (NSG) güvenlik kuralları tanımına göre denetlenir. 
 
Özel bir bitiş noktası oluşturmak için portalı kullanırken, bu ayar oluşturma işleminin bir parçası olarak otomatik olarak devre dışı bırakılır. Diğer istemcileri kullanarak dağıtım, bu ayarı değiştirmek için ek bir adım gerektirir. Azure portalından bulut bulutu veya Azure PowerShell, Azure CLI'nin yerel yüklemelerini kullanarak ayarı devre dışı kalabilir veya Azure Kaynak Yöneticisi şablonlarını kullanabilirsiniz.  
 
Aşağıdaki örnekler, *myResourceGroup* `PrivateEndpointNetworkPolicies` adlı bir kaynak grubunda barındırılan *varsayılan* bir alt ağ ile *myVirtualNetwork* adlı sanal bir ağ için devre dışı nasıl açıklanır.

## <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma
Bu bölümde, Azure PowerShell kullanarak özel uç nokta ilkelerinin nasıl devre dışı atılabildiği açıklanmaktadır.

```azurepowershell
$virtualNetwork= Get-AzVirtualNetwork `
  -Name "myVirtualNetwork" ` 
  -ResourceGroupName "myResourceGroup"  
   
($virtualNetwork | Select -ExpandProperty subnets | Where-Object  {$_.Name -eq 'default'} ).PrivateEndpointNetworkPolicies = "Disabled" 
 
$virtualNetwork | Set-AzVirtualNetwork 
```
## <a name="using-azure-cli"></a>Azure CLI’yı kullanma
Bu bölümde, Azure CLI kullanarak alt nokta ilkeleri nasıl devre dışı kığına açıklanmaktadır.
```azurecli
az network vnet subnet update \ 
  --name default \ 
  --resource-group myResourceGroup \ 
  --vnet-name myVirtualNetwork \ 
  --disable-private-endpoint-network-policies true
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
                                    "privateEndpointNetworkPolicies": "Disabled" 
                                 } 
                         } 
                  ] 
          } 
} 
```
## <a name="next-steps"></a>Sonraki adımlar
- [Azure özel bitiş noktası](private-endpoint-overview.md) hakkında daha fazla bilgi edinin
 

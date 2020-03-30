---
title: Azure Red Hat OpenShift 3.11 | Microsoft Dokümanlar
description: Azure Red Hat OpenShift 3.11 ile özel küme oluşturma
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 03/02/2020
keywords: aro, openshift, özel küme, kırmızı şapka
ms.openlocfilehash: b34b5d622527742447847102526eba9ee6ca220d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78399425"
---
# <a name="create-a-private-cluster-with-azure-red-hat-openshift-311"></a>Azure Red Hat OpenShift 3.11 ile özel küme oluşturma

> [!IMPORTANT]
> Azure Red Hat OpenShift (ARO) özel kümeleri şu anda yalnızca Doğu ABD 2'de özel önizlemede kullanılabilir. Özel önizleme kabul sadece davetiye ile. Lütfen bu özelliği etkinleştirmeye çalışmadan önce aboneliğinizi kaydettiğinizden emin olun.

Özel kümeler aşağıdaki avantajları sağlar:

* Özel kümeler, genel bir IP adresinde küme denetim düzlemi bileşenlerini (API sunucuları gibi) açığa çıkarmaz.
* Özel bir kümenin sanal ağı müşteriler tarafından yapılandırılabilir ve ExpressRoute ortamları da dahil olmak üzere diğer sanal ağlarla eşleme olanağı sağlayacak ağ kurmanıza olanak tanır. Ayrıca, sanal ağdaki özel DNS'yi dahili hizmetlerle tümleştirmek için yapılandırabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce

> [!NOTE]
> Bu özellik, ARO HTTP API sürümü 2019-10-27-önizleme gerektirir. Azure CLI'de henüz desteklenmez.

Aşağıdaki yapılandırma snippet alanları yenidir ve küme yapılandırmanıza dahil edilmelidir. `managementSubnetCidr`küme sanal ağ içinde olmalıdır ve kümeyi yönetmek için Azure tarafından kullanılır.

```json
properties:
 networkProfile:
   managementSubnetCidr: 10.0.1.0/24
 masterPoolProfile:
   apiProperties:
     privateApiServer: true
```

Özel bir küme, aşağıda sağlanan örnek komut dosyaları kullanılarak dağıtılabilir. Küme dağıtıldıktan sonra, openshift API sunucusunun özel IP adresini belirlemek için komutu çalıştırın `cluster get` ve `properties.FQDN` özelliği görüntüleyin.

Küme sanal ağ, değiştirebilmeniz için izinlerle oluşturulmuş olur. Daha sonra ihtiyaçlarınız için gerekli sanal ağa (ExpressRoute, VPN, sanal ağ izleme) erişmek için ağ ayarlayabilirsiniz.

Küme sanal ağındaki DNS ad sunucularını değiştirirseniz, VM'lerin yeniden görüntülenebilmeleri `true` için kümeüzerinde `properties.RefreshCluster` özellik ayarlanmış bir güncelleştirme düzenlemeniz gerekir. Bu güncelleştirme, yeni ad sunucularını almalarına olanak sağlar.

## <a name="sample-configuration-scripts"></a>Örnek yapılandırma komut dosyaları

Özel kümenizi kurmak ve dağıtmak için bu bölümdeki örnek komut dosyalarını kullanın.

### <a name="environment"></a>Ortam

Kendi değerlerinizi kullanarak aşağıdaki ortam değişkenlerini doldurun.

> [!NOTE]
> Bu, şu anda özel kümeler için desteklenen tek konum olduğundan, konum ayarlanmalıdır. `eastus2`

``` bash
export CLUSTER_NAME=
export LOCATION=eastus2
export TOKEN=$(az account get-access-token --query 'accessToken' -o tsv)
export SUBID=
export TENANT_ID=
export ADMIN_GROUP=
export CLIENT_ID=
export SECRET=
```

### <a name="private-clusterjson"></a>özel cluster.json

Yukarıda tanımlanan ortam değişkenlerini kullanarak, özel küme etkin leştirilmiş bir örnek küme yapılandırması aşağıda veda edebilirsiniz.

```json
{
   "location": "$LOCATION",
   "name": "$CLUSTER_NAME",
   "properties": {
       "openShiftVersion": "v3.11",
       "networkProfile": {
           "vnetCIDR": "10.0.0.0/8",
           "managementSubnetCIDR" : "10.0.1.0/24"
       },
       "authProfile": {
           "identityProviders": [
               {
                   "name": "Azure AD",
                   "provider": {
                       "kind": "AADIdentityProvider",
                       "clientId": "$CLIENT_ID",
                       "secret": "$SECRET",
                       "tenantId": "$TENANT_ID",
                       "customerAdminGroupID": "$ADMIN_GROUP"
                   }
               }
           ]
       },
       "masterPoolProfile": {
           "name": "master",
           "count": 3,
           "vmSize": "Standard_D4s_v3",
           "osType": "Linux",
           "subnetCIDR": "10.0.0.0/24",
           "apiProperties": {
                "privateApiServer": true
            }
       },
       "agentPoolProfiles": [
           {
               "role": "compute",
               "name": "compute",
               "count": 1,
               "vmSize": "Standard_D4s_v3",
               "osType": "Linux",
               "subnetCIDR": "10.0.0.0/24"
           },
           {
               "role": "infra",
               "name": "infra",
               "count": 3,
               "vmSize": "Standard_D4s_v3",
               "osType": "Linux",
               "subnetCIDR": "10.0.0.0/24"
           }
       ],
       "routerProfiles": [
           {
               "name": "default"
           }
       ]
   }
}
```

## <a name="deploy-a-private-cluster"></a>Özel küme dağıtma

Özel kümenizi yukarıdaki örnek komut dosyalarıyla yapılandırdıktan sonra, özel kümenizi dağıtmak için aşağıdaki komutu çalıştırın.

``` bash
az group create --name $CLUSTER_NAME --location $LOCATION
cat private-cluster.json | envsubst | curl -v -X PUT \
-H 'Content-Type: application/json; charset=utf-8' \
-H 'Authorization: Bearer '$TOKEN'' -d @- \
 https://management.azure.com/subscriptions/$SUBID/resourceGroups/$CLUSTER_NAME/providers/Microsoft.ContainerService/openShiftManagedClusters/$CLUSTER_NAME?api-version=2019-10-27-preview
```

## <a name="next-steps"></a>Sonraki adımlar

OpenShift konsoluna nasıl erişilacağı hakkında bilgi edinmek için [Web Konsolu Walkthrough'a](https://docs.openshift.com/container-platform/3.11/getting_started/developers_console.html)bakın.

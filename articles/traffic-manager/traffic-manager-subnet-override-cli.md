---
title: Azure CLI kullanarak Azure Trafik Yöneticisi alt net geçersiz kılma | Microsoft Dokümanlar
description: Bu makalede, Trafik Yöneticisi alt net geçersiz kılma nın, trafiği bitiş noktası eşlemelerine önceden tanımlanmış IP aralığı üzerinden son kullanıcı IP adresine göre bir uç noktasına yönlendirmek için Trafik Yöneticisi profilinin yönlendirme yöntemini geçersiz kılmak için nasıl kullanılabileceğini anlamanıza yardımcı olacaktır.
services: traffic-manager
documentationcenter: ''
author: rohinkoul
manager: twooley
ms.topic: article
ms.service: traffic-manager
ms.date: 09/18/2019
ms.author: rohink
ms.openlocfilehash: 818b692884bd9d31efd08663a582ebcfec2032e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938463"
---
# <a name="traffic-manager-subnet-override-using-azure-cli"></a>Azure CLI kullanarak Trafik Yöneticisi alt net geçersiz kılma

Trafik Yöneticisi alt net geçersiz kılma, bir profilin yönlendirme yöntemini değiştirmenize olanak tanır.  Geçersiz kılmanın eklenmesi, son kullanıcının IP adresine bağlı olarak trafiği, önceden tanımlanmış bir IP aralığıyla uç nokta eşlemesi ile yönlendirir. 

## <a name="how-subnet-override-works"></a>Alt ağ geçersiz kılma nasıl çalışır?

Alt ağ geçersiz kılmaları bir trafik yöneticisi profiline eklendiğinde, Trafik Yöneticisi önce son kullanıcının IP adresi için bir alt net geçersiz kılma olup olmadığını denetler. Biri bulunursa, kullanıcının DNS sorgusu ilgili bitiş noktasına yönlendirilir.  Eşleme bulunamazsa, Trafik Yöneticisi profilin özgün yönlendirme yöntemine geri döner. 

IP adresi aralıkları CIDR aralıkları (örneğin, 1.2.3.0/24) veya adres aralıkları (örneğin, 1.2.3.4-5.6.7.8) olarak belirtilebilir. Her uç noktayla ilişkili IP aralıkları bu uç noktaya özgü olmalıdır. IP aralıklarının farklı uç noktalar arasında çakışması, profilin Trafik Yöneticisi tarafından reddedilmesine neden olur.

Alt ağı geçersiz kılan iki tür yönlendirme profili vardır:

* **Coğrafi** - Trafik Yöneticisi DNS sorgusunun IP adresi için bir alt ağ geçersiz kılma bulursa, son noktanın durumu ne olursa olsun sorguyu bitiş noktasına yönlendirir.
* **Performans** - Trafik Yöneticisi DNS sorgusunun IP adresi için bir alt ağ geçersiz kılma bulursa, trafiği yalnızca sağlıklıysa bitiş noktasına yönlendirir.  Alt ağ bitiş ucunu geçersiz kılma sağlıklı değilse Trafik Yöneticisi performans yönlendirme sezgisel geri düşecek.

## <a name="create-a-traffic-manager-subnet-override"></a>Trafik Yöneticisi alt ağ geçersiz kılma oluşturma

Trafik Yöneticisi alt ağ geçersiz kılma oluşturmak için, geçersiz kılmanın alt ağlarını Trafik Yöneticisi bitiş noktasına eklemek için Azure CLI'yi kullanabilirsiniz.

## <a name="azure-cli"></a>Azure CLI

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.28 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="update-the-traffic-manager-endpoint-with-subnet-override"></a>Trafik Yöneticisi bitiş noktasını alt ağ geçersiz kılma yla güncelleştirin.
Az ağ trafiği yöneticisi bitiş [noktası güncelleştirmesiyle](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint?view=azure-cli-latest#az-network-traffic-manager-endpoint-update)bitiş noktanızı güncelleştirmek için Azure CLI'yi kullanın.

```azurecli

### Add a range of IPs ###
az network traffic-manager endpoint update \
    --name MyEndpoint \
    --profile-name MyTmProfile \
    --resource-group MyResourceGroup \
    --subnets 1.2.3.4-5.6.7.8 \
    --type AzureEndpoints

### Add a subnet ###
az network traffic-manager endpoint update \
    --name MyEndpoint \
    --profile-name MyTmProfile \
    --resource-group MyResourceGroup \
    --subnets 9.10.11.0:24 \
    --type AzureEndpoints

```

[AZ ağ trafik yöneticisi uç noktası güncelleştirmesini](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint?view=azure-cli-latest#az-network-traffic-manager-endpoint-update) **--kaldır** seçeneğiyle çalıştırarak IP adresi aralıklarını kaldırabilirsiniz.

```azurecli

az network traffic-manager endpoint update \
    --name MyEndpoint \
    --profile-name MyTmProfile \
    --resource-group MyResourceGroup \
    --remove subnets \
    --type AzureEndpoints

```
## <a name="next-steps"></a>Sonraki Adımlar
Trafik Yöneticisi [trafik yönlendirme yöntemleri](traffic-manager-routing-methods.md)hakkında daha fazla bilgi edinin.

[Subnet trafik yönlendirme yöntemi](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods#subnet-traffic-routing-method) hakkında bilgi edinin
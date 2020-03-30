---
title: Azure SignalR Hizmeti'nin bir örneğini ölçeklendirin
description: Azure portalı veya Azure CLI aracılığıyla kapasite eklemek veya azaltmak için Bir Azure Sinyal Hizmeti örneğini nasıl ölçeklendireceklerini öğrenin.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 12/11/2019
ms.author: zhshang
ms.openlocfilehash: c8d74342e624b837c7ee803a2bcdcc12a3fb814b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75659296"
---
# <a name="how-to-scale-an-azure-signalr-service-instance"></a>Azure Sinyal Hizmeti örneğini ölçeklendirme nasıl?
Bu makalede, Azure SinyalR Hizmeti örneğini nasıl ölçeklendirileceğinizi gösterilmektedir. Ölçeklendirme, ölçeklendirme ve ölçeklendirme için iki senaryo vardır.

* [Ölçeklendirme](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Daha fazla birim, bağlantı, mesaj ve daha fazlasını alın. Fiyatlandırma katmanını Ücretsiz'den Standard'a değiştirerek ölçeklendirirsiniz.
* [Ölçeklendir](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): SignalR birimlerinin sayısını artırın. 100 adede kadar ölçeklendirebilirsiniz.

Ölçek ayarlarının uygulanması birkaç dakika sürer. Nadir durumlarda, uygulamak için yaklaşık 30 dakika sürebilir. Kodunuzu değiştirmenizi veya sunucu uygulamanızı yeniden dağıtmanızı gerektirmezler.

Tek tek SignalR Hizmetinin fiyatlandırması ve kapasiteleri hakkında bilgi için [Azure SignalR Hizmet Fiyatlandırma Ayrıntıları'na](https://azure.microsoft.com/pricing/details/signalr-service/)bakın.  

> [!NOTE]
> SignalR Hizmetini **Ücretsiz** katmandan **Standart** katmana veya tam tersi olarak değiştirmek, kamu hizmeti IP'si değiştirilir ve genellikle tüm internet üzerinden DNS sunucularında yapılan değişikliği yaymak 30-60 dakika sürer. DNS güncelleştirilmeden önce hizmetiniz erişilemeyebilir. Genellikle fiyatlandırma katmanınızı çok sık değiştirmeniz önerilmez.


## <a name="scale-on-azure-portal"></a>Azure portalında ölçekle

1. Tarayıcınızda [Azure portalı](https://portal.azure.com)’nı açın.

2. SignalR Hizmeti sayfanızda, sol menüden **Ölçek'i**seçin.
   
3. Fiyatlandırma katmanınızı seçin ve sonra **Seç'i**tıklatın. Birim sayısını **Standart** Katman için ayarlayın.
   
    ![Portalda Ölçeklendir](./media/signalr-howto-scale/signalr-howto-scale.png)

4. **Kaydet**'e tıklayın.

## <a name="scale-using-azure-cli"></a>Azure CLI'yi kullanarak ölçeklendirme

Bu komut dosyası, **Free** Tier'in yeni bir SignalR Service kaynağı nı ve yeni bir kaynak grubunu oluşturur ve standart **katmana** kadar ölçeklendirir. 

```azurecli-interactive
#!/bin/bash

# Generate a unique suffix for the service name
let randomNum=$RANDOM*$RANDOM

# Generate a unique service and group name with the suffix
SignalRName=SignalRTestSvc$randomNum
#resource name must be lowercase
mySignalRSvcName=${SignalRName,,}
myResourceGroupName=$SignalRName"Group"

# Create resource group 
az group create --name $myResourceGroupName --location eastus

# Create the Azure SignalR Service resource
az signalr create \
  --name $mySignalRSvcName \
  --resource-group $myResourceGroupName \
  --sku Free_F1 \
  --service-mode Default

# Scale up to Standard Tier, and scale out to 50 units
az signalr update \
  --name $mySignalRSvcName \
  --resource-group $myResourceGroupName \
  --sku Standard_S1 \
  --unit-count 50
```

Yeni kaynak grubu için oluşturulan gerçek adı not edin. Tüm grup kaynaklarını silmek istediğinizde bu kaynak grubu adını kullanacaksınız.

[!INCLUDE [cli-script-clean-up](../../includes/cli-script-clean-up.md)]

## <a name="compare-pricing-tiers"></a>Fiyatlandırma katmanlarını karşılaştırın

Her fiyatlandırma katmanı için dahil edilen mesajlar ve bağlantılar gibi ayrıntılı bilgi için [SignalR Hizmet Fiyatlandırma Ayrıntıları'na](https://azure.microsoft.com/pricing/details/signalr-service/)bakın.

Her katmandaki hizmet sınırları, kotalar ve kısıtlamalar tablosu için [SignalR Hizmet sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-signalr-service-limits)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, tek SignalR Hizmeti örneğini nasıl ölçeklendirdiğinizi öğrendiniz.

Ölçekleme, parçalama ve bölgeler arası senaryolar için birden çok uç nokta da desteklenir.

> [!div class="nextstepaction"]
> [birden çok örnekle sinyalr hizmetini ölçeklendir](./signalr-howto-scale-multi-instances.md)

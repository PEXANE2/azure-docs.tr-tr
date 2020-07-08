---
title: Azure SignalR hizmeti örneğini ölçeklendirme
description: Azure portal veya Azure CLı aracılığıyla kapasiteyi eklemek veya azaltmak için bir Azure SignalR hizmeti örneğini ölçeklendirmenin nasıl yapılacağını öğrenin.
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 12/11/2019
ms.author: zhshang
ms.openlocfilehash: c8d74342e624b837c7ee803a2bcdcc12a3fb814b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75659296"
---
# <a name="how-to-scale-an-azure-signalr-service-instance"></a>Azure SignalR hizmet örneğini ölçeklendirme
Bu makalede, Azure SignalR hizmeti örneğinizin nasıl ölçeklenmesi gösterilmektedir. Ölçeklendirme, ölçek artırma ve genişleme için iki senaryo vardır.

* [Ölçeği artırma](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): daha fazla birim, bağlantı, ileti ve daha fazlasını öğrenin. Fiyatlandırma katmanını ücretsiz olarak standart olarak değiştirerek ölçeği değiştirmiş olursunuz.
* [Ölçeği genişletme](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): SignalR birimlerinin sayısını artırın. Ölçeği 100 birim kadar genişletebilirsiniz.

Ölçek ayarlarının uygulanması birkaç dakika sürer. Nadir durumlarda, uygulanması yaklaşık 30 dakika sürebilir. Bunlar, kodunuzun değiştirilmesini veya sunucu uygulamanızı yeniden dağıtmanıza gerek kalmaz.

Tek bir SignalR hizmetinin fiyatlandırma ve kapasiteleri hakkında daha fazla bilgi için bkz. [Azure SignalR hizmeti fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/signalr-service/).  

> [!NOTE]
> SignalR hizmetini **ücretsiz** katmandan **Standart** katmana değiştirme veya bunun tersini yapmak, genel hizmet IP 'si değiştirilir ve genellikle DEĞIŞIKLIĞI tüm internet genelinde DNS sunucularına yaymaya 30-60 dakika sürer. DNS 'in güncelleştirilebilmesi için hizmetinize ulaşılamıyor olabilir. Genellikle fiyatlandırma katmanınızı çok sık değiştirmeniz önerilmez.


## <a name="scale-on-azure-portal"></a>Azure portal ölçeklendirin

1. Tarayıcınızda [Azure portalı](https://portal.azure.com)’nı açın.

2. SignalR hizmeti sayfanızda, sol menüden **Ölçek**' i seçin.
   
3. Fiyatlandırma katmanınızı seçin ve ardından **Seç**' e tıklayın. **Standart** katman için birim sayısını ayarlayın.
   
    ![Portalda Ölçeklendir](./media/signalr-howto-scale/signalr-howto-scale.png)

4. **Kaydet**’e tıklayın.

## <a name="scale-using-azure-cli"></a>Azure CLı kullanarak ölçeklendirme

Bu betik, **ücretsiz** katmanda yeni bir SignalR hizmeti kaynağı ve yeni bir kaynak grubu oluşturur ve **Standart** katmana ölçeklendirir. 

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

Her fiyatlandırma katmanı için dahil edilen mesajlar ve bağlantılar gibi ayrıntılı bilgiler için bkz. [SignalR hizmeti fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/signalr-service/).

Her katmandaki hizmet limitleri, Kotalar ve kısıtlamalar tablosu için bkz. [SignalR hizmet limitleri](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-signalr-service-limits).

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, tek bir SignalR hizmet örneğinin nasıl ölçeklendirileceğini öğrendiniz.

Ölçeklendirme, parçalama ve çapraz bölge senaryolarında de birden fazla uç nokta desteklenir.

> [!div class="nextstepaction"]
> [SignalR hizmetini birden çok örnek ile ölçeklendirme](./signalr-howto-scale-multi-instances.md)

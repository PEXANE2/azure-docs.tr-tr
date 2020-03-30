---
title: Otomatik olarak iş büyütme birimlerini ölçeklendirin - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Otomatik olarak iş birimi birimlerini ölçeklendirmek için ad alanında Otomatik şişirme'yi etkinleştirin.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: dc6edaebebe89b6d4a35ada58d40795f86a935d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72264481"
---
# <a name="automatically-scale-up-azure-event-hubs-throughput-units"></a>Azure Etkinlik Hub'larını otomatik olarak ölçeklendirin
Azure Etkinlik Hub'ları son derece ölçeklenebilir bir veri akışı platformudur. Bu nedenle, Olay Hub'ları kullanımı genellikle hizmeti kullanmaya başladıktan sonra artar. Bu tür kullanım, Olay Hub'larını ölçeklendirmek ve daha büyük aktarım hızlarını işlemek için önceden belirlenmiş [iş aktarıbirimlerinin](event-hubs-scalability.md#throughput-units) artırılmasını gerektirir. Olay Hub'larının **Otomatik şişirme** özelliği, kullanım gereksinimlerini karşılamak için iş birimi sayısını artırarak otomatik olarak ölçeklendirilir. İş birimi birimlerinin artırılması, aşağıdakisenaryoların daraltılmasını önler:

* Veri giriş hızları ayarlanan iş birimi birimlerini aşıyor.
* Veri çıkış isteği oranları, ayarlanan iş birimi birimlerini aşar.

Olay Hub'ları hizmeti, yük minimum eşiğin dışına çıktığında, ServerBusy hatalarıyla ilgili herhangi bir istek başarısız olmadan iş verisini artırır.

## <a name="how-auto-inflate-works"></a>Otomatik şişirme nasıl çalışır?

Olay Hub'ları [trafiği, iş birimi birimleri](event-hubs-scalability.md#throughput-units)tarafından denetlenir. Tek bir iş birimi saniyede 1 MB giriş ve bu miktarın iki katına izin verir. Standart olay hub'ları 1-20 üretim birimiyle yapılandırılabilir. Otomatik şişirme, seçtiğiniz minimum gerekli iş birimiyle küçük başlatmanızı sağlar. Özellik, trafiğinizdeki artışa bağlı olarak otomatik olarak ihtiyacınız olan üretim birimlerinin maksimum sınırına ölçeklendirilir. Otomatik şişirme aşağıdaki avantajları sağlar:

- Küçük başlatmak ve büyüdükçe ölçeklendirmek için etkili bir ölçeklendirme mekanizması.
- Otomatik olarak azaltma sorunları olmadan belirtilen üst sınıra ölçeklendirin.
- Ölçekleme üzerinde daha fazla denetim, çünkü ne zaman ve ne kadar ölçeklendirilecek kadarını siz kontrol esiniz.

## <a name="enable-auto-inflate-on-a-namespace"></a>Ad alanında Otomatik şişirme'yi etkinleştirme

Aşağıdaki yöntemlerden birini kullanarak Standart katman Olay Hub'ları ad alanında Otomatik şişirme'yi etkinleştirebilir veya devre dışı kullanabilirsiniz:

- [Azure portalı.](https://portal.azure.com)
- [Azure Kaynak Yöneticisi şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate).

> [!NOTE]
> Temel katman Olay Hub'ları ad alanları Otomatik şişirme desteklemez.

### <a name="enable-auto-inflate-through-the-portal"></a>Portal üzerinden otomatik şişirme etkinleştirme


#### <a name="enable-at-the-time-of-creation"></a>Oluşturma sırasında etkinleştirme 
**Olay Hub'ları ad alanı oluştururken**Otomatik şişirme özelliğini etkinleştirebilirsiniz:
 
![Olay göbeği oluşturma sırasında otomatik şişirme etkinleştirme](./media/event-hubs-auto-inflate/event-hubs-auto-inflate1.png)

Bu seçenek etkinleştirildiğinde, üretim birimlerinizle küçük bir başlangıç yapabilir ve kullanım gereksinimleriniz arttıkça ölçeklendirebilirsiniz. Enflasyon için üst sınır, saat başına kullanılan iş başı birim sayısına bağlı olarak fiyatlandırmayı hemen etkilemez.

#### <a name="enable-auto-inflate-for-an-existing-event-hub"></a>Varolan bir olay hub'ı için otomatik şişirme sağlama
Ayrıca, otomatik şişirme özelliğini etkinleştirebilir ve aşağıdaki yönergeleri kullanarak ayarlarını değiştirebilirsiniz: 
 
1. Olay **Hub'ları Ad Alanı** sayfasında, **Otomatik şişirme iş birimi**altında Devre **Dışı'** yı seçin.  

    ![Olay Hub'ları ad alanı sayfasında iş yeri birimlerini seçme](./media/event-hubs-auto-inflate/select-throughput-units.png)
2. Ölçek **Ayarları** sayfasında, **Etkinleştir** için onay kutusunu seçin (otomatik ölçek özelliği etkinleştirilmeseydi).

    ![Etkinleştir'i seçin](./media/event-hubs-auto-inflate/scale-settings.png)
3. **Maksimum** iş birimi sayısını girin veya değeri ayarlamak için kaydırma çubuğunu kullanın. 
4. (isteğe bağlı) Bu sayfanın üst kısmındaki **en az** iş birimi sayısını güncelleştirin. 


> [!NOTE]
> İş birimi birimlerini artırmak için otomatik şişirme yapılandırmasını uyguladığınızda, Olay Hub'ları hizmeti, üretim inneden ve ne zaman arttığı hakkında bilgi veren tanılama günlükleri yayar. Bir olay hub'ı için tanılama günlüğe kaydetmeyi etkinleştirmek için, Azure portalındaki Event Hub sayfasındaki sol menüdeki **Tanılama ayarlarını** seçin. Daha fazla bilgi için [bkz.](event-hubs-diagnostic-logs.md) 

### <a name="enable-auto-inflate-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak Otomatik Şişirme'yi etkinleştirme

Azure Kaynak Yöneticisi şablon dağıtımı sırasında Otomatik şişirme'yi etkinleştirebilirsiniz. Örneğin, `isAutoInflateEnabled` özelliği **doğru** olarak ayarlayın `maximumThroughputUnits` ve 10 olarak ayarlayın. Örnek:

```json
"resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('namespaceName')]",
            "type": "Microsoft.EventHub/Namespaces",
            "location": "[variables('location')]",
            "sku": {
                "name": "Standard",
                "tier": "Standard"
            },
            "properties": {
                "isAutoInflateEnabled": true,
                "maximumThroughputUnits": 10
            },
            "resources": [
                {
                    "apiVersion": "2017-04-01",
                    "name": "[parameters('eventHubName')]",
                    "type": "EventHubs",
                    "dependsOn": [
                        "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
                    ],
                    "properties": {},
                    "resources": [
                        {
                            "apiVersion": "2017-04-01",
                            "name": "[parameters('consumerGroupName')]",
                            "type": "ConsumerGroups",
                            "dependsOn": [
                                "[parameters('eventHubName')]"
                            ],
                            "properties": {}
                        }
                    ]
                }
            ]
        }
    ]
```

Şablonun tamamı için [Etkinlik Hub'ları Oluştur ad alanına](https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-inflate) bakın ve GitHub'da şablonu şişirme'yi etkinleştirin.


## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Olay Hub'larına genel bakış](event-hubs-what-is-event-hubs.md)


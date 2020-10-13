---
title: Azure Service Bus-mesajlaşma varlıklarını askıya alma
description: Bu makalede Azure Service Bus ileti varlıklarının (kuyruklar, konular ve abonelikler) geçici olarak askıya alınması ve yeniden etkinleştirilmesi açıklanmaktadır.
ms.topic: article
ms.date: 09/29/2020
ms.openlocfilehash: f89e17e494cc777691b7f7ca47538cd29114d2dc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91575267"
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Mesajlaşma varlıklarını askıya alma ve yeniden etkinleştirme (devre dışı bırakma)

Kuyruklar, konular ve abonelikler geçici olarak askıya alınabilir. Askıya alma, varlığı tüm iletilerin depolamada bulunduğu devre dışı durumuna geçirir. Ancak, iletiler kaldırılamaz veya eklenemez ve ilgili protokol işlemleri hatalara sebep olur.

Bir varlığın askıya alınması genellikle acil yönetim nedenleriyle yapılır. Tek bir senaryo, iletileri kuyruktan alan, işleme başarısız olan ve iletileri yanlış bir şekilde tamamlayan ve bunları kaldıran hatalı bir alıcı dağıtmış olur. Bu davranış tanılıyorsa, düzeltilen kod dağıtılana ve hatalı kodun neden olduğu daha fazla veri kaybı önyükleninceye kadar kuyruk alma için devre dışı bırakılabilir.

Askıya alma veya yeniden etkinleştirme, Kullanıcı ya da sistem tarafından gerçekleştirilebilir. Sistem yalnızca abonelik harcama sınırına vurmaya yönelik aksan nedeniyle yönetim nedenlerinden dolayı varlıkları askıya alır. Sistem devre dışı bırakılmış varlıklar Kullanıcı tarafından yeniden etkinleştirilemez, ancak askıya alınma nedeni sağlandığında geri yüklenir.

## <a name="queue-status"></a>Sıra durumu 
Bir kuyruk için ayarlanabilir durumlar şunlardır:

-   **Etkin**: kuyruk etkin.
-   **Devre dışı**: sıra askıya alındı. Hem **Senddisabled** hem de **receivedisabled**ayarlamaya eşdeğerdir. 
-   **Senddisabled**: sıra, almaya izin verildiğinde kısmen askıya alınır.
-   **Receivedisabled**: sıra, gönderilmeye izin verilen kısmen askıya alındı.

### <a name="change-the-queue-status-in-the-azure-portal"></a>Azure portal sıra durumunu değiştirin: 

1. Azure portal, Service Bus ad alanına gidin. 
1. Durumunu değiştirmek istediğiniz kuyruğu seçin. Kuyrukları ortadaki alt bölmede görürsünüz. 
1. **Service Bus kuyruğu** sayfasında, sıranın geçerli durumuna bir köprü olarak bakın. Sol menüde **genel bakış** seçilmezse, sıranın durumunu görmek için seçin. Değişiklik için kuyruğun geçerli durumunu seçin. 

    :::image type="content" source="./media/entity-suspend/select-state.png" alt-text="Kuyruğun durumunu seçin":::
4. Kuyruğun yeni durumunu seçin ve **Tamam**' ı seçin. 

    :::image type="content" source="./media/entity-suspend/entity-state-change.png" alt-text="Kuyruğun durumunu seçin":::
    
Portal yalnızca sıraların tamamen devre dışı bırakılmasını sağlar. Ayrıca, .NET Framework SDK 'sında Service Bus [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API 'lerini kullanarak veya Azure clı veya Azure PowerShell aracılığıyla Azure Resource Manager şablonuyla, gönderme ve alma işlemlerini ayrı olarak devre dışı bırakabilirsiniz.

### <a name="change-the-queue-status-using-azure-powershell"></a>Azure PowerShell kullanarak sıra durumunu değiştirme
Bir sırayı devre dışı bırakmak için PowerShell komutu aşağıdaki örnekte gösterilmiştir. Yeniden etkinleştirme komutu eşdeğerdir, `Status` **etkin**olarak ayarlanıyor.

```powershell
$q = Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="topic-status"></a>Konu durumu
Azure portal konu durumunun değiştirilmesi, sıranın değiştirme durumuna benzerdir. Konunun geçerli durumunu seçtiğinizde, durumu değiştirmenize olanak sağlayan aşağıdaki sayfayı görürsünüz. 

:::image type="content" source="./media/entity-suspend/topic-state-change.png" alt-text="Kuyruğun durumunu seçin":::

Bir konu için ayarlanabilir durumlar şunlardır:
- **Etkin**: konu etkin.
- **Devre dışı**: konu askıya alındı.
- **Senddisabled**: **devre dışı**olarak aynı etkiye sahiptir.

## <a name="subscription-status"></a>Abonelik durumu
Azure portal abonelik durumunun değiştirilmesi, bir konunun veya kuyruğun değiştirme durumuna benzerdir. Aboneliğin geçerli durumunu seçtiğinizde, durumu değiştirmenize olanak sağlayan aşağıdaki sayfayı görürsünüz. 

:::image type="content" source="./media/entity-suspend/subscription-state-change.png" alt-text="Kuyruğun durumunu seçin":::

Bir konu için ayarlanabilir durumlar şunlardır:
- **Etkin**: konu etkin.
- **Devre dışı**: konu askıya alındı.
- **Receivedisabled**: **devre dışı**olarak aynı efekt.

## <a name="other-statuses"></a>Diğer durumlar
[EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) numaralandırması Ayrıca yalnızca sistem tarafından ayarlanabileceğini belirten bir geçiş durumları kümesi tanımlar. 


## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/entity-state-change.png


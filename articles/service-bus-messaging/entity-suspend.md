---
title: Azure Service Bus-mesajlaşma varlıklarını askıya alma
description: Bu makalede Azure Service Bus ileti varlıklarının (kuyruklar, konular ve abonelikler) geçici olarak askıya alınması ve yeniden etkinleştirilmesi açıklanmaktadır.
ms.topic: article
ms.date: 09/29/2020
ms.openlocfilehash: ea1acab3d0a86b0064f8b3eef7bfd1496bd17041
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94543060"
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Mesajlaşma varlıklarını askıya alma ve yeniden etkinleştirme (devre dışı bırakma)

Kuyruklar, konular ve abonelikler geçici olarak askıya alınabilir. Askıya alma, varlığı tüm iletilerin depolamada bulunduğu devre dışı durumuna geçirir. Ancak, iletiler kaldırılamaz veya eklenemez ve ilgili protokol işlemleri hatalara sebep olur.

Acil Yönetim nedenleriyle bir varlığı askıya almak isteyebilirsiniz. Örneğin, hatalı alıcı iletileri kuyruktan çıkarır, işleme başarısız olur ve iletileri yanlış bir şekilde tamamlar ve onları kaldırır. Bu durumda, kodu düzeltip ve dağıtana kadar kuyruğu alma için devre dışı bırakmak isteyebilirsiniz. 

Askıya alma veya yeniden etkinleştirme, Kullanıcı ya da sistem tarafından gerçekleştirilebilir. Yalnızca abonelik harcama sınırına vurmaya yönelik aksan, yönetim nedenlerinden dolayı sistem varlıkları askıya alır. Sistem devre dışı bırakılmış varlıklar Kullanıcı tarafından yeniden etkinleştirilemez, ancak askıya alınma nedeni sağlandığında geri yüklenir.

## <a name="queue-status"></a>Sıra durumu 
Bir **kuyruk** için ayarlanabilir durumlar şunlardır:

-   **Etkin**: kuyruk etkin. Kuyruğa ileti gönderebilir ve kuyruktan ileti alabilirsiniz. 
-   **Devre dışı**: sıra askıya alındı. Hem **Senddisabled** hem de **receivedisabled** ayarlamaya eşdeğerdir. 
-   **Senddisabled**: kuyruğa ileti gönderemezsiniz, ancak bundan ileti alabilirsiniz. Kuyruğa ileti göndermeye çalışırsanız bir özel durum alırsınız. 
-   **Receivedisabled**: kuyruğa ileti gönderebilirsiniz, ancak bundan ileti alamazsınız. Kuyruğa ileti almaya çalışırsanız bir özel durum alırsınız.


### <a name="change-the-queue-status-in-the-azure-portal"></a>Azure portal sıra durumunu değiştirin: 

1. Azure portal, Service Bus ad alanına gidin. 
1. Durumunu değiştirmek istediğiniz kuyruğu seçin. Kuyrukları ortadaki alt bölmede görürsünüz. 
1. **Service Bus kuyruğu** sayfasında, sıranın geçerli durumuna bir köprü olarak bakın. Sol menüde **genel bakış** seçilmezse, sıranın durumunu görmek için seçin. Değişiklik için kuyruğun geçerli durumunu seçin. 

    :::image type="content" source="./media/entity-suspend/select-state.png" alt-text="Kuyruğun durumunu seçin":::
4. Kuyruğun yeni durumunu seçin ve **Tamam**' ı seçin. 

    :::image type="content" source="./media/entity-suspend/entity-state-change.png" alt-text="Kuyruğun durumunu ayarla":::
    
Ayrıca, .NET SDK 'sında Service Bus [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) API 'lerini kullanarak gönderme ve alma işlemlerini devre dışı bırakabilir veya Azure clı veya Azure PowerShell aracılığıyla bir Azure Resource Manager şablonu kullanabilirsiniz.

### <a name="change-the-queue-status-using-azure-powershell"></a>Azure PowerShell kullanarak sıra durumunu değiştirme
Bir sırayı devre dışı bırakmak için PowerShell komutu aşağıdaki örnekte gösterilmiştir. Yeniden etkinleştirme komutu eşdeğerdir, `Status` **etkin** olarak ayarlanıyor.

```powershell
$q = Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="topic-status"></a>Konu durumu
Azure portal konu durumu ' nu değiştirebilirsiniz. Durumu değiştirmenize olanak sağlayan aşağıdaki sayfayı görmek için konunun geçerli durumunu seçin. 

:::image type="content" source="./media/entity-suspend/topic-state-change.png" alt-text="Konu durumunu değiştir":::

Bir **Konu** için ayarlanabilir durumlar şunlardır:
- **Etkin**: konu etkin. Konuya ileti gönderebilirsiniz. 
- **Devre dışı**: konu askıya alındı. Konuya ileti gönderemezsiniz. 
- **Senddisabled**: **devre dışı** olarak aynı etkiye sahiptir. Konuya ileti gönderemezsiniz. Konuya ileti göndermeye çalışırsanız bir özel durum alırsınız. 

## <a name="subscription-status"></a>Abonelik durumu
Azure portal abonelik durumunu değiştirebilirsiniz. Durumu değiştirmenize olanak sağlayan aşağıdaki sayfayı görmek için aboneliğin geçerli durumunu seçin. 

:::image type="content" source="./media/entity-suspend/subscription-state-change.png" alt-text="Abonelik durumunu değiştir":::

Bir **abonelik** için ayarlanabilir durumlar şunlardır:
- **Etkin**: Abonelik etkin. Aboneliği frm olarak alabilirsiniz.
- **Devre dışı**: abonelik askıya alındı. Abonelikten ileti alamazsınız. 
- **Receivedisabled**: **devre dışı** olarak aynı efekt. Abonelikten ileti alamazsınız. Abonelik için ileti almaya çalışırsanız bir özel durum alırsınız.

| Konu durumu | Abonelik durumu | Davranış | 
| ------------ | ------------------- | -------- | 
| Etkin | Etkin | Konuya ileti gönderebilir ve abonelikten ileti alabilirsiniz. | 
| Etkin | Devre dışı veya alma devre dışı | Konuya ileti gönderebilirsiniz, ancak abonelikten ileti alamazsınız | 
| Devre dışı veya Gönder devre dışı | Etkin | Konuya ileti gönderemezsiniz, ancak zaten abonelikte olan iletileri alabilirsiniz. | 
| Devre dışı veya Gönder devre dışı | Devre dışı veya alma devre dışı | Konuya ileti gönderemezsiniz ve abonelikten birini alamazsınız. | 

## <a name="other-statuses"></a>Diğer durumlar
[EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) numaralandırması Ayrıca yalnızca sistem tarafından ayarlanabileceğini belirten bir geçiş durumları kümesi tanımlar. 


## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/entity-state-change.png


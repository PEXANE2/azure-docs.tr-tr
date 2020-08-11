---
title: Tanılama günlüklerini Azure Service Bus | Microsoft Docs
description: Bu makalede, Azure Service Bus için kullanılabilen tüm işletimsel ve tanılama günlüklerine bir genel bakış sunulmaktadır.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: f227f5a988ccd51425b4f43e87b4ed4d9af74e1f
ms.sourcegitcommit: d8b8768d62672e9c287a04f2578383d0eb857950
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88064443"
---
# <a name="enable-diagnostics-logs-for-service-bus"></a>Service Bus için tanılama günlüklerini etkinleştirme

Azure Service Bus ad alanınızı kullanmaya başladığınızda, ad alanınızı nasıl ve ne zaman oluşturduğunu, silineceğini veya erişildiğini izlemek isteyebilirsiniz. Bu makale, kullanılabilir olan tüm işletimsel ve tanılama günlüklerine genel bir bakış sağlar.

Azure Service Bus Şu anda Azure Service Bus ad alanında gerçekleştirilen *yönetim işlemlerini* yakalayan etkinlik ve işletimsel günlükleri desteklemektedir. Özellikle, bu Günlükler kuyruk oluşturma, kullanılan kaynaklar ve işlemin durumu dahil olmak üzere işlem türünü yakalar.

## <a name="operational-logs-schema"></a>İşletimsel Günlükler şeması

Tüm Günlükler aşağıdaki iki konumda JavaScript Nesne Gösterimi (JSON) biçiminde depolanır:

- **AzureActivity**: Azure portal veya Azure Resource Manager şablon dağıtımları aracılığıyla ad alanınız üzerinde yürütülen işlemlerden ve eylemlerden günlükleri görüntüler.
- **AzureDiagnostics**: API kullanarak veya dil SDK 'sindeki yönetim istemcileri aracılığıyla, ad alanınız üzerinde yürütülen işlemlerden ve eylemlerden günlükleri görüntüler.

İşletimsel günlük JSON dizeleri aşağıdaki tabloda listelenen öğeleri içerir:

| Ad | Açıklama |
| ------- | ------- |
| Etkinlik kimliği | Belirtilen etkinliği tanımlamak için kullanılan iç KIMLIK |
| EventName | İşlem adı |
| ResourceId | Azure Resource Manager kaynak KIMLIĞI |
| kaynak grubundaki | Abonelik Kimliği |
| EventTimeString | İşlem süresi |
| EventProperties | İşlem özellikleri |
| Durum | İşlem durumu |
| Çağıran | İşlem çağıranı (Azure portal veya yönetim istemcisi) |
| Kategori | OperationalLogs |

İşte işlem günlüğü JSON dizesine bir örnek:

```json
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="events-and-operations-captured-in-operational-logs"></a>İşlemsel günlüklerde yakalanan olaylar ve işlemler

İşletimsel Günlükler Azure Service Bus ad alanında gerçekleştirilen tüm yönetim işlemlerini yakalar. Azure Service Bus üzerinde gerçekleştirilen yüksek miktarda veri işlemi nedeniyle veri işlemleri yakalanmaz.

> [!NOTE]
> Veri işlemlerini daha iyi izlemenize yardımcı olması için istemci tarafı izlemenin kullanılması önerilir.

Aşağıdaki yönetim işlemleri işlemsel günlüklerde yakalanır: 

| Kapsam | İşlem|
|-------| -------- |
| Ad Alanı | <ul> <li> Ad alanı oluştur</li> <li> Ad alanını güncelleştir </li> <li> Ad alanını sil </li> <li> Ad alanı SharedAccess Ilkesini güncelleştirme </li> </ul> | 
| Kuyruk | <ul> <li> Sıra oluştur</li> <li> Kuyruğu Güncelleştir</li> <li> Kuyruğu Sil </li> <li> Silme kuyruğunu oto Sil </li> </ul> | 
| Konu | <ul> <li> Konu Oluştur </li> <li> Konuyu Güncelleştir </li> <li> Konuyu sil </li> <li> Sil konusunu oto Sil </li> </ul> |
| Abonelik | <ul> <li> Abonelik Oluşturma </li> <li> Aboneliği Güncelleştir </li> <li> Aboneliği Sil </li> <li> Aboneliği silmeyi oto Sil </li> </ul> |

> [!NOTE]
> Şu anda, *okuma* işlemleri işlemsel günlüklerde izlenmez.

## <a name="enable-operational-logs"></a>İşletimsel günlükleri etkinleştir

İşletimsel Günlükler varsayılan olarak devre dışıdır. Tanılama günlüklerini etkinleştirmek için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com), Azure Service Bus ad alanına gidin ve ardından **izleme**altında **Tanılama ayarları**' nı seçin.

   !["Tanılama ayarları" bağlantısı](./media/service-bus-diagnostic-logs/image1.png)

1. **Tanılama ayarları** bölmesinde, **Tanılama ayarı Ekle**' yi seçin.  

   !["Tanılama ayarı Ekle" bağlantısı](./media/service-bus-diagnostic-logs/image2.png)

1. Tanılama ayarlarını aşağıdakileri yaparak yapılandırın:

   a. **Ad** kutusuna tanılama ayarları için bir ad girin.  

   b. Tanılama günlüklerinizi için aşağıdaki üç hedef arasından birini seçin:  
   - **Bir depolama hesabına arşiv**' i seçerseniz, tanılama günlüklerinin depolanacağı depolama hesabını yapılandırmanız gerekir.  
   - **Bir olay hub 'ına akış**' ı seçerseniz, tanılama günlüklerini akışa almak istediğiniz olay hub 'ını yapılandırmanız gerekir.
   - **Log Analytics gönder**' i seçerseniz, tanılamayı hangi Log Analytics hangi örneğine gönderileceğini belirtmeniz gerekir.  

   c. **Operationallogs** onay kutusunu seçin.

    !["Tanılama ayarları" bölmesi](./media/service-bus-diagnostic-logs/image3.png)

1. **Kaydet**’i seçin.

Yeni ayarlar yaklaşık 10 dakika içinde etkili olur. Günlükler, **tanılama günlükleri** bölmesindeki yapılandırılan arşiv hedefinde görüntülenir.

Tanılama ayarlarını yapılandırma hakkında daha fazla bilgi için bkz. [Azure tanılama günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

Service Bus hakkında daha fazla bilgi edinmek için bkz.:

* [Service Bus giriş](service-bus-messaging-overview.md)
* [Service Bus’ı kullanmaya başlama](service-bus-dotnet-get-started-with-queues.md)
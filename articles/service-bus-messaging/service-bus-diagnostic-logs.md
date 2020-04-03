---
title: Azure Hizmet Veri Servisi tanılama günlükleri | Microsoft Dokümanlar
description: Bu makalede, Azure Hizmet Veri Servisi için kullanılabilen tüm işletim ve tanılama günlüklerine genel bir bakış sunulmaktadır.
keywords: ''
documentationcenter: .net
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: a80fb97810fee04a4eb50c43178c168e66f29173
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80618721"
---
# <a name="enable-diagnostics-logs-for-service-bus"></a>Servis Veri Servisi için tanılama günlüklerini etkinleştirme

Azure Hizmet Veri Gönderi ad alanınızı kullanmaya başladığınızda, ad alanınızın nasıl ve ne zaman oluşturulduğunu, silinip silinip erişildiğini izlemek isteyebilirsiniz. Bu makalede, kullanılabilir tüm işletim ve tanılama günlükleri genel bir bakış sağlar.

Azure Hizmet Veri Servisi şu anda Azure Hizmet Veri Yolundas ad alanında gerçekleştirilen *yönetim işlemlerini* yakalayan etkinlik ve işlem günlüklerini destekler. Özellikle, bu günlükler sıra oluşturma, kullanılan kaynaklar ve işlemin durumu da dahil olmak üzere işlem türünü yakalar.

## <a name="operational-logs-schema"></a>Operasyonel günlükler şema

Tüm günlükler aşağıdaki iki konumda JavaScript Nesne Gösterimi (JSON) biçiminde depolanır:

- **AzureEtkinliği**: Azure portalında veya Azure Kaynak Yöneticisi şablon dağıtımlarında ad alanınıza karşı gerçekleştirilen işlemlerin ve eylemlerin günlüklerini görüntüler.
- **AzureDiagnostics**: API'yi kullanarak veya SDK dilindeki yönetim istemcileri aracılığıyla ad alanınıza karşı gerçekleştirilen işlemlerin ve eylemlerin günlüklerini görüntüler.

Operasyonel günlük JSON dizeleri aşağıdaki tabloda listelenen öğeleri içerir:

| Adı | Açıklama |
| ------- | ------- |
| Activityıd | Belirtilen etkinliği tanımlamak için kullanılan dahili kimlik |
| EventName | İşlem adı |
| ResourceId | Azure Kaynak Yöneticisi kaynak kimliği |
| SubscriptionId | Abonelik Kimliği |
| EventTimeString | Çalışma süresi |
| Olay Özellikleri | Çalışma özellikleri |
| Durum | İşlem durumu |
| Çağıran | İşleyiş arayan (Azure portalı veya yönetim istemcisi) |
| Kategori | OperationalLogs |

Burada bir operasyonel günlük JSON dize bir örnek:

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

## <a name="events-and-operations-captured-in-operational-logs"></a>Operasyonel günlüklerde yakalanan olaylar ve işlemler

İşletim günlükleri, Azure Hizmet Veri Yolundan ad alanında gerçekleştirilen tüm yönetim işlemlerini yakalar. Azure Hizmet Veri Veri Servisi'nde gerçekleştirilen yüksek veri işlemleri nedeniyle veri işlemleri yakalanmaz.

> [!NOTE]
> Veri işlemlerini daha iyi izlemenize yardımcı olmak için istemci tarafı izlemenizi kullanmanızı öneririz.

Aşağıdaki yönetim işlemleri operasyonel günlüklerde yakalanır: 

| Kapsam | İşlem|
|-------| -------- |
| Ad Alanı | <ul> <li> Ad Alanı Oluşturma</li> <li> Ad Alanını Güncelleştir </li> <li> Ad Alanını Sil </li> <li> Ad Alanı Paylaşılan Erişim İlkesi'ni Güncelleştir </li> </ul> | 
| Kuyruk | <ul> <li> Sıra Oluştur</li> <li> Sırayı Güncelleştir</li> <li> Sırayı Sil </li> <li> OtomatikSil Sil Sırası </li> </ul> | 
| Konu başlığı | <ul> <li> Konu Oluştur </li> <li> Konuyu Güncelle </li> <li> Konuyu Sil </li> <li> OtomatikDelete Konuyu Sil </li> </ul> |
| Abonelik | <ul> <li> Abonelik Oluşturma </li> <li> Aboneliği Güncelle </li> <li> Aboneliği Sil </li> <li> OtomatikDelete Aboneliği Sil </li> </ul> |

> [!NOTE]
> Şu anda, *Read* işlemleri işlem günlüklerinde izlenmemektedir.

## <a name="enable-operational-logs"></a>Çalışma günlüklerini etkinleştirme

Çalışma günlükleri varsayılan olarak devre dışı bırakılır. Tanılama günlüklerini etkinleştirmek için aşağıdakileri yapın:

1. Azure [portalında,](https://portal.azure.com)Azure Hizmet Veri Servisi ad alanınıza gidin ve ardından **İzleme**altında **Tanılama ayarlarını**seçin.

   !["Tanılama ayarları" bağlantısı](./media/service-bus-diagnostic-logs/image1.png)

1. **Tanılama ayarları** bölmesinde **tanılama ayarını ekle'yi**seçin.  

   !["Tanılama ayarı ekle" bağlantısı](./media/service-bus-diagnostic-logs/image2.png)

1. Tanılama ayarlarını aşağıdakileri yaparak yapılandırın:

   a. **Ad** kutusuna tanılama ayarları için bir ad girin.  

   b. Tanılama günlükleriniz için aşağıdaki üç hedeften birini seçin:  
   - **Bir depolama hesabına Arşiv'i**seçerseniz, tanılama günlüklerinin depolanacağı depolama hesabını yapılandırmanız gerekir.  
   - **Bir olay hub'ına Akış'ı**seçerseniz, tanılama günlüklerini akışla aktarmak istediğiniz olay merkezini yapılandırmanız gerekir.
   - **Günlük Analitiğine Gönder'i**seçerseniz, tanılamanın hangi günlük analizine gönderileceği belirtilmelidir.  

   c. **OperationalLogs** onay kutusunu seçin.

    !["Tanılama ayarları" bölmesi](./media/service-bus-diagnostic-logs/image3.png)

1. **Kaydet'i**seçin.

Yeni ayarlar yaklaşık 10 dakika içinde etkili olur. Günlükler, yapılandırılan arşiv hedefinde, **Tanılama günlükleri** bölmesinde görüntülenir.

Tanılama ayarlarını yapılandırma hakkında daha fazla bilgi için [Azure tanılama günlüklerine genel bakış](../azure-monitor/platform/diagnostic-logs-overview.md)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Servis Otobüsü hakkında daha fazla bilgi edinmek için bkz:

* [Servis Otobüsüne Giriş](service-bus-messaging-overview.md)
* [Service Bus’ı kullanmaya başlama](service-bus-dotnet-get-started-with-queues.md)

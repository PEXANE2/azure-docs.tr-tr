---
title: Azure Notification Hubs tanılama günlükleri | Microsoft Docs
description: Bu makalede, Azure Notification Hubs için kullanılabilen tüm işletimsel ve tanılama günlüklerine genel bir bakış sunulmaktadır.
author: brannon
ms.author: brjones
ms.service: notification-hubs
ms.topic: article
ms.date: 01/29/2021
ms.openlocfilehash: b98a04a70062461cec603bea83052c4f1224819e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101736245"
---
# <a name="enable-diagnostics-logs-for-notification-hubs"></a>Notification Hubs için tanılama günlüklerini etkinleştirme

Azure Notification Hubs ad alanınızı kullanmaya başladığınızda, ad alanınızı nasıl ve ne zaman oluşturduğunu, silineceğini veya erişildiğini izlemek isteyebilirsiniz. Bu makale, kullanılabilir olan tüm işletimsel ve tanılama günlüklerine genel bir bakış sağlar.

Azure Notification Hubs Şu anda Azure Notification Hubs ad alanında gerçekleştirilen *yönetim işlemlerini* yakalayan etkinlik ve işletimsel günlükleri desteklemektedir.

## <a name="diagnostic-logs-schema"></a>Tanılama günlükleri şeması

Tüm Günlükler aşağıdaki iki konumda JavaScript Nesne Gösterimi (JSON) biçiminde depolanır:

- **AzureActivity**: Azure portal veya Azure Resource Manager şablon dağıtımları aracılığıyla ad alanınız üzerinde yürütülen işlemlerden ve eylemlerden günlükleri görüntüler.
- **AzureDiagnostics**: API kullanarak veya dil SDK 'sindeki yönetim istemcileri aracılığıyla, ad alanınız üzerinde yürütülen işlemlerden ve eylemlerden günlükleri görüntüler.

Tanılama günlüğü JSON dizeleri aşağıdaki tabloda listelenen öğeleri içerir:

| Ad | Açıklama |
| ------- | ------- |
| time | Günlüğün UTC zaman damgası |
| resourceId | Azure kaynağının göreli yolu |
| operationName | Yönetim işleminin adı |
| category | Günlük kategorisi. Geçerli değerler: `OperationalLogs` |
| Callerıdentity | Yönetim işlemini başlatan çağıranın kimliği |
| resultType | Yönetim işleminin durumu. Geçerli değerler: `Succeeded` veya `Failed` |
| resultDescription | Yönetim işleminin açıklaması |
| correlationId | Yönetim işleminin bağıntı KIMLIĞI (belirtilmişse) |
| callerIpAddress | Çağıran IP adresi. Azure portal kaynaklı çağrılar için boş |

İşte işlem günlüğü JSON dizesine bir örnek:

```json
{
    "operationName": "Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action",
    "resourceId": "/SUBSCRIPTIONS/2CAC2A14-BA6B-46A6-BCE8-2D9781A41BA2/RESOURCEGROUPS/SAMPLES/PROVIDERS/MICROSOFT.NOTIFICATIONHUBS/NAMESPACES/SAMPLE-NS",
    "time": "1/1/2021 5:16:32 AM +00:00",
    "category": "OperationalLogs",
    "resultType": "Succeeded",
    "resultDescription": "Gets Hub Authorization Rules",
    "correlationId": "00000000-0000-0000-0000-000000000000",
    "callerIdentity": "{ \"identityType\": \"Portal\", \"identity\": \"\" }"
}
```

`callerIdentity`Alan boş veya aşağıdaki biçimlerden birine sahip BIR JSON dizesi olabilir.

Azure portal kaynaklı çağrılar için `identity` alan boştur. Günlüğe kaydedilen kullanıcının belirlenmesi için günlük, etkinlik günlükleriyle bağıntılı olabilir.
```json
{
    "identityType": "Portal",
    "identity": ""
}
```

Azure Resource Manager aracılığıyla yapılan çağrılar için `identity` alan, oturum açmış kullanıcının Kullanıcı adını içerir.
```json
{
   "identityType": "Username",
   "identity": "test@foo.com"
}
```

Notification Hubs çağrıları için REST API `identity` alan, SharedAccessSignature belirtecini oluşturmak için kullanılan erişim ilkesinin adını içerir.
```json
{
   "identityType": "KeyName",
   "identity": "SharedAccessRootKey2"
}
```

## <a name="events-and-operations-captured-in-operational-logs"></a>İşlemsel günlüklerde yakalanan olaylar ve işlemler

İşletimsel Günlükler Azure Notification Hubs ad alanında gerçekleştirilen tüm yönetim işlemlerini yakalar. Azure Notification Hubs üzerinde gerçekleştirilen yüksek miktarda veri işlemi nedeniyle veri işlemleri yakalanmaz.

Aşağıdaki yönetim işlemleri işlemsel günlüklerde yakalanır: 

| Kapsam | İşlem adı | İşlem açıklaması |
| :-- | :-- | :-- |
| Ad Alanı | Microsoft. Notificationhub 'Lar/ad alanları/authorizationRules/eylem | Yetkilendirme kurallarını listeleme |
| Ad Alanı | Microsoft. Notificationhub 'Lar/ad alanları/authorizationRules/silme | Yetkilendirme kuralını Sil |
| Ad Alanı | Microsoft. Notificationhub 'Lar/ad alanları/authorizationRules/ListKeys/Action | Anahtarları Listele |
| Ad Alanı | Microsoft. Notificationhub 'Lar/ad alanları/authorizationRules/okuma | Yetkilendirme kuralı al |
| Ad Alanı | Microsoft. Notificationhub 'Lar/ad alanları/authorizationRules/regenerateKeys/ACTION | Anahtarları yeniden oluştur |
| Ad Alanı | Microsoft. Notificationhub 'Lar/ad alanları/authorizationRules/yazma | Yetkilendirme kuralı oluştur veya güncelleştir |
| Ad Alanı | Microsoft. Notificationhub 'Lar/ad alanları/silme | Ad alanını sil |
| Ad Alanı | Microsoft. Notificationhub 'Lar/ad alanları/okuma | Ad alanı al |
| Ad Alanı | Microsoft. Notificationhub 'Lar/ad alanları/yazma | Ad alanı oluştur veya güncelleştir |
| Bildirim Hub'ı | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/authorizationRules/Action | Yetkilendirme kurallarını listeleme |
| Bildirim Hub'ı | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/authorizationRules/Delete | Yetkilendirme kuralını Sil |
| Bildirim Hub'ı | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/authorizationRules/ListKeys/Action | Anahtarları Listele |
| Bildirim Hub'ı | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/authorizationRules/Read | Yetkilendirme kuralını oku |
| Bildirim Hub'ı | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/authorizationRules/regenerateKeys/ACTION | Anahtarları yeniden oluştur |
| Bildirim Hub'ı | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/authorizationRules/Write | Yetkilendirme kuralı oluştur veya güncelleştir |
| Bildirim Hub'ı | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub 'Lar/silme | Bildirim Hub 'ını Sil |
| Bildirim Hub'ı | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/pnsCredentials/Action | PNS kimlik bilgilerini oluşturun, güncelleştirin veya alın |
| Bildirim Hub'ı | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/okuma | Bildirim Hub 'ı al |
| Bildirim Hub'ı | Microsoft. Notificationhub 'Lar/ad alanları/Notificationhub/yazma | Bildirim Hub 'ı oluştur veya güncelleştir |

## <a name="enable-operational-logs"></a>İşletimsel günlükleri etkinleştir

İşletimsel Günlükler varsayılan olarak devre dışıdır. Günlükleri etkinleştirmek için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com)Azure Notification Hubs ad alanına gidin ve ardından **izleme** altında **Tanılama ayarları**' nı seçin.

   !["Tanılama ayarları" bağlantısı](./media/notification-hubs-diagnostic-logs/image-1.png)

1. **Tanılama ayarları** bölmesinde, **Tanılama ayarı Ekle**' yi seçin.  

   !["Tanılama ayarı Ekle" bağlantısı](./media/notification-hubs-diagnostic-logs/image-2.png)

1. Tanılama ayarlarını aşağıdakileri yaparak yapılandırın:

   a. **Ad** kutusuna tanılama ayarları için bir ad girin.  

   b. Tanılama günlüklerinizi için aşağıdaki üç hedef arasından birini seçin:  
   - **Log Analytics gönder çalışma alanına gönder**' i seçerseniz, tanılamayı hangi Log Analytics hangi örneğine gönderileceğini belirtmeniz gerekir.  
   - **Bir depolama hesabına arşiv**' i seçerseniz, tanılama günlüklerinin depolanacağı depolama hesabını yapılandırmanız gerekir.  
   - **Bir olay hub 'ına akış**' ı seçerseniz, tanılama günlüklerini akışa almak istediğiniz olay hub 'ını yapılandırmanız gerekir.

   c. **Operationallogs** onay kutusunu seçin.

    !["Tanılama ayarları" bölmesi](./media/notification-hubs-diagnostic-logs/image-3.png)

1. **Kaydet**’i seçin.

Yeni ayarlar yaklaşık 10 dakika içinde etkili olur. Günlükler, **tanılama günlükleri** bölmesindeki yapılandırılan arşiv hedefinde görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Tanılama ayarlarını yapılandırma hakkında daha fazla bilgi edinmek için bkz.:
* [Azure tanılama günlüklerine genel bakış](../azure-monitor/essentials/platform-logs-overview.md).

Azure Notification Hubs hakkında daha fazla bilgi edinmek için bkz.:
* [Azure Notification Hubs nedir?](notification-hubs-push-notification-overview.md)
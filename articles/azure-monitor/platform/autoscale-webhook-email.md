---
title: E-posta ve webhook uyarı bildirimleri göndermek için otomatik ölçeklendirmeyi kullanın
description: Azure Monitor'da web URL'lerini aramak veya e-posta bildirimleri göndermek için otomatik ölçeklendirme eylemlerini nasıl kullanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 04/03/2017
ms.subservice: autoscale
ms.openlocfilehash: c82b170bb3801bdc701ed84230db57f5691523ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77120695"
---
# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>Azure Monitor'da e-posta ve webhook uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemlerini kullanın
Bu makalede, Azure'daki otomatik ölçeklendirme eylemlerine dayalı olarak belirli web URL'lerini arayabilmeniz veya e-posta gönderebilmeniz için nasıl ayarlanan tetikleyiciler gösterilmektedir.  

## <a name="webhooks"></a>Web Kancaları
Webhooks, Azure uyarı bildirimlerini işleme sonrası veya özel bildirimler için diğer sistemlere yönlendirmenize olanak tanır. Örneğin, SMS göndermek, hataları günlüğe kaydetmek, sohbet veya mesajlaşma hizmetlerini kullanarak bir ekibe bildirin, vb. için gelen web isteğini işleyebilir hizmetlere uyarı yönlendirme Webhook URI geçerli bir HTTP veya HTTPS bitiş noktası olmalıdır.

## <a name="email"></a>Email
E-posta herhangi bir geçerli e-posta adresine gönderilebilir. Kuralın çalıştığı aboneliğin yöneticileri ve yardımcı yöneticileri de bildirilir.

## <a name="cloud-services-and-app-services"></a>Bulut Hizmetleri ve Uygulama Hizmetleri
Bulut Hizmetleri ve Sunucu Çiftlikleri (Uygulama Hizmetleri) için Azure portalından kabul edebilirsiniz.

* **Ölçeğe göre ölçeği** seçin.

![tarafından ölçeklendirin](./media/autoscale-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a>Sanal Makine ölçek kümeleri
Kaynak Yöneticisi (Virtual Machine ölçek kümeleri) ile oluşturulan yeni Sanal Makineler için, bunu REST API, Resource Manager şablonları, PowerShell ve CLI'yi kullanarak yapılandırabilirsiniz. Portal arabirimi henüz kullanılamıyor.
REST API veya Kaynak Yöneticisi şablonunu kullanırken, bildirim öğesini aşağıdaki seçeneklerle [otomatik ölçeklendirme ayarlarınızda](https://docs.microsoft.com/azure/templates/microsoft.insights/2015-04-01/autoscalesettings) ekleyin.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```

| Alan | Zorunlu? | Açıklama |
| --- | --- | --- |
| Işlem |evet |değeri "Ölçek" olmalıdır |
| sendToSubscriptionAdministrator |evet |değeri "doğru" veya "yanlış" olmalıdır |
| sendToSubscriptionCoAdministrators |evet |değeri "doğru" veya "yanlış" olmalıdır |
| özelE-postalar |evet |değeri null olabilir [] veya e-postaların dize dizisi |
| webhooks |evet |değeri null veya geçerli Uri olabilir |
| serviceUri |evet |geçerli bir https Uri |
| properties |evet |değeri boş {} olmalı veya anahtar değeri çiftleri içerebilir |

## <a name="authentication-in-webhooks"></a>Webhooks kimlik doğrulama
Webhook, webhook URI'yi sorgu parametresi olarak belirteç kimliğiyle kaydettiğiniz belirteç tabanlı kimlik doğrulaması kullanarak kimlik doğrulaması yapabilir. Örneğin, https:\//mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue

## <a name="autoscale-notification-webhook-payload-schema"></a>Otomatik ölçek bildirim webhook yük şeması
Otomatik ölçek bildirimi oluşturulduğunda, webhook yüküne aşağıdaki meta veriler dahil edilir:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


| Alan | Zorunlu? | Açıklama |
| --- | --- | --- |
| durum |evet |Otomatik ölçeklendirme eyleminin oluşturulduğunu gösteren durum |
| Işlem |evet |Örneklerin artması için "Ölçeklendirme" ve örneklerde azalma için "Ölçeklendir" olacaktır. |
| bağlam |evet |Otomatik ölçeklendirme eylem bağlamı |
| timestamp |evet |Otomatik ölçeklendirme eylemi tetiklendiğinde zaman damgası |
| id |Evet |Otomatik ölçek ayarının Kaynak Yöneticisi Kimliği |
| ad |Evet |Otomatik ölçek ayaradının adı |
| Şey |Evet |Otomatik ölçeklendirme hizmetinin aldığı eylemin açıklaması ve örnek sayısındaki değişiklik |
| subscriptionId |Evet |Ölçeklendirilmekte olan hedef kaynağın abonelik kimliği |
| resourceGroupName |Evet |Ölçeklendirilmekte olan hedef kaynağın Kaynak Grubu adı |
| resourceName |Evet |Ölçeklendirilmekte olan hedef kaynağın adı |
| resourceType |Evet |Desteklenen üç değer: "microsoft.classiccompute/domainnames/slots/roles" - Bulut Hizmeti rolleri, "microsoft.compute/virtualmachinescalesets" - Virtual Machine Scale Sets ve "Microsoft.Web/serverfarms" - Web App |
| resourceId |Evet |Ölçeklendirilmekte olan hedef kaynağın Kaynak Yöneticisi Kimliği |
| portalLink |Evet |Hedef kaynağın özet sayfasına Azure portalı bağlantısı |
| oldKapasite |Evet |Otomatik ölçek lendirme eylemi yaptığında geçerli (eski) örnek sayısı |
| newKapasite |Evet |Otomatik ölçeklendirmenin kaynağı ölçeklendirdiğini yeni örnek sayısı |
| properties |Hayır |İsteğe bağlı. Anahtar <Kümesi, Değer> çiftleri (örneğin, Sözlük <String, String>). Özellikler alanı isteğe bağlıdır. Özel bir kullanıcı arabirimi veya Logic uygulaması tabanlı iş akışında, yükü kullanarak geçilebilen anahtarları ve değerleri girebilirsiniz. Giden webhook aramasına özel özellikleri geçirmenin alternatif bir yolu webhook URI'nin kendisini kullanmaktır (sorgu parametreleri olarak) |


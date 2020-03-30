---
title: Azure hizmet bildirimlerinde etkinlik günlüğü uyarıları alma
description: Azure hizmeti gerçekleştiğinde SMS, e-posta veya webhook yoluyla bilgi alın.
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: d318adc76959ac24f4be9946167965a83053f632
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75749313"
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Hizmet bildirimleri için etkinlik günlüğü uyarısı oluşturma
## <a name="overview"></a>Genel Bakış

Bu makalede, Azure portalını kullanarak hizmet durumu bildirimleri için etkinlik günlüğü uyarılarınasıl ayarlayabileceğiniz gösterilmektedir.  

Hizmet durumu bildirimleri Azure [etkinlik günlüğünde](../azure-monitor/platform/platform-logs-overview.md) depolanır Etkinlik günlüğünde depolanmış büyük miktarda bilgi göz önüne alındığında, hizmet durumu bildirimlerinde uyarıları görüntülemeyi ve ayarlamayı kolaylaştırmak için ayrı bir kullanıcı arabirimi vardır. 

Azure, Azure aboneliğinize hizmet durumu bildirimleri gönderdiğinde bir uyarı alabilirsiniz. Uyarıyı aşağıdakilere göre yapılandırabilirsiniz:

- Hizmet sağlık bildirimi sınıfı (Servis sorunları, Planlı bakım, Sağlık danışma önerileri).
- Abonelik etkilendi.
- Hizmet(ler) etkilenir.
- Bölge(ler) etkilendi.

> [!NOTE]
> Hizmet durumu bildirimleri, kaynak durumu olaylarıyla ilgili bir uyarı göndermez.

Ayrıca, uyarının kime gönderilmesi gerektiğini de yapılandırabilirsiniz:

- Varolan bir eylem grubu seçin.
- Yeni bir eylem grubu oluşturun (gelecekteki uyarılar için kullanılabilir).

Eylem grupları hakkında daha fazla bilgi edinmek için bkz. [Eylem grupları oluşturma ve yönetme](../azure-monitor/platform/action-groups.md).

Azure Kaynak Yöneticisi şablonlarını kullanarak hizmet durumu bildirimi uyarılarını nasıl yapılandırılabilirsiniz hakkında bilgi için [Kaynak Yöneticisi şablonlarına](../azure-monitor/platform/alerts-activity-log.md)bakın.

### <a name="watch-a-video-on-setting-up-your-first-azure-service-health-alert"></a>İlk Azure Hizmet Durumu uyarınızı ayarlama yla ilgili bir video izleyin

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OaXt]

## <a name="alert-and-new-action-group-using-azure-portal"></a>Azure portalLarını kullanarak uyarı ve yeni eylem grubu
1. [Portalda,](https://portal.azure.com) **Hizmet Durumu'nu**seçin.

    !["Hizmet Sağlığı" hizmeti](media/alerts-activity-log-service-notifications/home-servicehealth.png)

1. **Uyarılar** bölümünde, Sistem **Durumu uyarılarını**seçin.

    !["Sistem durumu uyarıları" sekmesi](media/alerts-activity-log-service-notifications/alerts-blades-sh.png)

1. **Hizmet durumu uyarısı oluştur'u** seçin ve alanları doldurun.

    !["Hizmet durumu uyarısı oluşturma" komutu](media/alerts-activity-log-service-notifications/service-health-alert.png)

1. Uyarılmak üzere **abonelik,** **hizmetler**ve **bölgeler** seçin.

    !["Etkinlik günlüğü uyarısı ekle" iletişim kutusu](media/alerts-activity-log-service-notifications/activity-log-alert-new-ux.png)

    > [!NOTE]
    > Bu abonelik, etkinlik günlüğü uyarısını kaydetmek için kullanılır. Uyarı kaynağı bu aboneğe dağıtılır ve etkinlik günlüğündeki olayları izler.

1. Uyarılmak istediğiniz **Etkinlik türlerini** seçin: *Hizmet sorunu,* *Planlı bakım*ve Sağlık *danışma ları* 

1. **Bir Uyarı kuralı adı** ve Açıklaması girerek uyarı bilgilerinizi **tanımlayın.**

1. Uyarının kaydolmasını istediğiniz **Kaynak grubunu** seçin.

1. **Yeni eylem grubu**seçerek yeni bir eylem grubu oluşturun. **Eylem grubu adı** kutusuna bir ad girin ve Kısa ad kutusuna bir **ad** girin. Bu uyarı çıktığında gönderilen bildirimlerde kısa ad başvurulmaktadır.

    ![Yeni bir eylem grubu oluşturma](media/alerts-activity-log-service-notifications/action-group-creation.png)

1. Alıcının listesini sağlayarak bir alıcı listesi tanımlayın:

    a. **Adı**: Alıcının adını, takma adını veya tanımlayıcısını girin.

    b. **Eylem Türü**: SMS, e-posta, webhook, Azure uygulaması ve daha fazlasını seçin.

    c. **Ayrıntılar**: Seçilen eylem türüne göre, bir telefon numarası, e-posta adresi, webhook URI, vb girin.

1. Eylem grubunu oluşturmak için **Tamam'ı** seçin ve ardından uyarınızı tamamlamak için **uyarı kuralı oluşturun.**

Birkaç dakika içinde uyarı etkin olur ve oluşturma sırasında belirttiğiniz koşullara bağlı olarak tetiklemeye başlar.

[Mevcut sorun yönetim sistemleri için webhook bildirimlerini nasıl yapılandırılatırmayı](service-health-alert-webhook-guide.md)öğrenin. Etkinlik günlüğü uyarıları için webhook şeması hakkında bilgi için [Azure etkinlik günlüğü uyarıları için Webhooks'a](../azure-monitor/platform/activity-log-alerts-webhook.md)bakın.

>[!NOTE]
>Bu adımlarda tanımlanan eylem grubu, gelecekteki tüm uyarı tanımları için varolan bir eylem grubu olarak yeniden kullanılabilir.
>

## <a name="alert-with-existing-action-group-using-azure-portal"></a>Azure portalını kullanarak varolan eylem grubuyla uyarı

1. Hizmet durumu bildiriminizi oluşturmak için önceki bölümdeki 1'den 6'ya kadar olan adımları izleyin. 

1. **Eylem grubunu tanımla altında,** **eylem grubunu seç** düğmesini tıklatın. Uygun eylem grubunu seçin.

1. Eylem grubunu eklemek için **Ekle'yi** ve ardından uyarınızı tamamlamak için **uyarı kuralı nı oluşturun'u** seçin.

Birkaç dakika içinde uyarı etkin olur ve oluşturma sırasında belirttiğiniz koşullara bağlı olarak tetiklemeye başlar.

## <a name="alert-and-new-action-group-using-the-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarını kullanarak uyarı ve yeni eylem grubu

Aşağıda, e-posta hedefi olan bir eylem grubu oluşturan ve hedef abonelik için tüm hizmet durumu bildirimlerini etkinleştiren bir örnek verilmiştir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "actionGroups_name": {
            "defaultValue": "SubHealth",
            "type": "String"
        },
        "activityLogAlerts_name": {
            "defaultValue": "ServiceHealthActivityLogAlert",
            "type": "String"
        },
        "emailAddress":{
            "type":"string"
        }
    },
    "variables": {
        "alertScope":"[concat('/','subscriptions','/',subscription().subscriptionId)]"
    },
    "resources": [
        {
            "comments": "Action Group",
            "type": "microsoft.insights/actionGroups",
            "name": "[parameters('actionGroups_name')]",
            "apiVersion": "2017-04-01",
            "location": "Global",
            "tags": {},
            "scale": null,
            "properties": {
                "groupShortName": "[parameters('actionGroups_name')]",
                "enabled": true,
                "emailReceivers": [
                    {
                        "name": "[parameters('actionGroups_name')]",
                        "emailAddress": "[parameters('emailAddress')]"
                    }
                ],
                "smsReceivers": [],
                "webhookReceivers": []
            },
            "dependsOn": []
        },
        {
            "comments": "Service Health Activity Log Alert",
            "type": "microsoft.insights/activityLogAlerts",
            "name": "[parameters('activityLogAlerts_name')]",
            "apiVersion": "2017-04-01",
            "location": "Global",
            "tags": {},
            "scale": null,
            "properties": {
                "scopes": [
                    "[variables('alertScope')]"
                ],
                "condition": {
                    "allOf": [
                        {
                            "field": "category",
                            "equals": "ServiceHealth"
                        },
                        {
                            "field": "properties.incidentType",
                            "equals": "Incident"
                        }
                    ]
                },
                "actions": {
                    "actionGroups": [
                        {
                            "actionGroupId": "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]",
                            "webhookProperties": {}
                        }
                    ]
                },
                "enabled": true,
                "description": ""
            },
            "dependsOn": [
                "[resourceId('microsoft.insights/actionGroups', parameters('actionGroups_name'))]"
            ]
        }
    ]
}
```

## <a name="manage-your-alerts"></a>Uyarılarınızı yönetme

Bir uyarı oluşturduktan sonra, **Monitör'ün** **Uyarılar** bölümünde görünür. Yönetmek istediğiniz uyarıyı seçin:

* Onu edin.
* Sil onu.
* Uyarı için bildirimleri geçici olarak durdurmak veya almaya devam etmek istiyorsanız, devre dışı bırakın veya etkinleştirin.

## <a name="next-steps"></a>Sonraki adımlar
- Azure [Hizmet Durumu uyarıları ayarlamak için en iyi uygulamalar](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUa)hakkında bilgi edinin.
- [Azure Hizmet Durumu için mobil anında iletme bildirimlerini](https://www.microsoft.com/en-us/videoplayer/embed/RE2OtUw)nasıl ayarlarınızı öğrenin.
- [Mevcut sorun yönetim sistemleri için webhook bildirimlerini nasıl yapılandırıştırılamayı](service-health-alert-webhook-guide.md)öğrenin.
- Hizmet [durumu bildirimleri](service-notifications.md)hakkında bilgi edinin.
- Bildirim [oranı sınırlaması](../azure-monitor/platform/alerts-rate-limiting.md)hakkında bilgi edinin.
- Etkinlik [günlüğü uyarısı webhook şema](../azure-monitor/platform/activity-log-alerts-webhook.md)gözden geçirin.
- Etkinlik [günlüğü uyarılarına genel](../azure-monitor/platform/alerts-overview.md)bir bakış alın ve uyarıları nasıl alacağınızı öğrenin.
- [Eylem grupları](../azure-monitor/platform/action-groups.md)hakkında daha fazla bilgi edinin.

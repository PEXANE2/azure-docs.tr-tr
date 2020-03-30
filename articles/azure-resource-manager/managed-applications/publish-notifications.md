---
title: Bildirimlerle yönetilen uygulamalar
description: Yönetilen uygulama örneklerinde oluşturmalar, güncelleştirmeler, silmeler ve hatalar hakkında bildirimler almak için yönetilen uygulamaları webhook uç noktalarıyla yapılandırın.
ms.topic: conceptual
ms.author: ilahat
author: ilahat
ms.date: 11/01/2019
ms.openlocfilehash: ff058d7b51bd2e5efd80db69e5928d58fc5a7725
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76715678"
---
# <a name="azure-managed-applications-with-notifications"></a>Bildirimlerle Azure yönetilen uygulamalar

Azure yönetilen uygulama bildirimleri, yayıncıların yönetilen uygulama örneklerinin yaşam döngüsü olaylarını temel alan eylemleri otomatikleştirmesine olanak tanır. Yayıncılar, yeni ve varolan yönetilen uygulama örnekleri hakkında olay bildirimleri almak için özel bildirim webhook uç noktaları belirtebilir. Yayımcılar, uygulama sağlama, güncelleştirme ler ve silme sırasında özel iş akışları ayarlayabilir.

## <a name="getting-started"></a>Başlarken
Yönetilen uygulamaları almaya başlamak için, genel bir HTTPS bitiş noktası nı döndürün ve hizmet kataloğu uygulama tanımını veya Azure Marketi teklifini yayımladığınızda belirtin.

Hızlı bir şekilde başlamak için önerilen adımlar şunlardır:
1. Gelen POST isteklerini ve döndürüğü `200 OK`ortak bir HTTPS bitiş noktasını döndürün.
2. Bu makalede daha sonra açıklandığı gibi hizmet kataloğu uygulama tanımına veya Azure Marketi teklifine bitiş noktasını ekleyin.
3. Uygulama tanımına veya Azure Marketi teklifine başvuran yönetilen bir uygulama örneği oluşturun.
4. Bildirimlerin alındığını doğrulayın.
5. Bu makalenin **Bitiş Noktası kimlik doğrulama** bölümünde açıklandığı gibi yetkilendirmeyi etkinleştirin.
6. Bildirim isteklerini ayrıştırmak ve bildirime dayalı olarak iş mantığınızı uygulamak için bu makalenin **Bildirim şeması** bölümündeki yönergeleri izleyin.

## <a name="add-service-catalog-application-definition-notifications"></a>Hizmet kataloğu uygulama tanımı bildirimleri ekleme
#### <a name="azure-portal"></a>Azure portalında
Başlamak için bkz. [Azure portalı üzerinden hizmet kataloğu uygulaması yayımla.](./publish-portal.md)

![Azure portalında hizmet kataloğu uygulama tanımı bildirimleri](./media/publish-notifications/service-catalog-notifications.png)

#### <a name="rest-api"></a>REST API

> [!NOTE]
> Şu anda, uygulama tanımı özelliklerinde `notificationEndpoints` yalnızca bir bitiş noktası sağlayabilirsiniz.

``` JSON
    {
      "properties": {
        "isEnabled": true,
        "lockLevel": "ReadOnly",
        "displayName": "Sample Application Definition",
        "description": "Notification-enabled application definition.",
        "notificationPolicy": {
            "notificationEndpoints": [
                {
                    "uri": "https://isv.azurewebsites.net:1214?sig=unique_token"
                }
            ]
        },
        "authorizations": [
          {
            "principalId": "d6b7fbd3-4d99-43fe-8a7a-f13aef11dc18",
            "roleDefinitionId": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635"
          },
        ...

```
## <a name="add-azure-marketplace-managed-application-notifications"></a>Azure Marketi yönetilen uygulama bildirimleri ekleme
Daha fazla bilgi için [bkz.](../../marketplace/cloud-partner-portal/azure-applications/cpp-create-offer.md)

![Azure Marketi, Azure portalında uygulama bildirimlerini yönetti](./media/publish-notifications/marketplace-notifications.png)
## <a name="event-triggers"></a>Olay tetikleyicileri
Aşağıdaki tabloda EventType ve ProvisioningState'in tüm olası birleşimleri ve tetikleyicileri açıklanmaktadır:

Olay türü | ProvisioningState | Bildirim için tetikleyici
---|---|---
PUT | Accepted | Yönetilen kaynak grubu oluşturuldu ve uygulama PUT sonra başarıyla yansıtılan (yönetilen kaynak grubu içinde dağıtım başladı önce).
PUT | Başarılı oldu | Yönetilen uygulamanın tam sağlanması bir PUT sonra başarılı oldu.
PUT | Başarısız | Herhangi bir noktada uygulama örneği sağlama PUT başarısızlığı.
Yama | Başarılı oldu | Etiketleri, JIT erişim ilkesini veya yönetilen kimliği güncelleştirmek için yönetilen uygulama örneğinde başarılı bir YAMA'dan sonra.
DELETE | Silme | Kullanıcı yönetilen bir uygulama örneğinin DELETE'sini başlattığı anda.
DELETE | Silme | Yönetilen uygulamanın tam ve başarılı silinmesi sonra.
DELETE | Başarısız | Silme işlemini engelleyen deprovisioning işlemi sırasında herhangi bir hata dan sonra.
## <a name="notification-schema"></a>Bildirim şeması
Bildirimleri işlemek için webhook bitiş noktanızı döndürdükten sonra, bildirimüzerine hareket etmek için önemli özellikleri elde etmek için yükü ayrışdırman gerekir. Hizmet kataloğu ve Azure Marketi tarafından yönetilen uygulama bildirimleri aynı özelliklerin çoğunu sağlar. Örnekleri izleyen tabloda iki küçük fark özetlenmiştir.

#### <a name="service-catalog-application-notification-schema"></a>Hizmet kataloğu uygulama bildirim şeması
Yönetilen bir uygulama örneğinin başarılı bir şekilde sağlanmasından sonra örnek bir hizmet kataloğu bildirimi aşağıda verilmiştir:
``` HTTP
POST https://{your_endpoint_URI}/resource?{optional_parameter}={optional_parameter_value} HTTP/1.1

{
    "eventType": "PUT",
    "applicationId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applications/<applicationName>",
    "eventTime": "2019-08-14T19:20:08.1707163Z",
    "provisioningState": "Succeeded",
    "applicationDefinitionId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applicationDefinitions/<appDefName>"    
}

```

Sağlama başarısız olursa, hata ayrıntılarını içeren bir bildirim belirtilen bitiş noktasına gönderilir.

``` HTTP
POST https://{your_endpoint_URI}/resource?{optional_parameter}={optional_parameter_value} HTTP/1.1

{
    "eventType": "PUT",
    "applicationId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applications/<applicationName>",
    "eventTime": "2019-08-14T19:20:08.1707163Z",
    "provisioningState": "Failed",
    "applicationDefinitionId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applicationDefinitions/<appDefName>",
    "error": {
        "code": "ErrorCode",
        "message": "error message",
        "details": [
            {
                "code": "DetailedErrorCode",
                "message": "error message"
            }
        ]
    }
}

```

#### <a name="azure-marketplace-application-notification-schema"></a>Azure Marketi uygulama bildirimi şeması

Yönetilen bir uygulama örneğinin başarılı bir şekilde sağlanmasından sonra örnek bir hizmet kataloğu bildirimi aşağıda verilmiştir:
``` HTTP
POST https://{your_endpoint_URI}/resource?{optional_parameter}={optional_parameter_value} HTTP/1.1

{
    "eventType": "PUT",
    "applicationId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applications/<applicationName>",
    "eventTime": "2019-08-14T19:20:08.1707163Z",
    "provisioningState": "Succeeded",
    "billingDetails": {
        "resourceUsageId":"<resourceUsageId>"
    },
    "plan": {
        "publisher": "publisherId",
        "product": "offer",
        "name": "skuName",
        "version": "1.0.1"
    }
}

```

Sağlama başarısız olursa, hata ayrıntılarını içeren bir bildirim belirtilen bitiş noktasına gönderilir.

``` HTTP
POST https://{your_endpoint_URI}/resource?{optional_parameter}={optional_parameter_value} HTTP/1.1

{
    "eventType": "PUT",
    "applicationId": "subscriptions/<subId>/resourceGroups/<rgName>/providers/Microsoft.Solutions/applications/<applicationName>",
    "eventTime": "2019-08-14T19:20:08.1707163Z",
    "provisioningState": "Failed",
    "billingDetails": {
        "resourceUsageId":"<resourceUsageId>"
    },
    "plan": {
        "publisher": "publisherId",
        "product": "offer",
        "name": "skuName",
        "version": "1.0.1"
    },
    "error": {
        "code": "ErrorCode",
        "message": "error message",
        "details": [
            {
                "code": "DetailedErrorCode",
                "message": "error message"
            }
        ]
    }
}

```

Parametre | Açıklama
---|---
Eventtype | Bildirimi tetikleyen olay türü. (Örneğin, PUT, PATCH, DELETE.)
applicationId | Bildirimin tetiklendiği yönetilen uygulamanın tam nitelikli kaynak tanımlayıcısı.
eventTime | Bildirimi tetikleyen olayın zaman damgası. (UTC ISO 8601 formatında tarih ve saat.)
provisioningDevlet | Yönetilen uygulama örneğinin sağlama durumu. (Örneğin, Başarılı, Başarısız, Silme, Silinmiş.)
error | *Yalnızca provisioningState Başarısız olduğunda belirtilir.* Hataya neden olan hata kodunu, iletiyi ve sorunun ayrıntılarını içerir.
uygulamaDefinitionId | *Yalnızca hizmet kataloğu yönetilen uygulamalar için belirtilir.* Yönetilen uygulama örneğinin sağlandığı uygulama tanımının tam nitelikli kaynak tanımlayıcısını temsil eder.
plan | *Yalnızca Azure Marketi tarafından yönetilen uygulamalar için belirtilir.* Yönetilen uygulama örneğinin yayımcısını, teklifini, SKU'sunu ve sürümünü temsil eder.
faturalandırmaAyrıntılar | *Yalnızca Azure Marketi tarafından yönetilen uygulamalar için belirtilir.* Yönetilen uygulama örneğinin fatura ayrıntıları. Kullanım ayrıntıları için Azure Marketi'ni sorgulamak için kullanabileceğiniz kaynakUsageId'i içerir.

## <a name="endpoint-authentication"></a>Uç nokta kimlik doğrulaması
Webhook bitiş noktasını güvenli hale getirmek ve bildirimin gerçekliğini sağlamak için:
1. Webhook URI'nin üstüne şöyle bir sorgu parametresi sağlayın: https\://your-endpoint.com?sig=Guid. Her bildirimde, sorgu parametresinin `sig` beklenen `Guid`değere sahip olup olmadığını denetleyin.
2. applicationId'i kullanarak yönetilen uygulama örneğinde GET sorunu. Tutarlılık sağlamak için provisioningState'in bildirimin hükmüDurumuyla eşleştiğini doğrulayın.

## <a name="notification-retries"></a>Bildirim yeniden denemeleri

Yönetilen Uygulama Bildirimi hizmeti, webhook bitiş noktasından bildirime yanıt `200 OK` bekler. Webhook bitiş noktası 500'den büyük veya eşit bir HTTP hata kodu döndürürse, 429 hata kodu döndürürse veya bitiş noktası geçici olarak erişilemezse bildirim hizmeti yeniden çalışır. Webhook bitiş noktası 10 saat içinde kullanıma sunulmazsa, bildirim iletisi bırakılır ve yeniden denemeler durdurulır.

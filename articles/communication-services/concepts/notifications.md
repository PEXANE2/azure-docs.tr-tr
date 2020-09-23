---
title: Azure Iletişim hizmetlerindeki bildirimler
titleSuffix: An Azure Communication Services concept document
description: Azure Iletişim Hizmetleri üzerinde oluşturulmuş uygulamaların kullanıcılarına bildirimler gönderin.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 5be2ff48ea5510c54c79e76e8bae082bd5085794
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90947943"
---
# <a name="communication-services-notifications"></a>İletişim Hizmetleri bildirimleri

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Azure Iletişim Hizmetleri sohbeti ve arama istemci kitaplıkları, sinyal iletilerinin bağlı istemcilere verimli ve güvenilir bir şekilde gönderilmesine izin veren gerçek zamanlı bir mesajlaşma kanalı oluşturur. Bu, karmaşık HTTP yoklama mantığını uygulamaya gerek kalmadan uygulamalarınıza zengin ve gerçek zamanlı iletişim işlevleri oluşturmanıza olanak sağlar. Ancak, mobil uygulamalarda, bu sinyal kanalı yalnızca uygulamanız ön planda etkin olduğunda bağlı kalır. Uygulamanız arka planda olduğunda kullanıcılarınızın gelen çağrıları veya sohbet iletilerini almasını istiyorsanız anında iletme bildirimleri kullanmanız gerekir.

Anında iletme bildirimleri uygulamanızdan kullanıcıların mobil cihazlarına bilgi göndermenize olanak tanır. Bir iletişim kutusu göstermek, ses çalmak veya gelen çağrı Kullanıcı arabirimini görüntülemek için anında iletme bildirimlerini kullanabilirsiniz. Azure Iletişim Hizmetleri, uygulamalarınıza anında iletme bildirimleri eklemenize olanak tanıyan [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) ve [Azure Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-overview) tümleştirmelerini sağlar.

## <a name="trigger-push-notifications-via-azure-event-grid"></a>Azure Event Grid aracılığıyla anında iletme bildirimlerini tetikleme

Azure Iletişim Hizmetleri, gerçek zamanlı olay bildirimlerini güvenilir, ölçeklenebilir ve güvenli bir şekilde sunmak için [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) ile tümleşir. [Azure işlevini](https://docs.microsoft.com/azure/azure-functions/functions-overview) veya Web kancasını tetikleyen bir olay Kılavuzu aboneliği oluşturarak kullanıcılarınıza mobil anında iletme bildirimleri sağlayan bir bildirim hizmeti oluşturmak için bu tümleştirmeden yararlanabilirsiniz.

:::image type="content" source="./media/notifications/acs-events-int.png" alt-text="Iletişim hizmetlerinin Event Grid ile nasıl tümleştirildiğini gösteren diyagram.":::

[Azure Iletişim hizmetlerinde olay işleme](./event-handling.md)hakkında daha fazla bilgi edinin.

## <a name="deliver-push-notifications-via-azure-notification-hubs"></a>Azure Notification Hubs aracılığıyla anında iletme bildirimleri sunun

Bir Azure Notification Hub 'ını, bir kullanıcının mobil cihazına gelen bir çağrı aldıklarında otomatik olarak anında iletme bildirimleri göndermek için Iletişim Hizmetleri kaynağına bağlayabilirsiniz. Bu anında iletme bildirimlerini, uygulamanızın arka planda uyandırmasını ve kullanıcının çağrıyı kabul etmesi veya reddetmesine izin veren Kullanıcı ARABIRIMINI görüntülemesi gerekir. 

:::image type="content" source="./media/notifications/acs-anh-int.png" alt-text="İletişim hizmetlerinin Azure Notifications hub ile nasıl tümleştirildiğini gösteren diyagram.":::

İletişim Hizmetleri, [doğrudan gönderme](https://docs.microsoft.com/rest/api/notificationhubs/direct-send) API 'sini kullanarak platforma özgü çeşitli anında iletme bildirimi hizmetleriyle iletişim kurmak Için Azure Notification Hub 'ını doğrudan bir geçiş hizmeti olarak kullanır. Bu, uygulamalarınıza düşük gecikme süresi ve güvenilir çağrı bildirimleri sunmak için mevcut Azure Notification Hub kaynaklarınızı ve yapılandırmanızı yeniden kullanmanıza olanak tanır.

### <a name="notification-hub-provisioning"></a>Notification Hub 'ı sağlama 

İstemci cihazlarına Notification Hubs kullanarak anında iletme bildirimleri göndermek için, Iletişim Hizmetleri kaynağınız ile aynı abonelikte [bir Bildirim Hub 'ı oluşturun](https://docs.microsoft.com/azure/notification-hubs/create-notification-hub-portal) . Kullanmak istediğiniz platform bildirimleri hizmeti için Azure Notification Hubs yapılandırılmalıdır. İstemci uygulamanızda Notification Hubs anında iletme bildirimleri alma hakkında bilgi edinmek için bkz. [Notification Hubs kullanmaya](https://docs.microsoft.com/azure/notification-hubs/ios-sdk-get-started) başlama ve sayfanın üst kısmındaki açılan listeden hedef istemci platformunuzu seçme.

Bildirim Hub 'ınız yapılandırıldıktan sonra, Azure Resource Manager Istemcisini veya Azure portal aracılığıyla Hub için bir bağlantı dizesi sağlayarak Iletişim Hizmetleri kaynağıyla ilişkilendirilebilmeniz gerekir. Bağlantı dizesi "Gönder" izinleri içermelidir. Yalnızca hub 'ınız için özel olarak "Gönder" izinlerine sahip başka bir erişim ilkesi oluşturmanızı öneririz. [Notification Hubs güvenlik ve erişim ilkeleri](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-push-notification-security) hakkında daha fazla bilgi edinin

> NOTE: Apple Anında İletilen Bildirim Servisi VOıP bildirimlerini etkinleştirmek için, Bildirim Hub 'ınızın adını sonek ile uygulama paketi KIMLIĞINIZ olacak şekilde ayarlamanız gerekir `.voip` . Bkz. [Notification Hubs aracılığıyla APNs VoIP kullanma](https://docs.microsoft.com/azure/notification-hubs/voip-apns).

#### <a name="using-the-azure-resource-manager-client-to-configure-the-notification-hub"></a>Bildirim Hub 'ını yapılandırmak için Azure Resource Manager istemcisini kullanma

Azure Resource Manager oturum açmak için aşağıdakini yürütün ve kimlik bilgilerinizi kullanarak oturum açın.

```console
armclient login
```

 Başarıyla oturum açıldıktan sonra Bildirim Hub 'ını sağlamak için aşağıdakini yürütün:

```console
armclient POST /subscriptions/<sub_id>/resourceGroups/<resource_group>/providers/Microsoft.Communication/CommunicationServices/<resource_id>/linkNotificationHub?api-version=2020-08-20-preview "{'connectionString': '<connection_string>','resourceId': '<resource_id>'}"
```

#### <a name="using-the-azure-portal-to-configure-the-notification-hub"></a>Bildirim Hub 'ını yapılandırmak için Azure portal kullanma

Portalda Azure Communication Services kaynağınız ' ne gidin. Iletişim Hizmetleri kaynağı içinde Iletişim Hizmetleri sayfasının sol menüsünde anında Iletme bildirimleri ' ni seçin ve daha önce sağladığınız Bildirim Hub 'ını bağlayın. Bağlantı dizenizi ve kaynak KIMLIĞINIZI buradan sağlamanız gerekir:

:::image type="content" source="./media/notifications/acs-anh-portal-int.png" alt-text="Azure Portal 'daki anında Iletme bildirimleri ayarlarını gösteren ekran görüntüsü.":::

#### <a name="device-registration"></a>Cihaz kaydı 

Cihaz işleyicinizi Iletişim hizmetleriyle nasıl kaydedeceğinizi öğrenmek için [sesli arama hızlı başlangıç](../quickstarts/voice-video-calling/getting-started-with-calling.md) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event Grid giriş için bkz. [Event Grid nedir?](https://docs.microsoft.com/azure/event-grid/overview)
* Azure Notification Hub 'ı kavramları hakkında daha fazla bilgi edinmek için bkz. [azure Notification Hubs belgeleri](https://docs.microsoft.com/azure/notification-hubs/)

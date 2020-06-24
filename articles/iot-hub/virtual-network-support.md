---
title: Sanal ağlar için Azure IoT Hub desteği
description: IoT Hub ile sanal ağlar bağlantı modelini kullanma
services: iot-hub
author: jlian
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 06/16/2020
ms.author: jlian
ms.openlocfilehash: bf193859c140001def83a18ca7965d9cbd312b02
ms.sourcegitcommit: 34eb5e4d303800d3b31b00b361523ccd9eeff0ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84907542"
---
# <a name="iot-hub-support-for-virtual-networks-with-private-link-and-managed-identity"></a>Özel bağlantı ve yönetilen kimlik ile sanal ağlar için IoT Hub desteği

Varsayılan olarak, IoT Hub ana bilgisayar adları, internet üzerinden genel olarak yönlendirilebilir bir IP adresi olan genel bir uç noktaya eşlenir. Farklı müşteriler bu IoT Hub genel uç noktasını paylaşır ve geniş alan ağlarda ve şirket içi ağlarda bulunan IoT cihazları buna erişebilir.

![IoT Hub ortak uç nokta](./media/virtual-network-support/public-endpoint.png)

[İleti yönlendirme](./iot-hub-devguide-messages-d2c.md), [dosya yükleme](./iot-hub-devguide-file-upload.md)ve [toplu cihaz içeri/dışarı aktarma](./iot-hub-bulk-identity-mgmt.md) dahil IoT Hub Özellikler Ayrıca, genel uç noktası üzerinden müşteriye ait bir Azure kaynağına IoT Hub bağlantı gerektirir. Bu bağlantı yolları topluca IoT Hub çıkış trafiğini müşteri kaynaklarına yapar.

Sahip olduğunuz ve çalışan bir sanal ağ üzerinden Azure kaynaklarınızla (IoT Hub dahil) olan bağlantıyı kısıtlamak isteyebilirsiniz. Bu nedenlerden bazıları şunlardır:

* Genel internet 'e bağlantı pozlamasını önleyerek IoT Hub 'ınız için ağ yalıtımına giriş.

* Verilerinizin ve trafiğinizin doğrudan Azure omurga ağına aktarılmasını sağlamak için şirket içi ağ varlıklarınızdan özel bir bağlantı deneyimini etkinleştirme.

* Gizli şirket içi ağlarda gerçekleştirilen ayıklanma saldırılarını önleyin. 

* [Özel uç noktaları](../private-link/private-endpoint-overview.md)kullanan Azure genelindeki Azure genelinde bağlantı desenleri aşağıda verilmiştir.

Bu makalede, giriş bağlantısı için [Azure özel bağlantısı](../private-link/private-link-overview.md) kullanılarak IoT Hub ve diğer azure kaynaklarına IoT Hub çıkış bağlantısı Için güvenilen Microsoft Hizmetleri özel durumu kullanılarak bu hedeflere nasıl ulaşmakta olduğunuz açıklanır.

## <a name="ingress-connectivity-to-iot-hub-using-azure-private-link"></a>Azure özel bağlantısı kullanarak IoT Hub giriş bağlantısı

Özel uç nokta, bir Azure kaynağına erişilebilen, müşterinin sahip olduğu VNet içinde ayrılmış özel bir IP adresidir. Azure özel bağlantısı ile, IoT Hub 'ınız için bir özel uç nokta ayarlayarak sanal ağınızdaki hizmetlerin, IoT Hub genel uç noktasına gönderilmesine gerek kalmadan IoT Hub ulaşmasını sağlayabilirsiniz. Benzer şekilde, şirket içi cihazlarınız [sanal özel ağ (VPN)](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [ExpressRoute](https://azure.microsoft.com/services/expressroute/) eşlemesi kullanarak VNET 'iniz ve IoT Hub (Özel uç noktası aracılığıyla) için bağlantı elde edebilir. Sonuç olarak, [IoT Hub IP filtresi](./iot-hub-ip-filtering.md) kullanarak IoT Hub 'ınızın genel uç noktalarına [olan](#built-in-event-hub-compatible-endpoint-doesnt-support-access-over-private-endpoint)bağlantıyı kısıtlayabilir veya tamamen devre dışı bırakabilirsiniz. Bu yaklaşım, cihazlar için özel uç nokta kullanarak hub 'ınıza bağlantı sağlar. Bu kurulumun ana odağı, şirket içi bir ağ içindeki cihazlar içindir. Bu kurulum, geniş alan ağlarda dağıtılan cihazlar için önerilmez.

![IoT Hub ortak uç nokta](./media/virtual-network-support/virtual-network-ingress.png)

Devam etmeden önce aşağıdaki önkoşulların karşılandığından emin olun:

* Özel uç noktanın oluşturulacağı bir alt ağa sahip [bir Azure VNET oluşturdunuz](../virtual-network/quick-create-portal.md) .

* Şirket içi ağlarda çalışan cihazlarda, [sanal özel ağ (VPN)](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [ExpressRoute](https://azure.microsoft.com/services/expressroute/) özel eşlemesini Azure VNET 'iniz ile ayarlayın.

### <a name="set-up-a-private-endpoint-for-iot-hub-ingress"></a>IoT Hub giriş için özel bir uç nokta ayarlama

1. Azure portal ' de **ağ**, **Özel uç nokta bağlantıları**' nı seçin ve **+ Özel uç noktasına**tıklayın.

    :::image type="content" source="media/virtual-network-support/private-link.png" alt-text="IoT Hub için özel uç noktanın nereye ekleneceğini gösteren ekran görüntüsü":::

1. İçinde yeni özel bitiş noktasını oluşturmak için abonelik, kaynak grubu, ad ve bölge belirtin. İdeal olarak, Özel uç noktanın hub 'ınız ile aynı bölgede oluşturulması gerekir.

1. **İleri: kaynak**' a tıklayın ve IoT Hub kaynağınız için abonelik sağlayın ve kaynak türü olarak **"Microsoft. Devices/iothubs"** , **kaynak**olarak IoT Hub adı ve hedef alt kaynak olarak **ıothub** seçeneğini belirleyin.

1. **İleri** ' ye tıklayın ve içinde özel uç nokta oluşturmak için Sanal ağınızı ve alt ağınızı sağlayın. İsterseniz Azure özel DNS bölgesi ile tümleştirme seçeneğini belirleyin.

1. **İleri: Etiketler**' e tıklayın ve isteğe bağlı olarak kaynağınız için Etiketler sağlayın.

1. Özel bağlantı kaynağınızı oluşturmak için **gözden geçir + oluştur** ' a tıklayın.

### <a name="built-in-event-hub-compatible-endpoint-doesnt-support-access-over-private-endpoint"></a>Yerleşik Olay Hub 'ı ile uyumlu uç nokta, Özel uç nokta üzerinden erişimi desteklemez

[Yerleşik Olay Hub 'ı ile uyumlu uç nokta](iot-hub-devguide-messages-read-builtin.md) , Özel uç nokta üzerinden erişimi desteklemez. Yapılandırıldığında, bir IoT Hub 'ının özel uç noktası yalnızca giriş bağlantısına yöneliktir. Yerleşik Olay Hub 'ı ile uyumlu uç noktadan veri tüketme yalnızca genel İnternet üzerinden yapılabilir. 

IoT Hub [IP filtresi](iot-hub-ip-filtering.md) , yerleşik uç noktaya genel erişimi de denetlemez. IoT Hub 'ınıza genel ağ erişimini tamamen engellemek için şunları yapmanız gerekir: 

1. IoT Hub için özel uç nokta erişimini yapılandırma
1. Tüm IP 'yi engellemek için IP filtresini kullanarak genel ağ erişimini devre dışı bırakın
1. [Veri göndermemek üzere yönlendirme ayarlayarak](iot-hub-devguide-messages-d2c.md) yerleşik Olay Hub 'ı uç noktasını devre dışı bırakın
1. [Geri dönüş yolunu](iot-hub-devguide-messages-d2c.md#fallback-route) kapat
1. [Güvenilen Microsoft hizmetini](#egress-connectivity-from-iot-hub-to-other-azure-resources) kullanarak çıkış diğer Azure kaynaklarına yapılandırma

### <a name="pricing-for-private-link"></a>Özel bağlantı fiyatlandırması

Fiyatlandırma ayrıntıları için bkz. [Azure özel bağlantı fiyatlandırması](https://azure.microsoft.com/pricing/details/private-link).

## <a name="egress-connectivity-from-iot-hub-to-other-azure-resources"></a>IoT Hub diğer Azure kaynaklarına çıkış bağlantısı

IoT Hub, Azure Blob depolama, Olay Hub 'ınıza, [ileti yönlendirme](./iot-hub-devguide-messages-d2c.md)için hizmet veri yolu kaynaklarına, [dosya yüklemeye](./iot-hub-devguide-file-upload.md)ve kaynakların genel uç noktasına [toplu cihaz içeri/dışarı aktarmaya](./iot-hub-bulk-identity-mgmt.md) bağlanabilir. Kaynağınızı bir sanal ağa bağlama varsayılan olarak kaynağa olan bağlantıyı engeller. Sonuç olarak, bu yapılandırma IoT Hub verilerinin kaynaklarınıza veri göndermesini engeller. Bu sorunu onarmak için, **güvenilir Microsoft hizmet** seçeneği aracılığıyla IoT Hub kaynağınızın depolama hesabınıza, Olay Hub 'ına veya hizmet veri yolu kaynaklarına bağlantıyı etkinleştirin.

### <a name="turn-on-managed-identity-for-iot-hub"></a>IoT Hub için yönetilen kimliği aç

Diğer hizmetlerin IoT Hub 'ınızı güvenilir bir Microsoft hizmeti olarak bulmasına izin vermek için, sistem tarafından atanan bir yönetilen kimliğe sahip olmalıdır.

1. IoT Hub portalınızdaki **kimlik** 'e gidin

1. **Durum**altında, **Açık**' ı seçin ve ardından **Kaydet**' e tıklayın.

    :::image type="content" source="media/virtual-network-support/managed-identity.png" alt-text="IoT Hub için yönetilen kimliği açmayı gösteren ekran görüntüsü":::

### <a name="assign-managed-identity-to-your-iot-hub-at-creation-time-using-arm-template"></a>ARM şablonunu kullanarak oluşturma sırasında IoT Hub yönetilen kimlik atama

Kaynak sağlama zamanında IoT Hub 'ınıza yönetilen kimlik atamak için aşağıdaki ARM şablonunu kullanın:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Devices/IotHubs",
      "apiVersion": "2020-03-01",
      "name": "<provide-a-valid-resource-name>",
      "location": "<any-of-supported-regions>",
      "identity": {
        "type": "SystemAssigned"
      },
      "sku": {
        "name": "<your-hubs-SKU-name>",
        "tier": "<your-hubs-SKU-tier>",
        "capacity": 1
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-02-01",
      "name": "updateIotHubWithKeyEncryptionKey",
      "dependsOn": [
        "<provide-a-valid-resource-name>"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
          "contentVersion": "0.9.0.0",
          "resources": [
            {
              "type": "Microsoft.Devices/IotHubs",
              "apiVersion": "2020-03-01",
              "name": "<provide-a-valid-resource-name>",
              "location": "<any-of-supported-regions>",
              "identity": {
                "type": "SystemAssigned"
              },
              "sku": {
                "name": "<your-hubs-SKU-name>",
                "tier": "<your-hubs-SKU-tier>",
                "capacity": 1
              }
            }
          ]
        }
      }
    }
  ]
}
```

Kaynağınızın değerlerini değiştirdikten sonra, `name` `location` `SKU.name` ve `SKU.tier` kullanarak kaynağı var olan bir kaynak grubuna dağıtmak için Azure CLI kullanabilirsiniz:

```azurecli-interactive
az deployment group create --name <deployment-name> --resource-group <resource-group-name> --template-file <template-file.json>
```

Kaynak oluşturulduktan sonra, Azure CLı kullanarak hub 'ınıza atanan yönetilen hizmet kimliğini alabilirsiniz:

```azurecli-interactive
az resource show --resource-type Microsoft.Devices/IotHubs --name <iot-hub-resource-name> --resource-group <resource-group-name>
```

### <a name="pricing-for-managed-identity"></a>Yönetilen kimlik fiyatlandırması

Güvenilen Microsoft ilk taraf hizmetleri özel durum özelliği ücretsizdir. Sağlanan depolama hesapları, Olay Hub 'ları veya hizmet veri yolu kaynakları için ücretler ayrıca geçerlidir.

### <a name="egress-connectivity-to-storage-account-endpoints-for-routing"></a>Yönlendirme için depolama hesabı uç noktalarına giden çıkış bağlantısı

IoT Hub, iletileri müşteriye ait bir depolama hesabına yönlendirebilir. Güvenlik duvarı kısıtlamaları olduğunda yönlendirme işlevselliğinin bir depolama hesabına erişmesine izin vermek için, IoT Hub [yönetilen bir kimliğe](#turn-on-managed-identity-for-iot-hub)sahip olması gerekir. Yönetilen bir kimlik sağlandıktan sonra, depolama hesabınıza erişmek için hub 'ın kaynak kimliğine RBAC izni vermek üzere aşağıdaki adımları izleyin.

1. Azure portal, depolama hesabınızın **erişim denetimi (IAM)** sekmesine gidin ve **rol ataması Ekle** bölümünde **Ekle** ' ye tıklayın.

2. **Rol**, **Azure AD kullanıcısı, Grup veya hizmet sorumlusu** tarafından, **erişim atama** ve açılır listede IoT Hub kaynak adını seçme olarak, **Depolama Blobu veri katılımcısı** ([*not* katılımcı veya depolama hesabı katılımcısı](../storage/common/storage-auth-aad-rbac-portal.md#rbac-roles-for-blobs-and-queues)) seçin. **Kaydet** düğmesine tıklayın.

3. Depolama hesabınızdaki **güvenlik duvarları ve sanal ağlar** sekmesine gidin ve **Seçili ağlardan erişime izin ver** seçeneğini etkinleştirin. **Özel durumlar** listesi altında, **Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

4. IoT Hub kaynak sayfasında **ileti yönlendirme** sekmesi ' ne gidin.

5. **Özel uç noktalar** bölümüne gidin ve **Ekle**' ye tıklayın. Uç nokta türü olarak **depolama** ' yı seçin.

6. Görüntülenen sayfada uç noktanız için bir ad sağlayın, blob depoınızda kullanmayı düşündüğünüz kapsayıcıyı seçin, kodlama ve dosya adı biçimini belirtin. Depolama uç noktanıza **kimlik doğrulama türü** olarak **atanan sistem** ' i seçin. **Oluştur** düğmesine tıklayın.

Artık özel depolama uç noktanız, hub 'ın sistem tarafından atanan kimliğini kullanacak şekilde ayarlanır ve güvenlik duvarı kısıtlamalarına rağmen depolama kaynağına erişme izni vardır. Artık bu uç noktayı bir yönlendirme kuralı ayarlamak için kullanabilirsiniz.

### <a name="egress-connectivity-to-event-hubs-endpoints-for-routing"></a>Yönlendirme için Olay Hub 'ları uç noktalarına giden bağlantı

IoT Hub, iletileri müşteriye ait bir olay hub 'ı ad alanına yönlendirmek üzere yapılandırılabilir. Güvenlik duvarı kısıtlamaları yerinde yönlendirme işlevselliğinin bir olay hub 'ı kaynağına erişmesine izin vermek için, IoT Hub yönetilen bir kimliğe sahip olmalıdır. Yönetilen bir kimlik oluşturulduktan sonra, Olay Hub 'larınızı erişmek üzere hub 'ın kaynak kimliğine RBAC izni vermek için aşağıdaki adımları izleyin.

1. Azure portal, Olay Hub 'ları **erişim denetimi (IAM)** sekmesine gidin ve **rol ataması Ekle** bölümünde **Ekle** ' ye tıklayın.

2. **Erişim atama** olarak **Event Hubs veri göndereni** **rol**, **Azure AD kullanıcısı, Grup veya hizmet sorumlusu** olarak seçin ve açılan listeden IoT Hub kaynak adını seçin. **Kaydet** düğmesine tıklayın.

3. Olay Hub 'larınızdaki **güvenlik duvarları ve sanal ağlar** sekmesine gidin ve **Seçili ağlardan erişime izin ver** seçeneğini etkinleştirin. **Özel durumlar** listesi altında, **Güvenilen Microsoft hizmetlerinin Olay Hub 'Larına erişmesine izin ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

4. IoT Hub kaynak sayfasında **ileti yönlendirme** sekmesi ' ne gidin.

5. **Özel uç noktalar** bölümüne gidin ve **Ekle**' ye tıklayın. Uç nokta türü olarak **Olay Hub 'larını** seçin.

6. Görüntülenen sayfada uç noktanız için bir ad sağlayın, Olay Hub 'larını ve örneğinizi seçin ve **Oluştur** düğmesine tıklayın.

Artık özel olay hub 'larınız, hub 'ın sistem tarafından atanan kimliğini kullanacak şekilde ayarlanmıştır ve güvenlik duvarı kısıtlamalarına rağmen Olay Hub 'ınıza erişme iznine sahiptir. Artık bu uç noktayı bir yönlendirme kuralı ayarlamak için kullanabilirsiniz.

### <a name="egress-connectivity-to-service-bus-endpoints-for-routing"></a>Yönlendirme için Service Bus uç noktalarına çıkış bağlantısı

IoT Hub, iletileri müşteriye ait Service Bus ad alanına yönlendirmek üzere yapılandırılabilir. Güvenlik duvarı kısıtlamaları varken yönlendirme işlevselliğinin bir hizmet veri yolu kaynağına erişmesine izin vermek için, IoT Hub yönetilen bir kimliğe sahip olması gerekir. Yönetilen bir kimlik sağlandıktan sonra, hizmet veri yoluna erişmek için hub 'ın kaynak kimliğine RBAC izni vermek üzere aşağıdaki adımları izleyin.

1. Azure portal, hizmet veri yolu **erişim denetimi (IAM)** sekmesine gidin ve **rol ataması Ekle** bölümünde **Ekle** ' ye tıklayın.

2. **Erişim atama** olarak **hizmet veri yolu verileri göndereni** **rol**, **Azure AD kullanıcısı, Grup veya hizmet sorumlusu** olarak seçin ve aşağı açılan listeden IoT Hub kaynak adını seçin. **Kaydet** düğmesine tıklayın.

3. Service Bus 'daki **güvenlik duvarları ve sanal ağlar** sekmesine gidin ve **Seçili ağlardan erişime izin ver** seçeneğini etkinleştirin. **Özel durumlar** listesi altında, **Güvenilen Microsoft hizmetlerinin bu hizmet veri yoluna erişmesine izin ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

4. IoT Hub kaynak sayfasında **ileti yönlendirme** sekmesi ' ne gidin.

5. **Özel uç noktalar** bölümüne gidin ve **Ekle**' ye tıklayın. Uç nokta türü olarak **Service Bus kuyruğu** veya **Service Bus konusunu** (geçerli olduğu gibi) seçin.

6. Görüntülenen sayfada uç noktanız için bir ad sağlayın, Service Bus ' ad alanı ve kuyruk veya konu başlığını (uygun olduğu gibi) seçin. **Oluştur** düğmesine tıklayın.

Artık özel Service Bus uç noktanız, hub 'ın sistem tarafından atanan kimliğini kullanacak şekilde ayarlanmıştır ve güvenlik duvarı kısıtlamalarına rağmen Service Bus kaynağına erişme izni vardır. Artık bu uç noktayı bir yönlendirme kuralı ayarlamak için kullanabilirsiniz.

### <a name="egress-connectivity-to-storage-accounts-for-file-upload"></a>Karşıya dosya yükleme için depolama hesaplarına çıkış bağlantısı

IoT Hub dosya karşıya yükleme özelliği, cihazların müşteriye ait bir depolama hesabına dosya yüklemesine olanak tanır. Karşıya dosya yüklemeye izin vermek için, hem cihazların hem de IoT Hub depolama hesabına bağlantısı olması gerekir. Depolama hesabında güvenlik duvarı kısıtlamaları varsa, cihazların bağlantı kazanmak için desteklenen depolama hesabının mekanizmasından ( [Özel uç noktalar](../private-link/create-private-endpoint-storage-portal.md), [hizmet uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md)veya [doğrudan güvenlik duvarı yapılandırması](../storage/common/storage-network-security.md)dahil) birini kullanması gerekir. Benzer şekilde, depolama hesabında güvenlik duvarı kısıtlamaları varsa, IoT Hub, güvenilen Microsoft Hizmetleri özel durumu aracılığıyla depolama kaynağına erişmek üzere yapılandırılmalıdır. Bu amaçla, IoT Hub yönetilen bir kimliğe sahip olmalıdır. Yönetilen bir kimlik sağlandıktan sonra, depolama hesabınıza erişmek için hub 'ın kaynak kimliğine RBAC izni vermek üzere aşağıdaki adımları izleyin.

1. Azure portal, depolama hesabınızın **erişim denetimi (IAM)** sekmesine gidin ve **rol ataması Ekle** bölümünde **Ekle** ' ye tıklayın.

2. **Rol**, **Azure AD kullanıcısı, Grup veya hizmet sorumlusu** tarafından, **erişim atama** ve açılır listede IoT Hub kaynak adını seçme olarak, **Depolama Blobu veri katılımcısı** ([*not* katılımcı veya depolama hesabı katılımcısı](../storage/common/storage-auth-aad-rbac-portal.md#rbac-roles-for-blobs-and-queues)) seçin. **Kaydet** düğmesine tıklayın.

3. Depolama hesabınızdaki **güvenlik duvarları ve sanal ağlar** sekmesine gidin ve **Seçili ağlardan erişime izin ver** seçeneğini etkinleştirin. **Özel durumlar** listesi altında, **Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

4. IoT Hub kaynak sayfasında **karşıya dosya yükleme** sekmesine gidin.

5. Görüntülenen sayfada, blob depoınızda kullanmayı düşündüğünüz kapsayıcıyı seçin, **Dosya bildirim ayarlarını**, **SAS TTL**, **varsayılan TTL**ve **en fazla teslim sayısını** istediğiniz şekilde yapılandırın. Depolama uç noktanıza **kimlik doğrulama türü** olarak **atanan sistem** ' i seçin. **Oluştur** düğmesine tıklayın.

Şimdi karşıya dosya yükleme için depolama uç noktanız, hub 'ın sistem tarafından atanan kimliğini kullanacak şekilde ayarlanır ve güvenlik duvarı kısıtlamalarına rağmen depolama kaynağına erişme izni vardır.

### <a name="egress-connectivity-to-storage-accounts-for-bulk-device-importexport"></a>Toplu cihaz içeri/dışarı aktarma için depolama hesaplarına çıkış bağlantısı

IoT Hub, cihaz bilgilerini müşteri tarafından sunulan bir depolama blobundan toplu olarak [içeri/dışarı aktarma](./iot-hub-bulk-identity-mgmt.md) işlevlerini destekler. Toplu içeri/dışarı aktarma özelliğinin işlevine izin vermek için, hem cihazların hem de IoT Hub depolama hesabına bağlantısı olması gerekir.

Bu işlevsellik IoT Hub depolama hesabına bağlantı gerektirir. Güvenlik duvarı kısıtlamaları varken bir Service Bus kaynağına erişmek için, IoT Hub yönetilen bir kimliğe sahip olması gerekir. Yönetilen bir kimlik sağlandıktan sonra, hizmet veri yoluna erişmek için hub 'ın kaynak kimliğine RBAC izni vermek üzere aşağıdaki adımları izleyin.

1. Azure portal, depolama hesabınızın **erişim denetimi (IAM)** sekmesine gidin ve **rol ataması Ekle** bölümünde **Ekle** ' ye tıklayın.

2. **Rol**, **Azure AD kullanıcısı, Grup veya hizmet sorumlusu** tarafından, **erişim atama** ve açılır listede IoT Hub kaynak adını seçme olarak, **Depolama Blobu veri katılımcısı** ([*not* katılımcı veya depolama hesabı katılımcısı](../storage/common/storage-auth-aad-rbac-portal.md#rbac-roles-for-blobs-and-queues)) seçin. **Kaydet** düğmesine tıklayın.

3. Depolama hesabınızdaki **güvenlik duvarları ve sanal ağlar** sekmesine gidin ve **Seçili ağlardan erişime izin ver** seçeneğini etkinleştirin. **Özel durumlar** listesi altında, **Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

Artık toplu içeri/dışarı aktarma işlevini kullanma hakkında bilgi için [içeri aktarma dışarı aktarma işleri oluşturmak](https://docs.microsoft.com/rest/api/iothub/service/jobclient/getimportexportjobs) üzere Azure IoT REST API 'lerini kullanabilirsiniz. `storageAuthenticationType="identityBased"` `inputBlobContainerUri="https://..."` `outputBlobContainerUri="https://..."` Depolama hesabınızın giriş ve çıkış URL 'leri sırasıyla, istek gövdesine ve kullanmanız gerekir.

Azure IoT Hub SDK 'Ları Ayrıca hizmet istemcisinin kayıt defteri yöneticisinde bu işlevselliği destekler. Aşağıdaki kod parçacığında, C# SDK 'SıNı kullanarak bir içeri aktarma işinin veya dışarı aktarma işinin nasıl başlatılacağı gösterilmektedir.

```csharp
// Call an import job on the IoT Hub
JobProperties importJob = 
await registryManager.ImportDevicesAsync(
  JobProperties.CreateForImportJob(inputBlobContainerUri, outputBlobContainerUri, null, StorageAuthenticationType.IdentityBased), 
  cancellationToken);

// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = 
await registryManager.ExportDevicesAsync(
    JobProperties.CreateForExportJob(outputBlobContainerUri, true, null, StorageAuthenticationType.IdentityBased),
    cancellationToken);
```

C#, Java ve Node.js için sanal ağ desteğiyle Azure IoT SDK 'larının bu sürümünü kullanmak için:

1. Adlı bir ortam değişkeni oluşturun `EnableStorageIdentity` ve değerini olarak ayarlayın `1` .

2. SDK 'yı indirin: [Java](https://aka.ms/vnetjavasdk)  |  [C#](https://aka.ms/vnetcsharpsdk)  |  [Node.js](https://aka.ms/vnetnodesdk)
 
Python için, GitHub 'dan sınırlı sürümümüzü indirin.

1. [GitHub sürüm sayfasına](https://aka.ms/vnetpythonsdk)gidin.

2. Aşağıdaki dosyayı indirin ve bu, yayın sayfasının alt kısmında bulunan **varlıklar**adlı başlık altında bulabilirsiniz.
    > *azure_iot_hub-2.2.0_limited-PY2. PY3-None-Any. WHL*

3. Bir Terminal açın ve indirilen dosyanın bulunduğu klasöre gidin.

4. Sanal ağlar desteğiyle Python hizmeti SDK 'sını yüklemek için aşağıdaki komutu çalıştırın:
    > PIP install./azure_iot_hub-2.2.0_limited-PY2. PY3-None-Any. WHL


## <a name="next-steps"></a>Sonraki adımlar

IoT Hub özellikler hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

* [İleti yönlendirme](./iot-hub-devguide-messages-d2c.md)
* [Karşıya dosya yükleme](./iot-hub-devguide-file-upload.md)
* [Toplu cihaz içeri/dışarı aktarma](./iot-hub-bulk-identity-mgmt.md) 

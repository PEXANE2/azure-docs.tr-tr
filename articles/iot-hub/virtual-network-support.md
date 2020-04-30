---
title: Sanal ağlar için Azure IoT Hub desteği
description: IoT Hub ile sanal ağlar bağlantı modelini kullanma
services: iot-hub
author: jlian
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 04/28/2020
ms.author: jlian
ms.openlocfilehash: c0d01ae6507864373a79282476846d6f96adf83b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82231450"
---
# <a name="iot-hub-support-for-virtual-networks"></a>Sanal ağlar için IoT Hub desteği

Bu makalede, VNET bağlantı deseninin yanı sıra, bir IoT Hub 'ına müşterinin sahip olduğu bir Azure VNET aracılığıyla özel bir bağlantı deneyimi ayarlama hakkında elaborates açıklanır.

> [!NOTE]
> Bu makalede açıklanan IoT Hub Özellikler Şu anda şu bölgelerde [yönetilen hizmet kimliğiyle oluşturulan](#create-an-iot-hub-with-managed-service-identity) IoT Hub 'ları için sunulmaktadır: Doğu ABD, Orta Güney ABD ve Batı ABD 2.


## <a name="introduction"></a>Giriş

Varsayılan olarak, IoT Hub ana bilgisayar adları, Internet üzerinden genel olarak yönlendirilebilir bir IP adresi olan genel bir uç noktaya eşlenir. Aşağıdaki çizimde gösterildiği gibi, bu IoT Hub genel uç noktası, farklı müşterilere ait olan hub 'lar arasında paylaşılır ve şirket içi ağların yanı sıra geniş alan ağlar üzerinde IoT cihazları tarafından erişilebilir.

[İleti yönlendirme](./iot-hub-devguide-messages-d2c.md), [dosya yükleme](./iot-hub-devguide-file-upload.md)ve [toplu cihaz içeri/dışarı aktarma](./iot-hub-bulk-identity-mgmt.md) gibi çeşitli IoT Hub özellikler, genel uç noktası üzerinden müşterinin sahip olduğu bir Azure kaynağına IoT Hub bağlantı gerektirir. Aşağıda gösterildiği gibi, bu bağlantı yolları IoT Hub çıkış trafiğini topluca müşteri kaynaklarına oluşturur.
![IoT Hub ortak uç nokta](./media/virtual-network-support/public-endpoint.png)


Birçok nedenden dolayı müşteriler, sahip oldukları ve işledikleri bir sanal ağ üzerinden Azure kaynaklarıyla (IoT Hub dahil) bağlantıyı kısıtlamak isteyebilir. Bu nedenlerden bazıları şunlardır:

* Genel Internet üzerinden merkezinize yönelik bağlantı pozlamasını önleyerek IoT Hub 'ınız için ağ düzeyi yalıtımıyla ek güvenlik katmanları ile tanışın.

* Verilerinizin ve trafiğinizin doğrudan Azure omurga ağına aktarılmasını sağlamak için şirket içi ağ varlıklarınızdan özel bir bağlantı deneyimini etkinleştirme.

* Gizli şirket içi ağlarda gerçekleştirilen ayıklanma saldırılarını önleyin. 

* [Özel uç noktaları](../private-link/private-endpoint-overview.md)kullanan Azure genelindeki Azure genelinde bağlantı desenleri aşağıda verilmiştir.


Bu makalede, IoT Hub diğer Azure kaynaklarına çıkış bağlantısı için Azure güvenilir ilk taraf hizmetleri özel durumu kullanılarak bu hedeflere IoT Hub giriş bağlantısı için [Özel uç noktalar](../private-link/private-endpoint-overview.md) kullanılarak nasıl elde edileceğini açıklanmaktadır.


## <a name="ingress-connectivity-to-iot-hub-using-private-endpoints"></a>Özel uç noktaları kullanarak IoT Hub giriş bağlantısı

Özel uç nokta, bir Azure kaynağına erişilebilen, müşterinin sahip olduğu VNET içinde ayrılmış özel bir IP adresidir. IoT Hub 'ınız için özel bir uç nokta sunarak, VNET 'iniz içinde çalışan hizmetlerin, trafiğin IoT Hub genel uç noktasına gönderilmesine gerek kalmadan IoT Hub ulaşmasına izin verebilirsiniz. Benzer şekilde, şirket içi şirketinizde çalışan cihazlar, Azure 'daki sanal ağınıza ve sonrasında IoT Hub (Özel uç noktası aracılığıyla) ile bağlantı kazanmak için [sanal özel ağ (VPN)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) veya [ExpressRoute](https://azure.microsoft.com/services/expressroute/) özel eşlemesini kullanabilir. Sonuç olarak, IoT Hub 'ının genel uç noktalarına bağlantıyı kısıtlamak (veya muhtemelen tamamen devre dışı bırakmak) isteyen müşteriler, Özel uç nokta kullanarak hub 'larıyla bağlantı tutarken [IoT Hub güvenlik duvarı kurallarını](./iot-hub-ip-filtering.md) kullanarak bu amaca ulaşabilmektedir.

> [!NOTE]
> Bu kurulumun ana odağı, şirket içi bir ağ içindeki cihazlar içindir. Bu kurulum, geniş alan ağlarda dağıtılan cihazlar için önerilmez.

![IoT Hub ortak uç nokta](./media/virtual-network-support/virtual-network-ingress.png)

Devam etmeden önce aşağıdaki önkoşulların karşılandığından emin olun:

* IoT Hub 'ınızın [yönetilen hizmet kimliğiyle](#create-an-iot-hub-with-managed-service-identity)sağlanması gerekir.

* IoT Hub 'ınızın [desteklenen bölgelerden](#regional-availability-private-endpoints)birinde sağlanması gerekir.

* Özel uç noktanın oluşturulacağı bir alt ağa sahip bir Azure VNET sağladınız. Daha fazla ayrıntı için bkz. [Azure CLI kullanarak sanal ağ oluşturma](../virtual-network/quick-create-cli.md) .

* Şirket içi ağların içinde çalışan cihazlarda [sanal özel ağ (VPN)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) veya [ExpressRoute](https://azure.microsoft.com/services/expressroute/) özel eşlemesini Azure VNET 'iniz ile ayarlayın.


### <a name="regional-availability-private-endpoints"></a>Bölgesel kullanılabilirlik (Özel uç noktalar)

Aşağıdaki bölgelerde oluşturulan IoT Hub desteklenen özel uç noktalar:

* Doğu ABD

* Orta Güney ABD

* Batı ABD 2


### <a name="set-up-a-private-endpoint-for-iot-hub-ingress"></a>IoT Hub giriş için özel bir uç nokta ayarlama

Özel bir uç nokta ayarlamak için şu adımları izleyin:

1. Azure IoT Hub sağlayıcısı 'nı aboneliğinizle yeniden kaydettirmek için aşağıdaki Azure CLı komutunu çalıştırın:

    ```azurecli-interactive
    az provider register --namespace Microsoft.Devices --wait --subscription  <subscription-name>
    ```

2. IoT Hub portalınızdaki **Özel uç nokta bağlantıları** sekmesine gidin (Bu sekme yalnızca [desteklenen bölgelerde](#regional-availability-private-endpoints)IoT Hub 'larda mevcuttur) ve yeni bir özel uç nokta eklemek için **+** imzala ' ya tıklayın.

3. Yeni özel bitiş noktasını oluşturmak için abonelik, kaynak grubu, ad ve bölge sağlayın (ideal olarak, Özel uç noktanın hub 'ınız ile aynı bölgede oluşturulması gerekir; daha fazla ayrıntı için [Bölgesel kullanılabilirlik bölümüne](#regional-availability-private-endpoints) bakın).

4. **İleri: kaynak**' a tıklayın ve IoT Hub kaynağınız için abonelik sağlayın ve kaynak türü olarak **"Microsoft. Devices/iothubs"** , **kaynak**olarak IoT Hub adı ve hedef alt kaynak olarak **ıothub** seçeneğini belirleyin.

5. **İleri** ' ye tıklayın ve içinde özel uç nokta oluşturmak için Sanal ağınızı ve alt ağınızı sağlayın. İsterseniz Azure özel DNS bölgesi ile tümleştirme seçeneğini belirleyin.

6. **İleri: Etiketler**' e tıklayın ve isteğe bağlı olarak kaynağınız için Etiketler sağlayın.

7. Özel uç nokta kaynağını oluşturmak için **gözden geçir + oluştur** ' a tıklayın.


### <a name="pricing-private-endpoints"></a>Fiyatlandırma (Özel uç noktalar)

Fiyatlandırma ayrıntıları için bkz. [Azure özel bağlantı fiyatlandırması](https://azure.microsoft.com/pricing/details/private-link).


## <a name="egress-connectivity-from-iot-hub-to-other-azure-resources"></a>IoT Hub diğer Azure kaynaklarına çıkış bağlantısı

IoT Hub Azure Blob depolama, Olay Hub 'larınız, [ileti yönlendirme](./iot-hub-devguide-messages-d2c.md)için hizmet veri yolu kaynakları, [dosya yükleme](./iot-hub-devguide-file-upload.md)ve tipik olarak kaynakların genel uç noktası üzerinden gerçekleşen [toplu cihaz içeri/dışarı aktarma](./iot-hub-bulk-identity-mgmt.md)için erişim gerektirir. Depolama hesabınızı, Olay Hub 'larını veya hizmet veri yolu kaynağını bir sanal ağa bağladığınızda, tavsiye edilen yapılandırma kaynağa varsayılan olarak bağlantıyı engeller. Bu nedenle, bu kaynaklara erişim gerektiren IoT Hub işlevselliğini engeller.

Bu durumu hafifetmek için, **Azure ilk taraf güvenilen hizmetler** seçeneği aracılığıyla IoT Hub kaynağınızın depolama hesabınıza, Olay Hub 'larına veya hizmet veri yolu kaynaklarına bağlantıyı etkinleştirmeniz gerekir.

Önkoşullar aşağıdaki gibidir:

* IoT Hub 'ınızın [desteklenen bölgelerden](#regional-availability-trusted-microsoft-first-party-services)birinde sağlanması gerekir.

* IoT Hub, hub sağlama zamanında bir yönetilen hizmet kimliği atanmalıdır. [Yönetilen hizmet kimliğiyle hub oluşturma](#create-an-iot-hub-with-managed-service-identity)yönergesini izleyin.


### <a name="regional-availability-trusted-microsoft-first-party-services"></a>Bölgesel kullanılabilirlik (güvenilen Microsoft ilk taraf hizmetleri)

Azure güvenilir ilk taraf hizmetleri özel durumu Azure depolama 'ya yönelik güvenlik duvarı kısıtlamalarını atlamak için, Olay Hub 'ları ve hizmet veri yolu kaynakları yalnızca aşağıdaki bölgelerde IoT Hub 'Lar için desteklenir:

* Doğu ABD

* Orta Güney ABD

* Batı ABD 2


### <a name="pricing-trusted-microsoft-first-party-services"></a>Fiyatlandırma (güvenilen Microsoft ilk taraf hizmetleri)

Güvenilen Microsoft ilk taraf hizmetleri özel durum özelliği, [desteklenen bölgelerde](#regional-availability-trusted-microsoft-first-party-services)IoT Hub 'larda ücretsizdir. Sağlanan depolama hesapları, Olay Hub 'ları veya hizmet veri yolu kaynakları için ücretler ayrıca geçerlidir.


### <a name="create-an-iot-hub-with-managed-service-identity"></a>Yönetilen hizmet kimliğiyle IoT Hub 'ı oluşturma

Bir yönetilen hizmet kimliği, kaynak sağlama sırasında Hub 'ınıza atanabilir (Bu özellik şu anda mevcut hub 'lar için desteklenmemektedir) ve IoT Hub 'ının en düşük sürüm olarak TLS 1,2 kullanmasını gerektirir. Bu amaçla, aşağıdaki ARM kaynak şablonunu kullanmanız gerekir:

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
      "properties": {
        "minTlsVersion": "1.2"
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
              "properties": {
                "minTlsVersion": "1.2"
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

Kaynağınızın `name` `location`değerlerini değiştirdikten sonra, `SKU.name` ve `SKU.tier`kullanarak kaynağı var olan bir kaynak grubuna dağıtmak için Azure CLI kullanabilirsiniz:

```azurecli-interactive
az deployment group create --name <deployment-name> --resource-group <resource-group-name> --template-file <template-file.json>
```

Kaynak oluşturulduktan sonra, Azure CLı kullanarak hub 'ınıza atanan yönetilen hizmet kimliğini alabilirsiniz:

```azurecli-interactive
az resource show --resource-type Microsoft.Devices/IotHubs --name <iot-hub-resource-name> --resource-group <resource-group-name>
```

Yönetilen hizmet kimliği ile IoT Hub sağlandığında, [depolama hesapları](#egress-connectivity-to-storage-account-endpoints-for-routing), [Olay Hub 'ları](#egress-connectivity-to-event-hubs-endpoints-for-routing)ve [hizmet veri yolu](#egress-connectivity-to-service-bus-endpoints-for-routing) kaynaklarına yönlendirme uç noktaları ayarlamak veya [karşıya dosya yükleme](#egress-connectivity-to-storage-accounts-for-file-upload) ve [toplu cihaz içeri/dışarı aktarma](#egress-connectivity-to-storage-accounts-for-bulk-device-importexport)'yı yapılandırmak için ilgili bölümü izleyin.


### <a name="egress-connectivity-to-storage-account-endpoints-for-routing"></a>Yönlendirme için depolama hesabı uç noktalarına giden çıkış bağlantısı

IoT Hub, iletileri müşterinin sahip olduğu bir depolama hesabına yönlendirmek üzere yapılandırılabilir. Güvenlik duvarı kısıtlamaları gerçekleştiği sırada yönlendirme işlevselliğinin bir depolama hesabına erişmesine izin vermek için, IoT Hub yönetilen bir hizmet kimliğine sahip olması gerekir (bkz. [yönetilen hizmet kimliğiyle hub oluşturma](#create-an-iot-hub-with-managed-service-identity)). Yönetilen bir hizmet kimliği sağlandıktan sonra, depolama hesabınıza erişmek için hub 'ın kaynak kimliğine RBAC izni vermek üzere aşağıdaki adımları izleyin.

1. Azure portal, depolama hesabınızın **erişim denetimi (IAM)** sekmesine gidin ve **rol ataması Ekle** bölümünde **Ekle** ' ye tıklayın.

2. **Erişim atama** olarak bir **rol**, **Azure AD kullanıcısı, Grup veya hizmet sorumlusu** olarak **Depolama Blobu verileri katılımcısı** ' nı seçin ve açılan listeden IoT Hub kaynak adını seçin. **Kaydet** düğmesine tıklayın.

3. Depolama hesabınızdaki **güvenlik duvarları ve sanal ağlar** sekmesine gidin ve **Seçili ağlardan erişime izin ver** seçeneğini etkinleştirin. **Özel durumlar** listesi altında, **Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

4. IoT Hub kaynak sayfasında **ileti yönlendirme** sekmesi ' ne gidin.

5. **Özel uç noktalar** bölümüne gidin ve **Ekle**' ye tıklayın. Uç nokta türü olarak **depolama** ' yı seçin.

6. Görüntülenen sayfada uç noktanız için bir ad sağlayın, blob depoınızda kullanmayı düşündüğünüz kapsayıcıyı seçin, kodlama ve dosya adı biçimini belirtin. Depolama uç noktanıza **kimlik doğrulama türü** olarak **atanan sistem** ' i seçin. **Oluştur** düğmesine tıklayın.

Artık özel depolama uç noktanız, hub 'ın sistem tarafından atanan kimliğini kullanacak şekilde ayarlanır ve güvenlik duvarı kısıtlamalarına rağmen depolama kaynağına erişme izni vardır. Artık bu uç noktayı bir yönlendirme kuralı ayarlamak için kullanabilirsiniz.


### <a name="egress-connectivity-to-event-hubs-endpoints-for-routing"></a>Yönlendirme için Olay Hub 'ları uç noktalarına giden bağlantı

IoT Hub, iletileri müşteriye ait bir olay hub 'ı ad alanına yönlendirmek üzere yapılandırılabilir. Güvenlik duvarı kısıtlamaları yerinde yönlendirme işlevselliğinin bir olay hub 'ına erişmesine izin vermek için, IoT Hub yönetilen bir hizmet kimliğine sahip olması gerekir (bkz. [yönetilen hizmet kimliğiyle hub oluşturma](#create-an-iot-hub-with-managed-service-identity)). Yönetilen bir hizmet kimliği sağlandıktan sonra, kuruluşunuzun kaynak kimliğine, Olay Hub 'ınıza erişim sağlamak için RBAC izni vermek üzere aşağıdaki adımları izleyin.

1. Azure portal, Olay Hub 'ları **erişim denetimi (IAM)** sekmesine gidin ve **rol ataması Ekle** bölümünde **Ekle** ' ye tıklayın.

2. **Erişim atama** olarak **Event Hubs veri göndereni** **rol**, **Azure AD kullanıcısı, Grup veya hizmet sorumlusu** olarak seçin ve açılan listeden IoT Hub kaynak adını seçin. **Kaydet** düğmesine tıklayın.

3. Olay Hub 'larınızdaki **güvenlik duvarları ve sanal ağlar** sekmesine gidin ve **Seçili ağlardan erişime izin ver** seçeneğini etkinleştirin. **Özel durumlar** listesi altında, **Güvenilen Microsoft hizmetlerinin Olay Hub 'Larına erişmesine izin ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

4. IoT Hub kaynak sayfasında **ileti yönlendirme** sekmesi ' ne gidin.

5. **Özel uç noktalar** bölümüne gidin ve **Ekle**' ye tıklayın. Uç nokta türü olarak **Olay Hub 'larını** seçin.

6. Görüntülenen sayfada uç noktanız için bir ad sağlayın, Olay Hub 'larını ve örneğinizi seçin ve **Oluştur** düğmesine tıklayın.

Artık özel olay hub 'larınız, hub 'ın sistem tarafından atanan kimliğini kullanacak şekilde ayarlanmıştır ve güvenlik duvarı kısıtlamalarına rağmen Olay Hub 'ınıza erişme iznine sahiptir. Artık bu uç noktayı bir yönlendirme kuralı ayarlamak için kullanabilirsiniz.


### <a name="egress-connectivity-to-service-bus-endpoints-for-routing"></a>Yönlendirme için Service Bus uç noktalarına çıkış bağlantısı

IoT Hub, iletileri müşteriye ait Service Bus ad alanına yönlendirmek üzere yapılandırılabilir. Güvenlik duvarı kısıtlamaları yerinde yönlendirme işlevselliğinin bir Service Bus kaynağına erişmesine izin vermek için, IoT Hub yönetilen bir hizmet kimliğine sahip olması gerekir (bkz. [yönetilen hizmet kimliğiyle hub oluşturma](#create-an-iot-hub-with-managed-service-identity)). Yönetilen bir hizmet kimliği sağlandıktan sonra, hizmet veri yoluna erişmek için hub 'ın kaynak kimliğine RBAC izni vermek üzere aşağıdaki adımları izleyin.

1. Azure portal, hizmet veri yolu **erişim denetimi (IAM)** sekmesine gidin ve **rol ataması Ekle** bölümünde **Ekle** ' ye tıklayın.

2. **Erişim atama** olarak **hizmet veri yolu verileri göndereni** **rol**, **Azure AD kullanıcısı, Grup veya hizmet sorumlusu** olarak seçin ve aşağı açılan listeden IoT Hub kaynak adını seçin. **Kaydet** düğmesine tıklayın.

3. Service Bus 'daki **güvenlik duvarları ve sanal ağlar** sekmesine gidin ve **Seçili ağlardan erişime izin ver** seçeneğini etkinleştirin. **Özel durumlar** listesi altında, **Güvenilen Microsoft hizmetlerinin bu hizmet veri yoluna erişmesine izin ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

4. IoT Hub kaynak sayfasında **ileti yönlendirme** sekmesi ' ne gidin.

5. **Özel uç noktalar** bölümüne gidin ve **Ekle**' ye tıklayın. Uç nokta türü olarak **Service Bus kuyruğu** veya **Service Bus konusunu** (geçerli olduğu gibi) seçin.

6. Görüntülenen sayfada uç noktanız için bir ad sağlayın, Service Bus ' ad alanı ve kuyruk veya konu başlığını (uygun olduğu gibi) seçin. **Oluştur** düğmesine tıklayın.

Artık özel Service Bus uç noktanız, hub 'ın sistem tarafından atanan kimliğini kullanacak şekilde ayarlanmıştır ve güvenlik duvarı kısıtlamalarına rağmen Service Bus kaynağına erişme izni vardır. Artık bu uç noktayı bir yönlendirme kuralı ayarlamak için kullanabilirsiniz.


### <a name="egress-connectivity-to-storage-accounts-for-file-upload"></a>Karşıya dosya yükleme için depolama hesaplarına çıkış bağlantısı

IoT Hub dosya karşıya yükleme özelliği, cihazların müşteriye ait bir depolama hesabına dosya yüklemesine olanak tanır. Karşıya dosya yüklemeye izin vermek için, hem cihazların hem de IoT Hub depolama hesabına bağlantısı olması gerekir. Depolama hesabında güvenlik duvarı kısıtlamaları varsa, cihazların bağlantı kazanmak için desteklenen depolama hesabı mekanizmasından ( [Özel uç noktalar](../private-link/create-private-endpoint-storage-portal.md), [hizmet uç](../virtual-network/virtual-network-service-endpoints-overview.md) noktaları veya [doğrudan güvenlik duvarı yapılandırması](../storage/common/storage-network-security.md)dahil) birini kullanması gerekir. Benzer şekilde, depolama hesabında güvenlik duvarı kısıtlamaları varsa, IoT Hub, güvenilen Microsoft Hizmetleri özel durumu aracılığıyla depolama kaynağına erişmek üzere yapılandırılmalıdır. Bu amaçla, IoT Hub yönetilen bir hizmet kimliğine sahip olmalıdır (bkz. [yönetilen hizmet kimliğiyle hub oluşturma](#create-an-iot-hub-with-managed-service-identity)). Yönetilen bir hizmet kimliği sağlandıktan sonra, depolama hesabınıza erişmek için hub 'ın kaynak kimliğine RBAC izni vermek üzere aşağıdaki adımları izleyin.

1. Azure portal, depolama hesabınızın **erişim denetimi (IAM)** sekmesine gidin ve **rol ataması Ekle** bölümünde **Ekle** ' ye tıklayın.

2. **Erişim atama** olarak bir **rol**, **Azure AD kullanıcısı, Grup veya hizmet sorumlusu** olarak **Depolama Blobu verileri katılımcısı** ' nı seçin ve açılan listeden IoT Hub kaynak adını seçin. **Kaydet** düğmesine tıklayın.

3. Depolama hesabınızdaki **güvenlik duvarları ve sanal ağlar** sekmesine gidin ve **Seçili ağlardan erişime izin ver** seçeneğini etkinleştirin. **Özel durumlar** listesi altında, **Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

4. IoT Hub kaynak sayfasında **karşıya dosya yükleme** sekmesine gidin.

5. Görüntülenen sayfada, blob depoınızda kullanmayı düşündüğünüz kapsayıcıyı seçin, **Dosya bildirim ayarlarını**, **SAS TTL**, **varsayılan TTL** ve **en yüksek teslimat sayısını** istediğiniz şekilde yapılandırın. Depolama uç noktanıza **kimlik doğrulama türü** olarak **atanan sistem** ' i seçin. **Oluştur** düğmesine tıklayın.

Şimdi karşıya dosya yükleme için depolama uç noktanız, hub 'ın sistem tarafından atanan kimliğini kullanacak şekilde ayarlanır ve güvenlik duvarı kısıtlamalarına rağmen depolama kaynağına erişme izni vardır.


### <a name="egress-connectivity-to-storage-accounts-for-bulk-device-importexport"></a>Toplu cihaz içeri/dışarı aktarma için depolama hesaplarına çıkış bağlantısı

IoT Hub, cihaz bilgilerini müşteri tarafından sunulan bir depolama blobundan toplu olarak [içeri/dışarı aktarma](./iot-hub-bulk-identity-mgmt.md) işlevlerini destekler. Toplu içeri/dışarı aktarma özelliğinin işlevine izin vermek için, hem cihazların hem de IoT Hub depolama hesabına bağlantısı olması gerekir.

Bu işlevsellik IoT Hub depolama hesabına bağlantı gerektirir. Güvenlik duvarı kısıtlamaları yerinde bir Service Bus kaynağına erişmek için, IoT Hub yönetilen bir hizmet kimliğine sahip olması gerekir (bkz. [yönetilen hizmet kimliğiyle hub oluşturma](#create-an-iot-hub-with-managed-service-identity)). Yönetilen bir hizmet kimliği sağlandıktan sonra, hizmet veri yoluna erişmek için hub 'ın kaynak kimliğine RBAC izni vermek üzere aşağıdaki adımları izleyin.

1. Azure portal, depolama hesabınızın **erişim denetimi (IAM)** sekmesine gidin ve **rol ataması Ekle** bölümünde **Ekle** ' ye tıklayın.

2. **Erişim atama** olarak bir **rol**, **Azure AD kullanıcısı, Grup veya hizmet sorumlusu** olarak **Depolama Blobu verileri katılımcısı** ' nı seçin ve açılan listeden IoT Hub kaynak adını seçin. **Kaydet** düğmesine tıklayın.

3. Depolama hesabınızdaki **güvenlik duvarları ve sanal ağlar** sekmesine gidin ve **Seçili ağlardan erişime izin ver** seçeneğini etkinleştirin. **Özel durumlar** listesi altında, **Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

Toplu içeri/dışarı aktarma işlevini kullanma hakkında bilgi için, [içeri aktarma dışarı aktarma işleri oluşturmak](https://docs.microsoft.com/rest/api/iothub/service/jobclient/getimportexportjobs) üzere Azure IoT REST API 'yi kullanabilirsiniz. İstek gövdesini ve kullanmanız `storageAuthenticationType="identityBased"` `inputBlobContainerUri="https://..."` `outputBlobContainerUri="https://..."` gerektiğini, sırasıyla depolama hesabınızın giriş ve çıkış URL 'sini belirtmeniz gerektiğini unutmayın.


Azure IoT Hub SDK 'Sı Ayrıca hizmet istemcisinin kayıt defteri yöneticisinde bu işlevselliği destekler. Aşağıdaki kod parçacığında, C# SDK 'SıNı kullanarak bir içeri aktarma işinin veya dışarı aktarma işinin nasıl başlatılacağı gösterilmektedir.

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


C#, Java ve Node. js için sanal ağ desteğiyle Azure IoT SDK 'larının bu bölge sınırlı sürümünü kullanmak için:

1. Adlı `EnableStorageIdentity` bir ortam değişkeni oluşturun ve değerini olarak `1`ayarlayın.

2. SDK 'yı indirin: [Java](https://aka.ms/vnetjavasdk) | [C#](https://aka.ms/vnetcsharpsdk) | [Node. js](https://aka.ms/vnetnodesdk)
 
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

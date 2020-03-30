---
title: Sanal ağlar için Azure IoT Hub desteği
description: IoT Hub ile sanal ağlar bağlantı deseni nasıl kullanılır?
services: iot-hub
author: rezasherafat
ms.service: iot-fundamentals
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: rezas
ms.openlocfilehash: 34f66c13b0e7eb7092332a48744f9abfd8f0db80
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79501428"
---
# <a name="iot-hub-support-for-virtual-networks"></a>Sanal ağlar için IoT Hub desteği

Bu makalede, VNET bağlantı deseni tanıtılıyor ve müşteriye ait bir Azure VNET aracılığıyla bir IoT hub'ına nasıl özel bir bağlantı deneyimi ayarlanış edileceği ayrıntılı olarak açıklanmaktadır.

> [!NOTE]
> Bu makalede açıklanan IoT Hub özellikleri şu anda aşağıdaki bölgelerde [yönetilen hizmet kimliğiyle oluşturulan](#create-an-iot-hub-with-managed-service-identity) IoT hub'ları tarafından kullanılabilir: Doğu ABD, Güney Orta ABD ve Batı ABD 2.


## <a name="introduction"></a>Giriş

Varsayılan olarak, IoT Hub ana bilgisayar adları, Internet üzerinden herkese açık bir IP adresiyle ortak bir bitiş noktasına eşleninir. Aşağıdaki resimde gösterildiği gibi, bu IoT Hub ortak bitiş noktası farklı müşterilere ait hub'lar arasında paylaşılır ve ioT aygıtları tarafından geniş alan ağları nın yanı sıra şirket içi ağlar üzerinden erişilebilir.

[İleti yönlendirme,](./iot-hub-devguide-messages-d2c.md) [dosya yükleme](./iot-hub-devguide-file-upload.md)ve toplu aygıt [alma/dışa aktarma](./iot-hub-bulk-identity-mgmt.md) gibi çeşitli IoT Hub özellikleri, ioT Hub'dan müşteriye ait bir Azure kaynağına genel bitiş noktası üzerinden bağlantı gerektirir. Aşağıda gösterildiği gibi, bu bağlantı yolları topluca IoT Hub'dan müşteri kaynaklarına çıkış trafiğini oluşturur.
![IoT Hub genel bitiş noktası](./media/virtual-network-support/public-endpoint.png)


Müşteriler çeşitli nedenlerden dolayı, sahip oldukları ve işlettikleri bir VNET aracılığıyla Azure kaynaklarına (IoT Hub dahil) bağlantı kısıtlamak isteyebilir. Bu nedenler şunlardır:

* Ortak Internet üzerinden hub'ınıza bağlantı maruziyetini önleyerek IoT hub'ınız için ağ düzeyi yalıtımı yoluyla ek güvenlik katmanları tanıtın.

* Şirket içi ağ varlıklarınızdan verilerinizin ve trafiğinizin doğrudan Azure omurga ağına iletilmesini sağlayan özel bir bağlantı deneyimi etkinleştirme.

* Hassas şirket içi ağlardan sızma saldırılarını önleme. 

* [Özel uç noktaları](../private-link/private-endpoint-overview.md)kullanarak azure çapında bağlantı desenleri oluşturuldu.


Bu makalede, IoT Hub'a giriş bağlantısı için [özel uç noktaları](../private-link/private-endpoint-overview.md) kullanarak bu hedeflere nasıl ulaşılabildiğinizaçıkolarak, IoT Hub'dan diğer Azure kaynaklarına çıkış bağlantısı için Azure güvenilen ilk taraf hizmetleri özel durumu olarak.


## <a name="ingress-connectivity-to-iot-hub-using-private-endpoints"></a>Özel uç noktaları kullanarak IoT Hub'a giriş bağlantısı

Özel bitiş noktası, azure kaynağına ulaşılabilen müşteriye ait bir VNET içinde ayrılan özel bir IP adresidir. IoT hub'ınız için özel bir bitiş noktasına sahip olarak, Trafiğin IoT Hub'ın genel bitiş noktasına gönderilmesine gerek kalmadan VNET'inizin içinde çalışan hizmetlerin IoT Hub'ına ulaşmasına izin verebilirsiniz. Benzer şekilde, şirket içinde çalışan aygıtlar, Azure'daki VNET'inize ve daha sonra IoT Hub'ınıza (özel bitiş noktası üzerinden) bağlantı sağlamak için [Sanal Özel Ağ (VPN)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) veya [ExpressRoute](https://azure.microsoft.com/services/expressroute/) Private Peering'i kullanabilir. Sonuç olarak, bağlantılarını IoT hub'larının ortak uç noktalarıyla sınırlamak (veya büyük olasılıkla tamamen engellemek) isteyen müşteriler, özel bitiş noktasını kullanarak Hub'larına bağlantı sağlarken [IoT Hub güvenlik duvarı kurallarını](./iot-hub-ip-filtering.md) kullanarak bu hedefe ulaşabilirler.

> [!NOTE]
> Bu kurulumun ana odak noktası, şirket içi ağ içindeki aygıtlar içindir. Bu kurulum, geniş alanağında dağıtılan aygıtlar için tavsiye edilmez.

![IoT Hub genel bitiş noktası](./media/virtual-network-support/virtual-network-ingress.png)

Devam etmeden önce aşağıdaki ön koşulların yerine getirildiğinden emin olun:

* IoT hub'ınız yönetilen [hizmet kimliğiyle](#create-an-iot-hub-with-managed-service-identity)sağlanmalıdır.

* IoT hub'ınız [desteklenen bölgelerden](#regional-availability-private-endpoints)birinde sağlanmalıdır.

* Özel bitiş noktasının oluşturulacağı bir alt net içeren bir Azure VNET'i saysanız. Daha fazla ayrıntı için [Azure CLI'yi kullanarak sanal ağ oluşturma](../virtual-network/quick-create-cli.md) bilgisine bakın.

* Şirket içi ağlarda çalışan aygıtlar için Azure VNET'inize [sanal özel ağ (VPN)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) veya [ExpressRoute](https://azure.microsoft.com/services/expressroute/) özel bakışları ayarlayın.


### <a name="regional-availability-private-endpoints"></a>Bölgesel kullanılabilirlik (özel uç noktalar)

Aşağıdaki bölgelerde oluşturulan IoT Hub'ınözel uç noktaları desteklenir:

* Doğu ABD

* Orta Güney ABD

* Batı ABD 2


### <a name="set-up-a-private-endpoint-for-iot-hub-ingress"></a>IoT Hub girişi için özel bir bitiş noktası ayarlama

Özel bir bitiş noktası ayarlamak için aşağıdaki adımları izleyin:

1. Azure IoT Hub sağlayıcısını aboneliğinizle yeniden kaydetmek için aşağıdaki Azure CLI komutunu çalıştırın:

    ```azurecli-interactive
    az provider register --namespace Microsoft.Devices --wait --subscription  <subscription-name>
    ```

2. IoT Hub portalınızdaki **Özel uç nokta bağlantıları** sekmesine gidin (bu sekme yalnızca desteklenen [bölgelerdeki](#regional-availability-private-endpoints)IoT **+** Hub'larında kullanılabilir) ve yeni bir özel bitiş noktası eklemek için işareti tıklatın.

3. Yeni özel bitiş noktasını oluşturmak için abonelik, kaynak grubu, ad ve bölge sağlayın (ideal olarak, hub'ınızla aynı bölgede özel bitiş noktası oluşturulmalıdır; daha fazla ayrıntı için [bölgesel kullanılabilirlik bölümüne](#regional-availability-private-endpoints) bakın).

4. **Sonraki'** yi tıklatın ve IoT Hub kaynağınızın aboneliğini sağlayın ve kaynak türü olarak **"Microsoft.Devices/IotHubs"ı,** **kaynak**olarak IoT Hub adınız ve hedef alt kaynak olarak **iotHub'ı** seçin.

5. **Sonraki'yi tıklatın: Yapılandırma** ve özel bitiş noktası oluşturmak için sanal ağ ve alt ağı sağlayın. İstenirseniz Azure özel DNS bölgesiyle tümleştirme seçeneğini seçin.

6. **Sonraki'yi tıklatın: Etiketler**ve isteğe bağlı olarak kaynağınız için etiketler sağlayın.

7. Özel bitiş noktası kaynağınızı oluşturmak için **Gözden Geçir + oluştur'u** tıklatın.


### <a name="pricing-private-endpoints"></a>Fiyatlandırma (özel uç noktalar)

Fiyatlandırma ayrıntıları için Azure [Özel Bağlantı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/private-link)bakın.


## <a name="egress-connectivity-from-iot-hub-to-other-azure-resources"></a>IoT Hub'dan diğer Azure kaynaklarına çıkış bağlantısı

IoT Hub'ın Azure blob depolama alanınıza, etkinlik hub'larınıza, [ileti yönlendirme,](./iot-hub-devguide-messages-d2c.md) [dosya yükleme](./iot-hub-devguide-file-upload.md)ve genellikle kaynakların genel bitiş noktası üzerinden gerçekleşen toplu [aygıt alma/dışa aktarma](./iot-hub-bulk-identity-mgmt.md)için servis veri aracı kaynaklarına erişmesi gerekir. Depolama hesabınızı, olay hub'larınızı veya servis veri yolu kaynağınızı bir VNET'e bağlamanız durumunda, tavsiye edilen yapılandırma varsayılan olarak kaynağa bağlantının engellenmesini sağlar. Sonuç olarak, bu, IoT Hub'ın bu kaynaklara erişim gerektiren işlevselliğini engeller.

Bu durumu hafifletmek için, IoT Hub kaynağınızdan depolama hesabınıza, etkinlik hub'larınıza veya hizmet veri veri günü kaynaklarına **Azure birinci taraf güvenilir hizmetler** seçeneği aracılığıyla bağlantı sağlamanız gerekir.

Önkoşullar aşağıdaki gibidir:

* IoT hub'ınız [desteklenen bölgelerden](#regional-availability-trusted-microsoft-first-party-services)birinde sağlanmalıdır.

* IoT Hub'ınız, hub sağlama zamanında yönetilen bir hizmet kimliği atanmalıdır. [Yönetilen hizmet kimliğine sahip bir hub oluşturma](#create-an-iot-hub-with-managed-service-identity)yönergesini izleyin.


### <a name="regional-availability-trusted-microsoft-first-party-services"></a>Bölgesel kullanılabilirlik (güvenilir Microsoft birinci taraf hizmetleri)

Azure depolama, etkinlik hub'ları ve servis veri aracı kaynaklarına yönelik güvenlik duvarı kısıtlamalarını atlamak için azure'a güvenilen ilk taraf hizmetleri istisnası yalnızca aşağıdaki bölgelerdeki IoT Hub'ları için desteklenir:

* Doğu ABD

* Orta Güney ABD

* Batı ABD 2


### <a name="pricing-trusted-microsoft-first-party-services"></a>Fiyatlandırma (güvenilir Microsoft birinci taraf hizmetleri)

Güvenilen Microsoft birinci taraf hizmetleri özel durumu, [desteklenen bölgelerdeki](#regional-availability-trusted-microsoft-first-party-services)IoT Hub'larında ücretsizdir. Sağlanan depolama hesapları, olay merkezleri veya servis veri günü kaynakları için ücretler ayrı olarak uygulanır.


### <a name="create-an-iot-hub-with-managed-service-identity"></a>Yönetilen hizmet kimliğine sahip bir IoT hub'ı oluşturma

Yönetilen bir hizmet kimliği kaynak sağlama zamanında hub'ınıza atanabilir (bu özellik şu anda varolan hub'lar için desteklenmez), bu da IoT hub'ın tls 1.2'yi minimum sürüm olarak kullanmasını gerektirir. Bu amaçla, aşağıdaki ARM kaynak şablonunu kullanmanız gerekir:

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

Kaynağınızın `name` `location` `SKU.name` değerlerini (ve `SKU.tier`, kaynağınızın) yerine geçtikten sonra, kaynağı aşağıdakileri kullanarak varolan bir kaynak grubunda dağıtmak için Azure CLI'yi kullanabilirsiniz:

```azurecli-interactive
az group deployment create --name <deployment-name> --resource-group <resource-group-name> --template-file <template-file.json>
```

Kaynak oluşturulduktan sonra, Azure CLI'yi kullanarak merkezinize atanan yönetilen hizmet kimliğini alabilirsiniz:

```azurecli-interactive
az resource show --resource-type Microsoft.Devices/IotHubs --name <iot-hub-resource-name> --resource-group <resource-group-name>
```

Yönetilen hizmet kimliğine sahip IoT Hub'ı sağlandıktan sonra, [depolama hesaplarına,](#egress-connectivity-to-storage-account-endpoints-for-routing) [olay hub'larına](#egress-connectivity-to-event-hubs-endpoints-for-routing)ve [servis veri yolu](#egress-connectivity-to-service-bus-endpoints-for-routing) kaynaklarına yönlendirme uç noktaları ayarlamak veya dosya [yükleme](#egress-connectivity-to-storage-accounts-for-file-upload) ve toplu [aygıt alma/dışa](#egress-connectivity-to-storage-accounts-for-bulk-device-importexport)aktarma yapılandırmak için ilgili bölümü izleyin.


### <a name="egress-connectivity-to-storage-account-endpoints-for-routing"></a>Yönlendirme için depolama hesabı uç noktalarına çıkış bağlantısı

IoT Hub, iletileri müşteriye ait bir depolama hesabına yönlendirmek için yapılandırılabilir. Güvenlik duvarı kısıtlamaları yerindeyken yönlendirme işlevinin bir depolama hesabına erişmesine izin vermek için, IoT Hub'ınızın yönetilen bir hizmet kimliğine sahip olması gerekir [(yönetilen hizmet kimliğine sahip hub'ı nasıl oluşturabilirsiniz).](#create-an-iot-hub-with-managed-service-identity) Yönetilen bir hizmet kimliği sağlandıktan sonra, RBAC'a depolama hesabınıza erişmek için hub'ınızın kaynak kimliğine izin vermek için aşağıdaki adımları izleyin.

1. Azure portalında, depolama hesabınızın **Access denetimine (IAM)** gidin ve **rol atama ekle** bölümünün altına **Ekle'yi** tıklatın.

2. **Rol**olarak **Depolama Blob Veri** Katılımcısı'nı seçin , **Azure AD kullanıcısı, grubu veya hizmet sorumlusu** olarak erişim **atamave** açılan listede IoT Hub'ınızın kaynak adını seçin. **Kaydet** düğmesine tıklayın.

3. Depolama hesabınızdaki **Güvenlik Duvarları ve sanal ağlar** sekmesine gidin ve **seçili ağlardan erişime izin verme** seçeneğini etkinleştirin. Özel **Durumlar** listesinde, bu depolama hesabına erişmek için **güvenilen Microsoft hizmetlerine İzin Ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

4. IoT Hub'ınızın kaynak sayfasında İleti **yönlendirme** sekmesine gidin.

5. Özel **uç noktaları** bölümüne gidin ve **Ekle'yi**tıklatın. Bitiş noktası türü olarak **Depolama'yı** seçin.

6. Görünen sayfada, bitiş noktanız için bir ad sağlayın, blob depolama alanınızda kullanmak istediğiniz kapsayıcıyı seçin, kodlama sağlayın ve dosya adı biçimi sağlayın. Depolama bitiş noktanıza **Kimlik Doğrulama türü** olarak Atanan **Sistem'i** seçin. **Oluştur** düğmesine tıklayın.

Artık özel depolama bitiş noktanız hub'ınızın atanmış kimliğini kullanacak şekilde ayarlanmıştır ve güvenlik duvarı kısıtlamalarına rağmen depolama kaynağınıza erişim iznine sahiptir. Artık bir yönlendirme kuralı ayarlamak için bu bitiş noktasını kullanabilirsiniz.


### <a name="egress-connectivity-to-event-hubs-endpoints-for-routing"></a>Yönlendirme için olay hub'larının uç noktalarına çıkış bağlantısı

IoT Hub, iletileri müşteriye ait olay hub'larına yönlendirecek şekilde yapılandırılabilir. Güvenlik duvarı kısıtlamaları yerindeyken yönlendirme işlevinin bir olay hub'ları kaynağına erişmesine izin vermek için, IoT Hub'ınızın yönetilen bir hizmet kimliğine sahip olması gerekir [(yönetilen hizmet kimliğine sahip hub'ı nasıl oluşturabilirsiniz).](#create-an-iot-hub-with-managed-service-identity) Yönetilen bir hizmet kimliği sağlandıktan sonra, RBAC'a etkinlik hub'larınıza erişme izni vermek için aşağıdaki adımları izleyin.

1. Azure portalında, etkinlik hub'larınıza gidin **Access denetimi (IAM)** sekmesine gidin ve **rol atama ekle** bölümünün altına **Ekle'yi** tıklatın.

2. **Rol**olarak **Olay Hub'ları Veri Gönderen'i** , Azure **AD kullanıcısı, grubu veya hizmet sorumlusu** olarak erişim **atamayı** seçin ve açılan listede IoT Hub'ınızın kaynak adını seçin. **Kaydet** düğmesine tıklayın.

3. Etkinlik hub'larınızdaki **Güvenlik Duvarları ve sanal ağlar** sekmesine gidin ve **seçili ağlardan erişime izin verme** seçeneğini etkinleştirin. Özel **Durumlar** listesinde, etkinlik hub'larına erişmek için **güvenilen Microsoft hizmetlerine İzin Ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

4. IoT Hub'ınızın kaynak sayfasında İleti **yönlendirme** sekmesine gidin.

5. Özel **uç noktaları** bölümüne gidin ve **Ekle'yi**tıklatın. Bitiş noktası türü olarak **Olay hub'larını** seçin.

6. Görünen sayfada, bitiş noktanız için bir ad sağlayın, etkinlik hub'larınızı seçin ad alanı ve örneği seçin ve **Oluştur** düğmesini tıklatın.

Artık özel olay hub'larınız bitiş noktası hub'ınızın atanan kimliğini kullanacak şekilde ayarlanmıştır ve güvenlik duvarı kısıtlamalarına rağmen olay hub'ları kaynağınıza erişme iznine sahiptir. Artık bir yönlendirme kuralı ayarlamak için bu bitiş noktasını kullanabilirsiniz.


### <a name="egress-connectivity-to-service-bus-endpoints-for-routing"></a>Yönlendirme için servis veri günü uç noktalarına çıkış bağlantısı

IoT Hub, iletileri müşteriye ait bir hizmet veri yolu ad alanına yönlendirecek şekilde yapılandırılabilir. Güvenlik duvarı kısıtlamaları yerindeyken yönlendirme işlevinin bir servis veri yolu kaynağına erişmesine izin vermek için, IoT Hub'ınızın yönetilen bir hizmet kimliğine sahip olması gerekir [(yönetilen hizmet kimliğine sahip hub'ı nasıl oluşturabilirsiniz).](#create-an-iot-hub-with-managed-service-identity) Yönetilen bir hizmet kimliği sağlandıktan sonra, RBAC'a hizmet veri yolu nıza erişme izni vermek için aşağıdaki adımları izleyin.

1. Azure portalında, servis veri yolunun **'Access denetimi (IAM)** sekmesine gidin ve **rol atama ekle** bölümünün altına **Ekle'yi** tıklatın.

2. **Rol**olarak **Hizmet veri veri göndercisi** , Azure **AD kullanıcısı, grup veya hizmet sorumlusu** olarak Erişim **atamayı** seçin ve açılan listede IoT Hub'ınızın kaynak adını seçin. **Kaydet** düğmesine tıklayın.

3. Servis veri yolunuzdaki **Güvenlik Duvarları ve sanal ağlar** sekmesine gidin ve **seçili ağlardan erişime izin verme** seçeneğini etkinleştirin. Özel **Durumlar** listesi altında, bu hizmet veri veri nize erişmek için **güvenilen Microsoft hizmetlerine İzin ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

4. IoT Hub'ınızın kaynak sayfasında İleti **yönlendirme** sekmesine gidin.

5. Özel **uç noktaları** bölümüne gidin ve **Ekle'yi**tıklatın. Son nokta türü olarak **Servis veri günü kuyruğunu** veya **Servis veri aracı konusunu** (uygulanabilir olduğu şekilde) seçin.

6. Görünen sayfada, bitiş noktanız için bir ad girin, servis veri aracınızın ad alanını ve sırasını veya konusunu seçin (mümkün olduğunca). **Oluştur** düğmesine tıklayın.

Artık özel hizmet veri yolu bitiş noktanız hub'ınızın atanan kimliğini kullanmak üzere ayarlanmıştır ve güvenlik duvarı kısıtlamalarına rağmen servis veri yolu kaynağınıza erişme iznine sahiptir. Artık bir yönlendirme kuralı ayarlamak için bu bitiş noktasını kullanabilirsiniz.


### <a name="egress-connectivity-to-storage-accounts-for-file-upload"></a>Dosya yükleme için depolama hesaplarına çıkış bağlantısı

IoT Hub'ın dosya yükleme özelliği, aygıtların müşteriye ait bir depolama hesabına dosya yüklemesine olanak tanır. Dosya yüklemesinin çalışmasına izin vermek için hem aygıtların hem de IoT Hub'ın depolama hesabına bağlantı sağlaması gerekir. Depolama hesabında güvenlik duvarı kısıtlamaları varsa, bağlantı kazanmak için aygıtlarınızın desteklenen depolama hesabının mekanizmasından herhangi birini [(özel uç noktalar,](../private-link/create-private-endpoint-storage-portal.md) [hizmet bitiş noktaları](../virtual-network/virtual-network-service-endpoints-overview.md) veya doğrudan güvenlik duvarı [yapılandırması](../storage/common/storage-network-security.md)dahil) kullanması gerekir. Benzer şekilde, depolama hesabında güvenlik duvarı kısıtlamaları varsa, IoT Hub'ın depolama kaynağına güvenilen Microsoft hizmetleri özel durumu üzerinden erişmek üzere yapılandırılması gerekir. Bu amaçla, IoT Hub'ınızın yönetilen bir hizmet kimliğine sahip olması gerekir [(yönetilen hizmet kimliğine sahip hub'ın](#create-an-iot-hub-with-managed-service-identity)nasıl oluşturulabildiğini görün). Yönetilen bir hizmet kimliği sağlandıktan sonra, RBAC'a depolama hesabınıza erişmek için hub'ınızın kaynak kimliğine izin vermek için aşağıdaki adımları izleyin.

1. Azure portalında, depolama hesabınızın **Access denetimine (IAM)** gidin ve **rol atama ekle** bölümünün altına **Ekle'yi** tıklatın.

2. **Rol**olarak **Depolama Blob Veri** Katılımcısı'nı seçin , **Azure AD kullanıcısı, grubu veya hizmet sorumlusu** olarak erişim **atamave** açılan listede IoT Hub'ınızın kaynak adını seçin. **Kaydet** düğmesine tıklayın.

3. Depolama hesabınızdaki **Güvenlik Duvarları ve sanal ağlar** sekmesine gidin ve **seçili ağlardan erişime izin verme** seçeneğini etkinleştirin. Özel **Durumlar** listesinde, bu depolama hesabına erişmek için **güvenilen Microsoft hizmetlerine İzin Ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

4. IoT Hub'ınızın kaynak sayfasında **Dosya yükleme** sekmesine gidin.

5. Görünen sayfada, blob depolamanızda kullanmak istediğiniz kapsayıcıyı seçin, **Dosya bildirim ayarlarını,** **SAS TTL'yi,** **Varsayılan TTL'yi** ve **Maksimum teslimat sayısını** istediğiniz gibi yapılandırın. Depolama bitiş noktanıza **Kimlik Doğrulama türü** olarak Atanan **Sistem'i** seçin. **Oluştur** düğmesine tıklayın.

Artık dosya yükleme için depolama bitiş noktanız hub'ınızın atanan kimliğini kullanacak şekilde ayarlanmıştır ve güvenlik duvarı kısıtlamalarına rağmen depolama kaynağınıza erişim iznine sahiptir.


### <a name="egress-connectivity-to-storage-accounts-for-bulk-device-importexport"></a>Dökme cihaz alma/dışa aktarma için depolama hesaplarına çıkış bağlantısı

IoT Hub, aygıtların bilgilerini müşteri tarafından sağlanan depolama blobundan toplu olarak [alma/dışa aktarma](./iot-hub-bulk-identity-mgmt.md) işlevini destekler. Toplu alma/dışa aktarma özelliğinin çalışmasına izin vermek için, hem aygıtların hem de IoT Hub'ın depolama hesabına bağlantı sağlaması gerekir.

Bu işlevsellik, IoT Hub'dan depolama hesabına bağlantı gerektirir. Güvenlik duvarı kısıtlamaları devam ederken bir servis veri yolu kaynağına erişmek için, IoT Hub'ınızın yönetilen bir hizmet kimliğine sahip olması gerekir [(yönetilen hizmet kimliğine sahip hub'ı](#create-an-iot-hub-with-managed-service-identity)nasıl oluşturabilirsiniz). Yönetilen bir hizmet kimliği sağlandıktan sonra, RBAC'a hizmet veri yolu nıza erişme izni vermek için aşağıdaki adımları izleyin.

1. Azure portalında, depolama hesabınızın **Access denetimine (IAM)** gidin ve **rol atama ekle** bölümünün altına **Ekle'yi** tıklatın.

2. **Rol**olarak **Depolama Blob Veri** Katılımcısı'nı seçin , **Azure AD kullanıcısı, grubu veya hizmet sorumlusu** olarak erişim **atamave** açılan listede IoT Hub'ınızın kaynak adını seçin. **Kaydet** düğmesine tıklayın.

3. Depolama hesabınızdaki **Güvenlik Duvarları ve sanal ağlar** sekmesine gidin ve **seçili ağlardan erişime izin verme** seçeneğini etkinleştirin. Özel **Durumlar** listesinde, bu depolama hesabına erişmek için **güvenilen Microsoft hizmetlerine İzin Ver**kutusunu işaretleyin. **Kaydet** düğmesine tıklayın.

Azure IoT REST API'lerini, toplu alma/dışa aktarma işlevinin nasıl kullanılacağı hakkında bilgi almak için [içe aktarma işleri oluşturmak](https://docs.microsoft.com/rest/api/iothub/service/jobclient/getimportexportjobs) için kullanabilirsiniz. İstek gövdenizi ve kullanımınızı `storageAuthenticationType="identityBased"` `inputBlobContainerUri="https://..."` ve `outputBlobContainerUri="https://..."` sırasıyla depolama hesabınızın giriş ve çıkış URL'si olarak sağlamanız gerektiğini unutmayın.


Azure IoT Hub SDK'lar, hizmet istemcisinin kayıt defteri yöneticisinde de bu işlevi destekler. Aşağıdaki kod snippet C# SDK kullanarak bir alma işi başlatmak veya iş dışa nasıl gösterir.

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


Azure IoT SDK'larının bu bölge sınırlı sürümünü C#, Java ve Node.js için sanal ağ desteğiyle kullanmak için:

1. Adlı `EnableStorageIdentity` bir ortam değişkeni oluşturun `1`ve değerini 'ye ayarlayın.

2. SDK indirin: [Java](https://aka.ms/vnetjavasdk) | [C#](https://aka.ms/vnetcsharpsdk) | [Node.js](https://aka.ms/vnetnodesdk)
 
Python için sınırlı sürümümüzü GitHub'dan indirin.

1. [GitHub sürüm sayfasına](https://aka.ms/vnetpythonsdk)gidin.

2. **Varlıklar**adlı üstbilginin altında sürüm sayfasının alt kısmında bulacağınız aşağıdaki dosyayı indirin.
    > *azure_iot_hub-2.2.0_limited-py2.py3-none-any.whl*

3. Bir terminal açın ve indirilen dosyanın olduğu klasöre gidin.

4. Sanal ağlar için destek ile Python Service SDK yüklemek için aşağıdaki komutu çalıştırın:
    > pip yüklemek ./azure_iot_hub-2.2.0_limited-py2.py3-none-any.whl


## <a name="next-steps"></a>Sonraki adımlar

IoT Hub özellikleri hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları kullanın:

* [İleti yönlendirme](./iot-hub-devguide-messages-d2c.md)
* [Dosya yükleme](./iot-hub-devguide-file-upload.md)
* [Dökme cihaz alma/dışa aktarma](./iot-hub-bulk-identity-mgmt.md) 

---
title: Azure İşlerinden IP adresleri
description: İşlev uygulamaları için gelen ve giden IP adreslerini nasıl bulacağınızı ve bunların değişmesine neyin neden olduğunu öğrenin.
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: bfd2d573e0a1c78d0ef4c68be224f92e8f689f62
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656777"
---
# <a name="ip-addresses-in-azure-functions"></a>Azure İşlerinden IP adresleri

Bu makalede, işlev uygulamalarının IP adresleri ile ilgili aşağıdaki konular açıklanmaktadır:

* Bir işlev uygulaması tarafından şu anda kullanılmakta olan IP adreslerini nasıl bulabilirsiniz?
* Bir işlev uygulamasının IP adreslerinin değiştirilmesine neden olan şey.
* Bir işlev uygulamasına erişebilen IP adresleri nasıl kısıtlar?
* Bir işlev uygulaması için özel IP adresleri nasıl elde edilir?

IP adresleri tek tek işlevlerle değil, işlev uygulamalarıyla ilişkilidir. Gelen HTTP istekleri, gelen IP adresini tek tek işlevleri aramak için kullanamaz; varsayılan etki alanı adını (functionappname.azurewebsites.net) veya özel bir etki alanı adı kullanmaları gerekir.

## <a name="function-app-inbound-ip-address"></a>Fonksiyon uygulaması gelen IP adresi

Her işlev uygulamasının tek bir gelen IP adresi vardır. Ip adresini bulmak için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. İşlev uygulamasına gidin.
3. **Platform özellikleri**'ni seçin.
4. **Özellikleri**seçin ve gelen IP adresi **Sanal IP adresi**nin altında görünür.

## <a name="function-app-outbound-ip-addresses"></a><a name="find-outbound-ip-addresses"></a>İşlev uygulaması giden IP adresleri

Her işlev uygulaması, kullanılabilir giden IP adresleri kümesine sahiptir. Bir işlevden arka uç veritabanıgibi giden herhangi bir bağlantı, kullanılabilir giden IP adreslerinden birini başlangıç IP adresi olarak kullanır. Belirli bir bağlantının hangi IP adresini kullanacağını önceden bilemezsiniz. Bu nedenle, arka uç hizmetinizin güvenlik duvarını işlev uygulamasının giden TÜM IP adreslerine açması gerekir.

Bir işlev uygulamasının kullanabileceği giden IP adreslerini bulmak için:

1. Azure Kaynak [Gezgini'nde](https://resources.azure.com)oturum açın.
2. **Microsoft.Web > siteleri > abonelik >leri {aboneliğiniz} >**> seçin.
3. JSON panelinde, işlev uygulamanız `id` adına biten bir özelliğe sahip siteyi bulun.
4. Bakın `outboundIpAddresses` `possibleOutboundIpAddresses`ve . 

Kümesi `outboundIpAddresses` şu anda işlev uygulaması için kullanılabilir. Küme, `possibleOutboundIpAddresses` yalnızca işlev uygulaması [diğer fiyatlandırma katmanlarına ölçeklendirildirse](#outbound-ip-address-changes)kullanılabilir olacak IP adreslerini içerir.

Kullanılabilir giden IP adreslerini bulmanın alternatif bir yolu [Bulut Kabuğunu](../cloud-shell/quickstart.md)kullanmaktır:

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```
> [!NOTE]
> [Tüketim planında](functions-scale.md#consumption-plan) çalışan bir işlev uygulaması ölçeklendiğinde, yeni bir giden IP adresi aralığı atanabilir. Tüketim planı üzerinde çalışırken, tüm veri merkezinin beyaz listesini almanız gerekebilir.

## <a name="data-center-outbound-ip-addresses"></a>Veri merkezi giden IP adresleri

İşlev uygulamalarınız tarafından kullanılan giden IP adreslerini beyaz listeye almanız gerekiyorsa, işlev uygulamalarının veri merkezini (Azure bölgesi) beyaz listeye almak başka bir seçenektir. Tüm [Azure veri merkezleriiçin IP adreslerini listeleyen bir JSON dosyasını indirebilirsiniz.](https://www.microsoft.com/en-us/download/details.aspx?id=56519) Ardından, işlev uygulamanızın çalıştığı bölge için geçerli olan JSON parçasını bulun.

Örneğin, Batı Avrupa JSON parçası gibi görünebilir:

```
{
  "name": "AzureCloud.westeurope",
  "id": "AzureCloud.westeurope",
  "properties": {
    "changeNumber": 9,
    "region": "westeurope",
    "platform": "Azure",
    "systemService": "",
    "addressPrefixes": [
      "13.69.0.0/17",
      "13.73.128.0/18",
      ... Some IP addresses not shown here
     "213.199.180.192/27",
     "213.199.183.0/24"
    ]
  }
}
```

 Bu dosyanın ne zaman güncelleştirilip IP adreslerinin ne zaman değiştiği hakkında bilgi [için, İndirme Merkezi sayfasının](https://www.microsoft.com/en-us/download/details.aspx?id=56519) **Ayrıntılar** bölümünü genişletin.

## <a name="inbound-ip-address-changes"></a><a name="inbound-ip-address-changes"></a>Gelen IP adresi değişiklikleri

Gelen IP adresi aşağıdaki durumlarda **değişebilir:**

- Bir işlev uygulamasını silin ve farklı bir kaynak grubunda yeniden oluşturun.
- Kaynak grubu ve bölge birleşimindeki son işlev uygulamasını silin ve yeniden oluşturun.
- [Sertifika yenileme](../app-service/configure-ssl-certificate.md#renew-certificate)sırasında olduğu gibi TLS bağlamayı silin.

İşlev uygulamanız bir [Tüketim planında](functions-scale.md#consumption-plan)çalıştığında, [yukarıda listelenenler](#inbound-ip-address-changes)gibi herhangi bir işlem yapmamış olsanız bile gelen IP adresi de değişebilir.

## <a name="outbound-ip-address-changes"></a>Giden IP adresi değişiklikleri

Bir işlev uygulaması için kullanılabilir giden IP adresleri kümesi aşağıdaki leri yaptığınızda değişebilir:

* Gelen IP adresini değiştirebilecek herhangi bir eylemde.
* App Service planı fiyatlandırma katmanınızı değiştirin. Uygulamanızın kullanabileceği tüm olası giden IP adreslerinin listesi, tüm fiyatlandırma katmanları `possibleOutboundIPAddresses` için tesistedir. Bkz. [Giden IP'leri bul.](#find-outbound-ip-addresses)

İşlev uygulamanız bir [Tüketim planında](functions-scale.md#consumption-plan)çalıştığında, yukarıda [listelenenler](#inbound-ip-address-changes)gibi herhangi bir işlem yapmamış olsanız bile giden IP adresi de değişebilir.

Giden IP adresi değişikliğini kasıtlı olarak zorlamak için:

1. Uygulama Hizmeti planınızı Standart ve Premium v2 fiyatlandırma katmanları arasında yukarı veya aşağı ölçeklendirin.
2. 10 dakika bekleyin.
3. Başladığın yere geri dön.

## <a name="ip-address-restrictions"></a>IP adresi kısıtlamaları

Bir işlev uygulamasına erişime izin vermek veya reddetmek istediğiniz IP adreslerinin listesini yapılandırabilirsiniz. Daha fazla bilgi için Azure [Uygulama Hizmeti Statik IP Kısıtlamaları'na](../app-service/app-service-ip-restrictions.md)bakın.

## <a name="dedicated-ip-addresses"></a>Özel IP adresleri

Statik, özel IP adreslerine ihtiyacınız varsa, [Uygulama Hizmet Ortamları](../app-service/environment/intro.md) 'nı (Uygulama Hizmeti planlarının [yalıtılmış katmanı)](https://azure.microsoft.com/pricing/details/app-service/) öneririz. Daha fazla bilgi için, [Uygulama Hizmeti Ortamı IP adresleri](../app-service/environment/network-info.md#ase-ip-addresses) ve Bir Uygulama Servis [Ortamı'na gelen trafiği nasıl kontrol edin.](../app-service/environment/app-service-app-service-environment-control-inbound-traffic.md)

İşlev uygulamanızın Bir Uygulama Hizmet Ortamında çalışıp çalışmaz olduğunu öğrenmek için:

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. İşlev uygulamasına gidin.
3. **Genel Bakış** sekmesini seçin.
4. App Service planı **katmanı, App Service planı/fiyatlandırma katmanı**altında görünür. App Service Environment fiyatlandırma katmanı **yalıtılır.**
 
Alternatif [olarak, Bulut Kabuğu'nu](../cloud-shell/quickstart.md)kullanabilirsiniz:

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query sku --output tsv
```

Uygulama Hizmet `sku` Ortamı `Isolated`.

## <a name="next-steps"></a>Sonraki adımlar

IP değişikliklerinin yaygın bir nedeni işlev uygulama ölçeği değişiklikleridir. [Fonksiyon uygulaması ölçekleme hakkında daha fazla bilgi edinin.](functions-scale.md)

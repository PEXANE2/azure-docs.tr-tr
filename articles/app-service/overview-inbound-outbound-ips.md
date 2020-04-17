---
title: Gelen/Giden IP adresleri
description: Azure Uygulama Hizmeti'nde gelen ve giden IP adreslerinin nasıl kullanıldığını, ne zaman değiştiğini ve uygulamanızın adreslerini nasıl bulacağınızı öğrenin.
ms.topic: article
ms.date: 06/06/2019
ms.custom: seodec18
ms.openlocfilehash: 8bcd80fde95e467513590f3ed09b1dadd2646aee
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537636"
---
# <a name="inbound-and-outbound-ip-addresses-in-azure-app-service"></a>Azure Uygulama Hizmeti'nde gelen ve giden IP adresleri

[Azure Uygulama Hizmeti,](overview.md) [Uygulama Hizmet Ortamları](environment/intro.md)dışında çok kiracılı bir hizmettir. Uygulama Hizmeti ortamında olmayan uygulamalar [(Yalıtılmış katmanda](https://azure.microsoft.com/pricing/details/app-service/)değil) ağ altyapısını diğer uygulamalarla paylaşır. Sonuç olarak, bir uygulamanın gelen ve giden IP adresleri farklı olabilir ve hatta belirli durumlarda değişebilir. 

[Uygulama Hizmet Ortamları](environment/intro.md) özel ağ altyapılarını kullanır, böylece Bir Uygulama Hizmeti ortamında çalışan uygulamalar hem gelen hem de giden bağlantılar için statik, özel IP adresleri alır.

## <a name="when-inbound-ip-changes"></a>Gelen IP değiştiğinde

Ölçeklenmiş örneklerin sayısı ne olursa olsun, her uygulamanın tek bir gelen IP adresi vardır. Aşağıdaki eylemlerden birini gerçekleştirdiğinizde gelen IP adresi değişebilir:

- Bir uygulamayı silin ve farklı bir kaynak grubunda yeniden oluşturun.
- Kaynak grubu _ve_ bölge birleşimindeki son uygulamayı silin ve yeniden oluşturun.
- Sertifika yenileme sırasında olduğu gibi varolan tls bağlamayı silme (bkz. [sertifika yı yenile](configure-ssl-certificate.md#renew-certificate)).

## <a name="find-the-inbound-ip"></a>Gelen IP'yi bulma

Yerel bir terminalde aşağıdaki komutu çalıştırmam:

```bash
nslookup <app-name>.azurewebsites.net
```

## <a name="get-a-static-inbound-ip"></a>Statik gelen IP'yi alın

Bazen uygulamanız için özel, statik bir IP adresi isteyebilirsiniz. Statik gelen IP adresini almak için [özel bir etki alanı sağlamanız](configure-ssl-bindings.md#secure-a-custom-domain)gerekir. Uygulamanızın güvenliğini sağlamak için TLS işlevine gerçekten ihtiyacınız yoksa, bu bağlama için kendi imzalı bir sertifika bile yükleyebilirsiniz. IP tabanlı TLS bağlamada, sertifika IP adresine bağlıdır, bu nedenle Uygulama Hizmeti bunu gerçekleştirmek için statik bir IP adresi sağlar. 

## <a name="when-outbound-ips-change"></a>Giden IP'ler değiştiğinde

Ölçeklenmiş örneklerin sayısı ne olursa olsun, her uygulamanın belirli bir zamanda belirli sayıda giden IP adresi vardır. App Service uygulamasından arka uç veritabanı gibi giden herhangi bir bağlantı, giden IP adreslerinden birini başlangıç IP adresi olarak kullanır. Belirli bir uygulama örneğinin giden bağlantıyı yapmak için hangi IP adresini kullanacağını önceden bilemezsiniz, bu nedenle arka uç hizmetinizin güvenlik duvarını uygulamanızın tüm giden IP adreslerine açması gerekir.

Uygulamanızı alt katmanlar **(Temel,** **Standart**ve **Premium)** ile **Premium V2** katmanı arasında ölçeklendirdiğinizde, uygulamanızın giden IP adresleri kümesi değişir.

Uygulamanızın kullanabileceği tüm olası giden IP adresleri kümesini, fiyatlandırma katmanlarından bağımsız olarak, `possibleOutboundIpAddresses` özelliği arayarak veya Azure portalındaki **Özellikler** bıçakalanındaki **Ek Giden IP Adresleri** alanında bulabilirsiniz. Bkz. [Giden IP'leri bul.](#find-outbound-ips)

## <a name="find-outbound-ips"></a>Giden IP'leri bulma

Azure portalında uygulamanız tarafından şu anda kullanılan giden IP adreslerini bulmak için uygulamanızın sol navigasyonundaki **Özellikler'i** tıklatın. **Bunlar Giden IP Adresleri** alanında listelenir.

[Bulut Kabuğu'nda](../cloud-shell/quickstart.md)aşağıdaki komutu çalıştırarak aynı bilgileri bulabilirsiniz.

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query outboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).OutboundIpAddresses
```

Fiyatlandırma katmanlarından bağımsız olarak uygulamanız için olası _tüm_ giden IP adreslerini bulmak için uygulamanızın sol navigasyonundaki **Özellikler'e** tıklayın. Bunlar **Ek Giden IP Adresleri** alanında listelenir.

[Bulut Kabuğu'nda](../cloud-shell/quickstart.md)aşağıdaki komutu çalıştırarak aynı bilgileri bulabilirsiniz.

```azurecli-interactive
az webapp show --resource-group <group_name> --name <app_name> --query possibleOutboundIpAddresses --output tsv
```

```azurepowershell
(Get-AzWebApp -ResourceGroup <group_name> -name <app_name>).PossibleOutboundIpAddresses
```

## <a name="next-steps"></a>Sonraki adımlar

Gelen trafiği kaynak IP adreslerine göre nasıl kısıtlayın öğrenin.

> [!div class="nextstepaction"]
> [Statik IP kısıtlamaları](app-service-ip-restrictions.md)

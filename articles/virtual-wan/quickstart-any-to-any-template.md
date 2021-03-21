---
title: 'Hızlı başlangıç: ARM şablonunu kullanarak herhangi bir yapılandırmaya sahip bir yapılandırma oluşturma'
titleSuffix: Azure Virtual WAN
description: Bu hızlı başlangıçta, bir Azure Resource Manager şablonu (ARM şablonu) kullanarak herhangi bir yapılandırmaya nasıl bir yapılandırma oluşturacağınız gösterilmektedir.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: quickstart
ms.date: 02/02/2021
ms.author: cherylmc
ms.custom: subject-armqs
ms.openlocfilehash: d31f490baec49e8e0b6fcf89caa8c19202fdf763
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102431389"
---
# <a name="quickstart-create-an-any-to-any-configuration-using-an-arm-template"></a>Hızlı başlangıç: ARM şablonunu kullanarak herhangi bir yapılandırmaya sahip bir yapılandırma oluşturma

Bu hızlı başlangıçta, bir Azure Resource Manager şablonunun (ARM şablonu) nasıl kullanılacağı açıklanır ve herhangi bir bağlı bileşen başka bir bağlı bileşene ulaşabilirler.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure’a dağıtma** düğmesini seçin. Şablon Azure portalda açılır.

[![Azure’a dağıtma](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f201-virtual-wan-with-all-gateways%2fazuredeploy.json)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* Bu yapılandırma için ortak anahtar sertifikası verileri gereklidir. Örnek veriler, makalesinde verilmiştir. Ancak, örnek veriler yalnızca bir P2S ağ geçidi oluşturmak için şablon gereksinimlerini karşılamak üzere sağlanır. Şablon tamamlandıktan ve kaynaklar dağıtıldıktan sonra, yapılandırmanın çalışması için bu alanı kendi sertifika verileriyle güncelleştirmeniz gerekir. Bkz. [Kullanıcı VPN sertifikaları](certificates-point-to-site.md#cer).

## <a name="review-the-template"></a><a name="review"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç Şablonlarından](https://azure.microsoft.com/resources/templates/201-virtual-wan-with-all-gateways) alınmıştır. Bu makalenin şablonu burada görüntülenemeyecek kadar uzun. Şablonu görüntülemek için bkz. [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/201-virtual-wan-with-all-gateways/azuredeploy.json).

Bu hızlı başlangıçta, tüm ağ geçitleri ve VNet bağlantıları dahil olmak üzere bir Azure sanal WAN çok hub dağıtımı oluşturacaksınız. Giriş parametrelerinin listesi, seyrek olarak en düşük düzeyde tutulmuştur. IP adresleme şeması, şablonun içindeki değişkenler değiştirilerek değiştirilebilir. Senaryo, [herhangi bir senaryo](scenario-any-to-any.md) makalesinde daha ayrıntılı olarak açıklanmıştır.

:::image type="content" source="./media/routing-scenarios/any-any/figure-1.png" alt-text="Dağıtım mimarisi":::

Bu şablon, aşağıdaki kaynaklarla tam işlevli bir Azure sanal WAN ortamı oluşturur:

* Farklı bölgelerde iki ayrı hub
* Dört Azure sanal ağı (VNet)
* Her VWAN hub 'ı için iki VNet bağlantısı
* Her hub 'da bir noktadan siteye (P2S) VPN ağ geçidi
* Her hub 'da bir siteden siteye (S2S) VPN ağ geçidi
* Her hub 'da bir ExpressRoute ağ geçidi

Şablonda birden çok Azure kaynağı tanımlanmış:

* [**Microsoft. Network/virtualwan 'lar**](/azure/templates/microsoft.network/virtualwans)
* [**Microsoft. Network/virtualhub 'ları**](/azure/templates/microsoft.network/virtualhubs)
* [**Microsoft. Network/virtualnetworks**](/azure/templates/microsoft.network/virtualnetworks)
* [**Microsoft. Network/hubvirtualnetworkconnections**](/azure/templates/microsoft.network/virtualhubs/hubvirtualnetworkconnections)
* [**Microsoft. Network/p2svpngateways**](/azure/templates/microsoft.network/p2svpngateways)
* [**Microsoft. Network/vpngateway 'ler**](/azure/templates/microsoft.network/vpngateways) 
* [**Microsoft. Network/expressroutegateway 'ler**](/azure/templates/microsoft.network/expressroutegateways)
* [**Microsoft. Network/vpnserverconfigurations**](/azure/templates/microsoft.network/vpnserverconfigurations)

>[!NOTE]
> Bu ARM şablonu, karma bağlantı için gerekli olan müşteri tarafı kaynaklarını oluşturmaz. Şablonu dağıttıktan sonra, P2S VPN istemcileri, VPN dalları (yerel siteler) oluşturmanız ve yapılandırmanız ve ExpressRoute devrelerine bağlanmanız gerekir.
>

Daha fazla şablon bulmak için bkz. [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

## <a name="deploy-the-template"></a><a name="deploy"></a>Şablonu dağıtma

Bu şablonu düzgün bir şekilde dağıtmak için, aşağıdaki nedenlerden dolayı diğer yöntemler yerine Azure 'a ve Azure portal dağıtmak üzere düğmesini kullanmanız gerekir:

* P2S yapılandırmasını oluşturmak için kök sertifika verilerini karşıya yüklemeniz gerekir. Veri alanı, PowerShell veya CLı kullanılırken sertifika verilerini kabul etmez.
* Bu şablon, sertifika verileri karşıya yüklemesi nedeniyle Cloud Shell kullanılarak düzgün çalışmıyor.
* Ayrıca, IP adresi aralıklarını ve diğer değerleri karşılamak için portalda şablon ve parametreleri kolayca değiştirebilirsiniz.

1. **Azure’a dağıt**’a tıklayın.

   [![Azure’a dağıtın](../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f201-virtual-wan-with-all-gateways%2fazuredeploy.json)
1. Şablonu görüntülemek için **Şablonu Düzenle**' ye tıklayın. Bu sayfada, adres alanı veya belirli kaynakların adı gibi bazı değerleri ayarlayabilirsiniz. Değişikliklerinizi kaydetmek için **kaydedin** veya **atın**.
1. Şablon sayfasında, değerleri girin. Bu şablon için P2S genel sertifika verileri gereklidir. Bu makaleyi bir alıştırma olarak kullanıyorsanız, her iki hub için de bu. cer dosyasındaki aşağıdaki verileri örnek veri olarak kullanabilirsiniz. Şablon çalıştıktan ve dağıtım tamamlandıktan sonra, P2S yapılandırmasını kullanmak için, bu bilgileri kendi dağıtımınızın ortak anahtar [sertifika verileriyle](certificates-point-to-site.md#cer) değiştirmelisiniz.

   ```certificate-data
   MIIC5zCCAc+gAwIBAgIQGxd3Av1q6LJDZ71e3TzqcTANBgkqhkiG9w0BAQsFADAW
   MRQwEgYDVQQDDAtQMlNSb290Q2VydDAeFw0yMDExMDkyMjMxNTVaFw0yMTExMDky
   MjUxNTVaMBYxFDASBgNVBAMMC1AyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEF
   AAOCAQ8AMIIBCgKCAQEA33fFra/E0YmGuXLKmYcdvjsYpKwQmw8DjjDkbwhE9jcc
   Dp50e7F1P6Rxo1T6Hm3dIhEji+0QkP4Ie0XPpw0eW77+RWUiG9XJxGqtJ3Q4tyRy
   vBfsHORcqMlpV3VZOXIxrk+L/1sSm2xAc2QGuOqKaDNNoKmjrSGNVAeQHigxbTQg
   zCcyeuhFxHxAaxpW0bslK2hEZ9PhuAe22c2SHht6fOIDeXkadzqTFeV8wEZdltLr
   6Per0krxf7N2hFo5Cfz0KgWlvgdKLL7dUc9cjHo6b6BL2pNbLh8YofwHQOQbwt6H
   miAkEnx1EJ5N8AWuruUTByR2jcWyCnEAUSH41+nk4QIDAQABozEwLzAOBgNVHQ8B
   Af8EBAMCAgQwHQYDVR0OBBYEFJMgnJSYHH5AJ+9XB11usKRwjbjNMA0GCSqGSIb3
   DQEBCwUAA4IBAQBOy8Z5FBd/nvgDcjvAwNCw9h5RHzgtgQqDP0qUjEqeQv3ALeC+
   k/F2Tz0OWiPEzX5N+MMrf/jiYsL2exXuaPWCF5U9fu8bvs89GabHma8MGU3Qua2x
   Imvt0whWExQMjoyU8SNUi2S13fnRie9ZlSwNh8B/OIUUEtVhQsd4OfuZZFVH4xGp
   ibJMSMe5JBbZJC2tCdSdTLYfYJqrLkVuTjynXOjmz2JXfwnDNqEMdIMMjXzlNavR
   J8SNtAoptMOK5vAvlySg4LYtFyXkl0W0vLKIbbHf+2UszuSCijTUa3o/Y1FoYSfi
   eJH431YTnVLuwdd6fXkXFBrXDhjNsU866+hE
   ```

1. Değer girmeyi bitirdiğinizde, **gözden geçir + oluştur**' u seçin.
1. **Gözden geçir + oluştur** sayfasında, doğrulama geçtikten sonra **Oluştur**' u seçin.
1. Dağıtımın tamamlanabilmesi yaklaşık 75 dakika sürer. İlerlemeyi şablon **genel bakış** sayfasında görüntüleyebilirsiniz.  Portalı kapatırsanız dağıtım devam eder.

   :::image type="content" source="./media/quickstart-any-to-any-template/template.png" alt-text="Dağıtım örneği Tamam":::

## <a name="validate-the-deployment"></a><a name="validate"></a>Dağıtımı doğrulama

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol bölmeden **kaynak grupları** ' nı seçin.
1. Önceki bölümde oluşturduğunuz kaynak grubunu seçin. **Genel bakış** sayfasında, aşağıdaki örneğe benzer bir şey görürsünüz: :::image type="content" source="./media/quickstart-any-to-any-template/resources.png" alt-text="kaynak örneği" lightbox="./media/quickstart-any-to-any-template/resources.png":::

1. Hub 'ları görüntülemek için sanal WAN ' a tıklayın. Sanal WAN sayfasında, bağlantıları ve diğer Merkez bilgilerini görüntülemek için her bir hub 'a tıklayın.
   :::image type="content" source="./media/quickstart-any-to-any-template/hub.png" alt-text="Hub örneği" lightbox="./media/quickstart-any-to-any-template/hub.png":::

## <a name="complete-the-hybrid-configuration"></a><a name="complete"></a>Karma yapılandırmayı doldurun

Şablon, karma ağ için gereken tüm ayarları yapılandırmaz. Gereksinimlerinize bağlı olarak aşağıdaki yapılandırma ve ayarları gerçekleştirmeniz gerekir.

* [VPN dalları-yerel siteleri yapılandırma](virtual-wan-site-to-site-portal.md#site)
* [P2S VPN yapılandırmasını doldurun](virtual-wan-point-to-site-portal.md)
* [ExpressRoute devrelerini bağlama](virtual-wan-expressroute-portal.md)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, bunları silin. Bağımlılıklar nedeniyle sanal WAN kaynaklarından bazılarının belirli bir sırada silinmesi gerekir. Silme işleminin tamamlanması yaklaşık 30 dakika sürebilir.

[!INCLUDE [Delete resources](../../includes/virtual-wan-resource-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [P2S VPN yapılandırmasını doldurun](virtual-wan-point-to-site-portal.md)

> [!div class="nextstepaction"]
> [VPN dalları-yerel siteleri yapılandırma](virtual-wan-site-to-site-portal.md#site)

> [!div class="nextstepaction"]
> [ExpressRoute devrelerini bağlama](virtual-wan-expressroute-portal.md)
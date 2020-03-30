---
title: Azure DNS'yi Azure kaynaklarınızla tümleştirin - Azure DNS
description: Bu makalede, Azure kaynaklarınız için DNS sağlamak için Azure DNS'yi nasıl kullanacağınızı öğrenin.
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 7/13/2019
ms.author: rohink
ms.openlocfilehash: d84a7a908bd3bb5cfb2958a617be437f3b6b154e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266240"
---
# <a name="use-azure-dns-to-provide-custom-domain-settings-for-an-azure-service"></a>Azure hizmeti için özel etki alanı ayarları sağlamak için Azure DNS'yi kullanın

Azure DNS, özel etki alanlarını destekleyen veya tam nitelikli etki alanı adı (FQDN) olan Azure kaynaklarınız için özel bir etki alanı için DNS sağlar. Bir örnek, bir Azure web uygulamanız olması ve kullanıcılarınızın bu uygulamaya\.contoso.com veya www contoso.com'yi FQDN olarak kullanarak erişmelerini istemenizdir. Bu makale, Azure hizmetinizi azure dns ile özel etki alanlarını kullanmak için yapılandırmakonusunda size yol açmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Azure DNS'yi özel etki alanınız için kullanabilmek için öncelikle etki alanınızı Azure DNS'ye devretmeniz gerekir. Ad sunucularınızı temsilci olarak nasıl yapılandıracağınıza ilişkin yönergeler için [bir etki alanını Azure DNS'ye devret'i](./dns-delegate-domain-azure-dns.md) ziyaret edin. Etki alanınız Azure DNS bölgenize devredildikten sonra, gereken DNS kayıtlarını yapılandırabilirsiniz.

[Azure İşlev Uygulamaları,](#azure-function-app) [Genel IP adresleri,](#public-ip-address)Uygulama Hizmeti [(Web Uygulamaları)](#app-service-web-apps), [Blob depolama](#blob-storage)alanı ve Azure [CDN](#azure-cdn)için bir makyaj veya özel etki alanı yapılandırabilirsiniz.

## <a name="azure-function-app"></a>Azure İşlevi Uygulaması

Azure işlev uygulamaları için özel bir etki alanını yapılandırmak için, işlev uygulamasının kendisinde yapılandırmanın yanı sıra bir CNAME kaydı da oluşturulur.
 
**İşlev Uygulaması'na** gidin ve işlev uygulamanızı seçin. **Platform özelliklerini** tıklatın ve **Ağ** altında Özel **etki alanlarını**tıklatın.

![fonksiyon uygulama bıçağı](./media/dns-custom-domain/functionapp.png)

**Özel etki alanları** bıçak üzerinde geçerli url dikkat, bu adres oluşturulan DNS kaydı için takma ad olarak kullanılır.

![özel etki alanı bıçak](./media/dns-custom-domain/functionshostname.png)

DNS Bölgenize gidin ve **+ Kayıt kümesini**tıklatın. **Kayıt ekle kümesi** nde aşağıdaki bilgileri doldurun ve oluşturmak için **Tamam'ı** tıklatın.

|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Adı     | myfunctionapp        | Etki alanı adı etiketiyle birlikte bu değer, özel alan adı için FQDN'dir.        |
|Tür     | CNAME        | CNAME kaydı kullanmak bir takma ad kullanıyor.        |
|TTL     | 1        | 1 1 saat için kullanılır        |
|TTL birimi     | Saat        | Saat ölçümü olarak saatler kullanılır         |
|Diğer ad     | adatumfunction.azurewebsites.net        | Diğer adı oluşturduğunuz DNS adı, bu örnekte işlev uygulamasına varsayılan olarak sağlanan adatumfunction.azurewebsites.net DNS adıdır.        |

İşlev uygulamanıza geri gidin, **Platform özelliklerini**tıklatın ve **Ağ** altında Özel **etki alanlarını**tıklatın, ardından Özel Ana Bilgisayar Adları altında + **Hostname** **Ekle'yi**tıklatın.

Ana **bilgisayar adı ekle** bıçağında, ana **bilgisayar metin** alanına CNAME kaydını girin ve **Doğrula'yı**tıklatın. Kayıt bulunursa, **ana bilgisayar adı ekle** düğmesi görüntülenir. Takma ad eklemek için **hostname ekle'yi** tıklatın.

![fonksiyon uygulamaları ana bilgisayar adı blade eklemek](./media/dns-custom-domain/functionaddhostname.png)

## <a name="public-ip-address"></a>Genel IP adresi

Uygulama Ağ Geçidi, Yük Dengeleyicisi, Bulut Hizmeti, Kaynak Yöneticisi VM'leri ve Klasik VM'ler gibi genel IP adresi kaynağı kullanan hizmetler için özel bir etki alanını yapılandırmak için bir kayıt kullanılır.

Ağ **Genel** > **IP adresine**gidin, Ortak IP kaynağını seçin ve **Yapılandırma'yı**tıklatın. Gösterilen IP adresini not edin.

![kamu ip bıçak](./media/dns-custom-domain/publicip.png)

DNS Bölgenize gidin ve **+ Kayıt kümesini**tıklatın. **Kayıt ekle kümesi** nde aşağıdaki bilgileri doldurun ve oluşturmak için **Tamam'ı** tıklatın.


|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Adı     | mywebserver        | Etki alanı adı etiketiyle birlikte bu değer, özel alan adı için FQDN'dir.        |
|Tür     | A        | Kaynak bir IP adresi olduğu için A kaydı kullanın.        |
|TTL     | 1        | 1 1 saat için kullanılır        |
|TTL birimi     | Saat        | Saat ölçümü olarak saatler kullanılır         |
|IP Adresi     | `<your ip address>`       | Genel IP adresi.|

![A kaydı oluşturma](./media/dns-custom-domain/arecord.png)

A kaydı oluşturulduktan sonra, kaydı doğrulamak için çalıştırın. `nslookup`

![kamu ip dns arama](./media/dns-custom-domain/publicipnslookup.png)

## <a name="app-service-web-apps"></a>Uygulama Hizmeti (Web Uygulamaları)

Aşağıdaki adımlar, bir uygulama hizmeti web uygulaması için özel bir etki alanı yapılandırma yoluyla götürür.

Uygulama **Hizmeti'ne** gidin ve özel bir etki alanı adı yapılandırdığınız kaynağı seçin ve **Özel etki alanlarını**tıklatın.

**Özel etki alanları** bıçak üzerinde geçerli url dikkat, bu adres oluşturulan DNS kaydı için takma ad olarak kullanılır.

![özel etki alanları bıçak](./media/dns-custom-domain/url.png)

DNS Bölgenize gidin ve **+ Kayıt kümesini**tıklatın. **Kayıt ekle kümesi** nde aşağıdaki bilgileri doldurun ve oluşturmak için **Tamam'ı** tıklatın.


|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Adı     | mywebserver        | Etki alanı adı etiketiyle birlikte bu değer, özel alan adı için FQDN'dir.        |
|Tür     | CNAME        | CNAME kaydı kullanmak bir takma ad kullanıyor. Kaynak bir IP adresi kullanırsa, A kaydı kullanılır.        |
|TTL     | 1        | 1 1 saat için kullanılır        |
|TTL birimi     | Saat        | Saat ölçümü olarak saatler kullanılır         |
|Diğer ad     | webserver.azurewebsites.net        | Diğer adı oluşturduğunuz DNS adı, bu örnekte varsayılan olarak web uygulamasına sağlanan webserver.azurewebsites.net DNS adıdır.        |


![CNAME kaydı oluşturma](./media/dns-custom-domain/createcnamerecord.png)

Özel etki alanı adı için yapılandırılan uygulama hizmetine geri gidin. Özel etki alanlarını tıklatın, ardından **Ana Bilgisayar Adlarını**tıklatın. **Custom domains** Oluşturduğunuz CNAME kaydını eklemek için **+ Ana Bilgisayar Adı Ekle'yi**tıklatın.

![Şekil 1](./media/dns-custom-domain/figure1.png)

İşlem tamamlandıktan sonra, ad çözümlemesi doğrulamak için **nslookup** çalıştırın çalışıyor.

![Şekil 1](./media/dns-custom-domain/finalnslookup.png)

Özel bir etki alanını Uygulama Hizmetiyle eşleme hakkında daha fazla bilgi edinmek [için, varolan özel bir DNS adını Azure Web Uygulamaları'na eşleyin'i](../app-service/app-service-web-tutorial-custom-domain.md?toc=%dns%2ftoc.json)ziyaret edin.

Etkin bir DNS adını nasıl geçirteceklerini öğrenmek [için](../app-service/manage-custom-dns-migrate-domain.md)bkz.

Özel bir etki alanı satın almanız gerekiyorsa, Uygulama Hizmeti etki alanları hakkında daha fazla bilgi edinmek [için Azure Web Apps için özel bir etki alanı adı satın](../app-service/manage-custom-dns-buy-domain.md) alın'ı ziyaret edin.

## <a name="blob-storage"></a>Blob depolama

Aşağıdaki adımlar, asverify yöntemini kullanarak bir blob depolama hesabı için bir CNAME kaydı yapılandırma yoluyla alır. Bu yöntem, herhangi bir kapalı kalma süresi olmasını sağlar.

**Depolama** > **Depolama Hesapları'na**gidin, depolama hesabınızı seçin ve Özel **etki alanını**tıklatın. Adım 2 altında FQDN notate, bu değer ilk CNAME kaydı oluşturmak için kullanılır

![blob depolama özel etki alanı](./media/dns-custom-domain/blobcustomdomain.png)

DNS Bölgenize gidin ve **+ Kayıt kümesini**tıklatın. **Kayıt ekle kümesi** nde aşağıdaki bilgileri doldurun ve oluşturmak için **Tamam'ı** tıklatın.


|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Adı     | asverify.mystorageaccount        | Etki alanı adı etiketiyle birlikte bu değer, özel alan adı için FQDN'dir.        |
|Tür     | CNAME        | CNAME kaydı kullanmak bir takma ad kullanıyor.        |
|TTL     | 1        | 1 1 saat için kullanılır        |
|TTL birimi     | Saat        | Saat ölçümü olarak saatler kullanılır         |
|Diğer ad     | asverify.adatumfunctiona9ed.blob.core.windows.net        | Diğer adı oluşturduğunuz DNS adı, bu örnekte depolama hesabına varsayılan olarak sağlanan asverify.adatumfunctiona9ed.blob.core.windows.net DNS adıdır.        |

**Depolama** > **Alanı Hesapları'nı**tıklatarak depolama hesabınıza geri gidin, depolama hesabınızı seçin ve Özel **etki alanını**tıklatın. Metin kutusunda asverify öneki olmadan oluşturduğunuz takma ad yazın, **Dolaylı CNAME doğrulamasını kullanın ve **Kaydet'i**tıklatın. Bu adım tamamlandıktan sonra, DNS bölgenize dönün ve asverify öneki olmadan bir CNAME kaydı oluşturun.  Bu noktadan sonra, cdnverify öneki ile CNAME kaydını silmek için güvenlidir.

![blob depolama özel etki alanı](./media/dns-custom-domain/indirectvalidate.png)

Çalıştırarak DNS çözünürlüğünü doğrulayın`nslookup`

Blob depolama bitiş noktası için özel bir etki alanı eşleme hakkında daha fazla bilgi edinmek için ziyaret [Blob depolama bitiş noktanız için özel bir etki alanı adı yapılandırma](../storage/blobs/storage-custom-domain-name.md?toc=%dns%2ftoc.json)

## <a name="azure-cdn"></a>Azure CDN

Aşağıdaki adımlar cdnverify yöntemini kullanarak bir CDN bitiş noktası için bir CNAME kaydı yapılandırma yoluyla alır. Bu yöntem, herhangi bir kapalı kalma süresi olmasını sağlar.

**Ağ** > **CDN Profilleri'ne**gidin, CDN profilinizi seçin.

Çalıştığınız bitiş noktasını seçin ve **+ Özel etki alanına**tıklayın. Bu değer CNAME kaydının işaret ettiği kayıt olduğu için **Bitiş Noktası ana bilgisayar adını** not edin.

![CDN özel etki alanı](./media/dns-custom-domain/endpointcustomdomain.png)

DNS Bölgenize gidin ve **+ Kayıt kümesini**tıklatın. **Kayıt ekle kümesi** nde aşağıdaki bilgileri doldurun ve oluşturmak için **Tamam'ı** tıklatın.

|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Adı     | cdnverify.mycdnendpoint        | Etki alanı adı etiketiyle birlikte bu değer, özel alan adı için FQDN'dir.        |
|Tür     | CNAME        | CNAME kaydı kullanmak bir takma ad kullanıyor.        |
|TTL     | 1        | 1 1 saat için kullanılır        |
|TTL birimi     | Saat        | Saat ölçümü olarak saatler kullanılır         |
|Diğer ad     | cdnverify.adatumcdnendpoint.azureedge.net        | Diğer adı oluşturduğunuz DNS adı, bu örnekte depolama hesabına varsayılan olarak sağlanan cdnverify.adatumcdnendpoint.azureedge.net DNS adıdır.        |

**Ağ CDN** > Profilleri'ni tıklatarak**CDN**bitiş noktanıza geri gidin ve CDN profilinizi seçin. **+ Özel etki alanını** tıklatın ve cdnverify öneki olmadan CNAME kayıt takma girin ve **Ekle'yi**tıklatın.

Bu adım tamamlandıktan sonra, DNS bölgenize dönün ve cdnverify öneki olmadan bir CNAME kaydı oluşturun.  Bu noktadan sonra, cdnverify öneki ile CNAME kaydını silmek için güvenlidir. CDN hakkında daha fazla bilgi ve ara kayıt adımı olmadan özel bir etki alanının nasıl yapılandırılabildiğini öğrenmek [için Harita Azure CDN içeriğini özel bir etki alanına](../cdn/cdn-map-content-to-custom-domain.md?toc=%dns%2ftoc.json)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

[Azure'da barındırılan hizmetler için ters DNS'yi nasıl yapılandırabileceğinizi](dns-reverse-dns-for-azure-services.md)öğrenin.

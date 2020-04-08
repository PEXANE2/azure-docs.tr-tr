---
title: TLS/SSL bağlama ile özel bir DNS'yi emniyete alma
description: Sertifika ile tls/SSL bağlama oluşturarak özel etki alanınıza HTTPS erişimini güvence altına ala. HTTPS veya TLS 1.2'yi uygulayarak web sitenizin güvenliğini artırın.
tags: buy-ssl-certificates
ms.topic: tutorial
ms.date: 10/25/2019
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: 9792181379bfa6f9e0337bf14208fe853c16b745
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811755"
---
# <a name="secure-a-custom-dns-name-with-a-tlsssl-binding-in-azure-app-service"></a>Azure Uygulama Hizmetinde TLS/SSL bağlama ile özel bir DNS adı güvenliğini sağlama

Bu makalede, bir sertifika bağlama oluşturarak [App Service uygulamanızda](https://docs.microsoft.com/azure/app-service/) veya [işlev uygulamanızda](https://docs.microsoft.com/azure/azure-functions/) [özel etki alanının](app-service-web-tutorial-custom-domain.md) nasıl güvence altına alınırsınız gösterilmektedir. İşinizi bitirdiğinizde, Özel DNS adınız `https://` için bitiş noktasındaki Uygulama Hizmeti uygulamasına erişebilirsiniz (örneğin,). `https://www.contoso.com` 

![Özel TLS/SSL sertifikasına sahip web uygulaması](./media/configure-ssl-bindings/app-with-custom-ssl.png)

[Sertifikaile özel](app-service-web-tutorial-custom-domain.md) bir etki alanı nın güvenliğini sağlamak iki adımdan oluşur:

- Tüm [özel sertifika gereksinimlerini](configure-ssl-certificate.md#private-certificate-requirements)karşılayan Uygulama Hizmetine özel sertifika [ekleyin.](configure-ssl-certificate.md)
-  İlgili özel etki alanına bağlanan bir TLS oluşturun. Bu ikinci adım bu makale kapsamındadır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Uygulamanızın fiyatlandırma katmanını yükseltme
> * Sertifikayla özel bir etki alanını güvenli hale
> * HTTPS zorlama
> * TLS 1.1/1.2 zorlama
> * TLS yönetimini betiklerle otomatikleştirme

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını takip etmek için:

- [App Service uygulaması oluşturma](/azure/app-service/)
- [Uygulamanızla bir etki alanı adı eşlenin](app-service-web-tutorial-custom-domain.md) veya [Azure'da satın alıp yapılandırır](manage-custom-dns-buy-domain.md)
- [Uygulamanıza özel sertifika ekleme](configure-ssl-certificate.md)

> [!NOTE]
> Özel sertifika eklemenin en kolay yolu [ücretsiz bir Uygulama Hizmeti Yönetilen Sertifikası](configure-ssl-certificate.md#create-a-free-certificate-preview) (Önizleme) oluşturmaktır.

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

<a name="upload"></a>

## <a name="secure-a-custom-domain"></a>Özel bir etki alanını güvenli hale

Aşağıdaki adımları yapın:

Azure <a href="https://portal.azure.com" target="_blank">portalında,</a>sol menüden **Uygulama Hizmetleri** > **\<uygulama adını>' **yi seçin.

Uygulamanızın sol navigasyonundan **TLS/SSL Bağlama** iletişim kutusunu şu şekilde başlatın:

- Özel **etki alanlarını** > seçme**Bağlama**
- **TLS/SSL ayarlarını** > seçme**TLS/SSL bağlama ekleme**

![Etki alanına bağlama ekleme](./media/configure-ssl-bindings/secure-domain-launch.png)

**Özel Etki Alanı'nda,** bağlayıcı eklemek istediğiniz özel etki alanını seçin.

Uygulamanızın zaten seçili özel etki alanı için bir sertifikası varsa, doğrudan [bağlayıcı oluştur'a](#create-binding) gidin. Yoksa devam et.

### <a name="add-a-certificate-for-custom-domain"></a>Özel etki alanı için sertifika ekleme

Uygulamanızın seçili özel etki alanı için sertifikası yoksa, iki seçeneğiniz vardır:

- **PFX Sertifikası Yükle** - [Özel sertifika Yükle'deki](configure-ssl-certificate.md#upload-a-private-certificate)iş akışını izleyin, ardından bu seçeneği buradan seçin.
- **Uygulama Hizmet SertifikasıNı İçe Aktar** - [Uygulama Hizmeti Sertifikası Nı İçe'deki](configure-ssl-certificate.md#import-an-app-service-certificate)iş akışını izleyin ve ardından bu seçeneği buradan seçin.

> [!NOTE]
> [Ayrıca ücretsiz bir sertifika](configure-ssl-certificate.md#create-a-free-certificate-preview) (Önizleme) oluşturabilir veya Bir Anahtar Kasa sertifikası içe [aktarabilirsiniz,](configure-ssl-certificate.md#import-a-certificate-from-key-vault)ancak bunu ayrı olarak yapmanız ve ardından **TLS/SSL Bağlama** iletişim kutusuna dönmeniz gerekir.

### <a name="create-binding"></a>Bağlayıcı oluşturma

**TLS/SSL Bağlama** iletişim kutusunda TLS bağlama yapılandırmanıza yardımcı olmak için aşağıdaki tabloyu kullanın, ardından **Bağlayıcı ekle'yi**tıklatın.

| Ayar | Açıklama |
|-|-|
| Özel etki alanı | TLS/SSL bağlayıcısı için alan adı. |
| Özel Sertifika Parmak İzi | Bağlatılmak için sertifika. |
| TLS/SSL Tipi | <ul><li>**[SNI SSL](https://en.wikipedia.org/wiki/Server_Name_Indication)** - Birden fazla SNI SSL bağlaması eklenebilir. Bu seçenek, aynı IP adresi üzerinde birden çok etki alanını güvenli hale getirmek için birden çok TLS/SSL sertifikasısağlar. Çoğu modern tarayıcı (Internet Explorer, Chrome, Firefox ve Opera dahil) SNI'yi destekler (daha fazla bilgi için [Sunucu Adı Göstergesi'ne](https://wikipedia.org/wiki/Server_Name_Indication)bakın).</li><li>**IP SSL** - Yalnızca bir IP SSL bağlama eklenebilir. Bu seçenek, özel bir genel IP adresini güvence altına almak için yalnızca bir TLS/SSL sertifikası sağlar. Bağlamayı yapılandırıldıktan sonra, [IP SSL için Remap A kaydındaki](#remap-a-record-for-ip-ssl)adımları izleyin.<br/>IP SSL yalnızca Üretim veya İzole katmanlarında desteklenir. </li></ul> |

İşlem tamamlandıktan sonra, özel etki alanının TLS/SSL durumu **Secure**olarak değiştirilir.

![TLS/SSL bağlama başarılı](./media/configure-ssl-bindings/secure-domain-finished.png)

> [!NOTE]
> **Özel etki alanlarında** **güvenli** bir durum, sertifikayla güvenli olduğu anlamına gelir, ancak Uygulama Hizmeti sertifikanın kendi imzalanmış veya süresi dolmuş olup olmadığını denetlemez, bu da tarayıcıların hata veya uyarı göstermesine neden olabilir.

## <a name="remap-a-record-for-ip-ssl"></a>IP SSL için A kaydını yeniden eşleme

Uygulamanızda IP SSL kullanmıyorsanız, [özel etki alanınız için HTTPS Test'e](#test-https)atlayın.

Varsayılan olarak, uygulamanız paylaşılan bir genel IP adresi kullanır. Bir sertifikayı IP SSL'ye bağladiğinizde, App Service uygulamanız için yeni ve özel bir IP adresi oluşturur.

Uygulamanıza a kaydı eşlediyseniz, bu yeni, özel IP adresiyle etki alanı kayıt defterinizi güncelleyin.

Uygulamanızın **Özel etki alanı** sayfası yeni, özel IP adresiyle güncellenir. [Bu IP adresini kopyalayın](app-service-web-tutorial-custom-domain.md#info), ardından bu yeni IP adresine [A kaydını yeniden eşleyin](app-service-web-tutorial-custom-domain.md#map-an-a-record).

## <a name="test-https"></a>HTTPS’yi test etme

Çeşitli tarayıcılarda, uygulamanızın `https://<your.custom.domain>` hizmet verdiğini doğrulamak için göz atın.

![Azure uygulamasına portal gezintisi](./media/configure-ssl-bindings/app-with-custom-ssl.png)

Uygulama kodunuz protokolü "x-appservice-proto" üstbilgisi üzerinden inceleyebilir. Üstbilginin bir değeri `http` olacak `https`veya . 

> [!NOTE]
> Uygulamanız size sertifika doğrulama hataları veriyorsa, büyük olasılıkla kendi imzalı bir sertifika kullanıyorsunuz.
>
> Böyle bir durum söz konusu değilse, sertifikanızı PFX dosyasına aktardığınızda ara sertifikaları dışarıda bırakmış olabilirsiniz.

## <a name="prevent-ip-changes"></a>IP değişikliklerini önleme

Gelen IP adresiniz, bu bağlama IP SSL olsa bile, bir bağlamayı sildiğinizde değişebilir. Bu, özellikle IP SSL bağlamasında bulunan bir sertifikayı yenilediğinizde önemlidir. Uygulamanızın IP adresinin değişmesini önlemek için şu adımları sırasıyla izleyin:

1. Yeni sertifikayı karşıya yükleyin.
2. Eskisini silmeden yeni sertifikayı istediğiniz özel etki alanına bağlayın. Bu eylem, eskisini kaldırmak yerine bağlamayı değiştirir.
3. Eski sertifikayı silin. 

## <a name="enforce-https"></a>HTTPS zorlama

Varsayılan olarak, herkes HTTP'yi kullanarak uygulamanıza erişebilir. Tüm HTTPS isteklerini HTTP bağlantı noktasına yeniden yönlendirebilirsiniz.

Uygulama sayfanızda, sol navigasyonda **SSL ayarlarını**seçin. Ardından **Yalnızca HTTPS** menüsünde **Açık**’ı seçin.

![HTTPS zorlama](./media/configure-ssl-bindings/enforce-https.png)

İşlem tamamlandığında, uygulamanıza işaret eden HTTP URL'lerinden herhangi birine gidin. Örneğin:

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="enforce-tls-versions"></a>TLS sürümlerini zorlama

Uygulamanız varsayılan olarak [TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.2 sürümüne izin verir. Bu, [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard) gibi endüstri standartlarınca önerilen TLS düzeyidir. Farklı TLS sürümlerini zorlamak için aşağıdaki adımları uygulayın:

Uygulama sayfanızda, sol navigasyonda **SSL ayarlarını**seçin. Ardından **TLS sürümü**’nde istediğiniz en düşük TLS sürümünü seçin. Bu ayar yalnızca gelen çağrıları denetler. 

![TLS 1.1 veya 1.2’yi zorlama](./media/configure-ssl-bindings/enforce-tls1-2.png)

İşlem tamamlandığında, uygulamanız daha düşük TLS sürümleriyle tüm bağlantıları reddeder.

## <a name="handle-tls-termination"></a>TLS sonlandırma işlemini işleme

App Service'de [TLS sonlandırma](https://wikipedia.org/wiki/TLS_termination_proxy) ağı yük bakiyelerinde gerçekleşir, böylece tüm HTTPS istekleri uygulamanıza şifrelenmemiş HTTP istekleri olarak ulaşır. Uygulama mantığınızın kullanıcı isteklerinin şifreli olup olmadığını denetlemesi gerekiyorsa, üstbilgide inceleme. `X-Forwarded-Proto`

[Linux Node.js yapılandırma](containers/configure-language-nodejs.md#detect-https-session) kılavuzu gibi dile özgü yapılandırma kılavuzları, uygulama kodunuzda bir HTTPS oturumunu nasıl algıladığınızı gösterir.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

### <a name="azure-cli"></a>Azure CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom TLS/SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom TLS/SSL certificate to a web app")]

## <a name="more-resources"></a>Diğer kaynaklar

* [Azure Uygulama Hizmeti'nde kodunuzda TLS/SSL sertifikası kullanma](configure-ssl-certificate-in-code.md)
* [SSS : Uygulama Hizmet Sertifikaları](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/)

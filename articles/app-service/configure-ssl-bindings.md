---
title: Bir TLS/SSL bağlaması ile özel bir DNS güvenliğini sağlama
description: Sertifika ile bir TLS/SSL bağlaması oluşturarak özel etki alanınız için güvenli HTTPS erişimi. HTTPS veya TLS 1,2 ' i zorlayarak Web sitenizin güvenliğini geliştirebilirsiniz.
tags: buy-ssl-certificates
ms.topic: tutorial
ms.date: 04/30/2020
ms.reviewer: yutlin
ms.custom: seodec18
ms.openlocfilehash: fb62d4d2ca22b6043e63645006c2d60cf0b7859b
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88078640"
---
# <a name="secure-a-custom-dns-name-with-a-tlsssl-binding-in-azure-app-service"></a>Azure App Service 'de TLS/SSL bağlaması ile özel bir DNS adının güvenliğini sağlama

Bu makalede, bir sertifika bağlaması oluşturarak [App Service uygulamanızda](https://docs.microsoft.com/azure/app-service/) veya [işlev uygulamanızda](https://docs.microsoft.com/azure/azure-functions/) [özel etki alanının](app-service-web-tutorial-custom-domain.md) güvenliğini sağlama işlemi gösterilmektedir. İşiniz bittiğinde, `https://` özel DNS adınızın uç noktasında App Service uygulamanıza erişebilirsiniz (örneğin, `https://www.contoso.com` ). 

![Özel TLS/SSL sertifikasıyla Web uygulaması](./media/configure-ssl-bindings/app-with-custom-ssl.png)

Özel bir [etki alanını](app-service-web-tutorial-custom-domain.md) sertifikayla güvenli hale getirmek iki adımdan oluşur:

- Tüm [özel sertifika gereksinimlerini](configure-ssl-certificate.md#private-certificate-requirements)karşılayan [App Service özel bir sertifika ekleyin](configure-ssl-certificate.md) .
-  Karşılık gelen özel etki alanına bir TLS bağlaması oluşturun. Bu ikinci adım bu makalede ele alınmıştır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Uygulamanızın fiyatlandırma katmanını yükseltme
> * Özel bir etki alanını sertifikayla güvenli hale getirme
> * HTTPS'yi zorunlu tutma
> * TLS 1.1/1.2 zorlama
> * TLS yönetimini betiklerle otomatikleştirme

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu izlemek için:

- [App Service uygulaması oluşturma](/azure/app-service/)
- [Uygulamanıza bir etki alanı adı eşleyin](app-service-web-tutorial-custom-domain.md) veya [Azure 'da satın alıp yapılandırın](manage-custom-dns-buy-domain.md)
- [Uygulamanıza özel bir sertifika ekleyin](configure-ssl-certificate.md)

> [!NOTE]
> Özel bir sertifika eklemenin en kolay yolu, [ücretsiz App Service yönetilen bir sertifika oluşturmaktır](configure-ssl-certificate.md#create-a-free-certificate-preview) (Önizleme).

[!INCLUDE [Prepare your web app](../../includes/app-service-ssl-prepare-app.md)]

<a name="upload"></a>

## <a name="secure-a-custom-domain"></a>Özel bir etki alanının güvenliğini sağlama

Aşağıdaki adımları uygulayın:

<a href="https://portal.azure.com" target="_blank">Azure Portal</a>, sol menüden **uygulama hizmetleri**' ni seçin  >  **\<app-name>** .

Uygulamanızın sol gezinti bölmesinde, **TLS/SSL bağlama** iletişim kutusunu şu şekilde başlatın:

- **Özel etki alanlarını**seçme  >  **bağlama ekleme**
- TLS/ **SSL ayarlarını**seçme  >  **TLS/SSL bağlaması Ekle**

![Etki alanına bağlama ekleme](./media/configure-ssl-bindings/secure-domain-launch.png)

**Özel etki alanı**' nda, bir bağlama eklemek istediğiniz özel etki alanını seçin.

Uygulamanızda zaten seçili özel etki alanı için bir sertifika varsa doğrudan [bağlama oluştur](#create-binding) ' a gidin. Aksi takdirde, devam edin.

### <a name="add-a-certificate-for-custom-domain"></a>Özel etki alanı için sertifika ekleme

Uygulamanızda seçili özel etki alanı için sertifika yoksa, iki seçeneğiniz vardır:

- **PFX sertifikasını karşıya yükleme** - [özel bir sertifikayı karşıya yükleme](configure-ssl-certificate.md#upload-a-private-certificate)sırasında iş akışını izleyin ve burada bu seçeneği belirleyin.
- **Içeri aktarma App Service sertifikası** - [App Service sertifikasını içeri aktarma](configure-ssl-certificate.md#import-an-app-service-certificate)sırasında iş akışını izleyin ve burada bu seçeneği belirleyin.

> [!NOTE]
> Ayrıca, [ücretsiz bir sertifika](configure-ssl-certificate.md#create-a-free-certificate-preview) (Önizleme) oluşturabilir veya [bir Key Vault sertifikasını içeri aktarabilirsiniz](configure-ssl-certificate.md#import-a-certificate-from-key-vault), ancak bunu ayrı olarak yapmanız ve sonra **TLS/SSL bağlama** iletişim kutusuna geri dönmeniz gerekir.

### <a name="create-binding"></a>Bağlama oluştur

TLS **/SSL bağlama** ILETIŞIM kutusunda TLS bağlamasını yapılandırmanıza yardımcı olması için aşağıdaki tabloyu kullanın, sonra **bağlama Ekle**' ye tıklayın.

| Ayar | Açıklama |
|-|-|
| Özel etki alanı | İçin TLS/SSL bağlamasını eklemek için etki alanı adı. |
| Özel sertifika parmak Izi | Bağlanacak sertifika. |
| TLS/SSL türü | <ul><li>**[SNI SSL](https://en.wikipedia.org/wiki/Server_Name_Indication)** -çoklu SNI SSL bağlamaları eklenebilir. Bu seçenek, birden fazla TLS/SSL sertifikasının aynı IP adresindeki birden fazla etki alanının güvenliğini sağlar. En modern tarayıcıların (Internet Explorer, Chrome, Firefox ve Opera dahil) SNı desteği (daha fazla bilgi için bkz. [sunucu adı belirtme](https://wikipedia.org/wiki/Server_Name_Indication)).</li><li>**IP SSL** -yalnızca bir IP SSL bağlama eklenebilir. Bu seçenek, yalnızca bir TLS/SSL sertifikasının ayrılmış bir genel IP adresinin güvenliğini sağlamasına izin verir. Bağlamayı yapılandırdıktan sonra [IP SSL kayıtları](#remap-records-for-ip-ssl)yeniden eşleme bölümündeki adımları uygulayın.<br/>IP SSL yalnızca **Standart** katmanda veya yukarıda desteklenir. </li></ul> |

İşlem tamamlandıktan sonra, özel etki alanının TLS/SSL durumu **güvenli**olarak değiştirilir.

![TLS/SSL bağlama başarılı](./media/configure-ssl-bindings/secure-domain-finished.png)

> [!NOTE]
> **Özel etki alanlarında** **güvenli** bir durum, bir sertifikayla güvenli hale getirildiği anlamına gelir, App Service ancak örneğin, tarayıcının otomatik olarak imzalanıp imzalanmadığını veya dolduğunu (örneğin, tarayıcıların bir hata veya uyarı görüntülemesine da neden olabilir) kontrol etmez.

## <a name="remap-records-for-ip-ssl"></a>IP SSL için kayıtları yeniden eşleyin

Uygulamanızda IP SSL kullanmıyorsanız, [özel etki alanınız Için test https](#test-https)'ye atlayın.

Yapmanız gereken iki değişiklik olabilir:

- Varsayılan olarak, uygulamanız paylaşılan bir genel IP adresi kullanır. Bir sertifikayı IP SSL bağladığınızda App Service, uygulamanız için yeni ve ayrılmış bir IP adresi oluşturur. Bir kaydı uygulamanızla eşleştirdiyseniz, etki alanı kayıt defterinizi bu yeni, ayrılmış IP adresiyle güncelleştirin.

    Uygulamanızın **özel etki alanı** sayfası, yeni ve ayrılmış IP adresi ile güncelleştirilir. [Bu IP adresini kopyalayın](app-service-web-tutorial-custom-domain.md#info), ardından bu yeni IP adresine [A kaydını yeniden eşleyin](app-service-web-tutorial-custom-domain.md#map-an-a-record).

- ' A bir SNI SSL bağlamanız varsa `<app-name>.azurewebsites.net` , [HERHANGI bir CNAME eşlemesini](app-service-web-tutorial-custom-domain.md#map-a-cname-record) `sni.<app-name>.azurewebsites.net` (öneki ekleyin) işaret etmek için yeniden eşleyin `sni` .

## <a name="test-https"></a>HTTPS’yi test etme

Çeşitli tarayıcılarda, `https://<your.custom.domain>` uygulamanızı hizmet ettiğini doğrulamak için öğesine gidin.

![Azure uygulamasına portal gezintisi](./media/configure-ssl-bindings/app-with-custom-ssl.png)

Uygulama kodunuz, "x-appservice-proto" üst bilgisi aracılığıyla Protokolü inceleyebilir. Üst bilgi, veya değerine sahip olacaktır `http` `https` . 

> [!NOTE]
> Uygulamanız size sertifika doğrulama hataları veriyorsa, muhtemelen kendinden imzalı bir sertifika kullanıyorsunuz demektir.
>
> Böyle bir durum söz konusu değilse, sertifikanızı PFX dosyasına aktardığınızda ara sertifikaları dışarıda bırakmış olabilirsiniz.

## <a name="prevent-ip-changes"></a>IP değişikliklerini engelle

Bağlama IP SSL olsa bile, bir bağlamayı sildiğinizde gelen IP adresiniz değişebilir. Bu, daha önce IP SSL bağlamasındaki bir sertifikayı yenileçalıştığınızda özellikle önemlidir. Uygulamanızın IP adresinin değişmesini önlemek için şu adımları sırasıyla izleyin:

1. Yeni sertifikayı karşıya yükleyin.
2. Eskisini silmeden yeni sertifikayı istediğiniz özel etki alanına bağlayın. Bu eylem, eskisini kaldırmak yerine bağlamayı değiştirir.
3. Eski sertifikayı silin. 

## <a name="enforce-https"></a>HTTPS'yi zorunlu tutma

Varsayılan olarak, herhangi biri HTTP kullanarak uygulamanıza erişmeye devam edebilir. Tüm HTTPS isteklerini HTTP bağlantı noktasına yeniden yönlendirebilirsiniz.

Uygulama sayfanızda, sol gezinti bölmesinde **SSL ayarları**' nı seçin. Ardından **Yalnızca HTTPS** menüsünde **Açık**’ı seçin.

![HTTPS'yi zorunlu tutma](./media/configure-ssl-bindings/enforce-https.png)

İşlem tamamlandığında, uygulamanıza işaret eden HTTP URL'lerinden herhangi birine gidin. Örnek:

- `http://<app_name>.azurewebsites.net`
- `http://contoso.com`
- `http://www.contoso.com`

## <a name="enforce-tls-versions"></a>TLS sürümlerini zorlama

Uygulamanız varsayılan olarak [TLS](https://wikipedia.org/wiki/Transport_Layer_Security) 1.2 sürümüne izin verir. Bu, [PCI DSS](https://wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard) gibi endüstri standartlarınca önerilen TLS düzeyidir. Farklı TLS sürümlerini zorlamak için aşağıdaki adımları uygulayın:

Uygulama sayfanızda, sol gezinti bölmesinde **SSL ayarları**' nı seçin. Ardından **TLS sürümü**’nde istediğiniz en düşük TLS sürümünü seçin. Bu ayar yalnızca gelen çağrıları denetler. 

![TLS 1.1 veya 1.2’yi zorlama](./media/configure-ssl-bindings/enforce-tls1-2.png)

İşlem tamamlandığında, uygulamanız daha düşük TLS sürümleriyle tüm bağlantıları reddeder.

## <a name="handle-tls-termination"></a>TLS sonlandırmayı işle

App Service, ağ yükü dengeleyicilerde [TLS sonlandırması](https://wikipedia.org/wiki/TLS_termination_proxy) olur, bu nedenle tüm https istekleri UYGULAMANıZA şifrelenmemiş HTTP istekleri olarak ulaşacak. Uygulama mantığınızın kullanıcı isteklerinin şifrelenip şifrelenmediğini denetlemesi gerekiyorsa, `X-Forwarded-Proto` üstbilgiyi inceleyin.

[Linux Node.js yapılandırma](configure-language-nodejs.md#detect-https-session) kılavuzu gibi dile özgü yapılandırma kılavuzlarında, uygulama KODUNUZDA bir https oturumunun nasıl algılanacağı gösterilir.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

### <a name="azure-cli"></a>Azure CLI

[!code-azurecli[main](../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom TLS/SSL certificate to a web app")] 

### <a name="powershell"></a>PowerShell

[!code-powershell[main](../../powershell_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.ps1?highlight=1-3 "Bind a custom TLS/SSL certificate to a web app")]

## <a name="more-resources"></a>Diğer kaynaklar

* [Kodunuzda bir TLS/SSL sertifikası kullanın Azure App Service](configure-ssl-certificate-in-code.md)
* [SSS: sertifikalar App Service](https://docs.microsoft.com/azure/app-service/faq-configuration-and-management/)

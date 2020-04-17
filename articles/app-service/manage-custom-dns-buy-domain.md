---
title: Özel bir etki alanı adı satın alma
description: Bir Uygulama Hizmeti etki alanını nasıl satın alacağınızı ve uygulamanız Azure Uygulama Hizmeti için özel bir etki alanı olarak nasıl kullanacağınızı öğrenin.
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.topic: article
ms.date: 11/24/2017
ms.custom: seodec18
ms.openlocfilehash: 47daf4ecd034c390a1460610e78d4fffd9277ac7
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81535715"
---
# <a name="buy-a-custom-domain-name-for-azure-app-service"></a>Azure App Service için bir özel etki alanı adı satın alma

Uygulama Hizmeti etki alanları, doğrudan Azure'da yönetilen üst düzey etki alanlarıdır. [Azure Uygulama Hizmeti](overview.md)için özel etki alanlarını yönetmeyi kolaylaştırırlar. Bu öğretici, bir Uygulama Hizmeti etki alanını nasıl satın alacağınızı ve Azure Uygulama Hizmeti'ne DNS adlarını nasıl atayabileceğinizi gösterir.

Azure VM veya Azure Depolama için Bkz. [Uygulama Hizmeti etki alanını Azure VM veya Azure Depolama'ya ata.](https://azure.github.io/AppService/2017/07/31/Assign-App-Service-domain-to-Azure-VM-or-Azure-Storage) Bulut Hizmetleri için bkz: [Azure bulut hizmeti için özel bir etki alanı adı yapılandırma.](../cloud-services/cloud-services-custom-domain-name-portal.md)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için:

* [Bir App Service uygulaması oluşturun](/azure/app-service/) veya başka bir öğretici için oluşturduğunuz bir uygulamayı kullanın.
* [Aboneliğinizdeki harcama sınırını kaldırın.](../cost-management-billing/manage/spending-limit.md#remove) Ücretsiz abonelik kredisi ile App Service etki alanlarını satın alamazsınız.

## <a name="prepare-the-app"></a>Uygulamayı hazırlama

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Azure Uygulama Hizmeti'nde özel etki alanlarını kullanmak için uygulamanızın [Uygulama Hizmeti planının](https://azure.microsoft.com/pricing/details/app-service/) ücretli bir katman **(Paylaşılan,** **Temel,** **Standart**veya **Premium)** olması gerekir. Bu adımda, uygulamanın desteklenen fiyatlandırma katmanında olduğundan emin olun.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com)'ı açın ve Azure hesabınızla oturum açın.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Azure Portal'da uygulamaya gitme

Sol menüden **Uygulama Hizmetleri**'ni ve ardından uygulamanın adını seçin.

![Azure uygulamasına portal gezintisi](./media/app-service-web-tutorial-custom-domain/select-app.png)

App Service uygulamasının yönetim sayfasını görüyorsunuz.  

### <a name="check-the-pricing-tier"></a>Fiyatlandırma katmanını denetleme

Uygulama sayfasının sol gezintisini **Ayarlar** bölümüne kaydırın ve **Ölçeği artır (App Service planı)** öğesini seçin.

![Ölçeği artır menüsü](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Uygulamanın geçerli katmanı mavi kenarlıkla vurgulanmıştır. Uygulamanın **F1** katmanında olmadığından emin olun. **F1** katmanında özel DNS desteklenmez. 

![Fiyatlandırma katmanını denetleyin](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Uygulama Hizmeti planı **F1** katmanında değilse, **Ölçeklendirme** sayfasını kapatın ve [etki alanını satın](#buy-the-domain)almak için atlayın.

### <a name="scale-up-the-app-service-plan"></a>App Service planının ölçeğini artırma

Ücretsiz olmayan katmanlardan birini seçin (**D1**, **B1**, **B2**, **B3** veya **Üretim** kategorisindeki herhangi bir katmanı). Ek seçenekler için **Ek seçeneklere bakın**’a tıklayın.

**Uygula**’ya tıklayın.

![Fiyatlandırma katmanını denetleyin](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Aşağıdaki bildirimi gördüğünüzde, ölçeklendirme işlemi tamamlanmıştır.

![Ölçeklendirme işlemi onayı](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-the-domain"></a>Etki alanını satın alın

### <a name="pricing-information"></a>Fiyatlandırma Bilgileri
Azure Uygulama Hizmeti Etki Alanları hakkında fiyatlandırma bilgileri [için, App Service Fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/app-service/windows/) ziyaret edin ve App Service Domain'e gidin.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma
[Azure Portal](https://portal.azure.com/)'ı açın ve Azure hesabınızla oturum açın.

### <a name="launch-buy-domains"></a>Satın Alma etki alanlarını başlatın
Uygulama **Hizmetleri** sekmesinde, uygulamanızın adını tıklatın, **Ayarlar'ı**seçin ve ardından **Özel etki alanlarını** seçin
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

Özel **etki alanları** sayfasında, **Etki Alanı Satın'ı**tıklatın.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

> [!NOTE]
> **App Service Etki Alanları** bölümünü göremiyorsanız, Azure hesabınızdaki harcama sınırını kaldırmanız gerekir [(Bkz. Önkoşullar).](#prerequisites)
>
>

### <a name="configure-the-domain-purchase"></a>Etki alanı satın alma yapılandırma

App **Service Domain** sayfasında, **etki alanı için arama** kutusuna, satın almak `Enter`istediğiniz etki alanı adını yazın ve yazın. Önerilen kullanılabilir etki alanları metin kutusunun hemen altında gösterilir. Satın almak istediğiniz bir veya daha fazla etki alanı seçin.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

> [!NOTE]
> Aşağıdaki [üst düzey etki alanları](https://wikipedia.org/wiki/Top-level_domain) App Service etki alanları tarafından desteklenir: _com_, _net_, _co.uk_, _org_, _nl_, _in_, _biz_, _org.uk_, ve _co.in_.
>
>

İletişim **Bilgileri'ni** tıklatın ve alan adının iletişim bilgileri formunu doldurun. Tamamlandığında, Uygulama Hizmeti Etki Alanı sayfasına dönmek için **Tamam'ı** tıklatın.

Gerekli tüm alanları mümkün olduğunca doğru bir şekilde doldurmanız önemlidir. Kişi bilgileri için yanlış veriler etki alanlarının satın alınamamasıyla sonuçlanabilir.

Ardından, etki alanınız için istenen seçenekleri seçin. Açıklamalar için aşağıdaki tabloya bakın:

| Ayar | Önerilen Değer | Açıklama |
|-|-|-|
|Gizlilik koruması | Etkinleştirme | Ücretsiz satın alma fiyatına dahil olan "Gizlilik koruması"nı tercih _edin._ Bazı üst düzey etki alanları, gizlilik korumasını desteklemeyen kayıt şirketleri tarafından yönetilir ve **Gizlilik koruması** sayfasında listelenir. |
| Varsayılan ana bilgisayar adlarını atama | **www** ve**\@** | İstenirse istenen ana bilgisayar adı bağlamalarını seçin. Etki alanı satın alma işlemi tamamlandığında, uygulamanız seçili ana bilgisayar adlarında erişilebilir. Uygulama [Azure Trafik Yöneticisi'nin](https://azure.microsoft.com/services/traffic-manager/)arkasındaysa, Trafik Yöneticisi A kayıtlarını desteklemediği için kök etki alanını (@) atama seçeneğini görmezsiniz. Etki alanı satın alma işlemi tamamlandıktan sonra ana bilgisayar adı atamalarında değişiklik yapabilirsiniz. |

### <a name="accept-terms-and-purchase"></a>Şartları kabul et ve satın alma

Koşulları ve ücretleri incelemek için **Yasal Terimler'i** tıklatın, ardından **Satın Al'ı**tıklatın.

> [!NOTE]
> Uygulama Hizmeti Etki Alanları etki alanı kaydı için GoDaddy'yi ve etki alanlarını barındırmak için Azure DNS'yi kullanır. Etki alanı kayıt ücretine ek olarak, Azure DNS için kullanım ücretleri de geçerlidir. Daha fazla bilgi için [Azure DNS Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/dns/)bakın.
>
>

**Uygulama Hizmeti Etki Alanı** sayfasına geri dön, **Tamam'ı**tıklatın. İşlem devam ederken, aşağıdaki bildirimleri görürsünüz:

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-the-hostnames"></a>Ana bilgisayar adlarını test edin

Uygulamanıza varsayılan ana bilgisayar adları atadıysanız, seçilen her ana bilgisayar adı için bir başarı bildirimi de görürsünüz.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

Ayrıca, Özel **Ana Bilgisayar** Adları bölümünde, Özel etki alanları sayfasında seçili ana bilgisayar **adlarını** da görürsünüz.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

> [!NOTE]
> Özel etki alanınız için **Güvenli Olmayan** etiket, henüz bir TLS/SSL sertifikasına bağlı olmadığı anlamına gelir ve tarayıcıdan özel etki alanınıza herhangi bir HTTPS isteği tarayıcıya bağlı olarak bir hata veya uyarı alır. TLS bağlamayı yapılandırmak için Azure [Uygulama Hizmeti'nde TLS/SSL bağlayıcısı olan özel bir DNS adını güvenli](configure-ssl-bindings.md)olarak görün.
>

Ana bilgisayar adlarını sınamak için tarayıcıda listelenen ana bilgisayar adlarına gidin. Önceki ekran görüntüsündeki örnekte, _kontoso.net_ ve _www\.kontoso.net_gezinmeyi deneyin.

## <a name="assign-hostnames-to-app"></a>Uygulamaya ev sahibi adları atama

Satın alma işlemi sırasında uygulamanıza bir veya daha fazla varsayılan ana bilgisayar adı atamayın veya listede yer almayan bir ana bilgisayar adı atamanız gerekiyorsa, istediğiniz zaman bir ana bilgisayar adı atayabilirsiniz.

Ayrıca, App Service Etki Alanı'ndaki ana bilgisayar adlarını başka bir uygulamaya atayabilirsiniz. Adımlar, App Service Domain ve uygulamanın aynı aboneye ait olup olmadığına bağlıdır.

- Farklı abonelik: Uygulama Hizmeti Etki Alanı'ndan uygulamaya dışarıdan satın alınmış bir etki alanı gibi özel DNS kayıtlarını haritalandırın. Bir Uygulama Hizmeti Etki Alanına özel DNS adları ekleme hakkında bilgi [için](#custom)bkz. Harici satın alınan bir etki alanını bir uygulamaya eşlemek için, [varolan özel bir DNS adını Azure Uygulama Hizmeti'ne göre Eşle'ye](app-service-web-tutorial-custom-domain.md)bakın. 
- Aynı abonelik: Aşağıdaki adımları kullanın.

### <a name="launch-add-hostname"></a>Ana bilgisayar adını ekle'yi başlat
Uygulama **Hizmetleri** sayfasında, ana bilgisayar adlarını atamak istediğiniz uygulamanızın adını seçin, **Ayarlar'ı**seçin ve ardından **Özel etki alanlarını**seçin.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

Satın aldığınız etki alanının **App Service Etki Alanları** bölümünde listelenmiş olduğundan emin olun, ancak bu etki alanını seçmeyin. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> Aynı abonelikteki tüm Uygulama Hizmeti Etki Alanları, uygulamanın **Özel etki alanları** sayfasında gösterilir. Etki alanınız uygulamanın aboneliğindeyse, ancak uygulamanın **Özel etki alanları** sayfasında göremiyorsanız, **Özel etki alanları** sayfasını yeniden açmayı deneyin veya web sayfasını yenileyin. Ayrıca, ilerleme veya oluşturma hataları için Azure portalının üst kısmındaki bildirim zilini kontrol edin.
>
>

**Konak adı ekle**'yi seçin.

### <a name="configure-hostname"></a>Ana bilgisayar adını yapılandırma
**Hostname Ekle** iletişim kutusunda, App Service Etki Alanadınızın veya herhangi bir alt etki alanının tam nitelikli alan adını yazın. Örneğin:

- kontoso.net
- www\.kontoso.net
- abc.kontoso.net

Tamamlandığında, **Doğrula'yı**seçin. Ana bilgisayar adı kaydı türü sizin için otomatik olarak seçilir.

**Konak adı ekle**'yi seçin.

İşlem tamamlandığında, atanan ana bilgisayar adı için bir başarı bildirimi görürsünüz.  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a>Ana bilgisayar adını ekle'yi kapat
Ana **bilgisayar adı ekle** sayfasında, istediğiniz gibi uygulamanıza başka bir ana bilgisayar adı atayın. Tamamlandığında, ana **bilgisayar adı ekle** sayfasını kapatın.

Artık uygulamanızın **Özel etki alanları** sayfasında yeni atanan ana bilgisayar adını görmeniz gerekir.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-the-hostnames"></a>Ana bilgisayar adlarını test edin

Tarayıcıda listelenen ana bilgisayar adlarına gidin. Önceki ekran görüntüsündeki örnekte, _abc.kontoso.net'_ e gezinmeyi deneyin.

## <a name="renew-the-domain"></a>Etki alanını yenileme

Satın aldığınız App Service etki alanı, satın alma tarihinden itibaren bir yıl süreyle geçerlidir. Varsayılan olarak, etki alanı, gelecek yıl için ödeme yönteminizi şarj ederek otomatik olarak yenilenir. Etki alanı adınızı el ile yenileyebilirsiniz.

Otomatik yenilemeyi kapatmak veya etki alanınızı el ile yenilemek istiyorsanız, aşağıdaki adımları izleyin.

Uygulama **Hizmetleri** sekmesinde, uygulamanızın adını tıklatın, **Ayarlar'ı**seçin ve ardından **Özel etki alanlarını**seçin.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

App **Service Domains** bölümünde, yapılandırmak istediğiniz etki alanını seçin.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

Etki alanının sol daki gezintisinden **Etki Alanı yenilemesini**seçin. Etki alanınızı otomatik olarak yenilemeyi durdurmak için **Kapalı**'yı ve ardından **Kaydet'i**seçin.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-autorenew.png)

Etki alanınızı el ile yenilemek **için etki alanını yenile'yi**seçin. Ancak, bu düğme [etki alanının sona ermesinden 90 gün öncesine](#when-domain-expires)kadar etkin değildir.

Etki alanı yenilemeniz başarılı olursa, 24 saat içinde bir e-posta bildirimi alırsınız.

## <a name="when-domain-expires"></a>Etki alanının süresi dolduğunda

Azure, süresi dolan veya süresi dolan Uygulama Hizmeti etki alanlarıyla aşağıdaki gibi ilgilenir:

* Otomatik yenileme devre dışı bırakılırsa: etki alanının sona ermesinden 90 gün önce, size bir yenileme bildirimi e-postası gönderilir ve portalda **etki alanını yenile** düğmesi etkinleştirilir.
* Otomatik yenileme etkinse: Etki alanının son kullanma tarihinden sonraki gün, Azure alan adı yenilemeiçin size fatura landırmayı dener.
* Otomatik yenileme sırasında bir hata oluşursa (örneğin, dosyadaki kartınızın süresi doldu) veya otomatik yenileme devre dışı bırakılırsa ve etki alanının süresinin dolmasına izin verirseniz, Azure etki alanının sona ermesini size bildirer ve alan adınızı park eder. Etki alanınızı [el ile yenileyebilirsiniz.](#renew-the-domain)
* Azure, son kullanma tarihinden sonraki 4 ve 12 gün içinde size ek bildirim e-postaları gönderir. Etki alanınızı [el ile yenileyebilirsiniz.](#renew-the-domain)
* Sona erme tarihinden sonraki 19. Geçerli yenileme ve kullanım ücretlerine tabi olarak alan adınızı yenilemek için müşteri desteğini arayabilirsiniz.
* Sona erme tarihinden sonraki 25. Geçerli yenileme ve kullanım ücretlerine tabi olarak alan adınızı yenilemek için müşteri desteğini arayabilirsiniz.
* Sona erme tarihinden sonraki 30.

<a name="custom"></a>

## <a name="manage-custom-dns-records"></a>Özel DNS kayıtlarını yönetme

Azure'da, Bir Uygulama Hizmeti Etki Alanı'nın [DNS kayıtları Azure DNS](https://azure.microsoft.com/services/dns/)kullanılarak yönetilir. Harici olarak satın alınan bir etki alanı için olduğu gibi DNS kayıtlarını ekleyebilir, kaldırabilir ve güncelleştirebilirsiniz.

### <a name="open-app-service-domain"></a>Uygulama Hizmeti Etki Alanını Aç

Azure portalında, sol menüden **Tüm Hizmetler** > **Uygulama Hizmeti Etki Alanlarını**seçin.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Yönetilecek etki alanını seçin. 

### <a name="access-dns-zone"></a>DNS bölgesine erişim

Etki alanının sol menüsünde **DNS bölgesini**seçin.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

Bu eylem, Azure DNS'de Uygulama Hizmeti Etki Alanınızın [DNS bölge](../dns/dns-zones-records.md) sayfasını açar. DNS kayıtlarının nasıl değiştirilen hakkında bilgi için [Azure portalında DNS Bölgelerini nasıl yönetirebilirsiniz'e](../dns/dns-operations-dnszones-portal.md)bakın.

## <a name="cancel-purchase-delete-domain"></a>Satın alma işlemini iptal etme (etki alanını silme)

App Service Domain'i satın aldıktan sonra, tam bir geri ödeme için satın alma işlemini iptal etmek için beş gününüz vardır. Beş gün sonra, App Service Etki Alanını silebilirsiniz, ancak geri ödeme alamazsınız.

### <a name="open-app-service-domain"></a>Uygulama Hizmeti Etki Alanını Aç

Azure portalında, sol menüden **Tüm Hizmetler** > **Uygulama Hizmeti Etki Alanlarını**seçin.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

İptal etmek veya silmek istediğiniz etki alanını seçin. 

### <a name="delete-hostname-bindings"></a>Ana bilgisayar adı bağlamalarını silme

Etki alanının sol menüsünde **Ana Bilgisayar adı bağlamalarını**seçin. Tüm Azure hizmetlerinin ana bilgisayar adı bağlamaları burada listelenmiştir.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

Tüm ana bilgisayar adı bağlamaları silinene kadar Uygulama Hizmeti Etki Alanını silemezsiniz.

Seçerek her ana bilgisayar adı bağlama silin **...**  >  **Sil.** Tüm bağlamalar silindikten sonra **Kaydet'i**seçin.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a>İptal etme veya silme

Etki alanının sol menüsünde **Genel Bakış'ı**seçin. 

Satın alınan etki alanında iptal süresi dolmadıysa, **satın alma işlemini iptal**et'i seçin. Aksi takdirde, bunun yerine bir **Sil** düğmesi görürsünüz. Etki alanını para iadesi olmadan silmek için **Sil'i**seçin.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

İşlemi onaylamak için **Evet'i**seçin.

İşlem tamamlandıktan sonra, etki alanı aboneliğinizden serbest bırakılır ve herkesin yeniden satın alamaması için kullanılabilir. 

## <a name="direct-default-url-to-a-custom-directory"></a>Varsayılan URL'yi özel bir dizine yönlendirme

Varsayılan olarak, App Service web isteklerini uygulama kodunuzun kök dizinine yönlendirir. Onları bir alt dizine yönlendirmek `public`için , [bkz.](app-service-web-tutorial-custom-domain.md#virtualdir)

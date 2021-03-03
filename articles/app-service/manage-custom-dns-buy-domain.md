---
title: Özel bir etki alanı adı satın alın
description: App Service etki alanı satın almayı ve uygulama Azure App Service için özel etki alanı olarak kullanmayı öğrenin.
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.topic: article
ms.date: 11/30/2020
ms.custom: seodec18
ms.openlocfilehash: cdcf22a42375949cc4d6be0b4f3062cee26219d6
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101704863"
---
# <a name="buy-a-custom-domain-name-for-azure-app-service"></a>Azure App Service için bir özel etki alanı adı satın alma

App Service etki alanları doğrudan Azure 'da yönetilen özel etki alanlardır. [Azure App Service](overview.md)için özel etki alanlarını yönetmeyi kolaylaştırır. Bu öğreticide, bir App Service etki alanı satın alma ve Azure App Service DNS adlarını atama işlemlerinin nasıl yapılacağı gösterilmektedir.

Azure VM veya Azure depolama için bkz. [Azure VM veya Azure Storage 'a App Service etki alanı atama](https://azure.github.io/AppService/2017/07/31/Assign-App-Service-domain-to-Azure-VM-or-Azure-Storage). Cloud Services için bkz. [Azure bulut hizmeti için özel etki alanı adı yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* [Bir App Service uygulaması oluşturun](./index.yml) veya başka bir öğretici için oluşturduğunuz bir uygulamayı kullanın. Uygulamanın bir Azure ortak bölgesinde olması gerekir. Şu anda Azure Ulusal bulutlar desteklenmez.
* [Aboneliğinizdeki harcama limitini kaldırın](../cost-management-billing/manage/spending-limit.md#remove). Ücretsiz abonelik kredilerine sahip App Service etki alanı satın alamaz.

## <a name="buy-an-app-service-domain"></a>App Service etki alanı satın alma

App Service etki alanları hakkında fiyatlandırma bilgileri için [App Service fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/app-service/windows/) ziyaret edin ve aşağı kaydırın App Service etki alanı.

1. [Azure Portal](https://portal.azure.com)'ı açın ve Azure hesabınızla oturum açın.

1. Arama çubuğunda **App Service etki alanları**' nı arayıp seçin.

    ![Azure App Service etki alanlarına Portal gezintisi](./media/app-service-web-tutorial-custom-domain/view-app-service-domains.png)

1. **App Service etki alanları** görünümünde **Ekle**' ye tıklayın.

    ![App Service etki alanlarında Ekle ' ye tıklayın](./media/app-service-web-tutorial-custom-domain/add-app-service-domain.png)

1. **App Service etki alanı oluşturma deneyiminin daha yeni sürümünü denemek Için tıklayın ' ı** seçin.

    ![Yeni deneyime sahip App Service etki alanı oluşturun](./media/app-service-web-tutorial-custom-domain/select-new-create-experience.png)

### <a name="basics-tab"></a>Temel bilgiler sekmesi

1. **Temel bilgiler** sekmesinde, aşağıdaki tabloyu kullanarak ayarları yapılandırın:  

   | Ayar  | Açıklama |
   | -------- | ----------- |
   | **Abonelik** | Etki alanını satın almak için kullanılacak abonelik. |
   | **Kaynak Grubu** | Etki alanının içine yerleştirilecek kaynak grubu. Örneğin, uygulamanızın bulunduğu kaynak grubu. |
   | **Etki alanı** | İstediğiniz etki alanını yazın. Örneğin, **contoso.com**. İstediğiniz etki alanı kullanılamıyorsa, kullanılabilir etki alanları önerilerindeki bir listeden seçim yapabilir veya farklı bir etki alanı deneyebilirsiniz. |

    > [!NOTE]
    > Aşağıdaki [üst düzey etki alanları](https://wikipedia.org/wiki/Top-level_domain) App Service etki alanları tarafından desteklenir: _com_, _net_, _Co.uk_, _org_, _nl_, _in_, _biz_, _org.uk_ ve _Co.in_.
    >
    >
    
2. İşiniz bittiğinde Ileri ' ye tıklayın **: iletişim bilgileri**.

### <a name="contact-information-tab"></a>İletişim bilgileri sekmesi

1. Etki alanı kaydı için [ICANN](https://go.microsoft.com/fwlink/?linkid=2116641) tarafından gereken bilgileri girin. 

    Gerekli tüm alanları mümkün olduğunca fazla doğrulukta doldurmanız önemlidir. İletişim bilgileri için yanlış veriler, etki alanının satın alınamaması sonucunda oluşabilir.

1. İşiniz bittiğinde **İleri: Gelişmiş**' e tıklayın.

### <a name="advanced-tab"></a>Gelişmiş sekmesi

1. **Gelişmiş** sekmesinde, isteğe bağlı ayarları yapılandırın:  

   | Ayar  | Açıklama |
   | -------- | ----------- |
   | **Otomatik yenileme** | Varsayılan olarak etkindir. App Service etki alanınız size tek yıllık artışlarla kaydedilir. Otomatik yenileme, etki alanı kaydlarınızın süresi dolana ve etki alanının sahipliğini koruduğunuzdan emin olur. Azure aboneliğiniz, yenileme sırasında yıllık etki alanı kaydı ücretini otomatik olarak ücretlendirilir. Geri çevirmek için **devre dışı bırak**' ı seçin. Otomatik yenileme devre dışıysa, [el ile yenileyebilirsiniz](#renew-the-domain). |
   | **Gizlilik koruması** | Varsayılan olarak etkindir. Gizlilik Koruması, etki alanı kayıt iletişim bilgilerinizi WHOSıS veritabanından gizler. Gizlilik Koruması, yıllık etki alanı kayıt ücretine zaten dahil edilmiştir. Geri çevirmek için **devre dışı bırak**' ı seçin. |

2. İşiniz bittiğinde **İleri: Etiketler**' e tıklayın.

### <a name="finish"></a>Son

1. **Etiketler** sekmesinde, App Service etki alanınız için istediğiniz etiketleri ayarlayın. Etiketleme, App Service etki alanlarını kullanmak için gerekli değildir, ancak [Azure 'da kaynaklarınızı yönetmenize yardımcı olan bir özelliktir](../azure-resource-manager/management/tag-resources.md).

1. Ileri ' ye tıklayın **: gözden geçir + oluştur**.

1. **Gözden geçir + oluştur** sekmesinde, etki alanı siparişinizi gözden geçirin. Tamamladığınızda **Oluştur**’a tıklayın.

    > [!NOTE]
    > App Service etki alanları, etki alanı kaydı için GoDaddy kullanır ve etki alanlarını barındırmak için Azure DNS. Yıllık etki alanı kayıt ücretine ek olarak Azure DNS için kullanım ücretleri uygulanır. Bilgi için bkz. [Azure DNS fiyatlandırması](https://azure.microsoft.com/pricing/details/dns/).
    >
    >

1. Etki alanı kaydı tamamlandığında **Kaynağa Git** düğmesine görürsünüz. Yönetim sayfasını görmek için seçin.

    ![App Service etki alanı oluşturuldu. Kaynağa git](./media/app-service-web-tutorial-custom-domain/deployment-complete.png)

Artık bu özel etki alanına bir App Service uygulaması atamaya hazırsınız.

## <a name="prepare-the-app"></a>Uygulamayı hazırlama

Özel bir DNS adını bir Web uygulamasına eşlemek için, Web uygulamasının [App Service planı](https://azure.microsoft.com/pricing/details/app-service/) ücretli bir katman (paylaşılan, temel, standart, Premium veya Azure Işlevleri için tüketim) olmalıdır. Bu adımda, App Service uygulamasının desteklenen bir fiyatlandırma katmanında olduğundan emin olursunuz.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Azure Portal'da uygulamaya gitme

1. Üst arama çubuğundan **uygulama hizmetleri**' ni arayıp seçin.

    ![Uygulama Hizmetleri arama](./media/app-service-web-tutorial-custom-domain/app-services.png)

1. Uygulamanın adını seçin.

    ![Azure uygulamasına portal gezintisi](./media/app-service-web-tutorial-custom-domain/select-app.png)

    App Service uygulamasının yönetim sayfasını görüyorsunuz.  

### <a name="check-the-pricing-tier"></a>Fiyatlandırma katmanını denetleme

1. Uygulama sayfasının sol gezintisini **Ayarlar** bölümüne kaydırın ve **Ölçeği artır (App Service planı)** öğesini seçin.

    ![Ölçeği artır menüsü](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

1. Uygulamanın geçerli katmanı mavi kenarlıkla vurgulanmıştır. Uygulamanın **F1** katmanında olmadığından emin olun. **F1** katmanında özel DNS desteklenmez. 

    :::image type="content" source="./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png" alt-text="Ölçek Genişletme (App Service planı) seçiliyken uygulama sayfasının sol gezinti menüsünün ekran görüntüsü.":::

1. App Service planı **F1** katmanında değilse, **Ölçek artırma** sayfasını kapatın ve [etki alanını satın almak](#buy-an-app-service-domain)için atlayın.

### <a name="scale-up-the-app-service-plan"></a>App Service planının ölçeğini artırma

1. Ücretsiz olmayan katmanlardan birini seçin (**D1**, **B1**, **B2**, **B3** veya **Üretim** kategorisindeki herhangi bir katmanı). Ek seçenekler için **Ek seçeneklere bakın**’a tıklayın.

1. **Uygula**’ya tıklayın.

    :::image type="content" source="./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png" alt-text="Üretim kategorisindeki üretim sekmesindeki özel etki alanı fiyatlandırma katmanlarının ekran görüntüsü, B1 planı ve Uygula düğmesi vurgulanır.":::

    Aşağıdaki bildirimi gördüğünüzde, ölçeklendirme işlemi tamamlanmıştır.

    ![Ölçeklendirme işlemi onayı](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="map-app-service-domain-to-your-app"></a>App Service etki alanını uygulamanıza eşleyin

Aynı abonelikte olduğu sürece, App Service etki alanındaki bir ana bilgisayar adını App Service bir uygulamayla eşleştirmek kolaydır. App Service etki alanını veya onun alt etki alanını doğrudan uygulamanızda eşleştirdiğinizde, Azure sizin için gerekli DNS kayıtlarını oluşturur.

> [!NOTE]
> Etki alanı ve uygulama farklı aboneliklerdeyse, App Service etki alanını, [dışarıdan satın alınan bir etki alanını eşleme gibi bir](app-service-web-tutorial-custom-domain.md)uygulamayla eşleştirmeniz yeterlidir. Bu durumda, Azure DNS dış etki alanı sağlayıcıdır ve [gereklı DNS kayıtlarını el ile eklemeniz](#manage-custom-dns-records)gerekir.
>

### <a name="map-the-domain"></a>Etki alanını eşleme

1. Uygulama sayfasının sol gezinti bölmesinde **Ayarlar** bölümüne gidip **özel etki alanları**' nı seçin.

    ![Özel etki alanları menüsünü gösteren ekran görüntüsü.](./media/app-service-web-tutorial-custom-domain/custom-domain-menu.png)

1. **Özel etki alanı ekle**'yi seçin.

    ![Konak adı Ekle öğesini gösteren ekran görüntüsü.](./media/app-service-web-tutorial-custom-domain/add-host-name-cname.png)

1. App Service etki alanını (örneğin, **contoso.com**) veya bir alt etki alanını (örneğin, **www.contoso.com**) yazın ve **Doğrula**' ya tıklayın.

    > [!NOTE]
    > App Service etki alanı adında bir yazım hatası yaptıysanız, sayfanın en altında, bazı DNS kayıtlarının eksik olduğunu söyleyen bir doğrulama hatası görüntülenir. Bu kayıtları bir App Service etki alanı için el ile eklemeniz gerekmez. Etki alanı adını doğru yazdığınızdan emin olun ve yeniden **Doğrula** ' ya tıklayın.
    >
    > ![Doğrulama hatası gösteren ekran görüntüsü.](./media/app-service-web-tutorial-custom-domain/verification-error-cname.png)

1. **Konak adı kayıt türünü** kabul edin ve **özel etki alanı Ekle**' ye tıklayın.

    ![Özel etki alanı Ekle düğmesini gösteren ekran görüntüsü.](./media/app-service-web-tutorial-custom-domain/validate-domain-name-cname.png)

1. Yeni özel etki alanının uygulamanın **özel etki alanları** sayfasında yansıtılması biraz zaman alabilir. Verileri güncelleştirmek için tarayıcıyı yenileyin.

    ![CNAME kaydını eklemeyi gösteren ekran görüntüsü.](./media/app-service-web-tutorial-custom-domain/cname-record-added.png)

    > [!NOTE]
    > Özel etki alanınız için **güvenli olmayan** bir etiket henüz bir TLS/SSL sertifikasına bağlanmamış demektir. Bir tarayıcıdan özel etki alanınızı HTTPS istekleri, tarayıcıya bağlı olarak bir hata veya uyarı alır. TLS bağlama eklemek için, bkz. [Azure App Service BIR TLS/SSL bağlaması ile özel BIR DNS adını güvenli hale getirme](configure-ssl-bindings.md).
    
### <a name="test-the-custom-domain"></a>Özel etki alanını sınama

Özel etki alanını test etmek için tarayıcıda bu sayfaya gidin.

## <a name="renew-the-domain"></a>Etki alanını yenileme

Satın aldığınız App Service etki alanı, satın alma zamanından itibaren bir yıl için geçerlidir. Varsayılan olarak, etki alanı, ödeme yönteminizin bir sonraki yıla göre doldurularak otomatik olarak yenilenecek şekilde yapılandırılmıştır. Etki alanı adınızı el ile yenileyebilirsiniz.

Otomatik yenilemeyi devre dışı bırakmak istiyorsanız veya etki alanınızı el ile yenilemek istiyorsanız buradaki adımları izleyin.

1. Arama çubuğunda **App Service etki alanları**' nı arayıp seçin.

    ![Azure App Service etki alanlarına Portal gezintisi](./media/app-service-web-tutorial-custom-domain/view-app-service-domains.png)

1. **App Service etki alanları** bölümünde, yapılandırmak istediğiniz etki alanını seçin.

1. Etki alanının sol gezinti noktasından **etki alanı yenileme**' yi seçin. Etki alanınızı otomatik olarak yenilemeyi durdurmak için **kapalı**' yı seçin. Ayar hemen yürürlüğe girer.

    ![Etki alanınızı otomatik olarak yenileme seçeneğini gösteren ekran görüntüsü.](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-autorenew.png)

    > [!NOTE]
    > Sayfadan uzaklaştığınızda, **Tamam**' a tıklayarak "kaydedilmemiş düzenlemeleriniz atılacak" hatasını göz ardı edin.
    >

Etki alanınızı el ile yenilemek için **etki alanını Yenile**' yi seçin. Ancak, bu düğme, [etki alanının süre sonundan 90 gün önce](#when-domain-expires)etkin değildir.

Etki alanı yenileme işlemi başarılı olursa, 24 saat içinde bir e-posta bildirimi alırsınız.

## <a name="when-domain-expires"></a>Etki alanının süresi dolarsa

Azure, süresi dolan veya süresi dolan App Service etki alanları ile aşağıdaki şekilde anlaşmalar:

* Otomatik yenileme devre dışıysa: etki alanı süre sonundan 90 gün önce, bir yenileme bildirimi e-postası gönderilir ve portalda **etki alanını Yenile** düğmesi etkinleştirilir.
* Otomatik yenileme etkinse: etki alanı sona erme tarihi dolduktan sonra Azure, etki alanı adı yenileme için sizi faturalandırma girişiminde bulunur.
* Otomatik yenileme sırasında bir hata oluşursa (örneğin, dosyanızdaki kartın süresi dolmuşsa) veya otomatik yenileme devre dışıysa ve etki alanının süresinin dolmasına izin verirseniz Azure, etki alanı süre sonunu ve etki alanı adınızı park süresini size bildirir. Etki alanınızı [el ile yenileyebilirsiniz](#renew-the-domain) .
* 4 ve 12. günün süresi dolduktan sonra Azure size ek bildirim e-postaları gönderir. Etki alanınızı [el ile yenileyebilirsiniz](#renew-the-domain) . Süre dolduktan sonra 5 gün sonra DNS çözümlemesi, süresi doldu etki alanı için durduruluyor.
* Süre dolduktan sonra 19. günde, etki alanınız tutuluyor kalır, ancak bir teminat ücretine tabi olur. Uygun yenileme ve kullanım ücretlerine tabi olmak üzere etki alanı adınızı yenilemek için müşteri desteğini çağırabilirsiniz.
* Süre dolduktan sonra 25. günde Azure, etki alanı adı endüstri açık servisi olan bir etki alanını açık eksiltmeye geçirir. Uygun yenileme ve kullanım ücretlerine tabi olmak üzere etki alanı adınızı yenilemek için müşteri desteğini çağırabilirsiniz.
* Süresi dolduktan sonra 30 gün sonra, etki alanınızı artık kullanamayacak.

<a name="custom"></a>

## <a name="manage-custom-dns-records"></a>Özel DNS kayıtlarını yönetme

Azure 'da, bir App Service etki alanı için DNS kayıtları [Azure DNS](https://azure.microsoft.com/services/dns/)kullanılarak yönetilir. Yalnızca dışarıdan satın alınan etki alanı gibi DNS kayıtlarını ekleyebilir, kaldırabilir ve güncelleştirebilirsiniz. Özel DNS kayıtlarını yönetmek için:

1. Arama çubuğunda **App Service etki alanları**' nı arayıp seçin.

    ![Azure App Service etki alanlarına Portal gezintisi](./media/app-service-web-tutorial-custom-domain/view-app-service-domains.png)

1. **App Service etki alanları** bölümünde, yapılandırmak istediğiniz etki alanını seçin.

1. **Genel bakış** SAYFASıNDA, **DNS kayıtlarını Yönet**' i seçin.

    ![DNS kayıtlarının nerede erişebileceğini gösteren ekran görüntüsü.](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

DNS kayıtlarını düzenleme hakkında daha fazla bilgi için, bkz. [Azure portal DNS bölgeleri yönetme](../dns/dns-operations-dnszones-portal.md).

## <a name="cancel-purchase-delete-domain"></a>Satınalmayı iptal et (etki alanını sil)

App Service etki alanını satın aldıktan sonra, satın alma işlemini tam geri ödeme için iptal etmeniz beş gününüz vardır. Beş günden sonra App Service etki alanını silebilirsiniz, ancak para iadesi alamazsınız.

1. Arama çubuğunda **App Service etki alanları**' nı arayıp seçin.

    ![Azure App Service etki alanlarına Portal gezintisi](./media/app-service-web-tutorial-custom-domain/view-app-service-domains.png)

1. **App Service etki alanları** bölümünde, yapılandırmak istediğiniz etki alanını seçin.

1. Etki alanının sol gezintisinde **konak adı bağlamaları**' nı seçin. Tüm Azure hizmetlerinden konak adı bağlamaları burada listelenmiştir.

    ![Konak adı bağlamaları sayfasını gösteren ekran görüntüsü.](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

1. ' İ seçerek her bir konak adı bağlamayı Sil **...**  >  **Silin**. Tüm bağlamalar silindikten sonra **Kaydet**' i seçin.

    <!-- ![Screenshot that shows where to delete the hostname bindings.](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png) -->

1. Etki alanının sol gezintisinde **genel bakış**' ı seçin. 

1. Satın alınan etki alanındaki iptal süresi dolmuşsa, **satınalmayı Iptal et**' i seçin. Aksi takdirde, bunun yerine bir **Sil** düğmesi görürsünüz. Etki alanını para iadesi olmadan silmek için **Sil**' i seçin.

    ![Satın alınan bir etki alanının nerede silineceğini veya iptal edildiğini gösteren ekran görüntüsü.](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

1. **Evet**' i seçerek işlemi onaylayın.

    İşlem tamamlandıktan sonra, etki alanı aboneliğinizden serbest bırakılır ve herkes tarafından yeniden satın alınabilir. 

## <a name="direct-default-url-to-a-custom-directory"></a>Varsayılan URL'yi özel bir dizine yönlendirme

Varsayılan olarak, App Service web isteklerini uygulama kodunuzun kök dizinine yönlendirir. Bunları gibi bir alt dizine yönlendirmek için `public` bkz. [özel bir dizine yeniden yönlendirme](app-service-web-tutorial-custom-domain.md#redirect-to-a-custom-directory).

## <a name="next-steps"></a>Sonraki adımlar

App Service için özel bir TLS/SSL sertifikası bağlamayı öğrenin.

> [!div class="nextstepaction"]
> [Azure App Service bir TLS bağlaması ile özel bir DNS adının güvenliğini sağlama](configure-ssl-bindings.md)

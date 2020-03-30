---
title: Sorun giderme Kerberos kısıtlı delegasyon - App Proxy
description: Uygulama Proxy için Sorun Giderme Kerberos Kısıtlı Delegasyon yapılandırmaları
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5e866f61409960447e17ecb50b035eabd53dc38
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74275689"
---
# <a name="troubleshoot-kerberos-constrained-delegation-configurations-for-application-proxy"></a>Uygulama Proxy için Sorun Giderme Kerberos kısıtlı delegasyon yapılandırmaları

SSO'nun yayımlanmış uygulamalara ulaşması için kullanılan yöntemler bir uygulamadan diğerine değişebilir. Azure Active Directory (Azure AD) Application Proxy'nin varsayılan olarak sunduğu seçeneklerden biri de Kerberos kısıtlı delegasyonudur (KCD). Kullanıcılarınızın kısıtlanmış Kerberos kimlik doğrulamasını arka uç uygulamalarına çalıştırmak için bir bağlayıcıyı yapılandırabilirsiniz.

KCD'yi etkinleştirme yordamı basittir. SSO'yu destekleyen çeşitli bileşenlerin ve kimlik doğrulama akışının genel bir şekilde anlaşılmasını gerektirmez. Ama bazen, KCD SSO beklendiği gibi çalışmıyor. Bu senaryoları gidermek için iyi bilgi kaynaklarına ihtiyacınız vardır.

Bu makalede, sorun giderme ve en yaygın sorunlardan bazılarını kendi kendine düzeltmeyardımcı olan tek bir başvuru noktası sağlar. Ayrıca daha karmaşık uygulama sorunlarının tanılarını da kapsamaktadır.

Bu makalede, aşağıdaki varsayımlar yapar:

- Azure AD Uygulama Proxy'sinin [Uygulama Proxy'si ile başlatılması](application-proxy-add-on-premises-application.md) ve KCD olmayan uygulamalara genel erişim in in in dağıtımı beklendiği gibi çalışır.
- Yayınlanan hedef uygulama Internet Information Services (IIS) ve Kerberos Microsoft uygulaması dayanmaktadır.
- Sunucu ve uygulama ana bilgisayarları tek bir Azure Etkin Dizin etki alanında yer almaktadır. Etki alanları ve orman senaryoları hakkında ayrıntılı bilgi için [KCD teknik incelemesine](https://aka.ms/KCDPaper)bakın.
- Konu uygulaması, ön kimlik doğrulaması etkinleştirilmiş bir Azure kiracısında yayımlanır. Kullanıcıların form tabanlı kimlik doğrulaması yla Azure'a kimlik doğrulamaları beklenir. Zengin istemci kimlik doğrulama senaryoları bu makale kapsamında değildir. Gelecekte bir noktada eklenebilir.

## <a name="prerequisites"></a>Ön koşullar

Azure AD Application Proxy birçok altyapı veya ortamtürüne dağıtılabilir. Mimariler kuruluştan kuruluşa değişir. KCD ile ilgili sorunların en yaygın nedenleri ortamlar değildir. Basit yanlış yapılandırmalar veya genel hatalar çoğu soruna neden olur.

Bu nedenle, sorun gidermeye başlamadan önce [Uygulama Proxy ile KCD SSO kullanarak](application-proxy-configure-single-sign-on-with-kcd.md) tüm ön koşulları yerine getirildiğinden emin olmak en iyisidir. 2012R2'de Kerberos kısıtlı delegasyonunun yapılandırılması bölümüne dikkat edin. Bu işlem, Windows'un önceki sürümlerinde KCD yapılandırmak için farklı bir yaklaşım kullanır. Ayrıca, bu hususlara dikkat edin:

- Etki alanı üyesi sunucunun belirli bir etki alanı denetleyicisiyle (DC) güvenli bir kanal iletişim kutusu açması sık rastlanan bir durumdur. Ardından sunucu herhangi bir zamanda başka bir iletişim kutusuna geçebilir. Bu nedenle, bağlayıcı ana bilgisayarlar yalnızca belirli yerel site DC'leriyle iletişim kurmakla sınırlı değildir.
- Etki alanları arası senaryolar, bağlayıcı ana bilgisayarını yerel ağ çevresinin dışında olabilecek DC'lere yönlendiren yönlendirmelere dayanır. Bu gibi durumlarda, trafiği diğer ilgili etki alanlarını temsil eden DC'lere göndermek de aynı derecede önemlidir. Değilse, temsilci başarısız olur.
- Mümkün seçeneğde, konektör ana bilgisayarları ve DC' ler arasına etkin IPS veya IDS cihazları yerleştirmekten kaçının. Bu aygıtlar bazen çok müdahaleci dir ve çekirdek RPC trafiğini bozar.

Basit senaryolarda delegasyonu test edin. Ne kadar çok değişken sunarsanız, o kadar çok değişkenle uğraşmak zorunda klabilirsiniz. Zamandan tasarruf etmek için testlerinizi tek bir konektörle sınırlandırın. Sorun çözüldükten sonra ek bağlayıcılar ekleyin.

Bazı çevresel faktörler de bir soruna katkıda bulunabilir. Bu etkenleri önlemek için, sınama sırasında mümkün olduğunca mimari en aza indirin. Örneğin, yanlış yapılandırılmış iç güvenlik duvarı ALA'ları yaygındır. Mümkünse, tüm trafiği bir bağlayıcıdan doğrudan DC'lere ve arka uç uygulamasına gönderin.

Konektörleri konumlandırmak için en iyi yer hedeflerine mümkün olduğunca yakındır. Test sırasında satır satıra oturur ve gereksiz karmaşıklık ekler ve soruşturmalarınızı uzatabilir.

KCD sorununu ne gösterir? KCD SSO'nun başarısız olduğuna dair çeşitli yaygın göstergeler vardır. Bir sorunun ilk işaretleri tarayıcıda görünür.

![Örnek: Yanlış KCD yapılandırma hatası](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic1.png)

![Örnek: Yetkilendirme eksik izinler nedeniyle başarısız oldu](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic2.png)

Bu görüntülerin her ikisi de aynı belirti gösterir: SSO yetmezliği. Kullanıcının uygulamaya erişimi engellendi.

## <a name="troubleshooting"></a>Sorun giderme

Nasıl sorun giderme sorunu ve gözlemlemek belirtileri bağlıdır. Daha ileri gitmeden önce, aşağıdaki makaleleri keşfedin. Yararlı sorun giderme bilgileri sağlarlar:

- [Sorun Giderme Uygulama Proxy sorunları ve hata iletileri](application-proxy-troubleshoot.md)
- [Kerberos hataları ve belirtileri](application-proxy-troubleshoot.md#kerberos-errors)
- [Şirket içi ve bulut kimlikleri aynı olmadığında SSO ile çalışmak](application-proxy-configure-single-sign-on-with-kcd.md#working-with-different-on-premises-and-cloud-identities)

Eğer bu noktaya var, o zaman ana sorunu var. Başlamak için, akışı sorun giderebileceğiniz aşağıdaki üç aşamaya ayırın.

### <a name="client-pre-authentication"></a>İstemci ön kimlik doğrulama

Bir tarayıcı üzerinden Azure'a kimlik doğrulaması veren harici kullanıcı. KCD SSO'nun çalışması için Azure'a ön kimlik belirleme özelliği gereklidir. Herhangi bir sorun varsa bu yeteneği test edin ve ele alın. Ön kimlik doğrulama aşaması KCD veya yayımlanmış uygulamayla ilgili değildir. Konu hesabının Azure'da var olduğunu kontrol ederek tutarsızlıkları gidermek kolaydır. Ayrıca devre dışı bırakılmadığını veya engellenmediğini kontrol edin. Tarayıcıdaki hata yanıtı, nedenini açıklayacak kadar açıklayıcıdır. Emin değilseniz, doğrulamak için diğer Microsoft sorun giderme makalelerini kontrol edin.

### <a name="delegation-service"></a>Heyet hizmeti

Bir Kerberos Anahtar Dağıtım Merkezi'nden (KCD) kullanıcılar için Kerberos servis bileti alan Azure Proxy bağlayıcısı.

İstemci ve Azure ön ucu arasındaki dış iletişimin KCD ile hiçbir ilgisi yoktur. Bu iletişimler sadece KCD'nin çalıştığından emin olun. Azure Proxy hizmetine Kerberos bileti almak için kullanılan geçerli bir kullanıcı kimliği sağlanır. Bu kimlik olmadan, KCD mümkün değildir ve başarısız olur.

Daha önce de belirtildiği gibi, tarayıcı hata iletileri neden şeyler başarısız hakkında bazı iyi ipuçları sağlar. Yanıttaki etkinlik kimliğini ve zaman damgasını not edin. Bu bilgiler, davranışı Azure Proxy olay günlüğündeki gerçek olaylarla ilişkilendirmenize yardımcı olur.

![Örnek: Yanlış KCD yapılandırma hatası](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic3.png)

Olay günlüğünde görülen karşılık gelen girişler 13019 veya 12027 olayları olarak gösterir. **Uygulamalar ve Hizmetler Günlükleri** &gt; **Microsoft** &gt; **AadApplicationProxy** &gt; **Bağlayıcı** &gt; **Yönetici**bağlayıcı olay günlükleri bulun.

![Uygulama Proxy olay günlüğünden Olay 13019](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic4.png)

![Uygulama Proxy olay günlüğünden Olay 12027](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic5.png)

1. CName için değil, uygulamanın adresi için dahili DNS'nizde **bir A** kaydı kullanın. **CName**
1. Bağlayıcı ana bilgisayara belirlenen hedef hesabın SPN'sine temsilci verme hakkı verildiğini yeniden onaylayın. **Herhangi bir kimlik doğrulama iletişim kuralını kullan'ın** seçildiğini yeniden onaylayın. Daha fazla bilgi için [SSO yapılandırma makalesine](application-proxy-configure-single-sign-on-with-kcd.md)bakın.
1. Azure AD'de SPN'nin yalnızca bir örneği olduğunu doğrulayın. Herhangi `setspn -x` bir etki alanı üye ana bilgisayarüzerinde bir komut istemi sorunu.
1. [Verilen Kerberos belirteçlerinin maksimum boyutunu](https://blogs.technet.microsoft.com/askds/2012/09/12/maxtokensize-and-windows-8-and-windows-server-2012/)sınırlayan bir etki alanı ilkesinin uygulandığını denetleyin. Bu ilke, aşırı olduğu tespit edilirse bağlayıcının bir belirteç almamasını durdurur.

Bağlayıcı ana bilgisayar ve etki alanı KDC arasındaki alışverişleri yakalayan bir ağ izleme sorunları daha düşük düzeyli ayrıntı almak için bir sonraki en iyi adımdır. Daha fazla bilgi için [derin dalış Sorun Giderme kağıdına](https://aka.ms/proxytshootpaper)bakın.

Biletleme iyi görünüyorsa, günlüklerde uygulama 401 döndürdüğü için kimlik doğrulamanın başarısız olduğunu belirten bir olay görürsünüz. Bu olay, hedef uygulamanın biletinizi reddettiğini gösterir. Bir sonraki aşamaya geç.

### <a name="target-application"></a>Hedef uygulama

Konektör tarafından sağlanan Kerberos bilet tüketici. Bu aşamada, konektörün arka uca bir Kerberos servis bileti göndermesini bekleyin. Bu bilet, ilk başvuru isteğinde bir üstbilgidir.

1. Portalda tanımlanan uygulamanın dahili URL'sini kullanarak, uygulamanın doğrudan bağlayıcı ana bilgisayardaki tarayıcıdan erişilebildiğinden doğrulayın. Daha sonra başarılı bir şekilde oturum açabilirsiniz. Ayrıntılar bağlayıcı **Sorun Giderme** sayfasında bulunabilir.
1. Hala bağlayıcı ana bilgisayarda, tarayıcı ve uygulama arasındaki kimlik doğrulamakerberos kullandığını doğrulayın. Aşağıdaki eylemlerden birini uygulayın:
1. Internet Explorer'da DevTools **(F12)** çalıştırın veya bağlayıcı ana bilgisayardan [Fiddler'ı](https://blogs.msdn.microsoft.com/crminthefield/2012/10/10/using-fiddler-to-check-for-kerberos-auth/) kullanın. Dahili URL'yi kullanarak uygulamaya gidin. Teklif edilen WWW yetkilendirme üstbilgisini inceleyerek, başvurudan gelen yanıtta, müzakere veya Kerberos'un bulunduğundan emin olun.

   - Uygulama tarayıcıdan yanıt döndürülür sonraki Kerberos blob **YII**ile başlar. Bu mektuplar sana Kerberos'un kaçtığını söylüyor. Microsoft NT LAN Manager (NTLM), diğer taraftan, her zaman **TlRMTVNTUAAB**ile başlar , Hangi NTLM Güvenlik Destek Sağlayıcısı (NTLMSSP) Base64 deşifre okur. Eğer blob başında **TlRMTVNTUAAB** görürseniz, Kerberos mevcut değildir. **Eğer TlRMTVNTUAAB**görmüyorsanız, Kerberos büyük olasılıkla kullanılabilir.

      > [!NOTE]
      > Fiddler kullanıyorsanız, bu yöntem, uygulamanın IIS yapılandırmasında genişletilmiş korumayı geçici olarak devre dışı bilebilir.

      ![Tarayıcı ağı denetim penceresi](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic6.png)

   - Bu rakam blob **TIRMTVNTUAAB**ile başlamaz. Yani bu örnekte, Kerberos mevcuttur ve Kerberos blob **YII**ile başlamaz .

1. NTLM'yi Geçici olarak IIS sitesindeki sağlayıcılar listesinden kaldırın. Uygulamaya doğrudan Internet Explorer'dan konektör ana bilgisayarından erişin. NTLM artık sağlayıcılar listesinde değil. Uygulamaya sadece Kerberos'u kullanarak erişebilirsiniz. Erişim başarısız olursa, uygulamanın yapılandırmasıyla ilgili bir sorun olabilir. Kerberos kimlik doğrulaması çalışmıyor.

   - Kerberos kullanılamıyorsa, Uygulamanın kimlik doğrulama ayarlarını IIS'de denetleyin. **Negotiate'in** hemen altında NTLM ile en üstte listelendirilediğinden emin olun. Eğer **Değil Müzakere**görürseniz , Kerberos veya Müzakere , veya **PKU2U**, Sadece **Kerberos**işlevsel ise devam edin.

     ![Windows kimlik doğrulama sağlayıcıları](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic7.png)

   - Kerberos ve NTLM'nin yerinde olmasıyla, portaldaki uygulama için ön kimlik doğrulamayı geçici olarak devre dışı bırakma. Harici URL'yi kullanarak internetten erişmeye çalışın. Kimlik doğrulamanız istenir. Bunu, önceki adımda kullanılan aynı hesapla yapabilirsiniz. Değilse, arka uç uygulaması ile ilgili bir sorun var, KCD değil.
   - Portalda ön kimlik doğrulamayı yeniden etkinleştirin. Uygulamanın harici URL'si üzerinden bağlanmaya çalışarak Azure üzerinden kimlik doğrulaması yap. SSO başarısız olursa, tarayıcıda ve günlükte 13022 olayında yasak bir hata iletisi görürsünüz:

     *Arka uç sunucusu Kerberos kimlik doğrulama girişimlerine http 401 hatasıyla yanıt verdiği için Microsoft AAD Application Proxy Bağlayıcısı kullanıcının kimliğini doğrulayamaz.*

      ![HTTTP 401 yasak hata gösterir](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic8.png)

   - IIS uygulamasını kontrol edin. Yapılandırılan uygulama havuzunun ve SPN'nin Azure AD'de aynı hesabı kullanacak şekilde yapılandırıldığından emin olun. Aşağıdaki resimde gösterildiği gibi IIS'de gezinin:

      ![IIS uygulama yapılandırma penceresi](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic9.png)

      Kimliği nizi aldıktan sonra, bu hesabın söz konusu SPN ile yapılandırıldığından emin olun. `setspn –q http/spn.wacketywack.com` bunun bir örneğidir. Komut istemine aşağıdaki metni girin:

      ![SetSPN komut penceresini gösterir](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic10.png)

   - Tanımlanan SPN'yi portaldaki uygulama ayarlarıyla karşılaştırın. Hedef Azure AD hesabına göre yapılandırılan aynı SPN'nin uygulamanın uygulama havuzu tarafından kullanıldığından emin olun.

      ![Azure portalında SPN yapılandırması](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic11.png)

   - IIS'ye gidin ve uygulama için **Configuration Editor** seçeneğini seçin. **system.webServer/security/authentication/windowsAuthentication**adresine gidin. **UseAppPoolCredentials** değerinin **Doğru**olduğundan emin olun.

      ![IIS yapılandırma uygulaması kimlik bilgileri havuzu](./media/application-proxy-back-end-kerberos-constrained-delegation-how-to/graphic12.png)

      Bu değeri **True**olarak değiştirin. Önbelleğe alınmış tüm Kerberos biletlerini arka uç sunucusundan aşağıdaki komutu çalıştırarak kaldırın:

      ```powershell
      Get-WmiObject Win32_LogonSession | Where-Object {$_.AuthenticationPackage -ne 'NTLM'} | ForEach-Object {klist.exe purge -li ([Convert]::ToString($_.LogonId, 16))}
      ```

Daha fazla bilgi için, [tüm oturumlar için Kerberos istemci bilet önbelleğini temizleme'ye](https://gallery.technet.microsoft.com/scriptcenter/Purge-the-Kerberos-client-b56987bf)bakın.

Kernel modunu etkin bırakırsanız, Kerberos işlemlerinin performansını artırır. Ancak, istenen hizmetin biletin makine hesabı kullanılarak şifresinin çözülmesine de neden olur. Bu hesaba Yerel sistem de denir. Uygulama bir çiftlikte birden fazla sunucuda barındırıldığında KCD'yi kırmak için bu değeri **True** olarak ayarlayın.

- Ek bir denetim olarak, **Genişletilmiş** korumayı da devre dışı kaldırın. Bazı senaryolarda, **Genişletilmiş** koruma belirli yapılandırmalarda etkinleştirildiğinde KCD'yi kırdı. Bu gibi durumlarda, bir uygulama varsayılan web sitesinin bir alt klasörü olarak yayımlandı. Bu uygulama yalnızca anonim kimlik doğrulaması için yapılandırılır. Tüm iletişim kutuları gri renktedir ve bu da alt nesnelerin etkin ayarları devralmayacağını gösterir. Test etmenizi öneririz, ancak mümkünse **bu**değeri etkinleştirilenlere geri yüklemeyi unutmayın.

  Bu ek denetim, yayınlanmış uygulamanızı kullanmanızı doğru yola sokar. Temsilci olarak da yapılandırılan ek bağlayıcıları döndürebilirsiniz. Daha fazla bilgi için Azure AD Uygulama Proxy'si ile ilgili daha ayrıntılı teknik gezinme, [Sorun Giderme'yi](https://aka.ms/proxytshootpaper)okuyun.

Hala ilerleme kaydedemiyorsanız, Microsoft desteği size yardımcı olabilir. Doğrudan portal içinde bir destek bileti oluşturun. Bir mühendis sizinle irtibata geçecek.

## <a name="other-scenarios"></a>Diğer senaryolar

- Azure Application Proxy, isteğini bir uygulamaya göndermeden önce Bir Kerberos bileti ister. Bazı üçüncü taraf uygulamalar bu kimlik doğrulama yöntemini beğenmez. Bu uygulamalar daha geleneksel müzakerelerin gerçekleşmesini bekliyoruz. İlk istek anonimdir, bu da uygulamanın 401 üzerinden desteklediği kimlik doğrulama türleri ile yanıt vermesine olanak tanır.
- Multi-hop kimlik doğrulaması genellikle bir uygulama katmanlı senaryolarda kullanılır, arka uç ve ön uç, her ikisi de kimlik doğrulaması gerektiren (SQL Server Reporting Services gibi). Çoklu atlama senaryosunu yapılandırmak için, [Kerberos Kısıtlı Delegasyonu Çoklu Atlama Senaryolarında Protokol Geçişi Gerektirebilir](https://support.microsoft.com/help/2005838/kerberos-constrained-delegation-may-require-protocol-transition-in-mul)destek makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Yönetilen bir etki alanında KCD yapılandırma.](../../active-directory-domain-services/deploy-kcd.md)

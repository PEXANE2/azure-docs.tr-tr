---
title: Yapılandırma SSS'leri
description: Azure Uygulama Hizmeti için yapılandırma ve yönetim sorunları yla ilgili sık sorulan soruların yanıtlarını alın.
author: genlin
manager: dcscontentpm
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.topic: article
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 300294f37c809b01fe8fba7e627d6bc5bdc9903a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78942942"
---
# <a name="configuration-and-management-faqs-for-web-apps-in-azure"></a>Azure'da Web Apps için yapılandırma ve yönetim SSS'leri

Bu makalede, [Azure Uygulama Hizmeti'nin Web Apps özelliği](https://azure.microsoft.com/services/app-service/web/)için yapılandırma ve yönetim sorunları yla ilgili sık sorulan soruların (SSS' ler) yanıtları bulunmaktadır.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="are-there-limitations-i-should-be-aware-of-if-i-want-to-move-app-service-resources"></a>Uygulama Hizmeti kaynaklarını taşımak istersem dikkat etmem gereken sınırlamalar var mı?

Uygulama Hizmeti kaynaklarını yeni bir kaynak grubuna veya aboneye taşımayı planlıyorsanız, dikkat edilmesi gereken birkaç sınırlama vardır. Daha fazla bilgi için [Uygulama Hizmeti sınırlamaları'na](../azure-resource-manager/management/move-limitations/app-service-move-limitations.md)bakın.

## <a name="how-do-i-use-a-custom-domain-name-for-my-web-app"></a>Web uygulamam için özel bir etki alanı adını nasıl kullanırım?

Azure web uygulamanızla özel bir etki alanı adı kullanma yla ilgili sık sorulan soruların yanıtlarını almak için, yedi dakikalık [videomuza özel bir etki alanı adı ekle'ye](https://channel9.msdn.com/blogs/Azure-App-Service-Self-Help/Add-a-Custom-Domain-Name)bakın. Video, özel bir etki alanı adı ekleme nin bir gözden çıkışını sunar. Uygulama Hizmeti web uygulamanızla *.azurewebsites.net URL yerine kendi URL'nizi nasıl kullanacağınızı açıklar. Ayrıca, özel bir etki [alanı adının nasıl eşlenecek](app-service-web-tutorial-custom-domain.md)ile ilgili ayrıntılı bir gözden geçiriyi de görebilirsiniz.


## <a name="how-do-i-purchase-a-new-custom-domain-for-my-web-app"></a>Web uygulamam için yeni bir özel etki alanını nasıl satın alarım?

App Service web uygulamanız için nasıl satın alınıp özel bir etki alanı kurabilirsiniz öğrenmek için, [Uygulama Hizmeti'nde özel bir etki alanı adı satın alın ve yapılandırma](manage-custom-dns-buy-domain.md)kAdınızı görün.


## <a name="how-do-i-upload-and-configure-an-existing-ssl-certificate-for-my-web-app"></a>Web uygulamam için varolan bir SSL sertifikasını nasıl yükler ve yapılandırım?

Varolan özel bir SSL sertifikasını nasıl yükleyip kurup kurmayı öğrenmek [için](configure-ssl-certificate.md)bkz.


## <a name="how-do-i-purchase-and-configure-a-new-ssl-certificate-in-azure-for-my-web-app"></a>Web uygulamam için Azure'da yeni bir SSL sertifikasını nasıl satın alarım ve yapılandırmam?

App Service web uygulamanız için nasıl satın alınıp bir SSL sertifikası ayarlayınızı öğrenmek için uygulama [nız uygulamasına Bir SSL sertifikası ekle'ye](configure-ssl-certificate.md)bakın.


## <a name="how-do-i-move-application-insights-resources"></a>Application Insights kaynaklarını nasıl taşıyacağım?

Şu anda Azure Uygulama Öngörüleri taşıma işlemini desteklemez. Özgün kaynak grubunuz bir Application Insights kaynağı içeriyorsa, bu kaynağı taşıyamazsınız. Bir Uygulama Hizmeti uygulamasını taşımaya çalıştığınızda Application Insights kaynağını eklerseniz, tüm taşıma işlemi başarısız olur. Ancak, Uygulama Öngörüleri ve Uygulama Hizmeti planının uygulamanın doğru çalışması için uygulamayla aynı kaynak grubunda olması gerekmez.

Daha fazla bilgi için [Uygulama Hizmeti sınırlamaları'na](../azure-resource-manager/management/move-limitations/app-service-move-limitations.md)bakın.

## <a name="where-can-i-find-a-guidance-checklist-and-learn-more-about-resource-move-operations"></a>Bir kılavuz denetim listesini nerede bulabilir ve kaynak taşıma işlemleri hakkında daha fazla bilgi edinebilirim?

[Uygulama Hizmeti sınırlamaları,](../azure-resource-manager/management/move-limitations/app-service-move-limitations.md) kaynakları aynı abonelikteki yeni bir aboneye veya yeni bir kaynak grubuna nasıl taşıyabileceğinizi gösterir. Kaynak taşıma denetim listesi hakkında bilgi alabilir, hangi hizmetlerin taşıma işlemini desteklediğini öğrenebilir ve Uygulama Hizmeti sınırlamaları ve diğer konular hakkında daha fazla bilgi edinebilirsiniz.

## <a name="how-do-i-set-the-server-time-zone-for-my-web-app"></a>Web uygulamam Için sunucu saat dilimini nasıl ayarlarım?

Web uygulamanız için sunucu saat dilimini ayarlamak için:

1. Azure portalında, Uygulama Hizmeti aboneliğinizde **Uygulama ayarları** menüsüne gidin.
2. **Uygulama ayarları**altında, bu ayarı ekleyin:
    * Anahtar = WEBSITE_TIME_ZONE
    * Değer = *İstediğinzaman dilimi*
3. **Kaydet'i**seçin.

Windows'da çalışan Uygulama hizmetleri için, kabul edilen değerler için [Varsayılan Saat Dilimleri](https://docs.microsoft.com/windows-hardware/manufacture/desktop/default-time-zones) makalesindeki **Saat Dilimi** sütununa bakın. Linux üzerinde çalışan Uygulama hizmetleri için [TZ veritabanı adını](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) saat dilimi değeri olarak ayarlayın. Burada TZ veritabanı adı bir örnektir: Amerika / Adak.

## <a name="why-do-my-continuous-webjobs-sometimes-fail"></a>Sürekli Web İşlerim neden bazen başarısız oluyor?

Varsayılan olarak, belirli bir süre boşta kalırlarsa web uygulamaları boşaltılır. Bu, sistemin kaynakları korumasını sağlar. Temel ve Standart planlarda, web uygulamasını her zaman yüklü tutmak için **Her Zaman Ayarı'nı** açabilirsiniz. Web uygulamanız sürekli Web İşleri çalıştırDıysa, **Her Zaman Açık'ı**açmanız gerekir veya Webİşler güvenilir bir şekilde çalışmayabilir. Daha fazla bilgi için [bkz.](webjobs-create.md#CreateContinuous)

## <a name="how-do-i-get-the-outbound-ip-address-for-my-web-app"></a>Web uygulamamın giden IP adresini nasıl alabilirim?

Web uygulamanız için giden IP adreslerinin listesini almak için:

1. Azure portalında, web uygulama bıçaküzerinde **Özellikler** menüsüne gidin.
2. Giden **ip adreslerini**arayın.

Giden IP adreslerilistesi görüntülenir.

Web siteniz bir Uygulama Hizmet Ortamında barındırılansa giden IP adresini nasıl alacağınızı öğrenmek için [Giden ağ adreslerine](environment/app-service-app-service-environment-network-architecture-overview.md#outbound-network-addresses)bakın.

## <a name="how-do-i-get-a-reserved-or-dedicated-inbound-ip-address-for-my-web-app"></a>Web uygulamam için ayrılmış veya özel gelen IP adresini nasıl alabilirim?

Azure uygulama web sitenize yapılan gelen aramalar için özel veya ayrılmış bir IP adresi ayarlamak için IP tabanlı bir SSL sertifikası yükleyin ve yapılandırır.

Gelen aramalar için özel veya ayrılmış bir IP adresi kullanmak için Uygulama Hizmeti planınızın Temel veya daha yüksek bir hizmet planında olması gerektiğini unutmayın.

## <a name="can-i-export-my-app-service-certificate-to-use-outside-azure-such-as-for-a-website-hosted-elsewhere"></a>Uygulama Hizmeti sertifikamı Azure dışında kullanmak için dışa aktarabilir miyim? 

Evet, bunları Azure dışında kullanmak üzere dışa aktarabilirsiniz. Daha fazla bilgi [için, Uygulama Hizmeti sertifikaları ve özel etki alanları için SSS'lere](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview)bakın.

## <a name="can-i-export-my-app-service-certificate-to-use-with-other-azure-cloud-services"></a>Uygulama Hizmeti sertifikamı diğer Azure bulut hizmetlerinde kullanmak üzere dışa aktarabilir miyim?

Portal, Azure Key Vault üzerinden App Service uygulamalarına uygulama hizmeti sertifikası dağıtmak için birinci sınıf bir deneyim sağlar. Ancak, müşterilerden bu sertifikaları Uygulama Hizmeti platformu dışında (örneğin Azure Sanal Makineler) kullanmaları için istekler alıyoruz. Sertifikayı diğer Azure kaynaklarıyla kullanabilmek için Uygulama Hizmeti [sertifikanızın](https://blogs.msdn.microsoft.com/appserviceteam/2017/02/24/creating-a-local-pfx-copy-of-app-service-certificate/)yerel bir PFX kopyasını nasıl oluşturabileceğinizi öğrenmek için bkz.

Daha fazla bilgi [için, Uygulama Hizmeti sertifikaları ve özel etki alanları için SSS'lere](https://social.msdn.microsoft.com/Forums/azure/f3e6faeb-5ed4-435a-adaa-987d5db43b80/faq-on-app-service-certificates-and-custom-domains?forum=windowsazurewebsitespreview)bakın.


## <a name="why-do-i-see-the-message-partially-succeeded-when-i-try-to-back-up-my-web-app"></a>Web uygulamamı yedeklemeye çalıştığımda neden "Kısmen Başarılı Oldu" iletisini görüyorum?

Yedekleme hatasının yaygın bir nedeni, bazı dosyaların uygulama tarafından kullanılmasıdır. Siz yedeklemeyi gerçekleştirirken kullanılan dosyalar kilitlenir. Bu, bu dosyaların yedeklenmesini önler ve "Kısmen Başarılı" durumuna neden olabilir. Dosyaları yedekleme işleminden hariç tutarak bunun oluşmasını engelleyebilirsiniz. Yalnızca gerekenleri yedeklemeyi seçebilirsiniz. Daha fazla bilgi için Azure [web uygulamalarıyla sitenizin yalnızca önemli bölümlerini Yedekleme'ye](https://zainrizvi.io/blog/creating-partial-backups-of-your-site-with-azure-web-apps/)bakın.

## <a name="how-do-i-remove-a-header-from-the-http-response"></a>BIR üstbilgi http yanıtından nasıl kaldırılır?

Üstbilgileri HTTP yanıtından kaldırmak için sitenizin web.config dosyasını güncelleyin. Daha fazla bilgi için azure [web sitelerinizdeki standart sunucu üstbilgilerini kaldır'a](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/)bakın.

## <a name="is-app-service-compliant-with-pci-standard-30-and-31"></a>Uygulama Hizmeti PCI Standart 3.0 ve 3.1 ile uyumlu mu?

Şu anda Azure Uygulama Hizmeti'nin Web Uygulamaları özelliği PCI Veri Güvenliği Standardı (DSS) sürüm 3.0 Düzey 1 ile uyumludur. PCI DSS sürüm 3.1 yol haritamızda yer almaktadır. En son standardın benimsenmesinin nasıl ilerleyeceği konusunda planlama zaten devam ediyor.

PCI DSS sürüm 3.1 sertifikası, Taşıma Katmanı Güvenliğini (TLS) 1.0 devre dışı bırakmayı gerektirir. Şu anda, TLS 1.0'ı devre dışı bırakmak çoğu App Service planı için bir seçenek değildir. Ancak, Uygulama Hizmet Ortamı'nı kullanıyorsanız veya iş yükünüzü App Service Environment'a geçirmek istiyorsanız, ortamınız üzerinde daha fazla denetim edebilirsiniz. Bu, Azure Desteği'ne başvurarak TLS 1.0'ı devre dışı bırakmayı içerir. Yakın gelecekte, bu ayarları kullanıcılar için erişilebilir hale getirmeyi planlıyoruz.

Daha fazla bilgi için [PCI Standart 3.0 ve 3.1 ile Microsoft Azure Uygulama Hizmeti web uygulaması uyumluluğu](https://support.microsoft.com/help/3124528)'na bakın.

## <a name="how-do-i-use-the-staging-environment-and-deployment-slots"></a>Hazırlama ortamını ve dağıtım yuvalarını nasıl kullanırım?

Standart ve Premium Uygulama Hizmeti planlarında, web uygulamanızı App Service'e dağıttığınızda, varsayılan üretim yuvası yerine ayrı bir dağıtım yuvasına dağıtabilirsiniz. Dağıtım yuvaları, kendi ana bilgisayar adlarına sahip canlı web uygulamalarıdır. Web uygulaması içeriği ve yapılandırma öğeleri, üretim yuvası da dahil olmak üzere iki dağıtım yuvası arasında değiştirilebilir.

Dağıtım yuvalarını kullanma hakkında daha fazla bilgi için [bkz.](deploy-staging-slots.md)

## <a name="how-do-i-access-and-review-webjob-logs"></a>WebJob günlüklerine nasıl erişebilir ve bunları gözden geçirebilirim?

WebJob günlüklerini gözden geçirmek için:

1. [Kudu web sitenizde](https://*yourwebsitename*.scm.azurewebsites.net)oturum açın.
2. WebJob'u seçin.
3. **Çıkışını Toggle düğmesini** seçin.
4. Çıktı dosyasını indirmek için **İndir** bağlantısını seçin.
5. Tek tek çalıştırmalar için **Bireysel Çağrı'yı**seçin.
6. **Çıkışını Toggle düğmesini** seçin.
7. İndirme bağlantısını seçin.

## <a name="im-trying-to-use-hybrid-connections-with-sql-server-why-do-i-see-the-message-systemoverflowexception-arithmetic-operation-resulted-in-an-overflow"></a>SQL Server ile Hibrit Bağlantıları kullanmaya çalışıyorum. "System.OverflowException: Aritmetik işlem taşmaya neden oldu" iletisini neden görüyorum?

10 Mayıs 2016'daki Microsoft .NET güncelleştirmesi olan SQL Server'a erişmek için Karma Bağlantılar kullanıyorsanız, bağlantıların başarısız olması neden olabilir. Bu iletiyi görebilirsiniz:

```
Exception: System.Data.Entity.Core.EntityException: The underlying provider failed on Open. —> System.OverflowException: Arithmetic operation resulted in an overflow. or (64 bit Web app) System.OverflowException: Array dimensions exceeded supported range, at System.Data.SqlClient.TdsParser.ConsumePreLoginHandshake
```

### <a name="resolution"></a>Çözüm

Özel durum, Karma Bağlantı Yöneticisi ile ilgili o zamandan beri düzeltilen bir sorundan kaynaklanır. Bu sorunu gidermek için [Karma Bağlantı Yöneticinizi güncelleştirdiğinden](https://go.microsoft.com/fwlink/?LinkID=841308) emin olun.

## <a name="how-do-i-add-a-url-rewrite-rule"></a>URL yeniden yazma kuralını nasıl eklerim?

URL yeniden yazma kuralı eklemek için, **wwwroot** klasöründe ilgili config girişleri içeren bir web.config dosyası oluşturun. Daha fazla bilgi için Azure [Uygulama Hizmetleri: URL'yi yeniden yazma yı anlama](https://blogs.msdn.microsoft.com/madhurabharadwaj/2018/06/01/azure-app-services-understanding-url-re-write/).

## <a name="how-do-i-control-inbound-traffic-to-app-service"></a>App Service'e gelen trafiği nasıl kontrol edebilirim?

Site düzeyinde, Uygulama Hizmeti'ne gelen trafiği denetlemek için iki seçeneğiniz var:

* Dinamik IP kısıtlamalarını açın. Dinamik IP kısıtlamalarını nasıl açabilirsiniz öğrenmek [için Azure web siteleri için IP ve etki alanı kısıtlamalarına](https://azure.microsoft.com/blog/ip-and-domain-restrictions-for-windows-azure-web-sites/)bakın.
* Modül Güvenliği'ni açın. Modül Güvenliği'ni nasıl açabilirsiniz öğrenmek için [Azure web sitelerindeki ModSecurity web uygulaması güvenlik duvarı'na](https://azure.microsoft.com/blog/modsecurity-for-azure-websites/)bakın.

App Service Environment kullanıyorsanız, [Barracuda güvenlik duvarLarını](https://azure.microsoft.com/blog/configuring-barracuda-web-application-firewall-for-azure-app-service-environment/)kullanabilirsiniz.

## <a name="how-do-i-block-ports-in-an-app-service-web-app"></a>App Service web uygulamasında bağlantı noktalarını nasıl engellerim?

Uygulama Hizmeti paylaşılan kiracı ortamında, altyapının yapısı nedeniyle belirli bağlantı noktalarını engellemek mümkün değildir. TCP portları 4020, 4022 ve 4024 de Visual Studio uzaktan hata ayıklama için açık olabilir.

Uygulama Servis Ortamında, gelen ve giden trafik üzerinde tam kontrole sahipsiniz. Belirli bağlantı noktalarını kısıtlamak veya engellemek için Ağ Güvenlik Grupları'nı kullanabilirsiniz. Uygulama Hizmet Ortamı hakkında daha fazla bilgi için [bkz.](https://azure.microsoft.com/blog/introducing-app-service-environment/)

## <a name="how-do-i-capture-an-f12-trace"></a>F12 izini nasıl yakalarım?

Bir F12 izini yakalamak için iki seçeneğiniz var:

* F12 HTTP izleme
* F12 konsol çıkışı

### <a name="f12-http-trace"></a>F12 HTTP izleme

1. Internet Explorer'da web sitenize gidin. Sonraki adımları yapmadan önce oturum açmanız önemlidir. Aksi takdirde, F12 izi hassas oturum açma verilerini yakalar.
2. F12 tuşuna basın.
3. **Ağ** sekmesinin seçili olduğunu doğrulayın ve ardından yeşil **Oynat** düğmesini seçin.
4. Sorunu yeniden oluşturan adımları yapın.
5. Kırmızı **Durdur** düğmesini seçin.
6. **Kaydet** düğmesini (disk simgesi) seçin ve HAR dosyasını kaydedin (Internet Explorer ve Microsoft Edge'de) *veya* HAR dosyasını sağ tıklatın ve ardından içerikle (Chrome'da) **HAR olarak kaydet'i** seçin.

### <a name="f12-console-output"></a>F12 konsol çıkışı

1. **Konsol** sekmesini seçin.
2. Sıfırdan fazla öğe içeren her sekme için sekmeyi **(Hata**, **Uyarı**veya **Bilgi)** seçeneğini belirleyin. Sekme seçili değilse, imleci ondan uzaklaştırdığınızda sekme simgesi gri veya siyahtır.
3. Bölmenin ileti alanına sağ tıklayın ve ardından **Tümünü Kopyala'yı**seçin.
4. Kopyalanan metni bir dosyaya yapıştırın ve ardından dosyayı kaydedin.

Bir HAR dosyasını görüntülemek [için, HAR görüntüleyicik](https://www.softwareishard.com/har/viewer/)kullanabilirsiniz.

## <a name="why-do-i-get-an-error-when-i-try-to-connect-an-app-service-web-app-to-a-virtual-network-that-is-connected-to-expressroute"></a>Bir App Service web uygulamasını ExpressRoute'a bağlı sanal bir ağa bağlamaya çalıştığımda neden hata alıyorum?

Bir Azure web uygulamasını Azure ExpressRoute'a bağlı sanal bir ağa bağlamaya çalışırsanız, başarısız olur. Aşağıdaki ileti görüntülenir: "Ağ geçidi vpn ağ geçidi değildir."

Şu anda, ExpressRoute'a bağlı sanal bir ağa noktadan siteye VPN bağlantısı na sahip olamazsınız. Bir noktadan siteye VPN ve ExpressRoute aynı sanal ağ için bir arada bulunamaz. Daha fazla bilgi için [ExpressRoute ve siteden siteye VPN bağlantılarının sınırları ve sınırlamaları](../expressroute/expressroute-howto-coexist-classic.md#limits-and-limitations)hakkında bilgi alabiliyorum.

## <a name="how-do-i-connect-an-app-service-web-app-to-a-virtual-network-that-has-a-static-routing-policy-based-gateway"></a>Bir App Service web uygulamasını statik yönlendirme (ilke tabanlı) ağ geçidi olan sanal ağa nasıl bağlarım?

Şu anda, bir App Service web uygulamasını statik yönlendirme (ilke tabanlı) ağ geçidi olan sanal ağa bağlamak desteklenmez. Hedef sanal ağınız zaten varsa, bir uygulamaya bağlanabilmesi için önce dinamik bir yönlendirme ağ geçidiyle siteye nokta VPN'i etkinleştirilmiş olmalıdır. Ağ geçidiniz statik yönlendirmeye ayarlanmışsa, noktadan siteye VPN'i etkinleştiremezsiniz. 

Daha fazla bilgi için [bkz.](web-sites-integrate-with-vnet.md)

## <a name="in-my-app-service-environment-why-can-i-create-only-one-app-service-plan-even-though-i-have-two-workers-available"></a>Uygulama Hizmet Ortamımda, iki çalışanım olmasına rağmen neden yalnızca bir Uygulama Hizmeti planı oluşturabilirim?

Hata toleransı sağlamak için, App Service Environment her alt havuz için en az bir ek bilgi işlem kaynağı na ihtiyaç duyar. Ek bilgi işlem kaynağına iş yükü atanamıyor.

Daha fazla bilgi için Uygulama [Hizmeti Ortamının Nasıl Oluşturulabildiğini](environment/app-service-web-how-to-create-an-app-service-environment.md)öğrenin.

## <a name="why-do-i-see-timeouts-when-i-try-to-create-an-app-service-environment"></a>Bir Uygulama Hizmeti Ortamı oluşturmaya çalıştığımda neden zaman ekmeleri görüyorum?

Bazen, Bir Uygulama Hizmet Ortamı oluşturmak başarısız olur. Bu durumda, Etkinlik günlüklerinde aşağıdaki hatayı görürsünüz:
```
ResourceID: /subscriptions/{SubscriptionID}/resourceGroups/Default-Networking/providers/Microsoft.Web/hostingEnvironments/{ASEname}
Error:{"error":{"code":"ResourceDeploymentFailure","message":"The resource provision operation did not complete within the allowed timeout period."}}
```

Bunu çözmek için, aşağıdaki koşullardan hiçbirinin doğru olmadığından emin olun:
* Alt ağ çok küçük.
* Alt ağ boş değil.
* ExpressRoute, Bir Uygulama Hizmeti Ortamının ağ bağlantısı gereksinimlerini engeller.
* Kötü bir Ağ Güvenlik Grubu, Bir Uygulama Hizmeti Ortamının ağ bağlantısı gereksinimlerini engeller.
* Zorunlu tünel ler açık.

Daha fazla bilgi için, [yeni bir Azure Uygulama Hizmet Ortamı nı dağıtırken (oluştururken) Sık Karşılaşılan sorunlara](https://blogs.msdn.microsoft.com/waws/2016/05/13/most-frequent-issues-when-deploying-creating-a-new-azure-app-service-environment-ase/)bakın.

## <a name="why-cant-i-delete-my-app-service-plan"></a>Uygulama Hizmeti planımı neden semiyorum?

Herhangi bir Uygulama Hizmeti uygulaması Uygulama Hizmeti planıyla ilişkiliyse, bir Uygulama Hizmeti planını silemezsiniz. Bir Uygulama Hizmeti planını silmeden önce, ilişkili tüm Uygulama Hizmeti uygulamalarını Uygulama Hizmeti planından kaldırın.

## <a name="how-do-i-schedule-a-webjob"></a>WebJob'u nasıl zamanlayabilirim?

Cron ifadelerini kullanarak zamanlanmış bir WebJob oluşturabilirsiniz:

1. Ayarlar.iş dosyası oluşturun.
2. Bu JSON dosyasında, Cron ifadesini kullanarak bir zamanlama özelliği ekleyin: 
    ```json
    { "schedule": "{second}
    {minute} {hour} {day}
    {month} {day of the week}" }
    ```

Zamanlanan Web İşleri hakkında daha fazla bilgi için [bkz.](webjobs-create.md#CreateScheduledCRON)

## <a name="how-do-i-perform-penetration-testing-for-my-app-service-app"></a>App Service uygulamam için penetrasyon testini nasıl yaparım?

Penetrasyon testi gerçekleştirmek için [bir istek gönderin.](https://portal.msrc.microsoft.com/engage/pentest)

## <a name="how-do-i-configure-a-custom-domain-name-for-an-app-service-web-app-that-uses-traffic-manager"></a>Trafik Yöneticisi kullanan bir Uygulama Hizmeti web uygulaması için özel bir etki alanı adını nasıl yapılandırıyorum?

Yük dengeleme için Azure Trafik Yöneticisi'ni kullanan bir Uygulama Hizmeti uygulamasıyla özel bir etki alanı adını nasıl kullanacağınızı öğrenmek [için, Trafik Yöneticisi ile bir Azure web uygulaması için özel bir etki alanı adı yapılandırma'ya](configure-domain-traffic-manager.md)bakın.

## <a name="my-app-service-certificate-is-flagged-for-fraud-how-do-i-resolve-this"></a>App Service sertifikam sahtekarlık nedeniyle incelemeye alındı. Bu sorunu nasıl çözebilirim?

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir Uygulama Hizmeti sertifikası satın alma alanı nın etki alanı doğrulanması sırasında aşağıdaki iletiyi görebilirsiniz:

"Sertifikanız olası bir sahtekarlık için işaretlendi. İstek şu anda inceleme altında. Sertifika 24 saat içinde kullanılabilir hale gelmezse, lütfen Azure Desteği'ne başvurun."

İletinin de gösterdiği gibi, bu sahtekarlık doğrulama işleminin tamamlanması 24 saat kadar sürebilir. Bu süre zarfında, iletiyi görmeye devam eceksiniz.

Uygulama Hizmeti sertifikanız bu iletiyi 24 saat sonra göstermeye devam ederse, lütfen aşağıdaki PowerShell komut dosyasını çalıştırın. Komut dosyası, sorunu gidermek için [sertifika sağlayıcısıyla](https://www.godaddy.com/) doğrudan bağlantı kurur.

```powershell
Connect-AzAccount
Set-AzContext -SubscriptionId <subId>
$actionProperties = @{
    "Name"= "<Customer Email Address>"
    };
Invoke-AzResourceAction -ResourceGroupName "<App Service Certificate Resource Group Name>" -ResourceType Microsoft.CertificateRegistration/certificateOrders -ResourceName "<App Service Certificate Resource Name>" -Action resendRequestEmails -Parameters $actionProperties -ApiVersion 2015-08-01 -Force   
```

## <a name="how-do-authentication-and-authorization-work-in-app-service"></a>App Service'de kimlik doğrulama ve yetkilendirme nasıl çalışır?

Uygulama Hizmeti'nde kimlik doğrulama ve yetkilendirme için ayrıntılı belgeler için, çeşitli sağlayıcı oturum açma belgeleri için dokümanlara bakın:
* [Azure Active Directory](configure-authentication-provider-aad.md)
* [Facebook](configure-authentication-provider-facebook.md)
* [Google](configure-authentication-provider-google.md)
* [Microsoft Hesabı](configure-authentication-provider-microsoft.md)
* [Twitter](configure-authentication-provider-twitter.md)

## <a name="how-do-i-redirect-the-default-azurewebsitesnet-domain-to-my-azure-web-apps-custom-domain"></a>Varsayılan *.azurewebsites.net etki alanını Azure web uygulamamın özel etki alanına nasıl yönlendirebilirim?

Azure'da Web Apps'ı kullanarak yeni bir web sitesi oluşturduğunuzda, sitenize varsayılan bir *site adı*.azurewebsites.net etki alanı atanır. Sitenize özel bir ana bilgisayar adı eklerseniz ve kullanıcıların varsayılan *.azurewebsites.net etki alanınıza erişebilmesini istemiyorsanız, varsayılan URL'yi yeniden yönlendirebilirsiniz. Web sitenizin varsayılan etki alanından özel etki alanınıza tüm trafiği nasıl yönlendireceklerini öğrenmek için, [varsayılan etki alanını Azure web uygulamalarında özel etki alanınıza yönlendirme'ye](https://zainrizvi.io/blog/block-default-azure-websites-domain/)bakın.

## <a name="how-do-i-determine-which-version-of-net-version-is-installed-in-app-service"></a>.NET sürümünün Hangi sürümünün App Service'e yüklenmiş olduğunu nasıl belirleyebilirim?

Microsoft .NET'in App Service'de yüklü sürümünü bulmanın en hızlı yolu Kudu konsolunu kullanmaktır. Kudu konsoluna portaldan veya Uygulama Hizmeti uygulamanızın URL'sini kullanarak erişebilirsiniz. Ayrıntılı talimatlar için [bkz.](https://blogs.msdn.microsoft.com/waws/2016/11/02/how-to-determine-the-installed-net-version-in-azure-app-services/)

## <a name="why-isnt-autoscale-working-as-expected"></a>Otomatik ölçeklendirme neden beklendiği gibi çalışmıyor?

Azure Otomatik Ölçeklendirmesi web uygulaması örneğini beklediğiniz gibi ölçeklendirmediyse veya ölçeklendirmediyse, "kanat çırpma" nedeniyle sonsuz bir döngüyü önlemek için kasıtlı olarak ölçeklendirmemeyi seçtiğimiz bir senaryoyla karşınıza çıkıyor olabilirsiniz. Bu genellikle ölçeklendirme ve ölçek-in eşikleri arasında yeterli bir kenar boşluğu olmadığında gerçekleşir. "Çırpmaktan" nasıl kaçınılamayı öğrenmek ve diğer Otomatik Ölçeklendirme en iyi uygulamaları hakkında bilgi edinmek için [Otomatik Ölçeklendirme en iyi uygulamaları](../azure-monitor/platform/autoscale-best-practices.md#autoscale-best-practices)görün.

## <a name="why-does-autoscale-sometimes-scale-only-partially"></a>Otomatik ölçeklendirme bazen neden yalnızca kısmen ölçeklendirin?

Ölçümler önceden yapılandırılmış sınırları aştığında otomatik ölçeklendirme tetiklenir. Bazen, kapasitenin beklediğiniz ile karşılaştırıldığında yalnızca kısmen dolu olduğunu fark edebilirsiniz. Bu, istediğiniz örnek sayısı kullanılamıyorsa oluşabilir. Bu senaryoda, Otomatik Ölçek kullanılabilir örnek sayısıyla kısmen doldurur. Daha fazla kapasite elde etmek için otomatik ölçeklendirme yeniden dengeleme mantığını çalıştırır. Geri kalan örnekleri ayırır. Bunun birkaç dakika sürebileceğini unutmayın.

Birkaç dakika sonra beklenen örnek sayısını göremiyorsanız, bunun nedeni kısmi yeniden doldurmanın ölçümleri sınırlar içinde getirmek için yeterli olması olabilir. Veya, Otomatik Ölçek, daha düşük ölçüm sınırına ulaştığından küçültülmüş olabilir.

Bu koşullardan hiçbiri geçerli değilse ve sorun devam ederse, bir destek isteği gönderin.

## <a name="how-do-i-turn-on-http-compression-for-my-content"></a>İçeriğim için HTTP sıkıştırmayı nasıl açarım?

Hem statik hem de dinamik içerik türleri için sıkıştırmayı açmak için uygulama düzeyindeki web.config dosyasına aşağıdaki kodu ekleyin:

```xml
<system.webServer>
    <urlCompression doStaticCompression="true" doDynamicCompression="true" />
</system.webServer>
```

Sıkıştırmak istediğiniz belirli dinamik ve statik MIME türlerini de belirtebilirsiniz. Daha fazla bilgi için, basit bir [Azure web sitesinde httpSıkıştırma ayarlarında](https://social.msdn.microsoft.com/Forums/azure/890b6d25-f7dd-4272-8970-da7798bcf25d/httpcompression-settings-on-a-simple-azure-website?forum=windowsazurewebsitespreview)bir forum sorusuna yanıtımızı görün.

## <a name="how-do-i-migrate-from-an-on-premises-environment-to-app-service"></a>Şirket içi ortamdan Uygulama Hizmeti'ne nasıl geçiş yapabilirim?

Siteleri Windows ve Linux web sunucularından Uygulama Hizmeti'ne geçirmek için Azure Uygulama Hizmeti Geçiş Yardımcısı'nı kullanabilirsiniz. Geçiş aracı, gerektiğinde Azure'da web uygulamaları ve veritabanları oluşturur ve içeriği yayımlar. Daha fazla bilgi için Azure [Uygulama Hizmeti Geçiş Yardımcısı'na](https://appmigration.microsoft.com/)bakın.

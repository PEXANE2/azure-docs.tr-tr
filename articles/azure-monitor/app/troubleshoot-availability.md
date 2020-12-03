---
title: Azure Application Insights kullanılabilirlik testleriniz sorunlarını giderme
description: Azure Application Insights 'de Web testlerinde sorun giderme. Web sitesi kullanılamaz duruma gelirse veya yavaş yanıt verirse uyarı alın.
ms.topic: conceptual
author: lgayhardt
ms.author: lagayhar
ms.date: 11/19/2020
ms.reviewer: sdash
ms.openlocfilehash: 368c45433247c441631bdf79bfc9caa28a41f1b4
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96546765"
---
# <a name="troubleshooting"></a>Sorun giderme

Bu makale, kullanılabilirlik izlemeyi kullanırken oluşabilecek yaygın sorunları gidermenize yardımcı olur.

## <a name="troubleshooting-report-steps-for-ping-tests"></a>Ping testleri için sorun giderme raporu adımları

Sorun giderme raporu, **ping testlerinizin** başarısız olmasına neden olan yaygın sorunları kolayca tanılamanıza olanak sağlar.

![Sorun giderme raporunu görüntülemek için uçtan uca işlem ayrıntılarına bir hata seçerek kullanılabilirlik sekmesinden gezinme animasyonu](./media/troubleshoot-availability/availability-to-troubleshooter.gif)

1. Application Insights kaynağınızın kullanılabilirlik sekmesinde, kullanılabilirlik testlerinin genel veya bir kısmını seçin.
2. , Sol taraftaki "detaya git" bölümündeki bir test **başarısız** ' ı seçin veya dağılım çiziminde noktalarından birini seçin.
3. Uçtan uca işlem ayrıntısı sayfasında, bir olay seçin ve ardından "Rapor Özeti sorunlarını giderme" seçeneğinin altındaki sorun giderme raporunu görmek için **[adıma git]** seçeneğini belirleyin.

> [!NOTE]
>  Bağlantı yeniden kullanım adımı varsa DNS çözümlemesi, bağlantı kurulumu ve TLS taşıma adımları mevcut olmayacaktır.

|Adım | Hata iletisi | Olası nedeni |
|-----|---------------|----------------|
| Bağlantı yeniden kullanımı | yok | Genellikle önceden oluşturulmuş bir bağlantıya bağımlıdır Web test adımının bağımlıdır. Bu nedenle, DNS, bağlantı veya SSL adımı gerekmez. |
| DNS çözümleme | Uzak ad çözümlenemedi: "URL 'niz" | Büyük olasılıkla yanlış yapılandırılmış DNS kayıtları veya geçici DNS sunucusu hatalarından dolayı DNS çözümleme işlemi başarısız oldu. |
| Bağlantı kurma | Bağlı olan taraf bir süre sonra düzgün bir şekilde yanıt vermediği için bağlantı girişimi başarısız oldu. | Genel olarak, sunucunuz HTTP isteğine yanıt vermediği anlamına gelir. Yaygın bir nedeni, test aracılarımızın sunucunuzdaki bir güvenlik duvarı tarafından engellenmesidir. Bir Azure sanal ağı dahilinde test etmek isterseniz, kullanılabilirlik hizmeti etiketini ortamınıza eklemeniz gerekir.|
| TLS taşıma  | İstemci ve sunucu, ortak bir algoritmaya sahip olmadıkları için iletişim kuramıyor.| Yalnızca TLS 1,0, 1,1 ve 1,2 desteklenir. SSL desteklenmez. Bu adım SSL sertifikalarını doğrulamaz ve yalnızca güvenli bir bağlantı kurar. Bu adım, yalnızca bir hata oluştuğunda görüntülenir. |
| Yanıt üst bilgisi alınıyor | Aktarım bağlantısından veriler okunamıyor. Bağlantı kapatıldı. | Sunucunuz yanıt üstbilgisinde bir protokol hatası taahhüt ediyor. Örneğin, yanıt tam olarak olmadığında bağlantınız sunucu tarafından kapatıldı. |
| Yanıt gövdesi alma | Aktarım bağlantısından veri okunamıyor: bağlantı kapatıldı. | Sunucunuz yanıt gövdesinde bir protokol hatası taahhüt ediyor. Örneğin, yanıt tam olarak okunmadığı veya öbekli yanıt gövdesinde öbek boyutu yanlış olduğunda, bağlantınız sunucu tarafından kapatıldı. |
| Yeniden yönlendirme sınırı doğrulaması | Bu Web sayfasının çok fazla yeniden yönlendirmeleri vardır. Bu istek otomatik yeniden yönlendirmeler için sınırı aştığından, bu döngü burada sonlandırılacak. | Test başına 10 yeniden yönlendirme sınırı vardır. |
| Durum kodu doğrulaması | `200 - OK` beklenen durumla eşleşmiyor `400 - BadRequest` . | Başarılı olarak sayılan döndürülen durum kodu. 200, normal web sayfası döndürüldüğünü belirten koddur. |
| İçerik doğrulama | Gerekli ' Merhaba ' metni yanıtta görünmüyor. | Dize, yanıtta, "Welcome!" dizesi gibi tam bir büyük küçük harfe duyarlı eşleşme değildir. Joker karakter içermeyen (örneğin bir yıldız işareti) düz bir dize olmalıdır. Sayfa içeriğiniz değişirse dizeyi güncelleştirmeniz gerekebilir. İçerik eşleşmesi ile yalnızca Ingilizce karakterler desteklenir. |
  
## <a name="common-troubleshooting-questions"></a>Ortak sorun giderme soruları

### <a name="site-looks-okay-but-i-see-test-failures-why-is-application-insights-alerting-me"></a>Site sorunsuz görünüyor, ancak test hataları görüyorum? Neden Application Insights uyarı veriyor?

   * Testiniz **bağımlı Istekleri Ayrıştır** mi? Bu, komut dosyaları, görüntüler vb. kaynaklar üzerinde katı bir denetim elde ediyor. Bu tür hataların bir tarayıcıda dikkat çekici olmaması olabilir. Tüm görüntüleri, betikleri, stil sayfalarını ve sayfa tarafından yüklenen diğer dosyaları denetleyin. Bunlardan herhangi biri başarısız olursa, ana HTML sayfası sorun olmadan yüklense bile test başarısız olarak bildirilir. Testi bu tür kaynak hatalarıyla desensitize için, test yapılandırmasından gelen bağımlı Istekleri Ayrışmanızın işaretini kaldırmanız yeterlidir.

   * Geçici ağ hüklarından gürültü düzeyini azaltmak için, test arızaları yapılandırması için yeniden denemeleri etkinleştirin ' in işaretli olduğundan emin olun. Ayrıca, daha fazla konumdan test edebilir ve uyarı kuralı eşiğini, hataya özel sorunları engellemek için uygun şekilde yönetebilirsiniz.

   * Neden başarısız olduğunu bildirdiğimiz ayrıntılarını görmek için, kullanılabilirlik dağılımı çizim deneyimi deneyiminden kırmızı noktalardan herhangi birine veya arama Gezgini 'nden herhangi bir kullanılabilirlik hatasına tıklayın. Test sonucu, ilişkili sunucu tarafı telemetri (etkinse) ile birlikte testin neden başarısız olduğunu anlamanıza yardımcı olmalıdır. Geçici sorunların yaygın nedenleri ağ veya bağlantı sorunlarıdır.

   * Test zaman aşımına uğrar mı? 2 dakika sonra testleri iptal ediyoruz. Ping veya çok adımlı testiniz 2 dakikadan uzun sürerse, bunu hata olarak raporlarız. Testi daha kısa süreler içinde tamamlayabileceğiniz birden çok tabloya bölmek göz önünde bulundurun.

   * Tüm konumlar hata verdi mi, yoksa yalnızca bir kısmı mı? Yalnızca bazı sorunlar bildirilmezse, bu durum ağ/CDN sorunlarından kaynaklanabilir. Daha sonra, kırmızı noktalara tıklanması, konumun neden hatalara neden olduğunu anlamanıza yardımcı olmalıdır.

### <a name="i-did-not-get-an-email-when-the-alert-triggered-or-resolved-or-both"></a>Uyarı tetiklendiğinde veya her ikisini de bir e-posta almadı mi?

E-postanızı doğrudan listelendiğini doğrulamak için klasik uyarılar yapılandırmasını denetleyin veya açık olduğunuz bir dağıtım listesi bildirimleri alacak şekilde yapılandırılır. Bu durumda, dağıtım listesi yapılandırmasını denetleyerek dış e-posta alabileceğinden emin olun. Ayrıca, e-posta yöneticinizin bu soruna neden olabilecek bir ilke yapılandırılıp yapılandırılmadığını denetleyin.

### <a name="i-did-not-receive-the-webhook-notification"></a>Web kancası bildirimi aldım mı?

Web kancası bildirimini alan uygulamanın kullanılabilir olduğundan ve Web kancası isteklerini başarıyla işlediğinden emin olun. Daha fazla [bilgi için bkz](../platform/alerts-log-webhook.md) ..

### <a name="i-am-getting--403-forbidden-errors-what-does-this-mean"></a>403 yasaklanmış hata alıyorum, bu ne anlama geliyor?

Bu hata, kullanılabilirlik aracılarının hedef URL 'nizin test yapmasına izin vermek için güvenlik duvarı özel durumları eklemeniz gerektiğini gösterir. İzin verilecek aracı IP adreslerinin tam listesi için, [IP özel durum makalesine](./ip-addresses.md#availability-tests)başvurun.

### <a name="intermittent-test-failure-with-a-protocol-violation-error"></a>Protokol ihlali hatası ile aralıklı test hatası?

Hata ("protokol ihlali..CR’den sonra LF gelmelidir") sunucu (veya bağımlılıklar) ile ilgili bir sorun olduğunu gösterir. Bu durum, yanıtta hatalı biçimlendirilmiş üst bilgiler ayarlandığında meydana gelir. Yük dengeleyiciler veya CDN'lerden kaynaklanabilir. Özellikle, bazı üst bilgiler satır sonunu belirtmek için CRLF kullanmıyor olabilir, bu da HTTP belirtimini ihlal eder ve bu nedenle .NET WebRequest düzeyinde doğrulama başarısız olur. İhlal halinde olabilecek nokta üst bilgilerine olan yanıtı inceleyin.

> [!NOTE]
> URL, HTTP üstbilgileri gevşek olarak doğrulanmış tarayıcılarda başarısız olmayabilir. Bu sorunun ayrıntılı bir açıklaması için bu blog gönderisine bakın: http://mehdi.me/a-tale-of-debugging-the-linkedin-api-net-and-http-protocol-violations/  

### <a name="i-dont-see-any-related-server-side-telemetry-to-diagnose-test-failures"></a>Test başarısızlıklarını tanılamak için ilgili sunucu tarafı telemetri görmüyorum? *

Sunucu tarafı uygulamanız için Application Insights ayarlanmışsa, bunun nedeni [örnekleme](./sampling.md) işleminin devam ediyor olması olabilir. Farklı bir kullanılabilirlik sonucu seçin.

### <a name="can-i-call-code-from-my-web-test"></a>Web testimden kod çağırabilir miyim?

Hayır. Test adımları .webtest dosyasında olmalıdır. Bu nedenle, başka web testlerini çağıramaz, döngüleri kullanamazsınız. Ancak yararlı bulabileceğiniz bir dizi eklenti vardır.


### <a name="is-there-a-difference-between-web-tests-and-availability-tests"></a>"Web testleri" ve "kullanılabilirlik testleri" arasında bir fark var mı?

Bu iki terim birbirlerinin yerine kullanılabilir. Kullanılabilirlik testleri, çok adımlı web testlerine ek olarak tek URL ping testlerini de içeren daha genel bir terimdir.

### <a name="id-like-to-use-availability-tests-on-our-internal-server-that-runs-behind-a-firewall"></a>Kullanılabilirlik testlerini, güvenlik duvarının arkasında çalışan kendi dahili sunucumuzda kullanmayı tercih ediyorum.

   İki olası çözümü vardır:

   * Güvenlik duvarınızı, [Web testi aracılarımızın IP adreslerinden](./ip-addresses.md) gelen isteklere izin verecek şekilde yapılandırın.
   * İç sunucunuzu düzenli olarak test etmek için kendi kodunuzu yazın. Kodu, güvenlik duvarınızın arkasındaki bir test sunucusunda arka plan işlemi olarak çalıştırın. Test işleminiz, temel SDK paketindeki [TrackAvailability()](/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) API’sini kullanarak sonuçları Application Insights’a gönderebilir. Bunun için test sunucunuzun Application Insights alım uç noktası ile giden bağlantısının olması gerekir, ancak bu, gelen isteklere izin vermeye göre çok daha küçük bir güvenlik riski oluşturur. Sonuçlar kullanılabilirlik Web testleri dikey penceresinde görünür ancak deneyim Portal aracılığıyla oluşturulan testler için kullanılabilir olan yeniliklerden biraz basitleşmiş olur. Özel kullanılabilirlik testleri analiz, arama ve ölçümler ' de kullanılabilirlik sonuçları olarak da görünür.

### <a name="uploading-a-multi-step-web-test-fails"></a>Çok adımlı web testi yüklenemiyor

Bunun gerçekleşebileceği bazı nedenler:
   * 300 K boyut sınırı vardır.
   * Döngüler desteklenmez.
   * Başka web testlerine başvurular desteklenmez.
   * Veri kaynakları desteklenmez.

### <a name="my-multi-step-test-doesnt-complete"></a>Çok adımlı testim tamamlanmadı

Test başına 100 istek sınırı var. Ayrıca, iki dakikadan uzun süre çalışırsa test durdurulur.

### <a name="how-can-i-run-a-test-with-client-certificates"></a>İstemci sertifikasıyla testi nasıl çalıştırırım?

Bu şu anda desteklenmiyor.

## <a name="who-receives-the-classic-alert-notifications"></a>(Klasik) uyarı bildirimlerini kim alıyor?

Bu bölüm yalnızca, klasik uyarılar için geçerlidir ve yalnızca istediğiniz alıcıların bildirimleri almasını sağlamak için uyarı bildirimlerinizi iyileştirmenize yardımcı olur. [Klasik uyarılar](../platform/alerts-classic.overview.md)arasındaki fark hakkında daha fazla bilgi edinmek ve yeni uyarılar deneyimi hakkında daha fazla bilgi edinmek için [uyarılara genel bakış makalesine](../platform/alerts-overview.md)bakın. Yeni uyarılar deneyiminde uyarı bildirimini denetlemek için [eylem gruplarını](../platform/action-groups.md)kullanın.

* Klasik uyarı bildirimleri için belirli alıcıların kullanılmasını öneririz.

* X-Y konumlarından oluşan hatalarda oluşan uyarılarda, **toplu/grup** onay kutusu seçeneği etkinleştirildiyse, yönetici/ortak yönetici rollerine sahip kullanıcılara gönderir.  Esas olarak, _aboneliğin_ _Tüm_ yöneticileri bildirim alır.

* Kullanılabilirlik ölçümlerinde uyarılar için **toplu/grup** onay kutusu seçeneği etkinleştirilirse, abonelikte sahip, katkıda bulunan veya okuyucu rollerine sahip kullanıcılara gönderir. Aslında, aboneliğe erişimi olan _Tüm_ kullanıcılar Application Insights kaynak kapsamdadır ve bildirimler alacaktır. 

> [!NOTE]
> Şu anda **toplu/grup** onay kutusu seçeneğini kullanırsanız ve devre dışı bıraktığınızda, değişikliği döndüremezsiniz.

Kullanıcılara rollerine göre bildirimde bulunan yeni uyarı deneyimini veya neredeyse gerçek zamanlı uyarıları kullanın. [Eylem gruplarıyla](../platform/action-groups.md), katkıda bulunan/sahip/okuyucu rollerinin herhangi birine sahip kullanıcılar için e-posta bildirimleri yapılandırabilirsiniz (tek bir seçenek olarak birlikte birleştirilemez).

## <a name="next-steps"></a>Sonraki adımlar

* [Çok adımlı Web testi](availability-multistep.md)
* [URL ping testleri](monitor-web-app-availability.md)
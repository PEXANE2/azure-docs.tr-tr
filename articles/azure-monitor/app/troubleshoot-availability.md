---
title: Azure Uygulama Öngörüleri kullanılabilirlik testlerinizi giderin | Microsoft Dokümanlar
description: Azure Application Insights'ta web testlerini sorun giderin. Web sitesi kullanılamaz duruma gelirse veya yavaş yanıt verirse uyarı alın.
ms.topic: conceptual
author: lgayhardt
ms.author: lagayhar
ms.date: 09/19/2019
ms.reviewer: sdash
ms.openlocfilehash: f135aa6c0a4a55f8a42fd858572cc811e25b27c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671112"
---
# <a name="troubleshooting"></a>Sorun giderme

Bu makale, kullanılabilirlik izleme kullanırken oluşabilecek sık karşılaşılan sorunları gidermenize yardımcı olacaktır.

## <a name="ssltls-errors"></a>SSL/TLS hataları

|Belirti/hata iletisi| Olası nedenler|
|--------|------|
|SSL/TLS Secure Channel oluşturulamadı  | SSL sürümü. Yalnızca TLS 1.0, 1.1 ve 1.2 desteklenir. **SSLv3 desteklenmez.**
|TLSv1.2 Kayıt Katmanı: Uyarı (Düzey: Ölümcül, Açıklama: Kötü Kayıt MAC)| [Daha fazla bilgi](https://security.stackexchange.com/questions/39844/getting-ssl-alert-write-fatal-bad-record-mac-during-openssl-handshake)için StackExchange iş parçacığına bakın.
|Başarısız olan URL BIR CDN 'dir (İçerik Dağıtım Ağı) | Bu, CDN'nizdeki bir yanlış yapılandırmadan kaynaklanabilir |  

### <a name="possible-workaround"></a>Olası geçici çözüm

* Sorunu yaşayan URL'ler her zaman bağımlı kaynaklara bağlıysa, web sınaması için **bağımlı istekleri ayrıştırmak** önerilir.

## <a name="test-fails-only-from-certain-locations"></a>Test yalnızca belirli konumlardan başarısız olur

|Belirti/hata iletisi| Olası nedenler|
|----|---------|
|Bağlı taraf bir süre sonra düzgün yanıt vermediği için bir bağlantı girişimi başarısız oldu  | Belirli konumlardaki test aracıları bir güvenlik duvarı tarafından engelleniyor.|
|    |Belirli IP adreslerinin yeniden yönlendirilmesi (Yük Dengeleyicileri, Coğrafi trafik yöneticileri, Azure Express Rotası.) 
|    |Azure ExpressRoute kullanıyorsanız, [Asimetrik Yönlendirme'nin gerçekleştiği](https://docs.microsoft.com/azure/expressroute/expressroute-asymmetric-routing)durumlarda paketlerin bırakılabildiği senaryolar vardır.|

## <a name="test-failure-with-a-protocol-violation-error"></a>Protokol ihlali hatası yla test hatası

|Belirti/hata iletisi| Olası nedenler| Olası Çözümler |
|----|---------|-----|
|Sunucu bir protokol ihlali işledi. Bölüm=ResponseHeader Detay=CR LF tarafından takip edilmelidir | Bu, hatalı biçimlendirilmiş üstbilgi algılandığında oluşur. Özellikle, bazı üstbilgi, HTTP belirtimini ihlal eden satır sonunu belirtmek için CRLF kullanmıyor olabilir. Uygulama Öngörüleri bu HTTP belirtimini zorlar ve hatalı biçimlendirilmiş üstbilgilerle yanıtları başarısız olur.| a. Hatalı sunucuları düzeltmek için web sitesi ana bilgisayar sağlayıcısına /CDN sağlayıcısına başvurun. <br> b. Başarısız isteklerin kaynak olması durumunda (örn. stil dosyaları, resimler, komut dosyaları), bağımlı istekleriayrıştırmayı devre dışı bırakmayı düşünebilirsiniz. Unutmayın, bunu yaparsanız bu dosyaların kullanılabilirliğini izleme yeteneğini kaybedersiniz).

> [!NOTE]
> URL, HTTP üstbilgilerinin rahat bir şekilde doğrulanmasına sahip tarayıcılarda başarısız olmayabilir. Bu sorunun ayrıntılı bir açıklaması için bu blog gönderisine bakın: http://mehdi.me/a-tale-of-debugging-the-linkedin-api-net-and-http-protocol-violations/  

## <a name="common-troubleshooting-questions"></a>Sık karşılaşılan sorun giderme soruları

### <a name="site-looks-okay-but-i-see-test-failures-why-is-application-insights-alerting-me"></a>Site sorunsuz görünüyor, ancak test hataları görüyorum? Application Insights neden beni uyarıyor?

   * Testinizin **Parse bağımlı istekleri** etkin mi? Bu, komut dosyaları, resimler vb. gibi kaynakların sıkı bir şekilde kontrol edilebilmektedir. Bu tür hatalar tarayıcıda fark edilemeyebilir. Tüm görüntüleri, betikleri, stil sayfalarını ve sayfa tarafından yüklenen diğer dosyaları denetleyin. Bunlardan herhangi biri başarısız olursa, ana HTML sayfası sorunsuz yüklenirse bile test başarısız olarak bildirilir. Testi bu tür kaynak hatalarına karşı duyarsızlaştırmak için, ayrıştırık bağımlı isteklerini test yapılandırmasından kaldırın.

   * Geçici ağ hatalarından kaynaklanan gürültü olasılığını azaltmak için, test hataları yapılandırması için yeniden denemeleri etkinleştir'in denetlediğinden emin olun. Ayrıca, konuma özgü gereksiz uyarılara neden olan sorunları önlemek için daha fazla konumdan test edebilir ve buna göre uyarı kuralı eşiğini yönetebilirsiniz.

   * Hatayı neden bildirdiğimiz hakkındaki ayrıntıları görmek için Kullanılabilirlik deneyimindeki kırmızı noktalardan herhangi birini veya Arama gezgininin kullanılabilirlik hatalarını tıklatın. Test sonucu, ilişkili sunucu tarafı telemetrisi ile birlikte (etkinse) testin neden başarısız olduğunu anlamanız gerekir. Geçici sorunların yaygın nedenleri ağ veya bağlantı sorunlarıdır.

   * Test zaman doldu mu? 2 dakika sonra testleri iptal ediyoruz. Ping veya çok adımlı testiniz 2 dakikadan uzun sürüyorsa, bunu bir hata olarak bildireceğiz. Testi daha kısa sürelerde tamamlayabilen birden çok teste bölmeyi düşünün.

   * Tüm konumlar arıza mı bildirdi, yoksa sadece bazıları mı? Yalnızca bazı bildirilen hatalar varsa, bunun nedeni ağ/CDN sorunları olabilir. Yine, kırmızı nokta tıklayarak neden konumu hataları bildirilen anlamanız gerekir.

### <a name="i-did-not-get-an-email-when-the-alert-triggered-or-resolved-or-both"></a>Uyarı tetiklendiğinde veya çözüldüğünde veya her ikisi de çözüldüğünde bir e-posta almadım?

E-postanızın doğrudan listeli olduğunu veya üzerinde olduğunuz dağıtım listesinin bildirim alacak şekilde yapılandırıldığını doğrulamak için klasik uyarı yapılandırmasını kontrol edin. Öyleyse, harici e-posta alabileceğini doğrulamak için dağıtım listesi yapılandırmasını kontrol edin. Ayrıca, posta yöneticinizin bu soruna neden olabilecek herhangi bir ilkeyi yapılandırıp yapılandırmadığını da kontrol edin.

### <a name="i-did-not-receive-the-webhook-notification"></a>Webhook bildirimini almadım mı?

Webhook bildirimialan uygulamanın kullanılabilir olduğundan ve webhook isteklerini başarıyla işlediğinden emin olun. Daha fazla bilgi için [bu](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) bakınız.

### <a name="intermittent-test-failure-with-a-protocol-violation-error"></a>Protokol ihlali hatası ile aralıklı test hatası?

Hata ("protokol ihlali..CR’den sonra LF gelmelidir") sunucu (veya bağımlılıklar) ile ilgili bir sorun olduğunu gösterir. Bu durum, yanıtta hatalı biçimlendirilmiş üst bilgiler ayarlandığında meydana gelir. Yük dengeleyiciler veya CDN'lerden kaynaklanabilir. Özellikle, bazı üstbilgi, HTTP belirtimini ihlal eden ve bu nedenle .NET WebRequest düzeyinde doğrulamayı başarısız olan satır sonunu belirtmek için CRLF kullanmıyor olabilir. İhlal edilebilir nokta üstbilgilerine verilen yanıtı inceleyin.

> [!NOTE]
> URL, HTTP üstbilgilerinin rahat bir şekilde doğrulanmasına sahip tarayıcılarda başarısız olmayabilir. Bu sorunun ayrıntılı bir açıklaması için bu blog gönderisine bakın: http://mehdi.me/a-tale-of-debugging-the-linkedin-api-net-and-http-protocol-violations/  

### <a name="i-dont-see-any-related-server-side-telemetry-to-diagnose-test-failures"></a>Test hatalarını tanılamak için ilgili sunucu tarafı telemetrisi görmüyorum?*

Sunucu tarafı uygulamanız için Application Insights ayarlanmışsa, bunun nedeni [örnekleme](../../azure-monitor/app/sampling.md) işleminin devam ediyor olması olabilir. Farklı bir kullanılabilirlik sonucu seçin.

### <a name="can-i-call-code-from-my-web-test"></a>Web testimden kod çağırabilir miyim?

Hayır. Test adımları .webtest dosyasında olmalıdır. Bu nedenle, başka web testlerini çağıramaz, döngüleri kullanamazsınız. Ancak yararlı bulabileceğiniz bir dizi eklenti vardır.


### <a name="is-there-a-difference-between-web-tests-and-availability-tests"></a>"Web testleri" ve "kullanılabilirlik testleri" arasında bir fark var mı?

Bu iki terim birbirlerinin yerine kullanılabilir. Kullanılabilirlik testleri, çok adımlı web testlerine ek olarak tek URL ping testlerini de içeren daha genel bir terimdir.

### <a name="id-like-to-use-availability-tests-on-our-internal-server-that-runs-behind-a-firewall"></a>Kullanılabilirlik testlerini, güvenlik duvarının arkasında çalışan kendi dahili sunucumuzda kullanmayı tercih ediyorum.

   İki olası çözümü vardır:

   * Güvenlik duvarınızı, [Web testi aracılarımızın IP adreslerinden](../../azure-monitor/app/ip-addresses.md) gelen isteklere izin verecek şekilde yapılandırın.
   * İç sunucunuzu düzenli olarak test etmek için kendi kodunuzu yazın. Kodu, güvenlik duvarınızın arkasındaki bir test sunucusunda arka plan işlemi olarak çalıştırın. Test işleminiz, temel SDK paketindeki [TrackAvailability()](https://docs.microsoft.com/dotnet/api/microsoft.applicationinsights.telemetryclient.trackavailability) API’sini kullanarak sonuçları Application Insights’a gönderebilir. Bunun için test sunucunuzun Application Insights alım uç noktası ile giden bağlantısının olması gerekir, ancak bu, gelen isteklere izin vermeye göre çok daha küçük bir güvenlik riski oluşturur. Deneyim portal üzerinden oluşturulan testler için kullanılabilir ne biraz basitleştirilmiş olsa sonuçları kullanılabilirlik web testleri bıçakları görünür. Özel kullanılabilirlik testleri, Analytics, Arama ve Ölçümler'de kullanılabilirlik sonuçları olarak da görünür.

### <a name="uploading-a-multi-step-web-test-fails"></a>Çok adımlı web testi yüklenemiyor

Bunun bazı nedenleri olabilir:
   * 300 K boyut sınırı vardır.
   * Döngüler desteklenmez.
   * Başka web testlerine başvurular desteklenmez.
   * Veri kaynakları desteklenmez.

### <a name="my-multi-step-test-doesnt-complete"></a>Çok adımlı testim tamamlanmadı

Test başına 100 istek sınırı var. Ayrıca, iki dakikadan uzun çalışırsa test durdurulur.

### <a name="how-can-i-run-a-test-with-client-certificates"></a>İstemci sertifikasıyla testi nasıl çalıştırırım?

Bu şu anda desteklenmiyor.

## <a name="who-receives-the-classic-alert-notifications"></a>(Klasik) uyarı bildirimlerini kim alır?

Bu bölüm yalnızca klasik uyarılar için geçerlidir ve yalnızca istediğiniz alıcıların bildirim aldığından emin olmak için uyarı bildirimlerinizi optimize etmenize yardımcı olur. [Klasik uyarılar](../platform/alerts-classic.overview.md)ve yeni uyarılar deneyimi arasındaki fark hakkında daha fazla bilgi için [uyarılara genel bakış makalesine](../platform/alerts-overview.md)bakın. Yeni uyarılar deneyimi uyarı bildirimi kontrol etmek için [eylem grupları](../platform/action-groups.md)kullanın.

* Klasik uyarı bildirimleri için belirli alıcıların kullanılmasını öneririz.

* X'in Y konumlarından çıkan arızalarla ilgili uyarılar **için, etkinse toplu/grup** onay kutusu seçeneği, yönetici/ortak yönetici rolleriyle kullanıcılara gönderir.  Esasen _aboneliğin_ _tüm_ yöneticileri bildirim alır.

* Kullanılabilirlik ölçümlerindeki uyarılar **için, etkinleştirilmişse toplu/grup** onay kutusu seçeneği, abonelikte sahibi, katılımcısı veya okuyucu rolü olan kullanıcılara gönderir. Sonuç olarak, abonelik erişimi olan _tüm_ kullanıcılar Uygulama Öngörüleri kaynağı kapsamındadır ve bildirim alırsınız. 

> [!NOTE]
> Şu anda **toplu/grup** onay kutusu seçeneğini kullanıyorsanız ve devre dışı ederseniz, değişikliği geri alamazsınız.

Kullanıcıları rollerine göre bilgilendirmeniz gerekiyorsa, yeni uyarı deneyimini/neredeyse gerçek zamanlı uyarıları kullanın. [Eylem gruplarıyla,](../platform/action-groups.md)e-posta bildirimlerini katkıda bulunan/sahip/okuyucu rollerinden herhangi biriyle kullanıcılara yapılandırabilirsiniz (tek bir seçenek olarak biraraya getirilmez).

## <a name="next-steps"></a>Sonraki adımlar

* [Çok adımlı web testi](availability-multistep.md)
* [URL ping testleri](monitor-web-app-availability.md)

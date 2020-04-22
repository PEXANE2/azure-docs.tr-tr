---
title: Azure Media Player Changelog
description: Azure Media Player changelog.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: 15f8a3ac8c2777b3a878de92db495e559f64ad20
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726553"
---
# <a name="changelog"></a>Changelog #

## <a name="224-official-update-february-22-2019"></a>2.2.4 (Resmi Güncelleme 22 Şubat 2019) ##

### <a name="bug-fixes-224"></a>Hata Düzeltmeleri 2.2.4 ###

- [Hata Düzeltme] [AMP] [Erişilebilirlik] Hata ekranı göründüğünde erişilebilen bir hayalet sekmesi kaldırıldı
- [Hata Düzeltme] [AMP] IE11 ve Edge için hotkey 'M' düzeltildi
- [Hata Düzeltme] [AMP] CEA708 altyazıları için bir özel durum düzeltildi
- [Hata Düzeltme] [AMP] Edge tarayıcısı için video dondurma sorunu düzeltildi

### <a name="changes-224"></a>Değişiklikler 2.2.4 ###

- [Değiştir] [AMP] Bir parça şifre çözme hatası olduğunda, oyuncu oynatmayı kurtarmak için akımı ve çeşitli parçaları yeniden dener
- [Değiştir] [AMP] AMP'yi çakışan video veya ses parçalarına karşı daha toleranslı hale getirdi

## <a name="223-official-update-january-9-2019"></a>2.2.3 (Resmi Güncelleme 9 Ocak 2019) ##

### <a name="features"></a>Özellikler ###

- [Özellik] [HLS] Safari HLS oynatma için ses parça menüsü eklendi

### <a name="bug-fixes-223"></a>Hata Düzeltmeleri 2.2.3 ###

- [Hata Düzeltme] [AMP] [Erişilebilirlik] Canlı yayın oynatma sırasında klavye kullanılarak "canlı" düğmesi seçilemez
- [Hata Düzeltme] [AMP] Başarısız MSE testi nedeniyle 0x0400003 hataları düzeltildi
- [Hata Düzeltme] [AMP] Canlı akış başlatılırken videonun donabileceği bir sorun düzeltildi

### <a name="changes-223"></a>Değişiklikler 2.2.3 ###

- [Değiştir] [AMP] Daha iyi tanılama sağlamak için günlükte daha fazla bilgi eklendi
- [Değiştir] [AMP] Aynı ekran çözünürlükte birden fazla bit hızı kullanılabilirolduğunda, tüm bitrate'ler seçim için kullanılabilir

## <a name="222-official-update"></a>2.2.2 (Resmi Güncelleme) ##

### <a name="bug-fixes-222"></a>Hata Düzeltmeleri 2.2.2 ###

- [Hata Düzeltme] [AMP] Oyuncu geçici bir ağ kesintisiile karşılaştığında, oynatmayı hemen durdurur
- [Hata Düzeltme] [AMP] [Erişilebilirlik] Hata iletişim kutusuna klavyeyle erişilemez
- [Hata Düzeltme] [AMP] Desteklenmeyen hata yerine yalnızca varlık ses çalarken görüntülenen sonsuz spinner

### <a name="changes-222"></a>Değişiklikler 2.2.2 ###

- [Değiştir] [AMP] reklam AraBirimi için yerelleştirilmiş dizeleri eklendi

## <a name="221-official-update"></a>2.2.1 (Resmi Güncelleme) ##

### <a name="features-221"></a>Özellikler 2.2.1 ###

- [Özellik] [CMAF] HLS CMAF için ek destek

### <a name="bug-fixes"></a>Hata Düzeltmeleri ###

- [Hata Düzeltme] [AMP] yeniden deneme mantığı oynatma hataları verimli [AMP] belirsiz zamanlayıcılar
- [Hata Düzeltme] [AMP] [Firefox] sona erdi olay Firefox ve Chrome canlı programı durduruldu ateş değil
- [Hata Düzeltme] [AMP] Denetimler oyuncu seçeneklerinde yanlış olarak ayarlandığında bile setsource'dan sonra görüntülenen denetimler

### <a name="changes"></a>Değişiklikler ###

- [Değiştir] [Canlı altyazı] CEA altyazılarının API adı 608'den 708'e değiştirildi. Daha fazla bilgi için [CEA708 Resim Yazıları Ayarları'na](https://docs.microsoft.com/javascript/api/azuremediaplayer/amp.player.cea708captionssettings) bakın-->

## <a name="220-official-release"></a>2.2.0 (Resmi Yayın) ##

### <a name="features-220"></a>Özellikler 2.2.0 ###

- [Özellik] [Azurehtml5JS] [Flash] [LiveCaptions] Net ve AES içeriği için Azurehtml5JS ve FlashSS teknolojisinde CEA 708 altyazı desteği.

### <a name="bug-fixes-220"></a>Hata Düzeltmeleri 2.2.0 ###

- [Hata Düzeltme] Chrome/Edge'de çalışmıyor Flash sürüm algılama

### <a name="changes-220"></a>Değişiklikler 2.2.0 ###

- [Değiştir] [AMP] [Sezgisel] Sezgisel profil adı QuickStartive'dan LowLatency'ye değiştirildi
- [Değiştir] [Flash] Yeni Adobe Flash güncellemesiile AES içeriğinin oynatılmasını sağlamak için sürüm algılaması için Flash oynatıcıda değişiklik.

## <a name="219-official-hotfix"></a>2.1.9 (Resmi Düzeltme) ##

### <a name="bug-fixes-219"></a>Hata Düzeltmeleri 2.1.9 ###

- [Hata Düzeltme] [Canlı] İsteğe bağlı videoya/canlı arşivlere canlı akışgeçişinde oluşan özel durum

### <a name="changes-219"></a>Değişiklikler 2.1.9 ###

- [Değiştir] [Flash] [AES] Adobe Flash 30 itibariyle kullanımı engelledi olarak AES şifre çözme için paylaşılan bytearrays kullanmak için modifiye Flash teknoloji mantığı. Adobe, v30'daki bir hata nedeniyle Flash'ın yeni bir sürümünü dağıttıktan sonra oynatma nın yalnızca işe yaraacağını unutmayın. Daha fazla bilgi için [lütfen bilinen sorunlara](azure-media-player-known-issues.md) bakın

## <a name="218-official-update"></a>2.1.8 (Resmi Güncelleme) ##

### <a name="bug-fixes-218"></a>Hata Düzeltmeleri 2.1.8 ###

- [Hata düzeltmesi] Spinner bazen sonrası arama ve ön-play göstermez
- [Hata Düzeltme] Sessiz seçenek etkinleştirildiğinde oyuncu sessiz başlamaz
- [Hata Düzeltme] Denetimler false olarak ayarlandığında birim kaydırıcısı görüntülenir
- [Hata Düzeltme] Kullanıcı canlı kenarına atladığında zaman zaman yineleme
- [Hata Düzeltme] [Firefox] Oyuncu bazen yük javascript özel durum atar
- [Hata Düzeltme] [Erişilebilirlik] Klavye denetimleri kullanılarak seçildiğinde Yürüt/ Duraklat/Ses düğmesi odak anahattını kaybeder
- [Hata Düzeltme] Oyuncudaki sabit bellek sızıntısı bertaraf edilir
- [Hata Düzeltme] Oyuncu hatalarından sonra src() çağırmak kaynağı sıfırlamaz
- [Hata Düzeltme] [Canlı] Yayın sona erdikten sonra kullanıcı Canlı düğmesini tıklattığında AMP sürekli yükleme durumundadır
- [Hata Düzeltme] [Krom] Tarayıcı arka plana küçültüldüğünde oyuncu asılı kalır ve oynatma başarısız olur.

### <a name="changes-218"></a>Değişiklikler 2.1.8 ###

- [Değiştir] AES içeriği flash 30 ile oynaşınca görüntülemek için 0x0600001 errror güncellendi, çünkü şu anda desteklenmiyor. Daha fazla bilgi için [lütfen bilinen sorunlara](azure-media-player-known-issues.md) bakın
- [Değiştir] Bildirim 404 isteği nde veya boş bildirimleri döndürdüğünde canlı senaryolar için ek yeniden denemeler eklendi.

## <a name="217-official-update"></a>2.1.7 (Resmi Güncelleme) ##

### <a name="features-217"></a>Özellikler 2.1.7 ###

- [Özellik] [AzureHtml5JS] Ortam kaynağı arabelleğindeki eski verileri temizlemek için yapılandırma seçeneği eklendi

### <a name="bug-fixes-217"></a>Hata Düzeltmeleri 2.1.7 ###

- [Hata Düzeltme] [Erişilebilirlik] [Ekran Okuyucu] Başlık ayarlanmadığında oyuncunun dahil ettiği boş üstbilgi kaldırıldı
- [Hata Düzeltme] [UWA] Amp, Universal Windows App'te oynatma olduğunda özel durum oluşturur
- [Hata Düzeltme] [OSX] setActiveTextTrack() OSx Safari çalışmıyor
- [Hata Düzeltme] [Canlı] Oyuncu nun özel olarak atılması ve yeniden başlatılmasından sonra canlı kenarına tıkladığınızda
- [Hata Düzeltme] [Cilt] Belirli varlıklar için kesilen geçerli süre
- [Hata Düzeltme] [DRM] düzeltmesi, birden çok CENC DRM'yi destekleyen tarayıcılarda oynatmayı desteklemek için dahil

### <a name="changes-217"></a>Değişiklikler 2.1.7 ###

- [Değiştir] [Örnekler] [Erişilebilirlik] Tüm örneklere dil etiketi eklendi

## <a name="216-official-update"></a>2.1.6 (Resmi Güncelleme) ##

### <a name="bug-fixes-216"></a>Hata Düzeltmeleri 2.1.6 ###

- [Hata Düzeltme] Belirli bir varlık için yanlış süre yi gösteren AMP
- [Hata Düzeltme] [FairPlay-HLS] Fairplay hataları UI'ye yayılmaz
- [Hata Düzeltme] AMP 2.1.5'te özel Sezgisel özellikler göz ardı ediliyor.

### <a name="changes-216"></a>Değişiklikler 2.1.6 ###

- [Değiştir] [FairPlayDRM] PlayReady ve Widevine uygulamalarıyla eşitliği sağlamak için FairPlay için hem Cert isteği hem de lisans isteği için zaman arayı kaldırıldı
- [Değiştir] Bulanık içerikle mücadele için Misc Sezgisel iyileştirmeler

### <a name="features-216"></a>Özellikler 2.1.6 ###

- [Özellik] Destek mpd-time-cmaf formatı eklendi

## <a name="215-official-hotfix"></a>2.1.5 (Resmi Düzeltme) ##

### <a name="bug-fixes-215"></a>Hata Düzeltmeleri 2.1.5 ###

- [Hata Düzeltme] [Başlıklar] VTT stil oyuncu tarafından doğru işlenmedi
- [Hata Düzeltme] [Erişilebilirlik] Canlı düğmede arya etiketi yok

## <a name="214-official-update"></a>2.1.4 (Resmi Güncelleme) ##

### <a name="bug-fixes-214"></a>Hata Düzeltmeleri 2.1.4 ###

- [Hata Düzeltme] [Erişilebilirlik] [Odak] Kullanıcılar, denetim çubuğundaki tam ekran düğmesinin sağına eklenen özel düğmelere odaklanmak için sekemez
- [Hata Düzeltme] [IE11] [Ses çubuğu] Tam ekran modundayken IE11'de tüm video ekranı nın yanıp sönmesini toplu açılır pencereye tabbing yapar
- [Hata Düzeltme] [Cilt| Flush] Denetim çubuğu ile ses çubuğu açılır arasında görüntülenen boşluk
- [Hata Düzeltme] [AMP] [Başlıklar] Varolan bir oyuncuda kaynak değiştirildiğinde eski gömülü parçalar temizlenmez
- [Hata Düzeltme] [Erişilebilirlik] [Anlatıcı] Ekran Okuyucu ses denetimini yanlış okur
- [Hata Düzeltme] [Flashss] Play Event bazen Flash teknolojiden ateş lemiyor
- [Hata Düzeltme] [AMP] [Odak] Oynatma/duraklatma, oyuncu odaklanmışken ve tam ekran modundayken iki tıklama gerektirir
- [Hata Düzeltme] [AMP] [Cilt] Belirli bir varlık için ilerleme çubuğunda görüntülenen yanlış süre
- [Hata Düzeltme] [Reklamlar] [Reklam Uşak] VAST parser ilerleme olayı olmayan VAST dosya işlemez
- [Hata Düzeltme] [SDN] [AMP 2.1.1] Hive SDN eklenti desteği için sorun düzeltildi
- [Hata Düzeltme] [Erişilebilirlik] Kullanıcı ses düğmesine odaklandığında Ekran Okuyucusu "Gece Yarısı Sessiz Düğmesi"ni okur

### <a name="changes-214"></a>Değişiklikler 2.1.4 ###

- [Değiştir] [Erişilebilirlik] [Yardımcı Teknoloji] Düğmeler artık yardımcı teknoloji ile deneyimini geliştirmek için arya-canlı özelliği var
- [Değiştir] [Erişilebilirlik] [Ses düzeyi düğmesi| Ekran Okuyucusu]Sekme işlevini ve kaydırıcı davranışını değiştirerek ses düğmesinin erişilebilirliğini artırın. Bu değişiklikler klavye kullanıcılarının oyuncunun ses düzeyini değiştirmesini kolaylaştırır
- [Değiştir] Artırılmış etkinlik bağlamı bağlamı menü zaman 3'ten 5 saniyeye
- [Değiştir] [Erişilebilirlik] [Parlaklık] Resim yazısı ayarlarındaki açılır menülerde geliştirilmiş parlaklık kontrast oranı

## <a name="213-official-update"></a>2.1.3 (Resmi Güncelleme) ##

### <a name="bug-fixes-213"></a>Hata Düzeltmeleri 2.1.3 ###

- [Hata Düzeltme] [Eklentiler| Başlık Bindirme] Başlık Bindirme eklentisi AMP v2 ile JS özel durumlar atar. X+
- [Hata Düzeltme] Kaynak Kümesi olayı, günlüğe kaydetme kapatıldığında bile JavaScript konsoluna gönderilir
- [Hata Düzeltme] [Cilt] Oyuncu zaman ipuçları, her iki bitiş süresi çubuğunun üzerinde gezinirken oyuncunun bağlamı dışında işlenir
- [Hata Düzeltme] [Erişilebilirlik] [Ekran Okuyucu] Ekran Okuyucusu, görüntüleyen oyuncuya odaklandığında "Region Landmark" veya "Video Player Region Landmark" okur
- [Hata Düzeltme] [AMP] CSS üzerinden oyuncu anahatlarını devre dışı kalamaz
- [Hata Düzeltme] [Erişilebilirlik] Kullanıcı tam ekran modundayken tüm oyuncuya odaklanmak için sekemez
- [Hata Düzeltme] [Cilt] [Canlı] Japonca'da yerelleştirilmiş LIVE metnine yanıt vermeyen cilt
- [Hata Düzeltme] [Cilt] Akış 60 dk > -[Bug Fix][iPhone| Canlı]oynatıcı, denetim çubuğunda geçerli zaman/süre için metni gösterir
- [Hata Düzeltme] [AMP] Oyuncu heuristics API'leri arama JavaScript özel durumlar verir
- [Hata Düzeltme] [Yerel Html5|iOS] Videotag özelliği "playsinline" oynatıcıya yaymıyor
- [Hata Düzeltme] [iOS|iframe] Oyuncu bir iframe'e yüklenirse, oyuncu iPhone'da tam ekrana giremez
- [Hata Düzeltme] [AMP] [Sezgisel] AMP, oyuncu seçeneklerinden bağımsız olarak her zaman hibrit profille çalışır
- [Hata Düzeltme] [AMP| Win8.1], web görünümüyle Win8.1 uygulamasında barındırıldığında

### <a name="changes-213"></a>Değişiklikler 2.1.3 ###

- [Değiştir] [AMP] FragmentDownloadComplete olayında CDN uç nokta bilgisi eklendi
- [Değiştir] [AMP] [Canlı] Geliştirilmiş ve optimize edilmiş canlı akış gecikmesi

## <a name="212-official-hotfix"></a>2.1.2 (Resmi Düzeltme) ##

### <a name="bug-fixes-212"></a>Hata Düzeltmeleri 2.1.2 ####

- [Hata Düzeltme] [Erişilebilirlik] [Windows Anlatıcı] Kullanıcı ilerleme çubuğu bağlamına sahip olduğunda ve geçerli saat 00:00 olduğunda Ekran Okuyucusu "İlerleme gece yarısı" ifadesini okur
- [Hata Düzeltme] [Skin]logo boyutu JavaScript kodunda sabit kodlanmış
- [Erişilebilirlik] [Anahtarlar] Oyuncu tıklatıldığında önemli olan olmaz.

### <a name="changes-212"></a>Değişiklikler 2.1.2 ####

- [Değiştir] [Günlük günlüğü] Oyuncu bildirimi yüklemezse manifesto URL'sini günlüğe kaydedin

### <a name="features-212"></a>Özellikler 2.1.2 ###

- [Değiştir] [Performans] [Optimizasyon] Geliştirilmiş oyuncu yükü ve başlatma süreleri

## <a name="211-official-update"></a>2.1.1 (Resmi Güncelleme) ##

### <a name="bug-fixes-211"></a>Hata Düzeltmeleri 2.1.1 ####

- [Hata Düzeltme] [iOS] iOS için Safari'de Otomatik Oynatma'yı false yields'e ayarlama
- [Hata Düzeltme] İçerik süresinden daha büyük bir zaman arayışsonsuz spinner verir
- [Hata Düzeltme] Kısa yol tuşları, oyuncunun çalışması için birden çok klavye sekmesi gerektirir
- [Hata Düzeltme] Video, belirli varlıklarda oynatıcıyı yeniden boyutlandırmadan sonra birkaç saniye donar
- [Hata Düzeltme] Kullanıcı birden çok aramayı hızlı bir şekilde yaptığında sonsuz spinner (arama tamamlandıktan sonra)
- [Hata Düzeltme] Denetim çubuğu hareketsizlik sırasında gizli değil
- [Hata Düzeltme] AMP'yi barındıran bir web uygulamasının açılması, web sayfasının iki kez yüklenmesine neden olabilir
- [Hata Düzeltme] Flash Tech ile içerik belirli varlıkları oynarken sonsuz
- [Hata Düzeltme] 3. taraf eklentileriyle görüntülenmiyor diğer Seçenekler menüsü
- [Hata Düzeltme] [Cilt| Tube][Live] Oyuncu bir programın canlı kenarındayken iki canlı simge görüntülenir
- [Hata Düzeltme] [Cilt] Logo devre dışı tutulamaz
- [Hata Düzeltme] [DD+ İçerik] Dolby Digital ses kaydı içeren varlıklar için sürekli spinner ortaya çıkar
- [Hata Düzeltme] Canlı akış sırasında ses dil parçalarını değiştirirken en son AMP donuyor
- [Bug Fix] spinner için sabit arka plan kaybolması
- [Hata Düzeltme] AES flaş belirteci statik örnekleri hata düzeltmeleri sonsuz spinner

### <a name="changes-211"></a>Değişiklikler 2.1.1 ####

- [Değiştir] Widevine Https gereksinimi için Hata Kodu eklendi: Chrome v58 itibariyle, widevine içeriği `https://` protokol aracılığıyla yüklenmeli/oynatılmalıdır aksi takdirde oynatma başarısız olur.
- [Değiştir] Yardımcı teknolojinin içerik yüklenirken "video yüklemeyi" anlatabilmesi için yükleme spinner için aria etiketi eklendi  

## <a name="210-official-release"></a>2.1.0 (Resmi Yayın) ##

### <a name="features-210"></a>Özellikler 2.1.0 ###

- [Özellik] [AzureHtml5JS] Ön-orta-post-rolls için VOD Reklam Desteği
- [Özellik] [Beta] [AzureHtml5JS] Post-roll öncesi için Canlı Reklam desteği
- [Özellik] Yeni cilt seçeneği eklendi - AMP-flush
- [Özellik] Ekran okuyucular/yardımcı teknoloji ile daha iyi entegrasyon için geliştirilmiş arya etiketleri eklendi
- [Özellik] [Cilt] Skin artık tüm simgeleri ve düğmeleri yüksek kontrast modunda net bir şekilde gösterir

### <a name="bug-fixes-210"></a>Hata Düzeltmeleri 2.1.0 ###

- [Hata Düzeltme] Erişilebilirlik ve UI düzeltmelerinin sayısı
- [Hata Düzeltme] AMP IE9'da doğru yüklenmiyor

### <a name="changes-210"></a>Değişiklikler 2.1.0 ###

- [Değiştir] Reklamlar adedine uyum sağlamak için oyuncudaki DOM öğeleriyeniden yapılandırıldı
- [Değiştir] Cilt gelişimi için CSS'den SCSS'ye geçiş
- [Değiştir] [Örnekler] VOD reklamları için örnek eklendi
- [Değiştir] [Örnekler] Oynatma hızı için örnek eklendi
- [Değiştir] [Örnekler] Flush Skin için örnek eklendi

## <a name="200-beta-release"></a>2.0.0 (Beta Sürümü) ##

- [Değiştir], VJS5 olarak güncellendi
- [özellik] Oyuncu yanıt sıvısı için yeni sıvı API'si eklendi
- [Özellik] Oynatma hızı
- [Değiştir] Cilt için CSS'den SCSS'ye geçiş

## <a name="183-official-hotfix-update"></a>1.8.3 (Resmi Düzeltme Güncellemesi) ##

### <a name="bug-fixes-183"></a>Hata Düzeltmeleri 1.8.3 ###

- [Hata Düzeltme] [AzureHtml5JS] Negatif DTS'ye sahip bazı varlıklar Chrome'da oynatılamayacak

## <a name="182-official-hotfix-update"></a>1.8.2 (Resmi Düzeltme Güncellemesi) ##

### <a name="bug-fixes-182"></a>Hata Düzeltmeleri 1.8.2 ###

- [Hata Düzeltme] [AzureHtml5JS] AzureHtml5JS ile daha yüksek ses bithızları oynatılmıyor

## <a name="181-official-update"></a>1.8.1 (Resmi Güncelleme) ##

### <a name="bug-fixes-181"></a>Hata Düzeltmeleri 1.8.1 ###

- [Hata Düzeltme] [iOS] Yerel oyuncuda görünmeyen altyazılar/altyazılar
- [Hata Düzeltme] [AMP] Kimlik doğrulama belirteçleri oynatılmayan CDN destekli akış URL'leri
- [Hata Düzeltme] [FairPlay] Hata Kodunun FairPlay Hata Kodu eksik Tech ID (Hata Kodu Bit [31-28] ) daha fazla bilgi için Hata Kodları bakın
- [Hata Düzeltme] [Safari] [PlayReady] Safari'de PlayReady içeriği sonsuz spinner verimli

### <a name="changes-181"></a>Değişiklikler 1.8.1 ###

- [Değiştir] [Html5] VideoTag'tan olayları içerecek şekilde yerel Html5 teknoloji ayrıntılı günlüklerini değiştirme

## <a name="180-official-update"></a>1.8.0 (Resmi Güncelleme) ##

### <a name="features-180"></a>Özellikler 1.8.0 ###

- [Özellikler] [DRM] FairPlay Desteği eklendi (daha fazla bilgi için [Korumalı İçerik'e](azure-media-player-protected-content.md) bakın)

### <a name="bug-fixes-180"></a>Hata Düzeltmeleri 1.8.0 ###

- [Hata Düzeltme] [AMP] Ağ daraltıldığında kullanıcı araması bekleme olayını tetiklemiyor
- [Hata Düzeltme] [Flashss] Flash teknoloji kalite seçimi istisna atar
- [Hata Düzeltme] [AMP] Dinamik olarak kalite seçimi bağlam menüsünde gösterir
- [Hata Düzeltme] [Cilt] Bağlam menülerinin son menü öğesini seçmek zordur

### <a name="changes-180"></a>Değişiklikler 1.8.0 ###

- [Değiştir] Geçerli Chrome EME gereksinimlerine güncelleştirilmiş oynatıcı
- [Değiştir] Varsayılan techOrder yeni teknoloji karşılamak için değiştirildi- html5FairPlayHLS (daha fazla bilgi için [Korumalı İçerik](azure-media-player-protected-content.md) bakınız)
- [Değiştir] [AzureHtml5JS] Safari'de Etkin MPEG-Dash oynatma
- [Değiştir] [Örnekler] FairPlay'e uyum sağlamak için Çoklu DRM örnekleri değiştirildi

## <a name="174-official-hotfix-update"></a>1.7.4 (Resmi Düzeltme Güncellemesi) ##

### <a name="bug-fixes-174"></a>Hata Düzeltmeleri 1.7.4 ###

- [Hata Düzeltme] [Krom] Kullanıcı oyuncu bağlamına sahipolduğunda arama tutamacı çevresinde mavi anahat görünür
- [Hata Düzeltme] [IE9] IE9'da yüklenen oyuncuya JavaScript özel durumu

## <a name="173-official-hotfix-update"></a>1.7.3 (Resmi Düzeltme Güncellemesi) ##

### <a name="bug-fixes-173"></a>Hata Düzeltmeleri 1.7.3 ###

- [Hata Düzeltme] [AzureHtml5JS] Kısıtlı ağlarda oyuncu zamanlaması

### <a name="changes-173"></a>Değişiklikler 1.7.3 ###

- [Değiştir] AES içeriğinin şifresini çözmek için Edge'de Webcrypto'u etkinleştirme
- [Değiştir] Önbelleğe alınmış parçalar için amp sezgisel optimize etme
- [Değiştir] [AzureHtml5JS] Bant genişliği tahmini gecikmesüresini azaltarak sezgisel ioptimize etme

## <a name="172-official-hotfix-update"></a>1.7.2 (Resmi Düzeltme Güncellemesi) ##

### <a name="features-172"></a>Özellikler 1.7.2 ###
<!---API needs onboarding. Removed link to API until remedied.--->
- [Özellik] [AzureHtml5JS| Firefox] Firefox 47+ için EME ile Widevine oynatmayı etkinleştirin
- [Özellik] Oyuncu disposing için olay ekleme
<!-- ([disposing](index.html#static-amp.eventname.disposing)) -->

### <a name="bug-fixes-172"></a>Hata Düzeltmeleri 1.7.2 ###

- [Hata Düzeltme] Kodlanmış Akamai CDN URL sorgu parametreleri doğru çözülmüş değil
- [Hata Düzeltme] Özel durum manifestPlayableWindowLength atılıyor()
- [Hata Düzeltme] Görüntüleyici, video yeniden izlemek için sona erdikten sonra videoda oynat'ı her zaman tıklatamaz
- [Hata Düzeltme] Hızlı pencere boyutu değişikliklerine uymayan duyarlı boyutlandırma
- [Hata Düzeltme] [Kenar| IE] Genişlik=x, yükseklik=otomatik için sayfa yükünde etkili olmayan duyarlı boyutlandırma
- [Hata Düzeltme] [Android| Chrome] Chrome, içerik şifrelenmediğinde DRM içeriğini oynatma kinleri istiyor
- [Hata Düzeltme] [Erişilebilirlik] [Kenar] Klavye denetimleri bağlam menü öğelerini doğru seçmez
- [Hata Düzeltme] [Erişilebilirlik] Yüksek karşıtlık modunda eksik görüntükenar kenarlık
- [Hata Düzeltme] [Flashss] Oyuncu elden çıkarmadan kaldırılmayan fare kadar olay dinleyicisi özel durumlara neden olur
- [Hata Düzeltme] [Flashss] Kodlanmış boşluklarla ayrıştırma bildirimi URL'si sorunu
- [Hata Düzeltme] [iOS] Tech.featuresVolumeControl değerlendirilirken yazı hatası

### <a name="changes-172"></a>Değişiklikler 1.7.2 ###

- [Değiştir] [DRM] Yalnızca içeriğin şifrelendiğinde kontrol etmek için kaynağı ayarladıktan sonra taşınan DRM denetimleri
- [Değiştir] [AES] Anahtar teslim isteğinden tip/düz tanımlanmamış gövde kaldırıldı
- [Değiştir] [Erişilebilirlik] Windows anlatıcı şimdi bağlam özellikleri yerine oynatıcı üzerinde yken "Media Player" okur

## <a name="171-official-hotfix-update"></a>1.7.1 (Resmi Düzeltme Güncellemesi) ##

### <a name="features-171"></a>Özellikler 1.7.1 ###

- [Özellik] Hibrit Sezgisel profil için eklenen seçenek (bu profil varsayılan olarak ayarlanır)

### <a name="bug-fixes-171"></a>Hata Düzeltmeleri 1.7.1 ###

- [Hata Düzeltme] Duyarlı tasarım HTML5 standardına göre çalışmaz (width=%100, height=auto)
- [Hata Düzeltme] V1.7.0'da beklendiği gibi davranamayan genişlik ve yükseklik için yüzde değerleri

## <a name="170-official-update"></a>1.7.0 (Resmi Güncelleme) ##

### <a name="features-170"></a>Özellikler 1.7.0 ###
<!---API needs onboarding. Removed link until remedied.--->
- [Özellik] [AzureHtml5JS] [Flashss] Geçerli saatin kodlayıcı ortam saatini saniyeler içinde almak için currentMediaTime() eklendi
- [Özellik] [Flashss] VideoBufferData() ve audioBufferData() ile uygulanan indirme telemetri API'leri<!-- (see [BufferData](index.html#amp.bufferdata) for more details) -->
- [Özellik] [Flashss] 'downloadbitratechanged' etkinliği eklendi
- [Özellik] Yükleme süresi oyuncunun eski sürümlerine göre daha iyi
- [Özellik] Hatalar JavaScript konsoluna günlüğe kaydedilir

### <a name="bug-fixes-170"></a>Hata Düzeltmeleri 1.7.0 ###

- [Hata Düzeltme] Oyuncuda görüntülmeyen sorgu dize parametreleri içeren kodlanmış poster URL'si
- [Hata Düzeltme] Özel durum hiçbir teknoloji yüklü ve API amp attı. Player.poster() denir
- [Hata Düzeltme] Fonksiyonlar elden edildikten sonra oyuncuya erişmeye çalışırken atılan özel durum
- [Hata Düzeltme] [Erişilebilirlik] İlerleme çubuğuna odaklanma nın anahatlarını eksik kafa aramak
- [Hata Düzeltme] [Erişilebilirlik] Bağlam menülerinde yüksek kontrast modunda gölge vardır
- [Hata Düzeltme] [iOS] yerli oyuncu WebVTT altyazılar oynatma çalışmıyor
- [Hata Düzeltme] [AzureHtml5JS] Hata 0x0100002 yerine karışık içerik sonucu sonsuz spinner verir HTTPS sitesinde HTTP akışı oynarken gösterilmelidir
- [Hata Düzeltme] [AzureHtml5JS] Algılanan sonsuz arabelleğe alma durumunu görüntüleyen döngü sistem durumu denetimi hatasına neden olan eksik uç segmenti
- [Hata Düzeltme] [AzureHtml5JS] ManifestForLabel=false ve üç harfli dil kodları kullanıldığında menüdeki yanlış ses parça adı
- [Hata Düzeltme] [AzureHtml5JS| Chrome] Chrome'da JavaScript ile süre içinde kayan nokta hassasiyetinin neden olduğu içeriğin sonunda algılanan sonsuz arabellek durumu
- [Hata Düzeltme] [Flashss] Flash oynatıcı oluşturulduğunda kısa bir süre için ölümcül olmayan aralıklı hata görüntüleniyor
- [Hata Düzeltme] [Flashss] Video ve ses akışları "Fragment url (...) ile yuvarlama hatası nedeniyle farklı zaman ölçekleri kullandığında oynatma başarısız oldu FLVTags oluşturmak için başarısız oldu"
- [Hata Düzeltme] [Flashss] Kodlanmış boşluklarla bildirim urllerini ayrıştıran sorunlar
- [Hata Düzeltme] [Flashss] Flash player sürümü techOrder sonraki teknolojiye geri düşmek yerine oynatma bir hataya neden = 11.4 >olup olmadığını belirlemek için eksik kontrol
- [Hata Düzeltme] [Flashss] [AES] Alt çizerleri olan AES belirteçlerini kabul eden sorunlar
- [Hata Düzeltme] [SilverlightSS| OSX] "//" protokol (HTTP veya HTTPS) yerine bir manifesto öne, sonsuz spinner veren yerel bir dosya olarak kabul edilir

### <a name="changes-170"></a>Değişiklikler 1.7.0 ###

- [Değiştir] [Flashss] SWF Scripts ("MSAdaptiveStreamingPlugin-osmf2.0.swf" ve "StrobeMediaPlayback.2.0.swf") "StrobeMediaPlayback.2.0.swf" adlı tek bir SWF'de birleştirildi.
- [Değiştir] [Flashss] Daha kesin hata kodları (ör. 404s şimdi 0x30200194 yerine genel hata 0x30200000) sonuçlanır

## <a name="163-official-hotfix-update"></a>1.6.3 (Resmi Düzeltme Güncellemesi) ##

### <a name="bug-fixes-163"></a>Hata Düzeltmeleri 1.6.3 ###

- [Hata Düzeltme] Kısayol tuşları olay işleyicisi oynatıcının atılmasından sonra yürütüldüğünde JavaScript çalışma zamanı özel durumu
- [Hata Düzeltme] [Android] [AzureHtml5JS] Hücresel ağ kullanarak mobil cihazda oynatma yok
- [Hata Düzeltme] Web'i boşaltmak için web çalışanı olarak çalışacak Forge güncellendi

## <a name="162-official-hotfix-update"></a>1.6.2 (Resmi Düzeltme Güncellemesi) ##

### <a name="features-162"></a>Özellikler 1.6.2 ###

- [Özellik] Yerelleştirme için ek diller eklendi (daha fazla ayrıntı için belgelere bakın)

### <a name="bug-fixes-162"></a>Hata Düzeltmeleri 1.6.2 ###

- [Hata Düzeltme] [IE9-10] Window.blur'da en aza indirgenen IE9/IE10 hatası nedeniyle oyuncunun en aza indirilmiş tarayıcı penceresinin çevresindeki alanlara tıkladığınızda()
- [Hata Düzeltme] [Flashss] Alt çizer ile AES belirteçleri kabul etmeme

## <a name="161-official-hotfix-update"></a>1.6.1 (Resmi Düzeltme Güncellemesi) ##

### <a name="bug-fixes-161"></a>Hata Düzeltmeleri 1.6.1 ###

- [Hata Düzeltme] [Flashss| Kenar,IE][SilverlightSS| IE] Girişler veya IE/Edge'deki diğer kullanıcı arabirimi öğelerine odaklanamıyorum
- [Hata Düzeltme] Tanımlanmamış forge zaman AES oynatma başarısız
- [Hata Düzeltme] [Android] [AzureHtml5JS| Chrome] Sürekli spinner, sistem durumu kontrol döngüsünde yken içeriği oynatma
- [Hata Düzeltme] [IE9] console.log() IE 9 tarafından desteklenmiyor istisna

## <a name="160-official-update"></a>1.6.0 (Resmi Güncelleme) ##

### <a name="features-160"></a>Özellikler 1.6.0 ###

- [Özellik] azuremediaplayer.min.js%33 boyut azaltma
- [Özellik] [AzureHtml5JS| Edge'de DD+ ses akışları için Kenar][Test edilmemiş] Destek (ilk seçimden sonra codec geçişi yapılmaz). Uygulama şu anda doğru ses akışını seçmelidir.
- [Özellik] Sıcak anahtar denetimleri (daha fazla ayrıntı için dokümanlara bakın)
- [Özellik] Zaman doğru arama için ilerleme süresi ipucu hover
- [Özellik] KurulumEklentide Done yöntemi varsa eklentilerin async algılaması için izin ver

### <a name="bug-fixes-160"></a>Hata Düzeltmeleri 1.6.0 ###

- [Hata Düzeltme] Bellek günlüğü getMemoryLog(true) üzerinde kızarma değil
- [Hata Düzeltme] Fare üzerinde bit hızı seçim kutusu sıfırlanır fare denetimi ile daha düşük bit hızları seçme sorununa neden
- [Hata Düzeltme] DRM denetimi yaparken uygulama çökmesinde Mac Office çöküyor
- [Hata Düzeltme] CSS sınıfları yanlışlıkla üzerine kolayca yazılır
- [Hata Düzeltme] [Krom] Kullanıcı aracısı dize tarayıcısından güncelleme tanımlaması Edge'dir
- [Hata Düzeltme] [AzureHtml5JS] Kenar(Win10) veya Chrome(Mac) araç çubuğunda resim yazısı düğmesi görünmüyor
- [Hata Düzeltme] [Android] [AzureHtml5JS| Chrome] SonOfStream() kısa videolar arama geçersiz Durum Hatası özel durum
- [Hata Düzeltme] [Firefox] Tarayıcı özelliklerini kontrol ederken Firefox'un neden olduğu DRM uyarısının kaldırılması
- [Hata Düzeltme] [Html5] Aşamalı mp4 içeriğiyle gösterilmeyan Altyazı/Altyazılar
- [Hata Düzeltme] [Flashss] Eşleşen zaman damgaları olan iletiler ters sırada günlüğe kaydedildi
- [Hata Düzeltme] [Erişilebilirlik] [Krom| Firefox] Sekme ve select denetimleri otomatik olarak ilk menü öğeyi seçin
- [Hata Düzeltme] [Erişilebilirlik] Ses düzeyini kontrol etmek için sekme

### <a name="changes-160"></a>Değişiklikler 1.6.0 ###

- [Değiştir] Kalite düzeyi seçiminde AES şifre çözme süresini kullanma
- [Değiştir] Çoklu ses akışları için HLS v3'den önce HLS v4 kullanmak için URL rewriter'ını güncelleştirin
- [Değiştir] NativeControlsForTouch'ı varsayılan olarak false olarak ayarlayın (doğru çalışması için yanlış olmalıdır)

## <a name="150-official-update"></a>1.5.0 (Resmi Güncelleme) ##

### <a name="features-150"></a>Özellikler 1.5.0 ###

- [Özellik] Genel web güvenliği için geliştirmeler (enjeksiyonun önlenmesi, XSS, vb.)
- [Özellik] Sourceset olay ve options.sdn için SDN eklenti tümleştirme kancaları
- [Özellik] Oynatma sırasında 5XX ve 4XX hatalarının sağlamlık işleme

### <a name="bug-fixes-150"></a>Hata Düzeltmeleri 1.5.0 ###

- [Hata Düzeltme] Unicode yerine düğmeler için HTML varlık yazı tipi kodlarını kullanmak için CSS minification'ı güncelleştirme
- [Hata Düzeltme] [AzureHtml5JS] Multi-DRM içeriği her zaman ikinci DRM başarısız neden protectionInfo gelen ilk öğenin belirteci seçerek
- [Hata Düzeltme] [AzureHtml5JS] Eksik segmentleri olan bir alanda ararken aradığınız asla tamamlar.
- [Hata Düzeltme] [AzureHtml5JS| Kenar] PlayReady oynatma için Edge güncelleştirmesinde önceden belirlenmiş EME'yi etkinleştirme
- [Hata Düzeltme] [AzureHtml5JS| Firefox] Firefox v42 + (MSE ile) korumalı içerik için Silverlight geri dönüş sağlamak için EME kontrol güncelleyin
- [Hata Düzeltme] [Flashss] hata.iletiyi numaradan ayrıntılı dizeye güncelleştirme

### <a name="changes-150"></a>Değişiklikler 1.5.0 ###

- [Değiştir] Posterler şu anda yalnızca mutlak URL olarak çalışır.

## <a name="140-official-update"></a>1.4.0 (Resmi Güncelleme) ##

### <a name="features-140"></a>Özellikler 1.4.0 ###

- [Özellik] [AzureHtml5JS| Chrome] Basit Widevine DRM desteği
- [Özellik] [AzureHtml5JS] Oynatma sırasında 404/412 hataların sağlamlık işleme

### <a name="bug-fixes-140"></a>Hata Düzeltmeleri 1.4.0 ###

- [Hata Düzeltme] [Flashss] Parametre doğrulaması için geliştirme

## <a name="130-official-update"></a>1.3.0 (Resmi Güncelleme) ##

### <a name="features-130"></a>Özellikler 1.3.0 ###

- [Özellik] [AzureHtml5JS] [Flashss] Aynı codec Multi-Audio içeriğinin ses geçişi

### <a name="bug-fixes-130"></a>Hata Düzeltmeleri 1.3.0 ###

- [Hata Düzeltme] [AzureHtml5JS| Krom] Aralıklı sonsuz spinner
- [Hata Düzeltme] [AzureHtml5JS| IE][Windows Phone] Windows Phone'un oynatma sorunlarına neden olan özel durum
- [Hata Düzeltme] [Flashss] Otomatik oynatma ek örnekler için false başarısız olarak ayarlanmış
- [Hata Düzeltme] UI menü boyutlandırma sorunları

## <a name="120-official-update"></a>1.2.0 (Resmi Güncelleme) ##

### <a name="features-120"></a>Özellikler 1.2.0 ###

- [Özellik] [AzureHtml5JS| MSE etkinleştirildiğinde Firefox] Desteği
- [Özellik] Artık geri dönüş teknoloji ikilileri (swf, xap) için yollar sağlamak için uygulama gerektirir. Yol, Azure Media Player komut dosyasına göredir.

### <a name="bug-fixes-120"></a>Hata Düzeltmeleri 1.2.0 ###

- [Hata Düzeltme] [AzureHtml5JS| Chrome] Oyuncu arka planda oyuncu canlı kenarın arkasında sürükleniyor
- [Hata Düzeltme] [AzureHtml5JS| Kenar] Tam ekran çalışmıyor
- [Hata Düzeltme] [AzureHtml5JS] Seçenekler ayarlandığında günlüğe kaydetme düzgün etkinleştirilememiş
- [Hata Düzeltme] [Flash] Bekleme olayı sırasında hem "arabellek" hem de arabelleğe alma simgesi gösterisi
- [Hata Düzeltme] İlk bant genişliği isteği başarısız olursa oynatmanın devam etmesine izin ver
- [Hata Düzeltme] Oyuncu tanımlanmamış seçeneklerle baş harfe bildiğinde yüklemeyi başaramaz
- [Hata Düzeltme] Oyuncu zaten elden çıkarıladıktan sonra elden çıkarmaya çalışırken, bir vdata özel durumu oluşur
- [Hata Düzeltme] Kalite çubuğu simgeleri yanlış eşlendi

## <a name="111-official-hotfix-update"></a>1.1.1 (Resmi Düzeltme Güncellemesi) ##

### <a name="bug-fixes-111"></a>Hata Düzeltmeleri 1.1.1 ###

- [Hata Düzeltme] Eski IE tam ekran sorunu
- [Hata Düzeltme] Eklentiler artık üzerine yazılmamış

## <a name="110-official-update"></a>1.1.0 (Resmi Güncelleme) ##

### <a name="features-110"></a>Özellikler 1.1.0 ###

- [Özellik] Kullanıcı Birleşme Yerelleşme dizeli

### <a name="bug-fixes-110"></a>Hata Düzeltmeleri 1.1.0 ###

- [Hata Düzeltme] Büyük Oynat Düğmesi yeterli kontrasta sahip değil
- [Hata Düzeltme] Görsel sekme odak göstergesi
- [Hata Düzeltme] Doğru çözünürlük bilgilerini kullanarak Bitrate menüsünü şimdi seçin
- [Hata Düzeltme] Daha fazla seçenek menüsü artık dinamik olarak boyutlandırılmış
- [Hata Düzeltme] Çeşitli UI sorunları

## <a name="100-official-release"></a>1.0.0 (Resmi Yayın) ##

### <a name="features-100"></a>Özellikler 1.0.0 ###

- [Özellik] Sekme kontrolü, odak kontrolü, ekran okuyucu, yüksek kontrastlı web bilgisi için temel erişilebilirlik testi
- [Özellik] Güncelleştirilmiş UI
- [Özellik] Dev günlüğü
- [Özellik] Altyazı/altyazı parçalarını dinamik olarak ayarlamak için API
- [Özellik] Temel yerelleştirme özellikleri
- [Özellik] Teknisyenler arasında hata kodu birleştirme
- [Özellik] Eklentilerin (Flash veya Silverlight gibi) yüklenmediğinde için yeni hata kodu
- [Özellik] [AzureHtml5JS] Uygulanan temel tanılama olayları

### <a name="bug-fixes-100"></a>Hata Düzeltmeleri 1.0.0 ###
<!---What is that actually supposed to say?--->
- [Hata Düzeltme] [AzureHtml5JS] Zaman damgasında küçük belirsizlikler olduğunda MPD güncellemelerinde canlı oynatma dondurma
- [Hata Düzeltme] [AzureHtml5JS] Birkaç Canlı oynatma sorunu azaltıldı
- [Hata Düzeltme] [AzureHtml5JS] Pencere boyutu sezgisel olduğunda arabellekleri temizle ve daha yüksek çözünürlüklü bir ekrana gidin
- [Hata Düzeltme] [AzureHtml5JS] Chrome artık sona eren olayı düzgün bir şekilde gösteriyor. Chrome'un bilinen önceki sayısına bağlı *olarak AzureHtml5JS kullanırken â&euro;œendedâ&euro;olayını düzgün bir şekilde göndermez. Altta yatan tarayıcıda bir sorun var.*
- [Hata Düzeltme] [AzureHtml5JS] *AzureHtml5JS teknolojisiyle OSX Yosemite ile Oynatma sorununu gidermek için bu teknoloji için Devre Dışı Safari. MSE uygulama sorunları vardır. Geçici Azaltma: kuvvet&euro;â&euro;œflashSSâ&euro;, â œsilverlightSSâ&euro;bu kullanıcı ajanları için teknoloji sipariş olarak*
- [Hata Düzeltme] [FlashSS] loadstart hata oluştuktan sonra ateşlendi

## <a name="020-beta"></a>0.2.0 (Beta) ##

### <a name="features-020"></a>Özellikler 0.2.0 ###

- [Özellik] İsteğe bağlı ve canlı olarak PlayReady ve AES için tamamlanan test - uyumluluk matrisine bakın
- [Özellik] Kullanım Süreksizlikleri
- [Özellik] 2^53'ten büyük zaman damgaları için destek
- [Özellik] URL sorgu parametresi bildirim isteğine devam eder
- [Özellik] [Test edilmemiş] Destek `QuickStart` ve `HighQuality` sezgisel profiller
- [Özellik] [Test edilmemiş] AzureHtml5JS ve FlashSS'ta bit hızları, genişlik ve yükseklik için video akışı bilgilerini açığa çıkarma
- [Özellik] [Test edilmemiş] AzureHtml5JS ve FlashSS'ta Bitrate'yi seçin (bkz. API belgeleri)

### <a name="bug-fixes-020"></a>Hata Düzeltmeleri 0.2.0 ###

- [Hata Düzeltme] büyük oynatma düğmesi artık WP8.1'de görüntülenebilir
- [Hata Düzeltme] birden çok canlı oynatma sorunları düzeltildi
- [Hata Düzeltme] unmute düğmesi artık UI'de çalışıyor
- [Hata Düzeltme] otomatik oynatma modu için güncelleştirilmiş UI yükleme deneyimi
- [Hata Düzeltme] AMD yükleyici sorunu ve yöntem çakışmaları tanımlamak
- [Hata Düzeltme] WP 8.1 Cordova Uygulaması yükleme sorunu
- [Hata Düzeltme] Korumalı içerik sorguları platform/teknoloji koruma için uygun teknolojiyi seçmek için ProtectionType'ı destekletir.  Chrome _(masaüstü) / Safari 8 (OSX Yosemite)_ üzerinde ' PlayReady içerik bilinen önceki sorunu giderir şu anda Silverlight oyuncu geri değil '
- [Hata Düzeltme] WinServer 2012 R2 Media Foundation varsayılan olarak bu makinede yüklü değil nedeniyle yakalanmış istisna.  Uygulanmayan HTML video etiketi API'lerini kullanmaya çalışarak hata yapmayı n için sorun. Geçerli azaltma, bu hatayı yakalamak ve hatayı atmak yerine yanlış döndürmektir.
- [Bug Fix] her zaman arama veya http oynatma sırasında hataları önlemek için başarısız sonra init segmentolsun
- [Hata Düzeltme] hata oluştuğunda benzetimli ilerleme ve zaman güncelleştirmelerini izlemeyi kapatın.
- [Hata Düzeltme] sağ tıklama menüsünü kaldır
- [Hata Düzeltme] [AzureHtml5JS] hata iletisi, PlayReady içeriği için geçersiz belirteç ayarlandığında görüntülenmiyor
- [Hata Düzeltme] [AzureHtml5JS] canlı oynatma sırasında tam ekran gidiyor dikkate pencere boyutu buluşsal alarak değildi
- [Hata Düzeltme] [Flashss] Kaldırılan Strobe Media Player iletileri yalnızca Azure Media Player iletilerinin gösterilmesi için görüntülenir
- [Hata Düzeltme] [SilverlightSS] biz süre veya 0'dan az aramak zaman 'aranmış' olay almıyorsanız

## <a name="010-beta-release"></a>0.1.0 (Beta Sürümü) ##

İlk Ön Sürüm

## <a name="next-steps"></a>Sonraki adımlar ##

- [Azure Media Player Quickstart](azure-media-player-quickstart.md)

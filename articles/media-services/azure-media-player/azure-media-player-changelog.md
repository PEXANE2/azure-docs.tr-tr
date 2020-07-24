---
title: CHANGELOG Azure Media Player
description: CHANGELOG Azure Media Player.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: overview
ms.date: 04/20/2020
ms.openlocfilehash: fc8304fb068152c800d7b71f77fb601956fb6510
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87023408"
---
# <a name="changelog"></a>Değişiklik günlüğü #

## <a name="224-official-update-february-22-2019"></a>2.2.4 (resmi güncelleştirme Şubat 22 2019) ##

### <a name="bug-fixes-224"></a>Hata düzeltmeleri 2.2.4 ###

- [Hata düzeltildi] AMP Larınızdaki Hata ekranı göründüğünde erişilebilir hayalet sekmesi kaldırıldı
- [Hata düzeltildi] AMP IE11 ve Edge için kısayol tuşu düzeltildi
- [Hata düzeltildi] AMP CEA708 başlıkları için özel durum düzeltildi
- [Hata düzeltildi] AMP Edge tarayıcısı için bir video dondurma sorunu düzeltildi

### <a name="changes-224"></a>2.2.4 değişiklikleri ###

- Değişebilir AMP Bir parça şifre çözme hatası oluştuğunda, Player kayıttan yürütmeyi kurtarmak için geçerli ve çeşitli parçaları yeniden dener
- Değişebilir AMP Çakışan video veya ses parçaları için AMP daha dayanıklı hale getirilir

## <a name="223-official-update-january-9-2019"></a>2.2.3 (resmi güncelleştirme Ocak 9 2019) ##

### <a name="features"></a>Özellikler ###

- Özellik HLS Safari HLS yürütmesi için ses izleme menüsü eklendi

### <a name="bug-fixes-223"></a>Hata düzeltmeleri 2.2.3 ###

- [Hata düzeltildi] AMP Larınızdaki Canlı yayın playyedekler sırasında, "canlı" düğmesi klavye kullanılarak seçilemez
- [Hata düzeltildi] AMP Hatalı MSE testi nedeniyle yanlış pozitif sonuçlar 0x0400003 hataları düzeltildi
- [Hata düzeltildi] AMP Canlı akış başlatıldığında videonun dondurmasına neden olan bir sorun düzeltildi

### <a name="changes-223"></a>2.2.3 değişiklikleri ###

- Değişebilir AMP Daha iyi tanılamayı sağlamak için günlüğe daha fazla bilgi eklendi
- Değişebilir AMP Aynı ekran çözünürlüğünden birden fazla bit hızı kullanılabilir olduğunda, tüm bitoranlar seçilebilir

## <a name="222-official-update"></a>2.2.2 (resmi güncelleştirme) ##

### <a name="bug-fixes-222"></a>Hata düzeltmeleri 2.2.2 ###

- [Hata düzeltildi] AMP Oyuncu geçici bir ağ kesintisi ile karşılaştığında, kayıttan yürütmeyi hemen sonlandırır
- [Hata düzeltildi] AMP Larınızdaki Hata iletişim kutusu klavye tarafından erişilebilir değil
- [Hata düzeltildi] AMP Desteklenmeyen hata yerine yalnızca ses varlığı çalınırken, sonsuz değer değiştirici görüntülendi

### <a name="changes-222"></a>2.2.2 değişiklikleri ###

- Değişebilir [AMP] tanıtım Kullanıcı arabirimi için yerelleştirilmiş dizeler eklendi

## <a name="221-official-update"></a>2.2.1 (resmi güncelleştirme) ##

### <a name="features-221"></a>Özellikler 2.2.1 ###

- Özellik [CMAF] HLS CMAF desteği eklendi

### <a name="bug-fixes"></a>Hata Düzeltmeleri ###

- [Hata düzeltildi] [AMP] yeniden yürütme sırasında temizlenmemiş Zamanlayıcı kaldırma hataları
- [Hata düzeltildi] AMP [Firefox] tamamlandı olayı, canlı program durdurulduğunda Firefox ve Chrome üzerinde tetiklenmez
- [Hata düzeltildi] AMP Oynatıcı seçeneklerinde denetimler false olarak ayarlandığında bile SetSource 'tan sonra görüntülenen denetimler

### <a name="changes"></a>Değişiklikler ###

- Değişebilir [Canlı açıklamalı alt yazı] 608 ile 708 arasındaki CEA açıklamalı alt yazılar için API adı değiştirildi. Daha fazla bilgi için bkz. [CEA708 açıklamalı altyazı ayarları](/javascript/api/azuremediaplayer/amp.player.cea708captionssettings)-->

## <a name="220-official-release"></a>2.2.0 (resmi sürüm) ##

### <a name="features-220"></a>Özellikler 2.2.0 ###

- Özellik [Azurehtml5JS] In [Liveaçıklamalı alt yazılar] CEA 708, Clear ve AES içeriği için Azurehtml5JS ve FlashSS Tech ' de BIR resim yazısı desteği.

### <a name="bug-fixes-220"></a>Hata düzeltmeleri 2.2.0 ###

- [Hata düzeltildi] Flash sürüm algılaması Chrome/Edge 'de çalışmıyor

### <a name="changes-220"></a>2.2.0 değişiklikleri ###

- Değişebilir AMP Lık Buluşsal profil adı hızlı Başlangıçdan LowLatency olarak değiştirildi
- Değişebilir In Yeni Adobe Flash güncelleştirmesiyle AES içeriğinin oynatılmasını etkinleştirmek üzere sürüm algılama için Flash Player 'da değişiklik.

## <a name="219-official-hotfix"></a>2.1.9 (resmi düzeltme) ##

### <a name="bug-fixes-219"></a>Hata düzeltmeleri 2.1.9 ###

- [Hata düzeltildi] Lı Canlı akışlar isteğe bağlı/canlı arşivlere geçiş yaparken oluşan özel durum

### <a name="changes-219"></a>2.1.9 değişiklikleri ###

- Değişebilir In AES Adobe, Flash 30 itibariyle kullanımı engellediği için, AES şifre çözme için sharedbytearrays kullanmayan Flash Tech Logic değiştirilmiştir. Lütfen kayıttan yürütme, v30 içindeki bir hata nedeniyle Adobe uygulamasının yalnızca Flash 'ın yeni bir sürümünü dağıttığı bir kez çalışmaya başladıktan sonra çalışır. Daha fazla ayrıntı için lütfen [bilinen sorunlara](azure-media-player-known-issues.md) bakın

## <a name="218-official-update"></a>2.1.8 (resmi güncelleştirme) ##

### <a name="bug-fixes-218"></a>Hata düzeltmeleri 2.1.8 ###

- [Hata düzeltildi] Ara sıra, arama sonrası ve yürütme öncesi gösterme
- [Hata düzeltildi] Kapalı seçeneği etkinken oynatıcı sessiz başlatılmaz
- [Hata düzeltildi] Denetimler false olarak ayarlandığında ses kaydırıcısı görüntülenir
- [Hata düzeltildi] Kullanıcı canlı bir kenara attığı zaman zaman zaman yineleniyor
- [Hata düzeltildi] 'U Oynatıcı bazen yükleme sırasında JavaScript özel durumu oluşturur
- [Hata düzeltildi] Larınızdaki Klavye denetimleri kullanılarak seçili olduğunda Oynat/Duraklat/hacim düğmesi odak ana hattını kaybeder
- [Hata düzeltildi] Player 'da sabit bellek sızıntısı atıldı
- [Hata düzeltildi] Player hataları kapatıldıktan sonra src () çağrısı kaynağı sıfırlayamıyor
- [Hata düzeltildi] Lı Yayın sona erdikten sonra Kullanıcı Live düğmesine tıkladığında AMP sabit yükleme durumunda
- [Hata düzeltildi] Mu Tarayıcı arka plana küçültüldüğünde oynatıcı askıda kalır ve kayıttan yürütme başarısız olur.

### <a name="changes-218"></a>2.1.8 değişiklikleri ###

- Değişebilir Şu anda desteklenmediğinden, AES içeriği Flash 30 ile kayıttan yürütüldüğünde, 0x0600001 hatası güncelleştirildi. Daha fazla ayrıntı için lütfen [bilinen sorunlara](azure-media-player-known-issues.md) bakın
- Değişebilir Bildirim 404 istediğinde veya boş bildirimler döndürdüğünde canlı senaryolar için ek yeniden denemeler eklendi.

## <a name="217-official-update"></a>2.1.7 (resmi güncelleştirme) ##

### <a name="features-217"></a>Özellikler 2.1.7 ###

- Özellik [AzureHtml5JS] Medya kaynağı arabelleğindeki eski verileri temizlemek için yapılandırma seçeneği eklendi

### <a name="bug-fixes-217"></a>Hata düzeltmeleri 2.1.7 ###

- [Hata düzeltildi] Larınızdaki [Ekran okuyucu] Başlık ayarlanmamışsa Player dahil edilen boş üst bilgi kaldırıldı
- [Hata düzeltildi] [UWA] Evrensel Windows uygulamasında kayıttan yürütme sırasında AMP özel durum oluşturur
- [Hata düzeltildi] [OSX] setActiveTextTrack () OSx üzerinde Safari 'de çalışmıyor
- [Hata düzeltildi] Lı Oynatıcıyı elden çıktıktan ve yeniden başlattıktan sonra canlı kenara tıklanması, özel durum verir
- [Hata düzeltildi] Görünümünün Belirli varlıklar için kesilen geçerli zaman
- [Hata düzeltildi] [DRM] birden çok CENC DRM 'yi destekleyen tarayıcılarda kayıttan yürütmeyi desteklemeye yönelik düzeltmeler eklenmiştir

### <a name="changes-217"></a>2.1.7 değişiklikleri ###

- Değişebilir Lerinizi Larınızdaki Tüm örneklere dil etiketi eklendi

## <a name="216-official-update"></a>2.1.6 (resmi güncelleştirme) ##

### <a name="bug-fixes-216"></a>Hata düzeltmeleri 2.1.6 ###

- [Hata düzeltildi] Belirli bir varlık için yanlış süre görüntüleme AMP
- [Hata düzeltildi] [FairPlay-HLS] Fairplay hataları Kullanıcı arabirimine yaymıyor
- [Hata düzeltildi] AMP 2.1.5 'de özel buluşsal özellikler yok sayılıyor.

### <a name="changes-216"></a>2.1.6 değişiklikleri ###

- Değişebilir [FairPlayDRM] PlayReady ve Widevine uygulamalarıyla eşlik sağlamak için hem CERT isteği hem de FairPlay için lisans isteği için zaman aşımı kaldırıldı
- Değişebilir Bulanık içeriğe yönelik çeşitli buluşsal iyileştirmeler

### <a name="features-216"></a>Özellikler 2.1.6 ###

- Özellik Destek MPD-Time-cmaf biçimi eklendi

## <a name="215-official-hotfix"></a>2.1.5 (resmi düzeltme) ##

### <a name="bug-fixes-215"></a>Hata düzeltmeleri 2.1.5 ###

- [Hata düzeltildi] Açıklamalı alt yazılar VTT stili, oynatıcı tarafından doğru şekilde işlenmez
- [Hata düzeltildi] Larınızdaki Live Button 'ın hiç Aria etiketi yok

## <a name="214-official-update"></a>2.1.4 (resmi güncelleştirme) ##

### <a name="bug-fixes-214"></a>Hata düzeltmeleri 2.1.4 ###

- [Hata düzeltildi] Larınızdaki Çı Kullanıcılar, denetim çubuğundaki tam ekran düğmesinin sağına eklenen özel düğmelere odaklanmak için sekme kullanamaz
- [Hata düzeltildi] IE11 [Birim çubuğu] Birim açılır penceresi, tam ekran modundayken tüm video ekranının ıE11 içinde yanıp sönmesini sağlar
- [Hata düzeltildi] [Dış görünüm | Flush] denetim çubuğu ve birim çubuğu açılır menüsü arasında görüntülenen boşluk
- [Hata düzeltildi] AMP Açıklamalı alt yazılar Mevcut bir oynatıcıda kaynak değiştirildiğinde eski gömülü parçalar temizlenmiyor
- [Hata düzeltildi] Larınızdaki Okuyucuyu Ekran okuyucusu birim denetimini yanlış okur
- [Hata düzeltildi] [FlashSS] Olay oynatma, Flash Tech 'tan bazen işletilmez
- [Hata düzeltildi] AMP Çı Oynatıcı odağa sahip olduğunda ve tam ekran modunda olduğunda Oynat/Duraklat iki tıklama gerektirir
- [Hata düzeltildi] AMP Görünümünün Belirli bir varlık için ilerleme çubuğunda hatalı süre gösteriliyor
- [Hata düzeltildi] Reklam [Ad Butler] Büyük ayrıştırıcı, ilerleme olayı olmayan çok büyük dosyayı işlemez
- [Hata düzeltildi] SDN [AMP 2.1.1] Hive SDN eklentisi desteği için sorun düzeltildi
- [Hata düzeltildi] Larınızdaki Ekran okuyucusu, Kullanıcı toplu düğmesine odaklanıldığında "gece yarısından sonra sessiz düğme" i okur

### <a name="changes-214"></a>2.1.4 değişiklikleri ###

- Değişebilir Larınızdaki [Yardımcı teknoloji] Düğmeler artık yardımcı teknolojiyle deneyimi geliştirmek için ARIA-Live özelliğine sahiptir
- Değişebilir Larınızdaki [Birim düğmesi | Ekran okuyucusu] sekme işlevinin ve kaydırıcı davranışının değiştirilerek, toplu düğmenin erişilebilirliği geliştirildi. Bu değişiklikler, klavye kullanıcılarının Player 'ın sesini değiştirmesini kolaylaştırır
- Değişebilir 3 ile 5 saniye arasında artan eylemsizlik bağlam menüsü zaman aşımı
- Değişebilir Larınızdaki Parlaklığı Açıklamalı alt yazı ayarlarında açılan menülerde iyileştirilmiş parlaklık karşıtlığı oranı

## <a name="213-official-update"></a>2.1.3 (resmi güncelleştirme) ##

### <a name="bug-fixes-213"></a>Hata düzeltmeleri 2.1.3 ###

- [Hata düzeltildi] [Eklentiler | Başlık kaplama] başlık kaplama eklentisi, AMP v2 ile JS özel durumları oluşturur. X +
- [Hata düzeltildi] Kaynak kümesi olayı, günlüğe kaydetme kapalıyken bile JavaScript konsoluna gönderilir
- [Hata düzeltildi] Görünümünün Player zaman ipuçları, herhangi bir uç süre çubuğunun üzerine gelindiğinde Player 'ın bağlamı dışında işlenir
- [Hata düzeltildi] Larınızdaki [Ekran okuyucu] Görüntüleyici Player 'a odaklanıldığında, okuyucu "bölge yer Işareti" veya "video oynatıcı bölgesi yer Işareti" okur
- [Hata düzeltildi] AMP Oynatıcı ana hattı CSS aracılığıyla devre dışı bırakılamıyor
- [Hata düzeltildi] Larınızdaki Kullanıcı tam ekran modundayken oynatıcının tamamına odaklanmak için sekme kullanılamaz
- [Hata düzeltildi] Görünümünün Lı Japonca, yerelleştirilmiş canlı metne yanıt vermiyor
- [Hata düzeltildi] Görünümünün Akış > 60 dk-[hata düzeltildi] [iPhone | için süre ve geçerli zaman al Canlı] oynatıcı denetim çubuğunda geçerli zaman/süre metnini gösterir
- [Hata düzeltildi] AMP Oynatıcı buluşsal yöntemler API 'Leri çağırmak JavaScript özel durumları verir
- [Hata düzeltildi] [Yerel HTML5 | iOS] Videotag özelliği "playsinline" Player 'a yaymıyor
- [Hata düzeltildi] [iOS | iframe] Oynatıcı bir iframe 'e yüklenirse iPhone üzerinde tam ekran moduna giremezsiniz
- [Hata düzeltildi] AMP Lık AMP, oynatıcı seçeneklerinden bağımsız olarak her zaman karma profille çalışır
- [Hata düzeltildi] [AMP | Win 8.1] bir WebView ile Win 8.1 uygulamasında barındırıldığı zaman atar

### <a name="changes-213"></a>2.1.3 değişiklikleri ###

- Değişebilir AMP Fragmentdownloadpoınt olayında CDN uç noktası bilgileri eklendi
- Değişebilir AMP Lı İyileştirilmiş ve iyileştirilmiş canlı akış gecikmesi

## <a name="212-official-hotfix"></a>2.1.2 'yi (resmi düzeltme) ##

### <a name="bug-fixes-212"></a>Hata düzeltmeleri 2.1.2 'yi ####

- [Hata düzeltildi] Larınızdaki [Windows ekran okuyucusu] Kullanıcının ilerleme çubuğu bağlamı olduğunda ve geçerli zaman 0:00 olduğunda, ekran okuyucusu "Ilerleme gece yarısı" okur
- [Hata düzeltildi] [Skin] logo boyutu, JavaScript kodunda sabit kodlanmış
- Larınızdaki Tuşlarının Oyuncu tıklandığında kısayol tuşları etkin değil.

### <a name="changes-212"></a>2.1.2 'yi değişiklikleri ####

- Değişebilir Açmak Player bildirimi yükleyemediğinde günlük bildirimi URL 'SI

### <a name="features-212"></a>Özellikler 2.1.2 'yi ###

- Değişebilir Mının Iyileştirme Geliştirilmiş oyuncu yükleme ve başlatma süreleri

## <a name="211-official-update"></a>2.1.1 (resmi güncelleştirme) ##

### <a name="bug-fixes-211"></a>Hata düzeltmeleri 2.1.1 ####

- [Hata düzeltildi] Işlemine AutoPlay özelliğinin false olarak ayarlanması, iOS için Safari 'de sonsuz değer değiştirici verir
- [Hata düzeltildi] İçerik süresinden büyük bir zamana arama sonsuz değer değiştirici veriyor
- [Hata düzeltildi] Kısayol tuşları, Player 'ın çalışma bağlamını almak için birden çok klavye sekmesi gerektirir
- [Hata düzeltildi] Belirli varlıklarda oynatıcıyı yeniden boyutlandırdıktan sonra video birkaç saniye donuyor
- [Hata düzeltildi] Kullanıcı birden çok çabuk arama yaparken sonsuz değer değiştirici (Seek tamamlandıktan sonra)
- [Hata düzeltildi] Denetim çubuğu, etkin olmama sırasında gizli değil
- [Hata düzeltildi] AMP barındıran bir WebApp açmak, Web sayfasının iki kez yüklenmesine neden olabilir
- [Hata düzeltildi] İçerik belirli varlıkları Flash Tech aracılığıyla yürütürken sonsuz
- [Hata düzeltildi] Diğer Seçenekler menüsü 3. taraf eklentilerle görüntülenmiyor
- [Hata düzeltildi] [Dış görünüm | Tüp] [Canlı] oynatıcı bir programın canlı kenarından çalışırken Iki canlı simge görüntülenir
- [Hata düzeltildi] Görünümünün Logo devre dışı bırakılamıyor
- [Hata düzeltildi] [Gg + Içerik] Sürekli değer değiştirici, Dolby Digital ses izi içeren varlıklar için görüntülenir
- [Hata düzeltildi] Canlı akış sırasında ses dili izlerini değiştirirken en son AMP donuyor
- [Hata düzeltme] değer değiştirici için arka plan geri görünümü düzeltildi
- [Hata düzeltildi] AES Flash Token statik örnekler hata düzeltmeleri içinde sonsuz değer değiştirici

### <a name="changes-211"></a>2.1.1 değişiklikleri ####

- Değişebilir Widevine https gereksinimi için hata kodu eklendi: Chrome V58 itibariyle wıdevine içeriği protokol aracılığıyla yüklenmelidir/çalınmalıdır, `https://` Aksi halde kayıttan yürütme başarısız olur.
- Değişebilir İçerik yüklenirken yardımcı teknolojinin "video yükleme" işlemi yapabilmesi için, yükleme değiştiricisi için Aria etiketi eklendi  

## <a name="210-official-release"></a>2.1.0 (resmi sürüm) ##

### <a name="features-210"></a>Özellikler 2.1.0 ###

- Özellik [AzureHtml5JS] Mantem sonrası için VOD ad desteği
- Özellik Beta [AzureHtml5JS] Ön baskı sonrası için canlı ad desteği
- Özellik Yeni dış görünüm seçeneği eklendi-AMP-Temizleme
- Özellik Ekran okuyucular/yardımcı teknolojisiyle daha iyi tümleştirme için geliştirilmiş Aria etiketleri eklendi
- Özellik Görünümünün Dış görünüm artık yüksek karşıtlık modundaki tüm simgeleri ve düğmeleri açıkça gösteriyor

### <a name="bug-fixes-210"></a>Hata düzeltmeleri 2.1.0 ###

- [Hata düzeltildi] Erişilebilirlik ve UI düzeltmelerinin sayısı
- [Hata düzeltildi] IE9 içinde AMP doğru yüklenmedi

### <a name="changes-210"></a>2.1.0 değişiklikleri ###

- Değişebilir Reklam çalışmalarını karşılamak için Player 'da yeniden yapılandırılmış DOM öğeleri
- Değişebilir Dış görünüm geliştirmesi için CSS 'den SCSS 'e geçti
- Değişebilir Lerinizi VOD reklamları için örnek eklendi
- Değişebilir Lerinizi Kayıttan yürütme hızı için örnek eklendi
- Değişebilir Lerinizi Temizleme kaplaması için örnek eklendi

## <a name="200-beta-release"></a>2.0.0 (Beta sürümü) ##

- [Değiştir] VJS5 olarak güncelleştirildi
- Özellik Player yanıt hızını akıcı olarak yeni akışkan API 'SI eklendi
- Özellik Kayıttan yürütme hızı
- Değişebilir Dış görünüm için CSS 'den SCSS 'e geçti

## <a name="183-official-hotfix-update"></a>1.8.3 birden fazla (resmi düzeltme güncelleştirmesi) ##

### <a name="bug-fixes-183"></a>Hata düzeltmeleri 1.8.3 birden fazla ###

- [Hata düzeltildi] [AzureHtml5JS] Negatif DTS içeren bazı varlıklar Chrome 'da kayıttan yürütme yapmayacaktır

## <a name="182-official-hotfix-update"></a>1.8.2 (resmi düzeltme güncelleştirmesi) ##

### <a name="bug-fixes-182"></a>Hata düzeltmeleri 1.8.2 ###

- [Hata düzeltildi] [AzureHtml5JS] Daha yüksek ses bitfiyatları, AzureHtml5JS aracılığıyla geri yürütülmeyecek

## <a name="181-official-update"></a>1.8.1 (resmi güncelleştirme) ##

### <a name="bug-fixes-181"></a>Hata düzeltmeleri 1.8.1 ###

- [Hata düzeltildi] Işlemine Yerel yürütücüyü gösteren açıklamalı alt yazılar/alt yazılar
- [Hata düzeltildi] AMP Kimlik doğrulama belirteçleri yürütülemeyen CDN ile desteklenen akış URL 'Leri
- [Hata düzeltildi] Fairplay FairPlay hata kodu eksik teknik KIMLIĞI (hata kodu ' nın bitleri [31-28]) daha fazla ayrıntı için bkz. hata kodları
- [Hata düzeltildi] Uygulamasını PlayReady Safari 'de PlayReady içeriği sonsuz değer değiştirici

### <a name="changes-181"></a>1.8.1 değişiklikleri ###

- Değişebilir HTML5 Yerel HTML5 teknik ayrıntılı günlüklerini VideoTag 'ten olay içerecek şekilde değiştirme

## <a name="180-official-update"></a>1.8.0 (resmi güncelleştirme) ##

### <a name="features-180"></a>Özellikler 1.8.0 ###

- Özelliklerinde DRM FairPlay desteği eklendi (daha fazla bilgi için [korunan içeriğe](azure-media-player-protected-content.md) bakın)

### <a name="bug-fixes-180"></a>Hata düzeltmeleri 1.8.0 ###

- [Hata düzeltildi] AMP Ağ kısıtlandığınızda Kullanıcı Seek bir bekleme olayı tetiklemez
- [Hata düzeltildi] [FlashSS] Flash Tech özel durum durumunda kalite seçme
- [Hata düzeltildi] AMP Kaliteyi dinamik olarak seçme bağlam menüsünde gösterir
- [Hata düzeltildi] Görünümünün Bağlam menülerinin son menü öğesini seçmek zordur

### <a name="changes-180"></a>1.8.0 değişiklikleri ###

- Değişebilir Oynatıcı, geçerli Chrome EME gereksinimlerine güncelleştirildi
- Değişebilir Varsayılan techOrder yeni Tech-html5FairPlayHLS uyacak şekilde değiştirilmiştir (daha fazla bilgi için [korunan içeriğe](azure-media-player-protected-content.md) bakın)
- Değişebilir [AzureHtml5JS] Safari 'de MPEG-Dash yürütme etkinleştirildi
- Değişebilir Lerinizi Çoklu DRM örnekleri FairPlay uyacak şekilde değiştirildi

## <a name="174-official-hotfix-update"></a>1.7.4 (resmi düzeltme güncelleştirmesi) ##

### <a name="bug-fixes-174"></a>Hata düzeltmeleri 1.7.4 ###

- [Hata düzeltildi] Mu Kullanıcının oynatıcı bağlamı olduğunda arama tutamacının etrafında mavi ana hat görünür
- [Hata düzeltildi] IE9 IE9 içinde oynatıcı yüklendiğinde JavaScript özel durumu oluşturuldu

## <a name="173-official-hotfix-update"></a>1.7.3 (resmi düzeltme güncelleştirmesi) ##

### <a name="bug-fixes-173"></a>Hata düzeltmeleri 1.7.3 ###

- [Hata düzeltildi] [AzureHtml5JS] Kısıtlı ağlarda Player zaman aşımına uğruyor

### <a name="changes-173"></a>1.7.3 değişiklikleri ###

- Değişebilir AES içeriğinin şifresini çözmek için kenarda Web şifre sağlama
- Değişebilir Önbelleğe alınmış öbekleri hesaba göre AMP buluşsal yöntemlerini en iyi duruma getirme
- Değişebilir [AzureHtml5JS] Bant genişliği tahmini gecikmesini azaltarak buluşsal yöntemi iyileştirin

## <a name="172-official-hotfix-update"></a>1.7.2 (resmi düzeltme güncelleştirmesi) ##

### <a name="features-172"></a>Özellikler 1.7.2 ###
<!---API needs onboarding. Removed link to API until remedied.--->
- Özellik [AzureHtml5JS | Firefox] Firefox için EME ile Widevine oynatmayı etkinleştirme 47 +
- Özellik Oynatıcı elden atılıyor için olay Ekle
<!-- ([disposing](index.html#static-amp.eventname.disposing)) -->

### <a name="bug-fixes-172"></a>Hata düzeltmeleri 1.7.2 ###

- [Hata düzeltildi] Kodlanmış Akamai CDN URL sorgu parametreleri düzgün şekilde çözülemedi
- [Hata düzeltildi] ManifestPlayableWindowLength () üzerinde özel durum oluşturuldu
- [Hata düzeltildi] Video yeniden izlenecek şekilde, Görüntüleyici her zaman videoda oynat ' a tıklamaz
- [Hata düzeltildi] Hızlı pencere boyutu değişikliklerine uygun değil, yanıt veren boyutlandırma
- [Hata düzeltildi] [Edge | IE] genişlik = x için sayfa yükleme, yükseklik = otomatik
- [Hata düzeltildi] [Android | Chrome] Chrome içerik şifrelenmediği zaman DRM içeriğini kayıttan yürütme izinleri istiyor
- [Hata düzeltildi] Larınızdaki Kenarını Klavye denetimleri bağlam menüsü öğelerini doğru seçme
- [Hata düzeltildi] Larınızdaki Yüksek karşıtlık modunda Görüntülenme kenarlığı eksik
- [Hata düzeltildi] [FlashSS] Oynatıcı Dispose özel duruma neden olduktan sonra fare yukarı olay dinleyicisi kaldırılmadı
- [Hata düzeltildi] [FlashSS] Kodlanmış alanlarla bildirim URL 'SI ayrıştırma sorunu
- [Hata düzeltildi] Işlemine Tech. featuresVolumeControl değerlendirilirken hata yazın

### <a name="changes-172"></a>1.7.2 değişiklikleri ###

- Değişebilir DRM Kaynak ayarlandıktan sonra DRM denetimleri, içeriğin ne zaman şifrelendiğini kontrol altına taşındı
- Değişebilir AES Anahtar teslim isteğinden tanımsız türde/şifresiz gövde kaldırıldı
- Değişebilir Larınızdaki Windows ekran okuyucusu artık özellikler yerine Player 'da olduğunda "Media Player" öğesini okur

## <a name="171-official-hotfix-update"></a>1.7.1 (resmi düzeltme güncelleştirmesi) ##

### <a name="features-171"></a>Özellikler 1.7.1 ###

- Özellik Karma buluşsal profile için seçenek eklendi (Bu profil varsayılan olarak ayarlanır)

### <a name="bug-fixes-171"></a>Hata düzeltmeleri 1.7.1 ###

- [Hata düzeltildi] Yanıt veren tasarım HTML5 standardına göre çalışmaz (Genişlik = %100, yükseklik = otomatik)
- [Hata düzeltildi] Genişlik ve yükseklik için yüzde değerleri v 1.7.0 içinde beklendiği gibi çalışmıyor

## <a name="170-official-update"></a>1.7.0 (resmi güncelleştirme) ##

### <a name="features-170"></a>Özellikler 1.7.0 ###
<!---API needs onboarding. Removed link until remedied.--->
- Özellik [AzureHtml5JS] [FlashSS] Geçerli zamanın Encoder medya süresini saniye olarak almak için currentMediaTime () eklendi
- Özellik [FlashSS] Video Bufferdata () ve audioBufferData () ile telemetri API 'Lerini indirme uygulandı<!-- (see [BufferData](index.html#amp.bufferdata) for more details) -->
- Özellik [FlashSS] ' Downloadbitratechanged ' olayı eklendi
- Özellik Player 'ın eski sürümlerine kıyasla iyileştirilmiş yükleme süresi
- Özellik Hatalar JavaScript konsoluna kaydedilir

### <a name="bug-fixes-170"></a>Hata düzeltmeleri 1.7.0 ###

- [Hata düzeltildi] Player 'da görüntülememe sorgu dizesi parametreleriyle kodlanmış poster URL 'SI
- [Hata düzeltildi] Teknik yüklü ve API amp olmadığında oluşan özel durum. Player. poster () çağrıldı
- [Hata düzeltildi] İşlevler atıldıktan sonra Player 'a erişmeye çalıştığında özel durum oluştu
- [Hata düzeltildi] Larınızdaki Devam eden çubukta ana hat eksik seviyelendirme arama kafası
- [Hata düzeltildi] Larınızdaki Bağlam menülerinin yüksek karşıtlık modunda gölge vardır
- [Hata düzeltildi] [iOS] yerel oynatıcı WebVTT açıklamalı altyazı kayıttan yürütme çalışmıyor
- [Hata düzeltildi] [AzureHtml5JS] HTTP akışı HTTPS sitesinde oynatılırken, karma içerik sonucu olarak sonsuz değer değiştirici veren hata 0x0100002 gösterilmelidir
- [Hata düzeltildi] [AzureHtml5JS] Algılanan sonsuz arabelleğe alma durumu görüntülenirken döngü durumu denetimi hatasına neden olan bitiş segmenti eksik
- [Hata düzeltildi] [AzureHtml5JS] UseManifestForLabel = false olduğunda ve üç harfli dil kodu kullanıldığında menüdeki ses izi adı yanlış
- [Hata düzeltildi] [AzureHtml5JS | Chrome] kayan nokta noktasında kesinlik eksikliği, Chrome 'da JavaScript ile zaman içinde olan kayan nokta nedeniyle oluşan sonsuz arabellek durumu algılanan
- [Hata düzeltildi] [FlashSS] Flash Player oluşturulduğunda kısa bir zaman aralıklı olmayan hata görüntüleniyor
- [Hata düzeltildi] [FlashSS] Video ve ses akışları, "parça URL 'si (...) ile başarısız olan yuvarlama noktasında kesinlik eksikliği nedeniyle farklı zaman dilimlerini kullanırken kayıttan yürütme başarısız oluyor FLVTags oluşturulamadı "
- [Hata düzeltildi] [FlashSS] Kodlanmış alanlarla bildirim URL 'lerini ayrıştırma sorunları
- [Hata düzeltildi] [FlashSS] Teknik siparişte bir sonraki Tech 'a geri dönmek yerine, Kayıttan yürütmede hata oluşmasına neden olan Flash Player sürümünün >= 11,4 olup olmadığını kontrol etmek için denetim eksik
- [Hata düzeltildi] [FlashSS] AES İçinde alt çizgi ile AES belirteçleri kabul eden sorunlar
- [Hata düzeltildi] [SilverlightSS | OSX] "//" protokol (HTTP veya HTTPS) yerine bir bildirime önek olarak bir yerel dosya olarak tanınır ve sonsuz değer değiştirici

### <a name="changes-170"></a>1.7.0 değişiklikleri ###

- Değişebilir [FlashSS] Birleştirilmiş SWF betikleri ("MSAdaptiveStreamingPlugin-osmf 2.0. swf" ve "StrobeMediaPlayback. 2.0. swf") "StrobeMediaPlayback. 2.0. swf" adlı tek bir SWF içine birleştirildi
- Değişebilir [FlashSS] Daha kesin hata kodları (örn.) almak için hata kodu yayılması güncelleştirildi. 404S, artık genel hata 0x30200000 yerine 0x30200194 ile sonuçlanır.

## <a name="163-official-hotfix-update"></a>1.6.3 (resmi düzeltme güncelleştirmesi) ##

### <a name="bug-fixes-163"></a>Hata düzeltmeleri 1.6.3 ###

- [Hata düzeltildi] Oyuncu elden çıktıktan sonra kısayol tuşları olay işleyicisi yürütüldüğünde JavaScript çalışma zamanı özel durumu
- [Hata düzeltildi] Android [AzureHtml5JS] Hücresel ağ kullanarak mobil cihazda kayıttan yürütme yok
- [Hata düzeltildi] Kullanıcı arabirimini boşaltmak için Web çalışanı olarak çalışacak şekilde güncelleştirildi

## <a name="162-official-hotfix-update"></a>1.6.2 (resmi düzeltme güncelleştirmesi) ##

### <a name="features-162"></a>Özellikler 1.6.2 ###

- Özellik Yerelleştirme için ek diller eklendi (daha fazla ayrıntı için belgelere bakın)

### <a name="bug-fixes-162"></a>Hata düzeltmeleri 1.6.2 ###

- [Hata düzeltildi] [IE9-10] Player 'da simge durumuna küçültülmüş, ıE9/ıE10 hatası nedeniyle Window. bulanıklığı () üzerinde en aza indirir.
- [Hata düzeltildi] [FlashSS] Alt çizgi ile AES belirteçleri kabul edilmez

## <a name="161-official-hotfix-update"></a>1.6.1 (resmi düzeltme güncelleştirmesi) ##

### <a name="bug-fixes-161"></a>Hata düzeltmeleri 1.6.1 ###

- [Hata düzeltildi] [FlashSS | Edge, IE] [SilverlightSS | IE], diğer Kullanıcı Arabirimi öğelerine ya da IE/Edge içindeki diğer UI öğelerine odaklanılamaz
- [Hata düzeltildi] Tanımsız bir şekilde başarısız olan AES yürütmesi
- [Hata düzeltildi] Android [AzureHtml5JS | Chrome] sistem durumu denetim döngüsünde sürekli olarak içerik çalınmıyor
- [Hata düzeltildi] [IE9] Console. log () IE 9 tarafından desteklenmiyor özel duruma neden oluyor

## <a name="160-official-update"></a>1.6.0 (resmi güncelleştirme) ##

### <a name="features-160"></a>Özellikler 1.6.0 ###

- [Özellik] %33 Boyut azaltma azuremediaplayer.min.js
- Özellik [AzureHtml5JS | Edge] [test edilmemiş] Edge 'de gg + ses akışları desteği (ilk seçimden sonra codec geçişi yok). Uygulamanın Şu anda doğru ses akışını seçmeli.
- Özellik Sık kullanılan tuş denetimleri (daha fazla ayrıntı için belgelere bakın)
- Özellik Zaman doğru arama için ilerleme süresi ipucu üzerine gelme
- Özellik Eklenti içinde setupDone yöntemi varsa, eklentilerin zaman uyumsuz olarak algılanmasına izin ver

### <a name="bug-fixes-160"></a>Hata düzeltmeleri 1.6.0 ###

- [Hata düzeltildi] GetMemoryLog üzerinde reçeteye göre bellek günlüğü (true)
- [Hata düzeltildi] Fare hareket halinde bit hızı seçim kutusu sıfırlamaları sorun nedeniyle fare denetimi üzerinden daha düşük bitoranlar seçiliyor
- [Hata düzeltildi] DRM denetimi gerçekleştirilirken uygulama Kilitlenmelerinde Mac Office
- [Hata düzeltildi] CSS sınıflarının kolayca yanlışlıkla üzerine yazılır
- [Hata düzeltildi] Mu Kullanıcı Aracısı dize tarayıcısından güncelleştirme kimliği, kenar
- [Hata düzeltildi] [AzureHtml5JS] Açıklamalı alt yazılar düğmesi Edge (win10) veya Chrome (Mac) içinde araç çubuğuna gösterilmiyor
- [Hata düzeltildi] Android [AzureHtml5JS | Chrome] kısa videolarda endOfStream () çağrısında ınvalidstateerror özel durumu
- [Hata düzeltildi] 'U Tarayıcı özellikleri denetlenirken Firefox 'un neden olduğu DRM uyarısının kaldırılması
- [Hata düzeltildi] HTML5 İlerleyen bir MP4 içeriğiyle görünmeyen alt başlık/açıklamalı alt yazı
- [Hata düzeltildi] [FlashSS] Eşleşen zaman damgalarına sahip iletiler ters sırada günlüğe kaydedilir
- [Hata düzeltildi] Larınızdaki [Chrome | Firefox] sekmesinde ve denetimleri otomatik olarak Seç ilk menü öğesi
- [Hata düzeltildi] Larınızdaki Birim düğmesini denetlemek için sekme

### <a name="changes-160"></a>1.6.0 değişiklikleri ###

- Değişebilir Kalite düzeyi seçiminde AES şifre çözme süresi kullan
- Değişebilir Çoklu ses akışları için HLS v3 'den önce HLS v4 'YI kullanacak şekilde URL yeniden yazıcısını Güncelleştir
- Değişebilir NativeControlsForTouch 'ı varsayılan olarak false olarak ayarlayın (doğru çalışması için yanlış olmalıdır)

## <a name="150-official-update"></a>1.5.0 (resmi güncelleştirme) ##

### <a name="features-150"></a>Özellikler 1.5.0 ###

- Özellik Genel web güvenliğine yönelik geliştirmeler (ekleme, XSS, vb.)
- Özellik Sourceset olayı ve seçenekleri için SDN eklentisi tümleştirme kancaları. Sdn
- Özellik Oynatma sırasında 5 xx ve 4XX hatalarının sağlamlık işlemesi

### <a name="bug-fixes-150"></a>Hata düzeltmeleri 1.5.0 ###

- [Hata düzeltildi] CSS 'yi, Unicode yerine düğmeler için HTML varlık yazı tipi kodları kullanacak şekilde Güncelleştir
- [Hata düzeltildi] [AzureHtml5JS] Çoklu DRM içeriği her zaman Protectionınfo 'dan ilk öğenin belirtecini seçerek ikinci DRM 'nin başarısız olmasına neden olur
- [Hata düzeltildi] [AzureHtml5JS] Kayıp kesimleri olan bir alanda arama yaparken, arama işlemi hiçbir zaman tamamlanmaz.
- [Hata düzeltildi] [AzureHtml5JS | Edge] PlayReady kayıttan yürütme için Edge güncelleştirmesinde ön eki etkinleştir
- [Hata düzeltildi] [AzureHtml5JS | Firefox] Firefox v42 + (MSE ile) ' i korumalı içerik için Silverlight 'a geri dönüşü sağlayan EME denetimini güncelleştirin
- [Hata düzeltildi] [FlashSS] Güncelleştirme hatası. sayıdan ayrıntılı dizeye ileti

### <a name="changes-150"></a>1.5.0 değişiklikleri ###

- Değişebilir Posterler Şu anda yalnızca mutlak URL 'Ler olarak çalışır.

## <a name="140-official-update"></a>1.4.0 (resmi güncelleştirme) ##

### <a name="features-140"></a>Özellikler 1.4.0 ###

- Özellik [AzureHtml5JS | Chrome] basit Widevine DRM desteği
- Özellik [AzureHtml5JS] Kayıttan yürütme sırasında 404/412 hatalarının sağlamlık işlemesi

### <a name="bug-fixes-140"></a>Hata düzeltmeleri 1.4.0 ###

- [Hata düzeltildi] [FlashSS] Parametre doğrulama geliştirmesi

## <a name="130-official-update"></a>1.3.0 (resmi güncelleştirme) ##

### <a name="features-130"></a>Özellikler 1.3.0 ###

- Özellik [AzureHtml5JS] [FlashSS] Aynı codec çoklu ses içeriğinin ses geçişi

### <a name="bug-fixes-130"></a>Hata düzeltmeleri 1.3.0 ###

- [Hata düzeltildi] [AzureHtml5JS | Chrome] aralıklı sonsuz değer değiştirici
- [Hata düzeltildi] [AzureHtml5JS | IE] [Windows Phone] Windows Phone kayıttan yürütme sorunlarına neden oluyor
- [Hata düzeltildi] [FlashSS] Ek örnekler için otomatik yürütme yanlış olarak ayarlandı
- [Hata düzeltildi] UI menü boyutlandırma sorunları

## <a name="120-official-update"></a>1.2.0 (resmi güncelleştirme) ##

### <a name="features-120"></a>Özellikler 1.2.0 ###

- Özellik [AzureHtml5JS | Firefox] MSE etkinleştirildiğinde desteklenir
- Özellik Artık uygulama için geri dönüş teknik ikilileri (SWF, xap) için yol sağlanması gerekmez. Yol Azure Media Player betiğine göredir.

### <a name="bug-fixes-120"></a>Hata düzeltmeleri 1.2.0 ###

- [Hata düzeltildi] [AzureHtml5JS | Chrome] arka planda oynatıcı çalışırken Player Drifts canlı kenar arkasında
- [Hata düzeltildi] [AzureHtml5JS | Edge] tam ekran çalışmıyor
- [Hata düzeltildi] [AzureHtml5JS] Seçenekler 'de ayarlandığında günlüğe kaydetme düzgün şekilde etkinleştirilmedi
- [Hata düzeltildi] In Bekleme olayı sırasında hem "arabelleğe alma" hem de arabelleğe alma simgesi göster
- [Hata düzeltildi] İlk bant genişliği isteği başarısız olursa kayıttan yürütmenin devam etmesine izin ver
- [Hata düzeltildi] , Tanımsız seçeneklerle başlatıldığında Player yükleme yapamıyor
- [Hata düzeltildi] Oynatıcıyı zaten atıldıktan sonra atmayı denerken bir vData özel durumu oluşur
- [Hata düzeltildi] Kalite çubuğu simgeleri yanlış eşlendi

## <a name="111-official-hotfix-update"></a>1.1.1 (resmi düzeltme güncelleştirmesi) ##

### <a name="bug-fixes-111"></a>Hata düzeltmeleri 1.1.1 ###

- [Hata düzeltildi] Eski IE tam ekran sorunu
- [Hata düzeltildi] Eklentilerin artık üzerine yazılmaz

## <a name="110-official-update"></a>1.1.0 (resmi güncelleştirme) ##

### <a name="features-110"></a>Özellikler 1.1.0 ###

- Özellik UI yerelleştirme dizelerini güncelleştirme

### <a name="bug-fixes-110"></a>Hata düzeltmeleri 1.1.0 ###

- [Hata düzeltildi] Büyük oynatma düğmesi yeterli karşıtlığa sahip değil
- [Hata düzeltildi] Görsel sekme odak göstergesi
- [Hata düzeltildi] Doğru çözüm bilgilerini kullanarak şimdi bit hızı menüsünü seçin
- [Hata düzeltildi] Daha fazla seçenek menüsü artık dinamik olarak boyutlandırılabilir
- [Hata düzeltildi] Çeşitli UI sorunları

## <a name="100-official-release"></a>1.0.0 (resmi sürüm) ##

### <a name="features-100"></a>Özellikler 1.0.0 ###

- Özellik Sekme denetimi, odak denetimi, ekran okuyucu, yüksek karşıtlık Kullanıcı arabirimi için temel erişilebilirlik testi
- Özellik Güncelleştirilmiş Kullanıcı arabirimi
- Özellik Geliştirme günlüğü
- Özellik Resim yazılarını/alt yazı izlerini dinamik olarak ayarlamaya yönelik API
- Özellik Temel yerelleştirme özellikleri
- Özellik Techs genelinde hata kodu birleştirme
- Özellik Eklentiler (Flash veya Silverlight gibi) yüklü olmadığında için yeni hata kodu
- Özellik [AzureHtml5JS] Uygulanan temel Tanılama olayları

### <a name="bug-fixes-100"></a>Hata düzeltmeleri 1.0.0 ###
<!---What is that actually supposed to say?--->
- [Hata düzeltildi] [AzureHtml5JS] Zaman damgasında küçük bir engel olduğunda, MPD güncelleştirmelerinde canlı kayıttan yürütme donduruyor
- [Hata düzeltildi] [AzureHtml5JS] Birkaç canlı kayıttan yürütme sorunu azaltıldığında
- [Hata düzeltildi] [AzureHtml5JS] Pencere boyutu buluşsal yöntemleri açık olduğunda ve daha yüksek bir çözünürlük ekranına gittiğinizde arabelleği temizle
- [Hata düzeltildi] [AzureHtml5JS] Chrome artık tamamlandı olayını doğru şekilde gösteriyor. AzureHtml5JS kullanılırken Chrome 'un bilinen önceki bir sorunuyla bağlantılı olarak *, son olay doğru şekilde gönderilmeyecektir. Temel tarayıcıda bir sorun var.*
- [Hata düzeltildi] [AzureHtml5JS] *AzureHtml5JS Tech Ile OSX Yosemite Ile kayıttan yürütme sorununu gidermek için bu teknik Için Safari devre dışı bırakıldı. MSE uygulama sorunları vardır. Geçici risk azaltma: bu kullanıcı aracıları için silverlightSS olarak flashSS 'yi zorla*
- [Hata düzeltildi] [FlashSS] loadstart hata oluştuğunda başlatıldı

## <a name="020-beta"></a>0.2.0 (Beta) ##

### <a name="features-020"></a>Özellikler 0.2.0 ###

- Özellik İsteğe bağlı ve canlı özellikli için PlayReady ve AES testi tamamlandı-bkz. uyumluluk matrisi
- Özellik Süreksizlikleri işleme
- Özellik 2 ^ 53 ' ten büyük zaman damgalarına yönelik destek
- Özellik URL sorgu parametresi bildirim isteğine devam ederse
- Özellik Test edilmemiş `QuickStart`Ve `HighQuality` buluşsal yöntemler profilleri desteği
- Özellik Test edilmemiş AzureHtml5JS ve FlashSS 'de bitücretler, genişlik ve yükseklik için video akışı bilgilerini gösterme
- Özellik Test edilmemiş AzureHtml5JS ve FlashSS bit hızını seçin (bkz. API belgeleri)

### <a name="bug-fixes-020"></a>Hata düzeltmeleri 0.2.0 ###

- [Hata düzeltildi] artık WP 8.1 üzerinde görüntülenebilen büyük yürütme düğmesi
- [Hata düzeltme] Çoklu canlı kayıttan yürütme sorunları düzeltildi
- [Hata düzeltildi] düğmeyi aç düğmesi artık Kullanıcı arabiriminde çalışmaktadır
- [Hata düzeltildi] otomatik yürütme modu için Kullanıcı Arabirimi yükleme deneyimi güncelleştirildi
- [Hata düzeltildi] AMD yükleyici sorunu ve Yöntem çakışmalarını tanımla
- [Hata düzeltildi] WP 8,1 Cordova uygulama yükleme sorunu
- [Hata düzeltildi] Korumalı içerik platform/Tech tarafından desteklenen ProtectionType 'ı, kayıttan yürütme için uygun bir teknoloji seçmek üzere sorgular.  _Şu anda ' PlayReady Content for Chrome (Masaüstü)/Safari 8 (OSX Yosemite üzerinde) ' ın bilinen, ' Silverlight Player 'a geri dönüş_
- [Hata düzeltildi] varsayılan olarak bu makinede yüklü olmayan Medya Altyapısı nedeniyle WinServer 2012 R2 üzerinde yakalanmayan özel durum.  Uygulanmayan ve bu nedenle bir hata oluşturan HTML video etiketi API 'Lerini kullanmayı deneyin. Güncel risk azaltma bu hatayı yakalayıp hatayı oluşturmak yerine false döndürür.
- [Hata düzeltilme] kayıttan yürütme sırasında hata oluşmasını engellemek için Seek veya http hatasından sonra her zaman init segmentini alın
- [Hata düzeltildi] hata oluştuğunda benzetimli ilerleme durumunu ve zaman güncelleştirmelerini izlemeyi devre dışı bırakın.
- [Hata düzeltilme] sağ tıklama menüsünü kaldır
- [Hata düzeltildi] [AzureHtml5JS] PlayReady içeriği için geçersiz belirteç kümesi olduğunda hata iletisi görüntülenmiyor
- [Hata düzeltildi] [AzureHtml5JS] canlı kayıttan yürütme sırasında tam ekran, hesap için pencere boyutu buluşsal yöntemlerini hesaba katmadı
- [Hata düzeltildi] [FlashSS] Yalnızca Azure Media Player iletilerin gösterilmesi için Media Player görüntülenen iletiler kaldırıldı
- [Hata düzeltildi] [SilverlightSS] süresi aşdığımızda veya 0 ' dan küçük olduğunda ' seeked ' olayı alınamadı

## <a name="010-beta-release"></a>0.1.0 (Beta sürümü) ##

İlk yayın öncesi

## <a name="next-steps"></a>Sonraki adımlar ##

- [Hızlı başlangıç Azure Media Player](azure-media-player-quickstart.md)

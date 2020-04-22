---
title: Azure Media Player Bilinen Sorunlar
description: Geçerli sürüm aşağıdaki bilinen sorunları vardır.
author: IngridAtMicrosoft
ms.author: inhenkel
ms.service: media-services
ms.topic: reference
ms.date: 04/20/2020
ms.openlocfilehash: ff8dc58b9122e5173a9a6065e2efdbc5697be0d7
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81727222"
---
# <a name="known-issues"></a>Bilinen Sorunlar #

Geçerli sürüm aşağıdaki bilinen sorunları vardır:

## <a name="azure-media-player"></a>Azure Media Player ##

- Yanlış yapılandırılmış kodlayıcılar oynatma ile ilgili sorunlara neden olabilir
- Zaman damgaları 2^53'ten büyük olan akışlarda oynatma sorunları olabilir.
  - Azaltma: 90 kHz video ve 44.1-kHz ses zaman ölçekleri kullanın
- Kullanıcı etkileşimi olmadan mobil cihazlarda otomatik oynatma yok. Platform tarafından engellendi.
- Yakın süreksizlikler aranıyor oynatma hatasına neden olabilir.
- Büyük sunuların karşıdan yüklenirse, ui'nin kilitlenmesine neden olabilir.
- Sunu sona erdikten sonra aynı kaynağı otomatik olarak oynatamıyorum.
  - Bir kaynağın sona erdikten sonra yeniden oynatılabilmesi için kaynağı yeniden ayarlaması gerekir.
- Boş bildirimler oyuncuyla ilgili sorunlara neden olabilir.
  - Bu sorun, canlı akış başlatılırken oluşabilir ve bildirimde yeterli öbek bulunmaz.
- Oynatma pozisyonu belki UI arama çubuğu dışında.
- Olay sıralama tüm teknisyenler arasında tutarlı değildir.
- Arabelleğe alma özelliği, teknisyenler arasında tutarlı değildir.
- nativeControlsForTouch "Nesne özelliği veya yöntemi 'setControls' desteklemez önlemek için yanlış (varsayılan) olmalıdır
- Posterler artık mutlak url'ler olmalıdır
- Cihazın yüksek kontrastlı modu kullanılarak UI'de küçük estetik sorunlar oluşabilir
- Tamamen çözülmüş dizede "%" veya "+" içeren URL'ler kaynağı ayarlarken sorun yaşayabilir

## <a name="ad-insertion"></a>Reklam ekleme ##

- Tarayıcıya bir reklam engelleyici yüklendiğinde reklamların eklenmesinde (isteğe bağlı veya canlı) sorunlar olabilir
- Mobil cihazlarda reklamları oynatma da sorun yaşayabilir.

## <a name="azurehtml5js"></a>AzureHtml5JS ##

- Canlı içeriğin DVR penceresinde, içerik bittiğinde zaman çizelgesi alana arayana veya sununun sonuna ulaşana kadar büyümeye devam eder.
- MSE etkin Firefox'ta canlı sunumlar bazı sorunlar var
- Yalnızca ses veya video olan varlıklar AzureHtml5JS teknolojisi aracılığıyla oynatılmıyor.
  - Varlıkları ses veya video olmadan oynatmak istiyorsanız, [Azure Media Services Explorer aracını](https://aka.ms/amse) kullanarak boş ses veya video ekleyerek bunu yapabilirsiniz
    - Sessiz ses inekletmek için nasıl talimatlar [burada](https://azure.microsoft.com/documentation/articles/media-services-advanced-encoding-with-mes/#silent_audio) bulunabilir

## <a name="flash"></a>Flash ##

- AES içeriği, Adobe'nin önbelleğe alma mantığındaki bir hata nedeniyle Flash sürüm 30.0.0.134'te oynatılmaz. Adobe sorunu giderdi ve 30.0.0.154 yılında yayımladı
- Teknik ve http hataları (404 ağ zaman aşımı gibi), oyuncu diğer teknisyenler daha kurtarmak için daha uzun sürer.
- flashSS teknolojisi ile video alanına tıklayın oynatmaz/duraklatamaz.
- Kullanıcı Flash yüklüyse ancak siteye yükleme izni vermiyorsa, sonsuz döndürme oluşabilir. Oyuncu eklentisi yüklü ve kullanılabilir olduğunu düşünüyor ve eklenti içeriği çalıştırıyor düşünüyor olmasıdır. JavaScript kodu gönderildi ancak kullanıcı eklentiye izin vermek için istem'i kabul edene kadar tarayıcı ayarları eklentinin çalıştırMasını engelledi. Bu, tüm tarayıcılarda oluşabilir.  

## <a name="silverlight"></a>Silverlight ##

- Eksik özellikler
- Teknik ve http hataları (404 ağ zaman aşımı gibi), oyuncu diğer teknisyenler daha kurtarmak için daha uzun sürer.
- Safari ve Firefox Silverlight ile Mac oynatma `"http://` açıkça `https://` tanımlama veya kaynak için gerektirir.
- Bu teknoloji için bir API eksikse, genellikle null geri döner.
- Kullanıcı Flash yüklüyse ancak siteye yükleme izni vermiyorsa, sonsuz döndürme oluşabilir. Oyuncu eklentisi yüklü ve kullanılabilir olduğunu düşünüyor ve eklenti içeriği çalıştırıyor düşünüyor olmasıdır. JavaScript kodu gönderildi ancak kullanıcı eklentiye izin vermek için istem'i kabul edene kadar tarayıcı ayarları eklentinin çalıştırMasını engelledi. Bu, tüm tarayıcılarda oluşabilir.  

## <a name="native-html5"></a>Yerel HTML5 ##

- Html5 tech sadece ilk set kaynağı için canplaythrough olay gönderiyor.
- Bu teknoloji yalnızca tarayıcının uyguladığı şeyi destekler.  Bazı özellikler bu teknoloji eksik olabilir.  
- Bu teknoloji için bir API eksikse, genellikle null geri döner.
- Bu teknoloji üzerinde Altyazılar ve Altyazılar ile ilgili sorunlar vardır. Bu teknolojide kullanılabilir veya görüntülenebilir veya görüntülenemeyebilirler.
- HLS/Html5 teknoloji senaryosunda sınırlı bant genişliğine sahip olmak, video olmadan ses oynatmayla sonuçlanır.

### <a name="microsoft"></a>Microsoft ###

- IE8 oynatma şu anda ECMAScript 5 ile uyumsuzluk nedeniyle çalışmıyor
- IE ve Edge'in bazı sürümlerinde, tam ekran düğmesine sekme yle ve seçilerek veya F/f hotkey kullanılarak girilemez.

## <a name="google"></a>Google ##

- Aynı bildirimdeki birden çok kodlama profili Chrome'da bazı oynatma sorunları vardır ve önerilmez.
- Chrome, He-AAC'yi AzureHtml5JS ile oynatamaz. [Hata izleyicisi](https://bugs.chromium.org/p/chromium/issues/detail?id=534301)hakkındaki ayrıntıları takip edin.
- Chrome v58 itibariyle, widevine içeriğihttps:// protokolü aracılığıyla yüklenmeli/oynatılmalıdır aksi takdirde oynatma başarısız olur.

## <a name="mozilla"></a>Mozilla ##

- Ses akışı anahtarı, AzureHtml5JS kullanırken ses akışlarının aynı codec özel verilere sahip olmasını gerektirir. Firefox platformu bunu gerektirir.

## <a name="apple"></a>Apple ##

- Mac'teki Safari, kullanıcıya lehine olmadığına inandıkları flash ve Silverlight gibi eklentileri engelleyen "Güç tasarrufu için eklentileri durdurun" ayarı ile genellikle Varsayılan olarak Power Saver moduna olanak tanır. Bu blok eklentinin var olan, yalnızca yeteneklerini engellemez. Varsayılan techOrder göz önüne alındığında, bu oynatmagirişiminde sorunlara neden olabilir
  - Azaltma 1: Video oynatıcı 'ön ve orta' ise (belgenin sol üst köşesinden başlayarak 3000 x 3000 piksel lik bir sınır içinde), yine de oynatılmalıdır.
  - Azaltma 2: Safari için techOrder'ı ["azureHtml5JS", "html5"] olarak değiştirin. Bu azaltma, FlashSS teknolojisinde bulunan tüm özellikleri almama imasa sahiptir.
- Silverlight üzerinden PlayReady içeriği Safari'de oynatılabilecek sorunlar olabilir.
- AES ve sınırlı belirteç içeriği iOS ve eski Android cihazları kullanarak oynatılamıyor.
  - Bu senaryoyu gerçekleştirmek için hizmetinize bir proxy eklenmesi gerekir.
- iOS Telefon için varsayılan görünüm ler ile gösterir.
- iPhone oynatma her zaman yerli oyuncu tam ekran oluşur.
  - Altyazılar da dahil olmak üzere özellikler, bu satır içi oynatma da devam olmayabilir.
- Canlı sunu sona erdiğinde, iOS aygıtları sunuyu düzgün bir şekilde sona erdirmez.
- iOS DVR yetenekleriiçin izin vermez.
- iOS ses akışı anahtarı yalnızca iOS yerel oyuncu ui olsa yapılabilir ve ses akışlarının aynı codec özel verilere sahip olmasını gerektirir
- Safari'nin eski sürümlerinde canlı akışları oynatma sorunları olabilir.

## <a name="older-android"></a>Eski Android ##

- AES ve sınırlı belirteç içeriği iOS ve eski Android cihazları kullanarak oynatılamıyor.
  - Bu senaryoyu gerçekleştirmek için hizmetinize bir proxy eklenmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar ##

- [Azure Media Player Quickstart](azure-media-player-quickstart.md)
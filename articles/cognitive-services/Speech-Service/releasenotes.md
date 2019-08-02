---
title: Sürüm notları-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti için çalışan bir dizi özellik sürümü, geliştirmeler, hata düzeltmeleri ve bilinen sorunlar hakkında bilgi bulabilirsiniz.
services: cognitive-services
author: BrianMouncer
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: jhakulin
ms.custom: seodec18
ms.openlocfilehash: 12eecc4998153cbeedeb907ecad33c56141a50e6
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559132"
---
# <a name="release-notes"></a>Sürüm notları

## <a name="speech-sdk-160-2019-june-release"></a>Konuşma SDK 1.6.0: 2019-Haziran sürümü

**Örnekler**
*   UWP ve Unity 'de metin okuma için hızlı başlangıç örnekleri
*   İOS 'ta Swift için hızlı başlangıç örneği
*   Konuşma & Amaç Tanıma ve çevirisi için Unity örnekleri
*   DialogServiceConnector için hızlı başlangıç örnekleri

**Geliştirmeler/değişiklikler**
* İletişim kutusu ad alanı:
    * SpeechBotConnector, DialogServiceConnector olarak yeniden adlandırıldı
    * BotConfig, DialogServiceConfig olarak yeniden adlandırıldı
    * BotConfig:: FromChannelSecret (), DialogServiceConfig:: FromBotSecret () ile yeniden eşlendi
    * Var olan tüm doğrudan hat konuşma istemcileri yeniden adlandırmadan sonra desteklenmeye devam eder
* Proxy, kalıcı bağlantıyı destekleyecek şekilde TTS REST bağdaştırıcısını güncelleştirme
* Geçersiz bir bölge geçirildiğinde hata iletisini geliştir
* Swift/amaç-C:
    * Geliştirilmiş hata raporlama: Hataya neden olan yöntemler iki sürümde mevcuttur: Bir `NSError` nesne, hata işleme için ve bir özel durum Başlatan bir nesnesi ortaya çıkaran bir. İlki Swift 'a açıktır. Bu değişiklik, mevcut Swift koduna uyarlamalar gerektirir.
    * Geliştirilmiş olay işleme

**Hata düzeltmeleri**
*   TTS için çözüm: Hoparlörktextasync, sesin işleme tamamlanana kadar beklemeden döndürüldüğü
*   Tam dil desteğini etkinleştirmek için C# içinde sıralama dizelerini onarma
*   Örneklerde net461 Target Framework ile çekirdek kitaplığı yükleme ile ilgili .NET Core uygulama sorunu için çözüm
*   Örneklerindeki çıkış klasörüne yerel kitaplıkları dağıtmaya yönelik zaman zaman sorun giderme
*   Güvenli bir şekilde Web yuvası kapatma için çözüm
*   Linux üzerinde çok ağır yük altında bir bağlantı açılırken olası kilitlenme için çözüm
*   MacOS için çerçeve paketinde eksik meta veriler için çözüm
*   Windows `pip install --user` 'da sorun giderme


## <a name="speech-sdk-151"></a>Konuşma SDK 1.5.1

Bu bir hata çözme sürümüdür ve yalnızca yerel/yönetilen SDK 'yi etkiliyor. SDK 'nın JavaScript sürümünü etkilemiyor.

**Hata düzeltmeleri**

* Konuşma dökümü ile kullanıldığında FromSubscription 'ı onarın.
* Ses-ilk Sanal Yardımcısı için anahtar sözcük biriktirmesine hata giderme.


## <a name="speech-sdk-150-2019-may-release"></a>Konuşma SDK 1.5.0: 2019-yayın Mayıs

**Yeni Özellikler**

* Word 'ü uyandırma (anahtar sözcük biriktirme/KWS) işlevselliği artık Windows ve Linux 'ta kullanılabilir. KWS işlevselliği herhangi bir mikrofon türü ile çalışabilir, ancak resmi KWS desteği şu anda Azure Kinect DK donanımında veya konuşma cihazları SDK 'sında bulunan mikrofon dizileri ile sınırlıdır.
* Tümcecik ipucu işlevselliği SDK aracılığıyla kullanılabilir. Daha fazla bilgi için [buraya](how-to-phrase-lists.md)bakın.
* Konuşma dökümü işlevselliği SDK aracılığıyla kullanılabilir. [Buraya](conversation-transcription-service.md)bakın.
* Doğrudan hat konuşma kanalını kullanarak, sesli ilk sanal Yardımcılar için destek ekleyin.

**Örnekler**

* Yeni özellikler veya SDK tarafından desteklenen yeni hizmetler için örnek eklendi.

**Geliştirmeler/değişiklikler**

* Hizmet davranışını veya hizmet sonuçlarını (maskeleme küfür ve diğerleri gibi) ayarlamak için çeşitli tanıyıcı özellikleri eklendi.
* Artık tanıyıcıyı oluşturmuş `FromEndpoint`olsanız bile, tanıyıcıyı standart yapılandırma özellikleriyle yapılandırabilirsiniz.
* SPXSpeechConfiguration 'e hedef- `OutputFormat` C: özelliği eklendi.
* SDK artık Linux dağıtımı olarak 9.

**Hata düzeltmeleri**

* Konuşmacı kaynağının metinden konuşmaya çok erken bir yere çıkarılması sorunu düzeltildi.
## <a name="speech-sdk-142"></a>Konuşma SDK 1.4.2

Bu bir hata çözme sürümüdür ve yalnızca yerel/yönetilen SDK 'yi etkiliyor. SDK 'nın JavaScript sürümünü etkilemiyor.

## <a name="speech-sdk-141"></a>Konuşma SDK 1.4.1

Bu yalnızca bir JavaScript sürümüdür. Hiçbir özellik eklenmedi. Aşağıdaki düzeltmeler yapılmıştır:

* Web paketinin HTTPS-proxy-Agent yüklemesini engelleyin.

## <a name="speech-sdk-140-2019-april-release"></a>Konuşma SDK 1.4.0: 2019-Nisan yayını

**Yeni Özellikler** 

* SDK artık beta sürümü olarak metinden konuşmaya hizmetini desteklemektedir. Ve ' C++ C#den Windows ve Linux Desktop 'ta desteklenir. Daha fazla bilgi için, [metinden konuşmaya genel bakış ' a](text-to-speech.md#get-started-with-text-to-speech)bakın.
* SDK artık MP3 ve Opus/OGG ses dosyalarını akış giriş dosyaları olarak desteklemektedir. Bu özellik yalnızca Linux C++ üzerinde kullanılabilir C# ve şu anda beta aşamasındadır (daha ayrıntılı bilgi). [](how-to-use-codec-compressed-audio-input-streams.md)
* Java, .NET Core C++ ve amaç-C için konuşma SDK 'Sı, MacOS desteği elde etmiş. MacOS için amaç-C desteği şu anda beta aşamasındadır.
* iOS: İOS için konuşma SDK 'Sı (hedef-C) artık bir CocoaPod olarak yayımlanmıştır.
* JavaScript Giriş cihazı olarak varsayılan olmayan mikrofon desteği.
* JavaScript Node. js için proxy desteği.

**Örnekler**

* MacOS üzerinde WITH C++ hedefi-C ile ve ile konuşma SDK 'sını kullanmaya yönelik örnekler eklenmiştir.
* Metin okuma hizmetinin kullanımını gösteren örnekler eklenmiştir.

**Geliştirmeler/değişiklikler**

* Python: Tanınma sonuçlarının ek özellikleri artık `properties` özelliği aracılığıyla kullanıma sunulmuştur.
* Ek geliştirme ve hata ayıklama desteği için, SDK günlüğü ve tanılama bilgilerini bir günlük dosyasına yeniden yönlendirebilir ( [burada](how-to-use-logging.md)daha fazla bilgi bulabilirsiniz).
* JavaScript Ses işleme performansını geliştirir.

**Hata düzeltmeleri**

* Mac/iOS: Konuşma hizmetine bir bağlantı kurulamazsa uzun bir bekleme süresine yol açmıştı.
* Python: Python geri çağırmalar içindeki bağımsız değişkenler için hata işlemeyi geliştir.
* JavaScript RequestSession 'da konuşma için yanlış durum bildirimi düzeltildi.

## <a name="speech-sdk-131-2019-february-refresh"></a>Konuşma SDK 1.3.1: 2019-Şubat yenilemesi

Bu bir hata çözme sürümüdür ve yalnızca yerel/yönetilen SDK 'yi etkiliyor. SDK 'nın JavaScript sürümünü etkilemiyor.

**Hata onarımı**

* Mikrofon girişi kullanılırken bellek sızıntısı düzeltildi. Akış tabanlı veya dosya girişi etkilenmez.

## <a name="speech-sdk-130-2019-february-release"></a>Konuşma SDK 1.3.0: 2019-Şubat yayını

**Yeni Özellikler**

* Konuşma SDK 'Sı, ses yapılandırma sınıfı aracılığıyla giriş mikrofonunun seçimini destekler. Bu, konuşma hizmetlerinde varsayılan olmayan bir mikrofondan ses verileri akışını sağlar. Daha fazla bilgi için bkz. [ses girişi cihaz seçimini](how-to-select-audio-input-devices.md)açıklayan belgeler. Bu özellik JavaScript 'te henüz kullanılamaz.
* Konuşma SDK 'Sı artık bir beta sürümünde Unity 'yi desteklemektedir. [GitHub örnek deposundaki](https://aka.ms/csspeech/samples)sorun bölümüne geri bildirim sağlayın. Bu sürüm, Windows x86 ve x64 (masaüstü veya Evrensel Windows Platformu uygulamaları) ve Android (ARM32/64, x86) üzerinde Unity 'yi destekler. [Unity hızlı](quickstart-csharp-unity.md)başlangıçmızda daha fazla bilgi bulabilirsiniz.
* Dosya `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (önceki sürümlerde sunulan) artık gerekli değildir. İşlevsellik artık temel SDK ile tümleşiktir.


**Örnekler**

[Örnek](https://aka.ms/csspeech/samples)depoımızda aşağıdaki yeni içerik mevcuttur:

* AudioConfig. Frommikro Phoneınput için ek örnekler.
* Amaç tanıma ve çeviri için ek Python örnekleri.
* İOS 'ta bağlantı nesnesini kullanmak için ek örnekler.
* Ses çıkışıyla çeviri için ek Java örnekleri.
* [Toplu Iş dökümü REST API](batch-transcription.md)kullanımı için yeni örnek.

**Geliştirmeler/değişiklikler**

* Python
  * SpeechConfig ' de geliştirilmiş parametre doğrulama ve hata iletileri.
  * Bağlantı nesnesi için destek ekleyin.
  * Windows üzerinde 32-bit Python (x86) için destek.
  * Python için konuşma SDK 'Sı beta dışında.
* iOS
  * SDK artık iOS SDK 12,1 sürümüne göre oluşturulmuştur.
  * SDK artık iOS sürümleri 9,2 ve üstünü desteklemektedir.
  * Başvuru belgelerini geliştirir ve birçok özellik adını düzeltir.
* JavaScript
  * Bağlantı nesnesi için destek ekleyin.
  * Paketlenmiş JavaScript için tür tanımı dosyaları ekleme
  * Tümcecik ipuçları için başlangıç desteği ve uygulama.
  * Tanıma için Service JSON ile Özellikler koleksiyonunu döndürün
* Windows dll 'Leri artık bir sürüm kaynağı içeriyor.
* Bir tanıyıcı `FromEndpoint` oluşturursanız, doğrudan Endpoint URL 'sine parametreler ekleyebilirsiniz. Kullanarak `FromEndpoint` , tanıyıcıyı standart yapılandırma özellikleri aracılığıyla yapılandıramazsınız.

**Hata düzeltmeleri**

* Boş Proxy Kullanıcı adı ve proxy parolası doğru işlenmedi. Bu sürümle birlikte, Proxy Kullanıcı adı ve proxy parolasını boş bir dizeye ayarlarsanız, ara sunucuya bağlanılırken gönderilmez.
* SDK tarafından oluşturulan SessionID, bazı diller&nbsp;/ortamlar için her zaman gerçekten gelişigüzel değildir. Bu sorunu giderecek rastgele Oluşturucu başlatması eklendi.
* Yetkilendirme belirtecinin işlenmesini geliştirme. Bir yetkilendirme belirteci kullanmak istiyorsanız, SpeechConfig içinde belirtin ve abonelik anahtarını boş bırakın. Ardından tanıyıcı her zamanki gibi oluşturun.
* Bazı durumlarda bağlantı nesnesi doğru şekilde yayımlanmamıştı. Bu sorun düzeltilmiştir.
* JavaScript örneği, çeviri birleştirmesinin ses çıkışını desteklemek için de Safari 'de düzeltildi.

## <a name="speech-sdk-121"></a>Konuşma SDK 'Sı 1.2.1

Bu yalnızca bir JavaScript sürümüdür. Hiçbir özellik eklenmedi. Aşağıdaki düzeltmeler yapılmıştır:

* Stream. End sırasında değil, Stream 'in sonuna kadar bir akış sonu tetiklendir.
* Geçerli gönderme başarısız olduysa, sonraki gönderimi zamanlamadığı ses göndericisinin hatasını düzeltir.
* Kimlik doğrulama belirteciyle sürekli tanımayı düzeltir.
* Farklı tanıyıcı/uç noktalar için hata düzeltildi.
* Belgeleri geliştirmeleri.

## <a name="speech-sdk-120-2018-december-release"></a>Konuşma SDK 1.2.0: 2018-Aralık yayını

**Yeni Özellikler**

* Python
  * Python desteğinin beta sürümü (3,5 ve üzeri) Bu sürümde kullanılabilir. Daha fazla bilgi için buraya bakın] (hızlı başlangıç-python.md).
* JavaScript
  * JavaScript için konuşma SDK 'Sı açık kaynaklıdır. Kaynak kodu [GitHub](https://github.com/Microsoft/cognitive-services-speech-sdk-js)' da kullanılabilir.
  * Artık Node. js ' yi destekliyoruz, [burada](quickstart-js-node.md)daha fazla bilgi bulabilirsiniz.
  * Ses oturumları için uzunluk kısıtlaması kaldırılmıştır, otomatik olarak yeniden bağlanma işlemi, kapak altında otomatik olarak gerçekleşir.
* Bağlantı nesnesi
  * Tanıyıcıdan bir bağlantı nesnesine erişebilirsiniz. Bu nesne, hizmet bağlantısını açık bir şekilde başlatabilmeniz ve olaylara bağlanıp bağlantıyı kesmeye abone olmanızı sağlar.
    (Bu özellik JavaScript ve Python 'da henüz kullanılamaz.)
* Ubuntu 18,04 desteği.
* Android
  * APK oluşturma sırasında ProGuard desteği etkinleştirildi.

**Geliştirmeleri**

* İç iş parçacığı kullanımındaki geliştirmeler, iş parçacığı sayısı, kilitler, zaman uyumu sağlayıcılar sayısını azaltır.
* Geliştirilmiş hata raporlama/bilgi. Birçok durumda, hata iletileri hiç bir şekilde yayılmaz.
* Güncel modüller kullanmak için JavaScript 'teki geliştirme bağımlılıkları güncelleştirildi.

**Hata düzeltmeleri**

* RecognizeAsync içindeki bir tür uyumsuzluğu nedeniyle sabit bellek sızıntıları.
* Bazı durumlarda özel durumlar sızdırılmıştı.
* Çeviri olay bağımsız değişkenlerinde bellek sızıntısı düzeltiliyor.
* Uzun süre çalışan oturumlarda yeniden bağlantı sırasında bir kilitleme sorunu düzeltildi.
* Başarısız çeviriler için eksik nihai sonuca neden olabilecek bir sorun düzeltildi.
* C# İÇİN: Zaman uyumsuz bir işlem ana iş parçacığında beklenmediyse, zaman uyumsuz görev tamamlanmadan önce tanıyıcı atılamamalıdır.
* Java: Bir sorun düzeltildi Java VM kilitlenmesiyle sonuçlanır.
* Amaç-C: Sabit enum eşlemesi; RecognizedIntent, Recognizingamaç yerine döndürüldü.
* JavaScript SpeechConfig içinde varsayılan çıkış biçimini ' Simple ' olarak ayarlayın.
* JavaScript JavaScript ve diğer dillerdeki yapılandırma nesnesindeki özellikler arasındaki tutarsızlığı kaldırma.

**Örnekler**

* Birkaç örnek güncelleştirildi ve düzeltildi (örneğin, çeviri için çıkış sesleriniz vb.).
* [Örnek depoya](https://aka.ms/csspeech/samples)Node. js örnekleri eklendi.

## <a name="speech-sdk-110"></a>Konuşma SDK 1.1.0

**Yeni Özellikler**

* Android x86 x64 desteği.
* Proxy desteği: SpeechConfig nesnesinde, artık proxy bilgilerini (ana bilgisayar adı, bağlantı noktası, Kullanıcı adı ve parola) ayarlamak için bir işlev çağırabilirsiniz. Bu özellik henüz İos'ta kullanılabilir değil.
* Geliştirilmiş hata kodu ve iletileri. Bir tanıma bir hata döndürülürse, bu zaten ayarlanmış `Reason` (içinde iptal edilen olay için) veya `CancellationDetails` (tanıma işleminin sonucu içinde) için `Error`. İptal edilen olay şimdi iki ek üyeleri içeren `ErrorCode` ve `ErrorDetails`. Sunucu ek hata bilgileri ile bildirilen hata döndürdüyse, artık yeni üyeleri kullanılabilir olacaktır.

**Geliştirmeleri**

* Eklenen ek doğrulama tanıyıcı yapılandırma ve eklenen ek hata iletisi.
* Uzun süreli sessizlik ortasında bir ses dosyasının işlenmesinde'yi tıklatın.
* NuGet paketi: isteğe bağlı olarak .NET Framework projeleri için yapı AnyCPU yapılandırmayla engeller.

**Hata düzeltmeleri**

* Tanıyıcılar işlem bulunan birkaç özel durum düzeltildi. Ayrıca, özel durumlar yakalanıp Iptal edildi olayına dönüştürülür.
* Özellik Yönetimi'nde bir bellek sızıntısı düzeltin.
* Ses giriş dosyası tanıyıcı kilitlenebiliyordu hata düzeltildi.
* Olayları sonra oturumu durdurulduğunda burada alınan bir hata düzeltildi.
* İş parçacığı içinde bazı yarış durumları düzeltildi.
* Bir iOS bir çökmesine neden neden olabilecek bir uyumluluk sorunu düzeltildi.
* Android mikrofon desteği kararlılık geliştirmeleri.
* Javascript'teki bir tanıyıcı tanıma dil yok burada bir hata düzeltildi.
* (Bazı durumlarda) EndpointId ayarlama engelleyen bir hatayı JavaScript'te düzeltildi.
* JavaScript ve eklenen eksik AddIntent JavaScript imza AddIntent sırayla değiştirilen bir parametre.

**Örnekler**

* C++ eklendi ve C# çekme ve itme stream kullanımına yönelik samplea [örnek depoyu](https://aka.ms/csspeech/samples).

## <a name="speech-sdk-101"></a>Konuşma SDK 1.0.1

Güvenilirlik geliştirmeleri ve hata düzeltmeleri:

* Disposing tanıyıcı yarış durumu nedeniyle olası sabit önemli hata
* Ayarlama özellikleri durumunda sabit olası önemli hata.
* Eklenen ek hata ve parametre denetimi.
* Amaç-C: NSString 'te ad geçersiz kılma nedeniyle oluşan olası önemli hata düzeltildi.
* Amaç-C: API 'nin ayarlanmış görünürlüğü
* JavaScript Olaylar ve yükleri hakkında düzeltildi.
* Belgeleri geliştirmeleri.

İçinde bizim [örnek depoyu](https://aka.ms/csspeech/samples), JavaScript için yeni bir örnek eklendi.

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>Bilişsel hizmetler konuşma SDK 1.0.0: 2018-Eylül yayını

**Yeni Özellikler**

* İOS üzerinde Objective-C için destek. Kullanıma sunduğumuz [iOS Objective-C hızlı](quickstart-objectivec-ios.md).
* Tarayıcıda JavaScript desteği. Kullanıma sunduğumuz [JavaScript hızlı](quickstart-js-browser.md).

**Bozucu değişiklikler**

* Bu sürümle birlikte, bir dizi Son değişiklik yapılmıştır.
  Ayrıntılar için [Bu sayfaya](https://aka.ms/csspeech/breakingchanges_1_0_0) bakın.

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>Bilişsel hizmetler konuşma SDK 0.6.0: 2018-Ağustos yayını

**Yeni Özellikler**

* UWP uygulamaları artık yerleşik olarak konuşma SDK'sı ile Windows uygulama sertifikası Kiti (WACK) geçirebilirsiniz.
  Kullanıma [UWP hızlı](quickstart-csharp-uwp.md).
* .NET Standard 2.0 Linux'ta (Ubuntu 16.04 x 64) için destek.
* Deneysel Windows (64-bit) ve Linux (Ubuntu 16,04 x64) üzerinde Java 8 desteği.
  Kullanıma [Java Çalışma zamanı ortamı hızlı](quickstart-java-jre.md).

**İşlev değişikliği**

* Ek hata bağlantı hatalarıyla ilgili ayrıntılı bilgi kullanıma sunar.

**Bozucu değişiklikler**

* Java (Android) üzerinde `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` işlevi artık bir yol parametresi gerektirir. Artık bir yolu, tüm desteklenen platformlarda otomatik olarak algılanır.
* Özelliğin get erişimcisine `EndpointUrl` Java ve C# ' kaldırıldı.

**Hata düzeltmeleri**

* Java dilinde çeviri tanıyıcı ses sentezi sonucuna artık uygulanır.
* Etkin olmayan iş parçacıkları ve açık ve kullanılmayan yuva sayısının artması neden olabilecek bir hata düzeltildi.
* Uzun süreli tanıma ortasında iletim burada sonlandırmak bir sorun düzeltildi.
* Tanıyıcı kapatma bir yarış durumu düzeltildi.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Bilişsel hizmetler konuşma SDK 0.5.0: 2018-Temmuz sürümü

**Yeni Özellikler**

* Android platformu desteği (API 23: Android 6,0 sıralamada Mallow veya üzeri). Kullanıma [Android Hızlı Başlangıç](quickstart-java-android.md).
* Windows üzerinde .NET Standard 2.0 desteği. Kullanıma [.NET Core hızlı](quickstart-csharp-dotnetcore-windows.md).
* Deneysel Windows 'da UWP 'yi destekleme (sürüm 1709 veya üzeri).
  * Kullanıma [UWP hızlı](quickstart-csharp-uwp.md).
  * Not: Konuşma SDK 'Sı ile derlenen UWP uygulamaları henüz Windows uygulama sertifika seti 'Ni (WACK) iletmez.
* Otomatik yeniden bağlanma ile uzun süreli tanıma destekler.

**İşlevsel değişiklikler**

* `StartContinuousRecognitionAsync()` uzun süreli tanıma destekler.
* Tanıma işleminin sonucu daha fazla alan içeriyor. Ses başına ve süresi (hem de saat döngüsü) ve tanınan metin tanıma durumu, örneğin, temsil eden ek değerler uzaklığı `InitialSilenceTimeout` ve `InitialBabbleTimeout`.
* AuthorizationToken factory örnekleri oluşturmak için destek.

**Bozucu değişiklikler**

* Tanıma olayları: NoMatch olay türü hata olayında birleştirildi.
* SpeechOutputFormat C#, C++ ile uyumlu kalmak için OutputFormat olarak değiştirildi.
* Bazı yöntemleri dönüş türünü `AudioInputStream` arabirim biraz değişti:
   * Java'da,. `read` yöntemi şimdi döndürür `long` yerine `int`.
   * C# ' ta, `Read` yöntemi şimdi döndürür `uint` yerine `int`.
   * C++ ' ta `Read` ve `GetFormat` yöntemleri artık dönüş `size_t` yerine `int`.
* C++: Artık ses giriş akışlarının örnekleri yalnızca olarak `shared_ptr`geçirilebilir.

**Hata düzeltmeleri**

* Sonuçta hatalı dönüş değerleri sabit olduğunda `RecognizeAsync()` zaman aşımına uğrar.
* Windows media foundation kitaplıkları bağımlılığı kaldırıldı. SDK'sı artık çekirdek ses API'leri kullanır.
* Belge onarımı: Desteklenen bölgeleri anlatmak için bir [bölgeler](regions.md) sayfası eklendi.

**Bilinen sorun**

* Android Speech SDK'sı için çeviri konuşma sentezi sonuçları bildirmez. Bu sorun, sonraki sürümde düzeltilecektir.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Bilişsel hizmetler konuşma SDK 0.4.0: 2018-Haziran sürümü

**İşlevsel değişiklikler**

- AudioInputStream

  Bir tanıyıcı, artık bir akışı ses kaynağı olarak tüketebilir. Daha fazla bilgi için bkz. ilgili [yapılır kılavuzunda](how-to-use-audio-input-streams.md).

- Ayrıntılı çıkış biçimi

  Oluştururken bir `SpeechRecognizer`, talep edebilir `Detailed` veya `Simple` çıkış biçimi. `DetailedSpeechRecognitionResult` Maskelenmiş küfür ile bir güven puanı, tanınan metin, ham bir sözcük formu, normalleştirilmiş form ve normalleştirilmiş form içerir.

**Yeni değişiklik**

- Değiştirilecek `SpeechRecognitionResult.Text` gelen `SpeechRecognitionResult.RecognizedText` C#.

**Hata düzeltmeleri**

- Olası bir geri çağırma sorunu USP katmanda kapatılırken düzeltildi.

- Ses giriş dosyası bir tanıyıcı kullanılan, onu için gerekenden daha uzun dosya tanıtıcısı bulunduran.

- İleti pompası tanıyıcı arasındaki birkaç kilitlenmeleri kaldırıldı.

- Ateş bir `NoMatch` hizmetinden gelen yanıt zaman aşımına neden.

- Windows media foundation kitaplıklarındaki Gecikmeli yüklendi ' dir. Bu kitaplık, yalnızca giriş mikrofon için gereklidir.

- Karşıya yükleme hızı için ses verilerini iki kez özgün ses hızı hakkında sınırlıdır.

- Windows üzerinde C# .NET derlemeleri artık strong adlandırılır.

- Belge düzeltmesi: `Region` bir tanıyıcı oluşturmak için gerekli bir bilgidir.

Daha fazla örnek eklenmiştir ve sürekli olarak güncelleştiriliyor. En son örnekleri için bkz [Speech SDK'sı örnekleri GitHub deposunda](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Bilişsel hizmetler konuşma SDK 0.2.12733: 2018-yayın Mayıs

Bu sürüm ilk Bilişsel hizmetler konuşma SDK'sı genel Önizleme sürümüdür.

---
title: Sürüm notları-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma hizmeti Özellik sürümlerinin, geliştirmelerin, hata düzeltmelerinin ve bilinen sorunların çalışan bir günlüğü.
services: cognitive-services
author: brianem
manager: jhakulin
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: brianem
ms.custom: seodec18
ms.openlocfilehash: cbe9c7cbd0f402e38d1163050d77b055f89948ba
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79219648"
---
# <a name="release-notes"></a>Sürüm notları
## <a name="speech-sdk-1100-2020-february-release"></a>Konuşma SDK 1.10.0:2020-Şubat yayını

**Yeni özellikler**
 - Python 'un yeni 3,8 sürümünü desteklemek için Python paketleri eklendi.
 - Red Hat Enterprise Linux (RHEL)/CentOS 8 x64 desteği (C++, C#, Java, Python).
   > [!NOTE] 
   > Müşterilerin OpenSSL 'yi [Bu yönergelere](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-configure-openssl-linux)göre yapılandırması gerekir.
 - DeARM32 ve Ubuntu için Linux desteği.
 - DialogServiceConnector artık BotFrameworkConfig üzerinde isteğe bağlı "bot ID" parametresini destekliyor. Bu parametre, tek bir Azure konuşma kaynağı ile birden çok doğrudan hat konuşma 'nın kullanımına izin verir. Belirtilen parametre olmadan, varsayılan bot (doğrudan hat konuşma kanalı yapılandırma sayfası tarafından belirlendiği şekilde) kullanılacaktır.
 - DialogServiceConnector artık bir SpeechActivityTemplate özelliğine sahip. Bu JSON dizesinin içeriği, konuşma tanıma gibi olaylara yanıt olarak otomatik olarak oluşturulan etkinlikler dahil olmak üzere doğrudan hat konuşma bot 'a ulaşan tüm etkinliklerde desteklenen çok çeşitli alanları önceden doldurmak için kullanılır.
 - Şimdi TTS, kimlik doğrulaması için abonelik anahtarını kullanır, birleştirici oluşturulduktan sonra ilk sende ilk bayt gecikme süresini azaltır.
 - Ortalama% 18,6 bir sözcük hata oranı azalmasıyla ilgili 19 yerel ayar için konuşma tanıma modelleri güncelleştirildi (ES-ES, es-MX, fr-CA, fr-FR, It-IT, ja-JP, ko-KR, PT-BR, zh-CN, zh-HK,, NB-NO, Fi-FL, ru-RU, PL-PL, CA-ES, zh-TW, TH-, Yeni modeller, dikte etme, çağrı merkezi dökümü ve video dizin oluşturma senaryoları dahil olmak üzere birden çok etki alanı arasında önemli geliştirmeler getirir.

**Hata düzeltmeleri**
 - Sohbet 'in JAVA API 'Lerinde düzgün bir şekilde beklemediği düzeltilen hata düzeltildi 
 - Xamarin [GitHub sorunu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/363) için Android x86 öykünücü düzeltilmesi
 - Eksik Ekle (Get | Özellik yöntemlerini AudioConfig olarak ayarla
 - Bağlantı başarısız olduğunda audioDataStream 'in durdurulabileceği bir TTS hatasını çözme
 - Bölge olmadan bir uç nokta kullanılması, konuşma çevirmeni için USP hatalarının oluşmasına neden olur
 - Evrensel Windows uygulamalarında KIMLIK oluşturma artık uygun bir benzersiz GUID algoritması kullanıyor; daha önce ve yanlışlıkla büyük etkileşimler kümesi üzerinde çarpışmaları üreten bir saplaması uygulamasına varsayılan olarak ayarlanır.
 
 **Örnekler**
 - [Unity mikrofon ve gönderim modu akışı](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/unity/from-unitymicrophone) ile konuşma SDK 'sını kullanmak için Unity örneği

**Diğer değişiklikler**
 - [Linux için OpenSSL yapılandırma belgeleri güncelleştirildi](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-configure-openssl-linux)

## <a name="speech-sdk-190-2020-january-release"></a>Konuşma SDK 1.9.0:2020-Ocak sürümü

**Yeni özellikler**

- Çoklu cihaz konuşması: birden çok cihazı aynı konuşmaya veya metin tabanlı konuşmaya bağlayın ve isteğe bağlı olarak bunlar arasında gönderilen iletileri çevirin. [Bu makalede](multi-device-conversation.md)daha fazla bilgi edinin. 
- Android. AAR paketi için anahtar sözcük tanıma desteği eklendi ve x86 ve x64 türleri için destek eklendi. 
- Amaç-C: `Connection` nesnesine eklenen `SendMessage` ve `SetMessageProperty` yöntemleri. [Belgelere bakın](https://docs.microsoft.com/objectivec/cognitive-services/speech/spxconnection).
- TTS C++ API 'si artık `std::wstring` birleştirilmiş metin girişi olarak destekler, bu da bir WSTRING 'i SDK 'ya geçirmeden önce dizeye dönüştürme gereksinimini ortadan kaldırır. [Ayrıntılara bakın](https://docs.microsoft.com/cpp/cognitive-services/speech/speechsynthesizer#speaktextasync). 
- C#: [DIL kimliği](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-automatic-language-detection?pivots=programming-language-csharp) ve [kaynak dil yapılandırması](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-specify-source-language?pivots=programming-language-csharp) artık kullanılabilir.
- JavaScript: konuşma hizmetinden geri çağırma `receivedServiceMessage`olarak özel iletilerden geçirilecek `Connection` nesnesine bir özellik eklendi.
- JavaScript: şirket içi kapsayıcılar ve bağımsız bulutlar ile kullanımı kolay `FromHost API` için destek eklendi. [Belgelere bakın](speech-container-howto.md).
- JavaScript: artık [orgads](https://github.com/orgads)' dan bir katkıda teşekkürler `NODE_TLS_REJECT_UNAUTHORIZED`. [Ayrıntılara bakın](https://github.com/microsoft/cognitive-services-speech-sdk-js/pull/75).


**Son değişiklikler**

- `OpenSSL`, 1.1.1 b sürümüne güncelleştirilmiştir ve Linux için konuşma SDK 'Sı temel kitaplığına statik olarak bağlanır. Bu, gelen kutusu `OpenSSL` sistemdeki `/usr/lib/ssl` dizinine yüklenmemişse kesintiye neden olabilir. Bu sorunu geçici olarak çözmek için lütfen konuşma SDK 'Sı belgeleri altındaki [belgelerimizi](how-to-configure-openssl-linux.md) kontrol edin.
- `long` `int` `WordLevelTimingResult.Offset` için C# döndürülen veri türünü, konuşma verileri 2 dakikadan uzun olduğunda `WordLevelTimingResults` erişime izin verecek şekilde değiştirdik.
- `PushAudioInputStream` ve `PullAudioInputStream` artık, oluşturma sırasında belirtilen `AudioStreamFormat`bağlı olarak konuşma hizmetine WAV başlık bilgilerini gönderiyor. Müşterilerin artık [desteklenen ses girişi biçimini](how-to-use-audio-input-streams.md)kullanması gerekir. Diğer tüm biçimler, en uygun tanıma sonuçlarını alır veya başka sorunlara neden olabilir. 


**Hata düzeltmeleri**

- Yukarıdaki son değişiklikler altında `OpenSSL` güncelleştirmesine bakın. Linux ve Java 'da aralıklı bir kilitlenmeyi ve bir performans sorununu (yüksek yük altında kilit çakışması) düzelttik. 
- Java: yüksek eşzamanlılık senaryolarında nesne kapanışına yönelik iyileştirmeler yapıldı.
- NuGet paketimize yeniden yapılandırılmış. `Microsoft.CognitiveServices.Speech.core.dll` ve `Microsoft.CognitiveServices.Speech.extension.kws.dll` lib klasörlerinin altındaki üç kopyasını kaldırdık, NuGet paketini indirmek için daha küçük ve daha hızlı hale getirerek, bazı C++ yerel uygulamaları derlemek için gereken üst bilgileri ekledik.
- [Burada](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp)sabit hızlı başlangıç örnekleri. Linux, MacOS, Windows 'ta "mikrofon bulunamadı" özel durumu görüntülenmeden çıkılıyor.
- Uzun konuşma tanıma özelliği, [Bu örnek](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/csharp/uwp/speechtotext-uwp)gibi belirli kod yollarında sonuçları elde eden sabit SDK kilitlenmesi.
- [Bu müşteri sorununa](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/396)yönelik olarak Azure Web App ORTAMıNDA düzeltilen SDK dağıtım hatası.
- [Bu müşteri sorununa](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/433)yönelik olarak çoklu `<voice>` etiketi veya `<audio>` etiketi KULLANıLıRKEN bir TTS hatası düzeltildi. 
- SDK askıya alındı durumundan kurtarıldığı zaman TTS 401 hatası düzeltildi.
- JavaScript: [euirim](https://github.com/euirim)'ten bir katkı sayesinde ses verilerinin dairesel bir şekilde Içe aktarılması düzeltildi. 
- JavaScript: 1,7 ' de eklenen hizmet özelliklerini ayarlama desteği eklendi.
- JavaScript: bir bağlantı hatasının sürekli, başarısız WebSocket yeniden bağlanma girişimlerinde oluşmasına neden olabilecek bir sorun düzeltildi.


**Örnekler**

- Android için anahtar sözcük tanıma örneği [buraya](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/java/android/sdkdemo)eklendi.
- [Burada](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/speech_synthesis_server_scenario_sample.cs)sunucu senaryosu için TTS örneği eklendi.
- C# C++ [Ve için](quickstarts/multi-device-conversation.md)çok cihazlı görüşme hızlı başlangıçlarını eklendi.


**Diğer değişiklikler**

- Android 'de iyileştirilmiş SDK temel kitaplığı boyutu.
- 1\.9.0 ve sonraki sürümlerde SDK, konuşma dökümü için ses imza sürümü alanındaki `int` ve `string` türlerini destekler.

## <a name="speech-sdk-180-2019-november-release"></a>Konuşma SDK 1.8.0:2019-Kasım sürümü

**Yeni özellikler**

- Şirket içi kapsayıcılar ve bağımsız bulutlar ile kullanım kolaylığı sağlamak için bir `FromHost()` API 'SI eklenmiştir.
- Konuşma tanıma için otomatik kaynak Dil Algılama eklendi (Java ve C++)
- Beklenen kaynak dillerini (Java ve C++) belirtmek Için kullanılan konuşma tanıma için `SourceLanguageConfig` nesnesi eklendi
- NuGet ve Unity paketleri aracılığıyla Windows (UWP), Android ve iOS 'ta `KeywordRecognizer` desteği eklendi
- Zaman uyumsuz toplu işlemlerdeki konuşma dökümü için uzak konuşma Java API 'SI eklendi.

**Son değişiklikler**

- Konuşma taşıma işlevleri ad alanı `Microsoft.CognitiveServices.Speech.Transcription`altına taşındı.
- Görüşmenin bir parçası olan yöntemler yeni `Conversation` sınıfına taşınır.
- 32-bit (ARMv7 ve x86) iOS için bırakılan destek

**Hata düzeltmeleri**

- Yerel `KeywordRecognizer`, geçerli bir konuşma hizmeti abonelik anahtarı olmadan kullanılıyorsa kilitlenme için çözüm yapın

**Örnekler**

- `KeywordRecognizer` için Xamarin örneği
- `KeywordRecognizer` için Unity örneği
- C++ve otomatik kaynak Dil Algılama için Java örnekleri.

## <a name="speech-sdk-170-2019-september-release"></a>Konuşma SDK 1.7.0:2019-Eylül sürümü

**Yeni özellikler**

- Xamarin on Evrensel Windows Platformu (UWP), Android ve iOS için Beta desteği eklendi
- Unity için iOS desteği eklendi
- ALaw, Mulaw, Android üzerinde FLAC, iOS ve Linux için `Compressed` giriş desteği eklendi
- Hizmete ileti göndermek için `Connection` sınıfında `SendMessageAsync` eklendi
- Bir iletinin özelliğini ayarlamak için `Connection` sınıfında `SetMessageProperty` eklendi
- Java (JRE ve Android), Python, Swift ve amaç-C için TTS eklendi bağlamaları
- TTS, macOS, iOS ve Android için kayıttan yürütme desteği ekledi.
- TTS için "sözcük sınırı" bilgileri eklendi.

**Hata düzeltmeleri**

- Android için Unity 2019 üzerinde IL2CPP derleme sorunu düzeltildi
- WAV dosya girişi hatalı şekilde işlenmekte olan hatalı biçimlendirilmiş üst bilgilerdeki sorun düzeltildi
- Bazı bağlantı özelliklerinde benzersiz olmayan UUID 'ler ile ilgili sorun düzeltildi
- Swift bağlamalarında null değer alabilme belirticileri hakkında birkaç uyarı düzeltildi (küçük kod değişiklikleri gerektirebilir)
- WebSocket bağlantılarına neden olan bir hata düzeltildi ağ yükü altında düzgün şekilde kapatılacak
- Android 'de, bazen `DialogServiceConnector` tarafından kullanılan yinelenen izlenme kimliklerine neden olan bir sorun düzeltildi
- Birden çok yönlü etkileşimler genelinde bağlantı kararlılığı ve hataların raporlanması (`Canceled` olaylar aracılığıyla) `DialogServiceConnector`
- `DialogServiceConnector` oturum başlatılır, etkin bir `StartKeywordRecognitionAsync()` sırasında `ListenOnceAsync()` çağrılırken de dahil olmak üzere artık olayları doğru şekilde sağlayacak
- Alınmakta olan `DialogServiceConnector` etkinlikleriyle ilişkili bir kilitlenme olduğunu biliyor

**Örnekler**

- Xamarin için hızlı başlangıç
- Linux ARM64 Information ile güncelleştirilmiş CPP hızlı başlangıcı
- İOS bilgileriyle Unity hızlı başlangıç güncelleştirildi

## <a name="speech-sdk-160-2019-june-release"></a>Konuşma SDK 1.6.0:2019-Haziran sürümü

**Örnekler**

- UWP ve Unity 'de metin okuma için hızlı başlangıç örnekleri
- İOS 'ta Swift için hızlı başlangıç örneği
- Konuşma & Amaç Tanıma ve çevirisi için Unity örnekleri
- `DialogServiceConnector` için hızlı başlangıç örnekleri güncelleştirildi

**Geliştirmeler/değişiklikler**

- İletişim kutusu ad alanı:
  - `SpeechBotConnector`, `DialogServiceConnector` olarak yeniden adlandırıldı
  - `BotConfig`, `DialogServiceConfig` olarak yeniden adlandırıldı
  - `BotConfig::FromChannelSecret()` `DialogServiceConfig::FromBotSecret()` yeniden eşlendi
  - Var olan tüm doğrudan hat konuşma istemcileri yeniden adlandırmadan sonra desteklenmeye devam eder
- Proxy, kalıcı bağlantıyı destekleyecek şekilde TTS REST bağdaştırıcısını güncelleştirme
- Geçersiz bir bölge geçirildiğinde hata iletisini geliştir
- Swift/amaç-C:
  - Geliştirilmiş hata raporlama: hata ile sonuçlanan Yöntemler Şu anda iki sürümde mevcuttur: hata işleme için bir `NSError` nesnesi ve bir özel durum Başlatan bir nesne. İlki Swift 'a açıktır. Bu değişiklik, mevcut Swift koduna uyarlamalar gerektirir.
  - Geliştirilmiş olay işleme

**Hata düzeltmeleri**

- TTS için çözüm: sesin işleme tamamlanana kadar beklemeden `SpeakTextAsync` gelecekte döndürüldüğü
- Tam dil desteğini etkinleştirmek için C# içinde sıralama dizelerini onarma
- Örneklerde net461 Target Framework ile çekirdek kitaplığı yükleme ile ilgili .NET Core uygulama sorunu için çözüm
- Örneklerindeki çıkış klasörüne yerel kitaplıkları dağıtmaya yönelik zaman zaman sorun giderme
- Güvenli bir şekilde Web yuvası kapatma için çözüm
- Linux üzerinde çok ağır yük altında bir bağlantı açılırken olası kilitlenme için çözüm
- MacOS için çerçeve paketinde eksik meta veriler için çözüm
- Windows üzerinde `pip install --user` sorunları giderme

## <a name="speech-sdk-151"></a>Konuşma SDK 1.5.1

Bu bir hata çözme sürümüdür ve yalnızca yerel/yönetilen SDK 'yi etkiliyor. SDK 'nın JavaScript sürümünü etkilemiyor.

**Hata düzeltmeleri**

- Konuşma dökümü ile kullanıldığında FromSubscription 'ı onarın.
- Sesli Yardımcılar için anahtar sözcük biriktirmesiyle ilgili hatayı düzeltir.

## <a name="speech-sdk-150-2019-may-release"></a>Konuşma SDK 1.5.0:2019-yayın Mayıs

**Yeni özellikler**

- Anahtar sözcük biriktirme (KWS) artık Windows ve Linux için kullanılabilir. KWS işlevselliği herhangi bir mikrofon türü ile çalışabilir, ancak resmi KWS desteği şu anda Azure Kinect DK donanımında veya konuşma cihazları SDK 'sında bulunan mikrofon dizileri ile sınırlıdır.
- Tümcecik ipucu işlevselliği SDK aracılığıyla kullanılabilir. Daha fazla bilgi için [buraya](how-to-phrase-lists.md)bakın.
- Konuşma dökümü işlevselliği SDK aracılığıyla kullanılabilir. [Buraya](conversation-transcription-service.md)bakın.
- Doğrudan hat konuşma kanalını kullanarak sesli Yardımcılar için destek ekleyin.

**Örnekler**

- Yeni özellikler veya SDK tarafından desteklenen yeni hizmetler için örnek eklendi.

**Geliştirmeler/değişiklikler**

- Hizmet davranışını veya hizmet sonuçlarını (maskeleme küfür ve diğerleri gibi) ayarlamak için çeşitli tanıyıcı özellikleri eklendi.
- Artık tanıyıcı `FromEndpoint`oluşturmuş olsanız bile, tanıyıcıyı standart yapılandırma özellikleri aracılığıyla yapılandırabilirsiniz.
- Hedef-C: `OutputFormat` Özellik `SPXSpeechConfiguration`eklendi.
- SDK artık Linux dağıtımı olarak 9.

**Hata düzeltmeleri**

- Konuşmacı kaynağının metinden konuşmaya çok erken bir yere çıkarılması sorunu düzeltildi.

## <a name="speech-sdk-142"></a>Konuşma SDK 1.4.2

Bu bir hata çözme sürümüdür ve yalnızca yerel/yönetilen SDK 'yi etkiliyor. SDK 'nın JavaScript sürümünü etkilemiyor.

## <a name="speech-sdk-141"></a>Konuşma SDK 1.4.1

Bu yalnızca bir JavaScript sürümüdür. Hiçbir özellik eklenmedi. Aşağıdaki düzeltmeler yapılmıştır:

- Web paketinin HTTPS-proxy-Agent yüklemesini engelleyin.

## <a name="speech-sdk-140-2019-april-release"></a>Konuşma SDK 1.4.0:2019-Nisan sürümü

**Yeni özellikler**

- SDK artık beta sürümü olarak metinden konuşmaya hizmetini desteklemektedir. Ve ' C++ C#den Windows ve Linux Desktop 'ta desteklenir. Daha fazla bilgi için, [metinden konuşmaya genel bakış ' a](text-to-speech.md#get-started)bakın.
- SDK artık MP3 ve Opus/OGG ses dosyalarını akış giriş dosyaları olarak desteklemektedir. Bu özellik yalnızca Linux C++ üzerinde kullanılabilir C# ve şu anda beta aşamasındadır (daha ayrıntılı bilgi). [](how-to-use-codec-compressed-audio-input-streams.md)
- Java, .NET Core C++ ve amaç-C için konuşma SDK 'Sı, MacOS desteği elde etmiş. MacOS için amaç-C desteği şu anda beta aşamasındadır.
- iOS: iOS için konuşma SDK 'Sı (hedef-C) artık bir CocoaPod olarak yayımlanmıştır.
- JavaScript: giriş cihazı olarak varsayılan olmayan mikrofonu destekler.
- JavaScript: node. js için proxy desteği.

**Örnekler**

- MacOS üzerinde WITH C++ hedefi-C ile ve ile konuşma SDK 'sını kullanmaya yönelik örnekler eklenmiştir.
- Metin okuma hizmetinin kullanımını gösteren örnekler eklenmiştir.

**Geliştirmeler/değişiklikler**

- Python: tanıma sonuçlarının ek özellikleri artık `properties` özelliği aracılığıyla kullanıma sunulmuştur.
- Ek geliştirme ve hata ayıklama desteği için, SDK günlüğü ve tanılama bilgilerini bir günlük dosyasına yeniden yönlendirebilir ( [burada](how-to-use-logging.md)daha fazla bilgi bulabilirsiniz).
- JavaScript: ses işleme performansını geliştirme.

**Hata düzeltmeleri**

- Mac/iOS: konuşma hizmetine bir bağlantı kurulamazsa uzun bir beklemeye yol gösteren bir hata düzeltildi.
- Python: Python geri çağırmalar içindeki bağımsız değişkenler için hata işlemeyi geliştir.
- JavaScript: RequestSession üzerinde konuşma için yanlış durum bildirimi düzeltildi.

## <a name="speech-sdk-131-2019-february-refresh"></a>Konuşma SDK 1.3.1:2019-Şubat yenilemesi

Bu bir hata çözme sürümüdür ve yalnızca yerel/yönetilen SDK 'yi etkiliyor. SDK 'nın JavaScript sürümünü etkilemiyor.

**Hata onarımı**

- Mikrofon girişi kullanılırken bellek sızıntısı düzeltildi. Akış tabanlı veya dosya girişi etkilenmez.

## <a name="speech-sdk-130-2019-february-release"></a>Konuşma SDK 1.3.0:2019-Şubat yayını

**Yeni özellikler**

- Konuşma SDK 'Sı, `AudioConfig` sınıfı aracılığıyla giriş mikrofonunun seçimini destekler. Bu, konuşma hizmetine varsayılan olmayan bir mikrofondan ses verileri akışını sağlar. Daha fazla bilgi için bkz. [ses girişi cihaz seçimini](how-to-select-audio-input-devices.md)açıklayan belgeler. Bu özellik JavaScript 'te henüz kullanılamaz.
- Konuşma SDK 'Sı artık bir beta sürümünde Unity 'yi desteklemektedir. [GitHub örnek deposundaki](https://aka.ms/csspeech/samples)sorun bölümüne geri bildirim sağlayın. Bu sürüm, Windows x86 ve x64 (masaüstü veya Evrensel Windows Platformu uygulamaları) ve Android (ARM32/64, x86) üzerinde Unity 'yi destekler. [Unity hızlı](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=unity)başlangıçmızda daha fazla bilgi bulabilirsiniz.
- `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` dosya (önceki sürümlerde sunulan) artık gerekli değildir. İşlevsellik artık temel SDK ile tümleşiktir.

**Örnekler**

[Örnek depoımızda](https://aka.ms/csspeech/samples)aşağıdaki yeni içerik mevcuttur:

- `AudioConfig.FromMicrophoneInput`için ek örnekler.
- Amaç tanıma ve çeviri için ek Python örnekleri.
- İOS 'ta `Connection` nesnesini kullanmak için ek örnekler.
- Ses çıkışıyla çeviri için ek Java örnekleri.
- [Toplu Iş dökümü REST API](batch-transcription.md)kullanımı için yeni örnek.

**Geliştirmeler/değişiklikler**

- Python
  - `SpeechConfig`'de geliştirilmiş parametre doğrulama ve hata iletileri.
  - `Connection` nesnesi için destek ekleyin.
  - Windows üzerinde 32-bit Python (x86) için destek.
  - Python için konuşma SDK 'Sı beta dışında.
- iOS
  - SDK artık iOS SDK 12,1 sürümüne göre oluşturulmuştur.
  - SDK artık iOS sürümleri 9,2 ve üstünü desteklemektedir.
  - Başvuru belgelerini geliştirir ve birçok özellik adını düzeltir.
- JavaScript
  - `Connection` nesnesi için destek ekleyin.
  - Paketlenmiş JavaScript için tür tanımı dosyaları ekleme
  - Tümcecik ipuçları için başlangıç desteği ve uygulama.
  - Tanıma için Service JSON ile Özellikler koleksiyonunu döndürün
- Windows dll 'Leri artık bir sürüm kaynağı içeriyor.
- Bir tanıyıcı oluşturursanız `FromEndpoint` doğrudan uç nokta URL 'sine parametreler ekleyebilirsiniz. `FromEndpoint` kullanarak, tanıyıcıyı standart yapılandırma özellikleri aracılığıyla yapılandıramazsınız.

**Hata düzeltmeleri**

- Boş Proxy Kullanıcı adı ve proxy parolası doğru işlenmedi. Bu sürümle birlikte, Proxy Kullanıcı adı ve proxy parolasını boş bir dizeye ayarlarsanız, ara sunucuya bağlanılırken gönderilmez.
- SDK tarafından oluşturulan SessionID, bazı diller&nbsp;/ortamları için her zaman gerçekten gelişigüzel değildir. Bu sorunu giderecek rastgele Oluşturucu başlatması eklendi.
- Yetkilendirme belirtecinin işlenmesini geliştirme. Bir yetkilendirme belirteci kullanmak istiyorsanız, `SpeechConfig` belirtin ve abonelik anahtarını boş bırakın. Ardından tanıyıcı her zamanki gibi oluşturun.
- Bazı durumlarda `Connection` nesnesi doğru bir şekilde yayınlanmadı. Bu sorun düzeltilmiştir.
- JavaScript örneği, çeviri birleştirmesinin ses çıkışını desteklemek için de Safari 'de düzeltildi.

## <a name="speech-sdk-121"></a>Konuşma SDK 'Sı 1.2.1

Bu yalnızca bir JavaScript sürümüdür. Hiçbir özellik eklenmedi. Aşağıdaki düzeltmeler yapılmıştır:

- Stream. End sırasında değil, Stream 'in sonuna kadar bir akış sonu tetiklendir.
- Geçerli gönderme başarısız olduysa, sonraki gönderimi zamanlamadığı ses göndericisinin hatasını düzeltir.
- Kimlik doğrulama belirteciyle sürekli tanımayı düzeltir.
- Farklı tanıyıcı/uç noktalar için hata düzeltildi.
- Belgeleri geliştirmeleri.

## <a name="speech-sdk-120-2018-december-release"></a>Konuşma SDK 1.2.0:2018-Aralık yayını

**Yeni özellikler**

- Python
  - Python desteğinin beta sürümü (3,5 ve üzeri) Bu sürümde kullanılabilir. Daha fazla bilgi için buraya bakın] (hızlı başlangıç-python.md).
- JavaScript
  - JavaScript için konuşma SDK 'Sı açık kaynaklıdır. Kaynak kodu [GitHub](https://github.com/Microsoft/cognitive-services-speech-sdk-js)' da kullanılabilir.
  - Artık Node. js ' yi destekliyoruz, [burada](quickstart-js-node.md)daha fazla bilgi bulabilirsiniz.
  - Ses oturumları için uzunluk kısıtlaması kaldırılmıştır, otomatik olarak yeniden bağlanma işlemi, kapak altında otomatik olarak gerçekleşir.
- `Connection` nesnesi
  - `Recognizer`, bir `Connection` nesnesine erişebilirsiniz. Bu nesne, hizmet bağlantısını açık bir şekilde başlatabilmeniz ve olaylara bağlanıp bağlantıyı kesmeye abone olmanızı sağlar.
    (Bu özellik JavaScript ve Python 'da henüz kullanılamaz.)
- Ubuntu 18,04 desteği.
- Android
  - APK oluşturma sırasında ProGuard desteği etkinleştirildi.

**Geliştirmeleri**

- İç iş parçacığı kullanımındaki geliştirmeler, iş parçacığı sayısı, kilitler, zaman uyumu sağlayıcılar sayısını azaltır.
- Geliştirilmiş hata raporlama/bilgi. Birçok durumda, hata iletileri hiç bir şekilde yayılmaz.
- Güncel modüller kullanmak için JavaScript 'teki geliştirme bağımlılıkları güncelleştirildi.

**Hata düzeltmeleri**

- `RecognizeAsync`bir tür uyumsuzluğu nedeniyle sabit bellek sızıntıları.
- Bazı durumlarda özel durumlar sızdırılmıştı.
- Çeviri olay bağımsız değişkenlerinde bellek sızıntısı düzeltiliyor.
- Uzun süre çalışan oturumlarda yeniden bağlantı sırasında bir kilitleme sorunu düzeltildi.
- Başarısız çeviriler için eksik nihai sonuca neden olabilecek bir sorun düzeltildi.
- C#: `async` bir işlem ana iş parçacığında beklenmediyse, zaman uyumsuz görev tamamlanmadan önce tanıyıcı atılamamalıdır.
- Java: bir sorun düzeltildi Java VM kilitlenmesiyle sonuçlanır.
- Amaç-C: sabit enum eşlemesi; `RecognizingIntent`yerine RecognizedIntent döndürüldü.
- JavaScript: `SpeechConfig`içinde varsayılan çıkış biçimini ' Simple ' olarak ayarlayın.
- JavaScript: JavaScript ve diğer dillerdeki yapılandırma nesnesindeki özellikler arasında tutarsızlık kaldırılıyor.

**Örnekler**

- Birkaç örnek güncelleştirildi ve düzeltildi (örneğin, çeviri için çıkış sesleriniz vb.).
- [Örnek depoya](https://aka.ms/csspeech/samples)Node. js örnekleri eklendi.

## <a name="speech-sdk-110"></a>Konuşma SDK 1.1.0

**Yeni özellikler**

- Android x86 x64 desteği.
- Proxy desteği: `SpeechConfig` nesnesinde, artık proxy bilgilerini (konak adı, bağlantı noktası, Kullanıcı adı ve parola) ayarlamak için bir işlev çağırabilirsiniz. Bu özellik henüz İos'ta kullanılabilir değil.
- Geliştirilmiş hata kodu ve iletileri. Bir tanıma hata döndürmediğinde bu, zaten `Reason` (iptal edildi olayında) veya `CancellationDetails` (tanınma sonucunda) `Error`olarak ayarlanmıştır. İptal edilen olay artık iki ek üye içerir, `ErrorCode` ve `ErrorDetails`. Sunucu ek hata bilgileri ile bildirilen hata döndürdüyse, artık yeni üyeleri kullanılabilir olacaktır.

**Geliştirmeleri**

- Eklenen ek doğrulama tanıyıcı yapılandırma ve eklenen ek hata iletisi.
- Uzun süreli sessizlik ortasında bir ses dosyasının işlenmesinde'yi tıklatın.
- NuGet paketi: isteğe bağlı olarak .NET Framework projeleri için yapı AnyCPU yapılandırmayla engeller.

**Hata düzeltmeleri**

- Tanıyıcılar işlem bulunan birkaç özel durum düzeltildi. Ayrıca, özel durumlar yakalanıp `Canceled` olayına dönüştürülür.
- Özellik Yönetimi'nde bir bellek sızıntısı düzeltin.
- Ses giriş dosyası tanıyıcı kilitlenebiliyordu hata düzeltildi.
- Olayları sonra oturumu durdurulduğunda burada alınan bir hata düzeltildi.
- İş parçacığı içinde bazı yarış durumları düzeltildi.
- Bir iOS bir çökmesine neden neden olabilecek bir uyumluluk sorunu düzeltildi.
- Android mikrofon desteği kararlılık geliştirmeleri.
- Javascript'teki bir tanıyıcı tanıma dil yok burada bir hata düzeltildi.
- JavaScript 'teki `EndpointId` (bazı durumlarda) ayarlanmasını engellediği bir hata düzeltildi.
- JavaScript 'te Addamaciçinde parametre sırası değiştirildi ve eksik `AddIntent` JavaScript imzası eklendi.

**Örnekler**

- C++ [Örnek depoda](https://aka.ms/csspeech/samples)çekme ve gönderme akışı kullanımı için ve C# örnekleri eklendi.

## <a name="speech-sdk-101"></a>Konuşma SDK 1.0.1

Güvenilirlik geliştirmeleri ve hata düzeltmeleri:

- Disposing tanıyıcı yarış durumu nedeniyle olası sabit önemli hata
- Ayarlama özellikleri durumunda sabit olası önemli hata.
- Eklenen ek hata ve parametre denetimi.
- İçinde NSString adı geçersiz kılma nedeni objective-C: sabit olası önemli hata oluştu.
- API görünürlüğünü objective-C: ayarlandı
- JavaScript: olaylar ve yükleri düzeltildi.
- Belgeleri geliştirmeleri.

[Örnek depoımızda](https://aka.ms/csspeech/samples)JavaScript için yeni bir örnek eklenmiştir.

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>Bilişsel hizmetler konuşma SDK 1.0.0: Eylül 2018 sürüm

**Yeni özellikler**

- İOS üzerinde Objective-C için destek. [İOS Için hedef C hızlı başlangıç](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone-langs/objectivec-ios.md)öğreticimize göz atın.
- Tarayıcıda JavaScript desteği. [JavaScript hızlı](quickstart-js-browser.md)başlangıçmıza göz atın.

**Son değişiklikler**

- Bu sürümle birlikte, bir dizi Son değişiklik yapılmıştır.
  Ayrıntılar için [Bu sayfaya](https://aka.ms/csspeech/breakingchanges_1_0_0) bakın.

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>Bilişsel hizmetler konuşma SDK 0.6.0: Ağustos 2018 sürüm

**Yeni özellikler**

- UWP uygulamaları artık yerleşik olarak konuşma SDK'sı ile Windows uygulama sertifikası Kiti (WACK) geçirebilirsiniz.
  [UWP hızlı](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp&tabs=uwp)başlangıcı ' nı inceleyin.
- .NET Standard 2.0 Linux'ta (Ubuntu 16.04 x 64) için destek.
- Deneysel: Java 8 (64-bit) Windows ve Linux (Ubuntu 16.04 x 64) desteği.
  [Java Runtime Environment hızlı](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=jre)başlangıcı ' nı inceleyin.

**İşlevsel değişiklik**

- Ek hata bağlantı hatalarıyla ilgili ayrıntılı bilgi kullanıma sunar.

**Son değişiklikler**

- Java 'da (Android) `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` işlevi artık bir yol parametresi gerektirmez. Artık bir yolu, tüm desteklenen platformlarda otomatik olarak algılanır.
- Özelliğin get-erişimcisi Java 'da `EndpointUrl` ve C# kaldırılmıştır.

**Hata düzeltmeleri**

- Java dilinde çeviri tanıyıcı ses sentezi sonucuna artık uygulanır.
- Etkin olmayan iş parçacıkları ve açık ve kullanılmayan yuva sayısının artması neden olabilecek bir hata düzeltildi.
- Uzun süreli tanıma ortasında iletim burada sonlandırmak bir sorun düzeltildi.
- Tanıyıcı kapatma bir yarış durumu düzeltildi.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Bilişsel hizmetler konuşma SDK buradan 0.5.0 sürümünü: Temmuz 2018 sürüm

**Yeni özellikler**

- Android platform desteği (API 23: Android 6.0 Marshmallow veya üzeri). [Android hızlı başlangıç](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=android)' a göz atın.
- Windows üzerinde .NET Standard 2.0 desteği. [.NET Core hızlı başlangıç](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnetcore)' i inceleyin.
- Deneysel: Destek UWP üzerinde Windows (sürüm 1709 veya üzeri).
  - [UWP hızlı](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp)başlangıcı ' nı inceleyin.
  - Not: Windows uygulama sertifikası Kiti (WACK) Speech SDK'sı ile oluşturulan UWP uygulamaları henüz geçmeyin.
- Otomatik yeniden bağlanma ile uzun süreli tanıma destekler.

**İşlevsel değişiklikler**

- `StartContinuousRecognitionAsync()` uzun süre çalışan tanımayı destekler.
- Tanıma işleminin sonucu daha fazla alan içeriyor. Bunlar, tanınan metnin ses başlangıcından ve süresinden (her ikisi de Tick) ve tanınma durumunu temsil eden ek değerlerin (örneğin, `InitialSilenceTimeout` ve `InitialBabbleTimeout`) denkleştirilir.
- AuthorizationToken factory örnekleri oluşturmak için destek.

**Son değişiklikler**

- Tanıma olayları: `NoMatch` olay türü `Error` olayında birleştirildi.
- C# SpeechOutputFormat, ile C++hizalı kalmak için `OutputFormat` olarak yeniden adlandırıldı.
- `AudioInputStream` arabirimindeki bazı yöntemlerin dönüş türü biraz değişti:
  - Java 'da `read` yöntemi artık `int`yerine `long` döndürüyor.
  - ' C#De `Read` yöntemi artık `int`yerine `uint` döndürüyor.
  - ' C++De, `Read` ve `GetFormat` yöntemleri artık `int`yerine `size_t` döndürüyor.
- C++: Ses giriş akışlarının örnekleri artık yalnızca bir `shared_ptr`olarak geçirilebilir.

**Hata düzeltmeleri**

- `RecognizeAsync()` zaman aşımı olduğunda sonuçdaki hatalı dönüş değerleri düzeltildi.
- Windows media foundation kitaplıkları bağımlılığı kaldırıldı. SDK'sı artık çekirdek ses API'leri kullanır.
- Belge onarımı: desteklenen bölgeleri anlatmak için bir [bölgeler](regions.md) sayfası eklendi.

**Bilinen sorun**

- Android Speech SDK'sı için çeviri konuşma sentezi sonuçları bildirmez. Bu sorun, sonraki sürümde düzeltilecektir.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Bilişsel hizmetler konuşma SDK 0.4.0: Haziran 2018'den sürüm

**İşlevsel değişiklikler**

- AudioInputStream

  Bir tanıyıcı, artık bir akışı ses kaynağı olarak tüketebilir. Daha fazla bilgi için ilgili [nasıl yapılır Kılavuzu](how-to-use-audio-input-streams.md)' na bakın.

- Ayrıntılı çıkış biçimi

  Bir `SpeechRecognizer`oluşturduğunuzda, `Detailed` veya `Simple` çıkış biçimi isteyebilirsiniz. `DetailedSpeechRecognitionResult`, maskeli bir küfür ile Güvenirlik puanı, tanınan metin, ham sözlü form, normalleştirilmiş form ve normalleştirilmiş form içerir.

**Son değişiklik**

- İçindeki C#`SpeechRecognitionResult.RecognizedText` `SpeechRecognitionResult.Text` olarak değiştirildi.

**Hata düzeltmeleri**

- Olası bir geri çağırma sorunu USP katmanda kapatılırken düzeltildi.
- Ses giriş dosyası bir tanıyıcı kullanılan, onu için gerekenden daha uzun dosya tanıtıcısı bulunduran.
- İleti pompası tanıyıcı arasındaki birkaç kilitlenmeleri kaldırıldı.
- Hizmetten gelen yanıt zaman aşımına uğradığından `NoMatch` bir sonuç tetikle.
- Windows media foundation kitaplıklarındaki Gecikmeli yüklendi ' dir. Bu kitaplık, yalnızca giriş mikrofon için gereklidir.
- Karşıya yükleme hızı için ses verilerini iki kez özgün ses hızı hakkında sınırlıdır.
- Windows üzerinde C# .NET derlemeleri artık strong adlandırılır.
- Belge onarımı: `Region` tanıyıcı oluşturmak için gerekli bilgiler.

Daha fazla örnek eklenmiştir ve sürekli olarak güncelleştiriliyor. En son örnek kümesi için bkz. [konuşma SDK örnekleri GitHub deposu](https://aka.ms/csspeech/samples).

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Bilişsel hizmetler konuşma SDK 0.2.12733: Mayıs 2018 sürüm

Bu sürüm ilk Bilişsel hizmetler konuşma SDK'sı genel Önizleme sürümüdür.

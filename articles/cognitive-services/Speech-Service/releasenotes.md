---
title: Yayın Notları - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma Hizmeti özelliğinin çalışan günlüğü sürümleri, geliştirmeler, hata düzeltmeleri ve bilinen sorunlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219648"
---
# <a name="release-notes"></a>Sürüm notları
## <a name="speech-sdk-1100-2020-february-release"></a>Konuşma SDK 1.10.0: 2020-Şubat açıklaması

**Yeni özellikler**
 - Python'un yeni 3.8 sürümüne destek olmak için Python paketleri eklendi.
 - Red Hat Enterprise Linux (RHEL)/CentOS 8 x64 desteği (C++, C#, Java, Python).
   > [!NOTE] 
   > Müşteriler OpenSSL'yi [bu talimatlara](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-configure-openssl-linux)göre yapılandırmalıdır.
 - Debian ve Ubuntu için Linux ARM32 desteği.
 - DialogServiceConnector artık BotFrameworkConfig'de isteğe bağlı bir "bot kimliği" parametresi destekler. Bu parametre, tek bir Azure konuşma kaynağına sahip birden çok Doğrudan Satır Konuşması botunun kullanılmasına izin verir. Parametre belirtilmeden varsayılan bot (Doğrudan Hat Konuşma kanalı yapılandırma sayfası tarafından belirlenir) kullanılır.
 - DialogServiceConnector artık bir SpeechActivityTemplate özelliğine sahiptir. Bu JSON dizesinin içeriği, konuşma tanıma gibi olaylara yanıt olarak otomatik olarak oluşturulan etkinlikler de dahil olmak üzere, Doğrudan Satır Konuşma botuna ulaşan tüm etkinliklerde desteklenen çok çeşitli alanları önceden doldurmak için Direct Line Speech tarafından kullanılacaktır.
 - TTS artık kimlik doğrulama için abonelik anahtarını kullanır ve bir synthesizer oluşturduktan sonra ilk sentez sonucunun ilk bayt gecikmesini azaltır.
 - Ortalama sözcük hata oranı %18,6 oranında azaltılması için 19 yerel bölge için güncelleştirilmiş konuşma tanıma modelleri (es-ES, es-MX, fr-CA, fr-FR, it-IT, ja-JP, ko-KR, pt-BR, zh-CN, zh-HK, nb-NO, fi-FL, ru-RU, pl-PL, ca-ES, zh-TW, th-TH, pt-pt-TR) Yeni modeller Dikte, Çağrı Merkezi Transkripsiyon ve Video Dizini senaryoları da dahil olmak üzere birden fazla etki alanında önemli iyileştirmeler getiriyor.

**Hata düzeltmeleri**
 - Konuşma Transkripsiyonu JAVA API'lerinde düzgün beklemediği hata düzeltildi 
 - Xamarin [GitHub sorunu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/363) için Android x86 emülatör düzeltme
 - Eksik ekle (Get| Set)AudioConfig özellik yöntemleri
 - Bağlantı kesildiğinde audioDataStream'in durdurulamadığı bir TTS hatasını düzeltme
 - Bölge olmadan bir bitiş noktası kullanmak, konuşma çevirmeni için USP hatalarına neden olur
 - Evrensel Windows Uygulamaları'nda kimlik oluşturma artık uygun benzersiz bir GUID algoritması kullanır; daha önce ve istemeden genellikle etkileşimleri büyük setleri üzerinde çakışma üretilen bir saptırılmış bir uygulama için varsayılan.
 
 **Örnekler**
 - [Unity mikrofon ve itme modu akışı](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/unity/from-unitymicrophone) ile Konuşma SDK kullanmak için birlik örneği

**Diğer değişiklikler**
 - [Linux için güncellenen OpenSSL yapılandırma belgeleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-configure-openssl-linux)

## <a name="speech-sdk-190-2020-january-release"></a>Konuşma SDK 1.9.0: 2020-Ocak açıklaması

**Yeni Özellikler**

- Çok aygıtlı konuşma: Birden çok aygıtı aynı konuşmaya veya metin tabanlı konuşmaya bağlayın ve isteğe bağlı olarak aralarında gönderilen iletileri çevirin. [Bu makalede](multi-device-conversation.md)daha fazla bilgi edinin. 
- Android .aar paketi için anahtar kelime tanıma desteği eklendi ve x86 ve x64 tatları için destek eklendi. 
- Amaç-C: `SendMessage` `SetMessageProperty` ve nesneye `Connection` eklenen yöntemler. [Burada](https://docs.microsoft.com/objectivec/cognitive-services/speech/spxconnection)belgelere bakın.
- TTS C++ api `std::wstring` şimdi sentez metin girişi olarak destekler, SDK geçmeden önce bir string bir wstring dönüştürmek için ihtiyaç kaldırarak. [Ayrıntılara buradan](https://docs.microsoft.com/cpp/cognitive-services/speech/speechsynthesizer#speaktextasync)bakın. 
- C#: [Dil kimliği](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-automatic-language-detection?pivots=programming-language-csharp) ve kaynak dil [config](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-specify-source-language?pivots=programming-language-csharp) artık kullanılabilir.
- JavaScript: Konuşma Hizmetinden gelen özel iletileri geri `Connection` `receivedServiceMessage`çağırma olarak aktarmak için nesneye bir özellik eklendi.
- JavaScript: Prem `FromHost API` kapları ve egemen bulutlarla kullanımı kolaylaştırmak için destek eklendi. [Burada](speech-container-howto.md)belgelere bakın.
- JavaScript: Şimdi `NODE_TLS_REJECT_UNAUTHORIZED` [orgads](https://github.com/orgads)bir katkı sayesinde onur . [Ayrıntılara buradan](https://github.com/microsoft/cognitive-services-speech-sdk-js/pull/75)bakın.


**Yeni değişiklikler**

- `OpenSSL`sürüm 1.1.1b olarak güncellenmiştir ve Linux için Konuşma SDK çekirdek kitaplığına statik olarak bağlıdır. Gelen kutunuz `OpenSSL` sistemdeki `/usr/lib/ssl` dizine yüklenmediyse, bu bir mola verebilir. Sorunu çözmek için lütfen Konuşma SDK dokümanları altındaki [belgelerimizi](how-to-configure-openssl-linux.md) kontrol edin.
- C# `WordLevelTimingResult.Offset` `int` için döndürülen veri türünü, `long` konuşma verilerinin `WordLevelTimingResults` 2 dakikadan uzun olduğu zamana erişime izin vermek için değiştirdik.
- `PushAudioInputStream`ve `PullAudioInputStream` şimdi wav üstbilgisini, oluşturulduklarında `AudioStreamFormat`isteğe bağlı olarak belirtilen konuşma hizmetine gönderin. Müşteriler artık [desteklenen ses giriş biçimini](how-to-use-audio-input-streams.md)kullanmalıdır. Diğer biçimler en uygun un altında tanıma sonuçları alır veya başka sorunlara neden olabilir. 


**Hata düzeltmeleri**

- Yukarıdaki `OpenSSL` Breaking değişiklikleri altında güncelleştirmeye bakın. Linux ve Java'da hem aralıklı bir çökme yitirme hem de bir performans sorununu (yüksek yük altında kilit anlaşmazlığı) düzelttik. 
- Java: Yüksek eşzamanlılık senaryolarında nesne kapatma için iyileştirmeler yaptı.
- NuGet paketimizi yeniden yapılandırdık. Lib klasörlerinin üç `Microsoft.CognitiveServices.Speech.core.dll` kopyasını `Microsoft.CognitiveServices.Speech.extension.kws.dll` ve altında nüget paketini daha küçük ve daha hızlı indirmeyi sağlamak için kaldırdık ve bazı C++ yerel uygulamalarını derlemek için gereken başlıkları ekledik.
- [Burada](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp)sabit quickstart örnekleri . Bunlar Linux, MacOS, Windows'da "mikrofon bulunamadı" istisnası görüntülemeden çıkıyordu.
- [Bu örnek](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/csharp/uwp/speechtotext-uwp)gibi belirli kod yollarında uzun konuşma tanıma sonuçları ile SDK çökmesi düzeltildi.
- [Bu müşteri sorununu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/396)gidermek için Azure Web Uygulaması ortamında SDK dağıtım hatası düzeltildi.
- [Bu müşteri sorununu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/433)gidermek için `<voice>` çoklu `<audio>` etiket veya etiket kullanırken bir TTS hatası düzeltildi. 
- SDK askıya alındıktan kurtarıldığınızda bir TTS 401 hatası düzeltildi.
- JavaScript: [Euirim](https://github.com/euirim)katkısı sayesinde ses verilerinin dairesel bir ithalat sabit . 
- JavaScript: 1,7'de eklendikçe hizmet özelliklerini ayarlamak için destek eklendi.
- JavaScript: bağlantı hatasının sürekli, başarısız websocket yeniden bağlanma girişimlerine neden olabileceği bir sorunu düzeltti.


**Örnekler**

- [Burada](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/java/android/sdkdemo)Android için anahtar kelime tanıma örneği eklendi.
- Sunucu senaryosu için [tts](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/csharp/sharedcontent/console/speech_synthesis_server_scenario_sample.cs)örneği burada eklendi.
- C# ve C++ için çoklu cihaz lı konuşma hızlı başlangıçları [burada](quickstarts/multi-device-conversation.md)eklendi.


**Diğer değişiklikler**

- Android'de Optimize Edilmiş SDK çekirdek kitaplığı boyutu.
- SDK 1.9.0 ve itibaren Konuşma `int` `string` Transkripsiyoncu için ses imzası sürüm alanında hem ve türleri destekler.

## <a name="speech-sdk-180-2019-november-release"></a>Konuşma SDK 1.8.0: 2019-Kasım açıklaması

**Yeni Özellikler**

- Prem `FromHost()` kapları ve egemen bulutlar ile kullanımı kolaylaştırmak için bir API eklendi.
- Konuşma Tanıma için Otomatik Kaynak Dil Algılama eklendi (Java ve C++'da)
- Beklenen `SourceLanguageConfig` kaynak dilleri belirtmek için kullanılan Konuşma Tanıma nesnesi eklendi (Java ve C++'da)
- NuGet ve Unity paketleri aracılığıyla Windows (UWP), Android ve iOS'a destek eklendi `KeywordRecognizer`
- Asynchronous toplu olarak Konuşma Transkripsiyon yapmak için Uzaktan Konuşma Java API eklendi.

**Yeni değişiklikler**

- Konuşma Transcriber işlevleri namespace `Microsoft.CognitiveServices.Speech.Transcription`altında taşındı.
- Konuşma Transkripsiyonyöntemlerinin bir `Conversation` bölümü yeni sınıfa taşınır.
- 32-bit (ARMv7 ve x86) iOS için bırakılan destek

**Hata düzeltmeleri**

- Geçerli bir Konuşma `KeywordRecognizer` hizmeti abonelik anahtarı olmadan yerel kullanılıyorsa kilitlenme yi düzeltme

**Örnekler**

- Xamarin örneği için`KeywordRecognizer`
- Birlik örneği için`KeywordRecognizer`
- Otomatik Kaynak Dil Algılama için C++ ve Java örnekleri.

## <a name="speech-sdk-170-2019-september-release"></a>Konuşma SDK 1.7.0: 2019-Eylül açıklaması

**Yeni Özellikler**

- Evrensel Windows Platformu (UWP), Android ve iOS'ta Xamarin için beta desteği eklendi
- Unity için iOS desteği eklendi
- Android, iOS ve Linux'ta ALaw, Mulaw, FLAC için giriş desteği eklendi `Compressed`
- Hizmete `Connection` ileti göndermek için sınıfa eklendi `SendMessageAsync`
- İletinin `Connection` özelliğini ayarlamak için sınıfa eklendi `SetMessageProperty`
- TTS Java (Jre ve Android), Python, Swift ve Objective-C için bağlamalar ekledi
- TTS, macOS, iOS ve Android için oynatma desteği ekledi.
- TTS için "sözcük sınırı" bilgileri eklendi.

**Hata düzeltmeleri**

- Android için Unity 2019'da IL2CPP oluşturma sorunu düzeltildi
- Wav dosya girişindeki hatalı biçimlendirilmiş üstbilgiile ilgili sorun yanlış işleniyor
- Bazı bağlantı özelliklerinde benzersiz olmayan UUI'ler ile ilgili sorun düzeltildi
- Swift bağlayıcılarında nullability belirteçleri hakkında birkaç uyarı düzeltildi (küçük kod değişiklikleri gerektirebilir)
- Ağ yükü altında websocket bağlantılarının nezaketsiz bir şekilde kapatılmasına neden olan bir hata düzeltildi
- Android'de bazen yinelenen gösterim ii'lerinin kullanıldığı bir sorun düzeltildi`DialogServiceConnector`
- Çok dönüşlü etkileşimler arasında bağlantıların kararlılığında iyileştirmeler ve hatalar `Canceled` (olaylar yoluyla)`DialogServiceConnector`
- `DialogServiceConnector`oturum başlar şimdi düzgün bir etkin sırasında `ListenOnceAsync()` arama da dahil olmak üzere olaylar sağlayacaktır`StartKeywordRecognitionAsync()`
- Alınan etkinliklerle `DialogServiceConnector` ilişkili bir çökmegiderilmiş

**Örnekler**

- Xamarin için hızlı başlangıç
- Linux ARM64 bilgileri ile Güncelleştirilmiş CPP Quickstart
- iOS bilgileriyle Güncelleştirilmiş Unity hızlı başlat

## <a name="speech-sdk-160-2019-june-release"></a>Konuşma SDK 1.6.0: 2019-Haziran açıklaması

**Örnekler**

- UWP ve Unity'de MetinDen Konuşmaya Yönelik Hızlı Başlangıç örnekleri
- iOS'ta Swift için hızlı başlangıç örneği
- Konuşma & Niyet Tanıma ve Çeviri için Birlik örnekleri
- Için güncelleştirilmiş quickstart örnekleri`DialogServiceConnector`

**Geliştirmeler / Değişiklikler**

- İletişim ad alanı:
  - `SpeechBotConnector`, `DialogServiceConnector` olarak yeniden adlandırıldı
  - `BotConfig`, `DialogServiceConfig` olarak yeniden adlandırıldı
  - `BotConfig::FromChannelSecret()`için yeniden`DialogServiceConfig::FromBotSecret()`
  - Varolan tüm Doğrudan Satır Konuşması istemcileri yeniden adlandırıldıktan sonra desteklenmeye devam ediyor
- Proxy, kalıcı bağlantıyı desteklemek için TTS REST bağdaştırıcısını güncelleştirin
- Geçersiz bir bölge geçirildiğinde hata iletisi geliştirme
- Swift/Hedef-C:
  - Geliştirilmiş hata raporlaması: Hatayla sonuçlabilen yöntemler artık iki sürümde `NSError` mevcuttur: Biri hata işleme için bir nesneyi, diğeri de özel durum oluşturan. İlki Swift'e maruz kaldı. Bu değişiklik, varolan Swift koduna uyarlamalar gerektirir.
  - Geliştirilmiş olay işleme

**Hata düzeltmeleri**

- TTS için düzeltme: ses işleme tamamlanana kadar beklemeden geleceğin döndüğü yer `SpeakTextAsync`
- Tam dil desteğini etkinleştirmek için C#'da dizeleri mareşalleme için düzeltme
- Örneklerde net461 hedef çerçevesi ile çekirdek kitaplığı yüklemek için .NET çekirdek uygulama sorunu için düzeltme
- Örneklerdeki çıktı klasörüne yerel kitaplıkları dağıtmak için ara sıra çıkan sorunları düzeltme
- Web soketinin güvenilir bir şekilde kapanması için düzeltme
- Linux üzerinde çok ağır yük altında bir bağlantı açarken olası çökme için düzeltme
- macOS için çerçeve paketinde eksik meta verileri düzeltme
- Windows'daki `pip install --user` sorunlar için düzeltme

## <a name="speech-sdk-151"></a>Konuşma SDK 1.5.1

Bu bir hata düzeltme sürümüdür ve yalnızca yerel/yönetilen SDK'yı etkiler. SDK'nın JavaScript sürümünü etkilemiyor.

**Hata düzeltmeleri**

- Konuşma Transkripsiyonile birlikte kullanıldığında FromSubscription'ı düzeltin.
- Ses asistanları için anahtar kelime tespitindeki hatayı düzeltme.

## <a name="speech-sdk-150-2019-may-release"></a>Konuşma SDK 1.5.0: 2019-Mayıs açıklaması

**Yeni özellikler**

- Anahtar kelime tespit (KWS) artık Windows ve Linux için kullanılabilir. KWS işlevselliği herhangi bir mikrofon türüyle çalışabilir, ancak resmi KWS desteği şu anda Azure Kinect DK donanımında veya Konuşma Aygıtları SDK'da bulunan mikrofon dizileriyle sınırlıdır.
- İfade ipucu işlevi SDK üzerinden kullanılabilir. Daha fazla bilgi için [buraya](how-to-phrase-lists.md)bakın.
- Konuşma transkripsiyon işlevselliği SDK üzerinden kullanılabilir. Buraya [here](conversation-transcription-service.md)bakın .
- Doğrudan Satır Konuşması kanalını kullanarak sesli asistanlar için destek ekleyin.

**Örnekler**

- SDK tarafından desteklenen yeni özellikler veya yeni hizmetler için örnekler eklendi.

**Geliştirmeler / Değişiklikler**

- Hizmet davranışını veya hizmet sonuçlarını ayarlamak için çeşitli tanıyıcı özellikleri eklendi (maskeleme küfür ve diğerleri gibi).
- Artık tanıyanı standart yapılandırma özellikleri aracılığıyla yapılandırabilirsiniz, tanıyanı `FromEndpoint`oluştursanız bile.
- Objective-C: `OutputFormat` özellik `SPXSpeechConfiguration`eklendi.
- SDK artık Debian 9'u Linux dağıtımı olarak destekliyor.

**Hata düzeltmeleri**

- Hoparlör kaynağının metin den konuşmaya çok erken verildiği bir sorun giderildi.

## <a name="speech-sdk-142"></a>Konuşma SDK 1.4.2

Bu bir hata düzeltme sürümüdür ve yalnızca yerel/yönetilen SDK'yı etkiler. SDK'nın JavaScript sürümünü etkilemiyor.

## <a name="speech-sdk-141"></a>Konuşma SDK 1.4.1

Bu yalnızca JavaScript sürümüdür. Hiçbir özellik eklenmedi. Aşağıdaki düzeltmeler yapıldı:

- Web paketinin https-proxy-agent yüklemesini engelleyin.

## <a name="speech-sdk-140-2019-april-release"></a>Konuşma SDK 1.4.0: 2019-Nisan açıklaması

**Yeni özellikler**

- SDK artık metinden konuşmaya hizmeti beta sürümü olarak desteklemektedir. Windows ve Linux Masaüstü'nde C++ ve C#'dan desteklenir. Daha fazla bilgi için [metinden konuşmaya genel bakışı](text-to-speech.md#get-started)kontrol edin.
- SDK artık MP3 ve Opus/OGG ses dosyalarını akış giriş dosyaları olarak destekler. Bu özellik yalnızca C++ ve C# adresinden Linux'ta kullanılabilir ve şu anda beta sürümündedir (daha fazla ayrıntı [burada).](how-to-use-codec-compressed-audio-input-streams.md)
- Java, .NET core, C++ ve Objective-C için Konuşma SDK macOS desteği kazanmıştır. macOS için Objective-C desteği şu anda beta da.
- iOS: iOS için Konuşma SDK (Objective-C) şimdi de bir CocoaPod olarak yayınlanır.
- JavaScript: Varsayılan olmayan mikrofonun giriş aygıtı olarak desteklenmesi.
- JavaScript: Node.js için proxy desteği.

**Örnekler**

- MacOS'ta C++ ve Objective-C ile Konuşma SDK'sını kullanmak için örnekler eklendi.
- Metinden konuşmaya hizmetinin kullanımını gösteren örnekler eklendi.

**Geliştirmeler / Değişiklikler**

- Python: Tanıma sonuçlarının ek özellikleri `properties` artık özellik üzerinden ortaya çıkıyor.
- Ek geliştirme ve hata ayıklama desteği için, SDK günlüğe kaydetme ve tanılama bilgilerini bir günlük dosyasına yönlendirebilirsiniz (daha fazla ayrıntı [burada).](how-to-use-logging.md)
- JavaScript: Ses işleme performansını artırın.

**Hata düzeltmeleri**

- Mac/iOS: Konuşma hizmetine bağlantı kurulamadığında uzun süre beklemeye neden olan hata düzeltildi.
- Python: Python geri aramaları için bağımsız değişkenler için hata işleme geliştirmek.
- JavaScript: Konuşma için sabit yanlış durum raporlaması RequestSession'da sona erdi.

## <a name="speech-sdk-131-2019-february-refresh"></a>Konuşma SDK 1.3.1: 2019-Şubat yenileme

Bu bir hata düzeltme sürümüdür ve yalnızca yerel/yönetilen SDK'yı etkiler. SDK'nın JavaScript sürümünü etkilemiyor.

**Hata düzeltmesi**

- Mikrofon girişi kullanırken bellek sızıntısı düzeltildi. Akış tabanlı veya dosya girişi etkilenmez.

## <a name="speech-sdk-130-2019-february-release"></a>Konuşma SDK 1.3.0: 2019-Şubat açıklaması

**Yeni Özellikler**

- Konuşma SDK `AudioConfig` sınıf üzerinden giriş mikrofonseçimi destekler. Bu, varsayılan olmayan bir mikrofondan Konuşma hizmetine ses verisi akışı yapmanızı sağlar. Daha fazla bilgi için [ses girişi aygıtı seçimini](how-to-select-audio-input-devices.md)açıklayan belgelere bakın. Bu özellik henüz JavaScript'ten kullanılamıyor.
- Konuşma SDK şimdi bir beta sürümünde Birlik destekler. [GitHub örnek deposundaki](https://aka.ms/csspeech/samples)sorun bölümü aracılığıyla geri bildirim sağlayın. Bu sürüm, Windows x86 ve x64 (masaüstü veya Evrensel Windows Platformu uygulamaları) ve Android (ARM32/64, x86) üzerinde Birlik'i destekler. Daha fazla bilgi [bizim Unity quickstart](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=unity)mevcuttur.
- Dosya `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (önceki sürümlerde sevk) artık gerekli değildir. İşlevsellik artık çekirdek SDK entegre edilmiştir.

**Örnekler**

Aşağıdaki yeni içerik örnek [depomuzda](https://aka.ms/csspeech/samples)mevcuttur:

- Için `AudioConfig.FromMicrophoneInput`ek örnekler .
- Niyet tanıma ve çeviri için ek Python örnekleri.
- nesneyi `Connection` iOS'ta kullanmak için ek örnekler.
- Ses çıkışı ile çeviri için ek Java örnekleri.
- [Toplu Transkripsiyon REST API](batch-transcription.md)kullanımı için yeni örnek .

**Geliştirmeler / Değişiklikler**

- Python
  - Gelişmiş parametre doğrulaması ve `SpeechConfig`hata iletileri .
  - `Connection` Nesne için destek ekleyin.
  - Windows'da 32 bit Python (x86) desteği.
  - Python için Konuşma SDK beta dışındadır.
- iOS
  - SDK şimdi iOS SDK sürüm 12.1 karşı inşa edilmiştir.
  - SDK şimdi iOS sürümleri 9.2 ve daha sonra destekler.
  - Başvuru belgelerini geliştirin ve birkaç özellik adLarını düzeltin.
- JavaScript
  - `Connection` Nesne için destek ekleyin.
  - Birlikte verilen JavaScript için tür tanım dosyaları ekleme
  - İfade ipuçları için ilk destek ve uygulama.
  - Tanıma için hizmet JSON ile iade özellikleri koleksiyonu
- Windows DLL'ler artık bir sürüm kaynağı içeriyor.
- Bir tanıyıcı `FromEndpoint` oluşturursanız, parametreleri doğrudan bitiş noktası URL'sine ekleyebilirsiniz. Bunu `FromEndpoint` kullanarak tanıyanı standart yapılandırma özellikleri aracılığıyla yapılandıramaz.

**Hata düzeltmeleri**

- Boş proxy kullanıcı adı ve proxy parolası doğru şekilde işlenmedi. Bu sürümle, proxy kullanıcı adı ve proxy parolasını boş bir dize olarak ayarlarsanız, proxy'ye bağlanırken gönderilmez.
- SessionId's SDK tarafından oluşturulan her zaman gerçekten&nbsp;bazı diller / ortamlar için rasgele değildi. Bu sorunu gidermek için rasgele jeneratör başlatma eklendi.
- Yetki belirteci işleme geliştirmek. Yetkilendirme belirteci kullanmak istiyorsanız, abonelik `SpeechConfig` anahtarını belirtin ve abonelik anahtarını boş bırakın. Sonra her zamanki gibi tanıyın oluşturun.
- Bazı durumlarda `Connection` nesne doğru serbest bırakılmadı. Bu sorun düzeltilmiştir.
- JavaScript örneği Safari'de de çeviri sentezi için ses çıktısını destekleyecek şekilde sabitlenmiştir.

## <a name="speech-sdk-121"></a>Konuşma SDK 1.2.1

Bu yalnızca JavaScript sürümüdür. Hiçbir özellik eklenmedi. Aşağıdaki düzeltmeler yapıldı:

- Ateşin sonu sırayla.sonunda, konuşmada değil.sonda.
- Geçerli gönderme başarısız olursa sonraki gönderiyi zamanlamayan ses pompasındaki hatayı düzeltin.
- Auth belirteci ile sürekli tanıma yı düzeltin.
- Farklı tanıyıcı / uç noktaları için hata düzeltmesi.
- Dokümantasyon geliştirmeleri.

## <a name="speech-sdk-120-2018-december-release"></a>Konuşma SDK 1.2.0: 2018-Aralık açıklaması

**Yeni Özellikler**

- Python
  - Python desteğinin Beta sürümü (3.5 ve üzeri) bu sürümle birlikte kullanılabilir. Daha fazla bilgi için buraya bakınız](quickstart-python.md).
- JavaScript
  - JavaScript için Konuşma SDK açık kaynaklı olmuştur. Kaynak kodu [GitHub'da](https://github.com/Microsoft/cognitive-services-speech-sdk-js)kullanılabilir.
  - Şimdi Node.js destek, daha fazla bilgi [burada](quickstart-js-node.md)bulabilirsiniz.
  - Ses oturumları için uzunluk kısıtlaması kaldırıldı, yeniden bağlantı kapağın altında otomatik olarak gerçekleşecek.
- `Connection`Nesne
  - `Recognizer`Bir `Connection` nesneye erişebilirsiniz. Bu nesne, hizmet bağlantısını açıkça başlatmanızı ve olayları bağlamak ve bağlantıyı kesmek için abone olmak için izin verir.
    (Bu özellik henüz JavaScript ve Python'dan kullanılamıyor.)
- Ubuntu 18.04 desteği.
- Android
  - APK üretimi sırasında Etkin ProGuard desteği.

**Gelişmeler**

- İş parçacığı, kilit, mutexes sayısını azaltarak, iç iş parçacığı kullanımında iyileştirmeler.
- Geliştirilmiş hata raporlama / bilgi. Bazı durumlarda, hata iletileri tüm yol dışarı yayılan olmamıştır.
- Güncel modülleri kullanmak için JavaScript'teki güncelleştirilmiş geliştirme bağımlılıkları.

**Hata düzeltmeleri**

- Bir tür uyuşmazlığı nedeniyle düzeltilen bellek sızıntıları. `RecognizeAsync`
- Bazı durumlarda istisnalar sızdırılıyordu.
- Çeviri olay argümanlarında bellek sızıntısını giderme.
- Uzun süren oturumlarda yeniden bağlanma yla ilgili kilitleme sorunu giderildi.
- Başarısız çeviriler için nihai sonucun eksik olması yla sonuçlanabilir bir sorun giderildi.
- C#: Ana `async` iş parçacığında bir işlem beklenmiyorsa, async görevi tamamlanmadan önce tanıyıcısın atılması mümkündür.
- Java: Java VM'nin çökmesine neden olan bir sorun giderildi.
- Amaç-C: Sabit enum haritalama; RecognizedIntent yerine `RecognizingIntent`döndürüldü.
- JavaScript: Varsayılan çıktı biçimini 'basit' olarak `SpeechConfig`ayarlayın.
- JavaScript: JavaScript ve diğer dillerde config nesnesi özellikleri arasındaki tutarsızlığı kaldırma.

**Örnekler**

- Çeşitli örnekleri güncelleştirip düzeltti (örneğin çeviri için çıktı sesleri vb.).
- [Örnek deposuna](https://aka.ms/csspeech/samples)Düğüm.js örnekleri eklendi.

## <a name="speech-sdk-110"></a>Konuşma SDK 1.1.0

**Yeni Özellikler**

- Android x86/x64 desteği.
- Proxy Desteği: `SpeechConfig` Nesnede artık proxy bilgilerini (ana bilgisayar adı, bağlantı noktası, kullanıcı adı ve parola) ayarlamak için bir işlev arayabilirsiniz. Bu özellik henüz iOS'ta kullanılamıyor.
- Geliştirilmiş hata kodu ve iletileri. Bir tanıma bir hata döndürürse, bu zaten `CancellationDetails` (iptal edilen `Error`olay) veya (tanıma sonucu) . `Reason` İptal edilen olay artık iki `ErrorCode` ek `ErrorDetails`üye içerir ve . Sunucu bildirilen hatayla ek hata bilgilerini döndürürse, artık yeni üyelerde kullanılabilir.

**Gelişmeler**

- Tanıyan yapılandırmasında ek doğrulama eklendi ve ek hata iletisi eklendi.
- Bir ses dosyasının ortasında uzun süreli sessizliğin daha iyi işlenmesi.
- NuGet paketi: .NET Framework projeleri için, AnyCPU yapılandırması ile bina oluşturmayı engeller.

**Hata düzeltmeleri**

- Tanıyanlarda bulunan birkaç özel durum düzeltildi. Buna ek olarak, özel durumlar `Canceled` yakalanır ve olaya dönüştürülür.
- Mülk yönetiminde bir bellek sızıntısını düzeltin.
- Bir ses giriş dosyasının tanıyanı çökertebileceği hata düzeltildi.
- Oturum durdurma olayından sonra olayların alınabileceği bir hata düzeltildi.
- Diş işparçacığı bazı yarış koşulları düzeltildi.
- Çökmeye neden olabilecek bir iOS uyumluluk sorunu düzeltildi.
- Android mikrofon desteği için kararlılık iyileştirmeleri.
- JavaScript'teki bir tanıyıcının tanıma dilini yoksayacağı bir hata düzeltildi.
- JavaScript'te `EndpointId` (bazı durumlarda) ayarlanmasını engelleyen bir hata düzeltildi.
- JavaScript'te AddIntent'de parametre sırasını değiştirdi ve eksik `AddIntent` JavaScript imzası eklendi.

**Örnekler**

- [Örnek deposunda](https://aka.ms/csspeech/samples)çekme ve itme akışı kullanımı için C++ ve C# örnekleri eklendi.

## <a name="speech-sdk-101"></a>Konuşma SDK 1.0.1

Güvenilirlik iyileştirmeleri ve hata düzeltmeleri:

- Tanıyanı ortadan katarak Yarış durumundan kaynaklanan olası ölümcül hata düzeltildi
- Ayarlanmamış özellikler durumunda olası önemli hata düzeltildi.
- Ek hata ve parametre denetimi eklendi.
- Amaç-C: NSString'te ad geçersiz kılmanın neden olduğu olası olası önemli hata düzeltildi.
- Amaç-C: API'nin ayarlanmış görünürlüğü
- JavaScript: Olaylar ve yükleri ile ilgili olarak sabit.
- Dokümantasyon geliştirmeleri.

Örnek [depomuza,](https://aka.ms/csspeech/samples)JavaScript için yeni bir örnek eklendi.

## <a name="cognitive-services-speech-sdk-100-2018-september-release"></a>Bilişsel Hizmetler Konuşma SDK 1.0.0: 2018-Eylül sürümü

**Yeni özellikler**

- ios'ta Objective-C desteği. [iOS için Objective-C hızlı başlatmamıza](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone-langs/objectivec-ios.md)göz atın.
- Tarayıcıda JavaScript desteği. [Bizim JavaScript quickstart](quickstart-js-browser.md)göz atın.

**Yeni değişiklikler**

- Bu sürümle birlikte, bir dizi kesme değişikliği getirilir.
  Ayrıntılar için [bu sayfayı](https://aka.ms/csspeech/breakingchanges_1_0_0) kontrol edin.

## <a name="cognitive-services-speech-sdk-060-2018-august-release"></a>Bilişsel Hizmetler Konuşma SDK 0.6.0: 2018-Ağustos açıklaması

**Yeni özellikler**

- Konuşma SDK ile oluşturulmuş UWP uygulamaları artık Windows Uygulama Sertifika Kiti 'ni (WACK) geçirebilir.
  [UWP quickstart](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-chsarp&tabs=uwp)göz atın.
- Linux'ta .NET Standard 2.0 desteği (Ubuntu 16.04 x64).
- Deneysel: Windows (64-bit) ve Linux 'ta (Ubuntu 16.04 x64) Java 8'i destekleyin.
  [Java Runtime Ortamı hızlı başlatın.](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=jre)

**Fonksiyonel değişim**

- Bağlantı hatalarıyla ilgili ek hata ayrıntısı bilgilerini açığa çıkar.

**Yeni değişiklikler**

- Java'da (Android) `SpeechFactory.configureNativePlatformBindingWithDefaultCertificate` işlev artık bir yol parametresi gerektirmez. Artık yol desteklenen tüm platformlarda otomatik olarak algılanır.
- Java ve C#'daki `EndpointUrl` özelliğin erişime giren özelliği kaldırıldı.

**Hata düzeltmeleri**

- Java'da çeviri tanıma da ses sentezi sonucu şimdi uygulanmaktadır.
- Etkin olmayan iş parçacıklarına ve açık ve kullanılmayan soket sayısının artmasına neden olabilecek bir hata düzeltildi.
- Uzun süreli bir tanıma iletimin ortasında sona erebilir bir sorun, giderildi.
- Tanıyan kapatmada bir yarış koşulu düzeltildi.

## <a name="cognitive-services-speech-sdk-050-2018-july-release"></a>Bilişsel Hizmetler Konuşma SDK 0.5.0: 2018-Temmuz sürümü

**Yeni özellikler**

- Destek Android platformu (API 23: Android 6.0 Marshmallow veya üstü). [Android quickstart](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java&tabs=android)göz atın.
- Windows'da .NET Standard 2.0'ı destekleyin. [.NET Core quickstart'a](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnetcore)göz atın.
- Deneysel: Windows'da UWP'yi destekleyin (sürüm 1709 veya sonraki sürüm).
  - [UWP quickstart](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=uwp)göz atın.
  - Not: Konuşma SDK ile oluşturulmuş UWP uygulamaları henüz Windows Uygulama Sertifika Kiti (WACK) geçmez.
- Otomatik yeniden bağlantı yla uzun süreli tanımayı destekleyin.

**İşlevsel değişiklikler**

- `StartContinuousRecognitionAsync()`uzun süreli tanınmayı destekler.
- Tanıma sonucu daha fazla alan içerir. Bunlar, tanınan metnin ses başlangıç ve süresinden (hem onaylar) hem de tanıma durumunu `InitialSilenceTimeout` `InitialBabbleTimeout`temsil eden ek değerlerden ve .
- Fabrika örnekleri oluşturmak için YetkilendirmeToken'i destekleyin.

**Yeni değişiklikler**

- Tanıma olayları: `NoMatch` olay türü `Error` olayla birleştirildi.
- C#'daki SpeechOutputFormat, `OutputFormat` C++ile uyumlu kalacak şekilde yeniden adlandırıldı.
- `AudioInputStream` Arabirimin bazı yöntemlerinin dönüş türü biraz değişti:
  - Java'da `read` yöntem artık `long` `int`.
  - C#'da `Read` yöntem artık `uint` `int`.
  - C++'da `Read` artık `GetFormat` `int`yöntem `size_t` yerine 've yöntemler döndürür.
- C++: Ses giriş akışları örnekleri artık yalnızca . `shared_ptr`

**Hata düzeltmeleri**

- Zaman dolduğunda `RecognizeAsync()` sonuç yanlış dönüş değerleri düzeltildi.
- Windows'daki medya temel kitaplıklarına olan bağımlılık kaldırıldı. SDK artık Core Audio API'lerini kullanıyor.
- Belge düzeltmesi: Desteklenen [bölgeleri](regions.md) açıklamak için bölgeler sayfası eklendi.

**Bilinen sorun**

- Android için Konuşma SDK çeviri için konuşma sentezi sonuçlarını bildirmez. Bu sorun bir sonraki sürümde giderilecektir.

## <a name="cognitive-services-speech-sdk-040-2018-june-release"></a>Bilişsel Hizmetler Konuşma SDK 0.4.0: 2018-Haziran sürümü

**İşlevsel değişiklikler**

- AudioInputStream

  Bir tanıyıcı artık ses kaynağı olarak bir akışı tüketebilir. Daha fazla bilgi için ilgili [nasıl yapılacağını kılavuzuna](how-to-use-audio-input-streams.md)bakın.

- Ayrıntılı çıktı biçimi

  Bir `SpeechRecognizer`, istediğiniz `Detailed` veya `Simple` çıktı biçimi oluşturduğunuzda. Bir `DetailedSpeechRecognitionResult` güven puanı, tanınan metin, ham sözlü form, normalleştirilmiş form ve maskeli küfür ile normalleştirilmiş formu içerir.

**Son dakika değişikliği**

- C#'dan itibaren `SpeechRecognitionResult.Text` `SpeechRecognitionResult.RecognizedText` değiştirildi.

**Hata düzeltmeleri**

- Kapatma sırasında USP katmanındaki olası bir geri arama sorunu giderildi.
- Bir tanıyıcı bir ses giriş dosyası tükettiyse, dosya tutamacını gerekenden daha uzun süre tutuyordu.
- İleti pompası ve tanıyan arasında birkaç kilitlenme kaldırıldı.
- Servisten `NoMatch` gelen yanıt zaman dolduğunda yangın sonucu.
- Windows'daki medya kuruluşu kitaplıkları gecikme yüklü. Bu kitaplık yalnızca mikrofon girişi için gereklidir.
- Ses verileri için yükleme hızı orijinal ses hızının yaklaşık iki katı ile sınırlıdır.
- Windows'da, C# .NET derlemeleri artık güçlü adlandırılmış.
- Dokümantasyon `Region` düzeltmesi: bir tanıyıcı oluşturmak için gerekli bilgilerdir.

Daha fazla örnek eklendi ve sürekli olarak güncelleniyor. En son örnek kümesi için [Konuşma SDK örnekleri GitHub deposuna](https://aka.ms/csspeech/samples)bakın.

## <a name="cognitive-services-speech-sdk-0212733-2018-may-release"></a>Bilişsel Hizmetler Konuşma SDK 0.2.12733: 2018-Mayıs yayın

Bu sürüm Bilişsel Hizmetler Konuşma SDK ilk kamu önizleme açıklamasıdır.

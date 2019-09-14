---
title: Translator Konuşma Çevirisi API'si Başvurusu
titleSuffix: Azure Cognitive Services
description: Translator Konuşma Çevirisi API'si için başvuru belgeleri.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-speech
ms.topic: reference
ms.date: 05/18/2018
ms.author: nitinme
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 9d2f78d05de6b966dd872e0b57a90d1c8e890975
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70965366"
---
# <a name="translator-speech-api"></a>Translator Konuşma Çevirisi API’si

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-translator-speech-deprecation-note.md)]

Bu hizmet, bir dildeki konuşma konuşmanızı başka bir dilin metnine aktarmak için bir akış API 'SI sunar. API, geri çevrilen metni seslendirme için metin okuma özellikleri de tümleştirir. Translator Konuşma Çevirisi API'si, Skype çeviricisinde görüldüğü gibi, konuşmaları gerçek zamanlı çevirisi gibi senaryolara izin vermez.

Translator Konuşma Çevirisi API'si ile istemci uygulamaları, konuşma sesini hizmete akışa alır ve kaynak dilde tanınan metni ve hedef dilde çevirisini içeren metin tabanlı sonuçların bir akışını geri alırlar. Metin sonuçları, gelen ses akışına derin sinir ağlarıyla desteklenen Otomatik Konuşma Tanıma (ASR) uygulanarak oluşturulur. Ham ASR çıkışı, Kullanıcı amacını daha yakından yansıtması için TrueText adlı yeni bir teknik tarafından daha fazla geliştirildi. Örneğin, TrueText, sürekliliklerini kaldırır (hmms ve II 'ler) ve uygun noktalama ve büyük harfleri geri yükler. Küfürleri maskeleme veya çıkarma olanağı da sağlanır. Tanıma ve çeviri altyapıları, konuşmaları işlemek için özel olarak eğitilmiştir. Konuşma çevirisi hizmeti, bir utterance 'in sonunu saptamak için sessizlik algılamayı kullanır. Ses etkinliğinde bir duraklama olduktan sonra, hizmet tamamlanan konuşma için nihai sonucun geri akışını yapar. Ayrıca hizmet kısmi sonuçları da geri gönderebilir; bu yolla devam eden konuşma için ara tanıma ve çeviriler sağlanır. Son sonuçlar için hizmet, hedef dillerdeki konuşulan metinden konuşmayı (metinden konuşmaya) birleştirme yeteneği sağlar. Metni konuşmaya dönüştürme sesi, istemci tarafından belirtilen biçimde oluşturulur. WAV ve MP3 biçimleri kullanılabilir.

Translator Konuşma Çevirisi API'si, istemci ve sunucu arasında tam çift yönlü iletişim kanalı sağlamak için WebSocket protokolünü kullanır. Bir uygulama, hizmeti kullanmak için bu adımları gerektirir:

## <a name="1-getting-started"></a>1. Başlarken
Translator Metin Çevirisi API'si erişmek için [Microsoft Azure kaydolmanız](translator-speech-how-to-signup.md)gerekir.

## <a name="2-authentication"></a>2. Authentication

Kimlik doğrulamak için abonelik anahtarını kullanın. Translator Konuşma Çevirisi API'si iki kimlik doğrulaması modunu destekler:

* **Erişim belirteci kullanma:** Uygulamanızda, belirteç hizmetinden bir erişim belirteci alın. Azure bilişsel hizmetler kimlik doğrulama hizmeti 'nden bir erişim belirteci almak için Translator Konuşma Çevirisi API'si abonelik anahtarınızı kullanın. Erişim belirteci 10 dakika için geçerlidir. 10 dakikada bir yeni bir erişim belirteci alın ve bu 10 dakika içinde yinelenen istekler için aynı erişim belirtecini kullanmaya devam edin.

* **Abonelik anahtarını doğrudan kullanma:** Uygulamanızda, abonelik anahtarınızı `Ocp-Apim-Subscription-Key` üst bilgide bir değer olarak geçirin.

Abonelik anahtarınızı ve erişim belirtecini, görünümden gizlenmesi gereken gizli diziler olarak değerlendirin.

## <a name="3-query-languages"></a>3. Sorgu dilleri
**Geçerli desteklenen diller kümesi için diller kaynağını sorgulayın.** [Diller kaynağı](languages-reference.md) , konuşma tanıma için kullanılabilen diller ve sesler kümesini, metin çevirisi ve metinden konuşmaya yönelik olarak kullanıma sunar. Her dile veya sese, Translator Konuşma Çevirisi API'si aynı dili veya sesi tanımlamak için kullandığı bir tanımlayıcı verilir.

## <a name="4-stream-audio"></a>4. Ses akışı
**Bir bağlantı açın ve hizmete ses akışı yapmaya başlayın.** Hizmet URL 'SI `wss://dev.microsofttranslator.com/speech/translate`. Hizmet tarafından beklenen parametreler ve ses biçimleri, `/speech/translate` işlemde aşağıda açıklanmaktadır. Parametrelerden biri, yukarıdaki 2. adımdaki erişim belirtecini geçirmek için kullanılır.

## <a name="5-process-the-results"></a>5. Sonuçları işleme
**Hizmetten geri akışlı sonuçları işleyin.** Kısmi sonuçların biçimi, son sonuçlar ve metinden konuşmaya ses kesimleri, aşağıdaki `/speech/translate` işlemin belgelerinde açıklanmıştır.

Translator Konuşma Çevirisi API'si kullanımını gösteren kod örnekleri, [Microsoft Translator GitHub sitesinde](https://github.com/MicrosoftTranslator)bulunabilir.

## <a name="implementation-notes"></a>Uygulama notları

GET/Speech/Translate, konuşma çevirisi için bir oturum oluşturur

### <a name="connecting"></a>Bağlanıyor
Hizmetine bağlanmadan önce, bu bölümün ilerleyen kısımlarında verilen parametrelerin listesini gözden geçirin. Örnek bir istek:

`GET wss://dev.microsofttranslator.com/speech/translate?from=en-US&to=it-IT&features=texttospeech&voice=it-IT-Elsa&api-version=1.0`
`Ocp-Apim-Subscription-Key: {subscription key}`
`X-ClientTraceId: {GUID}`

İstek, konuşulan Ingilizce 'nin hizmete akışını ve Italyanca 'e çevrileceğini belirtir. Her bir son tanınma sonucu, Elsa adlı kadın sesiyle bir metinden konuşmaya ses yanıtı oluşturacaktır. İsteğin içinde `Ocp-Apim-Subscription-Key header`kimlik bilgileri içerdiğine dikkat edin. İstek, üst bilgide `X-ClientTraceId`genel benzersiz bir tanımlayıcı ayarlayarak da en iyi yöntemi izler. İstemci uygulaması, oluşabilecek sorunları gidermek için kullanılabilmesi için izleme KIMLIĞINI günlüğe kaydeder.

### <a name="sending-audio"></a>Ses gönderiliyor
Bağlantı kurulduktan sonra istemci, hizmete ses akışı yapmaya başlar. İstemci parçalar halinde ses gönderir. Her bir öbek, Ikili türünde bir WebSocket iletisi kullanılarak iletilir.

Ses girişi, Waveform ses dosyası biçimindedir (dalga veya dosya adı uzantısı nedeniyle genellikle WAV olarak bilinir). İstemci uygulaması, 16 kHz ' de örneklenmiş bir tek kanal, imzalanmış 16bit PCM ses akışı içermelidir. İstemci tarafından akan ilk bayt kümesi, WAV üst bilgisini içerir. 16 kHz ' de örneklenmiş tek kanallı, imzalanmış 16 bit PCM akışı için 44 baytlık üst bilgi:

|Offset|Value|
|:---|:---|
|0 - 3|"RIFF"|
|4 - 7|0|
|8 - 11|SES|
|12 - 15|FMT|
|16 - 19|16|
|20 - 21|1\.|
|22 - 23|1\.|
|24 - 27|16000|
|28 - 31|32000|
|32-33|2|
|34-35|16|
|36-39|verileri|
|40-43|0|

Toplam dosya boyutu (bayt 4-7) ve "veri" (bayt 40-43) boyutunun sıfır olarak ayarlandığını unutmayın. Bu, toplam boyutunun bilinen olmadığı akış senaryosu için Tamam ' dır.

İstemci, WAV (RıFF) üst bilgisini gönderdikten sonra ses verilerinin öbeklerini gönderir. İstemci genellikle sabit büyüklükte öbekleri akışa alır (örneğin, her seferinde 100 ses akışı).

### <a name="signal-the-end-of-the-utterance"></a>Söylenişi 'in sonuna sinyal
Translator Konuşma Çevirisi API'si, sesi gönderirken ses akışının dökümünü ve çevirisini döndürür. Son TRANSCRIPT, son çeviri ve çevrilmiş ses yalnızca, utterance 'in sonundan sonra size döndürülür. Bazı durumlarda, utterance 'in sonuna kadar zorlamaya zorlamak isteyebilirsiniz. Lütfen deterance 'in sonuna zorlamak için 2,5 saniye sessizlik gönderin.

### <a name="final-result"></a>Nihai sonuç
Nihai bir konuşma tanıma sonucu, utterance 'in sonunda oluşturulur. Bir sonuç, metinden metin türünde bir WebSocket iletisi kullanılarak hizmetten istemciye iletilir. İleti içeriği, aşağıdaki özelliklere sahip bir nesnenin JSON serileştirmesi olur:

* `type`: Sonuç türünü tanımlamak için dize sabiti. Son sonuçlar için değer son ' dur.
* `id`: Tanıma sonucuna atanan dize tanımlayıcısı.
* `recognition`: Kaynak dilde tanınan metin. Metin, yanlış bir tanıma durumunda boş bir dize olabilir.
* `translation`: Hedef dilde çevrilen tanınan metin.
* `audioTimeOffset`: Tanımanın başlangıç zaman işaretleri (1 onay = 100 nanosaniye). Göreli konum akışın başlangıcına göre belirlenir.
* `audioTimeSize`: Tanımanın işaret (100 nanosaniye) cinsinden süre.
* `audioStreamPosition`: Tanımanın başlangıcının bayt kayması. Göreli konum akışın başlangıcına göre belirlenir.
* `audioSizeBytes`: Tanımanın bayt cinsinden boyutu.

Ses akışına tanımanın konumlandırmanın varsayılan olarak sonuçlara dahil edilmediğini unutmayın. Özelliğin istemci tarafından seçilmesi gerekir (bkz `features` . parametre). `TimingInfo`

Örnek nihai sonuç aşağıdaki gibidir:

```
{
  type: "final"
  id: "23",
  recognition: "what was said",
  translation: "translation of what was said",
  audioStreamPosition: 319680,
  audioSizeBytes: 35840,
  audioTimeOffset: 2731600000,
  audioTimeSize: 21900000
}
```

### <a name="partial-result"></a>Kısmi sonuç
Kısmi veya ara konuşma tanıma sonuçları istemciye varsayılan olarak akış içermez. İstemci, bu parametreleri istemek için özellikler sorgu parametresini kullanabilir.

Kısmi bir sonuç, metinden metin türünde bir WebSocket iletisi kullanılarak hizmetten istemciye iletilir. İleti içeriği, aşağıdaki özelliklere sahip bir nesnenin JSON serileştirmesi olur:

* `type`: Sonuç türünü tanımlamak için dize sabiti. Değer kısmi sonuçlar için kısmen.
* `id`: Tanıma sonucuna atanan dize tanımlayıcısı.
* `recognition`: Kaynak dilde tanınan metin.
* `translation`: Hedef dilde çevrilen tanınan metin.
* `audioTimeOffset`: Tanımanın başlangıç zaman işaretleri (1 onay = 100 nanosaniye). Göreli konum akışın başlangıcına göre belirlenir.
* `audioTimeSize`: Tanımanın işaret (100 nanosaniye) cinsinden süre.
* `audioStreamPosition`: Tanımanın başlangıcının bayt kayması. Göreli konum akışın başlangıcına göre belirlenir.
* `audioSizeBytes`: Tanımanın bayt cinsinden boyutu.

Ses akışına tanımanın konumlandırmanın varsayılan olarak sonuçlara dahil edilmediğini unutmayın. Tımingınfo özelliği istemci tarafından seçilmelidir (bkz. Özellikler parametresi).

Örnek nihai sonuç aşağıdaki gibidir:

```
{
  type: "partial"
  id: "23.2",
  recognition: "what was",
  translation: "translation of what was",
  audioStreamPosition: 319680,
  audioSizeBytes: 25840,
  audioTimeOffset: 2731600000,
  audioTimeSize: 11900000
}
```

### <a name="text-to-speech"></a>Metin okuma
Metinden konuşmaya özelliği etkin olduğunda (aşağıdaki parametreye bakın `features` ), nihai sonucun ardından, konuşulan çevrilmiş metnin sesi gelir. Ses verileri, yığın olarak, Ikili türünde bir WebSocket iletileri dizisi olarak hizmet üzerinden istemciye gönderilir. Bir istemci, her iletinin FIN bitini denetleyerek akışın sonunu tespit edebilir. Son Ikili iletinin, akış sonunu belirtmek için FIN biti bir tane olarak ayarlanmıştır. Akışın biçimi `format` parametrenin değerine bağlıdır.

### <a name="closing-the-connection"></a>Bağlantı kapatılıyor
Bir istemci uygulaması akışı seslerini bitirdiğinde ve son nihai sonucu aldıysa, WebSocket kapanış el sıkışma 'nı başlatarak bağlantıyı kapatması gerekir. Sunucunun bağlantıyı sonmasına neden olacak koşullar vardır. Aşağıdaki WebSocket kapalı kodları istemci tarafından alınabilir:

* `1003 - Invalid Message Type`: Sunucu, aldığı veri türünü kabul etmediğinden bağlantıyı sonlandırıyor. Bu genellikle gelen ses doğru bir üstbilgiyle başlamadığınızda meydana gelir.
* `1000 - Normal closure`: İstek karşılandıktan sonra bağlantı kapatıldı. Sunucu bağlantıyı kapatacak: istemciden uzun bir süre için hiçbir ses alınmadı; sessizlik uzun bir süre boyunca akışa eklendiğinde; bir oturum izin verilen en uzun süreye ulaştığında (yaklaşık 90 dakika).
* `1001 - Endpoint Unavailable`: Sunucunun kullanılamaz hale geldiğini gösterir. İstemci uygulaması yeniden deneme sayısı sınırı ile yeniden bağlanmayı deneyebilir.
* `1011 - Internal Server Error`: Sunucudaki bir hata nedeniyle bağlantı sunucu tarafından kapatılacak.

### <a name="parameters"></a>Parametreler

|Parametre|Value|Açıklama|Parametre türü|Veri Türü|
|:---|:---|:---|:---|:---|
|API sürümü|1.0|İstemci tarafından istenen API 'nin sürümü. İzin verilen değerler: `1.0`.|query   |dize|
|from|olmamalıdır   |Gelen konuşmanın dilini belirtir. Bu değer, diller API 'sinden gelen yanıttaki `speech` kapsamdaki dil tanımlayıcılarından biridir.|query|dize|
|to|olmamalıdır|Metnin içine çevrilmesi için kullanılacak dili belirtir. Bu değer, diller API 'sinden gelen yanıttaki `text` kapsamdaki dil tanımlayıcılarından biridir.|query|dize|
|SaaS Uygulamaları Geliştirme|olmamalıdır   |İstemci tarafından seçilen, virgülle ayrılmış özellikler kümesi. Kullanılabilir özellikler şunlardır:<ul><li>`TextToSpeech`: hizmetin, son çevrilmiş tümcenin çevrilmiş sesini döndürmesi gerektiğini belirtir.</li><li>`Partial`: ses hizmete akış yaparken hizmetin ara tanıma sonuçlarını döndürmesi gerektiğini belirtir.</li><li>`TimingInfo`: hizmetin her bir tanıma ile ilişkili zamanlama bilgilerini döndürmesi gerektiğini belirtir.</li></ul>Örnek olarak, bir istemci kısmi sonuçları ve `features=partial,texttospeech` metinden konuşmaya alma, ancak zamanlama bilgisi yok olarak belirler. Son sonuçların istemciye her zaman akışı olduğunu unutmayın.|query|dize|
|Sına|olmamalıdır|Çevrilmiş metnin metinden konuşmaya işleme için kullanılacak sesi tanımlar. Değer, dil API 'sinden alınan bir TTS kapsamındaki ses tanımlayıcılarından biridir. Ses belirtilmediyse, metinden konuşmaya özelliği etkinleştirildiğinde sistem otomatik olarak bir tane seçer.|query|dize|
|format|olmamalıdır|Hizmet tarafından döndürülen metin okuma ses akışının biçimini belirtir. Kullanılabilen seçenekler:<ul><li>`audio/wav`: Waveform ses akışı. İstemci, ses biçimini doğru şekilde yorumlamak için WAV üstbilgisini kullanmalıdır. Metin okuma için WAV sesi, 16 bit, tek kanallı PCM ise, 24 kHz veya 16kHz örnekleme oranına sahiptir.</li><li>`audio/mp3`: MP3 ses akışı.</li></ul>`audio/wav` varsayılan değerdir.|query|dize|
|ProfanityAction    |olmamalıdır    |Hizmetin konuşmaya tanınan profanities nasıl işleneceğini belirtir. Geçerli eylemler şunlardır:<ul><li>`NoAction`: Profanities, olduğu gibi bırakılır.</li><li>`Marked`: Profanities, bir işaret ile değiştirilmiştir. Bkz `ProfanityMarker` . parametresi.</li><li>`Deleted`: Profanities silinir. Örneğin, sözcük `"jackass"` bir küfür olarak kabul edilir ifadesi `"He is a jackass."` şu şekilde olur`"He is a .".`</li></ul>Varsayılan değer Işaretlenir.|query|dize|
|ProfanityMarker|olmamalıdır    |`ProfanityAction` Olarak`Marked`ayarlandığında, algılanan profanities nasıl işleneceğini belirtir. Geçerli seçenekler şunlardır:<ul><li>`Asterisk`: Profanities, dizesiyle `***`değiştirilmiştir. Örneğin, sözcük `"jackass"` bir küfür olarak kabul edilir ifadesi `"He is a jackass."` şu şekilde olur`"He is a ***.".`</li><li>`Tag`: Küfür bir küfür XML etiketiyle çevrelenmiş. Örneğin, sözcük `"jackass"` bir küfür olarak kabul edilir ifadesi `"He is a jackass."` `"He is a <profanity>jackass</profanity>."`olur.</li></ul>Varsayılan, `Asterisk` değeridir.|query|dize|
|Authorization|olmamalıdır  |İstemcinin taşıyıcı belirtecinin değerini belirtir. Ön eki `Bearer` ve ardından kimlik doğrulama belirteci hizmeti tarafından `access_token` döndürülen değerin değerini kullanın.|header   |dize|
|Ocp-Apim-Subscription-Key|olmamalıdır|`Authorization` Üst bilgi belirtilmemişse gereklidir.|header|dize|
|access_token|olmamalıdır   |Geçerli bir OAuth erişim belirtecini geçirmenin alternatif yolu. Taşıyıcı belirteç genellikle üst `Authorization`bilgiyle birlikte sağlanır. Bazı WebSocket kitaplıkları istemci kodunun üst bilgileri ayarlamaya izin vermez. Böyle bir durumda, istemci geçerli bir belirteci geçirmek `access_token` için Query parametresini kullanabilir. Kimlik doğrulaması için bir erişim belirteci kullanırken, `Authorization` üst bilgi ayarlanmamışsa `access_token` , ayarlanması gerekir. Hem üst bilgi hem de sorgu parametresi ayarlandıysa, sorgu parametresi yok sayılır. İstemciler belirteci geçirmek için yalnızca bir yöntem kullanmalıdır.|query|dize|
|abonelik-anahtar|olmamalıdır   |Abonelik anahtarını geçirmenin alternatif yolu. Bazı WebSocket kitaplıkları istemci kodunun üst bilgileri ayarlamaya izin vermez. Böyle bir durumda, istemci geçerli bir abonelik anahtarını `subscription-key` geçirmek için Query parametresini kullanabilir. Kimlik doğrulaması için bir abonelik anahtarı kullanırken, `Ocp-Apim-Subscription-Key` üst bilgi ayarlanmamışsa, abonelik anahtarı ayarlanmalıdır. Hem üst bilgi hem de sorgu parametresi ayarlandıysa, sorgu parametresi yok sayılır. İstemciler, `subscription key`iletmek için yalnızca bir yöntem kullanmalıdır.|query|dize|
|X-Clienttraceıd    |olmamalıdır    |Bir isteği izlemek için kullanılan, istemci tarafından oluşturulan bir GUID. Sorunların düzgün şekilde giderilmesi için istemciler her bir istekle yeni bir değer sağlamalıdır ve günlüğe kaydeder.<br/>Bu değer, bir üst bilgi kullanmak yerine sorgu parametresi `X-ClientTraceId`ile geçirilebilir. Hem üst bilgi hem de sorgu parametresi ayarlandıysa, sorgu parametresi yok sayılır.|header|dize|
|X-Bağıntıkimliği|olmamalıdır    |Bir konuşmadaki birden çok kanalı ilişkilendirmek için kullanılan, istemci tarafından oluşturulan bir tanımlayıcı. Kullanıcılar arasında konuşmaları etkinleştirmek için birden çok konuşma çevirisi oturumu oluşturulabilir. Bu senaryoda, tüm konuşma çevirisi oturumları kanalları birbirine bağlamak için aynı bağıntı KIMLIĞINI kullanır. Bu, izleme ve tanılamayı kolaylaştırır. Tanımlayıcı şu şekilde uyumlu olmalıdır:`^[a-zA-Z0-9-_.]{1,64}$`<br/>Bu değer, bir üst bilgi kullanmak yerine sorgu parametresi `X-CorrelationId`ile geçirilebilir. Hem üst bilgi hem de sorgu parametresi ayarlandıysa, sorgu parametresi yok sayılır.|header|dize|
|X-ClientVersion|olmamalıdır    |İstemci uygulamasının sürümünü tanımlar. Örnek: "2.1.0.123".<br/>Bu değer, bir üst bilgi kullanmak yerine sorgu parametresi `X-ClientVersion`ile geçirilebilir. Hem üst bilgi hem de sorgu parametresi ayarlandıysa, sorgu parametresi yok sayılır.|header|dize|
|X-OsPlatform|olmamalıdır   |İstemci uygulamasının üzerinde çalıştığı işletim sisteminin adını ve sürümünü tanımlar. Örnekler: "Android 5,0", "iOs 8.1.3", "Windows 8.1".<br/>Bu değer, bir üst bilgi kullanmak yerine sorgu parametresi `X-OsPlatform`ile geçirilebilir. Hem üst bilgi hem de sorgu parametresi ayarlandıysa, sorgu parametresi yok sayılır.|header|dize|

### <a name="response-messages"></a>Yanıt iletileri

|HTTP durum kodu|Reason|Yanıt modeli|Üst bilgiler|
|:--|:--|:--|:--|
|101    |WebSocket yükseltmesi.|Model örneği değeri <br/> Nesne{}|X-RequestId<br/>Sorun giderme amacıyla isteği tanımlayan bir değer.<br/>dize|
|400    |Hatalı istek. Geçerli olduklarından emin olmak için giriş parametrelerini denetleyin. Response nesnesi, hatanın daha ayrıntılı bir açıklamasını içerir.|||
|401    |Erişilmesini. Kimlik bilgilerinin geçerli olduğundan ve Azure Data Market aboneliğinizin kullanılabilir bir dengelemeye uygun olduğundan emin olun.|||
|500    |Bir hata oluştu. Hata devam ederse, lütfen istemci izleme tanımlayıcısı (X-Clienttraceıd) veya istek tanımlayıcısı (X-RequestId) ile bildirin.|||
|503    |Sunucu geçici olarak kullanılamıyor. Lütfen isteği yeniden deneyin. Hata devam ederse, lütfen istemci izleme tanımlayıcısı (X-Clienttraceıd) veya istek tanımlayıcısı (X-RequestId) ile bildirin.|||

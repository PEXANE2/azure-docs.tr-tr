---
title: Konuşma hizmeti kapları sık sorulan sorular (SSS)
titleSuffix: Azure Cognitive Services
description: Konuşma kapsayıcılarını yükleyin ve çalıştırın. konuşmadan metne ses akışlarını, uygulamalarınızın, araçlarınızın veya aygıtlarınızın tüketebileceği veya görüntüleyebileceği gerçek zamanlı olarak metne aktarır. Metinden konuşmaya giriş metnini insan benzeri sentezlenmiş konuşmaya dönüştürür.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: aahi
ms.openlocfilehash: 17582244aef173da6ac700c980f7bd7fb0fec307
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383093"
---
# <a name="speech-service-containers-frequently-asked-questions-faq"></a>Konuşma hizmeti kapları sık sorulan sorular (SSS)

Konuşma hizmetini kapsayıcılarla kullanırken, destek lenmeden önce sık sorulan sorulardan oluşan bu koleksiyona güvenin. Bu makalede, genelden teknike değişen sorular ele geçirin. Yanıtı genişletmek için soruya tıklayın.

## <a name="general-questions"></a>Genel sorular

<details>
<summary>
<b>Konuşma kapsayıcıları nasıl çalışır ve bunları nasıl ayarlıyorum?</b>
</summary>

**Cevap:** Üretim kümesini ayarlarken göz önünde bulundurulması gereken birkaç şey vardır. İlk olarak, aynı makinede tek bir dil, birden fazla kapsayıcı kurma, büyük bir sorun olmamalıdır. Sorunlar la karşınıza bakıyorsanız, donanımla ilgili bir sorun olabilir - bu nedenle önce kaynağa bakacağız, yani; CPU ve bellek özellikleri.

Bir an için `ja-JP` düşünün, konteyner ve son model. Akustik model cpu açısından en zorlu parçadır, dil modeli ise en çok bellek gerektirir. Kullanımı kıyasladığımızda, ses gerçek zamanlı olarak (mikrofondan gibi) akarken tek bir konuşma-metin isteğini işlemek yaklaşık 0,6 CPU çekirdeği gerekir. Sesi gerçek zamanlıdan daha hızlı beslerseniz (dosyadaki gibi), bu kullanım iki katına çıkabilir (1.2x çekirdek). Bu arada, aşağıda listelenen bellek konuşma çözme için bellek işletim. Dosya önbelleğinde bulunan dil modelinin gerçek tam boyutunu hesaba *katmaz.* Bunun `ja-JP` için ek bir 2 GB; için, `en-US`daha fazla (6-7 GB) olabilir.

Belleğin az olduğu bir makineniz varsa ve üzerinde birden çok dil dağıtmaya çalışıyorsanız, dosya önbelleğinin dolu olması ve işletim sistemi modelleri içeri ve dışa sayfalamak zorunda kalmaktadır. Çalışan bir transkripsiyon için, bu felaket olabilir ve yavaşlamalar ve diğer performans etkilerine yol açabilir.

Ayrıca, [gelişmiş vektör uzantısı (AVX2)](speech-container-howto.md#advanced-vector-extension-support) talimat setine sahip makineler için çalıştırılabilir leri önceden paketliyoruz. AVX512 talimat kümesine sahip bir makine, bu hedef için kod oluşturma yı gerektirir ve 10 dil için 10 kapsayıcı başlatmak CPU'yu geçici olarak tüketebilir. Bunun gibi bir ileti docker günlüklerinde görünür:

```console
2020-01-16 16:46:54.981118943 
[W:onnxruntime:Default, tvm_utils.cc:276 LoadTVMPackedFuncFromCache]
Cannot find Scan4_llvm__mcpu_skylake_avx512 in cache, using JIT...
```

Son olarak, değişkeni kullanarak `DECODER MAX_COUNT` *tek* bir kapsayıcı içinde istediğiniz kod çözücü sayısını ayarlayabilirsiniz. Yani, temelde, sizin SKU (CPU / bellek) ile başlamalı ve biz nasıl en iyi şekilde almak için önerebilirsiniz. Büyük bir başlangıç noktası önerilen ana makine kaynak özellikleri atıfta olduğunu.

<br>
</details>

<details>
<summary>
<b>Kapasite planlaması ve prem Konuşma kaplarının maliyet tahmini konusunda yardımcı olabilir misiniz?</b>
</summary>

**Cevap:** Toplu işlem modunda kapsayıcı kapasitesi için, her kod çözücü tek bir tanıma için iki CPU çekirdeğiyle gerçek zamanlı olarak 2-3x işleyebilir. Konteyner örneğine göre ikiden fazla eşzamanlı tanıma tutmanızı önermiyoruz, ancak yük dengeleyicisinin arkasında güvenilirlik/kullanılabilirlik nedenleriyle daha fazla kapsayıcı örneği çalıştırmanızı öneririz.

Yine de her bir konteyner örneğini daha fazla kod çözücüyle çalıştırabiliriz. Örneğin, konteyner örneğine göre 7 kod çözücüyi sekiz çekirdekli bir makineye (her biri 2 x'ten fazla) ayarlayabilir ve 15 kat verim elde edebilir. Dikkat edilmesi gereken `DECODER_MAX_COUNT` bir param var. Aşırı durumda, güvenilirlik ve gecikme sorunları ortaya çıkar, iş artışı ile önemli ölçüde. Bir mikrofon için, 1x gerçek zamanlı olacak. Genel kullanım tek bir tanıma için yaklaşık bir çekirdek olmalıdır.

Toplu işlem modunda 1 K saat/gün işleme senaryosu için, aşırı bir durumda, 3 VM 24 saat içinde işleyebilir, ancak garanti edilmez. Başak günlerini işlemek, başarısız olmak, güncellemek ve minimum yedekleme/BCP sağlamak için, küme başına 3 yerine ve 2+ kümeyle 4-5 makine öneririz.

Donanım için, standart Azure `DS13_v2` VM'yi referans olarak kullanırız (her çekirdek 2,6 GHz veya daha iyi olmalıdır, AVX2 talimat seti etkinleştirilmiş olmalıdır).

| Örnek  | vCPU(lar) | RAM    | Geçici depolama | AHB ile istediğiniz gibi öde | AHB (%Tasarruf) ile 1 yıllık rezerv | AHB (%Savings) ile 3 yıl ayrılmıştır |
|-----------|---------|--------|--------------|------------------------|-------------------------------------|--------------------------------------|
| `DS13 v2` | 8       | 56 GİB | 112 GİB      | $0.598/saat            | $0.3528/saat (~41%)                 | $0.2333/saat (~61%)                  |

Tasarım referansına göre (1 K saat/gün ses toplu işleme işlemek için 5 VM'den oluşan iki küme), 1 yıllık donanım maliyeti olacaktır:

> 2 (kümeler) * 5 (küme başına VM) * $0.3528/saat * 365 (gün) * 24 (saat) = $31K / yıl

Fiziksel makineye eşleme yaparken, genel bir tahmin 1 vCPU = 1 Fiziksel CPU Core'dur. Gerçekte, 1vCPU tek bir çekirdek daha güçlüdür.

On-prem için, tüm bu ek faktörler devreye girer:

- Fiziksel CPU'nun türünde ve üzerinde kaç çekirdek
- Aynı kutuda/makinede kaç CPU birlikte çalışır
- VM'ler nasıl ayarlanır?
- Nasıl hiper-threading / multi-threading kullanılır
- Bellek nasıl paylaşılır?
- İşletim sistemi, vb.

Normalde ortam Azure kadar iyi ayarlanmaz. Diğer genel overover göz önüne alındığında, güvenli bir tahmin 10 fiziksel CPU çekirdek = 8 Azure vCPU olduğunu söyleyebilirim. Popüler CPU'lar sadece sekiz çekirdek olmasına rağmen. Ön hazırlık dağıtımında maliyet, Azure VM'lerini kullanmaktan daha yüksek olacaktır. Ayrıca, amortisman oranını da göz önünde bulundurun.

Hizmet maliyeti çevrimiçi hizmetle aynıdır: konuşmadan metne kadar olan saat için $1/saat. Konuşma hizmet maliyeti:

> $1 * 1000 * 365 = $365K

Microsoft'a ödenen bakım maliyeti, hizmet düzeyine ve hizmetin içeriğine bağlıdır. Bu yerinde hizmet dahil ise yüz binlerce temel düzeyi için 29,99 $ /ay çeşitli. Kaba bir sayı hizmet / korumak için 300 $ / saat. İnsanlar maliyeti dahil değildir. Diğer altyapı maliyetleri (depolama, ağlar ve yük dengeleyicileri gibi) dahil edilmez.

<br>
</details>

<details>
<summary>
<b>Transkripsiyonda noktalama işaretleri neden eksik?</b>
</summary>

**Cevap:** Karbon `speech_recognition_language=<YOUR_LANGUAGE>` istemcisi kullanıyorlarsa istek açıkça yapılandırılmalıdır.

Örneğin:

```python
if not recognize_once(
    speechsdk.SpeechRecognizer(
        speech_config=speechsdk.SpeechConfig(
            endpoint=template.format("interactive"),
            speech_recognition_language="ja-JP"),
            audio_config=audio_config)):

    print("Failed interactive endpoint")
    exit(1)
```
Çıktı şöyledir:

```cmd
RECOGNIZED: SpeechRecognitionResult(
    result_id=2111117c8700404a84f521b7b805c4e7, 
    text="まだ早いまだ早いは猫である名前はまだないどこで生まれたかとんと見当を検討をなつかぬ。
    何でも薄暗いじめじめした所でながら泣いていた事だけは記憶している。
    まだは今ここで初めて人間と言うものを見た。
    しかも後で聞くと、それは書生という人間中で一番同額同額。",
    reason=ResultReason.RecognizedSpeech)
```

<br>
</details>

<details>
<summary>
<b>Konuşma kapsayıcılı özel bir akustik model ve dil modeli kullanabilir miyim?</b>
</summary>

Şu anda sadece bir model kimliği, özel dil modeli veya özel akustik model geçmek edebiliyoruz.

**Cevap:** Hem akustik hem de dil modellerini aynı anda *desteklememe* kararı alındı. Bu, API aralarını azaltmak için birleşik bir tanımlayıcı oluşturulana kadar geçerli kalır. Yani, ne yazık ki bu şu anda desteklenmiyor.

<br>
</details>

<details>
<summary>
<b>Bu hataları özel konuşma-metin kapsayıcısından açıklayabilir misiniz?</b>
</summary>

**Hata 1:**

```cmd
Failed to fetch manifest: Status: 400 Bad Request Body:
{
    "code": "InvalidModel",
    "message": "The specified model is not supported for endpoint manifests."
}
```

**Cevap 1:** En son özel modelle eğitim arıyorsanız, şu anda bunu destekliyoruz. Eski bir sürümü ile eğitim varsa, kullanmak mümkün olmalıdır. En son sürümleri desteklemeye devam ediyoruz.

Esasen, özel kaplar Halide veya ONNX tabanlı akustik modelleri (özel eğitim portalında varsayılan olan) desteklemez. Bunun nedeni özel modellerin şifrelenmemesidir ve ONNX modellerini ifşa etmek istemiyoruz; dil modelleri iyi. Müşterinin özel eğitim için açıkça ONNX olmayan eski bir modeli seçmesi gerekir. Doğruluk etkilenmez. Model boyutu daha büyük olabilir (100 MB ile).

> Destek modeli > 20190220 (v4.5 Unified)

**Hata 2:**

```cmd
HTTPAPI result code = HTTPAPI_OK.
HTTP status code = 400.
Reason:  Synthesis failed.
StatusCode: InvalidArgument,
Details: Voice does not match.
```

**Cevap 2:** Büyük/küçük harf duyarlı istekte doğru ses adını sağlamanız gerekir. Tam hizmet adı eşleneme bakın. Şu anda `en-US-JessaRUS`metinden konuşmaya kapsayıcı sürümünde bulunmadığı için `en-US-JessaNeural` kullanmanız gerekir.

**Hata 3:**

```json
{
    "code": "InvalidProductId",
    "message": "The subscription SKU \"CognitiveServices.S0\" is not supported in this service instance."
}
```

**Cevap 3:** Bilişsel Hizmetler kaynağı değil, konuşma kaynağı oluşturmak için saz.


<br>
</details>

<details>
<summary>
<b>Hangi API protokolleri desteklenir, REST veya WS?</b>
</summary>

**Cevap:** Konuşmadan metne ve özel konuşmadan metne kapsayıcılar için, şu anda yalnızca websocket tabanlı protokolü destekliyoruz. SDK sadece WS arama destekler ama REST değil. REST desteği eklemek için bir plan var, ama şu an için ETA değil. Her zaman resmi belgelere bakın, [sorgu tahmin uç noktalarına](speech-container-howto.md#query-the-containers-prediction-endpoint)bakın.

<br>
</details>

<details>
<summary>
<b>CentOS Konuşma kapsayıcıları için desteklenir mi?</b>
</summary>

**Cevap:** CentOS 7 henüz Python SDK tarafından desteklenmez, ayrıca Ubuntu 19.04 desteklenmez.

Python Speech SDK paketi bu işletim sistemleri için kullanılabilir:
- **Windows** - x64 ve x86
- **Mac** - macOS X sürüm 10.12 veya sonrası
- **Linux** - Ubuntu 16.04, Ubuntu 18.04, Debian 9 x64 üzerinde

Ortam kurulumu hakkında daha fazla bilgi için [Python platform kurulumuna](quickstarts/setup-platform.md?pivots=programming-language-python)bakın. Şimdilik, Ubuntu 18.04 önerilen sürümüdür.

<br>
</details>

<details>
<summary>
<b>LUIS tahmin bitiş noktalarını çağırmaya çalışırken neden hata alıyorum?</b>
</summary>

Bir IoT Edge dağıtım LUIS konteyner kullanıyorum ve başka bir kapsayıcıdan LUIS tahmin bitiş noktası aramak için çalışıyorum. LUIS konteyner port 5001 dinliyor ve kullandığım URL şudur:

```csharp
var luisEndpoint =
    $"ws://192.168.1.91:5001/luis/prediction/v3.0/apps/{luisAppId}/slots/production/predict";
var config = SpeechConfig.FromEndpoint(new Uri(luisEndpoint));
```

Ben alıyorum hata:

```cmd
WebSocket Upgrade failed with HTTP status code: 404 SessionId: 3cfe2509ef4e49919e594abf639ccfeb
```

Ben LUIS konteyner günlükleri istek görmek ve mesaj diyor:

```cmd
The request path /luis//predict" does not match a supported file type.
```

Bu ne anlama geliyor? Neyi kaçırıyorum? Ben konuşma SDK için örnek takip edildi, [buradan](https://github.com/Azure-Samples/cognitive-services-speech-sdk). Senaryo, sesi doğrudan PC mikrofonundan algılamamız ve eğittiğimiz LUIS uygulamasına dayanarak amacı belirlemeye çalışmamızdır. Ben bağlı örnek tam olarak bunu yapar. Ve LUIS bulut tabanlı hizmet ile iyi çalışır. Konuşma SDK kullanarak konuşma-to-metin API ve daha sonra LUIS için ikinci bir çağrı için ayrı bir açık arama yapmak zorunda bizi kurtarmak için görünüyordu.

Bu yüzden, tüm yapmaya çalışıyorum luis konteyner kullanarak bulut luis kullanma senaryosu geçiş. SDK'nın biri için işe yarayıp yaramadığını hayal bile edemiyorum.

**Cevap:** Konuşma SDK bir LUIS konteyner karşı kullanılmamalıdır. LUIS konteynerini kullanmak için LUIS SDK veya LUIS REST API kullanılmalıdır. Konuşma SDK bir konuşma konteyner karşı kullanılmalıdır.

Bulut, bir kapsayıcıdan farklıdır. Bir bulut birden çok birleştirilmiş kapsayıcıdan (bazen mikro hizmetler olarak da adlandırılır) oluşabilir. Yani bir LUIS konteyner ve sonra bir Konuşma konteyner - İki ayrı konteyner. Konuşma kapsayıcısı yalnızca konuşma yapar. LUIS konteyner sadece LUIS yok. Bulutta, her iki kapsayıcının da dağıtıldığı bilindiği nden ve uzak bir istemcinin buluta gitmesi, konuşma yapması, geri dönmesi, sonra tekrar buluta gitmesi ve LUIS yapması kötü bir performans olduğundan, istemcinin Konuşma'ya gitmesini, bulutta kalmasını, LUIS'e gitmesini ve ardından istemciye geri dönmesini sağlayan bir özellik sağlıyoruz. Böylece bu senaryoda bile Konuşma SDK ses ile Konuşma bulut konteyner gider ve daha sonra metin ile LUIS bulut konteyner konuşma bulut konteyner görüşmeleri. LUIS konteyner ses kabul hiçbir kavramı vardır (luis konteyner ses akışı kabul etmek için mantıklı olmaz - LUIS metin tabanlı bir hizmettir). On-prem ile, müşterimizin her iki konteynırı da dağıttıklarından emin değiliz, müşterilerimizin tesislerindeki konteynerler arasında düzenlemeyi düşünmüyoruz ve her iki konteyner de prem üzerinde dağıtılırsa, müşteriye daha yerel oldukları göz önüne alındığında, önce SR'ye geri dönmek ve müşterinin bu metni alıp LUIS'e gitmesini sağlamak bir yük değildir.

<br>
</details>

<details>
<summary>
<b>MacOS, Speech konteyneri ve Python SDK ile neden hata yapıyoruz?</b>
</summary>

Bir *.wav* dosyasını yazıya yazdıracak şekilde gönderdiğimizde, sonuç aşağıdakilerle birlikte gelir:

```cmd
recognition is running....
Speech Recognition canceled: CancellationReason.Error
Error details: Timeout: no recognition result received.
When creating a websocket connection from the browser a test, we get:
wb = new WebSocket("ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1")
WebSocket
{
    url: "ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1",
    readyState: 0,
    bufferedAmount: 0,
    onopen: null,
    onerror: null,
    ...
}
```

Websocket'in doğru kurulduğunu biliyoruz.

**Cevap:** Bu durumda, o zaman [bu GitHub sorunu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/310)bakın. [Burada bir](https://github.com/Azure-Samples/cognitive-services-speech-sdk/issues/310#issuecomment-527542722)çalışmamız var.

Karbon bunu sürüm 1.8'de düzeltti.


<br>
</details>

<details>
<summary>
<b>Konuşma kapsayıcı uç noktalarındaki farklar nelerdir?</b>
</summary>

Hangi işlevlerin test edilebildiği ve SDK ve REST API'lerinin nasıl test edilebildiği de dahil olmak üzere aşağıdaki test ölçümlerini doldurmanıza yardımcı olabilir misiniz? Özellikle, "interaktif" ve "konuşma" farklılıkları, ben mevcut doc / örnek görmedim.

| Uç Nokta                                                | Fonksiyonel test                                                   | SDK | REST API |
|---------------------------------------------------------|-------------------------------------------------------------------|-----|----------|
| `/speech/synthesize/cognitiveservices/v1`               | Metni Sentezleme (metinden konuşmaya)                                  |     | Evet      |
| `/speech/recognition/dictation/cognitiveservices/v1`    | Bilişsel Hizmetler on-prem dikte v1 websocket uç noktası        | Evet | Hayır       |
| `/speech/recognition/interactive/cognitiveservices/v1`  | Bilişsel Hizmetler on-prem interaktif v1 websocket uç noktası  |     |          |
| `/speech/recognition/conversation/cognitiveservices/v1` | Bilişsel hizmetler on-prem konuşma v1 websocket bitiş noktası |     |          |

**Cevap:** Bu bir füzyon:
- Kapsayıcılar için dikte bitiş noktasını deneyen kişiler, (bu URL'yi nasıl aldıklarından emin değilim)
- 1.<sup>st</sup> parti bitiş noktası bir kapsayıcıda olan.
- 1.<sup>st</sup> parti bitiş noktası dönen konuşma.iletiler `speech.hypothesis` yerine parça iletiler 3<sup>bölüm</sup> uç noktaları dikte bitiş noktası için geri döner.
- Karbon quickstarts tüm `RecognizeOnce` kullanım (etkileşimli mod)
- Karbon, etkileşimli `speech.fragment` modda döndürülmelerini gerektiren iletiler için bir iddiaya sahip.
- Karbon serbest iddia yangın sahip oluşturur (süreci öldürme).

Geçici çözüm, kodunuzda sürekli tanıma kullanmaya geçmek veya (daha hızlı) kapsayıcıdaki etkileşimli veya sürekli uç noktalara bağlanmaktır.
Kodunuz için bitiş noktasını <ana bilgisayar olarak ayarlayın: port>/konuşma/tanıma/etkileşimli/bilişsel hizmetler/v1

Çeşitli modlar için Konuşma modlarına bakın - aşağıya bakın:

[!INCLUDE [speech-modes](includes/speech-modes.md)]

Uygun düzeltme SDK 1.8, on-prem desteği (sağ bitiş noktası seçecek, bu yüzden online hizmet daha kötü olacak) ile geliyor. Bu arada, sürekli tanınmak için bir örnek var, neden işaret etmiyoruz?

https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/6805d96bf69d9e95c9137fe129bc5d81e35f6309/samples/python/console/speech_sample.py#L196

<br>
</details>

<details>
<summary>
<b>Çeşitli ses dosyaları için hangi modu kullanmalıyım?</b>
</summary>

**Cevap:** Python'u kullanmaya hızlı bir [başlangıç.](quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-python) Dokümanlar sitesinde bağlantılı diğer dilleri bulabilirsiniz.

Sadece interaktif, konuşma ve dikte için açıklığa kavuşturmak için; bu, hizmetimizin konuşma isteğini nasıl ele alacağı konusunda gelişmiş bir yoldur. Ne yazık ki, on-prem kaplar için biz tam URI belirtmek zorunda (yerel makine içerdiğinden), bu yüzden bu bilgi soyutlama sızdırılmış. Bunu gelecekte daha kullanılabilir hale getirmek için SDK ekibiyle birlikte çalışıyoruz.

<br>
</details>

<details>
<summary>
<b>İşlemlerin kaba bir ölçüsünü/saniye/çekirdeki nasıl kıyaslayabiliriz?</b>
</summary>

**Cevap:** Burada mevcut modelden beklemek kaba sayılar bazı (biz GA gemi olacak bir daha iyi değişecektir):

- Dosyalar için, azaltma konuşma SDK, 2x olacaktır. İlk beş saniyelik ses azaltılmadı. Decoder yaklaşık 3x gerçek zamanlı yapma yeteneğine sahiptir. Bunun için, genel CPU kullanımı tek bir tanıma için 2 çekirdek yakın olacaktır.
- Mikrofon için, 1x gerçek zamanlı olacak. Genel kullanım tek bir tanıma için yaklaşık 1 çekirdek olmalıdır.

Bunların tümü docker günlüklerinden doğrulanabilir. Biz aslında oturum ve ifade / söyleyiş istatistikleri ile satır dökümü ve RTF numaraları içerir.


<br>
</details>

<details>
<summary>
<b>Konuşma kapsayıcısı kullanımı için ses dosyalarını chuck'lara bölmek yaygın mıdır?</b>
</summary>

Benim şimdiki planı varolan bir ses dosyası almak ve 10 saniye parçalara bölmek ve konteyner yoluyla göndermektir. Bu kabul edilebilir bir senaryo mu?  Kapsayıcı ile daha büyük ses dosyalarını işlemek için daha iyi bir yol var mı?

**Cevap:** Sadece konuşma SDK kullanın ve dosya vermek, o doğru olanı yapacaktır. Neden dosyayı parçalamak gerekiyor?


<br>
</details>

<details>
<summary>
<b>Birden çok kapsayıcının aynı ana bilgisayarda çalışmasını nasıl sağlayabilirim?</b>
</summary>

Doktor farklı bir bağlantı noktasını ortaya çıkarmamı söylüyor, ki ben öyle, ama LUIS konteynırı hala 5000 portu dinliyor mu?

**Cevap:** Dene. `-p <outside_unique_port>:5000` Örneğin, `-p 5001:5000`.


<br>
</details>

## <a name="technical-questions"></a>Teknik sorular

<details>
<summary>
<b>15 saniye uzunluğundaki sesi &lt;işlemek için toplu olmayan API'leri nasıl alabilirim?</b>
</summary>

**Cevap:** `RecognizeOnce()` Etkileşimli modda yalnızca 15 saniyeye kadar ses işler, çünkü mod, konuşmaların kısa olmasının beklendiği Konuşma Komutu için tasarlanmıştır. Dikte veya `StartContinuousRecognition()` konuşma için kullanıyorsanız, 15 saniye sınırı yoktur.


<br>
</details>

<details>
<summary>
<b>Önerilen kaynaklar, CPU ve RAM nelerdir; 50 eşzamanlı istek için?</b>
</summary>

4 çekirdekli, 4 GB RAM kaç eşzamanlı istek işleyecek? Örneğin, 50 eşzamanlı istek, kaç Çekirdek ve RAM önerilir hizmet etmek zorunda?

**Cevap:** Gerçek zamanlı olarak, 8 `en-US`bizim son ile , bu yüzden 6 eşzamanlı istekleri ötesinde daha fazla docker konteyner kullanmanızı öneririz. 16 çekirdeğin üzerinde daha çılgın hale gelir ve tek düze olmayan bellek erişimi (NUMA) düğümü duyarlı hale gelir. Aşağıdaki tabloda, her Konuşma kapsayıcısı için en az ve önerilen kaynak tahsisi açıklanmaktadır.

# <a name="speech-to-text"></a>[Konuşmayı metne dönüştürme](#tab/stt)

| Kapsayıcı      | Minimum             | Önerilen         |
|----------------|---------------------|---------------------|
| Konuşmayı metne dönüştürme | 2 çekirdekli, 2 GB bellek | 4 çekirdekli, 4 GB bellek |

# <a name="custom-speech-to-text"></a>[Özel Konuşma-metin](#tab/cstt)

| Kapsayıcı             | Minimum             | Önerilen         |
|-----------------------|---------------------|---------------------|
| Özel Konuşma-metin | 2 çekirdekli, 2 GB bellek | 4 çekirdekli, 4 GB bellek |

# <a name="text-to-speech"></a>[Metin okuma](#tab/tts)

| Kapsayıcı      | Minimum             | Önerilen         |
|----------------|---------------------|---------------------|
| Metin okuma | 1 çekirdekli, 2 GB bellek | 2 çekirdekli, 3 GB bellek |

# <a name="custom-text-to-speech"></a>[Özel Metinden konuşmaya](#tab/ctts)

| Kapsayıcı             | Minimum             | Önerilen         |
|-----------------------|---------------------|---------------------|
| Özel Metinden konuşmaya | 1 çekirdekli, 2 GB bellek | 2 çekirdekli, 3 GB bellek |

***

- Her çekirdek en az 2,6 GHz veya daha hızlı olmalıdır.
- Dosyalar için, azaltma Konuşma SDK olacak, 2x (ses ilk 5 saniye daraltılmış değildir).
- Kod çözücü yaklaşık 2-3x gerçek zamanlı yapma yeteneğine sahiptir. Bunun için, genel CPU kullanımı tek bir tanıma için iki temele yakın olacaktır. Bu nedenle, konteyner örneğine göre ikiden fazla etkin bağlantı tutmanızı önermiyoruz. Aşırı tarafı gibi `DS13_V2`sekiz çekirdekli makinede 2x gerçek zamanlı olarak yaklaşık 10 kod çözücüler koymak olacaktır. Kapsayıcı sürüm 1.3 ve sonrası için, ayardeneyleyebileceğiniz `DECODER_MAX_COUNT=20`bir param var.
- Mikrofon için, 1x gerçek zamanlı olacak. Genel kullanım tek bir tanıma için yaklaşık bir çekirdek olmalıdır.

Sahip olduğunuz toplam ses saatsayısını göz önünde bulundurun. Sayı büyükse, güvenilirliği/kullanılabilirliği artırmak için, bir yük dengeleyicisinin arkasında, tek bir kutuda veya birden çok kutuda daha fazla kapsayıcı örneği çalıştırmanızı öneririz. Orkestrasyon Kubernetes (K8S) ve Helm kullanılarak veya Docker beste ile yapılabilir.

Örnek olarak, 1000 saat/24 saat işlemek için, VM başına 10 örnek/kod çözücü içeren 3-4 VM'ler ayarlamayı denedik.

<br>
</details>

<details>
<summary>
<b>Konuşma kapsayıcısı noktalama işaretlerini destekliyor mu?</b>
</summary>

**Cevap:** Prem kabında büyük harf (ITN) mevcuttur. Noktalama işaretleri dile bağlıdır ve Çince ve Japonca dahil olmak üzere bazı diller için desteklenmez.

*Varolan* kapsayıcılar için örtük ve temel noktalama işaretleme desteğimiz vardır, ancak varsayılan olarak böyledir. `off` Bunun anlamı, `.` karakteri örneğinize alabilirsiniz, ancak `。` karakteri elde edemezsiniz. Bu örtük mantığı etkinleştirmek için, Python'da Konuşma SDK'mızı kullanarak nasıl yapılacağının bir örneği verilmiştir (diğer dillerde benzer olacaktır):

```python
speech_config.set_service_property(
    name='punctuation',
    value='implicit',
    channel=speechsdk.ServicePropertyChannel.UriQueryParameter
)
```

<br>
</details>

<details>
<summary>
<b>Metinden metne veri göndermeye çalışırken neden 404 hata alıyorum?</b>
</summary>

Burada bir örnek HTTP POST:

```http
POST /speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codecs=audio/pcm; samplerate=16000
Transfer-Encoding: chunked
User-Agent: PostmanRuntime/7.18.0
Cache-Control: no-cache
Postman-Token: xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
Host: 10.0.75.2:5000
Accept-Encoding: gzip, deflate
Content-Length: 360044
Connection: keep-alive
HTTP/1.1 404 Not Found
Date: Tue, 22 Oct 2019 15:42:56 GMT
Server: Kestrel
Content-Length: 0
```

**Cevap:** Rest API'yi her iki konuşma-metin kapsayıcısında da desteklemiyoruz, yalnızca Konuşma SDK aracılığıyla WebSockets'i destekliyoruz. Her zaman resmi belgelere bakın, [sorgu tahmin uç noktalarına](speech-container-howto.md#query-the-containers-prediction-endpoint)bakın.

<br>
</details>

<details>
<summary>
<b>Konuşmadan metne hizmeti kullanırken, neden bu hatayı alıyorum?</b>
</summary>

```cmd
Error in STT call for file 9136835610040002161_413008000252496:
{
    "reason": "ResultReason.Canceled",
    "error_details": "Due to service inactivity the client buffer size exceeded. Resetting the buffer. SessionId: xxxxx..."
}
```

**Cevap:** Bu genellikle sesi Konuşma tanıma kapsayıcısının kaldırabileceğinden daha hızlı beslediğinizde gerçekleşir. İstemci arabellekleri doluyor ve iptal tetiklenir. Eşzamanlılığı ve sesi gönderdiğiniz RTF'yi kontrol etmeniz gerekir.

<br>
</details>

<details>
<summary>
<b>C++ örneklerinden bu metin-konuşma kapsayıcıhatalarını açıklayabilir misiniz?</b>
</summary>

**Cevap:** Kapsayıcı sürümü 1,3'ten eskiyse, bu kod kullanılmalıdır:

```cpp
const auto endpoint = "http://localhost:5000/speech/synthesize/cognitiveservices/v1";
auto config = SpeechConfig::FromEndpoint(endpoint);
auto synthesizer = SpeechSynthesizer::FromConfig(config);
auto result = synthesizer->SpeakTextAsync("{{{text1}}}").get();
```

Eski kapsayıcılar, Karbon'un `FromHost` API ile çalışması için gerekli bitiş noktasına sahip değildir. Sürüm 1.3 için kullanılan kapsayıcılar ise, bu kod kullanılmalıdır:

```cpp
const auto host = "http://localhost:5000";
auto config = SpeechConfig::FromHost(host);
config->SetSpeechSynthesisVoiceName(
    "Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)");
auto synthesizer = SpeechSynthesizer::FromConfig(config);
auto result = synthesizer->SpeakTextAsync("{{{text1}}}").get();
```

Aşağıda `FromEndpoint` API kullanarak bir örnektir:

```cpp
const auto endpoint = "http://localhost:5000/cognitiveservices/v1";
auto config = SpeechConfig::FromEndpoint(endpoint);
config->SetSpeechSynthesisVoiceName(
    "Microsoft Server Speech Text to Speech Voice (en-US, AriaRUS)");
auto synthesizer = SpeechSynthesizer::FromConfig(config);
auto result = synthesizer->SpeakTextAsync("{{{text2}}}").get();
```

 İşlev, `SetSpeechSynthesisVoiceName` güncelleştirilmiş metin den konuşmaya motoru olan kapsayıcılar ses adını gerektirdiğinden çağrılır.

<br>
</details>

<details>
<summary>
<b>Konuşma SDK'sının v1.7'sini Konuşma kapsayıcısı ile nasıl kullanabilirim?</b>
</summary>

**Cevap:** Konuşma kapsayıcısı üzerinde farklı kullanımlar için üç uç nokta vardır, bunlar Konuşma modları olarak tanımlanır - aşağıya bakın:

[!INCLUDE [speech-modes](includes/speech-modes.md)]

Bunlar farklı amaçlar için ve farklı kullanılır.

Python [örnekleri](https://github.com/Azure-Samples/cognitive-services-speech-sdk/blob/master/samples/python/console/speech_sample.py):
- Özel bir bitiş noktası olan tek tanıma (etkileşimli mod) için (diğer bir süre; `SpeechConfig` bir bitiş noktası parametresi ile), bkz. `speech_recognize_once_from_file_with_custom_endpoint_parameters()`
- Sürekli tanıma (konuşma modu) ve sadece yukarıdaki gibi özel `speech_recognize_continuous_from_file()`bir bitiş noktası kullanmak için değiştirin, bkz.
- Yukarıdaki gibi örneklerde dikte yi etkinleştirmek için (yalnızca gerçekten `speech_config`ihtiyacınız varsa), oluşturduktan hemen sonra , kod `speech_config.enable_dictation()`ekleyin.

C# dikteetkinleştirmek için işlevi `SpeechConfig.EnableDictation()` çağırın.

### <a name="fromendpoint-apis"></a>`FromEndpoint`Apı 'leri
| Dil | API ayrıntıları |
|----------|:------------|
| C++ | <a href="https://docs.microsoft.com/en-us/cpp/cognitive-services/speech/speechconfig#fromendpoint" target="_blank">`SpeechConfig::FromEndpoint` <span class="docon docon-navigate-external x-hidden-focus"></span></a> |
| C# | <a href="https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.fromendpoint?view=azure-dotnet" target="_blank">`SpeechConfig.FromEndpoint` <span class="docon docon-navigate-external x-hidden-focus"></span></a> |
| Java | <a href="https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig.fromendpoint?view=azure-java-stable" target="_blank">`SpeechConfig.fromendpoint` <span class="docon docon-navigate-external x-hidden-focus"></span></a> |
| Objective-C | <a href="https://docs.microsoft.com/en-us/objectivec/cognitive-services/speech/spxspeechconfiguration#initwithendpoint" target="_blank">`SPXSpeechConfiguration:initWithEndpoint;` <span class="docon docon-navigate-external x-hidden-focus"></span></a> |
| Python | <a href="https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python" target="_blank">`SpeechConfig;` <span class="docon docon-navigate-external x-hidden-focus"></span></a> |
| JavaScript | Şu anda desteklenmiyor ve planlanmıyor. |

<br>
</details>

<details>
<summary>
<b>Konuşma SDK'sının v1.8'ini Konuşma kapsayıcısı ile nasıl kullanabilirim?</b>
</summary>

**Cevap:** Yeni `FromHost` bir API var. Bu, varolan API'ların yerini almaz veya değiştirmez. Sadece özel bir ana bilgisayar kullanarak bir konuşma config oluşturmak için alternatif bir yol ekler.

### <a name="fromhost-apis"></a>`FromHost`Apı 'leri

| Dil | API ayrıntıları |
|--|:-|
| C# | <a href="https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig.fromhost?view=azure-dotnet" target="_blank">`SpeechConfig.FromHost` <span class="docon docon-navigate-external x-hidden-focus"></span></a> |
| C++ | <a href="https://docs.microsoft.com/en-us/cpp/cognitive-services/speech/speechconfig#fromhost" target="_blank">`SpeechConfig::FromHost` <span class="docon docon-navigate-external x-hidden-focus"></span></a> |
| Java | <a href="https://docs.microsoft.com/java/api/com.microsoft.cognitiveservices.speech.speechconfig.fromhost?view=azure-java-stable" target="_blank">`SpeechConfig.fromHost` <span class="docon docon-navigate-external x-hidden-focus"></span></a> |
| Objective-C | <a href="https://docs.microsoft.com/en-us/objectivec/cognitive-services/speech/spxspeechconfiguration#initwithhost" target="_blank">`SPXSpeechConfiguration:initWithHost;` <span class="docon docon-navigate-external x-hidden-focus"></span></a> |
| Python | <a href="https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python" target="_blank">`SpeechConfig;` <span class="docon docon-navigate-external x-hidden-focus"></span></a> |
| JavaScript | Şu anda desteklenmiyor |

> Parametreler: ana bilgisayar (zorunlu), abonelik anahtarı (isteğe bağlı, hizmeti onsuz kullanabiliyorsanız).

Ana bilgisayar `protocol://hostname:port` `:port` için biçim isteğe bağlıdır (aşağıya bakın):
- Kapsayıcı yerel olarak çalışıyorsa, ana `localhost`bilgisayar adı .
- Kapsayıcı uzak bir sunucuda çalışıyorsa, söz sunucunun ana bilgisayar adını veya IPv4 adresini kullanın.

Konuşmadan metne göre ana parametre örneklerini barındır:
- `ws://localhost:5000`- 5000 portu kullanarak yerel bir konteynere güvenli olmayan bağlantı
- `ws://some.host.com:5000`- Uzak bir sunucuda çalışan bir kapsayıcıya güvenli olmayan bağlantı

Python örnekleri yukarıdan, `host` ancak parametre yerine `endpoint`kullanın:

```python
speech_config = speechsdk.SpeechConfig(host="ws://localhost:5000")
```

<br>
</details>

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilişsel Hizmetler konteynerler](speech-container-howto.md)

---
title: Çağrı Merkezi Transkripsiyon - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma-metin için ortak bir senaryo, Etkileşimli Sesli Yanıt (IVR) gibi çeşitli sistemlerden gelen büyük hacimli telefon verilerini aktarıyor. Konuşma hizmetini ve Birleşik konuşma modelini kullanan bir işletme, ses yakalama sistemleriyle yüksek kaliteli transkripsiyonlar alabilir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: d959f4948d6b848f3b399c1310add06991d72012
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74806329"
---
# <a name="speech-service-for-telephony-data"></a>Telefon verileri için konuşma hizmeti

Sabit hatlar, cep telefonları ve radyolar aracılığıyla oluşturulan telefon verileri genellikle düşük kalitelidir ve 8 KHz aralığında dar banttır ve bu da konuşmadan metne dönüştürürken zorluklar yaratır. Konuşma hizmetinin en son konuşma tanıma modelleri, verilerin anlaşılmasının zor olduğu durumlarda bile bu telefon verilerini aktarmada başarılı olur. Bu modeller büyük hacimli telefon verileriyle eğitilmiştir ve gürültülü ortamlarda bile piyasadaki en iyi tanıma doğruluğuna sahiptir.

Konuşma-metin için ortak bir senaryo, Etkileşimli Sesli Yanıt (IVR) gibi çeşitli sistemlerden gelebilecek büyük hacimli telefon verilerini aktarıyor. Bu sistemlerin sağladığı ses stereo veya mono olabilir ve sinyal üzerinde yapılan az--no sonrası işleme ile ham. Konuşma hizmetini ve Birleşik Konuşma modelini kullanarak, bir işletme sesi yakalamak için hangi sistem kullanılırsa kullanılsın, yüksek kaliteli transkripsiyonlar alabilir.

Telefon verileri, müşterilerinizin ihtiyaçlarını daha iyi anlamak, yeni pazarlama fırsatlarını belirlemek veya çağrı merkezi aracılarının performansını değerlendirmek için kullanılabilir. Veriler transkripsiyonu alındıktan sonra, bir işletme çıktıyı geliştirilmiş telemetri, anahtar ifadeleri tanımlama veya müşteri duyarlılığını çözümleme gibi amaçlar için kullanabilir.

Bu sayfada özetlenen teknolojiler, hem gerçek zamanlı hem de toplu iş modunda çeşitli destek arama işleme hizmetleri için Microsoft tarafından dahili olarak hazırlanmaktadır.

Konuşma hizmetinin sunduğu bazı teknoloji ve ilgili özellikleri gözden geçirelim.

> [!IMPORTANT]
> Konuşma hizmeti Unified modeli çeşitli verilerle eğitilir ve Dikte'den Telefon analizine kadar bir dizi senaryoya tek modellik bir çözüm sunar.

## <a name="azure-technology-for-call-centers"></a>Çağrı Merkezleri için Azure Teknolojisi

Konuşma hizmeti özelliklerinin işlevsel yönünün ötesinde, çağrı merkezine uygulandığında birincil amaçları müşteri deneyimini geliştirmektir. Bu konuda üç açık etki alanı vardır:

- Arama sonrası analiz, aslında çağrı dan sonra çağrı kayıtlarıtoplu işleme.
- Çağrı gerçekleşirken çeşitli içgörüleri elde etmek için ses sinyalini işleyen gerçek zamanlı analiz (duyarlılık önemli bir kullanım örneğidir).
- Ses asistanları (botlar), ya müşteri ve bot arasındaki diyaloğu itici aracı katılımı olmadan müşterinin sorununu çözmek için, ya da aracı yardımcı olmak için yapay zeka (AI) protokolleri uygulaması olmak.

Bir toplu iş senaryosunun uygulanmasının tipik bir mimari ![diyagramı, Çağrı merkezi transkripsiyon mimarisinin altındaki resimde gösterilmiştir](media/scenarios/call-center-transcription-architecture.png)

## <a name="speech-analytics-technology-components"></a>Konuşma Analizi Teknolojisi Bileşenleri

Azure, etki alanının arama sonrası veya gerçek zamanlı olması olsun, müşteri deneyimini geliştirmek için bir dizi olgun ve gelişmekte olan teknolojiler sunar.

### <a name="speech-to-text-stt"></a>Metne konuşma (STT)

[Konuşma-to-metin](speech-to-text.md) herhangi bir çağrı merkezi çözümünde en çok aranan özelliktir. Akış aşağı analiz süreçlerinin çoğu transkripsiyonu metne dayandığıiçin, sözcük hata oranı _(WER)_ son derece önemlidir. Çağrı merkezi transkripsiyonunun en önemli zorluklarından biri, çağrı merkezinde yaygın olan gürültü (örneğin arka planda konuşan diğer ajanlar), zengin dil yerel likleri ve lehçelerinin yanı sıra gerçek telefon sinyalinin düşük kalitesidir. WER, akustik ve dil modellerinin belirli bir yerel bölge için ne kadar iyi eğitildiğiyle son derece ilişkilidir, bu nedenle modeli bulunduğunuz yeriçin özelleştirme yeteneği önemlidir. En son Unified sürüm 4.x modellerimiz hem transkripsiyon doğruluğuna hem de gecikmeye çözümdür. On binlerce saatlik akustik veri ve milyarlarca sözlü bilgi yle eğitilen Birleşik modeller, çağrı merkezi verilerini aktaracak piyasadaki en doğru modellerdir.

### <a name="sentiment"></a>Yaklaşım

Müşterinin iyi bir deneyim yaşayıp yaşamadığını ölçmek, çağrı merkezi alanına uygulandığında Konuşma analizinin en önemli alanlarından biridir. Bizim [Toplu Transkripsiyon API](batch-transcription.md) söyleyiş başına duyarlılık analizi sunuyor. Hem aracılarınız hem de müşteri için aramanın duyarlılığını belirlemek için arama transkriptinin bir parçası olarak elde edilen değerler kümesini toklayabilirsiniz.

### <a name="silence-non-talk"></a>Sessizlik (konuşulmayan)

Bir destek çağrısının yüzde 35'inin konuşmama zamanı dediğimiz şey olması nadir görülen bir durum değildir. Konuşmadığı bazı senaryolar şunlardır: bir müşteriyle önceki servis talebi geçmişini araştıran aracılar, müşterinin masaüstüne erişmelerine ve işlevlerini gerçekleştirmelerine olanak tanıyan araçları kullanan aracılar, aktarım için bekleyen müşteriler, vesaire. Bu tür senaryolar etrafında ve çağrıda nerede meydana gelen önemli müşteri hassasiyetleri sayısı olduğundan, bir çağrıda sessizliğin ne zaman oluştuğunu ölçmek son derece önemlidir.

### <a name="translation"></a>Çeviri

Bazı şirketler, teslimat yöneticilerinin müşterilerinin dünya çapındaki deneyimlerini anlayabilmeleri için yabancı dil destek aramalarından çevrilmiş transkriptler sağlamayı denemaktadır. [Çeviri](translation.md) yeteneklerimiz emsalsiz. Çok sayıda yerel bölge için ses-ses veya ses-metin çevirebiliriz.

### <a name="text-to-speech"></a>Metin Okuma

[Metinden konuşmaya,](text-to-speech.md) müşterilerle etkileşimde bulunan botların uygulanmasında önemli bir alandır. Tipik yol, müşterinin konuşması, sesinin metne aktarılması, metin inanılarak analiz edilmesi, tanınan amacın temel alınabilen bir yanıtın sentezlenmesi ve sonra bir varlığın müşteriye ya da sentezlenmiş sesli yanıtın ortaya çıkmasıdır. Oluşturulan. Tabii ki tüm bu hızlı bir şekilde meydana gelmek zorundadır - böylece düşük gecikme bu sistemlerin başarısında önemli bir bileşenidir.

Uçuça gecikmesüremiz, [Konuşma-metin](speech-to-text.md), [LUIS,](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/) [Bot Framework,](https://dev.botframework.com/)Metinden konuşmaya gibi çeşitli teknolojiler için oldukça [düşüktür.](text-to-speech.md)

Yeni seslerimiz de insan seslerinden ayırt edilemez. Robotunuzun eşsiz kişiliğini vermek için seslerimizi kullanabilirsiniz.

### <a name="search"></a>Search

Analitiğin bir diğer temel konusu da, belirli bir olayın veya deneyimin meydana geldiği etkileşimleri belirlemektir. Bu genellikle iki yaklaşımdan biri ile yapılır; kullanıcının yalnızca bir tümcecik yazdığı ve sistemin yanıt verdiği geçici bir arama veya bir analistin bir çağrıda bir senaryoyu tanımlayan bir dizi mantıksal deyim oluşturabileceği ve ardından her çağrının bu sorgu kümesine göre dizine dizileştirilebildiği daha yapılandırılmış bir sorgu. İyi bir arama örneği her yerde uyumluluk deyimi "bu çağrı kalite amaçlı kaydedilecektir ... ". Birçok şirket, aracılarının arama gerçekten kaydedilmeden önce müşterilere bu feragatnameyi sağladığından emin olmak ister. Çoğu analiz sistemi sorgu/arama algoritmaları tarafından bulunan davranışlara eğilim yeteneğine sahiptir ve bu eğilim raporlaması sonuçta bir analiz sisteminin en önemli işlevlerinden biridir. [Bilişsel hizmetler dizini](https://azure.microsoft.com/services/cognitive-services/directory/search/) sayesinde uçtan uca çözüm önemli ölçüde dizin oluşturma ve arama yetenekleri ile geliştirilebilir.

### <a name="key-phrase-extraction"></a>Anahtar İfade Ayıklama

Bu alan en zorlu analitik uygulamalarından biridir ve Yapay Eğitim ve Makine öğreniminin uygulanmasından yararlanan uygulamalardan biridir. Bu durumda birincil senaryo müşteri niyetini çıkarmaktır. Müşteri neden arıyor? Müşteri sorunu nedir? Müşteri neden olumsuz bir deneyim yaşadı? [Metin analizi hizmetimiz,](https://azure.microsoft.com/services/cognitive-services/text-analytics/) bu önemli anahtar kelimeleri veya ifadeleri ayıklamak için uçtan uca çözümünüzü hızla yükseltmek için kutunun dışında bir dizi analiz sağlar.

Şimdi biraz daha ayrıntılı olarak konuşma tanıma için toplu işleme ve gerçek zamanlı ardışık bir göz atalım.

## <a name="batch-transcription-of-call-center-data"></a>Çağrı merkezi verilerinin toplu transkripsiyonu

Toplu ses transkripsiyonu için [Toplu Transkripsiyon API](batch-transcription.md)geliştirdi. Toplu Transkripsiyon API, büyük miktarda ses verisini eşzamanlı olarak transkripsiyonu yapmak için geliştirilmiştir. Çağrı merkezi verilerinin aktarılması ile ilgili olarak, çözümümüz şu sütunlara dayanmaktadır:

- **Doğruluk** - Dördüncü nesil Birleşik modeller ile, benzersiz transkripsiyon kalitesi sunuyoruz.
- **Gecikme** - Toplu transkripsiyonlar yaparken transkripsiyonların hızlı bir şekilde gerekli olduğunu anlıyoruz. [Toplu İşlem API'si](batch-transcription.md) aracılığıyla başlatılan transkripsiyon işleri hemen sıraya alınır ve iş çalışmaya başladığında gerçek zamanlı transkripsiyondan daha hızlı gerçekleştirilir.
- **Güvenlik** - Aramaların hassas veriler içerebileceğini anlıyoruz. Güvenliğin en önemli önceliklerimizden biri olduğundan emin olabilirsiniz. Hizmetimiz ISO, SOC, HIPAA, PCI sertifikaları almıştır.

Çağrı merkezleri günlük olarak büyük hacimlerde ses verisi üretir. İşletmeniz telefon verilerini Azure Depolama gibi merkezi bir konumda depolarsa, [toplu işlem API'sini](batch-transcription.md) kullanarak transkripsiyon ları eşit bir şekilde talep edebilir ve alabilirsiniz.

Tipik bir çözüm şu hizmetleri kullanır:

- Konuşma hizmeti, konuşmadan metne transkripsiyonu yapmak için kullanılır. Toplu İşlem API'sini kullanmak için Konuşma hizmeti için standart bir abonelik (S0) gereklidir. Ücretsiz abonelikler (F0) çalışmaz.
- [Azure Depolama,](https://azure.microsoft.com/services/storage/) telefon verilerini depolamak için kullanılır ve transkriptler Toplu Transkripsiyon API'sı tarafından döndürülür. Bu depolama hesabı, özellikle yeni dosyalar eklendiğinde bildirimler kullanmalıdır. Bu bildirimler transkripsiyon işlemini tetiklemek için kullanılır.
- [Azure İşlevleri,](https://docs.microsoft.com/azure/azure-functions/) her kayıt için paylaşılan erişim imzaları (SAS) URI'yi oluşturmak ve transkripsiyon başlatmak için HTTP POST isteğini tetiklemek için kullanılır. Ayrıca, Toplu İşlem API'sini kullanarak transkripsiyonları almak ve silmek için istekler oluşturmak için Azure İşlevleri kullanılır.

Dahili olarak, Toplu İşlem modunda Microsoft müşteri çağrılarını desteklemek için yukarıdaki teknolojileri kullanıyoruz.
![Toplu Mimari](media/scenarios/call-center-batch-pipeline.png)

## <a name="real-time-transcription-for-call-center-data"></a>Çağrı merkezi verileri için gerçek zamanlı transkripsiyon

Bazı işletmelerin konuşmaları gerçek zamanlı olarak aktarmaları gerekir. Gerçek zamanlı transkripsiyon, anahtar kelimeleri tanımlamak ve konuşmayla ilgili içerik ve kaynakları bulmak, duyguları izlemek, erişilebilirliği artırmak veya yerel olmayan müşteriler ve aracılar için çevirisağlamak için kullanılabilir Hoparlör.

Gerçek zamanlı transkripsiyon gerektiren senaryolar için [Konuşma SDK'sını](speech-sdk.md)kullanmanızı öneririz. Şu anda, konuşma-metin [20'den fazla dilde](language-support.md)kullanılabilir ve SDK C++, C#, Java, Python, Node.js, Objective-C ve JavaScript mevcuttur. Örnekler [GitHub'daki](https://github.com/Azure-Samples/cognitive-services-speech-sdk)her dilde mevcuttur. En son haberler ve güncellemeler [için, Yayın notları'na](releasenotes.md)bakın.

Aşağıdaki diyagramda gösterildiği gibi, yukarıdaki teknolojileri gerçek zamanlı Microsoft müşteri çağrılarını gerçek zamanlı olarak analiz etmek için kullanıyoruz.

![Toplu Mimari](media/scenarios/call-center-reatime-pipeline.png)

## <a name="a-word-on-ivrs"></a>IVRs hakkında bir kelime

Konuşma hizmeti, [Konuşma SDK'sı](speech-sdk.md) veya [REST API'si](rest-apis.md)kullanılarak herhangi bir çözüme kolayca entegre edilebilir. Ancak, çağrı merkezi transkripsiyonu ek teknolojiler gerektirebilir. Genellikle, bir IVR sistemi ile Azure arasında bir bağlantı gereklidir. Biz bu tür bileşenleri sunmuyoruz rağmen, burada bir IVR için bir bağlantı gerektirir bir açıklamadır.

Genesys veya AudioCodes gibi çeşitli IVR veya telefon hizmeti ürünleri , bir Azure hizmetine gelen ve giden ses geçişini etkinleştirmek için yararlanılabilen tümleştirme özellikleri sunar. Temel olarak, özel bir Azure hizmeti, telefon görüşmesi oturumlarını tanımlamak (Başlat veya Arama Sonu gibi) tanımlamak ve Konuşma hizmetiyle birlikte kullanılan gelen akış sesini almak için Bir WebSocket API'sini ortaya çıkarmak için belirli bir arabirim sağlayabilir. Konuşma transkripsiyonu veya Bot Framework ile bağlantılar gibi giden yanıtlar, Microsoft'un metinden konuşmaya hizmetiyle sentezlenebilir ve oynatma için IVR'a döndürülebilir.

Başka bir senaryo oturum başlatma protokolü (SIP) ile doğrudan entegrasyondur. Azure hizmeti bir SIP Server'a bağlanır ve böylece konuşma-metin ve metinden konuşmaya aşamaları için kullanılan gelen akışı ve giden akışı elde eder. BIR SIP Server'a bağlanmak için, Ozeki SDK veya Ses aramaları için bu tür senaryoları desteklemek üzere tasarlanmış [Takımlar arama ve toplantı API](/graph/api/resources/communications-api-overview) 'si (şu anda beta) gibi ticari yazılım teklifleri vardır.

## <a name="customize-existing-experiences"></a>Varolan deneyimleri özelleştirme

 Konuşma hizmeti yerleşik modellerile iyi çalışır. Ancak, ürün ünüzün veya ortamınızın deneyimini daha da özelleştirmek ve ayarlamak isteyebilirsiniz. Özelleştirme seçenekleri, akustik model atondan markanız için benzersiz ses yazı tiplerine kadar çeşitlilik sağlar. Özel bir model yaptıktan sonra, konuşma hizmeti özelliklerinden herhangi biriyle gerçek zamanlı veya toplu iş modunda kullanabilirsiniz.

| Konuşma hizmeti | Model | Açıklama |
| -------------- | ----- | ----------- |
| Konuşmayı metne dönüştürme | [Akustik model](how-to-customize-acoustic-models.md) | Her biri belirli kayıt koşullarına sahip, arabada veya fabrika zemininde olduğu gibi belirli ortamlarda kullanılan uygulamalar, araçlar veya aygıtlar için özel bir akustik model oluşturun. Örnekler arasında aksanlı konuşma, belirli arka plan sesleri veya kayıt için belirli bir mikrofon kullanmak verilebilir. |
|                | [Dil modeli](how-to-customize-language-model.md) | Tıbbi terminoloji veya BT jargonu gibi sektöre özgü kelime ve dilbilgisinin transkripsiyonünü geliştirmek için özel bir dil modeli oluşturun. |
|                | [Söyleniş modeli](how-to-customize-pronunciation.md) | Özel bir telaffuz modeliyle, bir sözcük veya terim için fonetik formu ve ekranı tanımlayabilirsiniz. Ürün adları veya kısaltmalar gibi özelleştirilmiş terimleri işlemek için kullanışlıdır. Başlamak için ihtiyacınız olan tek şey basit `.txt` bir dosya olan bir telaffuz dosyasıdır. |
| Metin okuma | [Ses tipi](how-to-customize-voice-font.md) | Özel sesli yazı tipleri, markanız için tanınabilir, türünün tek bir sesini oluşturmanıza olanak sağlar. Başlamak için sadece küçük bir miktar veri gerekiyor. Ne kadar çok veri sağlarsanız, ses yazı tipiniz o kadar doğal ve insana benzer bir şekilde ses çıkarır. |

## <a name="sample-code"></a>Örnek kod

Konuşma hizmeti özelliklerinin her biri için GitHub'da örnek kod kullanılabilir. Bu örnekler, bir dosya veya akıştan ses okuma, sürekli ve tek çekim tanıma ve özel modeller ile çalışma gibi yaygın senaryoları kapsar. SDK ve REST örneklerini görüntülemek için bu bağlantıları kullanın:

- [Konuşma-metin ve konuşma çeviri örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
- [Toplu transkripsiyon örnekleri (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
- [Metinden konuşmaya örnekleri (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="reference-docs"></a>Referans dokümanları

- [Konuşma SDK'sı](speech-sdk-reference.md)
- [Konuşma Cihazları SDK’sı](speech-devices-sdk.md)
- [REST API: Konuşma-metin](rest-speech-to-text.md)
- [REST API: Metinden konuşmaya](rest-text-to-speech.md)
- [REST API: Toplu transkripsiyon ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşma hizmeti abonelik anahtarını ücretsiz alın](get-started.md)

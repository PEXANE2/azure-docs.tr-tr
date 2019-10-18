---
title: Çağrı merkezi dökümü-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşmayı metne dönüştürme için yaygın bir senaryo, etkileşimli sesli yanıt (ıVR) gibi çeşitli sistemlerden gelmiş olabilecek büyük miktarda telefon verilerinin dökümünü yaptırıyorsanız. Ses, bir stereo veya mono olabilir ve sinyal üzerinde gerçekleştirilen bir POST işlemi olmayan ham ile RAW. Konuşma Hizmetleri ve birleştirilmiş konuşma modelini kullanarak bir işletme, birçok ses yakalama sistemiyle yüksek kaliteli bir döküm alabilir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 4f5163ba448e4cc7e18b0e794a44003ce5ab1dce
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72516770"
---
# <a name="speech-services-for-telephony-data"></a>Telefon verileri için konuşma Hizmetleri

Landlines, cep telefonları ve radyolar aracılığıyla oluşturulan telefon verileri genellikle düşük kalitede ve okuma-metin dönüştürmesiyle ilgili zorluk yaratan 8 KHz aralığında daha dar bant oluşturur. Azure konuşma Hizmetleri Excel 'den en son konuşma tanıma modelleri, verilerin insanlar tarafından anlaşılması zor olduğu durumlarda bile bu telefon verilerini derleyen. Bu modeller büyük hacimlerdeki telefon verileriyle eğitilir ve gürültülü ortamlarda bile Pazar tanıma doğruluğunu en iyi şekilde sağlar.

Konuşmayı metne dönüştürme için yaygın bir senaryo, etkileşimli sesli yanıt (ıVR) gibi çeşitli sistemlerden gelmiş olabilecek büyük miktarda telefon verilerinin dökümünü yaptırıyorsanız. Bu sistemlerin sağladığı ses, bir stereo veya mono olabilir ve sinyal üzerinde çok az bir POST işlemi yapılmayan ham olabilir. Konuşma Hizmetleri ve birleştirilmiş konuşma modelini kullanarak bir işletme, ses yakalamak için kullanılan sistemlerin her ne kadar yüksek kaliteli bir döküm elde edebilir.

Telefon verileri, müşterilerinizin ihtiyaçlarını daha iyi anlamak, yeni pazarlama fırsatlarını belirlemek veya çağrı merkezi aracılarının performansını değerlendirmek için kullanılabilir. Veriler kaydedildikten sonra, bir işletme geliştirilmiş telemetri için çıktıyı kullanabilir, anahtar tümceleri belirleyebilir veya müşteri yaklaşımını analiz edebilir.

Bu sayfada özetlenen teknolojiler, hem gerçek zamanlı hem de toplu işlem modunda çeşitli destek çağrı işleme hizmetleri için dahili olarak Microsoft tarafından kullanılır.

Teknolojik ve ilgili özelliklerden bazılarını gözden geçirelim Azure konuşma Hizmetleri teklifi.

> [!IMPORTANT]
> Konuşma Hizmetleri Birleşik modeli, farklı verilerle eğitilmiş ve dikte etme ile telefon analizine kadar birçok senaryoya tek bir model çözümü sunuyor.

## <a name="azure-technology-for-call-centers"></a>Çağrı merkezleri için Azure teknolojisi

Konuşma hizmetlerinin birincil amacına göre işlevsel en iyi yönlerinin ötesinde – çağrı merkezine uygulandığında müşteri deneyimini geliştirmaktır. Bu şekilde üç şifresiz etki alanı mevcuttur:

* Çağrı sonrası analizi, çağrı kayıtlarının toplu işlemesi
* Arama gerçekleştiği sırada çeşitli içgörüler ayıklamak için ses sinyalinin gerçek zamanlı analiz işlemesi (önemli bir kullanım durumu olması durumunda) ve
* Sanal yardımcılar (robotlar), müşterinin aracı katılımı olmadan müşterinin sorununu çözme veya aracıya yardımcı olacak AI protokollerinin uygulaması gibi iletişim kutusu

Bir Batch senaryosu uygulamasının tipik bir mimari diyagramı, aşağıdaki resimde ![Call Center döküm mimarisi ](media/scenarios/call-center-transcription-architecture.png) gösterilmiştir.

## <a name="speech-analytics-technology-components"></a>Konuşma analizi teknoloji bileşenleri

Etki alanı, çağrı sonrası veya gerçek zamanlı olup olmadığı için Azure, müşteri deneyimini geliştirmek için çok sayıda ve yeni bir teknoloji kümesi sunar.

### <a name="speech-to-text-stt"></a>Konuşmayı metne dönüştürme (STT)

[Konuşmayı metne](speech-to-text.md) dönüştürme işlemi, herhangi bir çağrı merkezi çözümünde özelliğinden sonra en çok aranan özelliktir. Aşağı akış analizlerinin birçoğu, metnin sonuna kadar bir metin kullandığından, hata oranı (WER) en önemli öneme sahiptir. Çağrı merkezi 'ndeki önemli güçlüklerden biri, çağrı merkezi 'nde (örneğin, arka planda konuşuyor diğer aracılar) yaygın olarak kullanılan ve gerçek telefon sinyalinin en düşük kalitesindeki, çok sayıda dil yerel ayarı ve diapahı 'nin yaygın olduğu gürültü. WER, belirli bir yerel ayar için akustik ve dil modellerinin eğitilme ve bu sayede modeli yerel ayarınızdaki bir şekilde özelleştirebilme açısından önemli ölçüde bağıntılı bir seçenektir. En son birleştirilmiş sürüm 4. x modellerimiz, hem döküm doğruluğu hem de gecikme süresi çözümüdür. On binlerce saatlik akustik veri ve milyarlarca sözlü bilgi birleştirilmiş modellerle eğitilen, pazara giren en doğru modellerdir.

### <a name="sentiment"></a>Duygu
Ölçmek, arama merkezi alanına uygulandığında konuşma analizinin en önemli alanlarından biridir. [Toplu Iş dökümü API](batch-transcription.md) 'imiz, utterance başına yaklaşım analizi sunar. Hem aracılarınız hem de müşteri için çağrının yaklaşımını tespit etmek üzere çağrı dökümünü bir parçası olarak elde edilen değerler kümesini toplayabilirsiniz.

### <a name="silence-non-talk"></a>Sessizlik (konuş olmayan)
Bir destek çağrısının yüzde 35 ' unun, konuşmasız bir süre arayabiliriz. Konuşmadan oluşan bazı senaryolar şunlardır: bir müşteriyle önceki servis talebi geçmişini arayan aracılar, araçların müşterinin masaüstüne erişmelerine ve işlevleri gerçekleştirmesine izin veren araçlar, müşteriler bir aktarım beklerken beklemeye bakıyor ve bu şekilde devam eder. Bu tür senaryolar etrafında ve çağrıda nerede gerçekleştikleri önemli müşteri sensitivities sayısı olduğu için sessizlik bir çağrıda meydana geldiğinde, bu durum için çok önemlidir.

### <a name="translation"></a>Çeviri
Bazı şirketler, dağıtım yöneticilerinin müşterilerinin dünya çapında deneyimlerini anlayabilmesi için yabancı dillerdeki destek çağrılarından çevrilmiş yazılı betikler sağlamaya devam eder. [Çeviri](translation.md) olanaklarımız unsurpassed. Ses veya ses sesinize çok sayıda yerel gruptan metin çevirebiliriz.

### <a name="text-to-speech"></a>Metin Okuma
[Metinden konuşmaya](text-to-speech.md) , müşterilerle etkileşime geçen botları uygulamakta önemli bir alandır. Tipik bir yol, müşterinin konuştuğu, sesin metne göre çözümlenme, amaç için analiz olduğu, tanınan amaca göre bir yanıt elde edilmesi ve bir varlığın müşteriye veya bir sensörle bir ses yanıtı olduğu üret. Kuşkusuz bunun her şey hızlı bir şekilde gerçekleşmelidir; bu nedenle gecikme süresi, bu sistemlerin başarısı için önemli bir bileşendir.

Uçtan uca Geciktiğimiz, [konuşmadan metne](speech-to-text.md), [LUA](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/), [bot Framework](https://dev.botframework.com/), [metinden konuşmaya](text-to-speech.md)gibi çeşitli teknolojiler göz önünde bulundurularak oldukça düşüktür.

Yeni seslerimiz de insan seslerinden ayırt edilemez. Botunuza benzersiz kişiliğinizi sağlamak için sesler kullanabilirsiniz.

### <a name="search"></a>Arama
Analize ait başka bir zımba, belirli bir olay veya deneyimin gerçekleştiği etkileşimleri belirlemektir. Bu işlem genellikle, kullanıcının bir tümceciği ve sistem yanıt verdiğini veya daha yapılandırılmış bir sorguyu, yani bir analistin çağrı içindeki bir senaryoyu tanımlayan bir mantıksal deyimler kümesi oluşturmasının gerektiği daha yapılandırılmış bir sorgu olan iki yaklaşımdan biriyle yapılır ve sonra her çağrının bu sorgu kümesiyle dizini oluşturulabilir. İyi bir arama örneği, ubititous uyumluluk deyimidir "Bu çağrı kalite amacıyla kaydedilecek... "– birçok şirket, aracıların gerçekten kaydedilmeden önce bu vazgeçme belgesi müşterilere bu bildirimi sağladığından emin olmak istiyor. Çoğu analiz sistemi, bu eğilimleri raporlamak son olarak bir analiz sisteminin en önemli işlevlerinden biri olduğundan, sorgu/arama algoritmaları tarafından bulunan davranışları eğilim özelliğine sahiptir. Bilişsel [Hizmetler dizini](https://azure.microsoft.com/services/cognitive-services/directory/search/) sayesinde uçtan uca çözümünüz, dizin oluşturma ve arama özellikleri ile önemli ölçüde iyileştirilen olabilir.

### <a name="key-phrase-extraction"></a>Anahtar Tümcecik Ayıklama
Bu alan, daha zorlu analitik uygulamalardan biridir ve bunlardan biri AI ve ML uygulamasından yararlanırken. Burada birincil senaryo, müşteri amacını çıkarmaktır. Müşteri neden çağrılıyor? Müşteri sorunu nedir? Müşterinin neden negatif bir deneyimi var? [Metin analizi](https://azure.microsoft.com/services/cognitive-services/text-analytics/) hizmetimiz, bu önemli anahtar sözcükleri veya tümceleri ayıklamak üzere uçtan uca çözümünüzü hızlı bir şekilde yükseltmek için kutudan çıkan bir analiz kümesi sağlar.

Artık, toplu işleme ve konuşma tanıma için gerçek zamanlı işlem hatlarına biraz daha ayrıntılı bir göz atalım.

## <a name="batch-transcription-of-call-center-data"></a>Çağrı merkezi verilerinin toplu olarak dökümünü alma

Ses dökümü için [toplu Iş dökümü API](batch-transcription.md)'sini geliştirdik. Toplu Iş dökümü API 'SI, büyük miktarlarda ses verisi zaman uyumsuz olacak şekilde geliştirilmiştir. Çözüm çağrı merkezi verilerinin dökümünü yaparken, çözümümüzü bu sayfalarımdayına dayalıdır:

* **Doğruluk**: dördüncü nesil Birleşik modellerle unsurpassed döküm kalitesi sunuyoruz.
* **Gecikme süresi**: toplu döküm yaparken, bu işlemleri hızlı bir şekilde yapmanız gerektiğini anladık. [Toplu Iş dökümü API 'si](batch-transcription.md) aracılığıyla başlatılan döküm işleri hemen sıraya alınır ve iş çalışmaya başladıktan sonra gerçek zamanlı dökümden daha hızlı gerçekleştirilir.
* **Güvenlik**: çağrıların gizli veriler içerebileceğini anladık. Güvenlik, en yüksek önceliklerimizden biri olduğundan emin olur. Hizmetimiz ISO, SOC, HIPAA, PCI sertifikaları aldı.

Çağrı merkezleri günlük olarak büyük hacimli ses verileri oluşturur. İşiniz telefon verilerini Azure depolama gibi merkezi bir konumda depoluyorsa, [toplu Iş dökümü API](batch-transcription.md) 'sini kullanarak zaman uyumsuz istek yapabilir ve bu istekleri alabilirsiniz.

Tipik bir çözüm bu hizmetleri kullanır:

* Azure konuşma Hizmetleri, konuşmayı metne dönüştürme için kullanılır. Toplu Iş dökümü API 'sini kullanmak için konuşma Hizmetleri için standart bir abonelik (SO) gereklidir. Ücretsiz abonelikler (F0) çalışmayacak.
* [Azure depolama](https://azure.microsoft.com/services/storage/) , telefon verilerini depolamak Için ve toplu iş dökümü API 'si tarafından döndürülen döküm dosyaları için kullanılır. Bu depolama hesabı, özellikle yeni dosyaların eklendiği zaman için bildirimleri kullanmalıdır. Bu bildirimler, döküm işleminin tetiklenmesi için kullanılır.
* [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/) , her kayıt için paylaşılan erişim IMZALARı (SAS) URI 'si oluşturmak için KULLANıLıR ve http post isteğini bir döküm başlatacak şekilde tetikler. Ayrıca, Azure Işlevleri, toplu Iş dökümü API 'sini kullanarak döküm alma ve silme istekleri oluşturmak için kullanılır.
* [Web kancaları](webhooks.md) , döküm tamamlandığında bildirim almak için kullanılır.

Dahili olarak, Microsoft Müşteri çağrılarını toplu Iş modunda desteklemek için yukarıdaki teknolojileri kullanıyoruz.
![Batch mimari ](media/scenarios/call-center-batch-pipeline.png)

## <a name="real-time-transcription-for-call-center-data"></a>Çağrı merkezi verileri için gerçek zamanlı döküm

Bazı işletmeler, konuşmaları gerçek zamanlı olarak almak için gereklidir. Gerçek zamanlı döküm, önemli kelimeleri belirlemek, görüşmeyi izlemek, erişilebilirliği geliştirmek veya yerel olmayan müşteriler ve aracılar için çeviriler sağlamak üzere, konuşma açısından ilgili içerik ve kaynakları aramak için kullanılabilir lerinin.

Gerçek zamanlı döküm gerektiren senaryolar için [konuşma SDK 'sını](speech-sdk.md)kullanmanızı öneririz. Şu anda, konuşmadan metne [20 ' den fazla dilde](language-support.md)erişilebilir ve SDK, C++ C#, Java, Python, Node. js, amaç-C ve JavaScript ' te kullanılabilir. Örnek, [GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk)'daki her dilde kullanılabilir. En son haberler ve güncelleştirmeler için bkz. [sürüm notları](releasenotes.md).

Dahili olarak, gerçek zamanlı Microsoft Müşteri çağrılarında olduğu gibi çözümlemek için yukarıdaki teknolojileri kullanıyoruz.

![Batch mimarisi](media/scenarios/call-center-reatime-pipeline.png)

## <a name="a-word-on-ivrs"></a>IRS üzerinde bir kelime

Konuşma Hizmetleri, [konuşma SDK 'sı](speech-sdk.md) veya [REST API](rest-apis.md)kullanılarak herhangi bir çözüme kolayca tümleştirilebilir. Ancak, çağrı merkezi dökümü ek teknolojiler gerektirebilir. Genellikle, bir ıVR sistemi ve Azure arasında bir bağlantı gereklidir. Bu tür bileşenleri sunmuyoruz, ancak bir ıVR bağlantısının ne olduğunu anlatmak istiyoruz.

Birçok IR veya telefon hizmet ürünü (Genesys veya AudioCodes gibi), bir Azure hizmetine gelen ve giden ses geçişini etkinleştirmek için yararlanılabilir olabilecek tümleştirme özellikleri sunar. Temel olarak, özel bir Azure hizmeti telefon araması oturumlarını (çağrı başlatma veya çağrı bitişi gibi) tanımlamak için belirli bir arabirim sağlayabilir ve konuşma hizmetleriyle birlikte kullanılan gelen akış sesini almak için bir WebSocket API 'SI sunar. Konuşma ve bot Framework ile bağlantılar gibi giden yanıtlar, Microsoft 'un metin okuma hizmeti ile birlikte bulunabilir ve kayıttan yürütme için ıVR 'ye döndürülür.

Başka bir senaryo doğrudan SIP Tümleştirmesidir. Azure hizmeti bir SIP sunucusuna bağlanır, bu nedenle, konuşma ve metinden konuşmaya aşamalar için kullanılan bir gelen akış ve giden akış elde edilir. Bir SIP sunucusuna bağlanmak için, ozeki SDK gibi ticari yazılım teklifleri veya ses çağrıları için bu tür senaryoyu destekleyecek şekilde tasarlanan [takımlar çağıran ve toplantılar API 'si](/graph/api/resources/communications-api-overview) (Şu anda beta sürümünde) bulunur.

## <a name="customize-existing-experiences"></a>Mevcut deneyimleri özelleştirin

Azure konuşma Hizmetleri yerleşik modellerle iyi çalışmaktadır, ancak ürün veya ortamınızın deneyimini daha fazla özelleştirmek ve ayarlamak isteyebilirsiniz. Özelleştirme seçenekleri, akustik model ayarlamalarından markanızla ilgili benzersiz ses yazı tiplerine kadar değişir. Özel bir model oluşturduktan sonra, bunu hem gerçek zamanlı hem de toplu işlem modunda Azure konuşma hizmetlerinden biriyle kullanabilirsiniz.

| Konuşma hizmeti | Model | Açıklama |
|----------------|-------|-------------|
| Konuşmayı metne dönüştürme | [Akustik model](how-to-customize-acoustic-models.md) | Her biri belirli kayıt koşullarına sahip bir otomobil veya bir fabrika katında gibi belirli ortamlarda kullanılan uygulamalar, araçlar veya cihazlar için özel bir akustik model oluşturun. Bu örneklere, vurgulu konuşma, belirli arka plan gürültüler veya kayıt için belirli bir mikrofon kullanımı dahildir. |
| | [Dil modeli](how-to-customize-language-model.md) | Sektöre özgü sözlük ve dilbilgisinde tıp terminolojisi ya da It jarggibi bir tür dökümü geliştirmek için özel bir dil modeli oluşturun. |
| | [Söyleniş modeli](how-to-customize-pronunciation.md) | Özel bir telaffuz modeliyle, bir sözcük veya terimin fonetik biçimini tanımlayabilir ve görüntüleyebilirsiniz. Ürün adları veya kısaltmalar gibi özelleştirilmiş terimleri işlemek için faydalıdır. Kullanmaya başlamak için tek yapmanız gereken, basit bir. txt dosyası olan bir telaffuz dosyasıdır. |
| Metin okuma | [Ses tipi](how-to-customize-voice-font.md) | Özel ses yazı tipleri, markanız için tanınabilir, tek bir tür sesi oluşturmanızı sağlar. Başlamak için yalnızca az miktarda veri alır. Sağladığınız daha fazla veri, ses yazı tipi daha doğal ve insan gibi bir şekilde gönderilir. |

## <a name="sample-code"></a>Örnek kod

Örnek kod, Azure konuşma hizmetlerinin her biri için GitHub 'da kullanılabilir. Bu örnekler, bir dosya veya akıştan ses okuma, sürekli ve tek kararlı bir tanıma ve özel modellerle çalışma gibi yaygın senaryoları kapsar. SDK ve REST örneklerini görüntülemek için bu bağlantıları kullanın:

* [Konuşmadan metne ve konuşma çevirisi örnekleri (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Toplu iş dökümü örnekleri (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
* [Metinden konuşmaya örnekleri (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="reference-docs"></a>Başvuru belgeleri

* [Konuşma SDK'sı](speech-sdk-reference.md)
* [Konuşma cihazları SDK 'Sı](speech-devices-sdk.md)
* [REST API: konuşmayı metne dönüştürme](rest-speech-to-text.md)
* [REST API: metinden konuşmaya](rest-text-to-speech.md)
* [REST API: toplu Iş dökümü ve özelleştirme](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ücretsiz bir konuşma Hizmetleri abonelik anahtarı alın](get-started.md)

---
title: Metin konuşma sık sorulan sorular
titleSuffix: Azure Cognitive Services
description: Metinden Metine Konuşma hizmeti hakkında sık sorulan soruların yanıtlarını alın.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/4/2019
ms.author: panosper
ms.openlocfilehash: a279aebdd19ebd3a41ddad0c1c279937e00838c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77168455"
---
# <a name="speech-to-text-frequently-asked-questions"></a>Metin konuşma sık sorulan sorular

Bu SSS'de sorularınıza yanıt bulamıyorsanız, [diğer destek seçeneklerine](support.md)göz atın.

## <a name="general"></a>Genel

**S: Temel model ile özel metin konuşması modeli arasındaki fark nedir?**

**C**: Bir temel model, Microsoft'a ait veriler kullanılarak eğitilmiştir ve bulutta zaten dağıtılmıştır. Belirli ortam gürültüsü veya dili olan belirli bir ortama daha iyi uyacak şekilde bir modeli uyarlamak için özel bir model kullanabilirsiniz. Fabrika zeminler, arabalar, ya da gürültülü sokaklar uyarlanmış bir akustik model gerektirir. Biyoloji, fizik, radyoloji, ürün adları ve özel kısaltmalar gibi konular uyarlanmış bir dil modeli gerektirir.

**S: Temel bir model kullanmak istersem nereden başlarım?**

**C**: Öncelikle [bir abonelik anahtarı](get-started.md)alın. Önceden dağıtılan temel modellere REST aramaları yapmak istiyorsanız, [REST API'lerine](rest-apis.md)bakın. WebSockets kullanmak istiyorsanız, [SDK'yı indirin.](speech-sdk.md)

**S: Her zaman özel bir konuşma modeli oluşturmam gerekir mi?**

**A**: Hayır. Uygulamanız genel, günlük dil kullanıyorsa, bir modeli özelleştirmeniz gerekmez. Uygulamanız çok az arka plan gürültüsünün olduğu veya hiç olmadığı bir ortamda kullanılıyorsa, bir modeli özelleştirmeniz gerekmez.

Portalda taban çizgisi ve özelleştirilmiş modeller dağıtabilir ve bunlara karşı doğruluk testleri çalıştırabilirsiniz. Bu özelliği, bir temel modelin doğruluğunu özel bir modele karşı ölçmek için kullanabilirsiniz.

**S: Veri kümem veya modelimin işlenmesinin ne zaman tamamolduğunu nasıl bileceğim?**

**C**: Şu anda, tablodaki modelin veya veri kümesinin durumunu bilmek için tek yoldur. İşlem tamamlandığında, durum **Başarılı**oldu.

**S: Birden fazla model oluşturabilir miyim?**

**C**: Koleksiyonunuzda sahip olabileceğiniz model sayısında bir sınır yoktur.

**S: Bir hata yaptığımı fark ettim. Devam etmekte olan veri alma veya model oluşturma işlemimi nasıl iptal edebilirim?**

**C**: Şu anda, bir akustik veya dil adaptasyon işlemini geri alamazsınız. İçe aktarılan verileri ve modelleri terminal durumundayken silebilirsiniz.

**S: Arama ve Dikte modeli ile Konuşma modeli arasındaki fark nedir?**

**C**: Konuşma hizmetinde birden fazla temel model arasından seçim yapabilirsiniz. Konuşma modeli, konuşma stilinde konuşulan konuşmayı tanımak için yararlıdır. Bu model, telefon görüşmelerini yazıya çevirmek için idealdir. Arama ve Dikte modeli, sesli olarak tetiklenen uygulamalar için idealdir. Evrensel model, her iki senaryoyu da ele almayı amaçlayan yeni bir modeldir. Evrensel model şu anda çoğu yerel deki Konuşma modelinin kalite düzeyinde veya üzerindedir.

**S: Varolan modelimi (model istifleme) güncelleyebilir miyim?**

**C**: Varolan bir modeli güncelleştiremezsiniz. Çözüm olarak, eski veri kümesini yeni veri kümesiyle birleştirin ve yeniden yeniden uyarlayın.

Eski veri kümesi ve yeni veri kümesi tek bir .zip dosyasında (akustik veriler için) veya .txt dosyasında (dil verileri için) birleştirilmelidir. Uyarlama tamamlandığında, yeni bir bitiş noktası elde etmek için yeni, güncelleştirilmiş modelin yeniden dağıtılması gerekir

**S: Taban çizgisinin yeni bir sürümü kullanılabilir olduğunda, dağıtımım otomatik olarak güncellenir mi?**

**C**: Dağıtımlar OTOMATIK olarak güncelleştirilmeyecektir.

Temel V1.0'a sahip bir modeli uyarladıysanız ve dağıttıysanız, bu dağıtım olduğu gibi kalır. Müşteriler dağıtılan modeli devre dışı bırakıp taban çizgisinin yeni sürümünü kullanarak yeniden uyarlayabilir ve yeniden dağıtabilir.

**S: Modelimi indirip yerel olarak çalıştırabilir miyim?**

**C**: Modeller yerel olarak indirilip yürütülemez.

**S: İsteklerim günlüğe kaydedildi mi?**

**C**: İzlemeyi kapatmak için bir dağıtım oluştururken seçeneğiniz vardır. Bu noktada, hiçbir ses veya transkripsiyon günlüğe kaydedilir. Aksi takdirde, istekler genellikle güvenli depolama alanında Azure'da oturum açmaktadır.

**S: İsteklerim daraltıldı mı?**

**C**: REST API istekleri 5 saniyede 25 ile sınırlar. Ayrıntılar metin [konuşma](speech-to-text.md)için sayfalarımızda bulunabilir.

**S: Çift kanallı ses için nasıl ücretlendirilirim?**

**C**: Her kanalı ayrı ayrı gönderirseniz (her kanal kendi dosyasında), her dosya boyunca ücretlendirilirsiniz. Her kanal birlikte çok katlı tek bir dosya gönderirseniz, o zaman tek bir dosya süresi için tahsil edilecektir. Fiyatlandırma yla ilgili ayrıntılar için lütfen [Azure Bilişsel Hizmetler fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/)bakın.

> [!IMPORTANT]
> Özel Konuşma hizmetini kullanmanızı yasaklayan daha fazla gizlilik endişeniz varsa, destek kanallarından birine başvurun.

## <a name="increasing-concurrency"></a>Eşzamanlılığı artırma

**S: Dağıtılan modelim için portalda sunulandan daha yüksek eşzamanlılık gerekirse ne olur?**

**C**: Modelinizi 20 eşzamanlı istek le ölçeklendirebilirsiniz.

Gerekli bilgilerle, [Azure destek portalında](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)bir destek isteği oluşturun. [Destek sayfasında](support.md)belirtilen ortak kanalların (GitHub, Stackoverflow, ...) herhangi birinde bilgi göndermeyin.

Özel bir ***model***için eşzamanlılığı artırmak için aşağıdaki bilgilere ihtiyacımız vardır:

- Modelin dağıtıldığı bölge,
- dağıtılan modelin bitiş noktası kimliği:
  - [Özel Konuşma Portalı](https://aka.ms/customspeech)var ,
  - oturum açın (gerekirse),
  - projenizi ve dağıtımınızı seçin,
  - için eşzamanlılık artışına ihtiyacınız olan bitiş noktasını seçin,
  - kopyalayın. `Endpoint ID`

***Bir temel model***için eşzamanlılığı artırmak için aşağıdaki bilgilere ihtiyacımız var:

- Hizmet bölgeniz,

ve ya

- aboneliğiniz için bir erişim belirteci [(buraya](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-speech-to-text#how-to-get-an-access-token)bakın ),

or

- aboneliğiniz için Kaynak Kimliği:
  - [Azure portalına](https://portal.azure.com)gidin ,
  - arama `Cognitive Services` kutusunda seçin,
  - görüntülenen hizmetlerden eşzamanlılık için arttırılmış olmasını istediğiniz Konuşma hizmetini seçin,
  - bu `Properties` hizmet için görüntülemek,
  - tam `Resource ID`kopyalayın.

## <a name="importing-data"></a>Verileri içeri aktarma

**S: Bir veri kümesinin boyutundaki sınır nedir ve neden bu sınırdır?**

**C**: Bir veri kümesi için geçerli sınır 2 GB'dır. Sınır, HTTP yükleme için bir dosyanın boyutuüzerindeki kısıtlama nedeniyle.

**S: Daha büyük bir metin dosyası yükleyebilmek için metin dosyalarımı kapatabilir miyim?**

**A**: Hayır. Şu anda yalnızca sıkıştırılmamış metin dosyalarına izin verilir.

**S: Veri raporu, başarısız sözlerle ilgili olduğunu söylüyor. Sorun nedir?**

**C**: Bir dosyadaki sözcüklerin yüzde 100'ünü yüklememek sorun değildir. Akustik veya dil veri kümesindeki sözcüklerin büyük çoğunluğu (örneğin, yüzde 95'ten fazlası) başarıyla içe aktarılırsa, veri kümesi kullanılabilir. Ancak, bu söyleyişlerin neden başarısız olduğunu anlamaya çalışmanızı ve sorunları çözmenizi öneririz. Hataları biçimlendirme küçülme gibi sık karşılaşılan sorunların giderilen kolay.

## <a name="creating-an-acoustic-model"></a>Akustik model oluşturma

**S: Ne kadar akustik veriye ihtiyacım var?**

**C**: 30 dakika ile bir saat arasında akustik veri ile başlamanızı öneririz.

**S: Hangi verileri toplamalıyım?**

**C**: Uygulama senaryosuna yakın verileri toplayın ve olası örneği kullanın. Veri toplama, aygıt veya aygıtlar, ortamlar ve hoparlör türleri açısından hedef uygulama ve kullanıcılarla eşleşmelidir. Genel olarak, mümkün olduğunca geniş bir hoparlör yelpazesinden veri toplamanız gerekir.

**S: Akustik verileri nasıl toplamalıyım?**

**C**: Bağımsız bir veri toplama uygulaması oluşturabilir veya kullanıma hazır ses kayıt yazılımı kullanabilirsiniz. Ayrıca, uygulamanızın ses verilerini günlüğe kaydeden ve verileri kullanan bir sürümünü de oluşturabilirsiniz.

**S: Adaptasyon verilerini kendim aktarmam gerekiyor mu?**

**A**: Evet! Bunu kendiniz transkripsiyonu yapabilir veya profesyonel bir transkripsiyon hizmeti kullanabilirsiniz. Bazı kullanıcılar profesyonel transkripsiyonları tercih ve diğerleri crowdsourcing kullanmak veya transkripsiyonları kendileri yapmak.

## <a name="accuracy-testing"></a>Doğruluk testi

**S: Özel bir dil modeli kullanarak özel akustik modelimin çevrimdışı testini gerçekleştirebilir miyim?**

**C**: Evet, çevrimdışı testi ayarlarken açılır menüdeki özel dil modelini seçmeniz yeterlidir.

**S: Özel bir akustik model kullanarak özel dil modelimin çevrimdışı testini gerçekleştirebilir miyim?**

**C**: Evet, çevrimdışı testi ayarlarken açılır menüdeki özel akustik modeli seçmeniz yeterlidir.

**S: Sözcük hata oranı (WER) nedir ve nasıl hesaplanır?**

**C**: WER konuşma tanıma için değerlendirme ölçüsüdür. WER, eklemeler, silmeler ve ikameler içeren ve başvuru transkripsiyonundaki toplam sözcük sayısına bölünen toplam hata sayısı olarak sayılır. Daha fazla bilgi için [sözcük hata oranına](https://en.wikipedia.org/wiki/Word_error_rate)bakın.

**S: Doğruluk testinin sonuçlarının iyi olup olmadığını nasıl belirleyebilirim?**

**C**: Sonuçlar, temel model ile özelleştirdiğiniz model arasında bir karşılaştırma gösterir. Özelleştirme değerli hale getirmek için temel modeli yenmek amacı gerekir.

**S: Bir temel modelin WER'ini nasıl belirleyebilirim, böylece bir gelişme olup olmadığını görebiliyorum?**

**C**: Çevrimdışı test sonuçları, özel modelin temel doğruluğunu ve taban çizgisi üzerindeki iyileştirmeyi gösterir.

## <a name="creating-a-language-model"></a>Dil modeli oluşturma

**S: Ne kadar metin verisi yüklemem gerekiyor?**

**C**: Uygulamanızda kullanılan kelime ve deyimlerin başlangıç dil modellerinden ne kadar farklı olduğuna bağlıdır. Tüm yeni sözcükler için, bu sözcüklerin kullanımı mümkün olduğunca çok sayıda örnek sağlamak yararlıdır. Uygulamanızda kullanılan yaygın ifadeler için, dil verilerindeki tümcecikler de dahil olmak üzere, sisteme bu terimleri dinlemesini de söylediği için yararlıdır. Dil veri setinde en az 100 ve genellikle birkaç yüz veya daha fazla söz olması yaygındır. Ayrıca, bazı sorgu türlerinin diğerlerinden daha yaygın olması bekleniyorsa, veri kümesine ortak sorguların birden çok kopyasını ekleyebilirsiniz.

**S: Kelimelerin bir listesini yükleyebilir miyim?**

**C**: Sözcüklerin bir listesini yüklemek sözcükleri kelime dağarcığına ekler, ancak sisteme kelimelerin genellikle nasıl kullanıldığını öğretmez. Tam veya kısmi sözler (kullanıcıların söyleme olasılığı yüksek olan şeylerin tümcecikleri veya ifadeleri) sağlayarak, dil modeli yeni sözcükleri ve bunların nasıl kullanıldığını öğrenebilir. Özel dil modeli yalnızca sisteme yeni sözcükler eklemek için değil, aynı zamanda uygulamanız için bilinen sözcüklerin olasılığını ayarlamak için de iyidir. Tam söz lemeler sağlamak, sistemin daha iyi öğrenmesine yardımcı olur.

## <a name="tenant-model-custom-speech-with-office-365-data"></a>Kiracı Modeli (Office 365 verileriyle Özel Konuşma)

**S: Kiracı Modeli'nde hangi bilgiler bulunur ve nasıl oluşturulur?**

**A:** Kiracı Modeli, kuruluşunuzdaki herkes tarafından görülebilen [ortak grup](https://support.office.com/article/learn-about-office-365-groups-b565caa1-5c40-40ef-9915-60fdb2d97fa2) e-postaları ve belgeleri kullanılarak oluşturulur.

**S: Kiracı Modeli tarafından hangi konuşma deneyimleri geliştirilir?**

**A:** Kiracı Modeli etkinleştirildiğinde, oluşturulduğunda ve yayımlandığında, Konuşma hizmeti kullanılarak oluşturulan tüm kurumsal uygulamaların tanınmasını iyileştirmek için kullanılır; ayrıca kuruluşa üyelik gösteren bir kullanıcı AAD belirteci geçmek.

Dikte ve PowerPoint Altyazı gibi Office 365'te yerleşik konuşma deneyimleri, Konuşma hizmeti uygulamalarınız için bir Kiracı Modeli oluşturduğunuzda değişmez.

## <a name="next-steps"></a>Sonraki adımlar

- [Sorun giderme](troubleshooting.md)
- [Sürüm notları](releasenotes.md)

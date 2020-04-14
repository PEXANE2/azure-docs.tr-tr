---
title: Özel ses örneklerini kaydet - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Sağlam bir komut dosyası hazırlayarak, iyi bir ses yeteneği işe alarak ve profesyonel olarak kayıt yaparak üretim kalitesinde özel bir ses yapın.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/13/2020
ms.author: erhopf
ms.openlocfilehash: 2897fe2e0cc8aeb929b0a33f5cdaba9c60f2a244
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261593"
---
# <a name="record-voice-samples-to-create-a-custom-voice"></a>Özel bir ses oluşturmak için ses örneklerini kaydetme

Sıfırdan yüksek kaliteli bir üretim özel ses oluşturma rahat bir girişim değildir. Özel bir sesin merkezi bileşeni, insan konuşmasının ses örneklerinden oluşan geniş bir koleksiyondur. Bu ses kayıtlarının yüksek kalitede olması hayati önem taşıyor. Bu tür kayıtları yapma deneyimi olan bir ses yeteneği seçin ve bunları profesyonel ekipman kullanarak yetkili bir kayıt mühendisi tarafından kaydedilmiş olması.

Ancak, bu kayıtları yapmadan önce bir komut dosyasına ihtiyacınız vardır: ses örneklerini oluşturmak için ses yeteneğiniz tarafından konuşulacak sözcükler. En iyi sonuçlar için, komut dosyanızın özel ses modelini eğitmek için iyi fonetik kapsama alanına ve yeterli çeşitlie sahip olması gerekir.

Birçok küçük ama önemli ayrıntıları profesyonel bir ses kaydı oluşturma içine gidin. Bu kılavuz, iyi ve tutarlı sonuçlar elde eve yardımcı olacak bir işlem için bir yol haritasıdır.

> [!TIP]
> En yüksek kalitede sonuçlar elde etmek için, özel sesinizi geliştirmenize yardımcı olması için Microsoft'u meşgul etmeyi düşünün. Microsoft, Cortana ve Office de dahil olmak üzere kendi ürünleri için yüksek kaliteli sesler üretme konusunda geniş bir deneyime sahiptir.

## <a name="voice-recording-roles"></a>Ses kayıt rolleri

Özel bir ses kayıt projesinde dört temel rol vardır:

Rol|Amaç
-|-
Ses yeteneği        |Bu kişinin sesi özel sesin temelini oluşturacaktır.
Kayıt mühendisi  |Kaydın teknik yönlerini denetler ve kayıt ekipmanını çalıştırın.
Yönetmen            |Senaryoyu hazırlar ve ses yeteneğinin performansını yönlendirer.
Düzenleyici              |Ses dosyalarını kesinleştirir ve Özel Ses portalına yüklenmeleri için hazırlar.

Bir birey birden fazla rolü doldurabilir. Bu kılavuz, öncelikle yönetmen rolü doldurma ve hem bir ses yetenek ve bir kayıt mühendisi işe olacağını varsayar. Kayıtları kendiniz yapmak istiyorsanız, bu makalede kayıt mühendisi rolü hakkında bazı bilgiler yer almaktadır. Editör rolü oturum dan sonrasına kadar gerekli değildir, bu nedenle yönetmen veya kayıt mühendisi tarafından gerçekleştirilebilir.

## <a name="choose-your-voice-talent"></a>Ses yeteneğinizi seçin

Seslendirme veya ses karakteri çalışmalarında deneyime sahip aktörler iyi özel ses yeteneği olun. Ayrıca sık sık spikerler ve haber okuyucuları arasında uygun yetenek bulabilirsiniz.

Doğal sesini beğendiğiniz ses yeteneğini seçin. Bu benzersiz "karakter" sesleri oluşturmak mümkündür, ama çoğu yetenek için sürekli bunları gerçekleştirmek için çok daha zordur, ve çaba ses gerginliği neden olabilir.

> [!TIP]
> Genel olarak, özel bir ses oluşturmak için tanınabilir sesler kullanmaktan kaçının— tabii ki amacınız ünlü bir ses üretmek değilse. Daha az bilinen sesler genellikle kullanıcılar için daha az dikkat dağıtıcıdır.

Ses yeteneği seçiminde en önemli faktör tutarlılıktır. Kayıtların hepsi aynı gün aynı odada yapılmış gibi olmalı. Bu ideale iyi kayıt uygulamaları ve mühendislik yoluyla yaklaşabilirsiniz.

Ses yeteneğin denklemin diğer yarısı. Onlar tutarlı oranı, hacim düzeyi, pitch ve sesi ile konuşmak gerekir. Açık diksiyon bir zorunluluktur. Yetenek aynı zamanda kesinlikle kendi adım varyasyon, duygusal etkisi ve konuşma tavırları kontrol edebilmek gerekir.

Özel ses örneklerinin kaydedilmesi diğer ses çalışma larından daha yağlı olabilir. Çoğu ses yeteneği günde iki ya da üç saat kayıt yapabilir. Oturumları haftada üç veya dört ile sınırlandırın ve mümkünse arada bir gün izin verin.

Bir ses modeli için yapılan kayıtlar duygusal olarak tarafsız olmalıdır. Yani, üzücü bir söz üzücü bir şekilde okunmamalıdır. Ruh hali daha sonra prosody kontrolleri ile sentezlenen konuşma eklenebilir. Özel sesin genel ses ve duygusal tonunu tanımlayan bir "persona" geliştirmek için ses yeteneğinizle çalışın. Bu süreçte, bu kişi için "nötr"ün nasıl bir ses olduğunu tam olarak belirleyeceksiniz.

Bir kişilik, örneğin, doğal iyimser bir kişiliğe sahip olabilir. Yani "onların" sesi, tarafsız konuşsalar bile bir iyimserlik notu taşıyabilir. Ancak, böyle bir kişilik özelliği ince ve tutarlı olmalıdır. Ne hedeflediğiniz hakkında bir fikir edinmek için mevcut seslerin okumalarını dinleyin.

> [!TIP]
> Genellikle, yaptığınız ses kayıtlarına sahip olmak istersiniz. Ses yeteneğiniz, proje için kiralık bir sözleşmeye uygun olmalıdır.

## <a name="create-a-script"></a>Komut dosyası oluşturma

Herhangi bir özel ses kayıt oturumunun başlangıç noktası, ses yeteneğiniz tarafından konuşulacak sözleri içeren komut dosyasıdır. ("Söyleyiş" terimi hem tam cümleleri hem de daha kısa tümcecikleri kapsar.)

Senaryonuzdaki söylevler her yerden gelebilir: kurgu, kurgusal olmayan, konuşmaların transkriptleri, haber raporları ve basılı olarak bulunan herhangi bir şey. Sesinbelirli kelime türlerinde (tıbbi terminoloji veya programlama jargonu gibi) iyi iş yaptığından emin olmak istiyorsanız, bilimsel belgelerden veya teknik belgelerden cümleler eklemek isteyebilirsiniz. Potansiyel yasal konuların kısa bir tartışması için ["Yasallıklar"](#legalities) bölümüne bakın. Ayrıca kendi metninizi de yazabilirsiniz.

Söyleyişlerinizin aynı kaynaktan ya da aynı kaynaktan gelmesine gerek yok. Birbirleriyle bir ilgisi olmasına bile gerek yok. Ancak, konuşma uygulamanızda set tümcecikleri (örneğin, "Oturum açtınız" olarak adlandırılır) kullanacaksanız, bunları komut dosyanıza eklediğinizden emin olun. Bu, özel sesinizi bu ifadeleri iyi telaffuz etme şansı verecektir. Ve eğer sentezlenmiş konuşma yerine bir kayıt kullanmaya karar verirseniz, zaten aynı seste olacak.

Tutarlılık ses yeteneği seçiminde anahtar olsa da, çeşitlilik iyi bir komut dosyasının özelliğidir. Komut dosyanız, çeşitli cümle uzunlukları, yapıları ve ruh halleri içeren birçok farklı sözcük ve cümle içermelidir. Dildeki her ses birden çok kez ve çeşitli bağlamlarda temsil edilmelidir *(fonetik kapsama*denir).

Ayrıca, metin belirli bir sesin yazılı olarak temsil edilebildiği tüm yolları birarada bulunmalıdır ve her sesi cümlelerde farklı yerlere yerleştirmelidir. Hem açıklayıcı cümleler hem de sorular eklenmeli ve uygun tonlama ile okunmalıdır.

Özel Konuşma portalının iyi bir ses oluşturmasına izin verecek *kadar* veri sağlayan bir komut dosyası yazmak zordur. Uygulamada, sağlam fonetik kapsama sağlayan bir komut dosyası yapmanın en basit yolu çok sayıda örnek eklemektir. Microsoft'un sağladığı standart sesler on binlerce söyleyiş den oluşturulmuştür. Bir üretim kalitesinde özel ses oluşturmak için en az birkaç bin utterances kaydetmek için hazır olmalıdır.

Hatalar için komut dosyasını dikkatle denetleyin. Mümkünse, başka birinin de kontrol edin. Senaryoyu yeteneğinle incelediğinde, muhtemelen birkaç hata daha yakalarsın.

### <a name="script-format"></a>Komut dosyası biçimi

Komut dosyanızı Microsoft Word'e yazabilirsiniz. Komut dosyası kayıt oturumu sırasında kullanım içindir, böylece çalışmak kolay bulmak herhangi bir şekilde ayarlayabilirsiniz. Özel Ses portalı tarafından ayrı ayrı gerekli olan metin dosyasını oluşturun.

Temel komut dosyası biçimi üç sütun içerir:

* 1'den başlayan söz sayısı. Numaralandırma, stüdyodaki herkesin belirli bir söze başvurmasını kolaylaştırır ("356 numarayı tekrar deneyelim"). Tablonun satırlarını otomatik olarak numaralandırmak için Word paragraf numaralandırma özelliğini kullanabilirsiniz.
* Tamamlanmış kayıtta bulmanıza yardımcı olmak için her bir söyleyşinin alma numarasını veya zaman kodunu yazacağınız boş bir sütun.
* Söyleyiş metni.

![Örnek betik](media/custom-voice/script.png)

> [!NOTE]
> En stüdyoları olarak bilinen kısa segmentlerde kayıt *alır.* Her almak genellikle 10 ila 24 kelime içerir. Sadece alma numarasını belirterek daha sonra bir sözcük bulmak için yeterlidir. Daha uzun kayıt yapmayı tercih eden bir stüdyoda kayıt yapıyorsanız, bunun yerine zaman kodunu not etmek isteyebilirsiniz. Stüdyo önemli bir zaman göstergesi olacak.

Her satırdan sonra not yazmak için yeterli alan bırakın. Hiçbir söyleyiş sayfaları arasında bölünmüş olduğundan emin olun. Sayfaları numaralandırmave komut dosyanızı kağıdın bir tarafına yazdırın.

Senaryonun üç kopyasını yazdırın: biri yetenek için, biri mühendis için, diğeri de yönetmen (siz) için. Zımba yerine ataş kullanın: Deneyimli bir ses sanatçısı, sayfalar döndürüldükçe gürültü yapmamak için sayfaları ayırır.

### <a name="legalities"></a>Legalities

Telif hakkı yasasına göre, bir aktörün telif hakkı yla korunan metni okuması, eserin yazarının tazmin edilmesi gereken bir performans olabilir. Bu performans son ürün, özel ses tanınabilir olmayacaktır. Buna rağmen, bu amaçla telif hakkı yla korunan bir çalışmayı kullanmanın yasallığı iyi belirlenmemiş değildir. Microsoft bu konuda yasal danışmanlık sağlayamaz; kendi avukatınıza danışın.

Neyse ki, tamamen bu sorunları önlemek mümkündür. İzinsiz veya lisanssız kullanabileceğiniz birçok metin kaynağı vardır.

|Metin kaynağı|Açıklama|
|-|-|
|[CMU Arktik korpus](http://festvox.org/cmu_arctic/)|Telif hakkı dışında seçilen yaklaşık 1100 cümle, özellikle konuşma sentezi projelerinde kullanılmak üzere çalışır. Mükemmel bir başlangıç noktası.|
|Artık çalışmıyor<br>telif hakkı altında|Genellikle 1923'den önce yayınlanan eserler. Project [Gutenberg,](https://www.gutenberg.org/) İngilizce olarak on binlerce bu tür eser sunmaktadır. Dil modern İngilizce'ye daha yakın olacağı için yeni çalışmalara odaklanmak isteyebilirsiniz.|
|Hükümet&nbsp;çalışmaları|Amerika Birleşik Devletleri hükümeti tarafından oluşturulan eserlerin telif hakkı AMERIKA Birleşik Devletleri'nde değildir, ancak hükümet diğer ülkelerde/bölgelerde telif hakkı talep edebilir.|
|Kamu malı|Telif hakkıaçıkça reddedildiği veya kamu malı olarak tahsis edilmiş çalışmalar. Bazı yargı alanlarında telif haklarından tamamen feragat etmek mümkün olmayabilir.|
|İzin lisanslı işler|Creative Commons veya GNU Free Documentation License (GFDL) gibi lisans altında dağıtılan çalışmalar. Vikipedi GFDL kullanır. Ancak bazı lisanslar, lisanslı içeriğin performansına, özel bir ses modeli oluşturulmasını etkileyebilecek kısıtlamalar getirebilir, bu nedenle lisansı dikkatle okuyun.|

## <a name="recording-your-script"></a>Komut dosyanızı kaydetme

Senaryonuzu ses lendirme konusunda uzmanlaşmış profesyonel bir kayıt stüdyosunda kaydedin. Bir kayıt kabini, doğru ekipman ve onu çalıştırmak için doğru insanlar olacak. Kayıtta eksik olmamak işe yarıyor.

Projenizi stüdyonun kayıt mühendisiyle tartışın ve onların tavsiyelerini dinleyin. Kayıt çok az veya hiç dinamik aralık sıkıştırma (maksimum 4:1) olmalıdır. İstenmeyen seslerden arınmış ken, sesin tutarlı ses düzeyine ve yüksek sinyal-gürültü oranına sahip olması çok önemlidir.

### <a name="do-it-yourself"></a>Kendin yap.

Eğer kayıt kendiniz yapmak istiyorsanız, yerine bir kayıt stüdyosuna gidiyor, burada kısa bir astar' s. Ev kayıt ve podcasting yükselişi sayesinde, her zamankinden daha kolay iyi kayıt tavsiye ve kaynakları online bulmak için.

Sizin "kayıt kabini" hiçbir fark yankı veya "oda sesi ile küçük bir oda olmalıdır." Mümkün olduğunca sessiz ve ses geçirmez olmalıdır. Duvarlardaki perdeler yankıyı azaltmak ve odanın sesini nötralize etmek veya "deaden" için kullanılabilir.

Ses kaydı için tasarlanmış yüksek kaliteli stüdyo kondansatör mikrofonu (kısaca mikrofon) kullanın. Sennheiser, AKG ve hatta yeni Zoom mikrofonlar iyi sonuçlar verebilir. Bir mikrofon satın alabilir veya yerel bir görsel-işitsel kiralama firmasından kiralayabilirsiniz. USB arabirimi olan bir tane arayın. Bu mikrofon türü mikrofon öğesini, preamp'ı ve analogdan dijitale dönüştürücüöğeyi tek bir pakette rahatlıkla birleştirerek bağlantıyı basitleştirir.

Analog mikrofon da kullanabilirsiniz. Birçok kiralık evler kendi ses karakteri ile ünlü "vintage" mikrofonlar sunuyoruz. Profesyonel analog dişlinin tüketici ekipmanlarında kullanılan 1/4 inç fiş yerine dengeli XLR konektörleri kullandığını unutmayın. Analog giderseniz, ayrıca bu konektörler ile bir preamp ve bir bilgisayar ses arabirimi gerekir.

Mikrofonu bir standa veya bom'a tonuyla tonuyla tonuyla tonuyla mikrofonun önüne "p" ve "b" gibi "plosive" ünsüzlerden gelen gürültüyü ortadan kaldırın. Bazı mikrofonlar, onları standdaki titreşimlerden izole eden bir süspansiyon yuvasıyla birlikte gelir, bu da yardımcı olur.

Ses yeteneği mikrofondan tutarlı bir mesafede kalmalıdır. Nerede durmaları gerektiğini işaretlemek için yerdeki bandı kullanın. Yetenek oturmayı tercih ederse, mikrofon mesafesini izlemek ve sandalye gürültüsünden kaçınmak için özel dikkat edin.

Senaryoyu tutmak için bir stand kullanın. Sesi mikrofona yansıtabilmesi için standı oltaya kullanmaktan kaçının.

Kayıt ekipmanını işleten kişi-mühendis- kayıt kabinindeki yetenekle *(bir geri dönüş devresi)* konuşmak için yetenekten ayrı bir odada olmalıdır.

Kayıt, 80-db sinyal-gürültü oranı veya daha iyi bir hedefile mümkün olduğunca az gürültü içermelidir.

"Standınızda sessizlik kaydını" yakından dinleyin, herhangi bir gürültünün nereden geldiğini öğrenin ve nedenini ortadan kaldırın. Ortak gürültü kaynakları hava delikleri, floresan ışık balastları, yakındaki yollardaki trafik ve ekipman fanlarıdır (dizüstü bilgisayarların bile fanları olabilir). Mikrofonlar ve kablolar, genellikle uğultu veya vızıltı olmak üzere yakındaki AC kablolarından elektrik gürültüsü nükten alabilirsiniz. Bir vızıltı da bir *zemin döngüsü*neden olabilir , hangi ekipman birden fazla elektrik devresine takılı sahip neden olur.

> [!TIP]
> Bazı durumlarda, kayıtlarınızdaki gürültüyü kaldırmaya yardımcı olmak için bir ekolayzır veya gürültü azaltma yazılımı eklentisi kullanabilirsiniz, ancak her zaman kaynağında durdurmak en iyisidir.

Düzeyleri, kullanılabilir dinamik dijital kayıt aralığının çoğunun aşırı sürüş olmadan kullanılacak şekilde ayarlayın. Bu, sesi yüksek sesle ayarlamak anlamına gelir, ancak bozuk olacak kadar yüksek değil. İyi bir kaydın dalga formuna bir örnek aşağıdaki resimde gösterilmiştir:

![İyi bir kayıt dalga formu](media/custom-voice/good-recording.png)

Burada, aralığın (yükseklik) çoğu kullanılır, ancak sinyalin en yüksek zirveleri pencerenin üst veya alt ulaşmaz. Ayrıca, kayıttaki sessizliğin düşük gürültülü bir zemini gösteren ince yatay bir çizgiye yaklaşdığını da görebilirsiniz. Bu kayıt kabul edilebilir dinamik aralık ve sinyal-gürültü oranı vardır.

Kullandığınız mikrofona bağlı olarak, yüksek kaliteli ses arabirimi veya USB bağlantı noktası üzerinden doğrudan bilgisayara kaydedin. Analog için ses zincirini basit tutun: mikrofon, preamp, ses arabirimi, bilgisayar. Hem [Avid Pro Tools'u](https://www.avid.com/en/pro-tools) hem de [Adobe Audition'ı](https://www.adobe.com/products/audition.html) aylık olarak makul bir ücretle lisanslayabilirsiniz. Bütçeniz son derece sıkı ise, ücretsiz [Audacity](https://www.audacityteam.org/)deneyin.

44,1 kHz 16 bit monofonik (CD kalitesi) veya daha iyisi olarak kaydolun. Ekipmanınız destekliyorsa, geçerli son teknoloji ürünü 48 kHz 24 bit'tir. Özel Ses portalına göndermeden önce sesinizi 16 kHz 16 bit'e indireceksiniz. Yine de, bu durumda edinilmiş olması durumunda yüksek kaliteli orijinal kayıt için öder.

İdeal olarak, farklı insanlar yönetmen, mühendis ve yetenek rollerinde hizmet var. Hepsini kendin yapmaya çalışma. Bir tutam olarak, bir kişi hem yönetmen hem de mühendis olabilir.

### <a name="before-the-session"></a>Oturumdan önce

Stüdyo zamanını boşa harcamamak için, kayıt oturumundan önce ses yeteneğinizle senaryoyu gözden geçirin. Ses yeteneği metne aşina olurken, yabancı sözcüklerin telaffuzunu açıklığa kavuşturabilirler.

> [!NOTE]
> Çoğu kayıt stüdyosu kayıt kabininde komut dosyalarının elektronik ekranını sunar. Bu durumda, run-through notlarınızı doğrudan komut dosyasının belgesine yazın. Yine de oturum sırasında not almak için bir kağıt kopya isteyeceksiniz. Çoğu mühendis de basılı bir kopya isteyecektir. Ve yine de bilgisayar aşağı durumda yetenek için yedek olarak üçüncü bir basılı kopya isteyeceksiniz.

Ses yeteneğiniz bir sözcükte vurgulanan kelimeyi ("operatif kelime") sorabilir. Onlara, belirli bir vurgu olmadan doğal bir okuma istediğinizi söyleyin. Konuşma sentezlendiğinde vurgu eklenebilir; orijinal kaydın bir parçası olmamalıdır.

Kelimeleri belirgin bir şekilde telaffuz etmek için yeteneği yönlendirin. Senaryonun her kelimesi yazılı olarak telaffuz edilmelidir. Sesler, *komut dosyasında bu şekilde yazılmadığı sürece,* gündelik konuşmalarda yaygın olduğu gibi, atlanmamalı veya birlikte bulamaçlanmamalıdır.

|Yazılı metin|İstenmeyen gündelik telaffuz|
|-|-|
|Asla senden vazgeçmeyeceğim.|Asla senden vazgeçmeyeceğim.|
|dört ışık var|Dört ışık var.|
|Bugün hava nasıl?|bugün hava nasıl|
|küçük arkadaşıma merhaba de|lil arkadaşıma merhaba de|

Yetenek sözcükler arasında belirgin duraklamalar *eklememelidir.* Cümle hala doğal olarak akmalıdır, biraz resmi sondaj bile. Bu ince ayrım doğru almak için pratik alabilir.

### <a name="the-recording-session"></a>Kayıt oturumu

Oturumun başında tipik bir söyleyiş için bir başvuru kaydı veya *eşleç dosyası* oluşturun. Yetenekten bu satırı her sayfada tekrar etmesini isteyin. Her seferinde, yeni kaydı başvuruyla karşılaştırın. Bu uygulama yetenek hacim, tempo, pitch ve tonlama tutarlı kalmasına yardımcı olur. Bu arada, mühendis düzeyleri ve ses genel tutarlılık için bir referans olarak maç dosyası kullanabilirsiniz.

Bir aradan sonra veya başka bir gün kayıt devam ederken eşler dosyası özellikle önemlidir. Yetenek için birkaç kez oynamak ve iyi eşleşen kadar her zaman tekrar lamak isteyeceksiniz.

Koçluk yetenek derin bir nefes almak ve her söyleyiş önce bir an için duraklama. Söylenmeler arasında birkaç saniyelik sessizlik kaydet. Sözcükler, bağlam göz önünde bulundurularak, her göründüklerinde aynı şekilde telaffuz edilmelidir. Örneğin, fiil olarak "kayıt" bir isim olarak "kayıt" farklı telaffuz edilir.

"Oda tonunun" yakalanması için ilk kayıttan önce beş saniyelik iyi bir sessizlik kaydedin. Bu uygulama, Özel Ses portalının kayıtlarda kalan gürültüyü telafi etmesine yardımcı olur.

> [!TIP]
> Gerçekten yakalamak için gereken tek şey ses yeteneği, böylece sadece kendi hatları bir monofonik (tek kanallı) kayıt yapabilirsiniz. Ancak, stereo'da kaydederseniz, belirli satırların veya çekimlerin tartışmasını yakalamak için denetim odasındaki gevezeliği kaydetmek için ikinci kanalı kullanabilirsiniz. Bu parçayı Özel Ses portalına yüklenen sürümden kaldırın.

Kulaklık kullanarak ses yeteneğinin performansını yakından dinleyin. İyi ama doğal diksiyon, doğru telaffuz ve istenmeyen seslerin eksikliğini arıyorsunuz. Yeteneğinizden bu standartlara uymayan bir söyleyişi yeniden kaydetmesini istemekten çekinmeyin.

> [!TIP]
> Çok sayıda söyleyiş kullanıyorsanız, tek bir söyleyiş, ortaya çıkan özel ses üzerinde belirgin bir etkiye sahip olmayabilir. Sorunlarla ilgili tüm söyleyişleri not etmek, bunları veri setinizden dışlamak ve özel sesinizin nasıl çıktığını görmek daha uygun olabilir. Her zaman stüdyoya geri dönüp kaçırılan örnekleri daha sonra kaydedebilirsiniz.

Her bir söyleyiş için komut dosyanızdaki numara veya zaman kodunu not edin. Mühendisten kaydın meta verisindeki veya işaret sayfasındaki her bir söyleyiyi işaretlemesini isteyin.

Düzenli molalar verin ve ses yeteneğinizin seslerini iyi durumda tutmasına yardımcı olacak bir içecek sağlayın.

### <a name="after-the-session"></a>Oturumdan sonra

Modern kayıt stüdyoları bilgisayarlarda çalışır. Oturumun sonunda, bir veya daha fazla ses dosyası alırsınız, bir teyp değil. Bu dosyalar muhtemelen CD kalitesinde WAV veya AIFF formatında (44.1 kHz 16-bit) veya daha iyi olacaktır. 48 kHz 24-bit yaygın ve arzu edilir. 96 kHz gibi daha yüksek örnekleme oranları genellikle gerekli değildir.

Özel Ses portalı, sağlanan her bir söyleyyenin kendi dosyasında olmasını gerektirir. Stüdyo tarafından teslim edilen her ses dosyası birden fazla söyleyiş içerir. Yani birincil post-prodüksiyon görevi kayıtları bölmek ve teslim için hazırlamaktır. Kayıt mühendisi, her bir söyleyyenin nerede başladığını belirtmek için dosyaya işaretçiler koymuş (veya ayrı bir işaret sayfası sağlamış olabilir).

İstediğiniz tam alır bulmak için notlarınızı kullanın ve sonra yeni bir dosyaya her söyleyiş kopyalamak için [Avid Pro Tools](https://www.avid.com/en/pro-tools), [Adobe Audition](https://www.adobe.com/products/audition.html), veya ücretsiz [Audacity](https://www.audacityteam.org/)gibi bir ses düzenleme programı kullanın.

İlki hariç, her klibin başında ve sonunda yalnızca 0,2 saniyelik sessizlik bırakın. Bu dosya tam beş saniyelik sessizlikle başlamalı. Dosyanın sessiz bölümlerini "sıfırlamak" için ses düzenleyicisi kullanmayın. "Oda tonu" dahil Özel Ses algoritmaları herhangi bir artık arka plan gürültüsü telafi yardımcı olacaktır.

Her dosyayı dikkatle dinleyin. Bu aşamada, kayıt sırasında kaçırdığınız küçük istenmeyen sesleri, bir satırdan önce hafif bir dudak şaplak gibi düzenleyebilirsiniz, ancak gerçek konuşmaları kaldırmamaya dikkat edin. Bir dosyayı düzeltemezseniz, dosyayı veri setinizden kaldırın ve bunu yaptığınıza dikkat edin.

Her dosyayı kaydetmeden önce 16 bit ve örnek hızı 16 kHz'e dönüştürün ve stüdyo gevezeliğini kaydettiyseniz, ikinci kanalı kaldırın. Dosyaları komut dosyanızdan gelen sözcük numarasıyla adlandırarak her dosyayı WAV biçiminde kaydedin.

Son olarak, her WAV dosyasını ilgili söyleyinçliğin metin sürümüyle ilişkilendiren *transkripti* oluşturun. [Özel ses yazı tipleri oluşturma,](how-to-customize-voice-font.md) gerekli biçimin ayrıntılarını içerir. Metni doğrudan komut dosyanızdan kopyalayabilirsiniz. Sonra WAV dosyaları ve metin transkript bir Zip dosyası oluşturun.

Daha sonra ihtiyacınız olması durumunda orijinal kayıtları güvenli bir yerde arşivleyin. Senaryonuzu ve notlarınızı da koruyun.

## <a name="next-steps"></a>Sonraki adımlar

Kayıtlarınızı yüklemeye ve özel sesinizi oluşturmaya hazırsınız.

> [!div class="nextstepaction"]
> [Özel ses yazı tipleri oluşturma](how-to-customize-voice-font.md)

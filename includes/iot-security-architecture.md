---
title: include dosyası
description: include dosyası
services: iot-fundamentals
author: robinsh
ms.service: iot-fundamentals
ms.topic: include
ms.date: 08/07/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: a2eafd6bb34b897f3492ddcffd6841f0fabc4ca7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73034555"
---
Bir sistem tasarlarken, bu sisteme yönelik olası tehditleri anlamak ve sistem tasarlanıp tasarlandığından uygun savunmayı buna göre eklemek önemlidir. Bir saldırganın bir sistemden nasıl ödün verebileceğini anlamak, en başından beri uygun azaltıcı etkenlerin yerinde olduğundan emin olmaya yardımcı olduğundan, ürünü en başından beri güvenlik göz önünde bulundurularak tasarlamak önemlidir.

## <a name="security-starts-with-a-threat-model"></a>Güvenlik bir tehdit modeliyle başlar

Microsoft, ürünleri için uzun zamandır tehdit modelleri kullanmıştır ve şirketin tehdit modelleme işlemini herkese açık hale getirmiştir. Şirket deneyimi modelleme en endişe verici tehditlerin hemen anlayış ötesinde beklenmedik yararları olduğunu göstermektedir. Örneğin, geliştirme ekibinin dışındaki diğer kişilerle açık bir tartışma için de bir yol oluşturur ve bu da üründe yeni fikirler ve iyileştirmelere yol açabilir.

Tehdit modellemesinin amacı, bir saldırganın bir sistemi nasıl tehlikeye atabileceğini anlamak ve ardından uygun azaltıcı etkenlerin nasıl yerinde olduğundan emin olmaktır. Tehdit modelleme, sistem bir sistem dağıtıldıktan sonra değil, sistem tasarlanırken, tasarım ekibini azaltıcı etkenleri düşünmeye zorlar. Bu durum kritik öneme sahip, çünkü güvenlik savunmasını niçin alanında bulunan sayısız cihaza güçlendirmesi olanaksız, hataya yatkın ve müşterileri risk altında bırakıyor.

Birçok geliştirme ekibi, müşterilerin yararına olan sistem için işlevsel gereksinimleri yakalamak için mükemmel bir iş yapmak. Ancak, birinin sistemi kötüye kullanabileceği açık olmayan yolları belirlemek daha zordur. Tehdit modelleme, geliştirme ekiplerinin saldırganın ne yapabileceğini ve neden yapabileceğini anlamalarını yardımcı olabilir. Tehdit modelleme, sistemdeki güvenlik tasarım kararlarının yanı sıra güvenliği etkileyen yol boyunca yapılan tasarım değişiklikleri hakkında bir tartışma yaratan yapılandırılmış bir işlemdir. Tehdit modeli basit bir belge olsa da, bu dokümantasyon aynı zamanda bilginin sürekliliğini, öğrenilen derslerin tutulmasını ve yeni ekibin hızla gemide olmasına yardımcı olmak için ideal bir yolu temsil eder. Son olarak, tehdit modellemesinin bir sonucu, müşterilerinize sağlamak istediğiniz güvenlik taahhütleri gibi güvenliğin diğer yönlerini göz önünde bulundurmanızı sağlamaktır. Bu taahhütler, tehdit modelleme ile birlikte Nesnelerin İnterneti (IoT) çözümünüzü bilgilendirir ve test eder.

### <a name="when-to-do-threat-modeling"></a>Tehdit modellemesi ne zaman yapılacak?

[Tehdit modelleme,](https://www.microsoft.com/en-us/sdl/adopt/threatmodeling.aspx) tasarım aşamasına dahil ettiğinizde en büyük değeri sunar. Tasarlarken, tehditleri ortadan kaldırmak için değişiklik yapmak için en büyük esnekliğe sahip siniz. Tasarım la tehditleri ortadan kaldırmak istenen sonuçtur. Bu azaltıcı ekleme, test ve güncel ve dahası kalmasını sağlamak çok daha kolaydır, bu tür ortadan kaldırılması her zaman mümkün değildir. Bir ürün daha olgun hale geldikçe tehditleri ortadan kaldırmak zorlaşır ve sonuçta daha fazla iş ve geliştirme erken tehdit modelleme daha çok daha zor tradeoffs gerektirir.

### <a name="what-to-consider-for-threat-modeling"></a>Tehdit modellemesi için dikkat etmek gerekenler

Çözüme bir bütün olarak bakmalı ve aşağıdaki alanlara odaklanmalısınız:

* Güvenlik ve gizlilik özellikleri
* Hataları güvenlikle ilgili olan özellikler
* Güven sınırına dokunan özellikler

### <a name="who-performs-threat-modeling"></a>Tehdit modellemesi kim gerçekleştirir

Tehdit modellemesi de diğerleri gibi bir süreçtir. Tehdit modeli belgesini çözümün diğer herhangi bir bileşeni gibi ele almak ve doğrulamak iyi bir fikirdir. Birçok geliştirme ekibi, müşterilerin yararına olan sistem için işlevsel gereksinimleri yakalamak için mükemmel bir iş yapmak. Ancak, birinin sistemi kötüye kullanabileceği açık olmayan yolları belirlemek daha zordur. Tehdit modelleme, geliştirme ekiplerinin saldırganın ne yapabileceğini ve neden yapabileceğini anlamalarını yardımcı olabilir.

### <a name="how-to-perform-threat-modeling"></a>Tehdit modellemesi nasıl gerçekleştirilir?

Tehdit modelleme işlemi dört adımdan oluşur; adımlar şunlardır:

* Uygulamayı modelleme
* Tehditleri Sayısalat
* Tehditleri azaltma
* Azaltıcı etkenleri doğrula

#### <a name="the-process-steps"></a>İşlem adımları

Bir tehdit modeli oluştururken akılda tutulması gereken başparmak üç kural:

1. Başvuru mimarisinden bir diyagram oluşturun.

2. Önce genişlik başla. Derin dalışyapmadan önce genel bir bakış alın ve sistemi bir bütün olarak anlayın. Bu yaklaşım, doğru yerlere derinlemesine dalmak emin olur.

3. İşlemi yönlendirin, işlemin sizi yönlendirmesine izin vermeyin. Modelleme aşamasında bir sorun bulmak ve bunu keşfetmek istiyorsanız, bunun için gidin! Bu adımları kölece takip etmek zorunda hissetmiyorum.

#### <a name="threats"></a>Tehditler

Bir tehdit modelinin dört temel unsuru şunlardır:

* Web hizmetleri, Win32 hizmetleri ve *nix daemons gibi işlemler. Bazı karmaşık varlıklar (örneğin alan ağ geçitleri ve sensörler) bu alanlarda teknik bir detaylandırma nın mümkün olmadığı bir süreç olarak soyutlanabilir.

* Veri depoları (yapılandırma dosyası veya veritabanı gibi her yerde veri depolanır)

* Veri akışı (veri uygulamadaki diğer öğeler arasında hareket ettiği yer)

* Dış Varlıklar (sistemle etkileşimedebilen, ancak uygulamanın kontrolü altında olmayan her şey, örnekler arasında kullanıcılar ve uydu akışları yer almaktadır)

Mimari diyagramdaki tüm unsurlar çeşitli tehditlere tabidir; Bu makalestride mnemonic. Stride öğeleri hakkında daha fazla bilgi için [Threat Modeling Again, STRIDE](https://blogs.msdn.microsoft.com/larryosterman/2007/09/04/threat-modeling-again-stride/) okuyun.

Uygulama diyagramının farklı öğeleri belirli STRIDE tehditlerine tabidir:

* Süreçler STRIDE'a tabidir
* Veri akışları TID'ye tabidir
* Veri depoları TID'ye ve bazen de veri depoları günlük dosyaları olduğunda R'ye tabidir.
* Dış varlıklar SRD tabidir

## <a name="security-in-iot"></a>IoT'de güvenlik

Bağlı özel amaçlı aygıtlar, bu aygıtlara dijital erişimi sağlamak için bir çerçeve sağlamak için düşünülmelidir potansiyel etkileşim yüzey alanları ve etkileşim desenleri önemli sayıda var. "Dijital erişim" terimi burada, fiziksel erişim denetimi yoluyla erişim güvenliğinin sağlandığı doğrudan aygıt etkileşimi yoluyla gerçekleştirilen tüm işlemlerden ayırt etmek için kullanılır. Örneğin, cihazı kapıda kilit olan bir odaya yerleştirmek. Fiziksel erişim yazılım ve donanım kullanılarak reddedilemez olsa da, fiziksel erişimin sistem parazitine yol açtığını önlemek için önlemler alınabilir.

Etkileşim modellerini incelerken, aynı düzeyde dikkat çekerken "aygıt denetimi" ve "aygıt verilerine" bakın. "Aygıt denetimi", bir cihaza herhangi bir taraftarafından, kendi durumuna veya çevresinin durumuna yönelik davranışını değiştirmek veya etkilemek amacıyla sağlanan herhangi bir bilgi olarak sınıflandırılabilir. "Aygıt verileri", bir aygıtın durumu ve çevrenin gözlenen durumu hakkında başka bir tarafa yadığı herhangi bir bilgi olarak sınıflandırılabilir.

Güvenlik en iyi uygulamalarını optimize etmek için, tipik bir IoT mimarisinin tehdit modelleme alıştırmasının bir parçası olarak birkaç bileşene/bölgeye ayrılması önerilir. Bu bölgeler bu bölüm boyunca tam olarak açıklanmıştır ve şunları içerir:

* Aygıt
* Alan Ağ Geçidi,
* Bulut ağ geçitleri ve
* Hizmetleri.

Bölgeler, bir çözümü segmente etmenin geniş bir yoludur; her bölgenin genellikle kendi veri ve kimlik doğrulama ve yetkilendirme gereksinimleri vardır. Bölgeler, hasarı izole etmek ve düşük güven bölgelerinin daha yüksek güven bölgeleri üzerindeki etkisini kısıtlamak için de kullanılabilir.

Her bölge, aşağıdaki diyagramda noktalı kırmızı çizgi olarak belirtildiği bir Güven Sınırı ile ayrılır. Veri/bilginin bir kaynaktan diğerine geçişini temsil eder. Bu geçiş sırasında, veri/bilgi Sızdırma, Tahrifat, Reddetme, Bilgi Açıklama, Hizmet Reddi ve Ayrıcalık Yükselmesine (STRIDE) tabi olabilir.

![IoT Güvenlik Bölgeleri](media/iot-security-architecture/iot-security-architecture-fig1.png) 

Her sınır içinde betimlenen bileşenler de STRIDE'a tabi tutularak çözümün tam 360 tehdit modelleme görünümüne olanak sağlar. Aşağıdaki bölümlerde bileşenlerin her biri ve uygulamaya konması gereken belirli güvenlik endişeleri ve çözümleri ayrıntılı.

Aşağıdaki bölümlerde genellikle bu bölgelerde bulunan standart bileşenler tartışılır.

### <a name="the-device-zone"></a>Cihaz bölgesi

Cihaz ortamı, fiziksel erişimin ve/veya "yerel ağ" eşler arası dijital erişimin mümkün olduğu aygıtın çevresindeki hemen fiziksel alandır. "Yerel ağ"ın, kamu interneti ile farklı ve izole edilmiş ama potansiyel olarak köprülenmiş ve cihazların eşler arası iletişimine izin veren kısa menzilli kablosuz radyo teknolojisini içeren bir ağ olduğu varsayılır. Böyle bir yerel ağın yanılsamasını yaratan herhangi bir ağ sanallaştırma teknolojisi *içermez* ve eşler arası iletişim ilişkisi ne olursa olsun, herhangi iki aygıtın ortak ağ alanı üzerinden iletişim kurmasını gerektiren ortak operatör ağlarını da içermez.

### <a name="the-field-gateway-zone"></a>Alan ağ geçidi bölgesi

Alan ağ geçidi, iletişim aracı ve potansiyel olarak bir aygıt kontrol sistemi ve aygıt veri işleme merkezi olarak hareket eden bir aygıt/cihaz veya bazı genel amaçlı sunucu bilgisayar yazılımıdır. Alan ağ geçidi bölgesi, alan ağ geçidinin kendisini ve ona bağlı tüm aygıtları içerir. Adından da anlaşılacağı gibi, alan ağ geçitleri özel veri işleme tesisleri dışında hareket, genellikle konum bağlı, potansiyel fiziksel müdahaleye tabidir ve sınırlı operasyonel fazlalık vardır. Tüm bir alan ağ geçidi genellikle bir şey bir şey bir dokunmatik ve sabotaj işlevini ne olduğunu bilerek olduğunu söylemek.

Alan ağ geçidi, erişim ve bilgi akışını niçin yönetilmesinde etkin bir rolü olduğu için yalnızca trafik yönlendiricisinden farklıdır, yani uygulama nın ele aldığı varlık ve ağ bağlantısı veya oturum terminalidir. Buna karşılık, bir NAT aygıtı veya güvenlik duvarı, açık bağlantı veya oturum terminalleri değil, onlar aracılığıyla yapılan bir rota (veya blok) bağlantıları veya oturumları olduğundan alan ağ geçidi olarak nitelendirilir. Alan ağ geçidinin iki ayrı yüzey alanı vardır. Biri ona bağlı olan aygıtlara bakıyor ve bölgenin içini temsil ediyor, diğeri ise tüm dış taraflara bakıyor ve bölgenin kenarı.

### <a name="the-cloud-gateway-zone"></a>Bulut ağ geçidi bölgesi

Bulut ağ geçidi, genellikle bulut tabanlı bir denetim ve veri analiz sistemi olan bu tür sistemlerin federasyonu olan bulut tabanlı denetim ve veri analiz sistemine yönelik olarak, genel ağ alanı ndaki birkaç farklı siteden aygıtlardan veya alan ağ geçitlerinden uzaktan iletişim sağlayan bir sistemdir. Bazı durumlarda, bir bulut ağ geçidi tabletler veya telefonlar gibi terminallerden özel amaçlı aygıtlara erişimi hemen kolaylaştırabilir. Burada tartışılan bağlamda,"bulut", ekli aygıtlarla veya alan ağ geçitleri ile aynı siteye bağlı olmayan özel bir veri işleme sistemine başvurmak içindir. Ayrıca Bulut Bölgesi'nde, operasyonel önlemler hedeflenen fiziksel erişimi engeller ve mutlaka bir "genel bulut" altyapısına maruz kalmaktadır.  

Bulut ağ geçidi, bulut ağ geçidini ve bağlı tüm aygıtlarını veya alan ağ geçitlerini başka bir ağ trafiğinden yalıtmak için ağ sanallaştırma bindirmesine eşlenebilir. Bulut ağ geçidinin kendisi bir aygıt kontrol sistemi veya aygıt verileri için bir işleme veya depolama tesisi değildir; bu tesisler bulut ağ geçidi ile arayüz. Bulut ağ geçidi bölgesi, bulut ağ geçidinin kendisini, doğrudan veya dolaylı olarak bağlı tüm alan ağ geçitleri ve aygıtlarıyla birlikte içerir. Bölgenin kenarı, tüm dış tarafların iletişim kurduğu ayrı bir yüzey alanıdır.

### <a name="the-services-zone"></a>Hizmet bölgesi

Bu bağlamda bir "hizmet", veri toplama ve analiz için bir alan veya bulut ağ geçidi nin yanı sıra komuta ve denetim için aygıtlarla iç içe geçmiş herhangi bir yazılım bileşeni veya modülü olarak tanımlanır. Hizmetler arabuluculuk. Ağ geçitlerine ve diğer alt sistemlere karşı kimlikleri altında hareket ederler, verileri depolar ve analiz ederler, veri öngörülerine veya zamanlamalarına dayalı olarak aygıtlara bağımsız olarak komut lar verirler ve bilgi ve kontrol yeteneklerini yetkili son kullanıcılara sunarlar.

### <a name="information-devices-versus-special-purpose-devices"></a>Özel amaçlı cihazlara karşı bilgi cihazları

CD'ler, telefonlar ve tabletler öncelikle etkileşimli bilgi aygıtlarıdır. Telefonlar ve tabletler pil ömrünü en üst düzeye çıkarma konusunda açıkça optimize edilmiştir. Tercihen, bir kişiyle hemen etkileşime girmediğinde veya müzik çalmak veya sahibini belirli bir konuma yönlendirmek gibi hizmetler sunmadıkları nda kısmen kapatırlar. Sistem açısından bakıldığında, bu bilgi teknolojisi cihazları esas olarak insanlara karşı vekil olarak hareket ediyorlar. Onlar "insan aktüatörler" eylemleri ve "insan sensörleri" giriş toplama düşündüren vardır.

Basit sıcaklık sensörlerinden içinde binlerce bileşeniçeren karmaşık fabrika üretim hatlarına kadar özel amaçlı cihazlar farklıdır. Bu aygıtlar çok daha fazla amaca yöneliktir ve bazı kullanıcı arabirimi sağlasalar bile, büyük ölçüde fiziksel dünyadaki varlıklarla etkileşime girmek veya bunlara entegre olmak için kapsama sahiptirler. Çevresel koşulları ölçer ve rapor eder, vanaları döndürür, servoları kontrol eder, çalar, ışıkları değiştirir ve diğer birçok görevi yerine getirirler. Onlar için bir bilgi aygıtı ya çok genel, çok pahalı, çok büyük veya çok kırılgan olduğu iş yapmak için yardımcı olur. Somut amaç hemen kendi teknik tasarım yanı sıra üretim ve planlanan ömür boyu çalışma için mevcut parasal bütçe dikte. Bu iki önemli faktörün birleşimi mevcut operasyonel enerji bütçesini, fiziksel ayak izini ve dolayısıyla kullanılabilir depolama, hesaplama ve güvenlik yeteneklerini kısıtlar.

Otomatik veya uzaktan kontrol edilebilir aygıtlarla ilgili bir şey "yanlış giderse" örneğin, fiziksel hatalar veya kasıtlı yetkisiz izinsiz giriş ve manipülasyona yönelik kontrol mantığı hataları. Üretim kuraları yok edilebilir, binalar yağmalanabilir veya yakılabilir, ve insanlar yaralanabilir ya da ölebilir. Bu, çalınan kredi kartı limitini aşan birinden tamamen farklı bir hasar sınıfı. İşleri hareket ettiren aygıtların ve aynı zamanda nesnelerin taşınmasına neden olan komutlarla sonuçlanan sensör verileri için güvenlik çubuğu, herhangi bir e-ticaret veya bankacılık senaryosundan daha yüksek olmalıdır.

### <a name="device-control-and-device-data-interactions"></a>Cihaz kontrolü ve cihaz veri etkileşimleri

Bağlı özel amaçlı aygıtlar, bu aygıtlara dijital erişimi sağlamak için bir çerçeve sağlamak için düşünülmelidir potansiyel etkileşim yüzey alanları ve etkileşim desenleri önemli sayıda var. "Dijital erişim" terimi burada, fiziksel erişim denetimi yoluyla erişim güvenliğinin sağlandığı doğrudan aygıt etkileşimi yoluyla gerçekleştirilen tüm işlemlerden ayırt etmek için kullanılır. Örneğin, cihazı kapıda kilit olan bir odaya yerleştirmek. Fiziksel erişim yazılım ve donanım kullanılarak reddedilemez olsa da, fiziksel erişimin sistem parazitine yol açtığını önlemek için önlemler alınabilir.

Etkileşim modellerini incelerken, tehdit modellemesi sırasında "aygıt denetimi" ve "aygıt verilerine" aynı düzeyde dikkat ile bakın. "Aygıt denetimi", bir cihaza herhangi bir taraftarafından, kendi durumuna veya çevresinin durumuna yönelik davranışını değiştirmek veya etkilemek amacıyla sağlanan herhangi bir bilgi olarak sınıflandırılabilir. "Aygıt verileri", bir aygıtın durumu ve çevrenin gözlenen durumu hakkında başka bir tarafa yadığı herhangi bir bilgi olarak sınıflandırılabilir.

## <a name="performing-threat-modeling-for-the-azure-iot-reference-architecture"></a>Azure IoT başvuru mimarisi için tehdit modellemesi gerçekleştirme

Microsoft, Azure IoT için tehdit modellemesi yapmak için daha önce özetlenen çerçeveyi kullanır. Aşağıdaki bölümde, IoT için tehdit modellemesi hakkında nasıl düşüneceğiniz ve tanımlanan tehditlerin nasıl ele alınılabileceğini göstermek için Azure IoT Başvuru Mimarisi'nin somut örneğini kullanır. Bu örnekte dört ana odak alanı tanımnılatır:

* Cihazlar ve Veri Kaynakları,
* Veri Taşıma,
* Cihaz ve Olay İşleme ve
* Sunum

![Azure IoT için Tehdit Modelleme](media/iot-security-architecture/iot-security-architecture-fig2.png)

Aşağıdaki diyagram, Microsoft Tehdit Modelleme Aracı tarafından kullanılan bir Veri Akış Diyagramı modelini kullanarak Microsoft'un IoT Mimarisinin basitleştirilmiş bir görünümünü sağlar:

![MS Tehdit Modelleme Aracını kullanarak Azure IoT için Tehdit Modelleme](media/iot-security-architecture/iot-security-architecture-fig3.png)

Mimarinin aygıtı ve ağ geçidi özelliklerini birbirinden ayırdığını unutmayın. Bu yaklaşım, kullanıcının daha güvenli ağ geçidi aygıtlarından yararlanmasını sağlar: güvenli protokoller kullanarak bulut ağ geçidiyle iletişim kurabilme yeteneğine sahiptirler ve bu da genellikle termostat gibi yerel bir aygıtın kendi başına sağlar. Azure hizmetleri bölgesinde, Bulut Ağ Geçidi'nin Azure IoT Hub hizmeti tarafından temsil edildiğini varsayalım.

### <a name="device-and-data-sourcesdata-transport"></a>Cihaz ve veri kaynakları/veri aktarım

Bu bölümde, tehdit modelleme merceğinden daha önce özetlenen mimari incelenmiştir ve bazı doğal kaygıların nasıl ele alınacak hakkında genel bir bakış sunar. Bu örnek, bir tehdit modelinin temel öğelerine odaklanır:

* Süreçler (hem sizin denetiminizde hem de harici öğeler)
* İletişim (veri akışları olarak da adlandırılır)
* Depolama (veri depoları olarak da adlandırılır)

#### <a name="processes"></a>İşlemler

Azure IoT mimarisinde özetlenen kategorilerin her birinde, bu örnek, veri/bilginin var olduğu farklı aşamalarda bir dizi farklı tehdidi azaltmaya çalışır: süreç, iletişim ve depolama. Aşağıda "süreç" kategorisi için en yaygın olanların genel bir bakış, bu tehditlerin en iyi nasıl azaltılabilir bir bakış aşağıdaki gibidir:

**Sızdırma (S)**: Saldırgan, yazılım veya donanım düzeyinde bir aygıttan şifreleme anahtar materyali ayıklayabilir ve daha sonra anahtar malzemenin alındığı cihazın kimliği altında farklı bir fiziksel veya sanal aygıtla sisteme erişebilir. İyi bir illüstrasyon herhangi bir TV açabilirsiniz ve popüler şakacı araçları olan uzaktan kumandalar olduğunu.

**Hizmet Reddi (D)**: Bir cihaz, radyo frekanslarına müdahale ederek veya telleri keserek çalışmadan veya iletişim kuramadan kullanılabilir. Örneğin, gücü veya ağ bağlantısı kasıtlı olarak devre dışı olan bir güvenlik kamerası verileri hiç bildiremez.

**Tahrifat (T)**: Bir saldırgan, cihazda çalışan yazılımıkısmen veya tamamen değiştirebilir ve anahtar malzeme nin veya anahtar materyalleri tutan şifreleme tesislerinin yasadışı programa uygun olması durumunda değiştirilen yazılımın aygıtın orijinal kimliğinden yararlanabilmesine izin verebilir. Örneğin, bir saldırgan, iletişim yolundaki aygıttan gelen verileri engellemek ve bastırmak için çıkarılan anahtar malzemeden yararlanabilir ve bunu çalınan anahtar malzemeyle doğrulanmış yanlış verilerle değiştirebilir.

**Bilgi Açıklama (I)**: Cihaz manipüle edilmiş yazılım çalıştırıyorsa, bu tür manipüle edilmiş yazılımlar yetkisiz taraflara veri sızdırabilir. Örneğin, bir saldırgan, bilgi almak için aygıt la denetleyici veya alan ağ geçidi veya bulut ağ geçidi arasındaki iletişim yoluna kendini enjekte etmek için çıkarılan anahtar malzemeden yararlanabilir.

**Ayrıcalık Yüksekliği (E)**: Belirli bir işlevi yapan bir aygıt başka bir şey yapmaya zorlanabilir. Örneğin, yarım açmak için programlanmış bir vana tüm yol açmak için kandırdın olabilir.

| **Bileşen** | **Tehdit** | **Risk azaltma** | **Risk** | **Uygulama** |
| --- | --- | --- | --- | --- |
| Cihaz |S |Aygıta kimlik atama ve aygıtın kimliğini nital etme |Aygıtın veya aygıtın bir bölümünü başka bir aygıtla değiştirme. Doğru cihazla konuştuğunu nereden biliyorsun? |Aktarım Katmanı Güvenliği (TLS) veya IPSec kullanarak aygıtın kimliğinin doğrulanması. Altyapı, tam asimetrik şifrelemeyi işleyemeyen cihazlarda önceden paylaşılan anahtar (PSK) kullanılmasını desteklemelidir. Azure AD, [OAuth'dan](https://www.rfc-editor.org/pdfrfc/rfc6755.txt.pdf) yararlanın |
|| TRID |Örneğin, anahtarve diğer şifreleme materyallerinin cihazdan ayıklanması imkansız hale getirerek, aygıta kurcalanmayakarşı mekanizmalar uygulayın. |Risk, birinin cihazı kurcaladığı (fiziksel girişim) olmasıdır. O cihazın kurcalanmadığından nasıl eminsin? |En etkili azaltma, anahtarların okunamadığı, ancak yalnızca anahtarı kullanan ancak anahtarı asla ifşa edemeyen şifreleme işlemleri için kullanılabilen, özel çip devrelerinde anahtarların depolanmasına olanak tanıyan güvenilir bir platform modülü (TPM) yeteneğidir. Aygıtın bellek şifrelemesi. Aygıt için anahtar yönetimi. Kodu imzalıyor. |
|| E |Cihazın erişim denetimine sahip. Yetkilendirme şeması. |Cihaz, dışarıdan gelen komutlara ve hatta tehlikeye alınan sensörlere dayalı olarak tek tek eylemlerin gerçekleştirilmesine izin veriyorsa, saldırının başka türlü erişilemeyen işlemleri gerçekleştirmesine olanak tanır. |Aygıt için yetkilendirme şemasına sahip olmak |
| Alan Ağ Geçidi |S |Alan ağ geçidinin Cloud Ağ Geçidi'ne doğrulanması (cert tabanlı, PSK veya Talep tabanlı gibi.) |Birisi Field Gateway'i taklit edebilirse, o zaman kendisini herhangi bir aygıt olarak sunabilir. |TLS RSA/PSK, IPSec, [RFC 4279](https://tools.ietf.org/html/rfc4279). Genel olarak cihazların aynı anahtar depolama ve attestation endişeleri - en iyi durumda TPM kullanmaktır. Kablosuz Sensör Ağlarını (WSN) desteklemek için IPSec için 6LowPAN uzantısı. |
|| TRID |Alan Ağ Geçidini kurcalamaya karşı koruyun (TPM?) |Alan ağ geçidiyle konuştuğunu düşünerek bulut ağ geçidini kandıran sahte saldırılar bilgi ifşasına ve veri kurcalamasına neden olabilir |Bellek şifreleme, TPM' ler, kimlik doğrulama. |
|| E |Alan Ağ Geçidi için erişim denetim mekanizması | | |

Bu kategorideki tehditlere örnek olarak aşağıda verilmiştir:

**Sızdırma**: Saldırgan, yazılım veya donanım düzeyinde bir cihazdan şifreleme anahtar materyali ayıklayabilir ve daha sonra anahtar malzemenin alındığı cihazın kimliği altında farklı bir fiziksel veya sanal aygıtla sisteme erişebilir.

**Hizmet Reddi**: Cihaz, radyo frekanslarına müdahale ederek veya telleri keserek işleyemekten veya iletişim kuramadan kullanılabilir. Örneğin, gücü veya ağ bağlantısı kasıtlı olarak devre dışı olan bir güvenlik kamerası verileri hiç bildiremez.

**Kurcalama**: Bir saldırgan, cihazda çalışan yazılımıkısmen veya tamamen değiştirebilir ve anahtar malzeme nin veya anahtar materyalleri tutan şifreleme tesislerinin yasadışı programa uygun olması durumunda değiştirilen yazılımın cihazın orijinal kimliğinden yararlanabilmesine izin verebilir.

**Kurcalama**: Boş bir koridorun görünür spektrumlu resmini gösteren bir güvenlik kamerası böyle bir koridorun fotoğrafına yönelik olabilir. Bir duman ya da yangın sensörü altında çakmak tutan birini bildiriyor olabilir. Her iki durumda da, aygıt teknik olarak sisteme karşı tamamen güvenilir olabilir, ancak manipüle edilen bilgileri bildirir.

**Tahrifat**: Saldırgan, iletişim yolundaki aygıttan alınan verileri engellemek ve bastırmak için çıkarılan anahtar malzemeden yararlanabilir ve bunu çalınan anahtar malzemeyle doğrulanmış yanlış verilerle değiştirebilir.

**Tahrifat**: Bir saldırgan, cihazda çalışan yazılımıkısmen veya tamamen değiştirebilir ve anahtar malzeme nin veya anahtar materyalleri tutan şifreleme tesislerinin yasadışı programa uygun olması durumunda değiştirilen yazılımın cihazın orijinal kimliğinden yararlanabilmesine olanak verebilir.

**Bilgi Açıklaması**: Cihaz manipüle edilmiş yazılım çalıştırıyorsa, bu tür manipüle edilmiş yazılımlar yetkisiz taraflara veri sızdırabilir.

**Bilgi Açıklaması**: Saldırgan, bilgi almak için cihaz la bir denetleyici veya alan ağ geçidi veya bulut ağ geçidi arasındaki iletişim yoluna kendini enjekte etmek için çıkarılan anahtar malzemeden yararlanabilir.

**Hizmet Reddi**: Cihaz kapatılabilir veya iletişimin mümkün olmadığı bir moda dönüştürülebilir (birçok endüstriyel makinede kasıtlı olarak).

**Kurcalama**: Cihaz, kontrol sistemi bilinmeyen bir durumda (bilinen kalibrasyon parametreleri dışında) çalışacak şekilde yeniden yapılandırılabilir ve böylece yanlış yorumlanabilecek veriler sağlayabilir

**Ayrıcalık Yüksekliği**: Belirli bir işlevi yapan bir aygıt başka bir şey yapmaya zorlanabilir. Örneğin, yarım açmak için programlanmış bir vana tüm yol açmak için kandırdın olabilir.

**Hizmet Reddi**: Cihaz, iletişimin mümkün olmadığı bir duruma dönüştürülebilir.

**Kurcalama**: Cihaz, kontrol sistemi bilinmeyen bir durumda (bilinen kalibrasyon parametreleri dışında) çalışacak şekilde yeniden yapılandırılabilir ve böylece yanlış yorumlanabilecek veriler sağlayabilir.

**Sızdırma/Tahrif/Reddetme**: Güvenli değilse (ki bu tüketici uzaktan kumandalarında nadiren geçerli dir), saldırgan aygıtın durumunu anonim olarak manipüle edebilir. İyi bir illüstrasyon herhangi bir TV açabilirsiniz ve popüler şakacı araçları olan uzaktan kumandalar olduğunu.

#### <a name="communication"></a>İletişim

Aygıtlar, aygıtlar ve alan ağ geçitleri ile aygıt ve bulut ağ geçidi arasındaki iletişim yolu etrafındaki tehditler. Aşağıdaki tabloda aygıt/VPN'deki açık soketlerin etrafında bazı kılavuzlar vardır:

| **Bileşen** | **Tehdit** | **Risk azaltma** | **Risk** | **Uygulama** |
| --- | --- | --- | --- | --- |
| Cihaz IoT Hub |Tid |(D) TLS (PSK/RSA) trafiği şifrelemek için |Cihaz ve ağ geçidi arasındaki iletişimi dinleme veya müdahale etme |Protokol seviyesinde güvenlik. Özel protokollerle, onları nasıl koruyacağınızı bulmanız gerekir. Çoğu durumda, iletişim aygıttan IoT Hub'ına gerçekleşir (aygıt bağlantıyı başlatır). |
| Cihazdan Cihaza |Tid |(D) TLS (PSK/RSA) trafiği şifrelemek için. |Aygıtlar arasında aktarım daki verileri okuma. Verilerle oynamak. Cihazın yeni bağlantılarla aşırı yüklenmesi |Protokol düzeyinde güvenlik (MQTT/AMQP/HTTP/CoAP. Özel protokollerle, onları nasıl koruyacağınızı bulmanız gerekir. DoS tehdidinin azaltıcı etkisi, aygıtları bir bulut veya alan ağ geçidi üzerinden eşlemek ve yalnızca ağa karşı istemci olarak davranmalarını sağlamaktır. Bu bakış, ağ geçidi tarafından aracılık edildikten sonra eşler arasında doğrudan bir bağlantıya neden olabilir |
| Harici Varlık Aygıtı |Tid |Dış varlığın aygıtla güçlü eşleşmesi |Cihaza olan bağlantıyı dinliyor. Aygıtla iletişimi engelleme |Dış varlığı nfc/Bluetooth LE aygıtıyla güvenli bir şekilde eşleştirme. Cihazın çalışma panelini kontrol etme (Fiziksel) |
| Alan Ağ Geçidi Bulut Ağ Geçidi |Tid |TLS (PSK/RSA) trafiği şifrelemek için. |Cihaz ve ağ geçidi arasındaki iletişimi dinleme veya müdahale etme |Protokol düzeyinde güvenlik (MQTT/AMQP/HTTP/CoAP). Özel protokollerle, onları nasıl koruyacağınızı bulmanız gerekir. |
| Aygıt Bulut Ağ Geçidi |Tid |TLS (PSK/RSA) trafiği şifrelemek için. |Cihaz ve ağ geçidi arasındaki iletişimi dinleme veya müdahale etme |Protokol düzeyinde güvenlik (MQTT/AMQP/HTTP/CoAP). Özel protokollerle, onları nasıl koruyacağınızı bulmanız gerekir. |

Bu kategorideki tehditlere örnek olarak aşağıda verilmiştir:

**Hizmet Reddi**: Bir saldırgan paralel olarak birçok bağlantıyı açıp onlara hizmet vermemek veya yavaş yavaş hizmet vermemek veya cihaz istenmeyen trafikle dolup taşabildiği için, kısıtlanmış aygıtlar genellikle bir ağdaki gelen bağlantıları veya istenmeyen datagramları aktif olarak dinlediğinde DoS tehdidi altındadır. Her iki durumda da, aygıt ağda etkili bir şekilde çalışamaz hale getirilebilir.

**Sızdırma, Bilgi Açıklama**: Kısıtlı aygıtlar ve özel amaçlı aygıtlar genellikle parola veya PIN koruması gibi her şey için bir güvenlik olanaklarına sahiptir veya tamamen ağa güvenmeye güvenirler, bu da bir aygıt aynı ağdayken bilgilere erişim izni verdikleri anlamına gelir ve bu ağ genellikle yalnızca paylaşılan bir anahtar la korunur. Bu, aygıt veya ağa paylaşılan gizli açıklandığında, aygıtı denetlemek veya aygıttan yayılan verileri gözlemlemek mümkün olduğu anlamına gelir.  

**Sızdırma**: Saldırgan yayını yakalayabilir veya kısmen geçersiz kılabilir ve kaynağı (ortadaki adam)

**Tahrifat**: Saldırgan yayını yakalayabilir veya kısmen geçersiz kılabilir ve yanlış bilgi gönderebilir 

**Bilgi Açıklaması:** Saldırgan bir yayını dinleyebilir ve yetkilendirme olmadan bilgi alabilir **Hizmet Reddi:** saldırgan yayın sinyalini bozabilir ve bilgi dağıtımını reddedebilir

#### <a name="storage"></a>Depolama

Her aygıt ve alan ağ geçidinde bir tür depolama alanı vardır (verileri, işletim sistemini (OS) görüntü depolamayı sıraya dökmek için geçicidir).

| **Bileşen** | **Tehdit** | **Risk azaltma** | **Risk** | **Uygulama** |
| --- | --- | --- | --- | --- |
| Cihaz depolama |TRID |Depolama şifreleme, günlükleri imzalama |Depolama (PII verileri) verileri okuma, telemetri verileri ile kurcalama. Sıralı veya önbelleğe alınmış komut denetimi verileriyle oynama. Önbelleğe alınmış veya yerel olarak sıraya alınırken yapılandırma veya firmware güncelleştirme paketleriyle oynama, işletim sistemi ve/veya sistem bileşenlerinin tehlikeye atılmasına neden olabilir |Şifreleme, ileti kimlik doğrulama kodu (MAC) veya dijital imza. Mümkün olduğunda, kaynak erişim denetim listeleri (ALA'lar) veya izinler aracılığıyla güçlü erişim denetimi. |
| Aygıt işletim sistemi görüntüsü |TRID | |Os ile kurcalama /Işletim Sistemi bileşenlerinin değiştirilmesi |Salt okunur işletim sistemi bölümü, imzalı işletim sistemi görüntüsü, Şifreleme |
| Alan Ağ Geçidi depolama (verileri sıraya alma) |TRID |Depolama şifreleme, günlükleri imzalama |Depolama (PII verileri) verilerini okuma, telemetri verileriyle oynama, sıraya alınmış veya önbelleğe alınmış komut denetimi verilerini kurcalama. Önbelleğe alınmış veya yerel olarak sıraya alınırken yapılandırma veya firmware güncelleştirme paketleriyle (aygıtlara veya alan ağ geçidine yönelik) kurcalanma, işletim sistemi ve/veya sistem bileşenlerinin tehlikeye atılmasına neden olabilir |BitLocker |
| Alan Ağ Geçidi İşletim görüntüsü |TRID | |Os ile kurcalama /Işletim Sistemi bileşenlerinin değiştirilmesi |Salt okunur işletim sistemi bölümü, imzalı işletim sistemi görüntüsü, Şifreleme |

### <a name="device-and-event-processingcloud-gateway-zone"></a>Aygıt ve olay işleme/bulut ağ geçidi bölgesi

Bulut ağ geçidi, genellikle bulut tabanlı bir denetim ve veri analiz sistemi olan bu tür sistemlerin federasyonu olan bulut tabanlı denetim ve veri analiz sistemine yönelik olarak, genel ağ alanı ndaki birkaç farklı siteden aygıtlardan veya alan ağ geçitlerinden uzaktan iletişim sağlayan bir sistemdir. Bazı durumlarda, bir bulut ağ geçidi tabletler veya telefonlar gibi terminallerden özel amaçlı aygıtlara erişimi hemen kolaylaştırabilir. Burada tartışılan bağlamda ,"bulut", ekli aygıtlarla veya alan ağ geçitleri ile aynı siteye bağlı olmayan ve operasyonel önlemlerin hedeflenen fiziksel erişimi engellediği, ancak "bir " anlamına gelmemesi gereken özel bir veri işleme sistemine atıfta bulunmak içindir. genel bulut" altyapısı. Bulut ağ geçidi, bulut ağ geçidini ve bağlı tüm aygıtlarını veya alan ağ geçitlerini başka bir ağ trafiğinden yalıtmak için ağ sanallaştırma bindirmesine eşlenebilir. Bulut ağ geçidinin kendisi bir aygıt kontrol sistemi veya aygıt verileri için bir işleme veya depolama tesisi değildir; bu tesisler bulut ağ geçidi ile arayüz. Bulut ağ geçidi bölgesi, bulut ağ geçidinin kendisini, doğrudan veya dolaylı olarak bağlı tüm alan ağ geçitleri ve aygıtlarıyla birlikte içerir.

Bulut ağ geçidi, çoğunlukla alan ağ geçidinin ve aygıtların bağlandığı açık uç noktaları olan bir hizmet olarak çalışan özel olarak oluşturulmuş yazılım parçasıdır. Bu nedenle güvenlik göz önünde bulundurularak tasarlanmalıdır. Bu hizmeti tasarlamak ve oluşturmak için [SDL](https://www.microsoft.com/sdl) işlemini izleyin.

#### <a name="services-zone"></a>Hizmet bölgesi

Kontrol sistemi (veya denetleyici), bir veya birden fazla cihazı kontrol etmek ve/veya sunum veya sonraki kontrol amaçları için cihaz verilerini toplamak ve/veya depolamak ve/veya analiz etmek amacıyla bir aygıtla veya bir alan ağ geçidiyle veya bulut ağ geçidiyle arayüz oluşturan bir yazılım çözümüdür. Kontrol sistemleri, bu tartışma kapsamındaki kişilerle etkileşimi hemen kolaylaştırabilecek tek varlıklardır. İstisnalar, bir kişinin cihazı kapatmasına veya diğer özellikleri değiştirmesine olanak tanıyan ve dijital olarak erişilebilen işlevsel bir eşdeğeri bulunmayan bir anahtar gibi aygıtlardaki ara fiziksel kontrol yüzeyleridir.

Ara fiziksel kontrol yüzeyleri, yönetim mantığının fiziksel kontrol yüzeyinin işlevini kısıtladığı, eşdeğer bir işlevin uzaktan başlatılabildiği veya uzaktan girişle giriş çakışmaları önlenebileceği - bu tür ara kontrol yüzeyleri, aygıtın paralel olarak bağlanabileceği diğer uzaktan kumanda sistemleriyle aynı temel işlevselliği sağlayan yerel bir denetim sistemine kavramsal olarak bağlanır. Bulut bilgi işlem için en büyük tehditler [Bulut Güvenliği Birliği (CSA)](https://cloudsecurityalliance.org/articles/csa-releases-top-threats-to-cloud-computing-deep-dive/) sayfasından okunabilir.

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [SDL Tehdit Modelleme Aracı](https://www.microsoft.com/sdl/adopt/threatmodeling.aspx)
* [Microsoft Azure IoT başvuru mimarisi](https://azure.microsoft.com/updates/microsoft-azure-iot-reference-architecture-available/)

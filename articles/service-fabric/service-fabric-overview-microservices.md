---
title: Azure'da mikro hizmetlere giriş
description: Mikro hizmetler yaklaşımıyla bulut uygulamaları oluşturmanın modern uygulama geliştirme için neden önemli olduğuna ve Azure Service Fabric'in bunu başarmak için nasıl bir platform sağladığına genel bir bakış.
ms.topic: conceptual
ms.date: 01/07/2020
ms.custom: sfrev
ms.openlocfilehash: af18a6cb45808c0af5ec2782a3fd2100e3b7bf99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75750623"
---
# <a name="why-use-a-microservices-approach-to-building-applications"></a>Bina uygulamalarında neden mikrohizmetler yaklaşımı kullanılır?

Yazılım geliştiricileri için, bir uygulamayı bileşen parçalarına faktoring yeni bir şey değildir. Genellikle, bir arka uç deposu, orta katman iş mantığı ve bir ön uç kullanıcı arabirimi (UI) ile katmanlı bir yaklaşım kullanılır. Son *has* birkaç yılda değişen şey, geliştiricilerin bulut için dağıtılmış uygulamalar oluşturmalarıdır.

Değişen bazı iş gereksinimleri şunlardır:

* Yeni coğrafi bölgelerdeki müşterilere ulaşmak için ölçekte oluşturulmuş ve işletilen bir hizmet.
* Müşteri taleplerine çevik bir şekilde yanıt vermek için özelliklerin ve yeteneklerin daha hızlı teslimi.
* Maliyetleri azaltmak için geliştirilmiş kaynak kullanımı.

Bu iş ihtiyaçları, uygulamaları *oluşturma şeklimizi* etkiliyor.

Azure'un mikro hizmetlere yaklaşımı hakkında daha fazla bilgi için [Bkz. Mikro Hizmetler: Buluttarafından desteklenen bir uygulama devrimi.](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/)

## <a name="monolithic-vs-microservices-design-approach"></a>Monolitik vs microservices tasarım yaklaşımı

Uygulamalar zamanla gelişir. Başarılı uygulamalar insanlara yararlı olarak gelişir. Başarısız uygulamalar gelişmez ve sonunda amortismana alınır. İşte soru: ne kadar gereksinimleri bugün ve gelecekte ne olacak biliyor musunuz? Örneğin, şirketinizdeki bir departman için raporlama uygulaması oluşturuyorsunuz. Uygulamanın yalnızca şirketiniz kapsamında geçerli olduğundan ve raporların uzun süre tutulmayeceğinden eminsiniz. Yaklaşımınız, örneğin, on milyonlarca müşteriye video içeriği sunan bir hizmet oluşturmaktan farklı olacaktır.

Bazen, kavramın bir kanıtı olarak kapıdan bir şey almak itici faktördür. Uygulamanın daha sonra yeniden tasarlanabileceğini biliyorsunuz. Asla kullanılmamış bir şeyi aşırı mühendislikle halletmek için çok az şey vardır. Öte yandan, şirketler bulut için inşa ederken, beklenti büyüme ve kullanımdır. Büyüme ve ölçek öngörülemez. Gelecekteki başarılarla başa çıkabilecek bir yolda olduğumuzu bilerek hızlı bir prototip yapmak istiyoruz. Bu yalın başlangıç yaklaşımıdır: oluşturmak, ölçmek, öğrenmek ve yineleyin.

İstemci/sunucu döneminde, her katmanda belirli teknolojileri kullanarak katmanlı uygulamalar oluşturmaya odaklanma eğilimindeydik. Bu yaklaşımları tanımlamak için *monolitik* uygulama terimi ortaya çıkmıştır. Arabirimler katmanlar arasında olma eğilimindedir ve her katmandaki bileşenler arasında daha sıkı bir şekilde birleştirilmiş tasarım kullanılmıştır. Geliştiriciler, kitaplıklara derlenen ve birkaç yürütülebilir dosya ve DL'ye bağlanan sınıfları tasarlar ve etkene tabi attırılır.

Yekpare tasarım yaklaşımının faydaları vardır. Monolitik uygulamaların tasarlanması genellikle daha kolaydır ve bileşenler arasındaki çağrılar genellikle süreçler arası iletişim (IPC) üzerinden olduğundan daha hızlıdır. Ayrıca, herkes insan kaynaklarının daha verimli kullanımı olma eğilimindedir tek bir ürün, test eder. Dezavantajı katmanlı katmanlar arasında sıkı bir bağlantı olmasıdır ve tek tek bileşenleri ölçeklendiremezsiniz. Düzeltmeler veya yükseltmeler yapmanız gerekiyorsa, başkalarının testlerini bitirmesini beklemeniz gerekir. Çevik olmak daha zordur.

Mikro hizmetler bu olumsuz yanları giderir ve önceki iş gereksinimleriyle daha yakından uyumlu hale gelir. Ama aynı zamanda hem yararları hem de yükümlülükleri vardır. Mikro hizmetlerin yararları, her birinin genellikle daha basit iş işlevlerini kapsüllemenizdir, bu da bağımsız olarak ölçeklendirebilir veya küçültebilir, test edebilir, dağıtabilir ve yönetebilirsiniz. Mikro hizmet yaklaşımının önemli bir avantajı, ekiplerin teknolojiden çok iş senaryoları tarafından yönlendirilmiş olmasıdır. Küçük ekipler, müşteri senaryosuna dayalı bir mikro hizmet geliştirir ve kullanmak istedikleri teknolojileri kullanır.

Başka bir deyişle, kuruluşun mikro hizmet uygulamalarını korumak için teknolojiyi standartlaştırmasına gerek yoktur. Hizmetlere sahip olan bireysel ekipler, takım uzmanlığına veya sorunu çözmek için en uygun olanı temel alınarak kendileri için mantıklı olan her şeyi yapabilir. Uygulamada, belirli bir NoSQL deposu veya web uygulama çerçevesi gibi önerilen bir dizi teknoloji tercih edilir.

Mikro hizmetlerin dezavantajı, daha fazla ayrı varlıkları yönetmek ve daha karmaşık dağıtımlar ve sürümlerle uğraşmak zorunda kalmanızdır. Mikro hizmetler arasındaki ağ trafiği artar, ilgili ağ gecikmeleri gibi. Çok geveze, parçalı hizmetler bir performans kabusuna neden olabilir. Bu bağımlılıkları görüntülemenize yardımcı olacak araçlar olmadan, tüm sistemi görmek zordur.

Standartlar, katı sözleşmeler yerine yalnızca bir hizmetten ihtiyacınız olan şeyleri nasıl ileteceklerini belirterek ve bunlara tolerans vererek mikro hizmetler yaklaşımının çalışmasını sağlamdır. Hizmetler birbirinden bağımsız olarak güncelleştirilebildiği için bu sözleşmeleri tasarımda önceden tanımlamak önemlidir. Bir mikrohizmet yaklaşımı ile tasarım için icat başka bir açıklama "ince taneli hizmet odaklı mimari (SOA)."

***En basit haliyle, mikro hizmetler tasarım yaklaşımı, her biri ve üzerinde mutabık kalınan iletişim standartlarında bağımsız değişikliklerle ayrılmış bir hizmet federasyonu hakkındadır.***

Daha fazla bulut uygulaması üretildikçe, insanlar genel uygulamanın bağımsız, senaryo odaklı hizmetlere bu ayrıştırının daha iyi bir uzun vadeli yaklaşım olduğunu keşfettiler.

## <a name="comparison-between-application-development-approaches"></a>Uygulama geliştirme yaklaşımları arasında karşılaştırma

![Hizmet Kumaş platformu uygulama geliştirme][Image1]

1) Monolitik bir uygulama etki alanına özgü işlevsellik içerir ve normalde web, iş ve veri gibi işlevsel katmanlara ayrılır.

2) Yekpare bir uygulamayı birden çok sunucuda/sanal makinede/kapsayıcıda klonlayarak ölçeklendirin.

3) Microservice uygulaması işlevselliği ayrı küçük hizmetlere ayırır.

4) Mikro hizmetler yaklaşımı, her hizmeti bağımsız olarak dağıtarak, sunucular/sanal makineler/kapsayıcılar arasında bu hizmetlerin örneklerini oluşturarak ölçeklendirin.

Mikro hizmetler yaklaşımıyla tasarlamak tüm projeler için uygun değildir, ancak daha önce açıklanan iş hedefleriyle daha yakın bir uyum sağlar. Monolitik bir yaklaşımla başlamak, kodu daha sonra bir mikrohizmet tasarımında yeniden çalışma fırsatınız olacakbiliyorsanız mantıklı olabilir. Daha yaygın olarak, yekpare bir uygulama ile başlar ve yavaş yavaş aşamalı olarak, daha ölçeklenebilir veya çevik olması gereken işlevsel alanlardan başlayarak onu kırmak.

Bir mikro hizmet yaklaşımı kullandığınızda, birçok küçük hizmet uygulama oluşturmak. Bu hizmetler, bir makine kümesi arasında dağıtılan kapsayıcılarda çalışır. Küçük takımlar bir senaryoya odaklanan bir hizmet geliştirir ve tüm uygulamanın gelişabilmesi için her hizmeti bağımsız olarak sına, sürüm, dağıtma ve ölçeklendirir.

## <a name="what-is-a-microservice"></a>Mikro hizmet nedir?

Mikro hizmetlerin farklı tanımları vardır. Ancak mikro hizmetlerin bu özelliklerinin çoğu yaygın olarak kabul edilmektedir:

* Bir müşteri veya iş senaryosuna kapsülle. Hangi problemi çözüyorsun?
* Küçük bir mühendislik ekibi tarafından geliştirilmiştir.
* Herhangi bir programlama dilinde, herhangi bir çerçeve kullanarak yazılmıştır.
* Her ikisi de bağımsız olarak sürülmüş, dağıtılmış ve ölçeklenmiş olan koddan oluşur ve isteğe bağlı olarak durum.
* İyi tanımlanmış arabirimler ve protokoller üzerinden diğer mikro hizmetlerle etkileşim kurun.
* Konumlarını çözmek için kullanılan benzersiz adlara (URL'lere) sahip olun.
* Tutarlı ve hataların varlığında kullanılabilir kalır.

Özetlemek gerekirse:

***Microservice uygulamaları, iyi tanımlanmış arabirimlere sahip standart protokoller üzerinden birbirleriyle iletişim kurabilen küçük, bağımsız olarak sürümlenmiş ve ölçeklenebilir müşteri odaklı hizmetlerden oluşur.***

### <a name="written-in-any-programming-language-using-any-framework"></a>Herhangi bir programlama dilinde yazılmış, herhangi bir çerçeve kullanarak

Geliştiriciler olarak, becerilerimize ve oluşturduğumuz hizmetin ihtiyaçlarına bağlı olarak bir dil veya çerçeve seçmekte özgür olmak istiyoruz. Bazı hizmetlerde, C++'ın performans avantajlarına her şeyden daha fazla değer biçebilirsiniz. Diğerleri için, C# veya Java'dan aldığınız yönetilen geliştirme kolaylığı daha önemli olabilir. Bazı durumlarda, hizmeti istemcilere teşhir etmek için belirli bir iş ortağı kitaplığı, veri depolama teknolojisi veya yöntem kullanmanız gerekebilir.

Bir teknoloji seçtikten sonra, hizmetin operasyonel veya yaşam döngüsü yönetimini ve ölçeklemeyi göz önünde bulundurmanız gerekir.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Kodun ve devletin bağımsız olarak sürülmesine, dağıtılmasına ve ölçeklendirilmesine izin verir

Mikro hizmetlerinizi nasıl yazarsanız yazın, kod ve isteğe bağlı olarak durum, bağımsız olarak dağıtmalı, yükseltmeli ve ölçeklendirmelidir. Bu sorunu çözmek zordur, çünkü bu sizin seçtiğiniz teknolojilere gelir. Ölçekleme için, kodun ve devletin nasıl bölümlenebildiğini (veya parçalamanın) nasıl yapılacağını anlamak zordur. Kod ve durum bugün yaygın olan farklı teknolojileri kullandığında, mikro hizmetinizin dağıtım komut dosyalarının her ikisini de ölçeklendirebilmeniz gerekir. Bu ayrım aynı zamanda çeviklik ve esneklikle de ilgilidir, böylece mikro hizmetlerden bazılarını hepsini aynı anda yükseltmeye gerek kalmadan yükseltebilirsiniz.

Bir an için yekpare ve mikro hizmet yaklaşımlarını karşılaştırmamıza geri dönelim. Bu diyagram, durum depolama yaklaşımları farklılıklarını gösterir:

#### <a name="state-storage-for-the-two-approaches"></a>İki yaklaşım için devlet depolama

![Service Fabric platformu devlet depolama][Image2]

***Soldaki yekpare yaklaşım, tek bir veritabanına ve belirli teknolojilerin katmanlarına sahiptir.***

***Microservices yaklaşımı, sağda, devlet genellikle microservice ve çeşitli teknolojiler için kapsamlı birbirbirine mikro hizmetler bir grafik vardır.***

Yekpare bir yaklaşımda, uygulama genellikle tek bir veritabanı kullanır. Tek bir veritabanı kullanmanın avantajı, tek bir konumda olmasıdır, bu da dağıtımı kolaylaştırır. Her bileşenin durumunu depolamak için tek bir tablo olabilir. Takımların eyaleti kesinlikle ayırması gerekir, ki bu bir meydan okumadır. Kaçınılmaz olarak, birisi varolan bir müşteri tablosuna bir sütun eklemek, tablolar arasında birleşim yapmak ve depolama katmanında bağımlılıklar oluşturmak için cazip olacaktır. Bu durumdan sonra, bileşenleri tek tek ölçeklendiremezsiniz.

Mikro hizmetler yaklaşımında, her hizmet kendi durumunu yönetir ve saklar. Her hizmet, hizmetin taleplerini karşılamak için hem kodu hem de durumu birlikte ölçeklemekten sorumludur. Dezavantajı, uygulamanızın verilerinin görünümlerini veya sorgularını oluşturmanız gerektiğinde, birden çok durum deposunda sorgu yapmanız gerektiğidir. Bu sorun genellikle mikro hizmetler koleksiyonu nda bir görünüm oluşturan ayrı bir microservice tarafından çözülür. Veriler üzerinde birden çok hazırlıksız sorgu çalıştırmanız gerekiyorsa, her mikro hizmetin verilerini çevrimdışı analiz için bir veri depolama hizmetine yazmayı düşünmelisiniz.

Microservices sürümü vardır. Bir microservice farklı sürümleri için yan yana çalıştırmak mümkündür. Bir microservice'in daha yeni bir sürümü yükseltme sırasında başarısız olabilir ve daha önceki bir sürüme geri alınması gerekir. Sürüm, farklı kullanıcıların hizmetin farklı sürümlerini deneyimlemesi için De/B testi için de yararlıdır. Örneğin, bir mikro hizmeti daha yaygın olarak kullanıma sunmadan önce yeni işlevleri sınamak için belirli bir müşteri kümesi için yükseltmek yaygındır.

### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>İyi tanımlanmış arabirimler ve protokoller üzerinden diğer mikro hizmetlerle etkileşime girer

Son 10 yılda, hizmet odaklı mimarilerde iletişim kalıplarını açıklayan kapsamlı bilgiler yayınlanmıştır. Genellikle, hizmet iletişimi http ve TCP protokolleri ve XML veya JSON ile bir REST yaklaşımı kullanır serileştirme biçimi olarak. Arayüz açısından bakıldığında, bu bir web tasarım yaklaşımı alarak ilgili. Ancak hiçbir şey sizi ikili protokolleri veya kendi veri biçimlerinizi kullanmaktan alıkoyamaz. Bu protokoller ve biçimler açıkça kullanılamıyorsa, insanların mikro hizmetlerinizi kullanmakta daha zor zamanlar olacağını unutmayın.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Konumunu çözmek için kullanılan benzersiz bir ad (URL) vardır

Mikro hizmetinizin nerede çalışırsa çalıştırılsın adreslenebilir olması gerekir. Makineleri düşünüyorsanız ve hangisinin belirli bir microservice çalıştırıyor, işler hızla kötü gidebilir.

DNS'nin belirli bir URL'yi belirli bir makineye çözmesi gibi, microservice'inizin de geçerli konumunun bulunabilmesi için benzersiz bir ada ihtiyacı vardır. Mikro hizmetlerin, üzerinde çalıştıkları altyapıdan bağımsız olarak adreslenebilir adlar alması gerekir. Bu, hizmet kayıt defteri olması gerektiğinden, hizmetinizin nasıl dağıtıldığı ile nasıl keşfedildiği arasında bir etkileşim olduğu anlamına gelir. Bir makine başarısız olduğunda, kayıt defteri hizmetinin hizmetin nereye taşındığını size söylemesi gerekir.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Tutarlı kalır ve hataların varlığında kullanılabilir

Beklenmeyen hatalarla başa çıkmak, özellikle dağıtılmış bir sistemde çözülmesi en zor sorunlardan biridir. Geliştiriciler olarak yazdığımız kodun çoğu özel durumları işlemek içindir. Test sırasında, biz de istisna işleme en fazla zaman harcamak. İşlem, hataları işlemek için kod yazmaktan daha fazla ilgilidir. Mikro hizmetin çalıştırıldığı makine arızalandığında ne olur? Kendi başına zor bir sorun olan başarısızlığı algılamanız gerekir. Ama aynı zamanda microservice yeniden başlatmanız gerekir.

Kullanılabilirlik için, bir mikro hizmetin hatalara karşı dayanıklı olması ve başka bir makinede yeniden başlatılabilmesi gerekir. Bu esneklik gereksinimlerine ek olarak, veriler kaybolmamalı ve verilerin tutarlı kalması gerekir.

Bir uygulama yükseltmesi sırasında hatalar olduğunda esneklik elde etmek zordur. Dağıtım sistemiyle çalışan mikro hizmetin kurtarılmasıgerekmez. Tutarlı bir durumu korumak için yeni sürüme ilerlemeye devam edip edemeyeceğini veya önceki sürüme geri dönüp dönemeyeceğini belirlemesi gerekir. İlerlemeye devam etmek için yeterli makine nin kullanılabilir olup olmadığı ve mikro hizmetin önceki sürümlerini nasıl kurtarabileceğiniz gibi birkaç soruyu göz önünde bulundurmanız gerekir. Bu kararları almak için, sağlık bilgileri yontmak için mikro hizmet gerekir.

### <a name="reports-health-and-diagnostics"></a>Sağlık ve tanılama raporlarını raporlar

Bu açık görünebilir, ve genellikle göz ardı edilir, ama bir microservice sağlık ve tanı bildirmek gerekiyor. Aksi takdirde, bir operasyon açısından sağlık içine çok az fikir var. Bir dizi bağımsız hizmet arasında tanılama olaylarını ilişkilendirmek ve olay düzenini anlamak için makine saati eğriliğiyle uğraşmak zordur. Üzerinde anlaşmaya varılan protokoller ve veri biçimleri üzerinde bir mikro hizmetle etkileşimkurduğunuz gibi, sistem durumu ve tanılama olaylarını nasıl günlüğe kaydedeceğiniz ve sonunda sorgulama ve görüntüleme için bir olay deposunda nasıl sonuçlanacaklarını standartlaştırmanız gerekir. Mikro hizmetler yaklaşımıyla, farklı ekiplerin tek bir günlük biçimi üzerinde anlaştığı gerekir. Uygulamadaki tanıolaylarını bir bütün olarak görüntülemek için tutarlı bir yaklaşım olması gerekir.

Sağlık tanıdan farklıdır. Sağlık, uygun eylemleri yapmak için geçerli durumunu bildiren mikro hizmetle ilgilidir. İyi bir örnek, kullanılabilirliği korumak için yükseltme ve dağıtım mekanizmaları ile çalışmaktır. Bir işlem çökmesi veya makinenin yeniden başlatılması nedeniyle bir hizmet şu anda sağlıksız olsa da, hizmet yine de çalışır durumda olabilir. İhtiyacınız olan son şey bir yükseltme başlatarak durumu daha kötü hale getirmektir. En iyi yaklaşım ilk araştırmak veya mikrohizmet kurtarmak için zaman izin vermektir. Bir mikro hizmetten gelen sağlık olayları bilinçli kararlar vermemize ve aslında kendi kendini iyileştirme hizmetleri oluşturmamıza yardımcı olur.

## <a name="guidance-for-designing-microservices-on-azure"></a>Azure'da mikro hizmetler tasarlama kılavuzu

Azure'da mikro hizmetler tasarlama ve oluşturma konusunda rehberlik için Azure mimari merkezini ziyaret [edin.](https://docs.microsoft.com/azure/architecture/microservices/)

## <a name="service-fabric-as-a-microservices-platform"></a>Microservices platformu olarak Servis Kumaşı

Azure Hizmet Kumaşı, Microsoft'un genellikle yekpare olan kutulu ürünler sunmaktan hizmet sunmaya geçişiyle ortaya çıkmıştır. Azure SQL Veritabanı ve Azure Cosmos DB gibi büyük hizmetler oluşturma ve çalıştırma deneyimi Hizmet Kumaşını şekillendirdi. Platform zaman içinde daha fazla hizmet kabul olarak gelişti. Service Fabric yalnızca Azure'da değil, bağımsız Windows Server dağıtımlarında da çalışmak zorundaydı.

***Service Fabric'in amacı, hizmet oluşturma nın ve yürütmenin zor sorunlarını çözmek ve altyapı kaynaklarını verimli bir şekilde kullanmaktır, böylece ekipler mikrohizmet yaklaşımını kullanarak iş sorunlarını çözebilirler.***

Service Fabric, mikrohizmet yaklaşımını kullanan uygulamaları şunları sağlayarak oluşturmanıza yardımcı olur:

* Başarısız hizmetleri dağıtmak, yükseltmek, algılamak ve yeniden başlatmak, hizmetleri keşfetmek, iletileri yönlendirmek, durumu yönetmek ve sistem durumunu izlemek için sistem hizmetleri sağlayan bir platform.
* Kapsayıcılarda veya işlem olarak çalışan uygulamaları dağıtma yeteneği. Service Fabric bir konteyner ve proses orkestratördür.
* Mikro hizmetler olarak uygulama oluşturmanıza yardımcı olacak üretken programlama API'leri: [ASP.NET Çekirdek, Güvenilir Aktörler ve Güvenilir Hizmetler.](service-fabric-choose-framework.md) Örneğin, sistem durumu ve tanılama bilgilerini alabilir veya yerleşik yüksek kullanılabilirlik avantajlarından yararlanabilirsiniz.

***Service Fabric, hizmetinizi nasıl oluşturduğunuz konusunda agnostiktir ve herhangi bir teknolojiyi kullanabilirsiniz. Ancak, mikro hizmetler oluşturmayı kolaylaştıran yerleşik programlama API'leri sağlar.***

### <a name="migrating-existing-applications-to-service-fabric"></a>Mevcut uygulamaları Servis Kumaşına geçirme

Service Fabric, mevcut kodu yeniden kullanmanıza ve yeni mikro hizmetlerle modernize etmenizi sağlar. Uygulama modernizasyonunun beş aşaması vardır ve herhangi bir aşamada başlayıp durabilirsiniz. Aşamaları şunlardır:

1) Geleneksel yekpare bir uygulama ile başlayın.  
2) Geçirme. Hizmet Kumaşı'nda varolan kodu barındırmak için kapsayıcıları veya konuk yürütülebilir leri kullanın.  
3) Modernleştirin. Varolan kapsayıcı kodun yanına yeni mikro hizmetler ekleyin.  
4) Yenilik. Monolitik uygulamayı ihtiyaca göre mikrohizmetlere ayırın.  
5) Uygulamaları mikro hizmetlere dönüştürün. Varolan yekpare uygulamaları dönüştürün veya yeni yeşil alan uygulamaları oluşturun.

![Mikro hizmetlere geçiş][Image3]

Unutmayın, *bu aşamalardan herhangi birinde başlayıp durabilirsiniz.* Bir sonraki aşamaya ilerlemek zorunda değilsin. 

Bu aşamaların her biri için örneklere bakalım.

**Geçirme**  
İki nedenden dolayı, birçok şirket mevcut yekpare uygulamaları kapsayıcılara geçirerek:

* Maliyet azaltma, ya konsolidasyon ve mevcut donanım kaldırılması nedeniyle ya da daha yüksek yoğunlukta çalışan uygulamalar nedeniyle.
* Geliştirme ve işlemler arasında tutarlı bir dağıtım sözleşmesi.

Maliyet indirimleri basittir. Microsoft'ta, mevcut birçok uygulama konteynerize ediliyor ve bu da milyonlarca dolar tasarruf sağlıyor. Tutarlı dağıtım değerlendirmek daha zordur, ancak eşit derecede önemlidir. Bu, geliştiricilerin kendilerine uygun teknolojileri seçebileceği, ancak işlemlerin uygulamaları dağıtmak ve yönetmek için yalnızca tek bir yöntemi kabul edeceği anlamına gelir. Geliştiricileri yalnızca belirli teknolojileri seçmeye zorlamadan farklı teknolojileri desteklemenin karmaşıklığıyla uğraşmak zorunda kalmaktan işlemleri hafifletir. Esasen, her uygulama kendi kendine yeten dağıtım görüntüleri içine kapsayıcı.

Birçok kuruluş burada durur. Zaten kapsayıcıların avantajlarına sahiptirler ve Service Fabric dağıtım, yükseltmeler, sürümler, geri almalar ve sistem durumu izleme dahil olmak üzere tam yönetim deneyimi sağlar.

**Modernleştir**  
Modernizasyon, mevcut kapsayıcı kodun yanı sıra yeni hizmetlerin de eklenmesidir. Yeni kod yazacaksanız, mikro hizmetler yolunda küçük adımlar atmak en iyisidir. Bu, yeni bir REST API bitiş noktası veya yeni iş mantığı eklemek anlamına gelebilir. Bu şekilde, yeni mikro hizmetler oluşturma ve bunları geliştirme ve dağıtma pratiği sürecini başlatMış olursunuz.

**Yenilik**  
Mikro hizmetler yaklaşımı değişen iş gereksinimlerini karşılar. Bu aşamada, yekpare uygulamayı hizmetlere bölmeye mi yoksa yenilik yapmaya mı başlayacağınız konusunda karar vermeniz gerekir. Burada klasik bir örnek, iş akışı sırası olarak kullandığınız bir veritabanının işlem darboğazına dönüştüğünde ortaya çıkmaktadır. İş akışı isteklerinin sayısı arttıkça, çalışmanın ölçek için dağıtılması gerekir. Uygulamanın ölçekleme yapmayan veya daha sık güncellenmesi gereken belirli bir parçayı alın ve mikro hizmet ve yenilik olarak bölün.

**Uygulamaları mikro hizmetlere dönüştürme**  
Bu aşamada, uygulamanız tamamen mikro hizmetlerden (veya ikiye bölünmüş) oluşur. Bu noktaya ulaşmak için, mikro hizmetler yolculuğunu yaptınız. Burada başlayabilirsiniz, ancak bunu bir mikrohizmetler platformu olmadan size önemli bir yatırım gerektirir yardımcı olmak için bunu yapmak için.

### <a name="are-microservices-right-for-my-application"></a>Mikro hizmetler başvurum için uygun mu?

Belki. Microsoft'ta, iş nedenleriyle bulut için daha fazla ekip oluşturmaya başladığında, birçoğu mikro hizmet benzeri bir yaklaşımın faydalarını fark etti. Bing, örneğin, yıllardır mikro hizmetleri kullanıyor. Diğer takımlar için mikro hizmetler yaklaşımı yeniydi. Ekipler, temel güç alanları nın dışında çözülmesi zor sorunlar olduğunu buldular. Bu nedenle Service Fabric, bina hizmetleri için teknoloji olarak ilgi kazanmıştır.

Service Fabric'in amacı, mikro hizmet uygulamaları oluşturmanın karmaşıklığını azaltmaktır, böylece bu kadar çok maliyetli yeniden tasarımdan geçmek zorunda kalmamanız gerekir. Küçük başlayın, gerektiğinde ölçeklendirin, hizmetleri amortismana başlayın, yenilerini ekleyin ve müşteri kullanımıyla birlikte gelişin. Ayrıca, mikro hizmetleri çoğu geliştirici için daha ulaşılabilir hale getirmek için henüz çözülmesi gereken pek çok sorun olduğunu biliyoruz. Kapsayıcılar ve aktör programlama modeli bu yöndeki küçük adımlara örnektir. Mikro hizmetlere daha kolay yaklaşmak için daha fazla yeniliğin ortaya çıkacağından eminiz.

## <a name="next-steps"></a>Sonraki adımlar

* [Microservices: Bulut tarafından desteklenen bir uygulama devrimi](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/)
* [Azure Architecture Center: Azure'da mikro hizmetler oluşturma](https://docs.microsoft.com/azure/architecture/microservices/)
* [Azure Hizmet Kumaş uygulaması ve küme en iyi uygulamaları](service-fabric-best-practices-overview.md)
* [Hizmet Kumaş terminolojisi genel bakış](service-fabric-technical-overview.md)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
[Image3]: media/service-fabric-overview-microservices/microservices-migration.png

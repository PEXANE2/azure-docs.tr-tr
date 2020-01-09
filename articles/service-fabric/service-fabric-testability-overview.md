---
title: Hata analiz hizmetine genel bakış
description: Bu makalede, hizmetlerinize karşı hataları ve test senaryolarını çalıştırmak için Service Fabric 'daki hata analizi hizmeti açıklanmaktadır.
author: anmolah
ms.topic: conceptual
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: d5c770a4d823ebe9b2700b081c407c54dd1d18a3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75465577"
---
# <a name="introduction-to-the-fault-analysis-service"></a>Hata analiz hizmetine giriş
Hata analiz hizmeti, Microsoft Azure Service Fabric oluşturulan Hizmetleri test etmek için tasarlanmıştır. Hata analiz hizmeti sayesinde, anlamlı hatalara ve tüm test senaryolarını uygulamalarınıza göre çalıştırabilirsiniz. Bu hatalar ve senaryolar, bir hizmetin yaşam süresi boyunca, denetimli, güvenli ve tutarlı bir şekilde deneyilecek çok sayıda durum ve geçiş yapar ve doğrular.

Eylemler, test için bir hizmeti hedefleyen bireysel hatalardır. Bir hizmet geliştiricisi, bunları karmaşık senaryolar yazmak için yapı taşları olarak kullanabilir. Örneğin:

* Bir makinenin veya VM 'nin yeniden başlatıldığı sayıda durum benzetimi yapmak için bir düğümü yeniden başlatın.
* Yük Dengeleme, yük devretme veya uygulama yükseltme benzetimi yapmak için durum bilgisi olan hizmetinizin bir çoğaltmasını taşıyın.
* Yeni verileri kabul etmek için yeterli "yedekleme" veya "ikincil" çoğaltma olmadığından yazma işlemlerinin devam edemeyeceği bir durum oluşturmak için durum bilgisi olan bir hizmette çekirdek kaybı 'nı çağırın.
* Tüm bellek içi durumun tamamen temizlendiğinden oluşan bir durum oluşturmak için durum bilgisi olan bir hizmette veri kaybını çağırma.

Senaryolar, bir veya daha fazla eylemden oluşan karmaşık işlemlerdir. Hata Analizi hizmeti, iki yerleşik tam senaryo sağlar:

* Chaos senaryosu
* Yük devretme senaryosu

## <a name="testing-as-a-service"></a>Hizmet olarak test etme
Hata analiz hizmeti, Service Fabric kümesiyle otomatik olarak başlatılan bir Service Fabric sistem hizmetidir. Bu hizmet hata ekleme, test senaryosu yürütme ve sistem durumu analizi için konak görevi görür. 

![Hata analiz hizmeti][0]

Bir hata eylemi veya test senaryosu başlatıldığında, hata eylemi veya test senaryosunu çalıştırmak için hata analiz hizmetine bir komut gönderilir. Hata Analizi hizmeti, hata ve senaryoları güvenilir bir şekilde çalıştırıp sonuçları doğrulayabilecek şekilde durum bilgisine sahiptir. Örneğin, uzun süre çalışan bir test senaryosu hata analizi hizmeti tarafından güvenilir bir şekilde çalıştırılabilir. Testler küme içinde yürütüldüğü için, hizmet, sorunlar hakkında daha ayrıntılı bilgi sağlamak üzere kümenin ve hizmetlerinizin durumunu inceleyebilir.

## <a name="testing-distributed-systems"></a>Dağıtılmış sistemleri test etme
Service Fabric, dağıtılmış ölçeklenebilir uygulamaları yazma ve yönetme işini önemli ölçüde daha kolay hale getirir. Hata Analizi hizmeti, dağıtılmış bir uygulamayı test etmeyi benzer şekilde kolaylaştırır. Sınama sırasında çözülmesi gereken üç ana sorun vardır:

1. Gerçek dünyada senaryolarda oluşabilecek hataların benzetimi/üretilmesi: Service Fabric önemli yönlerinden biri, Dağıtılmış uygulamaların çeşitli hatalardan kurtarılmasına olanak tanımaktır. Bununla birlikte, uygulamanın bu hatalardan kurtulabilmesini test etmek için, denetlenen bir test ortamında bu gerçek dünyada oluşan sorunları benzetim/oluşturma mekanizmasına ihtiyacımız var.
1. Bağıntılı arızalar oluşturma özelliği: sistemdeki, ağ hatalarının ve makine hatalarının gibi temel hataların ayrı olarak üretilmesi kolaydır. Bu bireysel hataların etkileşimlerin bir sonucu olarak gerçek dünyada gerçekleşebilecek önemli sayıda senaryo oluşturma, önemsiz olmayan bir sayıdır.
1. Çeşitli geliştirme ve dağıtım seviyeleri arasında Birleşik deneyim: farklı hata türleri gerçekleştiren çok sayıda hata ekleme sistemi vardır. Ancak, bu Deneyimlerdeki deneyim, tek kutudan oluşan geliştirici senaryolarından geçiş yaparken büyük test ortamlarında aynı testleri çalıştırmak için, bunları üretimde testler için kullanmak üzere yetersiz.

Bu sorunları çözmek için çok sayıda mekanizma olsa da, bir tek Box geliştirici ortamından üretim kümelerinde test etmek için gereken tüm yollar--yok. Hata Analizi hizmeti, uygulama geliştiricilerinin iş mantığını test etmeye odaklanmasını sağlar. Hata analiz hizmeti, hizmet etkileşimini temel alınan dağıtılmış sistemle test etmek için gereken tüm özellikleri sağlar.

### <a name="simulatinggenerating-real-world-failure-scenarios"></a>Gerçek dünyada hata senaryolarına benzetim/oluşturma
Dağıtılmış bir sistemin hatalara karşı sağlamlığını test etmek için, başarısızlık oluşturmak için bir mekanizmaya ihtiyacımız var. Teorik olarak, düğüm azaltma gibi bir hata oluşturulması kolay görünse de, Service Fabric çözmeye çalıştığı aynı tutarlılık sorunları kümesine vurmaya başlar. Örnek olarak, bir düğümü kapatmak istiyoruz, gerekli iş akışı aşağıdaki gibidir:

1. İstemciden bir kapalı düğüm isteği yayınlayın.
1. İsteği sağ düğüme gönderin.
   
    a. Düğüm bulunamazsa, başarısız olmalıdır.
   
    b. Düğüm bulunursa, yalnızca düğüm kapalıysa döndürmelidir.

Test perspektifinden hatayı doğrulamak için, testin bu hata ortaya çıkar durumunda hatanın gerçekten meydana gelebildiğini bilmeleri gerekir. Service Fabric garantisi, düğüm bir alt düğüme ulaştığında veya zaten kapatılmış olmalıdır. Her iki durumda da test durum hakkında doğru bir neden olabilir ve başarılı veya doğrulamasında doğru bir şekilde başarısız olur. Aynı başarısızlık kümesini yapmak için Service Fabric dışında uygulanan bir sistem birçok ağ, donanım ve yazılım sorununa ulaşarak önceki garantiyi sağlamamasını önler. Daha önce belirtilen sorunların mevcut olması durumunda, Service Fabric sorunları geçici olarak çözmek için küme durumunu yeniden yapılandırır ve bu nedenle hata analiz hizmeti, doğru garanti kümesine yine de izin verebilecektir.

### <a name="generating-required-events-and-scenarios"></a>Gerekli olayları ve senaryoları oluşturma
Gerçek dünyanın bir başarısızlığının sürekli olarak benzetilirken, ilişkili hatalar oluşturma özelliği bile daha da yüksek bir değer sağlar. Örneğin, aşağıdaki şeyler gerçekleştiğinde durum bilgisi olan kalıcı bir hizmette veri kaybı oluşur:

1. Çoğaltmalarda yalnızca bir yazma çekirdeği yakalanır. Tüm ikincil çoğaltmalar, birincil arka planda gecikti.
1. Yazma çekirdeği, kopyaların kapanması nedeniyle aşağı gider (bir kod paketi veya düğüm nedeniyle).
1. Çoğaltma verileri kaybolduğundan (disk bozulması veya makine yeniden görüntüsü nedeniyle) yazma çekirdeği geri alınamıyor.

Bu bağıntılı arızalar gerçek dünyada gerçekleşir, ancak tek tek hatalarda genellikle bu değildir. Bu senaryoları üretimde gerçekleşmeden önce test etme özelliği kritiktir. Daha da önemli olan bu senaryolara, denetimli koşullarda (destedeki tüm mühendislerle günün ortasında) üretim iş yükleriyle ilgili olarak benzetim yapabilme olanağıdır. Bu, 2:00 saat ' de üretim sırasında ilk kez gerçekleşenden çok daha iyidir

### <a name="unified-experience-across-different-environments"></a>Farklı ortamlarda birleştirilmiş deneyim
Bu yöntem, geliştirme ortamı, biri testler ve bir üretim için bir tane olmak üzere üç farklı deneyim kümesi oluşturmaktır. Model:

1. Geliştirme ortamında, tek tek yöntemlerin birim testlerine izin veren durum geçişleri üretin.
1. Test ortamında, çeşitli hata senaryoları sunan uçtan uca testlere izin veren hatalar üretir.
1. Doğal olmayan hataları önlemek ve hataya son derece hızlı insan yanıtı olduğundan emin olmak için üretim ortamını Pristine tutun.

Service Fabric, hata analizi hizmeti aracılığıyla bunu açıp geliştirici ortamından üretim için aynı metodolojiyi kullanmayı öneriyor. Bunu başarmanın iki yolu vardır:

1. Denetlenen hatalara yönelik olarak, bir tek Box ortamından, üretim kümelerine kadar olan hata analiz hizmeti API 'Lerini kullanın.
1. Kümeye otomatik olarak sızmaya neden olan bir humması sağlamak için hata analiz hizmetini kullanarak otomatik hata oluşturun. Yapılandırma üzerinden başarısızlık oranını denetlemek, aynı hizmetin farklı ortamlarda farklı şekilde sınanmasını sağlar.

Service Fabric ile, hataların ölçeği farklı ortamlarda farklı olabilir, ancak gerçek mekanizmalar aynı olur. Bu, çok daha hızlı bir dağıtım işlem hattının ve gerçek hayatta yükleme altında hizmetleri test etmenize olanak tanır.

## <a name="using-the-fault-analysis-service"></a>Hata analiz hizmetini kullanma
**C#**

Hata çözümleme hizmeti özellikleri, Microsoft. ServiceFabric NuGet paketindeki System. Fabric ad alanıdır. Hata analiz hizmeti özelliklerini kullanmak için, NuGet paketini projenize bir başvuru olarak ekleyin.

**PowerShell**

PowerShell 'i kullanmak için Service Fabric SDK 'sını yüklemelisiniz. SDK yüklendikten sonra, kullanabilmeniz için ServiceFabric PowerShell modülü otomatik olarak yüklenir.

## <a name="next-steps"></a>Sonraki adımlar
Gerçekten bulut ölçeğinde hizmetler oluşturmak için, dağıtımdan önce ve sonra, bu hizmetlerin gerçek Uluslararası hatalardan bağımsız olarak olmasını sağlamak önemlidir. Günümüzde hizmetler dünyasında hızlı bir şekilde yenilik yapın ve kodu üretime hızlı bir şekilde taşıyabilirsiniz. Hata Analizi hizmeti, Service Developers 'ın tam olarak bu işlemleri belirlemesine yardımcı olur.

Yerleşik [test senaryolarını](service-fabric-testability-scenarios.md)kullanarak uygulamalarınızı ve hizmetlerinizi test etmeye başlayın veya hata analizi hizmeti tarafından sunulan [hata eylemlerini](service-fabric-testability-actions.md) kullanarak kendi test senaryolarınızı yazın.

<!--Image references-->
[0]: ./media/service-fabric-testability-overview/faultanalysisservice.png

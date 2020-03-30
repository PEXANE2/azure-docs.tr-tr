---
title: Arıza Analizi Hizmetine genel bakış
description: Bu makalede, hataları indükleyen ve hizmetlerinize karşı test senaryoları çalıştırmak için Hizmet Dokusunda Hata Çözümleme Hizmeti açıklanmaktadır.
author: anmolah
ms.topic: conceptual
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: d5c770a4d823ebe9b2700b081c407c54dd1d18a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75465577"
---
# <a name="introduction-to-the-fault-analysis-service"></a>Arıza Analizi Hizmetine Giriş
Hata Analizi Hizmeti, Microsoft Azure Hizmet Kumaşı'nda yerleşik hizmetleri test etmek için tasarlanmıştır. Hata Analizi Hizmeti ile anlamlı hatalara neden olabilir ve uygulamalarınıza karşı tam test senaryoları çalıştırabilirsiniz. Bu hatalar ve senaryolar, bir hizmetin ömrü boyunca, tümü kontrollü, güvenli ve tutarlı bir şekilde yaşayacağı çok sayıda durumu ve geçişi kullanır ve doğrular.

Eylemler, bir hizmeti sınayıp test etmek için tek tek hatalardır. Bir hizmet geliştiricisi karmaşık senaryolar yazmak için bunları yapı taşları olarak kullanabilir. Örnek:

* Makine veya VM'nin yeniden başlatıldığı durumların herhangi bir sayısını simüle etmek için düğümü yeniden başlatın.
* Yük dengeleme, başarısızlık veya uygulama yükseltmesini simüle etmek için devlet hizmetinizin bir kopyasını taşıyın.
* Yeni verileri kabul etmek için yeterli "yedekleme" veya "ikincil" yineleme olmadığından, yazma işlemlerinin devam edemediği bir durum oluşturmak için durum lu bir hizmette yeterli çoğunluk kaybını başlatın.
* Tüm bellek durumunun tamamen silindiği bir durum oluşturmak için durum bilgisine yönelik bir hizmette veri kaybı nı çağırın.

Senaryolar, bir veya daha fazla eylemden oluşan karmaşık işlemlerdir. Hata Çözümleme Hizmeti iki yerleşik tam senaryo sağlar:

* Kaos Senaryosu
* Failover Senaryosu

## <a name="testing-as-a-service"></a>Hizmet olarak test etme
Arıza Analizi Hizmeti, Service Fabric kümesi ile otomatik olarak başlatılan bir Service Fabric sistem hizmetidir. Bu hizmet, hata enjeksiyonu, test senaryosu yürütme ve sistem durumu çözümlemesi için ana bilgisayar görevi görür. 

![Arıza Analiz Hizmeti][0]

Bir hata eylemi veya test senaryosu başlatıldığında, hata eylemi veya test senaryosunu çalıştırmak için Hata Çözümleme Hizmeti'ne bir komut gönderilir. Hata Çözümleme Hizmeti, hataları ve senaryoları güvenilir bir şekilde çalıştırabilmesi ve sonuçları doğrulayabilmek için durum iyidir. Örneğin, uzun süren bir test senaryosu Hata Çözümleme Hizmeti tarafından güvenilir bir şekilde yürütülebilir. Ve testler küme içinde yürütüldüründen, hizmet hatalar hakkında daha ayrıntılı bilgi sağlamak için kümenin durumunu ve hizmetlerinizi inceleyebilir.

## <a name="testing-distributed-systems"></a>Dağıtılmış sistemlerin test edilmesi
Service Fabric, dağıtılabilir uygulamaları yazma ve yönetme işini önemli ölçüde kolaylaştırır. Hata Analizi Hizmeti, dağıtılmış bir uygulamayı benzer şekilde test etmeyi kolaylaştırır. Sınama sırasında çözülmesi gereken üç ana sorun vardır:

1. Gerçek dünya senaryolarında oluşabilecek hataları simüle etme/oluşturma: Service Fabric'in önemli yönlerinden biri, dağıtılmış uygulamaların çeşitli hatalardan kurtarılmasını sağlamasıdır. Ancak, uygulamanın bu hatalardan kurtulabildiğini test etmek için, kontrollü bir test ortamında bu gerçek dünyadaki hataları simüle etmek/oluşturmak için bir mekanizmaya ihtiyacımız vardır.
1. İlişkili hatalar oluşturma yeteneği: Ağ hataları ve makine hataları gibi sistemdeki temel hataların tek tek üretilemesi kolaydır. Bu bireysel hataların etkileşimleri sonucunda gerçek dünyada gerçekleşebilecek önemli sayıda senaryo oluşturmak önemsiz değildir.
1. Geliştirme ve dağıtım çeşitli düzeylerde birleşik deneyim: Hatalar çeşitli yapabilirsiniz birçok hata enjeksiyon sistemleri vardır. Ancak, tek kutugeliştirici senaryolarından büyük test ortamlarında aynı testleri çalıştırmaya ve bunları üretimdeki testler için kullanmaya taşınırken bunların tümlerindeki deneyim zayıftır.

Bu sorunları çözmek için birçok mekanizma olsa da, üretim kümelerinde test etmek için tek kutugeliştirici ortamından gerekli garantilerle aynı şeyi yapan bir sistem eksiktir. Hata Çözümleme Hizmeti, uygulama geliştiricilerin iş mantığını sınayın. Hata Analizi Hizmeti, hizmetin altta yatan dağıtılmış sistemle etkileşimini test etmek için gereken tüm yetenekleri sağlar.

### <a name="simulatinggenerating-real-world-failure-scenarios"></a>Gerçek dünyadaki hata senaryolarını simüle etme/oluşturma
Dağıtılmış bir sistemin sağlamlığını hatalara karşı test etmek için, hata oluşturmak için bir mekanizmaya ihtiyacımız vardır. Teoride, aşağı bir düğüm gibi bir başarısızlık üreten kolay görünüyor olsa da, Hizmet Kumaş çözmeye çalışıyor tutarlılık sorunları aynı dizi isabet başlar. Örnek olarak, bir düğümü kapatmak istiyorsak, gerekli iş akışı aşağıdaki gibidir:

1. İstemciden, kapatma düğümü isteği nde bulunun.
1. İsteği doğru düğüme gönderin.
   
    a. Düğüm bulunamazsa, başarısız olur.
   
    b. Düğüm bulunursa, yalnızca düğüm kapatılırsa döndürülür.

Bir test perspektifinden başarısızlığı doğrulamak için, test bu hata indüklendiğinde, hata nın gerçekte gerçekleştiğini bilmesi gerekir. Service Fabric'in sağladığı garanti, komut düğüme ulaştığında düğümün aşağı ineceği veya zaten inmiş olmasıdır. Her iki durumda da test, durum hakkında doğru bir şekilde neden olabilir ve başarılı olmalı veya doğru doğrulama başarısız. Aynı hata kümesini yapmak için Service Fabric dışında uygulanan bir sistem, birçok ağ, donanım ve yazılım sorunlarını vurarak önceki garantileri sağlamasını engelleyebilir. Daha önce belirtilen sorunların varlığında, Service Fabric küme durumunu sorunları aşmak üzere yeniden yapılandıracak ve bu nedenle Hata Analizi Hizmeti doğru garanti kümesini vermeye devam edebilecektir.

### <a name="generating-required-events-and-scenarios"></a>Gerekli olayları ve senaryoları oluşturma
Gerçek dünyadaki bir başarısızlığı sürekli olarak simüle etmek zor olsa da, ilişkili hatalar oluşturma yeteneği daha da zordur. Örneğin, aşağıdaki şeyler olduğunda, bir veri kaybı durum lu bir kalıcı hizmette gerçekleşir:

1. Çoğaltmaların yalnızca bir yazma yeter sayısı çoğaltma yakalanır. Tüm ikincil yinelemeler birincil arkasında kalıyor.
1. Yazma yeter sayısı, kopyaların (bir kod paketi veya düğümün aşağı inmelerinden dolayı) düşmesi nedeniyle düşer.
1. Yinelemelere ait veriler kaybolduğundan (disk bozulması veya makine yeniden görüntülemesi nedeniyle) yazma yeter sayısı geri alınamaz.

Bu ilişkili hatalar gerçek dünyada olur ama bireysel başarısızlıklar kadar sık değil. Bu senaryoları üretimde gerçekleşmeden önce sınayabilme yeteneği çok önemlidir. Daha da önemlisi kontrollü durumlarda (güvertede tüm mühendisler ile günün ortasında) üretim iş yükleri ile bu senaryoları simüle yeteneğidir. Bu, saat 02:00'de üretimde ilk kez olmasından çok daha iyi.

### <a name="unified-experience-across-different-environments"></a>Farklı ortamlarda birleşik deneyim
Uygulama geleneksel olarak üç farklı deneyim kümesi oluşturmak olmuştur, biri geliştirme ortamı için, biri testler için, diğeri de üretim için. Model şöyleydi:

1. Geliştirme ortamında, tek tek yöntemlerin birim testlerine izin veren durum geçişleri üretir.
1. Test ortamında, çeşitli hata senaryoları uygulayan uçlardan uca testlere izin vermekiçin hatalar üretin.
1. Doğal olmayan hataları önlemek ve hataya karşı son derece hızlı bir insan tepkisi olduğundan emin olmak için üretim ortamını el değmemiş halde tutun.

Service Fabric'te, Arıza Analizi Hizmeti aracılığıyla, bunu tersine çevirmeyi ve geliştirici ortamından üretime aynı metodolojiyi kullanmayı öneriyoruz. Bunu başarmanın iki yolu vardır:

1. Denetleniyi hataya neden olmak için, tek kutulu bir ortamdan üretim kümelerine kadar Hata Çözümleme Hizmeti API'lerini kullanın.
1. Kümeye otomatik arıza indüksiyonuna neden olan bir ateş vermek için, otomatik hatalar oluşturmak için Hata Analizi Hizmetini kullanın. Yapılandırma yoluyla hata oranını denetlemek, aynı hizmetin farklı ortamlarda farklı şekilde sınanmasını sağlar.

Service Fabric ile, hataların ölçeği farklı ortamlarda farklı olsa da, gerçek mekanizmalar aynı olacaktır. Bu, çok daha hızlı bir kod-dağıtım boru hattı ve gerçek dünya yükleri altında hizmetleri test etmek için yeteneği sağlar.

## <a name="using-the-fault-analysis-service"></a>Arıza Analiz Hizmetini Kullanma
**C #**

Hata Analizi Hizmeti özellikleri, Microsoft.ServiceFabric NuGet paketindeki System.Fabric ad alanında bulunmaktadır. Hata Analizi Hizmeti özelliklerini kullanmak için, projenize nuget paketini referans olarak ekleyin.

**Powershell**

PowerShell'i kullanmak için Service Fabric SDK'yı yüklemeniz gerekir. SDK kurulduktan sonra ServiceFabric PowerShell modülü kullanmanız için otomatik olarak yüklenir.

## <a name="next-steps"></a>Sonraki adımlar
Gerçek bulut ölçekli hizmetler oluşturmak için, dağıtımdan önce ve sonra hizmetlerin gerçek dünyadaki hatalara dayanabileceğinden emin olmak çok önemlidir. Günümüzde hizmet dünyasında hızlı bir şekilde yenilik yapma ve kodu hızlı üretime taşıma becerisi çok önemlidir. Hata Analizi Hizmeti, hizmet geliştiricilerin tam olarak bunu yapmasına yardımcı olur.

Yerleşik [test senaryolarını](service-fabric-testability-scenarios.md)kullanarak uygulamalarınızı ve hizmetlerinizi sınamaya başlayın veya Hata Analizi Hizmeti tarafından sağlanan [hata eylemlerini](service-fabric-testability-actions.md) kullanarak kendi test senaryolarınızı yazmaya başlayın.

<!--Image references-->
[0]: ./media/service-fabric-testability-overview/faultanalysisservice.png

---
title: Azure Cosmos DB ile küresel dağıtım- başlık altında
description: Bu makale, Azure Cosmos DB'nin küresel dağıtımıyla ilgili teknik ayrıntılar sağlar
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: sngun
ms.reviewer: sngun
ms.openlocfilehash: a46a69476a2ad6550bc7b3a533fd09565d461db3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74872137"
---
# <a name="global-data-distribution-with-azure-cosmos-db---under-the-hood"></a>Azure Cosmos DB ile küresel veri dağıtımı - başlık altında

Azure Cosmos DB, Azure'da temel bir hizmettir, bu nedenle kamu, egemen, Savunma Bakanlığı (DoD) ve devlet bulutları da dahil olmak üzere tüm Azure bölgelerinde dağıtılır. Bir veri merkezi içinde, Azure Cosmos DB'yi her biri özel yerel depolama alanına sahip devasa makine pulları üzerinde dağıtıyor ve yönetiyoruz. Bir veri merkezi içinde, Azure Cosmos DB, her biri birden çok nesil donanım çalıştıran birçok kümeye dağıtılır. Küme içindeki makineler genellikle bir bölge içinde yüksek kullanılabilirlik için 10-20 fay etki alanına yayılır. Aşağıdaki resim Cosmos DB küresel dağıtım sistemi topolojisini göstermektedir:

![Sistem Topolojisi](./media/global-dist-under-the-hood/distributed-system-topology.png)

**Azure Cosmos DB'deki genel dağıtım anahtar teslimidir:** İstediğiniz zaman, birkaç tıklamayla veya programlı olarak tek bir API çağrısıyla Cosmos veritabanınızla ilişkili coğrafi bölgeleri ekleyebilir veya kaldırabilirsiniz. Bir Cosmos veritabanı, buna karşılık, Cosmos kapsayıcılar bir dizi oluşur. Cosmos DB'de kapsayıcılar, mantıksal dağıtım ve ölçeklenebilirlik birimleri olarak hizmet vermektedir. Oluşturduğunuz koleksiyonlar, tablolar ve grafikler (dahili olarak) yalnızca Cosmos kaplarıdır. Kapsayıcılar tamamen şema-agnostik ve bir sorgu için bir kapsam sağlar. Cosmos kabındaki veriler, yutulması üzerine otomatik olarak dizine eklenir. Otomatik dizin oluşturma, kullanıcıların verileri şema veya dizin yönetimi güçlükleri olmadan, özellikle de genel olarak dağıtılmış bir kurulumda sorgulamalarına olanak tanır.  

- Belirli bir bölgede, kapsayıcı içindeki veriler, sağladığınız ve saydam olarak temel fiziksel bölümler *(yerel dağıtım)* tarafından yönetilen bir bölüm anahtarı kullanılarak dağıtılır.  

- Her fiziksel bölüm de coğrafi bölgeler *(küresel dağıtım)* arasında çoğaltılır. 

Cosmos DB kullanan bir uygulama, cosmos kapsayıcısı üzerindeki iş artışını elastik olarak ölçeklediğinde veya daha fazla depolama alanı tüketdiğinde, Cosmos DB tüm bölgelerde bölüm yönetimi işlemlerini (bölme, klonlama, silme) saydam olarak işler. Ölçek, dağıtım veya hatalardan bağımsız olarak Cosmos DB, kapsayıcılar içindeki verilerin tek bir sistem görüntüsünü sağlamaya devam eder ve bu görüntü herhangi bir sayıda bölgeye küresel olarak dağıtılır.  

Aşağıdaki resimde gösterildiği gibi, bir kapsayıcı içindeki veriler iki boyut boyunca dağıtılır - bir bölge içinde ve bölgeler arasında, dünya çapında:  

![fiziksel bölümler](./media/global-dist-under-the-hood/distribution-of-resource-partitions.png)

Fiziksel bir bölüm çoğaltma *kümesi*olarak adlandırılan çoğaltma grubu tarafından uygulanır. Her makine, yukarıdaki resimde gösterildiği gibi sabit bir işlem kümesi içinde çeşitli fiziksel bölümlere karşılık gelen yüzlerce yineleme barındırAr. Fiziksel bölümlere karşılık gelen yinelemeler dinamik olarak yerleştirilir ve bir küme içindeki makineler ve bir bölge içindeki veri merkezleri arasında dengeli bir şekilde yüklenir.  

Yineleme benzersiz bir Azure Cosmos DB kiracısına aittir. Her yineleme, kaynakların yanı sıra ilişkili dizinleri yöneten Cosmos DB [veritabanı altyapısının](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf)bir örneğini barındırır. Cosmos veritabanı motoru atom-kayıt sırası (ARS) tabanlı tip sistemi üzerinde çalışır. Motor, bir şema kavramına agnostiktir ve kayıtların yapısı ve örnek değerleri arasındaki sınırı bulanıklaştırır. Cosmos DB, kullanıcıların şema veya dizin yönetimi yle uğraşmak zorunda kalmadan küresel olarak dağıtılan verilerini sorgulamalarına olanak tanıyan, her şeyi etkin bir şekilde otomatik olarak dizine ekleyerek tam şema agnostisizmini elde eder.

Cosmos veritabanı altyapısı, çeşitli koordinasyon ilkellerinin, dil çalışma sürelerinin, sorgu işlemcisinin ve verilerin işlemsel depolama ve dizine eklenmesinden sorumlu alt sistemlerin depolanması ve dizine eklenmesi gibi bileşenlerden oluşur, Sıra -sıyla. Dayanıklılık ve yüksek kullanılabilirlik sağlamak için, veritabanı altyapısı verilerini ve dizinini SSD'lerde devam eder ve sırasıyla çoğaltma kümesi(ler) içindeki veritabanı altyapısı örnekleri arasında çoğaltır. Daha büyük kiracılar, daha yüksek iş boyutu ve depolama ölçeğine karşılık gelir ve daha büyük veya daha fazla yinelemeye veya her ikisine de sahiptir. Sistemin her bileşeni tamamen asynchronous - hiçbir iş parçacığı hiç bloklar ve her iş parçacığı herhangi bir gereksiz iplik anahtarları tahakkuk etmeden kısa ömürlü iş yok. Hız sınırlayıcı ve geri basınç, kabul kontrolünden tüm G/Ç yollarına kadar tüm yığın boyunca yer alar. Cosmos veritabanı altyapısı, ince taneli eşzamanlılıktan yararlanmak ve tutumlu sistem kaynakları içinde çalışırken yüksek iş hacmi sağlamak üzere tasarlanmıştır.

Cosmos DB'nin küresel dağılımı iki temel soyutlamaya dayanır – *çoğaltma kümeleri* ve *bölüm kümeleri.* Çoğaltma kümesi koordinasyon için modüler bir Lego bloğudur ve bölüm kümesi bir veya daha fazla coğrafi olarak dağıtılmış fiziksel bölümlerin dinamik bir bindirmesidir. Küresel dağılımın nasıl işlediğini anlamak için, bu iki temel soyutlamayı anlamamız gerekir. 

## <a name="replica-sets"></a>Çoğaltma kümeleri

Fiziksel bölüm, çoğaltma kümesi olarak adlandırılan birden çok hata etki alanına yayılmış kendi kendini yöneten ve dinamik olarak yük dengeli bir çoğaltma grubu olarak somutlaştırılır. Bu küme, fiziksel bölüm içindeki verileri son derece kullanılabilir, dayanıklı ve tutarlı hale getirmek için çoğaltılan durum makinesi protokolünü topluca uygular. Çoğaltma kümesi üyeliği *N* dinamiktir – hatalara, yönetim işlemlerine ve başarısız yinelemelerin yeniden oluşturma/kurtarma süresine bağlı olarak *NMin* ve *NMax* arasında dalgalanmaya devam eder. Üyelik değişikliklerine bağlı olarak, çoğaltma protokolü okuma ve yazma yeter sayısının boyutunu da yeniden yapılandırır. Belirli bir fiziksel bölüme atanan iş çıktısını düzgün bir şekilde dağıtmak için iki fikir kullanırız: 

- İlk olarak, lider üzerindeki yazma isteklerini işleme maliyeti, takipçideki güncelleştirmeleri uygulama maliyetinden daha yüksektir. Buna karşılık, lider takipçileri daha fazla sistem kaynakları bütçelenir. 

- İkinci olarak, mümkün olduğunca, belirli bir tutarlılık düzeyi için okunan çoğunluk yalnızca takipçi yinelemeler oluşur. Gerekli olmadıkça okuma servisi için liderle irtibata geçmekten kaçınıyoruz. Cosmos DB'nin desteklediği [beş tutarlılık modeli](consistency-levels.md) için çoğunluk tabanlı sistemlerde yük ve [kapasite](https://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf) ilişkisi üzerine yapılan araştırmadan bir dizi fikir alıyoruz.  

## <a name="partition-sets"></a>Bölüm kümeleri

Cosmos veritabanı bölgeleriyle yapılandırılan her birinden birer fiziksel bölüm grubu, yapılandırılan tüm bölgelerde çoğaltılan aynı anahtar kümesini yönetmek için oluşturulur. Bu yüksek koordinasyon ilkel bir *bölüm kümesi* denir - belirli bir anahtar kümesini yöneten fiziksel bölümlerin coğrafi olarak dağıtılmış dinamik bindirme. Belirli bir fiziksel bölüm (yineleme kümesi) bir küme içinde kapsamlı olsa da, bir bölüm kümesi aşağıdaki resimde gösterildiği gibi kümeleri, veri merkezlerini ve coğrafi bölgeleri kaplayabilir:  

![Bölüm Setleri](./media/global-dist-under-the-hood/dynamic-overlay-of-resource-partitions.png)

Bir bölüm kümesini, aynı anahtar kümesine sahip birden çok çoğaltma kümesinden oluşan coğrafi olarak dağılmış bir "süper çoğaltma kümesi" olarak düşünebilirsiniz. Çoğaltma kümesine benzer şekilde, bir bölüm kümesinin üyeliği de dinamiktir – belirli bir bölüm kümesine yeni bölümler eklemek/kaldırmak için örtük fiziksel bölüm yönetimi işlemlerine dayalı olarak dalgalanır (örneğin, bir kapsayıcıda iş çıktısını ölçeklediğinizde, Cosmos veritabanınıza bir bölge ekle/kaldırveya hatalar oluştuğunda). Bölümlerin her birinin (bir bölüm kümesinin) kendi çoğaltma kümesi içinde bölüm kümesi üyeliğini yönetmesi sayesinde, üyelik tamamen merkezi olmayan ve son derece kullanılabilir. Bir bölüm kümesinin yeniden yapılandırılması sırasında, fiziksel bölümler arasındaki bindirme topolojisi de kurulur. Topoloji, kaynak ve hedef fiziksel bölümler arasındaki tutarlılık düzeyine, coğrafi uzaklığa ve kullanılabilir ağ bant genişliğine göre dinamik olarak seçilir.  

Bu hizmet, Cosmos veritabanlarınızı tek bir yazma bölgesi yle veya birden çok yazma bölgesiyle yapılandırmanıza olanak tanır ve seçime bağlı olarak, bölüm kümeleri tam olarak bir veya tüm bölgelerdeki yazıları kabul etmek üzere yapılandırılır. Sistem iki düzeyli, iç içe konsensüs protokolü kullanır – bir düzey, yazıları kabul eden fiziksel bir bölümün yineleme kümesinin kopyaları içinde çalışır ve diğeri herkes için tam sipariş garantisi sağlamak için bir bölüm kümesi düzeyinde çalışır taahhüt edilen bölüm kümesi içinde yazar. Bu çok katmanlı, iç içe konsensüs, yüksek kullanılabilirlik için sıkı SLA'larımızın uygulanması nın yanı sıra Cosmos DB'nin müşterilerine sunduğu tutarlılık modellerinin uygulanması için çok önemlidir.  

## <a name="conflict-resolution"></a>Çakışma çözümleme

Güncelleştirme yayılımı, çakışma çözümü ve nedensellik izleme için [tasarımımız, salgın algoritmalar](https://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf) ve [Bayou](https://zoo.cs.yale.edu/classes/cs422/2013/bib/terry95managing.pdf) sistemi üzerinde önceki çalışmalardan esinlenerek yapılmıştır. Fikirlerin çekirdekleri hayatta kalmış ve Cosmos DB'nin sistem tasarımını iletmek için uygun bir referans çerçevesi sağlarken, cosmos DB sistemine uyguladığımız için önemli bir dönüşüm emilmiştir. Bu gerekliydi, çünkü önceki sistemler ne kaynak yönetimi yle ne de Cosmos DB'nin çalışması gereken ölçekle tasarlanmıştır, ne de yetenekleri (örneğin, sınırlı bayatlık tutarlılığı) sağlamak için ve sıkı ve Cosmos DB'nin müşterilerine sunduğu kapsamlı SLA'lar.  

Bir bölüm kümesinin birden çok bölgeye dağıtıldığını ve belirli bir bölüm kümesinden oluşan fiziksel bölümler arasındaki verileri çoğaltmak için Cosmos DBs (çok büyük) çoğaltma protokolünü izlediğini hatırlayın. Her fiziksel bölüm (bir bölüm kümesi) kabul eder ve genellikle o bölgeye yerel istemcilere okur hizmet vermektedir. Bir bölge içinde fiziksel bir bölüm tarafından kabul edilen yazılar, istemciye kabul edilmeden önce, fiziksel bölüm içinde son derece kullanılabilir hale getirilir. Bunlar geçici yazılardır ve bir anti-entropi kanalı kullanılarak bölüm kümesi içindeki diğer fiziksel bölümlere yayılır. İstemciler, bir istek üstbilgisinden geçerek geçici veya taahhütedilen yazıları isteyebilir. Anti-entropi yayılımı (yayılma sıklığı dahil) dinamiktir, bölüm kümesinin topolojisi, fiziksel bölümlerin bölgesel yakınlığı ve yapılandırılan tutarlılık düzeyine dayanır. Bir bölüm kümesi içinde, Cosmos DB dinamik olarak seçilmiş bir hakem bölümü ile birincil işleme düzeni izler. Hakem seçimi dinamiktir ve bindirmenin topolojisini temel alan bölüm kümesinin yeniden yapılandırılmasının ayrılmaz bir parçasıdır. Taahhüt edilen yazıların (çok satırlı/toplu güncelleştirmeler dahil) sipariş edilmesi garanti edilir. 

Güncelleştirme çakışmalarını algılamak ve çözmek için nedensellik izleme ve sürüm vektörleri için kodlanmış vektör saatlerini (sırasıyla çoğaltma kümesi ve bölüm kümesindeki her bir konsensüs düzeyine karşılık gelen bölge kimliği ve mantıksal saatler içeren) kullanırız. Topoloji ve eş seçimi algoritması, sabit ve minimum depolama ve sürüm vektörlerinin en az ağ yükü sağlamak üzere tasarlanmıştır. Algoritma sıkı yakınsama özelliğini garanti eder.  

Birden çok yazma bölgesiyle yapılandırılan Cosmos veritabanları için sistem, geliştiricilerin aralarından seçim yapabilmek için aşağıdakiler dahil olmak üzere bir dizi esnek otomatik çakışma çözümleme ilkeleri sunar: 

- Varsayılan olarak sistem tanımlı bir zaman damgası özelliği (zaman senkronizasyon saat idamı protokolünü temel alan) kullanan **Son Yazma-Kazan'lar (LWW)** Cosmos DB ayrıca çakışma çözümü için kullanılacak diğer özel sayısal özelliği belirtmenize de olanak tanır.  
- Çakışmaların uygulama tanımlı semantik mutabakatiçin tasarlanmış uygulama **tanımlı (Özel) çakışma çözümilkesi** (birleştirme yordamları ile ifade edilir). Bu yordamlar, sunucu tarafında bir veritabanı işleminin himayesinde yazma-yazma çakışmaları algılandıktan sonra çağrılır. Sistem, taahhüt protokolünün bir parçası olarak birleştirme yordamının yürütülmesi için tam olarak bir kez garanti sağlar. Oynayabileceğiniz [birkaç çakışma çözümlüğü örneği](how-to-manage-conflicts.md) vardır.  

## <a name="consistency-models"></a>Tutarlılık Modelleri

Cosmos veritabanınızı ister tek veya birden çok yazma bölgesiyle yapılandırın, iyi tanımlanmış beş tutarlılık modeli arasından seçim yapabilirsiniz. Birden çok yazma bölgesinde, tutarlılık düzeylerinin birkaç önemli yönü şunlardır:  

Sınırlı bayatlık tutarlılığı, tüm okumaların en son yazının herhangi bir bölgesinde *K* önekleri veya *T* saniyeleri içinde olacağını garanti eder. Ayrıca, sınırlı bayatlık tutarlılığı ile okur monoton ve tutarlı önek garantileri ile garanti edilir. Anti-entropi protokolü oran sınırlı bir şekilde çalışır ve öneeklerin birikmemesini ve yazmaüzerindeki geri basıncın uygulanmasını gerektirmez. Oturum tutarlılığı, dünya çapında monoton okumayı, monoton yazmayı, kendi yazılarınızı okumayı, aşağıdakileri yazmayı ve tutarlı önek garantilerini garanti eder. Güçlü tutarlılıkla yapılandırılan veritabanları için, bölgeler arasında eşzamanlı çoğaltma nedeniyle birden çok yazma bölgesinin yararları (düşük yazma gecikmesi, yüksek yazma kullanılabilirliği) uygulanmaz.

Cosmos DB beş tutarlılık modelleri semantik [burada](consistency-levels.md)açıklanan ve matematiksel [olarak burada](https://github.com/Azure/azure-cosmos-tla)bir üst düzey TLA + özellikleri kullanılarak açıklanan .

## <a name="next-steps"></a>Sonraki adımlar

Ardından, aşağıdaki makaleleri kullanarak küresel dağıtımı nasıl yapılandırıştırılacayacaköğrenebilirsiniz:

* [Veritabanı hesabınızda bölge ekleme/çıkarma işlemi gerçekleştirme](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [Çoklu homing için istemcileri yapılandırmak için nasıl](how-to-manage-database-account.md#configure-multiple-write-regions)
* [Özel çakışma çözümlemesi ilkesi nasıl oluşturulur?](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)

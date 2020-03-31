---
title: En iyi uygulamalar - Azure Toplu İş
description: Azure Toplu İş çözümünüzü geliştirmek için en iyi uygulamaları ve yararlı ipuçlarını öğrenin.
author: LauraBrenner
ms.author: labrenne
ms.date: 11/22/2019
ms.service: batch
ms.topic: article
manager: evansma
ms.openlocfilehash: 16fb2786f180b1e28b76d9246d599a871278d00d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022741"
---
# <a name="azure-batch-best-practices"></a>Azure Toplu En iyi uygulamalar

Bu makalede, Azure Toplu İş hizmetini etkili ve verimli bir şekilde kullanmak için en iyi uygulamalar koleksiyonu anlatılmaktadır. Bu en iyi uygulamalar, Toplu Işla'daki deneyimlerimizden ve Toplu müşterilerin deneyimlerinden türetilmiştir. Bu tasarım tuzakları, potansiyel performans sorunları önlemek için bu makaleyi anlamak önemlidir ve anti-desenler için geliştirirken, ve kullanırken, Toplu.

Bu makalede, öğreneceksiniz:

> [!div class="checklist"]
> - En iyi uygulamalar nelerdir
> - Neden en iyi uygulamaları kullanmalısınız
> - En iyi uygulamaları takip etmezseniz ne olabilir?
> - En iyi uygulamaları takip etme

## <a name="pools"></a>Havuzlar

Toplu iş havuzları, Toplu İşlem hizmetindeki işleri yürütmek için kullanılan işlem kaynaklarıdır. Aşağıdaki bölümler, Toplu Iş havuzları ile çalışırken izleyerek en iyi uygulamalar hakkında rehberlik sağlar.

### <a name="pool-configuration-and-naming"></a>Havuz yapılandırması ve adlandırma

- **Havuz ayırma modu**  
    Toplu Iş hesabı oluştururken, iki havuz ayırma modu arasında seçim yapabilirsiniz: **Toplu iş hizmeti** veya kullanıcı **aboneliği.** Çoğu durumda, toplu iş tarafından yönetilen aboneliklerde havuzların arka planda tahsis edildiği varsayılan Toplu İşlem hizmeti modunu kullanmanız gerekir. Alternatif Kullanıcı aboneliği modunda, bir havuz oluşturulduğunda Batch VM'leri ve diğer kaynaklar doğrudan aboneliğinizde oluşturulur. Kullanıcı abonelik hesapları öncelikle önemli, ancak küçük bir senaryo alt kümesini etkinleştirmek için kullanılır. [Kullanıcı abonelik modu için Ek yapılandırmada](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode)kullanıcı abonelik modu hakkında daha fazla bilgi edinebilirsiniz.

- **Havuz eşleme işi belirlerken iş ve görev çalışma süresini göz önünde bulundurun.**  
    Öncelikle kısa süren görevlerden oluşan işleriniz varsa ve beklenen toplam görev sayıları küçükse, böylece işin genel beklenen çalışma süresi uzun değilse, her iş için yeni bir havuz ayırmayın. Düğümlerin ayırma süresi, işin çalışma süresini azaltır.

- **Havuzlarda birden fazla işlem düğümü olmalıdır.**  
    Tek tek düğümlerin her zaman kullanılabilir olacağı garanti edilmez. Yaygın olmasa da, donanım hataları, işletim sistemi güncelleştirmeleri ve diğer birçok sorun tek tek düğümlerin çevrimdışı olmasına neden olabilir. Toplu iş yükünüz deterministik, garantili ilerleme gerektiriyorsa, birden çok düğümiçeren havuzlar ayırmanız gerekir.

- **Kaynak adlarını yeniden kullanmayın.**  
    Toplu iş kaynakları (işler, havuzlar, vb.) genellikle zaman içinde gelir ve gider. Örneğin, Pazartesi günü bir havuz oluşturabilir, Salı günü silebilir ve perşembe günü başka bir havuz oluşturabilirsiniz. Oluşturduğunuz her yeni kaynağa daha önce kullanmadığınız benzersiz bir ad verilmelidir. Bu, bir GUID (tüm kaynak adı olarak veya bunun bir parçası olarak) kullanılarak veya kaynağın kaynak adına oluşturulduğu zamanı katıştırarak yapılabilir. Toplu [Iş,](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.jobspecification.displayname?view=azure-dotnet)gerçek kaynak kimliği o kadar da insan dostu olmayan bir şey olsa bile, bir kaynağa insan tarafından okunabilir bir ad vermek için kullanılabilen DisplayName'i destekler. Benzersiz adlar kullanmak, günlüklerde ve ölçümlerde hangi kaynağın bir şey yaptığını ayırt etmenizi kolaylaştırır. Ayrıca, bir kaynak için destek talebi dosyalamanız gerekirse belirsizliği de ortadan kaldırır.

- **Havuz bakımı ve arıza sırasında süreklilik.**  
    En iyisi, işlerinizi dinamik olarak havuzlar kullanmak tır. İşleriniz her şey için aynı havuzu kullanıyorsa, havuzda bir sorun olursa işinizin çalışmama ihtimali vardır. Bu, özellikle zamana duyarlı iş yükleri için önemlidir. Bunu düzeltmek için, her işi zamanlarken dinamik olarak bir havuz seçin veya oluşturun veya sağlıksız bir havuzu atlayabilmeniz için havuz adını geçersiz kılmanın bir yolu var.

- **Havuz bakımı ve arızası sırasında iş sürekliliği**  
    Bir havuzun istediğiniz boyuta kadar büyümesini engelleyebilecek iç hatalar, kapasite kısıtlamaları gibi birçok olası nedeni vardır. Bu nedenle, gerekirse farklı bir havuzda (muhtemelen farklı bir VM boyutunda - Toplu iş [Güncelleştirme İşi](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.protocol.joboperationsextensions.update?view=azure-dotnet)ile bunu destekler) işleri yeniden hedeflemeye hazır olmalısınız. Hiçbir zaman silinmeyeceği ve asla değişmeyeceği beklentisiyle statik bir havuz kimliği kullanmaktan kaçının.

### <a name="pool-lifetime-and-billing"></a>Havuz ömrü ve faturalandırma

Havuz ömrü ayırma yöntemine ve havuz yapılandırmasına uygulanan seçeneklere bağlı olarak değişebilir. Havuzlar, zaman içinde herhangi bir noktada rasgele bir kullanım ömrüne ve havuzda değişen sayıda işlem düğümüne sahip olabilir. Havuzdaki bilgi işlem düğümlerini açık bir şekilde veya hizmet tarafından sağlanan özellikler (otomatik ölçeklendirme veya otomatik havuz) aracılığıyla yönetmek sizin sorumluluğunuzdadır.

- **Havuzları taze tutun.**  
    En son düğüm aracısı güncelleştirmelerini ve hata düzeltmelerini aldığınızdan emin olmak için havuzlarınızı birkaç ayda bir sıfıra yeniden boyutlandırmanız gerekir. Havuzunuz yeniden oluşturulmadığı veya 0 bilgi işlem düğümüne yeniden boyutlandırılmadığı sürece düğüm aracısı güncelleştirmelerini almaz. Havuzunuzu yeniden oluşturmadan veya yeniden boyutlandırmadan önce, Düğümler bölümünde belirtildiği gibi hata ayıklama amacıyla düğüm aracısı günlüklerini [indirmeniz](#nodes) önerilir.

- **Havuz yeniden oluşturma**  
    Benzer bir kayda göre, havuzlarınızı günlük olarak silmek ve yeniden oluşturmak önerilmez. Bunun yerine, yeni bir havuz oluşturun, yeni havuza işaret etmek için varolan işlerinizi güncelleştirin. Tüm görevler yeni havuza taşındıktan sonra eski havuzu silin.

- **Havuz verimliliği ve faturalandırma**  
    Toplu iş kendisi ekstra ücrete tabi değildir, ancak kullanılan işlem kaynakları için ücrete tabi siniz. Ne durumda olursa olsun, havuzdaki her işlem düğümü için faturalandırılırsınız. Bu, düğümün çalışması için depolama ve ağ maliyetleri gibi gerekli ücretleri içerir. Daha fazla en iyi uygulamaları öğrenmek [için Azure Toplu İşi için Maliyet çözümlemesi ve bütçeler](budget.md)hakkında bilgi edinin.

### <a name="pool-allocation-failures"></a>Havuz ayırma hataları

Havuz ayırma hataları ilk ayırma veya sonraki yeniden boyutlandırmalar sırasında herhangi bir noktada meydana gelebilir. Bunun nedeni, bir bölgedeki geçici kapasite tükenmesi veya Toplu İşlem'in dayandığı diğer Azure hizmetlerindeki hatalar olabilir. Çekirdek kotanız bir garanti değil, bir sınırdır.

### <a name="unplanned-downtime"></a>Planlanmamış kapalı kalma süresi

Toplu iş havuzlarının Azure'da kapalı kalma süresi etkinliklerini deneyimleması mümkündür. Toplu Iş için senaryonuzu veya iş akışınızı planlarken ve geliştirirken bunu göz önünde bulundurmanız önemlidir.

Bir düğümün başarısız olması durumunda, Toplu İşlem otomatik olarak bu işlem düğümlerini sizin adınıza kurtarmaya çalışır. Bu, kurtarılan düğümdeki çalışan herhangi bir görevi yeniden zamanlamayı tetikleyebilir. Kesilen görevler hakkında daha fazla bilgi edinmek [için yeniden denemeler için Tasarım](#designing-for-retries-and-re-execution) bölümüne bakın.

- **Azure bölge bağımlılığı**  
    Zamana duyarlı veya üretim iş yükünüz varsa tek bir Azure bölgesine bağlı kalmaması önerilir. Nadir olmakla birlikte, tüm bölgeyi etkileyebilecek sorunlar vardır. Örneğin, işlemeişleminizin belirli bir zamanda başlaması gerekiyorsa, *başlangıç saatinizden çok önce*birincil bölgenizdeki havuzu ölçeklemeyi düşünün. Bu havuz ölçeği başarısız olursa, yedek bölgede (veya bölgelerde) bir havuz ölçeklendirme için geri düşebilirsiniz. Farklı bölgelerdeki birden çok hesaptaki havuzlar, başka bir havuzda bir sorun olursa hazır ve kolay erişilebilir bir yedekleme sağlar. Daha fazla bilgi için, [yüksek kullanılabilirlik için uygulamanızı tasarla'ya](high-availability-disaster-recovery.md)bakın.

## <a name="jobs"></a>İşler

İş, yüzlerce, binlerce, hatta milyonlarca görevi içerecek şekilde tasarlanmış bir kapsayıcıdır.

- **Bir işe birçok görev koyun**  
    Tek bir görevi çalıştırmak için bir iş kullanmak verimsizdir. Örneğin, her biri 10 görev içeren 100 iş oluşturmak yerine 1000 görev içeren tek bir işi kullanmak daha verimlidir. Her biri tek bir görev le 1000 iş çalıştırmak, alınması en az verimli, en yavaş ve en pahalı yaklaşım olacaktır.  

    Aynı anda binlerce etkin iş gerektiren bir Toplu Iş çözümü tasarlamayın. Görevler için kota yoktur, bu nedenle mümkün olduğunca az sayıda iş altında birçok görev yürütmek [işinizi ve iş zamanlama kotalarınızı](batch-quota-limit.md#resource-quotas)verimli bir şekilde kullanır.

- **İş ömrü**  
    Toplu iş, sistemden silinene kadar belirsiz bir ömrü vardır. Bir işin durumu, zamanlama için daha fazla görev kabul edip edemeyeceğini belirler. Bir iş, açıkça sonlandırılmadığı sürece otomatik olarak tamamlanmış duruma geçmez. Bu otomatik olarak [onAllTasksComplete](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.onalltaskscomplete?view=azure-dotnet) özelliği veya [maxWallClockTime](https://docs.microsoft.com/rest/api/batchservice/job/add#jobconstraints)üzerinden tetiklenebilir.

Varsayılan etkin [iş ve iş zamanlama kotası](batch-quota-limit.md#resource-quotas)vardır. Tamamlanan durumdaki iş ve iş zamanlamaları bu kotaya dahil değildir.

## <a name="tasks"></a>Görevler

Görevler, bir işi oluşturan tek tek çalışma birimleridir. Görevler kullanıcı tarafından gönderilir ve Toplu İşlem tarafından işlem düğümlerine zamanlanır. Görevleri oluştururken ve yürükarırken dikkat edilmesi gereken birkaç tasarım vardır. Aşağıdaki bölümlerde sık karşılaşılan senaryolar ve sorunları işlemek ve verimli bir şekilde gerçekleştirmek için görevlerinizi nasıl tasarlayacağınızı açıklayınız.

- **Görev verilerini görevin bir parçası olarak kaydedin.**  
    İşlem düğümleri doğalarına göre geçicidir. Toplu İşlem'de düğümlerin kaybolmasını kolaylaştıran otomatik havuz ve otomatik ölçeklendirme gibi birçok özellik vardır. Düğümler havuzdan ayrıldıklarında (yeniden boyutlandırma veya havuz silme nedeniyle) bu düğümler üzerindeki tüm dosyalar da silinir. Bu nedenle, görev tamamlanmadan önce çıktısını çalıştığı düğümden çıkarıp dayanıklı bir depoya taşıması önerilir, benzer şekilde bir görev başarısız olursa, başarısız olan bir depoya hata yığmak için gerekli günlükleri taşıması gerekir. Toplu Iş, [OutputFiles](batch-task-output-files.md)aracılığıyla veri yüklemek için Azure Depolama'yı ve çeşitli paylaşılan dosya sistemlerini entegre etti veya yüklemeyi görevlerinizde kendiniz gerçekleştirebilirsiniz.

### <a name="task-lifetime"></a>Görev ömrü

- **Görevleri tamamlandığında silin.**  
    Görevleri artık gerekmediğinde silin veya [bir bekletmeZamanı](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskconstraints.retentiontime?view=azure-dotnet) görev kısıtlaması ayarlayın. A `retentionTime` ayarlanırsa, Toplu `retentionTime` İşlem, görevin süresi dolduğunda görev tarafından kullanılan disk alanını otomatik olarak temizler.  

    Görevleri silerken iki şey gerçekleştirir. İş yerinde bir bir dizi görev inmemesini sağlayarak, ilgilendiğiniz görevi sorgulamayı/bulmayı zorlaştırır (çünkü Tamamlanan görevlere filtre uygulamanız gerekir). Ayrıca düğüm üzerinde karşılık gelen görev verilerini temizler (sağlanan `retentionTime` zaten isabet değil). Bu, düğümlerinizin görev verileriyle dolmamasını ve disk alanının tükenmesini sağlar.

### <a name="task-submission"></a>Görev teslimi

- **Koleksiyonda çok sayıda görev gönderin.**  
    Görevler bireysel olarak veya koleksiyonlarda gönderilebilir. Genel yükü ve teslim süresini azaltmak için görevlerin toplu olarak gönderilmesini yaparken bir seferde en fazla 100'e kadar [koleksiyonda](https://docs.microsoft.com/rest/api/batchservice/task/addcollection) görev gönderin.

### <a name="task-execution"></a>Görev yürütme

- **Düğüm başına maksimum görevlerinizi seçme**  
    Toplu iş, düğümlere fazla abone olan görevleri destekler (düğüm çekirdeklerinden daha fazla görev çalıştırMa). Görevlerinizin havuzdaki düğümlere "uyduğunu" sağlamak size kalmış. Örneğin, her biri %25 CPU kullanımını tek bir düğüme (havuzda) tüketen sekiz görev zamanlamaya `maxTasksPerNode = 8`çalışırsanız, bozulmuş bir deneyiminiz olabilir.

### <a name="designing-for-retries-and-re-execution"></a>Yeniden denemeler ve yeniden yürütme için tasarım

Görevler Toplu İşlem tarafından otomatik olarak yeniden denenebilir. İki tür yeniden deneme vardır: kullanıcı denetimi ve dahili. Kullanıcı kontrollü yeniden denemeler görevin [maxTaskRetryCount](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskconstraints.maxtaskretrycount?view=azure-dotnet)tarafından belirtilir. Görevde belirtilen bir program sıfır olmayan bir çıkış koduyla çıktığında, görev `maxTaskRetryCount`.

Nadir olsa da, bir görev, işlem düğümündeki hatalar nedeniyle dahili durumu güncelleştirememe veya görev çalışırken düğümdeki bir hata nedeniyle dahili olarak yeniden denenebilir. Görev, görevden vazgeçmeden ve görevi büyük ölçüde farklı bir işlem düğümünde Toplu Iş olarak yeniden zamanlanacak şekilde erteleyerek önce, mümkünse aynı işlem düğümünde yeniden denenecektir.

- **Dayanıklı görevler oluşturun**  
    Görevler, hataya dayanacak ve yeniden denemeye dayanacak şekilde tasarlanmalıdır. Bu, özellikle uzun süren görevler için önemlidir. Bunu yapmak için, görevlerin birden fazla kez çalıştırılsalar bile aynı, tek sonucu oluşturduğundan emin olun. Bunu başarmak için bir yolu görevlerinizi "hedef arayan" yapmaktır. Başka bir yolu, görevlerinizin iktidarlı olduğundan emin olmaktır (görevler kaç kez çalıştırılırlarsa çalıştırılırlarsa çalıştırSınlar aynı sonuca sahip olacaktır).

    Sık karşılaşılan bir örnek, dosyaları bir işlem düğümüne kopyalama görevidir. Basit bir yaklaşım, her çalıştığında belirtilen tüm dosyaları kopyalayan ve verimsiz olan ve hataya dayanacak şekilde oluşturulmayan bir görevdir. Bunun yerine, dosyaların işlem düğümünde olduğundan emin olmak için bir görev oluşturun; zaten mevcut olan dosyaları yeniden kopyalamayan bir görev. Bu şekilde, görev kesintiye uğradığında kaldığı yerden devam eder.

- **Düşük öncelikli düğümler**  
    Görevlerinizi özel veya düşük öncelikli düğümlerde yürütürken tasarım farkı yoktur. Bir görev, düşük öncelikli bir düğümüzerinde çalışırken veya özel bir düğümdeki bir hata nedeniyle kesintiye uğrarken, her iki durum da görevin hataya dayanacak şekilde tasarlanmasıyla azaltılır.

- **Görev yürütme süresi**  
    Kısa yürütme süresi ile görevlerden kaçının. Yalnızca bir ila iki saniye çalışan görevler ideal değildir. Tek bir görevde önemli miktarda iş yapmayı denemelisiniz (saat veya gün sayısına kadar 10 saniyelik minimum). Her görev bir dakika (veya daha fazla) için yürütüliyorsa, genel bilgi işlem süresinin bir bölümü olarak zamanlama yükü küçüktür.

## <a name="nodes"></a>Düğümler

- **Başlangıç görevleri idempotent olmalıdır**  
    Diğer görevlere benzer şekilde, düğüm başlangıç görevi de her düğüm önyüklemesi her seferinde yeniden çalıştırılacak gibi idempotent olmalıdır. Bir idempotent görev sadece birden çok kez çalıştırıldığında tutarlı bir sonuç üreten biridir.

- **İşletim sistemi hizmetleri arabirimi aracılığıyla uzun süren hizmetleri yönetin.**  
    Bazen düğümden veri toplamak ve bildirmek için düğümdeki Toplu İşlem aracısının yanında başka bir aracı çalıştırmaya ihtiyaç vardır, örneğin. Bu aracıların Windows hizmeti veya Linux `systemd` hizmeti gibi işletim sistemi hizmetleri olarak dağıtılmasını öneririz.  

    Bu hizmetleri çalıştırırken, düğümdeki Toplu iş dizilişlerinde dosya kilitlerini almamalıdır, çünkü aksi takdirde Toplu dosya kilitleri nedeniyle bu dizinleri silemez. Örneğin, bir Windows hizmetini başlangıç görevinde yüklüyorsanız, hizmeti doğrudan başlangıç görevi çalışma dizininden başlatmak yerine, dosyaları başka bir yerden kopyalayın (dosyalar varsa kopyayı atlayın). Hizmeti bu konumdan yükleyin. Toplu İşlem başlangıç görevinizi yeniden çalıştırdığında, başlangıç görevi çalışma dizinini siler ve yeniden oluşturur. Hizmetin başlangıç görevi çalışma dizininde değil, diğer dizinde dosya kilitleri olduğundan bu çalışır.

- **Windows'da dizin bağlantıları oluşturmaktan kaçının**  
    Dizin kavşakları, bazen dizin sabit bağlantılar denir, görev ve iş temizleme sırasında başa çıkmak zordur. Sabit bağlantılar yerine symlinks (yumuşak bağlantılar) kullanın.

- **Bir sorun varsa Toplu İşlem aracı günlüklerini toplama**  
    Düğüm üzerinde çalışan bir düğümün veya görevlerin davranışını içeren bir sorun fark ederseniz, söz konusu düğümleri ayırmadan önce Toplu Iş aracıgünlüklerini toplamanız önerilir. Toplu İşlem günlükleri, Yükleme Toplu Hizmet günlükleri API'si kullanılarak toplanabilir. Bu günlükler Microsoft'a bir destek biletinin parçası olarak sağlanabilir ve sorun giderme ve çözümleme konusunda yardımcı olur.

## <a name="security"></a>Güvenlik

### <a name="security-isolation"></a>Güvenlik yalıtımı

İzolasyon amacıyla, senaryonuz işleri birbirinden yalıtmak gerektiriyorsa, bu işleri ayrı havuzlarda geçirerek yalıtmalısınız. Havuz, Toplu İşlem'deki güvenlik yalıtım sınırıdır ve varsayılan olarak iki havuz görünür değildir veya birbiriyle iletişim kuramaz. Yalıtım aracı olarak ayrı Toplu Iş hesapları kullanmaktan kaçının.

## <a name="moving"></a>Hareketli

### <a name="move-batch-account-across-regions"></a>Toplu İşlem hesabını bölgeler arasında taşıma 

Varolan Toplu İşlem hesabınızı bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, olağanüstü durum kurtarma planlamasının bir parçası olarak başka bir bölgeye geçmek isteyebilirsiniz.

Azure Toplu İş hesapları bir bölgeden diğerine taşınamaz. Ancak, Toplu İş hesabınızın varolan yapılandırmasını dışa aktarmak için bir Azure Kaynak Yöneticisi şablonu kullanabilirsiniz.  Daha sonra Toplu İşlem hesabını şablona dışlayarak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve ardından şablonu yeni bölgeye dağıtarak kaynağı başka bir bölgeye ayarlayabilirsiniz. Şablonu yeni bölgeye yükledikten sonra sertifikaları, iş zamançizelgelerini ve uygulama paketlerini yeniden oluşturmanız gerekir. Değişiklikleri işlemek ve Toplu İşlem hesabının hareketini tamamlamak için, orijinal Toplu Iş hesabı veya kaynak grubunu silmeyi unutmayın.  

Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için [Bkz. Hızlı Başlangıç: Azure portalını kullanarak Azure Kaynak Yöneticisi şablonları oluşturun ve dağıtın.](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal)



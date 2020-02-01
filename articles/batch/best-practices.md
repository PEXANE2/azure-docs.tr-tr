---
title: En iyi yöntemler-Azure Batch
description: Azure Batch çözümünüzü geliştirmeye yönelik en iyi yöntemleri ve yararlı ipuçlarını öğrenin.
author: ju-shim
ms.author: jushiman
ms.date: 11/22/2019
ms.service: batch
ms.topic: article
manager: gwallace
ms.openlocfilehash: c2acd09df51b942a08a85d96d907e064367377a7
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76900286"
---
# <a name="azure-batch-best-practices"></a>En iyi Azure Batch uygulamalar

Bu makalede, Azure Batch hizmetini etkili ve verimli bir şekilde kullanmaya yönelik en iyi yöntemler koleksiyonu ele alınmaktadır. Bu en iyi yöntemler Batch ve Batch müşterilerinin deneyimleri ile deneyimimizden türetilir. , Toplu Iş için geliştirme ve kullanma sırasında tasarım engellerini, olası performans sorunlarını ve daha fazla Işlem desenlerini önlemek için bu makalenin anlaşılması önemlidir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> - En iyi uygulamalar nelerdir?
> - Neden en iyi uygulamaları kullanmalısınız?
> - En iyi yöntemleri takip edemezseniz ne olur?
> - En iyi yöntemleri takip etme

## <a name="pools"></a>Havuzlar

Batch havuzları, Batch hizmetinde işlerin yürütülmesi için işlem kaynaklarıdır. Aşağıdaki bölümlerde, toplu Iş havuzlarıyla çalışırken izlenecek en iyi uygulamalar hakkında rehberlik sağlanmaktadır.

### <a name="pool-configuration-and-naming"></a>Havuz yapılandırması ve adlandırma

- **Havuz ayırma modu**  
    Batch hesabı oluştururken iki havuz ayırma modu arasından seçim yapabilirsiniz: **Batch hizmeti** veya **Kullanıcı aboneliği**. Çoğu durumda, toplu yönetilen aboneliklerde havuzların arkasında ayrıldığı varsayılan Batch hizmeti modunu kullanmanız gerekir. Alternatif Kullanıcı aboneliği modunda, bir havuz oluşturulduğunda Batch VM'leri ve diğer kaynaklar doğrudan aboneliğinizde oluşturulur. Kullanıcı aboneliği hesapları, önemli, ancak küçük bir senaryo alt kümesini etkinleştirmek için öncelikli olarak kullanılır. Kullanıcı aboneliği modu hakkında daha fazla bilgi için kullanıcı aboneliği [modu Için ek yapılandırma](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode)makalesini okuyun.

- **İş ve görev çalışma süresini, havuzdan eşleme işi belirlenirken göz önünde bulundurun.**  
    Öncelikle kısa süreli görevlerden oluşan işleriniz varsa ve beklenen toplam görev sayısı küçük olduğundan, işin genel beklenen çalışma zamanının uzun olmaması ve her iş için yeni bir havuz ayrılmaması gerekir. Düğümlerin ayırma süresi, işin çalışma süresini azaledecektir.

- **Havuzlar birden çok işlem düğümüne sahip olmalıdır.**  
    Bağımsız düğümlerin her zaman kullanılabilir olması garanti edilmez. Yaygın olarak, donanım hataları, işletim sistemi güncelleştirmeleri ve diğer sorunların bir konağı, tek tek düğümlerin çevrimdışı olmasına neden olabilir. Batch iş yükünüz belirleyici gerektiriyorsa, garantili ilerleme durumunda birden çok düğüm içeren havuzlar ayırmanız gerekir.

- **Kaynak adlarını yeniden kullanmayın.**  
    Batch kaynakları (işler, havuzlar vb.) genellikle zaman içinde gelir ve zaman içinde gider. Örneğin, Pazartesi günü bir havuz oluşturabilir, Salı günü silebilir ve ardından Perşembe üzerinde başka bir havuz oluşturabilirsiniz. Oluşturduğunuz her yeni kaynağa, daha önce kullanmadığınız benzersiz bir ad verilmelidir. Bu, bir GUID kullanılarak (tüm kaynak adı olarak veya bunun bir parçası olarak) ya da kaynağın kaynak adında oluşturulduğu zamanı katıştırarak yapılabilir. Batch, gerçek kaynak KIMLIĞI insana sahip olmayan bir şey olsa da kaynağa okunabilir bir ad vermek için kullanılabilen [DisplayName](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.jobspecification.displayname?view=azure-dotnet)'i destekler. Benzersiz adların kullanılması, hangi belirli kaynağın günlüklerde ve ölçümlerde bir şeyler olduğunu ayırt etmenize daha kolay hale getirir. Ayrıca, bir kaynak için bir destek durumu dosyası oluşturmanız gerekiyorsa belirsizlik kaldırılır.

- **Havuz bakımı ve başarısızlık sırasında süreklilik.**  
    İşlerinizin havuzları dinamik olarak kullanması en iyisidir. İşleriniz her şey için aynı havuzu kullanıyorsa, havuzda bir sorun oluşursa işlerin çalıştırılmayabileceği bir şansınız vardır. Bu, özellikle zamana duyarlı iş yükleri için önemlidir. Bunu çözmek için, her bir işi zamanlarken bir havuzu dinamik olarak seçin veya oluşturun veya uygun olmayan bir havuzu atlayabilmeniz için havuz adını geçersiz kılmak üzere bir yol belirtin.

- **Havuz bakımı ve başarısızlığı sırasında iş sürekliliği**  
    Bir havuzun, dahili hatalar, kapasite kısıtlamaları vb. gibi gerekli boyuta büyümesini engelleyebilen birçok nedeni vardır. Bu nedenle, işleri farklı bir havuzda yeniden hedeflemeniz (muhtemelen farklı bir VM boyutu ile) gerekirse bu Işlemi [Updatejob](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.protocol.joboperationsextensions.update?view=azure-dotnet)aracılığıyla destekler. Bir statik havuz KIMLIĞI kullanmaktan kaçının hiçbir şekilde silinmeyeceğinden ve hiçbir şekilde değişmeyeceğinden, beklenmez.

### <a name="pool-lifetime-and-billing"></a>Havuz ömrü ve faturalama

Havuz ömrü, havuz yapılandırmasına uygulanan ayırma ve seçenekler yöntemine bağlı olarak farklılık gösterebilir. Havuzlar, zaman içinde herhangi bir zamanda, havuzda rastgele bir yaşam süresine ve farklı sayıda işlem düğümüne sahip olabilir. Havuzdaki işlem düğümlerini açıkça veya hizmet tarafından sunulan özellikler aracılığıyla (otomatik ölçeklendirme veya otomatik havuz) yönetmek sizin sorumluluğunuzdadır.

- **Havuzları güncel tutun.**  
    En son düğüm Aracısı güncelleştirmelerini ve hata düzeltmelerini aldığınızdan emin olmak için havuzlarınızı her birkaç ayda bir sıfır olacak şekilde yeniden boyutlandırmalısınız. Havuzunuz, yeniden oluşturulup 0 işlem düğümlerine yeniden boyutlandırılana kadar düğüm Aracısı güncelleştirmeleri almaz. Havuzunuzu yeniden oluşturmadan veya yeniden boyutlandırabilmeniz için, [düğümler](#nodes) bölümünde anlatıldığı gibi, hata ayıklama amacıyla herhangi bir düğüm Aracısı günlüğünü indirmeniz önerilir.

- **Havuz yeniden oluşturma**  
    Benzer bir notta havuzlarınızın günlük olarak silinmesi ve yeniden oluşturulması önerilmez. Bunun yerine, yeni bir havuz oluşturun, mevcut işlerinizi yeni havuza işaret etmek üzere güncelleştirin. Tüm görevler yeni havuza taşındıktan sonra eski havuzu silin.

- **Havuz verimliliği ve faturalama**  
    Batch 'in kendisi ek ücret vermez, ancak kullanılan işlem kaynakları için ücret ödemeniz gerekir. İçindeki durum ne olursa olsun, havuzdaki her işlem düğümü için faturalandırılırsınız. Bu, düğüm için gereken depolama ve ağ maliyetleri gibi tüm ücretleri içerir. En iyi uygulamalar hakkında daha fazla bilgi edinmek için bkz. [Azure Batch Için maliyet analizi ve bütçeleri](budget.md).

### <a name="pool-allocation-failures"></a>Havuz ayırma sorunları

Havuz ayırma arızaları, ilk ayırma veya sonraki yeniden boyutlandırmalarda herhangi bir noktada gerçekleşebilir. Bunun nedeni, bir bölgedeki geçici kapasiteden veya toplu Işin bağımlı olduğu diğer Azure hizmetlerindeki hatalardan kaynaklanabilir. Çekirdek kotanızın garantisi, ancak bunun yerine bir sınır değildir.

### <a name="unplanned-downtime"></a>Planlanmamış kapalı kalma süresi

Toplu Iş havuzlarının Azure 'da kesinti süresi olaylarıyla karşılaşması mümkündür. Bu, senaryonuzu veya Batch için iş akışınızı planlarken ve geliştirirken göz önünde bulundurmanız önemlidir.

Bir düğümün başarısız olması durumunda, Batch otomatik olarak bu işlem düğümlerini sizin adınıza kurtarmaya çalışır. Bu, kurtarılan düğüm üzerinde çalışan herhangi bir görevin yeniden çizelgelenmesi tetiklenebilir. Kesilen görevler hakkında daha fazla bilgi edinmek için bkz. [yeniden denemeler tasarlama](#designing-for-retries-and-re-execution) .

- **Azure bölge bağımlılığı**  
    Zamana duyarlı veya üretim iş yükünüz varsa, tek bir Azure bölgesine bağlı olmaması önerilir. Nadir olarak, bir bölgenin tamamını etkileyebilecek sorunlar vardır. Örneğin, işlemelerinizin belirli bir zamanda başlaması gerekiyorsa, birincil bölgenizdeki havuzun ölçeğini *başlangıç zamanından önce da*ölçeklendirin. Havuz ölçeği başarısız olursa, bir yedekleme bölgesindeki (veya bölgelerde) bir havuzu ölçeklendirmeye geri dönebilirsiniz. Farklı bölgelerdeki birden çok hesap genelinde havuzlar, başka bir havuz ile yanlış bir sorun varsa, daha kolay erişilebilir bir yedekleme sağlar. Daha fazla bilgi için bkz. [uygulamanızı yüksek kullanılabilirlik Için tasarlama](high-availability-disaster-recovery.md).

## <a name="jobs"></a>İş

İş, yüzlerce, binlerce veya hatta milyonlarca görevi içerecek şekilde tasarlanan bir kapsayıcıdır.

- **Bir işe birçok görev yerleştirme**  
    Tek bir görevi çalıştırmak için bir iş kullanmak verimsiz değildir. Örneğin, her biri 10 görev içeren 100 iş oluşturmak yerine 1000 görevi içeren tek bir işi kullanmak daha etkilidir. Her biri tek bir görev içeren 1000 iş çalıştırmak, en az verimli, en yavaş ve en pahalı yaklaşım olacaktır.  

    Aynı anda binlerce etkin işi gerektiren bir Batch çözümü tasarlamayın. Görevler için kota yoktur, bu nedenle çok sayıda iş altında çok sayıda görev yürütülerek [iş ve iş zamanlama kotalarınızı](batch-quota-limit.md#resource-quotas)verimli bir şekilde kullanır.

- **İş ömrü**  
    Toplu işin, sistemden silinene kadar sınırsız bir ömrü vardır. Bir işin durumu, zamanlama için daha fazla görevi kabul edip edemeyeceğini belirler. Açık olarak sonlandırılmadığı takdirde bir iş otomatik olarak tamamlandı durumuna taşınamaz. Bu, [Onalltasksall](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.onalltaskscomplete?view=azure-dotnet) özelliği veya [Maxduvara Clocktime](https://docs.microsoft.com/rest/api/batchservice/job/add#jobconstraints)aracılığıyla otomatik olarak tetiklenebilir.

Varsayılan bir [etkin iş ve iş zamanlaması kotası](batch-quota-limit.md#resource-quotas)vardır. İşlerin ve iş zamanlamalarının tamamlandı durumunda bu kotaya dahil sayılmaz.

## <a name="tasks"></a>Görevler

Görevler, bir işi oluşturan bireysel iş birimleridir. Görevler kullanıcı tarafından gönderilir ve işlem düğümleri için toplu Iş tarafından zamanlanır. Görevleri oluştururken ve yürütürken yapmanız gereken çeşitli tasarım konuları vardır. Aşağıdaki bölümlerde, sorunları ele almak ve verimli bir şekilde gerçekleştirmek için genel senaryolar ve görevlerinizi tasarlamak açıklanmaktadır.

- **Görev verilerini görevin bir parçası olarak kaydedin.**  
    İşlem düğümleri doğası gereği daha kısa. Yığın içinde otomatik havuz ve otomatik ölçeklendirme gibi birçok özellik vardır ve bu da düğümlerin kaybolmasını kolaylaştırır. Düğümler havuzdan ayrıldığında (bir yeniden boyutlandırma veya havuz silme nedeniyle), bu düğümlerdeki tüm dosyalar da silinir. Bu nedenle, bir görev tamamlanmadan önce, bir görev başarısız olduğunda, bir görevin başarısız olması durumunda, bir görevin arızalandığı düğümü dayanıklı bir depoya taşımalıdır. Toplu işlem, Azure depolama 'yı kullanarak verileri [OutputFiles](batch-task-output-files.md)aracılığıyla karşıya yüklemeyi, ayrıca çeşitli paylaşılan dosya sistemlerini de veya kendi görevlerinizde karşıya yüklemeyi gerçekleştirmek için tümleşik destek içerir.

### <a name="task-lifetime"></a>Görev ömrü

- **Görevleri tamamlandığında silin.**  
    Görevleri artık gerekli olmadığında silin veya bir [retentionTime](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskconstraints.retentiontime?view=azure-dotnet) görev kısıtlaması ayarlayın. Bir `retentionTime` ayarlandıysa, toplu Işlem, `retentionTime` süresi dolarsa görev tarafından kullanılan disk alanını otomatik olarak temizler.  

    Görevleri silme işlemi iki şeyi gerçekleştirir. Bu, çalıştığınız görevi daha zor bir şekilde sorgulamayı/bulmayı (tamamlanan görevleri filtrelemeniz gerekecektir) sağlayan, iş içinde görev oluşturma işlemi yapılmasını sağlar. Ayrıca, düğümdeki karşılık gelen görev verilerini de temizler (belirtilen `retentionTime` zaten isabet aldıysa). Bu, düğümlerinizin görev verileriyle doldurulmamasını ve disk alanı tükenmesini sağlar.

### <a name="task-submission"></a>Görev gönderme

- **Bir koleksiyonda çok sayıda görev gönderebilirsiniz.**  
    Görevler ayrı ayrı veya koleksiyonlarda gönderilebilir. Ek yükü ve gönderim süresini azaltmak için görevler toplu gönderimi yaparken bir seferde en fazla 100 olan [koleksiyonlara](https://docs.microsoft.com/rest/api/batchservice/task/addcollection) görev gönderebilirsiniz.

### <a name="task-execution"></a>Görev yürütme

- **Düğüm başına en fazla görevlerinizi seçme**  
    Batch, düğümlerde fazla abone olan görevleri destekler (bir düğümden daha fazla görev çalıştırmak çekirdekler vardır). Görevlerinizin, havuzunuzdaki düğümlere "sığması" durumunda olduğundan emin olmanız gerekir. Örneğin, her birinin bir düğümde (`maxTasksPerNode = 8`sahip bir havuzda) %25 CPU kullanımı tükettiği sekiz görevi zamanlamaya çalışırsanız, düzeyi düşürülmüş bir deneyimle karşılaşabilirsiniz.

### <a name="designing-for-retries-and-re-execution"></a>Yeniden denemeler ve yeniden yürütme için tasarlama

Görevler, toplu Işlem tarafından otomatik olarak yeniden denenebilir. İki tür yeniden deneme vardır: Kullanıcı denetimli ve dahili. Kullanıcı denetimli yeniden denemeler, görevin [Maxtaskretrycount](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskconstraints.maxtaskretrycount?view=azure-dotnet)tarafından belirtilir. Görevde belirtilen bir program sıfır dışında bir çıkış kodu ile çıktığında, görev `maxTaskRetryCount`değerine kadar yeniden denenir.

Nadir olarak bir görev, işlem düğümündeki hatalar nedeniyle, görev çalışırken düğüm üzerinde iç durumu veya bir başarısızlığı güncelleştirmeme gibi yeniden deneniyor olabilir. Görev, mümkünse aynı işlem düğümünde yeniden denenecektir, bu, görevde bir süre önce bir iç sınıra kadar ve bir toplu Işlem tarafından, potansiyel olarak farklı bir işlem düğümünde yeniden zamanlanmasını ertelenir.

- **Dayanıklı Görevler oluşturun**  
    Görevler hatalı ve yeniden denemeye yetecek şekilde tasarlanmalıdır. Bu, özellikle uzun süre çalışan görevler için önemlidir. Bunu yapmak için, görevlerin aynı anda birden çok kez çalıştırılsa bile aynı, tek bir sonuç oluşturmasını sağlayın. Bunu başarmanın bir yolu, görevlerinizi "hedef arama" yapmak. Diğer bir yol da görevlerinizin ıdempotent olduğundan emin olmak (görevler kaç kez çalıştırıldıklarından bağımsız olarak aynı sonuca sahip olacaktır).

    Ortak bir örnek, dosyaları bir işlem düğümüne kopyalamak için kullanılan bir görevdir. Basit yaklaşım, her çalıştığında belirtilen her dosyayı kopyalayan, verimsiz olan ve geldiğinde hatası için oluşturulmamış bir görevdir. Bunun yerine, dosyaların işlem düğümünde olduğundan emin olmak için bir görev oluşturun; zaten mevcut olan dosyaları yeniden kopyalamamış bir görev. Bu şekilde, görev kesintiye uğratıldığında, görev ayrıldıktan sonra açılır.

- **Düşük öncelikli düğümler**  
    Görevlerinizi adanmış veya düşük öncelikli düğümlerde yürütürken hiçbir tasarım farkı yoktur. Bir görevin düşük öncelikli bir düğümde çalışırken geçersiz hale gelmiş veya ayrılmış bir düğümdeki bir hata nedeniyle kesintiye uğratıldığı, her iki durum da görevi withfailure hatası tasarlayarak azaltılmıştır.

- **Görev yürütme süresi**  
    Kısa yürütme süresine sahip görevlerden kaçının. Yalnızca bir ile iki saniye çalışan görevler ideal değildir. Tek bir görevde önemli miktarda iş yapmayı denemelisiniz (en az 10 saniye, saat veya güne kadar). Her görev bir dakika (veya daha fazla) boyunca yürütülüyordur, genel işlem zamanının bir bölümü olarak zamanlama yükü küçüktür.

## <a name="nodes"></a>Düğümler

- **Başlangıç görevleri ıdempotent olmalıdır**  
    Diğer görevlere benzer şekilde, düğüm başlangıç görevi, düğüm her önyüklendiğinde yeniden çalıştırılacak şekilde ıdempotent olmalıdır. Bir ıdempotent görevi, birden çok kez çalıştırıldığında tutarlı bir sonuç üreten bir görevdir.

- **İşletim sistemi Hizmetleri arabirimi aracılığıyla uzun süre çalışan hizmetleri yönetin.**  
    Bazen, düğümdeki verileri toplamak ve rapor etmek için, örneğin, düğümündeki Batch aracısının yanı sıra başka bir aracı da çalıştırmanız gerekir. Bu aracıların Windows hizmeti veya Linux `systemd` hizmeti gibi işletim sistemi hizmetleri olarak dağıtılmasını öneririz.  

    Bu Hizmetleri çalıştırırken, düğüm üzerindeki toplu yönetilen dizinlerde bulunan dosyalarda dosya kilitleri almalıdır, aksi takdirde toplu Işlem dosya kilitleri nedeniyle bu dizinlerin silinmesine neden olur. Örneğin, başlangıç görevinde bir Windows hizmeti yüklüyorsanız, hizmeti doğrudan başlangıç görevi çalışma dizininden başlatmak yerine, dosyaları başka bir yere kopyalayın (dosyalar varsa yalnızca kopyayı atlayın). Hizmeti bu konumdan yükler. Batch, başlangıç görevinizi yeniden çalıştırdığınızda, başlangıç görevi çalışma dizinini siler ve yeniden oluşturur. Bu, hizmetin başlangıç görevi çalışma dizini değil diğer dizindeki dosya kilitleri olduğu için geçerlidir.

- **Windows 'da Dizin geçişlerini oluşturmaktan kaçının**  
    Bazen Dizin sabit bağlantıları olarak adlandırılan dizin junler, görev ve iş temizliği sırasında uğraşmak zordur. Sabit bağlantılar yerine çözümlemeyin (geçici bağlantılar) kullanın.

- **Bir sorun varsa Batch Aracısı günlüklerini toplayın**  
    Düğüm üzerinde çalışan bir düğümün veya görevlerin davranışını içeren bir sorun fark ederseniz, söz konusu düğümlerin ayırmayı kaldırma işleminden önce Batch Aracısı günlüklerinin toplanması önerilir. Batch Aracısı günlükleri, Batch Hizmeti günlüklerini karşıya yükle API 'SI kullanılarak toplanabilir. Bu Günlükler, Microsoft 'a destek bileti kapsamında sağlanabilir ve sorun giderme ve çözümleme konularında yardımcı olur.

## <a name="security"></a>Güvenlik

### <a name="security-isolation"></a>Güvenlik yalıtımı

Yalıtım amaçları doğrultusunda, senaryonuzun işlerin yalıtılması gerekiyorsa, bu işleri ayrı havuzlarda bulundurarak yalıtabilirsiniz. Havuz, toplu Işteki güvenlik yalıtımı sınırıdır ve varsayılan olarak, iki havuz görünür değildir veya birbirleriyle iletişim kuramaz. Yalıtım yöntemi olarak ayrı Batch hesapları kullanmaktan kaçının.

## <a name="moving"></a>Taşınacağı

### <a name="move-batch-account-across-regions"></a>Batch hesabını bölgeler arasında taşıma 

Mevcut Batch hesabınızı bir bölgeden diğerine taşımak istediğiniz çeşitli senaryolar vardır. Örneğin, olağanüstü durum kurtarma planlamasının bir parçası olarak başka bir bölgeye geçmek isteyebilirsiniz.

Azure Batch hesaplar bir bölgeden diğerine taşınamaz. Ancak, Batch hesabınızın mevcut yapılandırmasını dışarı aktarmak için bir Azure Resource Manager şablonu kullanabilirsiniz.  Daha sonra, Batch hesabını bir şablona dışarı aktararak, parametreleri hedef bölgeyle eşleşecek şekilde değiştirerek ve sonra şablonu yeni bölgeye dağıtabilmeniz için kaynağı başka bir bölgede da oluşturabilirsiniz. Şablonu yeni bölgeye yükledikten sonra sertifikaların, iş zamanlamalarının ve uygulama paketlerinin yeniden oluşturulması gerekir. Değişiklikleri uygulamak ve Batch hesabını taşımayı tamamlamak için, özgün Batch hesabını veya kaynak grubunu silmeyi unutmayın.  

Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure Portal kullanarak Azure Resource Manager şablonları oluşturma ve dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).



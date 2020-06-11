---
title: En iyi uygulamalar
description: Azure Batch çözümünüzü geliştirmeye yönelik en iyi yöntemleri ve yararlı ipuçlarını öğrenin.
ms.date: 05/22/2020
ms.topic: conceptual
ms.openlocfilehash: 1d482eeb8b3da94e8af0a597ade1a1d834ccf6a0
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84677790"
---
# <a name="azure-batch-best-practices"></a>En iyi Azure Batch uygulamalar

Bu makalede, toplu Iş ile gerçek yaşam deneyimine göre Azure Batch hizmetini etkili ve verimli bir şekilde kullanmaya yönelik en iyi yöntemler koleksiyonu ele alınmaktadır. Toplu Iş için geliştirme ve kullanma konularında tasarım ve kullanım alışkanlıkları, olası performans sorunları ve daha fazla bilgi için bu makaleyi okuyun.

## <a name="pools"></a>Havuzlar

[Havuzlar](nodes-and-pools.md#pools) , Batch hizmetinde işlerin yürütülmesi için işlem kaynaklarıdır. Aşağıdaki bölümler, Batch havuzlarıyla çalışma önerileri sağlar.

### <a name="pool-configuration-and-naming"></a>Havuz yapılandırması ve adlandırma

- **Havuz ayırma modu** Batch hesabı oluştururken iki havuz ayırma modu arasından seçim yapabilirsiniz: **Batch hizmeti** veya **Kullanıcı aboneliği**. Çoğu durumda, toplu yönetilen aboneliklerde havuzların arkasında ayrıldığı varsayılan Batch hizmeti modunu kullanmanız gerekir. Alternatif Kullanıcı aboneliği modunda, bir havuz oluşturulduğunda Batch VM'leri ve diğer kaynaklar doğrudan aboneliğinizde oluşturulur. Kullanıcı aboneliği hesapları, önemli, ancak küçük bir senaryo alt kümesini etkinleştirmek için öncelikli olarak kullanılır. Kullanıcı aboneliği modu hakkında daha fazla bilgi için kullanıcı aboneliği [modu Için ek yapılandırma](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode)makalesini okuyun.

- **İş ve görev çalışma süresini, havuzdan eşleme işi belirlenirken göz önünde bulundurun.**
    Öncelikle kısa süreli görevlerden oluşan işleriniz varsa ve beklenen toplam görev sayısı küçük olduğundan, işin genel beklenen çalışma zamanının uzun olmaması ve her iş için yeni bir havuz ayrılmaması gerekir. Düğümlerin ayırma süresi, işin çalışma süresini azaledecektir.

- **Havuzlar birden çok işlem düğümüne sahip olmalıdır.**
    Bağımsız düğümlerin her zaman kullanılabilir olması garanti edilmez. Yaygın olarak, donanım hataları, işletim sistemi güncelleştirmeleri ve diğer sorunların bir konağı, tek tek düğümlerin çevrimdışı olmasına neden olabilir. Batch iş yükünüz belirleyici gerektiriyorsa, garantili ilerleme durumunda birden çok düğüm içeren havuzlar ayırmanız gerekir.

- **Kaynak adlarını yeniden kullanmayın.**
    Batch kaynakları (işler, havuzlar vb.) genellikle zaman içinde gelir ve zaman içinde gider. Örneğin, Pazartesi günü bir havuz oluşturabilir, Salı günü silebilir ve ardından Perşembe üzerinde başka bir havuz oluşturabilirsiniz. Oluşturduğunuz her yeni kaynağa, daha önce kullanmadığınız benzersiz bir ad verilmelidir. Bu, bir GUID kullanılarak (tüm kaynak adı olarak veya bunun bir parçası olarak) ya da kaynağın kaynak adında oluşturulduğu zamanı katıştırarak yapılabilir. Batch, gerçek kaynak KIMLIĞI insana sahip olmayan bir şey olsa da kaynağa okunabilir bir ad vermek için kullanılabilen [DisplayName](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.jobspecification.displayname?view=azure-dotnet)'i destekler. Benzersiz adların kullanılması, hangi belirli kaynağın günlüklerde ve ölçümlerde bir şeyler olduğunu ayırt etmenize daha kolay hale getirir. Ayrıca, bir kaynak için bir destek durumu dosyası oluşturmanız gerekiyorsa belirsizlik kaldırılır.

- **Havuz bakımı ve başarısızlık sırasında süreklilik.**
    İşlerinizin havuzları dinamik olarak kullanması en iyisidir. İşleriniz her şey için aynı havuzu kullanıyorsa, havuzda bir sorun oluşursa işlerin çalıştırılmayabileceği bir şansınız vardır. Bu, özellikle zamana duyarlı iş yükleri için önemlidir. Bunu çözmek için, her bir işi zamanlarken bir havuzu dinamik olarak seçin veya oluşturun veya uygun olmayan bir havuzu atlayabilmeniz için havuz adını geçersiz kılmak üzere bir yol belirtin.

- **Havuz bakımı ve başarısızlığı sırasında iş sürekliliği** Bir havuzun, dahili hatalar, kapasite kısıtlamaları vb. gibi gerekli boyuta büyümesini engelleyebilen birçok nedeni vardır. Bu nedenle, işleri farklı bir havuzda yeniden hedeflemeniz (muhtemelen farklı bir VM boyutu ile) gerekirse bu Işlemi [Updatejob](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.protocol.joboperationsextensions.update?view=azure-dotnet)aracılığıyla destekler. Bir statik havuz KIMLIĞI kullanmaktan kaçının hiçbir şekilde silinmeyeceğinden ve hiçbir şekilde değişmeyeceğinden, beklenmez.

### <a name="pool-lifetime-and-billing"></a>Havuz ömrü ve faturalama

Havuz ömrü, havuz yapılandırmasına uygulanan ayırma ve seçenekler yöntemine bağlı olarak farklılık gösterebilir. Havuzlar, zaman içinde herhangi bir zamanda, havuzda rastgele bir yaşam süresine ve farklı sayıda işlem düğümüne sahip olabilir. Havuzdaki işlem düğümlerini açıkça veya hizmet tarafından sunulan özellikler aracılığıyla (otomatik ölçeklendirme veya otomatik havuz) yönetmek sizin sorumluluğunuzdadır.

- **Havuzları güncel tutun.**
    En son düğüm Aracısı güncelleştirmelerini ve hata düzeltmelerini aldığınızdan emin olmak için havuzlarınızı her birkaç ayda bir sıfır olacak şekilde yeniden boyutlandırmalısınız. Havuzunuz, yeniden oluşturulup 0 işlem düğümlerine yeniden boyutlandırılana kadar düğüm Aracısı güncelleştirmeleri almaz. Havuzunuzu yeniden oluşturmadan veya yeniden boyutlandırabilmeniz için, [düğümler](#nodes) bölümünde anlatıldığı gibi, hata ayıklama amacıyla herhangi bir düğüm Aracısı günlüğünü indirmeniz önerilir.

- **Havuz yeniden oluşturma** Benzer bir notta havuzlarınızın günlük olarak silinmesi ve yeniden oluşturulması önerilmez. Bunun yerine, yeni bir havuz oluşturun, mevcut işlerinizi yeni havuza işaret etmek üzere güncelleştirin. Tüm görevler yeni havuza taşındıktan sonra eski havuzu silin.

- **Havuz verimliliği ve faturalama** Batch 'in kendisi ek ücret vermez, ancak kullanılan işlem kaynakları için ücret ödemeniz gerekir. İçindeki durum ne olursa olsun, havuzdaki her işlem düğümü için faturalandırılırsınız. Bu, düğüm için gereken depolama ve ağ maliyetleri gibi tüm ücretleri içerir. En iyi uygulamalar hakkında daha fazla bilgi edinmek için bkz. [Azure Batch Için maliyet analizi ve bütçeleri](budget.md).

### <a name="pool-allocation-failures"></a>Havuz ayırma sorunları

Havuz ayırma arızaları, ilk ayırma veya sonraki yeniden boyutlandırmalarda herhangi bir noktada gerçekleşebilir. Bunun nedeni, bir bölgedeki geçici kapasiteden veya toplu Işin bağımlı olduğu diğer Azure hizmetlerindeki hatalardan kaynaklanabilir. Çekirdek kotanızın garantisi, ancak bunun yerine bir sınır değildir.

### <a name="unplanned-downtime"></a>Planlanmamış kapalı kalma süresi

Toplu Iş havuzlarının Azure 'da kesinti süresi olaylarıyla karşılaşması mümkündür. Senaryonuzu veya Batch için iş akışınızı planlarken ve geliştirirken bunu göz önünde bulundurun.

Bir düğümün başarısız olması durumunda, Batch otomatik olarak bu işlem düğümlerini sizin adınıza kurtarmaya çalışır. Bu, kurtarılan düğüm üzerinde çalışan herhangi bir görevin yeniden çizelgelenmesi tetiklenebilir. Kesilen görevler hakkında daha fazla bilgi edinmek için bkz. [yeniden denemeler tasarlama](#design-for-retries-and-re-execution) .

### <a name="azure-region-dependency"></a>Azure bölge bağımlılığı

Zamana duyarlı veya üretim iş yükünüz varsa, tek bir Azure bölgesine bağlı olmaması önerilir. Nadir olarak, bir bölgenin tamamını etkileyebilecek sorunlar vardır. Örneğin, işlemelerinizin belirli bir zamanda başlaması gerekiyorsa, birincil bölgenizdeki havuzun ölçeğini *başlangıç zamanından önce da*ölçeklendirin. Havuz ölçeği başarısız olursa, bir yedekleme bölgesindeki (veya bölgelerde) bir havuzu ölçeklendirmeye geri dönebilirsiniz. Farklı bölgelerdeki birden çok hesap genelinde havuzlar, başka bir havuz ile yanlış bir sorun varsa, daha kolay erişilebilir bir yedekleme sağlar. Daha fazla bilgi için bkz. [uygulamanızı yüksek kullanılabilirlik Için tasarlama](high-availability-disaster-recovery.md).

## <a name="jobs"></a>İşler

[İş](jobs-and-tasks.md#jobs) , yüzlerce, binlerce veya hatta milyonlarca görevi içerecek şekilde tasarlanan bir kapsayıcıdır. İşleri oluştururken bu yönergeleri izleyin.

### <a name="fewer-jobs-more-tasks"></a>Daha az iş, daha fazla görev

Tek bir görevi çalıştırmak için bir iş kullanmak verimsiz değildir. Örneğin, her biri 10 görev içeren 100 iş oluşturmak yerine 1000 görevi içeren tek bir işi kullanmak daha etkilidir. Her biri tek bir görev içeren 1000 iş çalıştırmak, en az verimli, en yavaş ve en pahalı yaklaşım olacaktır.

Bu nedenle, binlerce eşzamanlı etkin iş gerektiren bir Batch çözümü tasarlayamadığından emin olun. Görevler için kota yoktur, bu nedenle çok sayıda görevi olabildiğince az iş olarak yürütmek, [işinizi ve iş zamanlaması kotalarını](batch-quota-limit.md#resource-quotas)verimli bir şekilde kullanır.

### <a name="job-lifetime"></a>İş ömrü

Toplu işin, sistemden silinene kadar sınırsız bir ömrü vardır. Durumu, zamanlama için daha fazla görevi kabul edip edemeyeceğini belirler.

Açık olarak sonlandırılmadığı takdirde bir iş otomatik olarak tamamlandı durumuna taşınamaz. Bu, [Onalltasksall](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.common.onalltaskscomplete?view=azure-dotnet) özelliği veya [Maxduvara Clocktime](https://docs.microsoft.com/rest/api/batchservice/job/add#jobconstraints)aracılığıyla otomatik olarak tetiklenebilir.

Varsayılan bir [etkin iş ve iş zamanlaması kotası](batch-quota-limit.md#resource-quotas)vardır. İşlerin ve iş zamanlamalarının tamamlandı durumunda bu kotaya dahil sayılmaz.

## <a name="tasks"></a>Görevler

[Görevler](jobs-and-tasks.md#tasks) , bir işi oluşturan bireysel iş birimleridir. Görevler kullanıcı tarafından gönderilir ve işlem düğümleri için toplu Iş tarafından zamanlanır. Görevleri oluştururken ve yürütürken yapmanız gereken çeşitli tasarım konuları vardır. Aşağıdaki bölümlerde, sorunları ele almak ve verimli bir şekilde gerçekleştirmek için genel senaryolar ve görevlerinizi tasarlamak açıklanmaktadır.

### <a name="save-task-data"></a>Görev verilerini Kaydet

İşlem düğümleri doğası gereği daha kısa. Yığın içinde otomatik havuz ve otomatik ölçeklendirme gibi birçok özellik vardır ve bu da düğümlerin kaybolmasını kolaylaştırır. Düğümler havuzdan ayrıldığında (bir yeniden boyutlandırma veya havuz silme nedeniyle), bu düğümlerdeki tüm dosyalar da silinir. Bu nedenle, bir görev tamamlanmadan önce üzerinde çalıştığı düğümün çıkışını ve kalıcı bir depoya taşımalıdır. Benzer şekilde, bir görev başarısız olursa, dayanıklı bir depoya hatayı tanılamak için gereken günlükleri taşımalıdır.

Toplu işlem, Azure depolama 'yı kullanarak verileri [OutputFiles](batch-task-output-files.md)aracılığıyla karşıya yüklemeyi, ayrıca çeşitli paylaşılan dosya sistemlerini de veya kendi görevlerinizde karşıya yüklemeyi gerçekleştirmek için tümleşik destek içerir.

### <a name="manage-task-lifetime"></a>Görev ömrünü yönetme

Görevleri artık gerekli olmadığında silin veya bir [retentionTime](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskconstraints.retentiontime?view=azure-dotnet) görev kısıtlaması ayarlayın. Bir `retentionTime` ayarlanmışsa, toplu iş süresi dolduktan sonra görev tarafından kullanılan disk alanını otomatik olarak temizler `retentionTime` .

Görevleri silme işlemi iki şeyi gerçekleştirir. İşte, ilgilendiğiniz görevi sorgulamayı/bulmayı daha da zorlamanızı sağlayan (tamamlanan görevleri filtrelemeniz gerektiği için) iş içinde görevlerin bir oluşturma işlemi olmamasını sağlar. Ayrıca, düğümdeki karşılık gelen görev verilerini de temizler (belirtilen `retentionTime` zaten isabet mıştır). Bu, düğümlerinizin görev verileriyle doldurulup disk alanı tükenmemesini sağlamaya yardımcı olur.

### <a name="submit-large-numbers-of-tasks-in-collection"></a>Koleksiyonda çok sayıda görev gönder

Görevler ayrı ayrı veya koleksiyonlarda gönderilebilir. Ek yükü ve gönderim süresini azaltmak için görevler toplu gönderimi yaparken bir seferde en fazla 100 olan [koleksiyonlara](https://docs.microsoft.com/rest/api/batchservice/task/addcollection) görev gönderebilirsiniz.

### <a name="set-max-tasks-per-node-appropriately"></a>Düğüm başına en fazla görevi uygun şekilde ayarlayın

Batch, düğümlerde fazla abone olan görevleri destekler (bir düğümden daha fazla görev çalıştırmak çekirdekler vardır). Görevlerinizin, havuzunuzdaki düğümlere "sığması" durumunda olduğundan emin olmanız gerekir. Örneğin, her birinin tek bir düğümde (ile bir havuzda) %25 CPU kullanımı tükettiği sekiz görevi zamanlamaya çalışırsanız, düzeyi düşürülmüş bir deneyimle karşılaşabilirsiniz `maxTasksPerNode = 8` .

### <a name="design-for-retries-and-re-execution"></a>Yeniden denemeler ve yeniden yürütme için tasarım

Görevler, toplu Işlem tarafından otomatik olarak yeniden denenebilir. İki tür yeniden deneme vardır: Kullanıcı denetimli ve dahili. Kullanıcı denetimli yeniden denemeler, görevin [Maxtaskretrycount](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.taskconstraints.maxtaskretrycount?view=azure-dotnet)tarafından belirtilir. Görevde belirtilen bir program sıfır dışında bir çıkış kodu ile çıktığında, görev değerine kadar yeniden denenir `maxTaskRetryCount` .

Nadir olarak bir görev, işlem düğümündeki hatalar nedeniyle, görev çalışırken düğüm üzerinde iç durumu veya bir başarısızlığı güncelleştirmeme gibi yeniden deneniyor olabilir. Görev, mümkünse aynı işlem düğümünde yeniden denenecektir, bu, görevde bir süre önce bir iç sınıra kadar ve bir toplu Işlem tarafından, potansiyel olarak farklı bir işlem düğümünde yeniden zamanlanmasını ertelenir.

Görevlerinizi adanmış veya düşük öncelikli düğümlerde yürütürken hiçbir tasarım farkı yoktur. Bir görevin düşük öncelikli bir düğümde çalışırken geçersiz hale gelmiş veya ayrılmış bir düğümdeki bir hata nedeniyle kesintiye uğratıldığı, her iki durum da görevi withfailure hatası tasarlayarak azaltılmıştır.

### <a name="build-durable-tasks"></a>Dayanıklı Görevler oluşturun

Görevler hatalı ve yeniden denemeye yetecek şekilde tasarlanmalıdır. Bu, özellikle uzun süre çalışan görevler için önemlidir. Bunu yapmak için, görevlerin aynı anda birden çok kez çalıştırılsa bile aynı, tek bir sonuç oluşturmasını sağlayın. Bunu gerçekleştirmenin bir yolu, görevlerinizi "hedef aramaktır". Diğer bir yol da görevlerinizin ıdempotent olduğundan emin olmak (görevler kaç kez çalıştırıldıklarından bağımsız olarak aynı sonuca sahip olacaktır).

Ortak bir örnek, dosyaları bir işlem düğümüne kopyalamak için kullanılan bir görevdir. Basit yaklaşım, her çalıştığında belirtilen her dosyayı kopyalayan, verimsiz olan ve geldiğinde hatası için oluşturulmamış bir görevdir. Bunun yerine, dosyaların işlem düğümünde olduğundan emin olmak için bir görev oluşturun; zaten mevcut olan dosyaları yeniden kopyalamamış bir görev. Bu şekilde, görev kesintiye uğratıldığında, görev ayrıldıktan sonra açılır.

### <a name="avoid-short-execution-time"></a>Kısa yürütme zamanından kaçının

Yalnızca bir ile iki saniye çalışan görevler ideal değildir. Tek bir görevde önemli miktarda iş yapmayı denemelisiniz (en az 10 saniye, saat veya güne kadar). Her görev bir dakika (veya daha fazla) boyunca yürütülüyordur, genel işlem zamanının bir bölümü olarak zamanlama yükü küçüktür.


## <a name="nodes"></a>Düğümler

[İşlem düğümü](nodes-and-pools.md#nodes) , uygulamanızın iş yükünün bir kısmını işlemeye ayrılmış bir Azure sanal MAKINESI (VM) veya bulut hizmeti sanal makinedir. Düğümlerle çalışırken bu yönergeleri izleyin.

### <a name="idempotent-start-tasks"></a>Idempotent başlangıç görevleri

Diğer görevlerde olduğu gibi, düğüm [Başlangıç görevi](jobs-and-tasks.md#start-task) , düğüm her önyüklendiğinde yeniden çalıştırılacak şekilde ıdempotent olmalıdır. Bir ıdempotent görevi, birden çok kez çalıştırıldığında tutarlı bir sonuç üreten bir görevdir.

### <a name="manage-long-running-services-via-the-operating-system-services-interface"></a>İşletim sistemi Hizmetleri arabirimi aracılığıyla uzun süre çalışan hizmetleri yönetme

Bazen, düğümdeki Batch aracısının yanı sıra başka bir aracı da çalıştırmanız gerekir. Örneğin, düğümden veri toplamak ve bunu raporlamak isteyebilirsiniz. Bu aracıların bir Windows hizmeti veya Linux hizmeti gibi işletim sistemi hizmetleri olarak dağıtılmasını öneririz `systemd` .

Bu Hizmetleri çalıştırırken, düğüm üzerindeki toplu yönetilen dizinlerde bulunan dosyalarda dosya kilitleri almalıdır, aksi takdirde toplu Işlem dosya kilitleri nedeniyle bu dizinlerin silinmesine neden olur. Örneğin, bir Windows hizmetini başlangıç görevinde yüklüyorsanız, hizmeti doğrudan başlangıç görevi çalışma dizininden başlatmak yerine, dosyaları başka bir yere kopyalayın (veya dosyalar varsa kopyayı atlayın). Ardından bu konumdan hizmeti yüklersiniz. Batch, başlangıç görevinizi yeniden çalıştırdığınızda, başlangıç görevi çalışma dizinini siler ve yeniden oluşturur. Bu, hizmetin başlangıç görevi çalışma dizinini değil diğer dizinde dosya kilitleri olduğu için geçerlidir.

### <a name="avoid-creating-directory-junctions-in-windows"></a>Windows 'da Dizin geçişlerini oluşturmaktan kaçının

Bazen Dizin sabit bağlantıları olarak adlandırılan dizin junler, görev ve iş temizliği sırasında uğraşmak zordur. Sabit bağlantılar yerine çözümlemeyin (geçici bağlantılar) kullanın.

### <a name="collect-the-batch-agent-logs"></a>Batch Aracısı günlüklerini toplayın

Düğüm üzerinde çalışan bir düğümün veya görevlerin davranışını içeren bir sorun fark ederseniz, söz konusu düğümlerin ayırmayı kaldırma işleminden önce Batch Aracısı günlüklerini toplayın. Batch Aracısı günlükleri, Batch Hizmeti günlüklerini karşıya yükle API 'SI kullanılarak toplanabilir. Bu Günlükler, Microsoft 'a destek bileti kapsamında sağlanabilir ve sorun giderme ve çözümleme konularında yardımcı olur.

## <a name="isolation-security"></a>Yalıtım güvenliği

Yalıtım amaçları doğrultusunda, senaryonuz işlerin birbirinden yalıtılması gerekiyorsa, bunları ayrı havuzlarda bulundurarak bunu yapın. Havuz, toplu Işteki güvenlik yalıtımı sınırıdır ve varsayılan olarak, iki havuz görünür değildir veya birbirleriyle iletişim kuramaz. Yalıtım yöntemi olarak ayrı Batch hesapları kullanmaktan kaçının.

## <a name="moving-batch-accounts-across-regions"></a>Toplu hesapları bölgeler arasında taşıma

Mevcut bir Batch hesabını bir bölgeden diğerine taşımak yararlı olabilecek senaryolar vardır. Örneğin, olağanüstü durum kurtarma planlamasının bir parçası olarak başka bir bölgeye geçmek isteyebilirsiniz.

Azure Batch hesapları doğrudan bir bölgeden diğerine taşınamaz. Ancak, Batch hesabınızın mevcut yapılandırmasını dışarı aktarmak için bir Azure Resource Manager şablonu kullanabilirsiniz. Daha sonra, Batch hesabını bir şablona dışarı aktararak, hedef bölgeyle eşleşecek parametreleri değiştirerek ve sonra şablonu yeni bölgeye dağıtarak kaynağı başka bir bölgede oluşturabilirsiniz.

Şablonu yeni bölgeye yükledikten sonra sertifikaların, iş zamanlamalarının ve uygulama paketlerinin yeniden oluşturulması gerekir. Değişiklikleri uygulamak ve Batch hesabını taşımayı tamamlamak için, özgün Batch hesabını veya kaynak grubunu silmeyi unutmayın.

Kaynak Yöneticisi ve şablonlar hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure Portal kullanarak Azure Resource Manager şablonları oluşturma ve dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).

## <a name="connectivity"></a>Bağlantı

Batch çözümlerinde bağlantıyı değerlendirirken aşağıdaki kılavuzu gözden geçirin.

### <a name="network-security-groups-nsgs-and-user-defined-routes-udrs"></a>Ağ güvenlik grupları (NSG 'ler) ve Kullanıcı tanımlı yollar (UDRs)

[Bir sanal ağda Batch havuzlarını](batch-virtual-network.md)sağlarken, `BatchNodeManagement` hizmet etiketi, bağlantı noktaları, protokoller ve kuralın kullanımıyla ilgili yönergeleri yakından takip edin.
Hizmet etiketinin kullanımı, temel alınan Batch hizmeti IP adresleri yerine kullanılması önemle önerilir. Bunun nedeni, IP adreslerinin zaman içinde değiştirebileceğinden kaynaklanır. Batch hizmeti IP adreslerini doğrudan kullanmak, Batch havuzlarınız için kararsızlığa, kesintilere veya kesintilere neden olabilir.

Kullanıcı tanımlı yollar (UDRs) için, bu adresler zaman içinde değişdiklerinden, Batch hizmeti IP adreslerini düzenli aralıklarla bir şekilde güncelleştirmek için bir işlem yapıldığından emin olun. Batch hizmeti IP adresleri listesini edinme hakkında bilgi edinmek için bkz. [Şirket Içi hizmet etiketleri](../virtual-network/service-tags-overview.md). Batch hizmeti IP adresleri, `BatchNodeManagement` hizmet etiketiyle (veya Batch hesabı bölgesiyle eşleşen bölgesel değişkenle) ilişkilendirilir.

### <a name="honoring-dns"></a>DNS 'i manlama

Sistemlerinizin Batch hesabı hizmet URL 'niz için DNS yaşam süresi (TTL) olduğundan emin olun. Ayrıca, Batch hizmeti istemcilerinizin ve Batch hizmetine yönelik diğer bağlantı mekanizmalarının IP adreslerine dayanmadığından emin olun (veya aşağıda açıklandığı gibi [statik genel IP adreslerine sahip bir havuz oluşturun](create-pool-public-ip.md) ).

İstekleriniz 5 xx düzeyinde HTTP yanıtı alıyorsa ve yanıtta bir "bağlantı: kapatma" üst bilgisi varsa, Batch hizmeti istemciniz var olan bağlantıyı kapatarak öneriyi gözlemlemelidir, Batch hesabı hizmet URL 'SI için DNS 'i yeniden çözümleyip yeni bir bağlantı isteği gerçekleştirmeye devam etmelidir.

### <a name="retry-requests-automatically"></a>İstekleri otomatik olarak yeniden dene

Batch hizmeti istemcilerinizde, normal işlem sırasında bile isteklerinizi otomatik olarak yeniden denemek için uygun yeniden deneme ilkelerine sahip olduğundan emin olun. Bu yeniden deneme ilkeleri, en az 5 dakikalık bir aralığı kapsamalıdır. Otomatik yeniden deneme özellikleri, [.net RetryPolicyProvider sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.retrypolicyprovider?view=azure-dotnet)gibi çeşitli Batch SDK 'leriyle sağlanır.

### <a name="static-public-ip-addresses"></a>Statik genel IP adresleri

Genellikle, bir Batch havuzundaki sanal makinelere, havuzun kullanım ömrü boyunca değişebilir genel IP adresleri üzerinden erişilir. Bu, belirli IP adreslerine erişimi sınırlayan bir veritabanı veya başka bir dış hizmetle etkileşim kurmayı zorlaştırır. Havuzunuzdaki genel IP adreslerinin beklenmedik şekilde değişmemesini sağlamak için, denetlediğiniz bir statik genel IP adresleri kümesini kullanarak bir havuz oluşturabilirsiniz. Daha fazla bilgi için bkz. [belirtilen genel IP adreslerine sahip bir Azure Batch havuzu oluşturma](create-pool-public-ip.md).

## <a name="batch-node-underlying-dependencies"></a>Toplu düğüm temel alınan bağımlılıklar

Batch çözümlerinizi tasarlarken aşağıdaki bağımlılıkları ve kısıtlamaları göz önünde bulundurun.

### <a name="system-created-resources"></a>Sistem tarafından oluşturulan kaynaklar

Azure Batch, sanal makinede değiştirilmemelidir bir Kullanıcı ve grup kümesi oluşturur ve yönetir. Bunlar şu şekildedir:

#### <a name="windows"></a>Windows

- **Poolnonadmin** adlı Kullanıcı
- **Wataskcommon** adlı bir Kullanıcı grubu

#### <a name="linux"></a>Linux

- **_Azbatch** adlı bir Kullanıcı

### <a name="file-cleanup"></a>Dosya temizleme

Toplu işlem, saklama süresi sona erdiğinde görevlerin çalıştırıldığı çalışma dizinini etkin bir şekilde temizlemeye çalışır. Bu dizinin dışında yazılan tüm dosyalar, disk alanını doldurmaktan kaçınmak için [Temizleme sorumluluğudur](#manage-task-lifetime) . 

Windows üzerinde bir hizmeti, startTask çalışma dizininden hala kullanımda olması nedeniyle çalıştırırsanız, çalışma dizini için otomatik temizleme engellenir. Bu, performansın düşmesine neden olur. Bunu yapmak için, bu hizmetin dizinini Batch tarafından yönetilmeyen ayrı bir dizinle değiştirin.

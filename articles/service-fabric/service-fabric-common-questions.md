---
title: Microsoft Azure Hizmet Kumaşı hakkında sık sorulan sorular
description: Özellikler, kullanım örnekleri ve yaygın senaryolar da dahil olmak üzere Service Fabric hakkında sık sorulan sorular.
ms.topic: troubleshooting
ms.date: 08/18/2017
ms.author: pepogors
ms.openlocfilehash: bf61858b446c1ac6d4a0210571fffaa721ad0166
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78254898"
---
# <a name="commonly-asked-service-fabric-questions"></a>Sık sorulan Service Fabric soruları

Service Fabric'in neler yapabileceği ve nasıl kullanılması gerektiği hakkında sık sorulan birçok soru vardır. Bu belge, bu sık sorulan soruların çoğunu ve yanıtlarını kapsar.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="cluster-setup-and-management"></a>Küme kurulumu ve yönetimi

### <a name="how-do-i-roll-back-my-service-fabric-cluster-certificate"></a>Service Fabric küme sertifikamı nasıl geri alarım?

Uygulamanızda herhangi bir yükseltmeyi geri almak, Hizmet Kumaşı küme çoğunluk taahhüdünüzden önce sağlık hatası nın algılanmasını gerektirir; taahhüt edilen değişiklikler yalnızca ileri ye doğru atılabilir. Yükseltme mühendisinin Müşteri Destek Hizmetleri aracılığıyla, denetlenmeyen bir kesme sertifikası değişikliği getirildiğinde kümenizi kurtarmak için gerekli olabilir.  [Service Fabric'in uygulama yükseltmesi](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade?branch=master) [Uygulama yükseltme parametreleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-upgrade-parameters?branch=master)uygular ve sıfır kesinti yükseltme sözü verir.  Önerilen uygulama yükseltme izleme modumuzu takiben, güncelleştirme etki alanlarında otomatik ilerleme, varsayılan bir hizmeti güncelleştirmebaşarısız olursa otomatik olarak geri alma, sağlık denetimleri geçen dayanmaktadır.
 
Kümeniz, Kaynak Yöneticisi şablonunuzdaki klasik Sertifika Parmak İzi özelliğinden hala yararlanıyorsa, kümeyi [sertifika parmak baskısından ortak ada değiştirmeniz](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-change-cert-thumbprint-to-cn)ve modern sırlar yönetimi özelliklerinden yararlanmanız önerilir.

### <a name="can-i-create-a-cluster-that-spans-multiple-azure-regions-or-my-own-datacenters"></a>Birden çok Azure bölgesine veya kendi veri merkezlerimi kapsayan bir küme oluşturabilir miyim?

Evet. 

Çekirdek Hizmet Kumaş kümeleme teknolojisi, birbirleriyle ağ bağlantısı olduğu sürece dünyanın her yerinden çalışan makineleri birleştirmek için kullanılabilir. Ancak, böyle bir küme oluşturma ve çalıştırma karmaşık olabilir.

Bu senaryoyla ilgileniyorsanız, ek rehberlik almak için [Service Fabric GitHub Sorunlar Listesi](https://github.com/azure/service-fabric-issues) veya destek temsilciniz aracılığıyla iletişime geçmenizi öneririz. Service Fabric ekibi, bu senaryo için ek netlik, kılavuzluk ve öneriler sağlamak için çalışmaktadır. 

Göz önünde bulundurulması gereken bazı şeyler: 

1. Azure'daki Hizmet Kumaşı küme kaynağı, kümenin üzerine inşa edilen sanal makine ölçeği kümeleri gibi bugün bölgeseldir. Bu, bölgesel bir hata durumunda, azure kaynak yöneticisi veya Azure portalı üzerinden kümeyi yönetme yeteneğini kaybedebileceğiniz anlamına gelir. Küme çalışmaya devam etse ve onunla doğrudan etkileşimkurabilirsiniz olsa bile bu durum olabilir. Buna ek olarak, Azure bugün bölgeler arasında kullanılabilir tek bir sanal ağa sahip olma olanağı sunmamaktadır. Bu, Azure'daki çok bölgeli bir kümenin [VM Ölçeği Kümeleri'ndeki her VM için Genel IP Adresleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-networking.md#public-ipv4-per-virtual-machine) veya Azure VPN Ağ [Geçitleri](../vpn-gateway/vpn-gateway-about-vpngateways.md)gerektirdiği anlamına gelir. Bu ağ seçenekleri maliyetler, performans ve bir dereceye kadar uygulama tasarımı üzerinde farklı etkileri vardır, bu nedenle dikkatli analiz ve planlama böyle bir ortam ayakta önce gereklidir.
2. Bu makinelerin bakımı, yönetimi ve izlenmesi, özellikle farklı bulut sağlayıcıları arasında veya şirket içi kaynaklar ile Azure arasında olduğu gibi _ortam türlerine_ yayılmışken karmaşık hale gelebilir. Yükseltmelerin, izlemenin, yönetimin ve tanılamanın, bu tür bir ortamda üretim iş yüklerini çalıştırmadan önce hem küme hem de uygulamalar için anlaşıldığından emin olmak için dikkatli olunmalıdır. Azure'da veya kendi veri merkezlerinizde bu sorunları çözme deneyiminiz varsa, Service Fabric kümenizi kurarken veya çalıştırırken aynı çözümlerin de uygulanabileceği olasıdır. 

### <a name="do-service-fabric-nodes-automatically-receive-os-updates"></a>Servis Kumaş düğümleri otomatik olarak işletim sistemi güncelleştirmeleri alır mı?

[Sanal Makine Ölçeği Seti Otomatik İşletim Güncelleme](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-automatic-upgrade) Genel Olarak Kullanılabilir özelliği ni bugün kullanabilirsiniz.

Azure'da çalıştırılamayan kümeler için, Hizmet Kumaş düğümlerinizin altındaki işletim sistemlerini yamalamak için [bir uygulama sağladık.](service-fabric-patch-orchestration-application.md)

### <a name="can-i-use-large-virtual-machine-scale-sets-in-my-sf-cluster"></a>SF kümemde büyük sanal makine ölçek kümelerini kullanabilir miyim? 

**Kısa cevap** - Hayır. 

**Uzun Yanıt** - Büyük sanal makine ölçeği kümeleri, 1000 VM örneğine ayarlanmış sanal bir makine ölçeğini ölçeklendirmenize olanak sayılsa da, bunu Yerleşim Grupları (PGs) kullanarak yapar. Hata etki alanları (FD'ler) ve yükseltme etki alanları (UD'ler) yalnızca bir yerleşim grubu içinde tutarlıdır Hizmet dokusu, hizmet yinelemelerinizin/Hizmet örneklerinin yerleşim kararlarını almak için FD'ler ve UD'ler kullanır. FD'ler ve UD'ler yalnızca bir yerleşim grubu içinde karşılaştırılabilir olduğundan, SF bunu kullanamaz. Örneğin, PG1'deki VM1'de FD=0 ve PG2'de VM9 topolojisi Varsa, bu VM1 ve VM2'nin iki farklı Donanım Rafı üzerinde olduğu anlamına gelmez, bu nedenle SF bu durumda fd değerlerini yerleşim kararları almak için kullanamaz.

Düzey-4 Yük dengeleme desteğinin eksikliği gibi, şu anda büyük sanal makine ölçek setleri ile ilgili diğer sorunlar vardır. [Büyük ölçekli kümelerle ilgili ayrıntılar](../virtual-machine-scale-sets/virtual-machine-scale-sets-placement-groups.md) için bkz.



### <a name="what-is-the-minimum-size-of-a-service-fabric-cluster-why-cant-it-be-smaller"></a>Service Fabric kümesinin minimum boyutu nedir? Neden daha küçük olamaz?

Üretim iş yüklerini çalıştıran bir Service Fabric kümesi için desteklenen minimum boyut beş düğümdür. Dev senaryoları için, bir düğümü (Visual Studio'da hızlı geliştirme deneyimi için optimize edilmiş) ve beş düğüm kümesini destekliyoruz.

Aşağıdaki üç nedenden dolayı bir üretim kümesinin en az 5 düğüme sahip olmasını şart görüyoruz:
1. Hiçbir kullanıcı hizmeti çalışmadığında bile, Service Fabric kümesi adlandırma hizmeti ve başarısız yönetici hizmeti de dahil olmak üzere bir dizi durumlu sistem hizmeti çalıştırın. Bu sistem hizmetleri kümenin çalışır durumda kalması için gereklidir.
2. Her zaman düğüm başına bir hizmetin bir kopyasını yer, bu nedenle küme boyutu bir hizmetin (aslında bir bölüm) sahip olabileceği yineleme sayısı için üst sınırdır.
3. Bir küme yükseltmesi en az bir düğüm aşağı getirecek olduğundan, en az bir düğüm bir arabellek olmasını istiyorum, bu nedenle, bir üretim kümesi çıplak minimum *ek olarak* en az iki düğüm olmasını istiyorum. Çıplak minimum aşağıda açıklandığı gibi bir sistem hizmeti nin çoğunluk boyutudur.  

Kümenin iki düğümün eşzamanlı arızası karşısında kullanılabilir olmasını istiyoruz. Hizmet Kumaşı kümesinin kullanılabilmesi için sistem hizmetlerinin kullanılabilir olması gerekir. Adlandırma hizmeti ve başarısız yönetici hizmeti gibi, kümeye hangi hizmetlerin dağıtıldığını ve şu anda nerede barındırıldığını izleyen durumlu sistem hizmetleri güçlü tutarlılığa bağlıdır. Bu güçlü tutarlılık, sırayla, bir çoğunluk belirli bir hizmet için yinelemeler (N/2 +1) sıkı bir çoğunluğu temsil bu hizmetlerin durumuna herhangi bir güncelleştirme için bir *çoğunluk* elde etmek için yeteneğine bağlıdır. Bu nedenle, iki düğümün eşzamanlı kaybına (böylece bir sistem hizmetinin iki kopyasının aynı anda kaybedilmesi) karşı dirençli olmak istiyorsak, minimum boyutu beş olmaya zorlayan ClusterSize - QuorumSize >= 2'ye sahip olmamız gerekir. Bunu görmek için, kümenin N düğümleri olduğunu ve bir sistem hizmetinin N kopyaları olduğunu düşünün -- her düğümde bir tane. Bir sistem hizmeti için çoğunluk boyutu (N/2 + 1). Yukarıdaki eşitsizlik N - (N/2 + 1) >= 2 gibi görünür. Göz önünde bulundurulması gereken iki durum vardır: N çift olduğunda ve N tek olduğunda. N çift ise, diyelim\*ki N = 2 m burada m\*>=\*1, eşitsizlik 2 m gibi görünür - (2 m/2 + 1) >= 2 veya m >= 3. N için minimum 6'dır ve m = 3 olduğunda elde edilir. Öte yandan, N tek ise, diyelim\*ki M = 2 m+1 burada m\*>= 1,\*eşitsizlik 2 m+1 gibi görünür -\*( (2 m+1)/2 + 1 ) >= 2 veya 2 m+1 - (m+1) >= 2 veya m >= 2. N için minimum 5'tir ve m = 2 olduğunda elde edilir. Bu nedenle, eşitsizlik ClusterSize - QuorumSize >= 2 tatmin N tüm değerleri arasında, en az 5'tir.

Not, yukarıdaki bağımsız değişkende her düğümün bir sistem hizmetinin kopyasına sahip olduğunu varsaydık, bu nedenle çoğunluk boyutu kümedeki düğüm sayısına göre hesaplanmıştır. Ancak, *TargetReplicaSetSize* değiştirerek biz çoğunluk boyutu daha az yapabilir (N/2+1) hangi bir küme daha küçük olabilir izlenimi verebilir 5 düğümleri ve hala çoğunluk boyutu üzerinde 2 ekstra düğümleri var. Örneğin, 4 düğüm kümesinde, TargetReplicaSetSize'ı 3 olarak ayarlarsak, TargetReplicaSetSize'a dayalı çoğunluk boyutu (3/2 + 1) veya 2 olur, böylece ClusterSize - QuorumSize = 4-2 >= 2 olur. Ancak, aynı anda herhangi bir düğüm çiftini kaybedersek sistem hizmetinin çoğunlukta veya üstünde olacağını garanti edemeyiz, bu da kaybettiğimiz iki düğümün iki kopyaya ev sahipliği yapması olabilir, bu nedenle sistem hizmeti çoğunluk kaybına (sadece tek bir çoğaltma kaldı) ve kullanılamaz hale gelecektir.

Bu arka planla, bazı olası küme yapılandırmalarını inceleyelim:

**Bir düğüm**: Tek bir düğümün herhangi bir nedenle kaybedilmesi tüm kümenin kaybı anlamına geldiğinden, bu seçenek yüksek kullanılabilirlik sağlamaz.

**İki düğüm:** Iki düğüm (N = 2) arasında dağıtılan bir hizmet için bir çoğunluk 2 'dir (2/2 + 1 = 2). Tek bir yineleme kaybolduğunda, bir çoğunluk oluşturmak mümkün değildir. Bir hizmet yükseltmesi gerçekleştirmek geçici olarak bir yinelemeyi aşağı çekmek gerektirdiğinden, bu yararlı bir yapılandırma değildir.

**Üç düğüm**: üç düğüm (N=3) ile, çoğunluk oluşturma gereksinimi hala iki düğümdür (3/2 + 1 = 2). Bu, tek bir düğümü kaybedebileceğiniz ve yine de yeterli çoğunluğu koruyabileceğiniz anlamına gelir, ancak iki düğümün eşzamanlı olarak başarısız olması sistem hizmetlerini çoğunluk kaybına sürükleyecek ve kümenin kullanılamamasına neden olur.

**Dört düğüm**: dört düğüm (N=4) ile, çoğunluk oluşturma gereksinimi üç düğümdür (4/2 + 1 = 3). Bu, tek bir düğümü kaybedebileceğiniz ve yine de yeterli çoğunluğu koruyabileceğiniz anlamına gelir, ancak iki düğümün eşzamanlı olarak başarısız olması sistem hizmetlerini çoğunluk kaybına sürükleyecek ve kümenin kullanılamamasına neden olur.

**Beş düğüm**: beş düğüm (N=5) ile, çoğunluk oluşturma gereksinimi hala üç düğümdür (5/2 + 1 = 3). Bu, aynı anda iki düğüm kaybedebileceğiniz ve sistem hizmetleri için yeterli çoğunluğu koruyabileceğiniz anlamına gelir.

Üretim iş yükleri için, en az iki düğümün eşzamanlı olarak arızalanmasına karşı dirençli olmalısınız (örneğin, biri küme yükseltmesi, diğeri başka nedenlerden dolayı), bu nedenle beş düğüm gereklidir.

### <a name="can-i-turn-off-my-cluster-at-nightweekends-to-save-costs"></a>Maliyetlerden tasarruf etmek için kümemi gece/hafta sonları kapatabilir miyim?

Genellikle yapamazsınız. Service Fabric yerel, kısa ömürlü disklerde durum depolar, yani sanal makine farklı bir ana bilgisayara taşınırsa, veriler onunla birlikte hareket etmez. Normal çalışmada, yeni düğüm diğer düğümler tarafından güncel olarak getirildiğinden, bu bir sorun değildir. Ancak, tüm düğümleri durdurur ve daha sonra yeniden başlatırsanız, düğümlerin çoğunun yeni ana bilgisayarlarda başlaması ve sistemin kurtarılamamasına neden olma olasılığı önemlitür.

Uygulamanız dağıtılmadan önce test etmek için kümeler oluşturmak istiyorsanız, [sürekli tümleştirme/sürekli dağıtım ardışık lığınızın](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)bir parçası olarak bu kümeleri dinamik olarak oluşturmanızı öneririz.


### <a name="how-do-i-upgrade-my-operating-system-for-example-from-windows-server-2012-to-windows-server-2016"></a>İşletim Sistemimi (örneğin Windows Server 2012'den Windows Server 2016'ya) nasıl yükseltebilirim?

Biz gelişmiş bir deneyim üzerinde çalışırken, bugün, yükseltme sorumludur. Kümenin sanal makinelerindeki işletim sistemi görüntüsünü her seferinde bir VM yükseltmeniz gerekir. 

### <a name="can-i-encrypt-attached-data-disks-in-a-cluster-node-type-virtual-machine-scale-set"></a>Bağlı veri disklerini küme düğümü türünde (sanal makine ölçek kümesi) şifreleyebilir miyim?
Evet.  Daha fazla bilgi için [bkz.](../virtual-machine-scale-sets/virtual-machine-scale-sets-attached-disks.md#create-a-service-fabric-cluster-with-attached-data-disks) [Azure Disk Encryption for Virtual Machine Scale Sets](../virtual-machine-scale-sets/disk-encryption-overview.md)

### <a name="can-i-use-low-priority-vms-in-a-cluster-node-type-virtual-machine-scale-set"></a>Küme düğüm türünde (sanal makine ölçek kümesi) düşük öncelikli VM'ler kullanabilir miyim?
Hayır. Düşük öncelikli VM'ler desteklenmez. 

### <a name="what-are-the-directories-and-processes-that-i-need-to-exclude-when-running-an-anti-virus-program-in-my-cluster"></a>Kümemde bir virüsten koruma programı çalıştırırken hariç tutmam gereken dizinler ve işlemler nelerdir?

| **Antivirus Dışlanmış dizinler** |
| --- |
| Program Files\Microsoft Service Fabric |
| FabricDataRoot (küme yapılandırmasından) |
| FabricLogRoot (küme yapılandırmasından) |

| **Antivirus Dışlanmış işlemler** |
| --- |
| Kumaş.exe |
| FabricHost.exe |
| FabricInstallerService.exe |
| FabricSetup.exe |
| FabricDeployer.exe |
| ImageBuilder.exe |
| FabricGateway.exe |
| FabricDCA.exe |
| FabricFAS.exe |
| FabricUOS.exe |
| FabricRM.exe |
| FileStoreService.exe |
 
### <a name="how-can-my-application-authenticate-to-keyvault-to-get-secrets"></a>Uygulamam sırları almak için KeyVault'a nasıl kimlik doğrulayabilir?
Uygulamanızın KeyVault kimlik bilgilerini almak için aşağıdaki araçlar şunlardır:

A. Uygulamalarınız oluşturma/paketleme işi sırasında, SF uygulamanızın veri paketine bir sertifika çekebilir ve bunu KeyVault'a kimlik doğrulamak için kullanabilirsiniz.
B. Sanal makine ölçeği seti MSI etkin ana bilgisayarları için, [MSI uç noktasından bir erişim belirteci](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token)almak için SF uygulamanız için basit bir PowerShell SetupEntryPoint geliştirebilir ve ardından [keyvault'tan sırlarınızı alabilirsiniz.](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret)

## <a name="application-design"></a>Uygulama Tasarımı

### <a name="whats-the-best-way-to-query-data-across-partitions-of-a-reliable-collection"></a>Güvenilir Koleksiyon'un bölümleri arasında verileri sorgulamanın en iyi yolu nedir?

Daha fazla performans ve iş elde etme için ölçeklendirmeyi etkinleştirmek için güvenilir koleksiyonlar genellikle [bölümlere ayrılmıştır.](service-fabric-concepts-partitioning.md) Bu, belirli bir hizmet için devlet onlarca veya yüzlerce makine yayılmış olabilir anlamına gelir. Bu tam veri kümesi üzerinde işlemleri gerçekleştirmek için birkaç seçeneğiniz var:

- Gerekli verileri çekmek için başka bir hizmetin tüm bölümlerini sorgulayan bir hizmet oluşturun.
- Başka bir hizmetin tüm bölümlerinden veri alabilen bir hizmet oluşturun.
- Her hizmetten gelen verileri düzenli aralıklarla harici bir depoya itin. Bu yaklaşım, yalnızca gerçekleştirdiğiniz sorgular temel iş mantığınızın bir parçası değilse, dış mağazanın verileri bayat olacaksa uygundur.
- Alternatif olarak, tüm kayıtlar arasında sorgulamayı desteklemesi gereken verileri güvenilir bir koleksiyon yerine doğrudan bir veri deposunda depolayın. Bu, eski verilerle ilgili sorunu ortadan kaldırır, ancak güvenilir koleksiyonların avantajlarından yararlanılmasına izin vermez.


### <a name="whats-the-best-way-to-query-data-across-my-actors"></a>Aktörlerim arasında verileri sorgulamanın en iyi yolu nedir?

Aktörler bağımsız devlet ve bilgi işlem birimleri olarak tasarlanmıştır, bu nedenle çalışma zamanında aktör durumu geniş sorguları gerçekleştirmek için tavsiye edilmez. Aktör durumu kümesinin tamamında sorgu yapmanız gerekiyorsa, aşağıdakilerden birini göz önünde bulundurmanız gerekir:

- Aktör hizmetlerinizi, aktör sayısından, oyuncu sayısından hizmetinizdeki bölüm sayısına kadar tüm verileri toplamak için ağ isteklerinin sayısının olağanüstü güvenilir hizmetlerle değiştirilmesi.
- Daha kolay sorgulama için durumlarını düzenli aralıklarla harici bir mağazaya itecek aktörlertasarlamak. Yukarıdaki gibi, bu yaklaşım yalnızca gerçekleştirdiğiniz sorgular çalışma zamanı davranışınız için gerekli değilse uygulanabilir.

### <a name="how-much-data-can-i-store-in-a-reliable-collection"></a>Güvenilir Bir Koleksiyonda ne kadar veri depolayabilirim?

Güvenilir hizmetler genellikle bölümlenir, bu nedenle depolayabildiğiniz miktar yalnızca kümedeki makine sayısı ve bu makinelerde bulunan bellek miktarıyla sınırlıdır.

Örnek olarak, 100 bölüm ve 3 yinelemeiçeren bir hizmette, ortalama 1 kb boyutundaki nesneleri depolayan güvenilir bir koleksiyona sahip olduğunuzu varsayalım. Şimdi makine başına 16 gb belleğe sahip 10 makine kümesine sahip olduğunuzu varsayalım. Basitlik ve muhafazakar olmak için, işletim sistemi ve sistem hizmetleri, Service Fabric çalışma süresi ve hizmetleri makine başına 10gb veya küme için 100 gb kullanılabilir bırakarak, bunun 6gb tüketmek varsayalım.

Her nesnenin üç kez (bir birincil ve iki yineleme) depolanması gerektiğini göz önünde bulundurarak, tam kapasitede çalışırken koleksiyonunuzdaki yaklaşık 35 milyon nesne için yeterli belleğe sahip olursunuz. Ancak, bir başarısızlık etki alanının ve kapasitenin yaklaşık 1/3'ünün ilerlediği ve bu sayıyı kabaca 23 milyona düşürecek bir yükseltme etki alanının eşzamanlı kaybına karşı dirençli olmanızı öneririz.

Bu hesaplamanın aşağıdakileri de varsaydığını unutmayın:

- Bölümler arasında veri dağılımıkabaca tek düze olduğunu veya yük ölçümlerini Küme Kaynak Yöneticisi'ne bildirdiğiniz. Varsayılan olarak, Service Fabric yineleme sayısına göre bakiye yükler. Önceki örnekte, kümedeki her düğüme 10 birincil yineleme ve 20 ikincil yineleme konur. Bu, bölümler arasında eşit olarak dağıtılan yük için iyi çalışır. Yük eşit değilse, Kaynak Yöneticisi'nin daha küçük yinelemeleri bir araya getirebilmesi ve daha büyük yinelemelerin tek bir düğümüzerinde daha fazla bellek tüketmesine izin vermesi için yüklemeyi bildirmeniz gerekir.

- Söz konusu güvenilir hizmet kümede tek depolama durumudur. Bir kümeye birden çok hizmet dağıtabildiğinizden, her birinin çalışması ve durumunu yönetmesi gereken kaynaklara dikkat etmeniz gerekir.

- Kümenin kendisinin büyümediğini ya da küçülmediğini. Daha fazla makine eklerseniz, Tek tek bir yineleme makineleri geçemez beri makine sayısı hizmetinizdeki bölüm sayısını aşana kadar Ek kapasite kaldıraç için Servis Kumaş ı çoğaltmaları yeniden dengeler. Bunun aksine, makineleri kaldırarak kümenin boyutunu küçültürseniz, kopyalarınız daha sıkı paketlenir ve genel kapasiteyi azaltır.

### <a name="how-much-data-can-i-store-in-an-actor"></a>Bir aktörde ne kadar veri depolayabilirim?

Güvenilir hizmetlerde olduğu gibi, bir aktör hizmetinde depolayabildiğiniz veri miktarı yalnızca kümenizdeki düğümler arasında bulunan toplam disk alanı ve bellekle sınırlıdır. Ancak, bireysel aktörler en çok devlet ve ilişkili iş mantığı küçük bir miktar kapsüllemek için kullanıldığında etkilidir. Genel bir kural olarak, tek bir aktör kilobayt cinsinden ölçülen devlet olmalıdır.

## <a name="other-questions"></a>Diğer sorular

### <a name="how-does-service-fabric-relate-to-containers"></a>Servis Kumaşı'nın konteynerler ile ilişkisi nedir?

Kapsayıcılar, tüm ortamlarda tutarlı bir şekilde çalışacak ve tek bir makinede izole bir şekilde çalışabilen hizmetleri ve bağımlılıklarını paketlemek için basit bir yol sunar. Service Fabric, [bir kapta paketlenmiş hizmetler](service-fabric-containers-overview.md)de dahil olmak üzere hizmetleri dağıtmak ve yönetmek için bir yol sunar.

### <a name="are-you-planning-to-open-source-service-fabric"></a>Açık kaynak Hizmet Kumaşı'nı mı planlıyorsunuz?

Biz Hizmet Kumaş açık kaynaklı parçaları var[(güvenilir hizmet çerçevesi,](https://github.com/Azure/service-fabric-services-and-actors-dotnet) [güvenilir aktörler çerçeve,](https://github.com/Azure/service-fabric-services-and-actors-dotnet) [ASP.NET Çekirdek entegrasyon kütüphaneleri](https://github.com/Azure/service-fabric-aspnetcore), [Hizmet Kumaş Explorer](https://github.com/Azure/service-fabric-explorer), ve [Hizmet Kumaş CLI](https://github.com/Azure/service-fabric-cli)) GitHub ve bu projelere topluluk katkıları kabul. 

Geçenlerde Hizmet Kumaş çalışma zamanı açık kaynak planı [duyurdu.](https://blogs.msdn.microsoft.com/azureservicefabric/2018/03/14/service-fabric-is-going-open-source/) Bu noktada, Linux oluşturma ve test araçlarıyla GitHub'da [Service Fabric repo'ya](https://github.com/Microsoft/service-fabric/) sahibiz, bu da repo'yu klonlayabileceğiniz, Linux için Service Fabric oluşturabileceğiniz, temel testleri çalıştırabileceğiniz, sorunları açabileceğiniz ve çekme istekleri gönderebileceğiniz anlamına gelir. Windows yapı ortamının da tamamen bir CI ortamıyla birlikte geçirilebiyi almak için çok çalışıyoruz.

Duyuruldukları gibi daha fazla ayrıntı için [Service Fabric blogunu](https://blogs.msdn.microsoft.com/azureservicefabric/) takip edin.

## <a name="next-steps"></a>Sonraki adımlar

Temel [Hizmet Kumaşı kavramları](service-fabric-technical-overview.md) ve [en iyi uygulamalar](service-fabric-best-practices-overview.md) hakkında bilgi edinin

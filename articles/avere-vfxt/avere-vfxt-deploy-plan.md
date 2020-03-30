---
title: Avere vFXT sisteminizi planlayın - Azure
description: Azure için Avere vFXT dağıtmadan önce yapmayı planlayanları açıklar
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: rohogue
ms.openlocfilehash: cd0c74c8aa40b3e96716ef37aa27b08b5f6aece1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76547549"
---
# <a name="plan-your-avere-vfxt-system"></a>Avere vFXT sisteminizi planlama

Bu makalede, Azure kümesi için gereksinimlerinize uygun şekilde konumlandırılmış ve boyutlandırılmış yeni bir Avere vFXT nasıl planlanış edilmiştir.

Azure Marketi'ne gitmeden veya herhangi bir VM oluşturmadan önce şu ayrıntıları göz önünde bulundurun:

* Küme diğer Azure kaynaklarıyla nasıl etkileşimde bulunacak?
* Küme elemanları özel ağlarda ve alt ağlarda nerede bulunmalıdır?
* Ne tür arka uç depolama alanı kullanacaksınız ve küme buna nasıl erişecek?
* İş akışınızı desteklemek için küme düğümlerinizin ne kadar güçlü olması gerekir?

Daha fazla bilgi edinmek için okumaya devam edin.

## <a name="learn-the-components-of-the-system"></a>Sistemin bileşenlerini öğrenin

Planlamaya başladığınızda Azure için Avere vFXT bileşenlerini anlamak yararlı olabilir.

* Küme düğümleri - Küme düğümleri olarak yapılandırılan üç veya daha fazla VM'den oluşur. Daha fazla düğüm, sisteme daha yüksek iş ve daha büyük bir önbellek verir.

* Önbellek - Önbellek kapasitesi küme düğümleri arasında eşit olarak bölünür. Küme oluştururken düğüm başına önbellek boyutunu ayarlayın; düğüm boyutları toplam önbellek boyutu na eklenir.

* Küme denetleyicisi - Küme denetleyicisi, küme düğümleri ile aynı alt ağ içinde bulunan ek bir VM'dir. Denetleyici küme oluşturmak ve devam eden yönetim görevleri için gereklidir.

* Arka uç depolama - Önbelleğe almak istediğiniz veriler, bir donanım depolama sisteminde veya Azure Blob kapsayıcısında uzun süreli olarak depolanır. Azure kümesi için Avere vFXT oluşturduktan sonra veya Blob depolama alanını kullanıyorsanız, küme oluştururken kapsayıcıyı ekleyip yapılandırabilirsiniz.

* İstemciler - Önbelleğe alınan dosyaları kullanan istemci makineleri, depolama sistemlerine doğrudan erişmek yerine sanal bir dosya yolu kullanarak kümeye bağlanır. (Mount [Avere vFXT kümesi](avere-vfxt-mount-clients.md)daha fazla bilgi edinin.)

## <a name="subscription-resource-group-and-network-infrastructure"></a>Abonelik, kaynak grubu ve ağ altyapısı

Azure dağıtımı için Avere vFXT öğelerinin nerede olacağını göz önünde bulundurun. Aşağıdaki diyagram, Azure bileşenleri için Avere vFXT için olası bir düzenlemeyi göstermektedir:

![Küme denetleyicisini ve küme VM'lerini bir alt ağ içinde gösteren diyagram. Alt ağ sınırı nın etrafında bir vnet sınırı vardır. Vnet içinde depolama hizmeti bitiş noktasını temsil eden bir altıgen; vnet'in dışındaki blob depolama alanına kesikli bir okla bağlanır.](media/avere-vfxt-components-option.png)

Avere vFXT kümenizin ağ altyapınızı planlarken aşağıdaki yönergeleri izleyin:

* Azure dağıtımı için her Avere vFXT için yeni bir abonelik oluşturun. Bu abonelikteki tüm bileşenleri yönetin.

  Her dağıtım için yeni bir abonelik kullanmanın yararları şunlardır:
  * Daha basit maliyet takibi - Kaynaklardan, altyapıdan ve işlem döngülerinden gelen tüm maliyetleri tek bir abonelikte görüntüleyin ve denetleyebilirsiniz.
  * Daha kolay temizleme - Proje tamamlandığında tüm aboneliği kaldırabilirsiniz.
  * Kaynak kotalarının kullanışlı bölümleme - Diğer kritik iş yüklerini olası kaynak azaltmalarından korumak için Avere vFXT istemcilerini ve kümele'yi tek bir abonelikte yalıtın. Bu ayırma, yüksek performanslı bir bilgi işlem iş akışı için çok sayıda istemci getirirken çakışmayı önler.

* VFXT kümesine yakın istemci hesaplama sistemlerinizi bulun. Arka uç depolama daha uzak olabilir.  

* vFXT kümesini ve küme denetleyicivm'i birlikte bulun - özellikle aşağıdakileri olmalıdır:

  * Aynı sanal ağda
  * Aynı kaynak grubunda
  * Aynı depolama hesabını kullanma
  
  Küme oluşturma şablonu çoğu durum için bu yapılandırmayı işler.

* Küme, istemcilerle veya diğer işlem kaynaklarıyla IP adresi çakışmasını önlemek için kendi alt netinde bulunmalıdır.

* Kaynak grupları, sanal ağlar, alt ağlar ve depolama hesapları da dahil olmak üzere küme için gerekli altyapı kaynaklarının çoğunu oluşturmak için küme oluşturma şablonunu kullanın.

  Zaten var olan kaynakları kullanmak istiyorsanız, bu tablodaki gereksinimleri karşıladıklarından emin olun.

  | Kaynak | Varolan kullanımı? | Gereksinimler |
  |----------|-----------|----------|
  | Kaynak grubu | Evet, boşsa | Boş olmalı|
  | Depolama hesabı | Küme oluşturmadan sonra varolan bir Blob kapsayıcısı bağlanırsa **evet** <br/>  Küme oluşturma sırasında yeni bir Blob kapsayıcısı oluşturuyorsanız **hayır** | Mevcut Blob kapsayıcısı boş olmalıdır <br/> &nbsp; |
  | Sanal ağ | Evet | Yeni bir Azure Blob kapsayıcısı oluşturuyorsanız bir depolama hizmeti bitiş noktası içermelidir |
  | Alt ağ | Evet | Diğer kaynakları içeremez |

## <a name="ip-address-requirements"></a>IP adresi gereksinimleri

Kümenizin alt netinin kümeyi destekleyecek kadar büyük bir IP adresi aralığına sahip olduğundan emin olun.

Avere vFXT kümesi aşağıdaki IP adreslerini kullanır:

* Bir küme yönetimi IP adresi. Bu adres, kümedeki düğümden düğüme gerektiği gibi taşınabilir, böylece her zaman kullanılabilir olur. Avere Control Panel yapılandırma aracına bağlanmak için bu adresi kullanın.
* Her küme düğümü için:
  * İstemcilere bakan en az bir IP adresi. (İstemcilere bakan tüm adresler kümenin *vserver'ı*tarafından yönetilir ve bu da IP adreslerini gerektiğinde düğümler arasında taşıyabilir.)
  * Küme iletişimi için bir IP adresi
  * Bir örnek IP adresi (VM'ye atanmış)

Azure Blob depolama alanını kullanıyorsanız, kümenizin sanal ağındaki IP adresleri de gerekebilir:  

* Azure Blob depolama hesabı için en az beş IP adresi gerekiyor. Blob depolama alanını kümenizle aynı sanal ağda bulursanız bu gereksinimi aklınızda bulundurun.
* Kümenin sanal ağının dışında olan Azure Blob depolama alanını kullanıyorsanız, sanal ağ içinde bir depolama hizmeti bitiş noktası oluşturun. Bitiş noktası IP adresi kullanmaz.

Kümeden farklı kaynak gruplarında ağ kaynaklarını ve Blob depolamasını (kullanılırsa) bulma seçeneğiniz vardır.

## <a name="vfxt-node-size"></a>vFXT düğüm boyutu

Küme düğümü olarak hizmet veren VM'ler önbelleğinizin istek üretim ve depolama kapasitesini belirler. <!-- The instance type offered has been chosen for its memory, processor, and local storage characteristics. You can choose from two instance types, with different memory, processor, and local storage characteristics. -->

Her vFXT düğümü aynı olacaktır. Diğer bir de, üç düğümlü bir küme oluşturursanız, aynı türde ve boyutta üç VM'niz olur.

| Örnek türü | Sanal çekirdek | Bellek  | Yerel SSD depolama  | Maksimum veri diskleri | Cached disk işbirliği | NIC (sayı) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_E32s_v3 | 32  | 256 GiB | 512 GiB  | 32 | 51.200 IOPS <br/> 768 MBps | 16.000 MBps (8)  |

Düğüm başına disk önbelleği yapılandırılabilir ve 1000 GB'dan 8000 GB'a kadar çıkabilir. Düğüm başına 4 TB, Standard_E32s_v3 düğümleri için önerilen önbellek boyutudur.

Bu SANAL Makineler hakkında daha fazla bilgi için Microsoft Azure belgelerini okuyun: [Bellek optimize edilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)

## <a name="account-quota"></a>Hesap kotası

Aboneliğinizin Avere vFXT kümesini ve kullanılan tüm bilgi işlem veya istemci sistemlerini çalıştırma kapasitesine sahip olduğundan emin olun. Ayrıntılar [için vFXT kümesi için Kota'yı](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) okuyun.

## <a name="back-end-data-storage"></a>Arka uç veri depolama

Arka uç depolama sistemleri hem kümenin önbelleğine dosya sağlar hem de önbellekten değiştirilen verileri alır. Çalışma setinizin uzun vadede yeni bir Blob kapsayıcısında mı yoksa varolan bir depolama sisteminde mi (bulut veya donanım) depolanacağına karar verin. Bu arka uç depolama sistemleri *çekirdek filers*denir.

### <a name="hardware-core-filers"></a>Donanım çekirdek filörleri

Kümeyi oluşturduktan sonra vFXT kümesine donanım depolama sistemleri ekleyin. Depolama sistemine kümenin alt ağınızdan ulaşılabildiği sürece, şirket içi sistemler de dahil olmak üzere çeşitli popüler donanım sistemlerini kullanabilirsiniz.

Avere vFXT kümesine varolan bir depolama sisteminin nasıl ekleyeceğiniz hakkında ayrıntılı yönergeler için [Yapıl](avere-vfxt-add-storage.md) depolamayı okuyun.

### <a name="cloud-core-filers"></a>Bulut çekirdek filörleri

Azure sistemi için Avere vFXT, arka uç depolama için boş Blob kapsayıcılarını kullanabilir. Kümeye eklendiğinde kapsayıcılar boş olmalıdır - vFXT sistemi varolan verileri korumaya gerek kalmadan nesne deposunu yönetebilmeli.

> [!TIP]
> Arka uç için Azure Blob depolama alanını kullanmak istiyorsanız, vFXT kümesini oluşturmanın bir parçası olarak yeni bir kapsayıcı oluşturun. Küme oluşturma şablonu, küme kullanılabilir olur olmaz kullanıma hazır olacak şekilde yeni bir Blob kapsayıcısı oluşturabilir ve yapılandırabilir. Daha sonra bir kapsayıcı eklemek daha karmaşıktır.
>
> Ayrıntılar [için Azure için Avere vFXT oluştur'u](avere-vfxt-deploy.md#create-the-avere-vfxt-for-azure) okuyun.

Boş Blob depolama kapsayıcısını çekirdek filer olarak ekledikten sonra, küme aracılığıyla verileri kopyalayabilirsiniz. Paralel, çok iş parçacığı lı kopyalama mekanizması kullanın. İstemci makineleri ve Avere vFXT önbelleği kullanarak verileri kümenin yeni kapsayıcısına verimli bir şekilde kopyalamayı öğrenmek [için verileri vFXT kümesine taşımayı](avere-vfxt-data-ingest.md) okuyun.

## <a name="cluster-access"></a>Küme erişimi

Azure kümesi için Avere vFXT özel bir alt ağda bulunur ve kümenin ortak bir IP adresi yoktur. Küme yönetimi ve istemci bağlantıları için özel alt ağa erişmek için bir yolunuz olmalıdır.

Erişim seçenekleri şunlardır:

* Atlama ana bilgisayarı - Özel ağ içindeki ayrı bir VM'ye ortak bir IP adresi atayın ve küme düğümlerine bir SSL tüneli oluşturmak için kullanın.

  > [!TIP]
  > Küme denetleyicisi üzerinde genel bir IP adresi ayarlarsanız, atlama ana bilgisayarı olarak kullanabilirsiniz. Daha fazla bilgi için [atlama ana bilgisayarı olarak Cluster denetleyicisi](#cluster-controller-as-jump-host) okuyun.

* Sanal özel ağ (VPN) - Azure'daki özel ağınızla kurumsal ağlar arasında bir noktadan siteye veya siteden siteye VPN yapılandırın.

* Azure ExpressRoute - Bir ExpressRoute iş ortağı aracılığıyla özel bir bağlantıyı yapılandırın.

Bu seçenekler hakkında ayrıntılı bilgi [için, Internet iletişimi ile ilgili Azure Sanal Ağ belgelerini](../virtual-network/virtual-networks-overview.md#communicate-with-the-internet)okuyun.

### <a name="cluster-controller-as-jump-host"></a>Atlama ana bilgisayar olarak küme denetleyicisi

Küme denetleyicisine ortak bir IP adresi ayarlarsanız, özel alt netin dışından Avere vFXT kümesine başvurmak için atlama ana bilgisayar olarak kullanabilirsiniz. Ancak, denetleyici küme düğümlerini değiştirmek için erişim ayrıcalıkları olduğundan, bu küçük bir güvenlik riski oluşturur.

Genel IP adresine sahip bir denetleyicinin güvenliğini artırmak için dağıtım komut dosyası, yalnızca 22 bağlantı noktasına gelen erişimi kısıtlayan bir ağ güvenlik grubu oluşturur. IP kaynak adresleri aralığınıza erişimi kilitleyerek sistemi daha da koruyabilirsiniz - yani yalnızca küme erişimi için kullanmayı planladığınız makinelerden bağlantılara izin verebilirsiniz.

Küme oluştururken, küme denetleyicisi üzerinde ortak bir IP adresi oluşturup oluşturmayacağını seçebilirsiniz.

* Yeni bir **sanal ağ** veya yeni bir **alt ağ**oluşturursanız, küme denetleyicisi **ortak** bir IP adresi atanır.
* Varolan bir sanal ağ ve alt ağ seçerseniz, küme denetleyicisinin yalnızca **özel** IP adresleri vardır.

## <a name="vm-access-roles"></a>VM erişim rolleri

Azure, küme VM'lerini belirli görevleri gerçekleştirmeye yetki vermek için [rol tabanlı erişim denetimi](../role-based-access-control/index.yml) (RBAC) kullanır. Örneğin, küme denetçisi oluşturmak ve küme düğüm VM'leri yapılandırmak için yetkilendirmeye ihtiyaç duyar. Küme düğümlerinin IP adreslerini diğer küme düğümlerine atayabilmesi veya yeniden atayabilmeleri gerekir.

Avere vFXT sanal makineleri için iki yerleşik Azure rolü kullanılır:

* Küme denetleyicisi yerleşik rolü [Avere Katılımcı](../role-based-access-control/built-in-roles.md#avere-contributor)kullanır.
* Küme düğümleri yerleşik rol [Avere Operator](../role-based-access-control/built-in-roles.md#avere-operator)kullanın.

Avere vFXT bileşenleri için erişim rollerini özelleştirmeniz gerekiyorsa, kendi rolünüzü tanımlamanız ve oluşturuldukları anda VM'lere atamanız gerekir. Azure Marketi'nde dağıtım şablonunu kullanamazsınız. Sisteminizden yardım al'da açıklandığı gibi Azure portalında bir bilet açarak Microsoft Müşteri Hizmetleri ve [Destek'e başvurun.](avere-vfxt-open-ticket.md)

## <a name="next-steps"></a>Sonraki adımlar

[Dağıtıma genel bakış,](avere-vfxt-deploy-overview.md) Azure sistemi için bir Avere vFXT oluşturmak ve verileri sunmaya hazır hale getirmek için gereken adımların büyük resim görünümünü verir.

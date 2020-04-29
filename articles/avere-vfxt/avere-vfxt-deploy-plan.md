---
title: Avere vFXT sisteminizi planlayın-Azure
description: Azure için avere vFXT dağıtılmadan önce yapılacak planlamayı açıklar
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: rohogue
ms.openlocfilehash: 6acc1ffd197ddba4290ff7c0751b259d98a70927
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80754398"
---
# <a name="plan-your-avere-vfxt-system"></a>Avere vFXT sisteminizi planlama

Bu makalede, Azure kümesi için gereksinimlerinize uygun şekilde yerleştirilmiş ve boyutlandırılabilen yeni bir avere vFXT 'nin nasıl planlanacağı açıklanır.

Azure Market 'e geçmeden veya herhangi bir VM oluşturmadan önce Şu ayrıntıları göz önünde bulundurun:

* Küme diğer Azure kaynaklarıyla nasıl etkileşime girecektir?
* Küme öğeleri özel ağlarda ve alt ağlarda nerede bulunuyor?
* Ne tür bir arka uç depolama alanı kullanacaksınız ve küme ona nasıl erişmeli?
* Küme düğümlerinizin iş akışınızı desteklemesi ne kadar güçlü olması gerekir?

Daha fazla bilgi edinmek için okumaya devam edin.

## <a name="learn-the-components-of-the-system"></a>Sistemin bileşenlerini öğrenin

Planlamaya başladığınızda Azure sistemi için avere vFXT 'nin bileşenlerinin anlaşılması yararlı olabilir.

* Küme düğümleri-küme, küme düğümü olarak yapılandırılmış üç veya daha fazla VM 'den oluşur. Daha fazla düğüm, sisteme daha yüksek aktarım hızı ve daha büyük bir önbellek sunar.

* Önbellek-önbellek kapasitesi, küme düğümleri arasında eşit olarak bölünür. Kümeyi oluştururken düğüm başına önbellek boyutunu ayarlama; düğüm boyutları toplam önbellek boyutu olacak şekilde eklenir.

* Küme denetleyicisi-küme denetleyicisi, küme düğümleri ile aynı alt ağda bulunan ek bir VM 'dir. Kümeyi oluşturmak ve devam eden yönetim görevleri için denetleyici gereklidir.

* Arka uç depolama-önbelleğe almak istediğiniz veriler bir donanım depolama sisteminde veya bir Azure Blob kapsayıcısında uzun süreli olarak depolanır. Azure kümesi için avere vFXT oluşturduktan sonra depolama ekleyebilir veya blob depolamayı kullanıyorsanız, kümeyi oluştururken kapsayıcıyı ekleyip yapılandırabilirsiniz.

* İstemciler-önbelleğe alınmış dosyaları kullanan Istemci makineler, depolama sistemlerine doğrudan erişmek yerine sanal bir dosya yolunu kullanarak kümeye bağlanır. ( [Avere vFXT kümesini bağlama](avere-vfxt-mount-clients.md)bölümünde daha fazla bilgi edinin.)

## <a name="subscription-resource-group-and-network-infrastructure"></a>Abonelik, kaynak grubu ve ağ altyapısı

Azure dağıtımı için avere vFXT 'nizin öğelerinin nerede olacağını göz önünde bulundurun. Aşağıdaki diyagramda Azure bileşenleri için avere vFXT için olası bir düzenleme gösterilmektedir:

![Küme denetleyicisinin ve küme VM 'lerinin bir alt ağ içinde gösterildiği diyagram. Alt ağ sınırının etrafında VNET sınırı vardır. VNET 'in içinde, depolama hizmeti uç noktasını temsil eden bir altıdır; VNET dışında bir BLOB depolama alanına kesik çizgili bir okla bağlanır.](media/avere-vfxt-components-option.png)

Avere vFXT kümenizin ağ altyapısını planlarken aşağıdaki yönergeleri izleyin:

* Azure dağıtımı için her avere vFXT için yeni bir abonelik oluşturun. Bu abonelikteki tüm bileşenleri yönetin.

  Her dağıtım için yeni bir abonelik kullanmanın avantajları şunlardır:
  * Daha basit maliyet izleme-bir abonelikte kaynakların, altyapıdan ve işlem döngülerinin tüm maliyetlerini görüntüleyin ve denetleyin.
  * Daha kolay temizlik-proje ile işiniz bittiğinde aboneliğin tamamını kaldırabilirsiniz.
  * Kaynak kotaları için uygun bölümlendirme-diğer kritik iş yüklerini olası kaynak azaltmaya karşı korumak için avere vFXT istemcilerini ve kümesini tek bir abonelikte yalıtın. Bu ayrım, yüksek performanslı bilgi işlem iş akışı için çok sayıda istemci getirilirken çakışmayı önler.

* VFXT kümesine yakın istemci işlem sistemlerinizi bulun. Arka uç depolama daha uzak olabilir.  

* VFXT kümesini ve küme denetleyicisi sanal makinesini birlikte bulun; özellikle şunları yapmanız gerekir:

  * Aynı sanal ağda
  * Aynı kaynak grubunda
  * Aynı depolama hesabını kullanma
  
  Küme oluşturma şablonu çoğu durum için bu yapılandırmayı işler.

* IP adresinin istemcilerle veya diğer işlem kaynaklarıyla çakışmasını önlemek için kümenin kendi alt ağında bulunması gerekir.

* Kaynak grupları, sanal ağlar, alt ağlar ve depolama hesapları dahil olmak üzere küme için gereken altyapı kaynaklarının çoğunu oluşturmak için küme oluşturma şablonunu kullanın.

  Zaten var olan kaynakları kullanmak istiyorsanız, bu tablodaki gereksinimleri karşıladığından emin olun.

  | Kaynak | Mevcut mi? | Gereksinimler |
  |----------|-----------|----------|
  | Kaynak grubu | Boşsa, Evet | Boş olmalıdır|
  | Depolama hesabı | Küme oluşturulduktan sonra var olan bir blob kapsayıcısını bağlıyorsanız **Evet** <br/>  Küme oluşturma sırasında yeni bir blob kapsayıcısı oluşturulmaksam **Hayır** | Mevcut blob kapsayıcısı boş olmalıdır <br/> &nbsp; |
  | Sanal ağ | Yes | Yeni bir Azure Blob kapsayıcısı oluşturulana bir depolama hizmeti uç noktası içermelidir |
  | Alt ağ | Yes | Diğer kaynakları içeremez |

## <a name="ip-address-requirements"></a>IP adresi gereksinimleri

Kümenizin alt ağının, kümeyi desteklemek için yeterince büyük bir IP adresi aralığına sahip olduğundan emin olun.

Avere vFXT kümesi aşağıdaki IP adreslerini kullanır:

* Tek bir küme yönetimi IP adresi. Bu adres, her zaman kullanılabilir olması için düğüm üzerinde kümedeki düğüme taşınabilir. Avere Denetim Masası yapılandırma aracına bağlanmak için bu adresi kullanın.
* Her küme düğümü için:
  * En az bir istemciye yönelik IP adresi. (İstemciye yönelik tüm adresler kümenin *vServer*tarafından yönetilir ve bu, IP adreslerini gerektiği gibi düğümler arasında taşıyabilirler.)
  * Küme iletişimi için bir IP adresi
  * Bir örnek IP adresi (VM 'ye atandı)

Azure Blob depolama kullanırsanız, kümenizin sanal ağından IP adresleri de gerektirebilir:  

* Azure Blob depolama hesabı en az beş IP adresi gerektirir. BLOB depolama alanını kümeniz ile aynı sanal ağda konumlandırdıysanız bu gereksinimi göz önünde bulundurun.
* Kümenin sanal ağı dışında bir Azure Blob depolama alanı kullanırsanız, sanal ağ içinde bir depolama hizmeti uç noktası oluşturun. Uç nokta bir IP adresi kullanmaz.

Kümeden farklı kaynak gruplarında ağ kaynaklarını ve BLOB depolamayı (kullanılıyorsa) bulma seçeneğiniz vardır.

## <a name="vfxt-node-size"></a>vFXT düğüm boyutu

Küme düğümleri olarak görev yapan VM 'Ler, önbelleğinizin istek aktarım hızını ve depolama kapasitesini tespit eder. <!-- The instance type offered has been chosen for its memory, processor, and local storage characteristics. You can choose from two instance types, with different memory, processor, and local storage characteristics. -->

Her vFXT düğümü aynı olacaktır. Diğer bir deyişle, üç düğümlü bir küme oluşturursanız, aynı türde ve boyutta üç sanal makine olur.

| Örnek türü | Sanal çekirdek | Bellek  | Yerel SSD depolaması  | Maksimum veri diskleri | Önbelleğe alınmamış disk aktarım hızı | NIC (sayı) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_E32s_v3 | 32  | 256 GiB | 512 GiB  | 32 | 51.200 ıOPS <br/> 768 MBps | 16.000 MBps (8)  |

Düğüm başına disk önbelleği yapılandırılabilir ve 1000 GB 'den 8000 GB 'a Rage olabilir. düğüm başına 4 TB, Standard_E32s_v3 düğümleri için önerilen önbellek boyutudur.

Bu VM 'Ler hakkında daha fazla bilgi için Microsoft Azure belgeleri okuyun: [bellek için iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)

## <a name="account-quota"></a>Hesap kotası

Aboneliğinizin avere vFXT kümesini çalıştırma kapasitesine ve kullanılan bilgi işlem veya istemci sistemlerine sahip olduğundan emin olun. Ayrıntılar için [vFXT kümesine yönelik kotayı](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster) okuyun.

## <a name="back-end-data-storage"></a>Arka uç veri depolama

Arka uç depolama sistemleri, her iki dosyayı da kümenin önbelleğine sağlar ve aynı zamanda önbellekten değiştirilen verileri alır. Çalışma kümesi 'nin, yeni bir blob kapsayıcısında veya var olan bir depolama sisteminde (bulut veya donanım) uzun vadede saklanıp saklanmayacağına karar verin. Bu arka uç depolama sistemlerine *çekirdek dosyasıları*denir.

### <a name="hardware-core-filers"></a>Donanım çekirdeği dosyasıları

Kümeyi oluşturduktan sonra, vFXT kümesine donanım depolama sistemleri ekleyin. Depolama sistemine kümenin alt ağından ulaşılamadığından, şirket içi sistemler dahil olmak üzere çeşitli popüler donanım sistemleri kullanabilirsiniz.

Avere vFXT kümesine mevcut bir depolama sisteminin nasıl ekleneceği hakkında ayrıntılı yönergeler için [depolama yapılandırma](avere-vfxt-add-storage.md) bölümünü okuyun.

### <a name="cloud-core-filers"></a>Bulut çekirdeği dosyasıları

Azure sistemi için avere vFXT, arka uç depolaması için boş blob kapsayıcıları kullanabilir. Kümeye eklendiğinde kapsayıcılar boş olmalıdır-vFXT sistemi, var olan verileri korumak gerekmeden nesne mağazasını yönetebilmelidir.

> [!TIP]
> Arka uç için Azure Blob depolamayı kullanmak istiyorsanız, vFXT kümesi oluşturmanın bir parçası olarak yeni bir kapsayıcı oluşturun. Küme oluşturma şablonu, yeni bir blob kapsayıcısı oluşturup yapılandırabilir, böylece küme kullanılabilir duruma geldiğinde kullanıma hazır hale gelir. Daha sonra kapsayıcı eklemek daha karmaşıktır.
>
> Ayrıntılar için [Azure Için avere vFXT oluşturma](avere-vfxt-deploy.md#create-the-avere-vfxt-for-azure) makalesini okuyun.

Boş BLOB depolama kapsayıcısını bir çekirdek filme olarak ekledikten sonra, verileri kümeye kopyalayabilir. Paralel, çok iş parçacıklı bir kopyalama mekanizması kullanın. İstemci makinelerini ve avere vFXT önbelleğini kullanarak verilerin kümenin yeni kapsayıcısına etkin bir şekilde kopyalanmasını öğrenmek için [verileri vFXT kümesine taşımayı](avere-vfxt-data-ingest.md) okuyun.

## <a name="cluster-access"></a>Küme erişimi

Azure kümesi için avere vFXT özel bir alt ağda bulunur ve kümenin ortak bir IP adresi yoktur. Küme yönetimi ve istemci bağlantıları için özel alt ağa erişmeniz için bir yola sahip olmanız gerekir.

Erişim seçenekleri şunlardır:

* Ana bilgisayar-özel ağ içindeki ayrı bir VM 'ye genel bir IP adresi atayın ve küme düğümlerine bir TLS tüneli oluşturmak için bunu kullanın.

  > [!TIP]
  > Küme denetleyicisinde genel bir IP adresi ayarlarsanız, bunu bir geçiş ana bilgisayarı olarak kullanabilirsiniz. Daha fazla bilgi için [küme denetleyicisini geçiş ana bilgisayarı olarak](#cluster-controller-as-jump-host) okuyun.

* Sanal özel ağ (VPN)-Azure ve kurumsal ağlardaki özel ağınız arasında noktadan siteye veya siteden siteye VPN yapılandırın.

* Azure ExpressRoute-bir ExpressRoute iş ortağı aracılığıyla özel bir bağlantı yapılandırın.

Bu seçenekler hakkında daha fazla bilgi için [Internet iletişimi hakkındaki Azure sanal ağ belgelerini](../virtual-network/virtual-networks-overview.md#communicate-with-the-internet)okuyun.

### <a name="cluster-controller-as-jump-host"></a>Küme denetleyicisini geçiş ana bilgisayarı olarak

Küme denetleyicisinde genel bir IP adresi ayarlarsanız, özel alt ağın dışından avere vFXT kümesiyle iletişim kurmak için bunu bir bağlantı ana bilgisayarı olarak kullanabilirsiniz. Ancak, denetleyici küme düğümlerini değiştirmek için erişim ayrıcalıklarına sahip olduğundan, bu küçük bir güvenlik riski oluşturur.

Genel IP adresi olan bir denetleyicinin güvenliğini artırmak için, dağıtım betiği otomatik olarak, gelen erişimi yalnızca 22 numaralı bağlantı noktasına kısıtlayan bir ağ güvenlik grubu oluşturur. IP kaynak adreslerinize erişimi kilitleyerek, diğer bir deyişle, yalnızca küme erişimi için kullanmayı düşündüğünüz makinelerden gelen bağlantılara izin vererek sistemi daha da koruyabilirsiniz.

Kümeyi oluştururken, küme denetleyicisinde genel IP adresi oluşturulup oluşturulmayacağını seçebilirsiniz.

* **Yeni bir sanal ağ** veya **Yeni bir alt ağ**oluşturursanız, küme denetleyicisine bir **genel** IP adresi atanır.
* Var olan bir sanal ağı ve alt ağı seçerseniz, küme denetleyicisi yalnızca **özel** IP adreslerine sahip olur.

## <a name="vm-access-roles"></a>VM erişim rolleri

Azure, belirli görevleri gerçekleştirmek üzere küme VM 'Leri yetkilendirmek için [rol tabanlı erişim denetimi](../role-based-access-control/index.yml) 'ni (RBAC) kullanır. Örneğin, küme denetleyicisinin küme düğümü VM 'lerini oluşturmak ve yapılandırmak için yetkilendirmeye ihtiyacı vardır. Küme düğümlerinin IP adreslerini diğer küme düğümlerine atayabilmesi veya yeniden atayabilmeleri gerekir.

Avere vFXT sanal makineleri için iki yerleşik Azure rolü kullanılır:

* Küme denetleyicisi yerleşik [avere katkıda](../role-based-access-control/built-in-roles.md#avere-contributor)bulunan rolünü kullanır.
* Küme düğümleri yerleşik rol [avere işlecini](../role-based-access-control/built-in-roles.md#avere-operator)kullanır.

Avere vFXT bileşenleri için erişim rollerini özelleştirmeniz gerekiyorsa, kendi rolünüzü tanımlamanız ve sonra bunları oluşturuldukları sırada VM 'lere atamanız gerekir. Dağıtım şablonunu Azure Marketi 'nde kullanamazsınız. Azure portal bir bileti açarak, [sisteminizle ilgili yardım alın](avere-vfxt-open-ticket.md)bölümünde açıklandığı gibi, Microsoft Müşteri Hizmetleri ve desteği 'ne başvurun.

## <a name="next-steps"></a>Sonraki adımlar

[Dağıtıma genel bakış](avere-vfxt-deploy-overview.md) , Azure sistemi Için avere vFXT oluşturmak ve verileri sunmaya hazırlamak için gereken adımların büyük resim görünümünü sağlar.

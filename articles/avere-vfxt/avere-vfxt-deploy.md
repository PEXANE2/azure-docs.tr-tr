---
title: Azure için avere vFXT dağıtma
description: Azure 'da avere vFXT kümesini dağıtma adımları
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: e70d1dfebcf25ee8f4e90a062cee6dd72a663e02
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76547532"
---
# <a name="deploy-the-vfxt-cluster"></a>vFXT kümesini dağıtma

Bu yordam, Azure Marketi 'nden sunulan Dağıtım Sihirbazı 'nı kullanarak size kılavuzluk eder. Sihirbaz Azure Resource Manager şablonu kullanarak kümeyi otomatik olarak dağıtır. Forma parametreleri girdikten ve **Oluştur**' a tıkladıktan sonra, Azure bu görevleri otomatik olarak tamamlar:

* Kümeyi dağıtmak ve yönetmek için gereken yazılımları içeren temel bir VM olan küme denetleyicisini oluşturur.
* Yeni öğe oluşturma da dahil olmak üzere kaynak grubu ve sanal ağ altyapısını ayarlar.
* Küme düğümü sanal makinelerini oluşturur ve avere kümesi olarak yapılandırır.
* İsteniyorsa, yeni bir Azure Blob kapsayıcısı oluşturur ve bunu bir küme çekirdeği fili olarak yapılandırır.

Bu belgedeki yönergeleri uyguladıktan sonra, aşağıdaki diyagramda gösterildiği gibi bir sanal ağ, bir alt ağ, bir küme denetleyicisi ve bir vFXT kümesi olacaktır. Bu diyagramda yeni bir BLOB depolama kapsayıcısı (yeni bir depolama hesabında, gösterilmez) ve alt ağ içinde Microsoft Storage için bir hizmet uç noktası içeren isteğe bağlı Azure Blob çekirdek filleyicisi gösterilmektedir.

![avere küme bileşenleriyle üç eşmerkezli dikdörtgeni gösteren diyagram. Dış dikdörtgen ' Resource Group ' olarak etiketlidir ve bir altıon etiketli ' Blob depolama alanı (isteğe bağlı) ' içeriyor. ' Deki sonraki dikdörtgen ' sanal ağ: 10.0.0.0/16 ' etiketlidir ve hiçbir benzersiz bileşen içermez. En içteki dikdörtgen ' subnet: 10.0.0.0/24 ' olarak etiketlidir ve ' küme denetleyicisi ' etiketli bir VM, ' vFXT düğümleri (vFXT kümesi) ' etiketli üç VM 'nin bir yığını ve ' hizmet uç noktası ' etiketli bir altıon ' ı içerir. Hizmet uç noktasını (alt ağ içinde olan) ve BLOB depolama alanını (kaynak grubunda alt ağ ve VNET dışında) bağlayan bir ok vardır. Ok, alt ağ ve sanal ağ sınırları üzerinden geçer.](media/avere-vfxt-deployment.png)

Oluşturma şablonunu kullanmadan önce, şu önkoşullara değindiğinizden emin olun:  

* [Yeni abonelik](avere-vfxt-prereqs.md#create-a-new-subscription)
* [Abonelik sahibi izinleri](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
* [VFXT kümesi için kota](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)
* [Depolama hizmeti uç noktası (gerekirse)](avere-vfxt-prereqs.md#create-a-storage-service-endpoint-in-your-virtual-network-if-needed) -var olan bir sanal ağı kullanan ve BLOB depolama alanı oluşturan dağıtımlar için gereklidir

Küme dağıtım adımları ve planlaması hakkında daha fazla bilgi için [avere vFXT sisteminizin](avere-vfxt-deploy-plan.md) ve [dağıtıma genel bakış](avere-vfxt-deploy-overview.md)konusunu okuyun.

## <a name="create-the-avere-vfxt-for-azure"></a>Azure için avere vFXT 'yi oluşturma

Avere araması yaparak Azure portal oluşturma şablonuna erişin ve "Azure ARM şablonu için avere vFXT" seçeneğini seçin.

![Crumbs "Yeni > Market > her şeyi Azure portal gösteren tarayıcı penceresi. Her şey sayfasında, arama alanı "avere" terimini ve ikinci sonuç olarak "avere vFXT for Azure ARM şablonu", vurgulamak için kırmızı renkle özetlenmektedir.](media/avere-vfxt-template-choose.png)

Azure ARM şablonu için avere vFXT sayfasında ayrıntıları okuduktan sonra başlamak için **Oluştur** düğmesine tıklayın.

![Dağıtım şablonunun ilk sayfasıyla Azure Marketi](media/avere-vfxt-deploy-first.png)

Şablon dört adımdan ayrılır-iki bilgi toplama sayfası, ayrıca doğrulama ve onay adımları.

* Tek sayfa, küme denetleyicisi sanal makinesinin ayarlarını toplar.
* İkinci sayfa, küme oluşturmak için parametreleri ve alt ağlar ve depolama gibi ek kaynakları toplar.
* Üç sayfa, seçimlerinizi özetler ve yapılandırmayı doğrular.
* Sayfa dört, yazılım hüküm ve koşullarını açıklar ve küme oluşturma işlemini başlatabilmeniz için izin verir.

## <a name="page-one-parameters---cluster-controller-information"></a>Sayfa bir parametre-küme denetleyicisi bilgileri

Dağıtım şablonunun ilk sayfası, küme denetleyicisine odaklanır.

![Dağıtım şablonunun ilk sayfası](media/avere-vfxt-deploy-1.png)

Aşağıdaki bilgileri girin:

* **Küme denetleyicisi adı** -küme denetleyicisi sanal makinesinin adını ayarlayın.

* **Denetleyici Kullanıcı adı** -küme denetleyicisi sanal makinesinin kök kullanıcı adını ayarlayın.

* **Kimlik doğrulama türü** -denetleyiciye bağlanmak için parola veya SSH ortak anahtar kimlik doğrulaması ' nı seçin. SSH ortak anahtar yöntemi önerilir; yardıma ihtiyacınız varsa [SSH anahtarlarını oluşturma ve kullanma hakkında](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) bilgi edinin.

* **Parola** veya **SSH ortak anahtarı** -seçtiğiniz kimlik doğrulama türüne bağlı olarak, bir RSA ortak anahtarı veya bir parolayı bir sonraki alanlarda belirtmeniz gerekir. Bu kimlik bilgisi, daha önce sağlanmış olan kullanıcı adıyla birlikte kullanılır.

* **Abonelik** -avere vFXT için aboneliği seçin.

* **Kaynak grubu** -avere vFXT kümesi için mevcut bir boş kaynak grubu seçin veya "Yeni oluştur" a tıklayın ve yeni bir kaynak grubu adı girin.

* **Konum** -kümeniz ve kaynaklarınız için Azure konumunu seçin.

Bittiğinde **Tamam** ' a tıklayın.

> [!NOTE]
> Küme denetleyicisinin herkese açık bir IP adresi olmasını istiyorsanız, var olan bir ağ seçmek yerine küme için yeni bir sanal ağ oluşturun. Bu ayar iki sayfada bulunur.

## <a name="page-two-parameters---vfxt-cluster-information"></a>Sayfa iki parametre-vFXT küme bilgileri

Dağıtım şablonunun ikinci sayfası, küme boyutunu, düğüm türünü, önbellek boyutunu ve depolama parametrelerini diğer ayarlar arasında ayarlamanıza olanak sağlar.

![Dağıtım şablonunun ikinci sayfası](media/avere-vfxt-deploy-2.png)

* **Avere vFXT küme düğümü sayısı** -kümedeki düğüm sayısını seçin. En az üç düğüm ve en yüksek değer on iki.

* **Küme yönetimi parolası** -küme yönetimi için parola oluşturun. Bu parola, küme Denetim Masası 'nda oturum açmak için ```admin```, kümeyi izleyip küme ayarlarını yapılandırabileceğiniz Kullanıcı adı ile kullanılır.

* **Avere vFXT küme adı** -kümeye benzersiz bir ad verin.

* **Boyut** -Bu bölüm, küme düğümleri IÇIN kullanılacak VM türünü gösterir. Yalnızca önerilen bir seçenek olsa da, **değişiklik boyutu** bağlantısı bu örnek türüyle ilgili ayrıntıları ve fiyat Hesaplayıcı bağlantısını içeren bir tablo açar.

* **Düğüm başına önbellek boyutu** -küme önbelleği küme düğümlerine yayılır, bu nedenle avere vFXT kümenizdeki toplam önbellek boyutu bu boyut, düğüm sayısıyla çarpılacak.

  Önerilen yapılandırma: Standard_E32s_v3 düğümleri için düğüm başına 4 TB kullanın.

* **Sanal ağ** -kümeyi barındırmak için yeni bir sanal ağ tanımlayın veya [avere Vfxt sisteminizi planlayın](avere-vfxt-deploy-plan.md#subscription-resource-group-and-network-infrastructure)bölümünde açıklanan önkoşulları karşılayan mevcut bir ağı seçin.

  > [!NOTE]
  > Yeni bir sanal ağ oluşturursanız, yeni özel ağa erişebilmek için küme denetleyicisinin ortak bir IP adresi olur. Var olan bir sanal ağı seçerseniz, küme denetleyicisi genel bir IP adresi olmadan yapılandırılır.
  >
  > Küme denetleyicisindeki genel olarak görünen bir IP adresi, vFXT kümesine daha kolay erişim sağlar, ancak küçük bir güvenlik riski oluşturur.
  >* Küme denetleyicisindeki genel bir IP adresi, özel alt ağın dışından avere vFXT kümesine bağlanmak için bir geçiş ana bilgisayarı olarak kullanmanıza olanak tanır.
  >* Denetleyicide genel IP adresiniz yoksa, kümeye erişmek için başka bir geçiş ana bilgisayarı, bir VPN bağlantısı veya ExpressRoute gerekir. Örneğin, zaten yapılandırılmış bir VPN bağlantısına sahip olan mevcut bir sanal ağı kullanın.
  >* Genel IP adresi olan bir denetleyici oluşturursanız, denetleyici VM 'sini bir ağ güvenlik grubuyla korumanız gerekir. Varsayılan olarak, Azure dağıtımı için avere vFXT, gelen erişimi yalnızca genel IP adreslerine sahip denetleyiciler için 22 numaralı bağlantı noktasına kısıtlayan bir ağ güvenlik grubu oluşturur. IP kaynak adreslerinize erişimi kilitleyerek, diğer bir deyişle, yalnızca küme erişimi için kullanmayı düşündüğünüz makinelerden gelen bağlantılara izin vererek sistemi daha da koruyabilirsiniz.

  Yeni bir sanal ağ Ayrıca, Azure Blob depolama için bir depolama hizmeti uç noktası ile yapılandırılır ve ağ erişim denetimi, küme alt ağından yalnızca IP 'Lere izin verecek şekilde kilitlenir.

* **Alt ağ** -bir alt ağ seçin veya yeni bir tane oluşturun.

* **BLOB depolama oluşturma ve kullanma** -yeni bir Azure Blob kapsayıcısı oluşturmak ve bunu yeni avere vFXT kümesi için arka uç depolama olarak yapılandırmak için **true** seçeneğini belirleyin. Bu seçenek Ayrıca kümenin kaynak grubunda yeni bir depolama hesabı oluşturur ve küme alt ağı içinde bir Microsoft Storage Service uç noktası oluşturur.
  
  Var olan bir sanal ağı sağlarsanız, kümeyi oluşturmadan önce bir depolama hizmeti uç noktası olması gerekir. (Daha fazla bilgi için [avere vFXT sisteminizin planını](avere-vfxt-deploy-plan.md)okuyun.)

  Yeni bir kapsayıcı oluşturmak istemiyorsanız bu alanı **false** olarak ayarlayın. Bu durumda, kümeyi oluşturduktan sonra depolamayı iliştirmeli ve yapılandırmalısınız. Yönergeler için [depolamayı yapılandırma](avere-vfxt-add-storage.md) makalesini okuyun.

* **(Yeni) depolama hesabı** -yeni bir Azure Blob kapsayıcısı oluşturuyorsanız, yeni depolama hesabı için bir ad girin.

## <a name="validation-and-purchase"></a>Doğrulama ve satın alma

Üç sayfa yapılandırmayı özetler ve parametreleri doğrular. Doğrulama başarılı olduktan sonra, Özeti denetleyin ve **Tamam** düğmesine tıklayın.

> [!TIP]
> **Tamam** düğmesinin yanındaki **şablon ve parametreleri indir** bağlantısına tıklayarak, bu kümenin oluşturma ayarlarını kaydedebilirsiniz. Bu bilgiler daha sonra benzer bir küme oluşturmanız gerekiyorsa (örneğin, bir olağanüstü durum kurtarma senaryosunda bir değiştirme kümesi oluşturmak için) yararlı olabilir. (Daha fazla bilgi edinmek için [olağanüstü durum kurtarma kılavuzunu](disaster-recovery.md) okuyun.)

![Dağıtım şablonunun üçüncü sayfası-doğrulama](media/avere-vfxt-deploy-3.png)

Sayfa dördü, kullanım koşullarını ve gizlilik ve fiyatlandırma bilgilerine yönelik bağlantıları sağlar.

Eksik iletişim bilgilerini girin, ardından **Oluştur** düğmesine tıklayarak koşulları kabul edin ve Azure kümesi Için avere vFXT 'yi oluşturun.

![Dağıtım şablonunun dördüncü sayfası-hüküm ve koşullar, Oluştur düğmesi](media/avere-vfxt-deploy-4.png)

Küme dağıtımı 15-20 dakika sürer.

## <a name="gather-template-output"></a>Şablon çıktısını topla

Avere vFXT şablonu kümeyi oluşturmayı bitirdiğinde, yeni küme hakkında önemli bilgiler verir.

> [!TIP]
> Şablon çıktısından **YÖNETIM IP adresini** kopyalamadığınızdan emin olun. Kümeyi yönetmek için bu adrese ihtiyacınız vardır.

Bilgileri bulmak için:

1. Küme denetleyicinizin kaynak grubuna gidin.

1. Sol tarafta **dağıtımlar**' a ve ardından **Microsoft-avere. vfxt-Template**' e tıklayın.

   ![Solda ve Microsoft-avere. vfxt-Template seçili dağıtımlar içeren kaynak grubu portalı sayfası dağıtım adı altındaki bir tabloda gösteriliyor](media/avere-vfxt-outputs-deployments.png)

1. Sol tarafta, **çıktılar**' e tıklayın. Her alandaki değerleri kopyalayın.

   ![başlıkların sağ tarafındaki alanlarda SSHSTRING, RESOURCE_GROUP, konum, NETWORK_RESOURCE_GROUP, ağ, alt ağ, SUBNET_ID, VSERVER_IPs ve MGMT_IP değerlerini gösteren sayfa çıkışları](media/avere-vfxt-outputs-values.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık küme çalışıyor olduğuna ve yönetim IP adresini bildiğinize göre, [küme yapılandırma aracına bağlanın](avere-vfxt-cluster-gui.md).

Aşağıdaki kurulum görevleri dahil olmak üzere kümenizi özelleştirmek için yapılandırma arabirimini kullanın:

* [Desteği etkinleştir](avere-vfxt-enable-support.md)
* [Depolama Ekle](avere-vfxt-add-storage.md) (gerekirse)

---
title: Azure için Avere vFXT dağıtma
description: Azure'da Avere vFXT kümesini dağıtma adımları
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: e70d1dfebcf25ee8f4e90a062cee6dd72a663e02
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252603"
---
# <a name="deploy-the-vfxt-cluster"></a>vFXT kümesini dağıtma

Bu yordam, Azure Marketi'nden kullanılabilen dağıtım sihirbazını kullanmanızı sağlar. Sihirbaz, Azure Kaynak Yöneticisi şablonu kullanarak kümeyi otomatik olarak dağıtır. Parametreleri forma girdikten ve **Oluştur'u**tıklattıktan sonra Azure bu görevleri otomatik olarak tamamlar:

* Kümedağıtmak ve yönetmek için gereken yazılımı içeren temel bir VM olan küme denetleyicisini oluşturur.
* Yeni öğeler oluşturmak da dahil olmak üzere kaynak grubu ve sanal ağ altyapısı ayarlar.
* Küme düğümü VM'leri oluşturur ve bunları Avere kümesi olarak yapılandırır.
* İstenirse, yeni bir Azure Blob kapsayıcısı oluşturur ve küme çekirdeği filer olarak yapılandırır.

Bu belgedeki yönergeleri izleyerek, aşağıdaki diyagramda gösterildiği gibi bir sanal ağa, bir alt ağa, küme denetleyiciye ve bir vFXT kümesine sahip olursunuz. Bu diyagram, yeni bir Blob depolama kapsayıcısı (yeni bir depolama hesabında gösterilmez) ve alt ağ içindeki Microsoft depolama alanı için bir hizmet bitiş noktası içeren isteğe bağlı Azure Blob çekirdek filer'ini gösterir.

![Avere küme bileşenleri ile üç eşmerkezli dikdörtgenler gösteren diyagram. Dış dikdörtgen 'Kaynak grubu' olarak etiketlenir ve 'Blob depolama (isteğe bağlı)' etiketli bir altıgen içerir. Bir sonraki dikdörtgen 'Sanal ağ: 10.0.0.0/16' olarak etiketlenir ve benzersiz bileşenler içermez. En içteki dikdörtgen 'Subnet:10.0.0.0/24' olarak etiketlenir ve 'Küme denetleyicisi', 'vFXT düğümleri (vFXT kümesi)' etiketli üç VM yığını ve 'Hizmet bitiş noktası' etiketli bir altıgen içerir. Hizmet bitiş noktasını (alt ağ içinde) ve blob depolamasını (kaynak grubunda alt ağ ve vnet'in dışında) bağlayan bir ok vardır. Ok alt ağ ve sanal ağ sınırlarından geçer.](media/avere-vfxt-deployment.png)

Oluşturma şablonunu kullanmadan önce, aşağıdaki ön koşulları ele aldığınızdan emin olun:  

* [Yeni abonelik](avere-vfxt-prereqs.md#create-a-new-subscription)
* [Abonelik sahibi izinleri](avere-vfxt-prereqs.md#configure-subscription-owner-permissions)
* [vFXT kümesi için kota](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)
* [Depolama hizmeti bitiş noktası (gerekirse)](avere-vfxt-prereqs.md#create-a-storage-service-endpoint-in-your-virtual-network-if-needed) - Varolan bir sanal ağ kullanan ve blob depolama alanı oluşturan dağıtımlar için gereklidir

Küme dağıtım adımları ve planlama hakkında daha fazla bilgi [için, Avere vFXT sisteminizi](avere-vfxt-deploy-plan.md) ve [Dağıtıma genel bakışı](avere-vfxt-deploy-overview.md)planlayın'ı okuyun.

## <a name="create-the-avere-vfxt-for-azure"></a>Azure için Avere vFXT oluşturma

Avere'i arayarak ve "Azure ARM Şablonu için Avere vFXT" seçeneğini seçerek Azure portalındaki oluşturma şablonuna erişin.

![Ekmek kırıntıları ile Azure portalını gösteren tarayıcı penceresi "Her şey > Yeni > Marketplace". Her şey sayfasında, arama alanında "avere" terimi vardır ve ikinci sonuç, "Azure ARM Şablonu için Avere vFXT" vurgulamak için kırmızı ile özetlenmiştir.](media/avere-vfxt-template-choose.png)

Azure ARM Şablonu için Avere vFXT'deki ayrıntıları okuduktan sonra başlamak için **Oluştur** düğmesini tıklatın.

![Dağıtım şablonunun ilk sayfasının gösterilmesiile Azure pazar](media/avere-vfxt-deploy-first.png)

Şablon dört adıma ayrılmıştır - iki bilgi toplama sayfası, artı doğrulama ve onay adımları.

* Birinci sayfa küme denetleyicisi VM için ayarları toplar.
* Sayfa iki küme oluşturmak için parametreleri ve alt ağlar ve depolama gibi ek kaynakları toplar.
* Üçüncü sayfa seçeneklerinizi özetler ve yapılandırmayı doğrular.
* Dördüncü sayfa yazılım hüküm ve koşullarını açıklar ve küme oluşturma işlemini başlatmanızı sağlar.

## <a name="page-one-parameters---cluster-controller-information"></a>Birinci sayfa parametreleri - küme denetleyici bilgileri

Dağıtım şablonunun ilk sayfası küme denetleyicisi üzerinde odaklanır.

![Dağıtım şablonunun ilk sayfası](media/avere-vfxt-deploy-1.png)

Aşağıdaki bilgileri doldurun:

* **Küme denetleyici adı** - Küme denetleyicisi VM'nin adını ayarlayın.

* **Denetleyici kullanıcı adı** - Küme denetleyicisi VM'nin kök kullanıcı adını ayarlayın.

* **Kimlik doğrulama türü** - Denetleyiciye bağlanmak için parola veya SSH ortak anahtar kimlik doğrulamasını seçin. SSH ortak anahtar yöntemi önerilir; yardıma ihtiyacınız varsa [SSH tuşlarının nasıl oluşturulup kullanılacağını](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows) okuyun.

* **Parola** veya **SSH ortak anahtarı** - Seçtiğiniz kimlik doğrulama türüne bağlı olarak, sonraki alanlarda bir RSA ortak anahtarı veya parola sağlamanız gerekir. Bu kimlik bilgisi, daha önce sağlanan kullanıcı adı ile kullanılır.

* **Abonelik** - Avere vFXT aboneliğini seçin.

* **Kaynak grubu** - Avere vFXT kümesi için varolan boş bir kaynak grubu seçin veya "Yeni oluştur" seçeneğini tıklayıp yeni bir kaynak grubu adı girin.

* **Konum** - Kümeniz ve kaynaklarınız için Azure konumunu seçin.

Bittiğinde **Tamam'ı** tıklatın.

> [!NOTE]
> Küme denetleyicisinin genel kullanıma açık bir IP adresine sahip olmasını istiyorsanız, varolan bir ağı seçmek yerine küme için yeni bir sanal ağ oluşturun. Bu ayar ikinci sayfada.

## <a name="page-two-parameters---vfxt-cluster-information"></a>Sayfa iki parametre - vFXT küme bilgileri

Dağıtım şablonunun ikinci sayfası, diğer ayarların yanı sıra küme boyutunu, düğüm türünü, önbellek boyutunu ve depolama parametrelerini ayarlamanızı sağlar.

![Dağıtım şablonunun ikinci sayfası](media/avere-vfxt-deploy-2.png)

* **Avere vFXT küme düğümü sayısı** - Kümedeki düğüm sayısını seçin. En az üç düğüm ve en fazla on ikidir.

* **Küme yönetimi parolası** - Küme yönetimi için parola oluşturun. Bu parola, küme denetim ```admin``` panelinde oturum açarak kümeyi izleyip küme ayarlarını yapılandırabileceğiniz kullanıcı adı ile birlikte kullanılır.

* **Avere vFXT küme adı** - Kümeye benzersiz bir ad verin.

* **Boyut** - Bu bölümde küme düğümleri için kullanılacak VM türü gösterilmektedir. Önerilen tek bir seçenek olmasına rağmen, **Boyut Değiştir** bağlantısı bu örnek türü ve fiyat hesap layıcısına bağlantı hakkında ayrıntıları içeren bir tablo açar.

* **Düğüm başına önbellek boyutu** - Küme önbelleği küme düğümlerine yayılır, böylece Avere vFXT kümenizdeki toplam önbellek boyutu düğüm sayısıyla çarpılır.

  Önerilen yapılandırma: Standard_E32s_v3 düğümleri için düğüm başına 4 TB kullanın.

* **Sanal ağ** - Kümeyi barındıracak yeni bir sanal ağ tanımlayın veya [Avere vFXT sisteminizi](avere-vfxt-deploy-plan.md#subscription-resource-group-and-network-infrastructure)Planla'da açıklanan ön koşulları karşılayan varolan bir ağ seçin.

  > [!NOTE]
  > Yeni bir sanal ağ oluşturursanız, küme denetleyicisinin yeni özel ağa erişebilmeniz için ortak bir IP adresi olur. Varolan bir sanal ağ seçerseniz, küme denetleyicisi ortak bir IP adresi olmadan yapılandırılır.
  >
  > Küme denetleyicisi üzerinde genel olarak görülebilen bir IP adresi vFXT kümesine daha kolay erişim sağlar, ancak küçük bir güvenlik riski oluşturur.
  >* Küme denetleyicisindeki genel bir IP adresi, özel alt netin dışından Avere vFXT kümesine bağlanmak için atlama ana bilgisayar olarak kullanmanıza olanak tanır.
  >* Denetleyicide ortak bir IP adresiniz yoksa, kümeye erişmek için başka bir atlama ana bilgisayarı, VPN bağlantısı veya ExpressRoute gerekir. Örneğin, VPN bağlantısı zaten yapılandırılmış olan varolan bir sanal ağı kullanın.
  >* Ortak IP adresine sahip bir denetleyici oluşturursanız, denetleyici VM'yi bir ağ güvenlik grubuyla korumanız gerekir. Varsayılan olarak, Azure dağıtımı için Avere vFXT, gelen erişimi yalnızca ortak IP adreslerine sahip denetleyiciler için 22 bağlantı noktasına kısıtlayan bir ağ güvenlik grubu oluşturur. IP kaynak adresleri aralığınıza erişimi kilitleyerek sistemi daha da koruyabilirsiniz - yani yalnızca küme erişimi için kullanmayı planladığınız makinelerden bağlantılara izin verebilirsiniz.

  Yeni bir sanal ağ, Azure Blob depolama alanı için bir depolama hizmeti bitiş noktası yla ve küme alt ağından yalnızca IP'lere izin verecek şekilde ağ erişim denetimi kilitli yken de yapılandırılır.

* **Subnet** - Bir alt ağ seçin veya yeni bir ağ oluşturun.

* **Blob depolama alanı oluşturun ve kullanın** - Yeni bir Azure Blob kapsayıcısı oluşturmak ve yeni Avere vFXT kümesi için arka uç depolama alanı olarak yapılandırmak için **true'yu** seçin. Bu seçenek ayrıca kümenin kaynak grubunda yeni bir depolama hesabı oluşturur ve küme alt ağı içinde bir Microsoft depolama hizmeti bitiş noktası oluşturur.
  
  Varolan bir sanal ağ sağlıyorsanız, kümeoluşturmadan önce bir depolama hizmeti bitiş noktası olması gerekir. (Daha fazla bilgi [için, Avere vFXT sisteminizi planlayın](avere-vfxt-deploy-plan.md)okuyun.)

  Yeni bir kapsayıcı oluşturmak istemiyorsanız bu alanı **false** olarak ayarlayın. Bu durumda, kümeoluşturduktan sonra depolama eklemeniz ve yapılandırmanız gerekir. Talimatlar için [Depolamayı Yapıla'yı](avere-vfxt-add-storage.md) okuyun.

* **(Yeni) Depolama hesabı** - Yeni bir Azure Blob kapsayıcısı oluşturuyorsanız, yeni depolama hesabı için bir ad girin.

## <a name="validation-and-purchase"></a>Doğrulama ve satın alma

Üçüncü sayfa yapılandırmayı özetler ve parametreleri doğrular. Doğrulama başarılı olduktan sonra, özeti kontrol edin ve **Tamam** düğmesini tıklatın.

> [!TIP]
> **Tamam** düğmesinin yanındaki **İndir şablonu ve parametreler** bağlantısını tıklatarak bu kümenin oluşturma ayarlarını kaydedebilirsiniz. Bu bilgiler, daha sonra benzer bir küme oluşturmanız gerekiyorsa yardımcı olabilir - örneğin, olağanüstü durum kurtarma senaryosunda yedek küme oluşturmak için. (Daha fazla bilgi edinmek için [Olağanüstü Durum kurtarma kılavuzunu](disaster-recovery.md) okuyun.)

![Dağıtım şablonunun üçüncü sayfası - doğrulama](media/avere-vfxt-deploy-3.png)

Dördüncü sayfa, kullanım koşullarını ve gizlilik ve fiyatlandırma bilgilerine bağlantılar verir.

Eksik kişi bilgilerini girin, ardından koşulları kabul etmek ve Azure kümesi için Avere vFXT oluşturmak için **Oluştur** düğmesini tıklatın.

![Dağıtım şablonunun dördüncü sayfası - şartlar ve koşullar, oluşturma düğmesi](media/avere-vfxt-deploy-4.png)

Küme dağıtımı 15-20 dakika sürer.

## <a name="gather-template-output"></a>Şablon çıktısını toplama

Avere vFXT şablonu küme oluşturmayı bitirdiğinde, yeni küme hakkında önemli bilgiler çıkar.

> [!TIP]
> **Yönetici IP adresini** şablon çıktısından kopyaladığından emin olun. Kümeyi yönetmek için bu adrese ihtiyacınız var.

Bilgileri bulmak için:

1. Küme denetleyiciniz için kaynak grubuna gidin.

1. Sol **tarafta, Dağıtımlar'ı**tıklatın ve ardından **microsoft-avere.vfxt-template'i**tıklatın.

   ![Solda seçilen Dağıtımlar ve Dağıtım adı altında bir tabloda gösterilen microsoft-avere.vfxt şablonu içeren kaynak grubu portal sayfası](media/avere-vfxt-outputs-deployments.png)

1. Sol tarafta, **Çıktılar'ı**tıklatın. Alanların her birinde değerleri kopyalayın.

   ![etiketlerin sağındaki alanlardaKi SSHSTRING, RESOURCE_GROUP, LOCATION, NETWORK_RESOURCE_GROUP, NETWORK, SUBNET, SUBNET_ID, VSERVER_IPs ve MGMT_IP değerlerini gösteren çıktılar sayfası](media/avere-vfxt-outputs-values.png)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi küme çalışıyor ve yönetim IP adresini biliyorum, [küme yapılandırma aracına bağlayın.](avere-vfxt-cluster-gui.md)

Bu kurulum görevleri de dahil olmak üzere kümenizi özelleştirmek için yapılandırma arabirimini kullanın:

* [Desteği etkinleştirme](avere-vfxt-enable-support.md)
* [Depolama alanı ekleme](avere-vfxt-add-storage.md) (gerekirse)

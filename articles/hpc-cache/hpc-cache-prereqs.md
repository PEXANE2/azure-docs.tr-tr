---
title: Azure HPC önbellek önkoşulları
description: Azure HPC önbelleğini kullanma önkoşulları
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 08/30/2019
ms.author: v-erkell
ms.openlocfilehash: 7bd1268784df50168e03673ac809b3a5854e2be7
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70775344"
---
# <a name="prerequisites-for-azure-hpc-cache"></a>Azure HPC önbelleği önkoşulları

Yeni bir Azure HPC önbelleği oluşturmak için Azure portal kullanmadan önce ortamınızın bu gereksinimleri karşıladığından emin olun.

## <a name="azure-subscription"></a>Azure aboneliği

Ücretli bir abonelik önerilir.

> [!NOTE]
> Genel önizleme sürümü sırasında, Azure HPC önbellek ekibinin, bir önbellek örneği oluşturmak için kullanılmadan önce aboneliğinizi erişim listesine eklemesi gerekir. Bu yordam, her müşterinin test Önbelleklerinden yüksek kaliteli yanıt hızını aldığından emin olmanıza yardımcı olur. Erişim istemek için [Bu formu](https://aka.ms/onboard-hpc-cache) doldurun.

## <a name="network-infrastructure"></a>Ağ altyapısı

Önbelleğinizi kullanabilmeniz için ağ ile ilgili iki seçenek ayarlanmalıdır:

* Azure HPC önbellek örneği için ayrılmış alt ağ
* Önbelleğin depolamaya ve diğer kaynaklara erişebilmesi için DNS desteği

### <a name="cache-subnet"></a>Önbellek alt ağı

Azure HPC önbelleği, bu kaliteleri olan ayrılmış bir alt ağa ihtiyaç duyuyor:

* Alt ağda en az 64 IP adresi kullanılabilir olmalıdır.
* Alt ağ, istemci makineler gibi ilgili hizmetler için bile diğer VM 'Leri barındıramaz.
* Birden çok önbellek örneği kullanırsanız, her birinin kendi alt ağına ihtiyacı vardır.

En iyi uygulama, önbellek için yeni bir alt ağ oluşturmaktır. Önbellek oluşturmanın bir parçası olarak yeni bir sanal ağ ve alt ağ oluşturabilirsiniz.

### <a name="dns-access"></a>DNS erişimi

Azure HPC önbelleğinin, sanal ağı dışındaki kaynaklara erişmesi için DNS gerekir. Kullandığınız kaynaklara bağlı olarak, özelleştirilmiş bir DNS sunucusu ayarlamanız ve bu sunucu ile Azure DNS sunucuları arasında iletmeyi yapılandırmanız gerekebilir: 

* Azure Blob depolama uç noktalarına ve diğer iç kaynaklara erişmek için, Azure tabanlı DNS sunucusuna ihtiyacınız vardır.
* Şirket içi depolamaya erişmek için, depolama ana bilgisayar adlarını çözümleyebilecek özel bir DNS sunucusu yapılandırmanız gerekir.

Yalnızca blob depolamaya erişmeniz gerekiyorsa önbelleğiniz için Azure tarafından sağlanmış varsayılan DNS sunucusunu kullanabilirsiniz. Ancak, diğer kaynaklara erişmeniz gerekiyorsa, özel bir DNS sunucusu oluşturmanız ve Azure 'a özgü tüm çözümleme isteklerini Azure DNS sunucusuna iletecek şekilde yapılandırmanız gerekir. (Basit bir DNS sunucusu Ayrıca, tüm kullanılabilir önbellek bağlama noktaları arasında istemci bağlantılarının yükünü dengelemek için de kullanılabilir.)

Azure sanal ağları ve DNS sunucusu konfigürasyonları hakkında daha fazla bilgi [için bkz. Azure sanal ağlardaki kaynaklar Için ad çözümlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances).

## <a name="permissions"></a>İzinler

Önbelleğinizi oluşturmaya başlamadan önce Bu izinle ilgili önkoşulları denetleyin.

* Azure HPC önbelleğinin sanal ağ arabirimlerini (NIC 'ler) oluşturabiliyor olması gerekir. Önbelleği oluşturan kullanıcının, NIC 'ler oluşturmak için abonelikte yeterli ayrıcalıklara sahip olması gerekir.
<!-- There are several ways to authorize this access; read [Additional prerequisites](media/preview-prereqs.md) to learn more. -->

* BLOB depolama kullanılıyorsa, Azure HPC önbellek örneğinin depolama hesabınıza erişmesi için yetkilendirme gerekir. Blob depolamaya önbellek erişimi sağlamak için rol tabanlı erişim denetimi 'ni (RBAC) kullanabilirsiniz. İki rol gereklidir: Depolama hesabı katılımcısı ve Depolama Blobu veri katılımcısı. [Önbelleğe depolama ekleme](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account)bölümündeki yönergeleri izleyin.

## <a name="storage-infrastructure"></a>Depolama altyapısı

Önbellek, Azure Blob kapsayıcılarını veya NFS donanım depolama dışarı aktarmaları destekler. Önbelleği oluştururken depolama hedeflerini tanımlayabilirsiniz, ancak bunları daha sonra da ekleyebilirsiniz. 

### <a name="nfs-storage-requirements"></a>NFS depolama gereksinimleri

Şirket içi donanım depolama kullanılıyorsa, önbelleğin alt ağından veri merkezine yüksek bant genişliğine sahip ağ erişimi olması gerekir. [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) veya benzer erişim önerilir.

NFS arka uç depolaması, uyumlu bir donanım/yazılım platformu olmalıdır. Ayrıntılar için Azure HPC önbellek ekibine başvurun.

### <a name="blob-storage-requirements"></a>BLOB depolama gereksinimleri

Azure bellek depolama 'yı Azure HPC önbelleğiyle birlikte kullanmak istiyorsanız, uyumlu bir depolama hesabı ve boş bir blob kapsayıcısı ya da [verileri Azure Blob depolama alanına taşıma](hpc-cache-ingest.md)bölümünde açıklandığı gıbı Azure HPC önbelleği biçimli verilerle doldurulmuş bir kapsayıcı gerekir.

Bir depolama hedefi olarak eklemeyi denemeden önce hesabı ve kapsayıcıyı oluşturun.

Uyumlu bir depolama hesabı oluşturmak için şu ayarları kullanın:

* Mının **Standart**
* Hesap türü: **StorageV2 (genel amaçlı v2)**
* Yinelemesi **Yerel olarak yedekli depolama (LRS)**
* Erişim katmanı (varsayılan): **Sık erişimli**

Önbelleğiniz ile aynı konumda bir depolama hesabı kullanmak iyi bir uygulamadır.

Ayrıca, önbellek uygulamasına Azure depolama hesabınıza erişim izni vermeniz gerekir. Önbellekte erişim rolleri depolama hesabı katılımcısı ve Depolama Blobu veri katılımcısı için [depolama alanı ekleme](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account) bölümündeki açıklamayı izleyin. Depolama hesabı sahibi değilseniz, sahibi bu adımı izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Azure portal [Azure HPC önbellek örneği oluşturma](hpc-cache-create.md)

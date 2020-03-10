---
title: Azure HPC önbellek önkoşulları
description: Azure HPC önbelleğini kullanma önkoşulları
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 02/20/2020
ms.author: rohogue
ms.openlocfilehash: 40d282ad30a800a5e5a36a8d2211ec8da7ce63ec
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78388259"
---
# <a name="prerequisites-for-azure-hpc-cache"></a>Azure HPC önbelleği önkoşulları

Yeni bir Azure HPC önbelleği oluşturmak için Azure portal kullanmadan önce ortamınızın bu gereksinimleri karşıladığından emin olun.

## <a name="azure-subscription"></a>Azure aboneliği

Ücretli bir abonelik önerilir.

> [!NOTE]
> GA sürümünün ilk birkaç ayı sırasında, Azure HPC önbellek ekibinin, bir önbellek örneği oluşturmak için kullanılmadan önce aboneliğinizi erişim listesine eklemesi gerekir. Bu yordam, her müşterinin Önbelleklerinden yüksek kaliteli yanıt verdiğini aldığından emin olmanıza yardımcı olur. Erişim istemek için [Bu formu](https://aka.ms/onboard-hpc-cache) doldurun.

## <a name="network-infrastructure"></a>Ağ altyapısı

Önbelleğinizi kullanabilmeniz için ağ ile ilgili iki önkoşul ayarlanmalıdır:

* Azure HPC önbellek örneği için ayrılmış alt ağ
* Önbelleğin depolamaya ve diğer kaynaklara erişebilmesi için DNS desteği

### <a name="cache-subnet"></a>Önbellek alt ağı

Azure HPC önbelleği, bu kaliteleri olan ayrılmış bir alt ağa ihtiyaç duyuyor:

* Alt ağda en az 64 IP adresi kullanılabilir olmalıdır.
* Alt ağ, istemci makineler gibi ilgili hizmetler için bile diğer VM 'Leri barındıramaz.
* Birden çok Azure HPC önbellek örneği kullanıyorsanız, her birinin kendi alt ağına ihtiyacı vardır.

En iyi yöntem, her önbellek için yeni bir alt ağ oluşturmaktır. Önbellek oluşturmanın bir parçası olarak yeni bir sanal ağ ve alt ağ oluşturabilirsiniz.

### <a name="dns-access"></a>DNS erişimi

Önbelleğin, sanal ağının dışındaki kaynaklara erişmesi için DNS ihtiyacı vardır. Kullandığınız kaynaklara bağlı olarak, özelleştirilmiş bir DNS sunucusu ayarlamanız ve bu sunucu ile Azure DNS sunucuları arasında iletmeyi yapılandırmanız gerekebilir:

* Azure Blob depolama uç noktalarına ve diğer iç kaynaklara erişmek için, Azure tabanlı DNS sunucusuna ihtiyacınız vardır.
* Şirket içi depolamaya erişmek için, depolama ana bilgisayar adlarını çözümleyebilecek özel bir DNS sunucusu yapılandırmanız gerekir.

Yalnızca blob depolamaya erişmeniz gerekiyorsa önbelleğiniz için Azure tarafından sağlanmış varsayılan DNS sunucusunu kullanabilirsiniz. Ancak, diğer kaynaklara erişmeniz gerekiyorsa, özel bir DNS sunucusu oluşturmanız ve Azure 'a özgü tüm çözümleme isteklerini Azure DNS sunucusuna iletecek şekilde yapılandırmanız gerekir.

Basit bir DNS sunucusu, istemci bağlantılarının tüm kullanılabilir önbellek bağlama noktaları arasında yükünü dengelemek için de kullanılabilir.

Azure sanal ağları ve DNS sunucusu konfigürasyonları hakkında daha fazla bilgi [için bkz. Azure sanal ağlardaki kaynaklar Için ad çözümlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances).

## <a name="permissions"></a>İzinler

Önbelleğinizi oluşturmaya başlamadan önce Bu izinle ilgili önkoşulları denetleyin.

* Önbellek örneğinin sanal ağ arabirimlerini (NIC 'ler) oluşturabiliyor olması gerekir. Önbelleği oluşturan kullanıcının, NIC 'ler oluşturmak için abonelikte yeterli ayrıcalıklara sahip olması gerekir.

* BLOB depolama kullanılıyorsa, Azure HPC önbelleğinin depolama hesabınıza erişmesi için yetkilendirmeye ihtiyacı vardır. Blob depolamaya önbellek erişimi sağlamak için rol tabanlı erişim denetimi (RBAC) kullanın. İki rol gereklidir: depolama hesabı katılımcısı ve Depolama Blobu veri katılımcısı.

  Rolleri eklemek için [depolama hedefleri ekleme](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account) bölümündeki yönergeleri izleyin.

## <a name="storage-infrastructure"></a>Depolama altyapısı

Önbellek, Azure Blob kapsayıcılarını veya NFS donanım depolama dışarı aktarmaları destekler. Önbelleği oluşturduktan sonra depolama hedefleri ekleyin.

Her depolama türünün belirli önkoşulları vardır.

### <a name="blob-storage-requirements"></a>BLOB depolama gereksinimleri

Önbelleğiniz ile Azure Blob depolama 'yı kullanmak istiyorsanız, uyumlu bir depolama hesabı ve boş bir blob kapsayıcısı ya da [verileri Azure Blob depolamaya taşıma](hpc-cache-ingest.md)bölümünde açıklandığı gıbı Azure HPC önbellek biçimli verilerle doldurulmuş bir kapsayıcı gerekir.

Depolama hedefi eklemeyi denemeden önce hesabı oluşturun. Hedefi eklediğinizde yeni bir kapsayıcı oluşturabilirsiniz.

Uyumlu bir depolama hesabı oluşturmak için şu ayarları kullanın:

* Performans: **Standart**
* Hesap türü: **StorageV2 (genel amaçlı v2)**
* Çoğaltma: **yerel olarak yedekli depolama (LRS)**
* Erişim katmanı (varsayılan): **sık** erişimli

Önbelleğiniz ile aynı konumda bir depolama hesabı kullanmak iyi bir uygulamadır.
<!-- clarify location - same region or same resource group or same virtual network? -->

Ayrıca, yukarıdaki [izinlerle](#permissions)belirtilen şekilde, önbellek uygulamasına Azure depolama hesabınıza erişim izni vermeniz gerekir. Önbelleğe gerekli erişim rollerini sağlamak için [depolama hedefleri ekleme](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account) bölümündeki yordamı izleyin. Depolama hesabı sahibi değilseniz, sahibi bu adımı izleyin.

### <a name="nfs-storage-requirements"></a>NFS depolama gereksinimleri

Bir NFS depolama sistemi (örneğin, şirket içi donanım NAS sistemi) kullanıyorsanız, bu gereksinimleri karşıladığından emin olun. Bu ayarları doğrulamak için depolama sisteminizin (veya veri merkezinizdeki) ağ yöneticileri veya güvenlik duvarı yöneticileri ile çalışmanız gerekebilir.

> [!NOTE]
> Önbellekte NFS depolama sistemine yeterli erişim yoksa depolama hedefi oluşturma işlemi başarısız olur.

Daha fazla bilgi için [bkz. NAS yapılandırması ve NFS depolama hedefi sorunları sorunlarını giderme](troubleshoot-nas.md).

* **Ağ bağlantısı:** Azure HPC önbelleğinin önbellek alt ağı ile NFS sisteminin veri merkezi arasında yüksek bant genişliğine sahip ağ erişimi olması gerekir. [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) veya benzer erişim önerilir. VPN kullanıyorsanız, büyük paketlerin engellenmediğinden emin olmak için bunu 1350 adresindeki Clamp TCP yönetim paketleri için yapılandırmanız gerekebilir. VPN ayarları sorunlarını gidermeye yönelik ek yardım için [VPN paket boyutu kısıtlamalarını](troubleshoot-nas.md#adjust-vpn-packet-size-restrictions) okuyun.

* **Bağlantı noktası erişimi:** Önbelleğin depolama sisteminizdeki belirli TCP/UDP bağlantı noktalarına erişmesi gerekir. Farklı depolama türlerinin farklı bağlantı noktası gereksinimleri vardır.

  Depolama sisteminizin ayarlarını denetlemek için bu yordamı izleyin.

  * Gerekli bağlantı noktalarını denetlemek için depolama sisteminize bir `rpcinfo` komutu verin. Aşağıdaki komut, bağlantı noktalarını listeler ve ilgili sonuçları bir tabloyla biçimlendirir. ( *< Storage_IP >* terimi yerıne sisteminizin IP adresini kullanın.)

    Bu komutu, NFS altyapısının yüklü olduğu herhangi bir Linux istemcisinden verebilirsiniz. Küme alt ağı içinde bir istemci kullanırsanız, alt ağ ve depolama sistemi arasındaki bağlantıyı doğrulamaya da yardımcı olabilir.

    ```bash
    rpcinfo -p <storage_IP> |egrep "100000\s+4\s+tcp|100005\s+3\s+tcp|100003\s+3\s+tcp|100024\s+1\s+tcp|100021\s+4\s+tcp"| awk '{print $4 "/" $3 " " $5}'|column -t
    ```

  ``rpcinfo`` sorgusunun döndürdüğü tüm bağlantı noktalarının Azure HPC önbelleğinin alt ağından sınırsız trafiğe izin verdiği şekilde emin olun.

  * `rpcinfo` komutu tarafından döndürülen bağlantı noktalarına ek olarak, bu yaygın olarak kullanılan bağlantı noktalarının gelen ve giden trafiğe izin verdiği şekilde emin olun:

    | Protokol | Bağlantı noktası  | Hizmet  |
    |----------|-------|----------|
    | TCP/UDP  | 111   | rpcbind  |
    | TCP/UDP  | 2049  | NFS      |
    | TCP/UDP  | 4045  | nlockmgr |
    | TCP/UDP  | 4046  | dağtd   |
    | TCP/UDP  | 4047  | status   |

  * Bu gerekli bağlantı noktalarının tümünde trafiğe izin verdiklerinden emin olmak için güvenlik duvarı ayarlarını kontrol edin. Azure 'da kullanılan güvenlik duvarlarını, veri merkezinizdeki şirket içi güvenlik duvarlarını da denetlediğinizden emin olun.

* **Dizin erişimi:** Depolama sisteminde `showmount` komutunu etkinleştirin. Azure HPC Cache, depolama hedefi yapılandırmanızın geçerli bir dışarı aktarmaya işaret ettiğini ve ayrıca birden çok takın aynı alt dizinlere (dosya çarpışması için risk) eriş, emin olup olmadığını denetlemek için bu komutu kullanır.

  > [!NOTE]
  > NFS depolama sisteminizde NetApp 'ın ONTAP 9,2 işletim sistemi kullanılıyorsa, **`showmount`etkinleştirmeyin** . Yardım için [Microsoft hizmetine ve desteğe başvurun](hpc-cache-support-ticket.md) .

  NFS depolama hedef [sorun giderme makalesinde](troubleshoot-nas.md#enable-export-listing)Dizin listeleme erişimi hakkında daha fazla bilgi edinin.

* **Kök erişimi:** Önbellek, Kullanıcı KIMLIĞI 0 olarak arka uç sistemine bağlanır. Depolama sisteminizde bu ayarları kontrol edin:
  
  * `no_root_squash`etkinleştirin. Bu seçenek, uzak kök kullanıcının köke ait dosyalara erişebilmesini sağlar.

  * Önbelleğin alt ağından kök erişim kısıtlamalarını dahil olmadıklarından emin olmak için dışarı aktarma ilkelerini denetleyin.

  * Depolama ortamınızda başka bir dışarı aktarmanın alt dizinleri olan dışarı aktarımlar varsa, önbelleğin yolun en düşük kesimine kök erişimi olduğundan emin olun. Ayrıntılar için NFS depolama hedefi sorun giderme makalesindeki [Dizin yollarında kök erişimi](troubleshoot-nas.md#allow-root-access-on-directory-paths) okuyun.

* NFS arka uç depolaması, uyumlu bir donanım/yazılım platformu olmalıdır. Ayrıntılar için Azure HPC önbellek ekibine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

* Azure portal [Azure HPC önbellek örneği oluşturma](hpc-cache-create.md)

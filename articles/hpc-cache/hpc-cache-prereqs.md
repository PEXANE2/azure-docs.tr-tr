---
title: Azure HPC Önbelleği ön koşulları
description: Azure HPC Önbelleğini kullanmak için ön koşullar
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: rohogue
ms.openlocfilehash: ab342dba5c8be2ff3793c0eb36926969b3e364e5
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537296"
---
# <a name="prerequisites-for-azure-hpc-cache"></a>Azure HPC Önbelleği için Ön Koşullar

Yeni bir Azure HPC Önbelleği oluşturmak için Azure portalını kullanmadan önce, ortamınızın bu gereksinimleri karşıladığından emin olun.

## <a name="azure-subscription"></a>Azure aboneliği

Ücretli abonelik önerilir.

## <a name="network-infrastructure"></a>Ağ altyapısı

Önbelleğinizi kullanamadan önce ağla ilgili iki ön koşul ayarlanmalıdır:

* Azure HPC Önbellek örneği için özel bir alt ağ
* Önbelleğin depolamaya ve diğer kaynaklara erişebilmeleri için DNS desteği

### <a name="cache-subnet"></a>Önbellek alt ağı

Azure HPC Önbelleği'nin bu niteliklere sahip özel bir alt ağa ihtiyacı vardır:

* Alt ağda en az 64 IP adresi olmalıdır.
* Alt ağ, istemci makineleri gibi ilgili hizmetler için bile başka hiçbir VM barındıramaz.
* Birden çok Azure HPC Önbellek örneği kullanıyorsanız, her birinin kendi alt ağına ihtiyacı vardır.

En iyi yöntem, her önbellek için yeni bir alt ağ oluşturmaktır. Önbelleği oluşturmanın bir parçası olarak yeni bir sanal ağ ve alt ağ oluşturabilirsiniz.

### <a name="dns-access"></a>DNS erişimi

Önbelleğin sanal ağının dışındaki kaynaklara erişebilmesi için DNS gerekir. Kullandığınız kaynaklara bağlı olarak, özelleştirilmiş bir DNS sunucusu ayarlamanız ve bu sunucu ile Azure DNS sunucuları arasında iletme yapılandırmanız gerekebilir:

* Azure Blob depolama bitiş noktalarına ve diğer dahili kaynaklara erişmek için Azure tabanlı DNS sunucusuna ihtiyacınız vardır.
* Şirket içi depolama alanına erişmek için, depolama ana bilgisayar adlarınızı çözebilecek özel bir DNS sunucusu yapılandırmanız gerekir.

Yalnızca Blob depolama alanına erişmeniz gerekiyorsa, önbelleğiniz için varsayılan Azure tarafından sağlanan DNS sunucusunu kullanabilirsiniz. Ancak, diğer kaynaklara erişmeniz gerekiyorsa, özel bir DNS sunucusu oluşturmalı ve Azure'a özgü çözüm isteklerini Azure DNS sunucusuna iletecek şekilde yapılandırmanız gerekir.

Basit bir DNS sunucusu, kullanılabilir önbellek montaj noktaları arasında bakiye istemci bağlantılarını yüklemek için de kullanılabilir.

Azure sanal [ağlarındaki kaynaklar için Ad çözümünde](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances)Azure sanal ağları ve DNS sunucu yapılandırmaları hakkında daha fazla bilgi edinin.

## <a name="permissions"></a>İzinler

Önbelleğinizi oluşturmaya başlamadan önce bu izinle ilgili ön koşulları denetleyin.

* Önbellek örneğinin sanal ağ arabirimleri (NIC' ler) oluşturabilmesi gerekir. Önbelleği oluşturan kullanıcının NIC'ler oluşturmak için abonelikte yeterli ayrıcalıklara sahip olması gerekir.

* Azure HPC Önbelleği, Blob depolama alanını kullanıyorsa, depolama hesabınıza erişmek için yetkilendirmeye ihtiyaç duyar. Önbelleğe Blob depolama alanınıza erişim sağlamak için rol tabanlı erişim denetimini (RBAC) kullanın. İki rol gereklidir: Depolama Hesabı Katılımcısı ve Depolama Blob Veri Katılımcısı.

  Rolleri eklemek için [depolama hedefleri ekle](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account) yönergelerini izleyin.

## <a name="storage-infrastructure"></a>Depolama altyapısı

Önbellek, Azure Blob kapsayıcılarını veya NFS donanım depolama dışaaklarını destekler. Önbelleği oluşturduktan sonra depolama hedefleri ekleyin.

Her depolama türü belirli ön koşullara sahiptir.

### <a name="blob-storage-requirements"></a>Blob depolama gereksinimleri

Önbelleğinizle Azure Blob depolama sını kullanmak istiyorsanız, uyumlu bir depolama hesabı ve boş bir Blob kapsayıcısı veya [Verileri Azure Blob depolamasına taşı'nda](hpc-cache-ingest.md)açıklandığı gibi Azure HPC Önbellek biçimlendirilmiş verilerle doldurulan bir kapsayıcıgerekir.

Depolama hedefi eklemeye çalışmadan önce hesabı oluşturun. Hedefi eklediğinizde yeni bir kapsayıcı oluşturabilirsiniz.

Uyumlu bir depolama hesabı oluşturmak için şu ayarları kullanın:

* Performans: **Standart**
* Hesap türü: **StorageV2 (genel amaçlı v2)**
* Çoğaltma: **Yerel olarak yedekli depolama (LRS)**
* Erişim katmanı (varsayılan): **Sıcak**

Önbelleğinizle aynı konumda bir depolama hesabı kullanmak iyi bir uygulamadır.

Ayrıca, önbellek uygulamasına Yukarıda [İzinler'de](#permissions)belirtildiği gibi Azure depolama hesabınıza erişim izni vermeniz gerekir. Önbelleğe gerekli erişim rollerini vermek için [depolama hedefleri ekle](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account) yordamını izleyin. Depolama hesabı sahibi değilseniz, sahibinin bu adımı yapmasını sorun.

### <a name="nfs-storage-requirements"></a>NFS depolama gereksinimleri
<!-- linked from configuration.md -->

Bir NFS depolama sistemi (örneğin, şirket içi donanım NAS sistemi) kullanıyorsanız, bu gereksinimleri karşıladığından emin olun. Bu ayarları doğrulamak için depolama sisteminiz (veya veri merkeziniz) için ağ yöneticileri veya güvenlik duvarı yöneticileriyle birlikte çalışmanız gerekebilir.

> [!NOTE]
> Önbellek NFS depolama sistemine yetersiz erişime sahipse depolama hedef oluşturma başarısız olur.

[Sorun giderme NAS yapılandırmasına ve NFS depolama hedef sorunlarına](troubleshoot-nas.md)daha fazla bilgi dahildir.

* **Ağ bağlantısı:** Azure HPC Önbelleği, önbellek alt ağı ile NFS sisteminin veri merkezi arasında yüksek bant genişliğine sahiptir. [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) veya benzeri erişim önerilir. VPN kullanıyorsanız, büyük paketlerin engellenmediğinden emin olmak için 1350'de TCP MSS'yi kıskaca edecek şekilde yapılandırmanız gerekebilir. EK yardım sorun giderme VPN ayarları için [VPN paket boyutu kısıtlamalarını](troubleshoot-nas.md#adjust-vpn-packet-size-restrictions) okuyun.

* **Bağlantı noktası erişimi:** Önbelleğin depolama sisteminizdeki belirli TCP/UDP bağlantı noktalarına erişmesi gerekir. Farklı depolama türleri farklı bağlantı noktası gereksinimlerine sahiptir.

  Depolama sisteminizin ayarlarını kontrol etmek için bu yordamı izleyin.

  * Gerekli `rpcinfo` bağlantı noktalarını denetlemek için depolama sisteminize bir komut düzenleyin. Aşağıdaki komut bağlantı noktalarını listeler ve ilgili sonuçları bir tabloda biçimlendirir. (storage_IP>terimi yerine sisteminiz *<in* IP adresini kullanın.)

    Bu komutu, NFS altyapısı yüklü olan herhangi bir Linux istemcisinden çıkarabilirsiniz. Küme alt ağı içinde bir istemci kullanıyorsanız, alt ağ ve depolama sistemi arasındaki bağlantıyı doğrulamaya da yardımcı olabilir.

    ```bash
    rpcinfo -p <storage_IP> |egrep "100000\s+4\s+tcp|100005\s+3\s+tcp|100003\s+3\s+tcp|100024\s+1\s+tcp|100021\s+4\s+tcp"| awk '{print $4 "/" $3 " " $5}'|column -t
    ```

  ``rpcinfo`` Sorgu tarafından döndürülen tüm bağlantı noktalarının Azure HPC Önbelleği'nin alt netinden sınırsız trafiğe izin verdiğinden emin olun.

  * Komutu `rpcinfo` kullanamıyorsanız, sık kullanılan bu bağlantı noktalarının gelen ve giden trafiğe izin verdiğinden emin olun:

    | Protokol | Bağlantı noktası  | Hizmet  |
    |----------|-------|----------|
    | TCP/UDP  | 111   | rpcbind  |
    | TCP/UDP  | 2049  | NFS      |
    | TCP/UDP  | 4045  | nlockmgr |
    | TCP/UDP  | 4046  | monte   |
    | TCP/UDP  | 4047  | durum   |

    Bazı sistemler bu hizmetler için farklı bağlantı noktası numaraları kullanır - emin olmak için depolama sisteminizin belgelerine başvurun.

  * Gerekli tüm bağlantı noktalarında trafiğe izin verdiklerinden emin olmak için güvenlik duvarı ayarlarını denetleyin. Azure'da kullanılan güvenlik duvarlarını ve veri merkezinizdeki şirket içi güvenlik duvarlarını kontrol ettiğinizden emin olun.

* **Dizin erişimi:** Depolama `showmount` sistemindeki komutu etkinleştirin. Azure HPC Önbelleği, depolama hedef yapılandırmanızın geçerli bir dış asayı işaret edip etmediğini denetlemek ve ayrıca birden çok bineğin aynı alt dizine (dosya çakışması riski) erişmediğinden emin olmak için bu komutu kullanır.

  > [!NOTE]
  > NFS depolama sisteminiz NetApp'in ONTAP 9.2 işletim sistemini **kullanıyorsa, etkinleştirme. `showmount` ** Yardım için [Microsoft Service ve Support'a başvurun.](hpc-cache-support-ticket.md)

  NFS depolama hedef [sorun giderme makalesinde](troubleshoot-nas.md#enable-export-listing)dizin listesi erişimi hakkında daha fazla bilgi edinin.

* **Kök erişimi** (okuma/yazma): Önbellek arka uç sistemine kullanıcı kimliği 0 olarak bağlanır. Depolama sisteminizdeki bu ayarları kontrol edin:
  
  * Etkinleştirin. `no_root_squash` Bu seçenek, uzak kök kullanıcıkök ait dosyalara erişebilmesini sağlar.

  * Önbelleğin alt ağından kök erişime ilişkin kısıtlamalar içermediğinden emin olmak için dışa aktarma ilkelerini denetleyin.

  * Depolamanızda başka bir dışa aktarmanın alt dizinleri olan dışa aktarımlar varsa, önbelleğin yolun en düşük kesimine kök erişimi olduğundan emin olun. Ayrıntılar için NFS depolama hedef sorun giderme makalesinde [dizin yollarında Root erişimini](troubleshoot-nas.md#allow-root-access-on-directory-paths) okuyun.

* NFS arka uç depolama uyumlu bir donanım/yazılım platformu olmalıdır. Ayrıntılar için Azure HPC Önbellek ekibine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

* Azure portalından [Bir Azure HPC Önbelleği örneği oluşturma](hpc-cache-create.md)

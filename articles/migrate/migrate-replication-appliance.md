---
title: Azure geçişi çoğaltma gereç mimarisi
description: Aracı tabanlı geçiş için Azure geçişi çoğaltma gereci 'na genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: raynew
ms.openlocfilehash: ba14767bde5d6cdca3a82dbe4e8a115ec25cc911
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74186548"
---
# <a name="replication-appliance"></a>Çoğaltma gereci

Bu makalede, Azure geçişi tarafından kullanılan çoğaltma gereci açıklanmaktadır: VMware VM 'Leri, fiziksel makineler ve özel/genel bulut VM 'lerini, aracı tabanlı bir geçiş kullanarak Azure 'a geçirirken sunucu değerlendirmesi. 

Araç [Azure geçiş](migrate-overview.md) Merkezi ' nde kullanılabilir. Hub, değerlendirme ve geçiş için yerel araçlar, diğer Azure hizmetlerinden araçlar ve üçüncü taraf bağımsız yazılım satıcıları (ISV 'Ler) için de sağlar.


## <a name="appliance-overview"></a>Gereç genel bakışı

Çoğaltma gereci, VMware VM ya da fiziksel sunucu olarak tek bir şirket içi makine olarak dağıtılır. Çalışır:
- **Çoğaltma**gereci: çoğaltma gereci iletişimleri koordine eder ve Azure 'a çoğaltılan şirket Içi VMware VM 'leri ve fiziksel sunucular için veri çoğaltmasını yönetir.
- **İşlem sunucusu**: çoğaltma gereci üzerinde varsayılan olarak yüklenen işlem sunucusu ve şunları yapar:
    - **Çoğaltma ağ geçidi**: çoğaltma ağ geçidi olarak davranır. Çoğaltma için etkinleştirilen makinelerden çoğaltma verilerini alır. Önbelleğe alma, sıkıştırma ve şifreleme ile çoğaltma verilerini iyileştirir ve Azure 'a gönderir.
    - **Aracı yükleyicisi**: Mobility hizmetinin anında yüklenmesini gerçekleştirir. Bu hizmet, geçiş için çoğaltmak istediğiniz her şirket içi makinede yüklü ve çalışır olmalıdır.

## <a name="appliance-deployment"></a>Gereç dağıtımı

**Farklı dağıt** | **İçin kullanılan** | **Ayrıntılar**
--- | --- |  ---
VMware VM | Genellikle, aracı tabanlı geçişle Azure geçişi geçiş aracı kullanılarak VMware VM 'Leri geçirilirken kullanılır. | OVA şablonunu Azure geçiş hub 'ından indirir ve gereç sanal makinesini oluşturmak için vCenter Server içeri aktarabilirsiniz.
Fiziksel makine | Bir VMware altyapınız yoksa veya bir OVA şablonu kullanarak bir VMware VM 'si oluşturmıyorsanız şirket içi fiziksel sunucuları geçirirken kullanılır. | Bir yazılım yükleyicisini Azure geçiş hub 'ından indirir ve gereci makinesini kurmak için çalıştırın.

## <a name="appliance-deployment-requirements"></a>Gereç dağıtım gereksinimleri

Dağıtım gereksinimlerini [gözden geçirin](migrate-support-matrix-vmware.md#agent-based-migration-replication-appliance-requirements) .



## <a name="appliance-license"></a>Gereç lisansı
Gereç, 180 gün için geçerli olan bir Windows Server 2016 değerlendirme lisansıyla gelir. Değerlendirme süresi sona ermeden yakın ise, yeni bir gereç indirmeniz ve dağıtmanız ya da gereç sanal makinesinin işletim sistemi lisansını etkinleştirmenizi öneririz.

## <a name="replication-process"></a>Çoğaltma işlemi

1. Bir sanal makine için çoğaltmayı etkinleştirdiğinizde, Azure Storage 'a ilk çoğaltma, belirtilen çoğaltma ilkesi kullanılarak başlar. 
2. Trafik, internet üzerinden Azure depolama genel uç noktalarına çoğaltılır. Şirket içi siteden Azure 'a siteden siteye sanal özel ağ (VPN) üzerinden trafik çoğaltma desteklenmez.
3. İlk çoğaltma tamamlandıktan sonra Delta çoğaltma başlar. Bir makine için izlenen değişiklikler günlüğe kaydedilir.
4. İletişim şu şekilde gerçekleşir:
    - VM 'Ler, çoğaltma yönetimi için HTTPS 443 gelen bağlantı noktasında çoğaltma gereci ile iletişim kurar.
    - Çoğaltma gereci, HTTPS 443 giden bağlantı noktası üzerinden Azure ile çoğaltmayı düzenler.
    - VM 'Ler, çoğaltma verilerini işlem sunucusuna (çoğaltma gereci üzerinde çalışır), HTTPS 9443 gelen bağlantı noktası üzerinden gönderir. Bu bağlantı noktası değiştirilebilir.
    - İşlem sunucusu çoğaltma verilerini alır, bunları iyileştirir ve şifreler ve 443 giden bağlantı noktası üzerinden Azure depolama 'ya gönderir.
5. Çoğaltma verileri günlüğü, ilk olarak Azure 'da bir önbellek depolama hesabında yer açar. Bu Günlükler işlenir ve veriler bir Azure yönetilen diskinde depolanır.

![Mimari](./media/migrate-replication-appliance/architecture.png)

## <a name="appliance-upgrades"></a>Gereç yükseltmeleri

Gereç, Azure geçiş hub 'ından el ile yükseltilir. En son sürümü her zaman çalıştırmanızı öneririz.

1. Azure geçişi > sunucuları > Azure geçişi: Sunucu değerlendirmesi, altyapı sunucuları, **yapılandırma sunucuları**' na tıklayın.
2. **Yapılandırma sunucularında**, çoğaltma gerecinin yeni bir sürümü kullanılabilir olduğunda **Aracı sürümünde** bir bağlantı görüntülenir. 
3. Yükleyiciyi, çoğaltma gereci makinesine indirin ve yükseltmeyi yükleyin. Yükleyici, Gereç üzerinde çalışan geçerli sürümü algılar.
 
## <a name="next-steps"></a>Sonraki adımlar

VMware için gereci ayarlamayı [öğrenin](tutorial-assess-vmware.md#set-up-the-appliance-vm) .
Hyper-V için gereci ayarlamayı [öğrenin](tutorial-assess-hyper-v.md#set-up-the-appliance-vm) .


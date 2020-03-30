---
title: Özel bulut VMware bileşenleri
titleSuffix: Azure VMware Solution by CloudSimple
description: VMware bileşenlerinin özel buluta nasıl yüklenirlerini açıklar
author: sharaths-cs
ms.author: dikamath
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 9c9b80cd4d8a7a7ac5597d10bbb87095564bd461
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279513"
---
# <a name="private-cloud-vmware-components"></a>Özel Bulut VMware bileşenleri

Özel Bulut, yönetim etki alanında bir vCenter sunucusu tarafından yönetilen yalıtılmış bir VMware yığınıdır (ESXi ana bilgisayarları, vCenter, vSAN ve NSX).  CloudSimple hizmeti, Azure konumlarında VMware'i azure çıplak metal altyapısında yerel olarak dağıtmanıza olanak tanır.  Özel Bulutlar, Azure Bulutu'nun geri kalanıyla tümleştirilir.  Aşağıdaki VMware yığın bileşenleriyle özel bir bulut dağıtılır:

* **VMware ESXi -** Azure'a özel düğümlerde hipervizör
* **VMware vCenter -** Özel bulut vSphere ortamının merkezi yönetimi için cihaz
* **VMware vSAN -** Hiper-yakınsama lı altyapı çözümü
* **VMware NSX Veri Merkezi -** Ağ Sanallaştırma ve Güvenlik Yazılımı  

## <a name="vmware-component-versions"></a>VMware bileşen sürümleri

Aşağıdaki yazılım sürümüyle birlikte Özel Bulut VMware yığını dağıtılır.

| Bileşen | Sürüm | Lisanslı sürüm |
|-----------|---------|------------------|
| Esxi | 6.7U2 ile | Kurumsal Plus |
| vCenter | 6.7U2 ile | vCenter Standardı |
| vSAN | 6.7 | Enterprise |
| NSX Veri Merkezi | 2.4.1 | Gelişmiş |

## <a name="esxi"></a>Esxi

Özel bir bulut oluşturduğunuzda VMware ESXi, sağlanan CloudSimple düğümlerine yüklenir.  ESXi, iş yükü sanal makinelerin (VM) dağıtımı için hipervizörü sağlar.  Düğümler, özel bulutunuzda hiper-yakınsama altyapısı (bilgi işlem ve depolama) sağlar.  Düğümler, özel buluttaki vSphere kümesinin bir parçasıdır.  Her düğümün alt ağ bağlı dört fiziksel ağ arabirimi vardır.  VCenter'da **bir vSphere Distributed Switch (VDS)** oluşturmak için iki fiziksel ağ arabirimi kullanılır ve iki tane **nsx tarafından yönetilen sanal dağıtılmış anahtar (N-VDS)** oluşturmak için kullanılır.  Ağ arabirimleri yüksek kullanılabilirlik için etkin-etkin modda yapılandırılır.

VMware ESXi hakkında daha fazla bilgi edinin

## <a name="vcenter-server-appliance"></a>vCenter sunucu cihazı

vCenter sunucu cihazı (VCSA), CloudSimple tarafından VMware Solution için kimlik doğrulama, yönetim ve orkestrasyon işlevleri sağlar. Özel bulutunuzu oluşturduğunuzda gömülü Platform Hizmetleri Denetleyicisine (PSC) sahip VCSA dağıtılır.  VCSA, özel bulutunuzu dağıttığınızda oluşturulan vSphere kümesinde dağıtılır.  Her özel bulutun kendi VCSA'sı vardır.  Özel bulutun genişletilmesi düğümleri özel buluttaki VCSA'ya ekler.

### <a name="vcenter-single-sign-on"></a>vCenter tek oturum açma

VCSA'daki Gömülü Platform Hizmetleri Denetleyicisi, **vCenter Tek Oturum Açma etki alanıyla**ilişkilidir.  Etki alanı adı **cloudsimple.local**.  VCenter'a erişmeniz için varsayılan bir kullanıcı **CloudOwner@cloudsimple.com** oluşturulur.  vCenter için şirket içi/Azure etkin dizin [kimlik kaynaklarınızı](set-vcenter-identity.md)ekleyebilirsiniz.

## <a name="vsan-storage"></a>vSAN depolama

Özel bulutlar, kümeye yerel olarak tamamen yapılandırılmış all-flash vSAN depolama alanıyla oluşturulur.  VSAN datastore ile bir vSphere kümesi oluşturmak için aynı SKU'dan en az üç düğüm gerekir.  VSAN veri deposunda varsayılan olarak çoğaltma ve sıkıştırma etkinleştirilir.  vSphere kümesinin her düğümünde iki disk grubu oluşturulur. Her disk grubunda bir önbellek diski ve üç kapasite diski bulunur.

Varsayılan vSAN depolama ilkesi vSphere kümesinde oluşturulur ve vSAN veri deposuna uygulanır.  Bu ilke, VM depolama nesnelerinin gerekli hizmet düzeyini garanti etmek için veri deposunda nasıl sağlanıp tahsis edildiğini belirler.  Depolama ilkesi **tolere Etme Hataları (FTT)** ve **Hata toleransı yöntemini**tanımlar.  Yeni depolama ilkeleri oluşturabilir ve bunları VM'lere uygulayabilirsiniz. SLA'yı korumak için vSAN veri deposunda %25 yedek kapasite sağlanmalıdır.  

### <a name="default-vsan-storage-policy"></a>Varsayılan vSAN depolama ilkesi

Aşağıdaki tabloda varsayılan vSAN depolama ilkesi parametreleri gösterilmektedir.

| vSphere Kümesi'ndeki düğüm sayısı | FTT | Hata toleransı yöntemi |
|------------------------------------|-----|--------------------------|
| 3 ve 4 düğüm | 1 | RAID 1 (yansıtma) - 2 kopya oluşturur |
| 5 ila 16 düğüm | 2 | RAID 1 (yansıtma) - 3 kopya oluşturur |

## <a name="nsx-data-center"></a>NSX Veri Merkezi

NSX Veri Merkezi, özel bulutunuzda ağ sanallaştırması, mikro segmentasyon ve ağ güvenliği özellikleri sağlar.  NSX Veri Merkezi tarafından desteklenen tüm hizmetleri özel bulutunuzda NSX aracılığıyla yapılandırabilirsiniz.  Özel bir bulut oluşturduğunuzda, aşağıdaki NSX bileşenleri yüklenir ve yapılandırılır.

* NSXT Yöneticisi
* Ulaşım Bölgeleri
* Ana Bilgisayar ve Kenar Uplink Profili
* Kenar Taşıma, Ext1 ve Ext2 için Mantıksal Anahtar
* ESXi Transport Düğümü için IP Havuzu
* Kenar Taşıma Düğümü için IP Havuzu
* Kenar Düğümleri
* Denetleyici ve Edge VM'ler için DRS Afinite önleyici kural
* Tier 0 Yönlendirici
* Tier0 Yönlendirici'de BGP'yi etkinleştirme

## <a name="vsphere-cluster"></a>vSphere kümesi

ESXi ana bilgisayarları, özel bulutun yüksek kullanılabilirliğini sağlamak için bir küme olarak yapılandırılır.  Özel bir bulut oluşturduğunuzda, vSphere'in yönetim bileşenleri ilk kümede dağıtılır.  Yönetim bileşenleri için bir kaynak havuzu oluşturulur ve tüm yönetim Sanal M'leri bu kaynak havuzunda dağıtılır. Özel bulutu küçültmek için ilk küme silinemez.  vSphere küme **vSphere HA**kullanarak VMs için yüksek kullanılabilirlik sağlar.  Tolere etme hataları kümedeki kullanılabilir düğüm sayısını temel alınr.  Formülü, ```Number of nodes = 2N+1``` tolere ```N``` edilmesi gereken hata sayısının olduğu yerde kullanabilirsiniz.

### <a name="vsphere-cluster-limits"></a>vSphere küme sınırları

| Kaynak | Sınır |
|----------|-------|
| Özel bir bulut oluşturmak için minimum düğüm sayısı (ilk vSphere kümesi) | 3 |
| Özel bir bulutta vSphere Cluster'daki maksimum düğüm sayısı | 16 |
| Özel buluttaki maksimum düğüm sayısı | 64 |
| Özel bir buluttaki maksimum vSphere Kümesayısı | 21 |
| Yeni bir vSphere Kümesi'nde minimum düğüm sayısı | 3 |

## <a name="vmware-infrastructure-maintenance"></a>VMware altyapı bakımı

Bazen VMware altyapısının yapılandırmasında değişiklik yapmak gerekir. Şu anda, bu aralıklar her 1-2 ayda bir oluşabilir, ancak sıklığı zaman içinde düşmesi bekleniyor. Bu tür bakımlar genellikle CloudSimple hizmetlerinin normal tüketimini kesintiye uğratmadan yapılabilir. VMware bakım aralığı sırasında, aşağıdaki hizmetler herhangi bir etki yaratmadan çalışmaya devam eder:

* VMware yönetim düzlemi ve uygulamaları
* vCenter erişimi
* Tüm ağ ve depolama
* Tüm Azure trafiği

## <a name="updates-and-upgrades"></a>Güncellemeler ve yükseltmeler

CloudSimple, özel buluttaki VMware yazılımının (ESXi, vCenter, PSC ve NSX) yaşam döngüsü yönetiminden sorumludur.

Yazılım güncellemeleri şunlardır:

* **Yamalar.** VMware tarafından yayımlanan güvenlik düzeltmeleri veya hata düzeltmeleri.
* **Güncelleştirmeler**. VMware yığın bileşeninin küçük sürüm değişikliği.
* **Yükseltmeleri**. VMware yığın bileşeninin ana sürüm değişikliği.

CloudSimple, VMware'den kullanıma sunulduğu anda kritik bir güvenlik yamanını sınar. CloudSimple, SLA'ya göre güvenlik yamasını bir hafta içinde özel bulut ortamlarına sunar.

CloudSimple, VMware yazılım bileşenleriiçin üç ayda bir bakım güncelleştirmesi sağlar. VMware yazılımının yeni bir ana sürümü kullanılabilir olduğunda, CloudSimple yükseltme için uygun bir bakım penceresini koordine etmek için müşterilerle birlikte çalışır.  

## <a name="next-steps"></a>Sonraki adımlar

* [CloudSimple bakım ve güncellemeleri](cloudsimple-maintenance-updates.md)

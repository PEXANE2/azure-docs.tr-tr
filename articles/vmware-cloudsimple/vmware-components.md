---
title: Azure VMware çözümleri (AVS)-AVS özel bulutu VMware bileşenleri
description: VMware bileşenlerinin AVS özel bulutuna nasıl yüklendiğini açıklar
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: dikamath
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3ea6c22c3957f72a0a416ce7ae42c62ff5a0791a
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77016791"
---
# <a name="avs-private-cloud-vmware-components"></a>AVS özel bulutu VMware bileşenleri

Bir AVS özel bulutu, bir yönetim etki alanındaki vCenter Server tarafından yönetilen yalıtılmış bir VMware yığını (ESXi Konakları, vCenter, vSAN ve NSX) ortamıdır. AVS hizmeti, VMware 'yi Azure konumlarındaki Azure çıplak altyapısına yerel olarak dağıtmanıza olanak tanır. AVS özel bulutlar, Azure bulutunun geri kalanı ile tümleşiktir. Aşağıdaki VMware Stack bileşenleriyle bir AVS özel bulutu dağıtılır:

* **VMware ESXi-** Azure adanmış düğümlerinde hiper yönetici
* **VMware vCenter-** AVS özel bulut vSphere ortamının merkezi yönetimi için gereç
* **VMware vSAN-** Hiper yakınsanmış altyapı çözümü
* **VMware NSX veri merkezi-** Ağ sanallaştırma ve güvenlik yazılımı  

## <a name="vmware-component-versions"></a>VMware bileşen sürümleri

Aşağıdaki yazılım sürümüyle bir AVS özel bulutu VMware yığını dağıtılır.

| Bileşen | Sürüm | Lisanslı sürüm |
|-----------|---------|------------------|
| ESXi | 6.7 U2 | Kurumsal Plus |
| vCenter | 6.7 U2 | vCenter Standard |
| vSAN | 6.7 | Kurumsal |
| NSX veri merkezi | 2.4.1 | Gelişmiş |

## <a name="esxi"></a>ESXi

VMware ESXi, bir AVS özel bulutu oluştururken sağlanan AVS düğümlerine yüklenir. ESXi, iş yükü sanal makinelerini (VM) dağıtmak için hiper yöneticiyi sağlar. Düğümler, AVS özel bulutunuzda hiper yakınsama altyapısı (işlem ve depolama) sağlar. Düğümler, AVS özel bulutundaki vSphere kümesinin bir parçasıdır. Her düğüm, ağı düşük düzenlemek için bağlı dört fiziksel ağ arabirimine sahiptir. İki fiziksel ağ arabirimi, vCenter üzerinde bir **vSphere dağıtılmış anahtarı (VDS)** oluşturmak için kullanılır ve Iki adet **NSX tarafından yönetilen bir sanal dağıtılmış anahtar (N-VDS)** oluşturmak için kullanılır. Ağ arabirimleri, yüksek kullanılabilirlik için etkin-etkin modda yapılandırılır.

VMware ESXi hakkında daha fazla bilgi edinin

## <a name="vcenter-server-appliance"></a>vCenter Server gereci

vCenter Server gereci (VCSA), VMware çözümleri (AVS) için kimlik doğrulama, yönetim ve düzenleme işlevlerini sağlar. , AVS özel bulutunuzu oluşturduğunuzda, katıştırılmış platform hizmetleri denetleyicisi (PSC) ile VCSA dağıtılır. VCSA, AVS özel bulutunuzu dağıtırken oluşturulan vSphere kümesinde dağıtılır. Her bir AVS özel bulutunun kendi VCSA 'sı vardır. Bir AVS özel bulutu genişletmesi, düğümleri AVS özel bulutundaki VCSA 'ya ekler.

### <a name="vcenter-single-sign-on"></a>vCenter çoklu oturum açma

VCSA 'daki katıştırılmış platform hizmetleri denetleyicisi bir **vCenter çoklu oturum açma etki alanıyla**ilişkilendirilir. Etki alanı adı **AVS. Local**' dır. VCenter 'a erişmeniz için varsayılan kullanıcı **CloudOwner@AVS.com** oluşturulur. VCenter için şirket içi/Azure Active Directory [kimlik kaynaklarınızı](set-vcenter-identity.md)ekleyebilirsiniz.

## <a name="vsan-storage"></a>vSAN depolaması

AVS özel bulutları, tümüyle yapılandırılmış All-Flash vSAN depolama, kümeyle yerel olarak oluşturulur. VSAN veri deposu ile vSphere kümesi oluşturmak için aynı SKU 'nun en az üç düğümü gereklidir. Devre dışı bırakma ve sıkıştırma, varsayılan olarak vSAN veri deposunda etkindir. VSphere kümesinin her bir düğümünde iki disk grubu oluşturulur. Her disk grubu, bir önbellek diski ve üç kapasite diski içerir.

VSphere kümesinde varsayılan bir vSAN depolama ilkesi oluşturulur ve vSAN veri deposuna uygulanır. Bu ilke, gerekli hizmet düzeyini güvence altına almak için VM depolama nesnelerinin veri deposu içinde nasıl sağlandığını ve ayrılacağını belirler. Depolama ilkesi, **tolerans (FTT)** ve **hata toleransı yöntemi**hatalarını tanımlar. Yeni depolama ilkeleri oluşturabilir ve bunları sanal makinelere uygulayabilirsiniz. SLA 'yı sürdürmek için, vSAN veri deposunda %25 yedek kapasitesinin korunması gerekir. 

### <a name="default-vsan-storage-policy"></a>Varsayılan vSAN depolama ilkesi

Aşağıdaki tabloda varsayılan vSAN depolama ilkesi parametreleri gösterilmektedir.

| VSphere kümesindeki düğüm sayısı | FTT | Hata toleransı yöntemi |
|------------------------------------|-----|--------------------------|
| 3 ve 4 düğümleri | 1 | RAID 1 (yansıtma)-2 kopya oluşturur |
| 5-16 düğümleri | 2 | RAID 1 (yansıtma)-3 kopya oluşturur |

## <a name="nsx-data-center"></a>NSX veri merkezi

NSX veri merkezi, AVS özel bulutunuzda ağ sanallaştırma, mikro segmentleme ve ağ güvenliği özellikleri sağlar. NSX veri merkezi tarafından desteklenen tüm hizmetleri, NSX aracılığıyla AVS özel bulutunuzda yapılandırabilirsiniz. Bir AVS özel bulutu oluşturduğunuzda, aşağıdaki NSX bileşenleri yüklenir ve yapılandırılır.

* NSXT Yöneticisi
* Taşıma bölgeleri
* Konak ve kenar yukarı bağlantı profili
* Edge Transport, Ext1 ve Ext2 için mantıksal anahtar
* ESXi aktarım düğümü için IP Havuzu
* Sınır taşıma düğümü için IP Havuzu
* Kenar düğümleri
* Denetleyici ve uç VM 'Ler için DRS, benzeşim önleme kuralı
* Katman 0 yönlendirici
* Tier0 yönlendiricisinde BGP 'yi etkinleştir

## <a name="vsphere-cluster"></a>vSphere kümesi

ESXi Konakları, AVS özel bulutunun yüksek oranda kullanılabilirliğini sağlamak için bir küme olarak yapılandırılır. Bir AVS özel bulutu oluşturduğunuzda, vSphere Yönetim bileşenleri ilk kümede dağıtılır. Yönetim bileşenleri için bir kaynak havuzu oluşturulur ve tüm yönetim VM 'Leri bu kaynak havuzunda dağıtılır. İlk küme, AVS özel bulutunu daraltmak için silinemez. vSphere kümesi, **VSPHERE ha**kullanan VM 'ler için yüksek kullanılabilirlik sağlar. Tolerans sorunları, kümedeki kullanılabilir düğümlerin sayısını temel alır. ```N```, kabul edilecek başarısızlık sayısıdır ```Number of nodes = 2N+1``` formülünü kullanabilirsiniz.

### <a name="vsphere-cluster-limits"></a>vSphere kümesi sınırları

| Kaynak | Sınır |
|----------|-------|
| Bir AVS özel bulutu oluşturmak için gereken en az düğüm sayısı (ilk vSphere kümesi) | 3 |
| Bir AVS özel bulutu 'ndaki vSphere kümesindeki en fazla düğüm sayısı | 16 |
| Bir AVS özel bulutundaki en fazla düğüm sayısı | 64 |
| Bir AVS özel bulutu 'nda maksimum vSphere kümesi sayısı | 21 |
| Yeni bir vSphere kümesindeki düğüm sayısı alt sınırı | 3 |

## <a name="vmware-infrastructure-maintenance"></a>VMware altyapı Bakımı

Bazen VMware altyapısının yapılandırmasında değişiklik yapmak gerekir. Şu anda bu aralıklar her 1-2 ayda bir gerçekleşebilir, ancak zamanın zaman içinde reddetmesi beklenir. Bu tür bir bakım, genellikle AVS hizmetlerinin normal tüketimini kesintiye uğramadan yapılabilir. Bir VMware bakım aralığı sırasında, aşağıdaki hizmetler herhangi bir etki olmadan çalışmaya devam eder:

* VMware yönetim düzlemi ve uygulamaları
* vCenter erişimi
* Tüm ağ ve depolama
* Tüm Azure trafiği

## <a name="updates-and-upgrades"></a>Güncelleştirmeler ve yükseltmeler

AVS, AVS özel bulutu 'ndaki VMware yazılımının (ESXi, vCenter, PSC ve NSX) yaşam döngüsü yönetiminden sorumludur.

Yazılım güncelleştirmeleri şunları içerir:

* **Düzeltme ekleri**. VMware tarafından yayınlanan güvenlik düzeltme ekleri veya hata düzeltmeleri.
* **Güncelleştirmeler**. VMware Stack bileşeninin küçük sürüm değişikliği.
* **Yükseltmeleri**. VMware Stack bileşeninin ana sürüm değişikliği.

AVS, VMware 'den kullanılabilir hale geldiğinde kritik bir güvenlik düzeltme ekini sınar. SLA başına, AVS bir haftada bir hafta içinde özel bulut ortamlarına güvenlik düzeltme ekini kaydeder.

AVS, VMware yazılım bileşenlerine üç aylık bakım güncelleştirmeleri sağlar. VMware yazılımının yeni bir ana sürümü kullanılabilir olduğunda, AVS, yükseltme için uygun bir bakım penceresini koordine etmek üzere müşterilerle birlikte çalışarak. 

## <a name="next-steps"></a>Sonraki adımlar

* [AVS bakım ve güncelleştirmeleri](cloudsimple-maintenance-updates.md)

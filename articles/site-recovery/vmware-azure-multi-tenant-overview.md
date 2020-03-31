---
title: Azure Site Kurtarma ile VMware VM çok kiracılı olağanüstü durum kurtarma
description: Çok kiracılı bir ortamda (CSP) programda VMWare olağanüstü durum kurtarma için Azure'a olağanüstü durum kurtarma desteğine genel bir bakış sağlar.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: 840049265d3b6e4d2fddd794646bfd5691aab9a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74083985"
---
# <a name="overview-of-multi-tenant-support-for-vmware-disaster-recovery-to-azure-with-csp"></a>CSP ile Azure'a VMware olağanüstü durum kurtarma için çok kiracılı desteğe genel bakış

[Azure Site Kurtarma,](site-recovery-overview.md) kiracı abonelikleri için çok kiracılı ortamları destekler. Ayrıca, Microsoft Bulut Çözüm Sağlayıcısı (CSP) programı aracılığıyla oluşturulan ve yönetilen kiracı abonelikleri için çoklu kirayı destekler.

Bu makalede, azure çoğaltma için çok kiracılı VMware uygulanması ve yönetilmesi ne genel bir bakış sağlar.

## <a name="multi-tenant-environments"></a>Çok kiracılı ortamlar

Üç büyük çok kiracılı model vardır:

* **Paylaşılan Barındırma Hizmetleri Sağlayıcısı (HSP)**: İş ortağı fiziksel altyapının sahibidir ve aynı altyapıda birden fazla kiracı VM barındırmak için paylaşılan kaynakları (vCenter, veri merkezleri, fiziksel depolama vb.) kullanır. İş ortağı, yönetilen bir hizmet olarak olağanüstü durum kurtarma yönetimi sağlayabilir veya kiracı self servis çözümü olarak olağanüstü durum kurtarma sahibi olabilir.

* **Özel Barındırma Hizmetleri Sağlayıcısı**: İş ortağı fiziksel altyapıya sahiptir, ancak her kiracının VM'lerini ayrı bir altyapıda barındırmak için özel kaynakları (birden çok vCenters, fiziksel veri depoları vb.) kullanır. İş ortağı, yönetilen bir hizmet olarak olağanüstü durum kurtarma yönetimi sağlayabilir veya kiracı bunu self servis bir çözüm olarak sahiplenebilir.

* **Yönetilen Hizmet Sağlayıcısı (MSP)**: Müşteri, VM'leri barındıran fiziksel altyapıya sahip olur ve iş ortağı olağanüstü durum kurtarma etkinleştirme ve yönetimi sağlar.

## <a name="shared-hosting-services-provider-hsp"></a>Paylaşılan barındırma hizmetleri sağlayıcısı (HSP)

Diğer iki senaryo, paylaşılan barındırma senaryosunun alt kümeleridir ve aynı ilkeleri kullanırlar. Farklar, paylaşılan barındırma kılavuzunun sonunda açıklanır.

Çok kiracılı bir senaryoda temel gereksinim, kiracıların yalıtılması dır. Bir kiracı, başka bir kiracının barındırdığı şeyi gözlemlememelidir. İş ortağı tarafından yönetilen bir ortamda, bu gereksinim, kritik olabileceği self servis ortamında olduğu kadar önemli değildir. Bu makalede, kiracı yalıtımı gerekli olduğunu varsayar.

Mimari aşağıdaki diyagramda gösterilmiştir.

![Paylaşılan HSP tek bir vCenter ile](./media/vmware-azure-multi-tenant-overview/shared-hosting-scenario.png)  

**Tek bir vCenter sunucusuyla paylaşılan barındırma**

Diyagramda, her müşterinin ayrı bir yönetim sunucusu vardır. Bu yapılandırma kiracıya özel VM'lere erişimi sınırlar ve kiracı yalıtımına olanak tanır. VMware VM çoğaltma, VM'leri keşfetmek ve aracıları yüklemek için yapılandırma sunucusunu kullanır. Aynı ilkeler, vCenter erişim denetimini kullanarak VM keşfini kısıtlamanın eklenmesiyle birlikte çok kiracılı ortamlar için de geçerlidir.

Veri yalıtımgereksinimi, tüm hassas altyapı bilgilerinin (erişim kimlik bilgileri gibi) kiracılara açıklanmayan olduğu anlamına gelir. Bu nedenle, yönetim sunucusunun tüm bileşenlerinin iş ortağının özel denetimi altında kalmasını öneririz. Yönetim sunucusu bileşenleri şunlardır:

* Yapılandırma Sunucusu
* İşlem sunucusu
* Ana hedef sunucu

Ayrı bir ölçeklenmiş işlem sunucusu da ortağın denetimi altındadır.

## <a name="configuration-server-accounts"></a>Yapılandırma sunucusu hesapları

Çok kiracılı senaryodaki her yapılandırma sunucusu iki hesap kullanır:

- **vCenter erişim hesabı**: Bu hesap kiracı VM'leri bulmak için kullanılır. Bu vCenter erişim izinleri atanmış vardır. Erişim sızıntılarını önlemeye yardımcı olmak için, iş ortaklarının bu kimlik bilgilerini yapılandırma aracına kendileri girmelerini öneririz.

- **Sanal makine erişim hesabı**: Bu hesap, mobilite servis aracısını kiracı VM'lerine otomatik itme ile yüklemek için kullanılır. Genellikle kiracının bir ortağa sağlayabileceği bir etki alanı hesabı veya iş ortağının doğrudan yönetebileceği bir hesaptır. Kiracı ayrıntıları doğrudan iş ortağıyla paylaşmak istemiyorsa, yapılandırma sunucusuna sınırlı süreli erişim yoluyla kimlik bilgilerini girebilir. Veya ortağın yardımıyla Mobilite servis aracısını el ile yükleyebilirler.

## <a name="vcenter-account-requirements"></a>vCenter hesap gereksinimleri

Yapılandırma sunucusunu, özel bir rolü olan bir hesapla yapılandırın.

- Rol ataması, her vCenter nesnesi için vCenter erişim hesabına uygulanmalı ve alt nesnelere yayılmamalıdır. Bu yapılandırma kiracı yalıtımı sağlar, çünkü erişim yayılımı diğer nesnelere yanlışlıkla erişime neden olabilir.

    ![Alt Nesnelere Yayılma seçeneği](./media/vmware-azure-multi-tenant-overview/assign-permissions-without-propagation.png)

- Alternatif yaklaşım, kullanıcı hesabını ve rolünü veri merkezi nesnesine atamak ve bunları alt nesnelere yaymaktır. Ardından, belirli bir kiracıtarafından erişilemeyen her nesne (diğer kiracılara ait VM'ler gibi) için hesaba **erişim yok** rolü verin. Bu yapılandırma hantal. Her yeni alt nesne de otomatik olarak üst devralınan erişim verilir, çünkü yanlışlıkla erişim denetimleri ortaya çıkarır. Bu nedenle, ilk yaklaşımı kullanmanızı öneririz.

### <a name="create-a-vcenter-account"></a>vCenter hesabı oluşturma

1. Önceden tanımlanmış *Salt Okunur* rolünü klonlayarak yeni bir rol oluşturun ve ardından uygun bir ad verin (bu örnekte gösterildiği gibi Azure_Site_Recovery gibi).
2. Bu rol için aşağıdaki izinleri atayın:

   * **Datastore**: Boşluk ayırma, datastore'a göz atma, Düşük seviyeli dosya işlemleri, Dosyayı kaldırma, sanal makine dosyalarını güncelleme
   * **Ağ**: Ağ atama
   * **Kaynak**: Kaynak havuzuna VM atama, VM'den güç alan geçir, VM'de geçiş
   * **Görevler**: Görev oluşturma, görevi güncelleştirme
   * **VM - Yapılandırma**: Tümü
   * **VM - Etkileşim** > Cevap soru, Cihaz bağlantısı, CD medya yapılandırma, Disket medya yapılandırma, Güç kapalı, Güç, VMware araçları yüklemek
   * **VM - Stok** > Varolandan Oluştur, Yeni Oluştur, Kayıt Ol, Kayıt Tan
   * **VM - Sağlama** > Sanal makine indirme izin ver, sanal makine dosyaları yükleme izin ver
   * **VM - Anlık görüntü yönetimi** > Anlık görüntüleri kaldırma

       ![Rolü Edin iletişim kutusu](./media/vmware-azure-multi-tenant-overview/edit-role-permissions.png)

3. Çeşitli nesneler için vCenter hesabına (kiracı yapılandırma sunucusunda kullanılan) erişim düzeyleri atama:

>| Nesne | Rol | Açıklamalar |
>| --- | --- | --- |
>| vCenter | Salt Okunur | Yalnızca farklı nesneleri yönetmek için vCenter erişimine izin vermek için gereklidir. Hesap hiçbir zaman kiracıya sağlanmayacaksa veya vCenter'daki yönetim işlemleri için kullanılacaksa bu izni kaldırabilirsiniz. |
>| Veri merkezi | Azure_Site_Recovery |  |
>| Ana bilgisayar ve ana bilgisayar kümesi | Azure_Site_Recovery | Erişimin nesne düzeyinde olmasını yeniden sağlar, böylece yalnızca erişilebilir ana bilgisayarların başarısız olmadan önce ve sonra kiracı VM'leri vardır. |
>| Datastore ve datastore kümesi | Azure_Site_Recovery | Öncekiyle aynı. |
>| Ağ | Azure_Site_Recovery |  |
>| Yönetim sunucusu | Azure_Site_Recovery | CS makinesi dışındaki tüm bileşenlere (CS, PS ve MT) erişim içerir. |
>| Kiracı VMs | Azure_Site_Recovery | Belirli bir kiracının yeni kiracı VM'lerinin de bu erişime sahip olmasını sağlar veya Azure portalı aracılığıyla bulunamaz. |

vCenter hesap erişimi tamamlandı. Bu adım, başarısız işlemleri tamamlamak için minimum izin gereksinimini karşılar. Bu erişim izinlerini varolan ilkelerinizle de kullanabilirsiniz. Daha önce ayrıntılı olarak açıklandığı adım 2'deki rol izinlerini içerecek şekilde ayarlanmış varolan izinlerinizi değiştirmeniz yeterlidir.

### <a name="failover-only"></a>Yalnızca failover
Olağanüstü durum kurtarma işlemlerini yalnızca başarısız olana kadar kısıtlamak için (diğer bir şekilde, geri dönüş özellikleri olmadan), aşağıdaki durumlar dışında önceki yordamı kullanın:

- *Azure_Site_Recovery* rolünü vCenter erişim hesabına atamak yerine, bu hesaba yalnızca *Salt Okunur* rolü atayın. Bu izin kümesi VM çoğaltma ve başarısızlık sağlar ve failback izin vermez.
- Önceki süreçte ki diğer her şey olduğu gibi kalır. Kiracı yalıtımı sağlamak ve VM bulmasını kısıtlamak için, her izin yine de yalnızca nesne düzeyinde atanır ve alt nesnelere yayılmaz.

### <a name="deploy-resources-to-the-tenant-subscription"></a>Kaynakları kiracı aboneliğine dağıtma

1. Azure portalında bir kaynak grubu oluşturun ve ardından her zamanki işleme göre bir Kurtarma Hizmetleri kasası dağıtın.
2. Kasa kayıt anahtarını indirin.
3. Kasa kayıt anahtarını kullanarak kiracı için CS kaydedin.
4. İki erişim hesabının kimlik bilgilerini, vCenter sunucusuna erişmek için hesabı ve VM'ye erişmek için hesabı girin.

    ![Yönetici yapılandırma sunucusu hesapları](./media/vmware-azure-multi-tenant-overview/config-server-account-display.png)

### <a name="register-servers-in-the-vault"></a>Sunucuları kasaya kaydet

1. Azure portalında, daha önce oluşturduğunuz kasada, oluşturduğunuz vCenter hesabını kullanarak vCenter sunucusunu yapılandırma sunucusuna kaydedin.
2. Her zamanki işleme göre Site Kurtarma için "Altyapı hazırlama" işlemini tamamlayın.
3. VM'ler artık çoğaltılmaya hazır. **Çoğaltma** > **Select sanal makinelerinde**yalnızca kiracının Sanal M'lerinin görüntülendiğini doğrulayın.

## <a name="dedicated-hosting-solution"></a>Özel barındırma çözümü

Aşağıdaki diyagramda gösterildiği gibi, özel bir barındırma çözümündeki mimari fark, her kiracının altyapısının yalnızca o kiracı için ayarlanmış olmasıdır.

![mimari-paylaşılan-hsp](./media/vmware-azure-multi-tenant-overview/dedicated-hosting-scenario.png)  
**Birden fazla vCenters ile özel barındırma senaryosu**

## <a name="managed-service-solution"></a>Yönetilen servis çözümü

Aşağıdaki diyagramda gösterildiği gibi, yönetilen bir hizmet çözümündeki mimari fark, her kiracının altyapısının diğer kiracıların altyapısından fiziksel olarak ayrı olmasıdır. Bu senaryo genellikle kiracı altyapıya sahip olduğunda ve olağanüstü durum kurtarmayı yönetmek için bir çözüm sağlayıcısı istediğinde bulunur.

![mimari-paylaşılan-hsp](./media/vmware-azure-multi-tenant-overview/managed-service-scenario.png)  
**Birden çok vCenters ile yönetilen hizmet senaryosu**

## <a name="next-steps"></a>Sonraki adımlar
- Site Kurtarma'da rol tabanlı erişim denetimi hakkında [daha fazla bilgi edinin.](site-recovery-role-based-linked-access-control.md)
- [VMware VM'lerin Azure'a nasıl olağanüstü durum kurtarma sını ayarlayacağım öğrenin.](vmware-azure-tutorial.md)
- [VMWare VM'ler için CSP ile çoklu kiralama](vmware-azure-multi-tenant-csp-disaster-recovery.md)hakkında daha fazla bilgi edinin.

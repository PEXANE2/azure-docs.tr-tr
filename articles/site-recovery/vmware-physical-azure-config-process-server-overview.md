---
title: Azure Site Recovery yapılandırma/işlem/ana hedef sunucuları hakkında
description: Bu makalede, Azure Site Recovery ile şirket içi VMware VM 'lerinin olağanüstü durum kurtarması ayarlanırken yapılandırma, süreç ve ana hedef sunuculara genel bir bakış sunulmaktadır
ms.topic: conceptual
ms.date: 03/17/2020
ms.openlocfilehash: cd5ded18d1a8f1f5fd96212d37725bb5db13002f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80062082"
---
# <a name="about-site-recovery-components-configuration-process-master-target"></a>Site Recovery bileşenleri (yapılandırma, işlem, ana hedef) hakkında

Bu makalede, VMware VM 'lerini ve fiziksel sunucuları Azure 'a çoğaltmak için [Site Recovery](site-recovery-overview.md) hizmeti tarafından kullanılan yapılandırma, işlem ve ana hedef sunucuları açıklanmaktadır.

## <a name="configuration-server"></a>Yapılandırma Sunucusu

Şirket içi VMware VM 'Leri ve fiziksel sunucular için olağanüstü durum kurtarma için, şirket içi Site Recovery yapılandırma sunucusu dağıtın.

**Ayar** | **Ayrıntılar** | **Bağlantılar**
--- | --- | ---
**Bileşenler**  | Yapılandırma sunucusu makinesi, yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucu dahil olmak üzere tüm şirket içi Site Recovery bileşenlerini çalıştırır.<br/><br/> Yapılandırma sunucusunu ayarlarken, tüm bileşenler otomatik olarak yüklenir. | Yapılandırma sunucusu SSS [makalesini okuyun](vmware-azure-common-questions.md#configuration-server) .
**Role** | Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir. | Azure 'da [VMware](vmware-azure-architecture.md) ve [fiziksel sunucu](physical-azure-architecture.md) olağanüstü durum kurtarma mimarisi hakkında daha fazla bilgi edinin.
**VMware gereksinimleri** | Şirket içi VMware VM 'lerinin olağanüstü durum kurtarması için yapılandırma sunucusunu şirket içi, yüksek oranda kullanılabilir bir VMware VM 'si olarak yükleyip çalıştırmanız gerekir. | Önkoşullar [hakkında bilgi edinin](vmware-azure-deploy-configuration-server.md#prerequisites) .
**VMware dağıtımı** | Yapılandırma sunucusunu indirilen bir OVA şablonu kullanarak dağıtmanızı öneririz. Bu yöntem, tüm gereksinimleri ve önkoşulları karşılayan bir yapılandırma sunucusu kurmak için basit bir yol sağlar.<br/><br/> Bir nedenden dolayı bir OVA şablonu kullanarak bir VMware VM 'yi dağıtamazsınız, yapılandırma sunucusu makinelerini fiziksel makine olağanüstü durum kurtarma için aşağıda açıklandığı gibi el ile ayarlayabilirsiniz. | Bir OVA şablonuyla [dağıtın](vmware-azure-deploy-configuration-server.md#deploy-a-configuration-server-through-an-ova-template) .
**Fiziksel sunucu gereksinimleri** | Şirket içi fiziksel sunucularda olağanüstü durum kurtarma için yapılandırma sunucusunu el ile dağıtırsınız. | Önkoşullar [hakkında bilgi edinin](physical-azure-set-up-source.md#prerequisites) .
**Fiziksel sunucu dağıtımı** | VMware VM olarak yüklenebir fiziksel sunucuya yükleyebilirsiniz. | Yapılandırma sunucusunu el ile [dağıtın](physical-azure-set-up-source.md#set-up-the-source-environment) .

## <a name="process-server"></a>İşlem sunucusu

İşlem sunucusu, yük devretme ve yeniden çalışma sırasında çoğaltma verilerini işler ve şirket içi VMware VM 'Leri ve fiziksel sunucular için Mobility hizmetini kurar.

**Ayar** | **Ayrıntılar** | **Bağlantılar**
--- | --- | ---
**Dağıtım**  | Varsayılan olarak, yapılandırma sunucusu dağıtıldığında işlem sunucusu yüklenir. <br/><br/> Şirket içi VMware VM 'Leri ve fiziksel sunucular için olağanüstü durum kurtarma ve çoğaltma için bir şirket içi işlem sunucusu gereklidir. | [Daha fazla bilgi edinin](vmware-azure-architecture.md#architectural-components).
**Rol (Şirket içi**) | Çoğaltma için etkinleştirilen makinelerden çoğaltma verilerini alır. <br/><br/> Önbelleğe alma, sıkıştırma ve şifreleme ile çoğaltma verilerini iyileştirir ve Azure depolama 'ya gönderir. <br/><br/> Şirket içi VMware VM 'lerinde ve çoğaltmak istediğiniz fiziksel sunucularda Site Recovery Mobility hizmetinin anında yüklenmesini gerçekleştirir. <br/><br/> Şirket içi makinelerin otomatik olarak bulunmasını gerçekleştirir. | [Daha fazla bilgi edinin](vmware-azure-enable-replication.md).
**Rol (Azure 'dan yeniden çalışma)** | Şirket içi sitenizden yük devretmeden sonra, şirket içi konumunuza yeniden çalışmayı işlemek için Azure sanal makinesi olarak Azure 'da bir işlem sunucusu ayarlarsınız.<br/><br/> Azure 'daki işlem sunucusu geçicidir. Yeniden çalışma yapıldıktan sonra Azure VM silinebilir. | [Daha fazla bilgi edinin](vmware-azure-set-up-process-server-azure.md).
**Ölçeklendirme** | Daha büyük dağıtımlar için, şirket içi ek, genişleme işlem sunucuları ayarlayabilirsiniz. Ek sunucular, daha fazla sayıda çoğaltma makinesini ve daha büyük depolama kapasitesi trafiğini işleyerek kapasiteyi ölçeklendirir.<br/><br/> Çoğaltma trafiğinin yükünü dengelemek için makineleri iki işlem sunucusu arasında taşıyabilirsiniz. | [Daha fazla bilgi edinin](vmware-azure-set-up-process-server-scale.md).

## <a name="master-target-server"></a>Ana hedef sunucu

Ana hedef sunucu, Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.

- Varsayılan olarak, ana hedef sunucusu yapılandırma sunucusuna yüklenir.
- Büyük dağıtımlar için yeniden çalışma için ek ve ayrı bir ana hedef sunucu ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- VMware VM 'Leri ve fiziksel sunucular için olağanüstü durum kurtarma [mimarisini](vmware-azure-architecture.md) gözden geçirin.
- VMware VM 'Leri ve fiziksel sunucuları Azure 'a olağanüstü durum kurtarmaya yönelik [gereksinimleri ve önkoşulları](vmware-physical-azure-support-matrix.md) gözden geçirin.

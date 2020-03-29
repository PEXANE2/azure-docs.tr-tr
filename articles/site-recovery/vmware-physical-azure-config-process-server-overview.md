---
title: Azure Site Kurtarma yapılandırması/işlem/ana hedef sunucular hakkında
description: Bu makalede, azure site kurtarma ile şirket içi VMware VM'lerin Azure'a olağanüstü kurtarma sını ayarlarken kullandıkları yapılandırma, işlem ve ana hedef sunuculara genel bir bakış
ms.topic: conceptual
ms.date: 03/17/2020
ms.openlocfilehash: cd5ded18d1a8f1f5fd96212d37725bb5db13002f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062082"
---
# <a name="about-site-recovery-components-configuration-process-master-target"></a>Site Kurtarma bileşenleri hakkında (yapılandırma, işlem, ana hedef)

Bu makalede, VMware VM'leri ve fiziksel sunucuları Azure'da çoğaltmak için [Site Kurtarma](site-recovery-overview.md) hizmeti tarafından kullanılan yapılandırma, işlem ve ana hedef sunucular açıklanmaktadır.

## <a name="configuration-server"></a>Yapılandırma Sunucusu

Şirket içi VMware VM'lerinin ve fiziksel sunucuların olağanüstü kurtarma durumu için, şirket içinde bir Site Kurtarma yapılandırma sunucusu dağıtın.

**Ayar** | **Şey** | **Bağlantılar**
--- | --- | ---
**Bileşenler**  | Yapılandırma sunucusu makinesi, yapılandırma sunucusu, işlem sunucusu ve ana hedef sunucuyu içeren tüm şirket içi Site Kurtarma bileşenlerini çalıştırın.<br/><br/> Yapılandırma sunucusunu ayarladığınızda, tüm bileşenler otomatik olarak yüklenir. | Yapılandırma sunucusu SSS'yi [okuyun.](vmware-azure-common-questions.md#configuration-server)
**Rol** | Yapılandırma sunucusu yerinde bileşenler ile Azure arasındaki iletişimi düzenler ve veri çoğaltma işlemlerini yönetir. | [Azure'da VMware](vmware-azure-architecture.md) mimarisi ve [fiziksel sunucu](physical-azure-architecture.md) olağanüstü durum kurtarma hakkında daha fazla bilgi edinin.
**VMware gereksinimleri** | Şirket içi VMware VM'lerin olağanüstü kurtarma sı için yapılandırma sunucusunu şirket içi, yüksek kullanılabilir vmware VM olarak yüklemeniz ve çalıştırmanız gerekir. | Ön koşullar [hakkında bilgi edinin.](vmware-azure-deploy-configuration-server.md#prerequisites)
**VMware dağıtımı** | İndirilen BIR OVA şablonu kullanarak yapılandırma sunucusunu dağıtmanızı öneririz. Bu yöntem, tüm gereksinimlere ve ön koşullara uygun bir yapılandırma sunucusu kurmak için basit bir yol sağlar.<br/><br/> Herhangi bir nedenle BIR VMware VM'yi OVA şablonu kullanarak dağıtamıyorsanız, fiziksel makine olağanüstü durum kurtarma için aşağıda açıklandığı gibi yapılandırma sunucusu makinelerini el ile ayarlayabilirsiniz. | BIR OVA şablonuyla [dağıtın.](vmware-azure-deploy-configuration-server.md#deploy-a-configuration-server-through-an-ova-template)
**Fiziksel sunucu gereksinimleri** | Şirket içi fiziksel sunucularda olağanüstü durum kurtarma için yapılandırma sunucusunu el ile dağıtabilirsiniz. | Ön koşullar [hakkında bilgi edinin.](physical-azure-set-up-source.md#prerequisites)
**Fiziksel sunucu dağıtımı** | VMware VM olarak yüklenemiyorsa, fiziksel bir sunucuya yükleyebilirsiniz. | Yapılandırma sunucusunu el ile [dağıtın.](physical-azure-set-up-source.md#set-up-the-source-environment)

## <a name="process-server"></a>İşlem sunucusu

Bir işlem sunucusu, failover ve failback sırasında çoğaltma verilerini işler ve şirket içi VMware VM'ler ve fiziksel sunucular için Mobilite hizmetini yükler.

**Ayar** | **Şey** | **Bağlantılar**
--- | --- | ---
**Dağıtım**  | Varsayılan olarak, yapılandırma sunucusu dağıtıldığında işlem sunucusu yüklenir. <br/><br/> Olağanüstü durum kurtarma ve şirket içi VMware VM'lerin ve fiziksel sunucuların çoğaltılması için şirket içi işlem sunucusu gereklidir. | [Daha fazla bilgi edinin](vmware-azure-architecture.md#architectural-components).
**Rol (şirket içi)** | Çoğaltma için etkinleştirilen makinelerden çoğaltma verileri alır. <br/><br/> Çoğaltma verilerini önbelleğe alma, sıkıştırma ve şifreleme yle en iyi duruma getirin ve Azure Depolama'ya gönderir. <br/><br/> Site Kurtarma Mobilite hizmetinin şirket içi VMware VM'leri ve çoğaltmak istediğiniz fiziksel sunucularda itme yüklemesini gerçekleştirir. <br/><br/> Şirket içi makinelerin otomatik olarak keşfini gerçekleştirir. | [Daha fazla bilgi edinin](vmware-azure-enable-replication.md).
**Rol (Azure'dan geri dönüş)** | Şirket içi sitenizden geçemedikten sonra, şirket içi konumunuza geri dönme işlemini işlemek için Azure'da Azure'da Azure VM olarak bir işlem sunucusu ayarlarsınız.<br/><br/> Azure'daki işlem sunucusu geçicidir. Azure VM geri ödeme yapıldıktan sonra silinebilir. | [Daha fazla bilgi edinin](vmware-azure-set-up-process-server-azure.md).
**Ölçekleme** | Daha büyük dağıtımlar için şirket içinde ek, ölçeklendirilebilir işlem sunucuları ayarlayabilirsiniz. Ek sunucular, daha fazla sayıda çoğaltma makinesini ve daha büyük hacimli çoğaltma trafiğini işleyerek kapasiteyi ölçeklendirin.<br/><br/> Bakiye çoğaltma trafiğini yüklemek için makineleri iki işlem sunucusu arasında taşıyabilirsiniz. | [Daha fazla bilgi edinin](vmware-azure-set-up-process-server-scale.md).

## <a name="master-target-server"></a>Ana hedef sunucu

Ana hedef sunucu, Azure'dan yeniden çalışma sırasında çoğaltma verilerini işler.

- Varsayılan olarak, ana hedef sunucu yapılandırma sunucusuna yüklenir.
- Büyük dağıtımlar için, failback için ek, ayrı bir ana hedef sunucu ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- VMware VM'lerin ve fiziksel sunucuların olağanüstü durum kurtarma [mimarisini](vmware-azure-architecture.md) gözden geçirin.
- VMware VM'lerinin ve fiziksel sunucuların Azure'da olağanüstü durum kurtarma gereksinimlerini ve [ön koşulları](vmware-physical-azure-support-matrix.md) gözden geçirin.

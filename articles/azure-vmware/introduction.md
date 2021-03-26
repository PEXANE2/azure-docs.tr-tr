---
title: Giriş
description: Azure 'da VMware tabanlı iş yüklerini dağıtmak ve yönetmek için Azure VMware çözümünün özelliklerini ve avantajlarını öğrenin.
ms.topic: overview
ms.date: 03/24/2021
ms.openlocfilehash: a75c04cf6b51068ef9867e3f9438b2820da703fe
ms.sourcegitcommit: ed7376d919a66edcba3566efdee4bc3351c57eda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2021
ms.locfileid: "105048825"
---
# <a name="what-is-azure-vmware-solution"></a>Azure VMware Çözümü nedir?

Azure VMware çözümü, adanmış çıplak Azure altyapısından oluşturulan vSphere kümelerini içeren özel bulutlar sağlar. En düşük ilk dağıtım üç ana bilgisayar olmakla kalmaz, küme başına en fazla 16 ana bilgisayar için aynı anda ek konaklar eklenebilir.  Sağlanan tüm özel bulutların vCenter Server, vSAN, vSphere ve NSX-T vardır. Şirket içi ortamlarınızdaki iş yüklerini geçirebilir, yeni sanal makineler (VM 'Ler) dağıtabilir ve özel bulutlarınızdaki Azure hizmetlerini kullanabilirsiniz.

Azure VMware çözümü, geliştirme ve yükseltmelerin üzerinde doğrulanmasını ve test edilmesine yönelik bir VMware tarafından doğrulanan çözümdür. Microsoft özel bulut altyapısını ve yazılımlarını yönetir ve korur. Özel bulutlarınızın iş yüklerini geliştirmeye ve çalıştırmaya odaklanabilmenize olanak tanır. 

Diyagramda, Azure, Azure hizmetleri ve şirket içi ortamlarda özel bulutlar ile sanal ağlar arasındaki Bitişiklik gösterilmektedir. Özel bulutlardan Azure hizmetlerine veya sanal ağlara ağ erişimi, Azure hizmet uç noktaları için SLA odaklı tümleştirme sağlar. ExpressRoute Global Reach şirket içi ortamınızı Azure VMware çözümünüz özel bulutuna bağlar. 

![Azure VMware çözümünün özel bulut Bitişiklik ve şirket içi](./media/adjacency-overview-drawing-final.png)

## <a name="customer-communication"></a>Müşteri iletişimi
Hizmet sorunları, planlı bakım, sistem durumu danışmanları, Güvenlik Danışma belgeleri bildirimleri Azure portal **hizmet durumu** üzerinden yayımlanır. Bu bildirimler için etkinlik günlüğü uyarılarını ayarlarken zamanında işlem yapabilirsiniz. Daha fazla bilgi için bkz. [Azure Portal kullanarak hizmet durumu uyarıları oluşturma](../service-health/alerts-activity-log-service-notifications-portal.md#create-service-health-alert-using-azure-portal).

:::image type="content" source="media/service-health.png" alt-text="Hizmet durumu bildirimlerinin ekran görüntüsü":::

## <a name="hosts-clusters-and-private-clouds"></a>Konaklar, kümeler ve özel bulutlar

Azure VMware çözümü özel bulutları ve kümeleri, çıplak, hiper yakınsanmış bir Azure altyapı konağından oluşturulmuştur. Yüksek kaliteli konaklarda 576 GB RAM ve ikili Intel 18 Core, 2,3 GHz işlemcileri vardır. Ana bilgisayarlarında, ham vSAN kapasite katmanının 15,36 TB (SSD) ve 3,2 TB (NVMe) vSAN önbellek katmanı olan iki vSAN diskgroups vardır.

Yeni özel bulutlar Azure portal veya Azure CLı aracılığıyla dağıtılır.

## <a name="networking"></a>Ağ

[!INCLUDE [avs-networking-description](includes/azure-vmware-solution-networking-description.md)]

Daha fazla bilgi için bkz. [ağ kavramları](concepts-networking.md).

## <a name="access-and-security"></a>Erişim ve güvenlik

Azure VMware Çözüm özel bulutları, gelişmiş güvenlik için vSphere rol tabanlı erişim denetimi kullanır. VSphere SSO LDAP yeteneklerini Azure Active Directory ile tümleştirebilirsiniz. Daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md).  

vSAN verileri Rest şifrelemesi, varsayılan olarak etkindir ve vSAN veri deposu güvenliğini sağlamak için kullanılır. Daha fazla bilgi için bkz. [depolama kavramları](concepts-storage.md).

## <a name="host-and-software-lifecycle-maintenance"></a>Konak ve yazılım yaşam döngüsü Bakımı

Azure VMware çözümü özel bulutu ve VMware yazılımının düzenli yükseltmeleri, en son güvenlik, kararlılık ve özellik kümelerinin özel bulutlarda çalıştığından emin olun. Daha fazla bilgi için bkz. [özel bulut güncelleştirmeleri ve yükseltmeleri](concepts-upgrades.md).

## <a name="monitoring-your-private-cloud"></a>Özel bulutunuzu izleme

Azure VMware çözümü aboneliğinize dağıtıldıktan sonra, [Azure izleyici günlükleri](../azure-monitor/overview.md) otomatik olarak oluşturulur. 

Özel bulutunuzda şunları yapabilirsiniz:
- Her sanal makinelerinizdeki günlükleri toplayın.
- Linux ve Windows VM 'lerine [MMA aracısını indirip yükleyin](../azure-monitor/agents/log-analytics-agent.md#installation-options) .
- [Azure tanılama uzantısını](../azure-monitor/agents/diagnostics-extension-overview.md)etkinleştirin.
- [Yeni sorgular oluşturun ve çalıştırın](../azure-monitor/logs/data-platform-logs.md#log-queries).
- Genellikle sanal makinelerinize çalıştırdığınız sorguların aynısını çalıştırın.

Azure VMware çözümünün içindeki izleme desenleri, IaaS platformunda Azure VM 'lerine benzerdir. Daha fazla bilgi ve Nasıl-TOS için bkz. Azure [izleyici Ile Azure VM 'Leri izleme](../azure-monitor/vm/monitor-vm-azure.md).

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım, anahtar [özel bulut ve küme kavramlarını](concepts-private-clouds-clusters.md)öğrenecekti.

<!-- LINKS - external -->

<!-- LINKS - internal -->
[concepts-private-clouds-clusters]: ./concepts-private-clouds-clusters.md

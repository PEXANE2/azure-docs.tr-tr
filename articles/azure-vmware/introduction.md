---
title: Giriş
description: Azure 'da VMware tabanlı iş yüklerini dağıtmak ve yönetmek için Azure VMware çözümünün özelliklerini ve avantajlarını öğrenin.
ms.topic: overview
ms.date: 05/04/2020
ms.openlocfilehash: b71427402e08167e5868128ed1e553c69bef425d
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91574475"
---
# <a name="what-is-azure-vmware-solution"></a>Azure VMware Çözümü nedir?

Azure VMware çözümü size Azure 'da özel bulutlar sağlar. Özel bulutlar ayrılmış çıplak Azure altyapısından oluşturulan vSphere kümelerini içerir. Özel bulutları 3 ile 16 konak arasında ölçeklendirebilir ve tek özel bulutta birden fazla küme bulundurabilirsiniz. Tüm özel bulutlar vCenter Server, vSAN, vSphere ve NSX-T ile sağlanır. Şirket içi ortamlarınızdaki iş yükünü geçirebilir, yeni sanal makineler oluşturabilir veya dağıtabilir ve özel bulutlarınızda Azure hizmetlerini kullanabilirsiniz.

Azure VMware çözümü, geliştirme ve yükseltmelerin üzerinde doğrulanmasını ve test edilmesine yönelik bir VMware tarafından doğrulanan çözümdür. Özel bulut altyapısı ve yazılımı Microsoft tarafından yönetilir ve sürdürülür ve bu, özel bulutlarınızın iş yüklerini geliştirmeye ve çalıştırmaya odaklanmanızı sağlar.

Aşağıdaki diyagramda, Azure, Azure hizmetleri ve şirket içi ortamlarda özel bulutlar ile sanal ağlar arasındaki Bitişiklik gösterilmektedir. Özel bulutlardan Azure hizmetlerine veya sanal ağlara ağ erişimi, Azure hizmet uç noktaları için SLA odaklı tümleştirme sağlar. Şirket içi ortamlarından özel bulut erişimi, özel ve güvenli bir bağlantı için ExpressRoute Global Reach kullanır.

![Azure VMware çözümünün özel bulut Bitişiklik ve şirket içi](./media/adjacency-overview-drawing-final.png)

## <a name="hosts-clusters-and-private-clouds"></a>Konaklar, kümeler ve özel bulutlar

Azure VMware çözümü özel bulutları ve kümeleri, çıplak, hiper yakınsanmış bir Azure altyapı konağından oluşturulmuştur. Yüksek kaliteli konaklarda 576 GB RAM ve ikili Intel 18 Core, 2,3 GHz işlemcileri vardır. Ana bilgisayarlarının toplam 15,36 TB (SSD) ham vSAN kapasite katmanını ve 3,2 TB (NVMe) vSAN önbellek katmanını içeren iki vSAN diskgroups vardır.

Yeni özel bulutlar Azure portal veya Azure CLı aracılığıyla dağıtılır.

## <a name="networking"></a>Ağ

[!INCLUDE [avs-networking-description](includes/azure-vmware-solution-networking-description.md)]

Ağ ve karşılıklı bağlantı hakkında daha fazla bilgi için [ağ kavramları](concepts-networking.md) makalesine bakın.

## <a name="access-and-security"></a>Erişim ve güvenlik

Gelişmiş güvenlik için Azure VMware çözümü özel bulutları vSphere rol tabanlı erişim denetimi kullanır. vSphere SSO LDAP özellikleri, Azure Active Directory ile tümleştirilebilir. Kimlik ve ayrıcalıklar hakkında daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md) makalesi.

vSAN verileri Rest şifrelemesi varsayılan olarak etkindir ve vSAN veri deposu güvenliğini sağlamak için kullanılır. [Depolama kavramları](concepts-storage.md) makalesinde daha ayrıntılı olarak açıklanmıştır.

## <a name="host-and-software-lifecycle-maintenance"></a>Konak ve yazılım yaşam döngüsü Bakımı

Azure VMware çözümü özel bulutu ve VMware yazılımının düzenli yükseltmeleri, en son güvenlik, kararlılık ve özellik kümelerinin özel bulutlarda çalışıyor olmasını sağlar. Platform bakımı ve yükseltmeleri hakkında daha fazla ayrıntı, [yükseltme kavramları](concepts-upgrades.md) makalesinde bulunabilir.

## <a name="monitoring-your-private-cloud"></a>Özel bulutunuzu izleme

Azure VMware çözümü aboneliğinize dağıtıldıktan sonra, [Azure izleyici günlükleri](../azure-monitor/overview.md) otomatik olarak oluşturulur. Ayrıca, özel bulutunuzun içindeki sanal makinelerinizde bulunan günlükleri toplayabilirsiniz. [MMA aracısını](../azure-monitor/platform/log-analytics-agent.md#installation-options) Azure VMware çözümünüz özel bulutlarında çalışan Linux ve Windows sanal makinelerine indirip yükleyebilir, ayrıca [Azure tanılama uzantısını](../azure-monitor/platform/diagnostics-extension-overview.md)etkinleştirebilirsiniz. Hatta, normalde sanal makinelerinizde çalıştırdığınız sorguların aynısını de çalıştırabilirsiniz. Sorgu oluşturma hakkında daha fazla bilgi için bkz. [sorguları yazma](../azure-monitor/log-query/log-query-overview.md#how-can-i-learn-how-to-write-queries). Azure VMware çözümünün içindeki izleme desenleri, IaaS platformu içindeki Azure sanal makinelerine benzerdir. Ek bilgi ve nasıl yapılır için bkz. Azure [izleyici Ile Azure sanal makinelerini izleme](../azure-monitor/insights/monitor-vm-azure.md).

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım, anahtar [özel bulut ve küme kavramlarını](concepts-private-clouds-clusters.md)öğrenecekti.

<!-- LINKS - external -->

<!-- LINKS - internal -->
[concepts-private-clouds-clusters]: ./concepts-private-clouds-clusters.md

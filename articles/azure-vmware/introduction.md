---
title: Giriş
description: Azure 'da VMware tabanlı iş yüklerini dağıtmak ve yönetmek için Azure VMware çözümünün (AVS) özelliklerini ve avantajlarını öğrenin.
ms.topic: overview
ms.date: 05/04/2020
ms.openlocfilehash: e227b6c7c2f25fbf62f11c17dc62293e4fbd5267
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82740335"
---
# <a name="what-is-azure-vmware-solution-avs-preview"></a>Azure VMware çözümü (AVS) önizlemesi nedir?

Azure VMware çözümü (AVS) size Azure 'da özel bulutlar sağlar. Özel bulutlar, adanmış çıplak Azure altyapısından oluşturulan vSphere kümelerini içerir. Özel bulut kümelerini, tek bir özel bulutta birden çok kümeye sahip olacak şekilde 3 ' ten 16 ana bilgisayar arasında ölçeklendirebilirsiniz. Tüm özel bulutlar vCenter Server, VSAN, vSphere ve NSX-T ile sağlanır. Şirket içi ortamlarınızdaki iş yüklerini geçirebilir, yeni sanal makineler oluşturabilir veya dağıtabilir ve özel bulutlarınızdaki Azure hizmetlerini kullanabilirsiniz.

AVS, geliştirme ve yükseltmelerin üzerinde doğrulanmasını ve test edilmesine yönelik bir VMware tarafından doğrulanan çözümdür. Özel bulut altyapısı ve yazılımı Microsoft tarafından yönetilir ve sürdürülür ve bu, özel bulutlarınızın iş yüklerini geliştirmeye ve çalıştırmaya odaklanmanızı sağlar.

Aşağıdaki diyagramda, Azure, Azure hizmetleri ve şirket içi ortamlarda özel bulutlar ile sanal ağlar arasındaki Bitişiklik gösterilmektedir. Özel bulutlardan Azure hizmetlerine veya sanal ağlara ağ erişimi, Azure hizmet uç noktaları için SLA odaklı tümleştirme sağlar. Şirket içi ortamlarından özel bulut erişimi, özel ve güvenli bir bağlantı için ExpressRoute Global Reach kullanır.

![AVS özel bulut Bitişiklik ve şirket içi Azure 'a ait görüntü](./media/adjacency-overview-drawing-final.png)

## <a name="hosts-clusters-and-private-clouds"></a>Konaklar, kümeler ve özel bulutlar

AVS özel bulutları ve kümeleri, çıplak, hiper yakınsanmış bir Azure altyapı konağından oluşturulmuştur. Yüksek kaliteli konaklarda 576 GB RAM ve ikili Intel 18 Core, 2,3 GHz işlemcileri vardır. Ana bilgisayarlarının toplam 15,36 TB (SSD) ham vSAN kapasite katmanını ve 3,2 TB (NVMe) vSAN önbellek katmanını içeren iki vSAN diskgroups vardır.

Yeni özel bulutlar Azure portal veya Azure CLı aracılığıyla dağıtılır.

## <a name="networking"></a>Ağ

Özel bir bulut dağıtıldığında, yönetim, sağlama ve vMotion için özel ağlar oluşturulur. Bu özel ağlar vCenter ve NSX-T Yöneticisi erişimi için ve sanal makine vMotion veya dağıtımı için kullanılır. Tüm özel ağlara Azure 'daki VNet 'ten veya şirket içi ortamlarından erişilebilir. ExpressRoute Global Reach, özel bulutları şirket içi ortamlara bağlamak için kullanılır ve bu bağlantı aboneliğinizde bir ExpressRoute devresine sahip VNet gerektirir.

Özel bir bulut dağıtıldığında Internet ve Azure hizmetlerine erişim sağlanır. Erişim, üretim iş yükü ağlarındaki VM 'Lerin Azure veya Internet tabanlı Hizmetleri kullanabilmesi için sağlanır. Internet erişimi, yeni özel bulutlar için varsayılan olarak devre dışıdır ve herhangi bir zamanda etkinleştirilebilir veya devre dışı bırakılabilir.

Ağ ve karşılıklı bağlantı hakkında daha fazla bilgi için [ağ kavramları](concepts-networking.md) makalesine bakın.

## <a name="access-and-security"></a>Erişim ve güvenlik

Gelişmiş güvenlik için, AVS özel bulutları vSphere rol tabanlı erişim denetimi kullanır. vSphere SSO LDAP özellikleri, Azure Active Directory ile tümleştirilebilir. Kimlik ve ayrıcalıklar hakkında daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md) makalesi.

vSAN verileri Rest şifrelemesi varsayılan olarak etkindir ve vSAN veri deposu güvenliğini sağlamak için kullanılır. [Depolama kavramları](concepts-storage.md) makalesinde daha ayrıntılı olarak açıklanmıştır.

## <a name="host-and-software-lifecycle-maintenance"></a>Konak ve yazılım yaşam döngüsü Bakımı

AVS özel bulutu ve VMware yazılımının düzenli yükseltmeleri, en son güvenlik, kararlılık ve özellik kümelerinin özel bulutlarda çalışıyor olmasını sağlar. Platform bakımı ve yükseltmeleri hakkında daha fazla ayrıntı, [yükseltme kavramları](concepts-upgrades.md) makalesinde bulunabilir.

## <a name="monitoring-your-private-cloud"></a>Özel bulutunuzu izleme

[Azure izleyici 'de](../azure-monitor/overview.md) , sanal MAKINELERINIZDE AVS özel bulutunuzda çalışan günlükleri toplayabilirsiniz. MMA aracısını, şirket içi sanal makinelerinizde çalıştırdığınız sorguları kullanarak AVS özel bulutlarınızda çalışan Linux ve Windows sanal makinelerine [indirebilir ve yükleyebilirsiniz](../azure-monitor/platform/log-analytics-agent.md#installation-and-configuration) . Normalde sanal makinelerinizde çalıştırdığınız sorguları yalnızca aynı şekilde çalıştırabilirsiniz. Sorgu oluşturma hakkında daha fazla bilgi için bkz. [sorguları yazma](../azure-monitor/log-query/log-query-overview.md#how-can-i-learn-how-to-write-queries).

## <a name="next-steps"></a>Sonraki adımlar

Bir sonraki adım, anahtar [özel bulut ve küme kavramlarını](concepts-private-clouds-clusters.md)öğrenecekti.

<!-- LINKS - external -->

<!-- LINKS - internal -->
[concepts-private-clouds-clusters]: ./concepts-private-clouds-clusters.md

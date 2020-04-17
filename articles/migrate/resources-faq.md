---
title: Azure Geçiş SSS
description: Azure Geçiş hizmeti yle ilgili sık sorulan soruların yanıtlarını alın.
ms.topic: conceptual
ms.date: 04/15/2020
ms.openlocfilehash: 9b71888b284fd9cc125def4758d1e3800a92acf2
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81530326"
---
# <a name="azure-migrate-common-questions"></a>Azure Geçiş: Sık sorulan sorular

Bu makalede, Azure Geçiş hakkında sık sorulan soruları yanıtlar. Bu makaleyi okuduktan sonra sorularınız varsa, bunları [Azure Geçiş forumunda](https://aka.ms/AzureMigrateForum)yayınlayabilirsiniz. Ayrıca bu makaleleri gözden geçirebilirsiniz:

- [Azure Geçiş cihazı](common-questions-appliance.md) yla ilgili sorular
- [Keşif, değerlendirme ve bağımlılık görselleştirme](common-questions-discovery-assessment.md) ile ilgili sorular

## <a name="what-is-azure-migrate"></a>Azure Geçişi nedir?

Azure Geçiş, şirket içi uygulamalarınızın ve iş yüklerinizin ve özel ve genel bulut VM'lerinizin Azure'a keşfini, değerlendirmesini ve geçişini izlemek için merkezi bir hub sağlar. Hub, değerlendirme ve geçiş için Azure Geçiş araçları ve üçüncü taraf ISV teklifleri sağlar. [Daha fazla bilgi edinin](migrate-services-overview.md).

## <a name="what-can-i-do-with-azure-migrate"></a>Azure Geçiş ile ne yapabilirim?

Şirket içi altyapıyı, uygulamaları ve verileri Azure'a keşfetmek, değerlendirmek ve geçirmek için Azure Geçiş'i kullanın. Azure Geçiş, şirket içi VMware VM'lerin, Hyper-V VM'lerin, fiziksel sunucuların, diğer sanallaştırılmış SANAL'ların, veritabanlarının, web uygulamalarının ve sanal masaüstü bilgisayarların değerlendirilmesini ve geçişini destekler. 

## <a name="whats-the-difference-between-azure-migrate-and-azure-site-recovery"></a>Azure Geçiş ve Azure Site Kurtarma arasındaki fark nedir?

[Azure Geçişi,](migrate-services-overview.md) değerlendirme ve Azure'a geçiş için merkezi bir hub sağlar. 

[Azure Site Kurtarma](../site-recovery/site-recovery-overview.md) bir olağanüstü durum kurtarma çözümüdür. 

Azure Geçiş: Sunucu Geçişi aracı, bazı şirket içi makinelerin kaldırma ve kaydırma geçişi için bazı arka uç Site Kurtarma işlevlerini kullanır.

## <a name="whats-the-difference-between-azure-migrate-server-assessment-and-the-map-toolkit"></a>Azure Geçiş: Sunucu Değerlendirmesi ve MAP Araç Seti arasındaki fark nedir?

Sunucu Değerlendirmesi, geçişe hazır olma ve Azure'a geçiş için iş yüklerinin değerlendirilmesi konusunda yardımcı olmak için değerlendirme sağlar. [Microsoft Değerlendirme ve Planlama (MAP) Araç Seti,](https://www.microsoft.com/download/details.aspx?id=7826) Windows istemcisi ve sunucu işletim sistemlerinin yeni sürümleri için geçiş planlaması ve yazılım kullanım izleme dahil olmak üzere diğer görevlere yardımcı olur. Bu senaryolar için MAP Araç Kiti'ni kullanmaya devam edin.

## <a name="whats-the-difference-between-server-assessment-and-the-site-recovery-deployment-planner"></a>Sunucu Değerlendirmesi ile Site Kurtarma Dağıtım Planlayıcısı arasındaki fark nedir?

Sunucu Değerlendirmesi bir geçiş planlama aracıdır. Site Kurtarma Dağıtım Planlayıcısı bir olağanüstü durum kurtarma planlama aracıdır.

Yapmak istediğiniz şeye göre aracınızı seçin:

- **Şirket içi Azure'a geçiş planı**: Şirket içi sunucularınızı Azure'a taşımayı planlıyorsanız, geçiş planlaması için Sunucu Değerlendirmesi'ni kullanın. Sunucu Değerlendirmesi şirket içi iş yüklerini değerlendirir ve geçiş edersiniz yardımcı olacak kılavuz lar ve araçlar sağlar. Geçiş planı kullanıma hazır olduktan sonra, makineleri Azure'a geçirmek için Azure Geçiş: Sunucu Geçişi gibi araçları kullanabilirsiniz.
- **Azure'a olağanüstü durum kurtarma planı :** Site Kurtarma ile şirket içi olağanüstü durum kurtarmayı şirket içinde Azure'a ayarlamayı planlıyorsanız, Site Kurtarma Dağıtım Planlayıcısı'nı kullanın. Dağıtım Planlayıcısı, olağanüstü durum kurtarma amacıyla şirket içi ortamınızın site içi Kurtarma'ya özgü derin bir değerlendirmesini sağlar. Çoğaltma ve başarısızlık gibi olağanüstü durum kurtarma yla ilgili öneriler sağlar.

## <a name="how-does-server-migration-work-with-site-recovery"></a>Site Kurtarma ile Sunucu Geçişi nasıl çalışır?

- Şirket içi VMware VM'lerin *aracısız* geçişini gerçekleştirmek için Azure Geçişi: Sunucu Geçişi'ni kullanıyorsanız, geçiş azure geçişine özgüdür ve Site Kurtarma kullanılmaz.
- VMware VM'lerin *aracı tabanlı* geçişini gerçekleştirmek için Azure Geçiş: Sunucu Geçişi'ni kullanıyorsanız veya Hyper-VM'leri veya fiziksel sunucuları geçişi yaparsanız, Azure Geçiş: Sunucu Geçişi Azure Site Kurtarma çoğaltma altyapısını kullanır.

## <a name="which-geographies-are-supported"></a>Hangi coğrafyalar desteklenir?

Kamu ve [hükümet bulutları](migrate-support-matrix.md#supported-geographies-azure-government)için desteklenen coğrafyaları gözden [geçirin.](migrate-support-matrix.md#supported-geographies-public-cloud)

## <a name="how-do-i-get-started"></a>Nasıl kullanmaya başlayabilirim?

İhtiyacınız olan aracı tanımlayın ve ardından aracı bir Azure Geçiş projesine ekleyin. 

ISV aracı veya Movere eklemek için:

1. Araç politikasına uygun olarak lisans alarak başlayın veya ücretsiz deneme sürümüne kaydolun. Araçlar için lisanslama ISV veya araç lisanslama modeline uygun olarak yapılır.
2. Her araçta, Azure Geçiş'e bağlanma seçeneği vardır. Aracı Azure Geçiş'e bağlamak için araç yönergelerini ve belgelerini izleyin.

Geçiş seyahatinizi Azure Geçiş projesi nin içinden, Azure'dan ve diğer araçlardan izleyebilirsiniz.

## <a name="how-do-i-delete-a-project"></a>Bir projeyi nasıl silebilirim?

Projeyi nasıl [sildiğinizi](how-to-delete-project.md)öğrenin. 

## <a name="next-steps"></a>Sonraki adımlar

Azure [Geçiş genel görünümünü](migrate-services-overview.md)okuyun.

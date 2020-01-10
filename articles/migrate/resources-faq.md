---
title: Azure geçişi ile ilgili sık sorulan sorular
description: Azure geçişi hizmeti hakkında sık sorulan soruların yanıtlarını alın.
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: snehaa
ms.openlocfilehash: 8bfa9237d365636c0bdaa3af06c5af23b683231d
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75563555"
---
# <a name="azure-migrate-common-questions"></a>Azure geçişi: sık sorulan sorular

Bu makalede, Azure geçişi hakkında sık sorulan sorular yanıtlanmaktadır. Bu makaleyi okuduktan sonra daha fazla sorgunuz varsa, bunları [Azure geçişi forumuna](https://aka.ms/AzureMigrateForum)gönderin. Başka sorularınız varsa, şu makaleleri gözden geçirin:

- Azure geçişi gereci hakkında [sorular](common-questions-appliance.md) .
- Bulma, değerlendirme ve bağımlılık görselleştirmesiyle ilgili [sorular](common-questions-discovery-assessment.md) .


## <a name="what-is-azure-migrate"></a>Azure Geçişi nedir?

Azure geçişi, şirket içi uygulamalarınızı, iş yüklerinizi ve özel/genel bulut VM 'lerini Azure 'a bulmayı, değerlendirmeyi ve geçirmeyi izlemek için bir merkezi Merkez sağlar. Hub, değerlendirme ve geçiş için Azure geçiş araçları ve ayrıca üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar. [Daha fazla bilgi edinin](migrate-services-overview.md).


## <a name="what-can-i-do-with-azure-migrate"></a>Azure geçişi ile ne yapabilirim?

Azure geçişi 'ni kullanarak şirket içi altyapıyı, uygulamaları ve verileri bulun, değerlendirin ve Azure 'a geçirin. Azure geçişi, şirket içi VMware VM 'Leri, Hyper-V VM 'Leri, fiziksel sunucular, diğer sanallaştırılmış VM 'Ler, veritabanları, Web uygulamaları ve sanal masaüstleri için değerlendirme ve geçişi destekler. 

## <a name="whats-the-difference-between-azure-migrate-and-site-recovery"></a>Azure geçişi ile Site Recovery arasındaki fark nedir?

Azure geçişi, değerlendirme ve Azure 'a geçiş için merkezi bir merkez sağlar. [Azure Site Recovery](../site-recovery/site-recovery-overview.md) bir olağanüstü durum kurtarma çözümüdür. Azure geçişi: sunucu geçiş aracı, bazı şirket içi makinelerin geçiş ve geçiş geçişi için bazı arka uç Site Recovery işlevleri kullanır.

## <a name="whats-the-difference-between-azure-migrate-server-assessment-and-the-map-toolkit"></a>Azure geçişi sunucu değerlendirmesi ve harıta araç seti arasındaki fark nedir?

Sunucu değerlendirmesi, geçiş hazırlığı ve Azure 'a geçiş için iş yüklerinin değerlendirmesi hakkında değerlendirme sağlar. [Microsoft Assessment and planlama (Map) araç seti](https://www.microsoft.com/download/details.aspx?id=7826) , Windows istemci/sunucu işletim sistemlerinin daha yeni sürümleri için geçiş planlaması ve yazılım kullanımı izleme dahil diğer görevlerle yardımcı olur. Bu senaryolar için harıta araç setini kullanmaya devam edin.

## <a name="whats-the-difference-between-server-assessment-and-the-site-recovery-deployment-planner"></a>Sunucu değerlendirmesi ve Site Recovery Dağıtım Planlayıcısı arasındaki fark nedir?

Sunucu değerlendirmesi bir geçiş planlama aracıdır. Site Recovery Dağıtım Planlayıcısı bir olağanüstü durum kurtarma planlama aracıdır.

- **Azure 'da şirket içi geçiş planlama**: Şirket Içi sunucularınızı Azure 'a geçirmeyi planlıyorsanız, geçiş planlaması Için Sunucu değerlendirmesi kullanın. Şirket içi iş yüklerini değerlendirir ve geçiş yapmanıza yardımcı olacak yönergeler ve araçlar sağlar. Geçiş planı kurulduktan sonra, makineleri Azure 'a geçirmek için Azure geçiş sunucusu geçişi de dahil olmak üzere araçları kullanabilirsiniz.
- **Azure 'da olağanüstü durum kurtarmayı planlayın**: Şirket içinden Azure 'a olağanüstü durum kurtarma ayarlamayı Site Recovery için Site Recovery dağıtım planlayıcısı kullanın. Dağıtım Planlayıcısı, olağanüstü durum kurtarma amacıyla Şirket içi ortamınızın derinlemesine ve Site Recovery özgü bir değerlendirmesini sağlar. Çoğaltma ve yük devretme gibi olağanüstü durum kurtarma konusunda öneriler sağlar.

## <a name="how-does-server-migration-work-with-site-recovery"></a>Sunucu geçişi Site Recovery ile nasıl çalışır?

- Şirket içi VMware VM 'lerinin aracısız geçişini gerçekleştirmek için Azure geçişi: sunucu geçişi ' ni kullanıyorsanız, geçiş işlemi Azure geçişi 'ne yereldir ve Site Recovery kullanılmaz.
- VMware VM 'lerinin aracı tabanlı geçişini gerçekleştirmek için Azure geçişi: sunucu geçişi kullanıyorsanız veya Hyper-V VM 'lerini veya fiziksel sunucuları geçirmek için Azure geçişi sunucu geçişi Azure Site Recovery çoğaltma altyapısını kullanır.


## <a name="which-geographies-are-supported"></a>Hangi geographıes destekleniyor?

[VMware VM](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) ve [Hyper-V VM 'Leri](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects)için Azure geçişi 'nin desteklenen coğrafi graflarını gözden geçirin.

## <a name="how-do-i-get-started"></a>Nasıl kullanmaya başlarım?

İhtiyacınız olan aracı tanımlayabilir ve bunu bir Azure geçişi projesine ekleyebilirsiniz. Bir ISV aracı veya Movere ekliyorsanız:
- Bir lisans edinerek veya ücretsiz deneme için kaydolduktan sonra araç ilkesine uygun olarak çalışmaya başlayın. Araçlar için lisanslama, ISV veya araç lisanslama modeline göre belirlenir.
- Her araçta Azure geçişi 'ne bağlanma seçeneği vardır. Aracı Azure geçişi ile bağlamak için araç yönergelerini ve belgeleri izleyin.
Azure geçişi projesi içindeki geçiş yolculuğunun Azure ve diğer araçların tamamında merkezi olarak izlenmesi.

### <a name="how-do-i-delete-a-project"></a>Nasıl yaparım? bir proje silinsin mi?

Projeyi silmeyi [öğrenin](how-to-delete-project.md) . 






## <a name="next-steps"></a>Sonraki adımlar
[Azure geçişi 'ne genel bakış](migrate-services-overview.md)konusunu okuyun.

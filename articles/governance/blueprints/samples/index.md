---
title: Blueprint örnekleri dizini
description: Azure Blueprints ile dağıtıma yönelik uyumluluk ve standart ortam örnekleri dizini.
author: DCtheGeek
manager: carmonm
ms.service: blueprints
ms.topic: sample
ms.date: 08/21/2019
ms.author: dacoulte
ms.custom: fasttrack-edit
ms.openlocfilehash: fc05e8c7c1536544d9ac7c2a45f66da8316fe088
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70076928"
---
# <a name="azure-blueprints-samples"></a>Azure Blueprints örnekleri

Aşağıdaki tabloda Azure Blueprints'e ait örneklerin bağlantıları yer alır. Her örnek üretim kalitesindedir ve çeşitli uyumluluk gereksinimlerinizi karşılamanızda size yardımcı olması için hemen dağıtıma hazır olarak sunulur.

## <a name="standards-based-blueprint-samples"></a>Standartlara dayalı şema örnekleri

|  |  |
|---------|---------|
| [Kanada Federal PBMM](./canada-federal-pbmm/control-mapping.md) | Kanada Federal Tarafından Korunan B, Orta Düzeyde Bütünlük, Orta Düzeyde Kullanılabilirlik (PBMM) için uyumluluğa yönelik koruma sağlar. |
| [CIS Microsoft Azure Foundations Benchmark](./cis-azure-1.1.0/index.md)| CIS Microsoft Azure Foundations Benchmark önerileriyle uyumlu olmaya yardımcı olan bir dizi ilke sağlar. |
| [IRS 1075](./irs-1075/index.md) | IRS 1075 ile uyumluluğa yönelik koruma sağlar.|
| [ISO 27001](./iso27001/index.md) | ISO 27001 ile uyumluluğa yönelik koruma sağlar. |
| [ISO 27001 Paylaşılan Hizmetler](./iso27001-shared/index.md) | ISO 27001 onayı almaya yardımcı olacak bir dizi uyumlu altyapı deseni ve ilke koruması sağlar. |
| [ISO 27001 App Service Ortamı/SQL Veritabanı iş yükü](./iso27001-ase-sql-workload/index.md) | [ISO 27001 Paylaşılan Hizmetler](./iso27001-shared/index.md) şema örneğine ek altyapı sağlar. |
| [NIST SP 800-53 R4](./nist-sp-800-53-rev4/index.md) | NIST SP 800-53 R4 ile uyumluluğa yönelik koruma sağlar. |
| [PCI-DSS v3.2.1](./pci-dss-3.2.1/index.md) | PCI-DSS v3.2.1 uyumluluğunu desteklemeye yönelik bir ilke kümesi sağlar. |
| [UK OFFICIAL ve UK NHS İdaresi](./ukofficial/index.md) | UK OFFICIAL ve UK NHS onayı almaya yardımcı olacak bir dizi uyumlu altyapı deseni ve ilke koruması sağlar. |
| [CAF Foundation](./caf-foundation/index.md) | Bulut varlığınızı [Azure için Microsoft Bulutu Benimseme Çerçevesi (CAF)](/azure/architecture/cloud-adoption/governance/journeys/index) ile uyumlu şekilde yönetmenize yardımcı olacak bir denetim kümesi sağlar. |
| [CAF Geçişi giriş bölgesi](./caf-migrate-landing-zone/index.md) | İlk iş yükünüzü geçirmek üzere ayarlamanıza ve bulut varlığınızı [Azure için Microsoft Bulutu Benimseme Çerçevesi (CAF)](/azure/architecture/cloud-adoption/migrate/index) ile uyumlu şekilde yönetmenize yardımcı olacak bir denetim kümesi sağlar. |

## <a name="samples-strategy"></a>Örnek Stratejisi

![Şema örnekleri stratejisi](../media/blueprint-samples-strategy.png)

CAF Foundation ve CAF Geçişi giriş bölgesi şemalarında, müşterinin şirket içi varlıkları/iş yüklerini Azure’a geçirmek amacıyla mevcut bir temiz tek abonelik hazırladığı varsayılır.
(Yukarıdaki Şekilde A ve B Bölgesi).  

Örnek şemaları yineleme ve müşterinin uyguladığı özelleştirme desenlerini arama fırsatından yararlanabilirsiniz. Ayrıca, finansal hizmetler ve e-ticaret gibi sektöre özgü şemaları proaktif olarak ele alma fırsatınız da vardır (B Bölgesinin üst ucu). Benzer şekilde, çoklu abonelikler, yüksek kullanılabilirlik, bölgeler arası kullanılan kaynaklar ile mevcut abonelikler ve kaynaklar üzerinde denetim uygulayan müşteriler gibi karmaşık mimari değerlendirme noktalarına yönelik şemaları oluşturmayı düşünüyoruz (C ve D Bölgesi).

Uyumluluk gereksinimlerinin ve mimari karmaşıklığın yüksek olduğu (yukarıdaki şekilde E Bölgesi) müşteri senaryosunu ele alan örnek şemalar vardır. Yukarıdaki F bölgesi, örnek şemalardan yararlanan ve bunları benzersiz ihtiyaçlarına yönelik olarak özelleştiren müşteriler ve iş ortakları tarafından ele alınacak bölgelerden biridir.

## <a name="next-steps"></a>Sonraki adımlar

- [Şema yaşam döngüsü](../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.
---
title: Şema örnekleri dizini
description: Azure Blueprints ile ortamlar, ilkeler ve Bulut Benimseme Çerçevesi temelleri dağıtmak için uyumluluk ve standart örnek dizini.
ms.date: 01/27/2021
ms.topic: sample
ms.openlocfilehash: 7865a3ea5be374f36de70e62c7bc215fc88d04a5
ms.sourcegitcommit: 436518116963bd7e81e0217e246c80a9808dc88c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/27/2021
ms.locfileid: "98915701"
---
# <a name="azure-blueprints-samples"></a>Azure Blueprints örnekleri

Aşağıdaki tabloda Azure Blueprints'e ait örneklerin bağlantıları yer alır. Her örnek üretim kalitesindedir ve çeşitli uyumluluk gereksinimlerinizi karşılamanızda size yardımcı olması için hemen dağıtıma hazır olarak sunulur.

## <a name="standards-based-blueprint-samples"></a>Standartlara dayalı şema örnekleri

| Örnek | Açıklama |
|---------|---------|
| [ISM İLE KORUNAN Avustralya Kamu](./ism-protected/index.md) | Avustralya kamu IZININ KORUNMASıNDAKI uyumluluk için guardrayları sağlar. |
| [Azure Güvenlik kıyaslaması v1](./azure-security-benchmark.md) | [Azure Güvenlik kıyaslaması v1](../../../security/benchmarks/overview.md)ile uyumluluk için guardrayları sağlar. |
| [Kanada Federal PBMM](./canada-federal-pbmm/index.md) | Kanada Federal korumalı B, Orta bütünlük, orta kullanılabilirlik (PBMM) ile uyumluluk için guardrayları sağlar. |
| [CIS Microsoft Azure Foundations Benchmark](./cis-azure-1-1-0.md)| CIS Microsoft Azure Foundations Benchmark önerileriyle uyumlu olmaya yardımcı olan bir dizi ilke sağlar. |
| [DoD Etki Düzeyi 4](./dod-impact-level-4/index.md) | DoD Etki Düzeyi 4 ile uyumluluk sağlamaya yardımcı olmak için bir dizi ilke sağlar. |
| [DoD Etki Düzeyi 5](./dod-impact-level-5/index.md) | DoD Etki Düzeyi 5 ile uyumluluk sağlamaya yardımcı olmak için bir dizi ilke sağlar. |
| [FedRAMP Moderate](./fedramp-m/index.md) | FedRAMP Moderate ile uyumluluk sağlamaya yardımcı olmak için bir dizi ilke sağlar. |
| [FedRAMP High](./fedramp-h/index.md) | FedRAMP High ile uyumluluk sağlamaya yardımcı olmak için bir dizi ilke sağlar. |
| [HIPAA HITRUST 9.2](./hipaa-hitrust-9-2.md) | HIPAA HITRUST ile uyumluluk sağlamaya yardımcı olmak için bir dizi ilke sağlar. |
| [IRS 1075](./irs-1075/index.md) | IRS 1075 ile uyumluluk için guardrayları sağlar.|
| [ISO 27001](./iso27001/index.md) | ISO 27001 ile uyumluluğa yönelik koruma sağlar. |
| [ISO 27001 Paylaşılan Hizmetler](./iso27001-shared/index.md) | ISO 27001 onayı almaya yardımcı olacak bir dizi uyumlu altyapı deseni ve ilke koruması sağlar. |
| [ISO 27001 App Service Ortamı/SQL Veritabanı iş yükü](./iso27001-ase-sql-workload/index.md) | [ISO 27001 paylaşılan hizmetleri](./iso27001-shared/index.md) şema örneğine daha fazla altyapı sağlar. |
| [Medya](./media/index.md) | Medya MPAA ile uyumluluk sağlamaya yardımcı olmak için bir dizi ilke sağlar. |
| [NIST SP 800-53 R4](./nist-sp-800-53-r4.md) | NıST SP 800-53 R4 uyumluluk için guardrayları sağlar. |
| [NIST SP 800-171 R2](./nist-sp-800-171-r2.md) | NıST SP 800-171 R2 ile uyumluluk için guardrayları sağlar. |
| [PCI-DSS v3.2.1](./pci-dss-3.2.1/index.md) | PCI-DSS v3.2.1 uyumluluğunu desteklemeye yönelik bir ilke kümesi sağlar. |
| [SWIFT CSP-CSCF v2020](./swift-2020/index.md) | SWIFT CSP-CSCF v2020 uyumluluğunu destekler. |
| [UK OFFICIAL ve UK NHS İdaresi](./ukofficial/index.md) | UK OFFICIAL ve UK NHS onayı almaya yardımcı olacak bir dizi uyumlu altyapı deseni ve ilke koruması sağlar. |
| [CAF Foundation](./caf-foundation/index.md) | Bulut varlığınızı [Azure için Microsoft Bulutu Benimseme Çerçevesi (CAF)](/azure/architecture/cloud-adoption/governance/journeys/index) ile uyumlu şekilde yönetmenize yardımcı olacak bir denetim kümesi sağlar. |
| [CAF Geçişi giriş bölgesi](./caf-migrate-landing-zone/index.md) | İlk iş yükünüzü geçirmek üzere ayarlamanıza ve bulut varlığınızı [Azure için Microsoft Bulutu Benimseme Çerçevesi (CAF)](/azure/architecture/cloud-adoption/migrate/index) ile uyumlu şekilde yönetmenize yardımcı olacak bir denetim kümesi sağlar. |

## <a name="samples-strategy"></a>Örnek stratejisi

:::image type="complex" source="../media/blueprint-samples-strategy.png" alt-text="Mimari karmaşıklık ile uyumluluk gereksinimleri karşılaştırmasında Şema örneklerinin yerini gösteren diyagram." border="false":::
   Mimari karmaşıklığın X ekseninde, uyumluluk gereksinimlerinin Y ekseninde olduğu bir koordinat sistemini tanımlar.  Mimari karmaşıklık ve uyumluluk gereksinimleri artdıkça, E bölgesinde belirtilen portaldan standart şema örneklerini benimseyin.  Azure ile çalışmaya başlama müşterileri için, A ve B bölgesi tarafından belirlenen bulut benimseme çerçevesi (C A F) tabanlı bir temel ve giriş bölgesi şemaları kullanın.  Kalan alan, müşteriler tarafından oluşturulan özel şemalara yöneliktir C, D ve F bölgeleri için iş ortakları. :::image-end:::

CAF Foundation ve CAF Geçişi giriş bölgesi şemalarında, müşterinin şirket içi varlıkları ve iş yüklerini Azure’a geçirmek amacıyla mevcut bir temiz tek abonelik hazırladığı varsayılır.
(Şekilde A ve B bölgesi).  

Örnek şemaları yineleme ve müşterinin uyguladığı özelleştirme desenlerini arama fırsatından yararlanabilirsiniz. Ayrıca, finansal hizmetler ve e-ticaret gibi sektöre özgü şemaları proaktif olarak ele alma fırsatınız da vardır (B Bölgesinin üst ucu). Benzer şekilde, çoklu abonelikler, yüksek kullanılabilirlik, bölgeler arası kullanılan kaynaklar ile mevcut abonelikler ve kaynaklar üzerinde denetim uygulayan müşteriler gibi karmaşık mimari değerlendirme noktalarına yönelik şemaları oluşturmayı düşünüyoruz (C ve D Bölgesi).

Uyumluluk gereksinimlerinin ve mimari karmaşıklığın yüksek olduğu (şekilde E Bölgesi) müşteri senaryosunu ele alan örnek şemalar vardır. Şekildeki bölge, örnek şemaları uygulayan ve her birini benzersiz ihtiyaçlarına göre özelleştiren müşteriler ve iş ortakları tarafından giderilen bir kişidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Şema yaşam döngüsü](../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../how-to/update-existing-assignments.md) öğrenin.
- [Genel sorun giderme](../troubleshoot/general.md) adımlarıyla şema atama sorunlarını giderin.

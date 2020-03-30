---
title: Azure Güvenlik Merkezi SSS - mevcut MMA'lar hakkında sorular
description: Bu SSS, Microsoft İzleme Aracısını kullanan ve tehditleri önlemenize, algılamanıza ve bunlara yanıt vermenize yardımcı olan bir ürün olan Azure Güvenlik Merkezi'ni göz önünde bulundurarak müşteriler e-sorularını yanıtlar.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: 5c433140c3982813e372fd3f63243a96197d220c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77661898"
---
# <a name="faq-for-customers-already-using-azure-monitor-logs"></a>Azure Monitor günlüklerini kullanan müşteriler için SSS<a name="existingloganalyticscust"></a>

## <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Güvenlik Merkezi, VM'ler ve çalışma alanları arasındaki varolan bağlantıları geçersiz kılar mı?

Bir VM'de Azure uzantısı olarak Microsoft İzleme Aracısı zaten yüklüyse, Güvenlik Merkezi varolan çalışma alanı bağlantısını geçersiz kılmaz. Bunun yerine, Güvenlik Merkezi varolan çalışma alanını kullanır. VM, "Güvenlik" veya "SecurityCenterFree" çözümlerinin bildirdiği çalışma alanına yüklenmesi koşuluyla korunur. 

Veri Toplama ekranında seçilen çalışma alanına bir Güvenlik Merkezi çözümü yüklenir ve çözüm yalnızca ilgili Sanal Taşıtlara uygulanır. Bir çözüm eklediğinizde, bu çözüm varsayılan olarak Log Analytics çalışma alanınıza bağlı tüm Windows ve Linux aracıları için otomatik olarak dağıtılır. [Çözüm Hedefleme,](../operations-management-suite/operations-management-suite-solution-targeting.md) çözümlerinize bir kapsam uygulamanızı sağlar.

> [!TIP]
> Microsoft İzleme Aracısı doğrudan VM'ye yüklenmişse (Azure uzantısı olarak değil), Güvenlik Merkezi Microsoft İzleme Aracısı'nı yüklemez ve güvenlik izleme sınırlıdır.

## <a name="does-security-center-install-solutions-on-my-existing-log-analytics-workspaces-what-are-the-billing-implications"></a>Security Center, varolan Log Analytics çalışma alanlarıma çözümler yüklüyor mu? Faturalandırmanın sonuçları nelerdir?
Güvenlik Merkezi, bir VM'nin oluşturduğunuz bir çalışma alanına zaten bağlı olduğunu belirlediğinde, Güvenlik Merkezi bu çalışma alanında fiyatlandırma katmanınıza göre çözümler sağlar. Çözümler yalnızca ilgili Azure VM'lerine [çözüm hedeflemesi](../operations-management-suite/operations-management-suite-solution-targeting.md)yoluyla uygulanır, böylece faturalandırma aynı kalır.

- **Ücretsiz katman** – Security Center çalışma alanına 'SecurityCenterFree' çözümlerini yükler. Ücretsiz katman için faturalandırılmazsınız.
- **Standart katman** – Security Center çalışma alanına 'Güvenlik' çözümlerini yükler.

   ![Varsayılan çalışma alanında çözümler](./media/security-center-platform-migration-faq/solutions.png)

## <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>Çevremde zaten çalışma alanları var, bunları güvenlik verilerini toplamak için kullanabilir miyim?
Bir VM'de Azure uzantısı olarak Microsoft İzleme Aracısı zaten yüklüyse, Güvenlik Merkezi varolan bağlı çalışma alanını kullanır. Zaten mevcut değilse çalışma alanına bir Güvenlik Merkezi çözümü yüklenir ve [çözüm yalnızca çözüm hedeflemesi](../operations-management-suite/operations-management-suite-solution-targeting.md)yoluyla ilgili VM'lere uygulanır.

Security Center, Microsoft İzleme Aracısını VM'lere yüklediğinde, Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanını kullanır.

## <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Çalışma alanlarımda zaten güvenlik çözümü var. Faturalandırmanın sonuçları nelerdir?
Güvenlik & Denetimi çözümü, Azure VM'leri için Güvenlik Merkezi standart katman özelliklerini etkinleştirmek için kullanılır. Güvenlik & Denetim çözümü zaten bir çalışma alanına yüklenmişse, Güvenlik Merkezi varolan çözümü kullanır. Faturalandırmada değişiklik yok.
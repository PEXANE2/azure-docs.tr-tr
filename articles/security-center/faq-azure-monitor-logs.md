---
title: Azure Güvenlik Merkezi hakkında SSS-mevcut Log Analytics aracılarıyla ilgili sorular
description: Bu SSS, Log Analytics aracısını kullanan müşterilere yönelik soruları yanıtlar ve tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olan Azure Güvenlik Merkezi 'nin bir ürünüdür.
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
ms.openlocfilehash: f6384c1e9e14e38b4c44c5ac79a674839b43b4ca
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80436161"
---
# <a name="faq-for-customers-already-using-azure-monitor-logs"></a>Zaten Azure Izleyici günlüklerini kullanan müşteriler için SSS<a name="existingloganalyticscust"></a>

## <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Güvenlik Merkezi, VM 'Ler ve çalışma alanları arasındaki mevcut bağlantıları geçersiz kılar mi?

Bir VM 'de zaten bir Azure uzantısı olarak yüklü Log Analytics Aracısı varsa, güvenlik merkezi mevcut çalışma alanı bağlantısını geçersiz kılmaz. Bunun yerine, güvenlik merkezi mevcut çalışma alanını kullanır. "Güvenlik" veya "SecurityCenterFree" çözümünün, raporlama yaptığı çalışma alanına yüklenmiş olması için VM korunacaktır. 

Zaten mevcut değilse, veri toplama ekranında seçilen çalışma alanına bir güvenlik merkezi çözümü yüklenir ve bu çözüm yalnızca ilgili VM 'lere uygulanır. Bir çözüm eklediğinizde, varsayılan olarak, Log Analytics çalışma alanınıza bağlı tüm Windows ve Linux aracılarına otomatik olarak dağıtılır. [Çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md) , çözümlerinizi bir kapsam uygulamanıza olanak tanır.

> [!TIP]
> Log Analytics Aracısı doğrudan VM 'ye (Azure uzantısı olarak değil) yüklenirse, güvenlik merkezi Log Analytics Aracısı 'nı yüklemez ve güvenlik izleme sınırlı olur.

## <a name="does-security-center-install-solutions-on-my-existing-log-analytics-workspaces-what-are-the-billing-implications"></a>Güvenlik Merkezi, mevcut Log Analytics çalışma alanlarıma çözüm yüklesin mi? Faturalandırma etkileri nelerdir?
Güvenlik Merkezi, bir sanal makinenin oluşturduğunuz bir çalışma alanına zaten bağlı olduğunu belirlediğinde, Güvenlik Merkezi, fiyatlandırma katmanınıza göre bu çalışma alanındaki çözümleri sağlar. Çözümler, [çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md)aracılığıyla yalnızca Ilgili Azure sanal makinelerine uygulanır, böylece faturalandırma aynı kalır.

- **Ücretsiz katman** : Güvenlik Merkezi, çalışma alanına ' SecurityCenterFree ' çözümünü yüklüyor. Ücretsiz katman için faturalandırılmaz.
- **Standart katman** : Güvenlik Merkezi, çalışma alanına ' Güvenlik ' çözümünü yüklüyor.

   ![Varsayılan çalışma alanındaki çözümler](./media/security-center-platform-migration-faq/solutions.png)

## <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>Ortammda çalışma alanım zaten var, bunları güvenlik verilerini toplamak için kullanabilir miyim?
Bir VM 'de zaten bir Azure uzantısı olarak yüklü Log Analytics Aracısı varsa, güvenlik merkezi var olan bağlı çalışma alanını kullanır. Zaten mevcut değilse, bir güvenlik merkezi çözümü çalışma alanına yüklenir ve çözüm [hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md)aracılığıyla yalnızca ilgili sanal makinelere uygulanır.

Güvenlik Merkezi, VM 'Lere Log Analytics aracısını yüklediğinde, Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanlarını kullanır.

## <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Çalışma alanlarım üzerinde zaten güvenlik çözümüdür. Faturalandırma etkileri nelerdir?
Güvenlik & denetim çözümü, Azure VM 'Leri için Güvenlik Merkezi Standart katmanı özelliklerini etkinleştirmek üzere kullanılır. Güvenlik & denetim çözümü bir çalışma alanında zaten yüklüyse, güvenlik merkezi mevcut çözümü kullanır. Faturalandırma sırasında değişiklik yapılmaz.
---
title: Azure Güvenlik Merkezi hakkında SSS-mevcut MMAs hakkında sorular
description: Bu SSS, Microsoft Monitoring Agent kullanan müşterilerle ilgili soruları yanıtlar ve tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olan Azure Güvenlik Merkezi 'nin bir ürünüdür.
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
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77661898"
---
# Zaten Azure Izleyici günlüklerini kullanan müşteriler için SSS<a name="existingloganalyticscust"></a>

## <a name="does-security-center-override-any-existing-connections-between-vms-and-workspaces"></a>Güvenlik Merkezi, Vm'leri ve çalışma alanları arasında herhangi bir mevcut bağlantı geçersiz kılmaz?

Güvenlik Merkezi bir VM zaten Microsoft Monitoring Agent yüklüyse Azure uzantı olarak varsa, varolan bir çalışma alanı bağlantıyı geçersiz kılmaz. Bunun yerine, Güvenlik Merkezi, varolan bir çalışma alanını kullanır. "Güvenlik" veya "SecurityCenterFree" çözümünün, raporlama yaptığı çalışma alanına yüklenmiş olması için VM korunacaktır. 

Zaten mevcut değilse, veri toplama ekranında seçilen çalışma alanına bir güvenlik merkezi çözümü yüklenir ve bu çözüm yalnızca ilgili VM 'lere uygulanır. Bir çözümü eklediğinizde, Log Analytics çalışma alanınıza bağlı tüm Windows ve Linux aracıları için varsayılan olarak otomatik olarak dağıtılır. [Çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md) , çözümlerinizi bir kapsam uygulamanıza olanak tanır.

> [!TIP]
> Microsoft Monitoring Agent doğrudan VM 'ye (Azure uzantısı olarak değil) yüklenirse, Güvenlik Merkezi Microsoft Monitoring Agent yüklemez ve güvenlik izleme sınırlı olur.

## <a name="does-security-center-install-solutions-on-my-existing-log-analytics-workspaces-what-are-the-billing-implications"></a>Güvenlik Merkezi, my mevcut Log Analytics çalışma alanları çözümleri yüklüyor mu? Fatura etkileri nelerdir?
Güvenlik Merkezi, Güvenlik Merkezi bir VM zaten oluşturduğunuz bir çalışma alanına bağlı belirlediğinde bu çalışma alanındaki fiyatlandırma katmanınızı göre çözümler sağlar. Çözümler, [çözüm hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md)aracılığıyla yalnızca Ilgili Azure sanal makinelerine uygulanır, böylece faturalandırma aynı kalır.

- **Ücretsiz katman** : Güvenlik Merkezi, çalışma alanına ' SecurityCenterFree ' çözümünü yüklüyor. Ücretsiz katman için faturalandırılmaz.
- **Standart katman** : Güvenlik Merkezi, çalışma alanına ' Güvenlik ' çözümünü yüklüyor.

   ![Varsayılan çalışma alanı çözümleri](./media/security-center-platform-migration-faq/solutions.png)

## <a name="i-already-have-workspaces-in-my-environment-can-i-use-them-to-collect-security-data"></a>Çalışma Alanım ortamda zaten var, bunların güvenlik verilerini toplamak için kullanabilir miyim?
Güvenlik Merkezi, bir VM'nin Microsoft Monitoring Agent yüklüyse Azure uzantı olarak zaten varsa, mevcut bağlı çalışma kullanır. Zaten mevcut değilse, bir güvenlik merkezi çözümü çalışma alanına yüklenir ve çözüm [hedefleme](../operations-management-suite/operations-management-suite-solution-targeting.md)aracılığıyla yalnızca ilgili sanal makinelere uygulanır.

Güvenlik Merkezi, Microsoft Monitoring Agent, Vm'lerde yüklendiğinde, Güvenlik Merkezi tarafından oluşturulan varsayılan çalışma alanlarını kullanır.

## <a name="i-already-have-security-solution-on-my-workspaces-what-are-the-billing-implications"></a>Zaten bir güvenlik çözümü üzerinde çalışma alanlarım var. Fatura etkileri nelerdir?
Güvenlik & denetim çözümü, Azure VM 'Leri için Güvenlik Merkezi Standart katmanı özelliklerini etkinleştirmek üzere kullanılır. Güvenlik Merkezi, güvenlik ve denetim çözümü, bir çalışma alanında zaten yüklüyse, varolan bir çözümü kullanır. Faturalama değişiklik yoktur.
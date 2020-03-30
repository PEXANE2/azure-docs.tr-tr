---
title: Azure İlkeleri ile Tümleştirme - Azure Toplu İşi | Microsoft Dokümanlar
description: ''
services: batch
documentationcenter: ''
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 28998df4-8693-431d-b6ad-974c2f8db5fb
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 02/24/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 9a306457f838fc79d12be3217d96cc8fb25c9c1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77618379"
---
# <a name="integration-with-azure-policy"></a>Azure İlkesiyle Tümleştirme

Azure İlkesi, Azure'da, bu kaynakların şirket standartlarınızla ve hizmet düzeyi sözleşmelerinizle uyumlu kalmasını sağlamak için kaynaklarınız üzerinde kurallar uygulayan ilkeler oluşturmak, atamak ve yönetmek için kullandığınız bir hizmettir. Azure İlkesi, kaynaklarınızı atadığınız ilkelere uymadığınıza göre değerlendirir. 

Azure Toplu İş'in, ilke uyumluluğunu yönetmenize yardımcı olacak iki yerleşik uzantısı vardır. 

|**Adı**...|   **Açıklama**|**Etki(ler)**|  **Sürüm**|    **Kaynak**
|----------------|----------|----------|----------------|---------------|
|Toplu iş hesaplarındaki tanılama günlükleri etkinleştirilmeli|   Tanılama günlüklerinin denetim için. Bu, araştırma amacıyla kullanmak üzere etkinlik izlerini yeniden oluşturmanıza olanak tanır; bir güvenlik olayı meydana geldiğinde veya ağınızın gizliliği ihlal edildiğinde|Auditifnotexists, Devre Dışı|  2.0.0|  GitHub|
|Metrik uyarı kuralları Toplu Iş hesaplarında yapılandırılmalıdır| Gerekli metrik etkinleştirmek için Toplu hesaptaki metrik uyarı kurallarının denetim yapılandırması|   Auditifnotexists, Devre Dışı| 1.0.0|  GitHub|

İlke tanımları karşılanması gereken koşulları açıklar. Bir koşul, kaynak özelliğini gerekli bir değerle karşılaştırır. Kaynak özelliği alanlarına önceden tanımlanmış diğer adlar kullanılarak erişilir. Kaynak türüiçin belirli özelliklere erişmek için özellik takma adlarını kullanırsınız. Diğer adlar, kaynaktaki bir özellik için hangi değerlerin veya koşulların izin verildiğini kısıtlamanızı sağlar. Her diğer ad, belirli bir kaynak türü için farklı API sürümlerindeki yollara eş ler. İlke değerlendirmesi sırasında, ilke altyapısı bu API sürümü için özellik yolunu alır.

Toplu İşlem tarafından gerekli kaynaklar şunlardır: hesap, işlem düğümü, havuz, iş ve görev. Bu nedenle, bu kaynaklar için belirli özelliklere erişmek için özellik takma adlarını kullanırsınız. [Diğer Adlar](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases) hakkında daha fazla bilgi edinin

Geçerli diğer adları bildiğinizden ve kaynaklarınızı ve ilkelerinizi gözden geçirdiğinizden emin olmak için Visual Studio Code için Azure ilke uzantısını kullanın. Visual Studio Code tarafından desteklenen tüm platformlarda kurulabilir. Bu destek Windows, Linux ve macOS'u içerir. [Yükleme yönergelerine](https://docs.microsoft.com/azure/governance/policy/how-to/extension-for-vscode)bakın.




---
title: Azure Ilkeleriyle tümleştirme-Azure Batch | Microsoft Docs
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
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77618379"
---
# <a name="integration-with-azure-policy"></a>Azure Ilkesiyle tümleştirme

Azure Ilkesi, kaynakların kurumsal standartlarınızla ve hizmet düzeyi sözleşmelerle uyumlu kalmasını sağlamak için kaynaklarınız üzerinde kurallar uygulayan ilkeler oluşturmak, atamak ve yönetmek için kullandığınız bir hizmettir. Azure Ilkesi, atadığınız ilkelerle uyumlu olmayan kaynaklarınızı hesaplar. 

Azure Batch, ilke uyumluluğunu yönetmenize yardımcı olmak için iki yerleşik uzantıya sahiptir. 

|**Ad**...|   **Açıklama**|**Efekt (ler)**|  **Sürüm**|    **Kaynak**
|----------------|----------|----------|----------------|---------------|
|Batch hesaplarındaki tanılama günlükleri etkinleştirilmelidir|   Tanılama günlüklerini etkinleştirme denetimi. Bu, araştırma amacıyla kullanılacak etkinlik izlerini yeniden oluşturmayı sağlar; bir güvenlik olayı gerçekleştiğinde veya ağınız tehlikede olduğunda|Auditınotexists, devre dışı|  2.0.0|  GitHub|
|Toplu Iş hesaplarında ölçüm uyarısı kuralları yapılandırılmalıdır| Gerekli ölçümü etkinleştirmek için Batch hesabındaki ölçüm uyarı kurallarının yapılandırmasını denetleme|   Auditınotexists, devre dışı| 1.0.0|  GitHub|

İlke tanımları, karşılanması gereken koşulları tanımlar. Bir koşul, kaynak özelliğini gerekli bir değerle karşılaştırır. Kaynak özelliği alanlarına önceden tanımlanmış diğer adlar kullanılarak erişilir. Bir kaynak türü için belirli özelliklere erişmek üzere özellik diğer adlarını kullanırsınız. Diğer adlar, kaynak üzerinde bir özellik için hangi değerleri veya koşullara izin sınırlamak sağlar. Belirtilen kaynak türü için farklı bir API sürümlerinde yolları her diğer adın eşlenir. İlke değerlendirmesi sırasında ilke altyapısı bu API sürümü için özellik yolu alır.

Batch tarafından gereken kaynaklar şunlardır: hesap, işlem düğümü, havuz, iş ve görev. Bu nedenle, bu kaynakların belirli özelliklerine erişmek için özellik diğer adlarını kullanırsınız. [Diğer adlar](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases) hakkında daha fazla bilgi

Geçerli diğer adları bildiğiniz ve kaynaklarınızı ve ilkelerinizi gözden geçirdiğinizden emin olmak için Visual Studio Code için Azure ilkesi uzantısını kullanın. Bu, Visual Studio Code tarafından desteklenen tüm platformlara yüklenebilir. Bu destek, Windows, Linux ve macOS içerir. Bkz. [yükleme yönergeleri](https://docs.microsoft.com/azure/governance/policy/how-to/extension-for-vscode).




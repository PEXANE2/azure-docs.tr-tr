---
title: Azure Ilkeleriyle tümleştirme
description: Azure Ilkesi, kaynaklarınız üzerinde kurallar uygulayan ilkeler oluşturmak, atamak ve yönetmek için kullandığınız bir Azure hizmetidir.
ms.topic: article
ms.date: 02/24/2020
ms.custom: seodec18
ms.openlocfilehash: 74c087031a024b0c50d6c40057a4e2eda554be6f
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82116086"
---
# <a name="integration-with-azure-policy"></a>Azure Ilkesiyle tümleştirme

Azure Ilkesi, kaynakların kurumsal standartlarınızla ve hizmet düzeyi sözleşmelerle uyumlu kalmasını sağlamak için kaynaklarınız üzerinde kurallar uygulayan ilkeler oluşturmak, atamak ve yönetmek için kullandığınız bir hizmettir. Azure Ilkesi, atadığınız ilkelerle uyumlu olmayan kaynaklarınızı hesaplar. 

Azure Batch, ilke uyumluluğunu yönetmenize yardımcı olmak için iki yerleşik uzantıya sahiptir. 

|**Ad**...|   **Açıklama**|**Efekt (ler)**|  **Sürüm**|    **Kaynak**
|----------------|----------|----------|----------------|---------------|
|Batch hesaplarındaki tanılama günlükleri etkinleştirilmelidir|   Tanılama günlüklerini etkinleştirme denetimi. Bu, araştırma amacıyla kullanılacak etkinlik izlerini yeniden oluşturmayı sağlar; bir güvenlik olayı gerçekleştiğinde veya ağınız tehlikede olduğunda|Auditınotexists, devre dışı|  2.0.0|  GitHub|
|Toplu Iş hesaplarında ölçüm uyarısı kuralları yapılandırılmalıdır| Gerekli ölçümü etkinleştirmek için Batch hesabındaki ölçüm uyarı kurallarının yapılandırmasını denetleme|   Auditınotexists, devre dışı| 1.0.0|  GitHub|

İlke tanımları, karşılanması gereken koşulları tanımlar. Bir koşul, kaynak özelliğini gerekli bir değerle karşılaştırır. Kaynak özelliği alanlarına önceden tanımlanmış diğer adlar kullanılarak erişilir. Bir kaynak türü için belirli özelliklere erişmek üzere özellik diğer adlarını kullanırsınız. Diğer adlar, bir kaynaktaki bir özellik için izin verilen değerleri veya koşulları kısıtlamanıza olanak sağlar. Her diğer ad, belirli bir kaynak türü için farklı API sürümlerindeki yollarla eşlenir. İlkesi değerlendirmesi sırasında, ilke altyapısı bu API sürümünün özellik yolunu alır.

Batch tarafından gereken kaynaklar şunlardır: hesap, işlem düğümü, havuz, iş ve görev. Bu nedenle, bu kaynakların belirli özelliklerine erişmek için özellik diğer adlarını kullanırsınız. [Diğer adlar](https://docs.microsoft.com/azure/governance/policy/concepts/definition-structure#aliases) hakkında daha fazla bilgi

Geçerli diğer adları bildiğiniz ve kaynaklarınızı ve ilkelerinizi gözden geçirdiğinizden emin olmak için Visual Studio Code için Azure ilkesi uzantısını kullanın. Bu, Visual Studio Code tarafından desteklenen tüm platformlara yüklenebilir. Bu destek, Windows, Linux ve macOS içerir. Bkz. [yükleme yönergeleri](https://docs.microsoft.com/azure/governance/policy/how-to/extension-for-vscode).




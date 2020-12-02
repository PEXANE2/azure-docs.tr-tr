---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 12/01/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: c2385d07ccb81041bd340a8bec0412a8f14cef56
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96476832"
---
## <a name="azure-security-benchmark"></a>Azure Güvenlik Karşılaştırması

[Azure Güvenlik kıyaslaması](../../../../articles/security/benchmarks/overview.md) , bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar. Bu hizmetin Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için bkz. [Azure Güvenlik kıyaslaması eşleme dosyaları](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

Tüm Azure hizmetleri için kullanılabilen Azure Ilkesi 'nin Bu uyumluluk standardına nasıl eşlendiğini gözden geçirmek için bkz. [Azure Ilke mevzuatı uyumluluğu-Azure Güvenlik kıyaslaması](../../../../articles/governance/policy/samples/azure-security-benchmark.md).

|Etki alanı |Denetim KIMLIĞI |Denetim başlığı |İlke<br /><sub>(Azure portal)</sub> |İlke sürümü<br /><sub>GitHub</sub>  |
|---|---|---|---|---|
|Veri Koruma |4.8 |Bekleyen gizli bilgileri şifreleyin |[Otomasyon hesabı değişkenleri şifrelenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3657f5a0-770e-44a3-b44e-9431ba1e9735) |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Automation/Automation_AuditUnencryptedVars_Audit.json) |

> [!NOTE]
> Bir Otomasyon hesabı değişkeni oluşturduğunuzda, Azure Otomasyonu tarafından güvenli bir varlık olarak şifrelemeyi ve depolamayı belirtebilirsiniz. Değişkeni oluşturduktan sonra, değişkeni yeniden oluşturmadan şifreleme durumunu değiştiremezsiniz. Önceden şifrelenmeyen hassas verileri depolayan Otomasyon hesabı değişkenleri varsa, bunları silmeniz ve şifrelenmiş değişkenler olarak yeniden oluşturmanız gerekir. Azure Güvenlik Merkezi 'nin önerisi, [Otomasyon hesabı değişkenlerinde](../../../../articles/security-center/recommendations-reference.md#recs-computeapp)açıklandığı şekilde tüm Azure Otomasyonu değişkenlerini şifreleyebilmelidir. Bu güvenlik önerilerinden hariç tutmak istediğiniz şifrelenmemiş değişkenlere sahipseniz, bkz. muafiyet kuralı oluşturmak için [öneriler ve güvenli puandan bir kaynağı muaf](../../../../articles/security-center/exempt-resource.md) tutma.
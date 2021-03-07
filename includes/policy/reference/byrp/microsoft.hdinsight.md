---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 03/05/2021
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 6de81dac36c5841efbc522318e6e5ce98edf5d0d
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102434274"
---
|Name<br /><sub>(Azure portal)</sub> |Açıklama |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure HDInsight kümeleri, bekleyen verileri şifrelemek için müşteri tarafından yönetilen anahtarları kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F64d314f6-6062-4780-a861-c23e8951bee5) |Azure HDInsight kümelerinizin geri kalanında şifrelemeyi yönetmek için müşteri tarafından yönetilen anahtarları kullanın. Varsayılan olarak, müşteri verileri hizmet tarafından yönetilen anahtarlarla şifrelenir, ancak yasal uyumluluk standartlarını karşılamak için müşteri tarafından yönetilen anahtarlar yaygın olarak gereklidir. Müşteri tarafından yönetilen anahtarlar, verilerin oluşturulmuş ve sizin tarafınızdan sahip olduğu bir Azure Key Vault anahtarla şifrelenmesini sağlar. Anahtar yaşam döngüsü için, döndürme ve yönetim de dahil olmak üzere tam denetim ve sorumluluğa sahipsiniz. Daha fazla bilgi edinin [https://aka.ms/hdi.cmk](https://aka.ms/hdi.cmk) . |Denetim, reddetme, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/HDInsight/HDInsight_CMK_Audit.json) |
|[Azure HDInsight kümeleri, bekleyen verileri şifrelemek için konakta şifrelemeyi kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1fd32ebd-e4c3-4e13-a54a-d7422d4d95f6) |Konakta şifrelemeyi etkinleştirmek, kuruluşunuzun güvenlik ve uyumluluk taahhütlerinizi karşılamak için verilerinizin korunmasını ve korunmasını sağlar. Konakta şifrelemeyi etkinleştirdiğinizde, VM konağında depolanan veriler, REST ve depolama hizmetine şifrelenen akışlara şifrelenir. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/HDInsight/HDInsight_EncryptionAtHost_Audit.json) |
|[Azure HDInsight kümeleri, Azure HDInsight küme düğümleri arasındaki iletişimi şifrelemek için geçişte şifrelemeyi kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd9da03a1-f3c3-412a-9709-947156872263) |Azure HDInsight küme düğümleri arasında iletim sırasında veri üzerinde değişiklik yapılabilir. Yoldaki şifrelemeyi etkinleştirmek, bu iletim sırasında kötüye kullanımı ve değişikliklere karşı sorunları giderir. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/HDInsight/HDInsight_EncryptionInTransit_Audit.json) |

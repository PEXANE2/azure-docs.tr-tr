---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 02/10/2020
ms.author: dacoulte
ms.openlocfilehash: 7aa6abb1a5fa9c969ca5e6d0730bed3b461fe81b
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172887"
---
|Ad |Açıklama |Efekt (ler) |Sürüm |
|---|---|---|---|
|[İzin verilen depolama hesabı SKU'ları](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/AllowedStorageSkus_Deny.json) |Bu ilke, kuruluşunuzun dağıtabileceğiniz bir depolama hesabı SKU 'Ları kümesi belirtmenizi sağlar. |Reddet |1.0.0 |
|[Depolama hesaplarına Kısıtlanmamış ağ erişimini denetleme](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |Depolama hesabı güvenlik duvarı ayarlarınızda Kısıtlanmamış ağ erişimini denetleyin. Bunun yerine, ağ kurallarını yalnızca izin verilen ağların uygulamalarının depolama hesabına erişebilmesi için yapılandırın. Belirli bir internet veya şirket içi istemcilerden gelen bağlantılara izin vermek için, belirli Azure sanal ağlarından veya genel İnternet IP adresi aralıklarına yönelik trafiğe erişim verilebilir |Denetim, devre dışı |1.0.0 |
|[Depolama hesaplarında Gelişmiş tehdit koruması dağıtma](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/StorageAdvancedThreatProtection_Deploy.json) |Bu ilke, depolama hesaplarında Gelişmiş tehdit koruması sunar. |DeployIfNotExists, devre dışı |1.0.0 |
|[Depolama hesapları için coğrafi olarak yedekli depolama etkinleştirilmelidir](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/GeoRedundant_StorageAccounts_Audit.json) |Bu ilke, coğrafi olarak yedekli depolama özellikli tüm depolama hesaplarını denetler. |Denetim, devre dışı |1.0.0 |
|[Depolama hesaplarına Güvenli aktarım etkinleştirilmelidir](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/Storage_AuditForHTTPSEnabled_Audit.json) |Depolama hesabınızda güvenli aktarım gereksinimlerini denetleyin. Güvenli aktarım, depolama hesabınızı yalnızca güvenli bağlantılardan (HTTPS) istekleri kabul edecek şekilde zorlayan bir seçenektir. HTTPS kullanımı, sunucu ile hizmet arasında kimlik doğrulaması sağlar ve geçiş sırasında ortadaki adam, gizlice dinleme ve oturum ele geçirme gibi ağ katmanı saldırılarına karşı verileri korur |Denetim, reddetme, devre dışı |1.0.0 |
|[Depolama hesapları güvenilen Microsoft hizmetlerinden erişime izin verilmelidir](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/StorageAccess_TrustedMicrosoftServices_Audit.json) |Depolama hesapları ile etkileşimde bulunan bazı Microsoft Hizmetleri, ağ kuralları aracılığıyla erişim verilemez ağlardan çalışır. Bu tür bir hizmet iş beklendiği gibi yardımcı olmak için ağ kuralları atlamak için güvenilen Microsoft Hizmetleri kümesi sağlar. Bu hizmetler, daha sonra depolama hesabına erişmek için güçlü kimlik doğrulaması kullanır. |Denetim, reddetme, devre dışı |1.0.0 |
|[Depolama hesaplarının yeni Azure Resource Manager kaynaklarına geçirilmesi gerekir](https://github.com/Azure/azure-policy/blob/masterbuilt-in-policies/policyDefinitions/Storage/Classic_AuditForClassicStorages_Audit.json) |Daha güçlü erişim denetimi (RBAC), daha iyi denetim, Azure Resource Manager tabanlı dağıtım ve idare, yönetilen kimliklere erişim, için anahtar kasasına erişim gibi güvenlik geliştirmeleri sağlamak üzere depolama hesaplarınız için yeni Azure Resource Manager kullanın. daha kolay güvenlik yönetimi için gizlilikler, Azure AD tabanlı kimlik doğrulaması ve Etiketler ve kaynak grupları desteği |Denetim, reddetme, devre dışı |1.0.0 |

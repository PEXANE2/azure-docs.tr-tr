---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 07/22/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: dfbbf6ec79644da77dbccda83a31428c31f4fba9
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89379257"
---
## <a name="azure-security-benchmark"></a>Azure Güvenlik Karşılaştırması

[Azure Güvenlik kıyaslaması](../../../../articles/security/benchmarks/overview.md) , bulut Çözümlerinizi Azure 'da nasıl güvence altına almak için öneriler sağlar. Bu hizmetin Azure Güvenlik kıyaslaması ile tamamen nasıl eşlendiğini görmek için bkz. [Azure Güvenlik kıyaslaması eşleme dosyaları](https://github.com/MicrosoftDocs/SecurityBenchmarks/tree/master/Azure%20Offer%20Security%20Baselines).

Tüm Azure hizmetleri için kullanılabilen Azure Ilkesi 'nin Bu uyumluluk standardına nasıl eşlendiğini gözden geçirmek için bkz. [Azure Ilke mevzuatı uyumluluğu-Azure Güvenlik kıyaslaması](../../../../articles/governance/policy/samples/azure-security-benchmark.md).

|Etki alanı |Denetim KIMLIĞI |Denetim başlığı |İlke<br /><sub>(Azure portal)</sub> |İlke sürümü<br /><sub>GitHub</sub>  |
|---|---|---|---|---|
|Ağ Güvenliği |1.1 |Sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruma |[Depolama hesapları, ağ erişimini kısıtlıyor olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F34c877ad-507e-4c82-993e-3452a6e0ad3c) |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |
|Ağ Güvenliği |1.1 |Sanal ağınızda Ağ güvenlik gruplarını veya Azure Güvenlik duvarını kullanarak kaynakları koruma |[Depolama hesapları bir sanal ağ hizmeti uç noktası kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F60d21c4f-21a3-4d94-85f4-b924e6aeeda4) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/VirtualNetworkServiceEndpoint_StorageAccount_Audit.json) |
|Veri Koruma |4.4 |Yoldaki tüm hassas bilgileri şifreleyin |[Depolama hesaplarına güvenli aktarım etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F404c3081-a854-4457-ae30-26a93ef643f9) |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_AuditForHTTPSEnabled_Audit.json) |
|Envanter ve Varlık Yönetimi |6,9 |Yalnızca onaylanan Azure hizmetlerini kullan |[Depolama hesaplarının yeni Azure Resource Manager kaynaklarına geçirilmesi gerekir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F37e0d2fe-28a5-43d6-a273-67d37d1f5606) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Classic_AuditForClassicStorages_Audit.json) |

## <a name="cis-microsoft-azure-foundations-benchmark"></a>CIS Microsoft Azure Foundations Benchmark

Tüm Azure hizmetleri için kullanılabilir olan Azure Ilkesi 'nin Bu uyumluluk standardına nasıl eşlendiğini gözden geçirmek için bkz. [Azure Ilke mevzuatı uyumluluğu-cıs Microsoft Azure temelleri kıyaslama 1.1.0](../../../../articles/governance/policy/samples/cis-azure-1-1-0.md).
Bu uyumluluk standardı hakkında daha fazla bilgi için bkz. [cıs Microsoft Azure kuruluş kıyaslaması](https://www.cisecurity.org/benchmark/azure/).

|Etki alanı |Denetim KIMLIĞI |Denetim başlığı |İlke<br /><sub>(Azure portal)</sub> |İlke sürümü<br /><sub>GitHub</sub>  |
|---|---|---|---|---|
|Depolama Hesapları |3,1 |' Güvenli aktarım gerekli ' ' Enabled ' olarak ayarlandığından emin olun |[Depolama hesaplarına güvenli aktarım etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F404c3081-a854-4457-ae30-26a93ef643f9) |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_AuditForHTTPSEnabled_Audit.json) |
|Depolama Hesapları |3.7 |Depolama hesapları için varsayılan ağ erişim kuralının Reddet olarak ayarlandığından emin olun |[Depolama hesapları, ağ erişimini kısıtlıyor olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F34c877ad-507e-4c82-993e-3452a6e0ad3c) |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |
|Depolama Hesapları |3,8 |Depolama hesabı erişimi için ' güvenilen Microsoft Hizmetleri 'nin etkinleştirildiğinden emin olun |[Depolama hesapları güvenilen Microsoft hizmetlerinden erişime izin verilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc9d007d0-c057-4772-b18c-01e546713bcd) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/StorageAccess_TrustedMicrosoftServices_Audit.json) |
|Günlüğe Kaydetme ve İzleme |5.1.6 |Etkinlik günlükleriyle kapsayıcıyı içeren depolama hesabının BYOK ile şifrelendiğinden emin olun (kendi anahtarınızı kullanın) |[Etkinlik günlükleri içeren kapsayıcıyı içeren depolama hesabı BYOK ile şifrelenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffbb99e8e-e444-4da0-9ff1-75c92f5a85b2) |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_StorageAccountBYOK_Audit.json) |

## <a name="nist-sp-800-171-r2"></a>NIST SP 800-171 R2

Tüm Azure hizmetleri için kullanılabilen Azure Ilkesinin Bu uyumluluk standardına nasıl eşlendiğini gözden geçirmek için bkz. [Azure Ilke mevzuatı uyumluluğu-NıST SP 800-171 R2](../../../../articles/governance/policy/samples/nist-sp-800-171-r2.md).
Bu uyumluluk standardı hakkında daha fazla bilgi için bkz. [NıST SP 800-171 R2](https://csrc.nist.gov/publications/detail/sp/800-171/rev-2/final).

|Etki alanı |Denetim KIMLIĞI |Denetim başlığı |İlke<br /><sub>(Azure portal)</sub> |İlke sürümü<br /><sub>GitHub</sub>  |
|---|---|---|---|---|
|Erişim Denetimi |3.1.1 |Yetkili kullanıcılarla sistem erişimini sınırlayın, yetkili kullanıcı adına işlem gören süreçler ve cihazlar (diğer sistemler dahil). |[Depolama hesapları, ağ erişimini kısıtlıyor olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F34c877ad-507e-4c82-993e-3452a6e0ad3c) |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |
|Erişim Denetimi |3.1.12 |Uzaktan erişim oturumlarını izleyin ve denetleyin. |[Depolama hesapları, ağ erişimini kısıtlıyor olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F34c877ad-507e-4c82-993e-3452a6e0ad3c) |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |
|Sistem ve Iletişim koruması |3.13.1 |Kuruluş sistemlerinin dış sınırları ve anahtar iç sınırları üzerinde, iletişimleri izleme, denetleme ve koruma (örneğin, kuruluş sistemleri tarafından gönderilen veya alınan bilgiler). |[Depolama hesaplarına güvenli aktarım etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F404c3081-a854-4457-ae30-26a93ef643f9) |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_AuditForHTTPSEnabled_Audit.json) |
|Sistem ve Iletişim koruması |3.13.1 |Kuruluş sistemlerinin dış sınırları ve anahtar iç sınırları üzerinde, iletişimleri izleme, denetleme ve koruma (örneğin, kuruluş sistemleri tarafından gönderilen veya alınan bilgiler). |[Depolama hesapları, ağ erişimini kısıtlıyor olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F34c877ad-507e-4c82-993e-3452a6e0ad3c) |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |
|Sistem ve Iletişim koruması |3.13.5 |İç ağlardan fiziksel veya mantıksal olarak ayrılmış, genel olarak erişilebilen sistem bileşenleri için alt ağları uygulayın. |[Depolama hesapları, ağ erişimini kısıtlıyor olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F34c877ad-507e-4c82-993e-3452a6e0ad3c) |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |
|Sistem ve Iletişim koruması |3.13.8 |Diğer fiziksel korumalardan aksi belirtilmedikçe, iletim sırasında CUı 'nin yetkisiz olarak açıklanmasını engellemek için şifreleme mekanizmaları uygulayın. |[Depolama hesaplarına güvenli aktarım etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F404c3081-a854-4457-ae30-26a93ef643f9) |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_AuditForHTTPSEnabled_Audit.json) |

## <a name="nist-sp-800-53-r4"></a>NIST SP 800-53 R4

Tüm Azure hizmetleri için kullanılabilen Azure Ilkesi 'nin Bu uyumluluk standardına nasıl eşlendiğini gözden geçirmek için bkz. [Azure Ilke mevzuatı uyumluluğu-NıST SP 800-53 R4](../../../../articles/governance/policy/samples/nist-sp-800-53-r4.md).
Bu uyumluluk standardı hakkında daha fazla bilgi için bkz. [NıST SP 800-53 R4](https://nvd.nist.gov/800-53).

|Etki alanı |Denetim KIMLIĞI |Denetim başlığı |İlke<br /><sub>(Azure portal)</sub> |İlke sürümü<br /><sub>GitHub</sub>  |
|---|---|---|---|---|
|Erişim Denetimi |AC-17 (1) |Uzaktan erişim \| otomatik izleme/denetim |[Depolama hesapları, ağ erişimini kısıtlıyor olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F34c877ad-507e-4c82-993e-3452a6e0ad3c) |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |
|Sistem ve Iletişim koruması |SC-7 |Sınır koruması |[Depolama hesapları, ağ erişimini kısıtlıyor olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F34c877ad-507e-4c82-993e-3452a6e0ad3c) |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |
|Sistem ve Iletişim koruması |SC-8 (1) |İletim gizliliği ve bütünlük \| şifreleme veya alternatif fiziksel koruma |[Depolama hesaplarına güvenli aktarım etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F404c3081-a854-4457-ae30-26a93ef643f9) |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_AuditForHTTPSEnabled_Audit.json) |


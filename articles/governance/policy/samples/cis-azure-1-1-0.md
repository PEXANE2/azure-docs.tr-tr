---
title: CIS için yasal uyumluluk ayrıntıları Microsoft Azure kuruluş kıyaslaması
description: CIS ayrıntıları, kıyaslama mevzuatı uyumluluğu yerleşik girişimi 'nin temelleri Microsoft Azure. Her denetim, değerlendirmede yardımcı olan bir veya daha fazla Azure Ilke tanımına eşlenir.
ms.date: 06/23/2020
ms.topic: sample
ms.custom: generated
ms.openlocfilehash: 400c4af97a693814d13ec3b8556db5eca44b39c6
ms.sourcegitcommit: 01cd19edb099d654198a6930cebd61cae9cb685b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85312359"
---
# <a name="details-of-the-cis-microsoft-azure-foundations-benchmark-regulatory-compliance-built-in-initiative"></a>CIS Microsoft Azure temel bilgiler kıyaslama mevzuatı uyumluluğu yerleşik girişimi

Aşağıdaki makalede, Azure Ilke mevzuatı uyumluluğu yerleşik girişim tanımının, CIS 'deki **Uyumluluk etki alanları** ve **denetimleriyle** Microsoft Azure, temel kıyaslama kıyaslaması ile nasıl eşleştiği açıklanır.
Bu uyumluluk standardı hakkında daha fazla bilgi için bkz. [cıs Microsoft Azure kuruluş kıyaslaması](https://www.cisecurity.org/benchmark/azure/). _Sahipliği_anlamak için bkz. [Azure ilke ilkesi tanımı](../concepts/definition-structure.md#type) ve [bulutta paylaşılan sorumluluk](../../../security/fundamentals/shared-responsibility.md).

Aşağıdaki eşlemeler, **cıs Microsoft Azure temel kıyaslama** denetimlerine göre yapılır. Doğrudan belirli bir **Uyumluluk etki alanına**geçmek için sağ taraftaki gezintiyi kullanın. Denetimlerin birçoğu bir [Azure Policy](../overview.md) Initiative tanımıyla uygulanır. Tüm girişim tanımını gözden geçirmek için Azure portal **ilkeyi** açın ve **tanımlar** sayfasını seçin.
Ardından, CIS Microsoft Azure temel bilgiler **kıyaslama 1.1.0** mevzuat uyumluluğu yerleşik girişim tanımını bulun ve seçin.

Bu yerleşik girişim, [cıs Microsoft Azure temelleri şeması örneklerinden](../../blueprints/samples/cis-azure-1-1-0.md)bir parçası olarak dağıtılır.

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure ilke](../overview.md) tanımı ile ilişkilidir.
> Bu ilkeler, denetimiyle [uyumluluğu değerlendirmenize](../how-to/get-compliance-data.md) yardımcı olabilir; Ancak, bir denetim ve bir veya daha fazla ilke arasında genellikle bir 1:1 veya bir eşleşme yoktur. Bu nedenle, Azure Ilkesiyle **uyumlu** yalnızca ilke tanımlarının kendilerine başvurur; Bu, bir denetimin tüm gereksinimleriyle tamamen uyumlu olduğunuzdan emin değildir. Buna ek olarak, uyumluluk standardı şu anda herhangi bir Azure Ilke tanımı tarafından açıklanmayan denetimler içerir. Bu nedenle, Azure Ilkesinde uyumluluk, genel uyumluluk durumunuzu yalnızca kısmi görünümüdür. Bu uyumluluk standardı için uyumluluk etki alanları, denetimler ve Azure Ilke tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub kayıt geçmişine](https://github.com/Azure/azure-policy/commits/master/built-in-policies/policySetDefinitions/Regulatory%20Compliance/CISv1_1_0_audit.json)bakın.

## <a name="appservice"></a>AppService

### <a name="ensure-app-service-authentication-is-set-on-azure-app-service"></a>Azure App Service üzerinde App Service kimlik doğrulamasının ayarlandığından emin olun

**Kimlik**: cıs Azure 9,1 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[API uygulamanızda kimlik doğrulaması etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc4ebc54a-46e1-481a-bee2-d4411e95d828) |Azure App Service kimlik doğrulaması, anonim HTTP isteklerinin API uygulamasına ulaşmasını engelleyebilen veya API uygulamasına erişmeden önce belirteçlere sahip olanların kimliğini engelleyebilecek bir özelliktir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_Authentication_ApiApp_Audit.json) |
|[Işlev uygulamanızda kimlik doğrulaması etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc75248c1-ea1d-4a9c-8fc9-29a6aabd5da8) |Azure App Service kimlik doğrulaması, anonim HTTP isteklerinin Işlev uygulamasına ulaşmasını engelleyebilen veya Işlev uygulamasına erişmeden önce belirteçlere sahip oldukları kimlik doğrulamasından geçebilen bir özelliktir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_Authentication_functionapp_Audit.json) |
|[Web uygulamanızda kimlik doğrulaması etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F95bccee9-a7f8-4bec-9ee9-62c3473701fc) |Azure App Service kimlik doğrulaması, anonim HTTP isteklerinin Web uygulamasına ulaşmasını engelleyebilecek veya Web uygulamasına erişmeden önce belirteçlere sahip oldukları kimlik doğrulamasından geçebilen bir özelliktir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_Authentication_WebApp_Audit.json) |

### <a name="ensure-that-net-framework-version-is-the-latest-if-used-as-a-part-of-the-web-app"></a>Web uygulamasının bir parçası olarak kullanılıyorsa, ' .NET Framework ' sürümünün en son sürümü olduğundan emin olun

**Kimlik**: cıs Azure 9,6 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[API uygulamasının bir parçası olarak kullanılıyorsa ' .NET Framework ' sürümünün en son sürümü olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc2e7ca55-f62c-49b2-89a4-d41eb661d2f0) |Güvenlik kusuru veya ek işlevsellik içermesi nedeniyle, düzenli aralıklarla, .NET Framework yazılım için daha yeni sürümler yayımlanır. Web uygulamaları için en son .NET Framework sürümünü kullanmak, en son sürümün ve/veya yeni işlevsellikleri varsa güvenlik düzeltmelerinin avantajlarından yararlanmak için önerilir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_ApiApp_Audit_NETFrameWork_Latest.json) |
|[İşlev Uygulaması bir parçası olarak kullanılıyorsa ' .NET Framework ' sürümünün en son sürümü olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F10c1859c-e1a7-4df3-ab97-a487fa8059f6) |Güvenlik kusuru veya ek işlevsellik içermesi nedeniyle, düzenli aralıklarla, .NET Framework yazılım için daha yeni sürümler yayımlanır. Web uygulamaları için en son .NET Framework sürümünü kullanmak, en son sürümün ve/veya yeni işlevsellikleri varsa güvenlik düzeltmelerinin avantajlarından yararlanmak için önerilir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_FunctionApp_Audit_NETFrameWork_Latest.json) |
|[Web uygulamasının bir parçası olarak kullanılıyorsa ' .NET Framework ' sürümünün en son sürümü olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F843664e0-7563-41ee-a9cb-7522c382d2c4) |Güvenlik kusuru veya ek işlevsellik içermesi nedeniyle, düzenli aralıklarla, .NET Framework yazılım için daha yeni sürümler yayımlanır. Web uygulamaları için en son .NET Framework sürümünü kullanmak, en son sürümün ve/veya yeni işlevsellikleri varsa güvenlik düzeltmelerinin avantajlarından yararlanmak için önerilir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_Webapp_Audit_NETFrameWork_Latest.json) |

### <a name="ensure-that-http-version-is-the-latest-if-used-to-run-the-web-app"></a>Web uygulamasını çalıştırmak için kullanılıyorsa, ' HTTP Version ' ' ın en son sürümü olduğundan emin olun

**Kimlik**: cıs Azure 9,10 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[API uygulamasını çalıştırmak için kullanılmışsa ' HTTP Version ' nin en son sürümü olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F991310cd-e9f3-47bc-b7b6-f57b557d07db) |Düzenli aralıklarla, güvenlik kusurlarında veya ek işlevsellik dahil olmak üzere HTTP için yeni sürümler yayımlanır. Web uygulamaları için en son HTTP sürümünü kullanarak, herhangi bir ve/veya daha yeni sürümün yeni işlevsellikleri, güvenlik düzeltmelerinin avantajlarından yararlanın. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_ApiApp_Audit_HTTP_Latest.json) |
|[Işlev uygulamasını çalıştırmak için kullanılmışsa ' HTTP Version ' nin en son sürümü olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe2c1c086-2d84-4019-bff3-c44ccd95113c) |Düzenli aralıklarla, güvenlik kusurlarında veya ek işlevsellik dahil olmak üzere HTTP için yeni sürümler yayımlanır. Web uygulamaları için en son HTTP sürümünü kullanarak, herhangi bir ve/veya daha yeni sürümün yeni işlevsellikleri, güvenlik düzeltmelerinin avantajlarından yararlanın. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_FunctionApp_Audit_HTTP_Latest.json) |
|[Web uygulamasını çalıştırmak için kullanılıyorsa, ' HTTP Version ' ' ın en son sürümü olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F8c122334-9d20-4eb8-89ea-ac9a705b74ae) |App Service yönetilen hizmet kimliği, bağlantı dizelerindeki kimlik bilgileri gibi, uygulamadaki gizli dizileri ortadan kaldırarak uygulamayı daha güvenli hale getirir. App Service 'te Azure Active Directory kaydetme sırasında, uygulama Kullanıcı adı ve parolalara gerek olmadan diğer Azure hizmetlerine güvenli bir şekilde bağlanır |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_WebApp_Audit_HTTP_Latest.json) |

### <a name="ensure-that-java-version-is-the-latest-if-used-to-run-the-web-app"></a>Web uygulamasını çalıştırmak için kullanıldıysa, ' Java sürümü ' nin en son sürümü olduğundan emin olun

**Kimlik**: cıs Azure 9,9 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[API uygulamasının bir parçası olarak kullanılıyorsa ' Java sürümü ' nin en son sürümü olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F88999f4c-376a-45c8-bcb3-4058f713cf39) |Güvenlik kusuru veya ek işlevsellik içermesi nedeniyle, düzenli aralıklarla, Java için yeni sürümler yayımlanır. En son sürümün ve/veya yeni işlevleri olan güvenlik düzeltmelerinin avantajlarından yararlanmak için API uygulamaları için en son Python sürümünü kullanmak önerilir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_ApiApp_Audit_java_Latest.json) |
|[Işlev uygulamasının bir parçası olarak kullanılıyorsa ' Java sürümü ' nin en son sürümü olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9d0b6ea4-93e2-4578-bf2f-6bb17d22b4bc) |Güvenlik kusuru veya ek işlevsellik içermesi nedeniyle, düzenli aralıklarla, Java Yazılımları için daha yeni sürümler yayımlanır. Işlev uygulamaları için en son Java sürümünü kullanmak, en son sürümün ve/veya yeni işlevsellikleri varsa güvenlik düzeltmelerinin avantajlarından yararlanmak için önerilir. |Auditınotexists, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_FunctionApp_Audit_java_Latest.json) |
|[Web uygulamasının bir parçası olarak kullanılıyorsa ' Java sürümü 'nin en son sürümü olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F496223c3-ad65-4ecd-878a-bae78737e9ed) |Güvenlik kusuru veya ek işlevsellik içermesi nedeniyle, düzenli aralıklarla, Java Yazılımları için daha yeni sürümler yayımlanır. Web uygulamaları için en son Java sürümünü kullanmak, en son sürümün ve/veya yeni işlevsellikleri varsa güvenlik düzeltmelerinin avantajlarından yararlanmak için önerilir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_WebApp_Audit_java_Latest.json) |

### <a name="ensure-that-php-version-is-the-latest-if-used-to-run-the-web-app"></a>Web uygulamasını çalıştırmak için kullanıldıysa, ' PHP Version ' nin en son sürümü olduğundan emin olun

**Kimlik**: cıs Azure 9,7 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[API uygulamasının bir parçası olarak kullanılıyorsa ' PHP Version ' öğesinin en son sürümü olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1bc1795e-d44a-4d48-9b3b-6fff0fd5f9ba) |Düzenli aralıklarla, güvenlik kusurlarında veya ek işlevsellik içermesi nedeniyle PHP yazılımları için yeni sürümler yayımlanır. En son sürümün ve/veya yeni işlevleri olan güvenlik düzeltmelerinin avantajlarından yararlanmak için API uygulamaları için en son PHP sürümünü kullanmak önerilir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_ApiApp_Audit_PHP_Latest.json) |
|[Işlev uygulamasının bir parçası olarak kullanılıyorsa, ' PHP Version ' öğesinin en son sürümü olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fab965db2-d2bf-4b64-8b39-c38ec8179461) |Düzenli aralıklarla, güvenlik kusurlarında veya ek işlevsellik içermesi nedeniyle PHP yazılımları için yeni sürümler yayımlanır. Işlev uygulamaları için en son PHP sürümünü kullanmak, en son sürümün ve/veya yeni işlevsellikleri varsa güvenlik düzeltmelerinin avantajlarından yararlanmak için önerilir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_FunctionApp_Audit_PHP_Latest.json) |
|[WEB uygulamasının bir parçası olarak kullanılıyorsa, ' PHP Version ' öğesinin en son sürümü olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7261b898-8a84-4db8-9e04-18527132abb3) |Düzenli aralıklarla, güvenlik kusurlarında veya ek işlevsellik içermesi nedeniyle PHP yazılımları için yeni sürümler yayımlanır. Web uygulamaları için en son PHP sürümünü kullanmak, en son sürümün ve/veya yeni işlevsellikleri varsa güvenlik düzeltmelerinin avantajlarından yararlanmak için önerilir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_Webapp_Audit_PHP_Latest.json) |

### <a name="ensure-that-python-version-is-the-latest-if-used-to-run-the-web-app"></a>Web uygulamasını çalıştırmak için kullanılıyorsa, ' Python sürümü ' nin en son sürümü olduğundan emin olun

**Kimlik**: cıs Azure 9,8 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[API uygulamasının bir parçası olarak kullanılıyorsa ' Python sürümü 'nin en son bir sürüm olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F74c3584d-afae-46f7-a20a-6f8adba71a16) |Düzenli aralıklarla, güvenlik kusurlarında veya ek işlevsellik içermesi nedeniyle Python yazılımı için yeni sürümler yayımlanır. En son sürümün ve/veya yeni işlevleri olan güvenlik düzeltmelerinin avantajlarından yararlanmak için API uygulamaları için en son Python sürümünü kullanmak önerilir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_ApiApp_Audit_python_Latest.json) |
|[Işlev uygulamasının bir parçası olarak kullanılıyorsa ' Python sürümü 'nin en son bir sürüm olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7238174a-fd10-4ef0-817e-fc820a951d73) |Düzenli aralıklarla, güvenlik kusurlarında veya ek işlevsellik içermesi nedeniyle Python yazılımı için yeni sürümler yayımlanır. En son sürümdeki güvenlik düzeltmelerinin ve/veya yeni işlevsellikleri avantajlarından yararlanmak için Işlev uygulamaları için en son Python sürümünü kullanmak önerilir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_FunctionApp_Audit_python_Latest.json) |
|[Web uygulamasının bir parçası olarak kullanılıyorsa ' Python sürümü 'nin en son bir sürüm olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7008174a-fd10-4ef0-817e-fc820a951d73) |Düzenli aralıklarla, güvenlik kusurlarında veya ek işlevsellik içermesi nedeniyle Python yazılımı için yeni sürümler yayımlanır. Web uygulamaları için en son Python sürümünü kullanmak, en son sürümün ve/veya yeni işlevsellikleri varsa güvenlik düzeltmelerinin avantajlarından yararlanmak için önerilir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_WebApp_Audit_python_Latest.json) |

### <a name="ensure-that-register-with-azure-active-directory-is-enabled-on-app-service"></a>Azure Active Directory kayıt App Service etkinleştirildiğinden emin olun

**Kimlik**: cıs Azure 9,5 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure Active Directory ile kaydolma özelliğinin API uygulamasında etkinleştirildiğinden emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F86d97760-d216-4d81-a3ad-163087b2b6c3) |App Service yönetilen hizmet kimliği, bağlantı dizelerindeki kimlik bilgileri gibi, uygulamadaki gizli dizileri ortadan kaldırarak uygulamayı daha güvenli hale getirir. App Service 'te Azure Active Directory kaydetme sırasında, uygulama Kullanıcı adı ve parolalara gerek olmadan diğer Azure hizmetlerine güvenli bir şekilde bağlanır |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_ApiApp_Audit_AD_Enabled.json) |
|[Azure Active Directory kayıt İşlev Uygulaması etkinleştirildiğinden emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff0473e7a-a1ba-4e86-afb2-e829e11b01d8) |App Service yönetilen hizmet kimliği, bağlantı dizelerindeki kimlik bilgileri gibi, uygulamadaki gizli dizileri ortadan kaldırarak uygulamayı daha güvenli hale getirir. App Service 'te Azure Active Directory kaydetme sırasında, uygulama Kullanıcı adı ve parolalara gerek olmadan diğer Azure hizmetlerine güvenli bir şekilde bağlanır |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_FunctionApp_Audit_AD_Enabled.json) |
|[Azure Active Directory kaydetme özelliğinin WEB uygulamasında etkinleştirildiğinden emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faa81768c-cb87-4ce2-bfaa-00baa10d760c) |App Service yönetilen hizmet kimliği, bağlantı dizelerindeki kimlik bilgileri gibi, uygulamadaki gizli dizileri ortadan kaldırarak uygulamayı daha güvenli hale getirir. App Service 'te Azure Active Directory kaydetme sırasında, uygulama Kullanıcı adı ve parolalara gerek olmadan diğer Azure hizmetlerine güvenli bir şekilde bağlanır |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_Webapp_Audit_AD_Enabled.json) |

### <a name="ensure-the-web-app-has-client-certificates-incoming-client-certificates-set-to-on"></a>Web uygulamasında ' Istemci sertifikaları (gelen istemci sertifikaları) ', ' on ' olarak ayarlanmış olduğundan emin olun

**Kimlik**: cıs Azure 9,4 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[API uygulamasında ' Istemci sertifikaları (gelen istemci sertifikaları) ', ' on ' olarak ayarlanmış olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0c192fe8-9cbb-4516-85b3-0ade8bd03886) |İstemci sertifikaları, uygulamanın gelen istekler için bir sertifika istemesine izin verir. Yalnızca geçerli sertifikaya sahip istemciler uygulamaya erişebiliyor. |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_ApiApp_Audit_ClientCert.json) |
|[Işlev uygulamasında ' Istemci sertifikaları (gelen istemci sertifikaları) ', ' on ' olarak ayarlanmış olduğundan emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Feaebaea7-8013-4ceb-9d14-7eb32271373c) |İstemci sertifikaları, uygulamanın gelen istekler için bir sertifika istemesine izin verir. Yalnızca geçerli sertifikaya sahip istemciler uygulamaya erişebiliyor. |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_FunctionApp_Audit_ClientCert.json) |
|[WEB uygulamasının ' Istemci sertifikaları (gelen istemci sertifikaları) ' olarak ' on ' olarak ayarlandığından emin olun](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5bb220d9-2698-4ee4-8404-b9c30c9df609) |İstemci sertifikaları, uygulamanın gelen istekler için bir sertifika istemesine izin verir. Yalnızca geçerli sertifikaya sahip istemciler uygulamaya erişebiliyor. |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_Webapp_Audit_ClientCert.json) |

### <a name="ensure-web-app-is-using-the-latest-version-of-tls-encryption"></a>Web uygulamasının en son TLS şifreleme sürümünü kullandığından emin olun

**Kimlik**: cıs Azure 9,3 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[API uygulamanızda en son TLS sürümü kullanılmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F8cb6aa8b-9e41-4f4e-aa25-089a7ac2581e) |En son TLS sürümüne yükselt |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_RequireLatestTls_ApiApp_Audit.json) |
|[İşlev Uygulaması en son TLS sürümü kullanılmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff9d614c5-c173-4d56-95a7-b4437057d193) |En son TLS sürümüne yükselt |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_RequireLatestTls_FunctionApp_Audit.json) |
|[Web uygulamanızda en son TLS sürümü kullanılmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff0e6e85b-9b9f-4a4b-b67b-f730d42f1b0b) |En son TLS sürümüne yükselt |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppService_RequireLatestTls_WebApp_Audit.json) |

### <a name="ensure-web-app-redirects-all-http-traffic-to-https-in-azure-app-service"></a>Web uygulamasının tüm HTTP trafiğini Azure App Service ' de HTTPS 'ye yönlendirdiğinden emin olun

**Kimlik**: cıs Azure 9,2 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa4af4a39-4135-47fb-b175-47fbdf85311d) |HTTPS kullanımı, sunucu/hizmet kimlik doğrulamasını sağlar ve ağ katmanı gizlice dinleme saldırılarına karşı geçiş sırasında verileri korur. |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/App%20Service/AppServiceWebapp_AuditHTTP_Audit.json) |

## <a name="database-services"></a>Veritabanı Hizmetleri

### <a name="ensure-enforce-ssl-connection-is-set-to-enabled-for-mysql-database-server"></a>MySQL veritabanı sunucusu için ' SSL bağlantısını zorla ' özelliğinin ' ENABLED ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 4,11 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[MySQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe802a67a-daf5-4436-9ea6-f6d821dd0c5d) |Bu ilke, SSL bağlantısını zorunlu olmayan tüm MySQL Server 'ı denetler. MySQL için Azure veritabanı, MySQL Server için Azure veritabanı sunucunuzu Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamalarına bağlamayı destekler. Veritabanı sunucunuz ile istemci uygulamalarınız arasında SSL bağlantılarını zorunlu tutma, sunucu ile uygulamanız arasındaki veri akışını şifreleyerek, ortadaki adam saldırılarına karşı korunmaya yardımcı olur. |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MySQL_EnableSSL_Audit.json) |

### <a name="ensure-enforce-ssl-connection-is-set-to-enabled-for-postgresql-database-server"></a>PostgreSQL veritabanı sunucusu için ' SSL bağlantısını zorla ' ' ENABLED ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 4,13 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[PostgreSQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd158790f-bfb0-486c-8631-2dc6b4e8e6af) |Bu ilke, SSL bağlantısını zorunlu olmayan tüm PostgreSQL sunucusunu denetler. PostgreSQL için Azure veritabanı, Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamalarınızı PostgreSQL hizmetine bağlamayı tercih eder. Veritabanı sunucunuz ile istemci uygulamalarınız arasında SSL bağlantılarını zorunlu tutma, sunucu ile uygulamanız arasındaki veri akışını şifreleyerek ' ortadaki adam ' saldırılarına karşı korunmaya yardımcı olur |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/PostgreSQL_EnableSSL_Audit.json) |

### <a name="ensure-server-parameter-connection_throttling-is-set-to-on-for-postgresql-database-server"></a>PostgreSQL veritabanı sunucusu için ' connection_throttling ' sunucu parametresinin ' ON ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 4,17 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[PostgreSQL veritabanı sunucuları için bağlantı azaltma etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5345bb39-67dc-4960-a1bf-427e16b9a0bd) |Bu ilke, bağlantı azaltma etkin olmadan ortamınızdaki tüm PostgreSQL veritabanlarını denetlemeye yardımcı olur. Bu ayar, çok sayıda geçersiz parola oturum açma başarısızlığı için IP başına geçici bağlantı azaltmayı mümkün. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/PostgreSQL_ConnectionThrottling_Enabled_Audit.json) |

### <a name="ensure-server-parameter-log_checkpoints-is-set-to-on-for-postgresql-database-server"></a>PostgreSQL veritabanı sunucusu için ' log_checkpoints ' sunucu parametresinin ' ON ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 4,12 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[PostgreSQL veritabanı sunucuları için günlük denetim noktaları etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Feb6f77b9-bd53-4e35-a23d-7f65d5f0e43d) |Bu ilke, ortamınızda log_checkpoints ayarı etkinleştirilmeden tüm PostgreSQL veritabanlarının denetlenmesini sağlar. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/PostgreSQL_EnableLogCheckpoint_Audit.json) |

### <a name="ensure-server-parameter-log_connections-is-set-to-on-for-postgresql-database-server"></a>PostgreSQL veritabanı sunucusu için ' log_connections ' sunucu parametresinin ' ON ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 4,14 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[PostgreSQL veritabanı sunucuları için günlük bağlantıları etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Feb6f77b9-bd53-4e35-a23d-7f65d5f0e442) |Bu ilke, ortamınızda log_connections ayarı etkinleştirilmeden tüm PostgreSQL veritabanlarının denetlenmesini sağlar. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/PostgreSQL_EnableLogConnections_Audit.json) |

### <a name="ensure-server-parameter-log_disconnections-is-set-to-on-for-postgresql-database-server"></a>PostgreSQL veritabanı sunucusu için ' log_disconnections ' sunucu parametresinin ' ON ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 4,15 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[PostgreSQL veritabanı sunucuları için bağlantılar günlüğe kaydedilmez.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Feb6f77b9-bd53-4e35-a23d-7f65d5f0e446) |Bu ilke, ortamınızda log_disconnections etkinleştirilmeden herhangi bir PostgreSQL veritabanının denetlenmesini sağlar. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/PostgreSQL_EnableLogDisconnections_Audit.json) |

### <a name="ensure-server-parameter-log_duration-is-set-to-on-for-postgresql-database-server"></a>PostgreSQL veritabanı sunucusu için ' log_duration ' sunucu parametresinin ' ON ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 4,16 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[PostgreSQL veritabanı sunucuları için günlük süresi etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Feb6f77b9-bd53-4e35-a23d-7f65d5f0e8f3) |Bu ilke, ortamınızda log_duration ayarı etkinleştirilmeden tüm PostgreSQL veritabanlarının denetlenmesini sağlar. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/PostgreSQL_EnableLogDuration_Audit.json) |

### <a name="ensure-sql-servers-tde-protector-is-encrypted-with-byok-use-your-own-key"></a>SQL Server 'ın TDE koruyucunun BYOK ile şifrelendiğinden emin olun (kendi anahtarınızı kullanın)

**Kimlik**: cıs Azure 4,10 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[SQL yönetilen örnek TDE koruyucusu kendi anahtarınızla şifrelenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F048248b0-55cd-46da-b1ff-39efd52db260) |Kendi anahtar desteğiniz ile Saydam Veri Şifrelemesi (TDE), TDE koruyucu üzerinde daha fazla saydamlık ve denetim, HSM destekli bir dış hizmetle artırılmış güvenlik ve görev ayrımı ile ilgili yükseltme sağlar. |Auditınotexists, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlManagedInstance_EnsureServerTDEisEncryptedWithYourOwnKey_Audit.json) |
|[SQL Server TDE koruyucusu kendi anahtarınızla şifrelenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0d134df8-db83-46fb-ad72-fe0c9428c8dd) |Kendi anahtar desteğiniz ile Saydam Veri Şifrelemesi (TDE), TDE koruyucu üzerinde daha fazla saydamlık ve denetim, HSM destekli bir dış hizmetle artırılmış güvenlik ve görev ayrımı ile ilgili yükseltme sağlar. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServer_EnsureServerTDEisEncryptedWithYourOwnKey_Audit.json) |

### <a name="ensure-that-advanced-data-security-on-a-sql-server-is-set-to-on"></a>SQL Server 'da ' Gelişmiş veri güvenliği 'nin ' açık ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 4,4 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Gelişmiş veri güvenliği SQL yönetilen örneği üzerinde etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fabfb7388-5bf4-4ad7-ba99-2cd2f41cebb9) |Gelişmiş veri güvenliği olmadan her SQL yönetilen örneğini denetleyin. |Auditınotexists, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlManagedInstance_AdvancedDataSecurity_Audit.json) |
|[Gelişmiş veri güvenliği SQL sunucularınızda etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fabfb4388-5bf4-4ad7-ba82-2cd2f41ceae9) |Gelişmiş veri güvenliği olmadan SQL sunucularını denetleme |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServer_AdvancedDataSecurity_Audit.json) |

### <a name="ensure-that-auditactiongroups-in-auditing-policy-for-a-sql-server-is-set-properly"></a>SQL Server için ' denetim ' ilkesindeki ' AuditActionGroups ' işleminin düzgün şekilde ayarlandığından emin olun

**Kimlik**: cıs Azure 4,2 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[SQL denetim ayarları, kritik etkinlikleri yakalamak için yapılandırılmış eylem grupları olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7ff426e2-515f-405a-91c8-4f2333442eb5) |AuditActionsAndGroups özelliği, kapsamlı bir denetim günlüğü sağlamak için en az SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP, FAILED_DATABASE_AUTHENTICATION_GROUP, BATCH_COMPLETED_GROUP içermelidir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServerAuditing_ActionsAndGroups_Audit.json) |

### <a name="ensure-that-auditing-is-set-to-on"></a>' Denetim ' in ' on ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 4,1 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[SQL Server üzerinde denetim etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9) |SQL Server denetim, sunucudaki tüm veritabanları genelinde veritabanı etkinliklerini izlemek ve bir denetim günlüğüne kaydetmek için etkinleştirilmelidir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServerAuditing_Audit.json) |

### <a name="ensure-that-auditing-retention-is-greater-than-90-days"></a>' Denetim ' saklama ' ın 90 günden daha fazla olduğundan emin olun

**Kimlik**: cıs Azure 4,3 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[SQL Server 'lar 90 günden daha uzun denetim bekletme günlerindeki şekilde yapılandırılmalıdır.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F89099bee-89e0-4b26-a5f4-165451757743) |90 günden daha az bir denetim saklama süresi ile yapılandırılmış SQL sunucularını denetleyin. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServerAuditingRetentionDays_Audit.json) |

### <a name="ensure-that-azure-active-directory-admin-is-configured"></a>Azure Active Directory yöneticisinin yapılandırıldığından emin olun

**Kimlik**: cıs Azure 4,8 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[SQL sunucuları için bir Azure Active Directory Yöneticisi sağlanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1f314764-cb73-4fc9-b863-8eca98ac36e9) |Azure AD kimlik doğrulamasını etkinleştirmek için SQL sunucunuz için bir Azure Active Directory yöneticisinin sağlamasını denetleyin. Azure AD kimlik doğrulaması, veritabanı kullanıcıları ve diğer Microsoft Hizmetleri için Basitleştirilmiş izin yönetimi ve merkezi kimlik yönetimine izin verebilir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SQL_DB_AuditServerADAdmins_Audit.json) |

### <a name="ensure-that-data-encryption-is-set-to-on-on-a-sql-database"></a>' Veri şifrelemesi 'nin bir SQL veritabanında ' on ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 4,9 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[SQL veritabanlarındaki Saydam Veri Şifrelemesi etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F17k78e20-9358-41c9-923c-fb736d382a12) |Rest verilerini korumak ve uyumluluk gereksinimlerini karşılamak için saydam veri şifrelemesi etkinleştirilmelidir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlDBEncryption_Audit.json) |

### <a name="ensure-that-email-service-and-co-administrators-is-enabled"></a>' E-posta hizmeti ve ortak yöneticiler ' ' in ' etkin ' olduğundan emin olun

**Kimlik**: cıs Azure 4,7 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[SQL yönetilen örnek gelişmiş veri güvenliği ayarları 'nda yöneticilere ve abonelik sahiplerine e-posta bildirimleri etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faeb23562-188d-47cb-80b8-551f16ef9fff) |SQL yönetilen örnek Gelişmiş tehdit koruması ayarları 'nda ' yönetici ve abonelik sahiplerine e-posta bildirimi ' nin etkinleştirildiğini denetleyin. Bu ayar, SQL yönetilen örneği üzerinde anormal etkinliklerin tüm algılamalarını yöneticilere mümkün olan en kısa sürede raporlanmasını sağlar. |Auditınotexists, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlManagedInstance_AdvancedDataSecurityEmailAdmins_Audit.json) |
|[SQL Server Gelişmiş veri güvenliği ayarları 'nda Yöneticiler ve abonelik sahiplerine e-posta bildirimleri etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc8343d2f-fdc9-4a97-b76f-fc71d1163bfc) |SQL Server Gelişmiş tehdit koruması ayarları 'nda ' yönetici ve abonelik sahiplerine e-posta bildirimi ' nin etkinleştirildiğini denetleyin. Bu, SQL Server 'daki anormal etkinliklerin tüm algılamalarını yöneticilere mümkün olan en kısa sürede raporlanmasını sağlar. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServer_AdvancedDataSecurityEmailAdmins_Audit.json) |

### <a name="ensure-that-send-alerts-to-is-set"></a>' Uyarıları gönder 'in ayarlanmış olduğundan emin olun

**Kimlik**: cıs Azure 4,6 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[SQL yönetilen örneği için gelişmiş veri güvenliği ayarları, güvenlik uyarılarını almak için bir e-posta adresi içermelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3965c43d-b5f4-482e-b74a-d89ee0e0b3a8) |Gelişmiş veri güvenliği ayarlarındaki ' uyarıları Gönder ' alanı için bir e-posta adresinin sağlandığından emin olun. Bu e-posta adresi, SQL yönetilen örneği üzerinde anormal etkinlikler algılandığında uyarı bildirimleri alır. |Auditınotexists, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlManagedInstance_AdvancedDataSecurityEmails_Audit.json) |
|[SQL Server için gelişmiş veri güvenliği ayarları, güvenlik uyarılarını almak için bir e-posta adresi içermelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9677b740-f641-4f3c-b9c5-466005c85278) |Gelişmiş veri güvenliği sunucu ayarlarındaki ' uyarıları Gönder ' alanı için bir e-posta adresinin sağlandığından emin olun. Bu e-posta adresi SQL Server 'lar üzerinde anormal etkinlikler algılandığında uyarı bildirimleri alır. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServer_AdvancedDataSecurityEmails_Audit.json) |

### <a name="ensure-that-threat-detection-types-is-set-to-all"></a>' Tehdit algılama türleri ' ' All ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 4,5 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Gelişmiş tehdit koruması türleri SQL yönetilen örneği gelişmiş veri güvenliği ayarları içinde ' All ' olarak ayarlanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fbda18df3-5e41-4709-add9-2554ce68c966) |SQL yönetilen örneğiniz üzerinde tüm gelişmiş tehdit koruması türlerinin etkinleştirilmesi önerilir. Tüm türlerin etkinleştirilmesi SQL ekleme, veritabanı güvenlik açıklarına ve diğer anormal etkinliklere karşı korunur. |Auditınotexists, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlManagedInstance_EnsureThreatDetectionTypes_Audit.json) |
|[Gelişmiş tehdit koruması türleri SQL Server Gelişmiş veri güvenliği ayarları 'nda ' tümü ' olarak ayarlanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe756b945-1b1b-480b-8de8-9a0859d5f7ad) |SQL sunucularınızda tüm gelişmiş tehdit koruması türlerinin etkinleştirilmesi önerilir. Tüm türlerin etkinleştirilmesi SQL ekleme, veritabanı güvenlik açıklarına ve diğer anormal etkinliklere karşı korunur. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServer_EnsureThreatDetectionTypes_Audit.json) |

## <a name="identity-and-access-management"></a>Kimlik ve Erişim Yönetimi

### <a name="ensure-that-multi-factor-authentication-is-enabled-for-all-non-privileged-users"></a>Multi-Factor Authentication 'ın tüm ayrıcalıklı olmayan kullanıcılar için etkinleştirildiğinden emin olun

**Kimlik**: cıs Azure 1,2 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe3576e28-8b17-4677-84c3-db2990658d64) |Hesapların veya kaynakların ihlal oluşmasını engellemek için okuma ayrıcalıklarına sahip tüm abonelik hesapları için Multi-Factor Authentication (MFA) etkinleştirilmelidir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableMFAForReadPermissions_Audit.json) |

### <a name="ensure-that-multi-factor-authentication-is-enabled-for-all-privileged-users"></a>Multi-Factor Authentication 'ın tüm ayrıcalıklı kullanıcılar için etkinleştirildiğinden emin olun

**Kimlik**: cıs Azure 1,1 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F9297c21d-2ed6-4474-b48f-163f75654ce3) |Hesapların veya kaynakların ihlal oluşmasını engellemek için yazma ayrıcalıklarına sahip tüm abonelik hesapları için Multi-Factor Authentication (MFA) etkinleştirilmelidir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableMFAForWritePermissions_Audit.json) |
|[MFA, aboneliğinizde sahip izinleri olan hesaplarda etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faa633080-8b72-40c4-a2d7-d00c03e80bed) |Multi-Factor Authentication (MFA), hesapların veya kaynakların ihlaline engel olmak için sahip izinleri olan tüm abonelik hesapları için etkinleştirilmelidir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableMFAForOwnerPermissions_Audit.json) |

### <a name="ensure-that-no-custom-subscription-owner-roles-are-created"></a>Özel abonelik sahibi rollerinin oluşturulmadığından emin olun

**Kimlik**: cıs Azure 1,23 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Özel abonelik sahibi rolleri mevcut olmamalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F10ee2ea2-fb4d-45b8-a7e9-a2e770044cd9) |Bu ilke, özel abonelik sahibi rolü olmamasını sağlar. |Denetim, devre dışı |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/General/CustomSubscription_OwnerRole_Audit.json) |

### <a name="ensure-that-there-are-no-guest-users"></a>Konuk Kullanıcı olmadığından emin olun

**Kimlik**: cıs Azure 1,3 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff8456c1c-aa66-4dfb-861a-25d127b775c9) |İzlenmeyen erişimi engellemek için sahip izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_RemoveExternalAccountsWithOwnerPermissions_Audit.json) |
|[Okuma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5f76cf89-fbf2-47fd-a3f4-b891fa780b60) |İzlenmeyen erişimi engellemek için, okuma ayrıcalıklarına sahip dış hesapların aboneliğinizden kaldırılması gerekir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_RemoveExternalAccountsReadPermissions_Audit.json) |
|[Yazma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F5c607a2e-c700-4744-8254-d77e7c9eb5e4) |İzlenmeyen erişimi engellemek için, yazma ayrıcalıklarına sahip dış hesapların aboneliğinizden kaldırılması gerekir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_RemoveExternalAccountsWritePermissions_Audit.json) |

## <a name="logging-and-monitoring"></a>Günlüğe Kaydetme ve İzleme

### <a name="ensure-audit-profile-captures-all-the-activities"></a>Denetim profilinin tüm etkinlikleri yakaladığından emin olun

**Kimlik**: CIS Azure 5.1.3 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure Izleyici günlük profili, ' Write, ' ' DELETE, ' ve ' Action ' kategorilerinin günlüklerini toplamalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F1a4e592a-6a6e-44a5-9814-e36264ca96e7) |Bu ilke, bir günlük profilinin ' Write, ' ' DELETE, ' ve ' Action ' kategorilerinin günlüklerini toplamasına sağlar |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_CaptureAllCategories.json) |

### <a name="ensure-that-a-log-profile-exists"></a>Bir günlük profilinin mevcut olduğundan emin olun

**Kimlik**: CIS Azure 5.1.1 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure aboneliklerinin etkinlik günlüğü için bir günlük profili olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7796937f-307b-4598-941c-67d3a05ebfe7) |Bu ilke, etkinlik günlüklerini dışarı aktarmak için bir günlük profilinin etkinleştirilip etkinleştirilmediğini sağlar. Günlükleri bir depolama hesabına veya bir olay hub 'ına aktarmak için oluşturulmuş bir günlük profili olup olmadığını denetler. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/Logprofile_activityLogs_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-create-or-update-network-security-group"></a>Ağ güvenlik grubu oluşturmak veya güncelleştirmek için etkinlik günlüğü uyarısının mevcut olduğundan emin olun

**Kimlik**: CIS Azure 5.2.2 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Belirli yönetim işlemleri için bir etkinlik günlüğü uyarısı bulunmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb954148f-4c11-4c38-8221-be76711e194a) |Bu ilke, hiçbir etkinlik günlüğü uyarısı yapılandırılmadığı belirli yönetim işlemlerini denetler. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_AdministrativeOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-create-or-update-network-security-group-rule"></a>Ağ güvenlik grubu kuralı oluşturmak veya güncelleştirmek için etkinlik günlüğü uyarısının mevcut olduğundan emin olun

**Kimlik**: CIS Azure 5.2.4 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Belirli yönetim işlemleri için bir etkinlik günlüğü uyarısı bulunmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb954148f-4c11-4c38-8221-be76711e194a) |Bu ilke, hiçbir etkinlik günlüğü uyarısı yapılandırılmadığı belirli yönetim işlemlerini denetler. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_AdministrativeOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-create-or-update-or-delete-sql-server-firewall-rule"></a>SQL Server güvenlik duvarı kuralı oluşturmak veya güncelleştirmek ya da silmek için etkinlik günlüğü uyarısının bulunduğundan emin olun

**Kimlik**: CIS Azure 5.2.8 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Belirli yönetim işlemleri için bir etkinlik günlüğü uyarısı bulunmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb954148f-4c11-4c38-8221-be76711e194a) |Bu ilke, hiçbir etkinlik günlüğü uyarısı yapılandırılmadığı belirli yönetim işlemlerini denetler. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_AdministrativeOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-create-or-update-security-solution"></a>Güvenlik çözümü oluşturmak veya güncelleştirmek için etkinlik günlüğü uyarısının mevcut olduğundan emin olun

**Kimlik**: CIS Azure 5.2.6 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Belirli güvenlik işlemleri için bir etkinlik günlüğü uyarısı bulunmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3b980d31-7904-4bb7-8575-5665739a8052) |Bu ilke, hiçbir etkinlik günlüğü uyarısı yapılandırılmadığı belirli güvenlik işlemlerini denetler. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_SecurityOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-create-policy-assignment"></a>Ilke ataması oluşturmak için etkinlik günlüğü uyarısının mevcut olduğundan emin olun

**Kimlik**: CIS Azure 5.2.1 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Belirli Ilke işlemleri için bir etkinlik günlüğü uyarısı bulunmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc5447c04-a4d7-4ba8-a263-c9ee321a6858) |Bu ilke, hiçbir etkinlik günlüğü uyarısı yapılandırılmadığı belirli Ilke işlemlerini denetler. |Auditınotexists, devre dışı |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_PolicyOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-delete-network-security-group"></a>Ağ güvenlik grubunu silmek için etkinlik günlüğü uyarısının mevcut olduğundan emin olun

**Kimlik**: CIS Azure 5.2.3 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Belirli yönetim işlemleri için bir etkinlik günlüğü uyarısı bulunmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb954148f-4c11-4c38-8221-be76711e194a) |Bu ilke, hiçbir etkinlik günlüğü uyarısı yapılandırılmadığı belirli yönetim işlemlerini denetler. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_AdministrativeOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-delete-security-solution"></a>Güvenlik çözümünü silmek için etkinlik günlüğü uyarısının mevcut olduğundan emin olun

**Kimlik**: CIS Azure 5.2.7 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Belirli güvenlik işlemleri için bir etkinlik günlüğü uyarısı bulunmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3b980d31-7904-4bb7-8575-5665739a8052) |Bu ilke, hiçbir etkinlik günlüğü uyarısı yapılandırılmadığı belirli güvenlik işlemlerini denetler. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_SecurityOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-the-delete-network-security-group-rule"></a>Ağ güvenlik grubu kuralını Sil kuralı için etkinlik günlüğü uyarısının mevcut olduğundan emin olun

**Kimlik**: CIS Azure 5.2.5 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Belirli yönetim işlemleri için bir etkinlik günlüğü uyarısı bulunmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb954148f-4c11-4c38-8221-be76711e194a) |Bu ilke, hiçbir etkinlik günlüğü uyarısı yapılandırılmadığı belirli yönetim işlemlerini denetler. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_AdministrativeOperations_Audit.json) |

### <a name="ensure-that-activity-log-alert-exists-for-update-security-policy"></a>Güncelleştirme Güvenlik Ilkesi için etkinlik günlüğü uyarısının mevcut olduğundan emin olun

**Kimlik**: CIS Azure 5.2.9 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Belirli güvenlik işlemleri için bir etkinlik günlüğü uyarısı bulunmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3b980d31-7904-4bb7-8575-5665739a8052) |Bu ilke, hiçbir etkinlik günlüğü uyarısı yapılandırılmadığı belirli güvenlik işlemlerini denetler. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_SecurityOperations_Audit.json) |

### <a name="ensure-that-activity-log-retention-is-set-365-days-or-greater"></a>Etkinlik günlüğü saklama 'nın 365 gün veya daha büyük bir süre önce ayarlandığından emin olun

**Kimlik**: CIS Azure 5.1.2 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Etkinlik günlüğü en az bir yıl bekletilecektir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb02aacc0-b073-424e-8298-42b22829ee0a) |Bu ilke, bekletme 365 gün veya süresiz olarak ayarlanmamışsa etkinlik günlüğünü denetler (bekletme günleri 0 olarak ayarlanır). |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLogRetention_365orGreater.json) |

### <a name="ensure-that-logging-for-azure-keyvault-is-enabled"></a>Azure Anahtar Kasası günlüğü 'nün ' Enabled ' olduğundan emin olun

**Kimlik**: CIS Azure 5.1.7 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Key Vault tanılama günlükleri etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fcf820ca0-f99e-4f3e-84fb-66e913812d21) |Tanılama günlüklerini etkinleştirme denetimi. Bu, bir güvenlik olayı gerçekleştiğinde veya ağınızın güvenliği tehlikeye düştüğünde araştırma amaçlarıyla kullanılacak etkinlik izlerini yeniden oluşturmayı sağlar |Auditınotexists, devre dışı |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/KeyVault_AuditDiagnosticLog_Audit.json) |

### <a name="ensure-the-log-profile-captures-activity-logs-for-all-regions-including-global"></a>Günlük profilinin genel dahil tüm bölgeler için etkinlik günlüklerini yakaladığından emin olun

**Kimlik**: CIS Azure 5.1.4 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure Izleyici, tüm bölgelerdeki etkinlik günlüklerini toplamalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F41388f1c-2db0-4c25-95b2-35d7f5ccbfa9) |Bu ilke, genel dahil olmak üzere tüm Azure desteklenen bölgelerden etkinlikleri dışarı aktarmayan Azure Izleyici günlük profilini denetler. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_CaptureAllRegions.json) |

### <a name="ensure-the-storage-account-containing-the-container-with-activity-logs-is-encrypted-with-byok-use-your-own-key"></a>Etkinlik günlükleriyle kapsayıcıyı içeren depolama hesabının BYOK ile şifrelendiğinden emin olun (kendi anahtarınızı kullanın)

**Kimlik**: CIS Azure 5.1.6 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Etkinlik günlükleri içeren kapsayıcıyı içeren depolama hesabı BYOK ile şifrelenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffbb99e8e-e444-4da0-9ff1-75c92f5a85b2) |Bu ilke, kapsayıcıyı etkinlik günlükleriyle içeren depolama hesabının BYOK ile şifrelenip şifrelenmediğini denetler. İlke yalnızca depolama hesabı tasarım tarafından etkinlik günlükleri ile aynı abonelikte yer alıyorsa geçerlidir. Rest 'te Azure depolama şifrelemesi hakkında daha fazla bilgi burada bulunabilir [https://aka.ms/azurestoragebyok](https://aka.ms/azurestoragebyok) .  |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Monitoring/ActivityLog_StorageAccountBYOK_Audit.json) |

## <a name="networking"></a>Ağ

### <a name="ensure-that-network-watcher-is-enabled"></a>Ağ Izleyicisi 'nin ' etkin ' olduğundan emin olun

**Kimlik**: cıs Azure 6,5 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Ağ Izleyicisi etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb6e2945c-0b7b-40f5-9233-7a5323b5cdc6) |Ağ Izleyicisi, Azure 'daki ve Azure 'dan bir ağ senaryosu düzeyinde koşulları izlemenizi ve tanılamanıza olanak tanıyan bölgesel bir hizmettir. Senaryo düzeyi izleme, bir uçtan uca ağ düzeyi görünümündeki sorunları tanılamanıza olanak sağlar. Ağ Izleyicisi ile kullanılabilen ağ tanılama ve görselleştirme araçları, Azure 'da ağınızı anlamanıza, tanılamanıza ve elde etmenize yardımcı olur. |Auditınotexists |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkWatcher_Enabled_Audit.json) |

### <a name="ensure-that-rdp-access-is-restricted-from-the-internet"></a>RDP erişiminin internet 'ten kısıtlanmasını sağlayın

**Kimlik**: cıs Azure 6,1 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Internet 'ten RDP erişimi engellenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe372f825-a257-4fb8-9175-797a8a8627d6) |Bu ilke, Internet 'ten RDP erişimine izin veren tüm ağ güvenlik kuralları 'nı denetler |Denetim, devre dışı |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkSecurityGroup_RDPAccess_Audit.json) |

### <a name="ensure-that-ssh-access-is-restricted-from-the-internet"></a>SSH erişiminin internet 'ten kısıtlanmasını sağlayın

**Kimlik**: cıs Azure 6,2 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Internet 'ten SSH erişimi engellenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2c89a2e5-7285-40fe-afe0-ae8654b92fab) |Bu ilke, Internet 'ten SSH erişimine izin veren tüm ağ güvenlik kuralları 'nı denetler |Denetim, devre dışı |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkSecurityGroup_SSHAccess_Audit.json) |

## <a name="other-security-considerations"></a>Güvenlik Hakkında Diğer Önemli Noktalar

### <a name="enable-role-based-access-control-rbac-within-azure-kubernetes-services"></a>Azure Kubernetes Hizmetleri içinde rol tabanlı erişim denetimi 'ni (RBAC) etkinleştirme

**Kimlik**: cıs Azure 8,5 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Rol tabanlı Access Control (RBAC), Kubernetes hizmetlerinde kullanılmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fac4a19c2-fa67-49b4-8ae5-0b2e78c49457) |Kullanıcıların gerçekleştirebileceği eylemlere ayrıntılı filtreleme sağlamak için, Kubernetes hizmet kümelerinde izinleri yönetmek ve ilgili yetkilendirme ilkelerini yapılandırmak için rol tabanlı Access Control (RBAC) kullanın. |Denetim, devre dışı |[1.0.1-Önizleme](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_EnableRBAC_KubernetesService_Audit.json) |

### <a name="ensure-the-key-vault-is-recoverable"></a>Anahtar kasasının kurtarılabilir olduğundan emin olun

**Kimlik**: cıs Azure 8,4 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Key Vault nesneler kurtarılabilir olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0b60c0b2-2dc2-4e1c-b5c9-abbed971de53) |Bu ilke, Anahtar Kasası nesnelerinin kurtarılabilir olup olmadığını denetler. Geçici silme özelliği, belirli bir bekletme dönemi (90 gün) için bir SILME işleminden sonra bile kaynakları etkin bir şekilde tutmaya yardımcı olur. ' Korumayı Temizle ' açık olduğunda, 90 günlük bekletme süresi geçene kadar bir kasa veya silinen durumundaki bir nesne temizlenemiyor. Bu kasalar ve nesneler yine de kurtarılabilir, bu da bekletme ilkesi takip edilecek. |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Key%20Vault/KeyVault_Recoverable_Audit.json) |

## <a name="security-center"></a>Güvenlik Merkezi

### <a name="ensure-asc-default-policy-setting-enable-next-generation-firewallngfw-monitoring-is-not-disabled"></a>"Sonraki nesil güvenlik duvarı 'Nı etkinleştir (NGFW) Izlemenin" "devre dışı" olarak Izlenmesi için ASC varsayılan ilke ayarını doğrulayın

**Kimlik**: cıs Azure 2,9 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Internet 'e yönelik sanal makineler ağ güvenlik gruplarıyla korunmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ff6de0be7-9a8a-4b8a-b349-43cf02d22f7c) |Ağ güvenlik grupları (NSG) ile erişimi kısıtlayarak sanal makinelerinizi olası tehditlere karşı koruyun. NSG 'ler ile trafiği denetleme hakkında daha fazla bilgi edinin[https://aka.ms/nsg-doc](https://aka.ms/nsg-doc) |Auditınotexists, devre dışı |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_NetworkSecurityGroupsOnInternetFacingVirtualMachines_Audit.json) |
|[Alt ağlar bir ağ güvenlik grubuyla ilişkilendirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe71308d3-144b-4262-b144-efdc3cc90517) |Ağ güvenlik grubu (NSG) ile erişimi kısıtlayarak alt ağınızı olası tehditlere karşı koruyun. NSG 'ler, alt ağınıza ağ trafiğine izin veren veya reddeden Access Control listesi (ACL) kurallarının bir listesini içerir. |Auditınotexists, devre dışı |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_NetworkSecurityGroupsOnSubnets_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-adaptive-application-whitelisting-is-not-disabled"></a>"Uyarlamalı uygulama beyaz listesini Izleme" için ASC varsayılan ilke ayarının "devre dışı" olmadığından emin olun

**Kimlik**: cıs Azure 2,13 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Güvenli uygulamalar için beyaz listeye yönelik Uyarlamalı uygulama denetimleri, makinelerinizde etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F47a6b606-51aa-4496-8bb7-64b11cf66adc) |Makinelerinizde çalışan bilinen güvenli uygulamaların listesini tanımlamak için uygulama denetimlerini etkinleştirin ve diğer uygulamalar çalıştırıldığında sizi uyarır. Bu, makinelerinizi kötü amaçlı yazılımlara karşı korumanıza yardımcı olur. Güvenlik Merkezi, kurallarınızı yapılandırma ve sürdürme sürecini basitleştirmek için makine öğrenimini kullanarak her makinede çalışan uygulamaları analiz eder ve bilinen güvenli uygulamalar listesini önerir. |Auditınotexists, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_AppWhitelistMonitoring_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-disk-encryption-is-not-disabled"></a>"Izleyici disk şifrelemesi" nin "devre dışı" olarak, ASC varsayılan ilke ayarını sağlayın

**Kimlik**: cıs Azure 2,6 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Disk şifrelemesi sanal makinelere uygulanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0961003e-5a0a-4549-abde-af6a37f2724d) |Etkin disk şifrelemesi olmayan VM 'Ler, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_UnencryptedVMDisks_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-endpoint-protection-is-not-disabled"></a>"Monitor Endpoint Protection" varsayılan ilke ayarının "devre dışı" olmadığından emin olun

**Kimlik**: cıs Azure 2,5 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure Güvenlik Merkezi 'nde eksik Endpoint Protection izleme](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faf6cd1bd-1635-48cb-bde7-5b15693900b9) |Yüklü bir Endpoint Protection Aracısı olmayan sunucular, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_MissingEndpointProtection_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-jit-network-access-is-not-disabled"></a>"JıT ağ erişimini Izleme" için ASC varsayılan ilke ayarının "devre dışı" olmadığından emin olun

**Kimlik**: cıs Azure 2,12 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Sanal makinelerin yönetim bağlantı noktaları, tam zamanında ağ erişim denetimiyle korunmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb0f33259-77d7-4c9e-aac6-3aabcfae693c) |Olası ağ tam zamanında (JıT) erişim, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir |Auditınotexists, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_JITNetworkAccess_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-network-security-groups-is-not-disabled"></a>"Ağ güvenlik gruplarını Izleme" için ASC varsayılan ilke ayarının "devre dışı" olmadığından emin olun

**Kimlik**: cıs Azure 2,7 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Uyarlamalı ağ sağlamlaştırma önerileri internet 'e yönelik sanal makinelere uygulanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F08e6af2d-db70-460a-bfe9-d5bd474ba9d6) |Azure Güvenlik Merkezi, Internet 'e yönelik sanal makinelerin trafik düzenlerini analiz eder ve olası saldırı yüzeyini azaltan ağ güvenlik grubu kuralı önerileri sağlar |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_AdaptiveNetworkHardenings_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-os-vulnerabilities-is-not-disabled"></a>"Monitor OS güvenlik açıklarını Izleme", "devre dışı" olarak ASC varsayılan ilke ayarını doğrulayın

**Kimlik**: cıs Azure 2,4 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe1e5fd5d-3e4c-4ce1-8661-7d1873ae6b15) |Yapılandırılmış temeli karşılamayan sunucular, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_OSVulnerabilities_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-sql-auditing-is-not-disabled"></a>"SQL denetimini Izle" için ASC varsayılan ilke ayarının "devre dışı" olmadığından emin olun

**Kimlik**: cıs Azure 2,14 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[SQL Server üzerinde denetim etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fa6fb4358-5bf4-4ad7-ba82-2cd2f41ce5e9) |SQL Server denetim, sunucudaki tüm veritabanları genelinde veritabanı etkinliklerini izlemek ve bir denetim günlüğüne kaydetmek için etkinleştirilmelidir. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlServerAuditing_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-sql-encryption-is-not-disabled"></a>"Izleyici SQL şifrelemeyi" varsayılan ilke ayarının "devre dışı" olmadığından emin olun

**Kimlik**: cıs Azure 2,15 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[SQL veritabanlarındaki Saydam Veri Şifrelemesi etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F17k78e20-9358-41c9-923c-fb736d382a12) |Rest verilerini korumak ve uyumluluk gereksinimlerini karşılamak için saydam veri şifrelemesi etkinleştirilmelidir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/SqlDBEncryption_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-system-updates-is-not-disabled"></a>"Sistem güncelleştirmelerini Izleme" için ASC varsayılan ilke ayarının "devre dışı" olmadığından emin olun

**Kimlik**: cıs Azure 2,3 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Makinelerinize sistem güncelleştirmeleri yüklenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F86b3d65f-7626-441e-b690-81a8b71cff60) |Sunucularınızdaki eksik güvenlik sistemi güncelleştirmeleri, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_MissingSystemUpdates_Audit.json) |

### <a name="ensure-asc-default-policy-setting-monitor-vulnerability-assessment-is-not-disabled"></a>"Monitor güvenlik açığı değerlendirmesi" için ASC varsayılan ilke ayarının "devre dışı" olmadığından emin olun

**Kimlik**: cıs Azure 2,10 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F760a85ff-6162-42b3-8d70-698e268f648c) |Azure Güvenlik Merkezi 'nde güvenlik açığı değerlendirmesi çözümü olmadan güvenlik açığı değerlendirme çözümü ve VM 'Ler tarafından algılanan güvenlik açıklarını izler. |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_VMVulnerabilities_Audit.json) |

### <a name="ensure-that-automatic-provisioning-of-monitoring-agent-is-set-to-on"></a>' İzleme aracısının otomatik sağlanması ' öğesinin ' on ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 2,2 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Log Analytics izleme aracısının otomatik olarak sağlanması aboneliğinizde etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F475aae12-b88a-4572-8b36-9b712b2b3a17) |Güvenlik verilerini toplamak için Log Analytics izleme aracısının otomatik sağlamasını etkinleştirin |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_Automatic_provisioning_log_analytics_monitoring_agent.json) |

### <a name="ensure-that-security-contact-emails-is-set"></a>' Güvenlik iletişim e-postalarının ' ayarlandığından emin olun

**Kimlik**: cıs Azure 2,16 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Aboneliğiniz için bir güvenlik iletişim e-posta adresi belirtilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F4f4f78b8-e367-4b10-a341-d9a4ad5cf1c7) |Azure Güvenlik Merkezi, güvenliği aşılmış kaynaklar algıladığında bildirim almak için bir e-posta adresi girin |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_Security_contact_email.json) |

### <a name="ensure-that-security-contact-phone-number-is-set"></a>Güvenlik kişisinin ' telefon numarası ' ayarlandığından emin olun

**Kimlik**: cıs Azure 2,17 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Aboneliğiniz için bir güvenlik ilgili kişisi telefon numarası belirtilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fb4d66858-c922-44e3-9566-5cdb7a7be744) |Azure Güvenlik Merkezi, güvenliği aşılmış kaynaklar algıladığında bildirim almak için bir telefon numarası girin |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_Security_contact_phone_number.json) |

### <a name="ensure-that-send-email-also-to-subscription-owners-is-set-to-on"></a>' E-postayı abonelik sahiplerine Gönder ' ' ın ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 2,19 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Yüksek önem derecesi uyarıları için abonelik sahibine e-posta bildirimi etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0b15565f-aa9e-48ba-8619-45960f2c314d) |Güvenlik uyarılarını Microsoft 'tan gelen güvenlik uyarısı e-postalarını almak için abonelik sahibine gönderme özelliğini etkinleştirin. Bu, olası güvenlik sorunlarından haberdar olmanızı sağlar ve riski zamanında azaltabilirsiniz |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_Email_notification_to_subscription_owner.json) |

### <a name="ensure-that-send-email-notification-for-high-severity-alerts-is-set-to-on"></a>' Yüksek önem derecesi uyarıları için e-posta bildirimi gönder ' ' in ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 2,18 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Yüksek önem derecesine sahip uyarılar için e-posta bildirimi etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F6e2593d9-add6-4083-9c9b-4b7d2188c899) |Güvenlik, Microsoft 'un güvenlik uyarısı e-postalarını almasını sağlamak için güvenlik ekibine güvenli hale getirmek için etkinleştirin. Bu, doğru kişilerin olası güvenlik sorunlarının farkında olmasını sağlar ve riskleri hafifletiyor |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_Email_notification.json) |

## <a name="storage-accounts"></a>Depolama Hesapları

### <a name="ensure-default-network-access-rule-for-storage-accounts-is-set-to-deny"></a>Depolama hesapları için varsayılan ağ erişim kuralının Reddet olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 3,7 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Depolama hesapları, ağ erişimini kısıtlıyor olmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F34c877ad-507e-4c82-993e-3452a6e0ad3c) |Depolama hesaplarına ağ erişimi kısıtlanmalıdır. Ağ kurallarını, yalnızca izin verilen ağların uygulamalarının depolama hesabına erişebilmesi için yapılandırın. Belirli bir internet veya şirket içi istemcilerden gelen bağlantılara izin vermek için, belirli Azure sanal ağlarından veya genel İnternet IP adresi aralıklarına yönelik trafiğe erişim verilebilir |Denetim, reddetme, devre dışı |[1.1.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_NetworkAcls_Audit.json) |

### <a name="ensure-that-secure-transfer-required-is-set-to-enabled"></a>' Güvenli aktarım gerekli ' ' Enabled ' olarak ayarlandığından emin olun

**Kimlik**: cıs Azure 3,1 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Depolama hesaplarına güvenli aktarım etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F404c3081-a854-4457-ae30-26a93ef643f9) |Depolama hesabınızda güvenli aktarım gereksinimini denetleyin. Güvenli aktarım, depolama hesabınızı yalnızca güvenli bağlantılardan (HTTPS) istekleri kabul edecek şekilde zorlayan bir seçenektir. HTTPS kullanımı, sunucu ile hizmet arasında kimlik doğrulaması sağlar ve geçiş sırasında ortadaki adam, gizlice dinleme ve oturum ele geçirme gibi ağ katmanı saldırılarına karşı verileri korur |Denetim, reddetme, devre dışı |[2.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/Storage_AuditForHTTPSEnabled_Audit.json) |

### <a name="ensure-trusted-microsoft-services-is-enabled-for-storage-account-access"></a>Depolama hesabı erişimi için ' güvenilen Microsoft Hizmetleri 'nin etkinleştirildiğinden emin olun

**Kimlik**: cıs Azure 3,8 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Depolama hesapları güvenilen Microsoft hizmetlerinden erişime izin verilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc9d007d0-c057-4772-b18c-01e546713bcd) |Depolama hesaplarıyla etkileşime geçen bazı Microsoft Hizmetleri, ağ kuralları aracılığıyla erişim izni verilebilen ağlardan çalışır. Bu tür bir hizmetin istendiği şekilde çalışmasını sağlamak için, güvenilir Microsoft hizmetlerinin ağ kurallarını atlamasına izin verin. Bu hizmetler daha sonra depolama hesabına erişmek için güçlü kimlik doğrulaması kullanacaktır. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Storage/StorageAccess_TrustedMicrosoftServices_Audit.json) |

## <a name="virtual-machines"></a>Sanal Makineler

### <a name="ensure-that-data-disks-are-encrypted"></a>' Veri diskleri ' ' nin şifrelendiğinden emin olun

**Kimlik**: cıs Azure 7,2 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Disk şifrelemesi sanal makinelere uygulanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0961003e-5a0a-4549-abde-af6a37f2724d) |Etkin disk şifrelemesi olmayan VM 'Ler, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_UnencryptedVMDisks_Audit.json) |

### <a name="ensure-that-only-approved-extensions-are-installed"></a>Yalnızca onaylanan uzantıların yüklü olduğundan emin olun

**Kimlik**: cıs Azure 7,4 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Yalnızca onaylanan VM uzantıları yüklenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc0e996f8-39cf-4af9-9f45-83fbde810432) |Bu ilke onaylanmamış sanal makine uzantılarını yönetir. |Denetim, reddetme, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Compute/VirtualMachines_ApprovedExtensions_Audit.json) |

### <a name="ensure-that-os-disk-are-encrypted"></a>' İşletim sistemi diski 'nin şifrelendiğinden emin olun

**Kimlik**: cıs Azure 7,1 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Disk şifrelemesi sanal makinelere uygulanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0961003e-5a0a-4549-abde-af6a37f2724d) |Etkin disk şifrelemesi olmayan VM 'Ler, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_UnencryptedVMDisks_Audit.json) |

### <a name="ensure-that-the-endpoint-protection-for-all-virtual-machines-is-installed"></a>Tüm sanal makineler için Endpoint Protection 'ın yüklü olduğundan emin olun

**Kimlik**: cıs Azure 7,6 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Azure Güvenlik Merkezi 'nde eksik Endpoint Protection izleme](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Faf6cd1bd-1635-48cb-bde7-5b15693900b9) |Yüklü bir Endpoint Protection Aracısı olmayan sunucular, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_MissingEndpointProtection_Audit.json) |

### <a name="ensure-that-the-latest-os-patches-for-all-virtual-machines-are-applied"></a>Tüm sanal makineler için en son işletim sistemi düzeltme eklerinin uygulandığından emin olun

**Kimlik**: cıs Azure 7,5 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Makinelerinize sistem güncelleştirmeleri yüklenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F86b3d65f-7626-441e-b690-81a8b71cff60) |Sunucularınızdaki eksik güvenlik sistemi güncelleştirmeleri, Azure Güvenlik Merkezi tarafından öneriler olarak izlenir |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Security%20Center/ASC_MissingSystemUpdates_Audit.json) |

### <a name="ensure-that-unattached-disks-are-encrypted"></a>' Eklenmemiş disklerin ' de şifrelendiğinden emin olun

**Kimlik**: cıs Azure 7,3 **sahipliği**: müşteri

|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[Eklenmemiş diskler şifrelenmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F2c89a2e5-7285-40fe-afe0-ae8654b92fb2) |Bu ilke, şifrelemeden önce eklenmemiş tüm diskleri denetler. |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Compute/UnattachedDisk_Encryption_Audit.json) |

> [!NOTE]
> Belirli Azure Ilke tanımlarının kullanılabilirliği, Azure Kamu ve diğer ulusal bulutlarda farklılık gösterebilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Ilkesi ile ilgili ek makaleler:

- [Mevzuata uyumluluğuna](../concepts/regulatory-compliance.md) genel bakış.
- [Girişim tanımı yapısına](../concepts/initiative-definition-structure.md)bakın.
- [Azure ilke örneklerinde](./index.md)diğer örnekleri gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](../how-to/remediate-resources.md)öğrenin.

---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/10/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 4a055c5fe6aaa31324eb91c57debda8ee18e7bfd
ms.sourcegitcommit: 43558caf1f3917f0c535ae0bf7ce7fe4723391f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90022090"
---
|Name<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[MySQL sunucuları için kendi anahtar veri korumanızı getir özelliği etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F83cef61d-dbd1-4b20-a4fc-5fbc7da10833) |Bu ilke, kendi anahtar veri korumasını etkin hale getirmeden ortamınızdaki MySQL sunucularını denetler. Daha fazla ayrıntı için, adresini ziyaret edin [https://aka.ms/mysqlbyok](https://aka.ms/mysqlbyok) . |Auditınotexists, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MySQL_EnableByok_Audit.json) |
|[MySQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fe802a67a-daf5-4436-9ea6-f6d821dd0c5d) |Bu ilke, SSL bağlantısını zorunlu olmayan tüm MySQL Server 'ı denetler. MySQL için Azure veritabanı, MySQL Server için Azure veritabanı sunucunuzu Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamalarına bağlamayı destekler. Veritabanı sunucunuz ile istemci uygulamalarınız arasında SSL bağlantılarını zorunlu tutma, sunucu ile uygulamanız arasındaki veri akışını şifreleyerek, ortadaki adam saldırılarına karşı korunmaya yardımcı olur. |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MySQL_EnableSSL_Audit.json) |
|[MySQL için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F82339799-d096-41ae-8538-b108becf0970) |Bu ilke, coğrafi olarak yedekli yedekleme özellikli MySQL için Azure veritabanı 'nı denetler. |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/GeoRedundant_DBForMySQL_Audit.json) |
|[MySQL sunucusu bir sanal ağ hizmeti uç noktası kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3375856c-3824-4e0e-ae6a-79e011dd4c47) |Bu ilke, bir sanal ağ hizmeti uç noktası kullanmak üzere yapılandırılmamış MySQL sunucularını denetler. Daha fazla ayrıntı için, adresini ziyaret edin [https://aka.ms/mysqlvnet](https://aka.ms/mysqlvnet) . |Auditınotexists, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MySQL_VirtualNetworkServiceEndpoint_Audit.json) |
|[MySQL sunucuları için özel uç nokta etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F7595c971-233d-4bcf-bd18-596129188c49) |Bu ilke, özel bir uç nokta kullanmak üzere yapılandırılmamış MySQL sunucularını denetler. Daha fazla ayrıntı için, adresini ziyaret edin [https://aka.ms/mysqlprivatelink](https://aka.ms/mysqlprivatelink) . |Auditınotexists, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MySQL_EnablePrivateEndPoint_Audit.json) |
|[MySQL sunucuları için genel ağ erişimi devre dışı bırakılmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fd9844e8a-1437-4aeb-a32c-0c992f056095) |Bu ilke, ortamınızda ortak ağ erişimi etkin olan MySQL sunucularını denetler. Daha fazla ayrıntı için, adresini ziyaret edin [https://go.microsoft.com/fwlink/?linkid=2120014](https://go.microsoft.com/fwlink/?linkid=2120014) . |Denetim, devre dışı |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MySQL_DisablePublicNetworkAccess_Audit.json) |

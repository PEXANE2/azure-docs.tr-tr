---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 11/17/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 47cded1df30ede858698c6b01c5126279c549af2
ms.sourcegitcommit: c2dd51aeaec24cd18f2e4e77d268de5bcc89e4a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94746600"
---
|Ad<br /><sub>(Azure portal)</sub> |Description |Efekt (ler) |Sürüm<br /><sub>GitHub</sub> |
|---|---|---|---|
|[MariaDB için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0ec47710-77ff-4a3d-9181-6aa50af424d0) |MariaDB için Azure veritabanı, veritabanı sunucunuz için artıklık seçeneğini seçmenize olanak sağlar. Verilerin yalnızca sunucunuzun barındırıldığı bölgede depolanmadığı, ancak aynı zamanda bir bölge hatası durumunda kurtarma seçeneği sağlamak için eşlenmiş bir bölgeye çoğaltılan coğrafi olarak yedekli bir yedekleme depolamasına ayarlanabilir. Yedekleme için coğrafi olarak yedekli depolamayı yapılandırmaya yalnızca sunucu oluşturma sırasında izin verilir. |Denetim, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/GeoRedundant_DBForMariaDB_Audit.json) |
|[MariaDB sunucusu bir sanal ağ hizmeti uç noktası kullanmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fdfbd9a64-6114-48de-a47d-90574dc2e489) |Sanal ağ tabanlı güvenlik duvarı kuralları, trafiğin Azure sınırları içinde kalmasını sağlarken, belirli bir alt ağdan MariaDB için Azure veritabanı 'na trafik sağlamak üzere kullanılır. Bu ilke, MariaDB için Azure veritabanı 'nın, kullanılmakta olan sanal ağ hizmeti uç noktası olup olmadığını denetlemek için bir yol sağlar. |Auditınotexists, devre dışı |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MariaDB_VirtualNetworkServiceEndpoint_Audit.json) |
|[MariaDB sunucuları için özel uç nokta etkinleştirilmelidir](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F0a1302fb-a631-4106-9753-f3d494733990) |Özel uç nokta bağlantıları, MariaDB için Azure veritabanı 'na özel bağlantı etkinleştirerek güvenli iletişim uygular. Yalnızca bilinen ağlardan gelen trafiğe erişimi etkinleştirmek ve Azure içinde olmak üzere diğer tüm IP adreslerinden erişimi engellemek için özel bir uç nokta bağlantısı yapılandırın. |Auditınotexists, devre dışı |[1.0.2](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MariaDB_EnablePrivateEndPoint_Audit.json) |
|[MariaDB sunucuları için genel ağ erişimi devre dışı bırakılmalıdır](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Ffdccbe47-f3e3-4213-ad5d-ea459b2fa077) |Ortak ağ erişimi özelliğinin devre dışı bırakılması, MariaDB için Azure veritabanınızın yalnızca özel bir uç noktadan erişilebilmesini sağlayarak güvenliği geliştirir. Bu yapılandırma, Azure IP aralığı dışındaki tüm ortak adres alanından erişimi tamamen devre dışı bırakır ve IP veya sanal ağ tabanlı güvenlik duvarı kurallarıyla eşleşen tüm oturum açmaları reddeder. |Denetim, devre dışı |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/SQL/MariaDB_DisablePublicNetworkAccess_Audit.json) |

---
title: Mobil uygulamalar& PaaS web güvenliğini sağlama
titleSuffix: Azure App Service
description: 'PaaS web ve mobil uygulamalarınızın güvenliğini sağlamak için Azure Uygulama Hizmeti güvenliği en iyi uygulamaları hakkında bilgi edinin. '
services: security
documentationcenter: na
author: techlake
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2019
ms.author: terrylan
ms.openlocfilehash: c3f3c7fbaa043a03b70ab770c06e493716c70daf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77500274"
---
# <a name="best-practices-for-securing-paas-web-and-mobile-applications-using-azure-app-service"></a>Azure Uygulama Hizmeti'ni kullanarak PaaS web ve mobil uygulamaları güvence altına almak için en iyi uygulamalar

Bu makalede, PaaS web ve mobil uygulamalarınızın güvenliğini sağlamak için [Azure App Service](/azure/app-service/overview) güvenlik en iyi uygulamaları bir koleksiyon tartışmak. Bu en iyi uygulamalar Azure ile olan deneyimlerimizden ve sizin gibi müşterilerin deneyimlerinden elde edilir.

Azure Uygulama Hizmeti, herhangi bir platform veya cihaz için web ve mobil uygulamalar oluşturmanıza ve her yerde, bulutta veya şirket içinde verilere bağlanmanıza olanak tanıyan hizmet olarak bir platform (PaaS) teklifidir. Uygulama Hizmeti, daha önce Azure Web Siteleri ve Azure Mobil Hizmetleri olarak ayrı olarak teslim edilen web ve mobil özellikleri içerir. Ayrıca iş süreçlerini otomatikleştirmek ve bulut API'leri barındırmak için yeni özellikler içerir. Tek bir entegre hizmet olarak, App Service web, mobil ve tümleştirme senaryolarına zengin bir yetenek kümesi getirir.

## <a name="authenticate-through-azure-active-directory-ad"></a>Azure Etkin Dizin (AD) aracılığıyla kimlik doğrulaması
App Service, kimlik sağlayıcınız için bir OAuth 2.0 hizmeti sağlar. OAuth 2.0, web uygulamaları, masaüstü uygulamaları ve cep telefonları için özel yetkilendirme akışları sağlarken istemci geliştirici basitliğine odaklanır. Azure AD, mobil ve web uygulamalarına erişimi yetkilendirmenizi sağlamak için OAuth 2.0'ı kullanır. Daha fazla bilgi edinmek için [Azure Uygulama Hizmeti'nde Kimlik Doğrulama ve yetkilendirme'ye](../../app-service/overview-authentication-authorization.md)bakın.

## <a name="restrict-access-based-on-role"></a>Role göre erişimi kısıtlama
Veri erişimi için güvenlik ilkeleri uygulamak isteyen kuruluşlar için erişimi kısıtlamak zorunludur. Kullanıcılara, gruplara ve uygulamalara belirli bir kapsamda izin atamak için rol tabanlı erişim denetimi (RBAC) kullanabilirsiniz( örneğin, bilme ve en az ayrıcalık güvenlik ilkeleri gereksinimi gibi. Kullanıcılara uygulamalara erişim izni verme hakkında daha fazla bilgi edinmek için [rol tabanlı erişim denetimi nedir'e](/azure/role-based-access-control/overview)bakın.

## <a name="protect-your-keys"></a>Anahtarlarınızı koruyun
Abonelik anahtarlarınızı kaybetmeniz in ne kadar iyi olduğu önemli değildir. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Key Vault ile anahtarları ve sırları (kimlik doğrulama anahtarları, depolama hesap anahtarları, veri şifreleme anahtarları gibi) şifreleyebilirsiniz. PFX dosyaları ve parolaları) donanım güvenlik modülleri (HSM) tarafından korunan anahtarları kullanarak. Ek güvenlik için HSM'lerde anahtarları içeri aktarabilir veya oluşturabilirsiniz. TLS sertifikalarınızı otomatik yenileme ile yönetmek için Key Vault'u da kullanabilirsiniz. Daha fazla bilgi edinmek için [Azure Key Vault nedir'](../../key-vault/key-vault-overview.md) de bakın.

## <a name="restrict-incoming-source-ip-addresses"></a>Gelen kaynak IP adreslerini kısıtlama
[Uygulama Hizmet Ortamları,](../../app-service/environment/intro.md) gelen kaynak IP adreslerini ağ güvenlik grupları (NSG'ler) aracılığıyla kısıtlamanıza yardımcı olan bir sanal ağ tümleştirme özelliğine sahiptir. Azure Sanal Ağları (VNETs) bilmiyorsanız, bu özellik Azure kaynaklarınızın çoğunu erişimi kontrol ettiğiniz internet olmayan, rahatsız edici bir ağa yerleştirmenize olanak tanıyan bir özelliktir. Daha fazla bilgi için [bkz.](../../app-service/web-sites-integrate-with-vnet.md)

Windows'daki Uygulama Hizmeti için, web.config'i yapılandırarak IP adreslerini dinamik olarak kısıtlayabilirsiniz. Daha fazla bilgi için [Dinamik IP Security'ye](/iis/configuration/system.webServer/security/dynamicIpSecurity/)bakın.


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, PaaS web ve mobil uygulamaları nızı güvence altına almak için App Service güvenlik en iyi uygulamaları bir koleksiyon tanıttı. PaaS dağıtımlarınızın güvenliğini sağlama hakkında daha fazla bilgi edinmek için bkz:

- [PaaS dağıtımlarının güvenliğini sağlama](paas-deployments.md)
- [Azure'da PaaS veritabanlarının güvenliğini sağlama](paas-applications-using-sql.md)

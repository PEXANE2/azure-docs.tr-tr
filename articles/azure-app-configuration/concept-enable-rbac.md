---
title: Azure Active Directory kullanarak Azure Uygulama yapılandırmasına erişim yetkisi verme
description: Azure RBAC 'yi etkinleştirerek Azure uygulama yapılandırma örneğinize erişim yetkisi verin
author: AlexandraKemperMS
ms.author: alkemper
ms.date: 05/26/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: f29be1807dfcc314c89d30301107670a970263ce
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102172884"
---
# <a name="authorize-access-to-azure-app-configuration-using-azure-active-directory"></a>Azure Active Directory kullanarak Azure Uygulama yapılandırmasına erişim yetkisi verme
Karma tabanlı İleti Kimlik Doğrulama Kodu (HMAC) kullanmanın yanı sıra Azure Uygulama yapılandırması, istekleri uygulama yapılandırma örneklerine yetkilendirmek için Azure Active Directory (Azure AD) kullanımını destekler.  Azure AD, bir güvenlik sorumlusu için izin vermek üzere Azure rol tabanlı erişim denetimi (Azure RBAC) kullanmanıza olanak tanır.  Bir güvenlik sorumlusu, bir Kullanıcı, yönetilen bir [kimlik](../active-directory/managed-identities-azure-resources/overview.md) veya [uygulama hizmeti sorumlusu](../active-directory/develop/app-objects-and-service-principals.md)olabilir.  Roller ve rol atamaları hakkında daha fazla bilgi edinmek için bkz. [farklı rolleri anlama](../role-based-access-control/overview.md).

## <a name="overview"></a>Genel Bakış
Bir güvenlik sorumlusu tarafından bir uygulama yapılandırma kaynağına erişmek için yapılan isteklerin yetkilendirilmiş olması gerekir. Azure AD ile bir kaynağa erişim iki adımlı bir işlemdir:
1. Güvenlik sorumlusunun kimliği doğrulanır ve bir OAuth 2,0 belirteci döndürülür.  Belirteç istemek için kaynak adı, `https://login.microsoftonline.com/{tenantID}` `{tenantID}` hizmet sorumlusunun ait olduğu Azure ACTIVE DIRECTORY kiracı kimliğiyle aynıdır.
2. Belirteç, belirtilen kaynağa erişim yetkisi vermek için uygulama yapılandırma hizmetine yapılan bir isteğin bir parçası olarak geçirilir.

Kimlik doğrulama adımı, bir uygulama isteğinin çalışma zamanında bir OAuth 2,0 erişim belirteci içermesi gerekir.  Bir uygulama Azure Işlevleri uygulaması, bir Azure Web uygulaması veya bir Azure VM gibi bir Azure varlığı içinde çalışıyorsa, kaynaklara erişmek için yönetilen bir kimlik kullanabilir.  Azure Uygulama yapılandırmasına yönetilen bir kimlik tarafından yapılan isteklerin nasıl doğrulanalınacağını öğrenmek için bkz. Azure [kaynakları için Azure Active Directory ve yönetilen kimlikler Ile Azure uygulama yapılandırma kaynaklarına erişim kimlik doğrulaması](howto-integrate-azure-managed-service-identity.md).

Yetkilendirme adımı, güvenlik sorumlusuna bir veya daha fazla Azure rolünün atanmasını gerektirir. Azure Uygulama yapılandırması, uygulama yapılandırma kaynakları için izin kümelerini çevreleyen Azure rolleri sağlar. Bir güvenlik sorumlusu 'na atanan roller, sorumlu için belirtilen izinleri tespit edilir. Azure rolleri hakkında daha fazla bilgi için bkz. Azure [uygulama yapılandırması Için Azure yerleşik rolleri](#azure-built-in-roles-for-azure-app-configuration). 

## <a name="assign-azure-roles-for-access-rights"></a>Erişim hakları için Azure rolleri atama
Azure Active Directory (Azure AD), [Azure rol tabanlı erişim denetimi (Azure RBAC)](../role-based-access-control/overview.md)aracılığıyla güvenli kaynaklara erişim hakları verir.

Azure AD güvenlik sorumlusuna bir Azure rolü atandığında Azure, bu güvenlik sorumlusu için bu kaynaklara erişim izni verir. Erişim, uygulama yapılandırma kaynağına göre kapsamlandırılır. Azure AD güvenlik sorumlusu, bir kullanıcı veya bir uygulama hizmeti sorumlusu ya da [Azure kaynakları için yönetilen bir kimlik](../active-directory/managed-identities-azure-resources/overview.md)olabilir.

## <a name="azure-built-in-roles-for-azure-app-configuration"></a>Azure Uygulama yapılandırması için Azure yerleşik rolleri
Azure, Azure AD ve OAuth kullanarak uygulama yapılandırma verilerine erişim yetkisi veren aşağıdaki Azure yerleşik rollerini sağlar:

- **Uygulama yapılandırma verileri sahibi**: uygulama yapılandırma verilerine okuma/yazma/silme erişimi sağlamak için bu rolü kullanın. Bu, uygulama yapılandırma kaynağına erişim vermez.
- **Uygulama yapılandırma veri okuyucusu**: uygulama yapılandırma verilerine okuma erişimi sağlamak için bu rolü kullanın. Bu, uygulama yapılandırma kaynağına erişim vermez.
- **Katkıda bulunan**: uygulama yapılandırma kaynağını yönetmek için bu rolü kullanın. Uygulama yapılandırma verilerine erişim tuşları kullanılarak erişilebilirken, bu rol Azure AD kullanarak verilere doğrudan erişim vermez.
- **Okuyucu**: uygulama yapılandırma kaynağına okuma erişimi sağlamak için bu rolü kullanın. Bu, kaynağın erişim anahtarlarına veya uygulama yapılandırmasında depolanan verilere erişim vermez.

## <a name="next-steps"></a>Sonraki adımlar
Uygulama yapılandırma hizmetinizi yönetmek için [Yönetilen kimlikler](howto-integrate-azure-managed-service-identity.md) kullanma hakkında daha fazla bilgi edinin.

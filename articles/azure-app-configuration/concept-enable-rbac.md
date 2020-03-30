---
title: Azure Etkin Dizini'ni kullanarak Azure Uygulama Yapılandırmasına erişimi yetkilendirme
description: Azure Uygulama Yapılandırma örneğinize erişimi yetkilendirmek için RBAC'ı etkinleştirin
author: lisaguthrie
ms.author: lcozzens
ms.date: 02/13/2020
ms.topic: conceptual
ms.service: azure-app-configuration
ms.openlocfilehash: 18fa1b60b15b7eef96efa8dcc4fbf9cd7c4dc7f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77472628"
---
# <a name="authorize-access-to-azure-app-configuration-using-azure-active-directory"></a>Azure Etkin Dizini'ni kullanarak Azure Uygulama Yapılandırmasına erişimi yetkilendirme
Azure Uygulama Yapılandırması, istekleri Uygulama Yapılandırma örneklerine yetkilendirmek için Azure Active Directory 'i (Azure AD) kullanmayı destekler.  Azure AD, bir güvenlik ilkesine izin vermek için rol tabanlı erişim denetimini (RBAC) kullanmanıza olanak tanır.  Bir güvenlik ilkesi bir kullanıcı veya bir [uygulama hizmeti ilkesi](../active-directory/develop/app-objects-and-service-principals.md)olabilir.  Roller ve rol atamaları hakkında daha fazla bilgi edinmek için [farklı rolleri anlama](../role-based-access-control/overview.md)'ya bakın.

## <a name="overview"></a>Genel Bakış
Bir Uygulama Yapılandırması kaynağına erişmek için güvenlik sorumlusu (kullanıcı veya uygulama) tarafından yapılan isteklere izin verilmelidir.  Azure AD ile kaynağa erişim iki adımlı bir işlemdir.
1. Güvenlik sorumlusunun kimliği doğrulandı ve bir OAuth 2.0 belirteci döndürüldü.  Belirteç istemek için kaynak `https://login.microsoftonline.com/{tenantID}` `{tenantID}` adı, hizmet yöneticisinin ait olduğu Azure Etkin Dizin kiracı kimliğiyle eşleştiği yerdir.
2. Belirteç, belirtilen kaynağa erişim yetkisi vermek için Uygulama Yapılandırma hizmetine yapılan isteğin bir parçası olarak geçirilir.

Kimlik doğrulama adımı, bir uygulama isteğinin çalışma zamanında bir OAuth 2.0 erişim belirteci içermesini gerektirir.  Bir uygulama Azure İşlevler uygulaması, Azure Web Uygulaması veya Azure VM gibi bir Azure varlığı içinde çalışıyorsa, kaynaklara erişmek için yönetilen bir kimliği kullanabilir.  Azure Uygulama Yapılandırması'nda yönetilen bir kimlik tarafından yapılan istekleri niçin doğrulayacağını öğrenmek için, [Azure Etkin Dizini ve Azure Kaynakları için yönetilen kimliklerle Azure Uygulama Konfigürasyon kaynaklarına kimlik doÄ](howto-integrate-azure-managed-service-identity.md)rama süre

Yetkilendirme adımı, bir veya daha fazla RBAC rolünün güvenlik ilkesine atanmasını gerektirir. Azure Uygulama Yapılandırması, Uygulama Yapılandırmakaynakları için izin kümelerini kapsayan RBAC rolleri sağlar. Bir güvenlik ilkesine atanan roller, müdüre verilen izinleri belirler. RBAC rolleri hakkında daha fazla bilgi için [Azure Uygulama Yapılandırması için Yerleşik RBAC rolleri'ne](#built-in-rbac-roles-for-azure-app-configuration)bakın. 

## <a name="assign-rbac-roles-for-access-rights"></a>Erişim hakları için RBAC rollerini atama
Azure Etkin Dizin (Azure AD), rol tabanlı [erişim denetimi (RBAC)](../role-based-access-control/overview.md)aracılığıyla güvenli kaynaklara erişim haklarına izin vermektedir.

Bir Azure REKLAM güvenlik ilkesine bir RBAC rolü atandığında, Azure bu güvenlik ilkesi için bu kaynaklara erişim sağlar. Erişim, Uygulama Yapılandırması kaynağına göre dir. Azure AD güvenlik ilkesi bir kullanıcı veya uygulama hizmeti ilkesi veya [Azure kaynakları için yönetilen](../active-directory/managed-identities-azure-resources/overview.md)bir kimlik olabilir.

## <a name="built-in-rbac-roles-for-azure-app-configuration"></a>Azure Uygulama Yapılandırması için yerleşik RBAC rolleri
Azure, Azure AD ve OAuth kullanarak Uygulama Yapılandırma verilerine erişim yetkisi vermek için aşağıdaki yerleşik RBAC rollerini sağlar:

- Azure Uygulama Yapılandırma Veri Sahibi: Uygulama Yapılandırma kaynaklarına okuma/yazma erişimi sağlamak için bu rolü kullanın.
- Azure Uygulama Yapılandırma Veri Okuyucusu: Uygulama Yapılandırma kaynaklarına okuma erişimi sağlamak için bu rolü kullanın.
- Katılımcı: Yöneticiye Uygulama Yapılandırması örneğinde depolanan verilere erişim izni vermeden hizmete erişim sağlamak için bu rolü kullanın.

## <a name="next-steps"></a>Sonraki adımlar
Uygulama Yapılandırma hizmetinizi yönetmek için [yönetilen kimlikleri](howto-integrate-azure-managed-service-identity.md) kullanma hakkında daha fazla bilgi edinin.
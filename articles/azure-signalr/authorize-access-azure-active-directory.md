---
title: Azure Active Directory'ye erişimi yetkilendirme
description: Bu makalede, Azure Active Directory kullanarak Azure SignalR hizmeti kaynaklarına erişimi yetkilendirme hakkında bilgi sağlanır.
author: terencefan
ms.author: tefa
ms.date: 08/03/2020
ms.service: signalr
ms.topic: conceptual
ms.openlocfilehash: 37c2e41b5c81f941245b895ecd144baee3b9db6d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97797584"
---
# <a name="authorize-access-to-azure-signalr-service-resources-using-azure-active-directory"></a>Azure Active Directory kullanarak Azure SignalR hizmeti kaynaklarına erişim yetkisi verme
Azure SignalR hizmeti, Azure SignalR hizmeti kaynaklarına yönelik istekleri yetkilendirmek için Azure Active Directory (Azure AD) kullanımını destekler. Azure AD ile rol tabanlı erişim denetimi 'ni (RBAC), bir kullanıcı veya uygulama hizmeti sorumlusu olabilecek bir güvenlik sorumlusu için izin vermek üzere kullanabilirsiniz. Roller ve rol atamaları hakkında daha fazla bilgi edinmek için bkz. [farklı rolleri anlama](../role-based-access-control/overview.md).

## <a name="overview"></a>Genel Bakış
Bir güvenlik sorumlusu (bir uygulama) bir Azure SignalR hizmeti kaynağına erişmeyi denediğinde, isteğin yetkilendirilmiş olması gerekir. Azure AD ile bir kaynağa erişim iki adımlı bir işlemdir. 

 1. İlk olarak, güvenlik sorumlusunun kimliği doğrulanır ve bir OAuth 2,0 belirteci döndürülür. Belirteç istemek için kaynak adı `https://signalr.azure.com/` .
 2. Sonra, belirteç, belirtilen kaynağa erişimi yetkilendirmek için Azure SignalR hizmetine yapılan bir isteğin parçası olarak geçirilir.

Kimlik doğrulama adımı, bir uygulama isteğinin çalışma zamanında bir OAuth 2,0 erişim belirteci içermesi gerekir. Hub sunucunuz bir Azure VM 'si, bir sanal makine ölçek kümesi veya bir Azure Işlev uygulaması gibi bir Azure varlığı içinde çalışıyorsa, kaynaklara erişmek için yönetilen bir kimlik kullanabilir. Yönetilen bir kimlik tarafından Azure SignalR hizmetine yapılan isteklerin nasıl doğrulanalınacağını öğrenmek için bkz. [Azure kaynakları için Azure Active Directory ve yönetilen kimlikler Ile Azure SignalR hizmeti kaynaklarına erişim kimlik doğrulaması](authenticate-managed-identity.md). 

Yetkilendirme adımı, güvenlik sorumlusuna bir veya daha fazla RBAC rolünün atanmasını gerektirir. Azure SignalR hizmeti, Azure SignalR kaynakları için izin kümelerini çevreleyen RBAC rolleri sağlar. Bir güvenlik sorumlusu 'na atanan roller, sorumlunun sahip olacağı izinleri belirleyebilir. RBAC rolleri hakkında daha fazla bilgi için bkz. Azure [SignalR hizmeti Için Azure yerleşik rolleri](#azure-built-in-roles-for-azure-signalr-service). 

Bir Azure varlığı içinde çalışmayan SignalR hub sunucusu, Azure AD ile de yetki verebilir. Erişim belirteci isteme ve Azure SignalR hizmeti kaynakları için istekleri yetkilendirmek üzere kullanma hakkında bilgi edinmek için bkz. [bir uygulamadan Azure SignalR hizmetine erişim kimlik doğrulaması](authenticate-application.md). 

## <a name="azure-built-in-roles-for-azure-signalr-service"></a>Azure SignalR hizmeti için Azure yerleşik rolleri

- [SignalR App Server]
- [SignalR hizmeti okuyucusu]
- [SignalR hizmeti sahibi]

## <a name="assign-rbac-roles-for-access-rights"></a>Erişim hakları için RBAC rolleri atama
Azure Active Directory (Azure AD), [rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md)aracılığıyla güvenli kaynaklara erişim haklarını yetkilendirir. Azure SignalR hizmeti, Azure SignalR hizmetine erişmek için kullanılan ortak izin kümelerini çevreleyen Azure yerleşik rollerinin bir kümesini tanımlar ve kaynağa erişmek için özel roller de tanımlayabilir.

Azure AD güvenlik sorumlusuna bir RBAC rolü atandığında Azure, bu güvenlik sorumlusu için bu kaynaklara erişim izni verir. Erişim, abonelik düzeyi, kaynak grubu veya herhangi bir Azure SignalR hizmeti kaynağı kapsamına eklenebilir. Azure AD güvenlik sorumlusu, bir kullanıcı veya uygulama ya da [Azure kaynakları için yönetilen bir kimlik](../active-directory/managed-identities-azure-resources/overview.md)olabilir.

## <a name="built-in-roles-for-azure-signalr-service"></a>Azure SignalR hizmeti için yerleşik roller
Azure, Azure AD ve OAuth kullanarak Azure SignalR hizmeti kaynağına erişimi yetkilendirmek için aşağıdaki Azure yerleşik rollerini sağlar:

### <a name="signalr-app-server"></a>SignalR App Server

Erişim sağlamak için bu rolü kullanarak istemci belirteçleri imzalama için geçici bir erişim anahtarı sağlayın.

### <a name="signalr-serverless-contributor"></a>SignalR sunucusuz Katılımcısı

Azure SignalR hizmetinden doğrudan istemci belirteci almaya erişim sağlamak için bu rolü kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki ilgili makalelere bakın:

- [Azure SignalR hizmetine erişmek için Azure AD ile bir uygulamanın kimliğini doğrulama](authenticate-application.md)
- [Azure SignalR hizmetine erişmek için Azure AD ile yönetilen bir kimliğin kimliğini doğrulama](authenticate-managed-identity.md)
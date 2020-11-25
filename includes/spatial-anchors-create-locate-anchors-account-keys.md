---
author: msftradford
ms.service: azure-spatial-anchors
ms.topic: include
ms.date: 11/20/2020
ms.author: parkerra
ms.openlocfilehash: 7210b05566f5cd6f3c56792bce0904b3c9b645da
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95993080"
---
## <a name="set-up-authentication"></a>Kimlik doğrulamasını ayarlama

Hizmete erişmek için bir hesap anahtarı, erişim belirteci veya Azure Active Directory kimlik doğrulama belirteci sağlamanız gerekir. Bu konuda, [kimlik doğrulama kavramı sayfasında](../articles/spatial-anchors/concepts/authentication.md)daha fazla bilgi edinebilirsiniz.

### <a name="account-keys"></a>Hesap anahtarları

Hesap anahtarları, uygulamanızın Azure uzamsal bağlayıcı hizmeti ile kimlik doğrulamasına izin veren bir kimlik bilgileridir. Hesap anahtarlarının amaçlanan amacı hızla kullanmaya başlamanıza yardımcı olur. Özellikle uygulamanızın Azure uzamsal bağlayıcılarla tümleştirilmesine yönelik geliştirme aşamasında. Bu nedenle, geliştirme sırasında istemci uygulamalarınıza katıştırarak hesap anahtarlarını kullanabilirsiniz. Geliştirme ötesinde ilerleyerek, üretim düzeyi olan, erişim belirteçleri tarafından desteklenen bir kimlik doğrulama mekanizmasına veya Kullanıcı kimlik doğrulamasına Azure Active Directory geçmeniz önemle önerilir. Geliştirmeye yönelik bir hesap anahtarı almak için, Azure uzamsal bağlayıcı hesabınızı ziyaret edin ve "anahtarlar" sekmesine gidin.
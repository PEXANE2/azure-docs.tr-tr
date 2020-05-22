---
title: Azure AD kimlik doğrulamasını kullanmaya başlama
description: Azure Media Services API 'sini kullanmak için Azure Active Directory (Azure AD) kimlik doğrulamasına erişme hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 05/21/2020
ms.author: juliako
ms.openlocfilehash: 9788c4663908497b51fbaaf7f824125d857e7c81
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83774405"
---
# <a name="get-credentials-to-access-media-services-api"></a>Media Services API 'sine erişim için kimlik bilgilerini alın  

Azure Media Services API 'sine erişmek için Azure AD kimlik doğrulaması kullandığınızda iki kimlik doğrulama seçeneğiniz vardır:

- **Hizmet sorumlusu kimlik doğrulaması** (önerilir)

    Bir hizmetin kimliğini doğrulayın. Bu kimlik doğrulama yöntemini yaygın olarak kullanan uygulamalar, Daemon Hizmetleri, orta katman hizmetler veya zamanlanan işleri çalıştıran uygulamalardır: Web Apps, işlev uygulamaları, Logic Apps, API 'Ler veya mikro hizmet.
- **Kullanıcı kimlik doğrulaması**

    Media Services kaynaklarla etkileşim kurmak için uygulamayı kullanan bir kişinin kimliğini doğrulayın. Etkileşimli uygulama öncelikle kullanıcıdan kimlik bilgilerini istemelidir. Bu örnek, yetkili kullanıcılar tarafından kodlama işlerini veya canlı akışı izlemek için kullanılan bir yönetim konsolu uygulamasıdır. 

Bu makalede Media Services API 'sine erişmek için kimlik bilgilerini alma adımları açıklanır. Aşağıdaki sekmelerden birini seçin.

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure hesabı. Hesabınız yoksa [Azure Ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/)ile başlayın. 
- Bir Media Services hesabı. Daha fazla bilgi için [Azure Portal kullanarak Azure Media Services hesabı oluşturma](create-account-howto.md)konusuna bakın.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

### <a name="api-access"></a>API erişimi 

**API erişimi** sayfası, API 'ye bağlanmak için kullanmak istediğiniz kimlik doğrulama yöntemini seçmenizi sağlar. Bu sayfa, API 'ye bağlanmak için gereken değerleri de sağlar.

1. [Azure portal](https://portal.azure.com/)Media Services hesabınızı seçin.
2. Media Services API 'sine nasıl bağlanılacağını seçin.
3. **MEDIA SERVICES API 'Sine Bağlan**altında, bağlanmak ISTEDIĞINIZ Media Services API sürümünü seçin (v3 hizmetin en son sürümüdür).

### <a name="service-principal-authentication--recommended"></a>Hizmet sorumlusu kimlik doğrulaması (önerilir)

Bir Azure Active Directory (Azure AD) uygulaması ve gizli anahtarı kullanarak bir hizmetin kimliğini doğrular. Bu, Media Services API 'sine çağıran tüm orta katman hizmetleri için önerilir. Örnekler şunlardır Web Apps, Işlevler, Logic Apps, API 'Ler ve mikro hizmetlerdir. Bu, önerilen kimlik doğrulama yöntemidir.

#### <a name="manage-your-azure-ad-app-and-secret"></a>Azure AD uygulamanızı ve gizli anahtarı yönetme

**AAD uygulamanızı ve gizli anahtarı yönetme** bölümü, yeni BIR Azure AD uygulaması seçmenize veya oluşturmanıza ve gizli anahtar oluşturmanıza imkan tanır. Güvenlik nedeniyle, dikey pencere kapatıldıktan sonra gizli anahtar gösterilemez. Uygulama, medya Hizmetleri için geçerli bir belirteç almak üzere kimlik doğrulaması için uygulama KIMLIĞI ve gizli anahtarını kullanır.

Azure AD kiracınızla bir uygulamayı kaydetmek ve uygulamayı Azure aboneliğinizdeki bir role atamak için yeterli izinlere sahip olduğunuzdan emin olun. Daha fazla bilgi için bkz. [gerekli izinler](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions).

#### <a name="connect-to-media-services-api"></a>Media Services API 'sine bağlanma

**Media Services Connect API 'si** , hizmet sorumlusu uygulamanızı bağlamak için kullandığınız değerleri sağlar. Metin değerleri alabilir veya JSON veya XML bloklarını kopyalayabilirsiniz.

### <a name="user-authentication"></a>Kullanıcı kimlik doğrulaması

Bu seçenek, Media Services kaynaklarla etkileşim kurmak için uygulama kullanan bir Azure Active Directory çalışanın veya üyenin kimliğini doğrulamak için kullanılabilir. Etkileşimli uygulama öncelikle kullanıcıdan kullanıcının kimlik bilgilerini istemelidir. Bu kimlik doğrulama yöntemi yalnızca yönetim uygulamaları için kullanılmalıdır.

#### <a name="connect-to-media-services-api"></a>Media Services API 'sine bağlanma

Kullanıcı uygulamanızı **MEDIA SERVICES API 'ye Bağlan** bölümüne bağlamak için kimlik bilgilerinizi kopyalayın. Metin değerleri alabilir veya JSON veya XML bloklarını kopyalayabilirsiniz.

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

---

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Media Services v3 ile videoları karşıya yükleyin, kodlayın ve akışla](stream-files-tutorial-with-api.md).

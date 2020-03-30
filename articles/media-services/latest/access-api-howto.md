---
title: Azure AD kimlik doğrulaması ile başlayın
description: Azure Medya Hizmetleri API'sini tüketmek için Azure Etkin Dizin (Azure AD) kimlik doğrulaması'na nasıl eriştiğinizi öğrenin.
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
ms.date: 03/15/2020
ms.author: juliako
ms.openlocfilehash: 6e1ab30e8b4cf44f7d1f82fd94fb9bf854915557
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79478259"
---
# <a name="get-credentials-to-access-media-services-api"></a>Medya Hizmetleri API'sine erişmek için kimlik bilgileri alın  

Azure Medya Hizmetleri API'sine erişmek için Azure AD kimlik doğrulamasını kullandığınızda, iki kimlik doğrulama seçeneğiniz vardır:

- **Hizmet temel kimlik doğrulaması** (önerilir)

    Bir hizmeti doğrula. Bu kimlik doğrulama yöntemini yaygın olarak kullanan uygulamalar, daemon hizmetlerini, orta katman hizmetlerini veya zamanlanmış işleri çalıştıran uygulamalardır: web uygulamaları, işlev uygulamaları, mantık uygulamaları, API'ler veya bir mikro hizmet.
- **Kullanıcı kimlik doğrulaması**

    Medya Hizmetleri kaynaklarıyla etkileşimde kalmak için uygulamayı kullanan kişinin kimliğini doğrulatın. Etkileşimli uygulama öncelikle kullanıcıdan kimlik bilgileri için istekte olmalıdır. Bir örnek, kodlama işlerini veya canlı akışı izlemek için yetkili kullanıcılar tarafından kullanılan bir yönetim konsolu uygulamasıdır. 

Bu makalede, Medya Hizmetleri API erişmek için kimlik bilgileri almak için adımlar açıklanmaktadır. Aşağıdaki sekmelerden birini seçin.

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure hesabı. Hesabınız yoksa, [Azure ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/)başlayın. 
- Bir Media Services hesabı. Daha fazla bilgi için Azure [portalını kullanarak Bir Azure Medya Hizmetleri hesabı oluşturma 'na](create-account-howto.md)bakın.

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

### <a name="api-access"></a>API erişimi 

API erişim sayfası, **API'ye** bağlanmak için kullanmak istediğiniz kimlik doğrulama yöntemini seçmenize olanak tanır. Sayfa, API'ye bağlanmak için gereken değerleri de sağlar.

1. Azure [portalında](https://portal.azure.com/)Medya Hizmetleri hesabınızı seçin.
2. Medya Hizmetleri API'sine nasıl bağlanabileceğinizi seçin.
3. **Medya Hizmetlerine Bağlan API**altında, bağlanmak istediğiniz Medya Hizmetleri API sürümünü seçin (V3 hizmetin en son sürümüdür).

### <a name="service-principal-authentication--recommended"></a>Hizmet temel kimlik doğrulaması (önerilir)

Bir Azure Etkin Dizin (Azure AD) uygulamasını ve gizli hizmetini kullanarak bir hizmetin kimliğini doğrular. Bu, Medya Hizmetleri API'sini çağıran orta katman hizmetler için önerilir. Örnekler Web Apps, Fonksiyonlar, Mantık Uygulamaları, API'ler ve mikro hizmetlerdir. Bu önerilen kimlik doğrulama yöntemidir.

#### <a name="manage-your-azure-ad-app-and-secret"></a>Azure AD uygulamanızı ve gizli durumunuzu yönetin

**AAD uygulamanızı yönet ve gizli** bölümü, yeni bir Azure AD uygulamasını seçmenize veya oluşturmanıza ve bir sır oluşturmanıza olanak tanır. Güvenlik amacıyla, bıçak kapatıldıktan sonra gizli gösterilemez. Uygulama, medya hizmetleri için geçerli bir belirteç elde etmek için kimlik doğrulama için uygulama kimliği ve gizli kullanır.

Bir uygulamayı Azure AD kiracınıza kaydettirmek ve uygulamayı Azure aboneliğinizdeki bir role atamak için yeterli izine sahip olduğunuzdan emin olun. Daha fazla bilgi için [gerekli izinlere](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions)bakın.

#### <a name="connect-to-media-services-api"></a>Medya Hizmetleri API'sine bağlanın

**Medya Hizmetlerine Bağlantı API,** hizmet temel uygulamanızı bağlamak için kullandığınız değerleri sağlar. Metin değerlerini alabilir veya JSON veya XML bloklarını kopyalayabilirsiniz.

### <a name="user-authentication"></a>Kullanıcı kimlik doğrulaması

Bu seçenek, Medya Hizmetleri kaynaklarıyla etkileşimde bulunan bir uygulamayı kullanan bir çalışanın veya Azure Etkin Dizini üyesinin kimliğini doğrulamak için kullanılabilir. Etkileşimli uygulama öncelikle kullanıcının kimlik bilgilerini ister. Bu kimlik doğrulama yöntemi yalnızca Yönetim uygulamaları için kullanılmalıdır.

#### <a name="connect-to-media-services-api"></a>Medya Hizmetleri API'sine bağlanın

Kullanıcı uygulamanızı **Medya Hizmetlerine Bağlan API** bölümünden bağlamak için kimlik bilgilerinizi kopyalayın. Metin değerlerini alabilir veya JSON veya XML bloklarını kopyalayabilirsiniz.

[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

---

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Upload, kodlamak ve Medya Hizmetleri v3 ile videoları akışı](stream-files-tutorial-with-api.md).

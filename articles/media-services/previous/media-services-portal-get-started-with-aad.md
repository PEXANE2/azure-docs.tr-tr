---
title: Azure portalını kullanarak Azure AD kimlik doğrulaması ile başlayın| Microsoft Dokümanlar
description: Azure Medya Hizmetleri API'sini kullanmak için Azure Active Directory (Azure AD) kimlik doğrulaması'na erişmek için Azure portalını nasıl kullanacağınızı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: ee04fa7120f5510d703d72e662036f4fe952cd66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78330677"
---
# <a name="get-started-with-azure-ad-authentication-by-using-the-azure-portal"></a>Azure portalını kullanarak Azure AD kimlik doğrulamasını kullanmaya başlama

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Azure Medya Hizmetleri API'sine erişmek için Azure Active Directory (Azure AD) kimlik doğrulamasına erişmek için Azure portalını nasıl kullanacağınızı öğrenin.

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure hesabı. Hesabınız yoksa, [Azure ücretsiz deneme sürümüyle](https://azure.microsoft.com/pricing/free-trial/)başlayın. 
- Bir Media Services hesabı. Daha fazla bilgi için Azure [portalını kullanarak Bir Azure Medya Hizmetleri hesabı oluşturma 'na](media-services-portal-create-account.md)bakın.

Azure Media Hizmetleri ile Azure AD kimlik doğrulamasını kullandığınızda, iki kimlik doğrulama seçeneğiniz vardır:

- **Hizmet ana kimlik doğrulaması.** Bir hizmeti doğrula. Bu kimlik doğrulama yöntemini yaygın olarak kullanan uygulamalar, daemon hizmetlerini, orta katman hizmetlerini veya zamanlanmış işleri çalıştıran uygulamalardır: web uygulamaları, işlev uygulamaları, mantık uygulamaları, API'ler veya bir mikro hizmet.
- **Kullanıcı kimlik doğrulaması.** Medya Hizmetleri kaynaklarıyla etkileşimde kalmak için uygulamayı kullanan kişinin kimliğini doğrulatın. Etkileşimli uygulama öncelikle kullanıcıdan kimlik bilgileri için istekte olmalıdır. Bir örnek, kodlama işlerini veya canlı akışı izlemek için yetkili kullanıcılar tarafından kullanılan bir yönetim konsolu uygulamasıdır. 

## <a name="access-the-media-services-api"></a>Media Services API’sine erişim

Bu sayfa, API'ye bağlanmak için kullanmak istediğiniz kimlik doğrulama yöntemini seçmenize olanak tanır. Sayfa, API'ye bağlanmak için gereken değerleri de sağlar.

1. Azure [portalında](https://portal.azure.com/)Medya Hizmetleri hesabınızı seçin.
2. Medya Hizmetleri API'sine nasıl bağlanabileceğinizi seçin.
3. **Medya Hizmetlerine Bağlan API**altında, bağlanmak istediğiniz Medya Hizmetleri API sürümünü seçin.

## <a name="service-principal-authentication--recommended"></a>Hizmet temel kimlik doğrulaması (önerilir)

Bir Azure Etkin Dizin (Azure AD) uygulamasını ve gizli hizmetini kullanarak bir hizmetin kimliğini doğrular. Bu, Medya Hizmetleri API'sini çağıran orta katman hizmetler için önerilir. Örnekler Web Apps, Fonksiyonlar, Mantık Uygulamaları, API'ler ve mikro hizmetlerdir. Bu önerilen kimlik doğrulama yöntemidir.

### <a name="manage-your-azure-ad-app-and-secret"></a>Azure AD uygulamanızı ve gizli durumunuzu yönetin

**AAD uygulamanızı yönet ve gizli** bölümü, yeni bir Azure AD uygulamasını seçmenize veya oluşturmanıza ve bir sır oluşturmanıza olanak tanır. Güvenlik amacıyla, bıçak kapatıldıktan sonra gizli gösterilemez. Uygulama, medya hizmetleri için geçerli bir belirteç elde etmek için kimlik doğrulama için uygulama kimliği ve gizli kullanır.

Bir uygulamayı Azure AD kiracınıza kaydettirmek ve uygulamayı Azure aboneliğinizdeki bir role atamak için yeterli izine sahip olduğunuzdan emin olun. Daha fazla bilgi için [gerekli izinlere](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#required-permissions)bakın.

### <a name="connect-to-media-services-api"></a>Medya Hizmetleri API'sine bağlanın

**Medya Hizmetlerine Bağlantı API,** hizmet temel uygulamanızı bağlamak için kullandığınız değerleri sağlar. Metin değerlerini alabilir veya JSON veya XML bloklarını kopyalayabilirsiniz.

## <a name="user-authentication"></a>Kullanıcı kimlik doğrulaması

Bu seçenek, Medya Hizmetleri kaynaklarıyla etkileşimde bulunan bir uygulamayı kullanan bir çalışanın veya Azure Etkin Dizini üyesinin kimliğini doğrulamak için kullanılabilir. Etkileşimli uygulama öncelikle kullanıcının kimlik bilgilerini ister. Bu kimlik doğrulama yöntemi yalnızca Yönetim uygulamaları için kullanılmalıdır.

### <a name="connect-to-media-services-api"></a>Medya Hizmetleri API'sine bağlanın

Kullanıcı uygulamanızı **Medya Hizmetlerine Bağlan API** bölümünden bağlamak için kimlik bilgilerinizi kopyalayın. Metin değerlerini alabilir veya JSON veya XML bloklarını kopyalayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Hesabınıza dosya yüklemeye](media-services-portal-upload-files.md)başlayın.

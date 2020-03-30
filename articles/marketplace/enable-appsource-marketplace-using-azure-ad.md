---
title: Azure Active Directory kullanarak Microsoft AppSource ve Azure Marketi girişini etkinleştirin | Azure
description: Uygulama ve hizmet yayıncıları için Azure Marketi ve AppSource'ta Azure Active Directory'yi kullanarak bir giriş türünü etkinleştirin.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: dsindona
ms.openlocfilehash: 45855038e60dcdc3be4f98cfdceed69df5e8c946
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80286328"
---
# <a name="enable-an-appsource-and-marketplace-listing-by-using-azure-active-directory"></a>Azure Active Directory kullanarak AppSource ve Market listesini etkinleştirme

 Azure Active Directory (Azure AD), bir Microsoft hesabıyla kimlik doğrulamayı sağlayan bir bulut kimliği hizmetidir. Azure AD, endüstri standardı çerçeveler kullanır. [Azure Etkin Dizini hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/services/active-directory)

## <a name="azure-ad-benefits"></a>Azure AD avantajları

Microsoft AppSource ve Azure Marketi müşterileri, listeleme kataloglarında arama yapmak için ürün içi deneyimleri kullanır. Bu eylemler, müşterilerin üründe oturum açmasını gerektirir. Azure AD tümleştirmesi aşağıdaki avantajları sağlar:

- Daha hızlı etkileşim ve optimize edilmiş müşteri deneyimi
- Milyonlarca kurumsal kullanıcı için tek oturum açma (SSO)
- Farklı iş ortakları tarafından yayınlanan uygulamalar arasında tutarlı, oturum açma deneyimi
- Mobil ve bulut uygulamaları için ölçeklenebilir, platformlar arası kimlik doğrulaması

## <a name="offers-that-require-azure-ad"></a>Azure AD gerektiren teklifler

AppSource ve Azure Marketi için çeşitli [listeleme seçenekleri](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type) ve teklif türleri Azure AD uygulaması için farklı gereksinimlere sahiptir. Ayrıntılar için aşağıdaki tabloya bakın:

| **Teklif türü**    | **Azure AD SSO gerekli?**  |  |   |  |
| :------------------- | :-------------------|:-------------------|:-------------------|:-------------------|
|  | Bana Ulaşın | Deneme | Test Sürüşü | Transact |
| Sanal Makine | Yok | Hayır | Hayır | Hayır |
| Azure Uygulamaları (çözüm şablonu)  | Yok | Yok | Yok | Yok |
| Yönetilen Uygulamalar  | Yok | Yok | Yok | Hayır |
| SaaS  | Hayır | Evet | Evet | Evet |
| Kapsayıcılar  | Yok | Yok | Yok | Hayır |
| Danışmanlık Hizmetleri  | Hayır | Yok | Yok | Yok |

SaaS teknik gereksinimleri hakkında daha fazla bilgi için, [SaaS uygulamaları Teklif Yayın Kılavuzu'na](https://docs.microsoft.com/azure/marketplace/marketplace-saas-applications-technical-publishing-guide)bakın.

## <a name="azure-ad-integration"></a>Azure AD tümleştirmesi

- Azure AD'yi girişinize entegre ederek tek oturum açmayı etkinleştirme hakkında bilgi için [geliştiriciler için Azure Etkin Dizini'ne]( https://aka.ms/aaddev)bakın.
- Azure AD tek oturum açma hakkında daha fazla bilgi almak için [bkz.](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

## <a name="enable-a-trial-listing"></a>Deneme listesini etkinleştirme

Otomatik müşteri kurulumu dönüştürme olasılığını artırabilir. Müşteriniz deneme girişinizi seçtiğinde ve deneme ortamınıza yönlendirildiğinde, ek oturum açma adımlarına gerek kalmadan müşteriyi doğrudan ayarlayabilirsiniz.

Kimlik doğrulama sırasında Azure AD, uygulamanıza veya teklifinize bir belirteç gönderir. Belirteç tarafından sağlanan kullanıcı bilgileri, uygulamanızda veya teklifinizde bir kullanıcı hesabı oluşturulmasını sağlar. Daha fazla bilgi için [Örnek belirteçleri'ne](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)bakın.

Uygulamanızda veya deneme kaydınızda tek tıklamayla kimlik doğrulamasını etkinleştirmek için Azure AD'yi kullandığınızda şunları

- Pazar Yeri'nden deneme listenize kadar müşteri deneyimini kolaylaştırın.
- Kullanıcı Market'ten etki alanınıza veya deneme ortamınıza yönlendirildiğinde bile ürün içi deneyim hissini koruyun.
- Ek oturum açma adımları olmadığından, kullanıcılar yeniden yönlendirildiğinde terk edilme olasılığını azaltın.
- Azure AD kullanıcılarının büyük nüfusu için dağıtım engellerini azaltın.

## <a name="verify-azure-ad-integration"></a>Azure AD tümleştirmeyi doğrulama

### <a name="multitenant-solutions"></a>Çok kiracılı çözümler

Aşağıdaki eylemleri desteklemek için Azure AD'yi kullanın:

- Uygulamanızı Market vitrinlerinden birine kaydedin. Daha fazla bilgi için [Uygulama kaydını](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications) veya [AppSource sertifikasını](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified) görüntüleyin.
- Tek tıklamayla deneme deneyimi elde etmek için Azure AD'deki çoklu kira desteği özelliğini etkinleştirin.

Azure AD federe tek oturum açma yı kullanmaya yeni yseniz, şu adımları izleyin:

1. Uygulamanızı Market'e kaydedin.
1. [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) veya [OpenID Connect'i](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code)kullanarak Azure AD ile SSO geliştirin.
1. Tek tıklamayla deneme deneyimi sağlamak için Azure AD'deki çoklu hizmet desteği özelliğini etkinleştirin.

### <a name="single-tenant-solutions"></a>Tek kiracıçözümleri

Aşağıdaki eylemlerden birini desteklemek için Azure AD'yi kullanın:

- [Azure AD B2B'yi](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)kullanarak dizininize konuk kullanıcı ekleyin.
- **Bana Ulaşın** yayımlama seçeneğini kullanarak müşteriler için denemeleri el ile ayarlayın.
- Müşteri başına bir test sürüşü geliştirin.
- SSO kullanan çok kiracılı örnek bir demo uygulaması oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Marketi'ne kaydolduğunuzdan](https://azuremarketplace.microsoft.com/sell)emin olun.
- Teklifinizi nasıl oluşturabileceğiniz veya tamamlayabileceğiniz hakkında daha fazla bilgi için [İş Ortağı Merkezi hesabının nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) öğrenin.

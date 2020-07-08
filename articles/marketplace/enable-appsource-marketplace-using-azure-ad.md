---
title: Microsoft ticari Market teklifinizi Azure Active Directory tümleştirin
description: Microsoft AppSource ve Azure Market tekliflerinizin kimliğini doğrulamak için Azure Active Directory kullanın.
author: qianw211
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: dsindona
ms.openlocfilehash: e2fff55fd9161aa41411dcb1abbf8ae0c1bae49c
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85957843"
---
# <a name="integrate-your-commercial-marketplace-listing-with-azure-active-directory"></a>Ticari Market listelerinizi Azure Active Directory ile tümleştirin

 Bu makalede, Azure Active Directory (Azure AD) ile ticari bir market listesinin veya teklifinin tümleştirilmesine yönelik gereksinimler açıklanmaktadır. Azure AD, Microsoft hesabı kimlik doğrulamasını etkinleştirmek için sektör standardı çerçeveler kullanan bir bulut kimlik hizmetidir. [Azure Active Directory hakkında daha fazla bilgi edinin](https://azure.microsoft.com/services/active-directory).

## <a name="azure-ad-benefits"></a>Azure AD avantajları

Microsoft AppSource ve Azure Market müşterileri, storefront listeleme kataloglarında arama yapmak için ürün içi deneyimler kullanır. Bu eylemler, müşterilerin üründe oturum açmasını gerektirir. Azure AD tümleştirmesi aşağıdaki avantajları sağlar:

- Daha hızlı katılım ve iyileştirilmiş müşteri deneyimi
- Milyonlarca kurumsal Kullanıcı için çoklu oturum açma (SSO)
- Farklı iş ortakları tarafından yayımlanan uygulamalar arasında tutarlı, oturum açma deneyimi
- Mobil ve bulut uygulamaları için ölçeklenebilir, platformlar arası kimlik doğrulaması

## <a name="offers-that-require-azure-ad"></a>Azure AD gerektiren teklifler

Çeşitli ticari Market [Listeleme seçenekleri ve teklif türleri](determine-your-listing-type.md) , Azure AD uygulamasına yönelik farklı gereksinimlere sahiptir. Ayrıntılar için aşağıdaki tabloya bakın.

| Teklif türü    | Benimle Iletişim kurmak için Azure AD SSO 'SU gerekiyor mu?  | Deneme için Azure AD SSO gerekiyor mu? | Test sürücüsü için Azure AD SSO 'SU gerekiyor mu?  | Transact için Azure AD SSO gerekir |
| :------------------- | :-------------------|:-------------------|:-------------------|:-------------------|
| Sanal Makine | YOK | Hayır | Hayır | Hayır |
| Azure uygulamaları (çözüm şablonu)  | YOK | YOK | YOK | YOK |
| Yönetilen uygulamalar  | YOK | YOK | YOK | Hayır |
| SaaS  | Hayır | Evet | Yes | Yes |
| Kapsayıcılar  | YOK | YOK | YOK | Hayır |
| Danışmanlık Hizmetleri  | Hayır | YOK | YOK | YOK |

SaaS teknik gereksinimleri hakkında daha fazla bilgi için bkz. [SaaS uygulamaları Için Yayımlama Kılavuzu](marketplace-saas-applications-technical-publishing-guide.md).

## <a name="azure-ad-integration"></a>Azure AD tümleştirmesi

- Azure AD 'yi listeleyerek tümleştirerek çoklu oturum açmayı etkinleştirme hakkında bilgi için, bkz. [Azure Active Directory geliştiriciler](../active-directory/develop/index.yml).
- Azure AD çoklu oturum açma hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](../active-directory/manage-apps/what-is-single-sign-on.md).

## <a name="enable-a-trial-listing"></a>Deneme listesini etkinleştirme

Otomatik müşteri kurulumu dönüştürme olasılığını artırabilir. Müşteriniz deneme listesini seçtiğinde ve deneme ortamınıza yönlendirildiğinde, ek oturum açma adımları gerekmeden müşteriyi doğrudan ayarlayabilirsiniz.

Kimlik doğrulama sırasında Azure AD, uygulamanıza veya teklifinizi bir belirteç gönderir. Belirteç tarafından belirtilen kullanıcı bilgileri, uygulamanızda veya teklifinizdeki bir kullanıcı hesabının oluşturulmasına izin verilir. Daha fazla bilgi için bkz. [örnek belirteçleri](../active-directory/develop/id-tokens.md).

Uygulamanızda tek tıklamayla kimlik doğrulamayı etkinleştirmek için Azure AD kullandığınızda şunları yapabilirsiniz:

- Market 'ten deneme listeinize müşteri deneyimini kolaylaştırın.
- Kullanıcı Market 'ten etki alanı veya deneme ortamınıza yeniden yönlendirildiğinde bile, ürün içi deneyim hisini koruyun.
- Ek oturum açma adımları olmadığından, kullanıcılar yeniden yönlendirildiğinde terk olma olasılığını azaltın.
- Azure AD kullanıcılarının büyük popülasyonu için dağıtım engelleri azaltma.

## <a name="verify-azure-ad-integration"></a>Azure AD tümleştirmesini doğrulama

### <a name="multitenant-solutions"></a>Çok kiracılı çözümler

Aşağıdaki eylemleri desteklemek için Azure AD 'yi kullanın:

- Uygulamanızı Market stokörkinden birine kaydedin. Daha fazla bilgi için [uygulama kaydını](../active-directory/develop/quickstart-register-app.md) veya [appsource sertifikasını](../active-directory/azuread-dev/howto-get-appsource-certified.md) görüntüleyin.
- Tek tıklamayla deneme deneyimi almak için Azure AD 'de çok kiracılı destek özelliğini etkinleştirin.

Azure AD Federasyon çoklu oturum açma 'yı kullanmaya yeni başladıysanız şu adımları uygulayın:

1. Uygulamanızı Market 'e kaydedin.
1. [OAuth 2,0](../active-directory/azuread-dev/v1-protocols-oauth-code.md) veya [OpenID Connect](../active-directory/azuread-dev/v1-protocols-openid-connect-code.md)kullanarak Azure AD ile SSO geliştirin.
1. Tek tıklamayla deneme deneyimi sağlamak için Azure AD 'de çok kiracılı destek özelliğini etkinleştirin.

### <a name="single-tenant-solutions"></a>Tek kiracılı çözümler

Aşağıdaki eylemlerden birini desteklemek için Azure AD 'yi kullanın:

- [Azure AD B2B](../active-directory/b2b/what-is-b2b.md)kullanarak dizininize Konuk kullanıcı ekleyin.
- **İletişim** yayımlama yayınlaması seçeneğini kullanarak müşteriler için denemeleri el ile ayarlayın.
- Müşteri başına test sürücüsü geliştirin.
- SSO kullanan çok kiracılı örnek Tanıtım uygulaması oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Daha önce yapmadıysanız, 

- Market hakkında [bilgi edinin](https://azuremarketplace.microsoft.com/sell) .

Iş Ortağı Merkezi 'ne kaydolmak için yeni bir teklif oluşturmaya veya var olan bir teklifle çalışmaya başlayın:

- Teklifinizi oluşturmak veya tamamlayabilmeniz için [Iş Ortağı Merkezi ' nde oturum açın](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/partnership) .

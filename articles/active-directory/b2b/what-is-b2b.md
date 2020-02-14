---
title: Azure Active Directory B2B işbirliği nedir?
description: Azure Active Directory B2B işbirliği, kaynakları güvenli bir şekilde paylaşmak ve şirket dışındaki ortaklarla işbirliği yapmak için konuk kullanıcı erişimini destekler.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: overview
ms.date: 02/12/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: c5ccfb4719d14d0ce73caf093c5fe63631eda2a7
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77195224"
---
# <a name="what-is-guest-user-access-in-azure-active-directory-b2b"></a>Azure Active Directory B2B’de konuk kullanıcı erişimi nedir?

Azure Active Directory (Azure AD) işletmeler arası (B2B) işbirliği, şirketinizin uygulamalarını ve hizmetlerini sizinki haricinde herhangi bir kuruluştaki konuk kullanıcılarla güvenli bir şekilde paylaşmanıza ve bunu yaparken de kurumsal verileriniz üzerindeki denetiminizi sürdürmenize olanak sağlar. Azure AD veya bir BT departmanı olmasa da şirket dışındaki ortaklarla güvenli ve emniyetli şekilde çalışın. Basit bir davet ve kullanım işlemi, ortakların şirketinizin kaynaklarına erişmek için kendi kimliklerini kullanmasına olanak sağlar. Geliştiriciler, self servis kayıt portalları gibi uygulamalar yazmak veya davet işlemini özelleştirmek için Azure AD işletmeler arası API’leri kullanabilir.

Konuk kullanıcıları kendi kimliklerini kullanarak şirketinizin uygulama ve hizmetlerinde oturum açmaları için davet ederek onlarla nasıl güvenli şekilde işbirliği yapabileceğinizi öğrenmek için videoyu izleyin.

Aşağıdaki video, kullanışlı bir genel bakış sağlar.

>[!VIDEO https://www.youtube.com/embed/AhwrweCBdsc]

## <a name="collaborate-with-any-partner-using-their-identities"></a>Kimliklerini kullanarak herhangi bir ortak ile işbirliği yapma

Azure AD B2B ile ortaklar kendi kimlik yönetimi çözümlerini kullanır; bu nedenle kuruluşunuz için harici bir yönetim iş yükü söz konusu değildir.

- İş ortağı kendi kimliklerini ve kimlik bilgilerini kullanır. Azure AD gerekli değildir.
- Harici hesapları veya parolaları yönetmeniz gerekmez.
- Hesapları eşitlemeniz veya hesap yaşam döngülerini yönetmeniz gerekmez.  

![Üye Ekle sayfasını gösteren ekran görüntüsü](media/what-is-b2b/add-member.png)

## <a name="invite-guest-users-with-a-simple-invitation-and-redemption-process"></a>Basit bir davet ve kullanım işlemiyle konuk kullanıcıları davet etme

Konuk kullanıcılar, kendi iş, okul veya sosyal kimlikleriyle uygulama ve hizmetlerinizde oturum açar. Konuk kullanıcının bir Microsoft hesabı veya bir Azure AD hesabı yoksa, kullanıcılar davetlerini kullandığında kendileri için bir hesap oluşturulur. 

- Kendi tercih ettikleri e-posta kimliğini kullanarak konuk kullanıcıları davet edin.
- Bir uygulamaya doğrudan bağlantı gönderin veya konuk kullanıcının kendi Erişim Paneline bir davet gönderin.
- Konuk kullanıcılar, oturum açmak için birkaç basit kullanım adımını izler.

![İzinleri gözden geçir sayfasını gösteren ekran görüntüsü](media/what-is-b2b/consentscreen.png)

## <a name="use-policies-to-securely-share-your-apps-and-services"></a>Uygulama ve hizmetlerinizi güvenli bir şekilde paylaşmak için ilkeleri kullanma

Şirket içeriğinizi korumak için yetkilendirme ilkeleri kullanabilirsiniz. Multi-Factor Authentication gibi koşullu erişim ilkeleri zorlanabilir:

- Kiracı düzeyinde.
- Uygulama düzeyinde.
- Belirli konuk kullanıcıların kurumsal uygulamalarını ve verilerini koruması için.

![Koşullu erişim seçeneğini gösteren ekran görüntüsü](media/what-is-b2b/tutorial-mfa-policy-2.png)


## <a name="easily-add-guest-users-in-the-azure-ad-portal"></a>Azure AD portalda kolayca konuk kullanıcılar ekleme

Yönetici olarak, Azure portalda kuruluşunuza kolayca konuk kullanıcılar ekleyebilirsiniz.

- Azure AD’de, yeni bir kullanıcı ekler gibi yeni bir konuk kullanıcı oluşturun.
- Konuk kullanıcı hemen Erişim Panelinde oturum açmalarını sağlayan özelleştirilebilir bir davet alır.
- Dizindeki konuk kullanıcılar, uygulamalara veya gruplara atanabilir.  

![Yeni Konuk Kullanıcı daveti giriş sayfasını gösteren ekran görüntüsü](media/what-is-b2b/add-a-b2b-user-to-azure-portal.png)

## <a name="let-application-and-group-owners-manage-their-own-guest-users"></a>Uygulama ve grup sahiplerinin kendi konuk kullanıcılarını yönetmesine izin verme

Uygulama sahiplerinin, Microsoft uygulaması olsun olmasın, paylaşmak istedikleri herhangi bir uygulamaya doğrudan konuk kullanıcı ekleyebilmeleri için uygulama sahiplerine konuk kullanıcı yönetimi yetkisini devredebilirsiniz.

- Yöneticiler, self servis uygulama ve grup yönetimini ayarlar.
- Yönetici olmayan kullanıcılar, uygulamalara veya gruplara konuk kullanıcılar eklemek için [Erişim Paneli](https://myapps.microsoft.com)’ni kullanır.

![Konuk Kullanıcı için erişim panelini gösteren ekran görüntüsü](media/what-is-b2b/access-panel-manage-app.png)

## <a name="customize-the-onboarding-experience-for-b2b-guest-users"></a>B2B Konuk kullanıcıları için ekleme deneyimini özelleştirme

Kuruluşunuzun gereksinimlerine göre özelleştirilmiş şekilde şirket dışındaki ortaklar ekleyin.

- [Dış kullanıcılara erişimi yöneten](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-external-users#how-access-works-for-external-users)ilkeleri yapılandırmak IÇIN [Azure AD Yetkilendirme Yönetimi](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview) 'ni kullanın.
- Ekleme deneyimlerinizi özelleştirmek için [B2B işbirliği davetiyesi API 'lerini](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD B2B işbirliği için lisanslama yönergeleri](licensing-guidance.md)
- [Portalda B2B işbirliği konuk kullanıcıları ekleme](add-users-administrator.md)
- [Davet kullanımı işlemini anlama](redemption-experience.md)
- Ayrıca her zaman olduğu gibi, [Microsoft Teknik Topluluğumuz](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b) aracılığıyla geri bildirim, tartışmalar ve öneriler için ürün ekibiyle bağlantı kurun.

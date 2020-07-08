---
title: Office 365 dış paylaşım ve B2B işbirliği-Azure AD
description: O365 ve Azure Active Directory B2B işbirliği kullanan dış iş ortaklarıyla kaynakları paylaşmayı açıklar.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/24/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: fcb407735cff5a0c43d3a584ff37a0eabc178381
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85367453"
---
# <a name="office-365-external-sharing-and-azure-active-directory-azure-ad-b2b-collaboration"></a>Office 365 dış paylaşım ve Azure Active Directory (Azure AD) B2B işbirliği

Azure AD B2B işbirliği ve Office 365 dış paylaşımında (OneDrive, SharePoint Online, Birleşik Gruplar, vb.), dış kullanıcıların kimliği Azure AD B2B kullanılarak doğrulanır.

## <a name="how-does-azure-ad-b2b-differ-from-external-sharing-in-sharepoint-online"></a>Azure AD B2B, SharePoint Online 'daki dış paylaşımlardan farklı midir?

OneDrive/SharePoint Online 'ın ayrı bir davet Yöneticisi vardır. OneDrive/SharePoint Online 'da dış paylaşım desteği, Azure AD tarafından destek geliştirilerek başlatıldı. Zaman içinde, OneDrive/SharePoint Online dış paylaşımı, ürünün yerleşik paylaşım modelini kullanan birçok farklı özelliği ve milyonlarca kullanıcıyı tahakkuk etmiş. Ancak, OneDrive/SharePoint Online dış paylaşımının nasıl çalıştığı ve Azure AD B2B işbirliğinin nasıl çalıştığı arasında bazı hafif farklılıklar vardır. [Dış paylaşıma genel bakışta](https://docs.microsoft.com/sharepoint/external-sharing-overview)OneDrive/SharePoint Online dış paylaşımı hakkında daha fazla bilgi edinebilirsiniz. İşlem genellikle Azure AD B2B 'den şu yollarla farklılık gösterir:

- OneDrive/SharePoint Online, kullanıcılar davetlerini kullandıktan sonra kullanıcıları dizine ekler. Bu nedenle, kullanmadan önce kullanıcıyı Azure AD portalında görmezsiniz. Bu sırada başka bir site bir kullanıcıyı davet eder, yeni bir davet oluşturulur. Ancak, Azure AD B2B işbirliğini kullandığınızda kullanıcılar her yerde gösterilmesi için davet üzerine hemen eklenir.

- OneDrive/SharePoint Online 'daki kullanım deneyimi, Azure AD B2B işbirliğinin deneyiminden farklı görünüyor. Bir Kullanıcı bir daveti etkinleştirdikten sonra, deneyimler benzer şekilde görünür.

- Azure AD B2B işbirliği davet edilen kullanıcılar OneDrive/SharePoint Online Paylaşım iletişim kutularından alınabilir. OneDrive/SharePoint Online davet edilen kullanıcılar, davetlerini kullandıktan sonra Azure AD 'de de görünür.

- Lisanslama gereksinimleri farklılık gösterir. Ücretli her Azure AD lisansı için, en fazla 5 Konuk kullanıcının ücretli Azure AD özelliklerine erişmesini sağlayabilirsiniz. Lisanslama hakkında daha fazla bilgi edinmek için bkz. [Azure AD B2B lisanslama](https://docs.microsoft.com/azure/active-directory/b2b/licensing-guidance) ve ["Dış Kullanıcı nedir?", SharePoint Online dış paylaşımına genel bakış](https://docs.microsoft.com/sharepoint/external-sharing-overview#what-happens-when-users-share).

OneDrive/SharePoint Online 'da dış paylaşımı Azure AD B2B işbirliğiyle yönetmek için, OneDrive/SharePoint Online dış paylaşım ayarını **yalnızca kuruluşunuzun dizininde zaten var olan dış kullanıcılarla paylaşıma Izin verecek**şekilde ayarlayın. Kullanıcılar dışarıdan paylaşılan sitelere gidebilir ve yöneticinin eklediği dış ortak çalışanlarla seçim yapabilir. Yönetici, B2B işbirliği daveti API 'Leri aracılığıyla dış ortak çalışanları ekleyebilir.


![OneDrive/SharePoint Online dış paylaşım ayarı](media/o365-external-user/odsp-sharing-setting.png)

Dış paylaşımı etkinleştirdikten sonra, SharePoint Online (SPO) kişi seçicisindeki mevcut Konuk kullanıcıları arama özelliği, eski davranışa uyacak şekilde varsayılan olarak KAPALıDıR.

Kiracı ve site koleksiyonu düzeyinde ' ShowPeoplePickerSuggestionsForGuestUsers ' ayarını kullanarak bu özelliği etkinleştirebilirsiniz. Özelliği set-SPOTenant ve set-SPOSite cmdlet 'lerini kullanarak ayarlayabilirsiniz. Bu özellik, üyelerin dizindeki tüm mevcut Konuk kullanıcıları aramasını sağlar. Kiracı kapsamındaki değişiklikler zaten sağlanmış olan SPO sitelerini etkilemez.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Bir role B2B işbirliği kullanıcısı ekleme](add-guest-to-role.md)
* [B2B işbirliği davetlerini devretmek](delegate-invitations.md)
* [Dinamik Gruplar ve B2B işbirliği](use-dynamic-groups.md)
* [B2B işbirliği Azure Active Directory sorunlarını giderme](troubleshoot.md)

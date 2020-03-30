---
title: Office 365 dış paylaşım ve B2B işbirliği - Azure AD
description: O365 ve Azure Active Directory B2B işbirliğini kullanarak kaynakları dış iş ortaklarıyla paylaşmayı tartışır.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/24/2017
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d70aed6fbe0f09ea6284f913c88186ecf94e297
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74272264"
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Office 365 dış paylaşım ve Azure Active Directory B2B işbirliği

Office 365 (OneDrive, SharePoint Online, Unified Groups, vb.) ve Azure Active Directory (Azure AD) B2B işbirliğinde dış paylaşım teknik olarak aynıdır. Office 365 Grupları'ndaki konuklar da dahil olmak üzere tüm dış paylaşımlar (OneDrive/SharePoint Online hariç), paylaşım için Azure AD B2B işbirliği daveti API'lerini zaten kullanır.

## <a name="how-does-azure-ad-b2b-differ-from-external-sharing-in-sharepoint-online"></a>Azure AD B2B'nin SharePoint Online'daki dış paylaşımdan farkı nedir?

OneDrive/SharePoint Online'ın ayrı bir davet yöneticisi vardır. OneDrive/SharePoint Online'da dış paylaşım desteği, Azure AD desteğini geliştirmeden önce başladı. Zaman içinde OneDrive/SharePoint Online dış paylaşım, ürünün dahili paylaşım desenini kullanan birçok özellik ve milyonlarca kullanıcı kazanmıştır. Ancak, OneDrive/SharePoint Online dış paylaşımın nasıl çalıştığı ile Azure AD B2B işbirliğinin nasıl çalıştığı arasında bazı ince farklar vardır. [Dış paylaşıma genel bakışta](https://docs.microsoft.com/sharepoint/external-sharing-overview)OneDrive/SharePoint Online dış paylaşım hakkında daha fazla bilgi edinebilirsiniz. Bu işlem genellikle Azure AD B2B'den şu şekilde farklıdır:

- OneDrive/SharePoint Online, kullanıcılar davetlerini kullandıktan sonra kullanıcıları dizine ekler. Bu nedenle, kullanımdan önce kullanıcıyı Azure AD portalında göremezsinz. Bu arada başka bir site kullanıcıyı davet ederse, yeni bir davet oluşturulur. Ancak, Azure AD B2B işbirliğini kullandığınızda, kullanıcılar her yerde görünmeleri için davetüzerine hemen eklenir.

- OneDrive/SharePoint Online'daki kullanım deneyimi, Azure AD B2B işbirliğindeki deneyimden farklı görünüyor. Bir kullanıcı bir daveti kullandıktan sonra, deneyimler birbirine benzer.

- Azure AD B2B işbirliği davetlisi kullanıcılar OneDrive/SharePoint Online paylaşım iletişim kutularından seçilebilir. OneDrive/SharePoint Online davet edilen kullanıcılar, davetlerini kullandıktan sonra Azure AD'de de yer aldı.

- Lisans gereksinimleri farklıdır. Her ücretli Azure REKLAM lisansı için, en fazla 5 konuk kullanıcının ücretli Azure REKLAM özelliklerinize erişmelerine izin verebilirsiniz. Lisanslama hakkında daha fazla bilgi edinmek için [SharePoint Online dış paylaşımgenel bakışında](https://docs.microsoft.com/sharepoint/external-sharing-overview#what-happens-when-users-share) [Azure AD B2B lisansı](https://docs.microsoft.com/azure/active-directory/b2b/licensing-guidance) ve "Harici kullanıcı nedir?" konusuna bakın.

OneDrive/SharePoint Online'da Azure AD B2B işbirliği ile dış paylaşımı yönetmek için OneDrive/SharePoint Online dış paylaşım ayarını **yalnızca kuruluşunuzun dizininde zaten var olan harici kullanıcılarla paylaşmaya izin**verecek şekilde ayarlayın. Kullanıcılar harici olarak paylaşılan sitelere gidebilir ve yöneticinin eklediği harici ortak çalışanlararasından seçim yapabilir. Yönetici, B2B işbirliği daveti API'leri aracılığıyla harici işbirlikçileri ekleyebilir.


![OneDrive/SharePoint Online dış paylaşım ayarı](media/o365-external-user/odsp-sharing-setting.png)

Dış paylaşımı etkinleştirdikten sonra, SharePoint Online (SPO) kişi seçicide varolan konuk kullanıcıları arama olanağı varsayılan olarak eski davranışla eşleşecek şekilde KAPATILIR.

Bu özelliği, kiracı ve site koleksiyonu düzeyindeki 'ShowPeoplePickerSuggestionsForGuestUsers' ayarını kullanarak etkinleştirebilirsiniz. Bu özelliği, üyelerin dizindeki tüm mevcut konuk kullanıcıları aramasına olanak tanıyan Set-SPOTenant ve Set-SPOSite cmdlets'i kullanarak ayarlayabilirsiniz. Kiracı kapsamındaki değişiklikler zaten sağlanan SPO sitelerini etkilemez.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Bir role B2B işbirliği kullanıcısı ekleme](add-guest-to-role.md)
* [Temsilci B2B işbirliği davetiyeleri](delegate-invitations.md)
* [Dinamik gruplar ve B2B işbirliği](use-dynamic-groups.md)
* [Azure Active Directory B2B işbirliğinde sorun giderme](troubleshoot.md)

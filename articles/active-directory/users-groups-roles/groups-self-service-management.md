---
title: Self Servis Grup yönetimini ayarlama-Azure Active Directory | Microsoft Docs
description: Azure Active Directory'de (Azure AD) güvenlik grupları veya Office 365 grupları oluşturma ve yönetin ve güvenlik grubu veya Office 365 grup üyelikleri isteme
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 541deb5cf44ad5440e31641b673ed5da5b5d2b26
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81768551"
---
# <a name="set-up-self-service-group-management-in-azure-active-directory"></a>Azure Active Directory içinde Self Servis Grup yönetimini ayarlama 

Kullanıcıların Azure Active Directory (Azure AD) içinde kendi güvenlik gruplarını veya Office 365 gruplarını oluşturmasını ve yönetmesini sağlayabilirsiniz. Grubun sahibi üyelik isteklerini onaylayabilir veya reddedebilir ve grup üyeliği denetimini temsil edebilir. Self Servis Grup yönetimi özellikleri posta etkin güvenlik grupları veya dağıtım listeleri için kullanılamaz.

## <a name="self-service-group-membership-defaults"></a>Self Servis Grup üyeliği Varsayılanları

Azure portal veya Azure AD PowerShell kullanılarak güvenlik grupları oluşturulduğunda, yalnızca grubun sahipleri üyeliği güncelleştirebilir. [Erişim panelinde](https://account.activedirectory.windowsazure.com/r#/joinGroups) self servis tarafından oluşturulan güvenlik grupları ve tüm Office 365 grupları, sahip tarafından onaylanmış veya otomatik olarak onaylanan tüm kullanıcılara katılabilir. Erişim panelinde, grubu oluştururken üyelik seçeneklerini değiştirebilirsiniz.

İçinde oluşturulan gruplar | Güvenlik grubu varsayılan davranışı | Office 365 grup varsayılan davranışı
------------------ | ------------------------------- | ---------------------------------
[Azure AD PowerShell](groups-settings-cmdlets.md) | Üyeler yalnızca sahipler tarafından eklenebilir<br>Görünür ancak erişim paneli 'ne katılması için kullanılamaz | Tüm kullanıcılara katılması için aç
[Azure portal](https://portal.azure.com) | Üyeler yalnızca sahipler tarafından eklenebilir<br>Görünür ancak erişim paneli 'ne katılması için kullanılamaz<br>Grup oluşturma sırasında otomatik olarak sahip atanmadı | Tüm kullanıcılara katılması için aç
[Erişim paneli](https://account.activedirectory.windowsazure.com/r#/joinGroups) | Tüm kullanıcılara katılması için aç<br>Grup oluşturulduğunda üyelik seçenekleri değiştirilebilir | Tüm kullanıcılara katılması için aç<br>Grup oluşturulduğunda üyelik seçenekleri değiştirilebilir

## <a name="self-service-group-management-scenarios"></a>Self Servis Grup yönetimi senaryoları

* **Temsilci Grup Yönetimi** Örneğin, şirketin kullandığı SaaS uygulamasına erişimi yöneten bir yönetici. Bu erişim haklarını yönetmek sıkıcı bir hal alabilir; bu durumda yönetici, işletme sahibinden yeni bir grup oluşturmasını ister. Yönetici, uygulama için yeni gruba erişim atar ve uygulamaya daha önce erişen tüm kişilerin grubuna ekler. Ardından işletme sahibi daha fazla kullanıcı ekleyebilir ve bu kullanıcılar uygulamaya otomatik olarak sağlanır. İşletme sahibinin, yöneticinin kullanıcılar için erişimi yönetmesini beklemesi gerekmez. Yönetici farklı bir iş grubundaki bir yöneticiye aynı izni veriyorsa, bu kişi kendi grup üyelerine erişimi de yönetebilir. Ne iş sahibi ne de yönetici, her birinin grup üyeliklerini görüntüleyebilir veya yönetebilir. Yönetici uygulamaya erişimi olan tüm kullanıcıları görebilir ve gerekirse erişim haklarını engelleyebilir.
* **Self servis grup yönetimi** Bu senaryoya bir örnek, her ikisi de bağımsız olarak ayarladıkları SharePoint Online sitelerine sahip olan iki kullanıcıdır. Her birinin takımlarına sitelerine erişim izni vermek ister. Bunun için Azure AD'de bir grup oluşturabilirler ve her biri SharePoint Online'da kendi sitelerine erişim sunmak üzere bu grubu seçerler. Birisi erişim istediğinde Erişim Paneli'nden ister ve onayın ardından otomatik olarak iki SharePoint Online sitesine de erişim sağlanır. Daha sonra bu kişilerden biri, siteye erişen herkesin belirli bir SaaS uygulamasına da erişmesi gerektiğine karar verir. SaaS uygulamasının yöneticisi uygulamanın erişim haklarını SharePoint Online sitesine ekleyebilir. Daha sonra onaylanan tüm istekler, iki SharePoint Online sitesine ve bu SaaS uygulamasına erişim sağlar.

## <a name="make-a-group-available-for-user-self-service"></a>Bir grubu kullanıcı self servisi için kullanıma sunma

1. Dizin için genel yönetici olan bir hesapla [Azure AD yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
1. **Grupları**seçin ve ardından **genel** ayarlar ' ı seçin.
1. Set **Owners, erişim panelinde grup üyeliği Isteklerini** **Evet**olarak yönetebilir.
1. **Erişim panelindeki gruplara erişimi kısıtla '** yı **Hayır**olarak ayarlayın.
1. **Kullanıcıları, Azure portalları 'nda güvenlik grupları oluşturabilir** veya **Kullanıcılar Azure portalları 'nda Office 365 grupları oluşturabilir**

    - **Evet**: Azure AD kuruluşunuzdaki tüm kullanıcılara yeni güvenlik grupları oluşturma ve bu gruplara üye ekleme izni verilir. Bu yeni gruplar ayrıca diğer tüm kullanıcılar Erişim Paneli’nde gösterilir. Gruptaki ilke ayarı izin veriyorsa, diğer kullanıcılar bu gruplara katılması için istek oluşturabilir
    - **Hayır**: kullanıcılar grup oluşturamaz ve sahibi oldukları mevcut grupları değiştiremez. Ancak, bu grupların üyeliklerini yönetmeye ve diğer kullanıcıların gruplara katılma isteklerini onaylamaya devam edebilirler.

Ayrıca, kullanıcılarınız için Self Servis Grup yönetimi üzerinde daha ayrıntılı erişim denetimi elde **etmek üzere Azure** portalları ' nda Grup sahipleri olarak üye atayabilecek **sahipler** ' i de kullanabilirsiniz.

Kullanıcılar grupları oluştur, kuruluşunuzdaki tüm kullanıcıların yeni gruplar oluşturmasına ve sonra varsayılan sahip olarak bu gruplara üye eklemesine izin verilir. Kendi gruplarını oluşturabileceğiniz bireyler belirtemezsiniz. Yalnızca başka bir grup üyesini Grup sahibi yapmak için bireyler belirtebilirsiniz.

> [!NOTE]
> Kullanıcıların bir güvenlik grubuna veya Office 365 grubuna katılması istemesi ve sahiplerin üyelik isteklerini onaylaması veya reddetmesi için bir Azure Active Directory Premium (P1 veya P2) lisansı gerekir. Azure Active Directory Premium lisansı olmadan kullanıcılar, erişim panelinde gruplarını yine de yönetebilir, ancak erişim panelinde sahip onayı gerektiren bir grup oluşturamaz ve bir gruba katılmayı isteyemez. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
* [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](groups-settings-cmdlets.md)
* [Azure Active Directory’de Uygulama Yönetimi](../manage-apps/what-is-application-management.md)
* [Azure Active Directory nedir?](../fundamentals/active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)

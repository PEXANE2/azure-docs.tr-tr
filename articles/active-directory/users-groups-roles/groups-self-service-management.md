---
title: Self servis grup yönetimini ayarlama - Azure Active Directory | Microsoft Dokümanlar
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
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81768551"
---
# <a name="set-up-self-service-group-management-in-azure-active-directory"></a>Azure Active Directory'de self servis grup yönetimini ayarlama 

Azure Etkin Dizini'nde (Azure AD) kullanıcıların kendi güvenlik gruplarını veya Office 365 gruplarını oluşturmalarına ve yönetmelerine olanak sağlayabilirsiniz. Grubun sahibi üyelik isteklerini onaylayabilir veya reddedebilir ve grup üyeliğinin denetimini devredebilir. Self servis grup yönetimi özellikleri posta yla etkin güvenlik grupları veya dağıtım listeleri için kullanılamaz.

## <a name="self-service-group-membership-defaults"></a>Self servis grup üyeliği varsayılanları

Azure portalında veya Azure AD PowerShell kullanılarak güvenlik grupları oluşturulduğunda, üyeliği yalnızca grubun sahipleri güncelleştirebilir. [Access panelinde](https://account.activedirectory.windowsazure.com/r#/joinGroups) self servis tarafından oluşturulan güvenlik grupları ve tüm Office 365 grupları, sahibi onaylı veya otomatik onaylı olsun, tüm kullanıcılar için katılabilir. Access panelinde, grubu oluştururken üyelik seçeneklerini değiştirebilirsiniz.

Oluşturulan gruplar | Güvenlik grubu varsayılan davranışı | Office 365 grubu varsayılan davranışı
------------------ | ------------------------------- | ---------------------------------
[Azure AD PowerShell](groups-settings-cmdlets.md) | Yalnızca sahipler üye ekleyebilir<br>Access paneline katılmak için görünür ancak kullanılamıyor | Tüm kullanıcılar için katılmak için açık
[Azure portal](https://portal.azure.com) | Yalnızca sahipler üye ekleyebilir<br>Access paneline katılmak için görünür ancak kullanılamıyor<br>Sahip grup oluşturmada otomatik olarak atanmıyor | Tüm kullanıcılar için katılmak için açık
[Erişim paneli](https://account.activedirectory.windowsazure.com/r#/joinGroups) | Tüm kullanıcılar için katılmak için açık<br>Grup oluşturulduğunda üyelik seçenekleri değiştirilebilir | Tüm kullanıcılar için katılmak için açık<br>Grup oluşturulduğunda üyelik seçenekleri değiştirilebilir

## <a name="self-service-group-management-scenarios"></a>Self servis grup yönetimi senaryoları

* **Delege grup yönetimi** Örnek olarak, şirketin kullandığı bir SaaS uygulamasına erişimi yöneten bir yönetici örnek tir. Bu erişim haklarını yönetmek sıkıcı bir hal alabilir; bu durumda yönetici, işletme sahibinden yeni bir grup oluşturmasını ister. Yönetici, uygulama için yeni gruba erişim atar ve uygulamaya zaten erişen tüm kişileri gruba ekler. Ardından işletme sahibi daha fazla kullanıcı ekleyebilir ve bu kullanıcılar uygulamaya otomatik olarak sağlanır. İşletme sahibinin, yöneticinin kullanıcılar için erişimi yönetmesini beklemesi gerekmez. Yönetici farklı bir iş grubundaki bir yöneticiye aynı izni veriyorsa, bu kişi kendi grup üyelerine erişimi de yönetebilir. Ne işletme sahibi ne de yönetici birbirlerinin grup üyeliklerini görüntüleyebilir veya yönetemez. Yönetici uygulamaya erişimi olan tüm kullanıcıları görebilir ve gerekirse erişim haklarını engelleyebilir.
* **Self servis grup yönetimi** Bu senaryoya örnek olarak, her ikisi de bağımsız olarak kurdukları SharePoint Online sitelerine sahip iki kullanıcı dır. Birbirlerinin ekiplerine sitelerine giriş izni vermek istiyorlar. Bunun için Azure AD'de bir grup oluşturabilirler ve her biri SharePoint Online'da kendi sitelerine erişim sunmak üzere bu grubu seçerler. Birisi erişim istediğinde Erişim Paneli'nden ister ve onayın ardından otomatik olarak iki SharePoint Online sitesine de erişim sağlanır. Daha sonra bu kişilerden biri, siteye erişen herkesin belirli bir SaaS uygulamasına da erişmesi gerektiğine karar verir. SaaS uygulamasının yöneticisi uygulamanın erişim haklarını SharePoint Online sitesine ekleyebilir. Daha sonra onaylanan tüm istekler, iki SharePoint Online sitesine ve bu SaaS uygulamasına erişim sağlar.

## <a name="make-a-group-available-for-user-self-service"></a>Bir grubu kullanıcı self servisi için kullanıma sunma

1. Dizin için genel yönetici olan bir hesapla [Azure AD yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
1. **Gruplar'ı**seçin ve ardından **Genel** ayarları seçin.
1. Set **Sahipleri, Erişim Paneli'ndeki grup üyelik isteklerini** **Evet'e**yönetebilir.
1. **Erişim Panelindeki Gruplara Erişimi No** ile **Sınırlandır'ı**ayarlayın.
1. **Kullanıcılar, Azure portallarında güvenlik grupları oluşturabilir** veya Kullanıcılar Azure **portallarında Office 365 grupları oluşturabilir**

    - **Evet**: Azure REKLAM kuruluşunuzdaki tüm kullanıcıların yeni güvenlik grupları oluşturmasına ve bu gruplara üye eklemesine izin verilir. Bu yeni gruplar ayrıca diğer tüm kullanıcılar Erişim Paneli’nde gösterilir. Gruptaki ilke ayarı buna izin veriyorsa, diğer kullanıcılar bu gruplara katılma istekleri oluşturabilir
    - **Hayır**: Kullanıcılar grup oluşturamaz ve sahip oldukları varolan grupları değiştiremez. Ancak, bu grupların üyeliklerini yönetmeye ve diğer kullanıcıların gruplara katılma isteklerini onaylamaya devam edebilirler.

Ayrıca, üyeleri **Azure portallarında grup sahibi olarak atayabilecek Sahipler** ve kullanıcılarınız için self servis grup yönetimi üzerinde daha ayrıntılı erişim denetimi elde etmek için **Azure portallarında üye atayabilecek Sahipler'i** de kullanabilirsiniz.

Kullanıcılar gruplar oluşturabildiği zaman, kuruluşunuzdaki tüm kullanıcıların yeni gruplar oluşturmasına izin verilir ve varsayılan sahibi olarak bu gruplara üye ekleyebilir. Kendi gruplarını oluşturabilecek kişileri belirtemezsiniz. Kişileri yalnızca başka bir grup üyesini grup sahibi yapmak için belirtebilirsiniz.

> [!NOTE]
> Kullanıcıların bir güvenlik grubuna veya Office 365 grubuna katılmayı talep etmesi ve sahiplerinin üyelik isteklerini onaylaması veya reddetmesi için Azure Active Directory Premium (P1 veya P2) lisansı gereklidir. Azure Active Directory Premium lisansı olmadan, kullanıcılar gruplarını Erişim Paneli'nde yönetmeye devam edebilir, ancak Access Paneli'nde sahip onayı gerektiren bir grup oluşturamaz ve bir gruba katılmak için istekte bulunamaz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
* [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](groups-settings-cmdlets.md)
* [Azure Active Directory’de Uygulama Yönetimi](../manage-apps/what-is-application-management.md)
* [Azure Active Directory nedir?](../fundamentals/active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)

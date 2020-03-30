---
title: B2B işbirliği kullanıcılarını bilgi çalışanı olarak ekleyin - Azure AD
description: B2B işbirliği, bilgi çalışanlarının ve uygulama sahiplerinin erişim için Azure AD'ye konuk kullanıcı eklemesine olanak tanır | Microsoft Dokümanlar
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/19/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: abb5c6939d8c88db35a776aa8f2c075a4bdcc609
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77565426"
---
# <a name="how-users-in-your-organization-can-invite-guest-users-to-an-app"></a>Kuruluşunuzdaki kullanıcılar konuk kullanıcıları bir uygulamaya nasıl davet edebilir?

Azure AD'de bir konuk kullanıcı dizine eklendikten sonra, uygulama sahibi konuk kullanıcıya paylaşmak istedikleri uygulamaya doğrudan bir bağlantı gönderebilir. Azure AD yöneticileri, Azure AD kiracılarında galeri veya SAML tabanlı uygulamalar için self servis yönetimi de ayarlayabilir. Bu şekilde, konuk kullanıcılar henüz dizine eklenmemiş olsa bile, uygulama sahipleri kendi konuk kullanıcılarını yönetebilir. Bir uygulama self servis için yapılandırıldığında, uygulama sahibi konuk kullanıcıyı bir uygulamaya davet etmek veya uygulamaya erişimi olan bir gruba konuk kullanıcı eklemek için Access Panelini kullanır. Galeri ve SAML tabanlı uygulamalar için self servis uygulama yönetimi, bir yönetici tarafından ilk kurulumu gerektirir. Aşağıdaki kurulum adımlarının bir özeti (daha ayrıntılı talimatlar için, bu sayfada daha sonra [Önkoşullar'a](#prerequisites) bakın):

 - Kiracınız için self servis grup yönetimini etkinleştirin
 - Uygulamaya atamak ve kullanıcıyı sahip yapmak için bir grup oluşturma
 - Uygulamayı self servis için yapılandırın ve grubu uygulamaya atama

> [!NOTE]
> Bu makalede, Azure AD kiracınıza eklediğiniz galeri ve SAML tabanlı uygulamalar için self servis yönetiminin nasıl ayarlanması açıklanmaktadır. Ayrıca, kullanıcılarınızın kendi Office 365 gruplarına erişimi yönetebilmeleri için [self servis Office 365 grupları da](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management) ayarlayabilirsiniz. Kullanıcıların Office dosyalarını ve uygulamalarını konuk kullanıcılarla paylaşabilmesinin daha fazla yolu için [Office 365 gruplarında Konuk erişimine](https://support.office.com/article/guest-access-in-office-365-groups-bfc7a840-868f-4fd6-a390-f347bf51aff6) bakın ve [SharePoint dosya veya klasörlerini paylaşın.](https://support.office.com/article/share-sharepoint-files-or-folders-1fe37332-0f9a-4719-970e-d2578da4941c)

## <a name="invite-a-guest-user-to-an-app-from-the-access-panel"></a>Konuk kullanıcıyı Access Paneli'nden bir uygulamaya davet etme

Bir uygulama self servis için yapılandırıldıktan sonra, uygulama sahipleri kendi Access Panellerini kullanarak konuk kullanıcıyı paylaşmak istedikleri uygulamaya davet edebilirler. Konuk kullanıcının Azure AD'ye önceden eklenmesi gerekmez. 

1. Erişim Panelinizi ' `https://myapps.microsoft.com`ye giderek açın.
2. Uygulamayı işaret edin, elipsleri seçin (**...**), ve ardından **uygulamayı yönet'i**seçin.
 
   ![Salesforce uygulaması için Uygulama alt menüsünü gösteren ekran görüntüsü](media/add-users-iw/access-panel-manage-app.png)
 
3. Kullanıcı listesinin en üstünde. **+**
   
   ![Uygulamaya üye eklemek için artı sembolü gösteren ekran görüntüsü](media/add-users-iw/access-panel-manage-app-add-user.png)
   
4. Üye **Ekle** arama kutusuna konuk kullanıcının e-posta adresini yazın. İsteğe bağlı olarak bir hoş geldiniz iletisi ekleyin.
   
   ![Konuk eklemek için üye ekle penceresini gösteren ekran görüntüsü](media/add-users-iw/access-panel-invitation.png)
   
5. Konuk kullanıcıya davet göndermek için **Ekle'yi** seçin. Daveti göndermenizin ardından kullanıcı hesabı otomatik olarak dizine konuk olarak eklenir.

## <a name="invite-someone-to-join-a-group-that-has-access-to-the-app"></a>Birini uygulamaya erişimi olan bir gruba katılmaya davet etme
Uygulama self servis için yapılandırıldıktan sonra, uygulama sahipleri konuk kullanıcıları paylaşmak istedikleri uygulamalara erişimi olan yönettikleri gruplara davet edebilir. Konuk kullanıcıların dizinde zaten var olması gerekmez. Uygulama sahibi, bir konuk kullanıcıyı uygulamaya erişebilmeleri için gruba davet etmek için aşağıdaki adımları izler.

1. Paylaşmak istediğiniz uygulamaya erişimi olan self servis grubunun sahibi olduğunuzdan emin olun.
2. Erişim Panelinizi ' `https://myapps.microsoft.com`ye giderek açın.
3. **Gruplar** uygulamasını seçin.
   
   ![Erişim Paneli'ndeki Gruplar uygulamasını gösteren ekran görüntüsü](media/add-users-iw/access-panel-groups.png)
   
4. **Sahip olduğum Gruplar**altında, paylaşmak istediğiniz uygulamaya erişimi olan grubu seçin.
   
   ![Sahip olduğum Gruplar altında bir grubun nerede seçilebileceğinizi gösteren ekran görüntüsü](media/add-users-iw/access-panel-groups-i-own.png)
   
5. Grup üyeleri listesinin en üstünde. **+**
   
   ![Gruba üye eklemek için artı sembolü gösteren ekran görüntüsü](media/add-users-iw/access-panel-groups-add-member.png)
   
6. Üye **Ekle** arama kutusuna konuk kullanıcının e-posta adresini yazın. İsteğe bağlı olarak bir hoş geldiniz iletisi ekleyin.
   
   ![Konuk eklemek için üye ekle penceresini gösteren ekran görüntüsü](media/add-users-iw/access-panel-invitation.png)
   
7. Daveti konuk kullanıcıya otomatik olarak göndermek için **Ekle'yi** seçin. Daveti göndermenizin ardından kullanıcı hesabı otomatik olarak dizine konuk olarak eklenir.


## <a name="prerequisites"></a>Ön koşullar

Self servis uygulama yönetimi, bir Global Administrator ve bir Azure AD yöneticisi tarafından bazı ilk kurulumgerektirir. Bu kurulumun bir parçası olarak, uygulamayı self servis için yapılandıracak ve uygulama sahibinin yönetebileceği bir grup atayacaksınız. Ayrıca, grubu herkesin üyelik istemesine izin verecek, ancak grup sahibinin onayını gerektirecek şekilde yapılandırabilirsiniz. (Self [servis grup yönetimi](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management)hakkında daha fazla bilgi edinin.) 

> [!NOTE]
> Konuk kullanıcıları dinamik bir gruba veya şirket içi Active Directory ile senkronize edilen bir gruba ekleyemezsiniz.

### <a name="enable-self-service-group-management-for-your-tenant"></a>Kiracınız için self servis grup yönetimini etkinleştirin
1. [Azure portalında](https://portal.azure.com) Global Administrator olarak oturum açın.
2. Gezinti panelinde **Azure Etkin Dizin'i**seçin.
3. **Grupları**seçin.
4. **Ayarlar**altında, **Genel'i**seçin.
5. **Self Servis Grup Yönetimi**altında, Sahiplerinin yanında Erişim **Paneli'nde grup üyelik isteklerini yönetebilirsiniz,** **Evet'i**seçin.
6. **Kaydet'i**seçin.

### <a name="create-a-group-to-assign-to-the-app-and-make-the-user-an-owner"></a>Uygulamaya atamak ve kullanıcıyı sahip yapmak için bir grup oluşturma
1. [Azure portalında](https://portal.azure.com) Azure AD yöneticisi veya Genel Yönetici olarak oturum açın.
2. Gezinti panelinde **Azure Etkin Dizin'i**seçin.
3. **Grupları**seçin.
4. **Yeni grubu**seçin.
5. **Grup türü altında** **Güvenlik'i**seçin.
6. Bir **Grup adı** ve **Grup açıklaması** girin.
7. **Üyelik türü altında,** **Atanmış'ı**seçin.
8. **Oluştur'u**seçin ve **Grup** sayfasını kapatın.
9. Gruplar **- Tüm gruplar** sayfasında grubu açın. 
10. **Yönet**altında, **sahipleri** > seçin**Sahipleri ekle sahipleri**. Uygulamaya erişimi yönetmesi gereken kullanıcıyı arayın. Kullanıcıyı seçin ve sonra **Seç'i**tıklatın.

### <a name="configure-the-app-for-self-service-and-assign-the-group-to-the-app"></a>Uygulamayı self servis için yapılandırın ve grubu uygulamaya atama
1. [Azure portalında](https://portal.azure.com) Azure AD yöneticisi veya Genel Yönetici olarak oturum açın.
2. Gezinti bölmesinde Azure **Etkin Dizin'i**seçin.
3. **Yönet**altında, **Kurumsal uygulamaları** > seçin**Tüm uygulamalar**.
4. Uygulama listesinde uygulamayı bulun ve açın.
5. **Yönet**altında, **Tek oturum açma'yı**seçin ve uygulamayı tek oturum açma için yapılandırın. (Ayrıntılar için, kurumsal uygulamalar için tek oturum açma nın [nasıl yönetilebildiğini](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-non-gallery-applications)görün.)
6. **Yönet**altında Self **servis'i**seçin ve self servis uygulama erişimini ayarlayın. (Ayrıntılar için [self servis uygulama erişiminin nasıl kullanılacağına](https://docs.microsoft.com/azure/active-directory/application-access-panel-self-service-applications-how-to)bakın.) 

    > [!NOTE]
    > Ayarı Kullanıcılara **hangi gruba eklenmelidir?**
7. **Yönet**altında, **Kullanıcıları ve grupları**seçin ve oluşturduğunuz self servis grubunun listede göründüğünü doğrulayın.
8. Uygulamayı grup sahibinin Erişim Masası'na eklemek için Kullanıcı > Kullanıcı**ekle'yi ve grupları**seçin. **Add user** Grup sahibini arayın ve kullanıcıyı seçin, **Seç'i**tıklatın ve ardından kullanıcıyı uygulamaya eklemek için **Atla'yı** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili aşağıdaki makalelere bakın:

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [Azure Active Directory yöneticileri B2B işbirliği kullanıcılarını nasıl ekler?](add-users-administrator.md)
- [B2B işbirliği daveti itfa](redemption-experience.md)
- [Azure AD B2B işbirliği lisanslaması](licensing-guidance.md)

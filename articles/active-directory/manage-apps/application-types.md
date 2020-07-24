---
title: Uygulamalarım listesinde beklenmeyen uygulama | Microsoft Docs
description: Kiracınızdaki tüm uygulamaları görme ve uygulamaların kurumsal uygulamalar altındaki tüm uygulamalar listenizde nasıl göründüğünü anlama
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 07/11/2017
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6c329b7de3e18989e7dbfa35aa6e9303c129654
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87019310"
---
# <a name="unexpected-application-in-my-applications-list"></a>Uygulamalarım listesinde beklenmeyen uygulama

Bu makale, uygulamaların **Kurumsal uygulamalar**altındaki **tüm uygulamalar** listenizde nasıl göründüğünü anlamanıza yardımcı olur. 

## <a name="how-to-see-all-applications-in-your-tenant"></a>Kiracınızdaki tüm uygulamaları görme

Kiracınızdaki tüm uygulamaları görmek için, **Tüm** uygulamalar listesi altındaki **tüm uygulamaları** göstermek için **filtre** denetimini kullanmanız gerekir. Şu adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5.  tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

6.  **tüm uygulamalar listesinin**en üstündeki **filtre** denetimini kullan ' a tıklayın.

7.  **Filtre** bölmesinde, **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a>Tüm uygulamalar listesinde neden belirli bir uygulama görünüyor?

**Tüm uygulamalara**filtre uygulandığında **tüm uygulamalar** **listesi** kiracınızdaki tüm hizmet sorumlusu nesnelerini gösterir. Hizmet sorumlusu nesneleri, bu listede çeşitli yollarla görünebilir:

1. Uygulama galerisinden herhangi bir uygulamayı eklediğinizde şunlar da dahildir:

   1. **Azure AD Galeri uygulamaları** – Azure AD ile çoklu oturum açma için önceden tümleştirilmiş bir uygulama

   2. **Uygulama proxy 'Si uygulamaları** – dışarıdan güvenli çoklu oturum açma sağlamak istediğiniz şirket içi ortamınızda çalışan bir uygulamadır

   3. **Özel geliştirmiş uygulamalar** : KURULUŞUNUZUN Azure AD uygulama geliştirme platformunda geliştirme ve henüz var olmayan bir uygulama

   4. **Galeri olmayan uygulamalar** – kendi uygulamalarınızı getirin! İstediğiniz herhangi bir Web bağlantısı veya bir Kullanıcı adı ve parola içeren herhangi bir uygulama, SAML veya OpenID Connect protokollerini destekler ya da Azure AD ile çoklu oturum açma için tümleştirmesini istediğiniz SCıM 'yi destekler.

2. İçin kaydolurken veya ' de oturum açarken, Azure Active Directory ile tümleştirilmiş 3 bir<sup>RD</sup> taraf uygulaması. [Smartsheet](https://app.smartsheet.com/b/home) veya [Docusign](https://www.docusign.net/member/MemberLogin.aspx)bir örnektir.

3. İçin kaydolurken veya bir kullanıcıya veya gruba bir lisans eklenirken [Microsoft Office 365](https://products.office.com/) gibi

4. [Uygulama kayıt defteri](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration) 'ni kullanarak özel olarak geliştirilmiş bir uygulama oluşturarak yeni bir uygulama kaydı eklediğinizde

5. [V 2.0 uygulama kayıt portalını](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration) kullanarak özel olarak geliştirilmiş bir uygulama oluşturarak yeni bir uygulama kaydı eklediğinizde

6. Visual Studio 'nun [ASP.NET kimlik doğrulama yöntemlerini](https://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) veya [bağlı hizmetlerini](https://devblogs.microsoft.com/visualstudio/connecting-to-cloud-services/) kullanarak geliştirmekte olduğunuz bir uygulamayı eklediğinizde

7. [Azure AD PowerShell modülünü](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0) kullanarak bir hizmet sorumlusu nesnesi oluşturduğunuzda

8. [Bir uygulamayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) kiracınızdaki verileri kullanmak üzere yönetici olarak kabul ettiğinizde

9. Bir Kullanıcı kiracınızdaki verileri kullanmak üzere [bir uygulamaya onay verirse](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview)

10. Kiracınızda veri depolayan belirli hizmetleri etkinleştirdiğinizde. Parola sıfırlama ilkenizi güvenli bir şekilde depolamak için bir hizmet sorumlusu olarak modellenen parola sıfırlama bir örnektir.

Uygulamaların dizininize nasıl eklendiği hakkında daha fazla bilgi edinmek için [Azure AD 'ye uygulamaların nasıl ve neden eklendiğini](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added)okuyun.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Belirli bir kullanıcının veya grubun atamasını bir uygulamaya kaldırmak istiyorum

Bir uygulamaya Kullanıcı veya grup atamasını kaldırmak için, Azure Active Directory makalesinde bir [Kurumsal uygulamadan Kullanıcı veya grup atamasını kaldırma](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) bölümünde listelenen adımları izleyin.

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>Her Kullanıcı için bir uygulamaya tüm erişimi devre dışı bırakmak istiyorum

Bir uygulamaya tüm Kullanıcı oturum açma işlemlerini devre dışı bırakmak için, [Azure Active Directory makalesinde bir kurumsal uygulama için Kullanıcı oturum açma Işlemlerini devre dışı bırak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) ' da listelenen adımları izleyin.

## <a name="i-want-to-delete-an-application-entirely"></a>Bir uygulamayı tamamen silmek istiyorum

**Bir uygulamayı silmek**için şu adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Silmek istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra üst uygulamanın **genel bakış** bölmesindeki **Sil** simgesine tıklayın.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Tüm gelecek Kullanıcı onay işlemlerini herhangi bir uygulamaya devre dışı bırakmak istiyorum

Tüm dizininiz için Kullanıcı onayını devre dışı bırakmak, son kullanıcıların herhangi bir uygulamaya yarışmasını önler. Yöneticiler kullanıcı adına yine de izin verebilir. Uygulama onayı hakkında daha fazla bilgi edinmek ve izin vermek istememeyi öğrenmek için [Kullanıcı ve yönetici onayını anlama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview)konusunu okuyun.

Tüm **dizininizde tüm gelecek Kullanıcı onay işlemlerini devre dışı bırakmak**için şu adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5.  **Kullanıcı ayarları**' na tıklayın.

6.  **Kullanıcıların, uygulamaların verilerine erişmesine izin verebilmeleri** **için tüm** gelecek Kullanıcı onay işlemlerini devre dışı bırakın ve **Kaydet** düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)

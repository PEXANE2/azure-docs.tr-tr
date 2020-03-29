---
title: Uygulama listemde beklenmeyen uygulama | Microsoft Dokümanlar
description: Kiracınızdaki tüm uygulamaları nasıl görebilirsiniz ve Kurumsal Uygulamalar altındaki Tüm Uygulamalar listenizde uygulamaların nasıl göründüğünü anlama
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: b1cb4eeb52d0680695bda266ad1a563b2ef5ee02
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65781106"
---
# <a name="unexpected-application-in-my-applications-list"></a>Uygulama listemdeki beklenmeyen uygulama

Bu makale, **Uygulamaların Kurumsal Uygulamalar**altında Tüm **Uygulamalar** listenizde nasıl göründüğünü anlamanıza yardımcı olur. 

## <a name="how-to-see-all-applications-in-your-tenant"></a>Kiracınızdaki tüm uygulamaları nasıl görebilirsiniz?

Kiracınızdaki tüm uygulamaları görmek için Tüm **Uygulamalar** listesinin altında **Tüm Uygulamaları** göstermek için **Filtre** denetimini kullanmanız gerekir. Şu adımları uygulayın:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5.  **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

6.  **Tüm Uygulamalar Listesi'nin**en üstündeki **Filtre** denetimini tıklatın.

7.  **Filtre** bölmesinde, Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a>Neden tüm uygulamalar listemde belirli bir uygulama görünüyor?

**Tüm Uygulamalara**filtrelendiğinde, **Tüm Uygulamalar** **Listesi** kiracınızdaki her Hizmet Sorumlusu nesnesini gösterir. Hizmet Sorumlusu nesneleri bu listede çeşitli şekillerde görünebilir:

1. Aşağıdakiler de dahil olmak üzere uygulama galerisinden herhangi bir uygulama eklediğinizde:

   1. **Azure AD Galerisi Uygulamaları** – Azure AD ile tek oturum açma için önceden tümleştirilmiş bir uygulama

   2. **Uygulama Proxy Uygulamaları** – Şirket içi ortamınızda çalışan ve harici olarak güvenli tek oturum açmanızı istediğiniz bir uygulama

   3. **Özel olarak geliştirilmiş Uygulamalar** – Kuruluşunuzun Azure AD Uygulama Geliştirme Platformu'nda geliştirmek istediği ancak henüz var olmayan bir uygulama

   4. **Galeri Dışı Uygulamalar** – Kendi uygulamalarınızı getirin! İstediğiniz herhangi bir web bağlantısı veya kullanıcı adı ve parola alanı oluşturan herhangi bir uygulama, SAML veya OpenID Connect protokollerini destekler veya Azure AD ile tek oturum açmak için tümleştirmek istediğiniz SCIM'i destekler.

2. Azure Active Directory ile tümleşik<sup>3.</sup> Bir örnek [Smartsheet](https://app.smartsheet.com/b/home) veya [DocuSign](https://www.docusign.net/member/MemberLogin.aspx)olduğunu.

3. [Microsoft Office 365](https://products.office.com/) gibi bir birinci taraf uygulamasına kaydolurken veya bir kullanıcıya veya gruba lisans eklerken

4. [Uygulama Kayıt Defteri'ni](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration) kullanarak özel olarak geliştirilmiş bir uygulama oluşturarak yeni bir uygulama kaydı eklediğinizde

5. [V2.0 Uygulama Kayıt portalını](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration) kullanarak özel olarak geliştirilmiş bir uygulama oluşturarak yeni bir uygulama kaydı eklediğinizde

6. Visual Studio'nun [ASP.net Kimlik Doğrulama Yöntemleri](https://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) ni veya Bağlı [Hizmetleri](https://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx) kullanarak geliştirdiğiniz bir uygulama eklerken

7. [Azure AD PowerShell Modül'u](/powershell/azure/install-adv2?view=azureadps-2.0) kullanarak bir hizmet temel nesnesi oluşturduğunuzda

8. Yönetici olarak [bir uygulamayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) kiracınızdaki verileri kullanmayı kabul ettiğinizde

9. Bir [kullanıcı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) kiracınızdaki verileri kullanmak için bir uygulamayı kabul ettiğinde

10. Kiracınızda veri depolayan belirli hizmetleri etkinleştirdiğinizde. Parola sıfırlama ilkenizi güvenli bir şekilde depolamak için hizmet sorumlusu olarak modellenen Parola Sıfırlama, bir örnektir.

Uygulamaların dizininize nasıl eklendiklerine ilişkin daha fazla bilgi almak [için, uygulamaların Azure AD'ye nasıl ve neden eklenmesini](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added)okuyun.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Belirli bir kullanıcının veya grubun atamasını bir uygulamaya kaldırmak istiyorum

Bir uygulamaya kullanıcı veya grup atamasını kaldırmak için, Azure Active Directory makalesindeki [bir kurumsal uygulamadan kullanıcı veya grup atamasını kaldır'da](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) listelenen adımları izleyin.

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>Her kullanıcı için bir uygulamaya tüm erişimi devre dışı kalmak istiyorum

Bir uygulamada tüm kullanıcı oturum açmalarını devre dışı katmak [için, Azure Active Directory makalesinde bir kurumsal uygulama için kullanıcı oturum açmalarını devre dışı bırak'da](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) listelenen adımları izleyin.

## <a name="i-want-to-delete-an-application-entirely"></a>Bir uygulamayı tamamen silmek istiyorum

**Bir uygulamayı silmek**için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Silmek istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, üst uygulamanın **Genel Bakış** bölmesinden **Sil** simgesini tıklatın.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Gelecekteki tüm kullanıcı onayı işlemlerini herhangi bir uygulamaya devre dışı kalmak istiyorum

Tüm dizininizin kullanıcı onayını devre dışı bırakmak, son kullanıcıların herhangi bir uygulamaya onay vermesini engeller. Yöneticiler yine de kullanıcı adına izin verebilir. Uygulama onayı hakkında daha fazla bilgi edinmek ve neden izin vermek veya istemeyebilirsiniz hakkında daha fazla bilgi edinmek için, [Kullanıcı yı ve yönetici onayLarını anlama'yı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview)okuyun.

**Tüm dizininizdeki gelecekteki tüm kullanıcı onayı işlemlerini devre dışı katmak**için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **Kullanıcı ayarlarını**tıklatın.

6.  Kullanıcılar'ı ayarlayarak gelecekteki tüm kullanıcı onayı işlemlerini devre dışı kullanabilirsiniz, **uygulamaların verilerine erişmesine izin vererek** **Hayır'a** geçiş yapabilir ve **Kaydet** düğmesini tıklatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)

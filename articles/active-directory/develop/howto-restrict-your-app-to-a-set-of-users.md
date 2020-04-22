---
title: Azure AD uygulamasını bir kullanıcı kümesiyle sınırlama | Azure
titleSuffix: Microsoft identity platform
description: Azure AD'de kayıtlı uygulamalarınız için erişimi seçili bir kullanıcı kümesiyle nasıl kısıtlayamanızı öğrenin.
services: active-directory
author: kalyankrishna1
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: kkrishna
ms.reviewer: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 8bdc7e6e3795719128a8ecfb1e8bc97c1a9a08c7
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759036"
---
# <a name="how-to-restrict-your-azure-ad-app-to-a-set-of-users-in-an-azure-ad-tenant"></a>Nasıl yapilir: Azure AD uygulamanızı Azure AD kiracısındaki bir kullanıcı kümesiyle sınırlama

Azure Etkin Dizin (Azure AD) kiracısına kayıtlı uygulamalar varsayılan olarak, kiracının başarılı bir şekilde kimlik doğrulaması yapan tüm kullanıcılar tarafından kullanılabilir.

Benzer şekilde, çok [kiracılı](howto-convert-app-to-be-multi-tenant.md) bir uygulama olması durumunda, bu uygulamanın sağlandığı Azure AD kiracısındaki tüm kullanıcılar, ilgili kiracılarında başarılı bir şekilde kimlik doğrulaması yaptıktan sonra bu uygulamaya erişebilecektir.

Kiracı yöneticilerin ve geliştiricilerin genellikle bir uygulamanın belirli bir kullanıcı kümesiyle sınırlanması gereken gereksinimleri vardır. Geliştiriciler, Role Based Access Control (RBAC) gibi popüler yetkilendirme kalıplarını kullanarak aynı şeyi gerçekleştirebilir, ancak bu yaklaşım geliştiricinin bir bölümünde önemli miktarda çalışma gerektirir.

Kiracı yöneticiler ve geliştiriciler, Azure AD'nin bu yerleşik özelliğini de kullanarak bir uygulamayı kiracıdaki belirli bir kullanıcı kümesiyle veya güvenlik gruplarıyla sınırlandırabilir.

## <a name="supported-app-configurations"></a>Desteklenen uygulama yapılandırmaları

Bir uygulamayı, kiracıdaki belirli bir kullanıcı kümesiyle veya güvenlik gruplarıyla sınırlama seçeneği aşağıdaki türuygulamalarla çalışır:

- SAML tabanlı kimlik doğrulama ile federal tek oturum açma için yapılandırılan uygulamalar
- Azure AD ön kimlik doğrulaması kullanan uygulama proxy uygulamaları
- Bir kullanıcı veya yönetici nin bu uygulamayı kabul etmesinin ardından OAuth 2.0/OpenID Connect kimlik doğrulamasını kullanan Azure AD uygulama platformunda doğrudan oluşturulmuş uygulamalar.

     > [!NOTE]
     > Bu özellik yalnızca web uygulaması/web API'si ve kurumsal uygulamalar için kullanılabilir. [Yerel](quickstart-v1-integrate-apps-with-azure-ad.md) olarak kaydedilmiş uygulamalar, kiracıdaki bir kullanıcı kümesi veya güvenlik gruplarıyla sınırlandırılamaz.

## <a name="update-the-app-to-enable-user-assignment"></a>Kullanıcı atamasını etkinleştirmek için uygulamayı güncelleştirin

Etkin kullanıcı ataması olan bir uygulama oluşturmanın iki yolu vardır. Biri **Global Administrator** rolünü gerektirir, ikincisi gerekmez.

### <a name="enterprise-applications-requires-the-global-administrator-role"></a>Kurumsal uygulamalar (Global Administrator rolü gerektirir)

1. [**Azure portalına**](https://portal.azure.com/) gidin ve **Global Administrator**olarak oturum açın.
1. Üst teki çubukta, oturum açmış hesabı seçin. 
1. **Dizin**altında, uygulamanın kaydolacağı Azure AD kiracısını seçin.
1. Soldaki gezintide Azure **Etkin Dizin'i**seçin. Azure Etkin Dizini gezinti bölmesinde kullanılamıyorsa aşağıdaki adımları izleyin:

    1. Ana sol navigasyon menüsünün üst kısmındaki **Tüm hizmetleri** seçin.
    1. Filtre arama kutusuna **Azure Etkin Dizini** yazın ve sonuçtan **Azure Etkin Dizin** öğesini seçin.

1. Azure **Etkin Dizin** bölmesinde, Azure Active **Directory** sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** seçin.
1. **Tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı seçin.

     Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, listeyi kısıtlamak veya uygulamanızı bulmak için listeyi aşağı kaydırmak için **Tüm uygulamalar** listesinin en üstündeki çeşitli filtreleri kullanın.

1. Listeden bir kullanıcı veya güvenlik grubu atamak istediğiniz uygulamayı seçin.
1. Uygulamanın Genel **Bakış** sayfasında, uygulamanın sol daki gezinme menüsünden **Özellikler'i** seçin.
1. Gerekli ayar **Kullanıcı atamasını** bulun ve **Evet**olarak ayarlayın. Bu seçenek **Evet**olarak ayarlandığında, kiracıdaki kullanıcıların önce bu uygulamaya atanması gerekir veya bu uygulamada oturum açamazlar.
1. Bu yapılandırma değişikliğini kaydetmek için **Kaydet'i** seçin.

### <a name="app-registration"></a>Uygulama kaydı

1. [**Azure portalına**](https://portal.azure.com/)gidin.
1. Üst teki çubukta, oturum açmış hesabı seçin. 
1. **Dizin**altında, uygulamanın kaydolacağı Azure AD kiracısını seçin.
1. Soldaki gezintide Azure **Etkin Dizin'i**seçin.
1. Azure **Etkin Dizin** bölmesinde, Azure Active **Directory** sol navigasyon menüsünden **Uygulama Kayıtları'nı** seçin.
1. Yönetmek istediğiniz uygulamayı oluşturun veya seçin. Bu uygulama kaydının **sahibi** olmanız gerekir.
1. Uygulamanın Genel **Bakış** sayfasında, sayfanın üst kısmındaki temel bilgiler altında **yerel dizin bağlantısındaki Yönetilen uygulamayı** izleyin. Bu, sizi uygulama kaydınızın _yönetilen Kurumsal Uygulamasına_ götürür.
1. Soldaki navigasyon bıçağından **Özellikler'i**seçin.
1. Gerekli ayar **Kullanıcı atamasını** bulun ve **Evet**olarak ayarlayın. Bu seçenek **Evet**olarak ayarlandığında, kiracıdaki kullanıcıların önce bu uygulamaya atanması gerekir veya bu uygulamada oturum açamazlar.
1. Bu yapılandırma değişikliğini kaydetmek için **Kaydet'i** seçin.

## <a name="assign-users-and-groups-to-the-app"></a>Kullanıcıları ve grupları uygulamaya atama

Uygulamanızı kullanıcı atamasını etkinleştirecek şekilde yapılandırdıktan sonra, kullanıcıları ve grupları uygulamaya atayabilirsiniz.

1. Uygulamanın sol navigasyon menüsünde **Kullanıcılar ve gruplar** bölmesini seçin.
1. **Kullanıcılar ve gruplar** listesinin en üstünde, Atama **Ekle** bölmesini açmak için **Kullanıcı Ekle** düğmesini seçin.
1. **Atama Ekle** bölmesinden **Kullanıcı** seçicisini seçin. 

     Belirli bir kullanıcıyı veya grubu aramak ve bulmak için bir textbox ile birlikte kullanıcıların ve güvenlik gruplarının listesi gösterilir. Bu ekran, tek bir denemede birden çok kullanıcı ve grup seçmenize olanak tanır.

1. Kullanıcıları ve grupları seçtikten sonra, bir sonraki bölüme geçmek için alttaki **Seç** düğmesine basın.
1. (İsteğe bağlı) Uygulamanızda Uygulama rollerini tanımladıysanız, seçili kullanıcıları ve grupları uygulamanın rollerinden birine atamak için **Rolü Seç** seçeneğini kullanabilirsiniz. 
1. Kullanıcıların ve grupların uygulamaya yönelik ödevlerini tamamlamak için alttaki **Atla** düğmesine basın. 
1. Eklediğiniz kullanıcıların ve grupların güncelleştirilmiş Kullanıcılar **ve gruplar** listesinde yer aldığına onaylayın.

## <a name="more-information"></a>Daha fazla bilgi

- [Nasıl yapilir: Uygulamanızda uygulama rolleri ekleme](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps)
- [ASP.NET Core web uygulamasına uygulama rollerini & rolleri ASP.NET iddialarını kullanarak yetkilendirme ekleme](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/5-WebApp-AuthZ/5-1-Roles)
- [Uygulamalarınızda Güvenlik Gruplarını ve Uygulama Rollerini Kullanma (Video)](https://www.youtube.com/watch?v=V8VUPixLSiM)
- [Azure Active Directory, şimdi Grup Talepleri ve Uygulama Rolleri ile](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-now-with-Group-Claims-and-Application/ba-p/243862)
- [Azure Active Directory uygulama bildirimi](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)
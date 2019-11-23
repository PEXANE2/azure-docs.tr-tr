---
title: Quickstart - Set up sign-in for a desktop app using Azure Active Directory B2C
description: In this Quickstart, run a sample WPF desktop application that uses Azure Active Directory B2C to provide account sign-in.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: quickstart
ms.custom: mvc
ms.date: 09/12/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: ed92605c7fb74186ddde6ff193a1365146494594
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2019
ms.locfileid: "74420231"
---
# <a name="quickstart-set-up-sign-in-for-a-desktop-app-using-azure-active-directory-b2c"></a>Hızlı Başlangıç - Azure Active Directory B2C kullanarak bir masaüstü uygulaması için oturum açma bilgileri ayarlama

Azure Active Directory B2C (Azure AD B2C) provides cloud identity management to keep your application, business, and customers protected. Azure AD B2C; uygulamalarınızın, açık standart protokolleri kullanarak sosyal hesaplarda ve kurumsal hesaplarda kimlik doğrulaması gerçekleştirmesine olanak tanır. Bu hızlı başlangıçta bir sosyal kimlik sağlayıcısı kullanarak oturum açmak ve Azure AD B2C korumalı bir web API’sini çağırmak için bir Windows Presentation Foundation (WPF) masaüstü uygulaması kullanacaksınız.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2019](https://www.visualstudio.com/downloads/) with the **ASP.NET and web development** workload.
- A social account from either Facebook, Google, or Microsoft.
- GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip) veya örnek web uygulamasını kopyalayın.

    ```
    git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
    ```

## <a name="run-the-application-in-visual-studio"></a>Uygulamayı Visual Studio'da çalıştırma

1. Örnek uygulama proje klasöründeki **active-directory-b2c-wpf.sln** çözümünü Visual Studio'da açın.
2. Uygulamada hata ayıklamak için **F5**’e basın.

## <a name="sign-in-using-your-account"></a>Hesabınızı kullanarak oturum açın

1. **Kaydolma veya Oturum Açma** iş akışını başlatmak için **Oturum aç**’a tıklayın.

    ![Screenshot of the sample WPF application](media/active-directory-b2c-quickstarts-desktop-app/wpf-sample-application.png)

    The sample supports several sign-up options. These options include using a social identity provider or creating a local account using an email address. For this quickstart, use a social identity provider account from either Facebook, Google, or Microsoft.


2. Azure AD B2C presents a sign-in page for a fictitious company called Fabrikam for the sample web application. Sosyal kimlik sağlayıcısı kullanarak kaydolmak için, kullanmak istediğiniz kimlik sağlayıcısının düğmesine tıklayın.

    ![Sign In or Sign Up page showing identity providers](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-wpf.png)

    You authenticate (sign in) using your social account credentials and authorize the application to read information from your social account. Erişim izni verdiğinizde uygulama sosyal hesabınızdan adınız ve şehriniz gibi profil bilgilerini alabilir.

2. Kimlik sağlayıcısına ilişkin oturum açma işlemini tamamlayın.

    Yeni hesap profili ayrıntılarınız, sosyal hesabınızdaki bilgilerle önceden doldurulur.

## <a name="edit-your-profile"></a>Profilinizi düzenleme

Azure AD B2C, kullanıcılara profillerini güncelleme olanağı tanıyan bir işlev sunar. The sample web app uses an Azure AD B2C edit profile user flow for the workflow.

1. Oluşturduğunuz profili düzenlemek için uygulamanın menü çubuğunda **Profili düzenle**’ye tıklayın.

    ![Edit profile button highlighted in WPF sample app](media/active-directory-b2c-quickstarts-desktop-app/edit-profile-wpf.png)

2. Oluşturduğunuz hesapla ilişkili kimlik sağlayıcısını seçin. For example, if you used Facebook as the identity provider when you created your account, choose Facebook to modify the associated profile details.

3. **Görünen adınızı** veya **Şehrinizi** değiştirip **Devam**’a tıklayın.

    *Belirteç bilgileri* metin kutusunda yeni bir erişim belirteci görüntülenir. Profilinizdeki değişiklikleri doğrulamak istiyorsanız erişim belirtecini kopyalayıp https://jwt.ms belirteç kod çözücüsüne yapıştırın.

## <a name="access-a-protected-api-resource"></a>Korumalı bir API kaynağına erişme

Korumalı kaynaktan istekte bulunmak için **API’yi çağır**’a tıklayın.

    ![Call API](media/active-directory-b2c-quickstarts-desktop-app/call-api-wpf.png)

    The application includes the Azure AD access token in the request to the protected web API resource. The web API sends back the display name contained in the access token.

Azure AD B2C korumalı bir web API’si için yetkili bir çağrıda bulunmak üzere Azure AD B2C kullanıcı hesabınızı başarıyla kullandınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C hızlı başlangıçlarını veya öğreticilerini denemeyi planlıyorsanız Azure AD B2C kiracınızı kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C kiracınızı silebilirsiniz](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

In this quickstart, you used a sample desktop application to:

* Sign in with a custom login page
* Sign in with a social identity provider
* Create an Azure AD B2C account
* Call a web API protected by Azure AD B2C

Kendi Azure AD B2C kiracınızı oluşturarak kullanmaya başlayın.

> [!div class="nextstepaction"]
> [Azure portalında Azure Active Directory B2C kiracısı oluşturma](tutorial-create-tenant.md)

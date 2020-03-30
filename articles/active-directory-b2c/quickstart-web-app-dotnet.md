---
title: 'Quickstart: ASP.NET bir web uygulaması için kaydolma ayarlama'
titleSuffix: Azure AD B2C
description: Bu Quickstart'ta, hesap oturum açma sağlamak için Azure Active Directory B2C kullanan bir web uygulaması ASP.NET örnek çalıştırın.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: quickstart
ms.custom: mvc
ms.date: 09/12/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 197155650bf7b8d113f96dbcfd6d9e2b58924b7f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78187211"
---
# <a name="quickstart-set-up-sign-in-for-an-aspnet-application-using-azure-active-directory-b2c"></a>Hızlı başlatma: Azure Active Directory B2C'yi kullanarak ASP.NET bir uygulama için oturum açma

Azure Active Directory B2C (Azure AD B2C), uygulamanızı, işletmenizi ve müşterilerinizi korumak için bulut kimlik yönetimi sağlar. Azure AD B2C; uygulamalarınızın, açık standart protokolleri kullanarak sosyal hesaplarda ve kurumsal hesaplarda kimlik doğrulaması gerçekleştirmesine olanak tanır. Bu hızlı başlangıçta, sosyal kimlik sağlayıcısı kullanıp Azure AD B2C korumalı web API’si çağırarak oturum açmak için bir ASP.NET uygulaması kullanacaksınız.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- [Visual Studio 2019](https://www.visualstudio.com/downloads/) **ASP.NET ve web geliştirme** iş yükü ile.
- Facebook, Google veya Microsoft'tan bir sosyal hesap.
- GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) veya örnek web uygulamasını kopyalayın.

    ```
    git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
    ```

    Örnek çözümde iki proje vardır:

    - **TaskWebApp**: Görev listesi oluşturan ve düzenleyen bir web uygulamasıdır. Web uygulaması, **kaydolmak veya** kullanıcılara oturum açmak için kaydolma veya oturum açma kullanıcı akışını kullanır.
    - **TaskService**: Görev listesi oluşturma, okuma, güncelleştirme ve silme işlevlerini destekleyen web API’sidir. Web API’si Azure AD B2C tarafından korunur ve web uygulaması tarafından çağrılır.

## <a name="run-the-application-in-visual-studio"></a>Uygulamayı Visual Studio'da çalıştırma

1. Örnek uygulama proje klasöründen, Visual Studio’da **B2C-WebAPI-DotNet.sln** çözümünü açın.
2. Bu hızlı başlangıç için aynı anda hem **TaskWebApp** hem de **TaskService** projesini çalıştırıyorsunuz. Çözüm Gezgini'nde **B2C-WebAPI-DotNet** çözümüne sağ tıklayıp **Başlangıç Projelerini Ayarla**'yı seçin.
3. **Çoklu başlangıç projeleri**'ni seçin ve iki projenin de **Eylem** alanını **Başlat** olarak değiştirin.
4. **Tamam**'a tıklayın.
5. Her iki uygulamada da hata ayıklama gerçekleştirmek için **F5**’e basın. Her uygulama kendi tarayıcı sekmesinde açılır:

    - `https://localhost:44316/` - ASP.NET web uygulaması. Hızlı başlangıçta doğrudan bu uygulamayla etkileşim kurarsınız.
    - `https://localhost:44332/` - ASP.NET web uygulaması tarafından çağrılan web API’si.

## <a name="sign-in-using-your-account"></a>Hesabınızı kullanarak oturum açın

1. İş akışını başlatmak için ASP.NET web uygulamasında **Kaydol/Oturum aç**'a tıklayın.

    ![Tarayıcıda web uygulamasını ASP.NET ve kaydol/kaydol bağlantısı vurgulanmış](./media/quickstart-web-app-dotnet/web-app-sign-in.png)

    Örnek, sosyal kimlik sağlayıcısı kullanma veya e-posta kullanarak yerel hesap oluşturma gibi çeşitli oturum açma seçeneklerini destekler. Bu hızlı başlangıç için, Facebook, Google veya Microsoft'tan bir sosyal kimlik sağlayıcı hesabı kullanın.

2. Azure AD B2C, örnek web uygulaması için Fabrikam adlı hayali bir şirket için bir oturum açma sayfası sunar. Sosyal kimlik sağlayıcısı kullanarak kaydolmak için, kullanmak istediğiniz kimlik sağlayıcısının düğmesine tıklayın.

    ![Kimlik sağlayıcı düğmelerini gösteren Oturum Aç veya Kaydol sayfası](./media/quickstart-web-app-dotnet/sign-in-or-sign-up-web.png)

    Sosyal hesap kimlik bilgilerinizi kullanarak kimlik doğrulaması (oturum açma) ve sosyal hesabınızdaki bilgileri okumak için başvuruyunu yetkilendiriyorsunuz. Erişim izni verdiğinizde uygulama sosyal hesabınızdan adınız ve şehriniz gibi profil bilgilerini alabilir.

3. Kimlik sağlayıcısına ilişkin oturum açma işlemini tamamlayın.

## <a name="edit-your-profile"></a>Profilinizi düzenleme

Azure Active Directory B2C, kullanıcılara profillerini güncelleme olanağı tanıyan bir işlev sunar. Örnek web uygulaması, iş akışı için bir Azure AD B2C randevu profili kullanıcı akışı kullanır.

1. Uygulamanın menü çubuğunda profil adınıza tıklayın ve oluşturduğunuz profili düzenlemek için **Profili düzenle**’yi seçin.

    ![Profil bağlantısı vurgulanmış tarayıcıda örnek web uygulaması](./media/quickstart-web-app-dotnet/edit-profile-web.png)

2. **Görünen adınızı** veya **Şehrinizi** değiştirin ve profilinizi güncelleştirmek için **Devam**’a tıklayın.

    Değişiklik, web uygulamasının giriş sayfasının sağ üst kısmında görüntülenir.

## <a name="access-a-protected-api-resource"></a>Korumalı bir API kaynağına erişme

1. Yapılacaklar listesi öğelerinizi girmek ve değiştirmek için **Yapılacaklar Listesi**’ne tıklayın.

2. Metni **Yeni Öğe** metin kutusuna girin. Yapılacaklar listesi öğesi ekleyen Azure AD B2C korumalı web API’sini çağırmak için **Ekle**’ye tıklayın.

    ![Yapılacaklar listesi öğesi ekle ile tarayıcıda örnek web uygulaması](./media/quickstart-web-app-dotnet/add-todo-item-web.png)

    ASP.NET web uygulaması, kullanıcının yapılacaklar listesi öğelerinde işlem gerçekleştirmek için korumalı web API’si kaynağına gönderilen isteğe bir Azure AD erişim belirteci ekler.

Azure AD B2C korumalı bir web API’si için yetkili bir çağrıda bulunmak üzere Azure AD B2C kullanıcı hesabınızı başarıyla kullandınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C hızlı başlangıçlarını veya öğreticilerini denemeyi planlıyorsanız Azure AD B2C kiracınızı kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C kiracınızı silebilirsiniz](faq.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, örnek ASP.NET uygulaması kullandınız:

* Özel bir giriş sayfasıyla oturum açma
* Sosyal kimlik sağlayıcısıyla oturum açın
* Azure AD B2C hesabı oluşturma
* Azure AD B2C tarafından korunan bir web API'sını arama

Kendi Azure AD B2C kiracınızı oluşturarak kullanmaya başlayın.

> [!div class="nextstepaction"]
> [Azure portalında Azure Active Directory B2C kiracısı oluşturma](tutorial-create-tenant.md)

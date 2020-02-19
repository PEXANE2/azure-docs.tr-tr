---
title: Apache Cordova kimlik doğrulaması ekle
description: Google, Facebook, Twitter ve Microsoft gibi kimlik sağlayıcılarıyla Apache Cordova uygulamanızın kullanıcılarının kimliğini doğrulamak için Azure App Service Mobile Apps nasıl kullanacağınızı öğrenin.
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 3714ce2a8098608851991115aa82afdc00d08a47
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77459428"
---
# <a name="add-authentication-to-your-apache-cordova-app"></a>Apache Cordova uygulamanıza kimlik doğrulaması ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Özet
Bu öğreticide, desteklenen bir kimlik sağlayıcısı kullanarak Apache Cordova ToDoList hızlı başlangıç projesine kimlik doğrulaması eklersiniz. Bu öğretici, ilk olarak gerçekleştirmeniz gereken Mobile Apps öğreticiye [Mobile Apps kullanmaya başlayın] ' i temel alır.

## <a name="register"></a>Uygulamanızı kimlik doğrulaması için kaydedin ve App Service yapılandırın
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Benzer adımları gösteren bir video izleyin](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <a name="permissions"></a>Kimliği doğrulanmış kullanıcılar için izinleri kısıtla
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Şimdi, arka ucunuza anonim erişimin devre dışı bırakıldığını doğrulayabilirsiniz. Visual Studio'da:

* Öğreticiyi tamamladığınızda oluşturduğunuz projeyi açın [Mobile Apps kullanmaya başlayın].
* Uygulamanızı **Google Android Emulator**içinde çalıştırın.
* Uygulama başladıktan sonra beklenmeyen bir bağlantı hatasının gösterildiğini doğrulayın.

Ardından, mobil uygulama arka ucundan kaynakları isteyerek önce kullanıcıların kimliğini doğrulamak üzere uygulamayı güncelleştirin.

## <a name="add-authentication"></a>Uygulamaya kimlik doğrulaması ekleme
1. Projenizi **Visual Studio**'da açın, sonra `www/index.html` dosyasını düzenlenmek üzere açın.
2. Baş bölümünde `Content-Security-Policy` meta etiketini bulun.  OAuth ana bilgisayarını izin verilen kaynaklar listesine ekleyin.

   | Sağlayıcı | SDK sağlayıcı adı | OAuth ana bilgisayar |
   |:--- |:--- |:--- |
   | Azure Active Directory | aad | https://login.microsoftonline.com |
   | Facebook | 'a | https://www.facebook.com |
   | Google | Google | https://accounts.google.com |
   | Microsoft | MicrosoftAccount | https://login.live.com |
   | Twitter | Twitter | https://api.twitter.com |

    Örnek Içerik-Güvenlik-Ilkesi (Azure Active Directory için uygulanır) aşağıdaki gibidir:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    `https://login.microsoftonline.com` önceki tablodaki OAuth ana bilgisayarıyla değiştirin.  Content-Security-Policy meta etiketi hakkında daha fazla bilgi için bkz. [İçerik-güvenlik-Ilke belgeleri].

    Bazı kimlik doğrulama sağlayıcıları, uygun mobil cihazlarda kullanıldığında Içerik Güvenlik Ilkesi değişiklikleri gerektirmez.  Örneğin, bir Android cihazında Google kimlik doğrulaması kullanılırken Içerik Güvenlik Ilkesi değişikliği yapmanız gerekir.

3. Düzenlenmek üzere `www/js/index.js` dosyasını açın, `onDeviceReady()` metodunu bulun ve istemci oluşturma kodu altına aşağıdaki kodu ekleyin:

        // Login to the service
        client.login('SDK_Provider_Name')
            .then(function () {

                // BEGINNING OF ORIGINAL CODE

                // Create a table reference
                todoItemTable = client.getTable('todoitem');

                // Refresh the todoItems
                refreshDisplay();

                // Wire up the UI Event Handler for the Add Item
                $('#add-item').submit(addItemHandler);
                $('#refresh').on('click', refreshDisplay);

                // END OF ORIGINAL CODE

            }, handleError);

    Bu kod, tablo başvurusunu oluşturan mevcut kodu değiştirir ve Kullanıcı arabirimini yeniler.

    Login () yöntemi sağlayıcıyla kimlik doğrulaması başlatır. Login () yöntemi bir JavaScript Promise döndüren zaman uyumsuz bir işlevdir.  Başlatmanın geri kalanı Promise yanıtının içine yerleştirilir, böylece Login () yöntemi tamamlanana kadar yürütülecektir.

4. Yeni eklediğiniz kodda `SDK_Provider_Name`, oturum açma sağlayıcınızın adıyla değiştirin. Örneğin, Azure Active Directory için `client.login('aad')`kullanın.
5. Projenizi çalıştırın.  Projenin başlatılması tamamlandığında, uygulamanız seçilen kimlik doğrulama sağlayıcısı için OAuth oturum açma sayfasını gösterir.

## <a name="next-steps"></a>Sonraki adımlar
* Azure App Service ile [Kimlik doğrulaması hakkında] daha fazla bilgi edinin.
* Apache Cordova uygulamanıza [anında Iletme bildirimleri] ekleyerek öğreticiye devam edin.

SDK'ları kullanmayı öğrenin.

* [Apache Cordova SDK]
* [ASP.NET Sunucusu SDK]
* [Node.js Sunucusu SDK]

<!-- URLs. -->
[Mobile Apps kullanmaya başlayın]: app-service-mobile-cordova-get-started.md
[İçerik-güvenlik-Ilke belgeleri]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Anında İletme Bildirimleri]: app-service-mobile-cordova-get-started-push.md
[Kimlik doğrulaması hakkında]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Sunucusu SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Sunucusu SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md

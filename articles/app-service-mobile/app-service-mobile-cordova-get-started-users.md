---
title: Apache Cordova üzerinde kimlik doğrulama ekleme
description: Google, Facebook, Twitter ve Microsoft gibi kimlik sağlayıcılarıyla Apache Cordova uygulamanızın kullanıcılarını doğrulamak için Azure Uygulama Hizmeti'ndeki Mobil Uygulamaları nasıl kullanacağınızı öğrenin.
ms.assetid: 10dd6dc9-ddf5-423d-8205-00ad74929f0d
ms.tgt_pltfrm: mobile-html
ms.devlang: javascript
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 3714ce2a8098608851991115aa82afdc00d08a47
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77459428"
---
# <a name="add-authentication-to-your-apache-cordova-app"></a>Apache Cordova uygulamanıza kimlik doğrulama ekleme
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>Özet
Bu öğreticide, desteklenen bir kimlik sağlayıcısı nı kullanarak Apache Cordova'daki todolist quickstart projesine kimlik doğrulamaeklersiniz. Bu öğretici, önce tamamlamanız gereken [Mobil Uygulamalar öğreticisini] başlatın'a dayanmaktadır.

## <a name="register-your-app-for-authentication-and-configure-the-app-service"></a><a name="register"></a>Uygulamanızı kimlik doğrulama için kaydedin ve Uygulama Hizmetini yapılandırın
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

[Benzer adımları gösteren bir video izleyin](https://channel9.msdn.com/series/Azure-connected-services-with-Cordova/Azure-connected-services-task-8-Azure-authentication)

## <a name="restrict-permissions-to-authenticated-users"></a><a name="permissions"></a>İzinleri kimlik doğrulaması verilen kullanıcılarla sınırlama
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Şimdi, arka uca anonim erişimin devre dışı bırakıldığını doğrulayabilirsiniz. Görsel Stüdyo'da:

* Öğreticiyi tamamladığınızda oluşturduğunuz projeyi açın [Mobil Uygulamalar ile başlayın.]
* Uygulamanızı Google **Android Emülatörü'nde**çalıştırın.
* Uygulama başladıktan sonra Beklenmeyen Bağlantı Hatası nın gösterildiğini doğrulayın.

Ardından, Mobil Uygulama arka ucundan kaynak istemeden önce kullanıcıların kimliğini doğrulamak için uygulamayı güncelleştirin.

## <a name="add-authentication-to-the-app"></a><a name="add-authentication"></a>Uygulamaya kimlik doğrulama ekleme
1. **Visual Studio'da**projenizi açın, ardından düzenleme için dosyayı `www/index.html` açın.
2. Baş `Content-Security-Policy` bölümündeki meta etiketibulun.  OAuth ana bilgisayarını izin verilen kaynaklar listesine ekleyin.

   | Sağlayıcı | SDK Sağlayıcı Adı | OAuth Ev Sahibi |
   |:--- |:--- |:--- |
   | Azure Active Directory | Acar | https://login.microsoftonline.com |
   | Facebook | Facebook | https://www.facebook.com |
   | Google | Google | https://accounts.google.com |
   | Microsoft | microsoftaccount | https://login.live.com |
   | Twitter | Twitter | https://api.twitter.com |

    Örnek bir İçerik-Güvenlik-İlkes (Azure Etkin Dizin için uygulanan) aşağıdaki gibidir:

        <meta http-equiv="Content-Security-Policy" content="default-src 'self'
            data: gap: https://login.microsoftonline.com https://yourapp.azurewebsites.net; style-src 'self'">

    Önceki `https://login.microsoftonline.com` tablodan OAuth ana bilgisayar ile değiştirin.  İçerik-güvenlik-ilke meta etiketi hakkında daha fazla bilgi için [İçerik-Güvenlik-İlke belgelerine]bakın.

    Bazı kimlik doğrulama sağlayıcıları, uygun mobil cihazlarda kullanıldığında İçerik-Güvenlik-İlke değişiklikleri gerektirmez.  Örneğin, bir Android cihazda Google kimlik doğrulaması kullanırken İçerik-Güvenlik-İlke değişikliği gerekmez.

3. Düzenleme `www/js/index.js` için dosyayı `onDeviceReady()` açın, yöntemi bulun ve istemci oluşturma kodu altında aşağıdaki kodu ekleyin:

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

    Bu kod, tablo başvurularını oluşturan varolan kodun yerine geçive UI'yi yeniler.

    Giriş() yöntemi sağlayıcı ile kimlik doğrulama başlar. Giriş() yöntemi, JavaScript Promise döndüren bir async işlevidir.  Başlatmanın geri kalanı, oturum açma() yöntemi tamamlanana kadar yürütülmemesi için söz yanıtı içine yerleştirilir.

4. Az önce eklediğiniz kodda, oturum açma sağlayıcınızın adıyla değiştirin. `SDK_Provider_Name` Örneğin, Azure Etkin Dizini `client.login('aad')`için .
5. Projenizi çalıştırın.  Proje başlatma tamamlandığında, uygulamanız seçilen kimlik doğrulama sağlayıcısının OAuth giriş sayfasını gösterir.

## <a name="next-steps"></a><a name="next-steps"></a>Sonraki Adımlar
* Azure Uygulama Hizmeti ile [Kimlik Doğrulama hakkında] daha fazla bilgi edinin.
* Apache Cordova uygulamanıza [Anında Iletme Bildirimleri] ekleyerek öğreticiye devam edin.

SDK'ları kullanmayı öğrenin.

* [Apache Cordova SDK]
* [ASP.NET Sunucusu SDK]
* [Node.js Sunucusu SDK]

<!-- URLs. -->
[Mobil Uygulamalarla başlayın]: app-service-mobile-cordova-get-started.md
[İçerik-Güvenlik-Politika dokümantasyonu]: https://cordova.apache.org/docs/en/latest/guide/appdev/whitelist/index.html
[Anında İletme Bildirimleri ]: app-service-mobile-cordova-get-started-push.md
[Kimlik Doğrulama Hakkında]: app-service-mobile-auth.md
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Sunucusu SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Sunucusu SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md

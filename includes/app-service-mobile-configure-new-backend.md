---
title: include dosyası
description: include dosyası
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 04/15/2020
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: b5b6389491fd91b716a05e7c57c98addb6352bd9
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81461945"
---
1. Aşağıdaki platformlar için istemci SDK quickstarts indirin:
    
    [iOS (Amaç-C)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS)  
    [iOS (Swift)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/iOS-Swift)  
    [Android (Java)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/android)  
    [Xamarin.iOS](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.iOS)  
    [Xamarin.Android](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.android)  
    [Xamarin.Forms](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/xamarin.forms)  
    [Cordova](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/cordova)  
    [Pencereler (C#)](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/client/windows-uwp-cs)  

    > [!NOTE]
    > iOS projesini kullanıyorsanız, [en son GitHub sürümünden](https://github.com/Azure/azure-mobile-apps-ios-client/releases/latest)\*"azuresdk-iOS- .zip" indirmeniz gerekir. Zip'i açın `MicrosoftAzureMobile.framework` ve dosyayı projenin köküne ekleyin.
    >

2. Bir veritabanı bağlantısı eklemeniz veya varolan bir bağlantıya bağlanmanız gerekir. İlk olarak, bir veri deposu oluşturup oluşturmayacağınızı veya varolan bir tane kullanıp kullanmayacağınıbelirleyin.

    - **Yeni bir veri deposu oluşturun**: Bir veri deposu oluşturacaksanız, aşağıdaki hızlı başlatmayı kullanın:

        [Quickstart: Azure SQL Veritabanı'nda tek veritabanları yla başlarken](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-quickstart-guide)

    - **Varolan veri kaynağı**: Varolan bir veritabanı bağlantısını kullanmak istiyorsanız aşağıdaki yönergeleri izleyin

        1. SQL Veritabanı Bağlantı String formatı -`Data Source=tcp:{your_SQLServer},{port};Initial Catalog={your_catalogue};User ID={your_username};Password={your_password}`

           **{your_SQLServer}** Sunucunun adı, bu veritabanınız için genel bakış sayfasında bulunabilir ve genellikle "server_name.database.windows.net" şeklindedir.
            **{port}** genellikle 1433.
            **{your_catalogue}** Veritabanının adı.
            **{your_username}** Veritabanınıza erişmek için kullanıcı adı.
            **{your_password}** Veritabanınıza erişmek için parola.

            [SQL Connection String formatı hakkında daha fazla bilgi edinin](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-string-syntax#sqlclient-connection-strings)

        2. **Mobil uygulamanıza** bağlantı dizesini ekleyin Uygulama Hizmeti'nde, menüdeki **Yapılandırma** seçeneğini kullanarak uygulamanızın bağlantı dizelerini yönetebilirsiniz.

            Bağlantı dizesi eklemek için:

            1. **Uygulama ayarları** sekmesine tıklayın.

            2. **[+] Yeni bağlantı dizesi**tıklayın.

            3. Bağlantı dizeniz için **Ad,** **Değer** ve **Tür** sağlamanız gerekir.

            4. **Tür Adı** olarak`MS_TableConnectionString`

            5. Değer, daha önce adımda oluşturduğunuz bağlantı dizesi olmalıdır.

            6. SQL Azure veritabanına bağlantı dizesi **ekliyorsanız, SQLAzure'u yazın**altında seçin. **SQLAzure**

3. Azure Mobile Apps'ta .NET ve Node.js arka uçları için SDK'lar vardır.

   - **Düğüm.js arka uç**
    
     Node.js quickstart uygulamasını kullanacaksanız, aşağıdaki talimatları uygulayın.
     
        1. Yeni bir API oluşturun - Azure portalında değişiklikleri doğru yapabilir veya geliştirme ortamınızda kodu yerel olarak değiştirebilir ve ardından Azure'da yayımlayabilirsiniz. Uygulama kodunuz için tarayıcı içi düzenleme deneyimi sağlayan `App Service Editor (Preview)` alt `Development Tools` menüye tıklayın.
        
        2. Tıklayın `Go` ve Uygulama Hizmeti Düzenleyicisi açıldıktan sonra kaynak kodu üzerinde tam kontrole sahip siniz. NPM install komutuyla ekspres ve azure-mobile-apps paketini yüklediğinizi varsayarsak, özel API oluşturmak veya yeniden oluşturmak için WWWROOT'un altındaki api klasörünü tıklatın. Kod dosyasında değişiklik yapın ve değişiklikler otomatik olarak kaydedilir.
        
        3. Uygulama verilerini depolamak için kullanılan Azure SQL Veritabanı'nda tam denetime sahipsiniz. Veritabanında kolayca yeni tablolar oluşturabilirsiniz.
 
   - **.NET arka ucu**
    
        .NET quickstart uygulamasını kullanacaksanız, aşağıdaki talimatları uygulayın.

        1. Azure Mobil Uygulamalar .NET sunucu projesini [azure-mobile-apps-quickstarts deposundan](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/backend/dotnet/Quickstart)indirin.

        2. .NET sunucu projesini Visual Studio'da yerel olarak oluşturun.

        3. Visual Studio'da Çözüm Gezgini'ni `ZUMOAPPNAMEService` açın, projeye sağ tıklayın, **Yayımla'yı**tıklatın, bir `Publish to App Service` pencere göreceksiniz. Mac üzerinde çalışıyorsanız, uygulamayı [burada](https://docs.microsoft.com/azure/app-service/deploy-local-git)dağıtmanın diğer yollarına göz atın.
        
           ![Görsel stüdyo yayıncılığı](./media/app-service-mobile-configure-new-backend/visual-studio-publish.png)

        4. Hedef olarak **Uygulama Hizmetini** seçin, ardından **Varolan'ı Seç'i**tıklatın ve pencerenin altındaki **Yayımla** düğmesini tıklatın.

        5. Önce Azure aboneliğinizle Visual Studio'ya giriş yapmanız gerekir. 'yi `Subscription` `Resource Group`ve uygulamanızın adını seçin. Hazır olduğunuzda, **Tamam'ı**tıklatın, bu uygulama hizmeti arka ucuna yerel olarak sahip olduğunuz .NET sunucu projesini dağıtacaktır. Dağıtım bittiğinde, tarayıcıya `http://{zumoappname}.azurewebsites.net/` yönlendirilirsiniz.                   

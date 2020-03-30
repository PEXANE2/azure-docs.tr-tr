---
title: include dosyası
description: include dosyası
services: app-service\mobile
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 05/06/2019
ms.author: crdun
ms.custom: include file
ms.openlocfilehash: a7e543dcad9ad1b016d1244451cd87cda5ad7492
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67440206"
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

     1. Azure portalında Kolay **Tablolar'a**gidin, bu ekranı göreceksiniz.
      
        ![Düğüm Kolay Tablolar](./media/app-service-mobile-configure-new-backend/node-easy-tables.png)

     2. SQL bağlantı dizesinin **Yapılandırma** sekmesine zaten eklenmiştir. Daha sonra **bunun tüm site içeriğinin üzerine yazacağını kabul ettiğim** kutusunu işaretleyin ve **TodoItem tablo oluştur** düğmesini tıklatın.
     
        ![Düğüm Kolay Tablolar Yapılandırması](./media/app-service-mobile-configure-new-backend/node-easy-tables-configuration.png)

     3. **Kolay Tablolar'da** **+ Ekle** düğmesini tıklatın.
    
        ![Düğüm Kolay Tablolar Ekle Düğmesi](./media/app-service-mobile-configure-new-backend/node-easy-tables-add.png)

     4. Anonim `TodoItem` erişimi olan bir tablo oluşturun.
      
        ![Düğüm Kolay Tablolar Tablo Ekle](./media/app-service-mobile-configure-new-backend/node-easy-tables-table-add.png)

   - **.NET arka ucu**
    
        .NET quickstart uygulamasını kullanacaksanız, aşağıdaki talimatları uygulayın.

        1. Azure Mobil Uygulamalar .NET sunucu projesini [azure-mobile-apps-quickstarts deposundan](https://github.com/Azure/azure-mobile-apps-quickstarts/tree/master/backend/dotnet/Quickstart)indirin.

        2. .NET sunucu projesini Visual Studio'da yerel olarak oluşturun.

        3. Visual Studio'da Çözüm Gezgini'ni `ZUMOAPPNAMEService` açın, projeye sağ tıklayın, **Yayımla'yı**tıklatın, bir `Publish to App Service` pencere göreceksiniz. Mac üzerinde çalışıyorsanız, uygulamayı [burada](https://docs.microsoft.com/azure/app-service/deploy-local-git)dağıtmanın diğer yollarına göz atın.
        
           ![Görsel stüdyo yayıncılığı](./media/app-service-mobile-configure-new-backend/visual-studio-publish.png)

        4. Hedef olarak **Uygulama Hizmetini** seçin, ardından **Varolan'ı Seç'i**tıklatın ve pencerenin altındaki **Yayımla** düğmesini tıklatın.

        5. Önce Azure aboneliğinizle Visual Studio'ya giriş yapmanız gerekir. 'yi `Subscription` `Resource Group`ve uygulamanızın adını seçin. Hazır olduğunuzda, **Tamam'ı**tıklatın, bu uygulama hizmeti arka ucuna yerel olarak sahip olduğunuz .NET sunucu projesini dağıtacaktır. Dağıtım bittiğinde, tarayıcıya `http://{zumoappname}.azurewebsites.net/` yönlendirilirsiniz.                   
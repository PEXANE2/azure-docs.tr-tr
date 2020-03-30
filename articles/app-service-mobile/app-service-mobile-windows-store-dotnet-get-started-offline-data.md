---
title: UWP uygulamanız için çevrimdışı eşitleme yi etkinleştirme
description: Evrensel Windows Platformu (UWP) uygulamanızda çevrimdışı verileri önbelleğe almak ve senkronize etmek için Azure Mobil Uygulamasını nasıl kullanacağınızı öğrenin.
ms.assetid: 8fe51773-90de-4014-8a38-41544446d9b5
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 236d4c199a13b02bcd82ae02657bbd35e45f729b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77458826"
---
# <a name="enable-offline-sync-for-your-windows-app"></a>Windows uygulamanız için çevrimdışı eşitlemeyi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, Azure Mobil Uygulaması arka ucunu kullanarak Evrensel Windows Platformu (UWP) uygulamasına çevrimdışı desteği nasıl ekleyeceğinizi gösterir. Çevrimdışı eşitleme, ağ bağlantısı olmasa bile son kullanıcıların bir mobil uygulamayla görüntülemesine, eklemesine veya değiştirerek etkileşimkurmasına olanak tanır. Değişiklikler yerel bir veritabanında depolanır. Aygıt yeniden çevrimiçi olduğunda, bu değişiklikler uzak arka uçla senkronize edilir.

Bu eğitimde, Azure Mobil Uygulamaları'nın çevrimdışı özelliklerini desteklemek için [Bir Windows uygulaması oluştur öğreticisinden] UWP uygulama projesini güncellersiniz. İndirilen hızlı başlangıç sunucusu projesini kullanmıyorsanız, projenize veri erişim uzantısı paketlerini eklemeniz gerekir. Sunucu uzantı paketleri hakkında daha fazla bilgi için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)'ya bakın.

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi edinmek için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitleme]konusuna bakın.

## <a name="requirements"></a>Gereksinimler  
Bu öğretici aşağıdaki ön koşulları gerektirir:

* Visual Studio 2013, Windows 8.1 veya sonraki saatlerde çalışır.
* Windows [uygulaması oluşturma]tamamlandıbir[windows uygulaması oluşturun.]
* [Azure Mobil Hizmetler SQLite Store][sqlite store nuget]
* [Evrensel Windows Platformu geliştirme için SQLite](https://marketplace.visualstudio.com/items?itemName=SQLiteDevelopmentTeam.SQLiteforUniversalWindowsPlatform) 

## <a name="update-the-client-app-to-support-offline-features"></a>Çevrimdışı özellikleri desteklemek için istemci uygulamasını güncelleştirin
Azure Mobile App çevrimdışı özellikleri, çevrimdışı bir senaryodayken yerel bir veritabanıyla etkileşimkurmanızı sağlar. Uygulamanızda bu özellikleri kullanmak için bir [SyncContext][synccontext] to a local store. Then reference your table through the [IMobileServiceSyncTable][IMobileServiceSyncTable] arabirimini başlatmanız gerekir. SQLite, aygıtta yerel mağaza olarak kullanılır.

1. Evrensel [Windows Platformu için SQLite çalışma süresini yükleyin.](https://sqlite.org/2016/sqlite-uwp-3120200.vsix)
2. Visual Studio'da, [Windows uygulaması oluştur] öğreticisinde tamamladığınız UWP uygulama projesi için NuGet paket yöneticisini açın.
    **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet paketini arayın ve yükleyin.
3. Solution Explorer'da, Sağ tıkla Referans Ekle > **Referans...** > **Evrensel Windows** > **Uzantıları,** ardından evrensel **Windows Platformu için SQLite'ı** ve **Evrensel Windows Platformu uygulamaları için Visual C++ 2015 Çalışma Süresini**etkinleştirin. **References**

    ![SQLite UWP başvurusu ekle][1]
4. MainPage.xaml.cs dosyasını `#define OFFLINE_SYNC_ENABLED` açın ve tanımı açıklamayı bırakın.
5. Visual Studio'da, istemci uygulamasını yeniden oluşturmak ve çalıştırmak için **F5** tuşuna basın. Uygulama, çevrimdışı eşitlemeyi etkinleştirmeden önce olduğu gibi çalışır. Ancak, yerel veritabanı artık çevrimdışı senaryoda kullanılabilecek verilerle doldurulur.

## <a name="update-the-app-to-disconnect-from-the-backend"></a><a name="update-sync"></a>Arka uçtan bağlantısını kesmek için uygulamayı güncelleştirin
Bu bölümde, çevrimdışı bir durumu simüle etmek için Mobil Uygulama arka uçbağlantınızı kırarsınız. Veri öğeleri eklediğinizde, özel durum işleyiciniz uygulamanın çevrimdışı modda olduğunu bildirir. Bu durumda, yerel mağazaya eklenen yeni öğeler, bağlı bir durumda bir sonraki çalıştırma olduğunda mobil uygulama arka ucuna eşitlenir.

1. Paylaşılan projede App.xaml.cs'yi edin. **MobileServiceClient'ın** başlatılmasını yorumlayın ve geçersiz bir mobil uygulama URL'si kullanan aşağıdaki satırı ekleyin:

         public static MobileServiceClient MobileService = new MobileServiceClient("https://your-service.azurewebsites.fail");

    Ayrıca, aygıttaki wifi ve hücresel ağları devre dışı bırakarak çevrimdışı davranışı gösterebilir veya uçak modunu kullanabilirsiniz.
2. Uygulamayı oluşturmak ve çalıştırmak için **F5** tuşuna basın. Uygulama başlatıldığında yenilemede eşitlemenin başarısız olduğunu fark edin.
3. Yeni öğeler girin ve **kaydet'i**her tıklattığınızda [İptal EdilenByNetworkError] durumuyla itme başarısız olur. Ancak, yeni todo öğeleri mobil uygulama arka ucuna itilene kadar yerel mağazada var.  Bir üretim uygulamasında, bu özel durumları bastırırsanız, istemci uygulaması hala mobil uygulama arka ucuna bağlı gibi hareket eder.
4. Uygulamayı kapatın ve oluşturduğunuz yeni öğelerin yerel mağazada kalıcı olduğunu doğrulamak için uygulamayı yeniden başlatın.
5. (İsteğe bağlı) Visual Studio'da **Sunucu Gezgini'ni**açın. **Azure**->**SQL Veritabanları'nda**veritabanınıza gidin. Veritabanınıza sağ tıklayın ve **SQL Server Object Explorer'da Aç'ı**seçin. Artık SQL veritabanı tablonuza ve içeriğine göz atabilirsiniz. Arka uç veritabanındaki verilerin değişmediğini doğrulayın.
6. (İsteğe bağlı) Formdaki `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`GET sorgusunu kullanarak mobil arka uçunuzu sorgulamak için Kemancı veya Postacı gibi bir REST aracı kullanın.

## <a name="update-the-app-to-reconnect-your-mobile-app-backend"></a><a name="update-online-app"></a>Mobil Uygulama arka uçunuzu yeniden bağlamak için uygulamayı güncelleştirin
Bu bölümde, uygulamayı mobil uygulama arka ucuna yeniden bağlarsınız. Bu değişiklikler, uygulamadaki ağ yeniden bağlantısını simüle edebilir.

Uygulamayı ilk çalıştırdığınızda, `OnNavigatedTo` olay işleyicisi çağırır. `InitLocalStoreAsync` Bu yöntem sırayla yerel mağazanızı arka uç veritabanıyla eşitlemek için çağırır. `SyncAsync` Uygulama başlangıç ta eşitlemeye çalışır.

1. Paylaşılan projede App.xaml.cs açın ve doğru mobil `MobileServiceClient` uygulama URL'sini kullanmak için önceki başlangıç uygulamanızı açıklamayın.
2. Uygulamayı yeniden oluşturmak ve çalıştırmak için **F5** tuşuna basın. Uygulama, `OnNavigatedTo` olay işleyicisi yürütürken itme ve çekme işlemlerini kullanarak yerel değişikliklerinizi Azure Mobil Uygulaması arka ucuyla eşitler.
3. (İsteğe bağlı) GÜNCELLENMİş veriyi SQL Server Object Explorer veya Fiddler gibi bir REST aracını kullanarak görüntüleyin. Verilerin Azure Mobile App arka uç veritabanı ile yerel mağaza arasında senkronize edildiğine dikkat edin.
4. Uygulamada, yerel mağazada tamamlamak için birkaç öğenin yanındaki onay kutusunu tıklatın.

   `UpdateCheckedTodoItem`tamamlanan `SyncAsync` her öğeyi Mobil Uygulama arka ucuyla eşitlemek için çağrılar. `SyncAsync`hem itme hem de çekme çağırır. Ancak, **istemcinin değişiklik yaptığı bir tabloya karşı çekme uyguladığınız da, bir itme her zaman otomatik olarak gerçekleştirilir.** Bu davranış, ilişkilerle birlikte yerel mağazadaki tüm tabloların tutarlı kalmasını sağlar. Bu davranış beklenmeyen bir itme neden olabilir.  Bu davranış hakkında daha fazla bilgi için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitleme'ye]bakın.

## <a name="api-summary"></a>API Özeti
Mobil hizmetlerin çevrimdışı özelliklerini desteklemek için, [iMobileServiceSyncTable] arabirimini kullandık ve yerel bir SQLite veritabanıyla [MobileServiceClient.SyncContext'ı][synccontext] kullandık. Çevrimdışı yken, Mobil Uygulamalar için normal CRUD işlemleri, işlemler yerel mağazaya karşı gerçekleşirken uygulama hala bağlı yatmış gibi çalışır. Yerel mağazayı sunucuyla eşitlemek için aşağıdaki yöntemler kullanılır:

* **[PushAsync]** Bu yöntem [IMobileServicesSyncContext]üyesi olduğundan, tüm tablolardaki değişiklikler arka uca itilir. Yalnızca yerel değişiklikleri olan kayıtlar sunucuya gönderilir.
* **[PullAsync]** Bir çekme [iMobileServiceSyncTable]başlatılır. Tabloda izlenen değişiklikler olduğunda, ilişkilerle birlikte yerel mağazadaki tüm tabloların tutarlı kalmasını sağlamak için örtülü bir itme çalıştırılır. *PushOtherTables* parametresi, bağlamdaki diğer tabloların örtülü bir itme de itilip itmediğini denetler. Sorgu *query* parametresi, döndürülen verileri filtrelemek için [bir IMobileServiceTableQuery\<T>][IMobileServiceTableQuery] veya OData sorgu dizesini alır. *QueryId* parametresi artımlı eşitleme tanımlamak için kullanılır. Daha fazla bilgi için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitleme'ye](app-service-mobile-offline-data-sync.md#how-sync-works)bakın.
* **[PurgeAsync]** Uygulamanız, yerel mağazadaki eski verileri temizlemek için düzenli aralıklarla bu yöntemi aramalıdır. Henüz eşitlenmemiş değişiklikleri temizlemeniz gerektiğinde *kuvvet* parametresini kullanın.

Bu kavramlar hakkında daha fazla bilgi için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitleme'ye](app-service-mobile-offline-data-sync.md#how-sync-works)bakın.

## <a name="more-info"></a>Daha fazla bilgi
Aşağıdaki konular Mobil Uygulamalar'ın çevrimdışı eşitleme özelliği hakkında ek arka plan bilgileri sağlar:

* [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]
* [Azure Mobil Uygulamalar .NET SDK HOWTO][8]

<!-- Anchors. -->
[Update the app to support offline features]: #enable-offline-app
[Update the sync behavior of the app]: #update-sync
[Update the app to reconnect your Mobile Apps backend]: #update-online-app
[Next Steps]:#next-steps

<!-- Images -->
[1]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-reference-sqlite-dialog.png
[11]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/app-service-mobile-add-wp81-reference-sqlite-dialog.png
[13]: ./media/app-service-mobile-windows-store-dotnet-get-started-offline-data/cpu-architecture.png


<!-- URLs. -->
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md
[windows uygulaması oluşturma]: app-service-mobile-windows-store-dotnet-get-started.md
[SQLite for Windows 8.1]: https://go.microsoft.com/fwlink/?LinkID=716919
[SQLite for Windows Phone 8.1]: https://go.microsoft.com/fwlink/?LinkID=716920
[SQLite for Windows 10]: https://go.microsoft.com/fwlink/?LinkID=716921
[synccontext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[sqlite store nuget]: https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client.SQLiteStore/
[iMobileServiceSyncTable]: https://msdn.microsoft.com/library/azure/mt691742(v=azure.10).aspx
[IMobileServiceTableQuery]: https://msdn.microsoft.com/library/azure/dn250631(v=azure.10).aspx
[iMobileServicesSyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynccontext(v=azure.10).aspx
[MobileServicePushFailedException]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushfailedexception(v=azure.10).aspx
[Status]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushcompletionresult.status(v=azure.10).aspx
[İptal EdilenByNetworkHatası]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.mobileservicepushstatus(v=azure.10).aspx
[PullAsync]: https://msdn.microsoft.com/library/azure/mt667558(v=azure.10).aspx
[PushAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileservicesynccontextextensions.pushasync(v=azure.10).aspx
[PurgeAsync]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable.purgeasync(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md

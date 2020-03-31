---
title: Çevrimdışı eşitleme (Xamarin Android) etkinleştirme
description: Xamarin Android uygulamanızda çevrimdışı verileri önbelleğe almak ve senkronize etmek için App Service Mobile App'ı nasıl kullanacağınızı öğrenin.
ms.assetid: 91d59e4b-abaa-41f4-80cf-ee7933b32568
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 4faab572cd77e20bb61d147ca6e58b25cd88ad9f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77458877"
---
# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Xamarin.Android mobil uygulamanız için çevrimdışı eşitleme yi etkinleştirme

[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış

Bu öğretici, Xamarin.Android için Azure Mobil Uygulamaları'nın çevrimdışı eşitleme özelliğini sunar. Çevrimdışı eşitleme, ağ bağlantısı olmasa bile son kullanıcıların bir mobil uygulamayla görüntülemesine, eklemesine veya değiştirerek etkileşimkurmasına olanak tanır. Değişiklikler yerel bir veritabanında depolanır.
Aygıt yeniden çevrimiçi olduğunda, bu değişiklikler uzak hizmetle eşitlenir.

Bu eğitimde, Azure Mobil Uygulamaları'nın çevrimdışı özelliklerini desteklemek için [bir Xamarin Android uygulaması oluştur özelliğinden] istemci projesini güncellersiniz. İndirilen hızlı başlangıç sunucusu projesini kullanmıyorsanız, projenize veri erişim uzantısı paketlerini eklemeniz gerekir. Sunucu uzantı paketleri hakkında daha fazla bilgi için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)'ya bakın.

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi edinmek için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitleme]konusuna bakın.

## <a name="update-the-client-app-to-support-offline-features"></a>Çevrimdışı özellikleri desteklemek için istemci uygulamasını güncelleştirin

Azure Mobile App çevrimdışı özellikleri, çevrimdışı bir senaryodayken yerel bir veritabanıyla etkileşimkurmanızı sağlar. Uygulamanızda bu özellikleri kullanmak için, yerel bir mağazaya [bir Eşitleme Bağlamı] başlatılmasını sağlarsınız. Ardından [tablonuza IMobileServiceSyncTable](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable?view=azure-dotnet) arabirimi üzerinden başvurun. SQLite, aygıtta yerel mağaza olarak kullanılır.

1. Visual Studio'da, [Xamarin Android uygulama] oluşturma öğreticisinde tamamladığınız projede NuGet paket yöneticisini açın.  **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet paketini arayın ve yükleyin.
2. ToDoActivity.cs dosyasını `#define OFFLINE_SYNC_ENABLED` açın ve tanımı açıklamayı bırakın.
3. Visual Studio'da, istemci uygulamasını yeniden oluşturmak ve çalıştırmak için **F5** tuşuna basın. Uygulama, çevrimdışı eşitlemeyi etkinleştirmeden önce olduğu gibi çalışır. Ancak, yerel veritabanı artık çevrimdışı senaryoda kullanılabilecek verilerle doldurulur.

## <a name="update-the-app-to-disconnect-from-the-backend"></a><a name="update-sync"></a>Arka uçtan bağlantısını kesmek için uygulamayı güncelleştirin

Bu bölümde, çevrimdışı bir durumu simüle etmek için Mobil Uygulama arka uçbağlantınızı kırarsınız. Veri öğeleri eklediğinizde, özel durum işleyiciniz uygulamanın çevrimdışı modda olduğunu bildirir. Bu durumda, yerel mağazaya eklenen yeni öğeler ve bağlı bir durumda bir itme yürütüldüğünde mobil uygulama arka ucuna eşitlenir.

1. Paylaşılan projede ToDoActivity.cs'yi edin. Geçersiz bir URL'ye işaret etmek için **uygulama URL'sini** değiştirin:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Ayrıca, aygıttaki wifi ve hücresel ağları devre dışı bırakarak veya uçak modunu kullanarak çevrimdışı davranışı da gösterebilirsiniz.
2. Uygulamayı oluşturmak ve çalıştırmak için **F5** tuşuna basın. Uygulama başlatıldığında yenilemede eşitlemenin başarısız olduğunu fark edin.
3. Yeni öğeler girin ve **kaydet'i**her tıklattığınızda itme nin [CanceledByNetworkError] durumuyla başarısız olduğunu fark edin. Ancak, yeni todo öğeleri mobil uygulama arka ucuna itilene kadar yerel mağazada var.  Bir üretim uygulamasında, bu özel durumları bastırırsanız, istemci uygulaması hala mobil uygulama arka ucuna bağlı gibi hareket eder.
4. Uygulamayı kapatın ve oluşturduğunuz yeni öğelerin yerel mağazada kalıcı olduğunu doğrulamak için uygulamayı yeniden başlatın.
5. (İsteğe bağlı) Visual Studio'da **Sunucu Gezgini'ni**açın. **Azure**->**SQL Veritabanları'nda**veritabanınıza gidin. Veritabanınıza sağ tıklayın ve **SQL Server Object Explorer'da Aç'ı**seçin. Artık SQL veritabanı tablonuza ve içeriğine göz atabilirsiniz. Arka uç veritabanındaki verilerin değişmediğini doğrulayın.
6. (İsteğe bağlı) Formdaki `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`GET sorgusunu kullanarak mobil arka uçunuzu sorgulamak için Kemancı veya Postacı gibi bir REST aracı kullanın.

## <a name="update-the-app-to-reconnect-your-mobile-app-backend"></a><a name="update-online-app"></a>Mobil Uygulama arka uçunuzu yeniden bağlamak için uygulamayı güncelleştirin

Bu bölümde, uygulamayı mobil uygulama arka ucuna yeniden bağlayın. Uygulamayı ilk çalıştırdığınızda, `OnCreate` olay işleyicisi çağırır. `OnRefreshItemsSelected` Bu yöntem, yerel mağazanızı arka uç veritabanıyla eşitlemeyi çağırır. `SyncAsync`

1. Paylaşılan projede ToDoActivity.cs açın ve **applicationURL** özelliğindeki değişikliğinizi tersine çevirin.
2. Uygulamayı yeniden oluşturmak ve çalıştırmak için **F5** tuşuna basın. Uygulama, `OnRefreshItemsSelected` yöntem çalıştırıldığında itme ve çekme işlemlerini kullanarak yerel değişikliklerinizi Azure Mobil Uygulaması arka ucuyla senkronize eder.
3. (İsteğe bağlı) GÜNCELLENMİş veriyi SQL Server Object Explorer veya Fiddler gibi bir REST aracını kullanarak görüntüleyin. Verilerin Azure Mobile App arka uç veritabanı ile yerel mağaza arasında senkronize edildiğine dikkat edin.
4. Uygulamada, yerel mağazada tamamlamak için birkaç öğenin yanındaki onay kutusunu tıklatın.

   `CheckItem`tamamlanan `SyncAsync` her öğeyi Mobil Uygulama arka ucuyla eşitlemek için çağrılar. `SyncAsync`hem itme hem de çekme çağırır. **İstemcinin değişiklik yaptığı bir tabloya karşı çekme uyguladığınız da, bir itme her zaman otomatik olarak gerçekleştirilir.** Bu, ilişkilerle birlikte yerel mağazadaki tüm tabloların tutarlı kalmasını sağlar. Bu davranış beklenmeyen bir itme neden olabilir. Bu davranış hakkında daha fazla bilgi için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitleme'ye]bakın.

## <a name="review-the-client-sync-code"></a>İstemci eşitleme kodunu gözden geçirme

Öğreticiyi tamamladığınızda indirdiğiniz Xamarin istemci projesi [Bir Xamarin Android uygulaması oluştur,] yerel bir SQLite veritabanı nı kullanarak çevrimdışı senkronizasyonu destekleyen kodlar içeriyor. Burada zaten öğretici kod da yer almaktadır ne kısa bir bakış. Özelliğin kavramsal bir özeti için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitle'ye]bakın.

* Herhangi bir tablo işlemi gerçekleştirilemeden önce, yerel mağazanın başlatılması gerekir. Yürütüldüğünde `ToDoActivity.OnCreate()` yerel mağaza veritabanı `ToDoActivity.InitLocalStoreAsync()`baş harfe işlenir. Bu yöntem, Azure Mobil Uygulamalar istemcisi SDK tarafından sağlanan `MobileServiceSQLiteStore` sınıfı kullanarak yerel bir SQLite veritabanı oluşturur.

    Yöntem, `DefineTable` bu durumda, `ToDoItem` yerel mağazada sağlanan türdeki alanlarla eşleşen bir tablo oluşturur. Tür, uzak veritabanında bulunan tüm sütunları içermelidir. Sütunların yalnızca bir alt kümesini depolamak mümkündür.

        // ToDoActivity.cs
        private async Task InitLocalStoreAsync()
        {
            // new code to initialize the SQLite store
            string path = Path.Combine(System.Environment
                .GetFolderPath(System.Environment.SpecialFolder.Personal), localDbFilename);

            if (!File.Exists(path))
            {
                File.Create(path).Dispose();
            }

            var store = new MobileServiceSQLiteStore(path);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            // To use a different conflict handler, pass a parameter to InitializeAsync.
            // For more details, see https://go.microsoft.com/fwlink/?LinkId=521416.
            await client.SyncContext.InitializeAsync(store);
        }
* Üye `toDoTable` yerine `ToDoActivity` `IMobileServiceSyncTable` `IMobileServiceTable`türündedir. IMobileServiceSyncTable tüm oluşturma, okuma, güncelleme ve silme (CRUD) tablo işlemlerini yerel mağaza veritabanına yönlendirir.

    Değişikliklerin Azure Mobil Uygulaması'na ne zaman itildiğine' i arayarak `IMobileServiceSyncContext.PushAsync()`siz karar verirsiniz. Eşitleme bağlamı, istemci uygulaması çağrıldığında `PushAsync` değiştirdiği tüm tablolardaki değişiklikleri izleyerek ve iterek tablo ilişkilerini korumaya yardımcı olur.

    Sağlanan kod, `ToDoActivity.SyncAsync()` todoitem listesi yenilendiğinde veya bir todoitem eklendiğinde veya tamamlandığında eşitlenmeye çağrır. Kod, her yerel değişiklikten sonra eşitler.

    Sağlanan kodda, uzak `TodoItem` tablodaki tüm kayıtlar sorgulanır, ancak sorgu kimliği ve sorguyu ' `PushAsync`ya geçirerek kayıtları filtrelemek de mümkündür. Daha fazla bilgi için Azure Mobil [Uygulamalarında Çevrimdışı Veri Eşitleme bölümünde] *artımlı eşitleme* bölümüne bakın.

        // ToDoActivity.cs
        private async Task SyncAsync()
        {
            try {
                await client.SyncContext.PushAsync();
                await toDoTable.PullAsync("allTodoItems", toDoTable.CreateQuery()); // query ID is used for incremental sync
            } catch (Java.Net.MalformedURLException) {
                CreateAndShowDialog (new Exception ("There was an error creating the Mobile Service. Verify the URL"), "Error");
            } catch (Exception e) {
                CreateAndShowDialog (e, "Error");
            }
        }

## <a name="additional-resources"></a>Ek Kaynaklar

* [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]
* [Azure Mobil Uygulamalar .NET SDK HOWTO][8]

<!-- URLs. -->
[Xamarin Android uygulaması oluşturma]: ./app-service-mobile-xamarin-android-get-started.md
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: ./app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Xamarin Android uygulaması oluşturma]: app-service-mobile-xamarin-android-get-started.md
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: https://xamarin.com/download
[Xamarin extension]: https://xamarin.com/visual-studio
[Eşitleme Bağlamı]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md

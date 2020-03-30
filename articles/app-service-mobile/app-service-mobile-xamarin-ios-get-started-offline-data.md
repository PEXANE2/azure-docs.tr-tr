---
title: Çevrimdışı eşitleme (Xamarin iOS) etkinleştirme
description: Xamarin iOS uygulamanızda çevrimdışı verileri önbelleğe almak ve senkronize etmek için App Service Mobile App'ı nasıl kullanacağınızı öğrenin.
ms.assetid: 828a287c-5d58-4540-9527-1309ebb0f32b
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 3a5128f6918b22be2ff1ef6adf3e453b1f373ea6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461309"
---
# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Xamarin.iOS mobil uygulamanız için çevrimdışı eşitleme yi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, Xamarin.iOS için Azure Mobil Uygulamaları'nın çevrimdışı eşitleme özelliğini sunar. Çevrimdışı eşitleme, ağ bağlantısı olmasa bile son kullanıcıların bir mobil uygulamayla görüntülemesine, eklemesine veya değiştirerek etkileşimkurmasına olanak tanır. Değişiklikler yerel bir veritabanında depolanır. Aygıt yeniden çevrimiçi olduğunda, bu değişiklikler uzak hizmetle eşitlenir.

Bu eğitimde, Azure Mobil Uygulamaları'nın çevrimdışı özelliklerini desteklemek için [Xamarin iOS uygulaması oluştur uygulamasından Xamarin.iOS] uygulama projesini güncelleyin. İndirilen hızlı başlangıç sunucusu projesini kullanmıyorsanız, projenize veri erişim uzantısı paketlerini eklemeniz gerekir. Sunucu uzantı paketleri hakkında daha fazla bilgi için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)'ya bakın.

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi edinmek için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitleme]konusuna bakın.

## <a name="update-the-client-app-to-support-offline-features"></a>Çevrimdışı özellikleri desteklemek için istemci uygulamasını güncelleştirin
Azure Mobile App çevrimdışı özellikleri, çevrimdışı bir senaryodayken yerel bir veritabanıyla etkileşimkurmanızı sağlar. Uygulamanızda bu özellikleri kullanmak için, yerel bir mağazaya [Bir Eşitleme] Bağlamı'nı başlatma. [IMobileServiceSyncTable] arabirimi üzerinden tablonuza başvurun. SQLite, aygıtta yerel mağaza olarak kullanılır.

1. [Xamarin iOS uygulama] öğreticisi oluştur'da tamamladığınız projede NuGet paket yöneticisini açın, ardından **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet paketini arayın ve yükleyin.
2. QSTodoService.cs dosyasını `#define OFFLINE_SYNC_ENABLED` açın ve tanımı açıklamayı bırakın.
3. İstemci uygulamasını yeniden oluşturve çalıştırın. Uygulama, çevrimdışı eşitlemeyi etkinleştirmeden önce olduğu gibi çalışır. Ancak, yerel veritabanı artık çevrimdışı senaryoda kullanılabilecek verilerle doldurulur.

## <a name="update-the-app-to-disconnect-from-the-backend"></a><a name="update-sync"></a>Arka uçtan bağlantısını kesmek için uygulamayı güncelleştirin
Bu bölümde, çevrimdışı bir durumu simüle etmek için Mobil Uygulama arka uçbağlantınızı kırarsınız. Veri öğeleri eklediğinizde, özel durum işleyiciniz uygulamanın çevrimdışı modda olduğunu bildirir. Bu durumda, yerel mağazaya eklenen yeni öğeler, bağlı bir durumda bir sonraki çalıştırma olduğunda mobil uygulama arka ucuna eşitlenir.

1. Paylaşılan projede QSToDoService.cs'yi edin. Geçersiz bir URL'ye işaret etmek için **uygulama URL'sini** değiştirin:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Ayrıca, aygıttaki wifi ve hücresel ağları devre dışı bırakarak veya uçak modunu kullanarak çevrimdışı davranışı da gösterebilirsiniz.
2. Uygulamayı derleyin ve çalıştırın. Uygulama başlatıldığında yenilemede eşitlemenin başarısız olduğunu fark edin.
3. Yeni öğeler girin ve **kaydet'i**her tıklattığınızda itme nin [CanceledByNetworkError] durumuyla başarısız olduğunu fark edin. Ancak, yeni todo öğeleri mobil uygulama arka ucuna itilene kadar yerel mağazada var.  Bir üretim uygulamasında, bu özel durumları bastırırsanız, istemci uygulaması hala mobil uygulama arka ucuna bağlı gibi hareket eder.
4. Uygulamayı kapatın ve oluşturduğunuz yeni öğelerin yerel mağazada kalıcı olduğunu doğrulamak için uygulamayı yeniden başlatın.
5. (İsteğe bağlı) Visual Studio bir bilgisayarda yüklüyse, **Server Explorer'ı**açın. **Azure**-> **SQL Veritabanları'nda**veritabanınıza gidin. Veritabanınıza sağ tıklayın ve **SQL Server Object Explorer'da Aç'ı**seçin. Artık SQL veritabanı tablonuza ve içeriğine göz atabilirsiniz. Arka uç veritabanındaki verilerin değişmediğini doğrulayın.
6. (İsteğe bağlı) Formdaki `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`GET sorgusunu kullanarak mobil arka uçunuzu sorgulamak için Kemancı veya Postacı gibi bir REST aracı kullanın.

## <a name="update-the-app-to-reconnect-your-mobile-app-backend"></a><a name="update-online-app"></a>Mobil Uygulama arka uçunuzu yeniden bağlamak için uygulamayı güncelleştirin
Bu bölümde, uygulamayı mobil uygulama arka ucuna yeniden bağlayın. Bu, uygulamanın çevrimdışı bir durumdan mobil uygulama arka ucuyla çevrimiçi bir duruma geçmesini simüle eder.   Ağ bağlantısını kapatarak ağ kırılmasını simüle ettiyseniz, kod değişikliği gerekmez.
Ağı yeniden açın.  Uygulamayı ilk çalıştırdığınızda, `RefreshDataAsync` yöntem çağrılır. Bu da `SyncAsync` yerel mağazanızı arka uç veritabanıyla eşitlemek için çağırır.

1. Paylaşılan projede QSToDoService.cs açın ve **applicationURL** özelliğindeki değişikliğinizi tersine çevirin.
2. Uygulamayı yeniden oluşturve çalıştırın. Uygulama, `OnRefreshItemsSelected` yöntem çalıştırıldığında itme ve çekme işlemlerini kullanarak yerel değişikliklerinizi Azure Mobil Uygulaması arka ucuyla senkronize eder.
3. (İsteğe bağlı) GÜNCELLENMİş veriyi SQL Server Object Explorer veya Fiddler gibi bir REST aracını kullanarak görüntüleyin. Verilerin Azure Mobile App arka uç veritabanı ile yerel mağaza arasında senkronize edildiğine dikkat edin.
4. Uygulamada, yerel mağazada tamamlamak için birkaç öğenin yanındaki onay kutusunu tıklatın.

   `CompleteItemAsync`tamamlanan `SyncAsync` her öğeyi Mobil Uygulama arka ucuyla eşitlemek için çağrılar. `SyncAsync`hem itme hem de çekme çağırır.
   **İstemcinin değişiklik yaptığı bir tabloya karşı çekme uyguladığınız da, istemci eşitleme bağlamına yönelik bir itme her zaman önce otomatik olarak gerçekleştirilir.** Örtük itme, ilişkilerle birlikte yerel mağazadaki tüm tabloların tutarlı kalmasını sağlar. Bu davranış hakkında daha fazla bilgi için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitleme'ye]bakın.

## <a name="review-the-client-sync-code"></a>İstemci eşitleme kodunu gözden geçirme
Öğreticiyi tamamladığınızda indirdiğiniz Xamarin istemci projesi [Bir Xamarin iOS uygulaması oluştur,] yerel bir SQLite veritabanı nı kullanarak çevrimdışı senkronizasyonu destekleyen kodlar içeriyor. Burada zaten öğretici kod da yer almaktadır ne kısa bir bakış. Özelliğin kavramsal bir özeti için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitle'ye]bakın.

* Herhangi bir tablo işlemi gerçekleştirilemeden önce, yerel mağazanın başlatılması gerekir. Yürütüldüğünde `QSTodoListViewController.ViewDidLoad()` yerel mağaza veritabanı `QSTodoService.InitializeStoreAsync()`baş harfe işlenir. Bu yöntem, Azure Mobil Uygulama istemcisi SDK tarafından sağlanan `MobileServiceSQLiteStore` sınıfı kullanarak yeni bir yerel SQLite veritabanı oluşturur.

    Yöntem, `DefineTable` bu durumda, `ToDoItem` yerel mağazada sağlanan türdeki alanlarla eşleşen bir tablo oluşturur. Tür, uzak veritabanında bulunan tüm sütunları içermelidir. Sütunların yalnızca bir alt kümesini depolamak mümkündür.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }
* Üye `todoTable` yerine `QSTodoService` `IMobileServiceSyncTable` `IMobileServiceTable`türündedir. IMobileServiceSyncTable tüm oluşturma, okuma, güncelleme ve silme (CRUD) tablo işlemlerini yerel mağaza veritabanına yönlendirir.

    Bu değişikliklerin Azure Mobil Uygulaması'na ne zaman `IMobileServiceSyncContext.PushAsync()`itildiğine' i arayarak siz karar verirsiniz. Eşitleme bağlamı, istemci uygulaması çağrıldığında `PushAsync` değiştirdiği tüm tablolardaki değişiklikleri izleyerek ve iterek tablo ilişkilerini korumaya yardımcı olur.

    Sağlanan kod, `QSTodoService.SyncAsync()` todoitem listesi yenilendiğinde veya bir todoitem eklendiğinde veya tamamlandığında eşitlenmeye çağrır. Uygulama her yerel değişiklikten sonra eşitler. Bir çekme, bağlam tarafından izlenen yerel güncelleştirmeleri bekleyen bir tabloya karşı yürütülürse, bu çekme işlemi önce otomatik olarak bir bağlam itme işlemini tetikler.

    Sağlanan kodda, uzak `TodoItem` tablodaki tüm kayıtlar sorgulanır, ancak sorgu kimliği ve sorguyu ' `PushAsync`ya geçirerek kayıtları filtrelemek de mümkündür. Daha fazla bilgi için Azure Mobil [Uygulamalarında Çevrimdışı Veri Eşitleme bölümünde] *artımlı eşitleme* bölümüne bakın.

        // QSTodoService.cs
        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]
* [Azure Mobil Uygulamalar .NET SDK HOWTO][8]

<!-- Images -->

<!-- URLs. -->
[Xamarin iOS uygulaması oluşturma]: app-service-mobile-xamarin-ios-get-started.md
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md
[Eşitleme Bağlamı]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md

---
title: Çevrimdışı eşitlemeyi etkinleştir (Xamarin iOS)
description: App Service mobil uygulamayı kullanarak Xamarin iOS uygulamanızdaki çevrimdışı verileri önbelleğe alma ve eşitleme hakkında bilgi edinin.
ms.assetid: 828a287c-5d58-4540-9527-1309ebb0f32b
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 3a5128f6918b22be2ff1ef6adf3e453b1f373ea6
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77461309"
---
# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>Xamarin. iOS mobil uygulamanız için çevrimdışı eşitlemeyi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğreticide, Xamarin. iOS için Azure Mobile Apps çevrimdışı eşitleme özelliği tanıtılmıştır. Çevrimdışı eşitleme son kullanıcıların bir mobil uygulamayla etkileşime geçmesini sağlar--ağ bağlantısı olmasa bile verileri görüntüleme, ekleme veya değiştirme. Değişiklikler yerel bir veritabanında depolanır. Cihaz yeniden çevrimiçi olduktan sonra, bu değişiklikler uzak hizmetle eşitlenir.

Bu öğreticide, Azure Mobile Apps çevrimdışı özelliklerini desteklemek için Xamarin. iOS uygulama projesini [Xamarin iOS uygulaması oluşturma] ' dan güncelleştirin. İndirilen hızlı başlangıç sunucusu projesini kullanmazsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Sunucu Uzantısı paketleri hakkında daha fazla bilgi için bkz. [Azure için .net arka uç sunucu SDK 'sı Mobile Apps çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi edinmek için [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]konusuna bakın.

## <a name="update-the-client-app-to-support-offline-features"></a>Çevrimdışı özellikleri desteklemek için istemci uygulamasını güncelleştirme
Azure mobil uygulaması çevrimdışı özellikleri, çevrimdışı bir senaryonuz olduğunda yerel bir veritabanıyla etkileşime girebilmeniz için izin verir. Uygulamanızda bu özellikleri kullanmak için yerel bir depoya [syncContext] başlatın. [Imobileservicesynctable] arabirimi aracılığıyla tablonuza başvurun. SQLite, cihazdaki yerel depo olarak kullanılır.

1. [Xamarin iOS uygulaması oluşturma] öğreticisinde tamamladığınız projede NuGet Paket Yöneticisi ' ni açın, sonra **Microsoft. Azure. Mobile. Client. SQLiteStore** NuGet paketini arayın ve bu uygulamayı yükleyebilirsiniz.
2. QSTodoService.cs dosyasını açın ve `#define OFFLINE_SYNC_ENABLED` tanımının açıklamasını kaldırın.
3. İstemci uygulamasını yeniden derleyin ve çalıştırın. Uygulama, çevrimdışı eşitleme 'yi etkinleştirmeden önceki ile aynı şekilde çalışacak. Ancak, yerel veritabanı artık çevrimdışı bir senaryoda kullanılabilecek verilerle doldurulmuştur.

## <a name="update-sync"></a>Arka uca bağlantıyı kesmek için uygulamayı güncelleştirme
Bu bölümde, çevrimdışı bir durumun benzetimini yapmak için mobil uygulama arka ucunuza olan bağlantıyı kesebilirsiniz. Veri öğeleri eklediğinizde, özel durum işleyiciniz uygulamanın çevrimdışı modda olduğunu söyler. Bu durumda, yerel depoya eklenen yeni öğeler, bağlı bir durumda gönderim bir sonraki çalıştırıldığında mobil uygulama arka ucuna eşitlenir.

1. Paylaşılan projedeki QSToDoService.cs 'yi düzenleyin. **ApplicationUrl** 'yi GEÇERSIZ bir URL 'ye işaret etmek için değiştirin:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Ayrıca, cihazdaki WiFi ve hücresel ağları devre dışı bırakarak veya uçak modunu kullanarak çevrimdışı davranışı da belirtebilirsiniz.
2. Uygulamayı derleyin ve çalıştırın. Uygulama başlatıldığında eşitlemenin yenileme işlemi başarısız olduğuna dikkat edin.
3. Yeni öğeleri girin ve **Kaydet**' e tıkladığınızda gönderim bir [CancelledByNetworkError] durumuyla başarısız olur. Ancak, yeni Todo öğeleri, mobil uygulama arka ucuna itilene kadar yerel depoda bulunur.  Bir üretim uygulamasında, bu özel durumları bastırdığınızda, istemci uygulaması hala mobil uygulama arka ucuna bağlı gibi davranır.
4. Uygulamayı kapatın ve oluşturduğunuz yeni öğelerin yerel depoda kalıcı olduğunu doğrulamak için yeniden başlatın.
5. Seçim Bilgisayarınızda Visual Studio yüklüyse, **Sunucu Gezgini**açın. **Azure**-> **SQL veritabanlarında**veritabanınıza gidin. Veritabanınıza sağ tıklayın ve **SQL Server Nesne Gezgini aç**' ı seçin. Artık SQL veritabanı tablonuza ve içindekilere gidebilirsiniz. Arka uç veritabanındaki verilerin değiştirilmediğini doğrulayın.
6. Seçim `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`form içindeki bir GET sorgusu kullanarak mobil arka ucunuzu sorgulamak için Fiddler veya Postman gibi bir REST aracı kullanın.

## <a name="update-online-app"></a>Mobil uygulama arka ucunuzu yeniden bağlamak için uygulamayı güncelleştirin
Bu bölümde, uygulamayı mobil uygulama arka ucuna yeniden bağlayın. Bu, mobil uygulama arka ucu ile çevrimdışı bir durumdan çevrimiçi duruma geçme uygulamanın benzetimini yapar.   Ağ bağlantısını devre dışı bırakarak ağ ayırıcısını sanal yaparsanız, kod değişikliği gerekmez.
Ağı tekrar açın.  Uygulamayı ilk kez çalıştırdığınızda `RefreshDataAsync` yöntemi çağrılır. Bu da, yerel deponuzi arka uç veritabanıyla eşitlemek için `SyncAsync` çağırır.

1. Paylaşılan projede QSToDoService.cs ' i açın ve **ApplicationUrl** özelliğinin değişikliğini yeniden alın.
2. Uygulamayı yeniden derleyin ve çalıştırın. Uygulama, `OnRefreshItemsSelected` yöntemi yürütüldüğünde gönderme ve çekme işlemlerini kullanarak yerel değişikliklerinizi Azure Mobil uygulama arka ucuna eşitler.
3. Seçim SQL Server Nesne Gezgini veya Fiddler gibi bir REST aracını kullanarak güncelleştirilmiş verileri görüntüleyin. Verilerin Azure Mobil uygulama arka uç veritabanı ve yerel depo arasında eşitlendiğini unutmayın.
4. Uygulamada, bunları yerel depoda tamamlamaya yönelik birkaç öğenin yanındaki onay kutusuna tıklayın.

   `CompleteItemAsync`, her tamamlanmış öğeyi mobil uygulama arka ucuna eşitlemek için `SyncAsync` çağırır. `SyncAsync` hem itme hem de çekme çağırır.
   **İstemcinin üzerinde değişiklik yaptığı bir tabloya karşı bir çekme işlemi yürüttüğünüzde, istemci eşitleme bağlamına gönderim her zaman otomatik olarak yürütülür**. Örtük gönderim, yerel depodaki tüm tabloların ilişkilerle birlikte tutarlı kalmasını sağlar. Bu davranış hakkında daha fazla bilgi için bkz. [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

## <a name="review-the-client-sync-code"></a>İstemci eşitleme kodunu gözden geçirin
[Xamarin iOS uygulaması oluşturma] öğreticisini tamamladığınızda indirdiğiniz Xamarin istemci projesi, yerel bir SQLite veritabanı kullanarak çevrimdışı eşitlemeyi destekleyen kodu zaten içeriyor. Öğretici kodunda zaten nelerin dahil olduğuna ilişkin kısa bir genel bakış aşağıda verilmiştir. Özelliğe kavramsal bir genel bakış için bkz. [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

* Herhangi bir tablo işlemi gerçekleştirilmeden önce, yerel deponun başlatılmış olması gerekir. `QSTodoListViewController.ViewDidLoad()` `QSTodoService.InitializeStoreAsync()`yürütüldüğünde yerel depo veritabanı başlatılır. Bu yöntem, Azure Mobil uygulama istemci SDK 'Sı tarafından sunulan `MobileServiceSQLiteStore` sınıfını kullanarak yeni bir yerel SQLite veritabanı oluşturur.

    `DefineTable` yöntemi, yerel depoda, bu durumda `ToDoItem`, belirtilen türdeki alanlarla eşleşen bir tablo oluşturur. Türün, uzak veritabanındaki tüm sütunları içermesi gerekmez. Yalnızca bir sütun alt kümesini depolamak mümkündür.

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }
* `QSTodoService` `todoTable` üyesi, `IMobileServiceTable`yerine `IMobileServiceSyncTable` türüdür. Imobileservicesynctable, tüm oluşturma, okuma, güncelleştirme ve silme (CRUD) tablosu işlemlerini yerel depo veritabanına yönlendirir.

    `IMobileServiceSyncContext.PushAsync()`çağırarak, bu değişikliklerin Azure Mobil uygulama arka ucuna ne zaman itileceğine karar verirsiniz. Eşitleme bağlamı, `PushAsync` çağrıldığında bir istemci uygulamasının değiştirdiği tüm tablolardaki değişiklikleri izleyerek ve ileterek Tablo ilişkilerinin korunmasına yardımcı olur.

    Belirtilen kod, TodoItem listesi yenilendiğinde veya bir TodoItem eklendiğinde veya tamamlandığında eşitlenecek `QSTodoService.SyncAsync()` çağırır. Uygulama, her yerel değişiklikten sonra eşitlenir. Bir çekme, bağlam tarafından izlenen bekleyen yerel güncellemeleri olan bir tabloya karşı yürütülürse, bu çekme işlemi önce otomatik olarak bir bağlam gönderimi tetikler.

    Belirtilen kodda, uzak `TodoItem` tablosundaki tüm kayıtlar sorgulanır, ancak `PushAsync`bir sorgu kimliği ve sorgu geçirerek kayıtların filtreleneceği de mümkündür. Daha fazla bilgi için bkz. [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme] *artımlı eşitleme* bölümü.

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
* [Azure Mobile Apps .NET SDK 'Sı nasıl yapılır][8]

<!-- Images -->

<!-- URLs. -->
[Xamarin iOS uygulaması oluşturma]: app-service-mobile-xamarin-ios-get-started.md
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md

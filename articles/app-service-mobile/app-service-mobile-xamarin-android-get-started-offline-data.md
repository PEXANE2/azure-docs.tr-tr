---
title: Çevrimdışı eşitlemeyi etkinleştir (Xamarin Android)
description: App Service mobil uygulamayı kullanarak Xamarin Android uygulamanızda çevrimdışı verileri önbelleğe alma ve eşitleme hakkında bilgi edinin.
ms.assetid: 91d59e4b-abaa-41f4-80cf-ee7933b32568
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: c1119a803f160c1833cd87413263b868928b82c2
ms.sourcegitcommit: 3d4917ed58603ab59d1902c5d8388b954147fe50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74668346"
---
# <a name="enable-offline-sync-for-your-xamarinandroid-mobile-app"></a>Xamarin. Android mobil uygulamanız için çevrimdışı eşitlemeyi etkinleştirme

[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

> [!NOTE]
> Visual Studio App Center mobil uygulama dağıtımında merkezi konumdaki uçtan uca ve tümleşik hizmetleri destekler. Geliştiriciler Sürekli Tümleştirme ve Teslim işlem hattını ayarlamak için **Oluşturma**, **Test** ve **Dağıtım** hizmetlerini kullanabilir. Uygulama dağıtıldıktan sonra, geliştiriciler **Analiz** ve **Tanılama** hizmetlerini kullanarak uygulamanın durumunu ve kullanımını izleyebilir, **Gönderme** hizmetini kullanarak kullanıcılarla etkileşim kurabilir. Geliştiriciler ayrıca kullanıcıların kimliğini doğrulamak için **Kimlik Doğrulaması**'ndan ve uygulama verilerini bulutta kalıcı hale getirmek ve eşitlemek için **Veri** hizmetinden yararlanabilir.
>
> Bulut hizmetlerini mobil uygulamanızla tümleştirmek istiyorsanız [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc)'a hemen kaydolun.

## <a name="overview"></a>Genel Bakış

Bu öğreticide, Xamarin. Android için Azure Mobile Apps çevrimdışı eşitleme özelliği tanıtılmıştır. Çevrimdışı eşitleme son kullanıcıların bir mobil uygulamayla etkileşime geçmesini sağlar--ağ bağlantısı olmasa bile verileri görüntüleme, ekleme veya değiştirme. Değişiklikler yerel bir veritabanında depolanır.
Cihaz yeniden çevrimiçi olduktan sonra, bu değişiklikler uzak hizmetle eşitlenir.

Bu öğreticide, Azure Mobile Apps çevrimdışı özelliklerini desteklemek için [Xamarin Android uygulaması oluşturma] öğreticisindeki istemci projesini güncelleştirin. İndirilen hızlı başlangıç sunucusu projesini kullanmazsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Sunucu Uzantısı paketleri hakkında daha fazla bilgi için bkz. [Azure için .net arka uç sunucu SDK 'sı Mobile Apps çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi edinmek için [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]konusuna bakın.

## <a name="update-the-client-app-to-support-offline-features"></a>Çevrimdışı özellikleri desteklemek için istemci uygulamasını güncelleştirme

Azure mobil uygulaması çevrimdışı özellikleri, çevrimdışı bir senaryonuz olduğunda yerel bir veritabanıyla etkileşime girebilmeniz için izin verir. Uygulamanızda bu özellikleri kullanmak için yerel bir depoya [syncContext] ' i başlatırsınız. Ardından, [ımobileservicesynctable](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.mobileservices.sync.imobileservicesynctable?view=azure-dotnet) arabirimi aracılığıyla tablonuza başvurun. SQLite, cihazdaki yerel depo olarak kullanılır.

1. Visual Studio 'da, [Xamarin Android uygulaması oluşturma] öğreticisinde tamamladığınız projede NuGet Paket Yöneticisi ' ni açın.  **Microsoft. Azure. Mobile. Client. SQLiteStore** NuGet paketini arayın ve yükler.
2. ToDoActivity.cs dosyasını açın ve `#define OFFLINE_SYNC_ENABLED` tanımının açıklamasını kaldırın.
3. Visual Studio 'da, istemci uygulamasını yeniden derlemek ve çalıştırmak için **F5** tuşuna basın. Uygulama, çevrimdışı eşitleme 'yi etkinleştirmeden önceki ile aynı şekilde çalışacak. Ancak, yerel veritabanı artık çevrimdışı bir senaryoda kullanılabilecek verilerle doldurulmuştur.

## <a name="update-sync"></a>Arka uca bağlantıyı kesmek için uygulamayı güncelleştirme

Bu bölümde, çevrimdışı bir durumun benzetimini yapmak için mobil uygulama arka ucunuza olan bağlantıyı kesebilirsiniz. Veri öğeleri eklediğinizde, özel durum işleyiciniz uygulamanın çevrimdışı modda olduğunu söyler. Bu durumda, yerel depoya eklenen yeni öğeler, bağlı durumda bir gönderim yürütüldüğünde mobil uygulama arka ucuna eşitlenir.

1. Paylaşılan projedeki ToDoActivity.cs 'yi düzenleyin. **ApplicationUrl** 'yi GEÇERSIZ bir URL 'ye işaret etmek için değiştirin:

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    Ayrıca, cihazdaki WiFi ve hücresel ağları devre dışı bırakarak veya uçak modunu kullanarak çevrimdışı davranışı da belirtebilirsiniz.
2. Uygulamayı derlemek ve çalıştırmak için **F5** tuşuna basın. Uygulama başlatıldığında eşitlemenin yenileme işlemi başarısız olduğuna dikkat edin.
3. Yeni öğeleri girin ve **Kaydet**' e tıkladığınızda gönderim bir [CancelledByNetworkError] durumuyla başarısız olur. Ancak, yeni Todo öğeleri, mobil uygulama arka ucuna itilene kadar yerel depoda bulunur.  Bir üretim uygulamasında, bu özel durumları bastırdığınızda, istemci uygulaması hala mobil uygulama arka ucuna bağlı gibi davranır.
4. Uygulamayı kapatın ve oluşturduğunuz yeni öğelerin yerel depoda kalıcı olduğunu doğrulamak için yeniden başlatın.
5. Seçim Visual Studio 'da **Sunucu Gezgini**açın. **Azure**->**SQL veritabanlarında**veritabanınıza gidin. Veritabanınıza sağ tıklayın ve **SQL Server Nesne Gezgini aç**' ı seçin. Artık SQL veritabanı tablonuza ve içindekilere gidebilirsiniz. Arka uç veritabanındaki verilerin değiştirilmediğini doğrulayın.
6. Seçim `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem`form içindeki bir GET sorgusu kullanarak mobil arka ucunuzu sorgulamak için Fiddler veya Postman gibi bir REST aracı kullanın.

## <a name="update-online-app"></a>Mobil uygulama arka ucunuzu yeniden bağlamak için uygulamayı güncelleştirin

Bu bölümde, uygulamayı mobil uygulama arka ucuna yeniden bağlayın. Uygulamayı ilk kez çalıştırdığınızda, `OnCreate` olay işleyicisi `OnRefreshItemsSelected`çağırır. Bu yöntem, yerel deponuzi arka uç veritabanıyla eşitlemek için `SyncAsync` çağırır.

1. Paylaşılan projede ToDoActivity.cs ' i açın ve **ApplicationUrl** özelliğinin değişikliğini yeniden alın.
2. Uygulamayı yeniden derlemek ve çalıştırmak için **F5** tuşuna basın. Uygulama, `OnRefreshItemsSelected` yöntemi yürütüldüğünde gönderme ve çekme işlemlerini kullanarak yerel değişikliklerinizi Azure Mobil uygulama arka ucuna eşitler.
3. Seçim SQL Server Nesne Gezgini veya Fiddler gibi bir REST aracını kullanarak güncelleştirilmiş verileri görüntüleyin. Verilerin Azure Mobil uygulama arka uç veritabanı ve yerel depo arasında eşitlendiğini unutmayın.
4. Uygulamada, bunları yerel depoda tamamlamaya yönelik birkaç öğenin yanındaki onay kutusuna tıklayın.

   `CheckItem`, her tamamlanmış öğeyi mobil uygulama arka ucuna eşitlemek için `SyncAsync` çağırır. `SyncAsync` hem itme hem de çekme çağırır. **İstemcinin üzerinde değişiklik yaptığı bir tabloya karşı bir çekme işlemi yürüttüğünüzde, her zaman otomatik olarak bir gönderim yürütülür**. Bu, yerel depodaki tüm tabloların ilişkilerle birlikte tutarlı kalmasını sağlar. Bu davranış beklenmedik bir gönderim oluşmasına neden olabilir. Bu davranış hakkında daha fazla bilgi için bkz. [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

## <a name="review-the-client-sync-code"></a>İstemci eşitleme kodunu gözden geçirin

[Xamarin Android uygulaması oluşturma] öğreticisini tamamladığınızda indirdiğiniz Xamarin istemci projesi, yerel bir SQLite veritabanı kullanarak çevrimdışı eşitlemeyi destekleyen kodu zaten içeriyor. Öğretici kodunda zaten nelerin dahil olduğuna ilişkin kısa bir genel bakış aşağıda verilmiştir. Özelliğe kavramsal bir genel bakış için bkz. [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme].

* Herhangi bir tablo işlemi gerçekleştirilmeden önce, yerel deponun başlatılmış olması gerekir. `ToDoActivity.OnCreate()` `ToDoActivity.InitLocalStoreAsync()`yürütüldüğünde yerel depo veritabanı başlatılır. Bu yöntem, Azure Mobile Apps istemci SDK 'Sı tarafından sunulan `MobileServiceSQLiteStore` sınıfını kullanarak yerel bir SQLite veritabanı oluşturur.

    `DefineTable` yöntemi, yerel depoda, bu durumda `ToDoItem`, belirtilen türdeki alanlarla eşleşen bir tablo oluşturur. Türün, uzak veritabanındaki tüm sütunları içermesi gerekmez. Yalnızca bir sütun alt kümesini depolamak mümkündür.

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
* `ToDoActivity` `toDoTable` üyesi, `IMobileServiceTable`yerine `IMobileServiceSyncTable` türüdür. Imobileservicesynctable, tüm oluşturma, okuma, güncelleştirme ve silme (CRUD) tablosu işlemlerini yerel depo veritabanına yönlendirir.

    `IMobileServiceSyncContext.PushAsync()`çağırarak Azure Mobil uygulama arka ucuna yapılan değişikliklerin ne zaman gönderileceğine karar verirsiniz. Eşitleme bağlamı, `PushAsync` çağrıldığında bir istemci uygulamasının değiştirdiği tüm tablolardaki değişiklikleri izleyerek ve ileterek Tablo ilişkilerinin korunmasına yardımcı olur.

    Belirtilen kod, TodoItem listesi yenilendiğinde veya bir TodoItem eklendiğinde veya tamamlandığında eşitlenecek `ToDoActivity.SyncAsync()` çağırır. Kod, her yerel değişiklikten sonra eşitlenir.

    Belirtilen kodda, uzak `TodoItem` tablosundaki tüm kayıtlar sorgulanır, ancak `PushAsync`bir sorgu kimliği ve sorgu geçirerek kayıtların filtreleneceği de mümkündür. Daha fazla bilgi için bkz. [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme] *artımlı eşitleme* bölümü.

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
* [Azure Mobile Apps .NET SDK 'Sı nasıl yapılır][8]

<!-- URLs. -->
[Xamarin Android uygulaması oluşturma]: ./app-service-mobile-xamarin-android-get-started.md
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: ./app-service-mobile-offline-data-sync.md

<!-- Images -->

<!-- URLs. -->
[Xamarin Android uygulaması oluşturma]: app-service-mobile-xamarin-android-get-started.md
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md
[Xamarin Studio]: https://xamarin.com/download
[Xamarin extension]: https://xamarin.com/visual-studio
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md

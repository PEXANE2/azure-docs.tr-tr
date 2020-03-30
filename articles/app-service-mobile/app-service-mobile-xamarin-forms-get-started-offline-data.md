---
title: Çevrimdışı eşitleme (Xamarin.Forms) | Microsoft Dokümanlar
description: Xamarin.Forms uygulamanızda çevrimdışı verileri önbelleğe almak ve senkronize etmek için App Service Mobile App'ı nasıl kullanacağınızı öğrenin.
ms.assetid: acf0f874-3ea5-4410-bd22-b0e72140f3b5
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 57776073ef7e2760b308df22280faf1d65b8d104
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77458860"
---
# <a name="enable-offline-sync-for-your-xamarinforms-mobile-app"></a>Xamarin.Forms mobil uygulamanız için çevrimdışı eşitleme'yi etkinleştirin
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, Xamarin.Forms için Azure Mobil Uygulamaları'nın çevrimdışı eşitleme özelliğini sunar. Çevrimdışı eşitleme, ağ bağlantısı olmasa bile son kullanıcıların bir mobil uygulamayla görüntülemesine, eklemesine veya değiştirerek etkileşimkurmasına olanak tanır. Değişiklikler yerel bir veritabanında depolanır. Aygıt yeniden çevrimiçi olduğunda, bu değişiklikler uzak hizmetle eşitlenir.

Bu öğretici, öğreticiyi tamamladığınızda oluşturduğunuz Mobil Uygulamalar için Xamarin.Forms quickstart çözümüne dayanmaktadır [Xamarin iOS uygulaması oluşturun]. Xamarin.Forms için hızlı başlangıç çözümü, çevrimdışı eşitlemeyi destekleyen ve etkinleştirilmesi gereken kodu içerir. Bu eğitimde, Azure Mobil Uygulamaları'nın çevrimdışı özelliklerini açmak için hızlı başlatma çözümünün güncellenir. Ayrıca uygulamada çevrimdışıözel kodu da vurgularız. İndirilen hızlı başlatma çözümlerini kullanmıyorsanız, projenize veri erişim uzantısı paketlerini eklemeniz gerekir. Sunucu uzantı paketleri hakkında daha fazla bilgi için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışma][1]'ya bakın.

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi edinmek için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitleme][2]konusuna bakın.

## <a name="enable-offline-sync-functionality-in-the-quickstart-solution"></a>Hızlı başlatma çözümünde çevrimdışı eşitleme işlevini etkinleştirme
Çevrimdışı eşitleme kodu C# önişlemci yönergeleri kullanılarak projeye dahil edilir. ÇEVRIMDıŞı **\_SYNC\_ENABLED** simgesi tanımlandığında, bu kod yolları yapıya dahil edilir. Windows uygulamaları için SQLite platform'u da yüklemeniz gerekir.

1. Visual Studio'da, **Çözüm için NuGet Paketlerini Yönet>** çözüme sağ tıklayın... ardından çözümdeki tüm projeler için **Microsoft.Azure.Mobile.Mobile.Client.SQLiteStore** NuGet paketini arayın ve yükleyin.
2. Çözüm Gezgini'nde, taşınabilir sınıf kitaplığı projesi olan **Portable** adlı projedeki TodoItemManager.cs dosyasını açın ve ardından aşağıdaki önişlemci yönergesinin yorumlarını bırakın:

        #define OFFLINE_SYNC_ENABLED
3. (İsteğe bağlı) Windows aygıtlarını desteklemek için aşağıdaki SQLite çalışma zamanı paketlerinden birini yükleyin:

   * **Windows 8.1 Çalışma Zamanı:** [Windows 8.1 için SQLite'yi][3]yükleyin.
   * **Windows Phone 8.1:** [Windows Phone 8.1 için SQLite'yi][4]yükleyin.
   * **Evrensel Windows Platformu** [Universal Windows Universal için SQLite'yi yükleyin.][5]

     Quickstart evrensel windows projesi içermese de, Evrensel Windows platformu Xamarin Forms ile desteklenir.
4. (İsteğe bağlı) Her Windows uygulaması projesinde, Sağ tıkla Başvuru **Windows** Ekle **Başvuru...** > **Add Reference...**, Windows klasörünü genişlet> **Uzantıları.**
    Windows SDK için **Visual C++ 2013 Çalışma Süresi** ile birlikte Windows SDK için uygun **SQLite'yi** etkinleştirin.
    SQLite SDK adları her Windows platformuna göre biraz farklılık gösterir.

## <a name="review-the-client-sync-code"></a>İstemci eşitleme kodunu gözden geçirme
Burada zaten `#if OFFLINE_SYNC_ENABLED` yönergeler içinde öğretici kod da yer almaktadır ne kısa bir genel bakış. Çevrimdışı eşitleme işlevi, Taşınabilir Sınıf Kitaplığı projesindeki TodoItemManager.cs proje dosyasında dır. Özelliğin kavramsal bir özeti için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitle'ye][2]bakın.

* Herhangi bir tablo işlemi gerçekleştirilemeden önce, yerel mağazanın başlatılması gerekir. Yerel mağaza veritabanı aşağıdaki kod kullanılarak **TodoItemManager** sınıf oluşturucusu olarak başharfe çevrilir:

        var store = new MobileServiceSQLiteStore(OfflineDbPath);
        store.DefineTable<TodoItem>();

        //Initializes the SyncContext using the default IMobileServiceSyncHandler.
        this.client.SyncContext.InitializeAsync(store);

        this.todoTable = client.GetSyncTable<TodoItem>();

    Bu **kod, MobileServiceSQLiteStore** sınıfını kullanarak yeni bir yerel SQLite veritabanı oluşturur.

    **DefineTable** yöntemi, yerel depoda sağlanan türdeki alanlara uyan bir tablo oluşturur.  Tür, uzak veritabanında bulunan tüm sütunları içermelidir. Sütunalt kümesini depolamak mümkündür.
* **TodoItemManager'daki** **todoTable** **alanı, IMobileServiceTable Table**yerine **iMobileServiceSyncTable** türüdür. Bu sınıf, tüm oluşturma, okuma, güncelleştirme ve silme (CRUD) tablo işlemleri için yerel veritabanını kullanır. Bu değişikliklerin Mobil Uygulama'ya ne zaman itildiğine **IMobileServiceSyncContext'da** **PushAsync'i** arayarak karar verirsiniz. Eşitleme bağlamı, **PushAsync** çağrıldığında istemci uygulamasının değiştirdiği tüm tablolardaki değişiklikleri izleyerek ve iterek tablo ilişkilerini korumaya yardımcı olur.

    Aşağıdaki **SyncAsync** yöntemi Mobil Uygulama arka uç ile eşitlemek için denir:

        public async Task SyncAsync()
        {
            ReadOnlyCollection<MobileServiceTableOperationError> syncErrors = null;

            try
            {
                await this.client.SyncContext.PushAsync();

                await this.todoTable.PullAsync(
                    "allTodoItems",
                    this.todoTable.CreateQuery());
            }
            catch (MobileServicePushFailedException exc)
            {
                if (exc.PushResult != null)
                {
                    syncErrors = exc.PushResult.Errors;
                }
            }

            // Simple error/conflict handling.
            if (syncErrors != null)
            {
                foreach (var error in syncErrors)
                {
                    if (error.OperationKind == MobileServiceTableOperationKind.Update && error.Result != null)
                    {
                        //Update failed, reverting to server's copy.
                        await error.CancelAndUpdateItemAsync(error.Result);
                    }
                    else
                    {
                        // Discard local change.
                        await error.CancelAndDiscardItemAsync();
                    }

                    Debug.WriteLine(@"Error executing sync operation. Item: {0} ({1}). Operation discarded.",
                        error.TableName, error.Item["id"]);
                }
            }
        }

    Bu örnekte varsayılan eşitleme işleyicisi ile basit hata işleme kullanır. Gerçek bir uygulama özel bir **IMobileServiceSyncHandler** uygulaması kullanarak ağ koşulları ve sunucu çakışmaları gibi çeşitli hataları ele alacak.

## <a name="offline-sync-considerations"></a>Çevrimdışı eşitleme hususları
Örnekte, **SyncAsync** yöntemi yalnızca başlatma ve eşitleme istendiğinde çağrılır.  Bir Android veya iOS uygulamasında eşitleme başlatmak için öğeler listesini aşağı çekin; Windows için **Eşitle** düğmesini kullanın. Gerçek bir uygulamada, ağ durumu değiştiğinde eşitleme tetikleyicisi de yapabilirsiniz.

Bir çekme, bağlam tarafından izlenen yerel güncelleştirmeleri bekleyen bir tabloya karşı yürütüldüğünde, bu çekme işlemi otomatik olarak önceki bir bağlam itme tetikler. Bu örnekteki öğeleri yenilerken, eklerken ve tamamlarken, açık **PushAsync** çağrısını atlayabilirsiniz.

Sağlanan kodda, uzak TodoItem tablosundaki tüm kayıtlar sorgulanır, ancak bir sorgu kimliği ve sorguyu **PushAsync'e**geçirerek kayıtları filtrelemek de mümkündür. Daha fazla bilgi için Azure Mobil [Uygulamalarında Çevrimdışı Veri Eşitleme bölümünde][2] *artımlı eşitleme* bölümüne bakın.

## <a name="run-the-client-app"></a>İstemci uygulamasını çalıştırma
Çevrimdışı eşitleme şimdi etkinken, yerel mağaza veritabanını doldurmak için istemci uygulamasını her platformda en az bir kez çalıştırın. Daha sonra, çevrimdışı bir senaryoyu simüle edin ve uygulama çevrimdışıyken yerel mağazadaki verileri değiştirin.

## <a name="update-the-sync-behavior-of-the-client-app"></a>İstemci uygulamasının eşitleme davranışını güncelleştirme
Bu bölümde, arka uç için geçersiz bir uygulama URL'si kullanarak çevrimdışı bir senaryo simüle etmek için istemci projesini değiştirin. Alternatif olarak, cihazınızı "Uçak moduna" taşıyarak ağ bağlantılarını kapatabilirsiniz.  Veri öğeleri eklediğinizde veya değiştirdiğinizde, bu değişiklikler yerel mağazada tutulur, ancak bağlantı yeniden kurulana kadar arka uç veri deposuyla eşitlenmez.

1. Çözüm Gezgini'nde, **Taşınabilir** projeden Constants.cs proje dosyasını `ApplicationURL` açın ve geçersiz bir URL'ye işaret etme değerini değiştirin:

        public static string ApplicationURL = @"https://your-service.azurewebsites.net/";
2. **Taşınabilir** projeden TodoItemManager.cs dosyasını açın, ardından denemeiçin taban **Özel Durum** sınıfı için bir **catch** **ekleyin... ** **SyncAsync'de**bloğu yakalamak. Bu **catch** bloğu konsola özel durum iletisini aşağıdaki gibi yazar:

            catch (Exception ex)
            {
                Console.Error.WriteLine(@"Exception: {0}", ex.Message);
            }
3. İstemci uygulamasını oluşturun ve çalıştırın.  Bazı yeni öğeler ekleyin. Arka uçla eşitlemeye yönelik her girişim için konsolda bir özel durum günlüğe kaydedilir. Bu yeni öğeler, mobil arka uça itilene kadar yalnızca yerel mağazada bulunur. İstemci uygulaması, tüm oluşturma, okuma, güncelleme, silme (CRUD) işlemlerini destekleyerek arka uca bağlı gibi davranmaz.
4. Uygulamayı kapatın ve oluşturduğunuz yeni öğelerin yerel mağazada kalıcı olduğunu doğrulamak için uygulamayı yeniden başlatın.
5. (İsteğe bağlı) Arka uç veritabanındaki verilerin değişmediğini görmek için Azure SQL Veritabanı tablonuzu görüntülemek için Visual Studio'yu kullanın.

    Visual Studio'da **Sunucu Gezgini'ni**açın. **Azure**->**SQL Veritabanları'nda**veritabanınıza gidin. Veritabanınıza sağ tıklayın ve **SQL Server Object Explorer'da Aç'ı**seçin. Artık SQL veritabanı tablonuza ve içeriğine göz atabilirsiniz.

## <a name="update-the-client-app-to-reconnect-your-mobile-backend"></a>Mobil arka uçunuzu yeniden bağlamak için istemci uygulamasını güncelleştirin
Bu bölümde, uygulamayı çevrimiçi duruma geri dönen uygulamayı simüle eden mobil arka uça yeniden bağlayın. Yenileme hareketi gerçekleştirdiğinizde, veriler mobil arka uçunuzla senkronize edilir.

1. Constants.cs yeniden açın. Doğru `applicationURL` URL'yi işaret eden idüzeltin.
2. İstemci uygulamasını yeniden oluşturve çalıştırın. Uygulama, başlatıldıktan sonra mobil uygulama arka ucuyla eşitlemeye çalışır. Hata ayıklama konsolunda hiçbir özel durum günlüğe kaydolmadığını doğrulayın.
3. (İsteğe bağlı) GÜNCELLENMİş veriyi SQL Server Object Explorer veya Fiddler veya [Postacı][6]gibi bir REST aracını kullanarak görüntüleyin. Verilerin arka uç veritabanı ile yerel mağaza arasında senkronize edildiğine dikkat edin.

    Verilerin veritabanı ve yerel mağaza arasında senkronize edildiğine ve uygulamanızın bağlantısı kesilirken eklediğiniz öğeleri içerdiğine dikkat edin.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme][2]
* [Azure Mobil Uygulamalar .NET SDK HOWTO][8]

<!-- URLs. -->
[1]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[2]: app-service-mobile-offline-data-sync.md
[3]: https://go.microsoft.com/fwlink/p/?LinkID=716919
[4]: https://go.microsoft.com/fwlink/p/?LinkID=716920
[5]: https://sqlite.org/2016/sqlite-uwp-3120200.vsix
[6]: https://www.getpostman.com/
[7]: https://www.telerik.com/fiddler
[8]: app-service-mobile-dotnet-how-to-use-client-library.md

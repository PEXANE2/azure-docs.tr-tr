---
title: Çevrimdışı eşitleme (Cordova) etkinleştirme
description: Cordova uygulamanızda çevrimdışı verileri önbelleğe almak ve senkronize etmek için App Service Mobile App'ı nasıl kullanacağınızı öğrenin.
ms.assetid: 1a3f685d-f79d-4f8b-ae11-ff96e79e9de9
ms.tgt_pltfrm: mobile-cordova-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 5a2d5ec8da5c1a317039e656f6df884a10efe7c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77459427"
---
# <a name="enable-offline-sync-for-your-cordova-mobile-app"></a>Cordova mobil uygulamanız için çevrimdışı eşitleme yi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, Cordova için Azure Mobil Uygulamaları'nın çevrimdışı eşitleme özelliğini sunar. Çevrimdışı eşitleme, ağ bağlantısı olmasa&mdash;&mdash;bile son kullanıcıların bir mobil uygulamayı görüntüleme, ekleme veya değiştirme ile etkileşimkurmasına olanak tanır. Değişiklikler yerel bir veritabanında depolanır.  Aygıt yeniden çevrimiçi olduğunda, bu değişiklikler uzak hizmetle eşitlenir.

Bu öğretici, [Apache Cordova]hızlı başlangıç eğitimini tamamladığınızda oluşturduğunuz Mobil Uygulamalar için Cordova quickstart çözümüne dayanmaktadır. Bu eğitimde, Azure Mobil Uygulamaları'nın çevrimdışı özelliklerini eklemek için hızlı başlangıç çözümünün güncellenir.  Ayrıca uygulamada çevrimdışıözel kodu da vurgularız.

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi edinmek için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitleme]konusuna bakın. API kullanımıyla ilgili ayrıntılar için [API belgelerine](https://azure.github.io/azure-mobile-apps-js-client)bakın.

## <a name="add-offline-sync-to-the-quickstart-solution"></a>Hızlı başlatma çözümüne çevrimdışı eşitleme ekleme
Çevrimdışı eşitleme kodu uygulamaya eklenmelidir. Çevrimdışı eşitleme, Azure Mobil Uygulamalar eklentisi projeye dahil edildiğinde otomatik olarak uygulamanıza eklenen cordova-sqlite-depolama eklentisini gerektirir. Quickstart projesi bu eklentilerin her ikisini de içerir.

1. Visual Studio'nun Solution Explorer'ında index.js'yi açın ve aşağıdaki kodu değiştirin

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable;      // Reference to a table endpoint on backend

    bu kod ile:

        var client,            // Connection to the Azure Mobile App backend
           todoItemTable,      // Reference to a table endpoint on backend
           syncContext;        // Reference to offline data sync context

2. Ardından, aşağıdaki kodu değiştirin:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');

    bu kod ile:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net');
        var store = new WindowsAzure.MobileServiceSqliteStore('store.db');

        store.defineTable({
          name: 'todoitem',
          columnDefinitions: {
              id: 'string',
              text: 'string',
              complete: 'boolean',
              version: 'string'
          }
        });

        // Get the sync context from the client
        syncContext = client.getSyncContext();

    Önceki kod eklemeleri yerel mağazanın başlatılmasını sağlar ve Azure arka uçunuzda kullanılan sütun değerleriyle eşleşen yerel bir tablo tanımlar. (Tüm sütun değerlerini bu koda eklemeniz gerekmez.)  Alan `version` mobil arka uç tarafından korunur ve çakışma çözümü için kullanılır.

    **getSyncContext'ı**arayarak eşitleme bağlamına bir başvuru alırsınız. Eşitleme bağlamı, istemci uygulaması çağrıldığında `.push()` değiştirdiği tüm tablolardaki değişiklikleri izleyerek ve iterek tablo ilişkilerini korumaya yardımcı olur.

3. Uygulama URL'sini Mobil Uygulama uygulama URL'nize güncelleyin.

4. Ardından, bu kodu değiştirin:

        todoItemTable = client.getTable('todoitem'); // todoitem is the table name

    bu kod ile:

        // Initialize the sync context with the store
        syncContext.initialize(store).then(function () {

        // Get the local table reference.
        todoItemTable = client.getSyncTable('todoitem');

        syncContext.pushHandler = {
            onConflict: function (pushError) {
                // Handle the conflict.
                console.log("Sync conflict! " + pushError.getError().message);
                // Update failed, revert to server's copy.
                pushError.cancelAndDiscard();
              },
              onError: function (pushError) {
                  // Handle the error
                  // In the simulated offline state, you get "Sync error! Unexpected connection failure."
                  console.log("Sync error! " + pushError.getError().message);
              }
        };

        // Call a function to perform the actual sync
        syncBackend();

        // Refresh the todoItems
        refreshDisplay();

        // Wire up the UI Event Handler for the Add Item
        $('#add-item').submit(addItemHandler);
        $('#refresh').on('click', refreshDisplay);

    Önceki kod eşitleme bağlamını başolarak alır ve ardından yerel tabloya başvuruda bulunmak için GetSyncTable 'ı (getTable yerine) çağırır.

    Bu kod, tüm oluşturma, okuma, güncelleştirme ve silme (CRUD) tablo işlemleri için yerel veritabanını kullanır.

    Bu örnek eşitleme çakışmaları üzerinde basit hata işleme gerçekleştirir. Gerçek bir uygulama ağ koşulları, sunucu çakışmaları ve diğerleri gibi çeşitli hataları işler. Kod örnekleri için çevrimdışı [eşitleme örneğine]bakın.

5. Ardından, gerçek eşitleme gerçekleştirmek için bu işlevi ekleyin.

        function syncBackend() {

          // Sync local store to Azure table when app loads, or when login complete.
          syncContext.push().then(function () {
              // Push completed

          });

          // Pull items from the Azure table after syncing to Azure.
          syncContext.pull(new WindowsAzure.Query('todoitem'));
        }

    **SyncContext.push()** numaralı telefonu arayarak Mobil Uygulama arka uçta değişiklikleri ne zaman iteceğine siz karar verirsiniz. Örneğin, eşitleme düğmesine bağlı bir düğme olay işleyicisinde **syncBackend'i** arayabilirsiniz.

## <a name="offline-sync-considerations"></a>Çevrimdışı eşitleme hususları

Örnekte, **eşitleme bağlamı** **itme** yöntemi yalnızca giriş için geri arama işlevinde uygulama başlatma olarak adlandırılır.  Gerçek bir uygulamada, bu eşitleme işlevini el ile veya ağ durumu değiştiğinde de tetikleebilirsiniz.

Bir çekme, bağlam tarafından izlenen yerel güncelleştirmeleri bekleyen bir tabloya karşı yürütüldüğünde, bu çekme işlemi otomatik olarak bir itme tetikler. Bu örnekteki öğeleri yenilerken, eklerken ve tamamlarken, gereksiz olabileceğinden açık **itme** çağrısını atlayabilirsiniz.

Sağlanan kodda, uzak todoItem tablosundaki tüm kayıtlar sorgulanır, ancak bir sorgu id ve sorgu itmek için geçerek kayıtları filtrelemek de **mümkündür.** Daha fazla bilgi için Azure Mobil [Uygulamalarında Çevrimdışı Veri Eşitleme bölümünde] *artımlı eşitleme* bölümüne bakın.

## <a name="optional-disable-authentication"></a>(İsteğe bağlı) Kimlik doğrulamayı devre dışı bırakma

Çevrimdışı eşitlemeyi test etmeden önce kimlik doğrulamasını ayarlamak istemiyorsanız, oturum açmak için geri arama işlevini yorumlayın, ancak kodu geri arama işlevinin içinde yorum yapmadan bırakın.  Giriş satırlarını yorumladıktan sonra, kod aşağıdaki gibidir:

      // Login to the service.
      // client.login('twitter')
      //    .then(function () {
        syncContext.initialize(store).then(function () {
          // Leave the rest of the code in this callback function  uncommented.
                ...
        });
      // }, handleError);

Şimdi, uygulamayı çalıştırdığınızda uygulama Azure arka uçile eşitler.

## <a name="run-the-client-app"></a>İstemci uygulamasını çalıştırma
Çevrimdışı eşitleme şimdi etkinken, istemci uygulamasını her platformda en az bir kez çalıştırarak yerel mağaza veritabanını doldurabilirsiniz. Daha sonra, çevrimdışı bir senaryoyu simüle edin ve uygulama çevrimdışıyken yerel mağazadaki verileri değiştirin.

## <a name="optional-test-the-sync-behavior"></a>(İsteğe bağlı) Eşitleme davranışını test edin
Bu bölümde, arka uç için geçersiz bir uygulama URL'si kullanarak çevrimdışı bir senaryo simüle etmek için istemci projesini değiştirirsiniz. Veri öğeleri eklediğinizde veya değiştirdiğinizde, bu değişiklikler yerel mağazada tutulur, ancak bağlantı yeniden kurulana kadar arka uç veri deposuyla eşitlenmez.

1. Çözüm Gezgini'nde index.js proje dosyasını açın ve uygulama URL'sini aşağıdaki kod gibi geçersiz bir URL'ye işaret etmek üzere değiştirin:

        client = new WindowsAzure.MobileServiceClient('http://yourmobileapp.azurewebsites.net-fail');

2. index.html'de, CSP `<meta>` öğesini aynı geçersiz URL ile güncelleştirin.

        <meta http-equiv="Content-Security-Policy" content="default-src 'self' data: gap: http://yourmobileapp.azurewebsites.net-fail; style-src 'self'; media-src *">

3. İstemci uygulamasını oluşturun ve çalıştırın ve uygulama oturum açtıktan sonra arka uçla eşitlemeye çalıştığında konsolda bir özel durum günlüğe kaydedildiğini fark edin. Eklediğiniz yeni öğeler, mobil arka uça itilene kadar yalnızca yerel mağazada bulunur. İstemci uygulaması arka uca bağlı gibi olur.

4. Uygulamayı kapatın ve oluşturduğunuz yeni öğelerin yerel mağazada kalıcı olduğunu doğrulamak için uygulamayı yeniden başlatın.

5. (İsteğe bağlı) Arka uç veritabanındaki verilerin değişmediğini görmek için Azure SQL Veritabanı tablonuzu görüntülemek için Visual Studio'yu kullanın.

    Visual Studio'da **Sunucu Gezgini'ni**açın. **Azure**->**SQL Veritabanları'nda**veritabanınıza gidin. Veritabanınıza sağ tıklayın ve **SQL Server Object Explorer'da Aç'ı**seçin. Artık SQL veritabanı tablonuza ve içeriğine göz atabilirsiniz.

## <a name="optional-test-the-reconnection-to-your-mobile-backend"></a>(İsteğe bağlı) Mobil arka uçunuza yeniden bağlantı nızı test edin

Bu bölümde, uygulamayı mobil arka uça yeniden bağlarsınız, bu da uygulamanın çevrimiçi bir duruma geri gelmesini taklit eder. Oturum açtığınızda, veriler mobil arka uçunuzla senkronize edilir.

1. index.js'yi yeniden açın ve uygulama URL'sini geri yükleyin.
2. index.html'i yeniden açın ve CSP `<meta>` öğesindeki uygulama URL'sini düzeltin.
3. İstemci uygulamasını yeniden oluşturve çalıştırın. Uygulama, oturum açtıktan sonra mobil uygulama arka ucuyla eşitlemeye çalışır. Hata ayıklama konsolunda hiçbir özel durum günlüğe kaydolmadığını doğrulayın.
4. (İsteğe bağlı) GÜNCELLENMİş veriyi SQL Server Object Explorer veya Fiddler gibi bir REST aracını kullanarak görüntüleyin. Verilerin arka uç veritabanı ile yerel mağaza arasında senkronize edildiğine dikkat edin.

    Verilerin veritabanı ve yerel mağaza arasında senkronize edildiğine ve uygulamanızın bağlantısı kesilirken eklediğiniz öğeleri içerdiğine dikkat edin.

## <a name="additional-resources"></a>Ek kaynaklar
* [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]
* [Apache Cordova için Visual Studio Araçları]

## <a name="next-steps"></a>Sonraki adımlar
* [Çevrimdışı eşitleme örneğinde] çakışma çözümü gibi daha gelişmiş çevrimdışı eşitleme özelliklerini gözden geçirme
* [API belgelerinde](https://azure.github.io/azure-mobile-apps-js-client)çevrimdışı eşitleme API başvurugözden geçirin.

<!-- ##Summary -->

<!-- Images -->

<!-- URLs. -->
[Apache Cordova hızlı başlangıç]: app-service-mobile-cordova-get-started.md
[çevrimdışı eşitleme örneği]: https://github.com/Azure-Samples/app-service-mobile-cordova-client-conflict-handling
[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md
[Cloud Cover: Offline Sync in Azure Mobile Services]: https://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Adding Authentication]: app-service-mobile-cordova-get-started-users.md
[authentication]: app-service-mobile-cordova-get-started-users.md
[Work with the .NET backend server SDK for Azure Mobile Apps]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Visual Studio Community 2015]: https://www.visualstudio.com/
[Apache Cordova için Visual Studio Araçları]: https://www.visualstudio.com/en-us/features/cordova-vs.aspx
[Apache Cordova SDK]: app-service-mobile-cordova-how-to-use-client-library.md
[ASP.NET Server SDK]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[Node.js Server SDK]: app-service-mobile-node-backend-how-to-use-server-sdk.md

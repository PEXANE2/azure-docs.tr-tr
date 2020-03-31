---
title: Çevrimdışı eşitleme (Android) etkinleştirme
description: Android uygulamanızda çevrimdışı verileri önbelleğe almak ve senkronize etmek için App Service Mobile Apps'ı nasıl kullanacağınızı öğrenin.
ms.assetid: 32a8a079-9b3c-4faf-8588-ccff02097224
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: c215105af5fe1ef8056b0d816cf2c2a6b96f2038
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461632"
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Android mobil uygulamanız için çevrimdışı eşitleme yi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, Android için Azure Mobil Uygulamaları'nın çevrimdışı eşitleme özelliğini kapsar. Çevrimdışı eşitleme, ağ bağlantısı olmasa&mdash;&mdash;bile son kullanıcıların bir mobil uygulamayı görüntüleme, ekleme veya değiştirme ile etkileşimkurmasına olanak tanır. Değişiklikler yerel bir veritabanında depolanır. Aygıt yeniden çevrimiçi olduğunda, bu değişiklikler uzak arka uçla senkronize edilir.

Azure Mobil Apps ile ilk deneyiminiz buysa, öncelikle [Bir Android Uygulaması Oluştur]eğitimini tamamlamanız gerekir. İndirilen hızlı başlangıç sunucusu projesini kullanmıyorsanız, projenize veri erişim uzantısı paketlerini eklemeniz gerekir. Sunucu uzantı paketleri hakkında daha fazla bilgi için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)'ya bakın.

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi edinmek için [Azure Mobil Uygulamalarında Çevrimdışı Veri Eşitleme]konusuna bakın.

## <a name="update-the-app-to-support-offline-sync"></a>Çevrimdışı eşitlemeyi desteklemek için uygulamayı güncelleştirin
Çevrimdışı eşitleme ile, cihazınızdaki bir **SQLite** veritabanının parçası olan bir *eşitleme tablosuna* *(IMobileServiceSyncTable* arabirimini kullanarak) okuyup yazarsınız.

Aygıt ve Azure Mobil Hizmetleri arasındaki değişiklikleri itmek ve çekmek için, verileri yerel olarak depolamak için yerel veritabanıyla baş harfe aldığınız bir *eşitleme bağlamı* *(MobileServiceClient.SyncContext)* kullanırsınız.

1. In, `TodoActivity.java`eşitleme tablosu `mToDoTable` sürümü varolan tanımı nı açıklama ve açıklama:
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. `onCreate` Yöntemde, varolan başharfe `mToDoTable` yorum yapın ve bu tanımı açıklamayın:
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. Bu `refreshItemsFromTable` tanımın `results` tanımı ve yorum uncomment dışarı yorum olarak:
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. Açıklama tanımı . `refreshItemsFromMobileServiceTable`
5. Uncomment `refreshItemsFromMobileServiceTableSyncTable`tanımı:
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. Uncomment `sync`tanımı:
   
        private AsyncTask<Void, Void, Void> sync() {
            AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
                @Override
                protected Void doInBackground(Void... params) {
                    try {
                        MobileServiceSyncContext syncContext = mClient.getSyncContext();
                        syncContext.push().get();
                        mToDoTable.pull(null).get();
                    } catch (final Exception e) {
                        createAndShowDialogFromTask(e, "Error");
                    }
                    return null;
                }
            };
            return runAsyncTask(task);
        }

## <a name="test-the-app"></a>Uygulamayı test edin
Bu bölümde, WiFi ile davranışı sınayın ve çevrimdışı bir senaryo oluşturmak için WiFi'ı kapatırsınız.

Veri öğeleri eklediğinizde, bunlar yerel SQLite deposunda tutulur, ancak **siz Yenile** düğmesine basana kadar mobil servisle eşitlenmez. Diğer uygulamaların verilerin ne zaman eşitlenmesi gerektiğine ilişkin farklı gereksinimleri olabilir, ancak demo amaçlı bu öğretici, kullanıcının açıkça talep etmesini sağlar.

Bu düğmeye bastığınızda, yeni bir arka plan görevi başlar. Önce eşitleme bağlamını kullanarak yerel mağazada yapılan tüm değişiklikleri iter, ardından değiştirilen tüm verileri Azure'dan yerel tabloya çeker.

### <a name="offline-testing"></a>Çevrimdışı test
1. Cihazı veya simülatörü *Uçak Modu'na*yerleştirin. Bu çevrimdışı bir senaryo oluşturur.
2. Bazı *ToDo* öğeleri ekleyin veya bazı öğeleri tamamlanmış olarak işaretleyin. Cihazı veya simülatörden çıkın (veya uygulamayı zorla kapatın) ve yeniden başlatın. Değişikliklerinizin aygıtta kalıcı olduğunu doğrulayın, çünkü bunlar yerel SQLite deposunda tutulur.
3. Azure *TodoItem* tablosunun içeriğini SQL Server Management *Studio*gibi bir SQL aracıyla veya *Fiddler* veya *Postacı*gibi bir REST istemcisiyle görüntüleyin. Yeni öğelerin sunucuyla *eşitlenmediğini* doğrulama
   
       + Bir Düğüm.js arka uç için [Azure portalına](https://portal.azure.com/)gidin ve Mobil Uygulama arka uçta `TodoItem` tablonun içeriğini görüntülemek için **Kolay Tablolar** > **TodoItem'i** tıklatın.
       + .NET arka uç için tablo içeriğini *SQL Server Management Studio*gibi bir SQL aracıyla veya *Fiddler* veya *Postacı*gibi bir REST istemcisiyle görüntüleyin.
4. Cihazda veya simülatörde WiFi'ı açın. Ardından, **Yenile** düğmesine basın.
5. TodoItem verilerini Azure portalında yeniden görüntüleyin. Yeni ve değiştirilen TodoItems şimdi görünmelidir.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]
* [Bulut Kapağı: Azure Mobil Hizmetlerde] \(Çevrimdışı Eşitleme: Video Mobil Hizmetler'dedir, ancak Çevrimdışı eşitleme Azure Mobil Uygulamalarında benzer şekilde çalışır\)

<!-- URLs. -->

[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md

[Android Uygulaması Oluşturma]: app-service-mobile-android-get-started.md

[Bulut Kapağı: Azure Mobil Hizmetlerinde Çevrimdışı Eşitleme]: https://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: https://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/


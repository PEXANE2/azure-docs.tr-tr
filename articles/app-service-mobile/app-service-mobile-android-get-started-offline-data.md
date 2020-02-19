---
title: Çevrimdışı eşitlemeyi etkinleştir (Android)
description: Android uygulamanızda çevrimdışı verileri önbelleğe almak ve eşitlemek için App Service Mobile Apps kullanmayı öğrenin.
ms.assetid: 32a8a079-9b3c-4faf-8588-ccff02097224
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: c215105af5fe1ef8056b0d816cf2c2a6b96f2038
ms.sourcegitcommit: 6ee876c800da7a14464d276cd726a49b504c45c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77461632"
---
# <a name="enable-offline-sync-for-your-android-mobile-app"></a>Android mobil uygulamanız için çevrimdışı eşitlemeyi etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, Android için Azure Mobile Apps çevrimdışı eşitleme özelliğini ele almaktadır. Çevrimdışı eşitleme son kullanıcıların bir mobil uygulamayla etkileşime geçmesini sağlar ve ağ bağlantısı olmasa bile veri&mdash;görüntüleme, ekleme veya değiştirme&mdash;. Değişiklikler yerel bir veritabanında depolanır. Cihaz yeniden çevrimiçi olduktan sonra, bu değişiklikler uzak arka uca eşitlenir.

Azure Mobile Apps ile ilgili ilk deneyiminiz varsa öncelikle [Android uygulaması oluşturma]öğreticisini tamamlamalısınız. İndirilen hızlı başlangıç sunucusu projesini kullanmazsanız, veri erişim uzantısı paketlerini projenize eklemeniz gerekir. Sunucu Uzantısı paketleri hakkında daha fazla bilgi için bkz. [Azure için .net arka uç sunucu SDK 'sı Mobile Apps çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi edinmek için [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]konusuna bakın.

## <a name="update-the-app-to-support-offline-sync"></a>Çevrimdışı eşitlemeyi destekleyecek şekilde uygulamayı güncelleştirme
Çevrimdışı eşitleme ile, cihazınızdaki bir **SQLite** veritabanının parçası olan bir *eşitleme tablosundan* ( *ımobileservicesynctable* arabirimini kullanarak) okuyup yazarsınız.

Cihaz ile Azure Mobile Services arasında anında iletme ve çekme değişiklikleri yapmak için, yerel veritabanıyla birlikte verileri yerel olarak depolamak üzere başlattığınız bir *eşitleme bağlamı* (*MobileServiceClient. syncContext*) kullanırsınız.

1. `TodoActivity.java`, var olan `mToDoTable` tanımını açıklama ve eşitleme tablosu sürümünün açıklamasını kaldırın:
   
        private MobileServiceSyncTable<ToDoItem> mToDoTable;
2. `onCreate` yönteminde, var olan `mToDoTable` başlatmasını ve bu tanımın açıklamasını kaldırın:
   
        mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
3. `refreshItemsFromTable` `results` tanımını not edin ve bu tanımın açıklamasını kaldırın:
   
        // Offline Sync
        final List<ToDoItem> results = refreshItemsFromMobileServiceTableSyncTable();
4. `refreshItemsFromMobileServiceTable`tanımını açıklama olarak değerlendirin.
5. `refreshItemsFromMobileServiceTableSyncTable`tanımının açıklamasını kaldırın:
   
        private List<ToDoItem> refreshItemsFromMobileServiceTableSyncTable() throws ExecutionException, InterruptedException {
            //sync the data
            sync().get();
            Query query = QueryOperations.field("complete").
                    eq(val(false));
            return mToDoTable.read(query).get();
        }
6. `sync`tanımının açıklamasını kaldırın:
   
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
Bu bölümde, davranışı WiFi ile test edersiniz ve sonra bir çevrimdışı senaryo oluşturmak için WiFi 'yi devre dışı bırakabilirsiniz.

Veri öğeleri eklediğinizde, bunlar yerel SQLite deposunda tutulur, ancak **Yenile** düğmesine bastığımda mobil hizmetle eşitlenmez. Diğer uygulamalar, verilerin eşitlenmesi gerektiği zaman ilgili farklı gereksinimlere sahip olabilir, ancak demo amacıyla bu öğreticide Kullanıcı tarafından açıkça istekte bulunur.

Bu düğmeye bastığınızda yeni bir arka plan görevi başlatılır. İlk olarak, eşitleme bağlamını kullanarak yerel depoda yapılan tüm değişiklikleri iter, ardından tüm değiştirilen verileri Azure 'dan yerel tabloya çeker.

### <a name="offline-testing"></a>Çevrimdışı test
1. Cihazı veya benzeticiyi *uçak moduna*koyun. Bu bir çevrimdışı senaryo oluşturur.
2. Bazı *Todo* öğeleri ekleyin veya bazı öğeleri tamamlanmış olarak işaretleyin. Cihazdan veya benzeticinden çıkın (ya da uygulamayı zorla kapatın) ve yeniden başlatın. Yerel SQLite deposunda tutuldıklarından, değişikliklerin cihazda kalıcı olduğunu doğrulayın.
3. Azure *TodoItem* tablosunun içeriğini *SQL Server Management STUDIO*gibi bir SQL aracıyla veya *Fiddler* veya *Postman*gibi bir rest istemcisi ile görüntüleyin. Yeni öğelerin sunucuyla eşitlendiğinden emin olun
   
       + Bir Node. js arka ucu için [Azure Portal](https://portal.azure.com/)gidin ve mobil uygulama arka ucunuzda `TodoItem` tablosunun içeriğini görüntülemek Için **kolay tablolar** > **TodoItem** ' e tıklayın.
       + .NET arka ucu için tablo içeriğini *SQL Server Management Studio*gıbı bir SQL aracıyla veya *Fiddler* veya *Postman*gibi bir rest istemcisi ile görüntüleyin.
4. Cihazda veya benzeticide WiFi 'yi açın. Sonra **Yenile** düğmesine basın.
5. TodoItem verilerini Azure portal yeniden görüntüleyin. Yeni ve değiştirilmiş Todoıtems artık görünmelidir.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]
* [Bulut kapağı: azure Mobile Services çevrimdışı eşitleme] \(Note: video Mobile Services açık, ancak çevrimdışı eşitleme Azure Mobile Apps benzer bir şekilde çalışıyor\)

<!-- URLs. -->

[Azure Mobile Apps’te Çevrimdışı Veri Eşitleme]: app-service-mobile-offline-data-sync.md

[Android uygulaması oluşturma]: app-service-mobile-android-get-started.md

[Bulut kapağı: Azure Mobile Services çevrimdışı eşitleme]: https://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: https://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/


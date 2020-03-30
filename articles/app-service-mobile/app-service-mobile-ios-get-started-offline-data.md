---
title: Çevrimdışı eşitleme (iOS) etkinleştirme
description: iOS uygulamalarında çevrimdışı verileri önbelleğe almak ve senkronize etmek için Azure App Service mobil uygulamalarını nasıl kullanacağınızı öğrenin.
ms.assetid: eb5b9520-0f39-4a09-940a-dadb6d940db8
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: d943213814b999f101a541abb0195a9fdd5a7423
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77459183"
---
# <a name="enable-offline-syncing-with-ios-mobile-apps"></a>iOS mobil uygulamalarıyla çevrimdışı senkronizasyonu etkinleştirme
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

## <a name="overview"></a>Genel Bakış
Bu öğretici, iOS için Azure Uygulama Hizmeti'nin Mobil Uygulamalar özelliğiyle çevrimdışı senkronizasyonu kapsar. Çevrimdışı eşitleme ile son kullanıcılar, ağ bağlantıları olmasa bile verileri görüntülemek, eklemek veya değiştirmek için bir mobil uygulamayla etkileşimkurabilir. Değişiklikler yerel bir veritabanında depolanır. Aygıt yeniden çevrimiçi olduktan sonra, değişiklikler uzak arka uçla senkronize edilir.

Mobil Uygulamalar la ilgili ilk deneyiminiz buysa, öncelikle [bir iOS Uygulaması Oluştur]eğitimini tamamlamanız gerekir. İndirilen hızlı başlangıç sunucusu projesini kullanmıyorsanız, projenize veri erişim uzantısı paketlerini eklemeniz gerekir. Sunucu uzantı paketleri hakkında daha fazla bilgi için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışma](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)'ya bakın.

Çevrimdışı eşitleme özelliği hakkında daha fazla bilgi edinmek için [Mobil Uygulamalarda Çevrimdışı Veri Eşitleme'ye]bakın.

## <a name="review-the-client-sync-code"></a><a name="review-sync"></a>İstemci eşitleme kodunu gözden geçirme
[iOS Uygulaması Oluştur] öğreticisi için indirdiğiniz istemci projesi, yerel Çekirdek Veri tabanlı bir veritabanı nı kullanarak çevrimdışı eşitlemi destekleyen kod lar zaten içerir. Bu bölümde, öğretici kodda zaten yer alan neler özetlenir. Özelliğin kavramsal bir özeti için, [Mobil Uygulamalarda Çevrimdışı Veri Eşitleme]bölümüne bakın.

Mobil Uygulamalar'ın çevrimdışı veri eşitleme özelliğini kullanan son kullanıcılar, ağa erişilmese bile yerel bir veritabanıyla etkileşimkurabilir. Uygulamanızda bu özellikleri kullanmak için, yerel bir `MSClient` mağazanın eşitleme bağlamını başlangıç olarak ve başvurusunuz. Ardından, **MSSyncTable** arabirimi üzerinden tablonuza başvurursunuz.

**QSTodoService.m** (Objective-C) veya **ToDoTableViewController.swift** (Swift) olarak, üye **syncTable** türü **MSSyncTable**olduğunu unutmayın. Çevrimdışı **eşitleme, MSTable**yerine bu eşitleme tablosu arabirimini kullanır. Eşitleme tablosu kullanıldığında, tüm işlemler yerel mağazaya gider ve yalnızca açık itme ve çekme işlemleriyle uzaktan arka uçla eşitlenir.

 Eşitleme tablosuna başvuru almak için **syncTableWithName** `MSClient`yöntemini kullanın. Çevrimdışı eşitleme işlevini kaldırmak için **tableWithName'i** kullanın.

Herhangi bir tablo işlemi gerçekleştirilemeden önce, yerel mağazanın başlatılması gerekir. İşte ilgili kod:

* **Amaç-C**. **QSTodoService.init** yönteminde:

   ```objc
   MSCoreDataStore *store = [[MSCoreDataStore alloc] initWithManagedObjectContext:context];
   self.client.syncContext = [[MSSyncContext alloc] initWithDelegate:nil dataSource:store callback:nil];
   ```    
* **Swift,** ne kadar hızlı. **ToDoTableViewController.viewDidLoad** yönteminde:

   ```swift
   let client = MSClient(applicationURLString: "http:// ...") // URI of the Mobile App
   let managedObjectContext = (UIApplication.sharedApplication().delegate as! AppDelegate).managedObjectContext!
   self.store = MSCoreDataStore(managedObjectContext: managedObjectContext)
   client.syncContext = MSSyncContext(delegate: nil, dataSource: self.store, callback: nil)
   ```
   Bu yöntem, Mobil Uygulamalar SDK'nın `MSCoreDataStore` sağladığı arabirimi kullanarak yerel bir mağaza oluşturur. Alternatif olarak, `MSSyncContextDataSource` protokolü uygulayarak farklı bir yerel mağaza sağlayabilirsiniz. Ayrıca, **MSSyncContext** ilk parametresi bir çakışma işleyicisi belirtmek için kullanılır. Biz geçtik `nil`çünkü, biz herhangi bir çakışma başarısız varsayılan çakışma işleyicisi olsun.

Şimdi, gerçek eşitleme işlemini gerçekleştirelim ve uzak arka uçtan veri alalım:

* **Amaç-C**. `syncData`önce yeni değişiklikler eler ve sonra uzak arka uçtan veri almak için **pullData'yı** arar. Buna karşılık, **pullData** yöntemi bir sorgu eşleşen yeni veriler alır:

   ```objc
   -(void)syncData:(QSCompletionBlock)completion
   {
       // Push all changes in the sync context, and then pull new data.
       [self.client.syncContext pushWithCompletion:^(NSError *error) {
           [self logErrorIfNotNil:error];
           [self pullData:completion];
       }];
   }

   -(void)pullData:(QSCompletionBlock)completion
   {
       MSQuery *query = [self.syncTable query];

       // Pulls data from the remote server into the local table.
       // We're pulling all items and filtering in the view.
       // Query ID is used for incremental sync.
       [self.syncTable pullWithQuery:query queryId:@"allTodoItems" completion:^(NSError *error) {
           [self logErrorIfNotNil:error];

           // Lets the caller know that we have finished.
           if (completion != nil) {
               dispatch_async(dispatch_get_main_queue(), completion);
           }
       }];
   }
   ```
* **Hızlı :**
   ```swift
   func onRefresh(sender: UIRefreshControl!) {
      UIApplication.sharedApplication().networkActivityIndicatorVisible = true

      self.table!.pullWithQuery(self.table?.query(), queryId: "AllRecords") {
          (error) -> Void in

          UIApplication.sharedApplication().networkActivityIndicatorVisible = false

          if error != nil {
              // A real application would handle various errors like network conditions,
              // server conflicts, etc. via the MSSyncContextDelegate
              print("Error: \(error!.description)")

              // We will discard our changes and keep the server's copy for simplicity
              if let opErrors = error!.userInfo[MSErrorPushResultKey] as? Array<MSTableOperationError> {
                  for opError in opErrors {
                      print("Attempted operation to item \(opError.itemId)")
                      if (opError.operation == .Insert || opError.operation == .Delete) {
                          print("Insert/Delete, failed discarding changes")
                          opError.cancelOperationAndDiscardItemWithCompletion(nil)
                      } else {
                          print("Update failed, reverting to server's copy")
                          opError.cancelOperationAndUpdateItem(opError.serverItem!, completion: nil)
                      }
                  }
              }
          }
          self.refreshControl?.endRefreshing()
      }
   }
   ```

Objective-C sürümünde, `syncData`ilk olarak **pushWithCompletion'ı** eşitleme bağlamında çağırırız. Bu yöntem, tüm `MSSyncContext` tablolardaki değişiklikleri ittirdiği için eşitleme tablosunun bir üyesidir (eşitleme tablosunun kendisi değildir). Yalnızca yerel olarak (CUD işlemleri yoluyla) bir şekilde değiştirilen kayıtlar sunucuya gönderilir. Daha sonra yardımcı **pullData** denir, uzak veri almak ve yerel veritabanında depolamak için **MSSyncTable.pullWithQuery** çağırır.

Swift sürümünde, itme işlemi kesinlikle gerekli olmadığından, **PushWithCompletion'ı zorlamaya**gerek yoktur. Bir itme işlemi yapan tablo için eşitleme bağlamında bekleyen herhangi bir değişiklik varsa, çekin her zaman önce bir itme sorunları. Ancak, birden fazla eşitleme tablonuz varsa, her şeyin ilgili tablolar arasında tutarlı olduğundan emin olmak için itme'yi açıkça aramak en iyisidir.

Hem Objective-C hem de Swift sürümlerinde, almak istediğiniz kayıtları filtrelemek için bir sorgu belirtmek için **pullWithQuery** yöntemini kullanabilirsiniz. Bu örnekte, sorgu uzak `TodoItem` tablodaki tüm kayıtları alır.

**pullWithQuery'nin** ikinci parametresi *artımlı eşitleme*için kullanılan bir sorgu kimliğidir. Artımlı eşitleme, kaydın `UpdatedAt` zaman damgasını kullanarak yalnızca son eşitlemeden `updatedAt` bu yana değiştirilen kayıtları alır (yerel mağazada çağrılır.) Sorgu kimliği, uygulamanızdaki her mantıksal sorgu için benzersiz olan açıklayıcı bir dize olmalıdır. Artımlı eşitlemeyi devre `nil` dışı bırakmak için sorgu kimliği olarak geçirin. Bu yaklaşım, her çekme işlemindeki tüm kayıtları aldığı için büyük ölçüde verimsiz olabilir.

Objective-C uygulaması, verileri değiştirdiğinizde veya eklediğinizde, kullanıcı yenileme jestini gerçekleştirdiğinde ve lansmanda eşitlenir.

Swift uygulaması, kullanıcı yenileme jestini gerçekleştirdiğinde ve lansmanda senkronize olur.

Veriler değiştirildiğinde (Objective-C) veya uygulama her başlatıldığında (Objective-C ve Swift) uygulama eşitlendiğinden, uygulama kullanıcının çevrimiçi olduğunu varsayar. Daha sonraki bir bölümde, kullanıcıların çevrimdışı olsalar bile düzenleyebilmeleri için uygulamayı güncellersiniz.

## <a name="review-the-core-data-model"></a><a name="review-core-data"></a>Çekirdek Veri modelini gözden geçirin
Çekirdek Verileri çevrimdışı deposunu kullandığınızda, veri modelinizdeki belirli tabloları ve alanları tanımlamanız gerekir. Örnek uygulama zaten doğru biçimde bir veri modeli içerir. Bu bölümde, nasıl kullanıldığını göstermek için bu tablolar üzerinden yürümek.

**Açık QSDataModel.xcdatamodeld**. Dört tablo tanımlanır-- üç SDK tarafından kullanılan ve bir tanesi yapılacak lar için kullanılan öğeleriçin kullanılır:
  * MS_TableOperations: Sunucuyla eşitlenilmesi gereken öğeleri izler.
  * MS_TableOperationErrors: Çevrimdışı eşitleme sırasında meydana gelen hataları izler.
  * MS_TableConfig: Tüm çekme işlemleri için son eşitleme işlemi için son güncelleştirme zamanını izler.
  * TodoItem: Yapılacak öğeleri depolar. Sistem sütunları **oluşturulanAt**, **updatedAt**, ve **sürüm** isteğe bağlı sistem özellikleridir.

> [!NOTE]
> Mobil Uygulamalar SDK ile başlayan sütun**``** adları ayırır " " ile. Bu önek'i sistem sütunları dışında hiçbir şeyle kullanmayın. Aksi takdirde, uzak arka ucu kullandığınızda sütun adları değiştirilir.
>
>

Çevrimdışı eşitleme özelliğini kullandığınızda, üç sistem tablosunu ve veri tablosunu tanımlayın.

### <a name="system-tables"></a>Sistem tabloları

**MS_TableOperations**  

![MS_TableOperations tablo öznitelikleri][defining-core-data-tableoperations-entity]

| Öznitelik | Tür |
| --- | --- |
| id | İntesayı 64 |
| ıtemıd | Dize |
| properties | İkili Veri |
| tablo | Dize |
| tabloTür | Sonsayı 16 |


**MS_TableOperationErrors**

 ![MS_TableOperationErrors tablo öznitelikleri][defining-core-data-tableoperationerrors-entity]

| Öznitelik | Tür |
| --- | --- |
| id |Dize |
| operationId |İntesayı 64 |
| properties |İkili Veri |
| tabloTür |Sonsayı 16 |

 **MS_TableConfig**

 ![][defining-core-data-tableconfig-entity]

| Öznitelik | Tür |
| --- | --- |
| id |Dize |
| anahtar |Dize |
| Keytype |İntesayı 64 |
| tablo |Dize |
| value |Dize |

### <a name="data-table"></a>Veri tablosu

**TodoItem**

| Öznitelik | Tür | Not |
| --- | --- | --- |
| id | String, gerekli işaretlenmiş |Uzak mağazada birincil anahtar |
| tamamlama | Boole | Yapılacaklar öğesi alanı |
| metin |Dize |Yapılacaklar öğesi alanı |
| createdAt | Tarih | (isteğe bağlı) Oluşturulan **haritalarAt** sistem özelliği |
| updatedAt | Tarih | (isteğe bağlı) Haritalar **dan updatedAt** sistem özelliği |
| version | Dize | (isteğe bağlı) Çakışmaları, haritaları ve sürümü algılamak için kullanılır |

## <a name="change-the-sync-behavior-of-the-app"></a><a name="setup-sync"></a>Uygulamanın eşitleme davranışını değiştirme
Bu bölümde, uygulamanın başlatırken veya öğeleri ekleyip güncellediğinizde senkronize olmayacak şekilde uygulamayı değiştirirsiniz. Yalnızca yenileme hareketi düğmesi yapıldığında eşitlenir.

**Amaç-C**:

1. **QSTodoListViewController.m,** metodun sonunda arama kaldırmak için `[self refresh]` **viewDidLoad** yöntemini değiştirin. Artık veriler uygulama başlangıcında sunucuyla eşitlenmez. Bunun yerine, yerel mağazanın içeriğiyle senkronize edilir.
2. **QSTodoService.m'de,** öğe `addItem` takıldıktan sonra eşitlenmemesi için tanımını değiştirin. `self syncData` Bloğu kaldırın ve aşağıdakilerle değiştirin:

   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```
3. Daha önce `completeItem` belirtildiği gibi tanımını değiştirin. Bloğu aşağıdakiler `self syncData` için kaldırın ve değiştirin:
   ```objc
   if (completion != nil) {
       dispatch_async(dispatch_get_main_queue(), completion);
   }
   ```

**Hızlı :**

`viewDidLoad` **ToDoTableViewController.swift'te,** uygulama başlangıcında senkronizasyonu durdurmak için burada gösterilen iki satırı yorumlayabilirsiniz. Bu yazı nın yazıldığı sırada, Swift Todo uygulaması, birisi bir öğe eklendiğinde veya tamamlandığında hizmeti güncelleştirmez. Hizmeti yalnızca uygulama başlangıcında günceller.

   ```swift
  self.refreshControl?.beginRefreshing()
  self.onRefresh(self.refreshControl)
```

## <a name="test-the-app"></a><a name="test-app"></a>Uygulamayı test edin
Bu bölümde, çevrimdışı bir senaryoyu simüle etmek için geçersiz bir URL'ye bağlanırsınız. Veri öğeleri eklediğinizde, bunlar yerel Çekirdek Veri deposunda tutulur, ancak bunlar mobil uygulama nın arka ucuyla senkronize edilmez.

1. **QSTodoService.m'deki** mobil uygulama URL'sini geçersiz bir URL ile değiştirin ve uygulamayı yeniden çalıştırın:

   **Amaç-C**. QSTodoService.m olarak:
   ```objc
   self.client = [MSClient clientWithApplicationURLString:@"https://sitename.azurewebsites.net.fail"];
   ```
   **Swift,** ne kadar hızlı. ToDoTableViewController.swift olarak:
   ```swift
   let client = MSClient(applicationURLString: "https://sitename.azurewebsites.net.fail")
   ```
2. Bazı yapılacaklar öğeleri ekleyin. Simülatörü bırakın (veya uygulamayı zorla kapatın) ve sonra yeniden başlatın. Değişikliklerinizin devam ettiğini doğrulayın.

3. Uzak **TodoItem** tablosunun içeriğini görüntüleyin:
   * Bir Düğüm.js arka uç için [Azure portalına](https://portal.azure.com/) gidin ve mobil uygulama arka uçta **Kolay Tablolar** > **TodoItem'i**tıklatın.  
   * .NET arka uç için, SQL Server Management Studio gibi bir SQL aracı nı veya Fiddler veya Postacı gibi bir REST istemcisi kullanın.  

4. Yeni öğelerin sunucuyla *eşitlenmediğini* doğrulayın.

5. URL'yi **QSTodoService.m'deki**doğru URL'ye geri değiştirin ve uygulamayı yeniden çalıştırın.

6. Öğelerin listesini aşağı çekerek yenileme hareketi gerçekleştirin.  
İlerleme spinner görüntülenir.

7. **TodoItem** verilerini yeniden görüntüleyin. Yeni ve değiştirilen yapılacaklar öğeleri şimdi görüntülenmelidir.

## <a name="summary"></a>Özet
Çevrimdışı eşitleme özelliğini desteklemek için `MSSyncTable` arabirimi `MSClient.syncContext` kullandık ve yerel bir mağazayla baş harfe bindirmek için. Bu durumda, yerel mağaza Çekirdek Veri tabanlı bir veritabanı oldu.

Çekirdek Veri yerel deposu kullandığınızda, doğru sistem [özelliklerine](#review-core-data)sahip birkaç tablo tanımlamanız gerekir.

Mobil uygulamalar için normal oluşturma, okuma, güncelleme ve silme (CRUD) işlemleri, uygulama hala bağlı yatsa çalışır, ancak tüm işlemler yerel mağazaya karşı gerçekleşir.

Yerel mağazayı sunucuyla senkronize ettiğimizde **MSSyncTable.pullWithQuery** yöntemini kullandık.

## <a name="additional-resources"></a>Ek kaynaklar
* [Mobil Uygulamalarda Çevrimdışı Veri Eşitle]
* [Bulut Kapağı: Azure Mobil Hizmetlerde] \(Çevrimdışı Eşitleme Video Mobil Hizmetlerle ilgilidir, ancak Mobil Uygulamalar çevrimdışı eşitleme de benzer şekilde çalışır.\)

<!-- URLs. -->


[iOS uygulaması oluşturma]: app-service-mobile-ios-get-started.md
[Mobil Uygulamalarda Çevrimdışı Veri Eşitle]: app-service-mobile-offline-data-sync.md

[defining-core-data-tableoperationerrors-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperationerrors-entity.png
[defining-core-data-tableoperations-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableoperations-entity.png
[defining-core-data-tableconfig-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-tableconfig-entity.png
[defining-core-data-todoitem-entity]: ./media/app-service-mobile-ios-get-started-offline-data/defining-core-data-todoitem-entity.png

[Bulut Kapağı: Azure Mobil Hizmetlerinde Çevrimdışı Eşitleme]: https://channel9.msdn.com/Shows/Cloud+Cover/Episode-155-Offline-Storage-with-Donna-Malayeri
[Azure Friday: Offline-enabled apps in Azure Mobile Services]: https://azure.microsoft.com/documentation/videos/azure-mobile-services-offline-enabled-apps-with-donna-malayeri/

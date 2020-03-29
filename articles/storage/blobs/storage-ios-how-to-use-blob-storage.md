---
title: iOS'tan nesne (Blob) depolama nasıl kullanılır - Azure | Microsoft Dokümanlar
description: Azure Blob Storage (nesne depolama) ile bulutta yapılandırılmamış veri depolayın.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 11/20/2018
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.openlocfilehash: 54085d602246d38adb970ed02f451241ca7ba19d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68726415"
---
# <a name="how-to-use-blob-storage-from-ios"></a>iOS'tan Blob depolama nasıl kullanılır?

Bu makalede, Microsoft Azure Blob depolama alanını kullanarak sık karşılaşılan senaryoların nasıl gerçekleştirildirilen gösterilmektedir. Örnekler Objective-C ile yazılır ve [iOS için Azure Depolama İstemci Kitaplığı'nı](https://github.com/Azure/azure-storage-ios)kullanır. Kapsanan senaryolar yükleme, listeleme, indirme ve blobs silme içerir. Lekeler hakkında daha fazla bilgi için [Sonraki Adımlar](#next-steps) bölümüne bakın. Azure Depolama'nın bir iOS uygulamasında kullanımını hızlı bir şekilde görmek için [örnek uygulamayı](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) da indirebilirsiniz.

Blob depolama hakkında daha fazla bilgi edinmek için Azure [Blob depolamasına Giriş 'e](storage-blobs-introduction.md)bakın.

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="import-the-azure-storage-ios-library-into-your-application"></a>Azure Depolama iOS kitaplığını uygulamanıza aktarın

Azure Depolama [cocoaPod'unu](https://cocoapods.org/pods/AZSClient) kullanarak veya **Çerçeve** dosyasını içe aktararak Azure Depolama iOS kitaplığını uygulamanız içine aktarabilirsiniz. CocoaPod, kitaplığı tümleştirmeyi kolaylaştıran önerilen yoldur, ancak çerçeve dosyasından içe aktarma mevcut projeniz için daha az müdahalecidir.

Bu kitaplığı kullanmak için aşağıdakileri yapmanız gerekir:

- iOS 8+
- Xcode 7+

## <a name="cocoapod"></a>CocoaPod

1. Bunu daha önce yapmadıysanız, bir terminal penceresi açarak ve aşağıdaki komutu çalıştırarak [bilgisayarınıza CocoaPods yükleyin](https://guides.cocoapods.org/using/getting-started.html#toc_3)

    ```shell
    sudo gem install cocoapods
    ```

2. Ardından, proje dizininde (.xcodeproj dosyanızı içeren dizin), _Podfile_(dosya uzantısı yok) adlı yeni bir dosya oluşturun. Aşağıdakileri _Podfile'ye_ ekleyin ve kaydedin.

    ```ruby
    platform :ios, '8.0'

    target 'TargetName' do
      pod 'AZSClient'
    end
    ```

3. Terminal penceresinde, proje dizinine gidin ve aşağıdaki komutu çalıştırın

    ```shell
    pod install
    ```

4. .xcodeproj'unuz Xcode'da açıksa kapatın. Proje dizininizde .xcworkspace uzantılı yeni oluşturulan proje dosyasını açın. Bu, şimdilik çalışacağın dosya.

## <a name="framework"></a>Framework

Kitaplığı kullanmanın diğer yolu çerçeveyi el ile oluşturmaktır:

1. İlk olarak, [azure-depolama-ios repo'yu](https://github.com/azure/azure-storage-ios)indirin veya klonla.
2. *Azure-depolama-ios* -> *Lib* -> *Azure Depolama İstemci Kitaplığına*gidin ve Xcode'da açın. `AZSClient.xcodeproj`
3. Xcode'un sol üst kısmında, etkin düzeni "Azure Depolama İstemci Kitaplığı"ndan "Framework" olarak değiştirin.
4. Projeyi oluşturun (10+B). Bu, Masaüstünüzde bir `AZSClient.framework` dosya oluşturur.

Daha sonra aşağıdakileri yaparak çerçeve dosyasını uygulamanız içine aktarabilirsiniz:

1. Yeni bir proje oluşturun veya xcode'da mevcut projenizi açın.
2. Xcode proje `AZSClient.framework` gezgininize sürükleyin ve bırakın.
3. *Gerekirse öğeleri kopyala'yı*seçin ve *Finish'e*tıklayın.
4. Sol navigasyonda projenize tıklayın ve proje düzenleyicisinin üst kısmındaki *Genel* sekmesini tıklatın.
5. Bağlantılı *Çerçeveler ve Kitaplıklar* bölümünde Ekle düğmesini (+) tıklatın.
6. Zaten sağlanan kitaplıklar listesinde, `libxml2.2.tbd` projenizi arayın ve ekleyebilirsiniz.

## <a name="import-the-library"></a>Kitaplığı Alma

```objc
// Include the following import statement to use blob APIs.
#import <AZSClient/AZSClient.h>
```

Swift kullanıyorsanız, bir köprü başlığı oluşturmanız ve azsclient/AZSClient.h> \<almanız gerekir:

1. Üstbilgi dosyası `Bridging-Header.h`oluşturun ve yukarıdaki alma deyimini ekleyin.
2. *Yapı Ayarları* sekmesine gidin ve *Objective-C Köprü Üstbilgisini*arayın.
3. *Objective-C Köprü Üstbilgi* alanına çift tıklayın ve üstbilgi dosyanıza yolu ekleyin:`ProjectName/Bridging-Header.h`
4. Köprüleme üstbilgisinin Xcode tarafından alındığını doğrulamak için projeyi (1+B) oluşturun.
5. Kitaplığı doğrudan herhangi bir Swift dosyasında kullanmaya başlayın, alma deyimleri için gerek yoktur.

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="asynchronous-operations"></a>Zaman Uyumsuz İşlemler

> [!NOTE]
> Hizmete karşı istek gerçekleştiren tüm yöntemler eşzamanlı işlemlerdir. Kod örneklerinde, bu yöntemlerin bir tamamlama işleyicisi olduğunu göreceksiniz. Tamamlanma işleyicisinin içindeki kod, istek **tamamlandıktan sonra** çalışacaktır. İstek **yapılırken** tamamlanma işleyicisi çalıştırılacak sonra kod.

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Azure Depolama'daki her blob bir kapta olmalıdır. Aşağıdaki örnek, zaten yoksa Depolama hesabınızda *yeni kapsayıcı*adı verilen bir kapsayıcının nasıl oluşturultuğu gösterilmektedir. Kapsayıcınız için bir ad seçerken, yukarıda belirtilen adlandırma kurallarına dikkat edin.

```objc
-(void)createContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

    // Create container in your Storage account if the container doesn't already exist
    [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

[Bunun Microsoft Azure Depolama Gezgini'ne](https://storageexplorer.com) bakarak ve yeni *kapsayıcının* Depolama hesabınız için kapsayıcılar listesinde olduğunu doğrulayarak çalıştığını doğrulayabilirsiniz.

## <a name="set-container-permissions"></a>Konteyner İzinlerini Ayarla

Bir kapsayıcının izinleri varsayılan olarak **Özel** erişim için yapılandırılır. Ancak, kapsayıcılar kapsayıcı erişimi için birkaç farklı seçenek sağlar:

- **Özel**: Konteyner ve blob verileri yalnızca hesap sahibi tarafından okunabilir.
- **Blob**: Bu kapsayıcının içindeki blob verileri anonim istek le okunabilir, ancak kapsayıcı verileri kullanılamaz. İstemciler, anonim istek le konteyner içindeki lekeleri sayısalaramaz.
- **Konteyner**: Konteyner ve blob verileri anonim istek ile okunabilir. İstemciler, anonim istek le kapsayıcıiçindeki lekeleri sayabilir, ancak depolama hesabı içindeki kapsayıcıları sayamaz.

Aşağıdaki örnek, **Kapsayıcı** erişim izinlerine sahip bir kapsayıcının nasıl oluşturulacağını ve Internet'teki tüm kullanıcılar için herkese açık, salt okunur erişime izin verecek bir kapsayıcıyı gösterir:

```objc
-(void)createContainerWithPublicAccess{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create container in your Storage account if the container doesn't already exist
    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
        if (error){
            NSLog(@"Error in creating container.");
        }
    }];
}
```

## <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme

Blob servis kavramları bölümünde belirtildiği gibi, Blob Depolama blobs üç farklı türde sunuyor: blok blobs, ek blobs, ve sayfa lekeleri. Azure Depolama iOS kitaplığı üç blob türünü de destekler. Çoğu durumda, kullanılması önerilen blob türü blok blobudur.

Aşağıdaki örnek, bir NSString'ten bir blok blob'unnasıl yüklenirken gösterilmektedir. Bu kapsayıcıda zaten aynı ada sahip bir blob varsa, bu blob'un içeriği üzerine yazılır.

```objc
-(void)uploadBlobToContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
        {
            if (error){
                NSLog(@"Error in creating container.");
            }
            else{
                // Create a local blob object
                AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                // Upload blob to Storage
                [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                    if (error){
                        NSLog(@"Error in creating blob.");
                    }
                }];
            }
        }];
}
```

Microsoft Azure Depolama Gezgini'ne [Microsoft Azure Storage Explorer](https://storageexplorer.com) bakarak ve *kapsayıcının, konteynerin genel*olarak blob, *sampleblob*içerdiğini doğrulayarak bunun işe yaradığını doğrulayabilirsiniz. Bu örnekte, bu uygulamanın URI blobs'a giderek çalıştığını da doğrulayabilmeniz için ortak bir kapsayıcı kullandık:

```http
https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob
```

NSString'ten blok blob yüklemeye ek olarak, NSData, NSInputStream veya yerel bir dosya için de benzer yöntemler bulunur.

## <a name="list-the-blobs-in-a-container"></a>Blob’ları bir kapsayıcıda listeleme

Aşağıdaki örnek, bir kapsayıcıdaki tüm lekelerin nasıl listelenebildiğini gösterir. Bu işlemi gerçekleştirirken aşağıdaki parametrelere dikkat edin:

- **continuationToken** - Devamı belirteci, listeleme işleminin nerede başlatılması gerektiğini gösterir. Belirteç sağlanmazsa, en başından itibaren lekeler listelenir. Herhangi bir sayıda blobs, sıfırdan bir set maksimuma kadar listelenebilir. Bu yöntem sıfır sonuç döndürse bile, `results.continuationToken` sıfır değilse, serviste listelenmemiş daha fazla leke olabilir.
- **önek** - Blob listesi için kullanılacak önek belirtebilirsiniz. Yalnızca bu önek ile başlayan lekeler listelenir.
- **useFlatBlobListing** - Blob hizmeti düz bir depolama düzeni olmasına rağmen, [adlandırma ve başvuru kapları ve blobs](/rest/api/storageservices/Naming-and-Referencing-Containers--Blobs--and-Metadata) bölümünde belirtildiği gibi, yol bilgileri ile blobs adlandırarak sanal bir hiyerarşi oluşturabilirsiniz. Ancak, düz olmayan liste şu anda desteklenmez. Bu özellik yakında geliyor. Şimdilik, bu değer **EVET**olmalıdır.
- **blobListingDetails** - Lekeleri listelerken hangi öğelerin dahil edilecegini belirtebilirsiniz
  - _AZSBlobListingDetailsNone_: Yalnızca taahhüt edilen lekeleri listelayın ve blob meta verilerini döndürmeyin.
  - _AZSBlobListingDetailsSnapshots_: Liste taahhüt lekeler ve blob anlık görüntüler.
  - _AZSBlobListingDetailsMetadata_: Girişte döndürülen her blob için blob meta verilerini alın.
  - _AZSBlobListingDetailsUncommittedBlobs_: Taahhüt edilen ve taahhüt edilmeyen blobs listesi.
  - _AZSBlobListingDetailsCopy_: Listeye kopya özelliklerini ekleyin.
  - _AZSBlobListingDetailsAll_: Tüm kullanılabilir taahhüt lekeler, taahhüt edilmemiş lekeler ve anlık görüntüler listelayın ve bu lekeler için tüm meta verileri ve kopyalama durumunu döndürün.
- **maxResults** - Bu işlem için döndürülecek maksimum sonuç sayısı. Bir sınır belirlememek için -1'i kullanın.
- **completionHandler** - Listeleme işleminin sonuçlarıyla birlikte yürütülecek kod bloğu.

Bu örnekte, bir devam belirteci döndürüldeğinde liste blobs yöntemini özyinelemeli olarak çağırmak için bir yardımcı yöntemi kullanılır.

```objc
-(void)listBlobsInContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    //List all blobs in container
    [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
        if (error != nil){
            NSLog(@"Error in creating container.");
        }
    }];
}

//List blobs helper method
-(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
{
    [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
        if (error)
        {
            completionHandler(error);
        }
        else
        {
            for (int i = 0; i < results.blobs.count; i++) {
                NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
            }
            if (results.continuationToken)
            {
                [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
            }
            else
            {
                completionHandler(nil);
            }
        }
    }];
}
```

## <a name="download-a-blob"></a>Blob indirme

Aşağıdaki örnek, bir NSString nesnesine bir blob nasıl indirilir gösterir.

```objc
-(void)downloadBlobToString{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

    // Download blob
    [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
        if (error) {
            NSLog(@"Error in downloading blob");
        }
        else{
            NSLog(@"%@",text);
        }
    }];
}
```

## <a name="delete-a-blob"></a>Blob silme

Aşağıdaki örnek, bir lekenin nasıl silinir olduğunu gösterir.

```objc
-(void)deleteBlob{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Create a local blob object
    AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

    // Delete blob
    [blockBlob deleteWithCompletionHandler:^(NSError *error) {
        if (error) {
            NSLog(@"Error in deleting blob.");
        }
    }];
}
```

## <a name="delete-a-blob-container"></a>Blob kapsayıcısı silme

Aşağıdaki örnekte, bir kapsayıcının nasıl silinir gösterilmektedir.

```objc
-(void)deleteContainer{
    NSError *accountCreationError;

    // Create a storage account object from a connection string.
    AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

    if(accountCreationError){
        NSLog(@"Error in creating account.");
    }

    // Create a blob service client object.
    AZSCloudBlobClient *blobClient = [account getBlobClient];

    // Create a local container object.
    AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

    // Delete container
    [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
        if(error){
            NSLog(@"Error in deleting container");
        }
    }];
}
```

## <a name="next-steps"></a>Sonraki adımlar

Blob Depolama'yı iOS'tan kullanmayı öğrendiğiniz için, iOS kitaplığı ve Depolama hizmeti hakkında daha fazla bilgi edinmek için bu bağlantıları takip edin.

- [iOS için Azure Depolama İstemci Kitaplığı](https://github.com/azure/azure-storage-ios)
- [Azure Depolama iOS Başvuru Belgeleri](https://azure.github.io/azure-storage-ios/)
- [Azure Storage Hizmetleri REST API’si](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure Depolama Ekibi Blogu](https://blogs.msdn.com/b/windowsazurestorage)

Bu kitaplıkla ilgili sorularınız varsa, [MSDN Azure forumumuza](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) veya Stack [Overflow'a](https://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files)göndermekle çekinmeyin.
Azure Depolama için özellik önerileriniz varsa, lütfen [Azure Depolama Geri Bildirimi'ne](https://feedback.azure.com/forums/217298-storage/)gönderin.

---
title: iOS SDK'yı kullanma
description: Azure Mobil Uygulamalar için iOS SDK Nasıl Kullanılır?
ms.assetid: 4e8e45df-c36a-4a60-9ad4-393ec10b7eb9
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 1bf8f8e198f6c4a4a0af308262cd830685698a80
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249353"
---
# <a name="how-to-use-ios-client-library-for-azure-mobile-apps"></a>Azure Mobil Uygulamalar için iOS İstemci Kitaplığı Nasıl Kullanılır?

[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz, en son [Azure Mobil Uygulamaları iOS SDK'yı][1]kullanarak sık karşılaşılan senaryoları gerçekleştirmenizi öğretir. Azure Mobile Apps'ta yeniyseniz, bir arka uç oluşturmak, tablo oluşturmak ve önceden oluşturulmuş bir iOS Xcode projesini indirmek için önce [Azure Mobile Apps Hızlı Başlat'ı] tamamlayın. Bu kılavuzda, istemci tarafı iOS SDK'ya odaklanıyoruz. Arka uç için sunucu tarafındaki SDK hakkında daha fazla bilgi edinmek için Server SDK HOWTOs'a bakın.

## <a name="reference-documentation"></a>Referans belgeleri

iOS istemcisi SDK için başvuru belgeleri burada bulunmaktadır: [Azure Mobile Apps iOS İstemci Başvurusu][2].

## <a name="supported-platforms"></a>Desteklenen Platformlar

iOS SDK Objective-C projelerini, Swift 2.2 projelerini ve 8.0 veya sonraki iOS sürümleri için Swift 2.3 projelerini destekler.

"Sunucu akışı" kimlik doğrulaması, sunulan web hizmeti için bir Web Görünümü kullanır.  Aygıt bir WebView Web Hizmeti aracı sunamıyorsa, ürünün kapsamı dışında başka bir kimlik doğrulama yöntemi gerekir.  
Bu nedenle Bu SDK, Watch tipi veya benzer şekilde kısıtlanmış aygıtlar için uygun değildir.

## <a name="setup-and-prerequisites"></a><a name="Setup"></a>Kurulum ve Ön koşullar

Bu kılavuz, bir tablo ile bir arka uç oluşturduğunuzvarsa. Bu kılavuz, tablonun bu öğreticilerde tablolarla aynı şema olduğunu varsayar. Bu kılavuz da kodunuzda, başvuru `MicrosoftAzureMobile.framework` ve `MicrosoftAzureMobile/MicrosoftAzureMobile.h`alma varsayar.

## <a name="how-to-create-client"></a><a name="create-client"></a>Nasıl yapılsın: İstemci oluşturma

Projenizde bir Azure Mobil Uygulaması arka ucuna erişmek için bir `MSClient`. Uygulama `AppUrl` URL'si ile değiştirin. Buradan `gatewayURLString` ayrılıp `applicationKey` boşalabilirsiniz. Kimlik doğrulaması için bir ağ geçidi `gatewayURLString` ayarladıysanız, ağ geçidi URL'sini doldurun.

**Amaç-C**:

```objc
MSClient *client = [MSClient clientWithApplicationURLString:@"AppUrl"];
```

**Hızlı :**

```swift
let client = MSClient(applicationURLString: "AppUrl")
```

## <a name="how-to-create-table-reference"></a><a name="table-reference"></a>Nasıl yapılır: Tablo Başvurusu Oluşturma

Verilere erişmek veya verileri güncelleştirmek için arka uç tablosuna başvuru oluşturun. `TodoItem` ifadesini tablonuzun adıyla değiştirin

**Amaç-C**:

```objc
MSTable *table = [client tableWithName:@"TodoItem"];
```

**Hızlı :**

```swift
let table = client.tableWithName("TodoItem")
```

## <a name="how-to-query-data"></a><a name="querying"></a>Nasıl kullanılır: Veri Sorgula

Veritabanı sorgusu oluşturmak için `MSTable` nesneyi sorgula. Aşağıdaki sorgu tüm öğeleri `TodoItem` alır ve her öğenin metnini kaydeder.

**Amaç-C**:

```objc
[table readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) { // error is nil if no error occurred
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) { // items is NSArray of records that match query
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Hızlı :**

```swift
table.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="how-to-filter-returned-data"></a><a name="filtering"></a>Nasıl kullanılır: Döndürülen Verileri Filtrele

Sonuçları filtrelemek için birçok seçenek vardır.

Bir yüklem kullanarak filtre lemek `NSPredicate` `readWithPredicate`için bir ve . Yalnızca eksik Todo öğelerini bulmak için aşağıdaki filtreler verileri döndürmüşoldu.

**Amaç-C**:

```objc
// Create a predicate that finds items where complete is false
NSPredicate * predicate = [NSPredicate predicateWithFormat:@"complete == NO"];
// Query the TodoItem table
[table readWithPredicate:predicate completion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Hızlı :**

```swift
// Create a predicate that finds items where complete is false
let predicate =  NSPredicate(format: "complete == NO")
// Query the TodoItem table
table.readWithPredicate(predicate) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="how-to-use-msquery"></a><a name="query-object"></a>Nasıl kullanılır: MSQuery kullanın

Karmaşık bir sorgu (sıralama ve sayfalama dahil) `MSQuery` gerçekleştirmek için, doğrudan veya bir yüklem kullanarak bir nesne oluşturun:

**Amaç-C**:

```objc
MSQuery *query = [table query];
MSQuery *query = [table queryWithPredicate: [NSPredicate predicateWithFormat:@"complete == NO"]];
```

**Hızlı :**

```swift
let query = table.query()
let query = table.queryWithPredicate(NSPredicate(format: "complete == NO"))
```

`MSQuery`birkaç sorgu davranışını denetlemenize olanak tanır.

* Sonuçların sırasını belirtin
* Hangi alanların döndürülecek lerini sınırlama
* Döndürülecek kaç kaydı sınırla
* Yanıtolarak toplam sayı yı belirtin
* İstekte özel sorgu dize parametrelerini belirtin
* Ek işlevler uygulayın

Nesneyi `MSQuery` arayarak `readWithCompletion` sorguyu yürütün.

## <a name="how-to-sort-data-with-msquery"></a><a name="sorting"></a>Nasıl kullanılır: MSQuery ile Verileri Sırala

Sonuçları sıralamak için bir örneğe bakalım. Artan alana göre sıralamak için 'metin' artan, daha `MSQuery` sonra 'tam' azalan, gibi çağırmak için:

**Amaç-C**:

```objc
[query orderByAscending:@"text"];
[query orderByDescending:@"complete"];
[query readWithCompletion:^(MSQueryResult *result, NSError *error) {
        if(error) {
                NSLog(@"ERROR %@", error);
        } else {
                for(NSDictionary *item in result.items) {
                        NSLog(@"Todo Item: %@", [item objectForKey:@"text"]);
                }
        }
}];
```

**Hızlı :**

```swift
query.orderByAscending("text")
query.orderByDescending("complete")
query.readWithCompletion { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let items = result?.items {
        for item in items {
            print("Todo Item: ", item["text"])
        }
    }
}
```

## <a name="how-to-limit-fields-and-expand-query-string-parameters-with-msquery"></a><a name="selecting"></a><a name="parameters"></a>Nasıl kullanılır: MSQuery ile Alanları Sınırlandırın ve Sorgu Dize Parametrelerini Genişletin

Sorguda döndürülecek alanları sınırlamak için, **selectFields** özelliğindeki alanların adlarını belirtin. Bu örnek, yalnızca metni ve tamamlanan alanları döndürür:

**Amaç-C**:

```objc
query.selectFields = @[@"text", @"complete"];
```

**Hızlı :**

```swift
query.selectFields = ["text", "complete"]
```

Sunucu isteğine ek sorgu dize parametreleri eklemek için (örneğin, özel bir `query.parameters` sunucu tarafı komut dosyası bunları kullandığından), aşağıdaki gibi doldurma:

**Amaç-C**:

```objc
query.parameters = @{
    @"myKey1" : @"value1",
    @"myKey2" : @"value2",
};
```

**Hızlı :**

```swift
query.parameters = ["myKey1": "value1", "myKey2": "value2"]
```

## <a name="how-to-configure-page-size"></a><a name="paging"></a>Nasıl yapılandırılır: Sayfa Boyutunu Yapılandır

Azure Mobile Apps ile sayfa boyutu, arka uç tablolarından aynı anda çekilen kayıt sayısını denetler. Verilere yapılan `pull` bir çağrı, bu sayfa boyutuna bağlı olarak, çekilecek başka kayıt olmayana kadar verileri toplu olarak tamamlar.

Aşağıda gösterildiği gibi **MSPullSettings** kullanarak bir sayfa boyutunu yapılandırmak mümkündür. Varsayılan sayfa boyutu 50'dir ve aşağıdaki örnek onu 3 olarak değiştirir.

Performans nedenleriyle farklı bir sayfa boyutunu yapılandırabilirsiniz. Çok sayıda küçük veri kaydınız varsa, yüksek sayfa boyutu sunucu nun gidiş-dönüş sayısını azaltır.

Bu ayar yalnızca istemci tarafındaki sayfa boyutunu denetler. İstemci Mobil Uygulamalar arka uç desteklerinin daha büyük bir sayfa boyutu isterse, sayfa boyutu destekiçin yapılandırılan en üst uçta kapatılır.

Bu ayar aynı zamanda veri kayıtlarının *sayısıdır,* *bayt boyutu*değil.

İstemci sayfa boyutunu artırırsanız, sunucudaki sayfa boyutunu da artırmanız gerekir. Bunu yapmak için adımlar için ["Nasıl tablo sayfalama boyutunu ayarlayın"](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) konusuna bakın.

**Amaç-C**:

```objc
  MSPullSettings *pullSettings = [[MSPullSettings alloc] initWithPageSize:3];
  [table  pullWithQuery:query queryId:@nil settings:pullSettings
                        completion:^(NSError * _Nullable error) {
                               if(error) {
                    NSLog(@"ERROR %@", error);
                }
                           }];
```

**Hızlı :**

```swift
let pullSettings = MSPullSettings(pageSize: 3)
table.pullWithQuery(query, queryId:nil, settings: pullSettings) { (error) in
    if let err = error {
        print("ERROR ", err)
    }
}
```

## <a name="how-to-insert-data"></a><a name="inserting"></a>Nasıl kullanılır: Veri Ekleme

Yeni bir tablo satırı eklemek `NSDictionary` için `table insert`bir tane oluşturun ve çağırın. [Dinamik Şema] etkinse, Azure Uygulama Hizmeti mobil arka ucu otomatik `NSDictionary`olarak .

`id` Sağlanmazsa, arka uç otomatik olarak yeni bir benzersiz kimlik oluşturur. E-posta `id` adreslerini, kullanıcı adlarınızı veya kendi özel değerlerinizi kimlik olarak kullanmak için kendi bilgilerinizi sağlayın. Kendi kimliğinizi sağlamak, birleştirmeleri ve iş odaklı veritabanı mantığını kolaylaştırabilir.

Eklenen `result` yeni öğeyi içerir. Sunucu mantığınıza bağlı olarak, sunucuya aktarılanverilerle karşılaştırıldığında ek veya değiştirilmiş verilere sahip olabilir.

**Amaç-C**:

```objc
NSDictionary *newItem = @{@"id": @"custom-id", @"text": @"my new item", @"complete" : @NO};
[table insert:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Hızlı :**

```swift
let newItem = ["id": "custom-id", "text": "my new item", "complete": false]
table.insert(newItem) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

## <a name="how-to-modify-data"></a><a name="modifying"></a>Nasıl Kullanılır: Verileri Değiştirme

Varolan bir satırı güncelleştirmek için `update`bir öğeyi değiştirin ve şu ları arayın:

**Amaç-C**:

```objc
NSMutableDictionary *newItem = [oldItem mutableCopy]; // oldItem is NSDictionary
[newItem setValue:@"Updated text" forKey:@"text"];
[table update:newItem completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Hızlı :**

```swift
if let newItem = oldItem.mutableCopy() as? NSMutableDictionary {
    newItem["text"] = "Updated text"
    table2.update(newItem as [NSObject: AnyObject], completion: { (result, error) -> Void in
        if let err = error {
            print("ERROR ", err)
        } else if let item = result {
            print("Todo Item: ", item["text"])
        }
    })
}
```

Alternatif olarak, satır kimliğini ve güncelleştirilmiş alanı sağlama:

**Amaç-C**:

```objc
[table update:@{@"id":@"custom-id", @"text":"my EDITED item"} completion:^(NSDictionary *result, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item: %@", [result objectForKey:@"text"]);
    }
}];
```

**Hızlı :**

```swift
table.update(["id": "custom-id", "text": "my EDITED item"]) { (result, error) in
    if let err = error {
        print("ERROR ", err)
    } else if let item = result {
        print("Todo Item: ", item["text"])
    }
}
```

En azından, `id` güncelleştirmeler yaparken öznitelik ayarlanmalıdır.

## <a name="how-to-delete-data"></a><a name="deleting"></a>Nasıl Kullanılır: Verileri Silme

Bir öğeyi silmek için, öğeyi çağırın: `delete`

**Amaç-C**:

```objc
[table delete:item completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Hızlı :**

```swift
table.delete(newItem as [NSObject: AnyObject]) { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

Alternatif olarak, bir satır kimliği sağlayarak silin:

**Amaç-C**:

```objc
[table deleteWithId:@"37BBF396-11F0-4B39-85C8-B319C729AF6D" completion:^(id itemId, NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    } else {
        NSLog(@"Todo Item ID: %@", itemId);
    }
}];
```

**Hızlı :**

```swift
table.deleteWithId("37BBF396-11F0-4B39-85C8-B319C729AF6D") { (itemId, error) in
    if let err = error {
        print("ERROR ", err)
    } else {
        print("Todo Item ID: ", itemId)
    }
}
```

En azından, `id` silme yaparken öznitelik ayarlanmalıdır.

## <a name="how-to-call-custom-api"></a><a name="customapi"></a>Nasıl yapılsın: Özel API'yi arayın

Özel bir API ile, herhangi bir arka uç işlevselliğini ortaya çıkarabilirsiniz. Bir tablo işleminin haritasını çıkarmasına gerek yok. Yalnızca mesajlaşma üzerinde daha fazla kontrol elde etmekle kalmıyor, üstbilgi başlıklarını okuyabilir/ayarlayabilir ve yanıt gövdesi biçimini değiştirebilirsiniz.

Özel bir API aramak `MSClient.invokeAPI`için. İstek ve yanıt içeriği JSON olarak kabul edilir. Diğer ortam türlerini kullanmak için, [diğer aşırı `invokeAPI`yüklemeyi kullanın. ][5]  `POST` İstek yerine `GET` istekte bulunmak için `HTTPMethod` `"GET"` parametreyi ve `body` `nil` parametreyi ayarla (GET istekleriileti gövdeleri olmadığından.) Özel API'niz diğer HTTP fiillerini destekliyorsa, uygun şekilde değiştirin. `HTTPMethod`

**Amaç-C**:

```objc
[self.client invokeAPI:@"sendEmail"
                  body:@{ @"contents": @"Hello world!" }
            HTTPMethod:@"POST"
            parameters:@{ @"to": @"bill@contoso.com", @"subject" : @"Hi!" }
               headers:nil
            completion: ^(NSData *result, NSHTTPURLResponse *response, NSError *error) {
                if(error) {
                    NSLog(@"ERROR %@", error);
                } else {
                    // Do something with result
                }
            }];
```

**Hızlı :**

```swift
client.invokeAPI("sendEmail",
            body: [ "contents": "Hello World" ],
            HTTPMethod: "POST",
            parameters: [ "to": "bill@contoso.com", "subject" : "Hi!" ],
            headers: nil)
            {
                (result, response, error) -> Void in
                if let err = error {
                    print("ERROR ", err)
                } else if let res = result {
                          // Do something with result
                }
        }
```

## <a name="how-to-register-push-templates-to-send-cross-platform-notifications"></a><a name="templates"></a>Nasıl yapilir: Platformlar arası bildirimler göndermek için itme şablonlarını kaydetme

Şablonları kaydetmek için, istemci uygulamanızda **istemci.push registerDeviceToken** yöntemi ile şablonları geçirin.

**Amaç-C**:

```objc
[client.push registerDeviceToken:deviceToken template:iOSTemplate completion:^(NSError *error) {
    if(error) {
        NSLog(@"ERROR %@", error);
    }
}];
```

**Hızlı :**

```swift
client.push?.registerDeviceToken(NSData(), template: iOSTemplate, completion: { (error) in
    if let err = error {
        print("ERROR ", err)
    }
})
```

Şablonlarınız NSDictionary türündedir ve aşağıdaki biçimde birden çok şablon içerebilir:

**Amaç-C**:

```objc
NSDictionary *iOSTemplate = @{ @"templateName": @{ @"body": @{ @"aps": @{ @"alert": @"$(message)" } } } };
```

**Hızlı :**

```swift
let iOSTemplate = ["templateName": ["body": ["aps": ["alert": "$(message)"]]]]
```

Tüm etiketler güvenlik isteğinden çıkarılır.  Yüklemelere veya yüklemelere etiketler eklemek için Azure [Mobil Uygulamaları için .NET arka uç sunucusu SDK ile çalışma][4]'ya bakın.  Bu kayıtlı şablonları kullanarak bildirim göndermek için [Bildirim Hub'ları API'leri][3]ile çalışın.

## <a name="how-to-handle-errors"></a><a name="errors"></a>Nasıl yapılır: Hataları işleme

Azure Uygulama Hizmeti mobil arka ucunu aradiğinizde, `NSError` tamamlama bloğu bir parametre içerir. Bir hata oluştuğunda, bu parametre sıfır değildir. Kodunuzda, bu parametreyi denetlemeli ve önceki kod parçacıklarında gösterildiği gibi hatayı gerektiği gibi işlemeniz gerekir.

Dosya [`<WindowsAzureMobileServices/MSError.h>`][6] `MSErrorResponseKey`sabitleri tanımlar , `MSErrorRequestKey`ve `MSErrorServerItemKey`. Hatayla ilgili daha fazla veri almak için:

**Amaç-C**:

```objc
NSDictionary *serverItem = [error.userInfo objectForKey:MSErrorServerItemKey];
```

**Hızlı :**

```swift
let serverItem = error.userInfo[MSErrorServerItemKey]
```

Buna ek olarak, dosya her hata kodu için sabitleri tanımlar:

**Amaç-C**:

```objc
if (error.code == MSErrorPreconditionFailed) {
```

**Hızlı :**

```swift
if (error.code == MSErrorPreconditionFailed) {
```

## <a name="how-to-authenticate-users-with-the-active-directory-authentication-library"></a><a name="adal"></a>Nasıl yapilir: Active Directory Authentication Library ile kullanıcıların kimliğini doğrula

Azure Active Directory kullanarak kullanıcıları uygulamanızda oturum haline getirmek için Etkin Dizin Kimlik Doğrulama Kitaplığını (ADAL) kullanabilirsiniz. Bir kimlik sağlayıcısı SDK kullanarak istemci akışı kimlik `loginWithProvider:completion:` doğrulaması yöntemi kullanarak tercih edilir.  İstemci akışı kimlik doğrulaması daha yerel bir UX hissi sağlar ve ek özelleştirme sağlar.

1. [Active Directory giriş][7] için Uygulama Hizmetini yapılandırma yı izleyerek AAD oturum açma için mobil uygulama arka uçunuzu yapılandırın. Yerel istemci uygulamasını kaydetmenin isteğe bağlı adımını tamamladıktan emin olun. iOS için, yeniden yönlendirme URI şeklinde `<app-scheme>://<bundle-id>`olduğunu öneririz. Daha fazla bilgi için [ADAL iOS hızlı başlatın.][8]
2. Kakaopodlar kullanarak ADAL yükleyin. Podfilenizi aşağıdaki tanımı içerecek şekilde ve **YOUR-PROJECT'i** Xcode projenizin adıyla değiştirerek edin:

        source 'https://github.com/CocoaPods/Specs.git'
        link_with ['YOUR-PROJECT']
        xcodeproj 'YOUR-PROJECT'

   ve Pod:

        pod 'ADALiOS'

3. Terminali kullanarak, `pod install` projenizi içeren dizinden çalıştırın ve ardından oluşturulan Xcode çalışma alanını (projeyi değil) açın.
4. Kullandığınız dile göre uygulamanıza aşağıdaki kodu ekleyin. Her birinde, şu değiştirmeleri yapın:

   * **INSERT-AUTHORITY-HERE'ı** başvurunuzu yaptığınız kiracının adı ile değiştirin. Biçimi olmalıdır. https://login.microsoftonline.com/contoso.onmicrosoft.com Bu değer, [Azure portalındaki]Azure Etkin Dizininizdeki Etki Alanı sekmesinden kopyalanabilir.
   * **INSERT-RESOURCE-ID-HERE'ı** mobil uygulama arka ucunuz için istemci kimliğiyle değiştirin. Portaldaki **Azure Etkin Dizin Ayarları** altında **Gelişmiş** sekmesinden istemci kimliğini edinebilirsiniz.
   * **INSERT-CLIENT-ID-HERE'ı** yerel istemci uygulamasından kopyaladığınız istemci kimliğiyle değiştirin.
   * HTTPS şemasını kullanarak **INSERT-REDIRECT-URI-HERE'ı** sitenizin */.auth/login/done* endpoint'iyle değiştirin. Bu değer *https://contoso.azurewebsites.net/.auth/login/done*.

**Amaç-C**:

```objc
#import <ADALiOS/ADAuthenticationContext.h>
#import <ADALiOS/ADAuthenticationSettings.h>
// ...
- (void) authenticate:(UIViewController*) parent
            completion:(void (^) (MSUser*, NSError*))completionBlock;
{
    NSString *authority = @"INSERT-AUTHORITY-HERE";
    NSString *resourceId = @"INSERT-RESOURCE-ID-HERE";
    NSString *clientId = @"INSERT-CLIENT-ID-HERE";
    NSURL *redirectUri = [[NSURL alloc]initWithString:@"INSERT-REDIRECT-URI-HERE"];
    ADAuthenticationError *error;
    ADAuthenticationContext *authContext = [ADAuthenticationContext authenticationContextWithAuthority:authority error:&error];
    authContext.parentController = parent;
    [ADAuthenticationSettings sharedInstance].enableFullScreen = YES;
    [authContext acquireTokenWithResource:resourceId
                                    clientId:clientId
                                redirectUri:redirectUri
                            completionBlock:^(ADAuthenticationResult *result) {
                                if (result.status != AD_SUCCEEDED)
                                {
                                    completionBlock(nil, result.error);;
                                }
                                else
                                {
                                    NSDictionary *payload = @{
                                                            @"access_token" : result.tokenCacheStoreItem.accessToken
                                                            };
                                    [client loginWithProvider:@"aad" token:payload completion:completionBlock];
                                }
                            }];
}
```

**Hızlı :**

```swift
// add the following imports to your bridging header:
//        #import <ADALiOS/ADAuthenticationContext.h>
//        #import <ADALiOS/ADAuthenticationSettings.h>

func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
    let authority = "INSERT-AUTHORITY-HERE"
    let resourceId = "INSERT-RESOURCE-ID-HERE"
    let clientId = "INSERT-CLIENT-ID-HERE"
    let redirectUri = NSURL(string: "INSERT-REDIRECT-URI-HERE")
    var error: AutoreleasingUnsafeMutablePointer<ADAuthenticationError?> = nil
    let authContext = ADAuthenticationContext(authority: authority, error: error)
    authContext.parentController = parent
    ADAuthenticationSettings.sharedInstance().enableFullScreen = true
    authContext.acquireTokenWithResource(resourceId, clientId: clientId, redirectUri: redirectUri) { (result) in
            if result.status != AD_SUCCEEDED {
                completion(nil, result.error)
            }
            else {
                let payload: [String: String] = ["access_token": result.tokenCacheStoreItem.accessToken]
                client.loginWithProvider("aad", token: payload, completion: completion)
            }
        }
}
```

## <a name="how-to-authenticate-users-with-the-facebook-sdk-for-ios"></a><a name="facebook-sdk"></a>Nasıl yapılı: iOS için Facebook SDK ile kullanıcıların kimliğini doğrula

Facebook'u kullanarak uygulamanızda kullanıcıları oturum alamak için iOS için Facebook SDK'yı kullanabilirsiniz.  İstemci akışı kimlik doğrulaması kullanmak `loginWithProvider:completion:` yöntemi kullanmaya tercih edilir.  İstemci akışı kimlik doğrulaması daha yerel bir UX hissi sağlar ve ek özelleştirme sağlar.

1. Facebook oturum açma için Uygulama [Hizmeti'ni nasıl yapılandırAbilirsiniz][9] uygulamasını takip ederek Facebook oturum açma için mobil uygulama arka uçunuzu yapılandırın.
2. iOS için Facebook SDK'sını takip ederek iOS için Facebook [SDK'yı yükleyin - Dokümantasyona Başlarken.][10] Bir uygulama oluşturmak yerine, iOS platformunu mevcut kaydınıza ekleyebilirsiniz.
3. Facebook'un belgeleri, Uygulama Temsilcisi'nde bazı Objective-C kodlarını içerir. **Swift**kullanıyorsanız, AppDelegate.swift için aşağıdaki çevirileri kullanabilirsiniz:

    ```swift
    // Add the following import to your bridging header:
    //        #import <FBSDKCoreKit/FBSDKCoreKit.h>

    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
        FBSDKApplicationDelegate.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions)
        // Add any custom logic here.
        return true
    }

    func application(application: UIApplication, openURL url: NSURL, sourceApplication: String?, annotation: AnyObject?) -> Bool {
        let handled = FBSDKApplicationDelegate.sharedInstance().application(application, openURL: url, sourceApplication: sourceApplication, annotation: annotation)
        // Add any custom logic here.
        return handled
    }
    ```
4. Projenize eklemenin `FBSDKCoreKit.framework` yanı sıra, aynı `FBSDKLoginKit.framework` şekilde bir başvuru da ekleyin.
5. Kullandığınız dile göre uygulamanıza aşağıdaki kodu ekleyin.

    **Amaç-C**:

    ```objc
    #import <FBSDKLoginKit/FBSDKLoginKit.h>
    #import <FBSDKCoreKit/FBSDKAccessToken.h>
    // ...
    - (void) authenticate:(UIViewController*) parent
                completion:(void (^) (MSUser*, NSError*)) completionBlock;
    {
        FBSDKLoginManager *loginManager = [[FBSDKLoginManager alloc] init];
        [loginManager
            logInWithReadPermissions: @[@"public_profile"]
            fromViewController:parent
            handler:^(FBSDKLoginManagerLoginResult *result, NSError *error) {
                if (error) {
                    completionBlock(nil, error);
                } else if (result.isCancelled) {
                    completionBlock(nil, error);
                } else {
                    NSDictionary *payload = @{
                                            @"access_token":result.token.tokenString
                                            };
                    [client loginWithProvider:@"facebook" token:payload completion:completionBlock];
                }
            }];
    }
    ```

    **Hızlı :**

    ```swift
    // Add the following imports to your bridging header:
    //        #import <FBSDKLoginKit/FBSDKLoginKit.h>
    //        #import <FBSDKCoreKit/FBSDKAccessToken.h>

    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let loginManager = FBSDKLoginManager()
        loginManager.logInWithReadPermissions(["public_profile"], fromViewController: parent) { (result, error) in
            if (error != nil) {
                completion(nil, error)
            }
            else if result.isCancelled {
                completion(nil, error)
            }
            else {
                let payload: [String: String] = ["access_token": result.token.tokenString]
                client.loginWithProvider("facebook", token: payload, completion: completion)
            }
        }
    }
    ```

## <a name="how-to-authenticate-users-with-twitter-fabric-for-ios"></a><a name="twitter-fabric"></a>Nasıl: iOS için Twitter Kumaş ı ile kullanıcıları doğrulamak

Kullanıcıları Twitter'ı kullanarak uygulamanızda oturum alabilmek için iOS için Fabric'i kullanabilirsiniz. İstemci Akışı kimlik doğrulaması, daha yerel bir UX hissi sağladığı ndan ve ek özelleştirmeye olanak sağladığından, `loginWithProvider:completion:` yöntemi kullanmaya tercih edilir.

1. Twitter oturum açma için mobil uygulama arka uçunuzu, Twitter oturum açma öğreticisi [için Uygulama Hizmeti'ni nasıl yapılandırAbilirsiniz'ı](../app-service/configure-authentication-provider-twitter.md) izleyerek yapılandırın.
2. iOS için Kumaş - [Dokümantasyona Başlarken] ve TwitterKit'i kurarak projenize Kumaş ekleyin.

   > [!NOTE]
   > Varsayılan olarak, Kumaş sizin için bir Twitter uygulaması oluşturur. Daha önce oluşturduğunuz Tüketici Anahtarı ve Tüketici Sırrı'nı aşağıdaki kod parçacıklarını kullanarak kaydederek uygulama oluşturmayı önleyebilirsiniz.    Alternatif olarak, Uygulama Hizmetine sağladığınız Tüketici Anahtarı ve Tüketici Sırrı değerlerini [Kumaş Panosunda]gördüğünüz değerlerle değiştirebilirsiniz. Bu seçeneği belirlerseniz, geri arama URL'sini yer tutucu değerine `https://<yoursitename>.azurewebsites.net/.auth/login/twitter/callback`ayarladığınızdan emin olun.

    Daha önce oluşturduğunuz sırları kullanmayı seçerseniz, Uygulama Temsilcinize aşağıdaki kodu ekleyin:

    **Amaç-C**:

    ```objc
    #import <Fabric/Fabric.h>
    #import <TwitterKit/TwitterKit.h>
    // ...
    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
        [[Twitter sharedInstance] startWithConsumerKey:@"your_key" consumerSecret:@"your_secret"];
        [Fabric with:@[[Twitter class]]];
        // Add any custom logic here.
        return YES;
    }
    ```

    **Hızlı :**

    ```swift
    import Fabric
    import TwitterKit
    // ...
    func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject : AnyObject]?) -> Bool {
        Twitter.sharedInstance().startWithConsumerKey("your_key", consumerSecret: "your_secret")
        Fabric.with([Twitter.self])
        // Add any custom logic here.
        return true
    }
    ```

3. Kullandığınız dile göre uygulamanıza aşağıdaki kodu ekleyin.

    **Amaç-C**:

    ```objc
    #import <TwitterKit/TwitterKit.h>
    // ...
    - (void)authenticate:(UIViewController*)parent completion:(void (^) (MSUser*, NSError*))completionBlock
    {
        [[Twitter sharedInstance] logInWithCompletion:^(TWTRSession *session, NSError *error) {
            if (session) {
                NSDictionary *payload = @{
                                            @"access_token":session.authToken,
                                            @"access_token_secret":session.authTokenSecret
                                        };
                [client loginWithProvider:@"twitter" token:payload completion:completionBlock];
            } else {
                completionBlock(nil, error);
            }
        }];
    }
    ```

    **Hızlı :**

    ```swift
    import TwitterKit
    // ...
    func authenticate(parent: UIViewController, completion: (MSUser?, NSError?) -> Void) {
        let client = self.table!.client
        Twitter.sharedInstance().logInWithCompletion { session, error in
            if (session != nil) {
                let payload: [String: String] = ["access_token": session!.authToken, "access_token_secret": session!.authTokenSecret]
                client.loginWithProvider("twitter", token: payload, completion: completion)
            } else {
                completion(nil, error)
            }
        }
    }
    ```

## <a name="how-to-authenticate-users-with-the-google-sign-in-sdk-for-ios"></a><a name="google-sdk"></a>Nasıl yapilir: iOS için Google Oturum Açma SDK'lı kullanıcıların kimliğini doğrulayın

Kullanıcıları bir Google hesabı kullanarak uygulamanızda oturum alabilmek için iOS için Google Oturum Açma SDK'sını kullanabilirsiniz.  Google son zamanlarda Kendi OAuth güvenlik politikalarında değişiklikler duyurdu.  Bu politika değişiklikleri, gelecekte Google SDK'nın kullanımını gerektirir.

1. Google oturum açma için mobil uygulama arka uçunuzu, Google oturum açma öğreticisi [için Uygulama Hizmeti'ni nasıl yapılandırılatır'](../app-service/configure-authentication-provider-google.md) ı izleyerek yapılandırın.
2. iOS için Google Oturum Açma'yı izleyerek iOS için Google SDK'yı yükleyin - Belgeleri [tümleştirmeye başlayın.](https://developers.google.com/identity/sign-in/ios/start-integrating) "Arka uç sunucusuyla kimlik doğrulaması" bölümünü atlayabilirsiniz.
3. Kullandığınız dile göre temsilcinizin `signIn:didSignInForUser:withError:` yöntemine aşağıdakileri ekleyin.

    **Amaç-C**:
    ```objc
    NSDictionary *payload = @{
                                @"id_token":user.authentication.idToken,
                                @"authorization_code":user.serverAuthCode
                                };

    [client loginWithProvider:@"google" token:payload completion:^(MSUser *user, NSError *error) {
        // ...
    }];
    ```

    **Hızlı :**

    ```swift
    let payload: [String: String] = ["id_token": user.authentication.idToken, "authorization_code": user.serverAuthCode]
    client.loginWithProvider("google", token: payload) { (user, error) in
        // ...
    }
    ```

4. Uygulama temsilcinize, "SERVER_CLIENT_ID"ı adım 1'de Uygulama Hizmeti'ni yapılandırmak için kullandığınız kimlikle değiştirerek aşağıdakileri `application:didFinishLaunchingWithOptions:` de eklediğinizden emin olun.

    **Amaç-C**:

    ```objc
    [GIDSignIn sharedInstance].serverClientID = @"SERVER_CLIENT_ID";
    ```

     **Hızlı :**

    ```swift
    GIDSignIn.sharedInstance().serverClientID = "SERVER_CLIENT_ID"
    ```

5. Kullandığınız dile göre `GIDSignInUIDelegate` protokolü uygulayan bir UIViewController'ta uygulamanız için aşağıdaki kodu ekleyin.  Yeniden oturum açmadan önce oturumunuz tamamlanır ve kimlik bilgilerinizi yeniden girmeniz gerekmese de bir onay iletişim kutusu görürsünüz.  Yalnızca oturum belirteci süresi dolduğunda bu yöntemi arayın.

   **Amaç-C**:

    ```objc
    #import <Google/SignIn.h>
    // ...
    - (void)authenticate
    {
            [GIDSignIn sharedInstance].uiDelegate = self;
            [[GIDSignIn sharedInstance] signOut];
            [[GIDSignIn sharedInstance] signIn];
    }
    ```

   **Hızlı :**

    ```swift
    // ...
    func authenticate() {
        GIDSignIn.sharedInstance().uiDelegate = self
        GIDSignIn.sharedInstance().signOut()
        GIDSignIn.sharedInstance().signIn()
    }
    ```

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[Setup and Prerequisites]: #Setup
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #table-reference
[How to: Query data from a mobile service]: #querying
[Filter returned data]: #filtering
[Sort returned data]: #sorting
[Return data in pages]: #paging
[Select specific columns]: #selecting
[How to: Bind data to the user interface]: #binding
[How to: Insert data into a mobile service]: #inserting
[How to: Modify data in a mobile service]: #modifying
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching-tokens
[How to: Upload images and large files]: #blobs
[How to: Handle errors]: #errors
[How to: Design unit tests]: #unit-testing
[How to: Customize the client]: #customizing
[Customize request headers]: #custom-headers
[Customize data type serialization]: #custom-serialization
[Next Steps]: #next-steps
[How to: Use MSQuery]: #query-object

<!-- Images. -->

<!-- URLs. -->
[Azure Mobil Uygulamalar Hızlı Başlangıç]: app-service-mobile-ios-get-started.md

[Add Mobile Services to Existing App]: /develop/mobile/tutorials/get-started-data
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Validate and modify data in Mobile Services by using server scripts]: /develop/mobile/tutorials/validate-modify-and-augment-data-ios
[Mobile Services SDK]: https://go.microsoft.com/fwLink/p/?LinkID=266533
[Authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[iOS SDK]: https://developer.apple.com/xcode
[Azure portalında]: https://portal.azure.com/
[Handling Expired Tokens]: https://go.microsoft.com/fwlink/p/?LinkId=301955
[Live Connect SDK]: https://go.microsoft.com/fwlink/p/?LinkId=301960
[Permissions]: https://msdn.microsoft.com/library/windowsazure/jj193161.aspx
[Service-side Authorization]: mobile-services-javascript-backend-service-side-authorization.md
[Use scripts to authorize users]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[Dinamik Şema]: https://github.com/Azure/azure-mobile-apps-node/tree/master/samples/dynamic-schema
[How to: access custom parameters]: /develop/mobile/how-to-guides/work-with-server-scripts#access-headers
[Create a table]: https://msdn.microsoft.com/library/windowsazure/jj193162.aspx
[NSDictionary object]: https://go.microsoft.com/fwlink/p/?LinkId=301965
[ASCII control codes C0 and C1]: https://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[CLI to manage Mobile Services tables]: /cli/azure/get-started-with-az-cli2
[Conflict-Handler]: mobile-services-ios-handling-conflicts-offline-data.md#add-conflict-handling

[Kumaş Pano]: https://www.fabric.io/home
[iOS için Kumaş - Başlarken]: https://docs.fabric.io/ios/fabric/getting-started.html
[1]: https://github.com/Azure/azure-mobile-apps-ios-client/blob/master/README.md#ios-client-sdk
[2]: https://azure.github.io/azure-mobile-apps-ios-client/
[3]: https://msdn.microsoft.com/library/azure/dn495101.aspx
[4]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#tags
[5]: https://azure.github.io/azure-mobile-services/iOS/v3/Classes/MSClient.html#//api/name/invokeAPI:data:HTTPMethod:parameters:headers:completion:
[6]: https://github.com/Azure/azure-mobile-services/blob/master/sdk/iOS/src/MSError.h
[7]: ../app-service/configure-authentication-provider-aad.md
[8]:../active-directory/develop/quickstart-v2-ios.md
[9]: ../app-service/configure-authentication-provider-facebook.md
[10]: https://developers.facebook.com/docs/ios/getting-started

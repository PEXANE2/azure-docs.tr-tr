---
title: Node.js için Azure CDN SDK ile başlayın | Microsoft Dokümanlar
description: Azure CDN'yi yönetmek için Node.js uygulamalarını nasıl yazacağız öğrenin.
services: cdn
documentationcenter: nodejs
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: c4bb6a61-de3d-4f0c-9dca-202554c43dfa
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 18dbcbf93947306334ccc2c156d9266884198e19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67594147"
---
# <a name="get-started-with-azure-cdn-development"></a>Azure CDN ile geliştirmeye başlama
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

CDN profillerinin ve uç noktalarının oluşturulmasını ve yönetimini otomatikleştirmek [için Node.js için Azure CDN SDK'yı](https://www.npmjs.com/package/azure-arm-cdn) kullanabilirsiniz.  Bu öğretici, kullanılabilir işlemlerin birkaçını gösteren basit bir Node.js konsol uygulamasının oluşturulmasında yürür.  Bu öğretici, Node.js için Azure CDN SDK'nın tüm yönlerini ayrıntılı olarak açıklamak için tasarlanmamıştır.

Bu öğreticiyi tamamlamak için, [node.js](https://www.nodejs.org) **4.x.x** veya daha yüksek yüklü ve yapılandırılmış olmalıdır.  Düğüm.js uygulamanızı oluşturmak için istediğiniz herhangi bir metin düzenleyicisini kullanabilirsiniz.  Bu öğretici yazmak için, Visual [Studio Code](https://code.visualstudio.com)kullanılır.  

> [!TIP]
> [Bu öğretici tamamlanan proje](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) MSDN indirmek için kullanılabilir.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Projenizi oluşturun ve NPM bağımlılıkları ekleyin
CDN profillerimiz için bir kaynak grubu oluşturduğumuza ve Azure AD uygulamamıza bu grup içindeki CDN profillerini ve uç noktalarını yönetme izni verdiğimize göre, uygulamamızı oluşturmaya başlayabiliriz.

Uygulamanızı depolamak için bir klasör oluşturun.  Geçerli yolunuzdaki Düğüm.js araçlarının bulunduğu bir konsoldan, geçerli konumunuzu bu yeni klasöre ayarlayın ve aşağıdakileri gerçekleştirerek projenizi başlatma:

    npm init

Daha sonra projenizi başlatmanız için bir dizi soru sunulacaktır.  **Giriş noktası**için, bu öğretici *app.js*kullanır.  Diğer seçeneklerimi aşağıdaki örnekte görebilirsiniz.

![NPM init çıkışı](./media/cdn-app-dev-node/cdn-npm-init.png)

Projemiz şimdi bir *packages.json* dosyası ile başharfe büründür.  Projemiz, NPM paketlerinde bulunan bazı Azure kitaplıklarını kullanacaktır.  Node.js için Azure İstemci Çalışma Süresi (ms-rest-azure) ve Node.js için Azure CDN İstemci Kitaplığını (azure-kol-cd) kullanırız.  Bunları projeye bağımlılık olarak ekleyelim.

    npm install --save ms-rest-azure
    npm install --save azure-arm-cdn

Paketler yükleme bittikten *sonra, package.json* dosyası bu örneğe benzer olmalıdır (sürüm numaraları farklı olabilir):

``` json
{
  "name": "cdn_node",
  "version": "1.0.0",
  "description": "Azure CDN Node.js tutorial project",
  "main": "app.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Cam Soper",
  "license": "MIT",
  "dependencies": {
    "azure-arm-cdn": "^0.2.1",
    "ms-rest-azure": "^1.14.4"
  }
}
```

Son olarak, metin düzenleyicinizi kullanarak boş bir metin dosyası oluşturun ve proje klasörümüzün köküne *app.js*olarak kaydedin.  Artık kod yazmaya hazırız.

## <a name="requires-constants-authentication-and-structure"></a>Gerektirir, sabitler, kimlik doğrulama ve yapı
Editörümüzde *app.js* açık ken, programımızın temel yapısını yazalım.

1. NPM paketlerimiz için en üstteki "gerektirir"i aşağıdakilerle ekleyin:
   
    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```
2. Yöntemlerimizin kullanacağı bazı sabitleri tanımlamalıyız.  Aşağıdakileri ekleyin.  Açı braketleri de dahil olmak üzere yer **tutucuları&gt;gerektiğinde kendi değerlerinizle değiştirdiğinizden emin olun. &lt;**
   
    ``` javascript
    //Tenant app constants
    const clientId = "<YOUR CLIENT ID>";
    const clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    const tenantId = "<YOUR TENANT ID>";
   
    //Application constants
    const subscriptionId = "<YOUR SUBSCRIPTION ID>";
    const resourceGroupName = "CdnConsoleTutorial";
    const resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```
3. Daha sonra, CDN yönetim istemcisini anında ve kimlik bilgilerimizi vereceğiz.
   
    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Tek tek kullanıcı kimlik doğrulaması kullanıyorsanız, bu iki satır biraz farklı görünecektir.
   
   > [!IMPORTANT]
   > Bu kod örneğini yalnızca hizmet sorumlusu yerine tek tek kullanıcı kimlik doğrulaması yapmayı tercih ediyorsanız kullanın.  Bireysel kullanıcı kimlik bilgilerinizi korumak ve onları gizli tutmak için dikkatli olun.
   > 
   > 
   
    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    ** &lt;Açı&gt; parantezindeki** öğeleri doğru bilgilerle değiştirdiğinden emin olun.  Çünkü, `<redirect URI>`uygulamayı Azure AD'ye kaydettirirken girdiğiniz yeniden yönlendirme URI'yi kullanın.
4. Bizim Node.js konsol uygulaması bazı komut satırı parametreleri alacak.  En az bir parametrenin geçtiğini doğrulayalım.
   
   ```javascript
   //Collect command-line parameters
   var parms = process.argv.slice(2);
   
   //Do we have parameters?
   if(parms == null || parms.length == 0)
   {
       console.log("Not enough parameters!");
       console.log("Valid commands are list, delete, create, and purge.");
       process.exit(1);
   }
   ```
5. Bu da bizi programımızın ana kısmına getiriyor, hangi parametrelerin aktarıldığına göre diğer işlevlere dallandık.
   
    ```javascript
    switch(parms[0].toLowerCase())
    {
        case "list":
            cdnList();
            break;
   
        case "create":
            cdnCreate();
            break;
   
        case "delete":
            cdnDelete();
            break;
   
        case "purge":
            cdnPurge();
            break;
   
        default:
            console.log("Valid commands are list, delete, create, and purge.");
            process.exit(1);
    }
    ```
6. Programımızın çeşitli yerlerinde, doğru sayıda parametrenin aktarıldığından emin olmamız ve doğru görünmedikleri takdirde yardım göstermemiz gerekir.  Bunu yapmak için fonksiyonlar oluşturalım.
   
   ```javascript
   function requireParms(parmCount) {
       if(parms.length < parmCount) {
           usageHelp(parms[0].toLowerCase());
           process.exit(1);
       }
   }
   
   function usageHelp(cmd) {
       console.log("Usage for " + cmd + ":");
       switch(cmd)
       {
           case "list":
               console.log("list profiles");
               console.log("list endpoints <profile name>");
               break;
   
           case "create":
               console.log("create profile <profile name>");
               console.log("create endpoint <profile name> <endpoint name> <origin hostname>");
               break;
   
           case "delete":
               console.log("delete profile <profile name>");
               console.log("delete endpoint <profile name> <endpoint name>");
               break;
   
           case "purge":
               console.log("purge <profile name> <endpoint name> <path>");
               break;
   
           default:
               console.log("Invalid command.");
       }
   }
   ```
7. Son olarak, CDN yönetim istemcisi üzerinde kullanacağımız işlevler eşzamanlıdır, bu nedenle iş bittiğinde geri çağırmak için bir yönteme ihtiyaçları vardır.  CDN yönetim istemcisinden çıktıyı görüntüleyebilen (varsa) ve programdan incelikle çıkabilen bir tane yapalım.
   
    ```javascript
    function callback(err, result, request, response) {
        if (err) {
            console.log(err);
            process.exit(1);
        } else {
            console.log((result == null) ? "Done!" : result);
            process.exit(0);
        }
    }
    ```

Artık programımızın temel yapısı yazıldığına göre, parametrelerimize göre adlandırılan işlevleri oluşturmalıyız.

## <a name="list-cdn-profiles-and-endpoints"></a>CDN profillerini ve uç noktalarını listele
Mevcut profillerimizi ve uç noktalarımızı listelemek için kodla başlayalım.  Kod açıklamalarım beklenen sözdizimini sağlar, böylece her parametrenin nereye gittiğini biliriz.

```javascript
// list profiles
// list endpoints <profile name>
function cdnList(){
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profiles":
            console.log("Listing profiles...");
            cdnClient.profiles.listByResourceGroup(resourceGroupName, callback);
            break;

        case "endpoints":
            requireParms(3);
            console.log("Listing endpoints...");
            cdnClient.endpoints.listByProfile(parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>CDN profilleri ve uç noktalar oluşturma
Ardından, profiller ve uç noktalar oluşturmak için işlevleri yazacağız.

```javascript
function cdnCreate() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        case "profile":
            cdnCreateProfile();
            break;

        case "endpoint":
            cdnCreateEndpoint();
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}

// create profile <profile name>
function cdnCreateProfile() {
    requireParms(3);
    console.log("Creating profile...");
    var standardCreateParameters = {
        location: resourceLocation,
        sku: {
            name: 'Standard_Verizon'
        }
    };

    cdnClient.profiles.create(parms[2], standardCreateParameters, resourceGroupName, callback);
}

// create endpoint <profile name> <endpoint name> <origin hostname>        
function cdnCreateEndpoint() {
    requireParms(5);
    console.log("Creating endpoint...");
    var endpointProperties = {
        location: resourceLocation,
        origins: [{
            name: parms[4],
            hostName: parms[4]
        }]
    };

    cdnClient.endpoints.create(parms[3], endpointProperties, parms[2], resourceGroupName, callback);
}
```

## <a name="purge-an-endpoint"></a>Bir bitiş noktasını temizleme
Bitiş noktasının oluşturulduğunu varsayarsak, programımızda gerçekleştirmek isteyebileceğimiz ortak bir görev, bitiş noktamızdaki içeriği temizlemektir.

```javascript
// purge <profile name> <endpoint name> <path>
function cdnPurge() {
    requireParms(4);
    console.log("Purging endpoint...");
    var purgeContentPaths = [ parms[3] ];
    cdnClient.endpoints.purgeContent(parms[2], parms[1], resourceGroupName, purgeContentPaths, callback);
}
```

## <a name="delete-cdn-profiles-and-endpoints"></a>CDN profillerini ve uç noktalarını silme
Ekleyeceğimiz son işlev uç noktaları ve profilleri siler.

```javascript
function cdnDelete() {
    requireParms(2);
    switch(parms[1].toLowerCase())
    {
        // delete profile <profile name>
        case "profile":
            requireParms(3);
            console.log("Deleting profile...");
            cdnClient.profiles.deleteIfExists(parms[2], resourceGroupName, callback);
            break;

        // delete endpoint <profile name> <endpoint name>
        case "endpoint":
            requireParms(4);
            console.log("Deleting endpoint...");
            cdnClient.endpoints.deleteIfExists(parms[3], parms[2], resourceGroupName, callback);
            break;

        default:
            console.log("Invalid parameter.");
            process.exit(1);
    }
}
```

## <a name="running-the-program"></a>Programı çalıştırma
Artık Node.js programımızı en sevdiğimiz hata ayıklayıcımızı kullanarak veya konsolda gerçekleştirebiliriz.

> [!TIP]
> Görsel Stüdyo Kodunu hata ayıklayıcınız olarak kullanıyorsanız, komut satırı parametrelerini geçmek için ortamınızı ayarlamanız gerekir.  Visual Studio Code **bunu launch.json** dosyasında yapar.  **Args** adlı bir özellik arayın ve parametreleriniz için dize değerleri bir `"args": ["list", "profiles"]`dizi ekleyin, böylece buna benzer görünüyor: .
> 
> 

Profillerimizi listeleyerek başlayalım.

![Liste profilleri](./media/cdn-app-dev-node/cdn-list-profiles.png)

Boş bir dizi geri aldık.  Kaynak grubumuzda herhangi bir profil olmadığından, bu beklenen bir şey.  Şimdi bir profil oluşturalım.

![Profil oluşturma](./media/cdn-app-dev-node/cdn-create-profile.png)

Şimdi bir son nokta ekleyelim.

![Uç nokta oluşturma](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Son olarak, profilimizi silelim.

![Profili sil](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Sonraki Adımlar
Tamamlanan projeyi bu gözden geçirme den görmek [için, örneği indirin.](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74)

Node.js için Azure CDN SDK başvurugörmek [için, başvuru](https://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/)görüntüleyin.

Node.js için Azure SDK'da ek belgeler bulmak için [başvurunun tamamını](https://azure.github.io/azure-sdk-for-node/)görüntüleyin.

[PowerShell](cdn-manage-powershell.md)ile CDN kaynaklarınızı yönetin.


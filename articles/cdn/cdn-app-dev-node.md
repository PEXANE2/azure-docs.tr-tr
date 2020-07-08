---
title: Node.js için Azure CDN SDK ile çalışmaya başlama | Microsoft Docs
description: Azure CDN yönetmek için Node.js uygulamaları yazmayı öğrenin.
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
ms.topic: how-to
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: c83323ff7b951892b96d49f3173c022592aac331
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85920171"
---
# <a name="get-started-with-azure-cdn-development"></a>Azure CDN ile geliştirmeye başlama
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

CDN profillerinin ve uç noktaların oluşturulmasını ve yönetimini otomatikleştirmek için [Node.jsiçin Azure CDN SDK 'sını](https://www.npmjs.com/package/azure-arm-cdn) kullanabilirsiniz.  Bu öğreticide, kullanılabilir işlemlerin birkaçını gösteren basit bir Node.js konsol uygulamasının oluşturulması gösterilmektedir.  Bu öğretici, Node.js için Azure CDN SDK 'sının tüm yönlerini ayrıntılı olarak tanımlamaya yönelik değildir.

Bu öğreticiyi tamamlayabilmeniz için, zaten [Node.js](https://www.nodejs.org) **4. x. x** veya üzeri yüklü ve yapılandırılmış olmalıdır.  Node.js uygulamanızı oluşturmak istediğiniz herhangi bir metin düzenleyicisini kullanabilirsiniz.  Bu öğreticiyi yazmak için [Visual Studio Code](https://code.visualstudio.com)kullandım.  

> [!TIP]
> [Bu öğreticiden tamamlanmış proje](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74) MSDN 'de indirilebilir.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-npm-dependencies"></a>Projenizi oluşturun ve NPM bağımlılıklarını ekleyin
CDN profillerimiz için bir kaynak grubu oluşturduğumuzdan ve bu grup içindeki CDN profillerini ve uç noktalarını yönetmek için Azure AD uygulama iznimize verildik. uygulamamızı oluşturmaya başlayabiliriz.

Uygulamanızı depolamak için bir klasör oluşturun.  Geçerli yolunuzda Node.js araçları olan bir konsolundan, geçerli konumunuzu bu yeni klasöre ayarlayın ve şunu yürüterek projenizi başlatın:

```console
npm init
```

Daha sonra projenizi başlatmak için bir dizi soru sunulacaktır.  **Giriş noktası**için bu öğretici *app.js*kullanır.  Aşağıdaki örnekte diğer seçimlerimi görebilirsiniz.

![NPM init çıkışı](./media/cdn-app-dev-node/cdn-npm-init.png)

Projemiz artık *packages.js* dosyası ile başlatılmış.  Projemiz NPM paketlerinde bulunan bazı Azure kitaplıklarını kullanacak.  Node.js (MS-Rest-Azure) için Azure Istemci çalışma zamanını ve Node.js için Azure CDN Istemci kitaplığını (Azure-ARM-CD) kullanacağız.  Projeye bağımlılıklar olarak ekleyelim.

```console
npm install --save ms-rest-azure
npm install --save azure-arm-cdn
```

Paketler yüklendikten sonra, dosyadaki *package.js* bu örneğe benzer görünmelidir (sürüm numaraları farklı olabilir):

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

Son olarak, metin düzenleyicinizi kullanarak boş bir metin dosyası oluşturun ve proje klasörünüzün köküne *app.js*olarak kaydedin.  Artık kod yazmaya başlamaya hazırsınız.

## <a name="requires-constants-authentication-and-structure"></a>, Sabitler, kimlik doğrulaması ve yapı gerektirir
Düzenleyicimizde *app.js* açık olduğunda programımızın temel yapısını yazalım.

1. En üstteki NPM paketlerimiz için "gerektirir" öğesini aşağıdaki şekilde ekleyin:
   
    ``` javascript
    var msRestAzure = require('ms-rest-azure');
    var cdnManagementClient = require('azure-arm-cdn');
    ```
2. Yöntemlerimizin kullanacağı bazı sabitleri tanımlamanız gerekiyor.  Aşağıdakileri ekleyin.  ** &lt; Açılı ayraçlar &gt; **dahil olmak üzere yer tutucuları, gerektiğinde kendi değerlerinizle değiştirdiğinizden emin olun.
   
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
3. Ardından, CDN yönetim istemcisini örnekliyoruz ve kimlik bilgilerinizle bize sunacağız.
   
    ``` javascript
    var credentials = new msRestAzure.ApplicationTokenCredentials(clientId, tenantId, clientSecret);
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    Bireysel kullanıcı kimlik doğrulaması kullanıyorsanız, bu iki satır biraz farklı görünecektir.
   
   > [!IMPORTANT]
   > Bu kod örneğini yalnızca bir hizmet sorumlusu yerine bireysel kullanıcı kimlik doğrulamasının olmasını tercih ediyorsanız kullanın.  Bireysel kullanıcı kimlik bilgilerinizi korumamaya ve gizli tutmaya dikkat edin.
   > 
   > 
   
    ``` javascript
    var credentials = new msRestAzure.UserTokenCredentials(clientId, 
        tenantId, '<username>', '<password>', '<redirect URI>');
    var cdnClient = new cdnManagementClient(credentials, subscriptionId);
    ```
   
    ** &lt; Açılı ayraçlar &gt; ** içindeki öğeleri doğru bilgilerle değiştirdiğinizden emin olun.  İçin `<redirect URI>` , uygulamayı Azure AD 'ye kaydettiğinizde girdiğiniz yeniden YÖNLENDIRME URI 'sini kullanın.
4. Node.js konsolu uygulamamız bazı komut satırı parametreleri alacak.  En az bir parametrenin geçtiğini doğrulayalim.
   
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
5. Bu, bize, hangi parametrelerin geçtiğini temel alarak diğer işlevlere daldığımız, programımızın ana kısmına getiriyor.
   
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
6. Programımız birkaç yerde, doğru şekilde parametre sayısının doğru şekilde geçirildiğinden emin olmamız gerekir.  Bunu yapmak için işlevler oluşturalım.
   
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
7. Son olarak, CDN yönetim istemcisinde kullanacağımız işlevler zaman uyumsuzdur ve bu nedenle, bittiğinde geri çağrı yapmak için bir yönteme ihtiyaç duyar.  CDN yönetim istemcisinden (varsa) çıktıyı görüntüleyebilen bir tane oluşturalım ve programdan sorunsuz bir şekilde çıkış yapalim.
   
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

Artık programımızın temel yapısı yazıldığı için, parametrelerimize göre adlı işlevleri oluşturacağız.

## <a name="list-cdn-profiles-and-endpoints"></a>CDN profillerini ve uç noktalarını listeleme
Mevcut profillerimizi ve uç noktalarınızı listelemek için kod ile başlayalım.  Her parametrenin nerede olduğunu bilmemiz için kod açıklamalarım beklenen sözdizimini sağlar.

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

## <a name="create-cdn-profiles-and-endpoints"></a>CDN profilleri ve uç noktaları oluşturma
Daha sonra, profil ve uç noktalar oluşturmak için işlevleri yazacağız.

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

## <a name="purge-an-endpoint"></a>Bir uç noktayı Temizleme
Uç noktanın oluşturulduğu varsayılarak, programımızda gerçekleştirmek isteyebileceğiniz ortak bir görev, uç noktanızda içerik temizliyoruz.

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
En son işlev, uç noktaları ve profilleri siler.

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
Artık sık kullanılan hata ayıklayıcımız veya konsolda Node.js programımızı yürütebiliriz.

> [!TIP]
> Hata ayıklayıcı olarak Visual Studio Code kullanıyorsanız, ortamınızı komut satırı parametrelerine geçirilecek şekilde ayarlamanız gerekir.  Visual Studio Code bunu dosyada **launch.js** .  **Bağımsız değişkenler** adlı bir özellik bulun ve parametreleriniz için bir dize değerleri dizisi ekleyin, böylece şuna benzer olur: `"args": ["list", "profiles"]` .
> 
> 

Profillerimizi listeleyerek başlayalım.

![Liste profilleri](./media/cdn-app-dev-node/cdn-list-profiles.png)

Boş bir diziyi geri aldık.  Kaynak grubunuzda hiç bir profil olmadığı için bu beklenmez.  Şimdi bir profil oluşturalım.

![Profil oluştur](./media/cdn-app-dev-node/cdn-create-profile.png)

Şimdi bir uç nokta ekleyelim.

![Uç nokta oluştur](./media/cdn-app-dev-node/cdn-create-endpoint.png)

Son olarak, profilimi silemem.

![Profili sil](./media/cdn-app-dev-node/cdn-delete-profile.png)

## <a name="next-steps"></a>Sonraki Adımlar
Bu izlenecek yolda tamamlanmış projeyi görmek için [örneği indirin](https://code.msdn.microsoft.com/Azure-CDN-SDK-for-Nodejs-c712bc74).

Node.js için Azure CDN SDK başvurusunu görmek için [başvuruyu](https://azure.github.io/azure-sdk-for-node/azure-arm-cdn/latest/)görüntüleyin.

Node.js için Azure SDK hakkında ek belgeler bulmak için, [tam başvuruyu](https://azure.github.io/azure-sdk-for-node/)görüntüleyin.

CDN kaynaklarınızı [PowerShell](cdn-manage-powershell.md)ile yönetin.


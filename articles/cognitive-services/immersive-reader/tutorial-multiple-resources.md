---
title: 'Öğretici: Birden çok Sürükleyici Okuyucu kaynaklarını tümleştir'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, birden çok Sürükleyici Okuyucu kaynaklarını kullanarak Sürükleyici Okuyucu'yu başlatan bir Düğüm.js uygulaması oluşturursunuz.
author: skamal
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 01/14/2020
ms.author: skamal
ms.openlocfilehash: 3912d55b13f3977818e8d898efa651ffeb1a798a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76046278"
---
# <a name="tutorial-integrate-multiple-immersive-reader-resources"></a>Öğretici: Birden çok Sürükleyici Okuyucu kaynaklarını tümleştir

Genel [bakışta,](./overview.md)Sürükleyici Okuyucu'nun ne olduğunu ve dil öğrenenler, yeni ortaya çıkan okuyucular ve öğrenme farklılıkları olan öğrenciler için okuma anlamayı geliştirmek için kanıtlanmış teknikleri nasıl uyguladığını öğrendiniz. [Düğüm.js quickstart,](./quickstart-nodejs.md)tek bir kaynak ile Immersive Reader nasıl kullanılacağını öğrendim. Bu öğretici, aynı uygulamada birden çok Sürükleyici Reader kaynağının nasıl tümleştirilenini kapsar. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Varolan bir kaynak grubu altında birden çok Sürükleyici Okuyucu kaynağı oluşturma
> * Birden çok kaynak kullanarak Sürükleyici Okuyucuyu başlatın

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* NodeJS ile Sürükleyici Okuyucu'yu başlatan bir web uygulaması oluşturmak için [hızlı başlatışları](./quickstart-nodejs.md) izleyin. Bu hızlı başlangıçta, tek bir Sürükleyici Okuyucu kaynağını yapılandırırsınız. Biz bu öğretici bunun üzerine inşa edecektir.

## <a name="create-the-immersive-reader-resources"></a>Sürükleyici Okuyucu kaynaklarını oluşturma

Her Sürükleyici Reader kaynağı oluşturmak için [bu yönergeleri](./how-to-create-immersive-reader.md) izleyin. **Create-ImmersiveReaderResource** komut `ResourceName`dosyası `ResourceSubdomain`, `ResourceLocation` ve parametreler olarak. Bunlar, oluşturulan her kaynak için benzersiz olmalıdır. Kalan parametreler, ilk Sürükleyici Okuyucu kaynağınızı ayarlarken kullandığınız parayla aynı olmalıdır. Bu şekilde, her kaynak aynı Azure kaynak grubuna ve Azure AD uygulamasına bağlanabilir.

Aşağıdaki örnek, biri WestUS'ta, diğeri EastUS'ta olmak üzere iki kaynağın nasıl oluşturulabildiğini göstermektedir. için benzersiz değerlere dikkat `ResourceLocation`edin , ve . `ResourceSubdomain` `ResourceName`

```azurepowershell-interactive
Create-ImmersiveReaderResource
  -SubscriptionName <SUBSCRIPTION_NAME> `
  -ResourceName Resource_name_wus `
  -ResourceSubdomain resource-subdomain-wus `
  -ResourceSKU <RESOURCE_SKU> `
  -ResourceLocation westus `
  -ResourceGroupName <RESOURCE_GROUP_NAME> `
  -ResourceGroupLocation <RESOURCE_GROUP_LOCATION> `
  -AADAppDisplayName <AAD_APP_DISPLAY_NAME> `
  -AADAppIdentifierUri <AAD_APP_IDENTIFIER_URI> `
  -AADAppClientSecret <AAD_APP_CLIENT_SECRET>

Create-ImmersiveReaderResource
  -SubscriptionName <SUBSCRIPTION_NAME> `
  -ResourceName Resource_name_eus `
  -ResourceSubdomain resource-subdomain-eus `
  -ResourceSKU <RESOURCE_SKU> `
  -ResourceLocation eastus `
  -ResourceGroupName <RESOURCE_GROUP_NAME> `
  -ResourceGroupLocation <RESOURCE_GROUP_LOCATION> `
  -AADAppDisplayName <AAD_APP_DISPLAY_NAME> `
  -AADAppIdentifierUri <AAD_APP_IDENTIFIER_URI> `
  -AADAppClientSecret <AAD_APP_CLIENT_SECRET>
```

## <a name="add-resources-to-environment-configuration"></a>Ortam yapılandırmasına kaynak ekleme

`TenantId`Hızlı başlatmada, , , `ClientId` `ClientSecret`ve `Subdomain` parametreleri içeren bir ortam yapılandırma dosyası oluşturdunuz. Tüm kaynaklarınız aynı Azure AD uygulamasını kullandığından, `TenantId`aynı değerleri `ClientId`, `ClientSecret`, ve . Yapılması gereken tek değişiklik, her kaynak için her alt etki alanını listelemektir.

Yeni __.env__ dosyanız artık aşağıdaki gibi görünmelidir:

```text
TENANT_ID={YOUR_TENANT_ID}
CLIENT_ID={YOUR_CLIENT_ID}
CLIENT_SECRET={YOUR_CLIENT_SECRET}
SUBDOMAIN_WUS={YOUR_WESTUS_SUBDOMAIN}
SUBDOMAIN_EUS={YOUR_EASTUS_SUBDOMAIN}
```

Bu dosyayı kaynak denetimine işlemeyeceğinden emin olun, çünkü kamuya açıklanmaması gereken sırlar içerir.

Daha sonra, birden çok kaynağımızı desteklemek için oluşturduğumuz _routes\index.js_ dosyasını değiştireceğiz. İçeriğini aşağıdaki kodla değiştirin.

Daha önce olduğu gibi, bu kod, hizmet temel parolanızı kullanarak bir Azure AD kimlik doğrulama belirteci alan bir API bitiş noktası oluşturur. Bu kez, kullanıcının bir kaynak konumu belirtmesine ve sorgu parametresi olarak geçirmesine olanak tanır. Daha sonra belirteç ve ilgili alt etki alanı içeren bir nesne döndürür.

```javascript
var express = require('express');
var router = express.Router();
var request = require('request');

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

router.get('/GetTokenAndSubdomain', function(req, res) {
    try {
        request.post({
            headers: {
                'content-type': 'application/x-www-form-urlencoded'
            },
            url: `https://login.windows.net/${process.env.TENANT_ID}/oauth2/token`,
            form: {
                grant_type: 'client_credentials',
                client_id: process.env.CLIENT_ID,
                client_secret: process.env.CLIENT_SECRET,
                resource: 'https://cognitiveservices.azure.com/'
            }
        },
        function(err, resp, tokenResult) {
            if (err) {
                console.log(err);
                return res.status(500).send('CogSvcs IssueToken error');
            }

            var tokenResultParsed = JSON.parse(tokenResult);

            if (tokenResultParsed.error) {
                console.log(tokenResult);
                return res.send({error :  "Unable to acquire Azure AD token. Check the debugger for more information."})
            }

            var token = tokenResultParsed.access_token;

            var subdomain = "";
            var region = req.query && req.query.region;
            switch (region) {
                case "eus":
                    subdomain = process.env.SUBDOMAIN_EUS
                    break;
                case "wus":
                default:
                    subdomain = process.env.SUBDOMAIN_WUS
            }

            return res.send({token, subdomain});
        });
    } catch (err) {
        console.log(err);
        return res.status(500).send('CogSvcs IssueToken error');
    }
});

module.exports = router;
```

**Getimmersivereaderlaunchparams** API bitiş noktası, yetkisiz kullanıcıların Sürükleyici Reader hizmetinize ve faturanıza karşı kullanılacak jetonları almasını önlemek için bir tür kimlik doğrulamanın (örneğin, [OAuth)](https://oauth.net/2/)arkasına sabitlenmelidir; bu çalışma bu öğretici kapsamı dışındadır.

## <a name="launch-the-immersive-reader-with-sample-content"></a>Örnek içerikle Sürükleyici Okuyucuyu başlatın

1. _Görünümler\index.pug'u_açın ve içeriğini aşağıdaki kodla değiştirin. Bu kod, sayfayı bazı örnek içerikle doldurur ve Sürükleyici Okuyucu'yu başlatan iki düğme ekler. Biri EastUS kaynağı için Sürükleyici Reader başlatmak için, diğeri de WestUS kaynağı için.

    ```pug
    doctype html
    html
        head
            title Immersive Reader Quickstart Node.js

            link(rel='stylesheet', href='https://stackpath.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css')

            // A polyfill for Promise is needed for IE11 support.
            script(src='https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js')

            script(src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.1.0.0.js')
            script(src='https://code.jquery.com/jquery-3.3.1.min.js')

            style(type="text/css").
                .immersive-reader-button {
                background-color: white;
                margin-top: 5px;
                border: 1px solid black;
                float: right;
                }
        body
            div(class="container")
                button(class="immersive-reader-button" data-button-style="icon" data-locale="en" onclick='handleLaunchImmersiveReader("wus")') WestUS Immersive Reader
                button(class="immersive-reader-button" data-button-style="icon" data-locale="en" onclick='handleLaunchImmersiveReader("eus")') EastUS Immersive Reader

                h1(id="ir-title") About Immersive Reader
                div(id="ir-content" lang="en-us")
                p Immersive Reader is a tool that implements proven techniques to improve reading comprehension for emerging readers, language learners, and people with learning differences. The Immersive Reader is designed to make reading more accessible for everyone. The Immersive Reader

                    ul
                        li Shows content in a minimal reading view
                        li Displays pictures of commonly used words
                        li Highlights nouns, verbs, adjectives, and adverbs
                        li Reads your content out loud to you
                        li Translates your content into another language
                        li Breaks down words into syllables

                h3 The Immersive Reader is available in many languages.

                p(lang="es-es") El Lector inmersivo está disponible en varios idiomas.
                p(lang="zh-cn") 沉浸式阅读器支持许多语言
                p(lang="de-de") Der plastische Reader ist in vielen Sprachen verfügbar.
                p(lang="ar-eg" dir="rtl" style="text-align:right") يتوفر \"القارئ الشامل\" في العديد من اللغات.

    script(type="text/javascript").
    function getTokenAndSubdomainAsync(region) {
            return new Promise(function (resolve, reject) {
                $.ajax({
                    url: "/GetTokenAndSubdomain",
                    type: "GET",
                    data: {
                        region: region
                    },
                    success: function (data) {
                        if (data.error) {
                            reject(data.error);
                        } else {
                            resolve(data);
                        }
                    },
                    error: function (err) {
                        reject(err);
                    }
                });
            });
        }

        function handleLaunchImmersiveReader(region) {
            getTokenAndSubdomainAsync(region)
                .then(function (response) {
                    const token = response["token"];
                    const subdomain = response["subdomain"];
                    // Learn more about chunk usage and supported MIME types https://docs.microsoft.com/azure/cognitive-services/immersive-reader/reference#chunk
                    const data = {
                        title: $("#ir-title").text(),
                        chunks: [{
                            content: $("#ir-content").html(),
                            mimeType: "text/html"
                        }]
                    };
                    // Learn more about options https://docs.microsoft.com/azure/cognitive-services/immersive-reader/reference#options
                    const options = {
                        "onExit": exitCallback,
                        "uiZIndex": 2000
                    };
                    ImmersiveReader.launchAsync(token, subdomain, data, options)
                        .catch(function (error) {
                            alert("Error in launching the Immersive Reader. Check the console.");
                            console.log(error);
                        });
                })
                .catch(function (error) {
                    alert("Error in getting the Immersive Reader token and subdomain. Check the console.");
                    console.log(error);
                });
        }

        function exitCallback() {
            console.log("This is the callback function. It is executed when the Immersive Reader closes.");
        }
    ```

3. Web uygulamamız artık hazır. Çalıştırarak uygulamayı başlatın:

    ```bash
    npm start
    ```

4. Tarayıcınızı açın ve [http://localhost:3000](http://localhost:3000)' ye gidin. Yukarıdaki içeriği sayfada görmelisiniz. Bu kaynakları kullanarak Sürükleyici Okuyucu'yu başlatmak için **EastUS Immersive Reader** düğmesine veya **WestUS Immersive Reader** düğmesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Immersive Reader SDK](https://github.com/microsoft/immersive-reader-sdk) ve [Immersive Reader SDK Referans](./reference.md) keşfedin
* Kod örneklerini [GitHub'da](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/advanced-csharp) görüntüleme
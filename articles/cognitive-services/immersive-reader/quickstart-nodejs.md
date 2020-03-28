---
title: 'Quickstart: Node.js ile Sürükleyici Okuyucu başlattı bir web uygulaması oluşturun'
titleSuffix: Azure Cognitive Services
description: Bu hızlı başlangıçta, sıfırdan bir web uygulaması oluşturur ve Sürükleyici Reader API işlevselliğini eklersiniz.
author: pasta
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: quickstart
ms.date: 01/14/2020
ms.author: pasta
ms.openlocfilehash: 749e75fed409632c613713a49154e4cd8dc265b3
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75946330"
---
# <a name="quickstart-create-a-web-app-that-launches-the-immersive-reader-nodejs"></a>Quickstart: Sürükleyici Okuyucu (Node.js) başlattı bir web uygulaması oluşturun

[Immersive Reader,](https://www.onenote.com/learningtools) okuma anlama geliştirmek için kanıtlanmış teknikleri uygulayan kapsayıcı bir şekilde tasarlanmış bir araçtır.

Bu hızlı başlangıçta, sıfırdan bir web uygulaması oluşturun ve Sürükleyici Reader SDK'yı kullanarak Sürükleyici Okuyucu'yu entegre edebilirsiniz. Bu quickstart tam bir çalışma örneği [burada](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/quickstart-nodejs)mevcuttur.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Azure Etkin Dizin kimlik doğrulaması için yapılandırılan Sürükleyici Bir Reader kaynağı. Kurulumu için [bu yönergeleri](./how-to-create-immersive-reader.md) izleyin. Ortam özelliklerini yapılandırırken burada oluşturulan bazı değerlere ihtiyacınız olacaktır. Oturumunuzun çıktısını ileride başvurmak için bir metin dosyasına kaydedin.
* [Düğüm.js](https://nodejs.org/) ve [İplik](https://yarnpkg.com)
* [Visual Studio Code](https://code.visualstudio.com/) gibi bir IDE

## <a name="create-a-nodejs-web-app-with-express"></a>Express ile Bir Düğüm.js web uygulaması oluşturma

`express-generator` Araçla bir Düğüm.js web uygulaması oluşturun.

```bash
npm install express-generator -g
express --view=pug quickstart-nodejs
cd quickstart-nodejs
```

Iplik bağımlılıklarını yükleyin ve `request` bağımlılıkları ekleyin ve `dotenv`daha sonra hızlı başlatmada kullanılacaktır.

```bash
yarn
yarn add request
yarn add dotenv
```

## <a name="set-up-authentication"></a>Kimlik doğrulamasını ayarlama

### <a name="configure-authentication-values"></a>Kimlik doğrulama değerlerini yapılandırma

Projenizin kökünde _.env_ adlı yeni bir dosya oluşturun. Immersive Reader kaynağınızı oluşturduğunuzda verilen değerleri sağlayarak aşağıdaki kodu yapıştırın.
Tırnak işaretlerini veya "{" ve "}" karakterlerini eklemeyin.

```text
TENANT_ID={YOUR_TENANT_ID}
CLIENT_ID={YOUR_CLIENT_ID}
CLIENT_SECRET={YOUR_CLIENT_SECRET}
SUBDOMAIN={YOUR_SUBDOMAIN}
```

Bu dosyayı kaynak denetimine işlemeyeceğinden emin olun, çünkü kamuya açıklanmaması gereken sırlar içerir.

Ardından _app.js'yi_ açın ve aşağıdakileri dosyanın üst bölümüne ekleyin. Bu, .env dosyasında tanımlanan özellikleri ortam değişkenleri olarak Düğüme yükler.

```javascript
require('dotenv').config();
```

### <a name="update-the-router-to-acquire-the-token"></a>Belirteci edinmek için yönlendiriciyi güncelleştirme
_Routes\index.js_ dosyasını açın ve otomatik olarak oluşturulan kodu aşağıdaki kodla değiştirin.

Bu kod, hizmet temel parolanızı kullanarak bir Azure AD kimlik doğrulama belirteci alan bir API bitiş noktası oluşturur. Ayrıca alt etki alanını da alır. Daha sonra belirteç ve alt etki alanı içeren bir nesne döndürür.

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
            var subdomain = process.env.SUBDOMAIN;
            return res.send({token, subdomain});
        });
    } catch (err) {
        console.log(err);
        return res.status(500).send('CogSvcs IssueToken error');
    }
});

module.exports = router;
```

**GetTokenAndSubdomain** API bitiş noktası, yetkisiz kullanıcıların Sürükleyici Reader hizmetinize ve faturanıza karşı kullanılacak jeton almalarını önlemek için bir tür kimlik doğrulamanın (örneğin, [OAuth)](https://oauth.net/2/)arkasına sabitlenmelidir; bu iş bu hızlı başlatma kapsamı dışındadır.

## <a name="add-sample-content"></a>Örnek içerik ekleme

Şimdi, bu web uygulamasına örnek içerik ekleyeceğiz. _Görünümler\index.pug'u_ açın ve otomatik olarak oluşturulan kodu bu örnekle değiştirin:

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
        button(class="immersive-reader-button" data-button-style="iconAndText" data-locale="en")

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
  function getTokenAndSubdomainAsync() {
        return new Promise(function (resolve, reject) {
            $.ajax({
                url: "/GetTokenAndSubdomain",
                type: "GET",
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

    $(".immersive-reader-button").click(function () {
        handleLaunchImmersiveReader();
    });

    function handleLaunchImmersiveReader() {
        getTokenAndSubdomainAsync()
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


Metnin tümünün, metnin dillerini açıklayan bir **lang** özniteliği ne sahip olduğuna dikkat edin. Bu öznitelik, Immersive Reader'ın ilgili dil ve dilbilgisi özellikleri sağlamasına yardımcı olur.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

Web uygulamamız artık hazır. Çalıştırarak uygulamayı başlatın:

```bash
npm start
```

Tarayıcınızı açın ve _http://localhost:3000_' ye gidin. Şunları görmeniz gerekir:

![Örnek uygulama](./media/quickstart-nodejs/1-buildapp.png)

## <a name="launch-the-immersive-reader"></a>Sürükleyici Okuyucuyu Başlat

"Sürükleyici Okuyucu" düğmesine tıkladığınızda, sayfadaki içerikle başlatılan Sürükleyici Okuyucuyu görürsünüz.

![Tam Ekran Okuyucu](./media/quickstart-nodejs/2-viewimmersivereader.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Immersive Reader SDK](https://github.com/microsoft/immersive-reader-sdk) ve [Immersive Reader SDK Referans](./reference.md) keşfedin
---
title: 'Öğretici: Node.js kullanarak Immersive Reader başlatın'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, Sürükleyici Okuyucu'yu başlatan bir Düğüm.js uygulaması oluşturursunuz.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 01/14/2020
ms.author: metan
ms.openlocfilehash: 139dd2ebdabbc91a6de3b0a1eb921b110d47c3f3
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76842036"
---
# <a name="tutorial-launch-the-immersive-reader-nodejs"></a>Öğretici: Immersive Reader (Node.js) başlatın

Genel [bakışta,](./overview.md)Sürükleyici Okuyucu'nun ne olduğunu ve dil öğrenenler, yeni ortaya çıkan okuyucular ve öğrenme farklılıkları olan öğrenciler için okuma anlamayı geliştirmek için kanıtlanmış teknikleri nasıl uyguladığını öğrendiniz. Bu öğretici, Sürükleyici Okuyucu başlattı bir Düğüm.js web uygulaması oluşturmak için nasıl kapsar. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Express ile Bir Düğüm.js web uygulaması oluşturma
> * Erişim belirteci alma
> * Örnek içerikle Sürükleyici Okuyucuyu başlatın
> * İçeriğinizin dilini belirtin
> * Immersive Reader arabiriminin dilini belirtin
> * Matematik içeriği yle Sürükleyici Okuyucuyu başlatın

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Azure Etkin Dizin kimlik doğrulaması için yapılandırılan Sürükleyici Bir Reader kaynağı. Kurulumu için [bu yönergeleri](./how-to-create-immersive-reader.md) izleyin. Ortam özelliklerini yapılandırırken burada oluşturulan bazı değerlere ihtiyacınız olacaktır. Oturumunuzun çıktısını ileride başvurmak için bir metin dosyasına kaydedin.
* [Düğüm.js](https://nodejs.org/) ve [İplik](https://yarnpkg.com)
* [Visual Studio Code](https://code.visualstudio.com/) gibi bir IDE

## <a name="create-a-nodejs-web-app-with-express"></a>Express ile Bir Düğüm.js web uygulaması oluşturma

`express-generator` Araçla bir Düğüm.js web uygulaması oluşturun.

```bash
npm install express-generator -g
express --view=pug myapp
cd myapp
```

İplik bağımlılıklarını yükleyin ve `request` `dotenv`daha sonra öğreticide kullanılacak bağımlılıkları ekleyin.

```bash
yarn
yarn add request
yarn add dotenv
```

## <a name="acquire-an-azure-ad-authentication-token"></a>Azure AD kimlik doğrulama belirteci edinme

Ardından, Azure AD kimlik doğrulama belirteci almak için bir arka uç API yazın.

Bu bölüm için yukarıdaki Azure AD auth yapılandırma ön koşul adımından bazı değerlere ihtiyacınız var. O oturumdan kaydettiğiniz metin dosyasına geri bakın.

````text
TenantId     => Azure subscription TenantId
ClientId     => Azure AD ApplicationId
ClientSecret => Azure AD Application Service Principal password
Subdomain    => Immersive Reader resource subdomain (resource 'Name' if the resource was created in the Azure portal, or 'CustomSubDomain' option if the resource was created with Azure CLI Powershell. Check the Azure portal for the subdomain on the Endpoint in the resource Overview page, for example, 'https://[SUBDOMAIN].cognitiveservices.azure.com/')
````

Bu değerlere sahip olduktan sonra _,.env_adlı yeni bir dosya oluşturun ve aşağıdaki kodu üstten özel özellik değerlerinizi sağlayarak içine yapıştırın. Tırnak işaretlerini veya "{" ve "}" karakterlerini eklemeyin.

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

_Routes\index.js_ dosyasını açın ve içeriğini aşağıdaki kodla değiştirin.

Bu kod, hizmet temel parolanızı kullanarak bir Azure AD kimlik doğrulama belirteci alan bir API bitiş noktası oluşturur. Ayrıca alt etki alanını da alır. Daha sonra belirteç ve alt etki alanı içeren bir nesne döndürür.

```javascript
var request = require('request');
var express = require('express');
var router = express.Router();

router.get('/getimmersivereaderlaunchparams', function(req, res) {
    request.post ({
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
        function(err, resp, tokenResponse) {
                if (err) {
                    return res.status(500).send('CogSvcs IssueToken error');
                }

                const token = JSON.parse(tokenResponse).access_token;
                const subdomain = process.env.SUBDOMAIN;
                return res.send({token: token, subdomain: subdomain});
        }
  );
});

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

module.exports = router;

```

**Getimmersivereaderlaunchparams** API bitiş noktası, yetkisiz kullanıcıların Sürükleyici Reader hizmetinize ve faturanıza karşı kullanılacak jetonları almasını önlemek için bir tür kimlik doğrulamanın (örneğin, [OAuth)](https://oauth.net/2/)arkasına sabitlenmelidir; bu çalışma bu öğretici kapsamı dışındadır.

## <a name="launch-the-immersive-reader-with-sample-content"></a>Örnek içerikle Sürükleyici Okuyucuyu başlatın

1. _Görünümler\layout.pug'u_açın ve `head` `body` etiketten önce etiketin altına aşağıdaki kodu ekleyin. Bu `script` etiketler [Immersive Reader SDK](https://github.com/microsoft/immersive-reader-sdk) ve jQuery yükler.

    ```pug
    script(src='https://contentstorage.onenote.office.net/onenoteltir/immersivereadersdk/immersive-reader-sdk.0.0.2.js')
    script(src='https://code.jquery.com/jquery-3.3.1.min.js')
    ```

2. _Görünümler\index.pug'u_açın ve içeriğini aşağıdaki kodla değiştirin. Bu kod, sayfayı bazı örnek içeriklerle doldurur ve Sürükleyici Okuyucu'yu başlatan bir düğme ekler.

    ```pug
    extends layout

    block content
          h2(id='title') Geography
          p(id='content') The study of Earth's landforms is called physical geography. Landforms can be mountains and valleys. They can also be glaciers, lakes or rivers.
          div(class='immersive-reader-button' data-button-style='iconAndText' data-locale='en-US' onclick='launchImmersiveReader()')
          script.

            function getImmersiveReaderLaunchParamsAsync() {
                    return new Promise((resolve, reject) => {
                        $.ajax({
                                url: '/getimmersivereaderlaunchparams',
                                type: 'GET',
                                success: data => {
                                        resolve(data);
                                },
                                error: err => {
                                        console.log('Error in getting token and subdomain!', err);
                                        reject(err);
                                }
                        });
                    });
            }

            async function launchImmersiveReader() {
                    const content = {
                            title: document.getElementById('title').innerText,
                            chunks: [{
                                    content: document.getElementById('content').innerText + '\n\n',
                                    lang: 'en'
                            }]
                    };

                    const launchParams = await getImmersiveReaderLaunchParamsAsync();
                    const token = launchParams.token;
                    const subdomain = launchParams.subdomain;

                    ImmersiveReader.launchAsync(token, subdomain, content);
            }
    ```

3. Web uygulamamız artık hazır. Çalıştırarak uygulamayı başlatın:

    ```bash
    npm start
    ```

4. Tarayıcınızı açın ve _http://localhost:3000_' ye gidin. Yukarıdaki içeriği sayfada görmelisiniz. İçeriğinizle Birlikte Sürükleyici **Okuyucuyu** başlatmak için Sürükleyici Okuyucu düğmesini tıklatın.

## <a name="specify-the-language-of-your-content"></a>İçeriğinizin dilini belirtin

Immersive Reader birçok farklı dil için destek vardır. Aşağıdaki adımları izleyerek içeriğinizin dilini belirtebilirsiniz.

1. _Görünümler\index.pug'u_ açın ve `p(id=content)` önceki adımda eklediğiniz etiketin altına aşağıdaki kodu ekleyin. Bu kod, sayfanıza bazı içerik İspanyolca içerik ekler.

    ```pug
    p(id='content-spanish') El estudio de las formas terrestres de la Tierra se llama geografía física. Los accidentes geográficos pueden ser montañas y valles. También pueden ser glaciares, lagos o ríos.
    ```

2. JavaScript kodunda, çağrının üstündeki `ImmersiveReader.launchAsync`aşağıdakileri ekleyin. Bu kod, İspanyolca içeriğini Sürükleyici Okuyucu'ya geçirir.

    ```pug
    content.chunks.push({
      content: document.getElementById('content-spanish').innerText + '\n\n',
      lang: 'es'
    });
    ```

3. Tekrar _http://localhost:3000_ gidin. Sayfada İspanyolca metin görmelisiniz, ve **Sürükleyici Reader**tıkladığınızda , o da Immersive Reader gösterecektir.

## <a name="specify-the-language-of-the-immersive-reader-interface"></a>Immersive Reader arabiriminin dilini belirtin

Varsayılan olarak, Immersive Reader arabiriminin dili tarayıcının dil ayarlarıyla eşleşir. Ayrıca, Aşağıdaki kodla Immersive Reader arabiriminin dilini de belirtebilirsiniz.

1. _views\index.pug_olarak, aşağıdaki `ImmersiveReader.launchAsync(token, subdomain, content)` kod ile arama değiştirin.

    ```javascript
    const options = {
        uiLang: 'fr',
    }
    ImmersiveReader.launchAsync(token, subdomain, content, options);
    ```

2. 'ye _http://localhost:3000_gidin. Sürükleyici Okuyucu'yu başlattığınızda, arayüz Fransızca olarak gösterilir.

## <a name="launch-the-immersive-reader-with-math-content"></a>Matematik içeriği yle Sürükleyici Okuyucuyu başlatın

[MathML'i](https://developer.mozilla.org/en-US/docs/Web/MathML)kullanarak Matematik içeriğini Sürükleyici Okuyucu'ya ekleyebilirsiniz.

1. _Görünümleri değiştir\index.pug_ çağrının üstündeki `ImmersiveReader.launchAsync`kodu eklemek için:

    ```javascript
    const mathML = '<math xmlns="https://www.w3.org/1998/Math/MathML" display="block"> \
      <munderover> \
        <mo>∫</mo> \
        <mn>0</mn> \
        <mn>1</mn> \
      </munderover> \
      <mrow> \
        <msup> \
          <mi>x</mi> \
          <mn>2</mn> \
        </msup> \
        <mo>ⅆ</mo> \
        <mi>x</mi> \
      </mrow> \
    </math>';

    content.chunks.push({
      content: mathML,
      mimeType: 'application/mathml+xml'
    });
    ```

2. 'ye _http://localhost:3000_gidin. Sürükleyici Okuyucu'yu başlatıp en altına kaydırdığınızda matematik formüllerini görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

* [Immersive Reader SDK](https://github.com/microsoft/immersive-reader-sdk) ve [Immersive Reader SDK Referans](./reference.md) keşfedin
* Kod örneklerini [GitHub'da](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/advanced-csharp) görüntüleme

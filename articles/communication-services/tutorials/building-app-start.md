---
title: Öğretici-Azure Iletişim Hizmetleri için bir Web uygulaması hazırlama (Node.js)
titleSuffix: An Azure Communication Services tutorial
description: Azure Iletişim hizmetlerini destekleyen bir temel Web uygulaması oluşturma hakkında bilgi edinin
author: nmurav
services: azure-communication-services
ms.author: nmurav
ms.date: 01/03/2012
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: db59a9e7693190582736b9460658f629f4f1e555
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100369637"
---
# <a name="tutorial-prepare-a-web-app-for-azure-communication-services-nodejs"></a>Öğretici: Azure Iletişim Hizmetleri için bir Web uygulaması hazırlama (Node.js)

[!INCLUDE [Public Preview Notice](../includes/public-preview-include.md)]

Azure Iletişim Hizmetleri, uygulamalarınıza gerçek zamanlı iletişimler eklemenize olanak tanır. Bu öğreticide, Azure Iletişim hizmetlerini destekleyen bir Web uygulamasını ayarlamayı öğreneceksiniz. Bu, gerçek zamanlı iletişimlerle çalışmaya başlamak isteyen yeni geliştiricilere yönelik tanıtım amaçlı bir öğreticidir.

Bu öğreticinin sonunda, gerçek zamanlı iletişim çözümünüzü oluşturmaya başlamak için kullanabileceğiniz Azure Communication Services istemci kitaplıkları ile yapılandırılmış bir temel Web uygulamanız olacak.

Geri bildirim sağlamak için [Azure Communication Services GitHub](https://github.com/Azure/communication) sayfasını ziyaret edebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> * Geliştirme ortamınızı yapılandırma
> * Yerel Web sunucusu kurma
> * Azure Iletişim Hizmetleri paketlerini Web sitenize ekleyin
> * Web sitenizi Azure statik Web siteleri 'ne yayımlama

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. Ayrıntılar için bkz. [ücretsiz hesap oluşturma](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). Ücretsiz hesabın, herhangi bir hizmet birleşimini denemek için size Azure kredileri $200 sunmadığını unutmayın.
- [Visual Studio Code](https://code.visualstudio.com/): bunu yerel geliştirme ortamınızda kodu düzenlemek için kullanacağız.
- [Web paketi](https://webpack.js.org/): Bu, kodunuzu paketleyip yerel olarak barındırmak için kullanılacaktır.
- [Node.js](https://nodejs.org/en/): Bu, Azure iletişim Hizmetleri istemci kitaplıkları ve WebPack gibi bağımlılıkları yüklemek ve yönetmek için kullanılacaktır.
- Sürüm denetimini işleyecek [NVM ve NPM](https://docs.microsoft.com/windows/nodejs/setup-on-windows) .
- Visual Studio Code için [Azure Storage uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) . Uygulamanızı Azure Storage 'da yayımlamak için bu uzantı gereklidir. [Azure depolama 'da statik Web siteleri barındırma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website)
- [Azure App Service uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Uzantı, Web sitelerinin (öncekine benzer) dağıtılmasına izin verir, ancak tamamen yönetilen sürekli tümleştirmeyi ve sürekli teslimi (CI/CD) yapılandırma seçeneğiyle birlikte.
- Kendi sunucusuz uygulamalarınızı derlemek için [Azure işlev uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) . Örneğin, kimlik doğrulama uygulamanızı Azure işlevleri 'nde barındırabilirsiniz.
- Etkin bir Iletişim Hizmetleri kaynağı ve bağlantı dizesi. [Iletişim Hizmetleri kaynağı oluşturun](../quickstarts/create-communication-resource.md).
- Bir Kullanıcı erişim belirteci. Yönergeler için bkz. [erişim belirteçleri hızlı başlangıç](https://docs.microsoft.com/azure/communication-services/quickstarts/access-tokens?pivots=programming-language-javascript) veya [güvenilir hizmet öğreticisi](https://docs.microsoft.com/azure/communication-services/tutorials/trusted-service-tutorial) .


## <a name="configure-your-development-environment"></a>Geliştirme ortamınızı yapılandırma

Yerel geliştirme ortamınız şu şekilde yapılandırılır:

:::image type="content" source="./media/step-one-pic-one.png" alt-text="Geliştirici ortamı mimarisi":::


### <a name="install-nodejs-nvm-and-npm"></a>Node.js, NVM ve NPM 'yi yükler

İstemci tarafı uygulamamız için gereken çeşitli bağımlılıkları indirmek ve yüklemek üzere Node.js kullanacağız. Daha sonra Azure 'da barındırdığımız statik dosyalar oluşturmak için bu uygulamayı kullanacağız, bu sayede sunucunuzda yapılandırma konusunda endişelenmenize gerek kalmaz.

Windows geliştiricileri, düğüm, NVM ve NPM 'yi yapılandırmak için [Bu NodeJS öğreticisini](https://docs.microsoft.com/windows/nodejs/setup-on-windows) izleyebilir. 

Bu öğreticiyi, LTS 12.20.0 sürümünü kullanarak test ettik. NVM 'yi yükledikten sonra, kullanmak istediğiniz sürümü dağıtmak için aşağıdaki PowerShell komutunu kullanın:

```PowerShell
nvm list available
nvm install 12.20.0
nvm use 12.20.0
```

:::image type="content" source="./media/step-one-pic-two.png" alt-text="Node.jsdağıtmak için NVM ile çalışma ":::

### <a name="configure-visual-studio-code"></a>Visual Studio Code’u yapılandırma

[Visual Studio Code](https://code.visualstudio.com/) , [desteklenen platformlardan](https://code.visualstudio.com/docs/supporting/requirements#_platforms)birine indirebilirsiniz.

### <a name="create-a-workspace-for-your-azure-communication-services-projects"></a>Azure Iletişim Hizmetleri projeleriniz için bir çalışma alanı oluşturma

Proje dosyalarınızı depolamak için aşağıdaki gibi yeni bir klasör oluşturun: `C:\Users\Documents\ACS\CallingApp` . Visual Studio Code ' de, "dosya", "klasörü çalışma alanına Ekle" seçeneğine tıklayın ve klasörü çalışma alanınıza ekleyin.

:::image type="content" source="./media/step-one-pic-three.png" alt-text="Yeni çalışma alanı oluşturuluyor":::

Sol bölmedeki Visual Studio Code "Gezgin" e gidin ve "Başlıksız" çalışma alanında "CallingApp" klasörünüzü görürsünüz.

:::image type="content" source="./media/step-one-pic-four.png" alt-text="Gezgin":::

Çalışma alanınızın adını güncelleştirmeyi ücretsiz olarak hissetmekten çekinmeyin. "CallingApp" klasörünüze sağ tıklayıp "tümleşik terminalde aç" seçeneğini belirleyerek Node.js sürümünüzü doğrulayabilirsiniz.

:::image type="content" source="./media/step-one-pic-five.png" alt-text="Terminal açma":::

Terminalde, önceki adımda yüklü node.js sürümünü doğrulamak için aşağıdaki komutu yazın:

```JavaScript
node --version
```

:::image type="content" source="./media/step-one-pic-six.png" alt-text="Node.js sürümü doğrulanıyor":::

### <a name="install-azure-extensions-for-visual-studio-code"></a>Visual Studio Code için Azure uzantıları 'nı yükler

[Azure Storage uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) Visual Studio Marketi aracılığıyla veya Visual Studio Code (azure storage > > uzantıları görüntüleyin) ile birlikte yükler.

:::image type="content" source="./media/step-one-pic-seven.png" alt-text="Azure depolama uzantısı 1 yükleniyor":::

[Azure işlevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) ve [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) uzantılarına yönelik aynı adımları izleyin.


## <a name="set-up-a-local-webserver"></a>Yerel Web sunucusu kurma

### <a name="create-a-new-npm-package"></a>Yeni NPM paketi oluşturma

Terminalinizde, çalışma alanı klasörünüzün yolundan şunu yazın:

``` console
npm init -y
```

Bu komut yeni bir NPM paketi başlatır ve `package.json` projenizin kök klasörüne ekler.

:::image type="content" source="./media/step-one-pic-eight.png" alt-text="Paket JSON 'ı":::

NPM init komutuna ek belgeler [burada](https://docs.npmjs.com/cli/v6/commands/npm-init) bulunabilir

### <a name="install-webpack"></a>WebPack 'i yükler

[WebPack](https://webpack.js.org/) , Azure 'a dağıtabileceğiniz statik dosyalara kod paketlemenizi sağlar. Ayrıca, çağıran örnekle kullanmak üzere yapılandırdığımız bir geliştirme sunucusuna sahiptir.

Terminalinizde, Web paketini yüklemek için aşağıdakini yazın:

``` Console
npm install webpack@4.42.0 webpack-cli@3.3.11 webpack-dev-server@3.10.3 --save-dev
```

Bu öğretici, yukarıda belirtilen sürümler kullanılarak test edilmiştir. Belirtme `-dev` , paket yöneticisine bu bağımlılığın geliştirme amaçlarıyla olduğunu ve Azure 'a dağıtdığımız koda eklenmeyeceğini söyler.

`package.json`Dosyanıza "devDependencies" olarak eklenen iki yeni paket görürsünüz. Paketler `./CallingApp/node_modules/` dizine yüklenir.

:::image type="content" source="./media/step-one-pic-ten.png" alt-text="WebPack yapılandırması":::

### <a name="configure-the-development-server"></a>Geliştirme sunucusunu yapılandırma

Tarayıcınızla bir statik uygulama (dosyanız gibi `index.html` ) çalıştırıldığında `file://` protokol kullanılır. NPM modüllerinizin düzgün çalışması için, Web Pack 'i yerel bir geliştirme sunucusu olarak kullanarak HTTP protokolüne ihtiyacımız olacak.

İki yapılandırma oluşturacağız: bir geliştirme ve diğeri üretim için. Üretim için hazırlanan dosyalar, kullanılmayan boşlukları ve karakterleri kaldıracağımız anlamına gelir. Bu, gecikme süresinin küçültülmesinin veya kodun karıştırılmış olması gereken üretim senaryoları için uygundur.

`webpack-merge` [Web paketi için farklı yapılandırma dosyalarıyla](https://webpack.js.org/guides/production/) çalışmak üzere aracı kullanacağız

Geliştirme ortamıyla başlayalım. İlk olarak yüklememiz gerekir `webpack merge` . Terminalinizde aşağıdakileri çalıştırın:

```Console
npm install --save-dev webpack-merge
```

`package.json`Dosyanızda, "devDependencies" öğesine eklenen bir bağımlılığı daha görebilirsiniz.

Bir sonraki adımda, yeni bir dosya oluşturmanız `webpack.common.js` ve aşağıdaki kodu eklemeniz gerekir:

```JavaScript
const path = require('path');
module.exports ={
    entry: './app.js',
    output: {
        filename:'app.js',
        path: path.resolve(__dirname, 'dist'),
    }     
}
```

Her yapılandırma için bir tane olmak üzere iki dosya daha ekleyeceğiz:

* webpack.dev.js
* webpack.prod.js

Bir sonraki adımda, dosyayı değiştirmemiz gerekiyor `webpack.dev.js` . Aşağıdaki kodu bu dosyaya ekleyin:

```JavaScript
const { merge } = require('webpack-merge');
const common = require('./webpack.common.js');

module.exports = merge(common, {
    mode: 'development',
    devtool: 'inline-source-map',
});
```
Bu yapılandırmada, ' den ortak parametreleri içeri aktardık, `webpack.common.js` iki dosyayı birleştirip, modu "geliştirme" olarak ayarlar ve SourceMap 'i "satır içi-kaynak-eşleme" olarak yapılandırın.

Geliştirme modu, WebPack 'in dosyaları küçültmeye ve en iyi duruma getirilmiş üretim dosyalarını üretmeyeceğini söyler. WebPack modlarında ayrıntılı belgeler [burada](https://webpack.js.org/configuration/mode/)bulunabilir.

Kaynak eşleme seçenekleri [burada](https://webpack.js.org/configuration/devtool/#root)listelenmiştir. Kaynak eşlemesinin ayarlanması, tarayıcınızda hata ayıklamanızı kolaylaştırır.

:::image type="content" source="./media/step-one-pic-11.png" alt-text="WebPack yapılandırılıyor":::

Geliştirme sunucusunu çalıştırmak için adresine gidin `package.json` ve betikler altına aşağıdaki kodu ekleyin:

```JavaScript
    "build:dev": "webpack-dev-server --config webpack.dev.js"
```

Dosyanız şu şekilde görünmelidir:

```JavaScript
{
  "name": "CallingApp",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build:dev": "webpack-dev-server --config webpack.dev.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.10.3"
  }
}
```

NPM 'den kullanılabilecek komutu eklediniz. 

:::image type="content" source="./media/step-one-pic-12.png" alt-text="package.jsdeğiştirme":::

### <a name="testing-the-development-server"></a>Geliştirme sunucusunu test etme

 Visual Studio Code, projenizin altında üç dosya oluşturun:

* `index.html`
* `app.js`
* `app.css` (isteğe bağlı, uygulamanızı stillemenizi sağlar)

Bunu içine yapıştırın `index.html` :

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My first ACS application</title>
    <link rel="stylesheet" href="./app.css"/>
    <script src="./app.js" defer></script>
</head>
<body>
    <h1>Hello from ACS!</h1>
</body>
</html>
```
:::image type="content" source="./media/step-one-pic-13.png" alt-text="HTML dosyası":::

Aşağıdaki kodu `app.js` dosyasına ekleyin:

```JavaScript
alert('Hello world alert!');
console.log('Hello world console!');
```
Aşağıdaki kodu `app.css` dosyasına ekleyin:

```CSS
html {
    font-family: sans-serif;
  }
```

 :::image type="content" source="./media/step-one-pic-14.png" alt-text=" JS kodu ileApp.js dosyası":::

Bu sayfayı açtığınızda, iletinizin bir uyarıyla ve tarayıcınızın konsolundan görüntülendiğini görmeniz gerekir.

:::image type="content" source="./media/step-one-pic-15.png" alt-text="App. css dosyası":::

Geliştirme yapılandırmanızı test etmek için aşağıdaki terminal komutunu kullanın:

```Console
npm run build:dev
```

Konsol, sunucunun nerede çalıştığını gösterir. Varsayılan olarak, bu `http://localhost:8080` . Build: dev komutu, daha önce eklediğimiz komuttur `package.json` .

 :::image type="content" source="./media/step-one-pic-16.png" alt-text="Geliştirme sunucusu başlatma":::
 
 Tarayıcınızdaki adrese gidin ve önceki adımlarda yapılandırılan sayfayı ve uyarıyı görmeniz gerekir.
 
  :::image type="content" source="./media/step-one-pic-17.png" alt-text="HTML sayfası":::
  
 
Sunucu çalışırken, kodu değiştirebilirsiniz ve sunucu ve HTML sayfası otomatik olarak yeniden yüklenir. 

Sonra, `app.js` Visual Studio Code dosyasına gidin ve silin `alert('Hello world alert!');` . Dosyanızı kaydedin ve uyarının tarayıcıdan kaybolduğunu doğrulayın.

Sunucunuzu durdurmak için `Ctrl+C` terminalinizde çalıştırabilirsiniz. Sunucunuzu başlatmak için `npm run build:dev` dilediğiniz zaman yazın.

## <a name="add-the-azure-communication-services-packages"></a>Azure Iletişim Hizmetleri paketlerini ekleyin

`npm install`JavaScript için istemci kitaplığı 'Nı çağıran Azure Iletişim hizmetlerini yüklemek için komutunu kullanın.

```Console
npm install @azure/communication-common --save
npm install @azure/communication-calling --save
```

Bu eylem, Azure Iletişim Hizmetleri ortak ve çağrı paketlerini paketinizin bağımlılıkları olarak ekleyecek. Dosyaya iki yeni paket eklendiğini göreceksiniz `package.json` . Komut hakkında daha fazla bilgiyi `npm install` [burada](https://docs.npmjs.com/cli/v6/commands/npm-install)bulabilirsiniz.

:::image type="content" source="./media/step-one-pic-nine.png" alt-text="Azure Iletişim Hizmetleri paketlerini yükleme":::

Bu paketler, Azure Iletişim Hizmetleri ekibi tarafından sağlanır ve kimlik doğrulama ve çağırma kitaplıklarını içerir. "--Save" komutu, uygulamamızın üretim kullanımı için bu paketlere bağlı olduğunu ve bu paketlere bu paketlere dahil edileceğini bildirir `dependencies` `package.json` . Üretim için uygulamayı oluştururken, paketler üretim kodumuza dahil edilir.


## <a name="publish-your-website-to-azure-static-websites"></a>Web sitenizi Azure statik Web siteleri 'ne yayımlama

### <a name="create-a-configuration-for-production-deployment"></a>Üretim dağıtımı için bir yapılandırma oluşturma

Aşağıdaki kodu öğesine ekleyin `webpack.prod.js` :

```JavaScript
const { merge } = require('webpack-merge');
 const common = require('./webpack.common.js');

 module.exports = merge(common, {
   mode: 'production',
 });
 ```

Bu yapılandırma, webpack.common.js (giriş dosyasını belirttiğimiz ve sonuçların depolanacağı konum) ile birleştirilecek ve modu "üretim" olarak ayarlayacaktır.
 
İçinde `package.json` , aşağıdaki kodu ekleyin:

```JavaScript
"build:prod": "webpack --config webpack.prod.js" 
```

Dosyanız şu şekilde görünmelidir:

```JavaScript
{
  "name": "CallingApp",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build:dev": "webpack-dev-server --config webpack.dev.js",
    "build:prod": "webpack --config webpack.prod.js" 
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@azure/communication-calling": "^1.0.0-beta.3",
    "@azure/communication-common": "^1.0.0-beta.3"
  },
  "devDependencies": {
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11",
    "webpack-dev-server": "^3.10.3",
    "webpack-merge": "^5.7.3"
  }
}
```

 :::image type="content" source="./media/step-one-pic-20.png" alt-text="Yapılandırılan dosyalar":::


Terminal çalıştırmasında:

```Console
npm run build:prod
```

Komut `dist` içinde bir klasör ve üretime Ready `app.js` statik dosya oluşturur. 

 :::image type="content" source="./media/step-one-pic-21.png" alt-text="Üretim derlemesi":::
 
 
### <a name="deploy-your-app-to-azure-storage"></a>Uygulamanızı Azure Storage 'a dağıtma
 
`index.html`Klasörünü ve `app.css` klasörüne kopyalayın `dist` .

`dist`Klasöründe yeni bir dosya oluşturun ve bu dosyayı adlandırın `404.html` . Aşağıdaki biçimlendirmeyi bu dosyaya kopyalayın:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="./app.css"/>
    <title>Document</title>
</head>
<body>
    <h1>The page does not exists.</h1>
</body>
</html>
```

Dosyayı kaydedin (CTRL + S).

Sağ tıklayın ve Azure depolama aracılığıyla statik Web sitesine Dağıt ' ı seçin.

:::image type="content" source="./media/step-one-pic-22.png" alt-text="Azure 'a dağıtmaya başlayın":::
 
`Select subscription`Alanında, daha önce bir abonelik oluşturmadıysanız, "Azure 'Da oturum aç (veya" ücretsiz bir Azure hesabı oluşturun ") seçeneğini belirleyin.
 
:::image type="content" source="./media/step-one-pic-23.png" alt-text="Azure'da oturum açma":::
 
Şunu seçin `Create new Storage Account`  >  `Advanced` :

 :::image type="content" source="./media/step-one-pic-24.png" alt-text="Depolama hesabı grubu oluşturuluyor":::
 
 Depolama grubunun adını belirtin:
 
 :::image type="content" source="./media/step-one-pic-25.png" alt-text="Hesap için bir ad ekleniyor":::
 
Gerekirse yeni bir kaynak grubu oluşturun:
 
  :::image type="content" source="./media/step-one-pic-26.png" alt-text="Yeni grup oluşturuluyor":::
  
  "Yes" yanıtını, statik Web sitesi barındırmayı etkinleştirmek istiyor musunuz? "
  
  :::image type="content" source="./media/step-one-pic-27.png" alt-text="Statik Web sitesi barındırmayı etkinleştirme seçeneği seçiliyor":::
  
Dosyayı oluşturduğumuz, "Dizin belge adını girin" bölümünde varsayılan dosya adını kabul edin `index.html` .

`404.html`"404 hata belge yolunu girin" yazın.  
  
Uygulamanın konumunu seçin. Seçtiğiniz konum, daha sonra Grup çağrılarında uygulamanızı çağırmak için hangi medya işlemcisinin kullanılacağını tanımlayacaktır. 

Azure Iletişim Hizmetleri, uygulama konumuna göre medya Işlemcisini seçer.

:::image type="content" source="./media/step-one-pic-28.png" alt-text="Konum seçin":::
  
Kaynak ve Web siteniz oluşturuluncaya kadar bekleyin. 
 
"Web sitesine gidin" seçeneğine tıklayın:

:::image type="content" source="./media/step-one-pic-29.png" alt-text="Dağıtım tamamlandı":::
 
Tarayıcınızın geliştirme araçlarından, kaynağı inceleyebilir ve üretime hazırlanan, bizim dosyanızı görebilirsiniz.
 
:::image type="content" source="./media/step-one-pic-30.png" alt-text="Web Sitesi":::

[Azure Portal](https://portal.azure.com/#home)gidin, kaynak grubunuzu seçin, oluşturduğunuz uygulamayı seçin ve adresine gidin `Settings`  >  `Static website` . Statik web sitelerinin etkinleştirildiğini görebilir ve birincil uç noktayı, Dizin belgesini ve hata yolu belge dosyalarını unutmayın.

:::image type="content" source="./media/step-one-pic-31.png" alt-text="Statik Web sitesi seçimi":::

"Blob hizmeti" altında "kapsayıcılar" ı seçin ve bir tane oluşturulan iki kapsayıcı, örneğin Günlükler ($logs) ve Web sitenizin içeriği ($web) görüntülenir

:::image type="content" source="./media/step-one-pic-32.png" alt-text="Kapsayıcı yapılandırması":::

' A giderseniz, `$web` Visual Studio 'da oluşturduğunuz ve Azure 'a dağıttığınız dosyaları görürsünüz. 

:::image type="content" source="./media/step-one-pic-33.png" alt-text="Dağıtım":::

Uygulamayı dilediğiniz zaman Visual Studio Code yeniden dağıtabilirsiniz.

Şimdi ilk Azure Communication Services Web uygulamanızı oluşturmaya hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulamanıza sesli arama ekleme](../quickstarts/voice-video-calling/getting-started-with-calling.md)

Ayrıca şunları yapmak isteyebilirsiniz:

- [Uygulamanıza sohbet ekleme](../quickstarts/chat/get-started.md)
- [Kullanıcı erişim belirteçleri oluşturma](../quickstarts/access-tokens.md)
- [İstemci ve sunucu mimarisi hakkında bilgi edinin](../concepts/client-and-server-architecture.md)
- [Kimlik doğrulaması hakkında bilgi edinin](../concepts/authentication.md)

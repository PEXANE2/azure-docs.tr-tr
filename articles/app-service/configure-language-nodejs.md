---
title: Windows Node.js uygulamalarını yapılandırma
description: App Service yerel Windows örneklerinde Node.js uygulamasının nasıl yapılandırılacağını öğrenin. Bu makalede en sık kullanılan yapılandırma görevleri gösterilmektedir.
ms.devlang: nodejs
ms.topic: article
ms.date: 06/02/2020
ms.openlocfilehash: 9f4ccdd04b8d57784f452dc28fa4507fb7ea94c2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84908202"
---
# <a name="configure-a-windows-nodejs-app-for-azure-app-service"></a>Azure App Service için bir Windows Node.js uygulaması yapılandırma

Node.js uygulamaların tüm gerekli NPM bağımlılıklarıyla dağıtılması gerekir. App Service dağıtım altyapısı, `npm install --production` bir [Git deposu](deploy-local-git.md)dağıtırken sizin için otomatik olarak çalışır veya derleme Otomasyonu etkin olan bir [ZIP paketidir](deploy-zip.md) . Ancak, [FTP/S](deploy-ftp.md)kullanarak dosyalarınızı dağıtırsanız, gerekli paketleri el ile yüklemeniz gerekir. Linux uygulamaları hakkında daha fazla bilgi için bkz. [Azure App Service Için LINUX php uygulaması yapılandırma](containers/configure-language-nodejs.md).

Bu kılavuzda, App Service dağıtan Node.js geliştiricilere yönelik temel kavramlar ve yönergeler sağlanmaktadır. Azure App Service hiç kullanmadıysanız, önce [Node.js hızlı](app-service-web-get-started-nodejs.md) başlangıcı ve [Node.js MongoDB öğreticisi ile](app-service-web-tutorial-nodejs-mongodb-app.md) izleyin.

## <a name="show-nodejs-version"></a>Node.js sürümünü göster

Geçerli Node.js sürümünü göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --query "[?name=='WEBSITE_NODE_DEFAULT_VERSION'].value"
```

Desteklenen tüm Node.js sürümlerini göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp list-runtimes | grep node
```

## <a name="set-nodejs-version"></a>Node.js sürümünü ayarla

Uygulamanızı [desteklenen bir Node.js sürümüne](#show-nodejs-version)ayarlamak için, [Cloud Shell](https://shell.azure.com) desteklenen bir sürüme ayarlamak için aşağıdaki komutu çalıştırın `WEBSITE_NODE_DEFAULT_VERSION` :

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings WEBSITE_NODE_DEFAULT_VERSION="10.15"
```

Bu ayar, App Service derleme Otomasyonu sırasında hem çalışma zamanında hem de otomatik paket geri yükleme sırasında kullanılacak Node.js sürümünü belirtir.

> [!NOTE]
> Projenizin Node.js sürümünü ayarlamanız gerekir `package.json` . Dağıtım altyapısı, desteklenen tüm Node.js sürümlerini içeren ayrı bir işlemde çalışır.

## <a name="access-environment-variables"></a> Ortam değişkenlerine erişme

App Service, uygulama ayarlarınızı uygulama kodunuzun dışında [ayarlayabilirsiniz](configure-common.md) . Ardından, standart Node.js modelini kullanarak bunlara erişebilirsiniz. Örneğin, adlı bir uygulama ayarına erişmek için `NODE_ENV` aşağıdaki kodu kullanın:

```javascript
process.env.NODE_ENV
```

## <a name="run-gruntbowergulp"></a>Grsıt/Bower/Gulp çalıştırma

Varsayılan olarak, App Service derleme Otomasyonu çalıştırmaları `npm install --production` Node.js bir uygulama, git (veya derleme Otomasyonu etkin olan ZIP dağıtımı) aracılığıyla dağıtılır. Uygulamanız Grsıt, Bower veya Gulp gibi popüler Otomasyon araçlarından herhangi birini gerektiriyorsa, çalıştırmak için [özel bir dağıtım betiği](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) sağlamanız gerekir.

Bu araçları çalıştırmak üzere deponuzu etkinleştirmek için, bunlarıpackage.jsiçindeki bağımlılıklara eklemeniz gerekir *.* Örneğin:

```json
"dependencies": {
  "bower": "^1.7.9",
  "grunt": "^1.0.1",
  "gulp": "^3.9.1",
  ...
}
```

Yerel bir terminal penceresinden dizini depo kökinizle değiştirin ve aşağıdaki komutları çalıştırın:

```bash
npm install kuduscript -g
kuduscript --node --scriptType bash --suppressPrompt
```

Depo kökünde Şu anda iki ek dosya vardır: *. Deployment* ve *Deploy.sh*.

*Deploy.sh* açın ve aşağıdaki `Deployment` gibi görünen bölümü bulun:

```bash
##################################################################################################################################
# Deployment
# ----------
```

Bu bölüm çalışmayı sonlandırır `npm install --production` . Bölüm *sonunda* gerekli aracı çalıştırmak için gereken kod bölümünü ekleyin `Deployment` :

- [Bower](#bower)
- [Gulp](#gulp)
- [Grunt](#grunt)

[MEAN.js örneğinde](https://github.com/Azure-Samples/meanjs/blob/master/deploy.sh#L112-L135), dağıtım komut dosyasının da özel bir komut çalıştırdığı bir örnek görürsünüz `npm install` .

### <a name="bower"></a>Bower

Bu kod parçacığı çalışır `bower install` .

```bash
if [ -e "$DEPLOYMENT_TARGET/bower.json" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/bower install
  exitWithMessageOnError "bower failed"
  cd - > /dev/null
fi
```

### <a name="gulp"></a>Gulp

Bu kod parçacığı çalışır `gulp imagemin` .

```bash
if [ -e "$DEPLOYMENT_TARGET/gulpfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/gulp imagemin
  exitWithMessageOnError "gulp failed"
  cd - > /dev/null
fi
```

### <a name="grunt"></a>Grunt

Bu kod parçacığı çalışır `grunt` .

```bash
if [ -e "$DEPLOYMENT_TARGET/Gruntfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/grunt
  exitWithMessageOnError "Grunt failed"
  cd - > /dev/null
fi
```

## <a name="detect-https-session"></a>HTTPS oturumunu Algıla

App Service, [SSL sonlandırması](https://wikipedia.org/wiki/TLS_termination_proxy) ağ yükü dengeleyicilerde gerçekleşinceye kadar, tüm https istekleri UYGULAMANıZA şifrelenmemiş HTTP istekleri olarak ulaşacak. Uygulama mantığınızın kullanıcı isteklerinin şifrelenip şifrelenmediğini denetlemesi gerekiyorsa, `X-Forwarded-Proto` üstbilgiyi inceleyin.

Popüler Web çerçeveleri `X-Forwarded-*` Standart uygulama hiyerarşinizdeki bilgilere erişmenizi sağlar. [Express](https://expressjs.com/)'te [güven proxy 'leri](https://expressjs.com/guide/behind-proxies.html)kullanabilirsiniz. Örneğin:

```javascript
app.set('trust proxy', 1)
...
if (req.secure) {
  // Do something when HTTPS is used
}
```

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

## <a name="troubleshooting"></a>Sorun giderme

Çalışan bir Node.js uygulaması App Service veya hata varsa, şunları deneyin:

- [Günlük akışına erişin](#access-diagnostic-logs).
- Uygulamayı üretim modunda yerel olarak test edin. App Service, Node.js uygulamalarınızı üretim modunda çalıştırır, bu nedenle projenizin üretim modunda yerel olarak beklendiği gibi çalıştığından emin olmanız gerekir. Örneğin:
    - *package.js*bağlı olarak, üretim modu ( `dependencies` vs.) için farklı paketler yüklenebilir `devDependencies` .
    - Bazı Web çerçeveleri, statik dosyaları üretim modunda farklı şekilde dağıtabilir.
    - Belirli Web çerçeveleri, üretim modunda çalışırken özel başlatma betikleri kullanabilir.
- Uygulamanızı geliştirme modunda App Service çalıştırın. Örneğin, [MEAN.js](https://meanjs.org/), uygulama [ `NODE_ENV` ayarını ayarlayarak](configure-common.md)uygulamanızı çalışma zamanında geliştirme moduna ayarlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: MongoDB ile uygulama Node.js](app-service-web-tutorial-nodejs-mongodb-app.md)


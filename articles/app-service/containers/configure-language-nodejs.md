---
title: Node.js uygulamalarını yapılandırma
description: Uygulamanız için önceden oluşturulmuş bir Node.js kapsayıcısını yapılandırmayı öğrenin. Bu makalede en sık kullanılan yapılandırma görevleri gösterilmektedir.
ms.devlang: nodejs
ms.topic: article
ms.date: 03/28/2019
ms.openlocfilehash: 699c77e937fc13cadf742d193ab1b0b8f00a2726
ms.sourcegitcommit: 34eb5e4d303800d3b31b00b361523ccd9eeff0ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84905723"
---
# <a name="configure-a-linux-nodejs-app-for-azure-app-service"></a>Azure App Service için bir Linux Node.js uygulaması yapılandırma

Node.js uygulamaların tüm gerekli NPM bağımlılıklarıyla dağıtılması gerekir. App Service dağıtım altyapısı (kudu), `npm install --production` bir [Git deposu](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)dağıtırken sizin için otomatik olarak çalışır veya derleme Işlemlerine sahip bir [ZIP paketi](../deploy-zip.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) üzerinde geçiş yapar. Ancak, [FTP/S](../deploy-ftp.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)kullanarak dosyalarınızı dağıtırsanız, gerekli paketleri el ile yüklemeniz gerekir.

Bu kılavuzda, App Service yerleşik bir Linux kapsayıcısını kullanan Node.js geliştiricilere yönelik temel kavramlar ve yönergeler sağlanmaktadır. Azure App Service hiç kullanmadıysanız, önce [Node.js hızlı](quickstart-nodejs.md) başlangıcı ve [Node.js MongoDB öğreticisi ile](tutorial-nodejs-mongodb-app.md) izleyin.

## <a name="show-nodejs-version"></a>Node.js sürümünü göster

Geçerli Node.js sürümünü göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Desteklenen tüm Node.js sürümlerini göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp list-runtimes --linux | grep NODE
```

## <a name="set-nodejs-version"></a>Node.js sürümünü ayarla

Uygulamanızı [desteklenen bir Node.js sürümüne](#show-nodejs-version)ayarlamak için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "NODE|10.14"
```

Bu ayar, çalışma zamanında ve kudu 'de otomatik paket geri yükleme sırasında kullanılacak Node.js sürümünü belirtir.

> [!NOTE]
> Projenizin Node.js sürümünü ayarlamanız gerekir `package.json` . Dağıtım altyapısı, desteklenen tüm Node.js sürümlerini içeren ayrı bir kapsayıcıda çalışır.

## <a name="customize-build-automation"></a>Derleme Otomasyonu 'nu özelleştirme

Uygulamanızı, derleme Otomasyonu açıkken git veya ZIP paketleri kullanarak dağıtırsanız, App Service aşağıdaki sırayla Otomasyon adımları oluşturun:

1. Tarafından belirtilmişse özel betiği çalıştırın `PRE_BUILD_SCRIPT_PATH` .
1. `npm install`NPM ve betikler dahil olmak üzere herhangi bir bayrak olmadan çalıştırın `preinstall` `postinstall` ve ayrıca yüklenir `devDependencies` .
1. `npm run build` *Üzerindepackage.js*bir derleme betiği belirtilmişse çalıştırın.
1. `npm run build:azure` *Üzerindepackage.js*bir derleme: Azure betiği belirtilmişse çalıştırın.
1. Tarafından belirtilmişse özel betiği çalıştırın `POST_BUILD_SCRIPT_PATH` .

> [!NOTE]
> [NPM belgeleri](https://docs.npmjs.com/misc/scripts)bölümünde açıklandığı gibi, `prebuild` `postbuild` belirtilen ve sırasıyla önce ve sonra çalıştırılan betikler `build` . `preinstall`ve `postinstall` sırasıyla önce ve sonra çalıştırın `install` .

`PRE_BUILD_COMMAND`ve `POST_BUILD_COMMAND` Varsayılan olarak boş olan ortam değişkenleridir. Oluşturma öncesi komutları çalıştırmak için, tanımlayın `PRE_BUILD_COMMAND` . Oluşturma sonrası komutları çalıştırmak için, tanımlayın `POST_BUILD_COMMAND` .

Aşağıdaki örnek, virgülle ayrılmış bir dizi komuta iki değişkeni belirtir.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PRE_BUILD_COMMAND="echo foo, scripts/prebuild.sh"
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings POST_BUILD_COMMAND="echo foo, scripts/postbuild.sh"
```

Derleme Otomasyonu 'nu özelleştirmek için ek ortam değişkenleri için bkz. [Oryx Configuration](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md).

App Service nasıl çalıştığı ve Linux 'ta Node.js uygulamalar derleme hakkında daha fazla bilgi için bkz. [Oryx belgeleri: Node.js uygulamalar nasıl algılanır ve oluşturulur](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/nodejs.md).

## <a name="configure-nodejs-server"></a>Node.js sunucusunu yapılandır

Node.js kapsayıcıları, bir üretim işlem yöneticisi olan [PM2](https://pm2.keymetrics.io/)ile gelir. Uygulamanızı PM2 veya NPM ile veya özel bir komutla başlayacak şekilde yapılandırabilirsiniz.

- [Özel komut Çalıştır](#run-custom-command)
- [NPM başlangıcını Çalıştır](#run-npm-start)
- [PM2 ile Çalıştır](#run-with-pm2)

### <a name="run-custom-command"></a>Özel komut Çalıştır

App Service, uygulamanızı *Run.sh*gibi bir yürütülebilir dosya gibi özel bir komut kullanarak başlatabilir. Örneğin, çalıştırmak için `npm run start:prod` [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "npm run start:prod"
```

### <a name="run-npm-start"></a>NPM başlangıcını Çalıştır

Uygulamanızı kullanarak başlatmak için `npm start` , yalnızca bir `start` betiğin dosyada *package.js* olduğundan emin olun. Örneğin:

```json
{
  ...
  "scripts": {
    "start": "gulp",
    ...
  },
  ...
}
```

Projenizdeki özel bir *package.js* kullanmak için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filename>.json"
```

### <a name="run-with-pm2"></a>PM2 ile Çalıştır

Kapsayıcıda ortak Node.js dosyalarından biri bulunduğunda kapsayıcı, uygulamanızı otomatik olarak PM2 ile başlatır:

- *bin/www*
- *server.js*
- *app.js*
- *index.js*
- *hostingstart.js*
- Aşağıdaki [PM2 dosyalarından](https://pm2.keymetrics.io/docs/usage/application-declaration/#process-file)biri: *process.js* ve *ecosystem.config.js*

Ayrıca, aşağıdaki uzantılara sahip özel bir başlangıç dosyası da yapılandırabilirsiniz:

- Bir *. js* dosyası
- *. JSON*, *.config.js*, *. YAML*veya *. yıml* uzantılı bir [PM2 dosyası](https://pm2.keymetrics.io/docs/usage/application-declaration/#process-file)

Özel bir başlangıç dosyası eklemek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filname-with-extension>"
```

## <a name="debug-remotely"></a>Uzaktan hata ayıklama

> [!NOTE]
> Uzaktan hata ayıklama Şu anda önizleme aşamasındadır.

Bir * .config.js, *. yıml veya *. YAML*kullanarak çalıştırmanız dışında, [PM2 ile çalışacak](#run-with-pm2)şekilde yapılandırırsanız Node.js uygulamanızda [Visual Studio Code](https://code.visualstudio.com/) uzaktan hata ayıklayabilirsiniz.

Çoğu durumda, uygulamanız için ek yapılandırma gerekmez. Uygulamanız dosyadaki bir *process.js* (varsayılan veya özel) ile ÇALıŞıYORSA, `script` JSON kökünde bir özelliği olmalıdır. Örneğin:

```json
{
  "name"        : "worker",
  "script"      : "./index.js",
  ...
}
```

Uzaktan hata ayıklama için Visual Studio Code ayarlamak için [App Service uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)kurun. Uzantı sayfasındaki yönergeleri izleyin ve Visual Studio Code Azure 'da oturum açın.

Azure Gezgini 'nde, hata ayıklamak istediğiniz uygulamayı bulun, sağ tıklayın ve **Uzaktan hata ayıklamayı Başlat**' ı seçin. Uygulamanızı etkinleştirmek için **Evet** ' e tıklayın. App Service, sizin için bir tünel proxy 'si başlatır ve hata ayıklayıcıyı iliştirir. Daha sonra uygulamaya istek yapabilir ve hata ayıklayıcıyı kesme noktalarında duraklatarak görebilirsiniz.

Hata ayıklama işlemi tamamlandıktan sonra, **bağlantıyı kes**' i seçerek hata ayıklayıcıyı durdurun. İstendiğinde, uzaktan hata ayıklamayı devre dışı bırakmak için **Evet** ' e tıklamanız gerekir. Daha sonra devre dışı bırakmak için, Azure Gezgini 'nde uygulamanızı tekrar sağ tıklayın ve **Uzaktan hata ayıklamayı devre dışı bırak**' ı seçin.

## <a name="access-environment-variables"></a> Ortam değişkenlerine erişme

App Service, uygulama ayarlarınızı uygulama kodunuzun dışında [ayarlayabilirsiniz](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) . Ardından, standart Node.js modelini kullanarak bunlara erişebilirsiniz. Örneğin, adlı bir uygulama ayarına erişmek için `NODE_ENV` aşağıdaki kodu kullanın:

```javascript
process.env.NODE_ENV
```

## <a name="run-gruntbowergulp"></a>Grsıt/Bower/Gulp çalıştırma

Varsayılan olarak kudu, `npm install --production` bir Node.js uygulaması dağıtıldığında çalışır. Uygulamanız Grsıt, Bower veya Gulp gibi popüler Otomasyon araçlarından herhangi birini gerektiriyorsa, çalıştırmak için [özel bir dağıtım betiği](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) sağlamanız gerekir.

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

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-linux-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>SSH oturumunu tarayıcıda aç

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Sorun giderme

Çalışan bir Node.js uygulaması App Service veya hata varsa, şunları deneyin:

- [Günlük akışına erişin](#access-diagnostic-logs).
- Uygulamayı üretim modunda yerel olarak test edin. App Service, Node.js uygulamalarınızı üretim modunda çalıştırır, bu nedenle projenizin üretim modunda yerel olarak beklendiği gibi çalıştığından emin olmanız gerekir. Örneğin:
    - *package.js*bağlı olarak, üretim modu ( `dependencies` vs.) için farklı paketler yüklenebilir `devDependencies` .
    - Bazı Web çerçeveleri, statik dosyaları üretim modunda farklı şekilde dağıtabilir.
    - Belirli Web çerçeveleri, üretim modunda çalışırken özel başlatma betikleri kullanabilir.
- Uygulamanızı geliştirme modunda App Service çalıştırın. Örneğin, [MEAN.js](https://meanjs.org/), uygulama [ `NODE_ENV` ayarını ayarlayarak](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings)uygulamanızı çalışma zamanında geliştirme moduna ayarlayabilirsiniz.

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: MongoDB ile uygulama Node.js](tutorial-nodejs-mongodb-app.md)

> [!div class="nextstepaction"]
> [App Service Linux SSS](app-service-linux-faq.md)

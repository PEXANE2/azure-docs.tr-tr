---
title: Node. js uygulamalarını yapılandırma
description: Uygulamanız için önceden oluşturulmuş bir Node. js kapsayıcısını yapılandırmayı öğrenin. Bu makalede en sık kullanılan yapılandırma görevleri gösterilmektedir.
ms.devlang: nodejs
ms.topic: article
ms.date: 03/28/2019
ms.openlocfilehash: fdc5129fc395f99cb4c244414ea952b2776dc4dc
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78255864"
---
# <a name="configure-a-linux-nodejs-app-for-azure-app-service"></a>Azure App Service için Linux Node. js uygulaması yapılandırma

Node. js uygulamaları tüm gerekli NPM bağımlılıklarıyla dağıtılmalıdır. App Service dağıtım altyapısı (kudu), bir [Git deposu](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)dağıtırken sizin için `npm install --production` otomatik olarak çalışır veya derleme işlemlerine sahip bir [ZIP paketi](../deploy-zip.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) üzerinde geçiş yapar. Ancak, [FTP/S](../deploy-ftp.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)kullanarak dosyalarınızı dağıtırsanız, gerekli paketleri el ile yüklemeniz gerekir.

Bu kılavuz, App Service içinde yerleşik bir Linux kapsayıcısı kullanan Node. JS geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure App Service hiç kullanmadıysanız, önce [MongoDB öğreticisi Ile](tutorial-nodejs-mongodb-app.md) [Node. js hızlı başlangıç](quickstart-nodejs.md) ve Node. js ' yi izleyin.

## <a name="show-nodejs-version"></a>Node. js sürümünü göster

Geçerli Node. js sürümünü göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Desteklenen tüm Node. js sürümlerini göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp list-runtimes --linux | grep NODE
```

## <a name="set-nodejs-version"></a>Node. js sürümünü ayarla

Uygulamanızı [desteklenen bir Node. js sürümüne](#show-nodejs-version)ayarlamak için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "NODE|10.14"
```

Bu ayar, çalışma zamanında ve kudu içinde otomatik paket geri yükleme sırasında kullanılacak Node. js sürümünü belirtir.

> [!NOTE]
> Projenizin `package.json`Node. js sürümünü ayarlamanız gerekir. Dağıtım altyapısı, desteklenen tüm Node. js sürümlerini içeren ayrı bir kapsayıcıda çalışır.

## <a name="customize-build-automation"></a>Derleme Otomasyonu 'nu özelleştirme

Uygulamanızı, derleme Otomasyonu açıkken git veya ZIP paketleri kullanarak dağıtırsanız, App Service aşağıdaki sırayla Otomasyon adımları oluşturun:

1. `PRE_BUILD_SCRIPT_PATH`tarafından belirtilmişse özel betiği çalıştırın.
1. `npm install`, NPM `preinstall` ve `postinstall` betikleri içeren ve ayrıca `devDependencies`yüklenen bayraklar olmadan çalıştırın.
1. *Package. JSON*içinde bir derleme betiği belirtilmişse `npm run build` çalıştırın.
1. Bir derleme: Azure betiği *Package. JSON*içinde belirtilmişse `npm run build:azure` Çalıştır.
1. `POST_BUILD_SCRIPT_PATH`tarafından belirtilmişse özel betiği çalıştırın.

> [!NOTE]
> [NPM belgeleri](https://docs.npmjs.com/misc/scripts)bölümünde açıklandığı gibi, `prebuild` ve `postbuild` olarak adlandırılan betikler, belirtilmişse sırasıyla `build`ve sonrasında çalışır. `preinstall` ve `postinstall` sırasıyla `install`önce ve sonra çalıştırın.

`PRE_BUILD_COMMAND` ve `POST_BUILD_COMMAND`, varsayılan olarak boş olan ortam değişkenleridir. Oluşturma öncesi komutları çalıştırmak için `PRE_BUILD_COMMAND`tanımlayın. Oluşturma sonrası komutları çalıştırmak için `POST_BUILD_COMMAND`tanımlayın.

Aşağıdaki örnek, virgülle ayrılmış bir dizi komuta iki değişkeni belirtir.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PRE_BUILD_COMMAND="echo foo, scripts/prebuild.sh"
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings POST_BUILD_COMMAND="echo foo, scripts/postbuild.sh"
```

Derleme Otomasyonu 'nu özelleştirmek için ek ortam değişkenleri için bkz. [Oryx Configuration](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md).

App Service nasıl çalıştığı ve Linux 'ta Node. js uygulamaları derleme hakkında daha fazla bilgi için bkz. [Oryx belgeleri: node. js uygulamaları nasıl algılanır ve oluşturulur](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/nodejs.md).

## <a name="configure-nodejs-server"></a>Node. js sunucusunu yapılandırma

Node. js kapsayıcıları, bir üretim işlem yöneticisi olan [PM2](https://pm2.keymetrics.io/)ile gelir. Uygulamanızı PM2 veya NPM ile veya özel bir komutla başlayacak şekilde yapılandırabilirsiniz.

- [Özel komut Çalıştır](#run-custom-command)
- [NPM başlangıcını Çalıştır](#run-npm-start)
- [PM2 ile Çalıştır](#run-with-pm2)

### <a name="run-custom-command"></a>Özel komut Çalıştır

App Service, uygulamanızı *Run.sh*gibi bir yürütülebilir dosya gibi özel bir komut kullanarak başlatabilir. Örneğin, `npm run start:prod`çalıştırmak için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "npm run start:prod"
```

### <a name="run-npm-start"></a>NPM başlangıcını Çalıştır

Uygulamanızı `npm start`kullanarak başlatmak için, yalnızca bir `start` betiğin *Package. JSON* dosyasında bulunduğundan emin olun. Örnek:

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

Projenizde özel bir *Package. JSON* kullanmak için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filename>.json"
```

### <a name="run-with-pm2"></a>PM2 ile Çalıştır

Projenizde ortak Node. js dosyalarından biri bulunduğunda kapsayıcı, uygulamanızı otomatik olarak PM2 ile başlatır:

- *bin/www*
- *Server. js*
- *App. js*
- *index. js*
- *hostingstart. js*
- Şu [PM2 dosyalarından](https://pm2.keymetrics.io/docs/usage/application-declaration/#process-file)biri: *Process. JSON* ve *ekosystem. config. js*

Ayrıca, aşağıdaki uzantılara sahip özel bir başlangıç dosyası da yapılandırabilirsiniz:

- Bir *. js* dosyası
- *. JSON*, *. config. js*, *. YAML*veya *. yıml* uzantılı bir [PM2 dosyası](https://pm2.keymetrics.io/docs/usage/application-declaration/#process-file)

Özel bir başlangıç dosyası eklemek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filname-with-extension>"
```

## <a name="debug-remotely"></a>Uzaktan hata ayıklama

> [!NOTE]
> Uzaktan hata ayıklama Şu anda önizleme aşamasındadır.

Bir *. config. js, *. yıml veya *. YAML*kullanarak çalıştırmanız dışında, [PM2 ile çalışacak](#run-with-pm2)şekilde yapılandırırsanız, Node. js uygulamanızı [Visual Studio Code](https://code.visualstudio.com/) uzaktan hata ayıklaması yapabilirsiniz.

Çoğu durumda, uygulamanız için ek yapılandırma gerekmez. Uygulamanız bir *Process. JSON* dosyası (varsayılan veya özel) ile ÇALıŞıYORSA, JSON kökünde bir `script` özelliği olmalıdır. Örnek:

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

## <a name="access-environment-variables"></a>Ortam değişkenlerine erişim

App Service, uygulama ayarlarınızı uygulama kodunuzun dışında [ayarlayabilirsiniz](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) . Ardından, standart Node. js modelini kullanarak bunlara erişebilirsiniz. Örneğin, `NODE_ENV`adlı bir uygulama ayarına erişmek için aşağıdaki kodu kullanın:

```javascript
process.env.NODE_ENV
```

## <a name="run-gruntbowergulp"></a>Grsıt/Bower/Gulp çalıştırma

Varsayılan olarak, kudu bir Node 'u algıladığında `npm install --production` çalışır. js uygulaması dağıtılır. Uygulamanız Grsıt, Bower veya Gulp gibi popüler Otomasyon araçlarından herhangi birini gerektiriyorsa, çalıştırmak için [özel bir dağıtım betiği](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) sağlamanız gerekir.

Bu araçları çalıştırmak üzere deponuzu etkinleştirmek için bunları *Package. JSON* içindeki bağımlılıklara eklemeniz gerekir. Örnek:

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

*Deploy.sh* açın ve şuna benzeyen `Deployment` bölümünü bulun:

```bash
##################################################################################################################################
# Deployment
# ----------
```

Bu bölüm, çalışan `npm install --production`biter. `Deployment` bölümünün *sonunda* gerekli aracı çalıştırmak için gereken kod bölümünü ekleyin:

- [Bower](#bower)
- [Gulp](#gulp)
- [Grunt](#grunt)

[Ortalama. js](https://github.com/Azure-Samples/meanjs/blob/master/deploy.sh#L112-L135)örneğinde, dağıtım komut dosyasının da özel bir `npm install` komutu çalıştırdığı bir örneğe bakın.

### <a name="bower"></a>Bower

Bu kod parçacığı `bower install`çalıştırır.

```bash
if [ -e "$DEPLOYMENT_TARGET/bower.json" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/bower install
  exitWithMessageOnError "bower failed"
  cd - > /dev/null
fi
```

### <a name="gulp"></a>Gulp

Bu kod parçacığı `gulp imagemin`çalıştırır.

```bash
if [ -e "$DEPLOYMENT_TARGET/gulpfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/gulp imagemin
  exitWithMessageOnError "gulp failed"
  cd - > /dev/null
fi
```

### <a name="grunt"></a>Grunt

Bu kod parçacığı `grunt`çalıştırır.

```bash
if [ -e "$DEPLOYMENT_TARGET/Gruntfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/grunt
  exitWithMessageOnError "Grunt failed"
  cd - > /dev/null
fi
```

## <a name="detect-https-session"></a>HTTPS oturumunu Algıla

App Service, [SSL sonlandırması](https://wikipedia.org/wiki/TLS_termination_proxy) ağ yükü dengeleyicilerde gerçekleşinceye kadar, tüm https istekleri UYGULAMANıZA şifrelenmemiş HTTP istekleri olarak ulaşacak. Uygulama mantığınızın kullanıcı isteklerinin şifrelenip şifrelenmediğini denetlemesi gerekiyorsa `X-Forwarded-Proto` üst bilgisini inceleyin.

Popüler Web çerçeveleri, standart uygulama hiyerarşinizdeki `X-Forwarded-*` bilgilerine erişmenizi sağlar. [Express](https://expressjs.com/)'te [güven proxy 'leri](https://expressjs.com/guide/behind-proxies.html)kullanabilirsiniz. Örnek:

```javascript
app.set('trust proxy', 1)
...
if (req.secure) {
  // Do something when HTTPS is used
}
```

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>SSH oturumunu tarayıcıda aç

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Sorun giderme

Çalışan bir Node. js uygulaması App Service farklı davrandığı veya hatalara sahip olduğunda, aşağıdakileri deneyin:

- [Günlük akışına erişin](#access-diagnostic-logs).
- Uygulamayı üretim modunda yerel olarak test edin. App Service, Node. js uygulamalarınızı üretim modunda çalıştırır, bu nedenle projenizin üretim modunda yerel olarak beklendiği gibi çalıştığından emin olmanız gerekir. Örnek:
    - *Package. JSON*' a bağlı olarak, üretim modu (`dependencies` vs. `devDependencies`) için farklı paketler yüklenebilir.
    - Bazı Web çerçeveleri, statik dosyaları üretim modunda farklı şekilde dağıtabilir.
    - Belirli Web çerçeveleri, üretim modunda çalışırken özel başlatma betikleri kullanabilir.
- Uygulamanızı geliştirme modunda App Service çalıştırın. Örneğin, [Ortalama. js](https://meanjs.org/)' de, [`NODE_ENV` uygulama ayarını ayarlayarak](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings)uygulamanızı çalışma zamanında geliştirme moduna ayarlayabilirsiniz.

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: MongoDB ile Node. js uygulaması](tutorial-nodejs-mongodb-app.md)

> [!div class="nextstepaction"]
> [App Service Linux SSS](app-service-linux-faq.md)

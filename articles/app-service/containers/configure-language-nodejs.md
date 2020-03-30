---
title: Node.js uygulamalarını yapılandır
description: Uygulamanız için önceden oluşturulmuş bir Düğüm.js kapsayıcısını nasıl yapılandıracaklarınızı öğrenin. Bu makalede, en yaygın yapılandırma görevleri gösterir.
ms.devlang: nodejs
ms.topic: article
ms.date: 03/28/2019
ms.openlocfilehash: fdc5129fc395f99cb4c244414ea952b2776dc4dc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252733"
---
# <a name="configure-a-linux-nodejs-app-for-azure-app-service"></a>Azure Uygulama Hizmeti için Bir Linux Node.js uygulamasını yapılandırma

Node.js uygulamaları gerekli tüm NPM bağımlılıklarıyla dağıtılmalıdır. Uygulama Hizmeti dağıtım motoru (Kudu), `npm install --production` [bir Git deposu](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)veya yapı işlemlerinin açık olduğu bir [Zip paketini](../deploy-zip.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) dağıttığınızda sizin için otomatik olarak çalışır. Ancak, dosyalarınızı [FTP/S](../deploy-ftp.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)kullanarak dağıtıyorsanız, gerekli paketleri el ile yüklemeniz gerekir.

Bu kılavuz, App Service'te yerleşik bir Linux kapsayıcısı kullanan Node.js geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure Uygulama Hizmeti'ni hiç kullanmadıysanız, önce [MongoDB öğreticisiyle](tutorial-nodejs-mongodb-app.md) [Node.js quickstart](quickstart-nodejs.md) ve Node.js'yi izleyin.

## <a name="show-nodejs-version"></a>Düğüm.js sürümünü göster

Geçerli Node.js sürümünü göstermek için [Bulut Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Desteklenen tüm Node.js sürümlerini göstermek için [Bulut Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp list-runtimes --linux | grep NODE
```

## <a name="set-nodejs-version"></a>Düğüm.js sürümünü ayarla

Uygulamanızı desteklenen bir [Düğüm.js sürümüne](#show-nodejs-version)ayarlamak için Bulut [Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "NODE|10.14"
```

Bu ayar, hem çalışma zamanında hem de Kudu'daki otomatik paket geri yükleme sırasında kullanılacak Node.js sürümünü belirtir.

> [!NOTE]
> Node.js sürümünü projenizde `package.json`ayarlamalısınız. Dağıtım altyapısı, desteklenen tüm Node.js sürümlerini içeren ayrı bir kapsayıcıda çalışır.

## <a name="customize-build-automation"></a>Yapı otomasyonu özelleştirme

Uygulamanızı Git veya zip paketlerini yapı otomasyonu açık ken kullanırsanız, Uygulama Hizmeti aşağıdaki sırayla otomasyon adımları oluşturur:

1. 'ye göre belirtilirse özel komut dosyası çalıştırın `PRE_BUILD_SCRIPT_PATH`
1. NPM `preinstall` ve `postinstall` komut dosyaları içeren ve aynı zamanda `devDependencies`yükler herhangi bir bayrak olmadan çalıştırın. `npm install`
1. Paketinizde bir yapı komut dosyası belirtilmişse `npm run build` *çalıştırın.json*.
1. Paketinizde bir yapı:azure komut dosyası belirtilmişse `npm run build:azure` *çalıştırın.json*.
1. 'ye göre belirtilirse özel komut dosyası çalıştırın `POST_BUILD_SCRIPT_PATH`

> [!NOTE]
> [npm dokümanlarında](https://docs.npmjs.com/misc/scripts)açıklandığı gibi, `prebuild` belirtilmişse, `build`sırasıyla, önce ve sonra adlı ve `postbuild` çalıştırılan komut dosyaları. `preinstall`ve `postinstall` sırasıyla `install`önce ve sonra çalıştırın.

`PRE_BUILD_COMMAND`ve `POST_BUILD_COMMAND` varsayılan olarak boş olan ortam değişkenleridir. Önceden yapı komutları çalıştırmak `PRE_BUILD_COMMAND`için tanımlayın. Yapı sonrası komutları çalıştırmak `POST_BUILD_COMMAND`için.

Aşağıdaki örnekte, iki değişken virgülle ayrılmış bir dizi komuta göre belirtilir.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PRE_BUILD_COMMAND="echo foo, scripts/prebuild.sh"
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings POST_BUILD_COMMAND="echo foo, scripts/postbuild.sh"
```

Yapı otomasyonu özelleştirmek için ek ortam değişkenleri için [Bkz. Oryx yapılandırması.](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md)

Uygulama Hizmeti'nin Linux'ta Node.js uygulamalarını nasıl çalıştırıp oluşturduğu hakkında daha fazla bilgi için [Oryx belgelerine bakın: Node.js uygulamaları nasıl algılanır ve oluşturulur.](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/nodejs.md)

## <a name="configure-nodejs-server"></a>Node.js sunucusuyapıla

Düğüm.js kapları [pm2](https://pm2.keymetrics.io/)ile birlikte gelir , bir üretim süreci yöneticisi. Uygulamanızı PM2 ile başlayacak, NPM ile veya özel bir komutla yapılandırabilirsiniz.

- [Özel komutu çalıştır](#run-custom-command)
- [Npm başlat](#run-npm-start)
- [PM2 ile çalıştırın](#run-with-pm2)

### <a name="run-custom-command"></a>Özel komutu çalıştır

Uygulama Hizmeti, *uygulamanızı run.sh*gibi yürütülebilir bir komut kullanarak başlatabilir. Örneğin, çalıştırmak `npm run start:prod`için, [Bulut Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "npm run start:prod"
```

### <a name="run-npm-start"></a>Npm başlat

Uygulamanızı kullanarak `npm start`başlatmak için, `start` bir komut dosyasının *package.json* dosyasında olduğundan emin olun. Örnek:

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

Projenizde özel bir *package.json* kullanmak için Bulut [Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filename>.json"
```

### <a name="run-with-pm2"></a>PM2 ile çalıştırın

Kapsayıcı, projenizde yaygın Node.js dosyalarından biri bulunduğunda uygulamanızı otomatik olarak PM2 ile başlatır:

- *bin/www*
- *server.js*
- *app.js*
- *index.js*
- *hostingstart.js*
- Aşağıdaki [PM2 dosyalarından](https://pm2.keymetrics.io/docs/usage/application-declaration/#process-file)biri : *process.json* ve *ecosystem.config.js*

Özel bir başlangıç dosyasını aşağıdaki uzantılarla da yapılandırabilirsiniz:

- Bir *.js* dosyası
- Uzantısı *.json*, *.config.js*, *.yaml*veya *.yml* olan bir [PM2 dosyası](https://pm2.keymetrics.io/docs/usage/application-declaration/#process-file)

Özel bir başlangıç dosyası eklemek için [Bulut Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filname-with-extension>"
```

## <a name="debug-remotely"></a>Uzaktan hata ayıklama

> [!NOTE]
> Uzaktan hata ayıklama şu anda Önizleme'de.

*.config.js, *.yml veya *.yaml*kullanarak çalıştırdığınız zamanlar dışında, [PM2 ile çalışacak](#run-with-pm2)şekilde yapılandırırsanız, Düğüm.js uygulamanızı [Visual Studio Code'da](https://code.visualstudio.com/) uzaktan hata ayıklayabilirsiniz.

Çoğu durumda, uygulamanız için ekstra yapılandırma gerekmez. Uygulamanız bir *process.json* dosyasıyla (varsayılan veya özel) `script` çalıştırılırsa, JSON kökünde bir özelliğe sahip olmalıdır. Örnek:

```json
{
  "name"        : "worker",
  "script"      : "./index.js",
  ...
}
```

Uzaktan hata ayıklama için Visual Studio Kodu ayarlamak [için, Uygulama Hizmeti uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)yükleyin. Uzantı sayfasındaki yönergeleri izleyin ve Visual Studio Code'da Azure'da oturum açın.

Azure gezgininde hata ayıklamak istediğiniz uygulamayı bulun, sağ tıklatın ve **Uzaktan Hata Ayıklama başlat'ı**seçin. Uygulamanız için etkinleştirmek için **Evet'i** tıklatın. Uygulama Hizmeti sizin için bir tünel proxy'si başlatır ve hata ayıklamayı ekler. Daha sonra uygulamaya istekte bulunabilir ve hata ayıklamanın kesme noktalarında duraklatma sını görebilirsiniz.

Hata ayıklama ile tamamlandıktan sonra, **Bağlantı Kesme'yi**seçerek hata ayıklamayı durdurun. İstendiğinde, uzaktan hata ayıklamayı devre dışı kışamdırmak için **Evet'i** tıklatmalısınız. Daha sonra devre dışı kalmak için Azure gezgininde uygulamanızı yeniden tıklatın ve **Uzaktan Hata Ayıklama'yı devre dışı bırakın'ı**seçin.

## <a name="access-environment-variables"></a> Ortam değişkenlerine erişme

Uygulama Hizmeti'nde, [uygulama ayarlarınızı](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) uygulama kodunuzu dışında ayarlayabilirsiniz. Daha sonra standart Düğüm.js deseni kullanarak onlara erişebilirsiniz. Örneğin, adlı `NODE_ENV`bir uygulama ayarına erişmek için aşağıdaki kodu kullanın:

```javascript
process.env.NODE_ENV
```

## <a name="run-gruntbowergulp"></a>Koş Homurtu/Bower/Gulp

Varsayılan olarak, Kudu bir Düğüm.js uygulamasının dağıtılmış olduğunu tanıdığında çalışır. `npm install --production` Uygulamanız Grunt, Bower veya Gulp gibi popüler otomasyon araçlarından herhangi birini gerektiriyorsa, çalıştırmak için özel bir [dağıtım komut dosyası](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) sağlamanız gerekir.

Deponuzun bu araçları çalıştırmasını sağlamak için, bunları *package.json'daki* bağımlılıklara eklemeniz gerekir. Örnek:

```json
"dependencies": {
  "bower": "^1.7.9",
  "grunt": "^1.0.1",
  "gulp": "^3.9.1",
  ...
}
```

Yerel bir terminal penceresinden, dizini depo kökünüzle değiştirin ve aşağıdaki komutları çalıştırın:

```bash
npm install kuduscript -g
kuduscript --node --scriptType bash --suppressPrompt
```

Depo kökünüzde artık iki ek dosya vardır: *.deployment* ve *deploy.sh.*

*deploy.sh* açın ve `Deployment` aşağıdaki gibi görünen bölümü bulun:

```bash
##################################################################################################################################
# Deployment
# ----------
```

Bu bölüm çalışma `npm install --production`ile sona erer. Bölümün sonunda gerekli aracı çalıştırmak için gereken kod bölümünü ekleyin: *at the end* `Deployment`

- [Bower](#bower)
- [Gulp](#gulp)
- [Grunt](#grunt)

Dağıtım komut dosyasının da özel `npm install` bir komut çalıştırdığı [MEAN.js örneğinde](https://github.com/Azure-Samples/meanjs/blob/master/deploy.sh#L112-L135)bir örneğe bakın.

### <a name="bower"></a>Bower

Bu parçacık çalışır. `bower install`

```bash
if [ -e "$DEPLOYMENT_TARGET/bower.json" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/bower install
  exitWithMessageOnError "bower failed"
  cd - > /dev/null
fi
```

### <a name="gulp"></a>Gulp

Bu parçacık çalışır. `gulp imagemin`

```bash
if [ -e "$DEPLOYMENT_TARGET/gulpfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/gulp imagemin
  exitWithMessageOnError "gulp failed"
  cd - > /dev/null
fi
```

### <a name="grunt"></a>Grunt

Bu parçacık çalışır. `grunt`

```bash
if [ -e "$DEPLOYMENT_TARGET/Gruntfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/grunt
  exitWithMessageOnError "Grunt failed"
  cd - > /dev/null
fi
```

## <a name="detect-https-session"></a>HTTPS oturumunu algıla

App Service'te, [SSL sonlandırma](https://wikipedia.org/wiki/TLS_termination_proxy) ağı yük dengeleyicilerinde gerçekleşir, böylece tüm HTTPS istekleri uygulamanıza şifrelenmemiş HTTP istekleri olarak ulaşır. Uygulama mantığınızın kullanıcı isteklerinin şifreli olup olmadığını denetlemesi gerekiyorsa, üstbilgide inceleme. `X-Forwarded-Proto`

Popüler web çerçeveleri, `X-Forwarded-*` standart uygulama deseninizdeki bilgilere erişmenizi sağlar. [Express'te](https://expressjs.com/)güven [vekillerini](https://expressjs.com/guide/behind-proxies.html)kullanabilirsiniz. Örnek:

```javascript
app.set('trust proxy', 1)
...
if (req.secure) {
  // Do something when HTTPS is used
}
```

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda SSH oturumunu açma

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Sorun giderme

Çalışan bir Düğüm.js uygulaması Uygulama Hizmeti'nde farklı davrantığında veya hataları varsa, aşağıdakileri deneyin:

- [Günlük akışına erişin.](#access-diagnostic-logs)
- Uygulamayı üretim modunda yerel olarak test edin. Uygulama Hizmeti, Node.js uygulamalarınızı üretim modunda çalıştırır, bu nedenle projenizin yerel üretim modunda beklendiği gibi çalıştığından emin olmanız gerekir. Örnek:
    - *Package.json*bağlı olarak, farklı paketler üretim modu için`dependencies` yüklenebilir ( vs). `devDependencies`
    - Bazı web çerçeveleri statik dosyaları üretim modunda farklı şekilde dağıtabilir.
    - Bazı web çerçeveleri üretim modunda çalışırken özel başlangıç komut dosyaları kullanabilir.
- Uygulamanızı Uygulama Hizmeti'nde geliştirme modunda çalıştırın. Örneğin, [MEAN.js'de,](https://meanjs.org/) [uygulama ayarını ayarlayarak `NODE_ENV` ](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings)uygulamanızı çalışma süresi içinde geliştirme moduna ayarlayabilirsiniz.

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: MongoDB ile Node.js uygulaması](tutorial-nodejs-mongodb-app.md)

> [!div class="nextstepaction"]
> [Uygulama Hizmeti Linux SSS](app-service-linux-faq.md)

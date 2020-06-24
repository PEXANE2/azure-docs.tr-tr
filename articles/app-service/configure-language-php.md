---
title: Windows PHP uygulamalarını yapılandırma
description: App Service yerel Windows örneklerinde PHP uygulamasını nasıl yapılandıracağınızı öğrenin. Bu makalede en sık kullanılan yapılandırma görevleri gösterilmektedir.
ms.devlang: php
ms.topic: article
ms.date: 06/02/2020
ms.openlocfilehash: 1eb4e9d349fdd0097cbde4e4cef3d5c61a167193
ms.sourcegitcommit: 34eb5e4d303800d3b31b00b361523ccd9eeff0ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84908201"
---
# <a name="configure-a-windows-php-app-for-azure-app-service"></a>Azure App Service için bir Windows PHP uygulaması yapılandırma

Bu kılavuzda, Azure App Service ' de PHP web uygulamalarınızı, mobil arka uçlarınızın ve API uygulamalarınızın nasıl yapılandırılacağı gösterilmektedir. Bu kılavuz, Windows platformunda barındırılan uygulamalar için geçerlidir. Linux uygulamaları hakkında daha fazla bilgi için bkz. [Azure App Service Için LINUX php uygulaması yapılandırma](containers/configure-language-php.md).

Bu kılavuzda, App Service için uygulama dağıtan PHP geliştiricilerine yönelik temel kavramlar ve yönergeler sağlanmaktadır. Azure App Service hiç kullanmadıysanız, önce [MySQL öğreticisiyle](app-service-web-tutorial-php-mysql.md) [php hızlı başlangıç](app-service-web-get-started-php.md) ve php 'yi izleyin.

## <a name="show-php-version"></a>PHP sürümünü göster

Geçerli PHP sürümünü göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query phpVersion
```

Tüm desteklenen PHP sürümlerini göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp list-runtimes | grep php
```

## <a name="set-php-version"></a>PHP sürümünü ayarla

PHP sürümünü 7,4 olarak ayarlamak için [Cloud Shell](https://shell.azure.com) aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --php-version 7.4
```

## <a name="run-composer"></a>Besteci Çalıştır

Dağıtım sırasında [besteci](https://getcomposer.org/) App Service çalıştırmak istiyorsanız, en kolay yol, besteci deponuzu eklemek olur.

Yerel bir Terminal penceresinde, Dizin ' i depo kökinizle değiştirin ve besteci [yükleme Oluşturucu](https://getcomposer.org/download/) ' daki yönergeleri izleyerek *Oluşturucu. phar* ' yi Dizin köküne indirin.

Aşağıdaki komutları çalıştırın ( [NPM](https://www.npmjs.com/get-npm) yüklü olmalıdır):

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

Bölüm *sonunda* gerekli aracı çalıştırmak için gereken kod bölümünü ekleyin `Deployment` :

```bash
# 4. Use composer
echo "$DEPLOYMENT_TARGET"
if [ -e "$DEPLOYMENT_TARGET/composer.json" ]; then
  echo "Found composer.json"
  pushd "$DEPLOYMENT_TARGET"
  php composer.phar install $COMPOSER_ARGS
  exitWithMessageOnError "Composer install failed"
  popd
fi
```

Tüm değişikliklerinizi işleyin ve git kullanarak kodunuzu dağıtın veya derleme Otomasyonu etkin olan ZIP dağıtımını yapın. Besteci Dağıtım Otomasyonu 'nun bir parçası olarak çalışıyor olmalıdır.

## <a name="run-gruntbowergulp"></a>Grsıt/Bower/Gulp çalıştırma

App Service, Grsıt, Bower veya Gulp gibi popüler Otomasyon araçlarını dağıtım zamanında çalıştırmak istiyorsanız, [özel bir dağıtım betiği](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script)sağlamanız gerekir. App Service, git ile dağıtırken veya derleme Otomasyonu etkinken [ZIP dağıtımıyla](deploy-zip.md) , bu betiği çalıştırır. 

Bu araçları çalıştırmak üzere deponuzu etkinleştirmek için, bunlarıpackage.jsiçindeki bağımlılıklara eklemeniz gerekir *.* Örneğin:

```json
"dependencies": {
  "bower": "^1.7.9",
  "grunt": "^1.0.1",
  "gulp": "^3.9.1",
  ...
}
```

Yerel bir terminal penceresinden dizini depo kökinizle değiştirin ve aşağıdaki komutları çalıştırın ( [NPM](https://www.npmjs.com/get-npm) yüklü olmalıdır):

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

## <a name="access-environment-variables"></a> Ortam değişkenlerine erişme

App Service, uygulama ayarlarınızı uygulama kodunuzun dışında [ayarlayabilirsiniz](configure-common.md#configure-app-settings) . Ardından, standart [getenv ()](https://secure.php.net/manual/function.getenv.php) modelini kullanarak bunlara erişebilirsiniz. Örneğin, adlı bir uygulama ayarına erişmek için `DB_HOST` aşağıdaki kodu kullanın:

```php
getenv("DB_HOST")
```

## <a name="change-site-root"></a>Site kökünü Değiştir

Seçtiğiniz Web çerçevesi, site kökü olarak bir alt dizin kullanabilir. Örneğin, [Laralevel](https://laravel.com/), site kökü olarak *ortak/* alt dizini kullanır.

Site kökünü özelleştirmek için, komutunu kullanarak uygulamanın sanal uygulama yolunu ayarlayın [`az resource update`](/cli/azure/resource#az-resource-update) . Aşağıdaki örnek, site kökünü deponuzdaki *ortak/* alt dizine ayarlar. 

```azurecli-interactive
az resource update --name web --resource-group <group-name> --namespace Microsoft.Web --resource-type config --parent sites/<app-name> --set properties.virtualApplications[0].physicalPath="site\wwwroot\public" --api-version 2015-06-01
```

Varsayılan olarak, Azure App Service kök sanal uygulama yolunu ( _/_ ) dağıtılan uygulama dosyalarının kök dizinine (_sites\wwwroot_) yönlendirir.

## <a name="detect-https-session"></a>HTTPS oturumunu Algıla

App Service, [SSL sonlandırması](https://wikipedia.org/wiki/TLS_termination_proxy) ağ yükü dengeleyicilerde gerçekleşinceye kadar, tüm https istekleri UYGULAMANıZA şifrelenmemiş HTTP istekleri olarak ulaşacak. Uygulama mantığınızın kullanıcı isteklerinin şifrelenip şifrelenmediğini denetlemesi gerekiyorsa, `X-Forwarded-Proto` üstbilgiyi inceleyin.

```php
if (isset($_SERVER['X-Forwarded-Proto']) && $_SERVER['X-Forwarded-Proto'] === 'https') {
  // Do something when HTTPS is used
}
```

Popüler Web çerçeveleri `X-Forwarded-*` Standart uygulama hiyerarşinizdeki bilgilere erişmenizi sağlar. [Codeigniter](https://codeigniter.com/)'da, [is_https ()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) `X_FORWARDED_PROTO` Varsayılan olarak değerini denetler.

## <a name="customize-phpini-settings"></a>php.ini ayarlarını özelleştirme

PHP yüklemenizde değişiklikler yapmanız gerekiyorsa, bu adımları izleyerek [php.ini yönergelerinden](https://www.php.net/manual/ini.list.php) herhangi birini değiştirebilirsiniz.

> [!NOTE]
> PHP sürümünü ve geçerli *php.ini* yapılandırmasını görmenin en iyi yolu, uygulamanızda [PHPINFO ()](https://php.net/manual/function.phpinfo.php) öğesini çağırmanız.
>

### <a name="customize-non-php_ini_system-directives"></a><a name="Customize-non-PHP_INI_SYSTEM directives"></a>Özelleştirme-PHP_INI_SYSTEM olmayan yönergeler

PHP_INI_USER, PHP_INI_PERDIR ve PHP_INI_ALL yönergelerini özelleştirmek için (bkz. [php.ini yönergeleri](https://www.php.net/manual/ini.list.php)), `.user.ini` uygulamanızın kök dizinine bir dosya ekleyin.

Dosya için `.user.ini` kullandığınız söz dizimini kullanarak yapılandırma ayarlarını dosyaya ekleyin `php.ini` . Örneğin, `display_errors` ayarı etkinleştirmek ve `upload_max_filesize` ayarı 10m olarak ayarlamak isterseniz `.user.ini` dosyanız şu metni içerir:

```
 ; Example Settings
 display_errors=On
 upload_max_filesize=10M

 ; Write errors to d:\home\LogFiles\php_errors.log
 ; log_errors=On
```

Değişiklikleri ile uygulamanızı yeniden dağıtın ve yeniden başlatın.

Bir dosya kullanımına alternatif olarak `.user.ini` , bu PHP_INI_SYSTEM olmayan yönergeleri özelleştirmek için uygulamanızdaki [ini_set ()](https://www.php.net/manual/function.ini-set.php) kullanabilirsiniz.

### <a name="customize-php_ini_system-directives"></a><a name="customize-php_ini_system-directives"></a>PHP_INI_SYSTEM yönergeleri özelleştirme

PHP_INI_SYSTEM yönergeleri özelleştirmek için (bkz. [php.ini yönergeleri](https://www.php.net/manual/ini.list.php)), *. htaccess* yaklaşımını kullanamazsınız. App Service, uygulama ayarını kullanarak ayrı bir mekanizma sağlar `PHP_INI_SCAN_DIR` .

İlk olarak, adlı bir uygulama ayarı eklemek için [Cloud Shell](https://shell.azure.com) aşağıdaki komutu çalıştırın `PHP_INI_SCAN_DIR` :

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PHP_INI_SCAN_DIR="d:\home\site\ini"
```

Kudu konsoluna gidin ( `https://<app-name>.scm.azurewebsites.net/DebugConsole` ) ve adresine gidin `d:\home\site` .

Çağrılan bir dizin oluşturun `d:\home\site` `ini` ve ardından *.ini* `d:\home\site\ini` dizinde (örneğin, *settings.ini)* özelleştirmek istediğiniz yönergeleri kullanarak bir. ini dosyası oluşturun. *php.ini* dosyasında kullanacağınız söz dizimini kullanın. 

Örneğin, [expose_php](https://php.net/manual/ini.core.php#ini.expose-php) değerini değiştirmek için aşağıdaki komutları çalıştırın:

```bash
cd /home/site
mkdir ini
echo "expose_php = Off" >> ini/setting.ini
```

Değişikliklerin etkili olması için uygulamayı yeniden başlatın.

## <a name="enable-php-extensions"></a>PHP uzantılarını etkinleştir

Yerleşik PHP yüklemeleri en yaygın olarak kullanılan uzantıları içerir. [php.ini yönergelerini özelleştirdiğiniz](#customize-php_ini_system-directives)şekilde ek uzantıları etkinleştirebilirsiniz.

> [!NOTE]
> PHP sürümünü ve geçerli *php.ini* yapılandırmasını görmenin en iyi yolu, uygulamanızda [PHPINFO ()](https://php.net/manual/function.phpinfo.php) öğesini çağırmanız.
>

Ek uzantıları etkinleştirmek için aşağıdaki adımları izleyin:

`bin`Uygulamanızın kök dizinine bir dizin ekleyin ve `.so` uzantı dosyalarını içine yerleştirin (örneğin, *MongoDB.so*). Uzantıların Azure 'daki PHP sürümü ile uyumlu olduğundan ve VC9 ve iş parçacığı olmayan güvenli (,) uyumlu olduğundan emin olun.

Değişikliklerinizi dağıtın.

[PHP_INI_SYSTEM yönergelerini özelleştirme](#customize-php_ini_system-directives)bölümündeki adımları uygulayın, uzantıları özel *. ını* dosyasına [uzantı](https://www.php.net/manual/ini.core.php#ini.extension) veya [zend_extension](https://www.php.net/manual/ini.core.php#ini.zend-extension) yönergeleriyle ekleyin.

```
extension=d:\home\site\wwwroot\bin\mongodb.so
zend_extension=d:\home\site\wwwroot\bin\xdebug.so
```

Değişikliklerin etkili olması için uygulamayı yeniden başlatın.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

Tanılama günlüklerinizin Azure App Service görünmesini sağlamak için standart [error_log ()](https://php.net/manual/function.error-log.php) yardımcı programını kullanın.

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

## <a name="troubleshooting"></a>Sorun giderme

Çalışan bir PHP uygulaması App Service veya hata durumunda farklı davrandığı zaman, aşağıdakileri deneyin:

- [Günlük akışına erişin](#access-diagnostic-logs).
- Uygulamayı üretim modunda yerel olarak test edin. App Service uygulamanızı üretim modunda çalıştırır, bu nedenle projenizin üretim modunda yerel olarak beklendiği gibi çalıştığından emin olmanız gerekir. Örneğin:
    - Bazı Web çerçeveleri, statik dosyaları üretim modunda farklı şekilde dağıtabilir.
    - Belirli Web çerçeveleri, üretim modunda çalışırken özel başlatma betikleri kullanabilir.
- Uygulamanızı hata ayıklama modunda App Service çalıştırın. Örneğin, [Laraesin](https://meanjs.org/)içinde, uygulama [ `APP_DEBUG` ayarını `true` olarak ayarlayarak ](configure-common.md#configure-app-settings)uygulamanızı üretim sırasında hata ayıklama iletilerini verecek şekilde yapılandırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: MySQL ile PHP uygulaması](app-service-web-tutorial-php-mysql.md)

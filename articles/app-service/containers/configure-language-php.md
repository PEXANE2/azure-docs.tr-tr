---
title: PHP uygulamalarını yapılandırma
description: Uygulamanız için önceden oluşturulmuş bir PHP kapsayıcısını yapılandırmayı öğrenin. Bu makalede en sık kullanılan yapılandırma görevleri gösterilmektedir.
ms.devlang: php
ms.topic: article
ms.date: 03/28/2019
ms.openlocfilehash: a3de4769193d95a3ef483924c4d65c4fa1cc9f8d
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74671844"
---
# <a name="configure-a-linux-php-app-for-azure-app-service"></a>Azure App Service için bir Linux PHP uygulaması yapılandırma

Bu kılavuzda, Azure App Service 'de Web Apps, mobil arka uçlar ve API uygulamaları için yerleşik PHP çalışma zamanının nasıl yapılandırılacağı gösterilir.

Bu kılavuz, App Service içinde yerleşik bir Linux kapsayıcısı kullanan PHP geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure App Service hiç kullanmadıysanız, önce [MySQL öğreticisiyle](tutorial-php-mysql-app.md) [php hızlı başlangıç](quickstart-php.md) ve php 'yi izleyin.

## <a name="show-php-version"></a>PHP sürümünü göster

Geçerli PHP sürümünü göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Tüm desteklenen PHP sürümlerini göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp list-runtimes --linux | grep PHP
```

## <a name="set-php-version"></a>PHP sürümünü ayarla

PHP sürümünü 7,2 olarak ayarlamak için [Cloud Shell](https://shell.azure.com) aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --linux-fx-version "PHP|7.2"
```

## <a name="run-composer"></a>Besteci Çalıştır

Varsayılan olarak kudu, [besteci](https://getcomposer.org/)çalıştırmaz. Kudu dağıtımı sırasında besteci Otomasyonu 'nu etkinleştirmek için [özel bir dağıtım betiği](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script)sağlamanız gerekir.

Yerel bir terminal penceresinden dizini depo kökinizle değiştirin. *Besteci. phar*'yi indirmek için [komut satırı yükleme adımlarını](https://getcomposer.org/download/) izleyin.

Aşağıdaki komutları çalıştırın:

```bash
npm install kuduscript -g
kuduscript --php --scriptType bash --suppressPrompt
```

Depo kökünde artık *besteci. phar*: *. Deployment* ve *Deploy.sh*'a ek olarak iki yeni dosya vardır. Bu dosyalar, App Service Windows ve Linux türleri için de çalışır.

*Deploy.sh* ' i açın ve `Deployment` bölümünü bulun. Tüm bölümünü aşağıdaki kodla değiştirin:

```bash
##################################################################################################################################
# Deployment
# ----------

echo PHP deployment

# 1. KuduSync
if [[ "$IN_PLACE_DEPLOYMENT" -ne "1" ]]; then
  "$KUDU_SYNC_CMD" -v 50 -f "$DEPLOYMENT_SOURCE" -t "$DEPLOYMENT_TARGET" -n "$NEXT_MANIFEST_PATH" -p "$PREVIOUS_MANIFEST_PATH" -i ".git;.hg;.deployment;deploy.sh"
  exitWithMessageOnError "Kudu Sync failed"
fi

# 3. Initialize Composer Config
initializeDeploymentConfig

# 4. Use composer
echo "$DEPLOYMENT_TARGET"
if [ -e "$DEPLOYMENT_TARGET/composer.json" ]; then
  echo "Found composer.json"
  pushd "$DEPLOYMENT_TARGET"
  php composer.phar install $COMPOSER_ARGS
  exitWithMessageOnError "Composer install failed"
  popd
fi
##################################################################################################################################
```

Tüm değişikliklerinizi işleyin ve kodunuzu yeniden dağıtın. Besteci Dağıtım Otomasyonu 'nun bir parçası olarak çalışıyor olmalıdır.

## <a name="customize-start-up"></a>Başlatmayı özelleştirme

Varsayılan olarak, yerleşik PHP kapsayıcısı Apache sunucusunu çalıştırır. Başlangıçta `apache2ctl -D FOREGROUND"`çalıştırır. İsterseniz, aşağıdaki komutu [Cloud Shell](https://shell.azure.com)çalıştırarak başlangıç sırasında farklı bir komut çalıştırabilirsiniz:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<custom-command>"
```

## <a name="access-environment-variables"></a>Ortam değişkenlerine erişin

App Service, uygulama ayarlarınızı uygulama kodunuzun dışında [ayarlayabilirsiniz](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) . Ardından, standart [getenv ()](https://secure.php.net/manual/function.getenv.php) modelini kullanarak bunlara erişebilirsiniz. Örneğin, `DB_HOST`adlı bir uygulama ayarına erişmek için aşağıdaki kodu kullanın:

```php
getenv("DB_HOST")
```

## <a name="change-site-root"></a>Site kökünü Değiştir

Seçtiğiniz Web çerçevesi, site kökü olarak bir alt dizin kullanabilir. Örneğin, [Laralevel](https://laravel.com/), site kökü olarak `public/` alt dizinini kullanır.

App Service için varsayılan PHP görüntüsü Apache kullanır ve uygulamanız için site kökünü özelleştirmenize izin vermez. Bu sınırlamaya geçici bir çözüm bulmak için, depo köküne aşağıdaki içerikle bir *. htaccess* dosyası ekleyin:

```
<IfModule mod_rewrite.c>
    RewriteEngine on

    RewriteRule ^.*$ /public/$1 [NC,L,QSA]
</IfModule>
```

*.htaccess* yeniden yazma işlemini kullanmazsanız, Laravel uygulamanızı bunun yerine bir [özel Docker görüntüsü](quickstart-docker-go.md) ile dağıtabilirsiniz.

## <a name="detect-https-session"></a>HTTPS oturumunu Algıla

App Service, [SSL sonlandırması](https://wikipedia.org/wiki/TLS_termination_proxy) ağ yükü dengeleyicilerde gerçekleşinceye kadar, tüm https istekleri UYGULAMANıZA şifrelenmemiş HTTP istekleri olarak ulaşacak. Uygulama mantığınızın kullanıcı isteklerinin şifrelenip şifrelenmediğini denetlemesi gerekiyorsa `X-Forwarded-Proto` üst bilgisini inceleyin.

```php
if (isset($_SERVER['X-Forwarded-Proto']) && $_SERVER['X-Forwarded-Proto'] === 'https') {
  // Do something when HTTPS is used
}
```

Popüler Web çerçeveleri, standart uygulama hiyerarşinizdeki `X-Forwarded-*` bilgilerine erişmenizi sağlar. [Codeigniter](https://codeigniter.com/)'da, [is_https ()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) varsayılan olarak `X_FORWARDED_PROTO` değerini denetler.

## <a name="customize-phpini-settings"></a>Php. ini ayarlarını özelleştirme

PHP yüklemenizde değişiklik yapmanız gerekiyorsa, bu adımları izleyerek [php. ini yönergelerinden](https://www.php.net/manual/ini.list.php) herhangi birini değiştirebilirsiniz.

> [!NOTE]
> PHP sürümünü ve geçerli *php. ini* yapılandırmasını görmenin en iyi yolu, uygulamanızda [PHPINFO ()](https://php.net/manual/function.phpinfo.php) öğesini çağırmanız.
>

### <a name="Customize-non-PHP_INI_SYSTEM directives"></a>Özelleştirme-PHP_INI_SYSTEM olmayan yönergeler

PHP_INI_USER, PHP_INI_PERDIR ve PHP_INI_ALL yönergeleri (bkz. [php. ını yönergeleri](https://www.php.net/manual/ini.list.php)) özelleştirmek için uygulamanızın kök dizinine bir *. htaccess* dosyası ekleyin.

*. Htaccess* dosyasında, `php_value <directive-name> <value>` söz dizimini kullanarak yönergeleri ekleyin. Örnek:

```
php_value upload_max_filesize 1000M
php_value post_max_size 2000M
php_value memory_limit 3000M
php_value max_execution_time 180
php_value max_input_time 180
php_value display_errors On
php_value upload_max_filesize 10M
```

Değişiklikleri ile uygulamanızı yeniden dağıtın ve yeniden başlatın. Kudu ile dağıtırsanız (örneğin, [Git](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)kullanarak), dağıtımdan sonra otomatik olarak yeniden başlatılır.

*. Htaccess*kullanımına alternatif olarak, bu PHP_INI_SYSTEM olmayan yönergeleri özelleştirmek için uygulamanızdaki [ini_set ()](https://www.php.net/manual/function.ini-set.php) kullanabilirsiniz.

### <a name="customize-php_ini_system-directives"></a>PHP_INI_SYSTEM yönergeleri özelleştirme

PHP_INI_SYSTEM yönergeleri özelleştirmek için (bkz. [php. ını yönergeleri](https://www.php.net/manual/ini.list.php)), *. htaccess* yaklaşımını kullanamazsınız. App Service, `PHP_INI_SCAN_DIR` uygulama ayarını kullanarak ayrı bir mekanizma sağlar.

İlk olarak, [Cloud Shell](https://shell.azure.com) `PHP_INI_SCAN_DIR`adlı bir uygulama ayarı eklemek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PHP_INI_SCAN_DIR="/usr/local/etc/php/conf.d:/home/site/ini"
```

`/usr/local/etc/php/conf.d`, *php. ini* dosyasının bulunduğu varsayılan dizindir. `/home/site/ini`, özel bir *. ini* dosyası ekleyeceğiniz özel dizindir. Değerleri bir `:`ayırın.

Linux kapsayıcınızda Web SSH oturumuna gidin (`https://<app-name>.scm.azurewebsites.net/webssh/host`).

`ini`adlı `/home/site` bir dizin oluşturun ve ardından `/home/site/ini` dizinde (örneğin, *Settings. ini)* özelleştirmek istediğiniz yönergeleri kullanarak bir *. ini* dosyası oluşturun. *Php. ini* dosyasında kullanacağınız söz dizimini kullanın. 

> [!TIP]
> App Service 'daki yerleşik Linux kapsayıcılarında, */Home* kalıcı paylaşılan depolama alanı olarak kullanılır. 
>

Örneğin, [expose_php](https://php.net/manual/ini.core.php#ini.expose-php) değerini değiştirmek için aşağıdaki komutları çalıştırın:

```bash
cd /home/site
mkdir ini
echo "expose_php = Off" >> ini/setting.ini
```

Değişikliklerin etkili olması için uygulamayı yeniden başlatın.

## <a name="enable-php-extensions"></a>PHP uzantılarını etkinleştir

Yerleşik PHP yüklemeleri en yaygın olarak kullanılan uzantıları içerir. [Php. ini yönergelerini özelleştirdiğiniz](#customize-php_ini_system-directives)şekilde ek uzantıları etkinleştirebilirsiniz.

> [!NOTE]
> PHP sürümünü ve geçerli *php. ini* yapılandırmasını görmenin en iyi yolu, uygulamanızda [PHPINFO ()](https://php.net/manual/function.phpinfo.php) öğesini çağırmanız.
>

Ek uzantıları etkinleştirmek için aşağıdaki adımları izleyin:

Uygulamanızın kök dizinine bir `bin` dizini ekleyin ve `.so` uzantı dosyalarını içine yerleştirin (örneğin, *MongoDB.so*). Uzantıların Azure 'daki PHP sürümü ile uyumlu olduğundan ve VC9 ve iş parçacığı olmayan güvenli (,) uyumlu olduğundan emin olun.

Değişikliklerinizi dağıtın.

[PHP_INI_SYSTEM yönergelerini özelleştirme](#customize-php_ini_system-directives)bölümündeki adımları uygulayın, uzantıları özel *. ını* dosyasına [uzantı](https://www.php.net/manual/ini.core.php#ini.extension) veya [zend_extension](https://www.php.net/manual/ini.core.php#ini.zend-extension) yönergeleriyle ekleyin.

```ini
extension=/home/site/wwwroot/bin/mongodb.so
zend_extension=/home/site/wwwroot/bin/xdebug.so
```

Değişikliklerin etkili olması için uygulamayı yeniden başlatın.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>SSH oturumunu tarayıcıda aç

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Sorun giderme

Çalışan bir PHP uygulaması App Service veya hata durumunda farklı davrandığı zaman, aşağıdakileri deneyin:

- [Günlük akışına erişin](#access-diagnostic-logs).
- Uygulamayı üretim modunda yerel olarak test edin. App Service, Node. js uygulamalarınızı üretim modunda çalıştırır, bu nedenle projenizin üretim modunda yerel olarak beklendiği gibi çalıştığından emin olmanız gerekir. Örnek:
    - *Besteci. JSON*'unuza bağlı olarak, üretim modu (`require` vs. `require-dev`) için farklı paketler yüklenebilir.
    - Bazı Web çerçeveleri, statik dosyaları üretim modunda farklı şekilde dağıtabilir.
    - Belirli Web çerçeveleri, üretim modunda çalışırken özel başlatma betikleri kullanabilir.
- Uygulamanızı hata ayıklama modunda App Service çalıştırın. Örneğin, [Laraesin](https://meanjs.org/)içinde, [`APP_DEBUG` uygulama ayarını `true`olarak ayarlayarak ](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings)uygulamanızı üretim sırasında hata ayıklama iletilerini verecek şekilde yapılandırabilirsiniz.

### <a name="robots933456"></a>robots933456

Kapsayıcı günlüklerinde şu iletiyi görebilirsiniz:

```
2019-04-08T14:07:56.641002476Z "-" - - [08/Apr/2019:14:07:56 +0000] "GET /robots933456.txt HTTP/1.1" 404 415 "-" "-"
```

Bu iletiyi güvenle yoksayabilirsiniz. `/robots933456.txt`, kapsayıcının istek sunma yeteneğine sahip olup olmadığını denetlemek için App Service tarafından kullanılan bir kukla URL yoludur. 404 yanıtı, yolun mevcut olmadığını gösterir, ancak kapsayıcının sağlıklı olduğunu ve isteklere yanıt vermeye hazırlandığını App Service sağlar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: MySQL ile PHP uygulaması](tutorial-php-mysql-app.md)

> [!div class="nextstepaction"]
> [App Service Linux SSS](app-service-linux-faq.md)

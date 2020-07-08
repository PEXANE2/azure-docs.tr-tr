---
title: PHP uygulamalarını yapılandırma
description: Uygulamanız için önceden oluşturulmuş bir PHP kapsayıcısını yapılandırmayı öğrenin. Bu makalede en sık kullanılan yapılandırma görevleri gösterilmektedir.
ms.devlang: php
ms.topic: article
ms.date: 03/28/2019
ms.openlocfilehash: 9e4237f1eecb9f6542aac946525ff4583e478c2e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84905706"
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

## <a name="customize-build-automation"></a>Derleme Otomasyonu 'nu özelleştirme

Uygulamanızı, derleme Otomasyonu açıkken git veya ZIP paketleri kullanarak dağıtırsanız, App Service aşağıdaki sırayla Otomasyon adımları oluşturun:

1. Tarafından belirtilmişse özel betiği çalıştırın `PRE_BUILD_SCRIPT_PATH` .
1. `php composer.phar install` öğesini çalıştırın.
1. Tarafından belirtilmişse özel betiği çalıştırın `POST_BUILD_SCRIPT_PATH` .

`PRE_BUILD_COMMAND`ve `POST_BUILD_COMMAND` Varsayılan olarak boş olan ortam değişkenleridir. Oluşturma öncesi komutları çalıştırmak için, tanımlayın `PRE_BUILD_COMMAND` . Oluşturma sonrası komutları çalıştırmak için, tanımlayın `POST_BUILD_COMMAND` .

Aşağıdaki örnek, virgülle ayrılmış bir dizi komuta iki değişkeni belirtir.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PRE_BUILD_COMMAND="echo foo, scripts/prebuild.sh"
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings POST_BUILD_COMMAND="echo foo, scripts/postbuild.sh"
```

Derleme Otomasyonu 'nu özelleştirmek için ek ortam değişkenleri için bkz. [Oryx Configuration](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md).

App Service nasıl çalıştığı ve Linux 'ta PHP uygulamaları oluşturma hakkında daha fazla bilgi için bkz. [Oryx belgeleri: PHP uygulamaları nasıl algılanır ve oluşturulur](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/php.md).

## <a name="customize-start-up"></a>Başlatmayı özelleştirme

Varsayılan olarak, yerleşik PHP kapsayıcısı Apache sunucusunu çalıştırır. Başlangıçta çalışır `apache2ctl -D FOREGROUND"` . İsterseniz, aşağıdaki komutu [Cloud Shell](https://shell.azure.com)çalıştırarak başlangıç sırasında farklı bir komut çalıştırabilirsiniz:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<custom-command>"
```

## <a name="access-environment-variables"></a> Ortam değişkenlerine erişme

App Service, uygulama ayarlarınızı uygulama kodunuzun dışında [ayarlayabilirsiniz](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) . Ardından, standart [getenv ()](https://secure.php.net/manual/function.getenv.php) modelini kullanarak bunlara erişebilirsiniz. Örneğin, adlı bir uygulama ayarına erişmek için `DB_HOST` aşağıdaki kodu kullanın:

```php
getenv("DB_HOST")
```

## <a name="change-site-root"></a>Site kökünü Değiştir

Seçtiğiniz Web çerçevesi, site kökü olarak bir alt dizin kullanabilir. Örneğin, [Laralevel](https://laravel.com/), `public/` alt dizini site kökü olarak kullanır.

App Service için varsayılan PHP görüntüsü Apache kullanır ve uygulamanız için site kökünü özelleştirmenize izin vermez. Bu sınırlamaya geçici bir çözüm bulmak için, depo köküne aşağıdaki içerikle bir *. htaccess* dosyası ekleyin:

```
<IfModule mod_rewrite.c>
    RewriteEngine on
    RewriteCond %{REQUEST_URI} ^/$
    RewriteRule ^(.*)$ /public/$1 [NC,L,QSA]
</IfModule>
```

*.htaccess* yeniden yazma işlemini kullanmazsanız, Laravel uygulamanızı bunun yerine bir [özel Docker görüntüsü](quickstart-docker-go.md) ile dağıtabilirsiniz.

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

PHP_INI_USER, PHP_INI_PERDIR ve PHP_INI_ALL yönergelerini özelleştirmek için (bkz. [php.ini yönergeleri](https://www.php.net/manual/ini.list.php)), uygulamanızın kök dizinine bir *. htaccess* dosyası ekleyin.

*. Htaccess* dosyasında, söz dizimini kullanarak yönergeleri ekleyin `php_value <directive-name> <value>` . Örneğin:

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

### <a name="customize-php_ini_system-directives"></a><a name="customize-php_ini_system-directives"></a>PHP_INI_SYSTEM yönergeleri özelleştirme

PHP_INI_SYSTEM yönergeleri özelleştirmek için (bkz. [php.ini yönergeleri](https://www.php.net/manual/ini.list.php)), *. htaccess* yaklaşımını kullanamazsınız. App Service, uygulama ayarını kullanarak ayrı bir mekanizma sağlar `PHP_INI_SCAN_DIR` .

İlk olarak, adlı bir uygulama ayarı eklemek için [Cloud Shell](https://shell.azure.com) aşağıdaki komutu çalıştırın `PHP_INI_SCAN_DIR` :

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PHP_INI_SCAN_DIR="/usr/local/etc/php/conf.d:/home/site/ini"
```

`/usr/local/etc/php/conf.d`, *php.ini* var olan varsayılan dizindir. `/home/site/ini`, özel bir *. ini* dosyası ekleyeceğiniz özel dizindir. Değerleri bir ile ayırın `:` .

Linux kapsayıcınızda () Web SSH oturumuna gidin `https://<app-name>.scm.azurewebsites.net/webssh/host` .

Çağrılan bir dizin oluşturun `/home/site` `ini` ve ardından *.ini* `/home/site/ini` dizinde (örneğin, *settings.ini)* özelleştirmek istediğiniz yönergeleri kullanarak bir. ini dosyası oluşturun. *php.ini* dosyasında kullanacağınız söz dizimini kullanın. 

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

Yerleşik PHP yüklemeleri en yaygın olarak kullanılan uzantıları içerir. [php.ini yönergelerini özelleştirdiğiniz](#customize-php_ini_system-directives)şekilde ek uzantıları etkinleştirebilirsiniz.

> [!NOTE]
> PHP sürümünü ve geçerli *php.ini* yapılandırmasını görmenin en iyi yolu, uygulamanızda [PHPINFO ()](https://php.net/manual/function.phpinfo.php) öğesini çağırmanız.
>

Ek uzantıları etkinleştirmek için aşağıdaki adımları izleyin:

`bin`Uygulamanızın kök dizinine bir dizin ekleyin ve `.so` uzantı dosyalarını içine yerleştirin (örneğin, *MongoDB.so*). Uzantıların Azure 'daki PHP sürümü ile uyumlu olduğundan ve VC9 ve iş parçacığı olmayan güvenli (,) uyumlu olduğundan emin olun.

Değişikliklerinizi dağıtın.

[PHP_INI_SYSTEM yönergelerini özelleştirme](#customize-php_ini_system-directives)bölümündeki adımları uygulayın, uzantıları özel *. ını* dosyasına [uzantı](https://www.php.net/manual/ini.core.php#ini.extension) veya [zend_extension](https://www.php.net/manual/ini.core.php#ini.zend-extension) yönergeleriyle ekleyin.

```ini
extension=/home/site/wwwroot/bin/mongodb.so
zend_extension=/home/site/wwwroot/bin/xdebug.so
```

Değişikliklerin etkili olması için uygulamayı yeniden başlatın.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-linux-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>SSH oturumunu tarayıcıda aç

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Sorun giderme

Çalışan bir PHP uygulaması App Service veya hata durumunda farklı davrandığı zaman, aşağıdakileri deneyin:

- [Günlük akışına erişin](#access-diagnostic-logs).
- Uygulamayı üretim modunda yerel olarak test edin. App Service uygulamanızı üretim modunda çalıştırır, bu nedenle projenizin üretim modunda yerel olarak beklendiği gibi çalıştığından emin olmanız gerekir. Örneğin:
    - *composer.js*bağlı olarak, üretim modu ( `require` vs.) için farklı paketler yüklenebilir `require-dev` .
    - Bazı Web çerçeveleri, statik dosyaları üretim modunda farklı şekilde dağıtabilir.
    - Belirli Web çerçeveleri, üretim modunda çalışırken özel başlatma betikleri kullanabilir.
- Uygulamanızı hata ayıklama modunda App Service çalıştırın. Örneğin, [Laraesin](https://meanjs.org/)içinde, uygulama [ `APP_DEBUG` ayarını `true` olarak ayarlayarak ](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings)uygulamanızı üretim sırasında hata ayıklama iletilerini verecek şekilde yapılandırabilirsiniz.

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: MySQL ile PHP uygulaması](tutorial-php-mysql-app.md)

> [!div class="nextstepaction"]
> [App Service Linux SSS](app-service-linux-faq.md)

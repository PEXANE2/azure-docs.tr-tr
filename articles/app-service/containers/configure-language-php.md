---
title: PHP uygulamalarını yapılandırma
description: Uygulamanız için önceden oluşturulmuş bir PHP kapsayıcısını nasıl yapılandırabilirsiniz öğrenin. Bu makalede, en yaygın yapılandırma görevleri gösterir.
ms.devlang: php
ms.topic: article
ms.date: 03/28/2019
ms.openlocfilehash: ad121d605e521704597471b446fa79cb43dfccc7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255845"
---
# <a name="configure-a-linux-php-app-for-azure-app-service"></a>Azure Uygulama Hizmeti için bir Linux PHP uygulamasını yapılandırma

Bu kılavuz, Azure Uygulama Hizmeti'nde web uygulamaları, mobil arka uçlar ve API uygulamaları için yerleşik PHP çalışma süresini nasıl yapılandırabileceğinizi gösterir.

Bu kılavuz, App Service'de yerleşik bir Linux kapsayıcısı kullanan PHP geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure Uygulama Hizmeti'ni hiç kullanmadıysanız, önce [MySQL öğreticisiyle](tutorial-php-mysql-app.md) [PHP quickstart](quickstart-php.md) ve PHP'yi takip edin.

## <a name="show-php-version"></a>PHP sürümünü göster

Geçerli PHP sürümünü göstermek için [Bulut Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Desteklenen tüm PHP sürümlerini göstermek için [Bulut Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp list-runtimes --linux | grep PHP
```

## <a name="set-php-version"></a>PHP sürümünü ayarlama

PHP sürümünü 7.2 olarak ayarlamak için [Bulut Kabuğu'nda](https://shell.azure.com) aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --name <app-name> --resource-group <resource-group-name> --linux-fx-version "PHP|7.2"
```

## <a name="customize-build-automation"></a>Yapı otomasyonu özelleştirme

Uygulamanızı Git veya zip paketlerini yapı otomasyonu açık ken kullanırsanız, Uygulama Hizmeti aşağıdaki sırayla otomasyon adımları oluşturur:

1. 'ye göre belirtilirse özel komut dosyası çalıştırın `PRE_BUILD_SCRIPT_PATH`
1. `php composer.phar install` öğesini çalıştırın.
1. 'ye göre belirtilirse özel komut dosyası çalıştırın `POST_BUILD_SCRIPT_PATH`

`PRE_BUILD_COMMAND`ve `POST_BUILD_COMMAND` varsayılan olarak boş olan ortam değişkenleridir. Önceden yapı komutları çalıştırmak `PRE_BUILD_COMMAND`için tanımlayın. Yapı sonrası komutları çalıştırmak `POST_BUILD_COMMAND`için.

Aşağıdaki örnekte, iki değişken virgülle ayrılmış bir dizi komuta göre belirtilir.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PRE_BUILD_COMMAND="echo foo, scripts/prebuild.sh"
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings POST_BUILD_COMMAND="echo foo, scripts/postbuild.sh"
```

Yapı otomasyonu özelleştirmek için ek ortam değişkenleri için [Bkz. Oryx yapılandırması.](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md)

Uygulama Hizmeti'nin Linux'ta PHP uygulamalarını nasıl çalıştırıp oluşturduğu hakkında daha fazla bilgi için [Oryx belgelerine bakın: PHP uygulamaları nasıl algılanır ve oluşturulur.](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/php.md)

## <a name="customize-start-up"></a>Başlatmayı özelleştirme

Varsayılan olarak, yerleşik PHP kapsayıcısı Apache sunucusunu çalıştırZ. Başlangıçta, çalışır. `apache2ctl -D FOREGROUND"` İsterseniz, [Bulut Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırarak başlangıç sırasında farklı bir komut çalıştırabilirsiniz:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<custom-command>"
```

## <a name="access-environment-variables"></a> Ortam değişkenlerine erişme

Uygulama Hizmeti'nde, [uygulama ayarlarınızı](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) uygulama kodunuzu dışında ayarlayabilirsiniz. Daha sonra standart [getenv()](https://secure.php.net/manual/function.getenv.php) deseni kullanarak onlara erişebilirsiniz. Örneğin, adlı `DB_HOST`bir uygulama ayarına erişmek için aşağıdaki kodu kullanın:

```php
getenv("DB_HOST")
```

## <a name="change-site-root"></a>Site kökünü değiştirme

Seçtiğiniz web çerçevesi site kökü olarak bir alt dizini kullanabilirsiniz. Örneğin, [Laravel,](https://laravel.com/)site `public/` kökü olarak alt dizini kullanır.

App Service için varsayılan PHP görüntüsü Apache kullanır ve uygulamanız için site kökünü özelleştirmenize izin vermez. Bu sınırlamayı aşmak için, aşağıdaki içerikle birlikte depo kökünüze bir *.htaccess* dosyası ekleyin:

```
<IfModule mod_rewrite.c>
    RewriteEngine on

    RewriteRule ^.*$ /public/$1 [NC,L,QSA]
</IfModule>
```

*.htaccess* yeniden yazma işlemini kullanmazsanız, Laravel uygulamanızı bunun yerine bir [özel Docker görüntüsü](quickstart-docker-go.md) ile dağıtabilirsiniz.

## <a name="detect-https-session"></a>HTTPS oturumunu algıla

App Service'te, [SSL sonlandırma](https://wikipedia.org/wiki/TLS_termination_proxy) ağı yük dengeleyicilerinde gerçekleşir, böylece tüm HTTPS istekleri uygulamanıza şifrelenmemiş HTTP istekleri olarak ulaşır. Uygulama mantığınızın kullanıcı isteklerinin şifreli olup olmadığını denetlemesi gerekiyorsa, üstbilgide inceleme. `X-Forwarded-Proto`

```php
if (isset($_SERVER['X-Forwarded-Proto']) && $_SERVER['X-Forwarded-Proto'] === 'https') {
  // Do something when HTTPS is used
}
```

Popüler web çerçeveleri, `X-Forwarded-*` standart uygulama deseninizdeki bilgilere erişmenizi sağlar. [CodeIgniter'da](https://codeigniter.com/) [is_https()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) varsayılan olarak `X_FORWARDED_PROTO` değerini denetler.

## <a name="customize-phpini-settings"></a>php.ini ayarlarını özelleştirin

PHP yüklemenizde değişiklik yapmanız gerekiyorsa, aşağıdaki adımları izleyerek [php.ini yönergelerinden](https://www.php.net/manual/ini.list.php) herhangi birini değiştirebilirsiniz.

> [!NOTE]
> PHP sürümünü ve geçerli *php.ini* yapılandırmasını görmenin en iyi yolu, uygulamanızda [phpinfo()](https://php.net/manual/function.phpinfo.php) numaralı telefonu aramaktır.
>

### <a name="customize-non-php_ini_system-directives"></a><a name="Customize-non-PHP_INI_SYSTEM directives"></a>PHP_INI_SYSTEM olmayan yönergeleri özelleştirme

PHP_INI_USER, PHP_INI_PERDIR ve PHP_INI_ALL yönergelerini özelleştirmek için [(bkz. php.ini yönergeleri),](https://www.php.net/manual/ini.list.php)uygulamanızın kök dizinine *bir .htaccess* dosyası ekleyin.

*.htaccess* dosyasında `php_value <directive-name> <value>` sözdizimini kullanarak yönergeleri ekleyin. Örnek:

```
php_value upload_max_filesize 1000M
php_value post_max_size 2000M
php_value memory_limit 3000M
php_value max_execution_time 180
php_value max_input_time 180
php_value display_errors On
php_value upload_max_filesize 10M
```

Uygulamanızı değişikliklerle yeniden dağıtın ve yeniden başlatın. Kudu ile dağıtırsanız (örneğin, [Git'i](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)kullanarak), dağıtımdan sonra otomatik olarak yeniden başlatılır.

*.htaccess'i*kullanmaya alternatif olarak, bu PHP_INI_SYSTEM olmayan yönergeleri özelleştirmek için uygulamanızda [ini_set()](https://www.php.net/manual/function.ini-set.php) kullanabilirsiniz.

### <a name="customize-php_ini_system-directives"></a><a name="customize-php_ini_system-directives"></a>PHP_INI_SYSTEM yönergelerini özelleştirme

PHP_INI_SYSTEM yönergelerini özelleştirmek için [(bkz. php.ini yönergeleri),](https://www.php.net/manual/ini.list.php) *.htaccess* yaklaşımını kullanamazsınız. Uygulama Hizmeti, uygulama ayarını `PHP_INI_SCAN_DIR` kullanarak ayrı bir mekanizma sağlar.

İlk olarak, aşağıdaki komutu [Bulut Kabuğu'nda](https://shell.azure.com) `PHP_INI_SCAN_DIR`çalıştırarak aşağıdaki bir uygulama ayarı ekleyin:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PHP_INI_SCAN_DIR="/usr/local/etc/php/conf.d:/home/site/ini"
```

`/usr/local/etc/php/conf.d`*php.ini'nin* bulunduğu varsayılan dizindir. `/home/site/ini`özel bir *.ini* dosyası ekleyeceğiniz özel dizinidir. Değerleri bir `:`. ile ayırırsınız

Linux kapsayıcınızla web SSH oturumuna`https://<app-name>.scm.azurewebsites.net/webssh/host`gidin ( ).

`/home/site` Adlı `ini`bir dizin oluşturun, ardından özelleştirmek istediğiniz `/home/site/ini` yönergeleri içeren dizinde (örneğin, *settings.ini)* bir *.ini* dosyası oluşturun. *Php.ini* dosyasında kullanacağınız sözdizimini kullanın. 

> [!TIP]
> App Service'deki yerleşik Linux kaplarında, */home* kalıcı paylaşılan depolama alanı olarak kullanılır. 
>

Örneğin, aşağıdaki komutları çalıştırmak [expose_php](https://php.net/manual/ini.core.php#ini.expose-php) değerini değiştirmek için:

```bash
cd /home/site
mkdir ini
echo "expose_php = Off" >> ini/setting.ini
```

Değişikliklerin etkili olması için uygulamayı yeniden başlatın.

## <a name="enable-php-extensions"></a>PHP uzantılarını etkinleştirme

Yerleşik PHP yüklemeleri en sık kullanılan uzantıları içerir. [Php.ini yönergelerini özelleştirdiğiniz](#customize-php_ini_system-directives)gibi ek uzantıları da etkinleştirebilirsiniz.

> [!NOTE]
> PHP sürümünü ve geçerli *php.ini* yapılandırmasını görmenin en iyi yolu, uygulamanızda [phpinfo()](https://php.net/manual/function.phpinfo.php) numaralı telefonu aramaktır.
>

Aşağıdaki adımları izleyerek ek uzantıları etkinleştirmek için:

Uygulamanızın `bin` kök dizinine bir dizin ekleyin `.so` ve uzantı dosyalarını içine koyun (örneğin, *mongodb.so).* Uzantıların Azure'daki PHP sürümüyle uyumlu olduğundan ve VC9 ve iş parçacığı için güvenli olmayan (nts) uyumlu olduğundan emin olun.

Değişikliklerinizi dağıtın.

[PHP_INI_SYSTEM yönergelerini özelleştir'deki](#customize-php_ini_system-directives)adımları izleyin, [uzantıları uzantı](https://www.php.net/manual/ini.core.php#ini.extension) veya [zend_extension](https://www.php.net/manual/ini.core.php#ini.zend-extension) yönergeleriyle özel *.ini* dosyasına ekleyin.

```ini
extension=/home/site/wwwroot/bin/mongodb.so
zend_extension=/home/site/wwwroot/bin/xdebug.so
```

Değişikliklerin etkili olması için uygulamayı yeniden başlatın.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda SSH oturumunu açma

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Sorun giderme

Çalışan bir PHP uygulaması Uygulama Hizmeti'nde farklı davrantığında veya hataları varsa, aşağıdakileri deneyin:

- [Günlük akışına erişin.](#access-diagnostic-logs)
- Uygulamayı üretim modunda yerel olarak test edin. Uygulama Hizmeti, Node.js uygulamalarınızı üretim modunda çalıştırır, bu nedenle projenizin yerel üretim modunda beklendiği gibi çalıştığından emin olmanız gerekir. Örnek:
    - *Besteci.json*bağlı olarak, farklı paketler üretim modu için`require` yüklenebilir (vs). `require-dev`
    - Bazı web çerçeveleri statik dosyaları üretim modunda farklı şekilde dağıtabilir.
    - Bazı web çerçeveleri üretim modunda çalışırken özel başlangıç komut dosyaları kullanabilir.
- Uygulamanızı Hata Ayıklama modunda Uygulama Hizmeti'nde çalıştırın. Örneğin, [Laravel'de,](https://meanjs.org/) [ `APP_DEBUG` uygulama ayarını `true`'da ayarlayarak ](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings)uygulamanızı üretimdeki çıktı hata ayıklama iletileri için yapılandırabilirsiniz.

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: MySQL ile PHP uygulaması](tutorial-php-mysql-app.md)

> [!div class="nextstepaction"]
> [Uygulama Hizmeti Linux SSS](app-service-linux-faq.md)

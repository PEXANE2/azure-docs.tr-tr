---
title: PHP çalışma zamanını yapılandırma
description: Varsayılan PHP yüklemesini nasıl yapılandıracak veya Azure Uygulama Hizmeti için özel bir PHP yüklemesi eklemeyi öğrenin.
author: msangapu-msft
ms.assetid: 95c4072b-8570-496b-9c48-ee21a223fb60
ms.devlang: php
ms.topic: article
ms.date: 04/11/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: c73fb55e485d0c92d27eac2ac197a81337b9d5e1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77016808"
---
# <a name="configure-php-in-azure-app-service"></a>Azure Uygulama Hizmetinde PHP'yi yapılandırma

## <a name="introduction"></a>Giriş

Bu kılavuz, [Azure Uygulama Hizmeti'ndeki](https://go.microsoft.com/fwlink/?LinkId=529714)web uygulamaları, mobil arka uçlar ve API uygulamaları için yerleşik PHP çalışma süresini nasıl yapılandırabileceğinizi, özel bir PHP çalışma süresi sağladığınız ve uzantıları etkinleştirmenizi gösterir. Uygulama Hizmetini kullanmak için [ücretsiz deneme sürümüne]kaydolun. Bu kılavuzdan en iyi şekilde haberdar olmak için öncelikle App Service'de bir PHP uygulaması oluşturmanız gerekir.

## <a name="how-to-change-the-built-in-php-version"></a>Nasıl yapılır: Yerleşik PHP sürümünü değiştirme

Varsayılan olarak, PHP 5.6 yüklenir ve bir Uygulama Hizmeti uygulaması oluşturduğunuzda hemen kullanılabilir. Kullanılabilir sürüm revizyonunu, varsayılan yapılandırmasını ve etkin uzantıları görmenin en iyi yolu [phpinfo()] işlevini çağıran bir komut dosyası dağıtmaktır.

PHP 7.0 ve PHP 7.2 sürümleri de mevcuttur, ancak varsayılan olarak etkinleştirilir. PHP sürümünü güncelleştirmek için aşağıdaki yöntemlerden birini izleyin:

### <a name="azure-portal"></a>Azure portalında

1. [Azure portalında](https://portal.azure.com) uygulamanıza göz atın ve **Yapılandırma** sayfasına gidin.

2. **Yapılandırma'dan** **Genel Ayarlar'ı** seçin ve yeni PHP sürümünü seçin.

3. **Genel ayarlar** bıçağının üst kısmındaki **Kaydet** düğmesini tıklatın.

### <a name="azure-cli"></a>Azure CLI 

Azure Komut Satırı Arabirimini kullanmak için [Bilgisayarınıza Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) yüklemeniz gerekir.

1. Terminal'i açın ve hesabınıza giriş yapın.

        az login

1. Desteklenen çalışma sürelerinin listesini görmek için denetleyin.

        az webapp list-runtimes | grep php

2. Uygulama için PHP sürümünü ayarlayın.

        az webapp config set --php-version {5.6 | 7.0 | 7.1 | 7.2} --name {app-name} --resource-group {resource-group-name}

3. PHP sürümü artık ayarlandı. Bu ayarları onaylayabilirsiniz:

        az webapp show --name {app-name} --resource-group {resource-group-name}

## <a name="how-to-change-the-built-in-php-configurations"></a>Nasıl yapılır: Yerleşik PHP yapılandırmalarını değiştirme

Yerleşik PHP çalışma zamanı için, aşağıdaki adımları izleyerek yapılandırma seçeneklerinden herhangi birini değiştirebilirsiniz. (php.ini yönergeleri hakkında bilgi için [php.ini direktifleri listesine]bakın.)

### <a name="changing-php_ini_user-php_ini_perdir-php_ini_all-configuration-settings"></a>PHP\_INI\_KULLANICISI,\_PHP\_INI\_\_PERDIR, PHP INI TÜM yapılandırma ayarlarını değiştirme

1. Kök dizininize bir [.user.ini] dosyası ekleyin.
1. Bir `php.ini` dosyada `.user.ini` kullanacağınız sözdizimini kullanarak dosyaya yapılandırma ayarları ekleyin. Örneğin, `display_errors` ayarı açıp ayarı 10M olarak ayarlamak `upload_max_filesize` istiyorsanız, dosyanız `.user.ini` bu metni içerir:

        ; Example Settings
        display_errors=On
        upload_max_filesize=10M

        ; OPTIONAL: Turn this on to write errors to d:\home\LogFiles\php_errors.log
        ; log_errors=On
2. Uygulamanızı dağıtın.
3. Uygulamayı yeniden başlatın. (PHP'nin dosyaları okuma `.user.ini` sıklığı sistem düzeyi ayarı `user_ini.cache_ttl` olan ve varsayılan olarak 300 saniye (5 dakika) olan ayarına göre yönetildiği için yeniden başlatma gereklidir. Uygulamayı yeniden başlatmak PHP'yi `.user.ini` dosyadaki yeni ayarları okumaya zorlar.)

`.user.ini` Dosya kullanmaya alternatif olarak, sistem düzeyinde yönergeolmayan yapılandırma seçeneklerini ayarlamak için komut dosyalarındaki [ini_set()] işlevini kullanabilirsiniz.

### <a name="changing-php_ini_system-configuration-settings"></a>PHP\_INI\_SYSTEM yapılandırma ayarlarını değiştirme

1. Uygulamanıza anahtar `PHP_INI_SCAN_DIR` ve değerle Uygulama Ayarı ekleme`d:\home\site\ini`
1. Dizinde `settings.ini` Kudu&lt;Console (http:// site&gt;adı .scm.azurewebsite.net) kullanarak bir dosya oluşturun. `d:\home\site\ini`
1. Bir `php.ini` dosyada `settings.ini` kullanacağınız sözdizimini kullanarak dosyaya yapılandırma ayarları ekleyin. Örneğin, `curl.cainfo` ayarı bir `*.crt` dosyaya doğrultmak ve 'wincache.maxfilesize' ayarını 512K olarak ayarlamak istiyorsanız, dosyanız `settings.ini` bu metni içerir:

        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
1. Değişiklikleri yeniden yüklemek için uygulamanızı yeniden başlatın.

## <a name="how-to-enable-extensions-in-the-default-php-runtime"></a>Nasıl yapılır: Varsayılan PHP çalışma zamanında uzantıları etkinleştirme

Önceki bölümde belirtildiği gibi, varsayılan PHP sürümünü, varsayılan yapılandırmasını ve etkin uzantıları görmenin en iyi yolu [phpinfo()]çağıran bir komut dosyası dağıtmaktır. Aşağıdaki adımları izleyerek ek uzantıları etkinleştirmek için:

### <a name="configure-via-ini-settings"></a>ini ayarları ile yapılandırma

1. `d:\home\site` Dizine bir `ext` dizin ekleyin.
1. Uzantı dosyalarını `ext` dizine koyun `.dll` `php_xdebug.dll`(örneğin, ). Uzantıların PHP'nin varsayılan sürümüyle uyumlu olduğundan ve VC9 ve iş parçacığı için güvenli olmayan (nts) uyumlu olduğundan emin olun.
1. Uygulamanıza anahtar `PHP_INI_SCAN_DIR` ve değerle Uygulama Ayarı ekleme`d:\home\site\ini`
1. 'de `ini` bir `extensions.ini`dosya oluşturun. `d:\home\site\ini`
1. Bir `php.ini` dosyada `extensions.ini` kullanacağınız sözdizimini kullanarak dosyaya yapılandırma ayarları ekleyin. Örneğin, MongoDB ve XDebug uzantılarını etkinleştirmek istiyorsanız, dosyanız `extensions.ini` bu metni içerir:

        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
1. Değişiklikleri yüklemek için uygulamanızı yeniden başlatın.

### <a name="configure-via-app-setting"></a>Uygulama Ayarı ile yapılandırma

1. Kök `bin` dizine bir dizin ekleyin.
2. Uzantı dosyalarını `bin` dizine koyun `.dll` `php_xdebug.dll`(örneğin, ). Uzantıların PHP'nin varsayılan sürümüyle uyumlu olduğundan ve VC9 ve iş parçacığı için güvenli olmayan (nts) uyumlu olduğundan emin olun.
3. Uygulamanızı dağıtın.
4. Azure portalında uygulamanıza göz atın ve **Ayarlar** bölümünün altında bulunan **Yapılandırma'ya** tıklayın.
5. **Yapılandırma** bıçağından **Uygulama Ayarları'nı**seçin.
6. Uygulama **ayarları** bölümünde + **Yeni uygulama ayarını** tıklatın ve **PHP_EXTENSIONS** bir anahtar oluşturun. Bu anahtarın değeri web sitesi köküne göre bir yol olacaktır: **bin\your-ext-file**.
7. Alttaki **Güncelleştir** düğmesini tıklatın ve **ardından Uygulama ayarları** sekmesinin üzerine **Kaydet'i** tıklatın.

Zend uzantıları da **PHP_ZENDEXTENSIONS** bir anahtar kullanılarak desteklenir. Birden çok uzantıyı etkinleştirmek için, `.dll` uygulama ayar değeri için virgülle ayrılmış bir dosya listesi ekleyin.

## <a name="how-to-use-a-custom-php-runtime"></a>Nasıl yapılır: Özel bir PHP çalışma zamanı kullanın

Varsayılan PHP çalışma zamanı yerine, Uygulama Hizmeti PHP komut dosyalarını çalıştırmak için sağladığınız bir PHP çalışma süresi kullanabilir. Sağladığınız çalışma süresi, sizin de `php.ini` sağladığınız bir dosya tarafından yapılandırılabilir. Aşağıdaki adımları izleyerek, App Service ile özel bir PHP çalışma zamanı kullanmak için.

1. Windows için PHP'nin iş parçacığı güvenli olmayan, VC9 veya VC11 uyumlu sürümünü edinin. Windows için PHP son sürümleri burada [https://windows.php.net/download/]bulabilirsiniz: . Eski sürümler burada arşivbulunabilir: [https://windows.php.net/downloads/releases/archives/].
2. Çalışma `php.ini` süreniz için dosyayı değiştirin. Yalnızca sistem düzeyindeki yönergeler olan tüm yapılandırma ayarları App Service tarafından yoksayılır. (Yalnızca sistem düzeyindeki yönergeler hakkında bilgi için [bkz. php.ini yönergeleri listesi).]
3. İsteğe bağlı olarak, PHP çalışma sürenize uzantılar ekleyin ve bunları dosyada etkinleştirin. `php.ini`
4. Kök `bin` dizininize bir dizin ekleyin ve PHP çalışma sürenizi içeren dizini (örneğin, `bin\php`) koyuna koyun.
5. Uygulamanızı dağıtın.
6. Azure portalında uygulamanıza göz atın ve **Yapılandırma** bıçağına tıklayın.
8. **Yapılandırma** bıçağından **Yol eşlemelerini**seçin. 
9. **+ Yeni İşleyici'yi** tıklatın ve Uzantı alanına ekleyin `*.php` ve Komut Dosyası `php-cgi.exe` **işlemcisinde**çalıştırılabilir yol ekleyin. PHP çalışma zamanınızı uygulamanızın `bin` kökündeki dizine koyarsanız, yol `D:\home\site\wwwroot\bin\php\php-cgi.exe`.
10. Altta, işleyici eşleci eklemeyi bitirmek için **Güncelleştir'i** tıklatın.
11. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a>Nasıl yapilir: Azure'da Besteci otomasyonuna olanak sağlama

Varsayılan olarak, PHP projenizde bir tane varsa, App Service composer.json ile hiçbir şey yapmaz. [Git dağıtımını](deploy-local-git.md)kullanırsanız, Besteci uzantısını `git push` etkinleştirerek composer.json işlemini etkinleştirebilirsiniz.

> [!NOTE]
> Burada [App Service birinci sınıf Besteci desteği için oy](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)verebilirsiniz!
>

1. [Azure portalındaki](https://portal.azure.com)PHP uygulamanızın bıçağında **Araçlar** > **Uzantıları'nı**tıklatın.

    ![Azure'da Besteci otomasyonuna olanak sağlamak için Azure portalı ayarları](./media/web-sites-php-configure/composer-extension-settings.png)
2. **Ekle'yi**tıklatın, ardından **Besteci'yi**tıklatın.

    ![Azure'da Besteci otomasyonuna olanak sağlamak için Besteci uzantısı ekleme](./media/web-sites-php-configure/composer-extension-add.png)
3. Yasal koşulları kabul etmek için **Tamam'ı** tıklatın. Uzantıyı eklemek için yeniden **Tamam'ı** tıklatın.

    **Yüklü uzantıları** bıçak Besteci uzantısını gösterir.
    ![Azure'da Besteci otomasyonuna olanak sağlamak için yasal koşulları kabul etme](./media/web-sites-php-configure/composer-extension-view.png)
4. Şimdi, yerel makinenizdeki bir terminal `git add`penceresinde, `git push` uygulamanıza ve `git commit`uygulamanıza gerçekleştirin. Besteci'nin composer.json'da tanımlanan bağımlılıkları yüklediğini unutmayın.

    ![Azure'da Besteci otomasyonu ile Git dağıtımı](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [PHP Geliştirici Merkezi'ne](https://azure.microsoft.com/develop/php/)bakın.

[ücretsiz deneme]: https://www.windowsazure.com/pricing/free-trial/
[phpinfo()]: https://php.net/manual/en/function.phpinfo.php
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
[php.ini direktifleri listesi]: https://www.php.net/manual/en/ini.list.php
[.user.ini]: https://www.php.net/manual/en/configuration.file.per-user.php
[ini_set()]: https://www.php.net/manual/en/function.ini-set.php
[application-settings]: ./media/web-sites-php-configure/application-settings.png
[settings-button]: ./media/web-sites-php-configure/settings-button.png
[save-button]: ./media/web-sites-php-configure/save-button.png
[php-extensions]: ./media/web-sites-php-configure/php-extensions.png
[handler-mappings]: ./media/web-sites-php-configure/handler-mappings.png
[https://windows.php.net/download/]: https://windows.php.net/download/
[https://windows.php.net/downloads/releases/archives/]: https://windows.php.net/downloads/releases/archives/
[SETPHPVERCLI]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png
[GETPHPVERCLI]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png
[SETPHPVERPS]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png
[GETPHPVERPS]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png

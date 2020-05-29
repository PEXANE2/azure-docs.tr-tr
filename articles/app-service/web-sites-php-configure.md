---
title: PHP çalışma zamanını yapılandırma
description: Varsayılan PHP yüklemesini yapılandırmayı veya Azure App Service için özel bir PHP yüklemesi eklemeyi öğrenin.
author: msangapu-msft
ms.assetid: 95c4072b-8570-496b-9c48-ee21a223fb60
ms.devlang: php
ms.topic: article
ms.date: 04/13/2020
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 0605249ea0602b33e144fce8d0a77439c2077a2f
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170758"
---
# <a name="configure-php-in-azure-app-service"></a>Azure App Service 'de PHP 'yi yapılandırma

## <a name="introduction"></a>Giriş

Bu kılavuzda, [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714)Web Apps ve API Apps IÇIN yerleşik php çalışma zamanının nasıl yapılandırılacağı, özel bir php çalışma zamanı sağlamanıza ve uzantıların nasıl etkinleştirileceği gösterilmektedir. App Service kullanmak için, [ücretsiz deneme]için kaydolun. Bu kılavuzdan en iyi şekilde yararlanmak için, öncelikle App Service bir PHP uygulaması oluşturmanız gerekir.

## <a name="how-to-change-the-built-in-php-version"></a>Nasıl yapılır: yerleşik PHP sürümünü değiştirme

Bir Web uygulaması oluştururken, yapılandırılacak PHP sürümünü seçebilirsiniz. Şu anda Desteklenen sürümlerin güncel bilgileri için [App Service php](https://github.com/Azure/app-service-linux-docs/blob/master/Runtime_Support/php_support.md) 'ye bakın.

Uygulamanızın mevcut çalışma zamanı sürümünü denetlemek için [phpinfo ()] işlevini çağıran bir betik dağıtabilirsiniz.

PHP sürümünü güncelleştirmek için aşağıdaki yöntemlerden birini izleyin:

### <a name="azure-portal"></a>Azure portal

1. [Azure Portal](https://portal.azure.com) uygulamanıza gidin ve **yapılandırma** sayfasına gidin.

2. **Yapılandırma**' dan **Genel ayarlar** ' ı seçin ve yeni PHP sürümünü seçin.

3. **Genel ayarlar** dikey penceresinin en üstündeki **Kaydet** düğmesine tıklayın.

### <a name="azure-cli"></a>Azure CLI 

Azure komut satırı arabirimini kullanmak için, bilgisayarınızda [Azure CLI 'Yı yüklemelisiniz](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) .

1. Terminal ' i açın ve hesabınızda oturum açın.

        az login

1. Desteklenen çalışma zamanlarının listesini görmek için işaretleyin.

        az webapp list-runtimes | grep php

2. Uygulamanın PHP sürümünü ayarlayın.

        az webapp config set --php-version {5.6 | 7.2 | 7.3} --name {app-name} --resource-group {resource-group-name}

3. PHP sürümü artık ayarlandı. Bu ayarları doğrulayabilirsiniz:

        az webapp show --name {app-name} --resource-group {resource-group-name}

## <a name="how-to-change-the-built-in-php-configurations"></a>Nasıl yapılır: yerleşik PHP yapılandırmasını değiştirme

Herhangi bir yerleşik PHP çalışma zamanı için, bu adımları izleyerek yapılandırma seçeneklerinden herhangi birini değiştirebilirsiniz. (Php. ini yönergeleri hakkında daha fazla bilgi için bkz. [php. ini yönergeleri listesi].)

### <a name="changing-php_ini_user-php_ini_perdir-php_ini_all-configuration-settings"></a>PHP ini \_ \_ kullanıcısını DEĞIŞTIRME, php \_ ini \_ perdir, php \_ ini \_ tüm yapılandırma ayarları

1. Kök Dizininize bir [. User. ini] dosyası ekleyin.
1. Dosya için `.user.ini` kullandığınız söz dizimini kullanarak yapılandırma ayarlarını dosyaya ekleyin `php.ini` . Örneğin, `display_errors` ayarı etkinleştirmek ve `upload_max_filesize` ayarı 10m olarak ayarlamak isterseniz `.user.ini` dosyanız şu metni içerir:

        ; Example Settings
        display_errors=On
        upload_max_filesize=10M

        ; OPTIONAL: Turn this on to write errors to d:\home\LogFiles\php_errors.log
        ; log_errors=On
2. Uygulamanızı dağıtın.
3. Uygulamayı yeniden başlatın. (PHP 'nin dosya okuduğu sıklık, `.user.ini` `user_ini.cache_ttl` sistem düzeyi ayarı olan ve varsayılan olarak 300 saniye (5 dakika) olan ayar tarafından yönetilmediği için yeniden başlatma gereklidir. Uygulamanın yeniden başlatılması, PHP 'nin dosyadaki yeni ayarları okumasını zorlar `.user.ini` .)

Bir dosya kullanmanın bir alternatifi olarak `.user.ini` , sistem düzeyindeki yönergeler olmayan yapılandırma seçeneklerini ayarlamak için betiklerdeki [ini_set ()] işlevini kullanabilirsiniz.

### <a name="changing-php_ini_system-configuration-settings"></a>PHP \_ ını \_ sistem yapılandırma ayarlarını değiştirme

1. Anahtar ve değer ile uygulamanıza bir uygulama ayarı ekleyin `PHP_INI_SCAN_DIR``d:\home\site\ini`
1. `settings.ini`Dizininde kudu konsolunu (http:// &lt; site-name &gt; . scm.azurewebsite.net) kullanarak bir dosya oluşturun `d:\home\site\ini` .
1. Dosya için `settings.ini` kullandığınız söz dizimini kullanarak yapılandırma ayarlarını dosyaya ekleyin `php.ini` . Örneğin, `curl.cainfo` ayarı bir dosyaya işaret etmek `*.crt` ve ' WinCache. MaxFileSize ' ayarını 512 K olarak ayarlamak isterseniz `settings.ini` dosyanız şu metni içerir:

        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
1. Değişiklikleri yeniden yüklemek için uygulamanızı yeniden başlatın.

## <a name="how-to-enable-extensions-in-the-default-php-runtime"></a>Nasıl yapılır: varsayılan PHP çalışma zamanında uzantıları etkinleştirme

Önceki bölümde belirtildiği gibi, varsayılan PHP sürümünü, varsayılan yapılandırmasını ve etkinleştirilmiş uzantıları görmenin en iyi yolu, [PHPINFO ()]öğesini çağıran bir betiği dağıtmaktır. Ek uzantıları etkinleştirmek için aşağıdaki adımları izleyin:

### <a name="configure-via-ini-settings"></a>INI ayarları aracılığıyla yapılandırma

1. Dizine bir `ext` Dizin ekleyin `d:\home\site` .
1. `.dll`Uzantı dosyalarını `ext` dizine yerleştirin (örneğin, `php_xdebug.dll` ). Uzantıların varsayılan PHP sürümü ile uyumlu olduğundan ve VC9 ve iş parçacığı olmayan güvenli (lar) uyumlu olduğundan emin olun.
1. Anahtar ve değer ile uygulamanıza bir uygulama ayarı ekleyin `PHP_INI_SCAN_DIR``d:\home\site\ini`
1. Çağrılan bir `ini` dosya oluşturun `d:\home\site\ini` `extensions.ini` .
1. Dosya için `extensions.ini` kullandığınız söz dizimini kullanarak yapılandırma ayarlarını dosyaya ekleyin `php.ini` . Örneğin, MongoDB ve XDebug uzantılarını etkinleştirmek isterseniz `extensions.ini` dosyanız şu metni içerir:

        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
1. Değişiklikleri yüklemek için uygulamanızı yeniden başlatın.

### <a name="configure-via-app-setting"></a>Uygulama ayarı aracılığıyla yapılandırma

1. `bin`Kök dizine bir dizin ekleyin.
2. `.dll`Uzantı dosyalarını `bin` dizine yerleştirin (örneğin, `php_xdebug.dll` ). Uzantıların varsayılan PHP sürümü ile uyumlu olduğundan ve VC9 ve iş parçacığı olmayan güvenli (lar) uyumlu olduğundan emin olun.
3. Uygulamanızı dağıtın.
4. Azure portal uygulamanıza gidin ve aşağıdaki **Ayarlar** bölümünde bulunan **yapılandırma** bölümüne tıklayın.
5. **Yapılandırma** dikey penceresinde **uygulama ayarları**' nı seçin.
6. **Uygulama ayarları** bölümünde **+ Yeni uygulama ayarı** ' na tıklayın ve bir **PHP_EXTENSIONS** anahtarı oluşturun. Bu anahtarın değeri, Web sitesi köküne göreli bir yol olacaktır: **bin\your-ext-File**.
7. Alttaki **Güncelleştir** düğmesine tıklayın ve ardından **uygulama ayarları** sekmesinin üzerine **Kaydet** ' e tıklayın.

Zend uzantıları, bir **PHP_ZENDEXTENSIONS** anahtarı kullanılarak da desteklenir. Birden çok uzantıyı etkinleştirmek için, `.dll` uygulama ayarı değeri için bir virgülle ayrılmış dosya listesi ekleyin.

## <a name="how-to-use-a-custom-php-runtime"></a>Nasıl yapılır: özel PHP çalışma zamanı kullanma

Varsayılan PHP çalışma zamanı yerine, App Service PHP betiklerini yürütmek için sağladığınız PHP çalışma zamanını kullanabilir. Sağladığınız çalışma zamanı `php.ini` da sağladığınız bir dosya tarafından yapılandırılabilir. App Service ile özel bir PHP çalışma zamanı kullanmak için, bu adımları takip edin.

1. Windows için PHP 'nin güvenli olmayan, VC9 veya VC11 ile uyumlu bir sürümünü edinin. Windows için PHP 'nin son sürümleri şurada bulunabilir: [https://windows.php.net/download/] . Daha eski sürümler burada arşiv içinde bulunabilir: [https://windows.php.net/downloads/releases/archives/] .
2. `php.ini`Çalışma zamanı için dosyayı değiştirin. Yalnızca sistem düzeyinde yönergeler olan yapılandırma ayarları App Service tarafından yok sayılır. (Sistem düzeyi yönergeleri hakkında daha fazla bilgi için bkz. [php. ini yönergeleri listesi]).
3. İsteğe bağlı olarak, PHP çalışma zamanına uzantı ekleyin ve bunları dosyada etkinleştirin `php.ini` .
4. `bin`Kök Dizininize bir dizin ekleyin ve php çalışma zamanını içeren dizini bu dizine yerleştirin (örneğin, `bin\php` ).
5. Uygulamanızı dağıtın.
6. Azure portal uygulamanıza gidin ve **yapılandırma** dikey penceresine tıklayın.
8. **Yapılandırma** dikey penceresinde **yol eşlemeleri**' ni seçin. 
9. **+ Yeni işleyici** ' ye tıklayın ve `*.php` uzantı alanına ekleyin ve `php-cgi.exe` **komut dosyası işlemcisinde**yolu çalıştırılabilir dosyaya ekleyin. PHP çalışma zamanını `bin` uygulamanızın kökündeki dizine yerleştirirseniz, yol olur `D:\home\site\wwwroot\bin\php\php-cgi.exe` .
10. İşleyici eşlemesini eklemeyi bitirmeden alt kısımdaki **Güncelleştir** ' e tıklayın.
11. Değişiklikleri kaydetmek için **Kaydet**’e tıklayın.

<a name="composer"></a>

## <a name="how-to-enable-composer-automation-in-azure"></a>Nasıl yapılır: Azure 'da besteci Otomasyonu etkinleştirme

PHP projenizde bir tane varsa, varsayılan olarak, App Service besteci Oluşturucu. JSON ile hiçbir şey yapmaz. [Git dağıtımını](deploy-local-git.md)kullanıyorsanız, Oluşturucu uzantısını etkinleştirerek sırasında besteci. JSON işlemesini etkinleştirebilirsiniz `git push` .

> [!NOTE]
> [Buradan App Service ilk sınıf besteci desteği için oy](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)verebilirsiniz!
>

1. [Azure Portal](https://portal.azure.com)php uygulamanızın dikey penceresinde **Araçlar**  >  **Uzantılar**' a tıklayın.

    ![Azure 'da besteci Otomasyonu 'nu etkinleştirmek için Azure portal ayarları dikey penceresi](./media/web-sites-php-configure/composer-extension-settings.png)
2. **Ekle**' ye ve ardından **besteci**' ye tıklayın.

    ![Azure 'da besteci Otomasyonu 'nu etkinleştirmek için besteci uzantısı ekleyin](./media/web-sites-php-configure/composer-extension-add.png)
3. Yasal koşulları kabul etmek için **Tamam** ' ı tıklatın. Uzantıyı eklemek için yeniden **Tamam** ' a tıklayın.

    **Yüklü uzantılar** dikey penceresi, besteci uzantısını gösterir.
    ![Azure 'da besteci Otomasyonu 'nu etkinleştirmek için yasal koşulları kabul edin](./media/web-sites-php-configure/composer-extension-view.png)
4. Şimdi, yerel makinenizde bir Terminal penceresinde,, `git add` `git commit` ve `git push` uygulamanızı gerçekleştirin. Besteci, besteci Oluşturucu. JSON içinde tanımlanan bağımlılıkları yüklediğine dikkat edin.

    ![Azure 'da besteci otomasyonu ile git dağıtımı](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [php Geliştirici Merkezi](https://azure.microsoft.com/develop/php/).

[Ücretsiz deneme]: https://www.windowsazure.com/pricing/free-trial/
[phpinfo ()]: https://php.net/manual/en/function.phpinfo.php
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
[Php. ini yönergelerinin listesi]: https://www.php.net/manual/en/ini.list.php
[. User. ini]: https://www.php.net/manual/en/configuration.file.per-user.php
[ini_set ()]: https://www.php.net/manual/en/function.ini-set.php
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

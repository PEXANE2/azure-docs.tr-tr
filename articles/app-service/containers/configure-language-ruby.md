---
title: Ruby uygulamalarını yapılandırma - Azure Uygulama Hizmeti
description: Uygulamanız için önceden oluşturulmuş bir Ruby kapsayıcısını nasıl yapılandıracaklarınızı öğrenin. Bu makalede, en yaygın yapılandırma görevleri gösterir.
ms.topic: quickstart
ms.date: 03/28/2019
ms.reviewer: astay; kraigb
ms.custom: mvc, seodec18
ms.openlocfilehash: 8daebba840223d050a14b4b99cb6ae15472ee4f5
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80046336"
---
# <a name="configure-a-linux-ruby-app-for-azure-app-service"></a>Azure Uygulama Hizmeti için bir Linux Ruby uygulamasını yapılandırma

Bu makalede, [Azure Uygulama Hizmeti'nin](app-service-linux-intro.md) Ruby uygulamalarını nasıl çalıştırdığınız ve gerektiğinde Uygulama Hizmeti'nin davranışını nasıl özelleştirebileceğiniz açıklanmaktadır. Ruby uygulamaları gerekli tüm [taşlar](https://rubygems.org/gems)ile dağıtılmalıdır.

Bu kılavuz, App Service'de yerleşik bir Linux kapsayıcısı kullanan Ruby geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure Uygulama Hizmeti'ni hiç kullanmadıysanız, önce [PostgreSQL öğreticisiyle](tutorial-ruby-postgres-app.md) [Ruby quickstart](quickstart-ruby.md) ve Ruby'yi takip etmelisiniz.

## <a name="show-ruby-version"></a>Ruby sürümünü göster

Geçerli Ruby sürümünü göstermek için [Bulut Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Desteklenen tüm Ruby sürümlerini göstermek için [Bulut Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp list-runtimes --linux | grep RUBY
```

Bunun yerine kendi kapsayıcı resminizi oluşturarak Ruby'nin desteklenmeyen bir sürümünü çalıştırabilirsiniz. Daha fazla bilgi için [bkz.](tutorial-custom-docker-image.md)

## <a name="set-ruby-version"></a>Ruby sürümünü ayarla

Ruby sürümünü 2,3 olarak ayarlamak için [Bulut Kabuğu'nda](https://shell.azure.com) aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "RUBY|2.3"
```

> [!NOTE]
> Dağıtım sırasında aşağıdakilere benzer hatalar görürseniz:
> ```
> Your Ruby version is 2.3.3, but your Gemfile specified 2.3.1
> ```
> or
> ```
> rbenv: version `2.3.1' is not installed
> ```
> Bu, projenizde yapılandırılan Ruby sürümünün, çalıştırdığınız kapsayıcıya yüklenen sürümden (yukarıdaki`2.3.3` örnekte) farklı olduğu anlamına gelir. Yukarıdaki örnekte, hem *Gemfile'yi* hem de *.ruby sürümünü* denetleyin ve Ruby sürümünün ayarlanmadığını veya çalıştırdığınız kapsayıcıda`2.3.3` (yukarıdaki örnekte) yüklü sürüme ayarlı olduğunu doğrulayın.

## <a name="access-environment-variables"></a> Ortam değişkenlerine erişme

Uygulama Hizmeti'nde, [uygulama ayarlarınızı](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) uygulama kodunuzu dışında ayarlayabilirsiniz. Daha sonra standart [ENV['\<yol adı>']](https://ruby-doc.org/core-2.3.3/ENV.html) deseni kullanarak bunlara erişebilirsiniz. Örneğin, adlı `WEBSITE_SITE_NAME`bir uygulama ayarına erişmek için aşağıdaki kodu kullanın:

```ruby
ENV['WEBSITE_SITE_NAME']
```

## <a name="customize-deployment"></a>Dağıtımı özelleştir

Bir Git [deposu](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)veya yapı işlemlerinin açık olduğu bir [Zip paketi](../deploy-zip.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) dağıttığınızda, dağıtım motoru (Kudu) varsayılan olarak aşağıdaki dağıtım sonrası adımları otomatik olarak çalıştırır:

1. *Gemfile* olup olmadığını kontrol edin.
1. `bundle clean` öğesini çalıştırın. 
1. `bundle install --path "vendor/bundle"` öğesini çalıştırın.
1. Değerli `bundle package` taşlar satıcı/önbellek klasörüne paketleyin.

### <a name="use---without-flag"></a>Kullanın -bayrak olmadan

[-bayrak olmadan](https://bundler.io/man/bundle-install.1.html) çalıştırmak `bundle install` için, `BUNDLE_WITHOUT` [uygulama ayarını](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) virgülle ayrılmış gruplar listesine ayarlayın. Örneğin, aşağıdaki komut onu `development,test`.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings BUNDLE_WITHOUT="development,test"
```

Bu ayar tanımlanırsa, dağıtım `bundle install` altyapısı `--without $BUNDLE_WITHOUT`.

### <a name="precompile-assets"></a>Varlıkları önceden derle

Dağıtım sonrası adımlar varsayılan olarak varlıkları önceden derlemez. Varlık ön derlemesini açmak için `ASSETS_PRECOMPILE` uygulama `true` [ayarını](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) ' da . Ardından komut, `bundle exec rake --trace assets:precompile` dağıtım sonrası adımların sonunda çalıştırılır. Örnek:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings ASSETS_PRECOMPILE=true
```

Daha fazla bilgi için [bkz.](#serve-static-assets)

## <a name="customize-start-up"></a>Başlatmayı özelleştirme

Varsayılan olarak, Ruby kapsayıcı aşağıdaki sırayla Rails sunucusunu başlatır (daha fazla bilgi için [başlangıç komut dosyasına](https://github.com/Azure-App-Service/ruby/blob/master/2.3.8/startup.sh)bakın):

1. Zaten yoksa, [secret_key_base](https://edgeguides.rubyonrails.org/security.html#environmental-security) bir değer oluşturun. Bu değer, uygulamanın üretim modunda çalışması için gereklidir.
1. Ortam `RAILS_ENV` değişkenini `production`' ye ayarlama
1. *Tmp/pids* dizininde daha önce çalışan rails sunucusutarafından bırakılan herhangi bir *.pid* dosyasını silin.
1. Tüm bağımlılıkların yüklü olup olmadığını denetleyin. Değilse, yerel *satıcı/önbellek* dizininden taşlar yüklemeyi deneyin.
1. `rails server -e $RAILS_ENV` öğesini çalıştırın.

Başlatma işlemini aşağıdaki şekillerde özelleştirebilirsiniz:

- [Statik varlıklara hizmet](#serve-static-assets)
- [Üretim dışı modda çalıştırma](#run-in-non-production-mode)
- [secret_key_base el ile ayarlama](#set-secret_key_base-manually)

### <a name="serve-static-assets"></a>Statik varlıklara hizmet

Ruby kapsayıcısındaki Rails sunucusu varsayılan olarak üretim modunda çalışır ve [varlıkların önceden derlenmiş olduğunu ve web sunucunuz tarafından sunulduğunu varsayar.](https://guides.rubyonrails.org/asset_pipeline.html#in-production) Rails sunucusundan statik varlıklara hizmet vermek için iki şey yapmanız gerekir:

- **Varlıkları önceden derleyin** - [Statik varlıkları yerel olarak derleyin](https://guides.rubyonrails.org/asset_pipeline.html#local-precompilation) ve el ile dağıtın. Veya, dağıtım altyapısının bunun yerine işlemesine izin verin (bkz. [Precompile varlıkları.](#precompile-assets)
- **Statik dosyaların servis edilmesini etkinleştirin** - Ruby kapsayıcısından statik `true`varlıklara hizmet vermek için [uygulama `RAILS_SERVE_STATIC_FILES` ayarını](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) . Örnek:

    ```azurecli-interactive
    az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings RAILS_SERVE_STATIC_FILES=true
    ```

### <a name="run-in-non-production-mode"></a>Üretim dışı modda çalıştırma

Rails sunucusu varsayılan olarak üretim modunda çalışır. Örneğin, geliştirme modunda çalıştırmak için `RAILS_ENV` `development` [uygulama ayarını](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) .

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings RAILS_ENV="development"
```

Ancak, bu ayar tek başına Rails sunucusunun yalnızca yerel barındırma isteklerini kabul eden ve kapsayıcının dışında erişilemeyen geliştirme modunda başlamasına neden olur. Uzak istemci isteklerini kabul `APP_COMMAND_LINE` etmek `rails server -b 0.0.0.0`için uygulama [ayarını](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) ' da . Bu uygulama ayarı, Ruby kapsayıcısında özel bir komut çalıştırmanızı sağlar. Örnek:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings APP_COMMAND_LINE="rails server -b 0.0.0.0"
```

### <a name="set-secret_key_base-manually"></a><a name="set-secret_key_base-manually"></a>secret_key_base el ile ayarlama

App Service'in sizin için bir tane oluşturmasına izin vermek yerine kendi `secret_key_base` değerinizi kullanmak için `SECRET_KEY_BASE` [uygulama ayarını](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) istediğiniz değerle ayarlayın. Örnek:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings SECRET_KEY_BASE="<key-base-value>"
```

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda SSH oturumunu açma

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: PostgreSQL ile Rails uygulaması](tutorial-ruby-postgres-app.md)

> [!div class="nextstepaction"]
> [Uygulama Hizmeti Linux SSS](app-service-linux-faq.md)

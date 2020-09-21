---
title: Ruby uygulamalarını yapılandırma - Azure App Service
description: Uygulamanız için önceden oluşturulmuş Ruby kapsayıcısını nasıl yapılandıracağınızı öğrenin. Bu makalede en yaygın yapılandırma görevlerine yer verilmiştir.
ms.topic: quickstart
ms.date: 06/18/2020
ms.reviewer: astay; kraigb
ms.custom: mvc, seodec18
ms.openlocfilehash: c822dbdf9940db7b38d354fa32906c16977df0c0
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88085458"
---
# <a name="configure-a-linux-ruby-app-for-azure-app-service"></a>Azure App Service için Linux Ruby uygulaması yapılandırma

Bu makalede [Azure App Service](overview.md) hizmetinin Linux kapsayıcısında Ruby uygulamalarını nasıl çalıştırdığı ve gerektiğinde App Service davranışlarını özelleştirme adımları anlatılmaktadır. Ruby uygulamalarının gerekli tüm [gem'ler](https://rubygems.org/gems) ile birlikte dağıtılması gerekir.

Bu kılavuzda App Service'teki yerleşik Linux kapsayıcısını kullanan Ruby geliştiricilerine yönelik temel kavramlara ve yönergelere yer verilmiştir. Daha önce Azure App Service hizmetini kullanmadıysanız [Ruby hızlı başlangıç](quickstart-ruby.md) ve [PostgreSQL ile Ruby öğreticisi](tutorial-ruby-postgres-app.md) sayfalarını incelemeniz gerekir.

## <a name="show-ruby-version"></a>Ruby sürümünü gösterme

Geçerli Ruby sürümünü göstermek için [Cloud Shell](https://shell.azure.com)'de aşağıdaki komut çalıştırın:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Desteklenen tüm Ruby sürümlerini göstermek için [Cloud Shell](https://shell.azure.com)'de aşağıdaki komut çalıştırın:

```azurecli-interactive
az webapp list-runtimes --linux | grep RUBY
```

İsterseniz kendi kapsayıcı görüntünüzü oluşturarak Ruby'nin desteklenmeyen sürümlerini de çalıştırabilirsiniz. Daha fazla bilgi için bkz. [Özel Docker görüntüsü kullanma](tutorial-custom-container.md?pivots=container-linux).

## <a name="set-ruby-version"></a>Ruby sürümünü ayarlama

Ruby sürümünü 2.3 olarak ayarlamak için [Cloud Shell](https://shell.azure.com)'de aşağıdaki komut çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "RUBY|2.3"
```

> [!NOTE]
> Dağıtım sırasında aşağıdakine benzer hatalarla karşılaşabilirsiniz:
> ```
> Your Ruby version is 2.3.3, but your Gemfile specified 2.3.1
> ```
> veya
> ```
> rbenv: version `2.3.1' is not installed
> ```
> Bu hatalar, projenizde yapılandırılmış olan Ruby sürümünün çalıştırdığınız kapsayıcıya yüklenmiş olan sürümden (yukarıdaki örnekte `2.3.3`) farklı olduğunu gösterir. Yukarıdaki örnekte *Gemfile* ve *.ruby-version* değerlerini kontrol ettikten sonra Ruby sürümünün ayarlanmamış olduğunu veya çalıştırdığınız kapsayıcıya yüklenmiş olan sürümle (yukarıdaki örnekte `2.3.3`) aynı olduğunu doğrulayın.

## <a name="access-environment-variables"></a> Ortam değişkenlerine erişme

App Service'te uygulama kodunuzun dışında [uygulama ayarlarını düzenleyebilirsiniz](configure-common.md#configure-app-settings). Daha sonra standart [ENV['\<path-name>']](https://ruby-doc.org/core-2.3.3/ENV.html) desenini kullanarak bu ayarlara erişebilirsiniz. Örneğin `WEBSITE_SITE_NAME` adlı bir uygulama ayarına erişmek için şu kodu kullanabilirsiniz:

```ruby
ENV['WEBSITE_SITE_NAME']
```

## <a name="customize-deployment"></a>Dağıtımı özelleştirme

Derleme işlemleri açık bir [Git deposu](deploy-local-git.md) veya [Zip paketi](deploy-zip.md) dağıttığınızda dağıtım altyapısı (Kudu), varsayılan dağıtım sonrası adımları otomatik olarak çalıştırır:

1. *Gemfile* olup olmadığını kontrol eder.
1. `bundle clean` öğesini çalıştırın. 
1. `bundle install --path "vendor/bundle"` öğesini çalıştırın.
1. gem'leri vendor/cache klasörüne paketlemek için `bundle package` komutunu çalıştırır.

### <a name="use---without-flag"></a>--without bayrağını kullanma

`bundle install` komutunu [--without](https://bundler.io/man/bundle-install.1.html) bayrağıyla çalıştırmak için `BUNDLE_WITHOUT` [uygulama ayarını](configure-common.md#configure-app-settings) virgülle ayrılmış grup listesi olarak ayarlayın. Örneğin aşağıdaki komut bunu `development,test` olarak ayarlar.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings BUNDLE_WITHOUT="development,test"
```

Bu ayar tanımlandığında dağıtım altyapısı `bundle install` komutunu `--without $BUNDLE_WITHOUT` ile çalıştırır.

### <a name="precompile-assets"></a>Varlıklar için ön derleme

Dağıtım sonrası adımlar, varsayılan olarak varlıklarda ön derleme yapmaz. Varlık ön derleme özelliğini açmak için `ASSETS_PRECOMPILE` [uygulama ayarını](configure-common.md#configure-app-settings) `true` yapın. Bunu yaptığınızda dağıtım sonrası adımların en sonunda `bundle exec rake --trace assets:precompile` komutu çalıştırılır. Örnek:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings ASSETS_PRECOMPILE=true
```

Daha fazla bilgi için bkz. [Statik varlıkları sunma](#serve-static-assets).

## <a name="customize-start-up"></a>Başlatmayı özelleştirme

Ruby kapsayıcısı varsayılan olarak Rails sunucusunu aşağıdaki sıralamaya göre başlatır (daha fazla bilgi için bkz. [başlangıç betiği](https://github.com/Azure-App-Service/ruby/blob/master/2.3.8/startup.sh)):

1. [secret_key_base](https://edgeguides.rubyonrails.org/security.html#environmental-security) değeri yoksa oluşturur. Uygulamanın üretim modunda çalışması için bu değer gereklidir.
1. `RAILS_ENV` ortam değişkenini `production` olarak ayarlar.
1. Önceden çalışan bir Rails sunucusundan kalan ve *tmp/pids* dizininde bulunan *.pid* dosyalarını siler.
1. Tüm bağımlılıkların yüklü olup olmadığını denetler. Yüklü değilse gem'leri yerel *vendor/cache* dizininden yüklemeyi dener.
1. `rails server -e $RAILS_ENV` öğesini çalıştırın.

Başlatma sürecini aşağıdaki şekillerde özelleştirebilirsiniz:

- [Statik varlıkları sunma](#serve-static-assets)
- [Üretim dışı modda çalıştırma](#run-in-non-production-mode)
- [secret_key_base değerini el ile ayarlama](#set-secret_key_base-manually)

### <a name="serve-static-assets"></a>Statik varlıkları sunma

Ruby kapsayıcısındaki Rails sunucusu varsayılan olarak üretim modunda çalışır ve [varlıkların önceden derlendiğini ve web sunucunuz tarafından sunulduğunu kabul eder](https://guides.rubyonrails.org/asset_pipeline.html#in-production). Rails sunucusundan statik varlık sunmak için yapmanız gereken iki işlem vardır:

- **Varlıkları önceden derleyin** - [Statik varlıkları yerel ortamda önceden derleyin](https://guides.rubyonrails.org/asset_pipeline.html#local-precompilation) ve el ile dağıtın. Ya da bu işi dağıtım altyapısına bırakın (bkz. [Varlıklar için ön derleme](#precompile-assets).
- **Statik dosya sunmayı etkinleştirme**: Ruby kapsayıcısından statik varlık sunmak için [`RAILS_SERVE_STATIC_FILES` uygulama ayarını](configure-common.md#configure-app-settings) `true` yapın. Örnek:

    ```azurecli-interactive
    az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings RAILS_SERVE_STATIC_FILES=true
    ```

### <a name="run-in-non-production-mode"></a>Üretim dışı modda çalıştırma

Rails sunucusu varsayılan olarak üretim modunda çalışır. Geliştirme modunda çalıştırmak için `RAILS_ENV` [uygulama ayarını](configure-common.md#configure-app-settings) `development` yapın.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings RAILS_ENV="development"
```

Ancak bu ayar tek başına Rails sunucusunun localhost isteklerini kabul eden ve kapsayıcı dışından erişim sağlanamayan geliştirme modunda başlatılmasına neden olur. Uzak istemci isteklerini kabul etmek için `APP_COMMAND_LINE` [uygulama ayarını](configure-common.md#configure-app-settings) `rails server -b 0.0.0.0` yapın. Bu uygulama ayarı, Ruby kapsayıcısında özel komut çalıştırmanızı sağlar. Örnek:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings APP_COMMAND_LINE="rails server -b 0.0.0.0"
```

### <a name="set-secret_key_base-manually"></a><a name="set-secret_key_base-manually"></a> secret_key_base değerini el ile ayarlama

App Service tarafından oluşturulan yerine kendi `secret_key_base` değerinizi kullanmak için `SECRET_KEY_BASE` [uygulama ayarını](configure-common.md#configure-app-settings) istediğiniz değerle değiştirin. Örnek:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings SECRET_KEY_BASE="<key-base-value>"
```

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda SSH oturumu açma

[!INCLUDE [Open SSH session in browser](../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

[!INCLUDE [robots933456](../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: PostgreSQL ile Rails uygulaması](tutorial-ruby-postgres-app.md)

> [!div class="nextstepaction"]
> [App Service Linux Hakkında SSS](faq-app-service-linux.md)

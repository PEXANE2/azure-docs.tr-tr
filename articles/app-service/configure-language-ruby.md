---
title: Ruby uygulamalarını Yapılandırma-Azure App Service
description: Uygulamanız için önceden oluşturulmuş bir Ruby kapsayıcısını nasıl yapılandıracağınızı öğrenin. Bu makalede en sık kullanılan yapılandırma görevleri gösterilmektedir.
ms.topic: quickstart
ms.date: 06/18/2020
ms.reviewer: astay; kraigb
ms.custom: mvc, seodec18
ms.openlocfilehash: c822dbdf9940db7b38d354fa32906c16977df0c0
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88085458"
---
# <a name="configure-a-linux-ruby-app-for-azure-app-service"></a>Azure App Service için Linux Ruby uygulaması yapılandırma

Bu makalede, [Azure App Service](overview.md) bir Linux kapsayıcısında Ruby uygulamalarını nasıl çalıştığı ve gerektiğinde App Service davranışının nasıl özelleştirileceği açıklanır. Ruby uygulamalarının tüm gerekli [Gems 'ler](https://rubygems.org/gems)ile dağıtılması gerekir.

Bu kılavuz, App Service içinde yerleşik bir Linux kapsayıcısı kullanan Ruby geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure App Service hiç kullanmadıysanız, önce [PostgreSQL öğreticisiyle](tutorial-ruby-postgres-app.md) [Ruby hızlı başlangıç](quickstart-ruby.md) ve Ruby 'yi izlemeniz gerekir.

## <a name="show-ruby-version"></a>Ruby sürümünü göster

Geçerli Ruby sürümünü göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Desteklenen tüm Ruby sürümlerini göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp list-runtimes --linux | grep RUBY
```

Bunun yerine kendi kapsayıcı görüntünüzü oluşturarak desteklenmeyen bir Ruby sürümü çalıştırabilirsiniz. Daha fazla bilgi için bkz. [özel bir Docker görüntüsü kullanma](tutorial-custom-container.md?pivots=container-linux).

## <a name="set-ruby-version"></a>Ruby sürümünü ayarla

[Cloud Shell](https://shell.azure.com) Ruby sürümünü 2,3 olarak ayarlamak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "RUBY|2.3"
```

> [!NOTE]
> Dağıtım saati sırasında aşağıdakine benzer hatalar görürseniz:
> ```
> Your Ruby version is 2.3.3, but your Gemfile specified 2.3.1
> ```
> veya
> ```
> rbenv: version `2.3.1' is not installed
> ```
> Bu, projenizde yapılandırılan Ruby sürümünün çalıştırdığınız kapsayıcıda yüklü olan sürümden (Yukarıdaki örnekte) farklı olduğu anlamına gelir `2.3.3` . Yukarıdaki örnekte, hem *Gemfile* hem de *. Ruby-Version* ' ı işaretleyin ve Ruby sürümünün ayarlı olmadığını veya çalıştırmakta olduğunuz kapsayıcıda yüklü olan sürüme ( `2.3.3` Yukarıdaki örnekte) ayarlandığını doğrulayın.

## <a name="access-environment-variables"></a> Ortam değişkenlerine erişme

App Service, uygulama ayarlarınızı uygulama kodunuzun dışında [ayarlayabilirsiniz](configure-common.md#configure-app-settings) . Daha sonra standart [env [' \<path-name> ']](https://ruby-doc.org/core-2.3.3/ENV.html) modelini kullanarak bunlara erişebilirsiniz. Örneğin, adlı bir uygulama ayarına erişmek için `WEBSITE_SITE_NAME` aşağıdaki kodu kullanın:

```ruby
ENV['WEBSITE_SITE_NAME']
```

## <a name="customize-deployment"></a>Dağıtımı Özelleştir

Bir [Git deposunu](deploy-local-git.md)veya derleme işlemleri Içeren bir [ZIP paketini](deploy-zip.md) dağıtırken, dağıtım altyapısı (kudu) varsayılan olarak aşağıdaki dağıtım sonrası adımları otomatik olarak çalıştırır:

1. Bir *Gemfile* var olup olmadığını denetleyin.
1. `bundle clean` komutunu çalıştırın. 
1. `bundle install --path "vendor/bundle"` komutunu çalıştırın.
1. `bundle package`Gems 'yi satıcı/önbellek klasörüne paketlemek için çalıştırın.

### <a name="use---without-flag"></a>Bayrak olmadan--kullanın

`bundle install`Bayrak [olmadan--](https://bundler.io/man/bundle-install.1.html) ile çalıştırmak için, `BUNDLE_WITHOUT` [uygulama ayarını](configure-common.md#configure-app-settings) virgülle ayrılmış gruplar listesine ayarlayın. Örneğin, aşağıdaki komut olarak ayarlanır `development,test` .

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings BUNDLE_WITHOUT="development,test"
```

Bu ayar tanımlanmışsa, dağıtım motoru `bundle install` ile çalışır `--without $BUNDLE_WITHOUT` .

### <a name="precompile-assets"></a>Varlıkları önceden derle

Dağıtım sonrası adımları, varsayılan olarak varlıkları önceden derlemeye yönelik değildir. Varlık ön derlemesini açmak için `ASSETS_PRECOMPILE` [uygulama ayarını](configure-common.md#configure-app-settings) olarak ayarlayın `true` . Ardından komut, `bundle exec rake --trace assets:precompile` dağıtım sonrası adımların sonunda çalıştırılır. Örnek:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings ASSETS_PRECOMPILE=true
```

Daha fazla bilgi için bkz. [statik varlıkları](#serve-static-assets)sunma.

## <a name="customize-start-up"></a>Başlatmayı özelleştirme

Varsayılan olarak, Ruby kapsayıcısı aşağıdaki sırada bir rayı sunucusunu başlatır (daha fazla bilgi için [Başlangıç betiğine](https://github.com/Azure-App-Service/ruby/blob/master/2.3.8/startup.sh)bakın):

1. Zaten yoksa bir [secret_key_base](https://edgeguides.rubyonrails.org/security.html#environmental-security) değeri oluşturun. Uygulamanın üretim modunda çalışması için bu değer gereklidir.
1. `RAILS_ENV`Ortam değişkenini olarak ayarlayın `production` .
1. Daha önce çalışan bir rayla sunucu tarafından bırakılan *tmp/PIDS* dizinindeki tüm *. pid* dosyalarını silin.
1. Tüm bağımlılıkların yüklü olup olmadığını denetleyin. Aksi takdirde, yerel *satıcı/önbellek* dizininden Gems 'yi yüklemeyi deneyin.
1. `rails server -e $RAILS_ENV` komutunu çalıştırın.

Başlangıç işlemini aşağıdaki yollarla özelleştirebilirsiniz:

- [Statik varlıkları sunma](#serve-static-assets)
- [Üretim dışı modda çalıştır](#run-in-non-production-mode)
- [Secret_key_base el ile ayarla](#set-secret_key_base-manually)

### <a name="serve-static-assets"></a>Statik varlıkları sunma

Ruby kapsayıcısındaki raya sunucusu varsayılan olarak üretim modunda çalışır ve [varlıkların önceden derlenmiş olduğunu ve Web sunucunuz tarafından sunulduğunu varsayar](https://guides.rubyonrails.org/asset_pipeline.html#in-production). Raylar sunucusundan statik varlıklara hizmeti sağlamak için iki şey yapmanız gerekir:

- **Varlıkları**  -  önceden derle [Statik varlıkları yerel olarak önceden derleyin](https://guides.rubyonrails.org/asset_pipeline.html#local-precompilation) ve el ile dağıtın. Ya da, bunun yerine dağıtım altyapısının bunu işlemesini sağlayın (bkz. [varlıkları önceden derle](#precompile-assets).
- **Statik dosyalara hizmet vermeye olanak sağlama** -Ruby kapsayıcısından statik varlıklar sunmak için [ `RAILS_SERVE_STATIC_FILES` uygulama ayarını olarak ayarlayın](configure-common.md#configure-app-settings) `true` . Örnek:

    ```azurecli-interactive
    az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings RAILS_SERVE_STATIC_FILES=true
    ```

### <a name="run-in-non-production-mode"></a>Üretim dışı modda çalıştır

Rayları sunucusu varsayılan olarak üretim modunda çalışır. Geliştirme modunda çalıştırmak için, örneğin, `RAILS_ENV` [uygulama ayarını](configure-common.md#configure-app-settings) olarak ayarlayın `development` .

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings RAILS_ENV="development"
```

Ancak, bu ayar tek başına, yalnızca localhost isteklerini kabul eden ve kapsayıcının dışında erişilebilir olmayan bir geliştirme modunda başlatılmasına neden olur. Uzak istemci isteklerini kabul etmek için, `APP_COMMAND_LINE` [uygulama ayarını](configure-common.md#configure-app-settings) olarak ayarlayın `rails server -b 0.0.0.0` . Bu uygulama ayarı, Ruby kapsayıcısında özel bir komut çalıştırmanızı sağlar. Örnek:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings APP_COMMAND_LINE="rails server -b 0.0.0.0"
```

### <a name="set-secret_key_base-manually"></a><a name="set-secret_key_base-manually"></a>Secret_key_base el ile ayarla

`secret_key_base`Sizin için bir tane oluşturmak App Service sağlamak yerine kendi değerini kullanmak için, `SECRET_KEY_BASE` [uygulama ayarını](configure-common.md#configure-app-settings) istediğiniz değerle ayarlayın. Örnek:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings SECRET_KEY_BASE="<key-base-value>"
```

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>SSH oturumunu tarayıcıda aç

[!INCLUDE [Open SSH session in browser](../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

[!INCLUDE [robots933456](../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: PostgreSQL ile raya uygulaması](tutorial-ruby-postgres-app.md)

> [!div class="nextstepaction"]
> [App Service Linux SSS](faq-app-service-linux.md)

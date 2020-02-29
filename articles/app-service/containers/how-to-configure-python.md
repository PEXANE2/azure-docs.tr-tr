---
title: Linux Python uygulamalarını yapılandırma
description: Uygulamanız için önceden oluşturulmuş bir Python kapsayıcısını yapılandırmayı öğrenin. Bu makalede en sık kullanılan yapılandırma görevleri gösterilmektedir.
ms.topic: quickstart
ms.date: 03/28/2019
ms.reviewer: astay; kraigb
ms.custom: seodec18
ms.openlocfilehash: d2c5a094c45eeca779a33a39261bd3fc17d53d1a
ms.sourcegitcommit: 3c925b84b5144f3be0a9cd3256d0886df9fa9dc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "77913863"
---
# <a name="configure-a-linux-python-app-for-azure-app-service"></a>Azure App Service için bir Linux Python uygulaması yapılandırma

Bu makalede, [Azure App Service](app-service-linux-intro.md) Python uygulamalarının nasıl çalıştığı ve gerektiğinde App Service davranışının nasıl özelleştirileceği açıklanır. Python uygulamalarının tüm gerekli [PIP](https://pypi.org/project/pip/) modülleri ile dağıtılması gerekir.

App Service dağıtım altyapısı bir sanal ortamı otomatik olarak etkinleştirir ve bir [Git deposu](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)dağıtırken sizin için `pip install -r requirements.txt` çalıştırır veya derleme işlemlerine sahip bir [ZIP paketi](../deploy-zip.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) üzerinde geçiş yapar.

Bu kılavuz, App Service içinde yerleşik bir Linux kapsayıcısı kullanan Python geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure App Service hiç kullanmadıysanız, ilk olarak [Python hızlı](quickstart-python.md) başlangıcı ve [Python 'U PostgreSQL öğreticisiyle](tutorial-python-postgresql-app.md) izlemelisiniz.

> [!NOTE]
> Linux Şu anda App Service ' de Python uygulamalarını çalıştırmak için önerilen seçenektir. Windows seçeneği hakkında bilgi için, [App Service Windows üzerinde Python](https://docs.microsoft.com/visualstudio/python/managing-python-on-azure-app-service)konusuna bakın.
>

## <a name="show-python-version"></a>Python sürümünü göster

Geçerli Python sürümünü göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Desteklenen tüm Python sürümlerini göstermek için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp list-runtimes --linux | grep PYTHON
```

Bunun yerine kendi kapsayıcı görüntünüzü oluşturarak desteklenmeyen bir Python sürümü çalıştırabilirsiniz. Daha fazla bilgi için bkz. [özel bir Docker görüntüsü kullanma](tutorial-custom-docker-image.md).

## <a name="set-python-version"></a>Python sürümünü ayarla

Python sürümünü 3,7 olarak ayarlamak için [Cloud Shell](https://shell.azure.com) aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "PYTHON|3.7"
```

## <a name="customize-build-automation"></a>Derleme Otomasyonu 'nu özelleştirme

Uygulamanızı, derleme Otomasyonu açıkken git veya ZIP paketleri kullanarak dağıtırsanız, App Service aşağıdaki sırayla Otomasyon adımları oluşturun:

1. `PRE_BUILD_SCRIPT_PATH`tarafından belirtilmişse özel betiği çalıştırın.
1. `pip install -r requirements.txt` öğesini çalıştırın.
1. Deponun kökünde *Manage.py* bulunursa, *Manage.py collectstatic*' i çalıştırın. Ancak, `DISABLE_COLLECTSTATIC` `true`olarak ayarlanırsa, bu adım atlanır.
1. `POST_BUILD_SCRIPT_PATH`tarafından belirtilmişse özel betiği çalıştırın.

`PRE_BUILD_COMMAND`, `POST_BUILD_COMMAND`ve `DISABLE_COLLECTSTATIC` varsayılan olarak boş olan ortam değişkenleridir. Oluşturma öncesi komutları çalıştırmak için `PRE_BUILD_COMMAND`tanımlayın. Oluşturma sonrası komutları çalıştırmak için `POST_BUILD_COMMAND`tanımlayın. Docgo uygulamaları oluştururken collectstatic çalışmasını devre dışı bırakmak için `DISABLE_COLLECTSTATIC=true`ayarlayın.

Aşağıdaki örnek, virgülle ayrılmış bir dizi komuta iki değişkeni belirtir.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PRE_BUILD_COMMAND="echo foo, scripts/prebuild.sh"
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings POST_BUILD_COMMAND="echo foo, scripts/postbuild.sh"
```

Derleme Otomasyonu 'nu özelleştirmek için ek ortam değişkenleri için bkz. [Oryx Configuration](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md).

App Service çalışma ve Linux 'ta Python uygulamaları oluşturma hakkında daha fazla bilgi için bkz. [Oryx belgeleri: Python uygulamalarının algılanıp oluşturulması](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/python.md).

## <a name="container-characteristics"></a>Kapsayıcı özellikleri

Linux üzerinde App Service dağıtılan Python uygulamaları, [App Service Python GitHub deposunda](https://github.com/Azure-App-Service/python)tanımlanan bir Docker kapsayıcısı içinde çalışır. Görüntü yapılandırmasını sürüme özgü dizinler içinde bulabilirsiniz.

Bu kapsayıcı aşağıdaki özelliklere sahiptir:

- Uygulamalar ek [ bağımsız değişkenleri kullanılarak ](https://gunicorn.org/)Gunicorn WSGI HTTP Server`--bind=0.0.0.0 --timeout 600` ile çalıştırılır.

- Temel görüntü varsayılan olarak Flask web çerçevesini içerir ancak kapsayıcı Django gibi WSGI ve Python 3.7 ile uyumlu diğer çerçeveleri de destekler.

- Django gibi ek paketleri yüklemek için [ kullanarak projenizin kök dizininde birrequirements.txt](https://pip.pypa.io/en/stable/user_guide/#requirements-files)`pip freeze > requirements.txt` dosyası oluşturun. Ardından projenizi Git dağıtımı kullanarak App Service'te yayımlayın. Bunu yaptığınızda uygulamanızın bağımlılıklarının yüklenmesi için kapsayıcıda otomatik olarak `pip install -r requirements.txt` çalıştırılır.

## <a name="container-startup-process"></a>Kapsayıcı başlangıç işlemi

Başlatma sırasında Linux'ta App Service kapsayıcısı şu adımları çalıştırır:

1. Sağlanmışsa, [özel bir başlangıç komutu](#customize-startup-command)kullanın.
2. Bir [Docgo uygulamasının](#django-app)mevcut olup olmadığını denetleyin ve algılanırsa gunicman 'ı başlatın.
3. [Flask uygulamasının](#flask-app)varolup olmadığını denetleyin ve algılanırsa, gunicgını başlatın.
4. Başka bir uygulama bulunamazsa yoksa kapsayıcıda bulunan varsayılan uygulamayı başlatır.

Aşağıdaki bölümlerde bu seçeneklerle ilgili ek ayrıntılar verilmiştir.

### <a name="django-app"></a>Django uygulaması

Django uygulamaları için App Service uygulama kodunuzda `wsgi.py` adlı bir dosya olup olmadığını denetler ve ardından şu komutu kullanarak Gunicorn'u çalıştırır:

```bash
# <module> is the path to the folder that contains wsgi.py
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

Başlangıç komutu üzerinde daha fazla denetime sahip olmak istiyorsanız [özel başlangıç komutu](#customize-startup-command) kullanın ve `<module>` yerine *wsgi.py* dosyasını içeren modülün adını yazın.

### <a name="flask-app"></a>Flask uygulaması

Flask için, App Service *Application.py* veya *app.py* adlı bir dosya arar ve gunicgını aşağıdaki gibi başlatır:

```bash
# If application.py
gunicorn --bind=0.0.0.0 --timeout 600 application:app
# If app.py
gunicorn --bind=0.0.0.0 --timeout 600 app:app
```

Ana uygulama modülünüz farklı bir dosyada bulunuyorsa uygulama nesnesi için farklı bir ad kullanın veya Gunicorn'a ek bağımsız değişkenler sağlamak istiyorsanız [özel başlangıç komutu](#customize-startup-command) kullanın.

### <a name="default-behavior"></a>Varsayılan davranış

App Service özel komut dosyası, Django uygulaması veya Flask uygulaması bulamazsa _opt/defaultsite_ klasöründe bulunan varsayılan salt okunur uygulamayı çalıştırır. Varsayılan uygulama aşağıdaki gibi görünür:

![Varsayılan Linux'ta App Service web sayfası](media/how-to-configure-python/default-python-app.png)

## <a name="customize-startup-command"></a>Başlangıç komutunu Özelleştir

Özel bir Gunicorn başlangıç komutu sağlayarak kapsayıcının başlangıç davranışını denetleyebilirsiniz. Bunu yapmak için [Cloud Shell](https://shell.azure.com)aşağıdaki komutu çalıştırın:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<custom-command>"
```

Örneğin, ana modülü *Hello.py* olan bir Flask uygulaması varsa ve bu dosyadaki Flask uygulama nesnesi `myapp`olarak adlandırılmışsa, *\<özel komut >* aşağıdaki gibidir:

```bash
gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
```

Ana modülünüz `website` gibi bir alt klasör ise bu klasörü `--chdir` bağımsız değişkeniyle belirtin:

```bash
gunicorn --bind=0.0.0.0 --timeout 600 --chdir website hello:myapp
```

Ayrıca, `--workers=4`gibi *özel komut >\<* , gunic, için ek bağımsız değişkenler de ekleyebilirsiniz. Daha fazla bilgi için bkz. [Gunicorn'u Çalıştırma](https://docs.gunicorn.org/en/stable/run.html) (docs.gunicorn.org).

[Aiohttp](https://aiohttp.readthedocs.io/en/stable/web_quickstart.html)gibi gunicbir Server kullanmak için, *\<özel komut >* aşağıdaki gibi bir şekilde değiştirebilirsiniz:

```bash
python3.7 -m aiohttp.web -H localhost -P 8080 package.module:init_func
```

> [!Note]
> App Service, özel komut dosyasının işlenmesi sırasında oluşan hataları yoksayar ve başlatma işlemine Django ve Flask uygulamalarını arayarak devam eder. Beklediğiniz davranışı görmüyorsanız başlangıç dosyanızın App Service'e dağıtıldığından ve dosyada hata bulunmadığından emin olun.

## <a name="access-environment-variables"></a>Ortam değişkenlerine erişim

App Service, uygulama ayarlarınızı uygulama kodunuzun dışında [ayarlayabilirsiniz](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) . Ardından, standart [OS. Environ](https://docs.python.org/3/library/os.html#os.environ) modelini kullanarak bunlara erişebilirsiniz. Örneğin, `WEBSITE_SITE_NAME`adlı bir uygulama ayarına erişmek için aşağıdaki kodu kullanın:

```python
os.environ['WEBSITE_SITE_NAME']
```

## <a name="detect-https-session"></a>HTTPS oturumunu Algıla

App Service, [SSL sonlandırması](https://wikipedia.org/wiki/TLS_termination_proxy) ağ yükü dengeleyicilerde gerçekleşinceye kadar, tüm https istekleri UYGULAMANıZA şifrelenmemiş HTTP istekleri olarak ulaşacak. Uygulama mantığınızın kullanıcı isteklerinin şifrelenip şifrelenmediğini denetlemesi gerekiyorsa `X-Forwarded-Proto` üst bilgisini inceleyin.

```python
if 'X-Forwarded-Proto' in request.headers and request.headers['X-Forwarded-Proto'] == 'https':
# Do something when HTTPS is used
```

Popüler Web çerçeveleri, standart uygulama hiyerarşinizdeki `X-Forwarded-*` bilgilerine erişmenizi sağlar. [Codeigniter](https://codeigniter.com/)'da, [is_https ()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) varsayılan olarak `X_FORWARDED_PROTO` değerini denetler.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>SSH oturumunu tarayıcıda aç

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Sorun giderme

- **Kendi uygulama kodunuzu dağıttıktan sonra varsayılan uygulamayı görüyorsunuz.** App Service için uygulama kodunuzu dağıtmadığınız ya da App Service uygulama kodunuzu bulamadığı ve bunun yerine varsayılan uygulamayı çalıştırmadığınız için varsayılan uygulama görüntülenir.
- App Service'i yeniden başlatın, 15-20 saniye bekleyin ve uygulamayı yeniden denetleyin.
- Windows tabanlı örnek yerine Linux için App Service’i kullandığınızdan emin olun. Azure CLI’de `az webapp show --resource-group <resource_group_name> --name <app_service_name> --query kind` ve `<resource_group_name>` hizmetini uygun bir şekilde değiştiren `<app_service_name>` komutunu çalıştırın. Çıktı olarak `app,linux` görünmelidir, aksi takdirde App Service’i yeniden oluşturun ve Linux’u seçin.
- SSH veya Kudu kullanarak doğrudan App Service'e bağlanın ve dosyalarınızın *site/wwwroot* dizininde bulunduğunu doğrulayın. Dosyalarınız orada değilse dağıtım işlemlerinizi gözden geçirin ve uygulamayı yeniden dağıtın.
- Dosyalarınız oradaysa App Service başlangıç dosyanızı tanımlayamamış olabilir. Uygulamanızın App Service'in [Django](#django-app) veya [Flask](#flask-app) için beklediği şekilde yapılandırılmış olduğundan emin olun veya [özel başlangıç komutu](#customize-startup-command) kullanın.
- **Tarayıcıda "Hizmet Kullanılamıyor" iletisini görüyorsunuz.** Bu durum, tarayıcının App Service'ten yanıt beklerken zaman aşımına uğradığını gösterir. Bunun nedeni App Service'in Gunicorn sunucusunu başlatmış olması ancak uygulama kodunu belirten bağımsız değişkenlerin hatalı olmasıdır.
- Özellikle App Service Planınızda en düşük fiyatlandırma katmanlarını kullanıyorsanız tarayıcıyı yenileyin. Ücretsiz katmanları kullandığınızda uygulamanın başlaması daha uzun sürebilir ve tarayıcıyı yenilediğinizde yanıt verebilir.
- Uygulamanızın App Service'in [Django](#django-app) veya [Flask](#flask-app) için beklediği şekilde yapılandırılmış olduğundan emin olun veya [özel başlangıç komutu](#customize-startup-command) kullanın.
- [Günlük akışına erişin](#access-diagnostic-logs).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: PostgreSQL ile Python uygulaması](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Öğretici: özel kapsayıcı deposundan dağıtma](tutorial-custom-docker-image.md)

> [!div class="nextstepaction"]
> [App Service Linux SSS](app-service-linux-faq.md)

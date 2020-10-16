---
title: Linux Python uygulamalarını yapılandırma
description: Hem Azure portal hem de Azure CLı kullanarak Web uygulamalarının çalıştırıldığı Python kapsayıcısını nasıl yapılandıracağınızı öğrenin.
ms.topic: quickstart
ms.date: 10/06/2020
ms.reviewer: astay; kraigb
ms.custom: mvc, seodec18, devx-track-python
ms.openlocfilehash: b489f7daebc9232088020948752c3792dca65095
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92018755"
---
# <a name="configure-a-linux-python-app-for-azure-app-service"></a>Azure App Service için bir Linux Python uygulaması yapılandırma

Bu makalede, [Azure App Service](overview.md) Python uygulamalarının nasıl çalıştığı ve gerektiğinde App Service davranışının nasıl özelleştirileceği açıklanır. Python uygulamalarının tüm gerekli [PIP](https://pypi.org/project/pip/) modülleri ile dağıtılması gerekir.

App Service dağıtım altyapısı bir sanal ortamı otomatik olarak etkinleştirir ve `pip install -r requirements.txt` bir [Git deposu](deploy-local-git.md)veya bir [ZIP paketi](deploy-zip.md)dağıttığınızda sizin için çalışır.

Bu kılavuz, App Service içinde yerleşik bir Linux kapsayıcısı kullanan Python geliştiricileri için temel kavramlar ve yönergeler sağlar. Azure App Service hiç kullanmadıysanız, önce [Python hızlı](quickstart-python.md) başlangıcı ' nı ve [PostgreSQL öğreticisi ile Python](tutorial-python-postgresql-app.md)'u izleyin.

Yapılandırma için [Azure Portal](https://portal.azure.com) ya da Azure CLI 'yi kullanabilirsiniz:

- **Azure Portal**, **Settings**  >  [Azure Portal App Service uygulama yapılandırma](configure-common.md)sayfasında açıklandığı şekilde uygulamanın Ayarlar**yapılandırma** sayfasını kullanın.

- **Azure CLI**: iki seçeneğiniz vardır.

    - Kod bloklarının sağ üst köşesinde bulunan **deneyin** düğmesini kullanarak açabileceğiniz [Azure Cloud Shell](../cloud-shell/overview.md)komutları çalıştırın.
    - [Azure CLI](/cli/azure/install-azure-cli)'nın en son sürümünü yükleyerek komutları yerel olarak çalıştırın ve [az Login](/cli/azure/reference-index#az-login)komutunu kullanarak Azure 'da oturum açın.
    
> [!NOTE]
> Linux Şu anda App Service ' de Python uygulamalarını çalıştırmak için önerilen seçenektir. Windows seçeneği hakkında bilgi için, [App Service Windows üzerinde Python](/visualstudio/python/managing-python-on-azure-app-service)konusuna bakın.

## <a name="configure-python-version"></a>Python sürümünü Yapılandır

- **Azure Portal**: **yapılandırma** sayfasında, Linux kapsayıcıları için [genel ayarları yapılandırma](configure-common.md#configure-general-settings) bölümünde açıklandığı gibi **Genel ayarlar** sekmesini kullanın.

- **Azure CLI**:

    -  [Az WebApp config Show](/cli/azure/webapp/config#az_webapp_config_show)komutuyla geçerli Python sürümünü göster:
    
        ```azurecli-interactive
        az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
        ```
        
        `<resource-group-name>`Ve ' i `<app-name>` Web uygulamanız için uygun adlarla değiştirin.
    
    - Python sürümünü [az WebApp config Set](/cli/azure/webapp/config#az_webapp_config_set) ile ayarla
        
        ```azurecli-interactive
        az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "PYTHON|3.7"
        ```
    
    - [Az WebApp List-çalışma zamanları](/cli/azure/webapp#az_webapp_list_runtimes)ile Azure App Service desteklenen tüm Python sürümlerini gösterin:
    
        ```azurecli-interactive
        az webapp list-runtimes --linux | grep PYTHON
        ```
    
Bunun yerine kendi kapsayıcı görüntünüzü oluşturarak desteklenmeyen bir Python sürümü çalıştırabilirsiniz. Daha fazla bilgi için bkz. [Özel Docker görüntüsü kullanma](tutorial-custom-container.md?pivots=container-linux).

<!-- <a> element here to preserve external links-->
<a name="access-environment-variables"></a>

## <a name="customize-build-automation"></a>Derleme Otomasyonu 'nu özelleştirme

App Service, Oryx olarak adlandırılan yapı sistemi, git veya ZIP paketleri kullanarak uygulamanızı dağıtırken aşağıdaki adımları gerçekleştirir:

1. Bu ayar tarafından belirtilmişse özel bir ön derleme betiği çalıştırın `PRE_BUILD_COMMAND` .
1. `pip install -r requirements.txt` komutunu çalıştırın. *requirements.txt* dosya projenin kök klasöründe bulunmalıdır. Aksi takdirde, yapı işlemi şu hatayı raporlar: "setup.py bulunamadı veya requirements.txt; Pınstall çalışmıyor. "
1. Depo kökünde *Manage.py* bulunursa (bir Docgo uygulaması olduğunu), *Manage.py collectstatic*komutunu çalıştırın. Ancak, `DISABLE_COLLECTSTATIC` ayar ise, `true` Bu adım atlanır.
1. Ayar tarafından belirtilmişse özel derleme sonrası betiği çalıştırın `POST_BUILD_COMMAND` .

Varsayılan olarak,, `PRE_BUILD_COMMAND` `POST_BUILD_COMMAND` ve `DISABLE_COLLECTSTATIC` ayarları boştur. 

- Docgo uygulamaları oluştururken collectstatic çalışmasını devre dışı bırakmak için `DISABLE_COLLECTSTATIC` ayarı true olarak ayarlayın.

- Oluşturma öncesi komutları çalıştırmak için, `PRE_BUILD_COMMAND` ayarı gibi bir komut `echo Pre-build command` ya da proje kökle ilişkili bir betik dosyasının yolunu (gibi) olarak ayarlayın `scripts/prebuild.sh` . Tüm komutların, proje kök klasörü için göreli yollar kullanması gerekir.

- Oluşturma sonrası komutları çalıştırmak için, `POST_BUILD_COMMAND` ayarı gibi bir komut ya da, `echo Post-build command` Proje kökle ilişkili bir betik dosyasının yolunu (gibi) ya da gibi ayarlayın `scripts/postbuild.sh` . Tüm komutların, proje kök klasörü için göreli yollar kullanması gerekir.

Derleme Otomasyonu 'nu özelleştiren ek ayarlar için bkz. [Oryx Configuration](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md). 

App Service çalışma ve Linux 'ta Python uygulamaları oluşturma hakkında daha fazla bilgi için bkz. [Python uygulamalarını ne şekilde algıladığını ve bunları yapılandırma](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/python.md).

> [!NOTE]
> Ve ayarları, ve ile `PRE_BUILD_SCRIPT_PATH` `POST_BUILD_SCRIPT_PATH` aynıdır `PRE_BUILD_COMMAND` ve `POST_BUILD_COMMAND` eski amaçlar için desteklenir.
> 
> `SCM_DO_BUILD_DURING_DEPLOYMENT`Ya da 1 içeriyorsa adlı bir ayar `true` , dağıtım sırasında bir Oryx derlemesini tetikler. Bu ayar, git kullanılarak dağıtıldığında, Azure CLı komutuyla `az webapp up` ve Visual Studio Code doğru olur.

> [!NOTE]
> Oryx 'in çalıştığı yapı kapsayıcısı uygulamanın çalıştığı çalışma zamanı kapsayıcısından farklı olduğundan, her zaman tüm ön ve derleme sonrası betiklerdeki göreli yolları kullanın. Uygulama Projesi klasörünüzün kapsayıcı içinde tam olarak yerleştirilme (örneğin, *site/Wwwroot*altına yerleştirilmiş olması) hiçbir şekilde güvenmeyin.

## <a name="production-settings-for-django-apps"></a>Docgo uygulamaları için üretim ayarları

Azure App Service gibi bir üretim ortamında, Docgo uygulamaları Docgo 'nun [dağıtım denetim listesini](https://docs.djangoproject.com/en/3.1/howto/deployment/checklist/) (djangoproject.com) izlemelidir.

Aşağıdaki tabloda, Azure ile ilgili üretim ayarları açıklanmaktadır. Bu ayarlar, uygulamanın *Setting.py* dosyasında tanımlanmıştır.

| Docgo ayarı | Azure için yönergeler |
| --- | --- |
| `SECRET_KEY` | Değeri, [uygulama ayarlarında ortam değişkenleri olarak erişim](#access-app-settings-as-environment-variables)bölümünde açıklandığı gibi bir App Service ayarında depolayın. [Değeri, Azure Key Vault ' de bir "secrete" olarak da saklayabilirsiniz](/azure/key-vault/secrets/quick-create-python). |
| `DEBUG` | `DEBUG`App Service 0 (false) değeriyle bir ayar oluşturun ve ardından değeri bir ortam değişkeni olarak yükleyin. Geliştirme ortamınızda, `DEBUG` 1 değerine sahip bir ortam değişkeni oluşturun (true). |
| `ALLOWED_HOSTS` | Üretimde, Docgo, uygulamanın URL 'sini `ALLOWED_HOSTS` *Settings.py*dizisine dahil etmeniz gerekir. Bu URL 'YI çalışma zamanında bu kodla elde edebilirsiniz `os.environ['WEBSITE_HOSTNAME']` . App Service, `WEBSITE_HOSTNAME` ortam değişkenini otomatik olarak uygulamanın URL 'si olarak ayarlar. |
| `DATABASES` | Veritabanı bağlantısı için App Service ayarları tanımlayın ve sözlüğü doldurmak için ortam değişkenleri olarak yükleyin [`DATABASES`](https://docs.djangoproject.com/en/3.1/ref/settings/#std:setting-DATABASES) . Değerleri (özellikle Kullanıcı adı ve parola) [gizli Azure Key Vault](/azure/key-vault/secrets/quick-create-python)olarak saklayabilirsiniz. |

## <a name="container-characteristics"></a>Kapsayıcı özellikleri

App Service 'ye dağıtıldığında, Python uygulamaları [App Service Python GitHub deposunda](https://github.com/Azure-App-Service/python)tanımlanan bir Linux Docker kapsayıcısı içinde çalışır. Görüntü yapılandırmasını sürüme özgü dizinler içinde bulabilirsiniz.

Bu kapsayıcı aşağıdaki özelliklere sahiptir:

- Uygulamalar ek `--bind=0.0.0.0 --timeout 600` bağımsız değişkenleri kullanılarak [Gunicorn WSGI HTTP Server](https://gunicorn.org/) ile çalıştırılır.
    - Gunic, [yapılandırmaya genel bakış](https://docs.gunicorn.org/en/stable/configure.html#configuration-file) (docs.gunicorn.org) bölümünde açıklandığı gibi, proje kökündeki bir *Gunicorn.conf.py* dosyası aracılığıyla gunic, için yapılandırma ayarları sağlayabilirsiniz. [Başlangıç komutunu](#customize-startup-command)alternatif olarak özelleştirebilirsiniz.

    - Web uygulamanızı yanlışlıkla veya bilinçli DDOS saldırılarına karşı korumak için gunictbıd, [Gunicx 'ı dağıtma](https://docs.gunicorn.org/en/latest/deploy.html) konusunda açıklandığı gibi bir NGINX ters proxy 'nin arkasında çalıştırılır (docs.gunicorn.org).

- Varsayılan olarak, temel kapsayıcı görüntüsü yalnızca Flask Web çerçevesini içerir, ancak kapsayıcı, WSGI uyumlu ve Python 3.6 + ile uyumlu olan ve Docgo gibi diğer çerçeveleri destekler.

- Docgo gibi ek paketler yüklemek için, projenizin kökünde doğrudan bağımlılıklarınızı belirten bir [*requirements.txt*](https://pip.pypa.io/en/stable/user_guide/#requirements-files) dosyası oluşturun. App Service sonra bu bağımlılıkları projenizi dağıtırken otomatik olarak yüklenir.

    *requirements.txt* dosyanın yüklenecek bağımlılıklar için proje kökünde olması *gerekir* . Aksi takdirde, yapı işlemi şu hatayı raporlar: "setup.py bulunamadı veya requirements.txt; Pınstall çalışmıyor. " Bu hatayla karşılaşırsanız, gereksinim dosyanızın konumunu kontrol edin.

- App Service `WEBSITE_HOSTNAME` , Web uygulamasının URL 'si ile adlı bir ortam değişkenini otomatik olarak tanımlar, örneğin `msdocs-hello-world.azurewebsites.net` . Ayrıca `WEBSITE_SITE_NAME` , uygulamanızın adıyla da tanımlar `msdocs-hello-world` . 
   
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
# <module> is the name of the folder that contains wsgi.py
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

Başlangıç komutu üzerinde daha özel denetim istiyorsanız, [özel bir başlangıç komutu](#customize-startup-command)kullanın, `<module>` *wsgi.py*içeren klasörün adıyla değiştirin ve `--chdir` Bu modül proje kökünde değilse bir bağımsız değişken ekleyin. Örneğin, *wsgi.py* 'niz proje kökünden *knboard/arka uç/yapılandırma* altında bulunuyorsa, bağımsız değişkenleri kullanın `--chdir knboard/backend config.wsgi` .

Üretim günlüğünü etkinleştirmek için, `--access-logfile` ve `--error-logfile` parametrelerini [özel başlangıç komutları](#customize-startup-command)için örneklerde gösterildiği gibi ekleyin.

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

![Varsayılan Linux'ta App Service web sayfası](media/configure-language-python/default-python-app.png)

## <a name="customize-startup-command"></a>Başlangıç komutunu Özelleştir

Bu makalede daha önce belirtildiği gibi, guniclerconfiguration ['a genel bakış](https://docs.gunicorn.org/en/stable/configure.html#configuration-file)bölümünde açıklandığı gibi, proje kökündeki bir *Gunicorn.conf.py* dosyası aracılığıyla gunicyasaları için yapılandırma ayarları sağlayabilirsiniz.

Bu yapılandırma yeterli değilse, bir başlangıç komut dosyasında özel bir başlangıç komutu veya birden çok komut sağlayarak kapsayıcının başlangıç davranışını kontrol edebilirsiniz. Bir başlangıç komut dosyası, seçtiğiniz adı (örneğin, *Startup.sh*, *Startup. cmd*, *startup.txt*vb.) kullanabilir.

Tüm komutların, proje kök klasörü için göreli yollar kullanması gerekir.

Başlangıç komutunu veya komut dosyasını belirtmek için:

- **Azure Portal**: uygulamanın **yapılandırma** sayfasını seçin ve ardından **Genel ayarlar**' ı seçin. **Başlangıç komutu** alanında, başlangıç komutunuz tam metnini veya başlangıç komut dosyanızın adını yerleştirin. Sonra değişiklikleri uygulamak için **Kaydet** ' i seçin. Bkz. Linux kapsayıcıları için [genel ayarları yapılandırma](configure-common.md#configure-general-settings) .

- **Azure CLI**: başlangıç komutunu veya dosyayı ayarlamak için [az WebApp config Set](/cli/azure/webapp/config#az_webapp_config_set) komutunu parametresiyle birlikte kullanın `--startup-file` :

    ```azurecli-interactive
    az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<custom-command>"
    ```
        
    `<custom-command>`Başlangıç komutunuz tam metniyle veya başlangıç komut dosyanızın adı ile değiştirin.
        
App Service, özel bir başlangıç komutunu veya dosyasını işlerken meydana gelen hataları yoksayar, ardından Docgo ve Flask uygulamalarını arayarak başlangıç işlemini devam ettirir. İstediğiniz davranışı görmüyorsanız, başlangıç komutınızın veya dosyanızın hatasız ve uygulama kodunuzla birlikte App Service bir başlangıç komut dosyasının dağıtıldığını kontrol edin. Daha fazla bilgi için [Tanılama günlüklerine](#access-diagnostic-logs) de bakabilirsiniz. Ayrıca [Azure Portal](https://portal.azure.com)uygulamanın **Tanılama ve çözme sorunlarını** da denetleyin.

### <a name="example-startup-commands"></a>Örnek başlangıç komutları

- **Gunicbir bağımsız değişken eklendi**: Aşağıdaki örnek, `--workers=4` bir dmongo uygulamasını başlatmak için gunicbir komut satırına ekler: 

    ```bash
    # <module-path> is the relative path to the folder that contains the module
    # that contains wsgi.py; <module> is the name of the folder containing wsgi.py.
    gunicorn --bind=0.0.0.0 --timeout 600 --workers=4 --chdir <module_path> <module>.wsgi
    ```    

    Daha fazla bilgi için bkz. [Gunicorn'u Çalıştırma](https://docs.gunicorn.org/en/stable/run.html) (docs.gunicorn.org).

- **Docgo için üretim günlüğünü etkinleştir**: `--access-logfile '-'` `--error-logfile '-'` komut satırına ve bağımsız değişkenlerini ekleyin:

    ```bash    
    # '-' for the log files means stdout for --access-logfile and stderr for --error-logfile.
    gunicorn --bind=0.0.0.0 --timeout 600 --workers=4 --chdir <module_path> <module>.wsgi --access-logfile '-' --error-logfile '-'
    ```    

    Bu Günlükler [App Service günlük akışında](#access-diagnostic-logs)görüntülenir.

    Daha fazla bilgi için bkz. [Gunicgınlogging](https://docs.gunicorn.org/en/stable/settings.html#logging) (docs.gunicorn.org).
    
- **Özel Flask ana modülü**: varsayılan olarak, App Service bir Flask uygulamasının ana modülünün *Application.py* veya *app.py*olduğunu varsayar. Ana modülünüzün farklı bir ad kullanması durumunda başlangıç komutunu özelleştirmeniz gerekir. Örneğin, YF, ana modülü *Hello.py* olan bir Flask uygulaması ve bu dosyadaki Flask uygulaması nesnesi olarak adlandırılmışsa `myapp` , komut aşağıdaki gibidir:

    ```bash
    gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
    ```
    
    Ana modülünüz `website` gibi bir alt klasör ise bu klasörü `--chdir` bağımsız değişkeniyle belirtin:
    
    ```bash
    gunicorn --bind=0.0.0.0 --timeout 600 --chdir website hello:myapp
    ```
    
- **Gunicıse sunucusu kullanın**: [aiohttp](https://aiohttp.readthedocs.io/en/stable/web_quickstart.html)gibi farklı bir Web sunucusu kullanmak için, başlangıç komutu olarak veya başlangıç komut dosyasında uygun komutu kullanın:

    ```bash
    python3.7 -m aiohttp.web -H localhost -P 8080 package.module:init_func
    ```

## <a name="access-app-settings-as-environment-variables"></a>Uygulama ayarlarına ortam değişkenleri olarak erişin

Uygulama ayarları, uygulama [ayarlarını yapılandırma](configure-common.md#configure-app-settings)bölümünde açıklandığı gibi, buluta özel olarak depolanmış değerlerdir. Bu ayarlar, uygulama kodunuzda ortam değişkenleri olarak kullanılabilir ve standart [OS. Environ](https://docs.python.org/3/library/os.html#os.environ) düzeniyle erişilir.

Örneğin, adlı uygulama ayarı oluşturduysanız `DATABASE_SERVER` , aşağıdaki kod bu ayarın değerini alır:

```python
db_server = os.environ['DATABASE_SERVER']
```
    
## <a name="detect-https-session"></a>HTTPS oturumunu Algıla

App Service, [SSL sonlandırma](https://wikipedia.org/wiki/TLS_termination_proxy) (wikipedia.org) ağ yükü dengeleyicilerde meydana gelir. bu nedenle, tüm https istekleri UYGULAMANıZA şifrelenmemiş HTTP istekleri olarak ulaşacak. Uygulama mantığınızın kullanıcı isteklerinin şifrelenip şifrelenmediğini denetlemesi gerekiyorsa, `X-Forwarded-Proto` üstbilgiyi inceleyin.

```python
if 'X-Forwarded-Proto' in request.headers and request.headers['X-Forwarded-Proto'] == 'https':
# Do something when HTTPS is used
```

Popüler Web çerçeveleri `X-Forwarded-*` Standart uygulama hiyerarşinizdeki bilgilere erişmenizi sağlar. [Codeigniter](https://codeigniter.com/)'da, [is_https ()](https://github.com/bcit-ci/CodeIgniter/blob/master/system/core/Common.php#L338-L365) `X_FORWARDED_PROTO` Varsayılan olarak değerini denetler.

## <a name="access-diagnostic-logs"></a>Tanılama günlüklerine erişim

[!INCLUDE [Access diagnostic logs](../../includes/app-service-web-logs-access-linux-no-h.md)]

Günlüklere Azure Portal erişmek için, **Monitoring**  >  uygulamanızın sol taraftaki menüsünde izleme**günlüğü akışı** ' nı seçin.

## <a name="open-ssh-session-in-browser"></a>Tarayıcıda SSH oturumu açma

[!INCLUDE [Open SSH session in browser](../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Sorun giderme

- **Kendi uygulama kodunuzu dağıttıktan sonra varsayılan uygulamayı görüyorsunuz.** App Service için uygulama kodunuzu dağıtmadığınız ya da App Service uygulama kodunuzu bulamadığı ve bunun yerine varsayılan uygulamayı çalıştırmadığınız için varsayılan uygulama görüntülenir.

    - App Service'i yeniden başlatın, 15-20 saniye bekleyin ve uygulamayı yeniden denetleyin.
    
    - Windows tabanlı örnek yerine Linux için App Service’i kullandığınızdan emin olun. Azure CLI’de `<resource-group-name>` ve `<app-service-name>` hizmetini uygun bir şekilde değiştiren `az webapp show --resource-group <resource-group-name> --name <app-name> --query kind` komutunu çalıştırın. Çıktı olarak `app,linux` görünmelidir, aksi takdirde App Service’i yeniden oluşturun ve Linux’u seçin.
    
    - SSH veya Kudu kullanarak doğrudan App Service'e bağlanın ve dosyalarınızın *site/wwwroot* dizininde bulunduğunu doğrulayın. Dosyalarınız orada değilse dağıtım işlemlerinizi gözden geçirin ve uygulamayı yeniden dağıtın.
    
    - Dosyalarınız oradaysa App Service başlangıç dosyanızı tanımlayamamış olabilir. Uygulamanızın App Service'in [Django](#django-app) veya [Flask](#flask-app) için beklediği şekilde yapılandırılmış olduğundan emin olun veya [özel başlangıç komutu](#customize-startup-command) kullanın.

- **Tarayıcıda "Hizmet Kullanılamıyor" iletisini görüyorsunuz.** Bu durum, tarayıcının App Service'ten yanıt beklerken zaman aşımına uğradığını gösterir. Bunun nedeni App Service'in Gunicorn sunucusunu başlatmış olması ancak uygulama kodunu belirten bağımsız değişkenlerin hatalı olmasıdır.

    - Özellikle App Service Planınızda en düşük fiyatlandırma katmanlarını kullanıyorsanız tarayıcıyı yenileyin. Ücretsiz katmanları kullandığınızda uygulamanın başlaması daha uzun sürebilir ve tarayıcıyı yenilediğinizde yanıt verebilir.

    - Uygulamanızın App Service'in [Django](#django-app) veya [Flask](#flask-app) için beklediği şekilde yapılandırılmış olduğundan emin olun veya [özel başlangıç komutu](#customize-startup-command) kullanın.

    - Herhangi bir hata iletisi için [günlük akışını](#access-diagnostic-logs) inceleyin.

- **Günlük akışında "Setup.py bulunamadı veya requirements.txt; Pyx Install çalışmıyor. "**: Oryx derleme işlemi *requirements.txt* dosyanızı bulamadı.

    - App Service doğrudan bağlanmak ve *requirements.txt* doğrudan *site/Wwwroot*altında var olduğunu doğrulamak Için SSH veya kudu konsolunu kullanın. Mevcut değilse, siteyi deponuzda mevcut yapın ve dağıtımınıza dahil edin. Ayrı bir klasörde varsa, onu köke taşıyın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: PostgreSQL ile Python uygulaması](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Öğretici: özel kapsayıcı deposundan dağıtma](tutorial-custom-container.md?pivots=container-linux)

> [!div class="nextstepaction"]
> [App Service Linux Hakkında SSS](faq-app-service-linux.md)

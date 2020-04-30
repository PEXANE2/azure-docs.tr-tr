---
title: 'Öğretici: Postgres ile Python (Docgo) dağıtma'
description: Bir PostgreSQL veritabanı ile Python uygulaması oluşturmayı ve Linux üzerinde Azure App Service için dağıtmayı öğrenin. Öğretici, tanıtım için bir Docgo örnek uygulaması kullanır.
ms.devlang: python
ms.topic: tutorial
ms.date: 04/14/2020
ms.custom:
- mvc
- seodec18
- seo-python-october2019
- cli-validate
ms.openlocfilehash: 0c9329b46d096df1afab6f7e457d143f9c6504be
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82085765"
---
# <a name="tutorial-deploy-a-python-django-web-app-with-postgresql-in-azure-app-service"></a>Öğretici: Azure App Service ' de PostgreSQL ile Python (Docgo) Web uygulaması dağıtma

Bu öğreticide, veri odaklı bir Python (Docgo) Web uygulamasının [Azure App Service](app-service-linux-intro.md) ve bunu PostgreSQL Için Azure veritabanı veritabanına nasıl bağlayacağınızı gösterir. App Service, yüksek düzeyde ölçeklenebilir, kendini yayama bir Web barındırma hizmeti sağlar.

![Azure App Service için Python Docgo Web uygulaması dağıtma](./media/tutorial-python-postgresql-app/deploy-python-django-app-in-azure.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * PostgreSQL için Azure veritabanı veritabanı oluşturma
> * Azure App Service kodu dağıtın ve Postgres 'e bağlanın
> * Kodunuzu güncelleştirin ve yeniden dağıtın
> * Tanılama günlüklerini görüntüleme
> * Azure portal web uygulamasını yönetme

Bu makaledeki adımları macOS, Linux veya Windows ' da izleyebilirsiniz.

## <a name="install-dependencies"></a>Bağımlılıkları yükleme

Bu öğreticiye başlamadan önce:

- [!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
- [Azure CLI](/cli/azure/install-azure-cli)'yı yükler.
- [Git](https://git-scm.com/)'i yükler.
- [Python 3](https://www.python.org/downloads/)' ü yükler.

## <a name="clone-the-sample-app"></a>Örnek uygulamayı kopyalama

Bir Terminal penceresinde, örnek uygulama deposunu kopyalamak ve depo köküne geçmek için aşağıdaki komutları çalıştırın:

```
git clone https://github.com/Azure-Samples/djangoapp
cd djangoapp
```

Docgoapp örnek deposu, Docgo belgelerine [Ilk Docgo uygulamanızı yazarak](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) aldığınız veri tabanlı [docgo](https://www.djangoproject.com/) yoklamaları uygulamasını içerir. Size kolaylık sağlamak için burada sunulmaktadır.

## <a name="prepare-app-for-app-service"></a>Uygulamayı App Service için hazırla

Birçok Python Web çerçevesi gibi, Docgo, [bir üretim sunucusunda çalıştırılmadan önce belirli değişiklikler gerektirir](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/)ve App Service farklı değildir. Uygulamanın App Service dağıtıldıktan sonra çalışması için varsayılan *azuresite/Settings. Kopyala* dosyasında bazı ayarları değiştirmeniz ve bazı ayarları eklemeniz gerekir. 

App Service için gerekli yapılandırmayı yapan *azuresite/Production. Kopyala*bölümüne göz atın. Kısaca şunları yapar:

- Tüm ayarları *azuresite/Settings. Kopyala*konumundan devralma.
- App Service uygulamasının tam etki alanı adını izin verilen konaklara ekleyin. 
- Docgo varsayılan olarak üretimde statik dosyalar hizmet vermediğinden, üretimde statik dosyalar sunma özelliğini etkinleştirmek için [Whitenoıse](https://whitenoise.evans.io/en/stable/) kullanın. Whitenoıse paketi, *requirements. txt*' ye zaten dahil edilmiştir.
- PostgreSQL veritabanı için yapılandırma ekleyin. Varsayılan olarak, Docgo veritabanı olarak SQLite3 kullanır, ancak üretim uygulamaları için uygun değildir. [Psycopg2-binary](https://pypi.org/project/psycopg2-binary/) paketi, *requirements. txt*dosyasına zaten dahildir.
- Postgres yapılandırması ortam değişkenlerini kullanır. Daha sonra, App Service ortam değişkenlerini nasıl ayarlayabileceğinizi öğreneceksiniz.

*azuresite/Production. Kopyala* , depoya kolaylık sağlaması için dahil edilmiştir, ancak henüz uygulama tarafından kullanılmıyor. App Service ayarların kullanıldığından emin olmak için, *Manage.py* ve *azuresite/wsgi. Kopyala*olmak üzere iki dosya yapılandırmanız gerekir.

- *Manage.py*içinde aşağıdaki satırı değiştirin:

    <pre>
    os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'azuresite.settings')
    </pre>

    Aşağıdaki koda:

    ```python
    if os.environ.get('DJANGO_ENV') == 'production':
        os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'azuresite.production')
    else:
        os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'azuresite.settings')
    ```

    App Service uygulamanızı yapılandırdığınızda, ortam değişkenini `DJANGO_ENV` daha sonra ayarlayacaksınız.

- *Azuresite/wsgi. Kopyala*içinde, yukarıdaki gibi aynı değişikliği yapın.

    App Service, veritabanı geçişlerini çalıştırmak için *Manage.py* kullanır ve App Service *azuresite/wsgi.* ını kullanarak üretimde docgo uygulamanızı çalıştırır. Her iki dosyada da bu değişiklik, üretim ayarlarının her iki durumda da kullanılmasını sağlar.

## <a name="sign-in-to-azure-cli"></a>Azure CLI'da oturum açma

Azure CLı 'nın zaten yüklü olması gerekir. [Azure CLI](/cli/azure/what-is-azure-cli) , komut satırı terminalinde Azure kaynaklarıyla çalışmanıza olanak sağlar. 

Azure 'da oturum açmak için şu [`az login`](/cli/azure/reference-index#az-login) komutu çalıştırın:

```azurecli
az login
```

Azure hesabınızda oturum açmak için terminaldeki yönergeleri izleyin. İşiniz bittiğinde, abonelikleriniz Terminal çıktısında JSON biçiminde gösterilir.

## <a name="create-postgres-database-in-azure"></a>Azure 'da Postgres veritabanı oluşturma

<!-- > [!NOTE]
> Before you create an Azure Database for PostgreSQL server, check which [compute generation](/azure/postgresql/concepts-pricing-tiers#compute-generations-and-vcores) is available in your region. If your region doesn't support Gen4 hardware, change *--sku-name* in the following command line to a value that's supported in your region, such as B_Gen4_1.  -->

Bu bölümde, PostgreSQL için Azure veritabanı sunucusu ve veritabanı oluşturacaksınız. Başlamak için, `db-up` uzantıyı aşağıdaki komutla birlikte yüklemelisiniz:

```azurecli
az extension add --name db-up
```

Aşağıdaki örnekte gösterildiği gibi, [`az postgres up`](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up) komutuyla Azure 'Da Postgres veritabanını oluşturun. * \<PostgreSQL-Name>* öğesini *benzersiz* bir adla değiştirin (sunucu uç noktası *\<https://PostgreSQL-Name>. Postgres.Database.Azure.com*). * \<Yönetici-Kullanıcı adı>* ve * \<yönetici-parola>* için, bu Postgres sunucusu için bir yönetici kullanıcı oluşturmak üzere kimlik bilgilerini belirtin.

<!-- Issue: without --location -->
```azurecli
az postgres up --resource-group myResourceGroup --location westus2 --server-name <postgresql-name> --database-name pollsdb --admin-user <admin-username> --admin-password <admin-password> --ssl-enforcement Enabled
```

Bu komut şu işlemleri yaparken biraz zaman alabilir:

- Mevcut değilse adlı `myResourceGroup`bir [kaynak grubu](../../azure-resource-manager/management/overview.md#terminology) oluşturur. Her Azure kaynağının bunlardan birinde olması gerekir. `--resource-group` isteğe bağlıdır.
- Yönetici kullanıcıyla bir Postgres sunucusu oluşturur.
- Bir `pollsdb` veritabanı oluşturur.
- Yerel IP adresinizden erişime izin verir.
- Azure hizmetlerinden erişime izin verir.
- `pollsdb` Veritabanına erişimi olan bir veritabanı kullanıcısı oluşturun.

Tüm adımları diğer `az postgres` komutlarla ayrı olarak yapabilirsiniz `psql`, ancak `az postgres up` tümünü tek bir adımda yapabilirsiniz.

Komut tamamlandığında, ile `Ran Database Query:`olan çıktı satırlarını bulun. Kullanıcı adı `root` ve parolasıyla `Pollsdb1`, sizin için oluşturulan veritabanı kullanıcısını gösterir. Uygulamanızı daha sonra veritabanına bağlamak için kullanacaksınız.

<!-- not all locations support az postgres up -->
> [!TIP]
> Postgres sunucunuzun konumunu belirtmek için, [Azure bölgelerinden](https://azure.microsoft.com/global-infrastructure/regions/)biri `<location_name>` olan bağımsız değişkenini `--location <location-name>`de ekleyin. [`az account list-locations`](/cli/azure/account#az-account-list-locations) Komutu ile aboneliğiniz için kullanılabilir bölgeleri edinebilirsiniz.

## <a name="deploy-the-app-service-app"></a>App Service uygulamasını dağıtma

Bu bölümde App Service uygulamasını oluşturacaksınız. Bu uygulamayı oluşturduğunuz Postgres veritabanına bağlayacaksınız ve kodunuzu dağıtırsınız.

### <a name="create-the-app-service-app"></a>App Service uygulamasını oluşturma

<!-- validation error: Parameter 'ResourceGroup.location' can not be None. -->
<!-- --resource-group is not respected at all -->

Uygulama bu dizinden dağıtılacağından depo köküne (`djangoapp`) geri olduğunuzdan emin olun.

Aşağıdaki örnekte gösterildiği gibi [`az webapp up`](/cli/azure/webapp#az-webapp-up) komutuyla bir App Service uygulaması oluşturun. * \<App-name>* değerini *benzersiz* bir adla değiştirin (sunucu uç noktası *https://\<app-name>. azurewebsites.net*). * \<App-name>* için izin verilen karakterler `A` - `Z`, `0` - `9`ve `-`' dir.

```azurecli
az webapp up --plan myAppServicePlan --sku B1 --name <app-name>
```
<!-- !!! without --sku creates PremiumV2 plan!! -->

Bu komut şu işlemleri yaparken biraz zaman alabilir:

<!-- - Create the resource group if it doesn't exist. `--resource-group` is optional. -->
<!-- No it doesn't. az webapp up doesn't respect --resource-group -->
- Otomatik olarak bir [kaynak grubu](../../azure-resource-manager/management/overview.md#terminology) oluşturur.
- Mevcut değilse, temel fiyatlandırma katmanında (B1), *myappserviceplan* [App Service planını](../overview-hosting-plans.md) oluşturur. `--plan`ve `--sku` isteğe bağlıdır.
- Yoksa App Service uygulamasını oluşturur.
- Zaten etkinleştirilmemişse, uygulama için varsayılan günlüğü etkinleştir.
- Derleme Otomasyonu etkinken ZIP dağıtımını kullanarak depoyu karşıya yükler.

Dağıtım tamamlandıktan sonra, aşağıdaki gibi bir JSON çıkışı görürsünüz:

<pre>
{
  "URL": "http://&lt;app-name&gt;.azurewebsites.net",
  "appserviceplan": "myAppServicePlan",
  "location": "westus",
  "name": "&lt;app-name&gt;",
  "os": "Linux",
  "resourcegroup": "&lt;app-resource-group&gt;",
  "runtime_version": "python|3.7",
  "runtime_version_detected": "-",
  "sku": "BASIC",
  "src_path": "//var//lib//postgresql//djangoapp"
}
</pre>

*App-Resource-Group>değerini kopyalayın. \< * Uygulamayı daha sonra yapılandırmak için gereklidir. 

> [!TIP]
> Daha sonra herhangi bir değişikliği dağıtmak ve tanılama günlüklerini hemen etkinleştirmek için aynı komutu kullanabilirsiniz:
> 
> ```azurecli
> az webapp up --name <app-name>
> ```

Örnek kod artık dağıtıldı, ancak uygulama Azure 'daki Postgres veritabanına henüz bağlanmıyor. Bunu bir sonraki adımda yapacaksınız.

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Uygulamanızı yerel olarak çalıştırdığınızda, Terminal oturumunda ortam değişkenlerini ayarlayabilirsiniz. App Service, [az WebApp config appSettings set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) komutunu kullanarak *uygulama ayarları*ile yapılır.

Veritabanı bağlantısı ayrıntılarını uygulama ayarları olarak belirtmek için aşağıdaki komutu çalıştırın. * \<App-name>*, * \<App-Resource-Group>* ve * \<PostgreSQL-Name>* değerlerini kendi değerlerinizle değiştirin. Kullanıcı kimlik bilgilerinin `root` `Pollsdb1` tarafından `az postgres up`sizin için oluşturulduğunu unutmayın.

```azurecli
az webapp config appsettings set --name <app-name> --resource-group <app-resource-group> --settings DJANGO_ENV="production" DBHOST="<postgresql-name>.postgres.database.azure.com" DBUSER="root@<postgresql-name>" DBPASS="Pollsdb1" DBNAME="pollsdb"
```

Kodunuzun bu uygulama ayarlarına nasıl eriştiği hakkında daha fazla bilgi için bkz. [ortam değişkenlerine erişim](how-to-configure-python.md#access-environment-variables).

### <a name="run-database-migrations"></a>Veritabanı geçişlerini Çalıştır

Veritabanı geçişlerini App Service içinde çalıştırmak için, *https://\<App-name>. scm.azurewebsites.net/webssh/Host*SAYFASıNA giderek tarayıcıda bir SSH oturumu açın:

<!-- doesn't work when container not started -->
<!-- ```azurecli
az webapp ssh --resource-group myResourceGroup --name <app-name>
``` -->

SSH oturumunda aşağıdaki komutları çalıştırın:

```bash
cd site/wwwroot

# Activate default virtual environment in App Service container
source /antenv/bin/activate
# Install requirements in environment
pip install -r requirements.txt
# Run database migrations
python manage.py migrate
# Create the super user (follow prompts)
python manage.py createsuperuser
```

### <a name="browse-to-the-azure-app"></a>Azure uygulamasına gidin

Bir tarayıcıda, *http:\//\<app-name>. azurewebsites.net* URL 'si ile dağıtılan uygulamaya gidin. **Hiçbir yoklama**yok iletisini görmeniz gerekir. 

*\//Http:\<app-name>. azurewebsites.net/admin* ' e gidin ve son adımda oluşturduğunuz yönetici kullanıcısını kullanarak oturum açın. **Soruların**yanına **Ekle** ' yi seçin ve bazı seçeneklerle bir yoklama sorusu oluşturun.

URL *http:\//\<app-name>. azurewebsites.net/admin*ile dağıtılan uygulamaya gidin ve bazı yoklama soruları oluşturun. Soruları *http:\//\<app-name>. azurewebsites.net/* adresinde görebilirsiniz. 

![Azure 'da uygulama hizmetlerinde Python Docgo uygulamasını çalıştırma](./media/tutorial-python-postgresql-app/deploy-python-django-app-in-azure.png)

Anket sorusunu görmek ve soruyu yanıtlamak için URL *http\//\<: app-name>. azurewebsites.net* ile dağıtılan uygulamaya gidin.

App Service, varsayılan olarak `manage.py startproject` oluşturulan her alt dizindeki bir *wsgi.py* dosyasını arayarak deponuzda bir docgo projesi algılar. App Service dosyayı bulduğunda Docgo Web uygulamasını yükler. App Service Python uygulamalarını nasıl yüklediği hakkında daha fazla bilgi için bkz. [yerleşik Python görüntüsünü yapılandırma](how-to-configure-python.md).

**Tebrikler!** Linux için Azure App Service bir Python (Docgo) Web uygulaması çalıştırıyorsunuz.

## <a name="develop-app-locally-and-redeploy"></a>Uygulamayı yerel olarak geliştirme ve yeniden dağıtma

Bu bölümde, uygulamanızı yerel ortamınızda geliştirir ve kodunuzu App Service olarak yeniden dağıtabilirsiniz.

### <a name="set-up-locally-and-run"></a>Yerel olarak ayarlama ve çalıştırma

Yerel geliştirme ortamınızı ayarlamak ve örnek uygulamayı ilk kez çalıştırmak için aşağıdaki komutları çalıştırın:

# <a name="bash"></a>[Bash](#tab/bash)

```bash
# Configure the Python virtual environment
python3 -m venv venv
source venv/bin/activate

# Install packages
pip install -r requirements.txt
# Run Django migrations
python manage.py migrate
# Create Django superuser (follow prompts)
python manage.py createsuperuser
# Run the dev server
python manage.py runserver
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
# Configure the Python virtual environment
py -3 -m venv venv
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
venv\scripts\activate

# Install packages
pip install -r requirements.txt
# Run Django migrations
python manage.py migrate
# Create Django superuser (follow prompts)
python manage.py createsuperuser
# Run the dev server
python manage.py runserver
```

# <a name="cmd"></a>[CMD](#tab/cmd)

```CMD
:: Configure the Python virtual environment
py -3 -m venv venv
venv\scripts\activate

:: Install packages
pip install -r requirements.txt
:: Run Django migrations
python manage.py migrate
:: Create Django superuser (follow prompts)
python manage.py createsuperuser
:: Run the dev server
python manage.py runserver
```
---

Docgo Web uygulaması tam olarak yüklendiğinde aşağıdaki iletiye benzer bir şey döndürür:

<pre>
Performing system checks...

System check identified no issues (0 silenced).
December 13, 2019 - 10:54:59
Django version 2.1.2, using settings 'azuresite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
</pre>

Bir tarayıcıda *http:\//localhost: 8000* adresine gidin. **Hiçbir yoklama**yok iletisini görmeniz gerekir. 

*Http:\//localhost: 8000/admin* adresine gidin ve son adımda oluşturduğunuz yönetici kullanıcısını kullanarak oturum açın. **Soruların**yanına **Ekle** ' yi seçin ve bazı seçeneklerle bir yoklama sorusu oluşturun.

![Uygulama hizmetlerinde yerel olarak Python Docgo uygulaması çalıştırma](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

Anket sorusunu görmek ve soruyu yanıtlamak için *http:\//localhost: 8000* ' e gidin. Yerel Docgo örnek uygulaması, Kullanıcı verilerini yerel bir SQLite3 veritabanına yazar ve depolar, böylece üretim veritabanınızı oluştururken endişelenmeniz gerekmez. Geliştirme ortamınızı Azure ortamıyla eşleştirmek için, bunun yerine yerel olarak bir Postgres veritabanı kullanmayı göz önünde bulundurun.

Docgo sunucusunu durdurmak için CTRL + C yazın.

### <a name="update-the-app"></a>Uygulamayı güncelleştirme

Uygulama güncelleştirmelerinin nasıl çalıştığını görmek için, içinde `polls/models.py`küçük bir değişiklik yapın. Satırı bulun:

<pre>
choice_text = models.CharField(max_length=200)
</pre>

Ve şu şekilde değiştirin:

```python
choice_text = models.CharField(max_length=100)
```

Veri modelini değiştirerek yeni bir Docgo geçişi oluşturmanız gerekir. Aşağıdaki komutla bunu yapın:

```
python manage.py makemigrations
```

Geçişleri çalıştırarak, geliştirme sunucusunu çalıştırarak ve *http:\//localhost: 8000/admin*' e giderek değişikliklerinizi yerel olarak test edebilirsiniz.

```
python manage.py migrate
python manage.py runserver
```

### <a name="redeploy-code-to-azure"></a>Kodu Azure 'a yeniden dağıtın

Değişiklikleri yeniden dağıtmak için depo kökünden aşağıdaki komutu çalıştırın:

```azurecli
az webapp up --name <app-name>
```

App Service, uygulamanın mevcut olduğunu algılar ve yalnızca kodu dağıtır.

### <a name="rerun-migrations-in-azure"></a>Azure 'da geçişleri yeniden çalıştırma

Veri modelinde değişiklikler yaptığınız için, App Service veritabanı geçişlerini yeniden çalıştırmanız gerekir. *Https://\<app-name>. scm.azurewebsites.net/webssh/Host*SAYFASıNA giderek tarayıcıda bir SSH oturumu açın. Aşağıdaki komutları çalıştırın:

```
cd site/wwwroot

# Activate default virtual environment in App Service container
source /antenv/bin/activate
# Run database migrations
python manage.py migrate
```

### <a name="review-app-in-production"></a>Üretimde uygulamayı gözden geçirin

*\//Http:\<app-name>. azurewebsites.net* öğesine gidin ve üretimde canlı olarak çalışan değişiklikleri görün. 

## <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

Kapsayıcının içinden oluşturulan konsol günlüklerine erişebilirsiniz.

> [!TIP]
> `az webapp up`Varsayılan günlük kaydını etkinleştirir. Performans nedenleriyle, bu günlüğe kaydetme bir süre sonra devre dışı bırakır, ancak yeniden çalıştırdığınızda `az webapp up` her seferinde geri döner. El ile açmak için aşağıdaki komutu çalıştırın:
>
> ```azurecli
> az webapp log config --name <app-name> --resource-group <app-resource-group> --docker-container-logging filesystem
> ```

Günlük akışını görmek için aşağıdaki Azure CLı komutunu çalıştırın:

```azurecli
az webapp log tail --name <app-name> --resource-group <app-resource-group>
```

Konsol günlüklerini hemen görmüyorsanız, 30 saniye içinde yeniden kontrol edin.

> [!NOTE]
> Ayrıca, konumundaki `https://<app-name>.scm.azurewebsites.net/api/logs/docker`tarayıcıdan günlük dosyalarını inceleyebilirsiniz.

Günlük akışını istediğiniz zaman durdurmak için, yazın `Ctrl` + `C`.

## <a name="manage-your-app-in-the-azure-portal"></a>Uygulamanızı Azure portal yönetme

[Azure Portal](https://portal.azure.com)oluşturduğunuz uygulamayı arayıp seçin.

![Azure portal Python Docgo uygulamanıza gidin](./media/tutorial-python-postgresql-app/navigate-to-django-app-in-app-services-in-the-azure-portal.png)

Varsayılan olarak, Portal uygulamanızın **genel bakış** sayfasını gösterir. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Burada, gezinme, durdurma, yeniden başlatma ve silme gibi temel yönetim görevlerini de gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Python Docgo uygulamanızı Azure portal genel bakış sayfasında yönetin](./media/tutorial-python-postgresql-app/manage-django-app-in-app-services-in-the-azure-portal.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Gelecekte bu kaynakların gerekli olmasını istemiyorsanız, aşağıdaki komutları çalıştırarak kaynak gruplarını silin:

```azurecli
az group delete --name myResourceGroup
az group delete --name <app-resource-group>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> * PostgreSQL için Azure veritabanı veritabanı oluşturma
> * Azure App Service kodu dağıtın ve Postgres 'e bağlanın
> * Kodunuzu güncelleştirin ve yeniden dağıtın
> * Tanılama günlüklerini görüntüleme
> * Azure portal web uygulamasını yönetme

Özel bir DNS adını uygulamanıza nasıl eşleyeceğinizi öğrenmek için bir sonraki Öğreticiye gidin:

> [!div class="nextstepaction"]
> [Öğretici: özel DNS adını uygulamanıza eşleyin](../app-service-web-tutorial-custom-domain.md)

Ya da diğer kaynaklara göz atın:

> [!div class="nextstepaction"]
> [Python uygulamasını yapılandırma](how-to-configure-python.md)

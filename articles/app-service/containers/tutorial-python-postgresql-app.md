---
title: 'Öğretici: Postgres ile Python (Django) dağıtın'
description: PostgreSQL veritabanına sahip bir Python uygulamasını nasıl oluşturup Linux'taki Azure Uygulama Hizmeti'ne nasıl dağıtılayabilirsiniz öğrenin. Öğretici gösteri için bir Django örnek uygulaması kullanır.
ms.devlang: python
ms.topic: tutorial
ms.date: 04/14/2020
ms.custom:
- mvc
- seodec18
- seo-python-october2019
- cli-validate
ms.openlocfilehash: 0c9329b46d096df1afab6f7e457d143f9c6504be
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085765"
---
# <a name="tutorial-deploy-a-python-django-web-app-with-postgresql-in-azure-app-service"></a>Öğretici: Azure Uygulama Hizmetinde PostgreSQL ile Python (Django) web uygulamasını dağıtma

Bu öğretici, veri odaklı bir Python (Django) web uygulamasının [Azure Uygulama Hizmetine](app-service-linux-intro.md) nasıl dağıtılanın ve PostgreSQL veritabanı için bir Azure Veritabanına nasıl bağlanıştırılagerektiğini gösterir. App Service, yüksek oranda ölçeklenebilir, kendi kendini yamalayan bir web barındırma hizmeti sunar.

![Python Django web uygulamasını Azure Uygulama Hizmetine dağıtın](./media/tutorial-python-postgresql-app/deploy-python-django-app-in-azure.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * PostgreSQL veritabanı için bir Azure Veritabanı oluşturma
> * Kodu Azure Uygulama Hizmetine dağıtın ve Postgres'e bağlanın
> * Kodunuzu güncelleştirin ve yeniden dağıtın
> * Tanılama günlüklerini görüntüleme
> * Azure portalındaki web uygulamasını yönetme

MacOS, Linux veya Windows ile ilgili bu makaledeki adımları izleyebilirsiniz.

## <a name="install-dependencies"></a>Bağımlılıkları yükleme

Bu öğreticibaşlamadan önce:

- [!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
- [Azure CLI'yi yükleyin.](/cli/azure/install-azure-cli)
- [Git'i yükleyin.](https://git-scm.com/)
- [Python 3'e](https://www.python.org/downloads/)yükleyin.

## <a name="clone-the-sample-app"></a>Örnek uygulamayı kopyalama

Terminal penceresinde, örnek uygulama deposunu klonlamak ve depo köküne değiştirmek için aşağıdaki komutları çalıştırın:

```
git clone https://github.com/Azure-Samples/djangoapp
cd djangoapp
```

Djangoapp örnek deposu, Django belgelerinde [ilk Django uygulamanızı yazarak](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) elde ettiğiniz veri odaklı [Django](https://www.djangoproject.com/) anket uygulamasını içerir. Burada sizin rahatınız için sağlanmıştır.

## <a name="prepare-app-for-app-service"></a>Uygulama Hizmeti için uygulama hazırlayın

Birçok Python web çerçevesi gibi, Django [da bir üretim sunucusunda çalıştırılmadan önce belirli değişiklikler gerektirir](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/)ve Uygulama Hizmeti ile de durum farklı değildir. Uygulamanın Uygulama Hizmeti'ne dağıtıldıktan sonra çalışması için varsayılan *azuresite/settings.py* dosyasındaki bazı ayarları değiştirmeniz ve eklemeniz gerekir. 

Uygulama Hizmeti için gerekli yapılandırmayı yapan *azuresite/production.py'ye*bir göz atın. Kısaca, aşağıdakileri yapar:

- *Azuresite/settings.py*adresinden tüm ayarları devral.
- Uygulama Hizmeti uygulamasının tam nitelikli alan adını izin verilen ana bilgisayarlara ekleyin. 
- Django varsayılan olarak üretimde statik dosyalara hizmet etmediğinden, üretimde statik dosyaların sunulmasını etkinleştirmek için [WhiteNoise'u](https://whitenoise.evans.io/en/stable/) kullanın. WhiteNoise paketi zaten *requirements.txt*dahildir.
- PostgreSQL veritabanı için yapılandırma ekleyin. Varsayılan olarak, Django veritabanı olarak Sqlite3 kullanır, ancak üretim uygulamaları için uygun değildir. [Psycopg2-ikili](https://pypi.org/project/psycopg2-binary/) paket zaten *requirements.txt*dahildir.
- Postgres yapılandırması ortam değişkenlerini kullanır. Daha sonra, Uygulama Hizmeti'nde ortam değişkenlerini nasıl ayarlacağınızı öğreneceksiniz.

*azuresite/production.py* kolaylık sağlamak için depoya dahildir, ancak uygulama tarafından henüz kullanılmamaktadır. Ayarlarının App Service'de kullanıldığından emin olmak için, bu dosyaya erişmek için *manage.py* ve *azuresite/wsgi.py*olmak üzere iki dosyayı yapılandırmanız gerekir.

- manage.py *manage.py*olarak, aşağıdaki satırı değiştirin:

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

    Uygulama Hizmeti uygulamanızı `DJANGO_ENV` yapılandırdığınızda ortam değişkenini daha sonra ayarlarsınız.

- *Azuresite/wsgi.py*olarak, yukarıdaki gibi aynı değişikliği yapın.

    Uygulama Hizmeti'nde, veritabanı geçişlerini çalıştırmak için *manage.py* kullanırsınız ve App Service, Django uygulamanızı üretimde çalıştırmak için *azuresite/wsgi.py* kullanır. Her iki dosyadaki bu değişiklik, her iki durumda da üretim ayarlarının kullanılmasını sağlar.

## <a name="sign-in-to-azure-cli"></a>Azure CLI'da oturum açma

Azure CLI'yi zaten yüklemiş olmalısınız. [Azure CLI,](/cli/azure/what-is-azure-cli) komut satırı terminalinden Azure kaynaklarıyla çalışmanızı sağlar. 

Azure'da oturum açabilmek için aşağıdaki komutu çalıştırın: [`az login`](/cli/azure/reference-index#az-login)

```azurecli
az login
```

Azure hesabınızda oturum açmanız için terminaldeki yönergeleri izleyin. Bitirdikten sonra, abonelikleriniz terminal çıkışında JSON formatında gösterilir.

## <a name="create-postgres-database-in-azure"></a>Azure'da Postgres veritabanı oluşturma

<!-- > [!NOTE]
> Before you create an Azure Database for PostgreSQL server, check which [compute generation](/azure/postgresql/concepts-pricing-tiers#compute-generations-and-vcores) is available in your region. If your region doesn't support Gen4 hardware, change *--sku-name* in the following command line to a value that's supported in your region, such as B_Gen4_1.  -->

Bu bölümde, PostgreSQL sunucusu ve veritabanı için bir Azure Veritabanı oluşturursunuz. Başlatmak için uzantıyı aşağıdaki komutla yükleyin: `db-up`

```azurecli
az extension add --name db-up
```

Aşağıdaki örnekte gösterildiği [`az postgres up`](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up) gibi, Komutla Birlikte Azure'da Postgres veritabanını oluşturun. * \<Postgresql adı>* *benzersiz* bir adla değiştirin (sunucu bitiş noktası *\<https:// postgresql adı>.postgres.database.azure.com).* Admin-kullanıcı adı>ve * \<yönetici-parola>* için, bu Postgres sunucusu için bir yönetici kullanıcı oluşturmak için kimlik bilgilerini belirtin. * \<*

<!-- Issue: without --location -->
```azurecli
az postgres up --resource-group myResourceGroup --location westus2 --server-name <postgresql-name> --database-name pollsdb --admin-user <admin-username> --admin-password <admin-password> --ssl-enforcement Enabled
```

Aşağıdakileri yaptığı için bu komut biraz zaman alabilir:

- Yoksa, [resource group](../../azure-resource-manager/management/overview.md#terminology) adlı `myResourceGroup`bir kaynak grubu oluşturur. Her Azure kaynağının bunlardan birinde olması gerekir. `--resource-group` isteğe bağlıdır.
- Yönetim kullanıcısıyla bir Postgres sunucusu oluşturur.
- Bir `pollsdb` veritabanı oluşturur.
- Yerel IP adresinizden erişim sağlar.
- Azure hizmetlerinden erişime izin verir.
- `pollsdb` Veritabanına erişimi olan bir veritabanı kullanıcısı oluşturun.

Tüm adımları diğer `az postgres` komutlarla ayrı ayrı `psql`yapabilirsiniz `az postgres up` ve bunların hepsini sizin için tek bir adımda yapabilirsiniz.

Komut bittiğinde, '' ile `Ran Database Query:`olan çıkış satırlarını bulun. Sizin için oluşturulan veritabanı kullanıcısını kullanıcı adı `root` ve parolayla `Pollsdb1`gösterirler. Uygulamanızı veritabanına bağlamak için bunları daha sonra kullanırsınız.

<!-- not all locations support az postgres up -->
> [!TIP]
> Postgres sunucunuzun konumunu belirtmek için, `--location <location-name>`Azure `<location_name>` [bölgelerinden](https://azure.microsoft.com/global-infrastructure/regions/)birinin bulunduğu bağımsız değişkeni ekleyin. Aboneliğiniz için kullanılabilen bölgeleri [`az account list-locations`](/cli/azure/account#az-account-list-locations) komutla alabilirsiniz.

## <a name="deploy-the-app-service-app"></a>Uygulama Hizmeti uygulamasını dağıtma

Bu bölümde, Uygulama Hizmeti uygulamasını oluşturursunuz. Bu uygulamayı oluşturduğunuz Postgres veritabanına bağlar ve kodunuzu dağıtabilirsiniz.

### <a name="create-the-app-service-app"></a>Uygulama Hizmeti uygulamasını oluşturma

<!-- validation error: Parameter 'ResourceGroup.location' can not be None. -->
<!-- --resource-group is not respected at all -->

Uygulama bu dizinden dağıtıladığından, depo`djangoapp`köküne geri döndüğünüzde emin olun.

Aşağıdaki örnekte gösterildiği [`az webapp up`](/cli/azure/webapp#az-webapp-up) gibi komutla birlikte bir Uygulama Hizmeti uygulaması oluşturun. * \<Uygulama adı>* *benzersiz* bir adla değiştirin (sunucu bitiş noktası *>.azurewebsites.net https://\<uygulama adıdır).* Uygulama adı * \<>* `A` - `Z`için izin `0` - `9`verilen `-`karakterler , , ve .

```azurecli
az webapp up --plan myAppServicePlan --sku B1 --name <app-name>
```
<!-- !!! without --sku creates PremiumV2 plan!! -->

Aşağıdakileri yaptığı için bu komut biraz zaman alabilir:

<!-- - Create the resource group if it doesn't exist. `--resource-group` is optional. -->
<!-- No it doesn't. az webapp up doesn't respect --resource-group -->
- Otomatik olarak bir [kaynak grubu](../../azure-resource-manager/management/overview.md#terminology) oluşturur.
- [Uygulama Hizmeti planı](../overview-hosting-plans.md) *myAppServicePlan'ı* Temel fiyatlandırma katmanında (B1) oluşturur, eğer yoksa. `--plan`ve `--sku` isteğe bağlıdır.
- Yoksa Uygulama Hizmeti uygulamasını oluşturur.
- Zaten etkin değilse, uygulama için varsayılan günlüğe kaydetmeyi etkinleştiri.
- Posta dağıtımı kullanılarak, yapı otomasyonu etkinleştirilmiş depoyu yükler.

Dağıtım tamamlandığında, aşağıdaki gibi bir JSON çıkışı görürsünüz:

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

Uygulama kaynak grubunun değerini>. * \< * Uygulamayı daha sonra yapılandırmak için bu uygulamagerekir. 

> [!TIP]
> Herhangi bir değişikliği dağıtmak ve tanılama günlüklerini aşağıdakilerle hemen etkinleştirmek için daha sonra aynı komutu kullanabilirsiniz:
> 
> ```azurecli
> az webapp up --name <app-name>
> ```

Örnek kod artık dağıtıldı, ancak uygulama henüz Azure'daki Postgres veritabanına bağlanmıyor. Bir dahaki sefere bunu yapacaksın.

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Uygulamanızı yerel olarak çalıştırdığınızda, terminal oturumunda ortam değişkenlerini ayarlayabilirsiniz. App Service'te, [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) komutunu kullanarak *uygulama ayarlarıyla*bunu yaparsınız.

Veritabanı bağlantı ayrıntılarını uygulama ayarları olarak belirtmek için aşağıdaki komutu çalıştırın. * \<Uygulama adı>, * * \<uygulama kaynak grubu>* ve * \<postgresql adı>'yi* kendi değerlerinizle değiştirin. Kullanıcı kimlik `root` bilgilerinin `Pollsdb1` sizin `az postgres up`için .

```azurecli
az webapp config appsettings set --name <app-name> --resource-group <app-resource-group> --settings DJANGO_ENV="production" DBHOST="<postgresql-name>.postgres.database.azure.com" DBUSER="root@<postgresql-name>" DBPASS="Pollsdb1" DBNAME="pollsdb"
```

Kodunuzun bu uygulama ayarlarına nasıl eriştiği hakkında bilgi için Access [ortamı değişleri](how-to-configure-python.md#access-environment-variables)gününe bakın.

### <a name="run-database-migrations"></a>Veritabanı geçişlerini çalıştırma

Uygulama Hizmeti'nde veritabanı geçişlerini çalıştırmak *için,>.scm.azurewebsites.net/webssh/host https://\<uygulama adına*gezinerek tarayıcıda bir SSH oturumu açın:

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

### <a name="browse-to-the-azure-app"></a>Azure uygulamasına göz atın

Dağıtılan uygulamaya URL http ile göz *\//\<atın: bir tarayıcıda uygulama adı>.azurewebsites.net.* İletiyi görmeniz gerekir **Yok anketler için var.** 

*http'ye göz\//\<atın: app-name>.azurewebsites.net/admin* ve son adımda oluşturduğunuz yönetici kullanıcısını kullanarak oturum açın. **Soruların**yanına **Ekle'yi** seçin ve bazı seçenekleriçeren bir anket sorusu oluşturun.

Dağıtılan uygulamaya URL http ile göz *\//\<atın:>.azurewebsites.net/admin app-name*ve bazı anket soruları oluşturun. Soruları *http: app-name\//\<>.azurewebsites.net/* adresinde niçin görebilirsiniz: 

![Azure'da Uygulama Hizmetlerinde Python Django uygulamasını çalıştırın](./media/tutorial-python-postgresql-app/deploy-python-django-app-in-azure.png)

URL http ile dağıtılan uygulamaya göz *\//\<atın: anket* sorusunu görmek ve soruyu yanıtlamak için yeniden>.azurewebsites.net app-name.

App Service, her alt dizinde varsayılan olarak `manage.py startproject` bir *wsgi.py* dosyası arayarak deponuzda bir Django projesi algılar. App Service dosyayı bulduğunda, Django web uygulamasını yükler. Uygulama Hizmetinin Python uygulamalarını nasıl yükleleri hakkında daha fazla bilgi için yerleşik [Python görüntüsünü yapılandırma'ya](how-to-configure-python.md)bakın.

**Tebrikler!** Linux için Azure Uygulama Hizmeti'nde bir Python (Django) web uygulaması çalıştırıyorsunuz.

## <a name="develop-app-locally-and-redeploy"></a>Uygulamayı yerel olarak geliştirin ve yeniden dağıtın

Bu bölümde, uygulamanızı yerel ortamınızda geliştirebilir ve kodunuzu Uygulama Hizmeti'ne yeniden dağıtabilirsiniz.

### <a name="set-up-locally-and-run"></a>Yerel olarak ayarlayın ve çalıştırın

Yerel geliştirme ortamınızı ayarlamak ve örnek uygulamayı ilk kez çalıştırmak için aşağıdaki komutları çalıştırın:

# <a name="bash"></a>[bash](#tab/bash)

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

# <a name="cmd"></a>[Cmd](#tab/cmd)

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

Django web uygulaması tam olarak yüklendiğinde, aşağıdaki mesaj gibi bir şey döndürür:

<pre>
Performing system checks...

System check identified no issues (0 silenced).
December 13, 2019 - 10:54:59
Django version 2.1.2, using settings 'azuresite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
</pre>

*http'ye\/gidin: /localhost:8000* tarayıcıda. İletiyi görmeniz gerekir **Yok anketler için var.** 

*\/http:/localhost:8000/admin'e* gidin ve son adımda oluşturduğunuz yönetici kullanıcısını kullanarak oturum açın. **Soruların**yanına **Ekle'yi** seçin ve bazı seçenekleriçeren bir anket sorusu oluşturun.

![Python Django uygulamasını Uygulama Hizmetleri'nde yerel olarak çalıştırın](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

Http *gidin:\//localhost:8000* tekrar anket soru görmek ve soruya cevap. Yerel Django örnek uygulaması kullanıcı verilerini yerel bir Sqlite3 veritabanına yazar ve depolar, böylece üretim veritabanınızı berbat etme konusunda endişelenmenize gerek kalmaz. Geliştirme ortamınızın Azure ortamıyla eşleşinsin, bunun yerine bir Postgres veritabanını yerel olarak kullanmayı düşünün.

Django sunucusunu durdurmak için Ctrl+C yazın.

### <a name="update-the-app"></a>Uygulamayı güncelleştirin

Uygulama güncelleştirmelerini nasıl yapmak için küçük bir `polls/models.py`değişiklik yapın. Satırı bulun:

<pre>
choice_text = models.CharField(max_length=200)
</pre>

Ve değiştirin:

```python
choice_text = models.CharField(max_length=100)
```

Veri modelini değiştirerek, yeni bir Django geçişi oluşturmanız gerekir. Aşağıdaki komutla yapın:

```
python manage.py makemigrations
```

Geçişleri çalıştırarak, geliştirme sunucusunu çalıştırarak ve *http://localhost:8000/admin adresine\/* yönlendirerek değişikliklerinizi yerel olarak sınlayabilirsiniz: /localhost:8000/admin:

```
python manage.py migrate
python manage.py runserver
```

### <a name="redeploy-code-to-azure"></a>Kodu Azure'a yeniden dağıtma

Değişiklikleri yeniden dağıtmak için, depo kökünden aşağıdaki komutu çalıştırın:

```azurecli
az webapp up --name <app-name>
```

Uygulama Hizmeti, uygulamanın var olduğunu algılar ve kodu dağıtabilir.

### <a name="rerun-migrations-in-azure"></a>Azure'da geçişleri yeniden çalıştırın

Veri modelinde değişiklik yaptığınızdan, Uygulama Hizmeti'nde veritabanı geçişlerini yeniden çalıştırmanız gerekir. *>.scm.azurewebsites.net/webssh/host https://\<uygulama adına*giderek tarayıcıda bir SSH oturumu açın. Aşağıdaki komutları çalıştırın:

```
cd site/wwwroot

# Activate default virtual environment in App Service container
source /antenv/bin/activate
# Run database migrations
python manage.py migrate
```

### <a name="review-app-in-production"></a>Üretimde uygulamayı gözden geçirme

http'ye göz *atın:\//\<>.azurewebsites.net uygulama adı* ve üretimde canlı çalışan değişiklikleri görün. 

## <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

Kapsayıcının içinden oluşturulan konsol günlüklerine erişebilirsiniz.

> [!TIP]
> `az webapp up`sizin için varsayılan günlüğe kaydetmeyi açar. Performans nedenleriyle, bu günlüğe kaydetme bir süre sonra kendiliğinden `az webapp up` kapanır, ancak yeniden çalıştırdığınızda tekrar açar. El ile açmak için aşağıdaki komutu çalıştırın:
>
> ```azurecli
> az webapp log config --name <app-name> --resource-group <app-resource-group> --docker-container-logging filesystem
> ```

Günlük akışını görmek için aşağıdaki Azure CLI komutunu çalıştırın:

```azurecli
az webapp log tail --name <app-name> --resource-group <app-resource-group>
```

Konsol günlüklerini hemen görmüyorsanız, 30 saniye içinde yeniden kontrol edin.

> [!NOTE]
> Ayrıca tarayıcıdan günlük dosyalarını da `https://<app-name>.scm.azurewebsites.net/api/logs/docker`inceleyebilirsiniz.

Günlük akışını herhangi bir zamanda `Ctrl` + `C`durdurmak için .

## <a name="manage-your-app-in-the-azure-portal"></a>Uygulamanızı Azure portalında yönetme

Azure [portalında,](https://portal.azure.com)oluşturduğunuz uygulamayı arayın ve seçin.

![Azure portalındaki Python Django uygulamanıza gidin](./media/tutorial-python-postgresql-app/navigate-to-django-app-in-app-services-in-the-azure-portal.png)

Varsayılan olarak, portal uygulamanızın **Genel Bakış** sayfasını gösterir. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Burada, göz atma, durdurma, yeniden başlatma ve silme gibi temel yönetim görevlerini de gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Python Django uygulamanızı Azure portalındaki Genel Bakış sayfasında yönetme](./media/tutorial-python-postgresql-app/manage-django-app-in-app-services-in-the-azure-portal.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Gelecekte bu kaynaklara ihtiyaç duymayı beklemiyorsanız, aşağıdaki komutları çalıştırarak kaynak gruplarını silin:

```azurecli
az group delete --name myResourceGroup
az group delete --name <app-resource-group>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> * PostgreSQL veritabanı için bir Azure Veritabanı oluşturma
> * Kodu Azure Uygulama Hizmetine dağıtın ve Postgres'e bağlanın
> * Kodunuzu güncelleştirin ve yeniden dağıtın
> * Tanılama günlüklerini görüntüleme
> * Azure portalındaki web uygulamasını yönetme

Özel bir DNS adının uygulamanızla nasıl eşleştini öğrenmek için bir sonraki öğreticiye gidin:

> [!div class="nextstepaction"]
> [Öğretici: Uygulamanıza özel DNS adını haritalandırın](../app-service-web-tutorial-custom-domain.md)

Veya diğer kaynaklara göz atın:

> [!div class="nextstepaction"]
> [Python uygulamasını yapılandırma](how-to-configure-python.md)

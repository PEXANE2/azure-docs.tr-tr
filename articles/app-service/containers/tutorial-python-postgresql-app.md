---
title: 'Öğretici: Postgre ile Linux Python uygulaması'
description: Azure'daki Bir PostgreSQL veritabanıyla bağlantılı bir Linux Python uygulamasını Azure Uygulama Hizmeti'nde nasıl çalıştırarak nasıl çalıştıran bir Linux Python uygulamasını öğrenin. Django bu eğitimde kullanılır.
ms.devlang: python
ms.topic: tutorial
ms.date: 01/23/2020
ms.custom:
- mvc
- seodec18
- seo-python-october2019
- cli-validate
ms.openlocfilehash: 13431b62e64774a4c31cf95200def3ba77f973d7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77523952"
---
# <a name="tutorial-run-a-python-django-web-app-with-postgresql-in-azure-app-service"></a>Öğretici: Azure Uygulama Hizmetinde PostgreSQL ile Python (Django) web uygulamasını çalıştırın

[Azure App Service](app-service-linux-intro.md), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğretici, veri odaklı python Django web uygulamasını PostgreSQL veritabanı için bir Azure Veritabanı'na nasıl bağlayabilirsiniz ve uygulamayı Azure Uygulama Hizmeti'nde nasıl dağıtıp çalıştıracağımı gösterir.

![Azure Uygulama Hizmeti'nde Python Django web uygulaması](./media/tutorial-python-postgresql-app/run-python-django-app-in-azure.png)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * PostgreSQL veritabanı için bir Azure Veritabanı oluşturun ve bir web uygulamasını ona bağlayın
> * Web uygulamasını Azure Uygulama Hizmetine dağıtma
> * Tanılama günlüklerini görüntüleme
> * Azure portalındaki web uygulamasını yönetme

MacOS, Linux veya Windows ile ilgili bu makaledeki adımları izleyebilirsiniz. Farklılıklar bu öğreticide ayrıntılı olmasa da, adımlar çoğu durumda benzerdir. Aşağıdaki örneklerin çoğu `bash` Linux üzerinde bir terminal penceresi kullanır. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticibaşlamadan önce:

- [!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
- [Git'i yükleyin.](https://git-scm.com/)
- [Python 3'e](https://www.python.org/downloads/)yükleyin.
- [PostgreSQL'i](https://www.postgresql.org/download/)yükleyin ve çalıştırın.

## <a name="test-postgresql-installation-and-create-a-database"></a>Test PostgreSQL yükleme ve bir veritabanı oluşturmak

İlk olarak, yerel PostgreSQL sunucunuza bağlanın ve bir veritabanı oluşturun: 

Yerel bir terminal penceresinde, yerleşik `psql` `postgres` kullanıcı olarak yerel PostgreSQL sunucunuza bağlanmak için çalıştırın.

```bash
sudo su - postgres
psql
```
or
```PowerShell
psql -U postgres
```

Bağlantınız başarılı olursa, PostgreSQL veritabanınız çalışır. Aksi takdirde, yerel PostgresQL veritabanınızın [İndirmeler - PostgreSQL Çekirdek Dağıtım](https://www.postgresql.org/download/) konusunda işletim sisteminiz için yönergeler izlenilerek başlatıldığından emin olun.

*Pollsdb*adında yeni bir veritabanı oluşturun ve parola *supersecretpass*ile *yönetici* adlı bir veritabanı kullanıcıkurmak:

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

PostgreSQL istemcisinden çıkmak için `\q` yazın.

<a name="step2"></a>
## <a name="create-and-run-the-local-python-app"></a>Yerel Python uygulamasını oluşturma ve çalıştırma

Ardından, python Django web uygulamasını ayarlayın ve çalıştırın.

[Djangoapp](https://github.com/Azure-Samples/djangoapp) örnek deposu, [Django](https://www.djangoproject.com/) belgelerinde [ilk Django uygulamanızı yazarak](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) elde ettiğiniz veri odaklı Django anket uygulamasını içerir.

### <a name="clone-the-sample-app"></a>Örnek uygulamayı kopyalama

Terminal penceresinde, örnek uygulama deposunu klonlamak ve yeni çalışma dizinine değiştirmek için aşağıdaki komutları çalıştırın:

```bash
git clone https://github.com/Azure-Samples/djangoapp.git
cd djangoapp
```

### <a name="configure-the-python-virtual-environment"></a>Python sanal ortamını yapılandırma

Uygulamanızı çalıştırmak için bir Python sanal ortamı oluşturun ve etkinleştirin.

```bash
python3 -m venv venv
source venv/bin/activate
```
or
```PowerShell
py -3 -m venv venv
venv\scripts\activate
```

`venv` Ortamda, *azuresite/settings.py'nin* veritabanı bağlantı ayarları için kullanacağı ortam değişkenlerini ayarlamak için *env.sh* veya *env.ps1* çalıştırın.

```bash
source ./env.sh
```
or
```PowerShell
.\env.ps1
```

*gereksinimleri.txt*gerekli paketleri yükleyin, [Django geçişleri](https://docs.djangoproject.com/en/2.1/topics/migrations/)çalıştırmak ve [bir yönetici kullanıcı oluşturun:](https://docs.djangoproject.com/en/2.1/intro/tutorial02/#creating-an-admin-user)

```bash
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
```

### <a name="run-the-web-app"></a>Web uygulamasını çalıştırma

Yönetici kullanıcısını oluşturduktan sonra Django sunucusunu çalıştırın.

```bash
python manage.py runserver
```

Django web uygulaması tam olarak yüklendiğinde, aşağıdaki mesaj gibi bir şey döndürür:

```bash
Performing system checks...

System check identified no issues (0 silenced).
December 13, 2019 - 10:54:59
Django version 2.1.2, using settings 'azuresite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

*http'ye\/gidin: /localhost:8000* tarayıcıda. İletiyi görmeniz gerekir **Yok anketler için var.** 

*\/http:/localhost:8000/admin'e* gidin ve son adımda oluşturduğunuz yönetici kullanıcısını kullanarak oturum açın. **Soruların**yanına **Ekle'yi** seçin ve bazı seçenekleriçeren bir anket sorusu oluşturun.

![Python Django uygulamasını Uygulama Hizmetleri'nde yerel olarak çalıştırın](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

Http *gidin:\//localhost:8000* tekrar anket soru görmek ve soruya cevap. Yerel Django örnek uygulaması, kullanıcı verilerini yerel PostgreSQL veritabanına yazar ve depolar.

Django sunucusunu durdurmak için terminale Ctrl+C yazın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Bu makaledeki kalan adımların çoğu, Azure Bulut Kabuğu'ndaki Azure CLI komutlarını kullanır. 

## <a name="create-and-connect-to-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı oluşturma ve bağlanma

Bu bölümde, PostgreSQL sunucusu ve veritabanı için bir Azure Veritabanı oluşturur ve web uygulamanızı bu veritabanına bağlarsınız. Web uygulamanızı Azure Uygulama Hizmeti'ne dağıttığınızda, uygulama bu bulut veritabanını kullanır. 

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

PostgreSQL sunucusu için Azure Veritabanınız için yeni bir kaynak grubu oluşturabilir veya varolan bir kaynak grubu kullanabilirsiniz. 

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

Cloud Shell'de [az postgres sunucusu oluşturma](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) komutu içeren bir PostgreSQL sunucusu oluşturursunuz.

> [!NOTE]
> PostgreSQL sunucusu için bir Azure Veritabanı oluşturmadan önce bölgenizde hangi [bilgi işlem oluşturmanın](/azure/postgresql/concepts-pricing-tiers#compute-generations-and-vcores) kullanılabildiğinizi denetleyin. Bölgeniz Gen4 donanımını desteklemiyorsa, aşağıdaki komut satırındaki *sku adını,* bölgenizde desteklenen Gen5 gibi bir değere değiştirin. 

Aşağıdaki komutta, * \<postgresql adı>'yi* benzersiz bir sunucu adı ile değiştirin. Sunucu adı PostgreSQL bitiş noktanızın bir parçasıdır *https://\<postgresql adı>.postgres.database.azure.com,* bu nedenle adın Azure'daki tüm sunucularda benzersiz olması gerekir. 

* \<Kaynak grubu adı>* ve * \<bölge>* kullanmak istediğiniz kaynak grubunun adı ve bölgesiyle değiştirin. Yönetici-kullanıcı adı>ve * \<yönetici-şifre>* için veritabanı yöneticisi hesabı için kullanıcı kimlik bilgilerini oluşturun. * \<* Daha * \<* sonra PostgreSQL sunucusunda ve veritabanlarında oturum açabilmek için kullanmak üzere yönetici kullanıcı adı>ve * \<yönetici-şifre>* unutmayın.

```azurecli-interactive
az postgres server create --resource-group <resourcegroup-name> --name <postgresql-name> --location "<region>" --admin-user <admin-username> --admin-password <admin-password> --sku-name B_Gen5_1
```

PostgreSQL sunucusu için Azure Veritabanı oluşturulduğunda, Azure CLI JSON kodunu aşağıdaki örnek gibi döndürür:

```json
{
  "administratorLogin": "myusername",
  "earliestRestoreDate": "2020-01-22T19:02:15.727000+00:00",
  "fullyQualifiedDomainName": "myservername.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/myservername",
  "location": "westeurope",
  "masterServerId": "",
  "name": "myservername",
  "replicaCapacity": 5,
  "replicationRole": "None",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 1,
    "family": "Gen5",
    "name": "B_Gen5_1",
    "size": null,
    "tier": "Basic"
  },
  < JSON data removed for brevity. >
}
```

### <a name="create-firewall-rules-for-the-azure-database-for-postgresql-server"></a>PostgreSQL sunucusu için Azure Veritabanı için güvenlik duvarı kuralları oluşturma

Azure kaynaklarından veritabanına erişime izin vermek için [az postgres sunucusu güvenlik duvarı kuralı oluşturma](/cli/azure/postgres/server/firewall-rule#az-postgres-server-firewall-rule-create) komutunu çalıştırın. Postgresql adı>ve * \<kaynak grubu adı>* yer tutucularını değerlerinizle değiştirin. * \<*

```azurecli-interactive
az postgres server firewall-rule create --resource-group <resourcegroup-name> --server-name <postgresql-name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAllAzureIPs
```

> [!NOTE]
> Önceki ayar, Azure ağı içindeki tüm IP adreslerinden ağ bağlantılarına izin verir. Üretim kullanımı için, [uygulamanızın yalnızca giden IP adreslerine izin vererek](../overview-inbound-outbound-ips.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips)mümkün olan en kısıtlayıcı güvenlik duvarı kurallarını yapılandırmaya çalışın.

Yerel `firewall-rule create` bilgisayarınızdan erişime izin vermek için komutu yeniden çalıştırın. * \<Ip adresinizi>* [yerel IPv4 IP adresinizle değiştirin.](https://www.whatsmyip.org/) Postgresql adı>ve * \<kaynak grubu adı>* yer tutucularını kendi değerlerinizle değiştirin. * \<*

```azurecli-interactive
az postgres server firewall-rule create --resource-group <resourcegroup-name> --server-name <postgresql-name> --start-ip-address=<your-ip-address> --end-ip-address=<your-ip-address> --name AllowLocalClient
```

### <a name="create-and-connect-to-the-azure-database-for-postgresql-database"></a>PostgreSQL veritabanı için Azure Veritabanı'nı oluşturun ve bağlanın

Aşağıdaki komutu çalıştırarak PostgreSQL sunucusu için Azure Veritabanınıza bağlanın. Kendi * \<postgresql adınızı>* ve * \<yönetici-kullanıcı adı>* kullanın ve oluşturduğunuz parolaile oturum açın.

```bash
psql -h <postgresql-name>.postgres.database.azure.com -U <admin-username>@<postgresql-name> postgres
```

Yerel PostgreSQL sunucunuzda yaptığınız gibi, PostgreSQL sunucusu için Azure Veritabanı'nda bir veritabanı ve kullanıcı oluşturun:

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

> [!NOTE]
> Yönetici kullanıcısını kullanmak yerine belirli uygulamalar için kısıtlı izinlere sahip veritabanı kullanıcıları oluşturmak en iyi yöntemdir. Kullanıcı, `manager` *yalnızca* veritabanıiçin `pollsdb` tam ayrıcalıklara sahiptir.

PostgreSQL istemcisinden çıkmak için `\q` yazın.

### <a name="test-app-connectivity-to-the-azure-postgresql-database"></a>Azure PostgreSQL veritabanına uygulama bağlantısını test etme

Postgresql adı>postgreSQL sunucu adı için Azure Veritabanınızla değiştirerek, yerel *env.sh* veya *env.ps1* dosyanızı bulut PostgreSQL veritabanınıza işaret etmek üzere düzenleme. * \<*

```bash
export DBHOST="<postgresql-name>.postgres.database.azure.com"
export DBUSER="manager@<postgresql-name>"
export DBNAME="pollsdb"
export DBPASS="supersecretpass"
```
or
```powershell
$Env:DBHOST = "<postgresql-name>.postgres.database.azure.com"
$Env:DBUSER = "manager@<postgresql-name>"
$Env:DBNAME = "pollsdb"
$Env:DBPASS = "supersecretpass"
```

Yerel `venv` terminal pencerenizdeki ortamda, düzenlenen *env.sh* veya *env.ps1'i*çalıştırın. 
```bash
source ./env.sh
```
or
```PowerShell
.\env.ps1
```

Django geçişini Azure veritabanına çalıştırın ve bir yönetici kullanıcısı oluşturun.

```bash
python manage.py migrate
python manage.py createsuperuser
```

Yönetici kullanıcı oluşturulduktan sonra Django sunucusunu çalıştırın.

```bash
python manage.py runserver
```

Bir tarayıcıda *\/http: /localhost:8000*adresine gidin ve iletiyi görmeniz gerekir **ki hiçbir anket** yok. 

*http'ye\/gidin: /localhost:8000/admin,* oluşturduğunuz yönetici kullanıcısını kullanarak oturum açın ve önceki gibi bir anket sorusu oluşturun.

![Python Django uygulamasını Uygulama Hizmetleri'nde yerel olarak çalıştırın](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

*\/http:/localhost:8000'e* tekrar gidin ve görüntülenen anket sorusunu görün. Uygulamanız artık PostgreSQL veritabanı için Azure Veritabanı'na veri yazıyor.

Django sunucusunu durdurmak için terminale Ctrl+C yazın.

## <a name="deploy-the-web-app-to-azure-app-service"></a>Web uygulamasını Azure Uygulama Hizmetine dağıtma

Bu adımda, PostgreSQL veritabanına bağlı Python uygulaması için Azure Veritabanı'nı Azure Uygulama Hizmeti'ne dağıtirsınız.

### <a name="configure-repository"></a>Depoyu yapılandırma

Bu öğretici bir Django örneği kullandığından, Azure Uygulama Hizmeti ile çalışmak için *djangoapp/azuresite/settings.py* dosyanızdaki bazı ayarları değiştirmeniz ve eklemeniz gerekir. 

1. Django gelen `HTTP_HOST` isteklerde üstbilgi doğrular. Django web uygulamanızın App Service'de çalışması için, uygulamanın tam nitelikli alan adını izin verilen ev sahiplerine eklemeniz gerekir. 
   
   Satırı aşağıdaki gibi değiştirmek için *azuresite/settings.py'yi* `ALLOWED_HOSTS` düzenle:
   
   ```python
   ALLOWED_HOSTS = [os.environ['WEBSITE_SITE_NAME'] + '.azurewebsites.net', '127.0.0.1'] if 'WEBSITE_SITE_NAME' in os.environ else []
   ```
   
1. Django [üretimde statik dosyaları hizmet](https://docs.djangoproject.com/en/2.1/howto/static-files/deployment/)desteklemiyor. Bu öğretici için, dosyaları hizmet etkinleştirmek için [WhiteNoise](https://whitenoise.evans.io/en/stable/) kullanın. WhiteNoise paketi zaten *requirements.txt*ile yüklendi. 
   
   Django'yu WhiteNoise'u kullanacak şekilde yapılandırmak için *azuresite/settings.py'de* `MIDDLEWARE` ayarı `django.middleware.security.SecurityMiddleware` bulun ve satırdan hemen sonra listeye ekleyin. `whitenoise.middleware.WhiteNoiseMiddleware` Ayarınız `MIDDLEWARE` şu şekilde görünmelidir:
   
   ```python
   MIDDLEWARE = [
       'django.middleware.security.SecurityMiddleware',
       'whitenoise.middleware.WhiteNoiseMiddleware',
       ...
   ]
   ```
   
1. *Azuresite/settings.py'nin*sonunda aşağıdaki satırları ekleyin:
   
   ```python
   STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
   STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
   ```
   
   WhiteNoise yapılandırma hakkında daha fazla bilgi için [WhiteNoise belgelerine](https://whitenoise.evans.io/en/stable/)bakın.

> [!IMPORTANT]
> Veritabanı ayarları bölümü zaten ortam değişkenlerini kullanarak güvenlik en iyi uygulama izler. Tam dağıtım önerileri için Bkz. [Django Documentation: dağıtım denetim listesi.](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/)

Değişikliklerinizi *djangoapp* deposunun çatalına işedin:

```bash
git commit -am "configure for App Service"
```

### <a name="configure-a-deployment-user"></a>Dağıtım kullanıcısı yapılandırma

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-python-linux-no-h.md)]

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Öğreticide daha önce PostgreSQL veritabanınıza bağlanmak için ortam değişkenleri tanımladınız.

Azure Uygulama Hizmeti'nde, [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komutunu kullanarak ortam değişkenlerini *uygulama ayarları*olarak ayarlarsınız.

Azure Bulut Kabuğu'nda, veritabanı bağlantı ayrıntılarını uygulama ayarları olarak belirtmek için aşağıdaki komutu çalıştırın. * \<Uygulama adı>, * * \<kaynak grubu adı>* ve * \<postgresql adı>* kendi değerlerinizle değiştirin.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resourcegroup-name> --settings DBHOST="<postgresql-name>.postgres.database.azure.com" DBUSER="manager@<postgresql-name>" DBPASS="supersecretpass" DBNAME="pollsdb"
```

Kodunuzun bu uygulama ayarlarına nasıl eriştiği hakkında bilgi için Access [ortamı değişleri](how-to-configure-python.md#access-environment-variables)gününe bakın.

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash 
Counting objects: 60, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (51/51), done.
Writing objects: 100% (60/60), 15.37 KiB | 749.00 KiB/s, done.
Total 60 (delta 9), reused 0 (delta 0)
remote: Deploy Async
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '06f3f7c0cb'.
remote: Repository path is /home/site/repository
remote: Running oryx build...
remote: Build orchestrated by Microsoft Oryx, https://github.com/Microsoft/Oryx
remote: You can report issues at https://github.com/Microsoft/Oryx/issues
. 
. 
. 
remote: Done in 100 sec(s).
remote: Running post deployment command(s)...
remote: Triggering recycle (preview mode disabled).
remote: Deployment successful.
remote: Deployment Logs : 'https://<app-name>.scm.azurewebsites.net/newui/jsonviewer?view_url=/api/deployments/06f3f7c0cb52ce3b4aff85c2b5099fbacb65ab94/log'
To https://<app-name>.scm.azurewebsites.net/<app-name>.git 
 * [new branch]      master -> master
```  

App Service dağıtım sunucusu *gereksinimler.txt* depo kökünde görür ve python `git push`paket yönetimini sonra otomatik olarak çalıştırır.

### <a name="browse-to-the-azure-app"></a>Azure uygulamasına göz atın

URL http ile dağıtılan uygulamaya göz *\//\<atın: app-name>.azurewebsites.net*. Kapsayıcının indirilmesi ve uygulama ilk kez istendiğinde çalıştırılması gerektiğinden, başlatılması biraz zaman alır. Sayfa zaman aşımına uğrar veya hata iletisi görüntülerse, birkaç dakika bekleyip sayfayı yenileyin.

Daha önce oluşturduğunuz anket sorularını görmelisiniz. 

App Service, her alt dizinde varsayılan olarak `manage.py startproject` bir *wsgi.py* dosyası arayarak deponuzda bir Django projesi algılar. App Service dosyayı bulduğunda, Django web uygulamasını yükler. Uygulama Hizmetinin Python uygulamalarını nasıl yükleleri hakkında daha fazla bilgi için yerleşik [Python görüntüsünü yapılandırma'ya](how-to-configure-python.md)bakın.

*http:>.azurewebsites.net/admin\//\<adresindeki uygulama adı* ve oluşturduğunuz yönetici kullanıcısını kullanarak oturum açın. İstersen, biraz daha anket soruları oluşturun.

![Azure'da Uygulama Hizmetlerinde Python Django uygulamasını çalıştırın](./media/tutorial-python-postgresql-app/run-python-django-app-in-azure.png)

**Tebrikler!** Linux için Azure Uygulama Hizmeti'nde bir Python (Django) web uygulaması çalıştırıyorsunuz.

## <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="manage-your-app-in-the-azure-portal"></a>Uygulamanızı Azure portalında yönetme

Azure [portalında,](https://portal.azure.com)oluşturduğunuz uygulamayı arayın ve seçin.

![Azure portalındaki Python Django uygulamanıza gidin](./media/tutorial-python-postgresql-app/navigate-to-django-app-in-app-services-in-the-azure-portal.png)

Varsayılan olarak, portal uygulamanızın **Genel Bakış** sayfasını gösterir. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Burada, göz atma, durdurma, yeniden başlatma ve silme gibi temel yönetim görevlerini de gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Python Django uygulamanızı Azure portalındaki Genel Bakış sayfasında yönetme](./media/tutorial-python-postgresql-app/manage-django-app-in-app-services-in-the-azure-portal.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Özel bir DNS adının uygulamanızla nasıl eşleştini öğrenmek için bir sonraki öğreticiye gidin:

> [!div class="nextstepaction"]
> [Öğretici: Uygulamanıza özel DNS adını haritalandırın](../app-service-web-tutorial-custom-domain.md)

Veya diğer kaynaklara göz atın:

> [!div class="nextstepaction"]
> [Python uygulamasını yapılandırma](how-to-configure-python.md)

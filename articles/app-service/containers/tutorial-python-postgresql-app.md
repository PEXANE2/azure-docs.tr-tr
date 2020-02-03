---
title: 'Öğretici: postgre ile Linux Python uygulaması'
description: Azure 'da bir PostgreSQL veritabanına bağlantı ile Azure App Service çalışan bir Linux Python uygulamasını nasıl alabileceğinizi öğrenin. Bu öğreticide docgo kullanılır.
ms.devlang: python
ms.topic: tutorial
ms.date: 01/23/2020
ms.custom:
- mvc
- seodec18
- seo-python-october2019
ms.openlocfilehash: 3aa5b5085a6120ca513f0aeba344e7f541f0fd72
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76713418"
---
# <a name="tutorial-run-a-python-django-web-app-with-postgresql-in-azure-app-service"></a>Öğretici: Azure App Service içindeki PostgreSQL ile Python (Docgo) Web uygulaması çalıştırma

[Azure App Service](app-service-linux-intro.md), yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğreticide, veri odaklı bir Python Docgo Web uygulamasını PostgreSQL için Azure veritabanı veritabanına bağlama ve uygulamayı Azure App Service dağıtma ve çalıştırma işlemlerinin nasıl yapılacağı gösterilmektedir.

![Azure App Service 'de Python Docgo Web uygulaması](./media/tutorial-python-postgresql-app/run-python-django-app-in-azure.png)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * PostgreSQL için Azure veritabanı veritabanı oluşturma ve bir Web uygulamasını buna bağlama
> * Web uygulamasını Azure App Service dağıtma
> * Tanılama günlüklerini görüntüleme
> * Azure portal web uygulamasını yönetme

Bu makaledeki adımları macOS, Linux veya Windows ' da izleyebilirsiniz. Adımlar çoğu durumda benzerdir, ancak farklar bu öğreticide ayrıntılı değildir. Aşağıdaki örneklerin çoğu Linux üzerinde `bash` Terminal penceresi kullanır. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce:

- [!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
- [Git](https://git-scm.com/)'i yükleyin.
- [Python 3](https://www.python.org/downloads/)' ü yükler.
- [PostgreSQL](https://www.postgresql.org/download/)'i yükleyip çalıştırın.

## <a name="test-postgresql-installation-and-create-a-database"></a>PostgreSQL yüklemesini test etme ve veritabanı oluşturma

İlk olarak, yerel PostgreSQL sunucunuza bağlanın ve bir veritabanı oluşturun: 

Yerel bir Terminal penceresinde, yerleşik `postgres` Kullanıcı olarak yerel PostgreSQL sunucunuza bağlanmak için `psql` çalıştırın.

```bash
sudo su - postgres
psql
```
or
```PowerShell
psql -U postgres
```

Bağlantınız başarılı olursa, PostgreSQL veritabanınız çalışır. Aksi takdirde, yerel PostgresQL veritabanınızın [İndirmeler - PostgreSQL Çekirdek Dağıtım](https://www.postgresql.org/download/) konusunda işletim sisteminiz için yönergeler izlenilerek başlatıldığından emin olun.

*Pollsdb*adlı yeni bir veritabanı oluşturun ve Password *supersecretpass*ile *yönetici* adlı bir veritabanı kullanıcısı ayarlayın:

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

PostgreSQL istemcisinden çıkmak için `\q` yazın.

<a name="step2"></a>
## <a name="create-and-run-the-local-python-app"></a>Yerel Python uygulaması oluşturma ve çalıştırma

Sonra, örnek Python Docgo Web uygulamasını ayarlayıp çalıştırın.

[Docgoapp](https://github.com/Azure-Samples/djangoapp) örnek deposu, docgo belgelerine [Ilk Docgo uygulamanızı yazarak](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) aldığınız veri tabanlı [docgo](https://www.djangoproject.com/) yoklamaları uygulamasını içerir.

### <a name="clone-the-sample-app"></a>Örnek uygulamayı kopyalama

Bir Terminal penceresinde, örnek uygulama deposunu kopyalamak için aşağıdaki komutları çalıştırın ve yeni çalışma dizinine geçin:

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

`venv` ortamında, *azuresite/Settings. Kopyala* ' nın veritabanı bağlantısı ayarları için kullanacağı ortam değişkenlerini ayarlamak için *env.sh* veya *env. ps1* ' yi çalıştırın.

```bash
source ./env.sh
```
or
```PowerShell
.\env.ps1
```

*Requirements. txt*dosyasından gerekli paketleri yükler, [docgo geçişlerini](https://docs.djangoproject.com/en/2.1/topics/migrations/)çalıştırın ve [bir yönetici kullanıcı oluşturun](https://docs.djangoproject.com/en/2.1/intro/tutorial02/#creating-an-admin-user):

```bash
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
```

### <a name="run-the-web-app"></a>Web uygulamasını çalıştırma

Yönetici kullanıcıyı oluşturduktan sonra Docgo sunucusunu çalıştırın.

```bash
python manage.py runserver
```

Docgo Web uygulaması tam olarak yüklendiğinde aşağıdaki iletiye benzer bir şey döndürür:

```bash
Performing system checks...

System check identified no issues (0 silenced).
December 13, 2019 - 10:54:59
Django version 2.1.2, using settings 'azuresite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

{1 & gt; *http\/: 1* & lt; 1} sayfasına gidin. **Hiçbir yoklama**yok iletisini görmeniz gerekir. 

*Http:\//localhost: 8000/admin* adresine gidin ve son adımda oluşturduğunuz yönetici kullanıcısını kullanarak oturum açın. **Soruların**yanına **Ekle** ' yi seçin ve bazı seçeneklerle bir yoklama sorusu oluşturun.

![Uygulama hizmetlerinde yerel olarak Python Docgo uygulaması çalıştırma](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

*Http:\//localhost:* soru-cevap sorusuna bakmak ve soruyu yanıtlamak için bu adrese gidin. Yerel Docgo örnek uygulaması, Kullanıcı verilerini yerel PostgreSQL veritabanına yazar ve depolar.

Docgo sunucusunu durdurmak için terminalde CTRL + C yazın.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Bu makaledeki diğer adımların çoğu Azure Cloud Shell Azure CLı komutlarını kullanır. 

## <a name="create-and-connect-to-azure-database-for-postgresql"></a>PostgreSQL için Azure veritabanı oluşturun ve bu veritabanına bağlanın

Bu bölümde, PostgreSQL için Azure veritabanı sunucusu ve veritabanı oluşturup Web uygulamanızı bu sunucuya bağlayabilirsiniz. Web uygulamanızı Azure App Service dağıtırken, uygulama bu bulut veritabanını kullanır. 

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

PostgreSQL için Azure veritabanı sunucunuz için yeni bir kaynak grubu oluşturabilir veya var olan bir kaynak grubunu kullanabilirsiniz. 

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

Cloud Shell [az Postgres Server Create](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) komutuyla bir PostgreSQL sunucusu oluşturursunuz.

> [!NOTE]
> PostgreSQL için Azure veritabanı sunucusu oluşturmadan önce, bölgenizde hangi [işlem oluşturmanın](/azure/postgresql/concepts-pricing-tiers#compute-generations-and-vcores) kullanılabilir olduğunu denetleyin. Bölgeniz 4. nesil donanımını desteklemiyorsa, aşağıdaki komut satırında *--SKU-adı* ' nı, bölgenizde desteklenen bir değere 5. nesil gibi değiştirin. 

Aşağıdaki komutta, *\<PostgreSQL-name >* öğesini benzersiz bir sunucu adıyla değiştirin. Sunucu adı PostgreSQL uç nokta *https://\<PostgreSQL-name >. Postgres. Database. Azure. com*' un bir parçası olduğundan, adın Azure 'daki tüm sunucular arasında benzersiz olması gerekir. 

*\<resourcegroup-name >* ve *\<Region >* , kullanmak istediğiniz kaynak grubunun adı ve bölgesiyle değiştirin. *\<admin-username >* ve *\<admin-Password >* için, veritabanı yönetici hesabı için Kullanıcı kimlik bilgileri oluşturun. Daha sonra PostgreSQL sunucusunda ve veritabanlarında oturum açmak için kullanmak üzere *Yönetici-Kullanıcı adı >* ve *\<yönetici parolası >* \<unutmayın.

```azurecli-interactive
az postgres server create --resource-group <resourcegroup-name> --name <postgresql-name> --location "<region>" --admin-user <admin-username> --admin-password <admin-password> --sku-name B_Gen5_1
```

PostgreSQL için Azure veritabanı sunucusu oluşturulduğunda Azure CLı, aşağıdaki örneğe benzer bir JSON kodu döndürür:

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

### <a name="create-firewall-rules-for-the-azure-database-for-postgresql-server"></a>PostgreSQL için Azure veritabanı sunucusu için güvenlik duvarı kuralları oluşturma

Azure kaynaklarından veritabanına erişime izin vermek için [az Postgres Server Firewall-Rule Create](/cli/azure/postgres/server/firewall-rule#az-postgres-server-firewall-rule-create) komutunu çalıştırın. *\<PostgreSQL-name >* ve *\<resourcegroup-name >* yer tutucuları değerlerinizle değiştirin.

```azurecli-interactive
az postgres server firewall-rule create --resource-group <resourcegroup-name> --server-name <postgresql-name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAllAzureIPs
```

> [!NOTE]
> Yukarıdaki ayar, Azure ağı içindeki tüm IP adreslerinden ağ bağlantılarına izin verir. Üretim kullanımı için, [yalnızca uygulamanızın kullandığı gıden IP adreslerine izin vererek](../overview-inbound-outbound-ips.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips)mümkün olan en kısıtlayıcı güvenlik duvarı kurallarını yapılandırmayı deneyin.

Yerel bilgisayarınızdan erişime izin vermek için `firewall-rule create` komutunu yeniden çalıştırın. *\<-IP adresi >* [yerel IPv4 IP adresiniz](https://www.whatsmyip.org/)ile değiştirin. *\<PostgreSQL-name >* ve *\<resourcegroup-name >* yer tutucuları kendi değerlerinizle değiştirin.

```azurecli-interactive
az postgres server firewall-rule create --resource-group <resourcegroup-name> --server-name <postgresql-name> --start-ip-address=<your-ip-address> --end-ip-address=<your-ip-address> --name AllowLocalClient
```

### <a name="create-and-connect-to-the-azure-database-for-postgresql-database"></a>PostgreSQL için Azure veritabanı veritabanını oluşturun ve buna bağlanın

Aşağıdaki komutu çalıştırarak PostgreSQL için Azure veritabanı sunucunuza bağlanın. Kendi *\<PostgreSQL-name >* ve *\<admin-username >* kullanın ve oluşturduğunuz parolayla oturum açın.

```bash
psql -h <postgresql-name>.postgres.database.azure.com -U <admin-username>@<postgresql-name> postgres
```

Yerel PostgreSQL sunucunuzda yaptığınız gibi, PostgreSQL için Azure veritabanı sunucusunda bir veritabanı ve kullanıcı oluşturun:

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

> [!NOTE]
> Yönetici Kullanıcı yerine belirli uygulamalar için kısıtlı izinlerle veritabanı kullanıcıları oluşturmak en iyi uygulamadır. `manager` kullanıcının *yalnızca* `pollsdb` veritabanı için tam ayrıcalıkları vardır.

PostgreSQL istemcisinden çıkmak için `\q` yazın.

### <a name="test-app-connectivity-to-the-azure-postgresql-database"></a>Azure PostgreSQL veritabanı ile uygulama bağlantısını test etme

*\<PostgreSQL-name >* 'ı PostgreSQL Için Azure veritabanı sunucu adı ile değiştirerek, yerel *env.sh* veya *env. ps1* Dosyanızı bulut PostgreSQL veritabanınıza işaret etmek üzere düzenleyin.

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

Yerel Terminal pencerenizde `venv` ortamda, düzenlenen *env.sh* veya *env. ps1*' yi çalıştırın. 
```bash
source ./env.sh
```
or
```PowerShell
.\env.ps1
```

Azure veritabanına Docgo geçişi çalıştırın ve bir yönetici kullanıcı oluşturun.

```bash
python manage.py migrate
python manage.py createsuperuser
```

Yönetici Kullanıcı oluşturulduktan sonra Docgo sunucusunu çalıştırın.

```bash
python manage.py runserver
```

Bir tarayıcıda *http:\//localhost: 8000*' e gidin ve bir **yokladığı** iletiyi tekrar kullanılabilir olmadığını görmeniz gerekir. 

*Http:\//localhost: 8000/admin*' e gidin, oluşturduğunuz yönetici kullanıcısını kullanarak oturum açın ve daha önce olduğu gibi bir yoklama sorusu oluşturun.

![Uygulama hizmetlerinde yerel olarak Python Docgo uygulaması çalıştırma](./media/tutorial-python-postgresql-app/run-python-django-app-locally.png)

*Http:\//localhost: 8000* ' e gidin ve görünen yoklama sorusunu inceleyin. Uygulamanız artık PostgreSQL için Azure veritabanı veritabanına veri yazıyor.

Docgo sunucusunu durdurmak için terminalde CTRL + C yazın.

## <a name="deploy-the-web-app-to-azure-app-service"></a>Web uygulamasını Azure App Service dağıtma

Bu adımda, PostgreSQL için Azure veritabanı veritabanına bağlı Python uygulamasını Azure App Service olarak dağıtırsınız.

### <a name="configure-repository"></a>Depoyu yapılandırma

Bu öğretici bir Docgo örneği kullandığından, Azure App Service çalışmak için *docgoapp/azuresite/Settings. Kopyala* dosyanızdaki bazı ayarları değiştirmeniz ve eklemeniz gerekir. 

1. Docgo, gelen isteklerindeki `HTTP_HOST` üst bilgisini doğrular. Docgo Web uygulamanızın App Service çalışması için, uygulamanın tam etki alanı adını izin verilen konaklara eklemeniz gerekir. 
   
   `ALLOWED_HOSTS` satırını şu şekilde değiştirmek için *azuresite/Settings. Kopyala* öğesini düzenleyin:
   
   ```python
   ALLOWED_HOSTS = [os.environ['WEBSITE_SITE_NAME'] + '.azurewebsites.net', '127.0.0.1'] if 'WEBSITE_SITE_NAME' in os.environ else []
   ```
   
1. Docgo [, üretimde statik dosyalara hizmet vermeye](https://docs.djangoproject.com/en/2.1/howto/static-files/deployment/)destek vermez. Bu öğreticide, dosyaları sunmaya olanak tanımak için [Whitenoıse](https://whitenoise.evans.io/en/stable/) kullanırsınız. Whitenoıse paketi, *requirements. txt*ile zaten yüklenmiş. 
   
   Docgo 'yu kullanmak için *azuresite/Settings. Kopyala*içinde, `MIDDLEWARE` ayarını bulun ve `django.middleware.security.SecurityMiddleware` satırından hemen sonra listeye `whitenoise.middleware.WhiteNoiseMiddleware` ekleyin. `MIDDLEWARE` ayarınız şöyle görünmelidir:
   
   ```python
   MIDDLEWARE = [
       'django.middleware.security.SecurityMiddleware',
       'whitenoise.middleware.WhiteNoiseMiddleware',
       ...
   ]
   ```
   
1. *Azuresite/Settings. Kopyala*sonunda aşağıdaki satırları ekleyin:
   
   ```python
   STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
   STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
   ```
   
   Whitenoıse 'yi yapılandırma hakkında daha fazla bilgi için bkz. [whitenoıse belgeleri](https://whitenoise.evans.io/en/stable/).

> [!IMPORTANT]
> Veritabanı ayarları bölümü, ortam değişkenlerini kullanmanın en iyi güvenlik uygulamasını zaten izler. Tüm dağıtım önerileri için bkz. [Docgo belgeleri: dağıtım denetim listesi](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/).

*Docgoapp* deposunun Çatalınıza yaptığınız değişiklikleri işleyin:

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

Azure App Service, [az WebApp config appSettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komutunu kullanarak ortam değişkenlerini *uygulama ayarları*olarak ayarlarsınız.

Azure Cloud Shell, veritabanı bağlantısı ayrıntılarını uygulama ayarları olarak belirtmek için aşağıdaki komutu çalıştırın. *\<app-name >* , *\<resourcegroup-name >* ve *\<PostgreSQL-name >* değerlerini kendi değerlerinizle değiştirin.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resourcegroup-name> --settings DBHOST="<postgresql-name>.postgres.database.azure.com" DBUSER="manager@<postgresql-name>" DBPASS="supersecretpass" DBNAME="pollsdb"
```

Kodunuzun bu uygulama ayarlarına nasıl eriştiği hakkında daha fazla bilgi için bkz. [ortam değişkenlerine erişim](how-to-configure-python.md#access-environment-variables).

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

App Service dağıtım sunucusu, depo kökünde *requirements. txt dosyasını* görür ve `git push`sonra Python paket yönetimini otomatik olarak çalıştırır.

### <a name="browse-to-the-azure-app"></a>Azure uygulamasına gidin

URL *http:\//\<app-name >. azurewebsites. net*ile dağıtılan uygulamaya gidin. Uygulama ilk kez istendiğinde, kapsayıcının indirilmesi ve çalıştırılması gerektiğinden, başlaması biraz zaman alır. Sayfa zaman aşımına uğrar veya hata iletisi görüntülerse, birkaç dakika bekleyip sayfayı yenileyin.

Daha önce oluşturduğunuz yoklama sorularını görmeniz gerekir. 

App Service, `manage.py startproject` varsayılan olarak oluşturduğu her alt dizinde bir *wsgi.py* dosyası arayarak deponuzdaki bir Docgo projesi algılar. App Service dosyayı bulduğunda Docgo Web uygulamasını yükler. App Service Python uygulamalarını nasıl yüklediği hakkında daha fazla bilgi için bkz. [yerleşik Python görüntüsünü yapılandırma](how-to-configure-python.md).

*Http:\//\<app-name >. azurewebsites. net/admin* ' e gidin ve oluşturduğunuz yönetici kullanıcısını kullanarak oturum açın. İsterseniz, bazı daha fazla yoklama soruları oluşturun.

![Azure 'da uygulama hizmetlerinde Python Docgo uygulamasını çalıştırma](./media/tutorial-python-postgresql-app/run-python-django-app-in-azure.png)

**Tebrikler!** Linux için Azure App Service bir Python (Docgo) Web uygulaması çalıştırıyorsunuz.

## <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="manage-your-app-in-the-azure-portal"></a>Uygulamanızı Azure portal yönetme

[Azure Portal](https://portal.azure.com)oluşturduğunuz uygulamayı arayıp seçin.

![Azure portal Python Docgo uygulamanıza gidin](./media/tutorial-python-postgresql-app/navigate-to-django-app-in-app-services-in-the-azure-portal.png)

Varsayılan olarak, Portal uygulamanızın **genel bakış** sayfasını gösterir. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Burada, gezinme, durdurma, yeniden başlatma ve silme gibi temel yönetim görevlerini de gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Python Docgo uygulamanızı Azure portal genel bakış sayfasında yönetin](./media/tutorial-python-postgresql-app/manage-django-app-in-app-services-in-the-azure-portal.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Özel bir DNS adını uygulamanıza nasıl eşleyeceğinizi öğrenmek için bir sonraki Öğreticiye gidin:

> [!div class="nextstepaction"]
> [Öğretici: özel DNS adını uygulamanıza eşleyin](../app-service-web-tutorial-custom-domain.md)

Ya da diğer kaynaklara göz atın:

> [!div class="nextstepaction"]
> [Python uygulamasını yapılandırma](how-to-configure-python.md)

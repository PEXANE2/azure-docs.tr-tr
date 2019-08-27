---
title: Linux üzerinde PostgreSQL ile Python (Docgo) Web uygulaması-Azure App Service | Microsoft Docs
description: Azure 'da bir PostgreSQL veritabanıyla bağlantı ile veri odaklı bir Python (Docgo) Web uygulamasını çalıştırmayı öğrenin.
services: app-service\web
documentationcenter: python
author: cephalin
manager: gwallace
ms.service: app-service-web
ms.workload: web
ms.devlang: python
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: b5a21bd144558459ac60ae2da405f9ea57ca2fd1
ms.sourcegitcommit: 80dff35a6ded18fa15bba633bf5b768aa2284fa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70020196"
---
# <a name="build-a-python-django-web-app-with-postgresql-in-azure-app-service"></a>Azure App Service içindeki PostgreSQL ile Python (Docgo) Web uygulaması oluşturma

[Linux’ta App Service](app-service-linux-intro.md) yüksek oranda ölçeklenebilen, kendi kendine düzeltme eki uygulayan bir web barındırma hizmeti sunar. Bu öğreticide, veritabanı arka ucu olarak PostgreSQL kullanılarak veri odaklı bir Python (Docgo) Web uygulamasının nasıl oluşturulacağı gösterilmektedir. İşiniz bittiğinde, Linux üzerinde Azure App Service çalıştıran bir Docgo Web uygulamanız vardır.

![Linux üzerinde App Service Python Docgo Web uygulaması](./media/tutorial-python-postgresql-app/django-admin-azure.png)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure’da PostgreSQL veritabanı oluşturma
> * Python web uygulamasını PostgreSQL 'e bağlama
> * Python web uygulamasını Azure 'a dağıtma
> * Tanılama günlüklerini görüntüleme
> * Azure portal Python web uygulamasını yönetme

> [!NOTE]
> PostgreSQL için Azure veritabanı oluşturmadan önce, [bölgenizde hangi işlem oluşturmanın kullanılabilir olduğunu](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#compute-generations-and-vcores)kontrol edin.

Bu makaledeki adımları macOS üzerinde izleyebilirsiniz. Linux ve Windows yönergeleri çoğu durumda aynıdır, ancak bu öğreticide farkları konusunda ayrıntıya girilmemiştir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

1. [Git'i yükleyin](https://git-scm.com/)
2. [Python'ı yükleyin](https://www.python.org/downloads/)
3. [PostgreSQL’i yükleyin ve çalıştırın](https://www.postgresql.org/download/)

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Yerel PostgreSQL yüklemesini test etme ve bir veritabanı oluşturma

Yerel bir terminal penceresinde `psql` komutunu çalıştırarak yerel PostgreSQL sunucunuza bağlanın.

```bash
sudo -u postgres psql postgres
```

`unknown user: postgres` gibi bir hata iletisi alırsanız, PostgreSQL yüklemeniz oturum açmış kullanıcı adınız ile yapılandırılabilir. Bunun yerine aşağıdaki komutu deneyin.

```bash
psql postgres
```

Bağlantınız başarılı olursa, PostgreSQL veritabanınız çalışır. Aksi takdirde, yerel PostgresQL veritabanınızın [İndirmeler - PostgreSQL Çekirdek Dağıtım](https://www.postgresql.org/download/) konusunda işletim sisteminiz için yönergeler izlenilerek başlatıldığından emin olun.

*Pollsdb* adlı bir veritabanı oluşturun ve Password *supersecretpass*ile *Manager* adlı ayrı bir veritabanı kullanıcısı ayarlayın.

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

PostgreSQL istemcisinden çıkmak için `\q` yazın.

<a name="step2"></a>

## <a name="create-local-python-app"></a>Yerel Python uygulaması oluşturma

Bu adımda, yerel Python Docgo projesini ayarlarsınız.

### <a name="clone-the-sample-app"></a>Örnek uygulamayı kopyalama

Terminal penceresini açın ve bir çalışma dizinine `CD` yazın.

Örnek depoyu kopyalamak için aşağıdaki komutları çalıştırın.

```bash
git clone https://github.com/Azure-Samples/djangoapp.git
cd djangoapp
```

Bu örnek depo, bir [Docgo](https://www.djangoproject.com/) uygulaması içerir. Bu, [Docgo belgelerindeki başlangıç öğreticisini](https://docs.djangoproject.com/en/2.1/intro/tutorial01/)izleyerek elde ettiğiniz veri tabanlı uygulama. Bu öğretici, size bir docgo Web uygulaması (veya başka bir veri odaklı Python uygulaması) Azure App Service için nasıl dağıtım yapılacağını ve çalıştırılacağını gösterir.

### <a name="configure-environment"></a>Ortamı yapılandırma

Bir Python sanal ortamı oluşturun ve veritabanı bağlantı ayarlarını ayarlamak için bir komut dosyası kullanın.

```bash
# Bash
python3 -m venv venv
source venv/bin/activate
source ./env.sh

# PowerShell
py -3 -m venv venv
venv\scripts\activate
.\env.ps1
```

*Env.sh* ve *env. ps1* içinde tanımlanan ortam değişkenleri, veritabanı ayarlarını tanımlamak için _azuresite/Settings. Kopyala_ içinde kullanılır.

### <a name="run-app-locally"></a>Uygulamayı yerel olarak çalıştırma

Gerekli paketleri yükler, [Docgo geçişleri çalıştırın](https://docs.djangoproject.com/en/2.1/topics/migrations/) ve [bir yönetici kullanıcı oluşturun](https://docs.djangoproject.com/en/2.1/intro/tutorial02/#creating-an-admin-user).

```bash
pip install -r requirements.txt
python manage.py migrate
python manage.py createsuperuser
```

Yönetici Kullanıcı oluşturulduktan sonra Docgo sunucusunu çalıştırın.

```bash
python manage.py runserver
```

Docgo Web uygulaması tam olarak yüklendiğinde aşağıdaki iletiye benzer bir şey görürsünüz:

```bash
Performing system checks...

System check identified no issues (0 silenced).
October 26, 2018 - 10:54:59
Django version 2.1.2, using settings 'azuresite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

`http://localhost:8000` Bir tarayıcıda adresine gidin. İletiyi `No polls are available.`görmeniz gerekir. 

Son adımda oluşturduğunuz yönetici kullanıcısını kullanarak adresine gidin veoturumaçın.`http://localhost:8000/admin` **Sorular** ' ın yanındaki **Ekle** ' yi seçin ve bazı seçeneklerle bir yoklama sorusu oluşturun.

![Yerel olarak çalışan Python Docgo uygulaması](./media/tutorial-python-postgresql-app/django-admin-local.png)

`http://localhost:8000` Tekrar gidin ve görünen yoklama sorusuna bakın.

Docgo örnek uygulaması, Kullanıcı verilerini veritabanında depolar. Bir anket sorusu eklerken başarılı olursanız, uygulamanız yerel PostgreSQL veritabanına veri yazıyor.

Docgo sunucusunu dilediğiniz zaman durdurmak için, terminalde CTRL + C yazın.

## <a name="create-a-production-postgresql-database"></a>Üretim PostgreSQL veritabanı oluşturma

Bu adımda, Azure’da bir SQL Veritabanı oluşturursunuz. Uygulamanız Azure’da dağıtıldığında bu bulut veritabanını kullanır.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)]

### <a name="create-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu oluşturma

Cloud Shell'de [`az postgres server create`](/cli/azure/postgres/server?view=azure-cli-latest#az-postgres-server-create) komutuyla bir PostgreSQL sunucusu oluşturun.

Aşağıdaki örnek komutta,  *\<PostgreSQL-name >* öğesini benzersiz bir sunucu adıyla değiştirin ve  *\<admin-username >* ve  *\<Admin-Password >* istenen kullanıcı kimlik bilgileriyle değiştirin. Kullanıcı kimlik bilgileri, veritabanı yöneticisi hesabı için geçerli olacaktır. Sunucu adı, PostgreSQL uç noktasının bir parçası olan `https://<postgresql-name>.postgres.database.azure.com` olarak kullanıldığından, adın Azure’daki tüm sunucularda benzersiz olması gerekir.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql-name> --location "West Europe" --admin-user <admin-username> --admin-password <admin-password> --sku-name B_Gen4_1
```

PostgreSQL için Azure Veritabanı sunucusu oluşturulduğunda Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "administratorLogin": "<admin-username>",
  "fullyQualifiedDomainName": "<postgresql-name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql-name>",
  "location": "westus",
  "name": "<postgresql-name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 1,
    "family": "Gen4",
    "name": "B_Gen4_1",
    "size": null,
    "tier": "Basic"
  },
  < JSON data removed for brevity. >
}
```

> [!NOTE]
> Daha \<sonra için admin-username \<> ve Admin-Password > hatırlayın. Postgre sunucusu ve veritabanlarında oturum açmak için bunlara ihtiyacınız vardır.

### <a name="create-firewall-rules-for-the-postgresql-server"></a>PostgreSQL sunucusu için güvenlik duvarı kuralları oluşturma

Azure kaynaklarından veritabanına erişim izni vermek için Cloud Shell'de aşağıdaki Azure CLI komutunu çalıştırın.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql-name> --start-ip-address=0.0.0.0 --end-ip-address=0.0.0.0 --name AllowAllAzureIPs
```

> [!NOTE]
> Bu ayar, Azure ağ içindeki tüm IP’lerden ağ bağlantılarına izin verir. Üretim kullanımı için, [yalnızca uygulamanızın kullandığı giden IP adreslerini kullanarak](../overview-inbound-outbound-ips.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips) en kısıtlayıcı güvenlik duvarı kurallarını yapılandırmayı deneyin.

Cloud Shell,  *\<IP adresi >* [yerel IPv4 IP adresinizle](https://www.whatsmyip.org/)değiştirerek yerel bilgisayarınızdan erişime izin vermek için komutu yeniden çalıştırın.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql-name> --start-ip-address=<your-ip-address> --end-ip-address=<your-ip-address> --name AllowLocalClient
```

## <a name="connect-python-app-to-production-database"></a>Python uygulamasını üretim veritabanına bağlama

Bu adımda, Docgo Web uygulamanızı oluşturduğunuz PostgreSQL için Azure veritabanı sunucusuna bağlayaöğreneceksiniz.

### <a name="create-empty-database-and-user-access"></a>Boş veritabanı oluşturma ve kullanıcı erişimi sağlama

Cloud Shell, aşağıdaki komutu çalıştırarak veritabanına bağlanın. Yönetici parolanızı girmeniz istendiğinde [PostgreSQL için Azure Veritabanı sunucusu oluşturma](#create-an-azure-database-for-postgresql-server) bölümünde belirttiğiniz parolayı kullanın.

```bash
psql -h <postgresql-name>.postgres.database.azure.com -U <admin-username>@<postgresql-name> postgres
```

Yerel Postgres sunucunuzda olduğu gibi, Azure Postgres sunucusunda veritabanı ve kullanıcıyı oluşturun.

```sql
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

PostgreSQL istemcisinden çıkmak için `\q` yazın.

> [!NOTE]
> Yönetici kullanıcıyı kullanmak yenine belirli uygulamalar için kısıtlı izinlere sahip olan veritabanı kullanıcıları oluşturmak en iyi uygulamadır. Bu örnekte, `manager` kullanıcısı _yalnızca_ `pollsdb` veritabanı için tam ayrıcalıklara sahiptir.

### <a name="test-app-connectivity-to-production-database"></a>Uygulamanın üretim veritabanına bağlanıp bağlanmadığını test etme

Yerel Terminal penceresinde, veritabanı ortam değişkenlerini ( *env.sh* veya *env. ps1*çalıştırarak daha önce yapılandırdığınız) değiştirin:

```bash
# Bash
export DBHOST="<postgresql-name>.postgres.database.azure.com"
export DBUSER="manager@<postgresql-name>"
export DBNAME="pollsdb"
export DBPASS="supersecretpass"

# PowerShell
$Env:DBHOST = "<postgresql-name>.postgres.database.azure.com"
$Env:DBUSER = "manager@<postgresql-name>"
$Env:DBNAME = "pollsdb"
$Env:DBPASS = "supersecretpass"
```

Azure veritabanına Docgo geçişini çalıştırın ve bir yönetici kullanıcı oluşturun.

```bash
python manage.py migrate
python manage.py createsuperuser
```

Yönetici Kullanıcı oluşturulduktan sonra Docgo sunucusunu çalıştırın.

```bash
python manage.py runserver
```

Yeniden ' `http://localhost:8000` ye gidin. İletiyi `No polls are available.` bir daha görmeniz gerekir. 

Oluşturduğunuz yönetici `http://localhost:8000/admin` kullanıcısını kullanarak öğesine gidin ve oturum açın ve daha önce olduğu gibi bir yoklama sorusu oluşturun.

![Yerel olarak çalışan Python Docgo uygulaması](./media/tutorial-python-postgresql-app/django-admin-local.png)

`http://localhost:8000` Tekrar gidin ve görünen yoklama sorusuna bakın. Uygulamanız artık Azure 'da veritabanına veri yazıyor.

## <a name="deploy-to-azure"></a>Azure’a dağıtma

Bu adımda, Postgres’e bağlı Python uygulamasını Azure App Service'e dağıtırsınız.

### <a name="configure-repository"></a>Depoyu yapılandırma

Docgo gelen isteklerindeki `HTTP_HOST` üstbilgiyi doğrular. Docgo Web uygulamanızın App Service çalışması için, uygulamanın tam etki alanı adını izin verilen konaklara eklemeniz gerekir. _Azuresite/Settings. Kopyala_ ' yı açın ve `ALLOWED_HOSTS` ayarı bulun. Satırı şu şekilde değiştirin:

```python
ALLOWED_HOSTS = [os.environ['WEBSITE_SITE_NAME'] + '.azurewebsites.net', '127.0.0.1'] if 'WEBSITE_SITE_NAME' in os.environ else []
```

Daha sonra, Docgo [üretimde statik dosyalara hizmet vermeye](https://docs.djangoproject.com/en/2.1/howto/static-files/deployment/), bu yüzden el ile etkinleştirmeniz gerekir. Bu öğreticide, [Whitenoıse](https://whitenoise.evans.io/en/stable/)kullanıyorsunuz. Whitenoıse paketi, _requirements. txt_' ye zaten dahil edilmiştir. Yalnızca Docgo 'Yu kullanmak için yapılandırmanız gerekir. 

_Azuresite/Settings. Kopyala_içinde, `MIDDLEWARE` ayarı bulun ve `whitenoise.middleware.WhiteNoiseMiddleware` ara yazılımı listeye, ara yazılımı hemen altına `django.middleware.security.SecurityMiddleware` ekleyin. `MIDDLEWARE` Ayarınız şöyle görünmelidir:

```python
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'whitenoise.middleware.WhiteNoiseMiddleware',
    ...
]
```

_Azuresite/Settings. Kopyala_sonunda aşağıdaki satırları ekleyin.

```python
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'

STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
```

Whitenoıse 'yi yapılandırma hakkında daha fazla bilgi için bkz. [whitenoıse belgeleri](https://whitenoise.evans.io/en/stable/).

> [!IMPORTANT]
> Veritabanı ayarları bölümü, ortam değişkenlerini kullanmanın en iyi güvenlik uygulamasını zaten izler. Dağıtım önerilerini tamamlayın, bkz. [Docgo belgeleri: dağıtım denetim listesi](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/).

Değişikliklerinizi depoya kaydedin.

```bash
git commit -am "configure for App Service"
```

### <a name="configure-deployment-user"></a>Dağıtım kullanıcısını yapılandırma

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-app-service-plan"></a>App Service planı oluşturma

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Web uygulaması oluşturma

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-python-linux-no-h.md)]

### <a name="configure-environment-variables"></a>Ortam değişkenlerini yapılandırma

Öğreticinin önceki bölümlerinde, PostgreSQL veritabanınıza bağlanmak üzere ortam değişkenleri tanımladınız.

App Service’te, Cloud Shell'de [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) komutunu kullanarak ortam değişkenlerini _uygulama ayarları_ olarak ayarlayabilirsiniz.

Şu örnek, veritabanı bağlantı ayrıntılarını uygulama ayarları olarak belirtir. 

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group myResourceGroup --settings DBHOST="<postgresql-name>.postgres.database.azure.com" DBUSER="manager@<postgresql-name>" DBPASS="supersecretpass" DBNAME="pollsdb"
```

Bu uygulama ayarlarına kodunuzda nasıl erişildiği hakkında daha fazla bilgi için bkz. [erişim ortamı değişkenleri](how-to-configure-python.md#access-environment-variables).

### <a name="push-to-azure-from-git"></a>Git üzerinden Azure'a gönderme

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash 
Counting objects: 7, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (7/7), done.
Writing objects: 100% (7/7), 775 bytes | 0 bytes/s, done.
Total 7 (delta 4), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id '6520eeafcc'.
remote: Generating deployment script.
remote: Running deployment command...
remote: Python deployment.
remote: Kudu sync from: '/home/site/repository' to: '/home/site/wwwroot'
. 
. 
. 
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://<app-name>.scm.azurewebsites.net/<app-name>.git 
   06b6df4..6520eea  master -> master
```  

App Service dağıtım sunucusu, depo kökünde _requirements. txt dosyasını_ görür ve sonra `git push`Python paket yönetimini otomatik olarak çalıştırır.

### <a name="browse-to-the-azure-app"></a>Azure uygulamasına gidin

Dağıtılan uygulamaya gidin. Uygulama ilk kez çağrıldığında kapsayıcının indirilmesi ve çalışması gerektiğinden başlatılması biraz uzun sürebilir. Sayfa zaman aşımına uğrar veya hata iletisi görüntülerse, birkaç dakika bekleyip sayfayı yenileyin.

```bash
http://<app-name>.azurewebsites.net
```

Daha önce oluşturduğunuz yoklama sorusunu görmeniz gerekir. 

App Service, varsayılan olarak tarafından `manage.py startproject` oluşturulan her alt dizinde bir _wsgi.py_ arayarak deponuzdaki bir docgo projesi algılar. Dosyayı bulduğunda, Docgo Web uygulamasını yükler. App Service Python uygulamalarını nasıl yüklediği hakkında daha fazla bilgi için bkz. [yerleşik Python görüntüsünü yapılandırma](how-to-configure-python.md).

`<app-name>.azurewebsites.net` ' A gidin ve oluşturduğunuz yönetici kullanıcı ile oturum açın. İsterseniz, bazı daha fazla yoklama soruları oluşturmayı deneyin.

![Yerel olarak çalışan Python Docgo uygulaması](./media/tutorial-python-postgresql-app/django-admin-azure.png)

**Tebrikler!** Linux için Azure App Service bir Python (Docgo) Web uygulaması çalıştırıyorsunuz.

## <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="manage-your-app-in-the-azure-portal"></a>Uygulamanızı Azure portal yönetme

Oluşturduğunuz uygulamayı görmek için [Azure Portal](https://portal.azure.com) gidin.

Sol menüden **uygulama hizmetleri**' ni ve ardından Azure uygulamanızın adını seçin.

![Azure uygulamasına portal gezintisi](./media/tutorial-python-postgresql-app/app-resource.png)

Varsayılan olarak, Portal uygulamanızın **genel bakış** sayfasını gösterir. Bu sayfa, uygulamanızın nasıl çalıştığını gösterir. Buradan ayrıca göz atma, durdurma, başlatma, yeniden başlatma ve silme gibi temel yönetim görevlerini gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Azure portalında App Service sayfası](./media/tutorial-python-postgresql-app/app-mgmt.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure’da PostgreSQL veritabanı oluşturma
> * Python web uygulamasını PostgreSQL 'e bağlama
> * Python web uygulamasını Azure 'a dağıtma
> * Tanılama günlüklerini görüntüleme
> * Azure portal Python web uygulamasını yönetme

Özel bir DNS adını uygulamanıza nasıl eşleyeceğinizi öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Öğretici: Özel DNS adını uygulamanıza eşleyin](../app-service-web-tutorial-custom-domain.md)

Ya da diğer kaynaklara göz atın:

> [!div class="nextstepaction"]
> [Python uygulamasını yapılandırma](how-to-configure-python.md)
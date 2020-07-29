---
title: 'Öğretici: Postgres ile Python Docgo uygulaması dağıtma'
description: PostgreSQL veritabanı ile bir Python web uygulaması oluşturun ve Azure 'a dağıtın. Öğretici Docgo çerçevesini kullanır ve uygulama Linux üzerinde Azure App Service barındırılır.
ms.devlang: python
ms.topic: tutorial
ms.date: 07/22/2020
ms.custom:
- mvc
- seodec18
- seo-python-october2019
- cli-validate
- tracking-python
ms.openlocfilehash: 718c9a62cc867e5d65cc3c79e78ce3282f1037c7
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87285858"
---
# <a name="tutorial-deploy-a-django-web-app-with-postgresql-in-azure-app-service"></a>Öğretici: Azure App Service içindeki PostgreSQL ile Docgo Web uygulaması dağıtma

Bu öğreticide, veri odaklı bir Python [Docgo](https://www.djangoproject.com/) web uygulamasını [Azure App Service](app-service-linux-intro.md) ve bir Postgres veritabanı için Azure veritabanı 'na bağlamak üzere nasıl dağıtacağınız gösterilmektedir. App Service, yüksek düzeyde ölçeklenebilir, kendini yayama bir Web barındırma hizmeti sağlar.

Bu öğreticide, Azure CLı kullanarak aşağıdaki görevleri tamamlayabilirsiniz:

> [!div class="checklist"]
> * Python ve Azure CLı ile ilk ortamınızı ayarlama
> * PostgreSQL için Azure veritabanı veritabanı oluşturma
> * Azure App Service kodu dağıtın ve Postgres 'e bağlanın
> * Kodunuzu güncelleştirin ve yeniden dağıtın
> * Tanılama günlüklerini görüntüleme
> * Azure portal web uygulamasını yönetme

## <a name="set-up-your-initial-environment"></a>İlk ortamınızı ayarlama

1. Etkin aboneliği olan bir Azure hesabına sahip olmanız gerekir. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
1. <a href="https://www.python.org/downloads/" target="_blank">Python 3,6 veya üstünü</a>yükler.
1. Azure <a href="/cli/azure/install-azure-cli" target="_blank">CLI</a> 2.0.80 veya üstünü yükleyerek Azure kaynaklarını temin etmek ve yapılandırmak için herhangi bir kabukta komut çalıştırırsınız.

Bir Terminal penceresi açın ve Python sürümünüzün 3,6 veya üzeri olduğunu denetleyin:

# <a name="bash"></a>[Bash](#tab/bash)

```bash
python3 --version
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```cmd
py -3 --version
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
py -3 --version
```

---

Azure CLı sürümünüzün 2.0.80 veya üzeri olduğunu kontrol edin:

```azurecli
az --version
```

Daha sonra CLı aracılığıyla Azure 'da oturum açın:

```azurecli
az login
```

Bu komut, kimlik bilgilerinizi toplamak için bir tarayıcı açar. Komut tamamlandığında, abonelikleriniz hakkında bilgi içeren JSON çıkışını gösterir.

Oturum açtıktan sonra, aboneliğinizdeki kaynaklarla çalışmak için Azure CLı ile Azure komutlarını çalıştırabilirsiniz.


## <a name="clone-or-download-the-sample-app"></a>Örnek uygulamayı kopyalayın veya indirin

# <a name="git-clone"></a>[Git kopyası](#tab/clone)

Örnek depoyu kopyalayın:

```terminal
git clone https://github.com/Azure-Samples/djangoapp
```

Daha sonra bu klasöre gidin:

```terminal
cd djangoapp
```

# <a name="download"></a>[İndir](#tab/download)

Web sitesini ziyaret edin [https://github.com/Azure-Samples/djangoapp](https://github.com/Azure-Samples/djangoapp) ve sonra ZIP 'ı **İndir** **' i**seçin. 

ZIP dosyasını *docgoapp*adlı bir klasöre ayıklayın. 

Ardından bu *docgoapp* klasöründe bir Terminal penceresi açın.

---

Docgoapp örneği, Docgo belgelerine [Ilk Docgo uygulamanızı yazmayı](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) izleyerek aldığınız veri tabanlı docgo yoklamaları uygulamasını içerir. Tamamlanmış uygulama size kolaylık sağlaması için verilmiştir.

Örnek ayrıca App Service gibi bir üretim ortamında çalışacak şekilde değiştirilmiştir:

- Üretim ayarları *azuresite/Production. Kopyala* dosyasında bulunur. Geliştirme ayrıntıları *azuresite/Settings. Kopyala*' da bulunur.
- Uygulama, `DJANGO_ENV` ortam değişkeni "üretim" olarak ayarlandığında üretim ayarlarını kullanır. Bu ortam değişkenini daha sonra öğreticide, PostgreSQL veritabanı yapılandırması için kullanılan diğer kullanıcılarla birlikte oluşturursunuz.

Bu değişiklikler, DMİ git 'in herhangi bir üretim ortamında çalıştırılmasını ve App Service özel olmadığını yapılandırmaya özgüdür. Daha fazla bilgi için bkz. [Docgo dağıtım denetim listesi](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/).

## <a name="create-postgres-database-in-azure"></a>Azure 'da Postgres veritabanı oluşturma

<!-- > [!NOTE]
> Before you create an Azure Database for PostgreSQL server, check which [compute generation](/azure/postgresql/concepts-pricing-tiers#compute-generations-and-vcores) is available in your region. If your region doesn't support Gen4 hardware, change *--sku-name* in the following command line to a value that's supported in your region, such as B_Gen4_1.  -->

`db-up`Azure CLI için uzantıyı yükler:

```azurecli
az extension add --name db-up
```

`az`Komut tanınmazsa, [Ilk ortamınızı ayarlama](#set-up-your-initial-environment)bölümünde AÇıKLANDıĞı gibi Azure CLI 'nin yüklü olduğundan emin olun.

Ardından şu komutla Azure 'da Postgres veritabanını oluşturun [`az postgres up`](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up) :

```azurecli
az postgres up --resource-group DjangoPostgres-tutorial-rg --location westus2 --sku-name B_Gen4_1 --server-name <postgre-server-name> --database-name pollsdb --admin-user <admin-username> --admin-password <admin-password> --ssl-enforcement Enabled
```

- *\<postgres-server-name>* Tüm Azure genelinde benzersiz bir adla değiştirin (sunucu uç noktası `https://\<postgres-server-name>.postgres.database.azure.com` ). İyi bir model, şirketinizin adı ve başka bir benzersiz değer birleşimini kullanmaktır.
- *\<admin-username>* Ve için *\<admin-password>* , bu Postgres sunucusu için bir yönetici kullanıcı oluşturmak üzere kimlik bilgilerini belirtin.
- Burada kullanılan B_Gen4_1 (Basic, 4. nesil, 1 çekirdek) [fiyatlandırma katmanı](../../postgresql/concepts-pricing-tiers.md) en düşük maliyetli bir işlemdir. Üretim veritabanları için, `--sku-name` bunun yerine GP_Gen5_2 (genel amaçlı, Gen 5, 2 çekirdek) katmanını kullanmak için bağımsız değişkenini atlayın.

Bu komut, birkaç dakika sürebilen aşağıdaki eylemleri gerçekleştirir:

- Zaten mevcut değilse adlı bir [kaynak grubu](../../azure-resource-manager/management/overview.md#terminology) oluşturun `DjangoPostgres-tutorial-rg` .
- Bir Postgres sunucusu oluşturun.
- Benzersiz bir Kullanıcı adı ve parolasıyla varsayılan bir yönetici hesabı oluşturun. (Kendi kimlik bilgilerinizi belirtmek için, `--admin-user` `--admin-password` komutuyla ve bağımsız değişkenlerini kullanın `az postgres up` .)
- Bir `pollsdb` veritabanı oluşturun.
- Yerel IP adresinizden erişimi etkinleştirin.
- Azure hizmetlerinden erişimi etkinleştirin.
- Veritabanına erişimi olan bir veritabanı kullanıcısı oluşturun `pollsdb` .

Tüm adımları diğer ve komutlarla ayrı olarak yapabilirsiniz `az postgres` `psql` , ancak `az postgres up` tüm adımları bir araya getirebilirsiniz.

Komut tamamlandığında, veritabanı için farklı bağlantı dizeleri içeren bir JSON nesnesi (sunucu URL 'SI, oluşturulan bir Kullanıcı adı (" joyfulKoala@msdocs-djangodb-12345 ") ve bır GUID parolası) verir. Kullanıcı adını ve parolayı, bu öğreticide daha sonra ihtiyacınız olan geçici bir metin dosyasına kopyalayın.

<!-- not all locations support az postgres up -->
> [!TIP]
> `-l <location-name>`, [Azure bölgelerinden](https://azure.microsoft.com/global-infrastructure/regions/)herhangi birine ayarlanabilir. Komutu ile aboneliğiniz için kullanılabilir bölgeleri edinebilirsiniz [`az account list-locations`](/cli/azure/account#az-account-list-locations) . Üretim uygulamaları için, veritabanınızı ve uygulamanızı aynı konuma koyun.

## <a name="deploy-the-code-to-azure-app-service"></a>Kodu Azure App Service dağıtma

Bu bölümde App Service uygulamasında uygulama konağı oluşturur, bu uygulamayı Postgres veritabanına bağlayın ve ardından kodunuzu bu konağa dağıtın.

### <a name="create-the-app-service-app"></a>App Service uygulamasını oluşturma

Terminalde, uygulama kodunu içeren depo kökünde olduğunuzdan emin olun `djangoapp` .

Komutuyla bir App Service uygulaması (ana bilgisayar işlemi) oluşturun [`az webapp up`](/cli/azure/webapp#az-webapp-up) :

```azurecli
az webapp up --resource-group DjangoPostgres-tutorial-rg --location westus2 --plan DjangoPostgres-tutorial-plan --sku B1 --name <app-name>
```
<!-- without --sku creates PremiumV2 plan -->

- `--location`Bağımsız değişkeni için, önceki bölümde veritabanıyla aynı konumu kullanın.
- *\<app-name>* Tüm Azure genelinde benzersiz bir adla değiştirin (sunucu uç noktası `https://\<app-name>.azurewebsites.net` ). *\<app-name>* `A` - `Z` , `0` - `9` Ve `-` için izin verilen karakterler. İyi bir model, şirketinizin adının ve uygulama tanımlayıcısının bir birleşimini kullanmaktır.

Bu komut, birkaç dakika sürebilen aşağıdaki eylemleri gerçekleştirir:

<!-- - Create the resource group if it doesn't exist. `--resource-group` is optional. -->
<!-- No it doesn't. az webapp up doesn't respect --resource-group -->
- Zaten mevcut değilse [kaynak grubunu](../../azure-resource-manager/management/overview.md#terminology) oluşturun. (Bu komutta veritabanını daha önce oluşturduğunuz kaynak grubunu kullanırsınız.)
- Mevcut değilse, temel fiyatlandırma katmanında (B1) *docgopostgres-öğretici-plan* [App Service planını](../overview-hosting-plans.md) oluşturun. `--plan`ve `--sku` isteğe bağlıdır.
- Yoksa App Service uygulamasını oluşturun.
- Zaten etkinleştirilmemişse, uygulama için varsayılan günlüğü etkinleştirin.
- Derleme Otomasyonu etkinken ZIP dağıtımını kullanarak depoyu karşıya yükleyin.

Dağıtım başarılı olduğunda, komut aşağıdaki örnekte olduğu gibi JSON çıktısı üretir:

![Örnek az WebApp Command Output](./media/tutorial-python-postgresql-app/az-webapp-up-output.png)

> [!TIP]
> Birçok Azure CLı komutu, kaynak grubunun adı ve App Service planı gibi ortak parametreleri önbelleğe *. Azure/config*dosyasına dönüştürür. Sonuç olarak, sonraki komutlarla aynı parametreyi belirtmeniz gerekmez. Örneğin, değişiklikleri yaptıktan sonra uygulamayı yeniden dağıtmak için `az webapp up` herhangi bir parametre olmadan yalnızca tekrar çalıştırabilirsiniz. Bununla birlikte, CLı uzantılarından gelen komutlar `az postgres up` mevcut değildir, bu nedenle kaynak grubunu ve konumunu ile burada belirtmek için gerekli olan önbelleği kullanın `az webapp up` .

> [!NOTE]
> Bu noktada uygulamanın URL 'sini ziyaret etmeye çalışırsanız, "DisallowedHost/" hatasıyla karşılaşırsınız. Bu hata, uygulamayı daha önce bahsedilen üretim ayarlarını kullanmak üzere henüz yapılandırmadıysanız aşağıdaki bölümde yaptığınız için oluşur.

### <a name="configure-environment-variables-to-connect-the-database"></a>Veritabanını bağlamak için ortam değişkenlerini yapılandırma

Kod artık App Service dağıtılır ve sonraki adım, uygulamayı Azure 'daki Postgres veritabanına bağlayabilmektedir.

Uygulama kodu, çeşitli ortam değişkenlerinde veritabanı bilgilerini bulmayı bekler. App Service ortam değişkenlerini ayarlamak için, [az WebApp config appSettings set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) komutuyla "uygulama ayarları" oluşturursunuz.

```azurecli
az webapp config appsettings set --settings DJANGO_ENV="production" DBHOST="<postgres-server-name>.postgres.database.azure.com" DBNAME="pollsdb" DBUSER="<username>" DBPASS="<password>"
```

- *\<postgres-server-name>* Daha önce komutuyla kullandığınız adla değiştirin `az postgres up` .
- *\<username>* Ve ' i, *\<password>* komutun sizin için oluşturduğu kimlik bilgileriyle değiştirin.
- Kaynak grubu ve uygulama adı, *. Azure/config* dosyasındaki önbelleğe alınmış değerlerden çizilir.
- Komut `DJANGO_ENV` , `DBHOST` `DBNAME` `DBUSER` `DBPASS` uygulama kodu tarafından beklenen,,, ve adlı ayarları oluşturur.
- Python kodunuzda bu ayarlara, gibi deyimlerle ortam değişkenleri olarak erişirsiniz `os.environ.get('DJANGO_ENV')` . Daha fazla bilgi için bkz. [ortam değişkenlerine erişin](how-to-configure-python.md#access-environment-variables).

### <a name="run-django-database-migrations"></a>Docgo veritabanı geçişlerini Çalıştır

Docgo veritabanı geçişleri, Azure veritabanı 'ndaki PostgreSQL içindeki şemanın kodunuzda açıklananlarla eşleştiğinden emin olun.

1. *Https:// \<app-name> . scm.azurewebsites.net/webssh/Host* sayfasına giderek ve Azure hesabı kimlik bilgilerinizle (veritabanı sunucusu kimlik bilgileri değil) oturum açarak tarayıcıda bir SSH oturumu açın.

1. SSH oturumunda aşağıdaki komutları çalıştırın ( **CTRL** + **SHIFT** + **V**kullanarak komutları yapıştırabilirsiniz):

    ```bash
    cd site/wwwroot
    
    # Activate default virtual environment in App Service container
    source /antenv/bin/activate
    # Install packages
    pip install -r requirements.txt
    # Run database migrations
    python manage.py migrate
    # Create the super user (follow prompts)
    python manage.py createsuperuser
    ```
    
1. Bu `createsuperuser` komut sizden süper kullanıcı kimlik bilgilerini ister. Bu öğreticinin amaçları doğrultusunda, varsayılan kullanıcı adını kullanın `root` , e-posta adresi Için **ENTER** tuşuna basarak boş bırakın ve `Pollsdb1` parola girin.
    
### <a name="create-a-poll-question-in-the-app"></a>Uygulamada bir yoklama sorusu oluşturma

1. Bir tarayıcıda *http: \/ / \<app-name> . azurewebsites.net*URL 'sini açın. Veritabanında henüz belirli bir yoklama olmadığından, uygulamanın "hiçbir yoklama yok" iletisini görüntülemesi gerekir.

1. *Http: \/ / \<app-name> . azurewebsites.net/admin*öğesine gidin. Önceki bölümde bulunan Süper Kullanıcı kimlik bilgilerini kullanarak oturum açın ( `root` ve `Pollsdb1` ). **Yoklamalar**altında, **sorular** ' ın yanındaki **Ekle** ' yi seçin ve bazı seçeneklerle bir yoklama sorusu oluşturun.

1. Soruların artık kullanıcıya sunulduğunu doğrulamak için *http: \/ / \<app-name> . azurewebsites.net/* öğesine tekrar gidin. Bu soruları yanıtlayın, ancak veritabanında bazı veriler oluşturmak istersiniz.

**Tebrikler!** Linux için Azure App Service, etkin bir Postgres veritabanıyla bir Python Docgo Web uygulaması çalıştırıyorsunuz.

> [!NOTE]
> App Service, varsayılan olarak oluşturulan her alt klasörde bir *wsgi.py* dosyası arayarak bir Docgo projesi algılar `manage.py startproject` . App Service bu dosyayı bulduğunda, Docgo Web uygulamasını yükler. Daha fazla bilgi için bkz. [yerleşik Python görüntüsünü yapılandırma](how-to-configure-python.md).


## <a name="make-code-changes-and-redeploy"></a>Kod değişiklikleri yap ve yeniden Dağıt

Bu bölümde, uygulamada yerel değişiklikler yapar ve kodu App Service yeniden dağıtabilirsiniz. İşleminde, devam eden işleri destekleyen bir Python sanal ortamı ayarlarsınız.

### <a name="run-the-app-locally"></a>Uygulamayı yerel olarak çalıştırma

Bir terminal penceresinde aşağıdaki komutları çalıştırın. Süper Kullanıcı oluştururken istemleri izlediğinizden emin olun:

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

```cmd
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

Web uygulaması tam olarak yüklendikten sonra, Dmongo geliştirme sunucusu, "geliştirme sunucusu başlatılıyor" iletisinde yerel uygulama URL 'sini sağlar http://127.0.0.1:8000/ . CTRL-BREAK ile sunucudan çıkın.

![Örnek Docgo geliştirme sunucusu çıkışı](./media/tutorial-python-postgresql-app/django-dev-server-output.png)

Uygulamayı aşağıdaki adımlarla yerel olarak test edin:

1. *Http: \/ /localhost: 8000* adresine giderek bir tarayıcıda "hiçbir yoklama yok" iletisini görüntülemesi gerekir. 

1. *Http: \/ /localhost: 8000/admin* adresine gidin ve daha önce oluşturduğunuz yönetici kullanıcıyı kullanarak oturum açın. **Yoklamalar**' ın altında, **sorular** ' ın yanındaki **Ekle** ' yi seçin ve bazı seçeneklerle bir yoklama sorusu oluşturun. 

1. *Http: \/ /localhost: 8000* ' e gidin ve uygulamayı test etmek için soruyu yanıtlayın. 

1. **CTRL**C tuşlarına basarak Docgo sunucusunu durdurun + **C**.

Yerel olarak çalıştırılırken, uygulama yerel bir SQLite3 veritabanı kullanıyor ve üretim veritabanınızı engellemez. İsterseniz üretim ortamınızın benzetimini yapmak için bir yerel PostgreSQL veritabanı da kullanabilirsiniz.

### <a name="update-the-app"></a>Uygulamayı güncelleştirme

İçinde `polls/models.py` , ile başlayan satırı bulun `choice_text` ve `max_length` parametreyi 100 olarak değiştirin:

```python
# Find this lie of code and set max_length to 100 instead of 200
choice_text = models.CharField(max_length=100)
```

Veri modelini değiştirdiğiniz için yeni bir Docgo geçişi oluşturun ve veritabanını geçirin:

```
python manage.py makemigrations
python manage.py migrate
```

Geliştirme sunucusunu ile yeniden çalıştırın `python manage.py runserver` ve uygulamayı *http: \/ /localhost: 8000/admin*konumunda test edin:

Docgo Web sunucusunu **CTRL** + **C**ile yeniden durdurun.

### <a name="redeploy-the-code-to-azure"></a>Kodu Azure 'a yeniden dağıtın

Depo kökünde aşağıdaki komutu çalıştırın:

```azurecli
az webapp up
```

Bu komut *. Azure/config* dosyasında önbelleğe alınan parametreleri kullanır. App Service, uygulamanın zaten var olduğunu algıladığında kodu yeniden dağıtır.

### <a name="rerun-migrations-in-azure"></a>Azure 'da geçişleri yeniden çalıştırma

Veri modelinde değişiklikler yaptığınız için, App Service veritabanı geçişlerini yeniden çalıştırmanız gerekir.

*Https:// \<app-name> . scm.azurewebsites.net/webssh/Host*SAYFASıNA giderek bir SSH oturumunu tarayıcıda tekrar açın. Sonra aşağıdaki komutları çalıştırın:

```
cd site/wwwroot

# Activate default virtual environment in App Service container
source /antenv/bin/activate
# Run database migrations
python manage.py migrate
```

### <a name="review-app-in-production"></a>Üretimde uygulamayı gözden geçirin

*Http: \/ / \<app-name> . azurewebsites.net* öğesine gidin ve uygulamayı üretimde yeniden test edin. (Yalnızca bir veritabanı alanının uzunluğunu değiştirdiğiniz için, değişiklik yalnızca bir soru oluştururken daha uzun bir yanıt girmeyi denerseniz fark edilir.)

## <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

Azure 'da uygulamayı barındıran kapsayıcının içinden oluşturulan konsol günlüklerine erişebilirsiniz.

Günlük akışını görmek için aşağıdaki Azure CLı komutunu çalıştırın. Bu komut *. Azure/config* dosyasında önbelleğe alınmış parametreleri kullanır.

```azurecli
az webapp log tail
```

Konsol günlüklerini hemen görmüyorsanız, 30 saniye içinde yeniden kontrol edin.

Günlük akışını istediğiniz zaman durdurmak için **CTRL** + **C**yazın.

> [!NOTE]
> Ayrıca, konumundaki tarayıcıdan günlük dosyalarını inceleyebilirsiniz `https://<app-name>.scm.azurewebsites.net/api/logs/docker` .
>
> `az webapp up`Varsayılan günlük kaydını etkinleştirir. Performans nedenleriyle, bu günlüğe kaydetme bir süre sonra devre dışı bırakır, ancak yeniden çalıştırdığınızda her seferinde geri döner `az webapp up` . El ile açmak için aşağıdaki komutu çalıştırın:
>
> ```azurecli
> az webapp log config --docker-container-logging filesystem
> ```

## <a name="manage-your-app-in-the-azure-portal"></a>Uygulamanızı Azure portal yönetme

[Azure Portal](https://portal.azure.com), uygulama adını arayın ve sonuçlarda uygulamayı seçin.

![Azure portal Python Docgo uygulamanıza gidin](./media/tutorial-python-postgresql-app/navigate-to-django-app-in-app-services-in-the-azure-portal.png)

Varsayılan olarak, portal genel bir performans görünümü sağlayan uygulamanızın **genel bakış** sayfasını gösterir. Burada, gezinme, durdurma, yeniden başlatma ve silme gibi temel yönetim görevlerini de gerçekleştirebilirsiniz. Sayfanın sol tarafındaki sekmeler, açabileceğiniz farklı yapılandırma sayfalarını gösterir.

![Python Docgo uygulamanızı Azure portal genel bakış sayfasında yönetin](./media/tutorial-python-postgresql-app/manage-django-app-in-app-services-in-the-azure-portal.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulamayı sürdürmek veya sonraki öğreticiye devam etmek isterseniz, [sonraki adımlara](#next-steps)atlayın. Aksi takdirde, devam eden ücretleri önlemek için bu öğreticide kaynak grubu oluştur ' u silebilirsiniz:

```azurecli
az group delete
```

Komut *. Azure/config* dosyasında önbelleğe alınmış kaynak grubu adını kullanır. Kaynak grubunu silerek, içinde yer alan tüm kaynakları serbest bırakabilir ve silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanıza özel bir DNS adı eşlemeyi öğrenin:

> [!div class="nextstepaction"]
> [Öğretici: özel DNS adını uygulamanıza eşleyin](../app-service-web-tutorial-custom-domain.md)

App Service bir Python uygulamasını nasıl yürüttüğinizi öğrenin:

> [!div class="nextstepaction"]
> [Python uygulamasını yapılandırma](how-to-configure-python.md)

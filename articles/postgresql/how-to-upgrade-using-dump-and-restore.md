---
title: Döküm ve geri yükleme kullanarak yükseltme-PostgreSQL için Azure veritabanı-tek sunucu
description: Daha yüksek bir PostgreSQL için Azure veritabanı 'na geçiş yapmak üzere veritabanlarının dökümünü almak ve geri yüklemek için bir dizi yöntemi açıklar-tek sunucu.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 11/03/2020
ms.openlocfilehash: 26154f4501daba373f1f8b108f1ee7105b1b194f
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93294355"
---
# <a name="upgrade-your-postgresql-database-using-dump-and-restore"></a>Döküm ve geri yükleme kullanarak PostgreSQL veritabanınızı yükseltme

PostgreSQL için Azure veritabanı-tek sunucu, aşağıdaki yöntemlerden birini kullanarak PostgreSQL veritabanı altyapısını daha yüksek bir ana sürüme yükseltmeniz önerilir:
* PostgreSQL [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) ve [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html)kullanarak çevrimdışı yöntem. Bu yöntemde, ilk olarak kaynak sunucunuzdaki dökümü gerçekleştirirsiniz ve ardından hedef sunucuda bu dökümü geri yüklemeniz gerekir.
* [**Veritabanı geçiş hizmeti**](https://docs.microsoft.com/azure/dms/tutorial-azure-postgresql-to-azure-postgresql-online-portal) (DMS) kullanan çevrimiçi yöntem. Bu yöntem hedef veritabanını kaynakla eşitlenmiş halde tutar ve ne zaman kesilmesini istediğinizi seçebilirsiniz. Ancak, değinilmesi gereken bazı Önkoşullar ve kısıtlamalar vardır. 

Ana sürüm yükseltmeleri gerçekleştirmeye yönelik çevrimiçi ve çevrimdışı Yöntemler arasında karar verirken aşağıdaki öneriyi kullanabilirsiniz.

| **Veritabanı** | **Döküm/geri yükleme (çevrimdışı)** | **DMS (çevrimiçi)** |
| ------ | :------: | :-----: |
| Küçük bir veritabanınız var ve yükseltmek için kapalı kalma süresine sahip olabilirsiniz  | X | |
| Küçük veritabanları (< 10 GB)  | X | X | 
| Küçük orta ölçekli DBs (10 GB – 100 GB) | X | X |
| Büyük veritabanları (> 100 GB) |  | X |
| Yükseltme için kapalı kalma süresi uygulanabilir (veritabanı boyutundan bağımsız olarak) | X |  |
| Yeniden başlatma dahil DMS [ön gereksinimleri](https://docs.microsoft.com/azure/dms/tutorial-azure-postgresql-to-azure-postgresql-online-portal#prerequisites) ele alabilir mi? |  | X |
| Yükseltme işlemi sırasında DDLs ve günlüğe kaydedilmeyeceğini kaldırmak olabilir mi? | |  X |

Bu nasıl yapılır Kılavuzu, PostgreSQL pg_dump ve pg_restore komutlarını kullanarak veritabanlarınızın nasıl yükseltileceğiyle ilgili iki örnek yöntem sağlar. Bu belgedeki işlem, veritabanı kaynak sunucudan hedef sunucuya **geçirilse** **yükseltme** olarak adlandırılır. 

> [!NOTE]
> PostgreSQL için döküm ve geri yükleme birçok şekilde gerçekleştirilebilir. Bu belgede adlandırıldıklarından farklı yöntemler kullanmayı tercih edebilirsiniz. Örneğin, bir döküm yapmak için bir PostgreSQL istemcisinden geri yükleme yaparak, adım adım yordam ayrıntıları ve en iyi uygulamalar için lütfen [belgelere](./howto-migrate-using-dump-and-restore.md) bakın. Ek parametrelerle ayrıntılı döküm ve geri yükleme söz dizimi için, [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) makalelere bakın ve [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html). 


## <a name="prerequisites-for-using-dump-and-restore-with-azure-postgresql"></a>Azure PostgreSQL ile döküm ve geri yükleme kullanımı önkoşulları
 
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- 9,5, 9,6 veya 10 çalıştıran bir kaynak veritabanı (PostgreSQL için Azure veritabanı – tek sunucu)
- İstenen PostgreSQL ana sürümü olan hedef veritabanı sunucusu [PostgreSQL Için Azure veritabanı sunucusu](quickstart-create-server-database-portal.md). 
- PostgreSQL içeren bir istemci sistemi (Linux) ve [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) ve [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html) komut satırı yardımcı programları yüklendi. 
- Alternatif olarak, [Azure Cloud Shell](https://shell.azure.com) kullanabilir veya [Azure Portal](https://portal.azure.com)sağ üst köşesindeki menü çubuğunda Azure Cloud Shell tıklayabilirsiniz. `az login`Döküm ve geri yükleme komutlarını çalıştırmadan önce hesabınızda oturum açmanız gerekir.
- Kaynak ve hedef sunucularla aynı bölgede çalışan bir VM gibi PostgreSQL istemci konumunuz. 

## <a name="additional-details-and-considerations"></a>Ek ayrıntılar ve önemli noktalar
- Portalda "bağlantı dizeleri" ne tıklayarak bağlantı dizesini kaynak ve hedef veritabanlarına bulabilirsiniz. 
- Sunucunuzda birden fazla veritabanı çalıştırıyor olabilirsiniz. Kaynak sunucunuza bağlanarak ve çalıştıran veritabanlarının listesini bulabilirsiniz `\l` .
- Hedef veritabanı sunucusunda karşılık gelen veritabanları oluşturun.
- Yükseltme `azure_maintenance` veya şablon veritabanlarını atlayabilirsiniz.
- Veritabanının bu geçiş moduna uygun olduğunu belirtmek için yukarıdaki tablolara bakın.
- Azure Cloud Shell kullanmak istiyorsanız, oturum 20 dakika sonra zaman aşımına uğrar. Veritabanınızın boyutu 10 GB < ise, zaman aşımına uğramadan yükseltmeyi tamamlayabilirsiniz. Aksi takdirde, <Enter> 10-15 dakika içinde tuşu bir kez basmak gibi, oturumu başka yollarla açık tutmanız gerekebilir. 

> [!TIP] 
> - Kaynak ve hedef veritabanı için aynı parolayı kullanıyorsanız, `PGPASSWORD=yourPassword` ortam değişkenini ayarlayabilirsiniz.  Daha sonra psql, pg_dump ve pg_restore gibi komutları çalıştırdığınız zaman parola sağlamanız gerekmez.  Benzer şekilde, vb. gibi ek değişkenler de ayarlayabilirsiniz. `PGUSER` `PGSSLMODE` [PostgreSQL ortam değişkenlerine](https://www.postgresql.org/docs/11/libpq-envars.html)bakın.
>  
> - PostgreSQL sunucunuz TLS/SSL bağlantıları gerektiriyorsa (PostgreSQL için Azure veritabanı sunucuları için varsayılan olarak açık), `PGSSLMODE=require` pg_restore ARACıNıN TLS ile bağlanacağı bir ortam değişkeni ayarlayın. TLS olmadan hata okunmayabilir  `FATAL:  SSL connection is required. Please specify SSL options and retry.`
>
> - Windows komut satırında, `SET PGSSLMODE=require` pg_restore komutunu çalıştırmadan önce komutunu çalıştırın. Linux veya bash içinde `export PGSSLMODE=require` pg_restore komutunu çalıştırmadan önce komutunu çalıştırın.

## <a name="example-database-used-in-this-guide"></a>Bu kılavuzda kullanılan örnek veritabanı

 | **Açıklama** | **Değer** |
 | ------- | ------- |
 | Kaynak sunucu (v 9.5) | pg-95.postgres.database.azure.com |
 | Kaynak veritabanı | bench5gb |
 | Kaynak veritabanı boyutu | 5 GB |
 | Kaynak Kullanıcı adı | pg@pg-95 |
 | Hedef sunucu (v11) | pg-11.postgres.database.azure.com |
 | Hedef veritabanı | bench5gb |
 | Hedef Kullanıcı adı | pg@pg-11 |

## <a name="method-1-upgrade-with-streaming-backups-to-the-target"></a>Yöntem 1: hedefe akış yedeklemeleriyle yükseltme 

Bu yöntemde, tüm veritabanı dökümü doğrudan hedef veritabanı sunucusuna akışla kaydedilir ve dökümünü istemciye depolamaz. Bu nedenle, bu, sınırlı depolamaya sahip bir istemciyle birlikte kullanılabilir ve hatta Azure Cloud Shell çalıştırılabilir. 

1. Veritabanının hedef sunucuda, komutunu kullanarak bulunduğundan emin olun `\l` . Veritabanı yoksa, veritabanını oluşturun.
   ```azurecli-interactive
    psql "host=myTargetServer port=5432 dbname=postgres user=myUser password=###### sslmode=mySSLmode"
    ```
    ```bash
    postgres> \l   
    postgres> create database myTargetDB;
   ```

2. Dökümünü ve geri yüklemeyi, kanal kullanarak tek bir komut satırı olarak çalıştırın. 
    ```azurecli-interactive
    pg_dump -Fc -v --mySourceServer --port=5432 --username=myUser --dbname=mySourceDB | pg_restore -v --no-owner --host=myTargetServer --port=5432 --username=myUser --dbname=myTargetDB
    ```

    Örneğin,

    ```azurecli-interactive
    pg_dump -Fc -v --host=pg-95.postgres.database.azure.com --port=5432 --username=pg@pg-95 --dbname=bench5gb | pg_restore -v --no-owner --host=pg-11.postgres.database.azure.com --port=5432 --username=pg@pg-11 --dbname=bench5gb
    ```  
3. Yükseltme (geçiş) işlemi tamamlandıktan sonra, uygulamanızı hedef sunucuyla test edebilirsiniz. 
4. Sunucu içindeki tüm veritabanları için bu işlemi tekrarlayın.

 Aşağıdaki tabloda, bu yöntemi kullanarak yükseltmenin gerçekleştiği zaman gösterilmektedir. Veriler [pgbench](https://www.postgresql.org/docs/10/pgbench.html)kullanılarak doldurulur. Veritabanınız, pgbench tarafından üretilen tablo ve dizinlerden değişen boyutlarda farklı sayıda nesneye sahip olduğundan, veritabanınızı yükseltmek için gereken gerçek süreyi anlamak üzere veritabanınızın dökümünü ve geri yüklemesini test etmek önemle önerilir. 

| **Veritabanı boyutu** | **Yaklaşık olarak geçen süre** | 
| ----- | ------ |
| 1 GB  | 1-2 dakika |
| 5 GB | 8-10 dakika |
| 10 GB | 15-20 dakika |
| 50 GB | 1-1.5 saat |
| 100 GB | 2,5-3 saat|
   
## <a name="method-2-upgrade-with-parallel-dump-and-restore"></a>Yöntem 2: paralel döküm ve geri yükleme ile yükseltme 

Bu yöntem, veritabanında birkaç daha büyük tablonuz varsa ve bu veritabanı için döküm ve geri yükleme işlemini paralel hale getirmek istiyorsanız faydalıdır. Veritabanlarınızın yedekleme dökümlerini barındırmak için yeterli yerel disk depolama alanı gerekir. Bu paralel döküm ve geri yükleme işlemi, tüm geçiş/yükseltme işlemlerini tamamlamaya yönelik zaman tüketimini azaltır. Örneğin, geçiş için 1-1.5 saat geçen 50 GB pgbench veritabanı 30 dakikadan kısa bir süre içinde tamamlandı.

1. Kaynak sunucunuzdaki her bir veritabanı için hedef sunucuda karşılık gelen bir veritabanını oluşturun.

   ```bash
    psql "host=myTargetServer port=5432 dbname=postgres user=myuser password=###### sslmode=mySSLmode"
    postgresl> create database myDB;
   ```
   Örneğin,
    ```bash
    psql "host=pg-11.postgres.database.azure.com port=5432 dbname=postgres user=pg@pg-11 password=###### sslmode=require"

    postgres> create database bench5gb;
    postgres> \q
    ```

2. Pg_dump komutunu, iş sayısı = 4 (veritabanındaki tablo sayısı) ile bir dizin biçiminde çalıştırın. Daha büyük işlem katmanıyla ve daha fazla tablo ile, daha yüksek bir sayı ile artırabilirsiniz. Bu pg_dump, her iş için sıkıştırılmış dosyaları depolamak üzere bir dizin oluşturacak.

    ```bash
    pg_dump -Fd -v --host=sourceServer --port=5432 --username=myUser --dbname=mySourceDB -j 4 -f myDumpDirectory
    ```
    Örneğin,
    ```bash
    pg_dump -Fd -v --host=pg-95.postgres.database.azure.com --port=5432 --username=pg@pg-95 --dbname=bench5gb -j 4 -f dump.dir
    ```

3. Ardından, hedef sunucuda yedeklemeyi geri yükleyin.
    ```bash
    $ pg_restore -v --no-owner --host=myTargetServer --port=5432 --username=myUser --dbname=myTargetDB -j 4 myDumpDir
    ```
    Örneğin,
    ```bash
    $ pg_restore -v --no-owner --host=pg-11.postgres.database.azure.com --port=5432 --username=pg@pg-11 --dbname=bench5gb -j 4 dump.dir
    ```

> [!TIP]
> Bu belgede bahsedilen işlem, önizleme aşamasında olan PostgreSQL için Azure veritabanınızı (esnek sunucu) yükseltmek için de kullanılabilir. Temel fark, esnek sunucu hedefinin bağlantı dizesidir `@dbName` .  Örneğin, Kullanıcı adı ise `pg` , bağlantı dizesindeki tek sunuculu Kullanıcı adı olacaktır `pg@pg-95` , esnek sunucu ile kullanmanız yeterlidir `pg` .

## <a name="next-steps"></a>Sonraki Adımlar

- Hedef veritabanı işlevine memnun olduktan sonra eski veritabanı sunucunuzu bırakabilirsiniz. 
- Kaynak sunucuyla aynı veritabanı uç noktasını kullanmak istiyorsanız eski kaynak veritabanı sunucunuzu sildikten sonra eski veritabanı sunucu adıyla bir okuma çoğaltması oluşturabilirsiniz. Kararlı durum kurulduktan sonra çoğaltmayı durdurabilirsiniz, bu da Çoğaltma sunucusunu bağımsız bir sunucu olacak şekilde yükseltir. Daha fazla ayrıntı için bkz. [çoğaltma](./concepts-read-replicas.md) .
- Bu komutları üretimde kullanmadan önce test ortamında sınamayı ve doğrulamayı unutmayın.

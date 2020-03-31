---
title: Dökümü ve geri yükleme - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: PostgreSQL veritabanının döküm dosyasına nasıl ayıklanır ve Pg_dump tarafından oluşturulan bir dosyadan PostgreSQL - Single Server için azure veritabanında nasıl geri yüklenir.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 4365338efa56593e80edcc19cba5944b213d2b72
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74770246"
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>PostgreSQL veritabanınızı döküm ve geri yükleme kullanarak geçirme
[pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) bir demleme dosyasına PostgreSQL veritabanı ayıklamak ve pg_dump tarafından oluşturulan bir arşiv dosyasından PostgreSQL veritabanını geri yüklemek için [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html) kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- Altında erişime ve veritabanına izin vermek için güvenlik duvarı kurallarına sahip [PostgreSQL sunucusu için](quickstart-create-server-database-portal.md) bir Azure Veritabanı.
- [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) ve [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html) komut satırı yardımcı programları yüklü

PostgreSQL veritabanınızı boşaltmak ve geri yüklemek için aşağıdaki adımları izleyin:

## <a name="create-a-dump-file-using-pg_dump-that-contains-the-data-to-be-loaded"></a>Yüklenecek verileri içeren pg_dump kullanarak bir döküm dosyası oluşturma
Varolan bir PostgreSQL veritabanını şirket içinde veya VM'de yedeklemek için aşağıdaki komutu çalıştırın:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> -f <database>.dump
```
Örneğin, içinde yerel bir sunucu ve **testdb** adında bir veritabanı varsa
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb -f testdb.dump
```


## <a name="restore-the-data-into-the-target-azure-database-for-postgresql-using-pg_restore"></a>verileri, pg_restore kullanarak PostgreSQL için hedef Azure Veritabanına geri yükleme
Hedef veritabanını oluşturduktan sonra, dökümü dosyasından hedef veritabanına verileri geri yüklemek için pg_restore komutunu ve -d, -dbname parametresini kullanabilirsiniz.
```bash
pg_restore -v --no-owner --host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
--sahipsiz parametre dahil olmak üzere, geri yükleme sırasında oluşturulan tüm nesnelerin kullanıcı adı ile belirtilen kullanıcıya ait olması neden olur. Daha fazla bilgi [için, pg_restore'daki](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html)resmi PostgreSQL belgelerine bakın.

> [!NOTE]
> PostgreSQL sunucunuz SSL bağlantıları gerektiriyorsa (Varsayılan olarak PostgreSQL sunucuları `PGSSLMODE=require` için Azure Veritabanı'nda), pg_restore aracının SSL'ye bağlanması için bir ortam değişkeni ayarlayın. SSL olmadan, hata okunabilir`FATAL:  SSL connection is required. Please specify SSL options and retry.`
>
> Windows komut satırında, pg_restore `SET PGSSLMODE=require` komutunu çalıştırmadan önce komutu çalıştırın. Linux veya Bash pg_restore `export PGSSLMODE=require` komutu çalıştırmadan önce komutu çalıştırın.
>

Bu örnekte, döküm dosyası **testdb.dump** veri **veritabanı mypgsqldb** hedef sunucu **mydemoserver.postgres.database.azure.com**verileri geri yükleyin. 
```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## <a name="optimizing-the-migration-process"></a>Geçiş işlemini optimize etme

Varolan PostgreSQL veritabanınızı PostgreSQL hizmeti için Azure Veritabanı'na geçirmenin bir yolu, kaynağındaki veritabanını yedeklemek ve Azure'da geri yüklemektir. Geçişi tamamlamak için gereken süreyi en aza indirmek için, yedekleme ve geri yükleme komutlarıyla aşağıdaki parametreleri kullanmayı düşünün.

> [!NOTE]
> Ayrıntılı sözdizimi bilgileri için [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) ve [pg_restore](https://www.postgresql.org/docs/current/static/app-pgrestore.html)makalelere bakın.
>

### <a name="for-the-backup"></a>Yedekleme için
- Yedeklemeyi -Fc anahtarıyla alın, böylece geri yüklemeyi hızlandırmak için paralel olarak gerçekleştirebilirsiniz. Örnek:

    ```
    pg_dump -h MySourceServerName -U MySourceUserName -Fc -d MySourceDatabaseName -f Z:\Data\Backups\MyDatabaseBackup.dump
    ```

### <a name="for-the-restore"></a>Geri yükleme için
- Yedekleme dosyasını, geçiş yaptığınız PostgreSQL sunucusu için Azure Veritabanı ile aynı bölgedeki bir Azure VM'sine taşımanızı ve ağ gecikmesüresini azaltmak için bu VM'den pg_restore yapmanızı öneririz. Ayrıca, VM'nin [hızlandırılmış ağ](../virtual-network/create-vm-accelerated-networking-powershell.md) etkinleştirilmiş olarak oluşturulmasını da öneririz.

- Zaten varsayılan olarak yapılmalıdır, ancak dizin oluşturma deyimlerinin verilerin eklenmesinden sonra olduğunu doğrulamak için döküm dosyasını açın. Durum böyle değilse, veriler ekildikten sonra dizin oluşturma deyimlerini taşıyın.

- Geri yükleme paralelleştirmek için -Fc ve -j *#* anahtarları ile geri yükleyin. *#* hedef sunucudaki çekirdek sayısıdır. Ayrıca, etkiyi *#* görmek için hedef sunucunun çekirdek sayısının iki katına kadar ayarlı olarak da deneyebilirsiniz. Örnek:

    ```
    pg_restore -h MyTargetServer.postgres.database.azure.com -U MyAzurePostgreSQLUserName -Fc -j 4 -d MyTargetDatabase Z:\Data\Backups\MyDatabaseBackup.dump
    ```

- Ayrıca komut *kümesi synchronous_commit = off;* başında ve komut kümesi synchronous_commit *= on ekleyerek* dökümü dosyasını da layabilirsiniz; sonunda. Uygulamalar verileri değiştirmeden önce sonunda açmamak, daha sonra veri kaybına neden olabilir.

- PostgreSQL sunucusu için hedef Azure Veritabanı'nda, geri yüklemeden önce aşağıdakileri yapmayı düşünün:
    - Geçiş sırasında bu istatistikler gerekli olmadığından sorgu performans izlemeyi kapatın. Bunu pg_stat_statements.track, pg_qs.query_capture_mode ve pgms_wait_sampling.query_capture_mode'ı NONE'a ayarlayarak yapabilirsiniz.

    - Geçişi hızlandırmak için 32 vCore Memory Optimize edilmiş gibi yüksek bir işlem ve yüksek bellek sku kullanın. Geri yükleme tamamlandıktan sonra kolayca tercih ettiğiniz sku aşağı ölçeklendirebilirsiniz. Sku ne kadar yüksekse, pg_restore komutundaki `-j` karşılık gelen parametreyi artırarak o kadar paralellik elde edebilirsiniz. 

    - Hedef sunucudaki daha fazla IOPS geri yükleme performansını artırabilir. Sunucunun depolama boyutunu artırarak daha fazla IOPS sağlayabilirsiniz. Bu ayar geri döndürülemez, ancak daha yüksek bir IOPS'nin gelecekte gerçek iş yükünüze fayda sağlayıp sağamayacağını düşünün.

Bu komutları üretimde kullanmadan önce test ortamında test etmeyi ve doğrulamayı unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
- Dışa aktarma ve alma kullanarak bir PostgreSQL veritabanını geçirmek için, [dışa aktarma ve alma'yı kullanarak PostgreSQL veritabanınızı geçir'e](howto-migrate-using-export-and-import.md)bakın.
- Veritabanlarını PostgreSQL için Azure Veritabanı'na geçirme hakkında daha fazla bilgi için [Veritabanı Geçiş Kılavuzu'na](https://aka.ms/datamigration)bakın.

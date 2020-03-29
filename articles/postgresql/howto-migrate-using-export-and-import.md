---
title: Veritabanını geçirin - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bir PostgreSQL veritabanını komut dosyası dosyası dosyasına nasıl ayıkladığını ve verileri bu dosyadan hedef veritabanına nasıl aktardığını açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: f7cf5d245383b8a58f03e2e3610750866a2f4b5a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74770212"
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>Dışa aktarma ve alma kullanarak PostgreSQL veritabanınızı geçirin
Bir PostgreSQL veritabanını komut dosyası dosyasına ve [psql'e](https://www.postgresql.org/docs/current/static/app-psql.html) ayıklamak için [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunda adım atmak için şunları yapmanız gerekir:
- Altında erişime ve veritabanına izin vermek için güvenlik duvarı kurallarına sahip [PostgreSQL sunucusu için](quickstart-create-server-database-portal.md) bir Azure Veritabanı.
- [pg_dump](https://www.postgresql.org/docs/current/static/app-pgdump.html) komut satırı yardımcı programı yüklü
- [psql](https://www.postgresql.org/docs/current/static/app-psql.html) command-line yardımcı programı yüklü

PostgreSQL veritabanınızı dışa aktarmak ve almak için aşağıdaki adımları izleyin.

## <a name="create-a-script-file-using-pg_dump-that-contains-the-data-to-be-loaded"></a>Yüklenecek verileri içeren pg_dump kullanarak bir komut dosyası dosyası oluşturma
Varolan PostgreSQL veritabanınızı şirket içinde veya VM'de bir sql script dosyasına aktarmak için, varolan ortamınızda aşağıdaki komutu çalıştırın:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Örneğin, içinde yerel bir sunucu ve **testdb** adında bir veritabanı varsa:
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postgresql"></a>PostgreSQL için hedef Azure Veritabanı'ndaki verileri alma
Verileri PostgreSQL sunucusu için Azure Veritabanı'na almak ve verileri sql dosyasından yüklemek için psql komut satırını ve --dbname parametresini (-d) kullanabilirsiniz.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
Bu örnekte, hedef sunucudaki **mypgsqldb** veritabanına veri almak için önceki adımdan **testdb.sql** adlı psql yardımcı programı ve bir komut dosyası dosyası **mydemoserver.postgres.database.azure.com.**
```bash
psql --file=testdb.sql --host=mydemoserver.database.windows.net --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb
```

## <a name="next-steps"></a>Sonraki adımlar
- Bir PostgreSQL veritabanını dökümü ve geri yüklemeyi kullanarak geçirmek için, [bkz.](howto-migrate-using-dump-and-restore.md)
- Veritabanlarını PostgreSQL için Azure Veritabanı'na geçirme hakkında daha fazla bilgi için [Veritabanı Geçiş Kılavuzu'na](https://aka.ms/datamigration)bakın. 

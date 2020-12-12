---
title: Bağlanma ve sorgulama-esnek sunucu PostgreSQL
description: Hızlı başlangıç bağlantıları, PostgreSQL için Azure veritabanı esnek sunucusuna nasıl bağlanacağınızı ve sorguları nasıl çalıştıracağınızı gösterir.
services: postgresql
ms.service: postgresql
ms.topic: how-to
author: mksuni
ms.author: sumuth
ms.date: 12/08/2020
ms.openlocfilehash: ee3b1f7db8bdafb1233b32579e032e8c864c37a9
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2020
ms.locfileid: "97364612"
---
# <a name="connect-and-query-overview-for-azure-database-for-postgresql--flexible-server"></a>PostgreSQL için Azure veritabanı 'na bağlanma ve sorguya genel bakış-esnek sunucu

Aşağıdaki belge, PostgreSQL için Azure veritabanı tek sunucu ile bağlanma ve sorgu oluşturmayı gösteren örneklere bağlantılar içerir. Bu kılavuzda, aşağıdaki desteklenen dillerde sunucusuna bağlanmak için kullanabileceğiniz TLS önerileri ve uzantısı da bulunur.

>[!IMPORTANT]
> PostgreSQL için Azure veritabanı esnek sunucu **önizlemededir**.

## <a name="quickstarts"></a>Hızlı Başlangıçlar

| Hızlı Başlangıç | Açıklama |
|---|---|
|[PgAdmin](https://www.pgadmin.org/)|Sunucuya bağlanmak için pgAdmin kullanabilirsiniz ve veritabanı nesnelerinin oluşturulmasını, bakımını ve kullanımını basitleştirir.|
|[Azure Cloud Shell 'de psql](./quickstart-create-server-cli.md#connect-using-postgresql-command-line-client)|Bu makalede, sunucunuzdaki verileri sorgulamak, eklemek, güncelleştirmek ve silmek için [Azure Cloud Shell](../../cloud-shell/overview.md) [**psql**](https://www.postgresql.org/docs/current/static/app-psql.html) 'in nasıl çalıştırılacağı gösterilmektedir. Geliştirme ortamınızda yüklüyse **psql** 'i çalıştırabilirsiniz|
|[Python](connect-python.md)|Bu hızlı başlangıçta Python kullanarak bir veritabanına bağlanma ve verileri sorgulamak için veritabanı nesneleriyle çalışma kullanma işlemlerinin nasıl yapılacağı gösterilmiştir. |
|[App Service ile docgo](tutorial-django-app-service-postgres.md)|Bu öğreticide, Ruby kullanarak bir veritabanına bağlanma ve verileri sorgulamak için veritabanı nesneleriyle çalışma kullanma adlı bir program oluşturma işlemlerinin nasıl yapılacağı gösterilmektedir.|

## <a name="tls-considerations-for-database-connectivity"></a>Veritabanı bağlantısı için TLS konuları

Aktarım Katmanı Güvenliği (TLS), Microsoft 'un PostgreSQL için Azure veritabanı 'nda veritabanlarına bağlanmak için sağladığı veya desteklediği tüm sürücüler tarafından kullanılır. Özel yapılandırma gerekmez, ancak yeni oluşturulan sunucular için TLS 1,2 ' i zorunlu tutun. TLS 1,0 ve 1,1 kullanıyorsanız, sunucularınızın TLS sürümünü güncelleştirmeniz önerilir. Bkz. [TLS Yapılandırma](how-to-connect-tls-ssl.md)

## <a name="postgresql-extensions"></a>PostgreSQL uzantıları

PostgreSQL, uzantıları kullanarak veritabanınızın işlevselliğini genişletmenizi sağlar. Uzantılar birden çok ilgili SQL nesnesini tek bir komutla veritabanınıza yüklenip kaldırılabilecek bir paket haline getirir. Veritabanınıza yüklenen uzantılar, yerleşik özellikler gibi çalışır.

- [Postgres 12 uzantıları](./concepts-extensions.md#postgres-12-extensions)
- [Postgres 11 uzantıları](./concepts-extensions.md#postgres-11-extensions)
- [dblink ve postgres_fdw](./concepts-extensions.md#dblink-and-postgres_fdw)
- [pg_prewarm](./concepts-extensions.md#pg_prewarm)
- [pg_stat_statements](./concepts-extensions.md#pg_stat_statements)

Daha fazla ayrıntı için bkz. [Esnek sunucuda PostgreSQL uzantılarını kullanma](concepts-extensions.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Döküm ve geri yükleme kullanarak verileri geçirme](../howto-migrate-using-dump-and-restore.md)
- [İçeri ve dışarı aktarma kullanarak verileri geçirme](../howto-migrate-using-export-and-import.md)

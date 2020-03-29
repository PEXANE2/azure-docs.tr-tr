---
title: Bağlantı kitaplıkları - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için Azure Veritabanı'nı bağlamak ve sorgulamak için uygulamaları kodlarken kullanabileceğiniz birkaç kitaplık ve sürücü açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 2305a02e5b094ec9e98f39363ddbd0c39221ab0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74768903"
---
# <a name="connection-libraries-for-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı için bağlantı kitaplıkları - Single Server
Bu makalede, geliştiricilerin PostgreSQL için Azure Veritabanı'na bağlanmak ve sorgulamak için uygulamalar geliştirmek için kullanabilecekleri kitaplıklar ve sürücüler listeleilmektedir.

## <a name="client-interfaces"></a>İstemci arabirimleri
PostgreSQL sunucusuna bağlanmak için kullanılan çoğu dil istemci kitaplıkları dış projelerdir ve bağımsız olarak dağıtılır. Listelenen kitaplıklar, PostgreSQL için Azure Veritabanı'na bağlanmak için Windows, Linux ve Mac platformlarında desteklenir. Sonraki adımlar bölümünde birkaç hızlı başlangıç örneği listelenir.

| **Dil** | **İstemci arabirimi** | **Ek bilgiler** | **İndir** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | DB API 2.0 uyumlu | [İndir](http://initd.org/psycopg/download/) |
| PHP | [php-pgsql](https://secure.php.net/manual/en/book.pgsql.php) | Veritabanı uzantısı | [Yükle](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [Pg npm paketi](https://www.npmjs.com/package/pg) | Pure JavaScript engelleyici olmayan istemci | [Yükle](https://www.npmjs.com/package/pg) |
| Java | [JDBC](https://jdbc.postgresql.org/) | Tip 4 JDBC sürücüsü | [Indir](https://jdbc.postgresql.org/download.html)  |
| Ruby | [Pg mücevher](https://deveiate.org/code/pg/) | Yakut Arayüzü | [İndir](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Başlayın | [Paket pq](https://godoc.org/github.com/lib/pq) | Pure Go postgres sürücüsü | [Yükle](https://github.com/lib/pq/blob/master/README.md) |
| C\#/ .NET | [Npgsql](https://www.npgsql.org/) | ADO.NET Veri Sağlayıcısı | [İndir](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | ODBC Sürücüsü | [İndir](https://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Birincil C dil arabirimi | Dahil |
| C++ | [libpqxx](http://pqxx.org/) | Yeni stil C++ arabirimi | [İndir](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Sonraki adımlar
Seçtiğiniz dili kullanarak PostgreSQL için Azure Veritabanı'na bağlanma ve sorgu la ilgili bu hızlı başlangıçları okuyun:

[Python](./connect-python.md) | [Düğümü.JS](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) | [PHP](./connect-php.md) | [.NET (C#)](./connect-csharp.md) | [Git](./connect-go.md)

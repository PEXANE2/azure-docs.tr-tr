---
title: Bağlantı kitaplıkları-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede, Connect ve PostgreSQL için Azure veritabanı-tek sunucu sorgulama için uygulama kodlayıp kullanabileceğiniz çeşitli kitaplıklar ve sürücüler açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 2305a02e5b094ec9e98f39363ddbd0c39221ab0b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74768903"
---
# <a name="connection-libraries-for-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı bağlantı kitaplıkları-tek sunucu
Bu makalede, geliştiricilerin PostgreSQL için Azure veritabanı 'na bağlanmak ve veritabanını sorgulamak üzere uygulama geliştirmek için kullanabilecekleri kitaplıklar ve sürücüler listelenmiştir.

## <a name="client-interfaces"></a>İstemci arabirimleri
PostgreSQL sunucusuna bağlanmak için kullanılan çoğu dil istemci kitaplığı, dış projelerdir ve bağımsız olarak dağıtılır. Listelenen kitaplıklar, PostgreSQL için Azure veritabanı 'na bağlanmak üzere Windows, Linux ve Mac platformlarında desteklenir. Birkaç hızlı başlangıç örneği, sonraki adımlar bölümünde listelenmiştir.

| **Dil** | **İstemci arabirimi** | **Ek bilgiler** | **İndir** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | DB API 2,0 uyumlu | [İndir](http://initd.org/psycopg/download/) |
| PHP | [php-pgsql](https://secure.php.net/manual/en/book.pgsql.php) | Veritabanı uzantısı | [Yükle](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [Sayfalık NPM paketi](https://www.npmjs.com/package/pg) | Saf JavaScript engelleyici olmayan istemci | [Yükle](https://www.npmjs.com/package/pg) |
| Java | [JDBC](https://jdbc.postgresql.org/) | Tür 4 JDBC sürücüsü | [İndirme](https://jdbc.postgresql.org/download.html)  |
| Ruby | [Sayfalık GED](https://deveiate.org/code/pg/) | Ruby arabirimi | [İndir](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Başlayın | [Paket PQ](https://godoc.org/github.com/lib/pq) | Saf go Postgres sürücüsü | [Yükle](https://github.com/lib/pq/blob/master/README.md) |
| C\#/.net | [Npgsql](https://www.npgsql.org/) | ADO.NET Veri Sağlayıcısı | [İndir](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | ODBC Sürücüsü | [İndir](https://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Birincil C dil arabirimi | Dahil |
| C++ | [libpqxx](http://pqxx.org/) | Yeni stil C++ arabirimi | [İndir](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Sonraki adımlar
Tercih ettiğiniz dili kullanarak PostgreSQL için Azure veritabanı 'na bağlanma ve sorgulama ile ilgili bu hızlı başlangıçlardan birini okuyun:

[Python](./connect-python.md) | [Node. js](./connect-nodejs.md) | [Java](./connect-java.md) | [Ruby](./connect-ruby.md) |  | [Go](./connect-go.md) [PHP](./connect-php.md)php .net[(C#) go](./connect-csharp.md) | 

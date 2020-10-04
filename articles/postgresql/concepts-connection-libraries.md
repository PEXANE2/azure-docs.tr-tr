---
title: Bağlantı kitaplıkları-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede, Connect ve PostgreSQL için Azure veritabanı-tek sunucu sorgulama için uygulama kodlayıp kullanabileceğiniz çeşitli kitaplıklar ve sürücüler açıklanır.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 45081c6ba161686498398f2c4ccae8b4cff4c0d1
ms.sourcegitcommit: 19dce034650c654b656f44aab44de0c7a8bd7efe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2020
ms.locfileid: "91704319"
---
# <a name="connection-libraries-for-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı bağlantı kitaplıkları-tek sunucu
Bu makalede, geliştiricilerin PostgreSQL için Azure veritabanı 'na bağlanmak ve veritabanını sorgulamak üzere uygulama geliştirmek için kullanabilecekleri kitaplıklar ve sürücüler listelenmiştir.

## <a name="client-interfaces"></a>İstemci arabirimleri
PostgreSQL sunucusuna bağlanmak için kullanılan çoğu dil istemci kitaplığı, dış projelerdir ve bağımsız olarak dağıtılır. Listelenen kitaplıklar, PostgreSQL için Azure veritabanı 'na bağlanmak üzere Windows, Linux ve Mac platformlarında desteklenir. Birkaç hızlı başlangıç örneği, sonraki adımlar bölümünde listelenmiştir.

| **Dil** | **İstemci arabirimi** | **Ek bilgi** | **İndir** |
|--------------|----------------------------------------------------------------|-------------------------------------|--------------------------------------------------------------------|
| Python | [psycopg](http://initd.org/psycopg/) | DB API 2,0 uyumlu | [İndir](http://initd.org/psycopg/download/) |
| PHP | [php-pgsql](https://secure.php.net/manual/en/book.pgsql.php) | Veritabanı uzantısı | [Yükleme](https://secure.php.net/manual/en/pgsql.installation.php) |
| Node.js | [Sayfalık NPM paketi](https://www.npmjs.com/package/pg) | Saf JavaScript engelleyici olmayan istemci | [Yükleme](https://www.npmjs.com/package/pg) |
| Java | [JDBC](https://jdbc.postgresql.org/) | Tür 4 JDBC sürücüsü | [İndirme](https://jdbc.postgresql.org/download.html)  |
| Ruby | [Sayfalık GED](https://deveiate.org/code/pg/) | Ruby arabirimi | [İndir](https://rubygems.org/downloads/pg-0.20.0.gem) |
| Go | [Paket PQ](https://godoc.org/github.com/lib/pq) | Saf go Postgres sürücüsü | [Yükleme](https://github.com/lib/pq/blob/master/README.md) |
| C \# /.net | [Npgsql](https://www.npgsql.org/) | ADO.NET Veri Sağlayıcısı | [İndir](https://www.microsoft.com/net/) |
| ODBC | [psqlODBC](https://odbc.postgresql.org/) | ODBC Sürücüsü | [İndir](https://www.postgresql.org/ftp/odbc/versions/) |
| C | [libpq](https://www.postgresql.org/docs/9.6/static/libpq.html) | Birincil C dil arabirimi | Dahil |
| C++ | [libpqxx](http://pqxx.org/) | Yeni stil C++ arabirimi | [İndir](http://pqxx.org/download/software/) |

## <a name="next-steps"></a>Sonraki adımlar
Tercih ettiğiniz dili kullanarak PostgreSQL için Azure veritabanı 'na bağlanma ve sorgulama ile ilgili bu hızlı başlangıçlardan birini okuyun:

[Python](./connect-python.md)  |  [Node.JS](./connect-nodejs.md)  |  [Java](./connect-java.md)  |  [Ruby](./connect-ruby.md)  |  [Php](./connect-php.md)  |  [.net (C#)](./connect-csharp.md)  |  [Git](./connect-go.md)

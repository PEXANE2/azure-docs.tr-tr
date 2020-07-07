---
title: Bağlantı kitaplıkları-MySQL için Azure veritabanı
description: Bu makalede, istemci programlarının MySQL için Azure veritabanı 'na bağlanırken kullanabileceği her bir kitaplık veya sürücü listelenir.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 5f83f937b8d9ec50ec7dc6ec781bff1f435eb45a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79537202"
---
# <a name="connection-libraries-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı bağlantı kitaplıkları
Bu makalede, istemci programlarının MySQL için Azure veritabanı 'na bağlanırken kullanabileceği her bir kitaplık veya sürücü listelenir.

## <a name="client-interfaces"></a>İstemci arabirimleri
MySQL, sektör standartları ODBC ve JDBC ile uyumlu uygulamalar ve araçlarla MySQL kullanmak için standart veritabanı sürücüsü bağlantısı sunar. ODBC veya JDBC ile birlikte çalışarak herhangi bir sistem MySQL kullanabilir.

| **Dil** | **Platform** | **Ek kaynak** | **İndir** |
| :----------- | :------------| :-----------------------| :------------|
| PHP | Windows, Linux | [PHP için MySQL Native Driver-mysqlnd](https://dev.mysql.com/downloads/connector/php-mysqlnd/) | [İndir](https://secure.php.net/downloads.php) |
| ODBC | Windows, Linux, Mac OS X ve UNIX platformları | [MySQL Bağlayıcısı/ODBC Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-odbc/en/) | [İndir](https://dev.mysql.com/downloads/connector/odbc/) |
| ADO.NET | Windows | [MySQL Bağlayıcısı/NET Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-net/en/) | [İndir](https://dev.mysql.com/downloads/connector/net/) |
| JDBC | Platformdan bağımsız | [MySQL Bağlayıcısı/J 5,1 Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-j/5.1/en/) | [İndir](https://dev.mysql.com/downloads/connector/j/) |
| Node.js | Windows, Linux, Mac OS X | [sıdorares/node-mysql2](https://github.com/sidorares/node-mysql2/tree/master/documentation) | [İndir](https://github.com/sidorares/node-mysql2) |
| Python | Windows, Linux, Mac OS X | [MySQL Bağlayıcısı/Python Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-python/en/) | [İndir](https://dev.mysql.com/downloads/connector/python/) |
| C++ | Windows, Linux, Mac OS X | [MySQL Bağlayıcısı/C++ Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-cpp/en/) | [İndir](https://dev.mysql.com/downloads/connector/python/) |
| C | Windows, Linux, Mac OS X | [MySQL Bağlayıcısı/C Geliştirici Kılavuzu](https://dev.mysql.com/doc/refman/8.0/en/c-api.html) | [İndir](https://dev.mysql.com/downloads/connector/c/)
| Perl | Windows, Linux, Mac OS X ve UNIX platformları | [DBD:: MySQL](https://metacpan.org/pod/DBD::mysql) | [İndir](https://metacpan.org/pod/DBD::mysql) |


## <a name="next-steps"></a>Sonraki adımlar
Tercih ettiğiniz dili kullanarak MySQL için Azure veritabanı 'na bağlanma ve sorgu oluşturma konusunda bu hızlı başlangıçlardan birini okuyun:

[Php](./connect-php.md)  |  [Java](./connect-java.md)  |   [.net (C#)](./connect-csharp.md)  |  [Python](./connect-python.md)  |  [Node.JS](./connect-nodejs.md)  |  [Ruby](./connect-ruby.md)  |  [C++](connect-cpp.md)  |  [Git](./connect-go.md)


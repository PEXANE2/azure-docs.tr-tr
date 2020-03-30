---
title: Bağlantı kitaplıkları - MySQL için Azure Veritabanı
description: Bu makalede, MySQL için Azure Veritabanı'na bağlanırken istemci programlarının kullanabileceği her kitaplık veya sürücü listelenilir.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 5f83f937b8d9ec50ec7dc6ec781bff1f435eb45a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79537202"
---
# <a name="connection-libraries-for-azure-database-for-mysql"></a>MySQL için Azure Veritabanı için bağlantı kitaplıkları
Bu makalede, MySQL için Azure Veritabanı'na bağlanırken istemci programlarının kullanabileceği her kitaplık veya sürücü listelenilir.

## <a name="client-interfaces"></a>İstemci arabirimleri
MySQL, Endüstri standartları ODBC ve JDBC ile uyumlu uygulamalar ve araçlarla MySQL'i kullanmak için standart veritabanı sürücüsü bağlantısı sunar. ODBC veya JDBC ile çalışan herhangi bir sistem MySQL'i kullanabilir.

| **Dil** | **Platform** | **Ek Kaynak** | **İndir** |
| :----------- | :------------| :-----------------------| :------------|
| PHP | Windows, Linux | [PHP için MySQL yerli sürücüsü - mysqlnd](https://dev.mysql.com/downloads/connector/php-mysqlnd/) | [İndir](https://secure.php.net/downloads.php) |
| ODBC | Windows, Linux, Mac OS X ve Unix platformları | [MySQL Bağlayıcısı/ODBC Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-odbc/en/) | [İndir](https://dev.mysql.com/downloads/connector/odbc/) |
| ADO.NET | Windows | [MySQL Bağlayıcısı/Net Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-net/en/) | [İndir](https://dev.mysql.com/downloads/connector/net/) |
| JDBC | Platform bağımsız | [MySQL Bağlayıcısı/J 5.1 Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-j/5.1/en/) | [İndir](https://dev.mysql.com/downloads/connector/j/) |
| Node.js | Windows, Linux, Mac OS X | [sidorares/düğüm-mysql2](https://github.com/sidorares/node-mysql2/tree/master/documentation) | [İndir](https://github.com/sidorares/node-mysql2) |
| Python | Windows, Linux, Mac OS X | [MySQL Bağlayıcısı/Python Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-python/en/) | [İndir](https://dev.mysql.com/downloads/connector/python/) |
| C++ | Windows, Linux, Mac OS X | [MySQL Bağlayıcısı/C++ Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-cpp/en/) | [İndir](https://dev.mysql.com/downloads/connector/python/) |
| C | Windows, Linux, Mac OS X | [MySQL Bağlayıcısı/C Geliştirici Kılavuzu](https://dev.mysql.com/doc/refman/8.0/en/c-api.html) | [İndir](https://dev.mysql.com/downloads/connector/c/)
| Perl | Windows, Linux, Mac OS X ve Unix platformları | [DBD::MySQL](https://metacpan.org/pod/DBD::mysql) | [İndir](https://metacpan.org/pod/DBD::mysql) |


## <a name="next-steps"></a>Sonraki adımlar
Seçtiğiniz dili kullanarak MySQL için Azure Veritabanı'na bağlanma ve sorgulama hakkında bu hızlı başlangıçları okuyun:

[PHP](./connect-php.md) | [Java](./connect-java.md) |  [.NET (C#)](./connect-csharp.md) | [Python](./connect-python.md) | [Düğümü.JS](./connect-nodejs.md) | [Ruby](./connect-ruby.md) | [C++](connect-cpp.md) | [Git](./connect-go.md)


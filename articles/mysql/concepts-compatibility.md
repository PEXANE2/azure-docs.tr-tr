---
title: Sürücü ve araç uyumluluğu - MySQL için Azure Veritabanı
description: Bu makalede, MySQL için Azure Veritabanı ile uyumlu MySQL sürücüleri ve yönetim araçları açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: e8917a0a5678c4c6b72352a0d4c1523bfea3c96d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79537219"
---
# <a name="mysql-drivers-and-management-tools-compatible-with-azure-database-for-mysql"></a>MySQL için Azure Veritabanı ile uyumlu MySQL sürücüleri ve yönetim araçları
Bu makalede, MySQL için Azure Veritabanı ile uyumlu sürücüler ve yönetim araçları açıklanmaktadır.

## <a name="mysql-drivers"></a>MySQL Sürücüleri
MySQL için Azure Veritabanı, MySQL veritabanının dünyanın en popüler topluluk sürümünü kullanır. Bu nedenle, programlama dilleri ve sürücüleri geniş bir yelpazede ile uyumludur. Amaç, MySQL sürücülerinin en son üç sürümü desteklemektir ve MySQL sürücülerinin işlevselliğini ve kullanılabilirliğini sürekli olarak geliştirmek için açık kaynak topluluğundan yazarlarla birlikte çalışmalar devam etmektedir. MySQL 5.6 ve 5.7 için Azure Veritabanı ile uyumlu olduğu tespit edilen ve test edilen sürücülerin listesi aşağıdaki tabloda verilmiştir:

| **Programlama Dili** | **Sürücü** | **Bağlantılar** | **Uyumlu Sürümler** | **Uyumsuz Sürümler** | **Notlar** |
| :----------------------- | :--------- | :-------- | :---------------------- | :------------------------ | :-------- |
| PHP | mysqli, pdo_mysql, mysqlnd | https://secure.php.net/downloads.php | 5.5, 5.6, 7.x | 5.3 | SSL MySQLi ile PHP 7.0 bağlantısı için bağlantı dizesinde MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT ekleyin. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO seti: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` yanlış seçeneği.|
| .NET | .NET için Async MySQL Konektörü | https://github.com/mysql-net/MySqlConnector <br> [Nuget'ten kurulum paketi](https://www.nuget.org/packages/MySqlConnector/) | 0.27 ve sonrası | 0.26.5 ve öncesi | |
| .NET | MySQL Bağlayıcısı/NET | https://github.com/mysql/mysql-connector-net | 6.6.3 ,7.0 ,8.0 |  | Bir kodlama hatası, UTF8 olmayan bazı Windows sistemlerinde bağlantıların başarısız lığa neden olabilir. |
| Node.js | mysqljs | https://github.com/mysqljs/mysql/ <br> NPM'den kurulum paketi:<br> NPM'den çalıştırın `npm install mysql` | 2.15 | 2.14.1 ve öncesi | |
| Node.js | düğüm-mysql2 | https://github.com/sidorares/node-mysql2 | 1.3.4+ | | |
| Başlayın | MySQL Sürücüsü git | https://github.com/go-sql-driver/mysql/releases | 1.3, 1.4 | 1.2 ve öncesi | Sürüm `allowNativePasswords=true` 1.3 için bağlantı dizesinde kullanın. Sürüm 1.4 bir `allowNativePasswords=true` düzeltme içerir ve artık gerekli değildir. |
| Python | MySQL Bağlayıcısı/Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2, MySQL 8.0 ile 8.0.16+ kullanın  | 1.2.2 ve öncesi | |
| Python | PyMySQL | https://pypi.org/project/PyMySQL/ | 0.7.11, 0.8.0, 0.8.1, 0.9.3+ | 0.9.0 - 0.9.2 (web2py'de gerileme) | |
| Java | MariaDB Konektör/J | https://downloads.mariadb.org/connector-java/ | 2.1, 2.0, 1.6 | 1.5.5 ve öncesi | | 
| Java | MySQL Bağlayıcısı/J | https://github.com/mysql/mysql-connector-j | 5.1.21+, MySQL 8.0 ile 8.0.17+ kullanın | 5.1.20 ve altı | |
| C | MySQL Bağlayıcısı/C (libmysqlclient) | https://dev.mysql.com/doc/refman/5.7/en/c-api-implementations.html | 6.0.2+ | | |
| C | MySQL Konektörü/ODBC (myodbc) | https://github.com/mysql/mysql-connector-odbc | 3.51.29+ | | |
| C++ | MySQL Bağlayıcısı/C++ | https://github.com/mysql/mysql-connector-cpp | 1.1.9+ | 1.1.3 ve altı | | 
| C++ | MySQL++| https://tangentsoft.net/mysql++ | 3.2.3+ | | |
| Ruby | mysql2 | https://github.com/brianmario/mysql2 | 0.4.10+ | | |
| R | RMySQL | https://github.com/rstats-db/RMySQL | 0.10.16+ | | |
| Swift | mysql-swift | https://github.com/novi/mysql-swift | 0.7.2+ | | |
| Swift | buhar/mysql | https://github.com/vapor/mysql-kit | 2.0.1+ | | |

## <a name="management-tools"></a>Yönetim Araçları
Uyumluluk avantajı veritabanı yönetim araçlarını da genişletir. Veritabanı işleme kullanıcı izinleri sınırları içinde çalıştığı sürece, varolan araçlarınız MySQL için Azure Veritabanı ile çalışmaya devam etmelidir. MySQL 5.6 ve 5.7 için Azure Veritabanı ile uyumlu olduğu tespit edilen ve test edilen üç yaygın veritabanı yönetim aracı aşağıdaki tabloda listelenmiştir:

|                                     | **MySQL Çalışma Tezgahı 6.x ve yukarı** | **Navicat 12** | **PHPMyAdmin 4.x ve yukarı** |
| :---------------------------------- | :----------------------------- | :------------- | :-------------------------|
| Oluşturma, Güncelleme, Okuma, Yazma, Silme | X | X | X |
| SSL Bağlantısı | X | X | X |
| SQL Query Otomatik Tamamlama | X | X |  |
| İçe Aktarma ve Verme Verileri | X | X | X | 
| Birden Çok Biçime Dışa Aktarma | X | X | X |
| Yedekleme ve Geri Yükleme |  | X |  |
| Sunucu Parametrelerini Görüntüle | X | X | X |
| İstemci Bağlantılarını Görüntüle | X | X | X |

## <a name="next-steps"></a>Sonraki adımlar

- [MySQL için Azure Veritabanı bağlantı sorunlarını giderme](howto-troubleshoot-common-connection-issues.md)
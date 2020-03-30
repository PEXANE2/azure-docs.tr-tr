---
title: Sürücüler ve araçlar uyumluluğu - MariaDB için Azure Veritabanı
description: Bu makalede, MariaDB için Azure Veritabanı ile uyumlu MariaDB sürücüleri ve yönetim araçları açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: a0cee198f028fd90e04dac15e98d7cd33aee9201
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79532357"
---
# <a name="mariadb-drivers-and-management-tools-compatible-with-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı ile uyumlu MariaDB sürücüleri ve yönetim araçları

Bu makalede, MariaDB için Azure Veritabanı ile uyumlu sürücüler ve yönetim araçları açıklanmaktadır.

## <a name="mariadb-drivers"></a>MariaDB Sürücüler

MariaDB için Azure Veritabanı, MariaDB sunucusunun topluluk sürümünü kullanır. Bu nedenle, programlama dilleri ve sürücüleri geniş bir yelpazede ile uyumludur. MariaDB'nin API'si ve protokolü MySQL tarafından kullanılanlarla uyumludur. Bu, MySQL ile çalışan bağlayıcıların MariaDB ile de çalışması gerektiği anlamına gelir.

Amaç, MariaDB sürücülerinin en son üç versiyonunu desteklemektir ve MariaDB sürücülerinin işlevselliğini ve kullanılabilirliğini sürekli olarak geliştirmek için açık kaynak topluluğundan yazarlarla birlikte çalışmalar devam etmektedir. MariaDB 10.2 için Azure Veritabanı ile uyumlu olduğu tespit edilen ve test edilmiş sürücülerin listesi aşağıdaki tabloda verilmiştir:

**Sürücü** | **Bağlantılar** | **Uyumlu Sürümler** | **Uyumsuz Sürümler** | **Notlar**
---|---|---|---|---
PHP | https://secure.php.net/downloads.php | 5.5, 5.6, 7.x | 5.3 | SSL MySQLi ile PHP 7.0 bağlantısı için bağlantı dizesinde MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT ekleyin. <br> ```mysqli_real_connect($conn, $host, $username, $password, $db_name, 3306, NULL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);```<br> PDO seti: ```PDO::MYSQL_ATTR_SSL_VERIFY_SERVER_CERT``` yanlış seçeneği.
.NET | [GitHub'da MySqlConnector](https://github.com/mysql-net/MySqlConnector) <br> [Nuget'ten kurulum paketi](https://www.nuget.org/packages/MySqlConnector/) | 0.27 ve sonrası | 0.26.5 ve öncesi |
MySQL Bağlayıcısı/NET | [MySQL Bağlayıcısı/NET](https://github.com/mysql/mysql-connector-net) | 8.0, 7.0, 6.10 |  | Bir kodlama hatası, UTF8 olmayan bazı Windows sistemlerinde bağlantıların başarısız lığa neden olabilir.
Node.js |  [GitHub üzerinde MySQLjs](https://github.com/mysqljs/mysql/) <br> NPM'den kurulum paketi:<br> NPM'den çalıştırın `npm install mysql` | 2.15 | 2.14.1 ve öncesi
GO | https://github.com/go-sql-driver/mysql/releases | 1.3, 1.4 | 1.2 ve öncesi | Sürüm `allowNativePasswords=true` 1.3 için bağlantı dizesinde kullanın. Sürüm 1.4 bir `allowNativePasswords=true` düzeltme içerir ve artık gerekli değildir.
Python | https://pypi.python.org/pypi/mysql-connector-python | 1.2.3, 2.0, 2.1, 2.2 | 1.2.2 ve öncesi |
Java | https://downloads.mariadb.org/connector-java/ | 2.1, 2.0, 1.6 | 1.5.5 ve öncesi |

## <a name="management-tools"></a>Yönetim Araçları

Uyumluluk avantajı veritabanı yönetim araçlarını da genişletir. Veritabanı işleme kullanıcı izinleri sınırları içinde çalıştığı sürece, varolan araçlarınız MariaDB için Azure Veritabanı ile çalışmaya devam etmelidir. Test edilmiş ve MariaDB 10.2 için Azure Veritabanı ile uyumlu olduğu tespit edilen üç yaygın veritabanı yönetim aracı aşağıdaki tabloda listelenmiştir:

| | **MySQL Çalışma Tezgahı 6.x ve yukarı** | **Navicat 12** | **PHPMyAdmin 4.x ve yukarı**
---|---|---|---
Oluşturma, Güncelleme, Okuma, Yazma, Silme | X | X | X
SSL Bağlantısı | X | X | X
SQL Query Otomatik Tamamlama | X | X |
İçe Aktarma ve Verme Verileri | X | X | X
Birden Çok Biçime Dışa Aktarma | X | X | X
Yedekleme ve Geri Yükleme |  | X |
Sunucu Parametrelerini Görüntüle | X | X | X
İstemci Bağlantılarını Görüntüle | X | X | X

## <a name="next-steps"></a>Sonraki adımlar

- [MariaDB için Azure Veritabanı bağlantı sorunlarını giderme](howto-troubleshoot-common-connection-issues.md)
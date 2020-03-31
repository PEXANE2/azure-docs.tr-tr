---
title: Bağlantı dizeleri - MariaDB için Azure Veritabanı
description: Bu belge, ADO.NET (C#), JDBC, Node.js, ODBC, PHP, Python ve Ruby de dahil olmak üzere MariaDB için Azure Veritabanı'na bağlanmak üzere uygulamalar için şu anda desteklenen bağlantı dizelerini listeler.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 74574fb905ae4ebd2552ef97cd0b5430eea6363a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79530232"
---
# <a name="how-to-connect-applications-to-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı'na uygulama bağlama
Bu konu, MariaDB için Azure Veritabanı tarafından desteklenen bağlantı dize türlerini şablonlar ve örneklerle birlikte listeler. Bağlantı dizenizde farklı parametreler ve ayarlar olabilir.

- Sertifikayı almak için [SSL'yi nasıl yapılandırılatırılabilirsiniz.](./howto-configure-ssl.md)
- {your_host} = [servername].mariadb.database.azure.com
- {your_user}@{servername} = doğru kimlik doğrulama için userID biçimi.  Yalnızca userID kullanırsanız, kimlik doğrulama başarısız olur.

## <a name="adonet"></a>ADO.NET
```csharp
Server={your_host}; Port=3306; Database={your_database}; Uid={username@servername}; Pwd={your_password}; SslMode=Preferred;
```

Bu örnekte, sunucu `mydemoserver`adı , veritabanı `wpdb`adı , `WPAdmin`kullanıcı adı , `mypassword!2`ve şifre . Sonuç olarak, bağlantı dizesi olmalıdır:

```csharp
Server= "mydemoserver.mariadb.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@mydemoserver"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a>JDBC
```java
String url ="jdbc:mariadb://{your_host}:3306/{your_database}?useSSL=true&trustServerCertificate=true"; myDbConn = DriverManager.getConnection(url, "{username@servername}", {your_password});
```

## <a name="nodejs"></a>Node.js
```javascript
var conn = mysql.createConnection({host: "{your_host}", user: "{your_username}", password: {your_password}, database: {your_database}, port: 3306, ssl:{ca:fs.readFileSync({ca-cert filename})}});
```

## <a name="odbc"></a>ODBC
```cpp
DRIVER={MARIADB ODBC 3.0 Driver}; Server="{your_host}"; Port=3306; Database={your_database}; Uid="{username@servername}"; Pwd={your_password}; sslca={ca-cert filename}; sslverify=1;
```

## <a name="php"></a>PHP
```php
$con=mysqli_init(); mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL); mysqli_real_connect($con, "{your_host}", "{username@servername}", {your_password}, {your_database}, 3306);
```

## <a name="python"></a>Python
```python
cnx = mysql.connector.connect(user="{username@servername}", password={your_password}, host="{your_host}", port=3306, database={your_database}, ssl_ca={ca-cert filename}, ssl_verify_cert=true)
```

## <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(username: "{username@servername}", password: {your_password}, database: {your_database}, host: "{your_host}", port: 3306, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA')
```

## <a name="get-the-connection-string-details-from-the-azure-portal"></a>Azure portalından bağlantı dizeayrıntılarını alma
Azure [portalında,](https://portal.azure.com)MariaDB sunucusu için Azure Veritabanınıza gidin ve ardından örnek için dize ![listesini almak için **Bağlantı dizeleri'ni** tıklatın: Azure portalındaki Bağlantı dizeleri bölmesi](./media/howto-connection-strings/connection-strings-on-portal.png)

Dize, sürücü, sunucu ve diğer veritabanı bağlantı parametreleri gibi ayrıntıları sağlar. Veritabanı adı, parola ve benzeri kendi parametrelerinizi kullanmak için bu örnekleri değiştirin. Daha sonra koduve uygulamaları sunucuya bağlanmak için bu dize kullanabilirsiniz.

<!-- 
## Next steps
- For more information about connection libraries, see [Concepts - Connection libraries](./concepts-connection-libraries.md).
- -->

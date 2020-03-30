---
title: Bağlantı dizeleri - MySQL için Azure Veritabanı
description: Bu belge, ADO.NET (C#), JDBC, Node.js, ODBC, PHP, Python ve Ruby de dahil olmak üzere MySQL için Azure Veritabanı'na bağlanmak üzere uygulamalar için şu anda desteklenen bağlantı dizelerini listeler.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: ff257e101ca925a554f2343d357e2042d2930b45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062388"
---
# <a name="how-to-connect-applications-to-azure-database-for-mysql"></a>MySQL için Azure Veritabanına nasıl uygulama bağlanır
Bu konu, MySQL için Azure Veritabanı tarafından desteklenen bağlantı dize türlerini şablonlar ve örneklerle birlikte listeler. Bağlantı dizenizde farklı parametreler ve ayarlar olabilir.

- Sertifikayı almak için [SSL'yi nasıl yapılandırılatırılabilirsiniz.](./howto-configure-ssl.md)
- {your_host} \<= servername>.mysql.database.azure.com
- {your_user}@{servername} = doğru kimlik doğrulama için userID biçimi.  Yalnızca userID kullanırsanız, kimlik doğrulama başarısız olur.

## <a name="adonet"></a>ADO.NET
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password};[SslMode=Required;]
```

Bu örnekte, sunucu `mydemoserver`adı , veritabanı `wpdb`adı , `WPAdmin`kullanıcı adı , `mypassword!2`ve şifre . Sonuç olarak, bağlantı dizesi olmalıdır:

```ado.net
Server= "mydemoserver.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@mydemoserver"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a>JDBC
```jdbc
String url ="jdbc:mysql://%s:%s/%s[?verifyServerCertificate=true&useSSL=true&requireSSL=true]",{your_host},{your_port},{your_database}"; myDbConn = DriverManager.getConnection(url, {username@servername}, {your_password}";
```

## <a name="nodejs"></a>Node.js
```node.js
var conn = mysql.createConnection({host: {your_host}, user: {username@servername}, password: {your_password}, database: {your_database}, Port: {your_port}[, ssl:{ca:fs.readFileSync({ca-cert filename})}}]);
```

## <a name="odbc"></a>ODBC
```odbc
DRIVER={MySQL ODBC 5.3 UNICODE Driver};Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password}; [sslca={ca-cert filename}; sslverify=1; Option=3;]
```

## <a name="php"></a>PHP
```php
$con=mysqli_init(); [mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL);] mysqli_real_connect($con, {your_host}, {username@servername}, {your_password}, {your_database}, {your_port});
```

## <a name="python"></a>Python
```python
cnx = mysql.connector.connect(user={username@servername}, password={your_password}, host={your_host}, port={your_port}, database={your_database}[, ssl_ca={ca-cert filename}, ssl_verify_cert=true])
```

## <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(username: {username@servername}, password: {your_password}, database: {your_database}, host: {your_host}, port: {your_port}[, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA'])
```

## <a name="get-the-connection-string-details-from-the-azure-portal"></a>Azure portalından bağlantı dizeayrıntılarını alma
Azure [portalında](https://portal.azure.com)MySQL sunucusu için Azure Veritabanınıza gidin ve ardından örnek için dize ![listesini almak için **Bağlantı dizelerini** tıklatın: Azure portalındaki Bağlantı dizeleri bölmesi](./media/howto-connection-strings/connection-strings-on-portal.png)

Dize, sürücü, sunucu ve diğer veritabanı bağlantı parametreleri gibi ayrıntıları sağlar. Veritabanı adı, parola ve benzeri kendi parametrelerinizi kullanmak için bu örnekleri değiştirin. Daha sonra koduve uygulamaları sunucuya bağlanmak için bu dize kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Bağlantı kitaplıkları hakkında daha fazla bilgi için [Bkz. Kavramlar - Bağlantı kitaplıkları.](./concepts-connection-libraries.md)

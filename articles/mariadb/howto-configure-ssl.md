---
title: MariaDB için SSL - Azure Veritabanını Yapılandırma
description: SSL bağlantılarını doğru kullanmak için MariaDB ve ilişkili uygulamalar için Azure Veritabanı'nın doğru şekilde nasıl yapılandırılacaklarına ilişkin yönergeler
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 8264e78d938d91782c45697cc226148adadadb14
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985839"
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanına güvenli bir şekilde bağlanmak için uygulamanızdaki SSL bağlantısını yapılandırın
MariaDB için Azure Veritabanı, Güvenli Soketkatmanı (SSL) kullanarak MariaDB sunucusu için Azure Veritabanınızı istemci uygulamalarına bağlamayı destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="obtain-ssl-certificate"></a>SSL sertifikası alma
MariaDB sunucusu için Azure Veritabanınızla SSL üzerinden iletişim [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) kurmak için gereken sertifikayı indirin ve sertifika dosyasını yerel sürücünüze kaydedin (bu öğretici örneğin c:\ssl kullanır).
**Microsoft Internet Explorer ve Microsoft Edge için:** İndirme tamamlandıktan sonra sertifikayı BaltimoreCyberTrustRoot.crt.pem olarak yeniden adlandırın.

## <a name="bind-ssl"></a>Bağlama SSL

### <a name="connecting-to-server-using-mysql-workbench-over-ssl"></a>SSL üzerinden MySQL Workbench kullanarak sunucuya bağlanma
MySQL Workbench'i SSL üzerinden güvenli bir şekilde bağlanmak için yapılandırın. 

1. Kurulum Yeni Bağlantı iletişiminden **SSL** sekmesine gidin. 

1. **SSL kullan** alanını "İsteyin" olarak güncelleştirin.

1. **SSL CA Dosyasında:** alan, **BaltimoreCyberTrustRoot.crt.pem**dosya konumunu girin. 
    
    ![SSL yapılandırması kaydet](./media/howto-configure-ssl/mysql-workbench-ssl.png)

Varolan bağlantılar için, bağlantı simgesine sağ tıklayarak SSL'yi bağlayabilir ve düzenlemeyi seçebilirsiniz. Ardından **SSL** sekmesine gidin ve sertifika dosyasını bağlayın.

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>SSL üzerinden MySQL CLI'yi kullanarak sunucuya bağlanma
SSL sertifikasını bağlamanın başka bir yolu da aşağıdaki komutları uygulayarak MySQL komut satırı arabirimini kullanmaktır. 

```bash
mysql.exe -h mydemoserver.mariadb.database.azure.com -u Username@mydemoserver -p --ssl-mode=REQUIRED --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

> [!NOTE]
> Windows'da MySQL komut satırı arabirimini kullanırken `SSL connection error: Certificate signature check failed`bir hata alabilirsiniz. Bu durumda, parametreleri `--ssl-mode=REQUIRED --ssl-ca={filepath}` `--ssl`.

## <a name="enforcing-ssl-connections-in-azure"></a>Azure'da SSL bağlantılarını uygulama 
### <a name="using-the-azure-portal"></a>Azure portalını kullanma
Azure portalını kullanarak, MariaDB sunucusu için Azure Veritabanınızı ziyaret edin ve ardından **Bağlantı güvenliğini**tıklatın. **SSL bağlantı** ayarını zorlamak veya devre dışı katmak için geçiş düğmesini kullanın ve ardından **Kaydet'i**tıklatın. Microsoft, gelişmiş güvenlik için **SSL bağlantı** ayarını her zaman etkinleştirmenizi önerir.
![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma
Azure CLI'de sırasıyla Etkin veya Devre dışı bırakılan değerleri kullanarak **ssl zorlama** parametresini etkinleştirebilir veya devre dışı bırakabilirsiniz.
```azurecli-interactive
az mariadb server update --resource-group myresource --name mydemoserver --ssl-enforcement Enabled
```

## <a name="verify-the-ssl-connection"></a>SSL bağlantısını doğrulama
SSL kullanarak MariaDB sunucunuza bağlı olduğunuzu doğrulamak için mysql **durum** komutunu uygulayın:
```sql
status
```
Bağlantının çıktıyı gözden geçirerek şifrelenerek şifrelenerek doğrulanın: **SSL: Kullanımdaki şifreleme AES256-SHA** 

## <a name="sample-code"></a>Örnek kod
Uygulamanızdan SSL üzerinden MariaDB için Azure Veritabanı'na güvenli bir bağlantı kurmak için aşağıdaki kod örneklerine bakın:

### <a name="php"></a>PHP
```php
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'mydemoserver.mariadb.database.azure.com', 'myadmin@mydemoserver', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="python-mysqlconnector-python"></a>Python (MySQLConnector Python)
```python
try:
    conn = mysql.connector.connect(user='myadmin@mydemoserver',
                                   password='yourpassword',
                                   database='quickstartdb',
                                   host='mydemoserver.mariadb.database.azure.com',
                                   ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
    print(err)
```
### <a name="python-pymysql"></a>Python (PyMySQL)
```python
conn = pymysql.connect(user='myadmin@mydemoserver',
                       password='yourpassword',
                       database='quickstartdb',
                       host='mydemoserver.mariadb.database.azure.com',
                       ssl={'ssl': {'ca': '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'}})
```
### <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(
        :host     => 'mydemoserver.mariadb.database.azure.com', 
        :username => 'myadmin@mydemoserver',      
        :password => 'yourpassword',    
        :database => 'quickstartdb',
        :sslca => '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'
        :ssl_mode => 'required'
    )
```
#### <a name="ruby-on-rails"></a>Raylı Yakut
```ruby
default: &default
  adapter: mysql2
  username: username@mydemoserver
  password: yourpassword
  host: mydemoserver.mariadb.database.azure.com
  sslca: BaltimoreCyberTrustRoot.crt.pem
  sslverify: true
```

### <a name="golang"></a>Golang
```go
rootCertPool := x509.NewCertPool()
pem, _ := ioutil.ReadFile("/var/www/html/BaltimoreCyberTrustRoot.crt.pem")
if ok := rootCertPool.AppendCertsFromPEM(pem); !ok {
    log.Fatal("Failed to append PEM.")
}
mysql.RegisterTLSConfig("custom", &tls.Config{RootCAs: rootCertPool})
var connectionString string
connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true&tls=custom",'myadmin@mydemoserver' , 'yourpassword', 'mydemoserver.mariadb.database.azure.com', 'quickstartdb') 
db, _ := sql.Open("mysql", connectionString)
```
### <a name="java-jdbc"></a>Java (JDBC)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mysql://%s/%s?serverTimezone=UTC&useSSL=true", 'mydemoserver.mariadb.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```
### <a name="java-mariadb"></a>Java (MariaDB)
```java
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mariadb://%s/%s?useSSL=true&trustServerCertificate=true", 'mydemoserver.mariadb.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@mydemoserver');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

### <a name="net-mysqlconnector"></a>.NET (MySqlConnector)
```csharp
var builder = new MySqlConnectionStringBuilder
{
    Server = "mydemoserver.mysql.database.azure.com",
    UserID = "myadmin@mydemoserver",
    Password = "yourpassword",
    Database = "quickstartdb",
    SslMode = MySqlSslMode.VerifyCA,
    CACertificateFile = "BaltimoreCyberTrustRoot.crt.pem",
};
using (var connection = new MySqlConnection(builder.ConnectionString))
{
    connection.Open();
}
```

<!-- 
## Next steps
Review various application connectivity options following [Connection libraries for Azure Database for MariaDB](concepts-connection-libraries.md)
-->

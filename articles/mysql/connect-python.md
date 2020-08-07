---
title: Python kullanarak bağlanma-MySQL için Azure veritabanı
description: Bu hızlı başlangıçta, MySQL için Azure Veritabanı'na bağlanmak ve buradan veri sorgulamak için kullanabileceğiniz birkaç Python kod örneği sağlanmıştır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom:
- mvc
- seo-python-october2019
- devx-track-python
ms.devlang: python
ms.topic: quickstart
ms.date: 5/26/2020
ms.openlocfilehash: 8d417fdc604fa132e3ada5380b9e0a1f7326af04
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87843241"
---
# <a name="quickstart-use-python-to-connect-and-query-data-with-azure-database-for-mysql"></a>Hızlı başlangıç: MySQL için Azure veritabanı ile bağlanmak ve veri sorgulamak için Python kullanma

Bu hızlı başlangıçta Python kullanarak MySQL için Azure veritabanı 'na bağlanırsınız. Ardından, Mac, Ubuntu Linux ve Windows platformlarındaki veritabanındaki verileri sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerini kullanın. 

Bu konuda, Python kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz ve MySQL için Azure veritabanı ile çalışmaya yeni başladığınızı varsaymaktadır.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- MySQL için Azure veritabanı sunucusu. [Azure Portal kullanarak MySQL Için Azure veritabanı sunucusu oluşturun](quickstart-create-mysql-server-database-using-azure-portal.md) veya [Azure CLI kullanarak MySQL için Azure veritabanı sunucusu oluşturun](quickstart-create-mysql-server-database-using-azure-cli.md).

> [!IMPORTANT] 
> Bağlanmakta olduğunuz IP adresinin [Azure Portal](./howto-manage-firewall-using-portal.md) veya [Azure CLI](./howto-manage-firewall-using-cli.md) kullanarak sunucunun güvenlik duvarı kurallarını eklendiğinden emin olun

## <a name="install-python-and-the-mysql-connector"></a>Python’u ve MySQL bağlayıcısını yükleme

Aşağıdaki adımları kullanarak bilgisayarınıza Python ve Python için MySQL bağlayıcısını yüklersiniz: 

> [!NOTE]
> Bu hızlı başlangıç, MySQL 'e bağlanmak için ham SQL sorgu yaklaşımını kullanır. Bir Web çerçevesi kullanıyorsanız, Framework için önerilen bağlayıcıyı kullanın, örneğin, Docgo için [mysqlclient](https://pypi.org/project/mysqlclient/) .

1. İşletim sistemi için [Python 3,7 veya üstünü](https://www.python.org/downloads/) indirin ve yükleyin. MySQL Bağlayıcısı için gerekli olduğundan Python 'u uygulamanıza eklediğinizden emin olun `PATH` .
   
1. Bir komut istemi veya `bash` kabuk açın ve `python -V` büyük harf V anahtarıyla çalıştırarak Python sürümünüzü denetleyin.
   
1. `pip`Paket yükleyicisi, Python 'un en son sürümlerine dahildir. ' İ `pip` çalıştırarak en son sürüme güncelleştirin `pip install -U pip` . 
   
   `pip`Yüklü değilse, ile indirip yükleyebilirsiniz `get-pip.py` . Daha fazla bilgi için bkz. [yükleme](https://pip.pypa.io/en/stable/installing/). 
   
1. `pip`Python Için MySQL bağlayıcısını ve bağımlılıklarını yüklemek için kullanın:
   
   ```bash
   pip install mysql-connector-python
   ```
   
   MySQL için Python bağlayıcısını [MySQL.com](https://dev.mysql.com/downloads/connector/python/)adresinden da yükleyebilirsiniz. Python için MySQL Bağlayıcısı hakkında daha fazla bilgi için [MySQL Bağlayıcısı/Python Geliştirici Kılavuzu](https://dev.mysql.com/doc/connector-python/en/)' na bakın. 

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

Azure portal MySQL için Azure veritabanı 'na bağlanmak üzere gereken bağlantı bilgilerini alın. Sunucu adı, veritabanı adı ve oturum açma kimlik bilgileri gerekir.

1. [Azure portalında](https://portal.azure.com/) oturum açın.
   
1. Portal arama çubuğunda, oluşturduğunuz MySQL sunucusu için Azure veritabanı 'nı ( **demosunucum**gibi) arayın ve seçin.
   
   ![MySQL için Azure Veritabanı sunucu adı](./media/connect-python/1_server-overview-name-login.png)
   
1. Sunucunun **genel bakış** sayfasında **sunucu adı** ve **Sunucu Yöneticisi oturum açma adı**' nı bir yere göz atın. Parolanızı unutursanız, bu sayfadan parolayı da sıfırlayabilirsiniz.
   
   ![MySQL için Azure Veritabanı sunucu adı](./media/connect-python/azure-database-for-mysql-server-overview-name-login.png)

## <a name="run-the-python-examples"></a>Python örneklerini çalıştırma

Bu makaledeki her kod örneği için:

1. Metin düzenleyicisinde yeni bir dosya oluşturun.
1. Kod örneğini dosyaya ekleyin. Kodda,,, `<mydemoserver>` `<myadmin>` `<mypassword>` ve `<mydatabase>` yer tutucuları MySQL sunucunuzun ve veritabanınızın değerleriyle değiştirin.
1. Dosyayı, *C:\pythonmysql\createtable.py* veya */Home/username/pythonmysql/CreateTable.py*gibi *. Kopyala* uzantısıyla bir proje klasörüne kaydedin.
1. Kodu çalıştırmak için, bir komut istemi veya kabuğu açın `bash` ve dizini proje klasörünüze değiştirin (örneğin,) `cd pythonmysql` . `python`Komutun ardından dosya adını yazın `python createtable.py` ve ENTER tuşuna basın. 
   
   > [!NOTE]
   > Windows 'ta *python.exe* BULUNAMAZSA, yol ortam değişkeninizin Python yolunu eklemeniz veya örneğin *python.exe*tam yolunu sağlamanız gerekebilir `C:\python27\python.exe createtable.py` .

## <a name="create-a-table-and-insert-data"></a>Tablo oluşturma ve veri ekleme

Sunucu ve veritabanına bağlanmak, tablo oluşturmak ve **Insert** SQL ifadesini kullanarak verileri yüklemek için aşağıdaki kodu kullanın. 

Kod MySQL. Connector kitaplığını içeri aktarır ve config koleksiyonundaki [bağımsız değişkenleri](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) kullanarak MySQL Için Azure veritabanı 'na bağlanmak üzere [Connect ()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) işlevini kullanır. Kod, bağlantıda bir imleç kullanır ve [cursor.exeCute ()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) yöntemi MySQL veritabanında SQL sorgusunu yürütür. 

```python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'<mydemoserver>.mysql.database.azure.com',
  'user':'<myadmin>@<mydemoserver>',
  'password':'<mypassword>',
  'database':'<mydatabase>'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Drop previous table of same name if one exists
  cursor.execute("DROP TABLE IF EXISTS inventory;")
  print("Finished dropping table (if existed).")

  # Create table
  cursor.execute("CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);")
  print("Finished creating table.")

  # Insert some data into table
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("banana", 150))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("orange", 154))
  print("Inserted",cursor.rowcount,"row(s) of data.")
  cursor.execute("INSERT INTO inventory (name, quantity) VALUES (%s, %s);", ("apple", 100))
  print("Inserted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="read-data"></a>Verileri okuma

Bağlanmak ve **SELECT** SQL deyimi kullanarak verileri okumak için aşağıdaki kodu kullanın. 

Kod MySQL. Connector kitaplığını içeri aktarır ve config koleksiyonundaki [bağımsız değişkenleri](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) kullanarak MySQL Için Azure veritabanı 'na bağlanmak üzere [Connect ()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) işlevini kullanır. Kod, bağlantıda bir imleç kullanır ve [cursor.exeCute ()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) yöntemi MySQL veritabanında SQL sorgusunu yürütür. 

Kod, [fetchAll ()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) yöntemini kullanarak veri satırlarını okur, sonuç kümesini bir koleksiyon satırında tutar ve `for` satırlarda döngü yapmak için bir yineleyici kullanır.

```python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'<mydemoserver>.mysql.database.azure.com',
  'user':'<myadmin>@<mydemoserver>',
  'password':'<mypassword>',
  'database':'<mydatabase>'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Read data
  cursor.execute("SELECT * FROM inventory;")
  rows = cursor.fetchall()
  print("Read",cursor.rowcount,"row(s) of data.")

  # Print all rows
  for row in rows:
    print("Data row = (%s, %s, %s)" %(str(row[0]), str(row[1]), str(row[2])))

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="update-data"></a>Verileri güncelleştirme

Bağlanmak ve bir **UPDATE** SQL deyimi kullanarak verileri güncelleştirmek için aşağıdaki kodu kullanın. 

Kod MySQL. Connector kitaplığını içeri aktarır ve config koleksiyonundaki [bağımsız değişkenleri](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) kullanarak MySQL Için Azure veritabanı 'na bağlanmak üzere [Connect ()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) işlevini kullanır. Kod, bağlantıda bir imleç kullanır ve [cursor.exeCute ()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) yöntemi MySQL veritabanında SQL sorgusunu yürütür. 

```python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'<mydemoserver>.mysql.database.azure.com',
  'user':'<myadmin>@<mydemoserver>',
  'password':'<mypassword>',
  'database':'<mydatabase>'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Update a data row in the table
  cursor.execute("UPDATE inventory SET quantity = %s WHERE name = %s;", (200, "banana"))
  print("Updated",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="delete-data"></a>Verileri silme

Bağlanmak ve **DELETE** SQL deyimini kullanarak verileri kaldırmak için aşağıdaki kodu kullanın. 

Kod MySQL. Connector kitaplığını içeri aktarır ve config koleksiyonundaki [bağımsız değişkenleri](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) kullanarak MySQL Için Azure veritabanı 'na bağlanmak üzere [Connect ()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) işlevini kullanır. Kod, bağlantıda bir imleç kullanır ve [cursor.exeCute ()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) yöntemi MySQL veritabanında SQL sorgusunu yürütür. 

```python
import mysql.connector
from mysql.connector import errorcode

# Obtain connection string information from the portal
config = {
  'host':'<mydemoserver>.mysql.database.azure.com',
  'user':'<myadmin>@<mydemoserver>',
  'password':'<mypassword>',
  'database':'<mydatabase>'
}

# Construct connection string
try:
   conn = mysql.connector.connect(**config)
   print("Connection established.")
except mysql.connector.Error as err:
  if err.errno == errorcode.ER_ACCESS_DENIED_ERROR:
    print("Something is wrong with the user name or password.")
  elif err.errno == errorcode.ER_BAD_DB_ERROR:
    print("Database does not exist.")
  else:
    print(err)
else:
  cursor = conn.cursor()

  # Delete a data row in the table
  cursor.execute("DELETE FROM inventory WHERE name=%(param1)s;", {'param1':"orange"})
  print("Deleted",cursor.rowcount,"row(s) of data.")

  # Cleanup
  conn.commit()
  cursor.close()
  conn.close()
  print("Done.")
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./concepts-migrate-import-export.md)

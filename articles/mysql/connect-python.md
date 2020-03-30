---
title: MySQL için Python - Azure Veritabanı'nı kullanarak bağlanın
description: Bu hızlı başlangıçta, MySQL için Azure Veritabanı'na bağlanmak ve buradan veri sorgulamak için kullanabileceğiniz birkaç Python kod örneği sağlanmıştır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom:
- mvc
- seo-python-october2019
ms.devlang: python
ms.topic: quickstart
ms.date: 01/09/2020
ms.openlocfilehash: c9ea155f3cc71dd961a3780e3b188a6d062606bc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80067910"
---
# <a name="quickstart-use-python-to-connect-and-query-data-with-azure-database-for-mysql"></a>Quickstart: MySQL için Azure Veritabanı ile veri bağlamak ve sorgulamak için Python'u kullanın

Bu hızlı başlangıçta, Python'u kullanarak MySQL için bir Azure Veritabanına bağlanırsınız. Daha sonra, veritabanındaki verileri Mac, Ubuntu Linux ve Windows platformlarından sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerini kullanırsınız. 

Bu konu Python'u kullanmaya aşina olduğunuzu varsayar, ancak MySQL için Azure Veritabanı ile çalışmaya yeni başlıyorsunuz.

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- MySQL sunucusu için bir Azure Veritabanı. [Azure portalı kullanarak MySQL sunucusu için bir Azure Veritabanı oluşturun](quickstart-create-mysql-server-database-using-azure-portal.md) veya Azure [CLI'yi kullanarak MySQL sunucusu için bir Azure Veritabanı oluşturun.](quickstart-create-mysql-server-database-using-azure-cli.md)

## <a name="install-python-and-the-mysql-connector"></a>Python’u ve MySQL bağlayıcısını yükleme

Python'u ve Python için MySQL konektörünü aşağıdaki adımları kullanarak bilgisayarınıza yükleyin: 

> [!NOTE]
> Bu hızlı başlatma, MySQL'e bağlanmak için ham bir SQL sorgu yaklaşımı kullanır. Bir web çerçevesi kullanıyorsanız, çerçeve için önerilen bağlayıcıyı (örneğin, Django için [mysqlclient)](https://pypi.org/project/mysqlclient/) kullanın.

1. İşletim sisteminiz için [Python 3.7 veya üzeri](https://www.python.org/downloads/) indirin ve yükleyin. MySQL konektörü `PATH`bunu gerektirdiğinden, python'u sizin ,' nize eklediğinizden emin olun.
   
1. Bir komut istemi `bash` veya kabuk açın ve `python -V` büyük V anahtarı ile çalıştırarak Python sürümünü kontrol edin.
   
1. Paket `pip` yükleyici Python'un en son sürümlerinde yer alan paket yükleyicidir. Çalıştırarak en son `pip install -U pip`sürüme güncelleştirin. `pip` 
   
   `pip` Yüklenmiyorsa, `get-pip.py`'yi ' yle indirip yükleyebilirsiniz. Daha fazla bilgi için [Yükleme'ye](https://pip.pypa.io/en/stable/installing/)bakın. 
   
1. Python `pip` ve bağımlılıkları için MySQL bağlayıcısını yüklemek için kullanın:
   
   ```bash
   pip install mysql-connector-python
   ```
   
   MySQL için Python bağlayıcısını [mysql.com'dan](https://dev.mysql.com/downloads/connector/python/)da yükleyebilirsiniz. Python için MySQL Bağlayıcısı hakkında daha fazla bilgi için [MySQL Bağlayıcısı/Python Geliştirici Kılavuzu'na](https://dev.mysql.com/doc/connector-python/en/)bakın. 

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

MySQL için Azure Veritabanı'na bağlanmak için gereken bağlantı bilgilerini Azure portalından alın. Sunucu adı, veritabanı adı ve giriş kimlik bilgilerine ihtiyacınız vardır.

1. [Azure portalında](https://portal.azure.com/)oturum açın.
   
1. Portal arama çubuğunda, **mydemoserver**gibi oluşturduğunuz MySQL sunucusu için Azure Veritabanı'nı arayın ve seçin.
   
   ![MySQL için Azure Veritabanı sunucu adı](./media/connect-python/1_server-overview-name-login.png)
   
1. Sunucunun Genel **Bakış** sayfasından, Sunucu **adını** ve **Server yönetici giriş adını**not edin. Parolanızı unutursanız, parolayı bu sayfadan da sıfırlayabilirsiniz.
   
   ![MySQL için Azure Veritabanı sunucu adı](./media/connect-python/azure-database-for-mysql-server-overview-name-login.png)

## <a name="run-the-python-examples"></a>Python örneklerini çalıştırma

Bu makaledeki her kod örneği için:

1. Metin düzenleyicisinde yeni bir dosya oluşturun.
1. Kod örneğini dosyaya ekleyin. Kodda, MySQL `<mydemoserver>` `<myadmin>`sunucunuz `<mydatabase>` ve veritabanınızın değerleri ile , , `<mypassword>`, ve yer tutucuları değiştirin.
1. Dosyayı *C:\pythonmysql\createtable.py* veya */home/username/pythonmysql/createtable.py*gibi *.py* uzantılı bir proje klasörüne kaydedin.
1. Kodu çalıştırmak için, bir komut `bash` istemi veya kabuk açın ve proje `cd pythonmysql`klasörünüzde dizin değiştirin, örneğin. Örneğin, `python` `python createtable.py`dosya adının ardından gelen komutu yazın ve Enter tuşuna basın. 
   
   > [!NOTE]
   > Windows'da, *python.exe* bulunamazsa, Python yolunu PATH ortamı değişkeninize eklemeniz veya *python.exe'ye* `C:\python27\python.exe createtable.py`tam yol sağlamanız gerekebilir.

## <a name="create-a-table-and-insert-data"></a>Tablo oluşturma ve veri ekleme

Sunucuya ve veritabanına bağlanmak, tablo oluşturmak ve **insert** SQL deyimi kullanarak veri yüklemek için aşağıdaki kodu kullanın. 

Kod mysql.connector kitaplığını alır ve config koleksiyonundaki [bağımsız değişkenleri](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) kullanarak MySQL için Azure Veritabanı'na bağlanmak için [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) işlevini kullanır. Kod bağlantıda bir imleç kullanır ve [imleç.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) yöntemi MYSQL veritabanına karşı SQL sorgusu yürütür. 

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

Kod mysql.connector kitaplığını alır ve config koleksiyonundaki [bağımsız değişkenleri](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) kullanarak MySQL için Azure Veritabanı'na bağlanmak için [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) işlevini kullanır. Kod bağlantıda bir imleç kullanır ve [imleç.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) yöntemi MYSQL veritabanına karşı SQL sorgusu yürütür. 

Kod, [fetchall()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-fetchall.html) yöntemini kullanarak veri satırlarını okur, sonucu bir toplama `for` satırında tutar ve satırların üzerine dönmek için bir yineleyici kullanır.

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

Kod mysql.connector kitaplığını alır ve config koleksiyonundaki [bağımsız değişkenleri](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) kullanarak MySQL için Azure Veritabanı'na bağlanmak için [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) işlevini kullanır. Kod bağlantıda bir imleç kullanır ve [imleç.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) yöntemi MYSQL veritabanına karşı SQL sorgusu yürütür. 

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

Kod mysql.connector kitaplığını alır ve config koleksiyonundaki [bağımsız değişkenleri](https://dev.mysql.com/doc/connector-python/en/connector-python-connectargs.html) kullanarak MySQL için Azure Veritabanı'na bağlanmak için [connect()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysql-connector-connect.html) işlevini kullanır. Kod bağlantıda bir imleç kullanır ve [imleç.execute()](https://dev.mysql.com/doc/connector-python/en/connector-python-api-mysqlcursor-execute.html) yöntemi MYSQL veritabanına karşı SQL sorgusu yürütür. 

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

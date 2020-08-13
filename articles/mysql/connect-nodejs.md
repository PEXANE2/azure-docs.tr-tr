---
title: 'Hızlı başlangıç: Node.js kullanarak bağlanma-MySQL için Azure veritabanı'
description: Bu hızlı başlangıçta, MySQL için Azure Veritabanı'na bağlanmak ve buradan veri sorgulamak için kullanabileceğiniz birkaç Node.js kod örneği sağlanmıştır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.custom: mvc, seo-javascript-september2019, seo-javascript-october2019, devx-track-javascript
ms.devlang: nodejs
ms.topic: quickstart
ms.date: 5/26/2020
ms.openlocfilehash: 1f8f4dee2ed1e6dc5cdf6985c1149db0dcaed7c2
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88185936"
---
# <a name="quickstart-use-nodejs-to-connect-and-query-data-in-azure-database-for-mysql"></a>Hızlı başlangıç: MySQL için Azure veritabanı 'na bağlanmak ve veri sorgulamak için Node.js kullanma

Bu hızlı başlangıçta Node.js kullanarak MySQL için Azure veritabanı 'na bağlanırsınız. Ardından, Mac, Ubuntu Linux ve Windows platformlarındaki veritabanındaki verileri sorgulamak, eklemek, güncelleştirmek ve silmek için SQL deyimlerini kullanın. 

Bu konuda, Node.js kullanarak geliştirmeyle ilgili bilgi sahibi olduğunuz ve MySQL için Azure veritabanı ile çalışmaya yeni başladığınızı varsaymaktadır.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- MySQL için Azure veritabanı sunucusu. [Azure Portal kullanarak MySQL Için Azure veritabanı sunucusu oluşturun](quickstart-create-mysql-server-database-using-azure-portal.md) veya [Azure CLI kullanarak MySQL için Azure veritabanı sunucusu oluşturun](quickstart-create-mysql-server-database-using-azure-cli.md).

> [!IMPORTANT] 
> Bağlanmakta olduğunuz IP adresinin [Azure Portal](./howto-manage-firewall-using-portal.md) veya [Azure CLI](./howto-manage-firewall-using-cli.md) kullanarak sunucunun güvenlik duvarı kurallarını eklendiğinden emin olun

## <a name="install-nodejs-and-the-mysql-connector"></a>Node.js’yi ve MySQL bağlayıcısını yükleme

Platformunuza bağlı olarak, [Node.js](https://nodejs.org)yüklemek için uygun bölümdeki yönergeleri izleyin. [MySQL](https://www.npmjs.com/package/mysql) paketini ve bağımlılıklarını proje klasörünüze yüklemek için NPM kullanın.

### <a name="windows"></a>Windows

1. [Node.js indirme sayfasını](https://nodejs.org/en/download/) ziyaret edin ve istediğiniz Windows yükleyici seçeneğini belirleyin.
2. `nodejsmysql` gibi yerel bir proje klasörü oluşturun. 
3. Komut istemi ' ni açın ve ardından dizini proje klasörüne değiştirin, örneğin`cd c:\nodejsmysql\`
4. MySQL kitaplığını proje klasörüne yüklemek için NPM aracını çalıştırın.

   ```cmd
   cd c:\nodejsmysql\
   "C:\Program Files\nodejs\npm" install mysql
   "C:\Program Files\nodejs\npm" list
   ```

5. Çıkış metnini denetleyerek yüklemeyi doğrulayın `npm list` . Yeni yamalar yayımlandıkça sürüm numarası değişebilir.

### <a name="linux-ubuntu"></a>Linux (Ubuntu)

1. **Node.js**’yi ve Node.js’nin paket yöneticisi olan **npm**’yi yüklemek için aşağıdaki komutları çalıştırın.

   ```bash
   sudo apt-get install -y nodejs npm
   ```

2. Bir proje klasörü oluşturmak `mysqlnodejs` ve MySQL paketini bu klasöre yüklemek için aşağıdaki komutları çalıştırın.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql
   npm list
   ```
3. NPM listesi çıkış metnini denetleyerek yüklemeyi doğrulayın. Yeni yamalar yayımlandıkça sürüm numarası değişebilir.

### <a name="mac-os"></a>Mac OS

1. Mac OS X ve **Node.js**’ye yönelik kullanımı kolay bir paket yöneticisi olan **brew** programını yüklemek için aşağıdaki komutları girin.

   ```bash
   ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
   brew install node
   ```
2. Bir proje klasörü oluşturmak `mysqlnodejs` ve MySQL paketini bu klasöre yüklemek için aşağıdaki komutları çalıştırın.

   ```bash
   mkdir nodejsmysql
   cd nodejsmysql
   npm install --save mysql
   npm list
   ```

3. Çıkış metnini denetleyerek yüklemeyi doğrulayın `npm list` . Yeni yamalar yayımlandıkça sürüm numarası değişebilir.

## <a name="get-connection-information"></a>Bağlantı bilgilerini alma

MySQL için Azure Veritabanı'na bağlanmak üzere gereken bağlantı bilgilerini alın. Tam sunucu adına ve oturum açma kimlik bilgilerine ihtiyacınız vardır.

1. [Azure Portal](https://portal.azure.com/)oturum açın.
2. Azure Portal sol taraftaki menüden **tüm kaynaklar**' ı seçin ve oluşturduğunuz sunucuyu (örneğin, **demosunucum**) arayın.
3. Sunucu adını seçin.
4. Sunucunun **Genel Bakış** panelinden **Sunucu adı** ile **Sunucu yöneticisi oturum açma adı**’nı not alın. Parolanızı unutursanız, bu panelden parolayı da sıfırlayabilirsiniz.
 ![MySQL için Azure Veritabanı sunucu adı](./media/connect-nodejs/server-name-azure-database-mysql.png)

## <a name="running-the-javascript-code-in-nodejs"></a>Node.js’de JavaScript kodunu çalıştırma

1. JavaScript kodunu metin dosyalarına yapıştırın ve dosyaları .js uzantısıyla bir proje klasörüne kaydedin (örneğin, C:\nodejsmysql\createtable.js veya /home/username/nodejsmysql/createtable.js).
2. Komut istemi veya bash kabuğunu açın ve ardından dizini proje klasörünüze değiştirin `cd nodejsmysql` .
3. Uygulamayı çalıştırmak için, Node komutunu ve ardından dosya adını girin (örneğin,) `node createtable.js` .
4. Windows’da düğüm uygulamanız ortam değişkeni yolunuzda değilse düğüm uygulamasını başlatmak için tam yolu kullanmanız gerekebilir; örneğin, `"C:\Program Files\nodejs\node.exe" createtable.js`

## <a name="connect-create-table-and-insert-data"></a>Bağlanma, tablo oluşturma ve veri ekleme

Bağlanıp **CREATE TABLE** ve **INSERT INTO** SQL deyimlerini kullanarak verileri yüklemek için aşağıdaki kodu kullanın.

MySQL sunucusuyla arabirim oluşturmak için [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) yöntemi, kullanılır. Sunucuyla bağlantı kurmak için [connect()](https://github.com/mysqljs/mysql#establishing-connections) işlevi kullanılır. MySQL veritabanına karşı SQL sorgusunu yürütmek için [query()](https://github.com/mysqljs/mysql#performing-queries) işlevi kullanılır. 

`host`, `user`, `password` ve `database` parametrelerini, sunucu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```javascript
const mysql = require('mysql');

var config =
{
    host: 'mydemoserver.mysql.database.azure.com',
    user: 'myadmin@mydemoserver',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
    if (err) { 
        console.log("!!! Cannot connect !!! Error:");
        throw err;
    }
    else
    {
       console.log("Connection established.");
           queryDatabase();
    }   
});

function queryDatabase(){
       conn.query('DROP TABLE IF EXISTS inventory;', function (err, results, fields) { 
            if (err) throw err; 
            console.log('Dropped inventory table if existed.');
        })
       conn.query('CREATE TABLE inventory (id serial PRIMARY KEY, name VARCHAR(50), quantity INTEGER);', 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Created inventory table.');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['banana', 150], 
            function (err, results, fields) {
                if (err) throw err;
            else console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['orange', 154], 
            function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.query('INSERT INTO inventory (name, quantity) VALUES (?, ?);', ['apple', 100], 
        function (err, results, fields) {
                if (err) throw err;
            console.log('Inserted ' + results.affectedRows + ' row(s).');
        })
       conn.end(function (err) { 
        if (err) throw err;
        else  console.log('Done.') 
        });
};
```

## <a name="read-data"></a>Verileri okuma

Bağlanmak ve **SELECT** SQL deyimi kullanarak verileri okumak için aşağıdaki kodu kullanın. 

MySQL sunucusuyla arabirim oluşturmak için [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) yöntemi, kullanılır. Sunucuyla bağlantı kurmak için [connect()](https://github.com/mysqljs/mysql#establishing-connections) yöntemi kullanılır. MySQL veritabanına karşı SQL sorgusunu yürütmek için [query()](https://github.com/mysqljs/mysql#performing-queries) yöntemi kullanılır. Sorgunun sonuçlarını tutmak için sonuç dizisi kullanılır.

`host`, `user`, `password` ve `database` parametrelerini, sunucu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```javascript
const mysql = require('mysql');

var config =
{
    host: 'mydemoserver.mysql.database.azure.com',
    user: 'myadmin@mydemoserver',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            readData();
        }   
    });

function readData(){
        conn.query('SELECT * FROM inventory', 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Selected ' + results.length + ' row(s).');
                for (i = 0; i < results.length; i++) {
                    console.log('Row: ' + JSON.stringify(results[i]));
                }
                console.log('Done.');
            })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Closing connection.') 
        });
};
```

## <a name="update-data"></a>Verileri güncelleştirme

Bağlanmak ve **UPDATE** SQL deyimi kullanarak verileri okumak için aşağıdaki kodu kullanın. 

MySQL sunucusuyla arabirim oluşturmak için [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) yöntemi, kullanılır. Sunucuyla bağlantı kurmak için [connect()](https://github.com/mysqljs/mysql#establishing-connections) yöntemi kullanılır. MySQL veritabanına karşı SQL sorgusunu yürütmek için [query()](https://github.com/mysqljs/mysql#performing-queries) yöntemi kullanılır. 

`host`, `user`, `password` ve `database` parametrelerini, sunucu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```javascript
const mysql = require('mysql');

var config =
{
    host: 'mydemoserver.mysql.database.azure.com',
    user: 'myadmin@mydemoserver',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            updateData();
        }   
    });

function updateData(){
       conn.query('UPDATE inventory SET quantity = ? WHERE name = ?', [200, 'banana'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Updated ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="delete-data"></a>Verileri silme

Bağlanmak ve **DELETE** SQL deyimi kullanarak verileri okumak için aşağıdaki kodu kullanın. 

MySQL sunucusuyla arabirim oluşturmak için [mysql.createConnection()](https://github.com/mysqljs/mysql#establishing-connections) yöntemi, kullanılır. Sunucuyla bağlantı kurmak için [connect()](https://github.com/mysqljs/mysql#establishing-connections) yöntemi kullanılır. MySQL veritabanına karşı SQL sorgusunu yürütmek için [query()](https://github.com/mysqljs/mysql#performing-queries) yöntemi kullanılır. 

`host`, `user`, `password` ve `database` parametrelerini, sunucu ve veritabanını oluştururken belirttiğiniz değerlerle değiştirin.

```javascript
const mysql = require('mysql');

var config =
{
    host: 'mydemoserver.mysql.database.azure.com',
    user: 'myadmin@mydemoserver',
    password: 'your_password',
    database: 'quickstartdb',
    port: 3306,
    ssl: true
};

const conn = new mysql.createConnection(config);

conn.connect(
    function (err) { 
        if (err) { 
            console.log("!!! Cannot connect !!! Error:");
            throw err;
        }
        else {
            console.log("Connection established.");
            deleteData();
        }   
    });

function deleteData(){
       conn.query('DELETE FROM inventory WHERE name = ?', ['orange'], 
            function (err, results, fields) {
                if (err) throw err;
                else console.log('Deleted ' + results.affectedRows + ' row(s).');
        })
       conn.end(
           function (err) { 
                if (err) throw err;
                else  console.log('Done.') 
        });
};
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dışarı Aktarma ve İçeri Aktarma seçeneğini kullanarak veritabanınızı geçirme](./concepts-migrate-import-export.md)

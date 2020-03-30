---
title: Kullanıcı oluşturma - MariaDB için Azure Veritabanı
description: Bu makalede, MariaDB sunucusu için bir Azure Veritabanı ile etkileşim kurmak için nasıl yeni kullanıcı hesapları oluşturabileceğiniz açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 23e3c9db6302d2db597283358ff71c3693ecf435
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79530181"
---
# <a name="create-users-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı’nda kullanıcı oluşturma 
Bu makalede, MariaDB için Azure Veritabanı'nda nasıl kullanıcı oluşturabileceğiniz açıklanmaktadır.

MariaDB için Azure Veritabanınızı ilk oluşturduğunuzda, bir sunucu yöneticisi giriş kullanıcı adı ve parola sağladınız. Daha fazla bilgi için [Quickstart'ı](quickstart-create-mariadb-server-database-using-azure-portal.md)takip edebilirsiniz. Azure portalından sunucu yönetici giriş kullanıcı adınızı bulabilirsiniz.

Sunucu yöneticisi kullanıcı listelenen sunucunuz için belirli ayrıcalıklar alır: SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLOLARI, EXECUTE, REPLICATION SLAVE, REPLICATION İSTEMCI, GÖRÜNÜM OLUŞTUR, GÖRÜNÜMÜ GÖSTER, RUTIN OLUŞTUR, RUTINI DEĞIŞTIRME, KULLANıCı OLUŞTURMA, OLAY, TETIKLEYICI

MariaDB sunucusu için Azure Veritabanı oluşturulduktan sonra, ek kullanıcılar oluşturmak ve yöneticierişimi vermek için ilk sunucu yöneticisi kullanıcı hesabını kullanabilirsiniz. Ayrıca, sunucu yöneticisi hesabı tek tek veritabanı şemalarına erişimi olan daha az ayrıcalıklı kullanıcılar oluşturmak için kullanılabilir.

## <a name="create-additional-admin-users"></a>Ek yönetici kullanıcıları oluşturma
1. Bağlantı bilgilerini ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adı ve oturum açma bilgilerini **sunucugenel bakış** sayfasından veya Azure portalındaki **Özellikler** sayfasından kolayca bulabilirsiniz. 

2. Veritabanı sunucunuza bağlanmak için yönetici hesabını ve parolasını kullanın. MySQL Workbench, mysql.exe, HeidiSQL veya diğerleri gibi tercih ettiğiniz istemci aracını kullanın. 
   Nasıl bağlanıp bağlanıp bağlanabileceğinizden emin değilseniz, [verileri bağlamak ve sorgulamak için MySQL Workbench'i kullanın](./connect-workbench.md)

3. Aşağıdaki SQL kodunu edin ve çalıştırın. Yer tutucu değeri `new_master_user`için yeni kullanıcı adınızı değiştirin. Bu sözdizimi, tüm veritabanı şemalarında (*.*) kullanıcı adına (bu örnekte new_master_user) listelenen ayrıcalıkları verir. 

   ```sql
   CREATE USER 'new_master_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'new_master_user'@'%' WITH GRANT OPTION; 
   
   FLUSH PRIVILEGES;
   ```

4. Hibeleri doğrulayın 
   ```sql
   USE sys;
   
   SHOW GRANTS FOR 'new_master_user'@'%';
   ```

## <a name="create-database-users"></a>Veritabanı kullanıcıları oluşturma

1. Bağlantı bilgilerini ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adı ve oturum açma bilgilerini **sunucugenel bakış** sayfasından veya Azure portalındaki **Özellikler** sayfasından kolayca bulabilirsiniz. 

2. Veritabanı sunucunuza bağlanmak için yönetici hesabını ve parolasını kullanın. MySQL Workbench, mysql.exe, HeidiSQL veya diğerleri gibi tercih ettiğiniz istemci aracını kullanın. 
   Nasıl bağlanıp bağlanıp bağlanabileceğinizden emin değilseniz, [verileri bağlamak ve sorgulamak için MySQL Workbench'i kullanın](./connect-workbench.md)

3. Aşağıdaki SQL kodunu edin ve çalıştırın. Yer tutucu değerini, `db_user` hedeflenenene yeni kullanıcı `testdb` adınızla ve yer tutucu değerini kendi veritabanı adınızile değiştirin.

   Bu sql kodu sözdizimi, örneğin amaçlariçin testdb adında yeni bir veritabanı oluşturur. Daha sonra MariaDB hizmeti için Azure Veritabanı'nda yeni bir kullanıcı oluşturur ve bu kullanıcı için\*yeni veritabanı şemasına (testdb. ) tüm ayrıcalıklar verir. 

   ```sql
   CREATE DATABASE testdb;
   
   CREATE USER 'db_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT ALL PRIVILEGES ON testdb . * TO 'db_user'@'%';
   
   FLUSH PRIVILEGES;
   ```

4. Veritabanındaki hibeleri doğrulayın.
   ```sql
   USE testdb;
   
   SHOW GRANTS FOR 'db_user'@'%';
   ```

5. Yeni kullanıcı adı ve parolayı kullanarak belirlenen veritabanını belirterek sunucuya giriş yapın. Bu örnek, mysql komut satırını gösterir. Bu komutla, kullanıcı adının parolası istenir. Kendi sunucu adınızı, veritabanı adınızı ve kullanıcı adınızı değiştirin.

   ```bash
   mysql --host mydemoserver.mariadb.database.azure.com --database testdb --user db_user@mydemoserver -p
   ```
   Kullanıcı hesabı yönetimi ile ilgili daha fazla bilgi için, [Kullanıcı hesabı yönetimi,](https://mariadb.com/kb/en/library/user-account-management/) [GRANT Sözdizimi](https://mariadb.com/kb/en/library/grant/)ve Ayrıcalıklar için MariaDB [belgelerine](https://mariadb.com/kb/en/library/grant/#privilege-levels)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Yeni kullanıcıların makinelerinin IP adreslerinin bağlantı kurmasını sağlamak için güvenlik duvarını açın: [Azure portalını kullanarak MariaDB güvenlik duvarı kuralları için Azure Veritabanı oluşturma ve yönetme](howto-manage-firewall-portal.md)  

<!--or [Azure CLI](howto-manage-firewall-using-cli.md).-->

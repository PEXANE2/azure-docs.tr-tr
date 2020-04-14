---
title: Kullanıcı oluşturma - MySQL için Azure Veritabanı
description: Bu makalede, MySQL sunucusu için bir Azure Veritabanı ile etkileşim kurmak için nasıl yeni kullanıcı hesapları oluşturabileceğiniz açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 4/2/2020
ms.openlocfilehash: f8c020da72e9cdf8777a3eefac266e97e4a312bd
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81263480"
---
# <a name="create-users-in-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure Veritabanı'nda kullanıcı oluşturma

Bu makalede, MySQL sunucusu için bir Azure Veritabanı'nda nasıl kullanıcı oluşturabileceğiniz açıklanmaktadır.

MySQL için Azure Veritabanınızı ilk oluşturduğunuzda, bir sunucu yöneticisi giriş kullanıcı adı ve parola sağladınız. Daha fazla bilgi için [Quickstart'ı](quickstart-create-mysql-server-database-using-azure-portal.md)takip edebilirsiniz. Azure portalından sunucu yönetici giriş kullanıcı adınızı bulabilirsiniz.

Sunucu yöneticisi kullanıcı listelenen sunucunuz için belirli ayrıcalıklar alır: SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER

MySQL sunucusu için Azure Veritabanı oluşturulduktan sonra, ek kullanıcılar oluşturmak ve yöneticierişimi vermek için ilk sunucu yöneticisi kullanıcı hesabını kullanabilirsiniz. Ayrıca, sunucu yöneticisi hesabı tek tek veritabanı şemalarına erişimi olan daha az ayrıcalıklı kullanıcılar oluşturmak için kullanılabilir.

> [!NOTE]
> SUPER ayrıcalığı ve DBA rolü desteklenmez. Hizmette desteklenmeyenleri anlamak için sınırlamalar makalesindeki [ayrıcalıkları](concepts-limits.md#privilege-support) gözden geçirin.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanı'nda ek yönetici kullanıcıları nasıl oluşturulur?

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

## <a name="how-to-create-database-users-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanı'nda veritabanı kullanıcıları nasıl oluşturulur?

1. Bağlantı bilgilerini ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adı ve oturum açma bilgilerini **sunucugenel bakış** sayfasından veya Azure portalındaki **Özellikler** sayfasından kolayca bulabilirsiniz.

2. Veritabanı sunucunuza bağlanmak için yönetici hesabını ve parolasını kullanın. MySQL Workbench, mysql.exe, HeidiSQL veya diğerleri gibi tercih ettiğiniz istemci aracını kullanın.
   Nasıl bağlanıp bağlanıp bağlanabileceğinizden emin değilseniz, [verileri bağlamak ve sorgulamak için MySQL Workbench'i kullanın](./connect-workbench.md)

3. Aşağıdaki SQL kodunu edin ve çalıştırın. Yer tutucu değerini, `db_user` hedeflenenene yeni kullanıcı `testdb` adınızla ve yer tutucu değerini kendi veritabanı adınızile değiştirin.

   Bu sql kodu sözdizimi, örneğin amaçlariçin testdb adında yeni bir veritabanı oluşturur. Daha sonra MySQL hizmetinde yeni bir kullanıcı oluşturur ve bu kullanıcı için yeni veritabanı\*şemasına (testdb. ) tüm ayrıcalıkları verir.

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

   ```azurecli-interactive
   mysql --host mydemoserver.mysql.database.azure.com --database testdb --user db_user@mydemoserver -p
   ```

## <a name="next-steps"></a>Sonraki adımlar

Yeni kullanıcıların makinelerinin IP adreslerinin bağlantı kurmasını sağlamak için güvenlik duvarını açın: Azure portalını veya [Azure CLI'yi](howto-manage-firewall-using-cli.md) [kullanarak MySQL güvenlik duvarı kuralları için Azure Veritabanı oluşturun ve yönetin.](howto-manage-firewall-using-portal.md)

Kullanıcı hesabı yönetimi ile ilgili daha fazla bilgi için, [Kullanıcı hesabı yönetimi,](https://dev.mysql.com/doc/refman/5.7/en/access-control.html) [GRANT Sözdizimi](https://dev.mysql.com/doc/refman/5.7/en/grant.html)ve Ayrıcalıklar için MySQL ürün [belgelerine](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html)bakın.

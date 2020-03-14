---
title: Kullanıcı oluşturma-MySQL için Azure veritabanı
description: Bu makalede, MySQL için Azure veritabanı sunucusu ile etkileşim kurmak üzere nasıl yeni kullanıcı hesapları oluşturabileceğiniz açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: e76e63030cc8e10c857d361cca69e1d35ba8c2c1
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79269412"
---
# <a name="create-users-in-azure-database-for-mysql-server"></a>MySQL için Azure veritabanı sunucusu 'nda Kullanıcı oluşturma 
Bu makalede, MySQL için Azure veritabanı sunucusunda nasıl Kullanıcı oluşturabileceğiniz açıklanır.

MySQL için Azure veritabanınızı ilk oluşturduğunuzda, bir Sunucu Yöneticisi oturum açma Kullanıcı adı ve parolası sağladınız. Daha fazla bilgi için [hızlı](quickstart-create-mysql-server-database-using-azure-portal.md)başlangıcı izleyebilirsiniz. Sunucu Yöneticisi oturum açma kullanıcı adınızı Azure portal bulabilirsiniz.

Sunucu Yöneticisi kullanıcısı, sunucunuzda listelenen belirli ayrıcalıkları alır: seçme, ekleme, GÜNCELLEŞTIRME, SILME, oluşturma, BıRAKMA, yeniden yükleme, Işleme, BAŞVURULARı, DIZIN, DEĞIŞTIRME, VERITABANLARıNı gösterme, GEÇICI tablolar oluşturma, tablo KILITLEME, yürütme, çoğaltma BAĞıMLı, çoğaltma ISTEMCI, GÖRÜNÜM OLUŞTUR, GÖRÜNÜM GÖSTER, YORDAM OLUŞTURMA, DEĞIŞTIRME YORDAMı, KULLANıCı OLUŞTURMA, OLAY, TETIKLEYICI

MySQL sunucusu için Azure veritabanı oluşturulduktan sonra, ek kullanıcılar oluşturmak ve bunlara yönetici erişimi vermek için ilk sunucu yöneticisi Kullanıcı hesabını kullanabilirsiniz. Ayrıca, sunucu yöneticisi hesabı ayrı veritabanı şemalarına erişimi olan daha az ayrıcalıklı kullanıcı oluşturmak için kullanılabilir.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda ek yönetici kullanıcılar oluşturma
1. Bağlantı bilgilerini ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve oturum açma bilgilerini sunucuya **genel bakış** sayfasından veya Azure Portal **Özellikler** sayfasından kolayca bulabilirsiniz. 

2. Veritabanı sunucunuza bağlanmak için yönetici hesabı ve parolasını kullanın. MySQL çalışma ekranı, MySQL. exe, HeidiSQL veya diğerleri gibi tercih ettiğiniz istemci aracını kullanın. 
   Nasıl bağlanacağınızdan emin değilseniz, bkz. [bağlanmak ve veri sorgulamak Için MySQL çalışma ekranı kullanma](./connect-workbench.md)

3. Aşağıdaki SQL kodunu düzenleyin ve çalıştırın. Yeni Kullanıcı adınızı yer tutucu değeri `new_master_user`değiştirin. Bu sözdizimi, tüm veritabanı şemalarında ( *.* ) listelenen ayrıcalıkları Kullanıcı adına (bu örnekte new_master_user) verir. 

   ```sql
   CREATE USER 'new_master_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT SELECT, INSERT, UPDATE, DELETE, CREATE, DROP, RELOAD, PROCESS, REFERENCES, INDEX, ALTER, SHOW DATABASES, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, REPLICATION SLAVE, REPLICATION CLIENT, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, CREATE USER, EVENT, TRIGGER ON *.* TO 'new_master_user'@'%' WITH GRANT OPTION; 
   
   FLUSH PRIVILEGES;
   ```

4. İzni doğrula 
   ```sql
   USE sys;
   
   SHOW GRANTS FOR 'new_master_user'@'%';
   ```

## <a name="how-to-create-database-users-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda veritabanı kullanıcıları oluşturma

1. Bağlantı bilgilerini ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve oturum açma bilgilerini sunucuya **genel bakış** sayfasından veya Azure Portal **Özellikler** sayfasından kolayca bulabilirsiniz. 

2. Veritabanı sunucunuza bağlanmak için yönetici hesabı ve parolasını kullanın. MySQL çalışma ekranı, MySQL. exe, HeidiSQL veya diğerleri gibi tercih ettiğiniz istemci aracını kullanın. 
   Nasıl bağlanacağınızdan emin değilseniz, bkz. [bağlanmak ve veri sorgulamak Için MySQL çalışma ekranı kullanma](./connect-workbench.md)

3. Aşağıdaki SQL kodunu düzenleyin ve çalıştırın. Yer tutucu değeri `db_user`, hedeflenen yeni kullanıcı adınızla değiştirin ve yer tutucu değeri kendi veritabanı adınızla `testdb`.

   Bu SQL kod sözdizimi, örnek olarak TestDB adlı yeni bir veritabanı oluşturur. Ardından MySQL hizmetinde yeni bir kullanıcı oluşturur ve bu kullanıcı için yeni veritabanı şemasına (TestDB.\*) tüm ayrıcalıklar verir. 

   ```sql
   CREATE DATABASE testdb;
   
   CREATE USER 'db_user'@'%' IDENTIFIED BY 'StrongPassword!';
   
   GRANT ALL PRIVILEGES ON testdb . * TO 'db_user'@'%';
   
   FLUSH PRIVILEGES;
   ```

4. Veritabanının içindeki izni doğrulayın.
   ```sql
   USE testdb;
   
   SHOW GRANTS FOR 'db_user'@'%';
   ```

5. Yeni Kullanıcı adı ve parolayı kullanarak, belirlenen veritabanını belirterek sunucuda oturum açın. Bu örnekte MySQL komut satırı gösterilmektedir. Bu komutla Kullanıcı adı için parola istenir. Kendi sunucu adı, veritabanı adı ve Kullanıcı adınızı değiştirin.

   ```azurecli-interactive
   mysql --host mydemoserver.mysql.database.azure.com --database testdb --user db_user@mydemoserver -p
   ```

## <a name="next-steps"></a>Sonraki adımlar
Bağlantı kurmasını sağlamak için yeni kullanıcıların makinelerinin IP adresleri için güvenlik duvarını açın: Azure portal veya [Azure CLI](howto-manage-firewall-using-cli.md) [kullanarak MySQL için Azure Güvenlik duvarı kuralları oluşturun ve yönetin](howto-manage-firewall-using-portal.md) .

Kullanıcı hesabı yönetimi hakkında daha fazla bilgi için bkz. [Kullanıcı hesabı yönetimi](https://dev.mysql.com/doc/refman/5.7/en/user-account-management.html)için MySQL ürün belgeleri, [sözdizimi verme](https://dev.mysql.com/doc/refman/5.7/en/grant.html)ve [ayrıcalıklar](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html).

---
title: Kullanıcı oluşturma-MySQL için Azure veritabanı
description: Bu makalede, MySQL için Azure veritabanı sunucusu ile etkileşim kurmak üzere nasıl yeni kullanıcı hesapları oluşturabileceğiniz açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 4/2/2020
ms.openlocfilehash: 606279c0db92ad3aeb76d4f7a1a914f14348dd8f
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84608477"
---
# <a name="create-users-in-azure-database-for-mysql-server"></a>MySQL için Azure veritabanı sunucusu 'nda Kullanıcı oluşturma

Bu makalede, MySQL için Azure veritabanı sunucusunda nasıl Kullanıcı oluşturabileceğiniz açıklanır.

> [!NOTE]
> Sapma ücretsiz iletişim
>
> Microsoft, farklı ve üçlü ortamları destekler. Bu makale, _İkincil_sözcüğe başvurular içerir. Kullanım açısından [ücretsiz iletişim Için Microsoft Stil Kılavuzu](https://github.com/MicrosoftDocs/microsoft-style-guide/blob/master/styleguide/bias-free-communication.md) bunu bir exclusionword olarak tanır. Bu makalede, şu anda yazılımda görüntülenen sözcük olduğundan, bu makalede tutarlılık için kullanılır. Yazılım, sözcüğü kaldıracak şekilde güncelleniyorsa, bu makale hizalamayla olacak şekilde güncelleştirilir.
>

MySQL için Azure veritabanınızı ilk oluşturduğunuzda, bir Sunucu Yöneticisi oturum açma Kullanıcı adı ve parolası sağladınız. Daha fazla bilgi için [hızlı](quickstart-create-mysql-server-database-using-azure-portal.md)başlangıcı izleyebilirsiniz. Sunucu Yöneticisi oturum açma kullanıcı adınızı Azure portal bulabilirsiniz.

Sunucu Yöneticisi kullanıcısı, sunucunuzda listelenen belirli ayrıcalıkları alır: seçme, ekleme, GÜNCELLEŞTIRME, SILME, oluşturma, BıRAKMA, yeniden yükleme, Işleme, BAŞVURULARı, DIZIN, DEĞIŞTIRME, VERITABANLARıNı gösterme, GEÇICI tablolar oluşturma, TABLOLARı KILITLEME, yürütme, çoğaltma BAĞıMLı, çoğaltma ISTEMCISI, görünüm oluşturma, görünümü gösterme, rutın oluşturma, yordamı DEĞIŞTIRME, Kullanıcı, olay, TETIKLEYICI oluşturma

MySQL sunucusu için Azure veritabanı oluşturulduktan sonra, ek kullanıcılar oluşturmak ve bunlara yönetici erişimi vermek için ilk sunucu yöneticisi Kullanıcı hesabını kullanabilirsiniz. Ayrıca, sunucu yöneticisi hesabı ayrı veritabanı şemalarına erişimi olan daha az ayrıcalıklı kullanıcı oluşturmak için kullanılabilir.

> [!NOTE]
> Süper ayrıcalık ve DBA rolü desteklenmez. Hizmette Nelerin desteklenmediğini anlamak için sınırlamalar makalesindeki [ayrıcalıkları](concepts-limits.md#privilege-support) gözden geçirin.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda ek yönetici kullanıcılar oluşturma

1. Bağlantı bilgilerini ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adını ve oturum açma bilgilerini sunucuya **genel bakış** sayfasından veya Azure Portal **Özellikler** sayfasından kolayca bulabilirsiniz.

2. Veritabanı sunucunuza bağlanmak için yönetici hesabı ve parolasını kullanın. MySQL çalışma ekranı, MySQL. exe, HeidiSQL veya diğerleri gibi tercih ettiğiniz istemci aracını kullanın.
   Nasıl bağlanacağınızdan emin değilseniz, bkz. [bağlanmak ve veri sorgulamak Için MySQL çalışma ekranı kullanma](./connect-workbench.md)

3. Aşağıdaki SQL kodunu düzenleyin ve çalıştırın. Yeni Kullanıcı adınızı yer tutucu değeri için değiştirin `new_master_user` . Bu sözdizimi, tüm veritabanı şemalarında (*.*) listelenen ayrıcalıkları Kullanıcı adına (bu örnekte new_master_user) verir.

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

3. Aşağıdaki SQL kodunu düzenleyin ve çalıştırın. Yer tutucu değerini `db_user` amaçlanan Yeni Kullanıcı adınızla ve yer tutucu değerini `testdb` kendi veritabanı adınızla değiştirin.

   Bu SQL kod sözdizimi, örnek olarak TestDB adlı yeni bir veritabanı oluşturur. Ardından MySQL hizmetinde yeni bir kullanıcı oluşturur ve bu kullanıcı için yeni veritabanı şemasına (TestDB.) tüm ayrıcalıklar verir \* .

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

Kullanıcı hesabı yönetimi hakkında daha fazla bilgi için bkz. [Kullanıcı hesabı yönetimi](https://dev.mysql.com/doc/refman/5.7/en/access-control.html)için MySQL ürün belgeleri, [sözdizimi verme](https://dev.mysql.com/doc/refman/5.7/en/grant.html)ve [ayrıcalıklar](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html).

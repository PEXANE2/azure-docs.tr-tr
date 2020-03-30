---
title: Kullanıcı oluşturma - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için bir Azure Veritabanı ile etkileşim kurmak için nasıl yeni kullanıcı hesapları oluşturabileceğiniz açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/22/2019
ms.openlocfilehash: 127d484d6cfc35368803069f9c3d602e787baa56
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80384356"
---
# <a name="create-users-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanında kullanıcı oluşturma - Single Server

Bu makalede, PostgreSQL sunucusu için bir Azure Veritabanı'nda nasıl kullanıcı oluşturabileceğiniz açıklanmaktadır.

Azure abonelik kullanıcılarının ve ayrıcalıklarının nasıl oluşturulacağı ve yönetileceğini öğrenmek istiyorsanız, [Azure rol tabanlı erişim denetimi (RBAC) makalesini](../role-based-access-control/built-in-roles.md) ziyaret edebilir veya rolleri nasıl [özelleştireceğinizi](../role-based-access-control/custom-roles.md)gözden geçirebilirsiniz.

## <a name="the-server-admin-account"></a>Sunucu yöneticisi hesabı

PostgreSQL için Azure Veritabanınızı ilk oluşturduğunuzda, bir sunucu yöneticisi kullanıcı adı ve parola sağladınız. Daha fazla bilgi için, adım adım yaklaşımı görmek için [Quickstart'ı](quickstart-create-server-database-portal.md) takip edebilirsiniz. Sunucu yöneticisi kullanıcı adı özel bir ad olduğundan, seçilen sunucu yöneticisi kullanıcı adını Azure portalından bulabilirsiniz.

PostgreSQL sunucusu için Azure Veritabanı, tanımlanan 3 varsayılan rolle oluşturulur. Komutu çalıştırarak bu rolleri görebilirsiniz:`SELECT rolname FROM pg_roles;`

- azure_pg_admin
- azure_superuser
- sunucu yönetici kullanıcınız

Sunucu yönetici kullanıcınız azure_pg_admin rolün bir üyesidir. Ancak, sunucu yöneticisi hesabı azure_superuser rolünün bir parçası değildir. Bu hizmet yönetilen bir PaaS hizmeti olduğundan, süper kullanıcı rolünün yalnızca Microsoft'u bir parçasıdır.

PostgreSQL altyapısı, [PostgreSQL ürün belgelerinde](https://www.postgresql.org/docs/current/static/sql-createrole.html)açıkldıldığı gibi veritabanı nesnelerine erişimi denetlemek için ayrıcalıklar kullanır. PostgreSQL için Azure Veritabanı'nda sunucu yöneticisi kullanıcısına şu ayrıcalıklar verilir: Gİrİş, NOSUPERUSER, INHERIT, CREATEDB, CREATEROLE, NOREPLICATION

Sunucu yöneticisi kullanıcı hesabı ek kullanıcılar oluşturmak ve bu kullanıcılara azure_pg_admin rolü vermek için kullanılabilir. Ayrıca, sunucu yöneticisi hesabı daha az ayrıcalıklı kullanıcılar ve tek tek veritabanları ve şemaerişimi olan roller oluşturmak için kullanılabilir.

## <a name="how-to-create-additional-admin-users-in-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı'nda ek yönetici kullanıcıları nasıl oluşturulur?

1. Bağlantı bilgilerini ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adı ve oturum açma bilgilerini **sunucugenel bakış** sayfasından veya Azure portalındaki **Özellikler** sayfasından kolayca bulabilirsiniz.

2. Veritabanı sunucunuza bağlanmak için yönetici hesabını ve parolasını kullanın. pgAdmin veya psql gibi tercih ettiğiniz istemci aracını kullanın.
   Nasıl bağlanabileceğinizden emin değilseniz, [hızlı başlangıç](./quickstart-create-server-database-portal.md)

3. Aşağıdaki SQL kodunu edin ve çalıştırın. Yer tutucu değeri <new_user> için yeni kullanıcı adınızı değiştirin ve yer tutucu parolasını kendi güçlü parolanızla değiştirin. 

   ```sql
   CREATE ROLE <new_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB CREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';

   GRANT azure_pg_admin TO <new_user>;
   ```

## <a name="how-to-create-database-users-in-azure-database-for-postgresql"></a>PostgreSQL için Azure Veritabanı'nda veritabanı kullanıcıları nasıl oluşturulur?

1. Bağlantı bilgilerini ve yönetici kullanıcı adını alın.
   Veritabanı sunucusuna bağlanmak için tam sunucu adı ve yönetici oturum açma kimlik bilgileri gerekir. Sunucu adı ve oturum açma bilgilerini **sunucugenel bakış** sayfasından veya Azure portalındaki **Özellikler** sayfasından kolayca bulabilirsiniz.

2. Veritabanı sunucunuza bağlanmak için yönetici hesabını ve parolasını kullanın. pgAdmin veya psql gibi tercih ettiğiniz istemci aracını kullanın.

3. Aşağıdaki SQL kodunu edin ve çalıştırın. Yer tutucu değerini, `<db_user>` hedeflenenene yeni kullanıcı `<newdb>` adınızla ve yer tutucu değerini kendi veritabanı adınızile değiştirin. Yer tutucu parolasını kendi güçlü parolanızla değiştirin.

   Bu sql kodu sözdizimi, örneğin amaçlar için testdb adında yeni bir veritabanı oluşturur. Daha sonra PostgreSQL hizmetinde yeni bir kullanıcı oluşturur ve bu kullanıcı için yeni veritabanına ayrıcalıklar bağlanır.

   ```sql
   CREATE DATABASE <newdb>;
   
   CREATE ROLE <db_user> WITH LOGIN NOSUPERUSER INHERIT CREATEDB NOCREATEROLE NOREPLICATION PASSWORD '<StrongPassword!>';
   
   GRANT CONNECT ON DATABASE <newdb> TO <db_user>;
   ```

4. Yönetici hesabı kullanarak, veritabanındaki nesnelerin güvenliğini sağlamak için ek ayrıcalıklar sağlamanız gerekebilir. Veritabanı rolleri ve ayrıcalıkları hakkında daha fazla bilgi için [PostgreSQL belgelerine](https://www.postgresql.org/docs/current/static/ddl-priv.html) bakın. Örnek:

   ```sql
   GRANT ALL PRIVILEGES ON DATABASE <newdb> TO <db_user>;
   ```

5. Yeni kullanıcı adı ve parolayı kullanarak, belirlenen veritabanını belirterek sunucunuzda oturum açın. Bu örnekte psql komut satırı gösterilmektedir. Bu komutla, kullanıcı adının parolası istenir. Kendi sunucu adınızı, veritabanı adınızı ve kullanıcı adınızı değiştirin.

   ```shell
   psql --host=mydemoserver.postgres.database.azure.com --port=5432 --username=db_user@mydemoserver --dbname=newdb
   ```

## <a name="next-steps"></a>Sonraki adımlar

Yeni kullanıcıların makinelerinin IP adreslerinin bağlantı kurmasını sağlamak için güvenlik duvarını açın: Azure portalını veya [Azure CLI'yi](howto-manage-firewall-using-cli.md) [kullanarak PostgreSQL güvenlik duvarı kuralları için Azure Veritabanı oluşturun ve yönetin.](howto-manage-firewall-using-portal.md)

Kullanıcı hesabı yönetimi yle ilgili daha fazla bilgi için Veritabanı [Rolleri ve Ayrıcalıkları,](https://www.postgresql.org/docs/current/static/user-manag.html) [GRANT Sözdizimi](https://www.postgresql.org/docs/current/static/sql-grant.html)ve [Ayrıcalıklar](https://www.postgresql.org/docs/current/static/ddl-priv.html)için PostgreSQL ürün belgelerine bakın.

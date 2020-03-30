---
title: Kullanıcı oluşturma - Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: Bu makalede, PostgreSQL - Hyperscale (Citus) için bir Azure Veritabanı ile etkileşim kurmak için nasıl yeni kullanıcı hesapları oluşturabileceğiniz açıklanmaktadır.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 1/8/2019
ms.openlocfilehash: 674fd4372bdf7c3782d18aaf04b48eb0067a9b2e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77484936"
---
# <a name="create-users-in-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL - Hyperscale (Citus) için Azure Veritabanı'nda kullanıcı oluşturma

> [!NOTE]
> "Kullanıcılar" terimi, Bir Hyperscale (Citus) sunucu grubundaki kullanıcıları ifade eder. Bunun yerine Azure abonelik kullanıcıları ve ayrıcalıkları hakkında bilgi edinmek için [Azure rol tabanlı erişim denetimi (RBAC) makalesini](../role-based-access-control/built-in-roles.md) ziyaret edin veya rolleri nasıl [özelleştirinir'](../role-based-access-control/custom-roles.md)i gözden geçirin.

## <a name="the-server-admin-account"></a>Sunucu yöneticisi hesabı

PostgreSQL altyapısı veritabanı nesnelerine erişimi denetlemek için [rolleri](https://www.postgresql.org/docs/current/sql-createrole.html) kullanır ve yeni oluşturulan Hyperscale (Citus) sunucu grubu önceden tanımlanmış çeşitli rollerle birlikte gelir:

* [Varsayılan PostgreSQL rolleri](https://www.postgresql.org/docs/current/default-roles.html)
* `azure_pg_admin`
* `postgres`
* `citus`

Hyperscale yönetilen bir PaaS hizmeti olduğundan, `postgres` süper kullanıcı rolüyle yalnızca Microsoft oturum açabilir. Sınırlı yönetim erişimi için, `citus` Hyperscale rolü sağlar.

`citus` Rol için izinler:

* Normalde yalnızca süper kullanıcılar tarafından görülebilen tüm yapılandırma değişkenlerini, hatta değişkenleri bile okuyun.
* Tüm pg\_\_ \* stat görünümlerini okuyun ve istatistiklerle ilgili çeşitli uzantılar kullanın - hatta görünümler veya uzantılar normalde yalnızca süper kullanıcılar tarafından görülebilir.
* Access SHARE kilitlerini tablolara alabilecek izleme işlevlerini uzun süre çalıştırın.
* [PostgreSQL uzantıları oluşturun](concepts-hyperscale-extensions.md) (çünkü rol `azure_pg_admin`üyesidir).

Özellikle, rolün `citus` bazı kısıtlamaları vardır:

* Roller oluşturamaz
* Veritabanları oluşturulamıyor

## <a name="how-to-create-additional-user-roles"></a>Ek kullanıcı rolleri oluşturma

Belirtildiği gibi, `citus` yönetici hesabı ek kullanıcılar oluşturmak için izin yoksun. Kullanıcı eklemek için Azure portal arabirimini kullanın.

1. Hiper ölçekli sunucu grubunuz için **Roller** sayfasına gidin ve **+ Ekle'yi**tıklatın :

   ![Roller sayfası](media/howto-hyperscale-create-users/1-role-page.png)

2. Rol adını ve parolayı girin. **Kaydet**'e tıklayın.

   ![Rol ekleme](media/howto-hyperscale-create-users/2-add-user-fields.png)

Kullanıcı sunucu grubunun koordinatör düğümünde oluşturulur ve tüm alt düğümlere yayılır. Azure portalı üzerinden oluşturulan `LOGIN` roller özniteliğe sahiptir, bu da veritabanında oturum açabilen gerçek kullanıcılar oldukları anlamına gelir.

## <a name="how-to-modify-privileges-for-user-role"></a>Kullanıcı rolü için ayrıcalıkları değiştirme

Yeni kullanıcı rolleri genellikle sınırlı ayrıcalıklarla veritabanı erişimi sağlamak için kullanılır. Kullanıcı ayrıcalıklarını değiştirmek için PgAdmin veya psql gibi bir aracı kullanarak standart PostgreSQL komutlarını kullanın. (Bkz. Hyperscale (Citus) quickstart'ında [psql ile bağlantı](quickstart-create-hyperscale-portal.md#connect-to-the-database-using-psql) kurun.)

Örneğin, okumak `mytable` `db_user` için izin vermek için, izin verin:

```sql
GRANT SELECT ON mytable TO db_user;
```

Hyperscale (Citus), tüm grup boyunca tek tablolu GRANT deyimlerini yayıklar ve bunları tüm işçi düğümlerine uygular. Ancak sistem çapında (örneğin, şemadaki tüm tablolar için) GRANT'lerin her tarih düğümünde çalıştırılması gerekir.  `run_command_on_workers()` Yardımcı işlevini kullanın:

```sql
-- applies to the coordinator node
GRANT SELECT ON ALL TABLES IN SCHEMA public TO db_user;

-- make it apply to workers as well
SELECT run_command_on_workers(
  'GRANT SELECT ON ALL TABLES IN SCHEMA public TO db_user;'
);
```

## <a name="how-to-delete-a-user-role-or-change-their-password"></a>Bir kullanıcı rolü silme veya parolasını değiştirme

Bir kullanıcıyı güncellemek için, Hiper ölçekli sunucu grubunuzun **Görevler** sayfasını ziyaret edin ve kullanıcının **yanındaki** elipsleri tıklatın. Elipsler, kullanıcıyı silmek veya parolalarını sıfırlamak için bir menü açar.

   ![Rolü edin](media/howto-hyperscale-create-users/edit-role.png)

Rol `citus` ayrıcalıklıdır ve silinemez.

## <a name="next-steps"></a>Sonraki adımlar

Yeni kullanıcıların makinelerinin IP adreslerinin bağlantı kurmasını sağlamak için güvenlik duvarını açın: [Azure portalını kullanarak Hyperscale (Citus) güvenlik duvarı kuralları oluşturun ve yönetin.](howto-hyperscale-manage-firewall-using-portal.md)

Veritabanı kullanıcı hesabı yönetimi hakkında daha fazla bilgi için PostgreSQL ürün belgelerine bakın:

* [Veritabanı Rolleri ve Ayrıcalıkları](https://www.postgresql.org/docs/current/static/user-manag.html)
* [GRANT Sözdizimi](https://www.postgresql.org/docs/current/static/sql-grant.html)
* [Ayrıcalıklar](https://www.postgresql.org/docs/current/static/ddl-priv.html)

---
title: Kullanıcı oluşturma-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: Bu makalede, PostgreSQL için Azure veritabanı-hiper ölçek (Citus) ile etkileşim kurmak üzere nasıl yeni kullanıcı hesapları oluşturabileceğiniz açıklanmaktadır.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 1/8/2019
ms.openlocfilehash: 8a1b38b9f673669adb0b5fcf67d9d560c24d5c2a
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87825965"
---
# <a name="create-users-in-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda Kullanıcı oluşturma-hiper ölçek (Citus)

> [!NOTE]
> "Kullanıcılar" terimi, bir Hyperscale (Citus) sunucu grubu içindeki kullanıcılara başvurur. Azure abonelik kullanıcıları ve ayrıcalıkları hakkında bilgi edinmek için [Azure rol tabanlı erişim denetimi (Azure RBAC) makalesini](../role-based-access-control/built-in-roles.md) ziyaret edin veya [rolleri nasıl özelleştireceğinizi](../role-based-access-control/custom-roles.md)inceleyin.

## <a name="the-server-admin-account"></a>Sunucu yöneticisi hesabı

PostgreSQL altyapısı, veritabanı nesnelerine erişimi denetlemek için [Roller](https://www.postgresql.org/docs/current/sql-createrole.html) kullanır ve yeni oluşturulan bir hiper ölçek (Citus) sunucu grubu önceden tanımlanmış çeşitli rollerle birlikte gelir:

* [Varsayılan PostgreSQL rolleri](https://www.postgresql.org/docs/current/default-roles.html)
* `azure_pg_admin`
* `postgres`
* `citus`

Hiper ölçek yönetilen bir PaaS hizmeti olduğundan, yalnızca Microsoft `postgres` süper kullanıcı rolüyle oturum açabilir. Sınırlı yönetim erişimi için hiper ölçek `citus` rolü sağlar.

`citus`Rol izinleri:

* Tüm yapılandırma değişkenlerini, hatta yalnızca süper kullanıcılar için görünür olan değişkenleri okuyun.
* Tüm pg \_ stat \_ \* görünümlerini okuyun ve normalde yalnızca süper kullanıcılar için görünür olan, hatta diğer istatistiklerle ilgili uzantıları kullanın.
* Büyük olasılıkla uzun bir süre boyunca tablolardaki kilitleri paylaşma ERIŞIMI olabilecek izleme işlevlerini yürütün.
* [PostgreSQL uzantıları oluşturun](concepts-hyperscale-extensions.md) (rol öğesinin üyesi olduğu için `azure_pg_admin` ).

Özellikle, `citus` rolde bazı kısıtlamalar vardır:

* Roller oluşturulamıyor
* Veritabanları oluşturulamıyor

## <a name="how-to-create-additional-user-roles"></a>Ek Kullanıcı rolleri oluşturma

Belirtildiği gibi, `citus` yönetici hesabının ek Kullanıcı oluşturma izni yoktur. Bir kullanıcı eklemek için Azure portal arabirimini kullanın.

1. Hiper ölçek sunucu grubunuzun **Roller** sayfasına gidin ve **+ Ekle**' ye tıklayın:

   ![Roller sayfası](media/howto-hyperscale-create-users/1-role-page.png)

2. Rol adını ve parolayı girin. **Kaydet**’e tıklayın.

   ![Rol Ekle](media/howto-hyperscale-create-users/2-add-user-fields.png)

Kullanıcı, sunucu grubunun düzenleyici düğümünde oluşturulacak ve tüm çalışan düğümlerine yayılacaktır. Azure portal aracılığıyla oluşturulan roller `LOGIN` özniteliği vardır ve bu, veritabanında oturum açabilen doğru Kullanıcı oldukları anlamına gelir.

## <a name="how-to-modify-privileges-for-user-role"></a>Kullanıcı rolü için ayrıcalıkları değiştirme

Yeni Kullanıcı rolleri genellikle kısıtlanmış ayrıcalıklarla veritabanı erişimi sağlamak için kullanılır. Kullanıcı ayrıcalıklarını değiştirmek için, PgAdmin veya psql gibi bir araç kullanarak standart PostgreSQL komutlarını kullanın. (Bkz. Hyperscale (Citus) hızlı başlangıç bölümünde [psql ile bağlanma](quickstart-create-hyperscale-portal.md#connect-to-the-database-using-psql) .)

Örneğin, okumaya izin vermek için `db_user` `mytable` izin verin:

```sql
GRANT SELECT ON mytable TO db_user;
```

Hiper ölçek (Citus) tek tablo verme deyimlerini tüm çalışan düğümlerine uygulayarak tüm küme aracılığıyla yayar. Ayrıca sistem genelinde (örneğin, bir şemadaki tüm tablolar için) Izin verir:

```sql
-- applies to the coordinator node and propagates to workers
GRANT SELECT ON ALL TABLES IN SCHEMA public TO db_user;
```

## <a name="how-to-delete-a-user-role-or-change-their-password"></a>Kullanıcı rolünü silme veya parolasını değiştirme

Bir kullanıcıyı güncelleştirmek için, hiper ölçek sunucu grubunuzun **Roller** sayfasını ziyaret edin ve kullanıcının yanındaki üç nokta **...** seçeneğine tıklayın. Üç nokta, kullanıcıyı silmek veya parolasını sıfırlamak için bir menü açar.

   ![Rol düzenleme](media/howto-hyperscale-create-users/edit-role.png)

`citus`Rol ayrıcalıklı ve silinemez.

## <a name="next-steps"></a>Sonraki adımlar

Bağlantı kurmasını sağlamak için yeni kullanıcıların makinelerinin IP adresleri için güvenlik duvarını açın: [Azure Portal kullanarak hiper ölçek (Citus) güvenlik duvarı kuralları oluşturun ve yönetin](howto-hyperscale-manage-firewall-using-portal.md).

Veritabanı kullanıcı hesabı yönetimi hakkında daha fazla bilgi için bkz. PostgreSQL ürün belgeleri:

* [Veritabanı rolleri ve ayrıcalıkları](https://www.postgresql.org/docs/current/static/user-manag.html)
* [Sözdizimi verme](https://www.postgresql.org/docs/current/static/sql-grant.html)
* [Ayrıcalıklar](https://www.postgresql.org/docs/current/static/ddl-priv.html)

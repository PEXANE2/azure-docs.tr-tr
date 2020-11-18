---
title: Desteklenen sürümler-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı 'nda desteklenen Postgres birincil ve ikincil sürümlerini açıklar-tek sunucu.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/16/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: 72d774b4ced6471ff7b355b2cb43c3c9127b5975
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94658528"
---
# <a name="supported-postgresql-major-versions"></a>Desteklenen PostgreSQL ana sürümleri

Destek ilkesi ayrıntıları için lütfen [PostgreSQL Için Azure veritabanı sürüm oluşturma ilkesine](concepts-version-policy.md) bakın.

PostgreSQL için Azure veritabanı şu anda aşağıdaki ana sürümleri desteklemektedir:

## <a name="postgresql-version-11"></a>PostgreSQL sürüm 11
Geçerli küçük yayın 11,6 ' dir. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/11/static/release-11-6.html) bakın.

## <a name="postgresql-version-10"></a>PostgreSQL sürüm 10
Geçerli küçük yayın 10,11 ' dir. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/10/static/release-10-11.html) bakın.

## <a name="postgresql-version-96"></a>PostgreSQL sürüm 9,6
Geçerli ikincil sürüm 9.6.16. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/9.6/static/release-9-6-16.html) bakın.

## <a name="postgresql-version-95"></a>PostgreSQL sürüm 9,5
Geçerli ikincil sürüm 9.5.20. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/9.5/static/release-9-5-20.html) bakın.

> [!NOTE]
> Postgres Community [sürüm oluşturma ilkesiyle](https://www.postgresql.org/support/versioning/)hizalama, PostgreSQL Için Azure veritabanı, 11 Şubat 2021 tarihinde Postgres sürüm 9,5 ' i devre dışı bırakır. Daha fazla ayrıntı ve kısıtlama için lütfen [PostgreSQL Için Azure veritabanı sürüm oluşturma ilkesine](concepts-version-policy.md) bakın.

## <a name="managing-upgrades"></a>Yükseltmeleri yönetme
PostgreSQL projesi, bildirilen hataları çözmek için düzenli olarak küçük yayınlar yayınlar. PostgreSQL için Azure veritabanı, hizmetin aylık dağıtımları sırasında küçük yayınlar içeren sunucuları otomatik olarak ekler. 

Ana sürümler için otomatik yerinde yükseltmeler desteklenmez. Bir sonraki ana sürüme yükseltmek için, şunları yapabilirsiniz 
   * [Döküm ve geri yükleme kullanarak önemli sürüm yükseltmeleri](./how-to-upgrade-using-dump-and-restore.md) gerçekleştirmeye yönelik çeşitli yöntemlere bakın
   * Yeni altyapı sürümüyle oluşturulan bir sunucuya bir veritabanını taşımak için [pg_dump ve pg_restore](./howto-migrate-using-dump-and-restore.md) kullanın
   * Alternatif olarak, [Azure veritabanı geçiş hizmeti](..\dms\tutorial-azure-postgresql-to-azure-postgresql-online-portal.md) 'Ni kullanarak PostgreSQL 10 ' dan 11 ' e yükseltebilirsiniz

### <a name="version-syntax"></a>Sürüm sözdizimi
PostgreSQL sürüm 10 ' dan önce, [PostgreSQL sürüm oluşturma ilkesi](https://www.postgresql.org/support/versioning/) , _ana sürüm_ yükseltmesini birinci _veya_ ikinci sayıda artış olacak şekilde kabul ediyor. Örneğin, 9,5 9,6, _ana_ sürüm yükseltmesi olarak kabul edildi. Sürüm 10 itibariyle, yalnızca ilk sayıdaki değişiklik ana sürüm yükseltmesi olarak kabul edilir. Örneğin, 10,0 ile 10,1 arasında _küçük_ bir yayın yükseltmesi vardır. Sürüm 10 ' dan 11 ' e _büyük_ bir sürüm yükseltirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Desteklenen PostgreSQL uzantıları hakkında daha fazla bilgi için bkz. [Uzantılar belgesi](concepts-extensions.md).

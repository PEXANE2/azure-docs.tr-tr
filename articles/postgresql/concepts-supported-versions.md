---
title: Desteklenen sürümler-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı 'nda desteklenen Postgres birincil ve ikincil sürümlerini açıklar-tek sunucu.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/17/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: 61d9712026daab63a002793adf2e17b9db943f94
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84976906"
---
# <a name="supported-postgresql-major-versions"></a>Desteklenen PostgreSQL ana sürümleri
Microsoft amaçlar, PostgreSQL için Azure veritabanı 'nda PostgreSQL altyapısının n-2 sürümlerini desteklemeye yöneliktir-tek sunucu. Sürümler, Azure 'daki geçerli ana sürümdür (n) ve önceki iki ana sürüm (-2) olacaktır.

PostgreSQL için Azure veritabanı şu anda aşağıdaki ana sürümleri desteklemektedir:

## <a name="postgresql-version-11"></a>PostgreSQL sürüm 11
Geçerli küçük yayın 11,6 ' dir. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/11/static/release-11-6.html) bakın.

## <a name="postgresql-version-10"></a>PostgreSQL sürüm 10
Geçerli küçük yayın 10,11 ' dir. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/10/static/release-10-11.html) bakın.

## <a name="postgresql-version-96"></a>PostgreSQL sürüm 9,6
Geçerli ikincil sürüm 9.6.16. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/9.6/static/release-9-6-16.html) bakın.

## <a name="postgresql-version-95"></a>PostgreSQL sürüm 9,5
Geçerli ikincil sürüm 9.5.20. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/9.5/static/release-9-5-20.html) bakın.

## <a name="managing-upgrades"></a>Yükseltmeleri yönetme
PostgreSQL projesi, bildirilen hataları çözmek için düzenli olarak küçük yayınlar yayınlar. PostgreSQL için Azure veritabanı, hizmetin aylık dağıtımları sırasında küçük yayınlar içeren sunucuları otomatik olarak ekler. 

Otomatik ana sürüm yükseltmesi desteklenmez. Örneğin PostgreSQL 9.5’ten PostgreSQL 9.6’ya otomatik yükseltme yoktur. Sonraki ana sürüme yükseltmek için, yeni altyapı sürümünde oluşturulmuş bir sunucuya [veritabanı dökümü ve yedeklemesi](./howto-migrate-using-dump-and-restore.md) oluşturun.

### <a name="version-syntax"></a>Sürüm sözdizimi
PostgreSQL sürüm 10 ' dan önce, [PostgreSQL sürüm oluşturma ilkesi](https://www.postgresql.org/support/versioning/) , _ana sürüm_ yükseltmesini birinci _veya_ ikinci sayıda artış olacak şekilde kabul ediyor. Örneğin, 9,5 9,6, _ana_ sürüm yükseltmesi olarak kabul edildi. Sürüm 10 itibariyle, yalnızca ilk sayıdaki değişiklik ana sürüm yükseltmesi olarak kabul edilir. Örneğin, 10,0 ile 10,1 arasında _küçük_ bir yayın yükseltmesi vardır. Sürüm 10 ' dan 11 ' e _büyük_ bir sürüm yükseltirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Desteklenen PostgreSQL uzantıları hakkında daha fazla bilgi için bkz. [Uzantılar belgesi](concepts-extensions.md).

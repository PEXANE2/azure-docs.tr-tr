---
title: Desteklenen sürümler - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: PostgreSQL - Single Server için Azure Veritabanı'nda desteklenen Postgres ana ve küçük sürümlerini açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 12/03/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 61dd98028b7342290984615ea19b561b48aaeadb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792226"
---
# <a name="supported-postgresql-major-versions"></a>Desteklenen PostgreSQL ana sürümleri
Microsoft, PostgreSQL - Single Server için Azure Veritabanı'ndaki PostgreSQL altyapısının n-2 sürümlerini desteklemeyi amaçlamaktadır. Sürümler, Azure (n) ve önceki iki ana sürüm (-2) üzerindeki geçerli ana sürüm olacaktır.

PostgreSQL için Azure Veritabanı şu anda aşağıdaki ana sürümleri destekler:

## <a name="postgresql-version-11"></a>PostgreSQL sürüm 11
Geçerli küçük sürüm 11.5'tir. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/11/static/release-11-5.html) bakın.

## <a name="postgresql-version-10"></a>PostgreSQL sürüm 10
Geçerli küçük sürüm 10.10'dur. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/10/static/release-10-10.html) bakın.

## <a name="postgresql-version-96"></a>PostgreSQL sürüm 9.6
Geçerli küçük sürüm 9.6.15 olduğunu. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/9.6/static/release-9-6-15.html) bakın.

## <a name="postgresql-version-95"></a>PostgreSQL sürüm 9.5
Geçerli küçük sürüm 9.5.19 olduğunu. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/9.5/static/release-9-5-19.html) bakın.

## <a name="managing-upgrades"></a>Yükseltmeleri yönetme
PostgreSQL projesi, bildirilen hataları düzeltmek için düzenli olarak küçük sürümler yayınlar. PostgreSQL için Azure Veritabanı, hizmetin aylık dağıtımları sırasında sunucuları otomatik olarak küçük sürümlerle yamalar. 

Otomatik ana sürüm yükseltmesi desteklenmez. Örneğin PostgreSQL 9.5’ten PostgreSQL 9.6’ya otomatik yükseltme yoktur. Sonraki ana sürüme yükseltmek için, yeni altyapı sürümünde oluşturulmuş bir sunucuya [veritabanı dökümü ve yedeklemesi](./howto-migrate-using-dump-and-restore.md) oluşturun.

### <a name="version-syntax"></a>Sürüm sözdizimi
PostgreSQL sürüm 10'dan önce, [PostgreSQL sürüm politikası](https://www.postgresql.org/support/versioning/) birinci _veya_ ikinci sayıda bir artış olarak önemli bir _sürüm_ yükseltmesi olarak kabul edilir. Örneğin, 9,5 ile 9,6 _önemli_ bir sürüm yükseltmesi olarak kabul edildi. Sürüm 10 itibariyle, yalnızca ilk sayıdabir değişiklik önemli bir sürüm yükseltmesi olarak kabul edilir. Örneğin, 10.0 ile 10.1 küçük _bir_ sürüm yükseltmesi. Sürüm 10 ile 11 önemli bir sürüm _yükseltmesidir._

## <a name="next-steps"></a>Sonraki adımlar
Desteklenen PostgreSQL uzantıları hakkında daha fazla bilgi için [uzantılar belgesine](concepts-extensions.md)bakın.

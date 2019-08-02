---
title: PostgreSQL için Azure veritabanı 'nda desteklenen sürümler-tek sunucu
description: PostgreSQL için Azure veritabanı 'nda desteklenen sürümleri (tek sunucu) açıklar.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 07/26/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: fae8dfb2e2cc532f4aed17f83f13bbee31d29113
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68551368"
---
# <a name="supported-postgresql-database-versions"></a>Desteklenen PostgreSQL veritabanı sürümleri
Microsoft amaçlar, PostgreSQL için Azure veritabanı 'nda PostgreSQL altyapısının n-2 sürümlerini desteklemeye yöneliktir-tek sunucu. Sürümler, Azure 'daki geçerli ana sürümdür (n) ve önceki iki ana sürüm (-2) olacaktır.

PostgreSQL için Azure veritabanı şu anda şu sürümleri desteklemektedir:

## <a name="postgresql-version-112"></a>PostgreSQL sürüm 11,2
Bu ikincil sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/11/static/release-11-2.html) bakın.

## <a name="postgresql-version-107"></a>PostgreSQL sürüm 10,7
Bu ikincil sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/10/static/release-10-7.html) bakın.

## <a name="postgresql-version-9612"></a>PostgreSQL sürüm 9.6.12
Bu ikincil sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/9.6/static/release-9-6-12.html) bakın.

## <a name="postgresql-version-9516"></a>PostgreSQL sürüm 9.5.16
Bu ikincil sürümdeki geliştirmeler ve düzeltmeler hakkında bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/9.5/static/release-9-5-16.html) başvurun.

## <a name="managing-updates-and-upgrades"></a>Güncelleştirmeleri ve yükseltmeleri yönetme
PostgreSQL için Azure veritabanı, ikincil sürüm düzeltme eklerini otomatik olarak yönetir. Şu anda ana sürüm yükseltmesi desteklenmiyor. Örneğin, PostgreSQL 9,5 ' den PostgreSQL 9,6 ' ye yükseltme desteklenmez. Bir sonraki ana sürüme yükseltmek isterseniz, bir veritabanı dökümü oluşturun ve yeni altyapı sürümüyle oluşturulmuş bir sunucuya [geri yükleyin](./howto-migrate-using-dump-and-restore.md) .

> PostgreSQL sürüm 10 ' dan önce, [PostgreSQL sürüm oluşturma ilkesinin](https://www.postgresql.org/support/versioning/) birinci _veya_ ikinci numaradan artış olacak _ana sürüm_ yükseltmesini kabul eden (örneğin, 9,5 ila 9,6 _ana_ sürüm yükseltmesi olarak kabul edildiği) göz önünde bulundurulmalıdır.
> Sürüm 10 ' dan itibaren, yalnızca ilk sayıdaki değişiklik büyük bir sürüm yükseltmesi olarak kabul edilir (örneğin, 10,0 ile 10,1 arası, _küçük_ bir sürüm yükseltirsiniz ve 10 ila 11 _ana_ sürüm yükseltmesiyle).

## <a name="next-steps"></a>Sonraki adımlar
Farklı PostgreSQL uzantılarının desteği hakkında daha fazla bilgi için bkz. [PostgreSQL uzantıları](concepts-extensions.md).

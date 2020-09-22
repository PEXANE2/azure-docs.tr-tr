---
title: Desteklenen sürümler-PostgreSQL için Azure veritabanı-esnek sunucu
description: PostgreSQL için Azure veritabanı 'nda desteklenen Postgres büyük ve küçük sürümlerini açıklar-esnek sunucu.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 9d651a38361ccaa7f8788fb4a375b39b2953228b
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941013"
---
# <a name="supported-postgresql-major-versions-in-azure-database-for-postgresql---flexible-server"></a>PostgreSQL için Azure veritabanı 'nda desteklenen PostgreSQL ana sürümleri-esnek sunucu

> [!IMPORTANT]
> PostgreSQL için Azure veritabanı-esnek sunucu önizlemededir

PostgreSQL için Azure veritabanı-esnek sunucu şu anda aşağıdaki ana sürümleri desteklemektedir:

## <a name="postgresql-version-12"></a>PostgreSQL sürüm 12

Geçerli küçük yayın 12,1 ' dir. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/12/static/release-12-1.html) bakın.

## <a name="postgresql-version-11"></a>PostgreSQL sürüm 11

Geçerli küçük yayın 11,8 ' dir. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/11/static/release-11-8.html) bakın.

## <a name="postgresql-version-10-and-older"></a>PostgreSQL sürüm 10 ve üzeri

PostgreSQL için Azure veritabanı-esnek sunucu için PostgreSQL sürüm 10 ve daha eskisini desteklemiyoruz. Eski sürümlere ihtiyacınız varsa lütfen [tek sunuculu](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions) dağıtım seçeneğini kullanın.

## <a name="managing-upgrades"></a>Yükseltmeleri yönetme

PostgreSQL projesi, bildirilen hataları çözmek için düzenli olarak küçük yayınlar yayınlar. PostgreSQL için Azure veritabanı, hizmetin aylık dağıtımları sırasında küçük yayınlar içeren sunucuları otomatik olarak ekler.

Otomatik ana sürüm yükseltmesi henüz desteklenmiyor. Örneğin, şu anda PostgreSQL 11 ' den PostgreSQL 12 ' ye otomatik yükseltme yoktur.<!-- To upgrade to the next major version, create a [database dump and restore](howto-migrate-using-dump-and-restore.md) to a server that was created with the new engine version.-->

<!--
## Next steps

For information on supported PostgreSQL extensions, see [the extensions document](concepts-extensions.md).
-->

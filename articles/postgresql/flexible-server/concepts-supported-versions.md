---
title: Desteklenen sürümler-PostgreSQL için Azure veritabanı-esnek sunucu
description: PostgreSQL için Azure veritabanı 'nda desteklenen PostgreSQL büyük ve küçük sürümlerini açıklar-esnek sunucu.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/16/2020
ms.openlocfilehash: b139e2f14379e0cd52ca72c347d49bcb4512c473
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92542089"
---
# <a name="supported-postgresql-major-versions-in-azure-database-for-postgresql---flexible-server"></a>PostgreSQL için Azure veritabanı 'nda desteklenen PostgreSQL ana sürümleri-esnek sunucu

> [!IMPORTANT]
> PostgreSQL için Azure veritabanı-esnek sunucu önizlemededir

PostgreSQL için Azure veritabanı-esnek sunucu şu anda aşağıdaki ana sürümleri desteklemektedir:

## <a name="postgresql-version-12"></a>PostgreSQL sürüm 12

Geçerli küçük yayın 12,4 ' dir. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/12/static/release-12-4.html) bakın.

## <a name="postgresql-version-11"></a>PostgreSQL sürüm 11

Geçerli küçük yayın 11,9 ' dir. Bu küçük sürümdeki geliştirmeler ve düzeltmeler hakkında daha fazla bilgi edinmek için [PostgreSQL belgelerine](https://www.postgresql.org/docs/11/static/release-11-9.html) bakın.

## <a name="postgresql-version-10-and-older"></a>PostgreSQL sürüm 10 ve üzeri

PostgreSQL için Azure veritabanı-esnek sunucu için PostgreSQL sürüm 10 ve daha eskisini desteklemiyoruz. Eski sürümlere ihtiyacınız varsa lütfen [tek sunuculu](../concepts-supported-versions.md) dağıtım seçeneğini kullanın.

## <a name="managing-upgrades"></a>Yükseltmeleri yönetme

PostgreSQL projesi, bildirilen hataları çözmek için düzenli olarak küçük yayınlar yayınlar. PostgreSQL için Azure veritabanı, hizmetin aylık dağıtımları sırasında küçük yayınlar içeren sunucuları otomatik olarak ekler.

Ana sürüm yükseltmesi için Otomasyon henüz desteklenmiyor. Örneğin, şu anda PostgreSQL 11 ' den PostgreSQL 12 ' ye otomatik yükseltme yoktur.<!-- To upgrade to the next major version, create a [database dump and restore](howto-migrate-using-dump-and-restore.md) to a server that was created with the new engine version.-->

<!--
## Next steps

For information on supported PostgreSQL extensions, see [the extensions document](concepts-extensions.md).
-->
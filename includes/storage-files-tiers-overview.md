---
title: dosya dahil etme
description: dosya dahil etme
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/27/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: f4e5c59a5ce7c0d743a574309bb1b9276ce80bea
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77597859"
---
Azure dosyaları, kendi senaryonuzu performans ve fiyat gereksinimlerine uyarlamanızı sağlamak için iki farklı depolama, Premium ve standart katmanı sunar:

- **Premium dosya paylaşımları**: Premium dosya paylaşımları, katı hal sürücüleri (SSD 'ler) tarafından desteklenir ve **FileStorage depolama hesabı** türüne dağıtılır. Premium dosya paylaşımları, GÇ yoğun iş yükleri için çoğu GÇ işlemleri için tek basamaklı milisaniye içinde tutarlı yüksek performans ve düşük gecikme sağlar. Bu, veritabanları, Web sitesi barındırma ve geliştirme ortamları gibi çok çeşitli iş yükleri için uygun hale getirir. Premium dosya paylaşımları yalnızca sağlanan faturalandırma modelinde kullanılabilir. Premium dosya paylaşımları için sağlanan faturalandırma modeli hakkında daha fazla bilgi için bkz. [Premium dosya paylaşımları için sağlamayı anlama](../articles/storage/files/storage-files-planning.md#understanding-provisioning-for-premium-file-shares).
- **Standart dosya paylaşımları**: Standart dosya paylaşımları sabit disk sürücüleri (HDD 'ler) tarafından desteklenir ve **genel amaçlı sürüm 2 (GPv2) depolama hesabı** türünde dağıtılır. Standart dosya paylaşımları, genel amaçlı dosya paylaşımları ve geliştirme/test ortamları gibi performans çeşitliliğine daha az duyarlı olan GÇ iş yükleri için güvenilir performans sağlar. Standart dosya paylaşımları yalnızca Kullandıkça Öde faturalandırma modelinde kullanılabilir.
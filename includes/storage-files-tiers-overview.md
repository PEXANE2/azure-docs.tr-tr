---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/27/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: f4e5c59a5ce7c0d743a574309bb1b9276ce80bea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597859"
---
Azure Files, hisselerinizi senaryonuzun performans ve fiyat gereksinimlerine göre uyarlamanızı sağlayan iki farklı depolama alanı katmanı, premium ve standart sunar:

- **Premium dosya paylaşımları**: Premium dosya paylaşımları katı hal sürücüleri (SSD'ler) tarafından desteklenir ve **FileStorage depolama hesabı** türünde dağıtılır. Premium dosya paylaşımları, çoğu IO işlemi için, IO yoğun iş yükleri için tek basamaklı milisaniyeler içinde tutarlı yüksek performans ve düşük gecikme süremi sağlar. Bu, onları veritabanları, web sitesi barındırma ve geliştirme ortamları gibi çok çeşitli iş yükleri için uygun hale getirir. Premium dosya paylaşımları yalnızca uygun faturalandırma modelinde kullanılabilir. Premium dosya paylaşımları için sağlanan faturalandırma modeli hakkında daha fazla bilgi [için](../articles/storage/files/storage-files-planning.md#understanding-provisioning-for-premium-file-shares)bkz.
- **Standart dosya paylaşımları**: Standart dosya paylaşımları sabit disk sürücüleri (HDD'ler) tarafından desteklenir ve **genel amaçlı sürüm 2 (GPv2) depolama hesabı** türünde dağıtılır. Standart dosya paylaşımları, genel amaçlı dosya paylaşımları ve geliştirme/test ortamları gibi performans değişkenliğine daha az duyarlı olan IO iş yükleri için güvenilir performans sağlar. Standart dosya paylaşımları yalnızca istediğiniz kadar öde faturalandırma modelinde kullanılabilir.
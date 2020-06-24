---
title: dosya dahil etme
description: dosya dahil etme
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 06/18/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 643cf8fd400adf06bf61f070947bd78ba7be50eb
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85242063"
---
| Kaynak | Temel | Standart | Premium |
|---|---|---|---|
| Dahil edilen depolama<sup>1</sup> (gib) | 10 | 100 | 500 |
| Depolama sınırı (TiB) | 20| 20 | 20 |
| Maksimum görüntü katmanı boyutu (GiB) | 200 | 200 | 200 |
| Dakikada ReadOps<sup>2, 3</sup> | 1000 | 3,000 | 10,000 |
| Dakikada WriteOps<sup>2, 4</sup> | 100 | 500 | 2.000 |
| İndirme bant genişliği MBps<sup>2</sup> | 30 | 60 | 100 |
| Karşıya yükleme bant genişliği MBps<sup>2</sup> | 10 | 20 | 50 |
| Web Kancaları | 2 | 10 | 500 |
| Coğrafi çoğaltma | Yok | Yok | [Destek][geo-replication] |
| İçerik güveni | Yok | Yok | [Destek][content-trust] |
| Özel uç noktalarla özel bağlantı | Yok | Yok | [Destek][plink] |
| Hizmet uç noktası VNet erişimi | Yok | Yok | [Önizleme][vnet] |
| Müşteri tarafından yönetilen anahtarlar | Yok | Yok | [Destek][cmk] |
| Depo kapsamlı izinler | Yok | Yok | [Önizleme][token]|
| &bull;Simgelerini | Yok | Yok | 20.000 |
| &bull;Kapsam eşlemeleri | Yok | Yok | 20.000 |
| &bull;Kapsam başına depo eşleme | Yok | Yok | 500 |


<sup>1</sup> depolama her katman için günlük fiyata dahildir. Ek depolama için, depolama sınırına kadar, GiB başına ek bir günlük ücret ödersiniz. Hız bilgileri için bkz. [Azure Container Registry fiyatlandırması][pricing].

<sup>2</sup>*readops*, *writeops*ve *bant genişliği* minimum tahminlerdir. Azure Container Registry, kullanım gerektirdiğinden performansı artırmak için çaba harcar.

<sup>3</sup> [Docker Pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) , görüntüdeki katmanların sayısına ve bildirim almaya göre birden çok okuma işlemine çevrilir.

<sup>4</sup> [Docker Push](https://docs.docker.com/registry/spec/api/#pushing-an-image) , itilmiş olması gereken katmanların sayısına bağlı olarak birden çok yazma işlemine çevrilir. , `docker push` Var olan bir görüntü için bildirim almak üzere *Readops 'leri* içerir.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
[vnet]: ../articles/container-registry/container-registry-vnet.md
[plink]: ../articles/container-registry/container-registry-private-link.md
[cmk]: ../articles/container-registry/container-registry-customer-managed-keys.md
[token]: ../articles/container-registry/container-registry-repository-scoped-permissions.md
---
title: include dosyası
description: include dosyası
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 03/11/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 0090f02382e024e5539383328b55d58798002d63
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79117108"
---
| Kaynak | Temel | Standart | Premium |
|---|---|---|---|
| Depolama<sup>1</sup> | 10 GiB | 100 GiB| 500 GiB |
| Maksimum görüntü katman boyutu | 200 GiB | 200 GiB | 200 GiB |
| Dakika başına ReadOps<sup>2, 3</sup> | 1000 | 3\.000 | 10,000 |
| Dakika başına WriteOps<sup>2, 4</sup> | 100 | 500 | 2\.000 |
| İndirme bant genişliği (Mb/sn)<sup>2</sup> | 30 | 60 | 100 |
| Karşıya yükleme bant genişliği (Mb/sn)<sup>2</sup> | 10 | 20 | 50 |
| Web Kancaları | 2 | 10 | 500 |
| Coğrafi çoğaltma | Yok | Yok | [Destekleniyor][geo-replication] |
| İçerik güveni | Yok | Yok | [Destekleniyor][content-trust] |
| Sanal ağ erişimi | Yok | Yok | [Önizleme][vnet] |
| Özel bağlantı tümleştirmesi | Yok | Yok | [Önizleme][plink] |
| Müşteri tarafından yönetilen anahtarlar | Yok | Yok | [Önizleme][cmk] |
| Depo kapsamında izinler | Yok | Yok | [Önizleme][token]|
| &bull; Belirteçler | Yok | Yok | 20.000 |
| &bull; Kapsam eşlemesi | Yok | Yok | 20.000 |
| Kapsam eşlemesi başına &bull; depo | Yok | Yok | 500 |


<sup>1</sup>Belirtilen depolama sınırları, her katmana *eklenen* depolama alanı miktarıdır. Bu sınırların üzerindeki görüntü depolama alanı için GiB başına günlük ek ücret ödersiniz. Fiyat bilgisi için bkz. [Azure Container Registry fiyatlandırması][pricing].

<sup>2</sup>*ReadOps*, *WriteOps* ve *Bandwidth* en düşük tahminlerdir. Azure Container Registry, performansı kullanımın gerektirdiği oranda artırmayı hedefler.

<sup>3</sup> [docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) görüntüdeki katmanların sayısı temel alınarak birden fazla okuma işlemine ve bildirim alma işlemine karşılık gelir.

<sup>4</sup> [docker push](https://docs.docker.com/registry/spec/api/#pushing-an-image) gönderilmesi gereken katan sayısı temel alınarak birden fazla yazma işlemine karşılık gelir. `docker push`, mevcut bir görüntü için bildirim alma olanağı tanıyan *ReadOps*’u içerir.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
[vnet]: ../articles/container-registry/container-registry-vnet.md
[plink]: ../articles/container-registry/container-registry-private-link.md
[cmk]: ../articles/container-registry/container-registry-customer-managed-keys.md
[token]: ../articles/container-registry/container-registry-repository-scoped-permissions.md
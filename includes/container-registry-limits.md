---
title: include dosyası
description: include dosyası
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 06/18/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 089b1b6f1af2f19c16866858324bde2e151e8bdb
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98052926"
---
| Kaynak | Temel | Standart | Premium |
|---|---|---|---|
| Dahil edilen depolama<sup>1</sup> (gib) | 10 | 100 | 500 |
| Depolama sınırı (TiB) | 20| 20 | 20 |
| Maksimum görüntü katmanı boyutu (GiB) | 200 | 200 | 200 |
| Dakikada ReadOps<sup>2, 3</sup> | 1.000 | 3.000 | 10,000 |
| Dakikada WriteOps<sup>2, 4</sup> | 100 | 500 | 2.000 |
| Bant genişliği<sup>2</sup> (Mbps) İndir | 30 | 60 | 100 |
| Bant genişliğini karşıya yükle <sup>2</sup> (Mbps) | 10 | 20 | 50 |
| Web Kancaları | 2 | 10 | 500 |
| Coğrafi çoğaltma | Yok | Yok | [Desteklenir][geo-replication] |
| Kullanılabilirlik alanları | Yok | Yok | [Önizleme][zones] |
| İçerik güveni | Yok | Yok | [Desteklenir][content-trust] |
| Özel uç noktalarla özel bağlantı | Yok | Yok | [Desteklenir][plink] |
| &bull; Özel uç noktalar | Yok | Yok | 10 |
| Hizmet uç noktası VNet erişimi | Yok | Yok | [Önizleme][vnet] |
| Müşteri tarafından yönetilen anahtarlar | Yok | Yok | [Desteklenir][cmk] |
| Depo kapsamlı izinler | Yok | Yok | [Önizleme][token]|
| &bull; Simgelerini | Yok | Yok | 20.000 |
| &bull; Kapsam eşlemeleri | Yok | Yok | 20.000 |
| &bull; Kapsam başına depo eşleme | Yok | Yok | 500 |


<sup>1</sup> depolama her katman için günlük fiyata dahildir. Ek depolama alanı, kayıt defteri depolama sınırına kadar, GiB başına ek günlük ücretle kullanılabilir. Hız bilgileri için bkz. [Azure Container Registry fiyatlandırması][pricing]. Kayıt defteri depolama sınırının ötesinde depolamaya ihtiyacınız varsa lütfen Azure desteğine başvurun.

<sup>2</sup>*readops*, *writeops* ve *bant genişliği* minimum tahminlerdir. Azure Container Registry, kullanım gerektirdiğinden performansı artırmak için çaba harcar.

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
[zones]: ../articles/container-registry/zone-redundancy.md

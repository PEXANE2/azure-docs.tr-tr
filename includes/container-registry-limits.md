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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79117108"
---
| Kaynak | Temel | Standart | Premium |
|---|---|---|---|
| Depolama<sup>1</sup> | 10 GİB | 100 GiB| 500 GİB |
| Maksimum görüntü katmanı boyutu | 200 GİB | 200 GİB | 200 GİB |
| Dakikada ReadOps<sup>2, 3</sup> | 1000 | 3,000 | 10,000 |
| Dakikada WriteOps<sup>2, 4</sup> | 100 | 500 | 2.000 |
| Bant genişliği MBps<sup>2'yi</sup> indirin | 30 | 60 | 100 |
| Bant genişliği MBps<sup>2'yi</sup> yükleme | 10 | 20 | 50 |
| Web Kancaları | 2 | 10 | 500 |
| Coğrafi çoğaltma | Yok | Yok | [Desteklenen][geo-replication] |
| İçerik güveni | Yok | Yok | [Desteklenen][content-trust] |
| Sanal ağ erişimi | Yok | Yok | [Önizleme][vnet] |
| Özel bağlantı tümleştirmesi | Yok | Yok | [Önizleme][plink] |
| Müşteri tarafından yönetilen anahtarlar | Yok | Yok | [Önizleme][cmk] |
| Depo kapsamı yla ilgili izinler | Yok | Yok | [Önizleme][token]|
| &bull;Belirteç -leri | Yok | Yok | 20.000 |
| &bull;Kapsam haritaları | Yok | Yok | 20.000 |
| &bull;Kapsam haritası başına depolar | Yok | Yok | 500 |


<sup>1.1.2</sup> Belirtilen depolama sınırları, her katman için *dahil edilen* depolama miktarıdır. Bu sınırların üzerindeki görüntü depolama için GiB başına ek bir günlük ücret lendirilirsiniz. Fiyat bilgileri için Azure [Kapsayıcı Kayıt Defteri fiyatlandırması'na][pricing]bakın.

<sup>2</sup>*ReadOps*, *WriteOps*, ve *Bant genişliği* minimum tahminlerdir. Azure Kapsayıcı Kayıt Defteri, kullanımın gerektirdiği performansı artırmaya çalışır.

<sup>3.2.2</sup> [Docker çekme,](https://docs.docker.com/registry/spec/api/#pulling-an-image) görüntüdeki katman sayısına ve bildirim alma durumuna bağlı olarak birden çok okuma işlemi anlamına gelir.

<sup>4.2.2</sup> [Docker push,](https://docs.docker.com/registry/spec/api/#pushing-an-image) itilmesi gereken katman sayısına bağlı olarak birden çok yazma işlemi anlamına gelir. A, `docker push` varolan bir görüntü için bir bildirim almak için *ReadOps* içerir.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
[vnet]: ../articles/container-registry/container-registry-vnet.md
[plink]: ../articles/container-registry/container-registry-private-link.md
[cmk]: ../articles/container-registry/container-registry-customer-managed-keys.md
[token]: ../articles/container-registry/container-registry-repository-scoped-permissions.md
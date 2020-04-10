---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/08/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a5e0e459800e7cb57672518597f3d04a74f53118
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81008664"
---
Şimdilik, ultra diskler ek sınırlamalar var, onlar aşağıdaki gibidir:

Şu anda ultra diskler için kullanılabilen tek altyapı artıklığı seçenekleri kullanılabilirlik bölgeleridir. Başka bir artıklık seçeneği kullanan VM'ler ultra disk ekleyemez.

Aşağıdaki tabloda, ultra disklerin bulunduğu bölgelerin yanı sıra ilgili kullanılabilirlik seçenekleri özetlenmektedir:

> [!NOTE]
> Bu bölgelerdeki bazı kullanılabilirlik bölgesi ultra diskler sunmaz.

|Bölgeler  |Altyapı artıklığı yok  |Kullanılabilirlik alanları  |
|---------|---------|---------|
|Batı ABD     |Evet         |Hayır         |
|Batı ABD 2    |Hayır         |Evet         |
|Doğu ABD     |Hayır         |Evet         |
|Doğu ABD 2     |Hayır         |Evet         |
|Güneydoğu Asya     |Hayır         |Evet         |
|Kuzey Avrupa     |Hayır         |Evet         |
|Batı Avrupa     |Hayır         |Evet         |
|Güney Birleşik Krallık     |Hayır         |Evet         |

- Yalnızca aşağıdaki VM serilerinde desteklenir:
    - [ESv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - [DSv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - FSv2
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- Her VM boyutu, desteklenen her bölgede ultra disklerle kullanılamaz
- Yalnızca veri diskleri olarak kullanılabilir ve yalnızca 4k fiziksel sektör boyutunu destekler. Ultra Disk'in 4K yerel sektör boyutu nedeniyle, ultra disklerle uyumlu olmayacak bazı uygulamalar vardır. Bir örnek, ultra diskleri desteklemek için sürüm 12.2 veya daha sonra gerektiren Oracle Veritabanı olacaktır.  
- Yalnızca boş diskler olarak oluşturulabilir  
- Şu anda disk anlık görüntülerini, VM görüntülerini, kullanılabilirlik kümelerini, Azure Özel Ana Bilgisayarlarını veya Azure disk şifrelemeyi desteklemiyor
- Şu anda Azure Yedekleme veya Azure Site Kurtarma ile tümleştirmeyi desteklemiyor
- GA VM'lerde IOPS için geçerli maksimum sınır 80.000'dir.

Azure ultra diskler varsayılan olarak her gün başına 16 TiB'ye kadar teklif sunar, ancak ultra diskler isteğe bağlı olarak daha yüksek kapasiteyi destekler. Kapasite artışı istemek için Azure Desteği'ne başvurun.
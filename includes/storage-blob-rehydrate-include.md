---
title: include dosyası
description: include dosyası
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/09/2019
ms.service: storage
ms.subservice: blobs
ms.topic: include
ms.reviewer: hux
ms.custom: include file
ms.openlocfilehash: 81ffc87ce97a936e693c59bca6caf721cb8599cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75780258"
---
Arşiv depolama birimindeki verileri okumak için katmanı önce sık erişimli veya seyrek erişimli blob katmanı olarak değiştirmeniz gerekir. Bu süreç rehidrasyon olarak bilinir ve tamamlanması saatler sürebilir. Optimum rehidrasyon performansı için büyük lekeler öneririz. Birkaç küçük blobu aynı anda yeniden doldurmak süreyi uzatabilir. Şu anda iki rehydrate öncelikleri vardır, Yüksek (önizleme) ve Standart, bir [Set Blob Tier](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) veya [Copy Blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob) işlemi isteğe bağlı *x-ms-rehydrate öncelikli* özelliği üzerinden ayarlanabilir.

* **Standart öncelik**: Rehidrasyon isteği alındığı sırada işlenir ve 15 saat kadar sürebilir.
* **Yüksek öncelik (önizleme)**: Rehidrasyon isteği Standart isteklere göre öncelikli olacak ve 1 saatten daha az bir sürede bitirilebilir. Yüksek öncelik, blob boyutuna ve mevcut talebe bağlı olarak 1 saatten uzun sürebilir. Yüksek öncelikli isteklerin Standart öncelikli isteklere göre önceliklendirilmesi garanti edilir.

> [!NOTE]
> Standart öncelik arşiv için varsayılan rehidrasyon seçeneğidir. Yüksek öncelik standart öncelikli rehidrasyon daha fazla mal olacak ve genellikle acil veri restorasyon durumlarda kullanılmak üzere ayrılmıştır daha hızlı bir seçenektir.

Bir rehidrasyon isteği başlatıldıktan sonra iptal edilemez. Yeniden doldurma sırasında katmanın değişip değişmediğini onaylamak için *Arşiv Durumu* blob özelliğini kontrol edebilirsiniz. Durum, hedef katmana göre "rehydrate-pending-to-hot" veya "rehydrate-pending-to-cool" olabilir. Tamamlandıktan sonra arşiv durumu özelliği kaldırılır ve *Erişim Katmanı* blob özelliği sık veya seyrek erişimli bu yeni katmanı gösterir.

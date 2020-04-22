---
title: include dosyası
description: include dosyası
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 04/08/2020
ms.service: storage
ms.subservice: blobs
ms.topic: include
ms.reviewer: hux
ms.custom: include file
ms.openlocfilehash: fc5f4d2c10cac23600025a72fedf7fe2cec5a34e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81684066"
---
Arşiv depolama birimindeki verileri okumak için katmanı önce sık erişimli veya seyrek erişimli blob katmanı olarak değiştirmeniz gerekir. Bu süreç rehidrasyon olarak bilinir ve tamamlanması saatler sürebilir. Optimum rehidrasyon performansı için büyük lekeler öneririz. Birkaç küçük blobu aynı anda yeniden doldurmak süreyi uzatabilir. Şu anda iki rehydrate öncelikleri vardır, Yüksek ve Standart, bir [Set Blob Tier](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) veya [Copy Blob](https://docs.microsoft.com/rest/api/storageservices/copy-blob) işlemi isteğe bağlı *x-ms-rehydrate öncelikli* özelliği üzerinden ayarlanabilir.

* **Standart öncelik**: Rehidrasyon isteği alındığı sırada işlenir ve 15 saat kadar sürebilir.
* **Yüksek öncelik**: Rehidrasyon isteği standart isteklere göre öncelikli olacak ve 1 saatten az bir sürede bitirilebilir. Yüksek öncelik, blob boyutuna ve mevcut talebe bağlı olarak 1 saatten uzun sürebilir. Yüksek öncelikli isteklerin Standart öncelikli isteklere göre önceliklendirilmesi garanti edilir.

> [!NOTE]
> Standart öncelik arşiv için varsayılan rehidrasyon seçeneğidir. Yüksek öncelik standart öncelikli rehidrasyon daha fazla mal olacak ve genellikle acil veri restorasyon durumlarda kullanılmak üzere ayrılmıştır daha hızlı bir seçenektir.

Bir rehidrasyon isteği başlatıldıktan sonra iptal edilemez. Rehidrasyon işlemi sırasında, *x-ms-erişim katmanı* blob özelliği rehidrasyon online bir katmana tamamlanana kadar arşiv olarak göstermeye devam edecektir. Rehidrasyon durumunu ve ilerlemesini doğrulamak için, *x-ms-arşiv durumunu* ve *x-ms-rehidrat öncelikli* blob özelliklerini kontrol etmek için [Get Blob Properties'i](https://docs.microsoft.com/rest/api/storageservices/get-blob-properties) arayabilirsiniz. Arşiv durumu, rehidrat hedef katmanına bağlı olarak "rehydrate-bekleyen-sıcak" veya "rehydrate-bekleyen-to-cool" okuyabilirsiniz. Rehidrat önceliği "Yüksek" veya "Standart" hızını gösterir. Tamamlandıktan sonra, arşiv durumu ve rehydrate öncelik özellikleri kaldırılır ve erişim katmanı blob özelliği seçilen sıcak veya serin katmanı yansıtacak şekilde güncellenir.

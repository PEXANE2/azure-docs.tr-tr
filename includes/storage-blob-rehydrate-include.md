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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81684066"
---
Arşiv depolama birimindeki verileri okumak için katmanı önce sık erişimli veya seyrek erişimli blob katmanı olarak değiştirmeniz gerekir. Bu işleme yeniden doldurma denir ve tamamlanması saat sürebilir. En iyi yeniden doldurma performansı için büyük blob boyutları öneririz. Birkaç küçük blobu aynı anda yeniden doldurmak süreyi uzatabilir. Şu anda iki adet yeniden doldurma önceliği vardır, yüksek ve standart. Bu, bir [BLOB katmanı kümesi](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) veya [BLOB kopyalama](https://docs.microsoft.com/rest/api/storageservices/copy-blob) işleminde isteğe bağlı *x-MS-rehite-Priority* özelliği aracılığıyla ayarlanabilir.

* **Standart öncelik**: yeniden doldurma isteği alındığı sırada işlenir ve 15 saate kadar sürebilir.
* **Yüksek öncelik**: yeniden doldurma isteğine standart istekler üzerinden öncelik alınacaktır ve 1 saat altında bitecektir. En yüksek öncelik, blob boyutuna ve geçerli talebe bağlı olarak 1 saatten uzun sürebilir. Yüksek öncelikli isteklerin standart öncelik istekleri üzerinden öncelikli olması garanti edilir.

> [!NOTE]
> Standart öncelik Arşiv için varsayılan yeniden hidratıon seçeneğidir. Yüksek öncelikli, standart öncelikten daha fazla yeniden doldurma maliyeti sağlayacak ve genellikle acil durum veri geri yükleme durumlarında kullanılmak üzere ayrılmış olan daha hızlı bir seçenektir.

Yeniden doldurma isteği başlatıldıktan sonra iptal edilemez. Yeniden doldurma işlemi sırasında, *x-MS-Access-Tier* blob özelliği çevrimiçi bir katmana yeniden doldurma tamamlanana kadar arşiv olarak gösterilmeye devam edecektir. Yeniden doldurma durumunu ve ilerlemesini onaylamak için, [BLOB özelliklerini al](https://docs.microsoft.com/rest/api/storageservices/get-blob-properties) ' ı çağırıp *x-MS-Archive-Status* ve *x-MS-rehibulunan öncelikli* blob özelliklerini kontrol edebilirsiniz. Arşiv durumu, yeniden doldurma hedef katmanına bağlı olarak "rehion-to-Hot" veya "rehion-with-soğuk" olarak okunabilir. Yeniden doldurma önceliği, "yüksek" veya "standart" hızını gösterecektir. Tamamlandıktan sonra, arşiv durumu ve yeniden doldurma önceliği özellikleri kaldırılır ve erişim katmanı blobu özelliği seçili sık veya seyrek katmanı yansıtacak şekilde güncelleştirilir.

---
title: Azure Veri Kutusu Ağ Geçidi sınırları | Microsoft Dokümanlar
description: Microsoft Azure Veri Kutusu Ağ Geçidi için sistem sınırlarını ve önerilen boyutları açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/20/2019
ms.author: alkohli
ms.openlocfilehash: e80b03f696a78887676e9f16750055a4dcfac230
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60755257"
---
# <a name="azure-data-box-gateway-limits"></a>Azure Veri Kutusu Ağ Geçidi sınırları

Microsoft Azure Veri Kutusu Ağ Geçidi çözümünüzü dağıtırken ve çalıştırırken bu sınırları göz önünde bulundurun. 


## <a name="data-box-gateway-service-limits"></a>Veri Kutusu Ağ Geçidi hizmet sınırları

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="data-box-gateway-device-limits"></a>Veri Kutusu Ağ Geçidi aygıt sınırları

Aşağıdaki tabloda Veri Kutusu Ağ Geçidi aygıtının sınırları açıklanmaktadır.

| Açıklama | Değer |
|---|---|
|Hayır. cihaz başına dosyaların |100 milyon <br> Limit 100 milyon maksimum limit ile disk alanı her 2 TB için ~ 25 milyon dosya |
|Hayır. cihaz başına hisse |24 |
|Hayır. Azure depolama kapsayıcısı başına hisse |1 |
|Bir paylaşıma yazılan en büyük dosya boyutu|2-TB sanal aygıt için maksimum dosya boyutu 500 GB'dır. <br> Maksimum dosya boyutu, veri diskboyutu yla birlikte en fazla 5 TB'ye ulaşana kadar önceki oranda artar. |

## <a name="azure-storage-limits"></a>Azure depolama sınırları

[!INCLUDE [data-box-edge-gateway-storage-limits](../../includes/data-box-edge-gateway-storage-limits.md)]

## <a name="data-upload-caveats"></a>Veri yükleme uyarıları

[!INCLUDE [data-box-edge-gateway-storage-data-upload-caveats](../../includes/data-box-edge-gateway-storage-data-upload-caveats.md)]

## <a name="azure-storage-account-size-and-object-size-limits"></a>Azure depolama hesabı boyutu ve nesne boyutu sınırları

[!INCLUDE [data-box-edge-gateway-storage-acct-limits](../../includes/data-box-edge-gateway-storage-acct-limits.md)]

## <a name="azure-object-size-limits"></a>Azure nesne boyutu sınırları

[!INCLUDE [data-box-edge-gateway-storage-object-limits](../../includes/data-box-edge-gateway-storage-object-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Box Gateway'i dağıtmaya hazırlanma](data-box-gateway-deploy-prep.md)

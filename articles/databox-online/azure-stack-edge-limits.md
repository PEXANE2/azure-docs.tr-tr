---
title: Azure Stack Edge limitleri | Microsoft Docs
description: Hizmet sınırları, cihaz limitleri ve depolama sınırları dahil olmak üzere Azure Stack Edge 'i dağıtırken ve işletirken sınırlar ve önerilen Boyutlar hakkında bilgi edinin.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/22/2019
ms.author: alkohli
ms.openlocfilehash: 8bbcf3f61121813b91cb951809992d10977a640c
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87922719"
---
# <a name="azure-stack-edge-limits"></a>Azure Stack Edge sınırları

Microsoft Azure Stack Edge çözümünüzü dağıtırken ve işletirken bu limitleri göz önünde bulundurun. 

## <a name="azure-stack-edge-service-limits"></a>Azure Stack Edge hizmeti limitleri

[!INCLUDE [data-box-edge-gateway-service-limits](../../includes/data-box-edge-gateway-service-limits.md)]

## <a name="azure-stack-device-limits"></a>Cihaz sınırlarını Azure Stack

Aşağıdaki tablo Azure Stack Edge cihazının sınırlarını açıklamaktadır. 

| Açıklama | Değer |
|---|---|
|Hayır. cihaz başına dosya |100.000.000 |
|Hayır. cihaz başına paylaşım sayısı |24 |
|Hayır. kapsayıcı başına paylaşım sayısı |1 |
|Bir paylaşıma yazılan en büyük dosya boyutu| 5 TB |

## <a name="azure-storage-limits"></a>Azure depolama sınırları

[!INCLUDE [data-box-edge-gateway-storage-limits](../../includes/data-box-edge-gateway-storage-limits.md)]

## <a name="data-upload-caveats"></a>Veri yükleme uyarıları

[!INCLUDE [data-box-edge-gateway-storage-data-upload-caveats](../../includes/data-box-edge-gateway-storage-data-upload-caveats.md)]

## <a name="azure-storage-account-size-and-object-size-limits"></a>Azure depolama hesabı boyutu ve nesne boyutu sınırları

[!INCLUDE [data-box-edge-gateway-storage-acct-limits](../../includes/data-box-edge-gateway-storage-acct-limits.md)]


## <a name="azure-object-size-limits"></a>Azure nesne boyutu sınırları

[!INCLUDE [data-box-edge-gateway-storage-object-limits](../../includes/data-box-edge-gateway-storage-object-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Edge dağıtmaya hazırlanma](azure-stack-edge-deploy-prep.md)

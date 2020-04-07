---
title: Sınırlar
description: SDK özellikleri için kod sınırlamaları
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: 6e23c716a965cdf178c9f847900e66c141d5d7eb
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680342"
---
# <a name="limits"></a>Sınırlar

Çalışan sistemin iç ayrıntıları nedeniyle bir dizi özellik boyut veya sayım sınırlamaları vardır.

## <a name="azure-frontend"></a>Azure Ön Uç

* İşlem başına toplam AzureFrontend örnekleri: 16.
* AzureFrontend başına toplam AzureSession örnekleri: 16.

## <a name="objects"></a>Nesneler

* Tek bir türde izin verilebilen toplam nesneler (Varlık, CutPlaneComponent, vb.): 16.777.215.
* İzin verilebilen toplam aktif kesme düzlemi: 8.

## <a name="materials"></a>Malzemeler

* Bir varlıkta izin verilebilen toplam malzeme sayısı: 65.535.

## <a name="overall-number-of-polygons"></a>Toplam çokgen sayısı

Tüm yüklenen modeller için izin verilebilen çokgen [sayısı, oturum yönetimi REST API'ye](../how-tos/session-rest-api.md#create-a-session)geçildiği gibi VM'nin boyutuna bağlıdır:

| VM boyutu | Maksimum çokgen sayısı |
|:--------|:------------------|
|Standart| 20 milyon |
|Premium| limitsiz |




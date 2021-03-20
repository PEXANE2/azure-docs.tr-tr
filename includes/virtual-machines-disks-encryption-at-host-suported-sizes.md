---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: e5a811620de8336abd3e0df6d72db761ce18b2b6
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "86230999"
---
Tüm VM boyutlarının en son nesli, konakta şifrelemeyi destekler:

|Tür  |Desteklenmiyor  |Desteklenir  |
|---------|---------|---------|
|Genel amaçlı     | Dv3, Dav4, Dv2, AV2        | B, DSv2, Dsv3, DC, DCv2, Dasv4        |
|İşlem için iyileştirilmiş     |         | Fsv2        |
|Bellek için iyileştirilmiş     | Ev3, Eav4        | DSv2, Esv3, d, Mv2, Easv4        |
|Depolama için iyileştirilmiş     |         | Ls, Lsv2 (NVMe diskleri şifrelenmez)        |
|GPU     | NC, NV        | NCv2, NCv3, ND, NVv3, NVv4, NDv2 (Önizleme)        |
|Yüksek performanslı işlem     | H        | HB, HC, HBv2        |
|Önceki nesiller     | F, A, D, L, G        | DS, GS, FS, NVv2        |

VM boyutunu yükseltme, yeni VM boyutunun EncryptionAtHost özelliğini destekleyip desteklemediğini denetbir doğrulamaya neden olur.

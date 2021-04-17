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
ms.openlocfilehash: e8ec7faf1562381288aeef630bf2076ce413017a
ms.sourcegitcommit: 49b2069d9bcee4ee7dd77b9f1791588fe2a23937
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2021
ms.locfileid: "107531537"
---
Tüm VM boyutlarının en son nesli, konakta şifrelemeyi destekler:

|Tür  |Desteklenmiyor  |Desteklenir  |
|---------|---------|---------|
|Genel amaçlı     | Dv3, Dv2, AV2        | B, DSv2, Dsv3, DC, DCv2, Dav4, Dasv4        |
|İşlem için iyileştirilmiş     |         | Fsv2        |
|Bellek için iyileştirilmiş     | Ev3        | DSv2, Esv3, d, Mv2, Eav4, Easv4        |
|Depolama için iyileştirilmiş     |         | Ls, Lsv2 (NVMe diskleri şifrelenmez)        |
|GPU     | NC, NV        | NCv2, NCv3, ND, NVv3, NVv4, NDv2 (Önizleme)        |
|Yüksek performanslı işlem     | H        | HB, HC, HBv2        |
|Önceki nesiller     | F, A, D, L, G        | DS, GS, FS, NVv2        |

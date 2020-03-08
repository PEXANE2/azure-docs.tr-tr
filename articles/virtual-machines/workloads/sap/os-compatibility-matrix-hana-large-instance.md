---
title: SAP HANA için işletim sistemi uyumluluk matrisi (büyük örnekler) | Microsoft Docs
description: Uyumluluk matrisi, farklı donanım türleriyle farklı Işletim sistemi sürümlerinin uyumluluğunu temsil eder (büyük örnekler)
services: virtual-machines-linux
documentationcenter: ''
author: sasarava
manager: hrushib
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/03/2020
ms.author: sasarava
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: aa19433ef6446932da3509694ccccd08538b964f
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78675635"
---
# <a name="compatible-operating-systems-for-hana-large-instances"></a>HANA büyük örnekleri için uyumlu Işletim sistemleri

## <a name="hana-large-instance-type-i"></a>HANA büyük örnek türü ı     
  | İşletim Sistemi | Kullanılabilirlik        | SKU                                                          |
  |------------------|---------------------|---------------------------------------------------------------|
  | SLES 12 SP2      | Artık sunulmadı | S72, S72m, S96, S144, S144m, S192, S192m, S192xm              |
  | SLES 12 SP3      | Kullanılabilir           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm              |
  | SLES 12 SP4      | Kullanılabilir           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224, S224m |
  
### <a name="persistent-memory-skus"></a>Kalıcı bellek SKU 'Ları
  | İşletim Sistemi | Kullanılabilirlik | SKU                             |
  |------------------|--------------|----------------------------------|
  | SLES 12 SP4      | Kullanılabilir    | S224oo, S224om, S224ooo, S224oom |
  
## <a name="hana-large-instance-type-ii"></a>HANA büyük örnek türü II     
  |  İşletim Sistemi       | Kullanılabilirlik        | SKU                                                              |
  |-------------------------|---------------------|-------------------------------------------------------------------|
  | SLES 12 SP2             | Artık sunulmadı | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m |
  | SLES 12 SP3             | Kullanılabilir           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m |
  
## <a name="related-documents"></a>İlgili belgeler

- [Kullanılabilir SKU 'lar](hana-available-skus.md) hakkında daha fazla bilgi edinmek için
- [Işletim sistemini yükseltme](os-upgrade-hana-large-instance.md) hakkında bilgi edinmek için
  

  
  

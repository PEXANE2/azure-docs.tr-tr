---
title: SAP HANA için İşletim Sistemi Uyumluluk Matrisi (Büyük Örnekler)| Microsoft Dokümanlar
description: Uyumluluk matrisi, İşletim Sisteminin farklı sürümlerinin farklı donanım türleri (Büyük Örnekler) ile uyumluluğunu temsil eder
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78675635"
---
# <a name="compatible-operating-systems-for-hana-large-instances"></a>HANA Büyük Örnekleri için Uyumlu İşletim Sistemleri

## <a name="hana-large-instance-type-i"></a>HANA Büyük Örnek Tip I     
  | İşletim Sistemi | Kullanılabilirlik        | SKU'lar                                                          |
  |------------------|---------------------|---------------------------------------------------------------|
  | SLES 12 SP2      | Artık teklif edilmedi | S72, S72m, S96, S144, S144m, S192, S192m, S192xm              |
  | SLES 12 SP3      | Kullanılabilir           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm              |
  | SLES 12 SP4      | Kullanılabilir           | S72, S72m, S96, S144, S144m, S192, S192m, S192xm, S224, S224m |
  
### <a name="persistent-memory-skus"></a>Kalıcı Bellek SKUs
  | İşletim Sistemi | Kullanılabilirlik | SKU'lar                             |
  |------------------|--------------|----------------------------------|
  | SLES 12 SP4      | Kullanılabilir    | S224oo, S224om, S224ooo, S224oom |
  
## <a name="hana-large-instance-type-ii"></a>HANA Büyük Örnek Tip II     
  |  İşletim Sistemi       | Kullanılabilirlik        | SKU'lar                                                              |
  |-------------------------|---------------------|-------------------------------------------------------------------|
  | SLES 12 SP2             | Artık teklif edilmedi | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m |
  | SLES 12 SP3             | Kullanılabilir           | S384, S384m, S384xm, S384xxm, S576m, S576xm, S768m, S768xm, S960m |
  
## <a name="related-documents"></a>İlgili Belgeler

- [Mevcut SK'lar](hana-available-skus.md) hakkında daha fazla şey öğrenmek için
- [İşletim Sistemini Yükseltme](os-upgrade-hana-large-instance.md) hakkında bilmek için
  

  
  

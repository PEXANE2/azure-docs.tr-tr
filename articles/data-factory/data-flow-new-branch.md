---
title: Veri akışını eşlemede birden çok dal
description: Birden çok dalla veri akışını eşlemede veri akışlarını çoğaltma
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 01/08/2020
ms.openlocfilehash: 71fb9f1ba9952be0e6b3910dd1079aa6d3c0482d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75834478"
---
# <a name="creating-a-new-branch-in-mapping-data-flow"></a>Veri akışını eşlemede yeni bir dal oluşturma

Aynı veri akışına karşı birden çok işlem ve dönüşüm kümesi yapmak için yeni bir dal ekleyin. Aynı kaynağı birden çok lavabo için veya kendi kendine biraraya gelen verileri bir araya getirmek için kullanmak istediğinizde yeni bir dal eklemek yararlıdır.

Dönüşüm listesinden diğer dönüşümlere benzer yeni bir dal eklenebilir. **Yeni Şube,** yalnızca dallandırmaya çalıştığınız dönüşümü izleyen varolan bir dönüşüm olduğunda eylem olarak kullanılabilir.

![Yeni bir dal ekleme](media/data-flow/new-branch2.png "Yeni bir dal ekleme")

Aşağıdaki örnekte, veri akışı taksi yolculuğu verilerini okuyor. Hem gün hem de satıcı tarafından toplanan çıktı gereklidir. Aynı kaynaktan okunan iki ayrı veri akışı oluşturmak yerine, yeni bir dal eklenebilir. Bu şekilde her iki toplama da aynı veri akışının bir parçası olarak yürütülebilir. 

![Yeni bir dal ekleme](media/data-flow/new-branch.png "Yeni bir dal ekleme")

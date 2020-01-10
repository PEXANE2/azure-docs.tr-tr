---
title: Eşleme veri akışında birden çok dal
description: Birden çok Dalla eşleme veri akışında veri akışlarını çoğaltma
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 01/08/2020
ms.openlocfilehash: 71fb9f1ba9952be0e6b3910dd1079aa6d3c0482d
ms.sourcegitcommit: f53cd24ca41e878b411d7787bd8aa911da4bc4ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75834478"
---
# <a name="creating-a-new-branch-in-mapping-data-flow"></a>Eşleme veri akışı 'nda yeni bir dal oluşturma

Aynı veri akışına karşı birden çok işlem ve dönüşüm kümesi yapmak için yeni bir dal ekleyin. Yeni bir dal eklemek, birden çok havuz için aynı kaynağı kullanmak veya bir arada verilerin birlikte katılmasını sağlamak istediğinizde faydalıdır.

Dönüştürme listesinden diğer dönüştürmelere benzer yeni bir dal eklenebilir. **Yeni dal** yalnızca, dala çalıştığınız dönüşümden sonra var olan bir dönüşüm olduğunda bir eylem olarak kullanılabilir olacaktır.

![Yeni dal ekleme](media/data-flow/new-branch2.png "Yeni dal ekleme")

Aşağıdaki örnekte veri akışı, TAXI seyahat verilerini okuyor. Hem gün hem de satıcı tarafından toplanan çıkış gereklidir. Aynı kaynaktan okunan iki ayrı veri akışı oluşturmak yerine yeni bir dal eklenebilir. Bu şekilde, her iki toplamaları de aynı veri akışının bir parçası olarak yürütülebilir. 

![Yeni dal ekleme](media/data-flow/new-branch.png "Yeni dal ekleme")

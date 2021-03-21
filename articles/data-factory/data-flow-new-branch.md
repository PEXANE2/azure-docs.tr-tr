---
title: Eşleme veri akışında birden çok dal
description: Birden çok Dalla eşleme veri akışında veri akışlarını çoğaltma
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 01/08/2020
ms.openlocfilehash: a11dbfbd6d6510b5c421e54cd2547c3aedb1bfb6
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100378205"
---
# <a name="creating-a-new-branch-in-mapping-data-flow"></a>Eşleme veri akışı 'nda yeni bir dal oluşturma

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Aynı veri akışına karşı birden çok işlem ve dönüşüm kümesi yapmak için yeni bir dal ekleyin. Yeni bir dal eklemek, birden çok havuz için aynı kaynağı kullanmak veya bir arada verilerin birlikte katılmasını sağlamak istediğinizde faydalıdır.

Dönüştürme listesinden diğer dönüştürmelere benzer yeni bir dal eklenebilir. **Yeni dal** yalnızca, dala çalıştığınız dönüşümden sonra var olan bir dönüşüm olduğunda bir eylem olarak kullanılabilir olacaktır.

![Ekran görüntüsü birden çok giriş/çıkış menüsünde yeni dal seçeneğini gösterir.](media/data-flow/new-branch2.png "Yeni dal ekleme")

Aşağıdaki örnekte veri akışı, TAXI seyahat verilerini okuyor. Hem gün hem de satıcı tarafından toplanan çıkış gereklidir. Aynı kaynaktan okunan iki ayrı veri akışı oluşturmak yerine yeni bir dal eklenebilir. Bu şekilde, her iki toplamaları de aynı veri akışının bir parçası olarak yürütülebilir. 

![Ekran görüntüsü, kaynaktan iki Dalla veri akışını gösterir.](media/data-flow/new-branch.png "Yeni dal ekleme")

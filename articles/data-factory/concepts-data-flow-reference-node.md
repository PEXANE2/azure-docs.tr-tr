---
title: Azure Data Factory eşleme veri akışı başvuru düğümü
description: Data Factory veri akışı, birleştirmeler, aramalar, birleşimler için bir başvuru düğümü ekler
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 4ed17114cc0ce586c68c5b3e087acffdb82ea96c
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72030498"
---
# <a name="mapping-data-flow-reference-node"></a>Veri akışı başvuru düğümünü eşleme



![Başvuru düğümü](media/data-flow/referencenode.png "başvuru düğümü")

Eklenmiş olan düğümün tuvalde varolan başka bir düğüme başvurduğundan emin olmak için bir başvuru düğümü otomatik olarak tuvale eklenir. Bir başvuru düğümünü işaretçi olarak veya başka bir veri akışı dönüştürmesi için bir başvuru olarak düşünün.

Örneğin: birden fazla veri akışı birleştirdiğinizde veya Toplandığınızda, veri akışı tuvali, birincil olmayan gelen akışın adını ve ayarlarını yansıtan bir başvuru düğümü ekleyebilir.

Başvuru düğümü taşınamaz veya silinemez. Ancak, kaynak dönüştürme ayarlarını değiştirmek için düğüme tıklayabilirsiniz.

Veri akışı, başvuru düğümünü eklediğinde görüntülenen kullanıcı arabirimi kuralları, kullanılabilir alanı ve satırlar arasındaki dikey boşluğu temel alır.

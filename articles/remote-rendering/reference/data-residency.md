---
title: Veri yerleşimi
description: Azure uzaktan Işleme kullanılırken veri fazlalığını açıklar.
author: rapete
ms.author: rapete
ms.date: 02/04/2021
ms.topic: reference
ms.openlocfilehash: f20b3bb3de877ac627f5f6909c98cb9e1553f2a9
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99576926"
---
# <a name="azure-remote-rendering-data-residency"></a>Azure uzaktan Işleme veri yerleşimi 
Bu makalede, veri yerleşimi kavramı ve Azure uzaktan Işleme için nasıl uygulandığı açıklanmaktadır. 

## <a name="data-residency"></a>Veri yerleşimi 
Azure uzaktan Işleme, model depolaması için Kullanıcı tarafından sağlanmış Azure Blob kapsayıcıları kullanır. Model kullanımda olmadığında Kullanıcı tarafından sağlanmış Azure Blob depolama bölgesinde kalır. Model işleme için kullanıldığında, veriler, kullanıcının işleme oturumunu başlatırken seçtiği bölgeye kopyalanır.

## <a name="next-steps"></a>Sonraki adımlar
Azure uzaktan Işleme için bir Azure Blob depolama kapsayıcısı ayarlamayı öğrenmek isterseniz, [bir modeli işleme Için dönüştürmeyi](../quickstarts/convert-model.md)inceleyin.

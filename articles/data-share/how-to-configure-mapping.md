---
title: Azure veri paylaşımında önizleme sürümünde bir veri kümesi eşlemesi yapılandırma
description: Azure veri paylaşma önizlemesi 'ni kullanarak alınan bir paylaşımın veri kümesi eşlemesini nasıl yapılandıracağınızı öğrenin.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 035235116240bdc6de3bc689c2430fee018b202d
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72169145"
---
# <a name="how-to-configure-a-dataset-mapping-for-a-received-share-in-azure-data-share-preview"></a>Azure veri paylaşımında önizleme sürümünde alınan bir paylaşıma yönelik veri kümesi eşlemesi yapılandırma

Bu makalede, Azure veri paylaşımının önizlemesi kullanılarak alınan bir paylaşımın veri kümesi eşlemesinin nasıl yapılandırılacağı açıklanır. Bir veri paylaşma davetini kabul ettiyseniz ancak "daha sonra kabul et ve Yapılandır" seçeneğini tercih ettiyseniz bunu yapmak isteyeceksiniz. Aksi takdirde, yalnızca alınan verileriniz için hedef depolama hesabını değiştirmek isteyebilirsiniz. 

## <a name="navigate-to-a-received-data-share"></a>Alınan bir veri paylaşımında gezin

Azure veri paylaşma hizmetinde, aldığınız paylaşıma gidin ve **Ayrıntılar** sekmesini seçin. 

![Veri kümesi eşleme](./media/dataset-mapping.png "veri kümesi") eşleme 

Hedef atamak istediğiniz veri kümesinin yanındaki kutuyu işaretleyin ve **+ hedefe eşle**' ye tıklayın. Zaten bir hedef depolama hesabı yapılandırdıysanız ve eşlemeyi farklı bir depolama hesabıyla değiştirmek istiyorsanız, öncelikle eşlemesini kaldırmanız gerekebilir. 

Hedef(./media/dataset-map-target.png "haritaya hedefle") ![eşle] 

## <a name="select-a-new-storage-account"></a>Yeni bir depolama hesabı seçin 

Verilerin içinde olmasını istediğiniz bir depolama hesabı seçin. Önceden eşlenmiş depolama hesaplarında bulunan tüm verilerin otomatik olarak yeni depolama hesabına taşınacağını unutmayın.

![Hedef depolama hesabı](./media/map-target.png "hedef depolama alanı") 

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin.




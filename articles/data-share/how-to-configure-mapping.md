---
title: Azure veri paylaşımında bir veri kümesi eşlemesi yapılandırma
description: Azure veri paylaşımının kullanıldığı bir alınan paylaşımda veri kümesi eşlemesinin nasıl yapılandırılacağını öğrenin.
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 08/14/2020
ms.openlocfilehash: eed3e8275400a3e677df53b9d62cf0e0bc70271c
ms.sourcegitcommit: ef055468d1cb0de4433e1403d6617fede7f5d00e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2020
ms.locfileid: "88257834"
---
# <a name="how-to-configure-a-dataset-mapping-for-a-received-share-in-azure-data-share"></a>Azure veri paylaşımında alınan bir paylaşıma yönelik veri kümesi eşlemesi yapılandırma

Bu makalede, Azure veri paylaşımının kullanıldığı bir alınan paylaşımda veri kümesi eşlemesinin nasıl yapılandırılacağı açıklanır. Veri almak istediğiniz bir hedef veri deposunu belirtmek için veya hedef veri deposunu değiştirmeniz gerekiyorsa bir veri kümesi eşlemesi yapılandırmak isteyebilirsiniz.

## <a name="navigate-to-a-received-data-share"></a>Alınan bir veri paylaşımında gezin

Azure veri paylaşma hizmetinde, aldığınız paylaşıma gidin ve **veri kümeleri** sekmesini seçin. 

![Veri kümesi eşleme](./media/dataset-mapping.png "Veri kümesi eşleme") 

Hedef atamak istediğiniz veri kümesinin yanındaki kutuyu işaretleyin. Yeni bir hedef depo seçmek için **+ hedefe eşle** ' yi seçin. Veri kümesi zaten eşlenmişse ve hedef veri deposunu değiştirmek istiyorsanız, önce **eşlemeyi kaldır** ' ı seçin.

![Hedefle eşle](./media/dataset-map-target.png "Hedefle eşle") 

## <a name="select-a-target-store"></a>Hedef depo seçin

Verilerin içinde olmasını istediğiniz hedef veri deposu türünü seçin. Anlık görüntü tabanlı paylaşım için, önceden eşlenmiş herhangi bir depolama hesabında zaten bulunan tüm veriler yeni hedef depoya otomatik olarak taşınmaz. Yerinde paylaşım için, belirtilen konumda bir veri deposu seçin. Konum, veri sağlayıcısının kaynak veri deposunun bulunduğu Azure veri merkezindedir.

![Hedef depolama hesabı](./media/dataset-map-target-sql.png "Hedef depolama") 

## <a name="select-a-file-format-sql-sources-only"></a>Dosya biçimi seçin (yalnızca SQL kaynakları)

Kaynak veriler SQL tabanlı bir kaynaktan ise ve dosyayı bir dosya olarak almak istiyorsanız, hangi biçimin alındığını seçebilirsiniz. 

![Biçim seçin](./media/sql-file-formats.png "SQL dosya biçimleri")

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin.




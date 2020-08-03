---
title: Azure veri paylaşımında bir veri kümesi eşlemesi yapılandırma
description: Azure veri paylaşımının kullanıldığı bir alınan paylaşımda veri kümesi eşlemesinin nasıl yapılandırılacağını öğrenin.
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 07/30/2020
ms.openlocfilehash: 82de05e7169c5803dd999521f61a33b9dd17b567
ms.sourcegitcommit: 29400316f0c221a43aff3962d591629f0757e780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2020
ms.locfileid: "87511964"
---
# <a name="how-to-configure-a-dataset-mapping-for-a-received-share-in-azure-data-share"></a>Azure veri paylaşımında alınan bir paylaşıma yönelik veri kümesi eşlemesi yapılandırma

Bu makalede, Azure veri paylaşımının kullanıldığı bir alınan paylaşımda veri kümesi eşlemesinin nasıl yapılandırılacağı açıklanır. Bir veri paylaşım davetini kabul ettiyseniz ancak "daha sonra kabul et ve Yapılandır" ya da veriler yerinde paylaşılmışsa bunu yapmak isteyeceksiniz. Sizinle paylaşılan verilerin hedefini değiştirmeniz veya bir SQL Server veri almak istiyorsanız, bir veri kümesi eşlemesi yapılandırmak isteyebilirsiniz. 

## <a name="navigate-to-a-received-data-share"></a>Alınan bir veri paylaşımında gezin

Azure veri paylaşma hizmetinde, aldığınız paylaşıma gidin ve **Ayrıntılar** sekmesini seçin. 

![Veri kümesi eşleme](./media/dataset-mapping.png "Veri kümesi eşleme") 

Hedef atamak istediğiniz veri kümesinin yanındaki kutuyu işaretleyin. Mevcut eşlemenin eşlemesini kaldırmak için **eşlemeyi kaldır** ' ı seçin. Yeni bir hedef depo seçmek için **+ hedefe eşle** ' yi seçin. 

![Hedefle eşle](./media/dataset-map-target.png "Hedefle eşle") 

## <a name="select-a-new-target-store"></a>Yeni bir hedef depo seçin

Verilerin içinde olmasını istediğiniz bir hedef veri türü seçin. Anlık görüntü tabanlı paylaşım için, önceden eşlenmiş herhangi bir depolama hesabında zaten bulunan tüm veriler yeni hedef depoya otomatik olarak taşınmaz. Yerinde paylaşım için, belirtilen konumda bir veri deposu seçin. Konum, veri sağlayıcısının kaynak veri deposunun bulunduğu Azure veri merkezindedir.

![Hedef depolama hesabı](./media/dataset-map-target-sql.png "Hedef depolama") 

## <a name="select-a-file-format-sql-sources-only"></a>Dosya biçimi seçin (yalnızca SQL kaynakları)

Kaynak veriler SQL tabanlı bir kaynaktan ise, hangi biçimin alındığını seçebilirsiniz. 

![Biçim seçin](./media/sql-file-formats.png "SQL dosya biçimleri")

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin.




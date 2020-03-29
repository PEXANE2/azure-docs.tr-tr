---
title: Azure Veri Paylaşımı'nda veri kümesi eşleme yapılandırma
description: Azure Veri Paylaşımı'nı kullanarak alınan bir paylaşım için veri kümesi eşlemesini nasıl yapılandırabilirsiniz öğrenin.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 1ff432c2073536448c8194ebe537c8bf8cf00663
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76964252"
---
# <a name="how-to-configure-a-dataset-mapping-for-a-received-share-in-azure-data-share"></a>Azure Veri Paylaşımı'nda alınan bir paylaşım için veri kümesi eşlemesini yapılandırma

Bu makalede, Azure Veri Paylaşımı'nı kullanarak Alınan Paylaşım için veri kümesi eşlemenin nasıl yapılandırılabildiğini açıklanmaktadır. Bir veri paylaşımı davetini kabul ettiyseniz ancak "Daha sonra kabul edin ve yapılandırın" sözcülüğü seçtiyseniz veya veriler yerinde paylaşılırsa bunu yapmak isteyebilirsiniz. Sizinle paylaşılan veri hedefini değiştirmeniz gerekiyorsa veya bir SQL Server'a veri almak istiyorsanız, bir veri kümesi eşlemi yapılandırmak isteyebilirsiniz. 

## <a name="navigate-to-a-received-data-share"></a>Alınan veri paylaşımına gidin

Azure Veri Paylaşımı hizmetinde, aldığınız paylaşıma gidin ve **Ayrıntılar** sekmesini seçin. 

![Dataset eşleme](./media/dataset-mapping.png "Dataset eşleme") 

Hedef atamak istediğiniz veri kümesinin yanındaki kutuyu işaretleyin. Varolan eşlemenin eşleminin eşleminin haritasını çıkarmak için **Harita Yı** seçin. Yeni bir hedef mağaza seçmek **için hedeflemek için + Harita'yı** seçin. 

![Hedefe harita](./media/dataset-map-target.png "Hedefe harita") 

## <a name="select-a-new-target-store"></a>Yeni bir hedef mağaza seçin

Verilerin inmesini istediğiniz bir hedef veri türü seçin. Anlık görüntü tabanlı paylaşım için, önceden eşlenen depolama hesaplarında zaten var olan veriler otomatik olarak yeni hedef depoya taşınmaz. Yerinde paylaşım için, belirtilen Konum'da bir veri deposu seçin. Konum, veri sağlayıcısının kaynak veri deposunun bulunduğu Azure veri merkezidir.

![Hedef depolama hesabı](./media/dataset-map-target-sql.png "Hedef depolama") 

## <a name="select-a-file-format-sql-sources-only"></a>Dosya biçimi seçin (yalnızca SQL kaynakları)

Kaynak veriler SQL tabanlı bir kaynaktan geliyorsa, hangi biçimde alındığını seçebilirsiniz. 

![Biçim seçin](./media/sql-file-formats.png "SQL dosya biçimleri")

## <a name="next-steps"></a>Sonraki adımlar

Veri paylaşmaya nasıl başlayacağınızı öğrenmek [için, veri öğreticinizi paylaşmaya devam edin.](share-your-data.md)




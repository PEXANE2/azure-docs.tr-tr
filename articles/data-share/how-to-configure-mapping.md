---
title: Azure veri paylaşımında bir veri kümesi eşlemesi yapılandırma
description: Azure veri paylaşımının kullanıldığı bir alınan paylaşımda veri kümesi eşlemesinin nasıl yapılandırılacağını öğrenin.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 1ff432c2073536448c8194ebe537c8bf8cf00663
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76964252"
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




---
title: Varolan bir Azure Veri Paylaşımı'na veri kümeleri ekleme
description: Azure Veri Paylaşımı'nda varolan bir veri paylaşımına veri kümeleri eklemeyi ve aynı alıcılarla nasıl paylaşışta bulunun.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 00c96950565b077e65f84e2d8b4977092df5e317
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73490552"
---
# <a name="how-to-add-datasets-to-an-existing-share-in-azure-data-share"></a>Azure Veri Paylaşımı'ndaki varolan bir paylaşıma veri kümeleri ekleme

Bu makalede, Azure Veri Paylaşımı'nı kullanarak önceden varolan bir veri paylaşımına nasıl veri kümeleri ekleyeceğiniz açıklanmaktadır. Bu, yeni bir paylaşım oluşturmak zorunda kalmadan aynı alıcılarla daha fazla veri paylaşmanızı sağlar.

Paylaşım oluştururken veri kümelerinin nasıl eklenöğretilen hakkında bilgi [için, Veri Paylaş](share-your-data.md) öğreticisine bakın.

## <a name="navigate-to-a-sent-data-share"></a>Gönderilen veri paylaşımına gidin

Azure Veri Paylaşımı'nda, gönderdiğiniz paylaşıma gidin ve **Veri Kümeleri** sekmesini seçin. Daha fazla veri kümesi eklemek için **Veri Setleri** Ekle düğmesini tıklatın.

![Veri Setleri Ekle](./media/how-to/how-to-add-datasets/add-datasets.png)

Sağdaki panelde, eklemek istediğiniz veri kümesi türünü seçin ve sonra **İleri'yi**tıklatın. Eklemek istediğiniz verilerin abonelik ve kaynak grubunu seçin. Açılan okları kullanarak, eklemek için verilerin yanındaki kutuyu bulun ve onaylayın.

![Veri Setleri Ekle](./media/how-to/how-to-add-datasets/add-datasets-side.png)

**Veri Seti Ekle'yi**tıklattığınızda, veri kümeleri paylaşımınıza eklenir. Not: Yeni veri kümelerini görebilmeleri için tüketicileriniz tarafından anlık görüntü tetiklenmelidir. Yapılandırılmış anlık görüntü ayarları varsa, tüketiciler bir sonraki zamanlanmış anlık görüntü tamamlandığında yeni veri kümelerini görür. Anlık görüntü ayarları yapılandırılmadan, tüketicinin güncelleştirmeleri almak için verilerin tam veya artımlı bir kopyasını el ile tetiklemesi gerekir. Anlık görüntüler hakkında daha fazla bilgi için [Anlık Görüntüler'e](terminology.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Varolan bir veri paylaşımına alıcı ekleme](how-to-add-recipients.md)hakkında daha fazla bilgi edinin.
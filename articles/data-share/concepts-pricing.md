---
title: Azure veri paylaşma fiyatlandırmasını anlama
description: Azure veri paylaşma fiyatlandırmasının nasıl çalıştığını öğrenin
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: conceptual
ms.date: 08/11/2020
ms.openlocfilehash: 1c2c58e206a70a3801c712df3b5582409712d3c8
ms.sourcegitcommit: 1aef4235aec3fd326ded18df7fdb750883809ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88137040"
---
# <a name="understand-azure-data-share-pricing"></a>Azure veri paylaşma fiyatlandırmasını anlama

Anlık görüntü tabanlı paylaşım için, veri kümesi anlık görüntüsü ve anlık görüntü yürütme için Azure veri paylaşımı ücretleri. Bu makalede, anlık görüntü geçmişi bilgilerini kullanarak veri kümesi anlık görüntüsünün ve anlık görüntü yürütmenin nasıl tahmin edilmesi açıklanmaktadır. Şu anda veri sağlayıcı, veri kümesi anlık görüntüsü ve anlık görüntü yürütme için faturalandırılır.

## <a name="dataset-snapshot"></a>Veri kümesi anlık görüntüsü

Veri kümesi anlık görüntüsü, veri kümesini kaynağından hedefe taşıma işlemidir. Bir paylaşımda anlık görüntü çekilirken, paylaşımdaki her veri kümesinin anlık görüntüsü bir veri kümesi anlık görüntüsü işlemidir. Veri kümesi anlık görüntülerinin listesini görüntülemek için aşağıdaki adımları izleyin. 

1. Azure portal, veri paylaşımının kaynağına gidin.

1. Gönderilen paylaşımdan veya alınan paylaşımdan bir paylaşma seçin.

1. **Geçmiş** sekmesine tıklayın.

1. Bir anlık görüntünün başlangıç saati ' ne tıklayın.
 
    ![Anlık görüntü geçmişi](./media/concepts/concepts-pricing/pricing-snapshot-history.png "Anlık görüntü geçmişi") 

1. Veri kümesi anlık görüntü işlemlerinin listesini görüntüleyin. Her satır öğesi bir veri kümesi anlık görüntüsü işlemine karşılık gelir. Bu örnekte, iki veri kümesi anlık görüntüsü vardır.

    ![DataSet anlık görüntü işlemi](./media/concepts/concepts-pricing/pricing-dataset-snapshot.png "DataSet anlık görüntü işlemi")

## <a name="snapshot-execution"></a>Anlık görüntü yürütme

Anlık görüntü yürütme, bir veri kümesini kaynaktan hedefe taşımak için gereken kaynakları içerir. Her veri kümesi anlık görüntü işlemi için anlık görüntü yürütme, anlık görüntü süresiyle çarpılan sanal çekirdek sayısı olarak hesaplanır. Ücretler dakikaya eşit olarak dağıtılır ve yuvarlanır. Sanal çekirdek sayısı, kaynak hedef çifti ve veri düzenine göre dinamik olarak seçilir. Bir veri kümesi anlık görüntüsü işlemi için kullanılan anlık görüntü başlangıç saati, bitiş saati ve sanal çekirdekleri görüntülemek için aşağıdaki adımları izleyin.

1. Azure portal, veri paylaşımının kaynağına gidin.

1. Gönderilen paylaşımdan veya alınan paylaşımdan bir paylaşma seçin.

1. **Geçmiş** sekmesine tıklayın.

1. Bir anlık görüntünün başlangıç saati ' ne tıklayın.

    ![Anlık görüntü geçmişi](./media/concepts/concepts-pricing/pricing-snapshot-history.png "Anlık görüntü geçmişi") 

1. Veri kümesi anlık görüntü işleminin durumuna tıklayın.

    ![Veri kümesi anlık görüntüsü durumu](./media/concepts/concepts-pricing/pricing-snapshot-status.png "Veri kümesi anlık görüntüsü durumu")

1. Bu veri kümesi anlık görüntüsü işlemi için kullanılan başlangıç saatini, bitiş saatini ve sanal çekirdekleri görüntüleyin. 

    ![Anlık görüntü yürütme](./media/concepts/concepts-pricing/pricing-snapshot-execution.png "Anlık görüntü yürütme")

## <a name="next-steps"></a>Sonraki adımlar

- En son fiyatlandırma bilgilerini al- [Azure veri paylaşımının fiyatlandırması](https://azure.microsoft.com/pricing/details/data-share/)
- Azure Fiyatlandırma Hesaplayıcı kullanarak maliyeti tahmin edin- [Azure Fiyatlandırma Hesaplayıcı](https://azure.microsoft.com/pricing/calculator/)

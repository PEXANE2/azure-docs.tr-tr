---
title: Azure Stream Analytics kümesinde iş oluşturma ve silme
description: Bir Azure Stream Analytics kümesindeki Stream Analytics işlerin nasıl yönetileceğini öğrenin
author: sidramadoss
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: overview
ms.custom: mvc
ms.date: 09/22/2020
ms.openlocfilehash: 6b475e34c01ca26abd3d8ab1d904521de19f941b
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90947875"
---
# <a name="create-and-delete-jobs-in-an-azure-stream-analytics-cluster"></a>Azure Stream Analytics kümesinde iş oluşturma ve silme

Birden çok Azure Stream Analytics işini bir Stream Analytics kümesinde çalıştırabilirsiniz. Bir kümede çalışan işleri basit bir 2 adımlı işlemdir: işi kümeye ekleyin ve işi başlatın. Bu makalede, var olan bir kümeden iş ekleme ve kaldırma işlemleri gösterilir. Henüz bir tane yoksa [Stream Analytics kümesi oluşturmak](create-cluster.md) için hızlı başlangıcı izleyin.

## <a name="add-a-stream-analytics-job-to-a-cluster"></a>Kümeye Stream Analytics işi ekleme

Kümelere yalnızca mevcut Stream Analytics işleri eklenebilir. Azure portal kullanarak [iş oluşturmayı öğrenmek](stream-analytics-quick-create-portal.md) için hızlı başlangıcı izleyin. Bir kümeye eklemek istediğiniz bir işiniz olduktan sonra, işi kümenize eklemek için aşağıdaki adımları kullanın.

1. Azure portal Stream Analytics kümenizi bulun ve seçin.

1. **Ayarlar**altında **Stream Analytics işler**' i seçin. Ardından, **mevcut Işi Ekle**' yi seçin.

1. Kümeye eklemek istediğiniz aboneliği ve Stream Analytics işi seçin. Kümeye yalnızca kümeyle aynı bölgedeki işleri Stream Analytics eklenebilir.

   ![Kümeye iş Ekle](./media/manage-jobs-cluster/add-job.png)

1. İşi kümeye ekledikten sonra iş kaynağına gidin ve [işi başlatın](start-job.md#azure-portal). Daha sonra iş kümenizdeki çalışmaya başlar.

İzleme, uyarı ve tanılama günlükleri gibi diğer tüm işlemleri, Stream Analytics işi kaynağı sayfasından gerçekleştirebilirsiniz.

## <a name="remove-a-stream-analytics-job-from-a-cluster"></a>Stream Analytics işini kümeden kaldırma

Stream Analytics işlerin kümeden kaldırılabilmesi için önce durdurulmuş durumda olması gerekir. İşiniz hala çalışıyorsa, aşağıdaki adımlara geçmeden önce işi durdurun.

1. Stream Analytics kümenizi bulun ve seçin.

1. **Ayarlar**altında **Stream Analytics işler**' i seçin.

1. Kümeden kaldırmak istediğiniz işleri seçin ve ardından **Kaldır**' ı seçin.

   ![işi kümeden kaldır](./media/manage-jobs-cluster/remove-job.png)

   Bir iş Stream Analytics kümeden kaldırıldığında standart çok kiracılı ortama döner.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Stream Analytics kümenize iş eklemeyi ve kaldırmayı öğrenirsiniz. Ardından, Özel uç noktaları yönetmeyi ve kümelerinizi ölçeklendirmeyi öğrenebilirsiniz:

* [Azure Stream Analytics kümesini ölçeklendirme](scale-cluster.md)
* [Azure Stream Analytics kümesinde özel uç noktaları yönetme](private-endpoints.md)
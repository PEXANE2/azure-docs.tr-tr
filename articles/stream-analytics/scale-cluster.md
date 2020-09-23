---
title: Azure Stream Analytics kümesini yeniden boyutlandırma
description: Azure Stream Analytics kümenin boyutunun ölçeğini artırma ve azaltma hakkında bilgi edinin.
author: sidramadoss
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: overview
ms.custom: mvc
ms.date: 09/22/2020
ms.openlocfilehash: 0763e56de6c72a36d39b17d153db6fc4d7dd821a
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948408"
---
# <a name="resize-an-azure-stream-analytics-cluster"></a>Azure Stream Analytics kümesini yeniden boyutlandırma

Stream Analytics kümenin kapasitesi, akış birimleri (SU) cinsinden ölçülür. Çalışan tüm işlere atanan SUs toplamı kümenin kapasitesini aşmadığı sürece birden çok iş paralel olarak aynı kümede çalışabilir.

Küme kapasitesi, akış iş yüklerinizin boyutuyla eşleşecek şekilde yukarı ve aşağı ölçeklendirilebilir. Bir kümenin ölçeklendirilmesi zaman alır ve sık ölçekleme amacını taşımaktadır. Kullanmasını planladığınız doğru sayıda SUs ile bir küme planlayıp sağlamanızı öneririz.

## <a name="change-the-scale-of-your-cluster"></a>Kümenizin ölçeğini değiştirme

1. Azure portal Stream Analytics kümenizi bulun ve seçin.

1. **Genel bakış** bölümünde **Ölçek**' i seçin. Kümenize kaç tane SUs atandığını görebilirsiniz. Gerektiğinde, SUs 'yi artırmak veya azaltmak için seçiciyi kullanın.

   ![kümeyi Ölçeklendir](./media/scale-cluster/scale-cluster.png)

Ölçeklendirme işlemi şu anda çalışmakta olan işleri etkilemez.

## <a name="next-steps"></a>Sonraki adımlar

Artık Stream Analytics kümelerinizin ölçeğini artırma ve azaltma hakkında bilgi edinirsiniz. Ardından, Özel uç noktaları yönetme ve işlerinizi otomatik ölçeklendirme hakkında bilgi edinebilirsiniz:

* [Azure Stream Analytics kümesinde özel uç noktaları yönetme](private-endpoints.md)
* [Azure Stream Analytics kümesinde işleri yönetme](manage-jobs-cluster.md)

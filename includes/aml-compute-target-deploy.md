---
title: include dosyası
description: include dosyası
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 08/23/2019
ms.openlocfilehash: 29da4ab949cf38419bc7a4d0a3b5da6669441887
ms.sourcegitcommit: adc1072b3858b84b2d6e4b639ee803b1dda5336a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70847749"
---
| Hedef işlem | Kullanıldığı yerler | GPU desteği | FPGA desteği | Açıklama |
| ----- | ----- | ----- | ----- | ----- |
| [Yerel&nbsp;Web&nbsp;hizmeti](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Test/hata ayıklama | &nbsp; | &nbsp; | Sınırlı test ve sorun giderme için kullanın. Donanım hızlandırma, yerel sistemdeki kitaplıkların kullanımına bağlıdır.
| [Not defteri&nbsp;VM&nbsp;Web hizmeti](../articles/machine-learning/service/how-to-deploy-and-where.md#notebookvm) | Test/hata ayıklama | &nbsp; | &nbsp; | Sınırlı test ve sorun giderme için kullanın.
| [Azure Kubernetes Service'i (AKS)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Gerçek zamanlı çıkarım |  [Evet](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md) (Web hizmeti dağıtımı) | [Evet](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Yüksek ölçekli üretim dağıtımları için kullanın. Dağıtılan hizmetin hızlı yanıt süresini ve otomatik ölçeklendirilmesini sağlar. Küme otomatik ölçeklendirme, Azure Machine Learning SDK aracılığıyla desteklenmez. AKS kümesindeki düğümleri değiştirmek için Azure portal AKS kümeniz için Kullanıcı arabirimini kullanın. AKS, görsel arabirim için kullanılabilen tek seçenektir. |
| [Azure Container Instances](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Test veya geliştirme | &nbsp;  | &nbsp; | 48 GB 'den az RAM gerektiren düşük ölçekli CPU tabanlı iş yükleri için kullanın. |
| [Azure Machine Learning işlem](../articles/machine-learning/service/how-to-run-batch-predictions.md) | Önizle Toplu&nbsp;çıkarım | [Evet](../articles/machine-learning/service/how-to-run-batch-predictions.md) (Machine Learning işlem hattı) | &nbsp;  | Batch Puanlama 'nı sunucusuz işlem üzerinde çalıştırın. Normal ve düşük öncelikli VM 'Leri destekler. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | Önizle IoT&nbsp;modülü |  &nbsp; | &nbsp; | IoT cihazlarında ML modellerini dağıtın ve sunun. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | IoT Edge aracılığıyla |  &nbsp; | Evet | IoT cihazlarında ML modellerini dağıtın ve sunun. |

> [!NOTE]
> Yerel, Notebook VM gibi işlem hedefleri ve eğitim ve deneme için Azure Machine Learning Işlem desteği GPU 'SU desteğini, __bir Web hizmeti olarak dağıtıldığında__ , yalnızca Azure Kubernetes hizmetinde desteklense de çıkarım için GPU kullanımı desteklenir.
>
> __Makine öğrenimi ardışık düzenine sahip Puanlama__ yalnızca Azure Machine Learning işlem sırasında desteklenirken çıkarımı için GPU kullanımı desteklenir.
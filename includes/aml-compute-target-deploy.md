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
ms.openlocfilehash: ba1e0eee5de65364d8e5e762195f7de98384c2a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76122601"
---
| İşlem hedefi | Kullanıldığı yerler | GPU desteği | FPGA desteği | Açıklama |
| ----- | ----- | ----- | ----- | ----- |
| [Yerel&nbsp;&nbsp;web hizmeti](../articles/machine-learning/how-to-deploy-and-where.md#local) | Test etme/hata ayıklama | &nbsp; | &nbsp; | Sınırlı sınama ve sorun giderme için kullanın. Donanım hızlandırması yerel sistemdeki kitaplıkların kullanımına bağlıdır.
| [Azure Machine Learning bilgi&nbsp;&nbsp;işlem örneği web hizmeti](../articles/machine-learning/how-to-deploy-and-where.md#notebookvm) | Test etme/hata ayıklama | &nbsp; | &nbsp; | Sınırlı sınama ve sorun giderme için kullanın.
| [Azure Kubernetes Hizmeti (AKS)](../articles/machine-learning/how-to-deploy-and-where.md#aks) | Gerçek zamanlı çıkarım |  [Evet](../articles/machine-learning/how-to-deploy-inferencing-gpus.md) (web hizmeti dağıtımı) | [Evet](../articles/machine-learning/how-to-deploy-fpga-web-service.md)   |Yüksek ölçekli üretim dağıtımları için kullanın. Hızlı yanıt süresi ve dağıtılan hizmetin otomatik ölçekleme sağlar. Küme otomatik ölçekleme, Azure Machine Learning SDK aracılığıyla desteklenmez. AKS kümesindeki düğümleri değiştirmek için Azure portalındaki AKS kümeniz için UI'yi kullanın. AKS tasarımcı için tek seçenektir. |
| [Azure Container Instances](../articles/machine-learning/how-to-deploy-and-where.md#aci) | Test veya geliştirme | &nbsp;  | &nbsp; | 48 GB'tan az RAM gerektiren düşük ölçekli CPU tabanlı iş yükleri için kullanın. |
| [Azure Machine Learning işlem kümeleri](../articles/machine-learning/how-to-use-parallel-run-step.md) | (Önizleme) Toplu&nbsp;çıkarım | [Evet](../articles/machine-learning/how-to-use-parallel-run-step.md) (makine öğrenimi boru hattı) | &nbsp;  | Sunucusuz bilgi işlemde toplu puanlama çalıştırın. Normal ve düşük öncelikli VM'leri destekler. |
| [Azure İşlevleri](../articles/machine-learning/how-to-deploy-functions.md) | (Önizleme) Gerçek zamanlı çıkarım | &nbsp; | &nbsp; | &nbsp; |
| [Azure IoT Edge](../articles/machine-learning/how-to-deploy-and-where.md#iotedge) | (Önizleme) IoT&nbsp;modülü |  &nbsp; | &nbsp; | ML modellerini IoT cihazlarına dağıtın ve servis edin. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | Via IoT Edge |  &nbsp; | Evet | ML modellerini IoT cihazlarına dağıtın ve servis edin. |

> [!NOTE]
> Yerel, Azure Machine Learning bilgi işlem örneği ve Azure Machine Learning bilgi işlem kümeleri gibi bilgi işlem hedefleri eğitim ve deneme için GPU'yı desteklese de, __web hizmeti olarak dağıtıldığında__ gpu kullanmak yalnızca Azure Kubernetes Hizmeti'nde desteklenir.
>
> __Makine öğrenimi ardışık boru hattıyla puanlama yaparken__ çıkarım için GPU kullanmak yalnızca Azure Machine Learning Compute'da desteklenir.
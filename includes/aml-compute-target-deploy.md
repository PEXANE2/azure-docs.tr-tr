---
title: dosya dahil etme
description: dosya dahil etme
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 06/25/2020
ms.openlocfilehash: bd3ac8d512c1b9d151c0dc549ffeee6a05c7f94b
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87542832"
---
Modelinizi barındırmak için kullandığınız işlem hedefi, dağıtılan uç noktanızın maliyetini ve kullanılabilirliğini etkiler. Uygun bir işlem hedefi seçmek için aşağıdaki tabloyu kullanın.

| İşlem hedefi | Kullanıldığı yerler | GPU desteği | FPGA desteği | Açıklama |
| ----- | ----- | ----- | ----- | ----- |
| [Yerel &nbsp; Web &nbsp; hizmeti](../articles/machine-learning/how-to-deploy-local-container-notebook-vm.md) | Test/hata ayıklama | &nbsp; | &nbsp; | Sınırlı test ve sorun giderme için kullanın. Donanım hızlandırma, yerel sistemdeki kitaplıkların kullanımına bağlıdır.
| [Azure Machine Learning işlem örneği &nbsp; Web &nbsp; hizmeti](../articles/machine-learning/how-to-deploy-local-container-notebook-vm.md) | Test/hata ayıklama | &nbsp; | &nbsp; | Sınırlı test ve sorun giderme için kullanın.
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/how-to-deploy-azure-kubernetes-service.md) | Gerçek zamanlı çıkarım |  [Evet](../articles/machine-learning/how-to-deploy-inferencing-gpus.md) (Web hizmeti dağıtımı) | [Evet](../articles/machine-learning/how-to-deploy-fpga-web-service.md)   |Yüksek ölçekli üretim dağıtımları için kullanın. Dağıtılan hizmetin hızlı yanıt süresini ve otomatik ölçeklendirilmesini sağlar. Küme otomatik ölçeklendirme, Azure Machine Learning SDK aracılığıyla desteklenmez. AKS kümesindeki düğümleri değiştirmek için Azure portal AKS kümeniz için Kullanıcı arabirimini kullanın. AKS, tasarımcı için kullanılabilen tek seçenektir. |
| [Azure Container Instances](../articles/machine-learning/how-to-deploy-azure-container-instance.md) | Test veya geliştirme | &nbsp;  | &nbsp; | 48 GB 'den az RAM gerektiren düşük ölçekli CPU tabanlı iş yükleri için kullanın. |
| [Azure Machine Learning işlem kümeleri](../articles/machine-learning/how-to-use-parallel-run-step.md) | Toplu &nbsp; çıkarım | [Evet](../articles/machine-learning/how-to-use-parallel-run-step.md) (Machine Learning işlem hattı) | &nbsp;  | Batch Puanlama 'nı sunucusuz işlem üzerinde çalıştırın. Normal ve düşük öncelikli VM 'Leri destekler. |
| [Azure İşlevleri](../articles/machine-learning/how-to-deploy-functions.md) | Önizle Gerçek zamanlı çıkarım | &nbsp; | &nbsp; | &nbsp; |
| [Azure IoT Edge](../articles/machine-learning/how-to-deploy-and-where.md#iotedge) | Önizle IoT &nbsp; modülü |  &nbsp; | &nbsp; | IoT cihazlarında ML modellerini dağıtın ve sunun. |
| [Azure Data Box Edge](../articles/databox-online/azure-stack-edge-overview.md)   | IoT Edge aracılığıyla |  &nbsp; | Yes | IoT cihazlarında ML modellerini dağıtın ve sunun. |

> [!NOTE]
> Yerel, Azure Machine Learning işlem örneği gibi işlem hedefleri ve Azure Machine Learning işlem kümeleri eğitim ve deneme için GPU 'YU desteklese de, __Web hizmeti olarak dağıtıldığında__ , yalnızca Azure Kubernetes hizmetinde desteklenir.
>
> __Makine öğrenimi ardışık düzenine sahip Puanlama__ yalnızca Azure Machine Learning işlem sırasında desteklenirken çıkarımı için GPU kullanımı desteklenir.

> [!NOTE]
> * Azure Container Instances (ACI) yalnızca boyutu 1 GB 'tan küçük olan küçük modeller için uygundur. 
> * Daha büyük modellerin geliştirme ve test için tek düğümlü Azure Kubernetes hizmeti (AKS) kümelerini kullanmanızı öneririz.
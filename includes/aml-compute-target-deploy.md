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
ms.openlocfilehash: 944b96e7726f2b2becd5960a17a89c00d00c878a
ms.sourcegitcommit: b87c7796c66ded500df42f707bdccf468519943c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91841977"
---
Modelinizi barındırmak için kullandığınız işlem hedefi, dağıtılan uç noktanızın maliyetini ve kullanılabilirliğini etkiler. Uygun bir işlem hedefi seçmek için bu tabloyu kullanın.

| İşlem hedefi | Kullanıldığı yerler | GPU desteği | FPGA desteği | Açıklama |
| ----- | ----- | ----- | ----- | ----- |
| [Yerel &nbsp; Web &nbsp; hizmeti](../articles/machine-learning/how-to-deploy-local-container-notebook-vm.md) | Test/hata ayıklama | &nbsp; | &nbsp; | Sınırlı test ve sorun giderme için kullanın. Donanım hızlandırma, yerel sistemdeki kitaplıkların kullanımına bağlıdır.
| [Azure Machine Learning işlem örneği &nbsp; Web &nbsp; hizmeti](../articles/machine-learning/how-to-deploy-local-container-notebook-vm.md) | Test/hata ayıklama | &nbsp; | &nbsp; | Sınırlı test ve sorun giderme için kullanın.
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/how-to-deploy-azure-kubernetes-service.md) | Gerçek zamanlı çıkarım |  [Evet](../articles/machine-learning/how-to-deploy-inferencing-gpus.md) (Web hizmeti dağıtımı) | [Evet](../articles/machine-learning/how-to-deploy-fpga-web-service.md)   |Yüksek ölçekli üretim dağıtımları için kullanın. Dağıtılan hizmetin hızlı yanıt süresini ve otomatik ölçeklendirilmesini sağlar. Küme otomatik ölçeklendirme, Azure Machine Learning SDK aracılığıyla desteklenmez. AKS kümesindeki düğümleri değiştirmek için Azure portal AKS kümeniz için Kullanıcı arabirimini kullanın. AKS, tasarımcı için kullanılabilen tek seçenektir. |
| [Azure Container Instances](../articles/machine-learning/how-to-deploy-azure-container-instance.md) | Test veya geliştirme | &nbsp;  | &nbsp; | 48 GB 'den az RAM gerektiren düşük ölçekli CPU tabanlı iş yükleri için kullanın. |
| [Azure Machine Learning işlem kümeleri](../articles/machine-learning/how-to-use-parallel-run-step.md) | Toplu &nbsp; çıkarım | [Evet](../articles/machine-learning/how-to-use-parallel-run-step.md) (Machine Learning işlem hattı) | &nbsp;  | Batch Puanlama 'nı sunucusuz işlem üzerinde çalıştırın. Normal ve düşük öncelikli VM 'Leri destekler. |
| [Azure İşlevleri](../articles/machine-learning/how-to-deploy-functions.md) | Önizle Gerçek zamanlı çıkarım | &nbsp; | &nbsp; | &nbsp; |
| [Azure IoT Edge](../articles/machine-learning/how-to-deploy-and-where.md#iotedge) | Önizle IoT &nbsp; modülü |  &nbsp; | &nbsp; | IoT cihazlarında makine öğrenimi modellerini dağıtın ve sunun. |
| [Azure Data Box Edge](../articles/databox-online/azure-stack-edge-overview.md)   | IoT Edge yoluyla |  &nbsp; | Evet | IoT cihazlarında makine öğrenimi modellerini dağıtın ve sunun. |

> [!NOTE]
> Yerel, Azure Machine Learning işlem ve Azure Machine Learning işlem kümeleri gibi işlem hedefleri eğitim ve deneme için GPU 'YU desteklese de, _bir Web hizmeti olarak dağıtıldığında_ , yalnızca aks üzerinde DESTEKLENMEYE yönelik GPU kullanımı.
>
> _Makine öğrenimi ardışık düzenine sahip Puanlama_ yalnızca Azure Machine Learning işlem sırasında desteklenirken çıkarımı için GPU kullanımı desteklenir.

> [!NOTE]
> * Kapsayıcı örnekleri yalnızca boyutu 1 GB 'tan küçük olan küçük modeller için uygundur.
> * Daha büyük modellerin geliştirme/test için tek düğümlü AKS kümelerini kullanın.
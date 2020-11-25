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
ms.date: 11/02/2020
ms.openlocfilehash: 31d00222da540751a1f95120bea00535b099403d
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96026680"
---
Modelinizi barındırmak için kullandığınız işlem hedefi, dağıtılan uç noktanızın maliyetini ve kullanılabilirliğini etkiler. Uygun bir işlem hedefi seçmek için bu tabloyu kullanın.

| İşlem hedefi | Kullanıldığı yerler | GPU desteği | FPGA desteği | Description |
| ----- | ----- | ----- | ----- | ----- |
| [Yerel &nbsp; Web &nbsp; hizmeti](../articles/machine-learning/how-to-deploy-local-container-notebook-vm.md) | Test/hata ayıklama | &nbsp; | &nbsp; | Sınırlı test ve sorun giderme için kullanın. Donanım hızlandırma, yerel sistemdeki kitaplıkların kullanımına bağlıdır.
| [Azure Kubernetes Service (AKS)](../articles/machine-learning/how-to-deploy-azure-kubernetes-service.md) | Gerçek zamanlı çıkarım |  [Evet](../articles/machine-learning/how-to-deploy-inferencing-gpus.md) (Web hizmeti dağıtımı) | [Evet](../articles/machine-learning/how-to-deploy-fpga-web-service.md)   |Yüksek ölçekli üretim dağıtımları için kullanın. Dağıtılan hizmetin hızlı yanıt süresini ve otomatik ölçeklendirilmesini sağlar. Küme otomatik ölçeklendirme, Azure Machine Learning SDK aracılığıyla desteklenmez. AKS kümesindeki düğümleri değiştirmek için Azure portal AKS kümeniz için Kullanıcı arabirimini kullanın. <br/><br/> Tasarımcıda destekleniyor. |
| [Azure Container Instances](../articles/machine-learning/how-to-deploy-azure-container-instance.md) | Test veya geliştirme | &nbsp;  | &nbsp; | 48 GB 'den az RAM gerektiren düşük ölçekli CPU tabanlı iş yükleri için kullanın. <br/><br/> Tasarımcıda destekleniyor. |
| [Azure Machine Learning işlem kümeleri](../articles/machine-learning/how-to-use-parallel-run-step.md) | Toplu &nbsp; çıkarım | [Evet](../articles/machine-learning/how-to-use-parallel-run-step.md) (Machine Learning işlem hattı) | &nbsp;  | Batch Puanlama 'nı sunucusuz işlem üzerinde çalıştırın. Normal ve düşük öncelikli VM 'Leri destekler. |

> [!NOTE]
> Yerel, Azure Machine Learning işlem ve Azure Machine Learning işlem kümeleri gibi işlem hedefleri eğitim ve deneme için GPU 'YU desteklese de, _bir Web hizmeti olarak dağıtıldığında_ , yalnızca aks üzerinde DESTEKLENMEYE yönelik GPU kullanımı.
>
> _Makine öğrenimi ardışık düzenine sahip Puanlama_ yalnızca Azure Machine Learning işlem sırasında desteklenirken çıkarımı için GPU kullanımı desteklenir.

> [!NOTE]
> * Kapsayıcı örnekleri yalnızca boyutu 1 GB 'tan küçük olan küçük modeller için uygundur.
> * Daha büyük modellerin geliştirme/test için tek düğümlü AKS kümelerini kullanın.
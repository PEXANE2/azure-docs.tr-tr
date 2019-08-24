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
ms.openlocfilehash: e402989c6bdbbc90ae10047fcf19f4f2d74328a6
ms.sourcegitcommit: dcf3e03ef228fcbdaf0c83ae1ec2ba996a4b1892
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70014823"
---
| Hedef işlem | Kullanım | GPU desteği | FPGA desteği | Açıklama |
| ----- | ----- | ----- | ----- | ----- |
| [Yerel&nbsp;Web&nbsp;hizmeti](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Test/hata ayıklama | &nbsp; | &nbsp; | Sınırlı test ve sorun giderme için iyi. Donanım hızlandırma, yerel sistemdeki kitaplıkların kullanılmasına bağlıdır.
| [Not defteri&nbsp;VM&nbsp;Web hizmeti](../articles/machine-learning/service/how-to-deploy-and-where.md#notebookvm) | Test/hata ayıklama | &nbsp; | &nbsp; | Sınırlı test ve sorun giderme için iyi. 
| [Azure Kubernetes Service'i (AKS)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Gerçek zamanlı çıkarım |  [Yes](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md)  | [Yes](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Büyük ölçekli üretim dağıtımları için idealdir. Dağıtılan hizmetin hızlı yanıt süresini ve otomatik ölçeklendirilmesini sağlar. Küme otomatik ölçeklendirme, Azure Machine Learning SDK aracılığıyla desteklenmez. AKS kümesindeki düğümleri değiştirmek için Azure portal AKS kümeniz için Kullanıcı arabirimini kullanın. AKS, görsel arabirim için kullanılabilen tek seçenektir. |
| [Azure Container Instances (ACI)](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Test veya geliştirme | &nbsp;  | &nbsp; | 48 GB RAM < gerektiren düşük ölçekli, CPU tabanlı iş yükleri için iyi |
| [Azure Machine Learning işlem](../articles/machine-learning/service/how-to-run-batch-predictions.md) | Önizle Toplu&nbsp;çıkarım | &nbsp; | &nbsp;  | Batch Puanlama 'nı sunucusuz işlem üzerinde çalıştırın. Normal ve düşük öncelikli VM 'Leri destekler. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | Önizle IoT&nbsp;modülü |  &nbsp; | &nbsp; | Dağıtım & IoT cihazlarında ML modellerini sunar. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | IoT Edge aracılığıyla |  &nbsp; | evet | Dağıtım & IoT cihazlarında ML modellerini sunar. |

> [!NOTE]
> Yerel, Not defteri sanal makinesi ve eğitim ve deneme için Azure Machine Learning Işlem desteği GPU 'SU gibi işlemleri yaparken, çıkarım için GPU kullanımı yalnızca Azure Kubernetes hizmetinde desteklenir.
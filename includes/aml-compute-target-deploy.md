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
ms.date: 05/30/2019
ms.openlocfilehash: c5a6f957cd3f799f9d7eff1df22df2200779ac30
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68946030"
---
| Hedef işlem | Kullanım | GPU desteği | FPGA desteği | Açıklama |
| ----- | ----- | ----- | ----- | ----- |
| [Yerel&nbsp;Web&nbsp;hizmeti](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Test/hata ayıklama | Belki | &nbsp; | Sınırlı test ve sorun giderme için iyi. Donanım hızlandırma, yerel sistemdeki kitaplıkların kullanılmasına bağlıdır.
| [Not defteri&nbsp;VM&nbsp;Web hizmeti](../articles/machine-learning/service/how-to-deploy-and-where.md#notebookvm) | Test/hata ayıklama | Belki | &nbsp; | Sınırlı test ve sorun giderme için iyi. 
| [Azure Kubernetes Service'i (AKS)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Gerçek zamanlı çıkarım |  [Yes](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md)  | [Yes](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Büyük ölçekli üretim dağıtımları için idealdir. Dağıtılan hizmetin hızlı yanıt süresini ve otomatik ölçeklendirilmesini sağlar. Küme otomatik ölçeklendirme, Azure Machine Learning SDK aracılığıyla desteklenmez. AKS kümesindeki düğümleri değiştirmek için Azure portal AKS kümeniz için Kullanıcı arabirimini kullanın. AKS, görsel arabirim için kullanılabilen tek seçenektir. |
| [Azure Container Instances (ACI)](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Test veya geliştirme | &nbsp;  | &nbsp; | 48 GB RAM < gerektiren düşük ölçekli, CPU tabanlı iş yükleri için iyi |
| [Azure Machine Learning işlem](../articles/machine-learning/service/how-to-run-batch-predictions.md) | Önizle Toplu&nbsp;çıkarım | evet | &nbsp;  | Batch Puanlama 'nı sunucusuz işlem üzerinde çalıştırın. Normal ve düşük öncelikli VM 'Leri destekler. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | Önizle IoT&nbsp;modülü |  &nbsp; | &nbsp; | Dağıtım & IoT cihazlarında ML modellerini sunar. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | IoT Edge aracılığıyla |  &nbsp; | evet | Dağıtım & IoT cihazlarında ML modellerini sunar. |

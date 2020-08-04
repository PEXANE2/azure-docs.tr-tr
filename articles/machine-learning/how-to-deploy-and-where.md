---
title: Modellerin nasıl ve nereye dağıtılacağı
titleSuffix: Azure Machine Learning
description: Azure Container Instances, Azure Kubernetes hizmeti, Azure IoT Edge ve alan programlanabilir kapı dizileri dahil olmak üzere Azure Machine Learning modellerinizi nasıl ve nereye dağıtacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: gopalv
author: gvashishtha
ms.reviewer: larryfr
ms.date: 07/08/2020
ms.topic: conceptual
ms.custom: how-to, tracking-python
zone_pivot_groups: aml-control-methods
ms.openlocfilehash: 42205f87e2025fa7f4492cb76aeb44a1fbf46eb6
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87542828"
---
# <a name="deploy-models-with-azure-machine-learning"></a>Azure Machine Learning ile modelleri dağıtma
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Machine Learning modelinizi bir Web hizmeti olarak Azure bulutu 'nda veya Azure IoT Edge cihazlarda dağıtmayı öğrenin.

Modeli nerede dağıtırsanız dağıtın benzer bir iş akışı kullanılır:

1. Modeli kaydedin.
1. Çıkarım yapılandırmasını hazırlama
1. Giriş betiği hazırlama ( [kod içermeyen dağıtım](how-to-deploy-no-code-deployment.md)kullanmadıkça)
1. Modeli işlem hedefine dağıtın.
1. Web hizmeti olarak da bilinen dağıtılan modeli test edin.

Dağıtım iş akışında yer alan kavramlar hakkında daha fazla bilgi için bkz. [Azure Machine Learning modelleri yönetme, dağıtma ve izleme](concept-model-management-and-deployment.md).


 
::: zone pivot="cli"
[!INCLUDE [CLI quickstart](../../includes/machine-learning-how-to-deploy-and-where-cli.md)]
::: zone-end

::: zone pivot="py-sdk"
[!INCLUDE [SDK quickstart](../../includes/machine-learning-how-to-deploy-and-where-sdk.md)]
::: zone-end


### <a name="understanding-service-state"></a>Hizmet durumunu anlama

Model dağıtımı sırasında hizmet durumu değişikliğini tam olarak dağıttığında görebilirsiniz.

Aşağıdaki tabloda farklı hizmet durumları açıklanmaktadır:

| Web hizmeti durumu | Açıklama | Son durum?
| ----- | ----- | ----- |
| Kta | Hizmet, dağıtım sürecinde. | No |
| Uygun Değil | Hizmet dağıtıldı, ancak şu anda ulaşılamaz durumda.  | No |
| Unschedulable | Kaynak eksikliği nedeniyle hizmet şu anda dağıtılamıyor. | No |
| Başarısız | Hizmet bir hata veya kilitlenme nedeniyle dağıtılamadı. | Yes |
| Sağlam | Hizmet sağlıklı ve uç nokta kullanılabilir. | Yes |


### <a name="batch-inference"></a><a id="azuremlcompute"></a>Toplu çıkarım
Azure Machine Learning Işlem hedefleri Azure Machine Learning tarafından oluşturulur ve yönetilir. Bunlar, Azure Machine Learning işlem hatlarından toplu tahmin için kullanılabilirler.

Azure Machine Learning Işlem ile Batch çıkarımı hakkında yönergeler için bkz. [Batch öngörülerini çalıştırma](tutorial-pipeline-batch-scoring-classification.md).

### <a name="iot-edge-inference"></a><a id="iotedge"></a>IoT Edge çıkarımı
Kenara dağıtım desteği önizleme aşamasındadır. Daha fazla bilgi için bkz. [Azure Machine Learning IoT Edge modül olarak dağıtma](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning).


## <a name="next-steps"></a>Sonraki adımlar

* [Başarısız bir dağıtımda sorun giderme](how-to-troubleshoot-deployment.md)
* [Azure Kubernetes Service’e dağıtma](how-to-deploy-azure-kubernetes-service.md)
* [Web hizmetlerini kullanmak için istemci uygulamaları oluşturma](how-to-consume-web-service.md)
* [Web hizmetini Güncelleştir](how-to-deploy-update-web-service.md)
* [Özel bir Docker görüntüsü kullanarak model dağıtma](how-to-deploy-custom-docker-image.md)
* [Azure Machine Learning aracılığıyla bir Web hizmetinin güvenliğini sağlamak için TLS kullanma](how-to-secure-web-service.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
* [Model dağıtımları için olay uyarıları ve Tetikleyicileri oluşturma](how-to-use-event-grid.md)


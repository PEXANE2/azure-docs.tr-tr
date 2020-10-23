---
title: Örnek Jupyıter Not defterleri
titleSuffix: Azure Machine Learning
description: SDK için Azure Machine Learning Python 'u araştırmak için örnek jupi not defterlerini bulun ve kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: 14ae5f01e23a7922f508c4a2674e1a3f29e354b8
ms.sourcegitcommit: 419c8c8061c0ff6dc12c66ad6eda1b266d2f40bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2020
ms.locfileid: "92166072"
---
# <a name="explore-azure-machine-learning-with-jupyter-notebooks"></a>Jupi Not defterleri ile Azure Machine Learning araştırma

[Örnek Azure Machine Learning Not defteri deposu](https://github.com/azure/machinelearningnotebooks) , en son Azure MACHINE LEARNING Python SDK örnekleri içerir. Bu Jupter Not defterleri, SDK 'Yı keşfetmenize ve kendi makine öğrenimi projeleriniz için model olarak sunmanıza yardımcı olacak şekilde tasarlanmıştır.

Bu makalede, aşağıdaki ortamlardan depoya nasıl erişebileceğiniz gösterilmektedir:

- [Azure Machine Learning işlem örneği](#notebookvm)
- [Kendi Not defteri sunucunuzu getir](#byo)
- [Veri Bilimi Sanal Makinesi](#dsvm)

> [!NOTE]
> Depoyu kopyaladıktan sonra, öğretici ve **kullanım için nasıl yapılır-azureml** klasöründeki ilgili not defterlerindeki **eğitim** not defterlerini bulacaksınız.

<a name="notebookvm"></a>
## <a name="get-samples-on-azure-machine-learning-compute-instance"></a>Azure Machine Learning işlem örneğinde örnekleri al

Örnekleri kullanmaya başlamanın en kolay yolu, öğreticiyi tamamlamaya yönelik bir yöntemdir [: Kurulum ortamı ve çalışma alanı](tutorial-1st-experiment-sdk-setup.md). Tamamlandıktan sonra, SDK ve örnek depoyla önceden yüklenmiş adanmış bir not defteri sunucusu olacaktır. İndirme veya yükleme gerekli değildir.

<a name="byo"></a>

## <a name="get-samples-on-your-notebook-server"></a>Not defteri sunucunuzda örnekleri alın

Yerel geliştirme için kendi Not defteri sunucunuzu getirmek isterseniz, aşağıdaki adımları izleyin:

[!INCLUDE [aml-your-server](../../includes/aml-your-server.md)]

Bu yönergeler hızlı başlangıç ve öğretici Not defterleri için gereken temel SDK paketlerini yükler. Diğer örnek Not defterleri, ek bileşenleri yüklemenizi gerektirebilir. Daha fazla bilgi için bkz. [Python için Azure Machine Learning SDK 'Sını yüklemeyin](https://docs.microsoft.com/python/api/overview/azure/ml/install).

<a name="dsvm"></a>
## <a name="get-samples-on-dsvm"></a>DSVM 'de örnekleri al

Veri Bilimi Sanal Makinesi (DSVM), veri bilimi çalışmaları için oluşturulmuş olan özelleştirilmiş bir VM görüntüsüdür. [DSVM oluşturursanız](how-to-configure-environment.md#dsvm), SDK ve Not defteri sunucusu yüklenir ve sizin için yapılandırılır. Ancak yine de bir çalışma alanı oluşturmanız ve örnek depoyu kopyalamanız gerekir.

[!INCLUDE [aml-dsvm-server](../../includes/aml-dsvm-server.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning neler yapabileceğini saptamak için [örnek not defterlerini](https://github.com/Azure/MachineLearningNotebooks) keşfetme.

Daha fazla GitHub örnek projesi ve örneği için şu depoya bakın:
+ [Azure/azureml-örnekler](https://github.com/Azure/azureml-examples)
+ [Microsoft/MLOps](https://github.com/Microsoft/MLOps)
+ [Microsoft/MLOpsPython](https://github.com/microsoft/MLOpsPython)

Bu öğreticileri deneyin:

- [IIST ile görüntü sınıflandırma modeli eğitme ve dağıtma](tutorial-train-models-with-aml.md)

- [NYC TAXI veri kümesiyle bir regresyon modeli eğitmek için veri hazırlama ve otomatik makine öğrenimi kullanma](tutorial-auto-train-models.md)

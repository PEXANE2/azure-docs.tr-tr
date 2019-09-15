---
title: Örnek Jupyter Not Defterleri
titleSuffix: Azure Machine Learning
description: SDK için Azure Machine Learning Python 'u araştırmak için örnek jupi not defterlerini bulun ve kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 07/31/2019
ms.custom: seodec18
ms.openlocfilehash: dd93e4e621a4fd474283f788e2cf67b305a1be3d
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70997072"
---
# <a name="explore-azure-machine-learning-with-jupyter-notebooks"></a>Jupi Not defterleri ile Azure Machine Learning araştırma

[Örnek Azure Machine Learning Not defteri deposu](https://github.com/azure/machinelearningnotebooks) , en son Azure MACHINE LEARNING Python SDK örnekleri içerir. Bu Jupter Not defterleri, SDK 'Yı keşfetmenize ve kendi makine öğrenimi projeleriniz için model olarak sunmanıza yardımcı olacak şekilde tasarlanmıştır.

Bu makalede, aşağıdaki ortamlardan depoya nasıl erişebileceğiniz gösterilmektedir:

- [Azure Machine Learning Not defteri VM 'si](#notebookvm)
- [Kendi Not defteri sunucunuzu getir](#byo)
- [Veri Bilimi Sanal Makinesi](#dsvm)

> [!NOTE]
> Depoyu kopyaladıktan sonra, öğretici ve **kullanım için nasıl yapılır-azureml** klasöründeki ilgili not defterlerindeki **eğitim** not defterlerini bulacaksınız.

<a name="notebookvm"></a>
## <a name="get-samples-on-azure-machine-learning-notebook-vm"></a>Azure Machine Learning Not defteri VM 'sinde örnekleri al

Örnekleri kullanmaya başlamak için en kolay yol, [öğreticiyi tamamdır: Kurulum ortamı ve çalışma](tutorial-1st-experiment-sdk-setup.md)alanı. Tamamlandıktan sonra, SDK ve örnek depoyla önceden yüklenmiş adanmış bir not defteri sunucusu olacaktır. İndirme veya yükleme gerekli değildir.

<a name="byo"></a>

## <a name="get-samples-on-your-notebook-server"></a>Not defteri sunucunuzda örnekleri alın

Yerel geliştirme için kendi Not defteri sunucunuzu getirmek isterseniz, aşağıdaki adımları izleyin:

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

Bu yönergeler hızlı başlangıç ve öğretici Not defterleri için gereken temel SDK paketlerini yükler. Diğer örnek Not defterleri, ek bileşenleri yüklemenizi gerektirebilir. Daha fazla bilgi için bkz. [Python için Azure Machine Learning SDK 'Sını yüklemeyin](https://docs.microsoft.com/python/api/overview/azure/ml/install).

<a name="dsvm"></a>
## <a name="get-samples-on-dsvm"></a>DSVM 'de örnekleri al

Veri Bilimi Sanal Makinesi (DSVM), özellikle veri bilimi yapmak için oluşturulmuş özelleştirilmiş bir VM görüntüsüdür. [DSVM oluşturursanız](how-to-configure-environment.md#dsvm), SDK ve Not defteri sunucusu yüklenir ve sizin için yapılandırılır. Ancak yine de bir çalışma alanı oluşturmanız ve örnek depoyu kopyalamanız gerekir.

[!INCLUDE [aml-dsvm-server](../../../includes/aml-dsvm-server.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning neler yapabileceğini öğrenmek için [örnek not defterlerini](https://aka.ms/aml-notebooks) bulun veya bu öğreticileri deneyin:

- [Eğitmek ve bir görüntü sınıflandırma modeli MNIST ile dağıtma](tutorial-train-models-with-aml.md)

- [Veri hazırlama ve NYC taksi veri kümesiyle bir regresyon modeli eğitmek için otomatik makine öğrenimi kullanıyor](tutorial-auto-train-models.md)

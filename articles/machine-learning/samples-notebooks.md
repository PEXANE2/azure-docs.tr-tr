---
title: Örnek Jupyter dizüstü bilgisayarlar
titleSuffix: Azure Machine Learning
description: SDK için Azure Machine Learning Python'u keşfetmek için örnek Jupyter dizüstü bilgisayarları bulun ve kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: 7242b82ee5c43878a33731bd1f02b685020f22b0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78673644"
---
# <a name="explore-azure-machine-learning-with-jupyter-notebooks"></a>Jupyter dizüstü bilgisayarlarla Azure Machine Learning'i keşfedin

[Azure Machine Learning Notebook deposu örneği,](https://github.com/azure/machinelearningnotebooks) en son Azure Machine Learning Python SDK örneklerini içerir. Bu Juypter dizüstü bilgisayarlar, SDK'yı keşfetmenize ve kendi makine öğrenimi projeleriniz için model olarak hizmet vermenize yardımcı olmak üzere tasarlanmıştır.

Bu makalede, aşağıdaki ortamlardan depoya nasıl erişilen gösterir:

- [Azure Machine Learning bilgi işlem örneği](#notebookvm)
- [Kendi dizüstü bilgisayar sunucunuzu getirin](#byo)
- [Veri Bilimi Sanal Makinesi](#dsvm)

> [!NOTE]
> Depoyu klonladıktan sonra, **öğreticiler** klasöründe öğretici not defterleri ve **kullanıma nasıl sunulur-azureml** klasöründe özel dizüstü bilgisayarlar bulacaksınız.

<a name="notebookvm"></a>
## <a name="get-samples-on-azure-machine-learning-compute-instance"></a>Azure Machine Learning bilgi işlem örneğinden örnekler alın

Örneklerle başlamanın en kolay yolu [Öğreticiyi tamamlamaktır: Kurulum ortamı ve çalışma alanı.](tutorial-1st-experiment-sdk-setup.md) Tamamlandıktan sonra, SDK ve örnek deposu ile önceden yüklenmiş özel bir dizüstü bilgisayar sunucusuna sahip olacaksınız. İndirme veya yükleme gerekmez.

<a name="byo"></a>

## <a name="get-samples-on-your-notebook-server"></a>Not defteri sunucunuzdan örnekler alın

Yerel geliştirme için kendi dizüstü bilgisayar sunucunuzu getirmek istiyorsanız aşağıdaki adımları izleyin:

[!INCLUDE [aml-your-server](../../includes/aml-your-server.md)]

Bu talimatlar, hızlı başlatma ve öğretici dizüstü bilgisayarlar için gerekli olan temel SDK paketlerini yükler. Diğer örnek not defterleri, ek bileşenler yüklemenizi gerektirebilir. Daha fazla bilgi için [Python için Azure Machine Learning SDK'yı yükleyin' e](https://docs.microsoft.com/python/api/overview/azure/ml/install)bakın.

<a name="dsvm"></a>
## <a name="get-samples-on-dsvm"></a>DSVM'den örnek alın

Veri Bilimi Sanal Makine (DSVM) özel olarak veri bilimi yapmak için oluşturulmuş özelleştirilmiş bir VM görüntüdür. Bir [DSVM oluşturursanız,](how-to-configure-environment.md#dsvm)SDK ve dizüstü bilgisayar sunucusu sizin için yüklenir ve yapılandırılır. Ancak, yine de bir çalışma alanı oluşturmanız ve örnek deposunu klonlamanız gerekir.

[!INCLUDE [aml-dsvm-server](../../includes/aml-dsvm-server.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning'in neler yapabileceğini keşfetmek için [örnek not defterlerini](https://aka.ms/aml-notebooks) keşfedin veya şu öğreticileri deneyin:

- [MNIST ile bir görüntü sınıflandırma modeli eğitin ve dağıtın](tutorial-train-models-with-aml.md)

- [Verileri hazırlamak ve NYC taksi veri seti ile bir regresyon modeli eğitmek için otomatik makine öğrenme kullanın](tutorial-auto-train-models.md)

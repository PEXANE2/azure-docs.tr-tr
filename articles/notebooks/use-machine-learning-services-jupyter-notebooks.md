---
title: Azure Notebooks önizlemede Azure Machine Learning kullanma
description: Azure Notebooks önizlemesiyle kullanabileceğiniz Azure Machine Learning yönelik örnek not defterlerine genel bakış.
ms.topic: how-to
ms.date: 12/04/2018
ms.openlocfilehash: 435abca83255221d438d530b63c237c08bb0b672
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75860580"
---
# <a name="use-azure-machine-learning-in-azure-notebooks-preview"></a>Azure Notebooks önizlemede Azure Machine Learning kullanma

Azure Notebooks, [Azure Machine Learning](/azure/machine-learning/)çalışmak için gerekli ortamla önceden yapılandırılmış olarak gelir. Not defterlerini hesabınızda çeşitli Machine Learning senaryolarını keşfetmek için örnek proje bir kolayca kopyalayabilirsiniz.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="clone-the-sample-into-your-account"></a>Hesabınızda örneği kopyalayın

1. Oturum [Azure not defterleri](https://notebooks.azure.com/).
1. Projeler panosuna gitmek için **Projelerim** ' ı seçin.
1. GitHub **deposunu karşıya** yükle açılan penceresini açmak Için **GitHub deposunu karşıya yükle** (yukarı ok) düğmesini seçin.
1. Açılan pencerede, **GitHub deposuna**`Azure/MachineLearningNotebooks` girin, proje **adında** "Azure Machine Learning" gibi proje için bir ad sağlayın, proje **kimliğinde**bir tanımlayıcı sağlayın, isterseniz **genel** ' i temizleyin, sonra **içeri aktar**' ı seçin.

    ![Azure Machine Learning not defteri örnek not defterleri hesabınızda içeri aktarma](media/azureml-import-project.png)

1. Bir veya iki dakika sonra Azure not defterleri, otomatik olarak yeni projenin panoya alır.

## <a name="run-a-sample-notebook"></a>Bir örnek not defteri çalıştırma

1. Seçin **00 - configuration.ipynb** not defterinin yapılandırma bölümü başlatın ve bir Azure Machine Learning çalışma alanı oluşturmak için kendi yönergelerini izleyin.

    - Azure not defterleri gerekli Python paketlerini içerdiğinden, kod parçacığının yalnızca 2. adımda Azure ML SDK sürümünü doğrulamak için Önkoşullar çalıştırabilirsiniz.

1. Yapılandırma tamamlandıktan sonra, her biri kendi kendine açıklayıcı olan üçüncü dört farklı örnek Not defteri içeren klasörü açmak için **01. Başlarken** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning belgeler, Not defterleri içinde Machine Learning çalışırken size rehberlik eden diğer kaynakları içerir:

- [Hızlı Başlangıç: Kullanımı Azure Machine Learning'i kullanmaya başlamak Python](https://docs.microsoft.com/azure/machine-learning/how-to-configure-environment#local)
- [Öğretici #1: bir görüntü sınıflandırma modelini Azure Machine Learning ile eğitme](https://docs.microsoft.com/azure/machine-learning/tutorial-train-models-with-aml)
- [Öğretici #2: bir görüntü sınıflandırma modeli Azure Container örneği (ACI) dağıtma](https://docs.microsoft.com/azure/machine-learning/tutorial-deploy-models-with-aml)
- [Öğretici: Azure Machine Learning 'de otomatik makine öğrenimi ile sınıflandırma modeli eğitme](https://docs.microsoft.com/azure/machine-learning/tutorial-auto-train-models)

Ayrıca belgelerine bakın [Python için Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

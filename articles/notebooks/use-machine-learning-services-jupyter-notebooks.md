---
title: Azure Notebooks Azure Machine Learning kullanma
description: Azure Notebooks ile kullanabileceğiniz Azure Machine Learning yönelik örnek not defterlerine genel bakış.
ms.topic: article
ms.date: 12/04/2018
ms.openlocfilehash: e3c4fbdf35bf7ea1f4dddbceb9d7235c67ed13a4
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277451"
---
# <a name="use-azure-machine-learning-in-a-notebook"></a>Bir not defterinde Azure Machine Learning kullanma

Azure Notebooks, [Azure Machine Learning](/azure/machine-learning/service/)çalışmak için gerekli ortamla önceden yapılandırılmış olarak gelir. Not defterlerini hesabınızda çeşitli Machine Learning senaryolarını keşfetmek için örnek proje bir kolayca kopyalayabilirsiniz.

## <a name="clone-the-sample-into-your-account"></a>Hesabınızda örneği kopyalayın

1. [Azure Notebooks](https://notebooks.azure.com/)oturum açın.
1. Projeler panosuna gitmek için **Projelerim** ' ı seçin.
1. GitHub **deposunu karşıya** yükle açılan penceresini açmak Için **GitHub deposunu karşıya yükle** (yukarı ok) düğmesini seçin.
1. Açılan pencerede, **GitHub deposuna**`Azure/MachineLearningNotebooks` girin, proje **adında** "Azure Machine Learning" gibi proje için bir ad sağlayın, proje **kimliğinde**bir tanımlayıcı sağlayın, isterseniz **genel** ' i temizleyin, sonra **içeri aktar**' ı seçin.

    ![Azure Machine Learning not defteri örnek not defterleri hesabınızda içeri aktarma](media/azureml-import-project.png)

1. Bir veya iki dakika sonra Azure not defterleri, otomatik olarak yeni projenin panoya alır.

## <a name="run-a-sample-notebook"></a>Bir örnek not defteri çalıştırma

1. Not defterinin yapılandırma bölümünü başlatmak için **00-Configuration. ipynb** ' yi seçin ve Azure Machine Learning çalışma alanı oluşturmak için yönergelerini izleyin.

    - Azure not defterleri gerekli Python paketlerini içerdiğinden, kod parçacığının yalnızca 2. adımda Azure ML SDK sürümünü doğrulamak için Önkoşullar çalıştırabilirsiniz.

1. Yapılandırma tamamlandıktan sonra, her biri kendi kendine açıklayıcı olan üçüncü dört farklı örnek Not defteri içeren klasörü açmak için **01. Başlarken** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning belgeler, Not defterleri içinde Machine Learning çalışırken size rehberlik eden diğer kaynakları içerir:

- [Hızlı başlangıç: Azure Machine Learning kullanmaya başlamak için Python kullanma](https://docs.microsoft.com/azure/machine-learning/service/quickstart-create-workspace-with-python)
- [Öğretici #1: bir görüntü sınıflandırma modelini Azure Machine Learning ile eğitme](https://docs.microsoft.com/azure/machine-learning/service/tutorial-train-models-with-aml)
- [Öğretici #2: Azure Container Instance (acı) içinde bir görüntü sınıflandırma modeli dağıtma](https://docs.microsoft.com/azure/machine-learning/service/tutorial-deploy-models-with-aml)
- [Öğretici: Azure Machine Learning 'de otomatik makine öğrenimi ile sınıflandırma modeli eğitme](https://docs.microsoft.com/azure/machine-learning/service/tutorial-auto-train-models)

Ayrıca, [Python için Azure MACHINE LEARNING SDK](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)belgelerine bakın.

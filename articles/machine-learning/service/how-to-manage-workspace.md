---
title: Portalda Azure ML çalışma alanları oluşturma
titleSuffix: Azure Machine Learning
description: Azure portal Azure Machine Learning çalışma alanlarını oluşturmayı, görüntülemeyi ve silmeyi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 05/10/2019
ms.custom: seodec18
ms.openlocfilehash: 511c737e160c0f0753e570314c9b29346972cb04
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "71269258"
---
# <a name="create-and-manage-azure-machine-learning-workspaces-in-the-azure-portal"></a>Azure portal Azure Machine Learning çalışma alanları oluşturun ve yönetin

Bu makalede, [Azure Machine Learning](overview-what-is-azure-ml.md)için Azure Portal [**Azure Machine Learning çalışma alanları**](concept-workspace.md) oluşturacaksınız, görüntüleyebilir ve silebilirsiniz.  Portal çalışma alanları ile çalışmaya başlamanın en kolay yoludur, ancak gereksinimleriniz değiştikçe veya Otomasyon artışı için gereksinimler değiştikçe, [Python kodu](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) veya [vs Code uzantısı aracılığıyla](how-to-vscode-tools.md#get-started-with-azure-machine-learning-for-visual-studio-code) [CLI kullanarak](reference-azure-machine-learning-cli.md)çalışma alanları oluşturabilir ve silebilirsiniz.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Bir çalışma alanı oluşturmak için bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

### <a name="download-a-configuration-file"></a>Yapılandırma dosyasını indir

1. Bir [Not DEFTERI VM](tutorial-1st-experiment-sdk-setup.md#azure)'si oluşturacağınız takdirde, bu adımı atlayın.

1. Yerel ortamınızda bu çalışma alanına başvuruda bulunan kodu kullanmayı planlıyorsanız, çalışma alanının **genel bakış** bölümünden **config. json dosyasını indir** ' i seçin.  

   ![Config. json dosyasını indir](./media/how-to-manage-workspace/configure.png)
   
   Dosyayı Python betikleriniz veya Jupyıter Not defterleriniz ile dizin yapısına yerleştirin. Aynı dizinde, *. azureml*adlı bir alt dizin veya bir üst dizin içinde olabilir. Bir not defteri VM oluşturduğunuzda, bu dosya VM 'deki doğru dizine eklenir.


## <a name="view"></a>Çalışma alanını görüntüleme

1. Portalın sol üst köşesinde **tüm hizmetler**' i seçin.

1. **Tüm hizmetler** Filtresi alanına **Machine Learning hizmeti**yazın.  

1. **Machine Learning hizmeti çalışma alanlarını**seçin.

   ![Azure Machine Learning çalışma alanı ara](media/how-to-manage-workspace/all-services.png)

1. Bulunan çalışma alanları listesine bakın. Abonelik, kaynak grupları ve konumlara göre filtre uygulayabilirsiniz.  

1. Özelliklerini göstermek için bir çalışma alanı seçin.
   ![Workspace Özellikler ](media/how-to-manage-workspace/allservices_view_workspace_full.PNG)

## <a name="delete-a-workspace"></a>Çalışma alanını silme

Silmek istediğiniz çalışma alanının üstündeki Sil düğmesini kullanın.

  ![Sil düğmesi](media/how-to-manage-workspace/delete-workspace.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning ile modeller derlemek, eğitme ve dağıtmak için bir çalışma alanı kullanmayı öğrenmek üzere tam uzunluklu öğreticiyi izleyin.

> [!div class="nextstepaction"]
> [Öğretici: modelleri eğitme](tutorial-train-models-with-aml.md)

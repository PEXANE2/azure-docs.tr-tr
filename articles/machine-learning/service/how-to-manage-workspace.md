---
title: Çalışma alanları oluşturma ve yönetme
titleSuffix: Azure Machine Learning service
description: Oluşturun, görüntüleyin ve Azure Machine Learning hizmeti çalışma alanları Azure portalında silme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 05/10/2019
ms.custom: seodec18
ms.openlocfilehash: 81e1104d71706194ba1c54e42722b4508df09091
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69534863"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces"></a>Oluşturma ve Azure Machine Learning hizmeti çalışma alanlarını yönetme

Bu makalede, oluşturun, görüntüleyin, silin ve [ **Azure Machine Learning hizmeti çalışma alanları** ](concept-workspace.md) Azure Portalı'nda [Azure Machine Learning hizmeti](overview-what-is-azure-ml.md).  Portal çalışma alanları ile çalışmaya başlamanın en kolay yoludur, ancak gereksinimleriniz değiştikçe veya Otomasyon artışı için gereksinimler değiştikçe, [Python kodu](https://aka.ms/aml-sdk) veya [vs Code uzantısı aracılığıyla](how-to-vscode-tools.md#get-started-with-azure-machine-learning) [CLI kullanarak](reference-azure-machine-learning-cli.md)çalışma alanları oluşturabilir ve silebilirsiniz.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Bir çalışma alanı oluşturmak için bir Azure aboneliğinizin olması gerekir. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning Service 'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

### <a name="download-a-configuration-file"></a>Yapılandırma dosyasını indir

1. Bir [Not DEFTERI VM](tutorial-1st-experiment-sdk-setup.md#azure)'si oluşturacağınız takdirde, bu adımı atlayın.

1. Yerel ortamınızda bu çalışma alanına başvuruda bulunan kodu kullanmayı planlıyorsanız, çalışma alanının **genel bakış** bölümünden **config. json dosyasını indir** ' i seçin.  

   ![Config.json dosyasını indir](./media/how-to-manage-workspace/configure.png)
   
   Dosyayı Python betikleriniz veya Jupyıter Not defterleriniz ile dizin yapısına yerleştirin. Aynı dizinde, *. azureml*adlı bir alt dizin veya bir üst dizin içinde olabilir. Bir not defteri VM oluşturduğunuzda, bu dosya VM 'deki doğru dizine eklenir.


## <a name="view"></a>Çalışma alanını görüntüleme

1. Sol üst köşede Portalı'nın, seçin **tüm hizmetleri**.

1. **Tüm hizmetler** Filtresi alanına **Machine Learning hizmeti**yazın.  

1. **Machine Learning hizmeti çalışma alanlarını**seçin.

   ![Azure Machine Learning hizmet çalışma alanı ara](media/how-to-manage-workspace/all-services.png)

1. Çalışma alanı bulunamadı listesinde arayın. Bağlı aboneliği, kaynak grupları ve konumlarını filtreleyebilirsiniz.  

1. Özelliklerini göstermek için bir çalışma alanı seçin.
   ![Çalışma alanı özellikleri](media/how-to-manage-workspace/allservices_view_workspace_full.PNG)

## <a name="delete-a-workspace"></a>Çalışma alanını silme

Silmek istediğiniz çalışma alanının üstündeki Sil düğmesini kullanın.

  ![Sil düğmesi](media/how-to-manage-workspace/delete-workspace.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bir çalışma alanı oluşturmak, eğitmek ve modeller Azure Machine Learning hizmeti ile dağıtmak için nasıl kullanılacağını öğrenmek için eksiksiz bir öğreticiyi izleyin.

> [!div class="nextstepaction"]
> [Öğretici: Modelleri eğitme](tutorial-train-models-with-aml.md)

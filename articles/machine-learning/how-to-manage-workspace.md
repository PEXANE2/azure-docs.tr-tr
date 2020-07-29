---
title: Portalda çalışma alanları oluşturma
titleSuffix: Azure Machine Learning
description: Azure portal Azure Machine Learning çalışma alanlarını oluşturmayı, görüntülemeyi ve silmeyi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: sgilley
author: sdgilley
ms.date: 12/27/2019
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: dccf8b2e9608de4f22f9782eb9f3cdb489e18be3
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87319720"
---
# <a name="create-and-manage-azure-machine-learning-workspaces-in-the-azure-portal"></a>Azure portal Azure Machine Learning çalışma alanları oluşturun ve yönetin
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure Machine Learning](overview-what-is-azure-ml.md)için Azure Portal [**Azure Machine Learning çalışma alanları**](concept-workspace.md) oluşturacaksınız, görüntüleyebilir ve silebilirsiniz.  Portal çalışma alanları ile çalışmaya başlamanın en kolay yoludur, ancak gereksinimleriniz değiştikçe veya Otomasyon artışı için gereksinimler değiştikçe, [Python kodu](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) veya [vs Code uzantısı aracılığıyla](tutorial-setup-vscode-extension.md) [CLI kullanarak](reference-azure-machine-learning-cli.md)çalışma alanları oluşturabilir ve silebilirsiniz.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Bir çalışma alanı oluşturmak için bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. [Azure Machine Learning ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

1. Azure aboneliğinizin kimlik bilgilerini kullanarak [Azure Portal](https://portal.azure.com/) oturum açın. 

1. Azure portal sol üst köşesinde **+ kaynak oluştur**' u seçin.

      ![Yeni kaynak oluşturma](./media/how-to-manage-workspace/create-workspace.gif)

1. **Machine Learning**bulmak için arama çubuğunu kullanın.

1. **Machine Learning**seçin.

1. Başlamak için **Machine Learning** bölmesinde **Oluştur** ' u seçin.

1. Yeni çalışma alanınızı yapılandırmak için aşağıdaki bilgileri sağlayın:

   Alan|Açıklama 
   ---|---
   Çalışma alanı adı |Çalışma alanınızı tanımlayan benzersiz bir ad girin. Bu örnekte **docs-WS**kullanıyoruz. Adlar, kaynak grubu genelinde benzersiz olmalıdır. Başkaları tarafından oluşturulan çalışma alanlarını birbirinden ayırmak ve geri çekmek için kolay bir ad kullanın. Çalışma alanı adı büyük/küçük harfe duyarlıdır.
   Abonelik |Kullanmak istediğiniz Azure aboneliğini seçin.
   Kaynak grubu | Aboneliğinizde mevcut kaynak gruplarından birini seçin veya bir ad girerek yeni bir kaynak grubu oluşturun. Kaynak grubu, bir Azure çözümü için ilgili kaynakları barındırır. Bu örnekte **docs-AML**kullanılır. 
   Konum | Çalışma alanınızı oluşturmak için kullanıcılarınıza en yakın konumu ve veri kaynaklarını seçin.
   Çalışma alanı sürümü | **Temel** veya **Kurumsal**' i seçin.  Bu çalışma alanı sürümü, erişim ve fiyatlandırmaya sahip olduğunuz özellikleri belirler. [Temel ve Kurumsal sürüm teklifleri](overview-what-is-azure-ml.md#sku)hakkında daha fazla bilgi edinin. 

    ![Çalışma alanınızı yapılandırma](./media/how-to-manage-workspace/select-edition.png)

1. Çalışma alanını yapılandırmayı bitirdiğinizde, **gözden geçir + oluştur**' u seçin.
2. Ayarları gözden geçirin ve ek değişiklik veya düzeltme yapın. Ayarları tatmin ediyorsanız **Oluştur**' u seçin.

   > [!Warning] 
   > Çalışma alanınızı bulutta oluşturmak birkaç dakika sürebilir.

   İşlem tamamlandığında, bir dağıtım başarı iletisi görüntülenir. 
 
 1. Yeni çalışma alanını görüntülemek için **Kaynağa Git**' i seçin.

### <a name="download-a-configuration-file"></a>Yapılandırma dosyasını indir

1. Bir [işlem örneği](tutorial-1st-experiment-sdk-setup.md#azure)oluşturacağınız takdirde, bu adımı atlayın.

1. Yerel ortamınızda bu çalışma alanına başvuran kodu kullanmayı planlıyorsanız, çalışma alanının **genel bakış** bölümünden **config.jsindir** ' i seçin.  

   ![config.jsindir](./media/how-to-manage-workspace/configure.png)
   
   Dosyayı Python betikleriniz veya Jupyıter Not defterleriniz ile dizin yapısına yerleştirin. Aynı dizinde, *. azureml*adlı bir alt dizin veya bir üst dizin içinde olabilir. Bir işlem örneği oluşturduğunuzda, bu dosya VM 'deki doğru dizine eklenir.

## <a name="upgrade-to-enterprise-edition"></a><a name="upgrade"></a>Enterprise Edition 'a yükseltme

Düşük kodlu deneyimler ve gelişmiş güvenlik özellikleri gibi gelişmiş özelliklerden yararlanmak için, çalışma alanınızı temel sürümden Enterprise Edition 'a yükseltebilirsiniz.

1. [Azure Machine Learning Studio](https://ml.azure.com)'da oturum açın.

1. Yükseltmek istediğiniz çalışma alanını seçin.

1. Sayfanın sağ üst kısmında **daha fazla bilgi edinin** ' i seçin.

   [![Çalışma alanını ](./media/how-to-manage-workspace/upgrade.png) yükseltme](./media/how-to-manage-workspace/upgrade.png#lightbox)

1. Görüntülenen pencerede **Yükselt** ' i seçin.


> [!IMPORTANT]
> Enterprise Edition çalışma alanını temel bir sürüm çalışma alanına indirgeyemezsiniz. 

## <a name="find-a-workspace"></a><a name="view"></a>Çalışma alanı bul

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. Üst arama alanına **Machine Learning**yazın.  

1. **Machine Learning**seçin.

   ![Azure Machine Learning çalışma alanı ara](./media/how-to-manage-workspace/find-workspaces.png)

1. Bulunan çalışma alanları listesine bakın. Abonelik, kaynak grupları ve konumlara göre filtre uygulayabilirsiniz.  

1. Özelliklerini göstermek için bir çalışma alanı seçin.

## <a name="delete-a-workspace"></a>Çalışma alanı silme

[Azure Portal](https://portal.azure.com/), silmek istediğiniz çalışma alanının en üstünde bulunan **Sil** ' i seçin.

:::image type="content" source="./media/how-to-manage-workspace/delete-workspace.png" alt-text="Çalışma alanını sil":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="troubleshooting"></a>Sorun giderme

### <a name="resource-provider-errors"></a>Kaynak sağlayıcısı hataları

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="moving-the-workspace"></a>Çalışma alanı taşınıyor

> [!WARNING]
> Azure Machine Learning çalışma alanınızı farklı bir aboneliğe taşımak veya sahip olunan aboneliğin yeni bir kiracıya taşınması desteklenmez. Bunun yapılması hatalara neden olabilir.

### <a name="deleting-the-azure-container-registry"></a>Azure Container Registry silme

Azure Machine Learning çalışma alanı bazı işlemler için Azure Container Registry (ACR) kullanır. İlk kez ihtiyaç duyduğunda, otomatik olarak bir ACR örneği oluşturur.

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning ile modeller derlemek, eğitme ve dağıtmak için bir çalışma alanı kullanmayı öğrenmek üzere tam uzunluklu öğreticiyi izleyin.

> [!div class="nextstepaction"]
> [Öğretici: modelleri eğitme](tutorial-train-models-with-aml.md)

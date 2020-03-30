---
title: Portalda Azure Machine Learning çalışma alanları oluşturma
titleSuffix: Azure Machine Learning
description: Azure portalında Azure Machine Learning çalışma alanlarını nasıl oluşturup görüntülemeyi ve silmeyi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 12/27/2019
ms.custom: seodec18
ms.openlocfilehash: f38b0895b0d6eddcf63c082d3df205f4d9de9d50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297057"
---
# <a name="create-and-manage-azure-machine-learning-workspaces-in-the-azure-portal"></a>Azure portalında Azure Machine Learning çalışma alanlarını oluşturma ve yönetme
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure Machine Learning](overview-what-is-azure-ml.md)için Azure portalında Azure Machine Learning çalışma [**alanlarını**](concept-workspace.md) oluşturacak, görüntüleyebilir ve sileceksiniz.  Portal çalışma alanları ile başlamak için en kolay yoldur ama ihtiyaçlarınız değiştikçe veya otomasyon için gereksinimleri de oluşturmak ve [CLI kullanarak](reference-azure-machine-learning-cli.md)çalışma alanları silmek , [Python kodu ile](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) veya VS Kodu uzantısı [üzerinden](tutorial-setup-vscode-extension.md).

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma

Çalışma alanı oluşturmak için bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliğiniz yoksa, başlamadan önce ücretsiz bir hesap oluşturun. Azure [Machine Learning'in ücretsiz veya ücretli sürümünü](https://aka.ms/AMLFree) bugün deneyin.

1. Azure aboneliğinizin kimlik bilgilerini kullanarak [Azure portalında](https://portal.azure.com/) oturum açın. 

1. Azure portalının sol üst köşesinde **+ Kaynak oluştur'u**seçin.

      ![Yeni kaynak oluşturma](./media/how-to-manage-workspace/create-workspace.gif)

1. **Machine Learning'i**bulmak için arama çubuğunu kullanın.

1. **Makine Öğrenimi'ni**seçin.

1. Machine **Learning** bölmesinde başlamak için **Oluştur'u** seçin.

1. Yeni çalışma alanınızı yapılandırmak için aşağıdaki bilgileri sağlayın:

   Alan|Açıklama 
   ---|---
   Çalışma alanı adı |Çalışma alanınızı tanımlayan benzersiz bir ad girin. Bu örnekte, **dokümanlar-ws**kullanırız. Adlar kaynak grubunda benzersiz olmalıdır. Hatırlaması ve başkaları tarafından oluşturulan çalışma alanlarından ayırt etmesi kolay bir ad kullanın. Çalışma alanı adı büyük/küçük harf duyarsız.
   Abonelik |Kullanmak istediğiniz Azure aboneliğini seçin.
   Kaynak grubu | Aboneliğinizde mevcut kaynak gruplarından birini seçin veya bir ad girerek yeni bir kaynak grubu oluşturun. Kaynak grubu, Azure çözümü için ilgili kaynakları tutar. Bu örnekte, **docs-aml**kullanırız. 
   Konum | Çalışma alanınızı oluşturmak için kullanıcılarınıza en yakın konumu ve veri kaynaklarını seçin.
   Çalışma alanı sürümü | **Temel** veya **Kurumsal'ı**seçin.  Bu çalışma alanı sürümü, erişeceğiniz ve fiyatlandırma yapacağınız özellikleri belirler. [Temel ve Kurumsal sürüm teklifleri](overview-what-is-azure-ml.md#sku)hakkında daha fazla bilgi edinin. 

    ![Çalışma alanınızı yapılandırın](./media/how-to-manage-workspace/select-edition.png)

1. Çalışma alanını yapılandırmayı **bitirdiğinizde, Gözden Geçir + Oluştur'u**seçin.
2. Ayarları gözden geçirin ve ek değişiklikler veya düzeltmeler yapın. Ayarlarla doyduğunda **Oluştur'u**seçin.

   > [!Warning] 
   > Çalışma alanınızı bulutta oluşturmak birkaç dakika sürebilir.

   İşlem tamamlandığında, bir dağıtım başarı iletisi görüntülenir. 
 
 1. Yeni çalışma alanını görüntülemek **için kaynağa git'i**seçin.

### <a name="download-a-configuration-file"></a>Yapılandırma dosyası indirme

1. Bir [işlem örneği](tutorial-1st-experiment-sdk-setup.md#azure)oluşturacaksanız, bu adımı atlayın.

1. Bu çalışma alanına başvuran yerel ortamınızda kod kullanmayı planlıyorsanız, çalışma alanının **Genel Bakış** bölümünden **config.json indir'i** seçin.  

   ![Config.json karşıdan yükleme](./media/how-to-manage-workspace/configure.png)
   
   Python komut dosyalarınız veya Jupyter Notebook'larınız ile dosyayı dizin yapısına yerleştirin. Aynı dizinde, *.azureml*adlı bir alt dizinde veya bir üst dizinde olabilir. Bir işlem örneği oluşturduğunuzda, bu dosya sizin için VM'deki doğru dizine eklenir.

## <a name="upgrade-to-enterprise-edition"></a><a name="upgrade"></a>Kurumsal sürümüne yükseltme

Düşük kod deneyimleri ve gelişmiş güvenlik özellikleri gibi gelişmiş özelliklerden yararlanmak için çalışma alanınızı Temel sürümden Enterprise sürümüne yükseltebilirsiniz.

1. [Azure Machine Learning stüdyosunda](https://ml.azure.com)oturum açın.

1. Yükseltmek istediğiniz çalışma alanını seçin.

1. Sayfanın sağ üst kısmında **daha fazla bilgi** edinin'i seçin.

   [![Çalışma alanını](./media/how-to-manage-workspace/upgrade.png) yükseltme](./media/how-to-manage-workspace/upgrade.png#lightbox)

1. Görünen pencerede **Yükseltme'yi** seçin.


> [!IMPORTANT]
> Kurumsal sürüm çalışma alanını Temel sürüm çalışma alanına indiremezsiniz. 

## <a name="find-a-workspace"></a><a name="view"></a>Çalışma alanı bulma

1. En üst arama alanında **Machine Learning**yazın.  

1. **Makine Öğrenimi'ni**seçin.

   ![Azure Makine Öğrenimi çalışma alanını ara](./media/how-to-manage-workspace/find-workspaces.png)

1. Bulunan çalışma alanları listesine bakın. Abonelik, kaynak grupları ve konumlara göre filtre uygulayabilirsiniz.  

1. Özelliklerini görüntülemek için bir çalışma alanı seçin.

## <a name="delete-a-workspace"></a>Çalışma alanını silme

Silmek istediğiniz çalışma alanının üst kısmındaki Sil düğmesini kullanın.

  ![Sil düğmesi](./media/how-to-manage-workspace/delete-workspace.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [aml-delete-resource-group](../../includes/aml-delete-resource-group.md)]

## <a name="troubleshooting"></a>Sorun giderme

### <a name="resource-provider-errors"></a>Kaynak sağlayıcı hataları

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="moving-the-workspace"></a>Çalışma alanını taşıma

> [!WARNING]
> Azure Machine Learning çalışma alanınızı farklı bir aboneye taşımak veya sahip olan aboneliğiyeni bir kiracıya taşımak desteklenmez. Bunu yapmak hatalara neden olabilir.

### <a name="deleting-the-azure-container-registry"></a>Azure Kapsayıcı Kayıt Defterini Silme

Azure Machine Learning çalışma alanı, bazı işlemler için Azure Kapsayıcı Kayıt Defteri'ni (ACR) kullanır. İlk ihtiyacı olduğunda otomatik olarak bir ACR örneği oluşturur.

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning ile modelleri oluşturmak, eğitmek ve dağıtmak için çalışma alanını nasıl kullanacağınızı öğrenmek için tam uzunluktaki öğreticiyi izleyin.

> [!div class="nextstepaction"]
> [Öğretici: Tren modelleri](tutorial-train-models-with-aml.md)

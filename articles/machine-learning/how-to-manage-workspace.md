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
ms.openlocfilehash: 1f68753e7b28163f3ec0c18c30cf5939e0bc5243
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89649424"
---
# <a name="create-and-manage-azure-machine-learning-workspaces-in-the-azure-portal"></a>Azure portal Azure Machine Learning çalışma alanları oluşturun ve yönetin
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, [Azure Machine Learning](overview-what-is-azure-ml.md)için Azure Portal [**Azure Machine Learning çalışma alanları**](concept-workspace.md) oluşturacaksınız, görüntüleyebilir ve silebilirsiniz.  Portal çalışma alanları ile çalışmaya başlamanın en kolay yoludur, ancak gereksinimleriniz değiştikçe veya Otomasyon artışı için gereksinimler değiştikçe, [Python kodu](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py&preserve-view=true) veya [vs Code uzantısı aracılığıyla](tutorial-setup-vscode-extension.md) [CLI kullanarak](reference-azure-machine-learning-cli.md)çalışma alanları oluşturabilir ve silebilirsiniz.

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
   Kaynak grubu | Aboneliğinizde mevcut kaynak gruplarından birini seçin veya bir ad girerek yeni bir kaynak grubu oluşturun. Kaynak grubu, bir Azure çözümü için ilgili kaynakları barındırır. Bu örnekte **docs-AML**kullanılır. Mevcut bir kaynak grubunu kullanmak için *katkıda bulunan* veya *sahip* rolünün olması gerekir.  Erişim hakkında daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanına erişimi yönetme](how-to-assign-roles.md).
   Konum | Çalışma alanınızı oluşturmak için kullanıcılarınıza en yakın konumu ve veri kaynaklarını seçin.
   Konum | Çalışma alanınızı oluşturmak için kullanıcılarınıza en yakın konumu ve veri kaynaklarını seçin.
   Çalışma alanı sürümü | **Temel** veya **Kurumsal**' i seçin.  Bu çalışma alanı sürümü, erişim ve fiyatlandırmaya sahip olduğunuz özellikleri belirler. [Temel ve Kurumsal sürüm teklifleri](overview-what-is-azure-ml.md#sku)hakkında daha fazla bilgi edinin. 

    ![Çalışma alanınızı yapılandırma](./media/how-to-manage-workspace/select-edition.png)

1. Çalışma alanını yapılandırmayı bitirdiğinizde, **gözden geçir + oluştur**' u seçin. İsteğe bağlı olarak, çalışma alanı için daha fazla ayar yapılandırmak üzere [ağ](#networking) ve [Gelişmiş](#advanced) bölümleri kullanın.
2. Ayarları gözden geçirin ve ek değişiklik veya düzeltme yapın. Ayarları tatmin ediyorsanız **Oluştur**' u seçin.

   > [!Warning] 
   > Çalışma alanınızı bulutta oluşturmak birkaç dakika sürebilir.

   İşlem tamamlandığında, bir dağıtım başarı iletisi görüntülenir. 
 
 1. Yeni çalışma alanını görüntülemek için **Kaynağa Git**' i seçin.


### <a name="networking"></a>Ağ

> [!IMPORTANT]
> Çalışma alanınıza özel bir uç nokta ve sanal ağ kullanma hakkında daha fazla bilgi için bkz. [ağ yalıtımı ve gizliliği](how-to-enable-virtual-network.md).

1. Varsayılan ağ yapılandırması, genel İnternet üzerinden erişilebilen __genel bir uç nokta__kullanmaktır. Çalışma alanınıza erişimi oluşturduğunuz bir Azure sanal ağıyla sınırlamak için, __bağlantı yöntemi__olarak __Özel uç nokta__ (Önizleme) seçeneğini belirleyip, ardından uç noktayı yapılandırmak için __+ Ekle__ ' yi kullanabilirsiniz.

   > [!IMPORTANT]
   > Azure Machine Learning çalışma alanı ile özel uç nokta kullanımı Şu anda genel önizlemededir. Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

   :::image type="content" source="media/how-to-manage-workspace/select-private-endpoint.png" alt-text="Özel uç nokta seçimi":::

1. __Özel uç nokta oluştur__ formunda, kullanılacak konumu, adı ve sanal ağı ayarlayın. Uç noktayı bir Özel DNS bölgesiyle kullanmak istiyorsanız, __özel DNS bölgesi Ile tümleştirin__ ' ı seçin ve __özel DNS bölgesi__ alanını kullanarak bölgeyi seçin. Uç noktayı oluşturmak için __Tamam ' ı__ seçin. 

   :::image type="content" source="media/how-to-manage-workspace/create-private-endpoint.png" alt-text="Özel uç nokta oluşturma":::

1. Ağı yapılandırmayı bitirdiğinizde, __gözden geçir + oluştur__' u seçebilir veya Isteğe bağlı __Gelişmiş__ yapılandırmaya ilerleyebilirsiniz.

    > [!WARNING]
    > Özel bir uç nokta oluşturduğunuzda, __Privatelink.api.azureml.MS__ adlı yeni bir özel DNS bölgesi oluşturulur. Bu, sanal ağın bağlantısını içerir. Aynı kaynak grubunda özel uç noktaları olan birden çok çalışma alanı oluşturursanız, DNS bölgesine yalnızca ilk özel uç nokta için sanal ağ eklenebilir. Ek çalışma alanları/özel uç noktalar tarafından kullanılan sanal ağların girdilerini eklemek için aşağıdaki adımları kullanın:
    > 
    > 1. [Azure Portal](https://portal.azure.com), çalışma alanını içeren kaynak grubunu seçin. Ardından __Privatelink.api.azureml.MS__adlı özel DNS bölgesi kaynağını seçin.
    > 2. __Ayarlar__' da, __sanal ağ bağlantıları__' nı seçin.
    > 3. __Ekle__’yi seçin. __Sanal ağ bağlantısı ekle__ sayfasında, benzersiz bir __bağlantı adı__girin ve eklenecek __sanal ağı__ seçin. Ağ bağlantısını eklemek için __Tamam ' ı__ seçin.
    >
    > Daha fazla bilgi için bkz. [Azure özel uç nokta DNS yapılandırması](/azure/private-link/private-endpoint-dns).

### <a name="advanced"></a>Gelişmiş

Varsayılan olarak, çalışma alanının ölçümleri ve meta verileri Microsoft 'un koruduğu bir Azure Cosmos DB örneğine depolanır. Bu veriler, Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. 

Çalışma alanınızda Microsoft 'un topladığı verileri sınırlandırmak için __yüksek iş etkisi çalışma alanını__seçin.

> [!IMPORTANT]
> Yüksek iş etkisi seçilmesi, yalnızca bir çalışma alanı oluşturulurken yapılabilir. Çalışma alanı oluşturulduktan sonra bu ayarı değiştiremezsiniz.

Azure Machine Learning __Enterprise__ sürümünü kullanıyorsanız kendi anahtarınızı sağlayabilirsiniz. Bunun yapılması, ölçümleri ve meta verileri Azure aboneliğinizde depolayan Azure Cosmos DB örneğini oluşturur. Kendi anahtarınızı kullanmak için aşağıdaki adımları kullanın:

> [!IMPORTANT]
> Bu adımları uygulamadan önce, önce aşağıdaki eylemleri gerçekleştirmeniz gerekir:
>
> 1. __Machine Learning uygulamayı__ (kimlik ve erişim yönetimi 'nde) aboneliğinizde katkıda bulunan izinlerle yetkilendirin.
> 1. [Müşteri tarafından yönetilen anahtarları yapılandırma](/azure/cosmos-db/how-to-setup-cmk) bölümündeki adımları izleyerek şunları yapın:
>     * Azure Cosmos DB sağlayıcıyı kaydetme
>     * Azure Key Vault oluşturma ve yapılandırma
>     * Anahtar oluştur
>
>     Azure Cosmos DB örneğini el ile oluşturmanız gerekmez, bir tane, çalışma alanı oluşturma sırasında sizin için oluşturulur. Bu Azure Cosmos DB örneği, bu düzene göre bir ad kullanılarak ayrı bir kaynak grubunda oluşturulacak: `<your-resource-group-name>_<GUID>` .
>
> Çalışma alanı oluşturulduktan sonra bu ayarı değiştiremezsiniz. Çalışma alanınız tarafından kullanılan Azure Cosmos DB silerseniz, onu kullanan çalışma alanını da silmeniz gerekir.

1. __Müşteri tarafından yönetilen anahtarlar__' ı seçin ve ardından __anahtar seçmek Için tıklayın ' ı__seçin.

    :::image type="content" source="media/how-to-manage-workspace/advanced-workspace.png" alt-text="Müşteri tarafından yönetilen anahtarlar":::

1. __Azure Key Vault anahtarı seç__ formunda, var olan bir Azure Key Vault, içerdiği bir anahtarı ve anahtarın sürümünü seçin. Bu anahtar, Azure Cosmos DB depolanan verileri şifrelemek için kullanılır. Son olarak, bu anahtarı kullanmak için __Seç__ düğmesini kullanın.

   :::image type="content" source="media/how-to-manage-workspace/select-key-vault.png" alt-text="Anahtarı seçin":::

## <a name="upgrade-to-enterprise-edition"></a><a name="upgrade"></a>Enterprise Edition 'a yükseltme

Düşük kodlu deneyimler ve gelişmiş güvenlik özellikleri gibi gelişmiş özelliklerden yararlanmak için, çalışma alanınızı temel sürümden Enterprise Edition 'a yükseltebilirsiniz.

1. [Azure Machine Learning Studio](https://ml.azure.com)'da oturum açın.

1. Yükseltmek istediğiniz çalışma alanını seçin.

1. Sayfanın sağ üst kısmında **daha fazla bilgi edinin**  ' i seçin.

   [![Çalışma alanını ](./media/how-to-manage-workspace/upgrade.png) yükseltme](./media/how-to-manage-workspace/upgrade.png#lightbox)

1. Görüntülenen pencerede **Yükselt** ' i seçin.


> [!IMPORTANT]
> Enterprise Edition çalışma alanını temel bir sürüm çalışma alanına indirgeyemezsiniz.

### <a name="download-a-configuration-file"></a>Yapılandırma dosyasını indir

1. Bir [işlem örneği](tutorial-1st-experiment-sdk-setup.md#azure)oluşturacağınız takdirde, bu adımı atlayın.

1. Yerel ortamınızda bu çalışma alanına başvuran kodu kullanmayı planlıyorsanız, çalışma alanının **genel bakış** bölümünden **config.jsindir** ' i seçin.  

   ![config.jsindir](./media/how-to-manage-workspace/configure.png)
   
   Dosyayı Python betikleriniz veya Jupyıter Not defterleriniz ile dizin yapısına yerleştirin. Aynı dizinde, *. azureml*adlı bir alt dizin veya bir üst dizin içinde olabilir. Bir işlem örneği oluşturduğunuzda, bu dosya VM 'deki doğru dizine eklenir.

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

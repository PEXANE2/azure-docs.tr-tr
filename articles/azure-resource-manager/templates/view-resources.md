---
title: Kaynak özelliklerini bulma
description: Kaynak özelliklerinin nasıl aranacağını açıklar.
ms.topic: conceptual
ms.date: 06/10/2020
ms.openlocfilehash: 6e00ee875d9b0f7fadf27a44c55920872c815045
ms.sourcegitcommit: eeba08c8eaa1d724635dcf3a5e931993c848c633
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84668568"
---
# <a name="discover-resource-properties"></a>Kaynak özelliklerini bulma

Kaynak Yöneticisi şablonları oluşturmadan önce, hangi kaynak türlerinin kullanılabilir olduğunu ve şablonunuzda hangi değerleri kullanacağınızı anlamanız gerekir. Bu makalede, şablonunuza dahil edilecek özellikleri bulabileceğiniz bazı yollar gösterilmektedir.

## <a name="find-resource-provider-namespaces"></a>Kaynak sağlayıcısı ad alanlarını bulma

ARM şablonundaki kaynaklar bir kaynak sağlayıcısı ad alanı ve kaynak türü ile tanımlanır. Örneğin, Microsoft. Storage/storageAccounts, depolama hesabı kaynak türünün tam adıdır. Microsoft. Storage, ad alanıdır. Kullanmak istediğiniz kaynak türleri için ad alanlarını henüz bilmiyorsanız bkz. [Azure hizmetleri Için kaynak sağlayıcıları](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-services-resource-providers).

![Kaynak Yöneticisi kaynak sağlayıcısı ad alanı eşlemesi](./media/view-resources/resource-provider-namespace-and-azure-service-mapping.png)

## <a name="export-templates"></a>Şablonları dışarı aktarma

Mevcut kaynaklarınızın şablon özelliklerini almanın en kolay yolu, şablonu dışarı aktarmanız. Daha fazla bilgi için bkz. [Azure Portal bir şablona tek ve çoklu kaynak dışarı aktarma](./export-template-portal.md).

## <a name="use-resource-manager-tools-extension"></a>Kaynak Yöneticisi Araçları uzantısını kullan

Visual Studio Code ve Azure Resource Manager araçları uzantısı, her kaynak türü için tam olarak hangi özelliklerin gerekli olduğunu görbaşlamanıza yardımcı olur. Bu kişiler, şablonunuzda bir kaynağı nasıl tanımlacağınızı kolaylaştıran IntelliSense ve kod parçacıkları sağlar. Daha fazla bilgi için bkz. [hızlı başlangıç: Visual Studio Code Azure Resource Manager şablonlar oluşturma](./quickstart-create-templates-use-visual-studio-code.md#add-an-azure-resource).

Aşağıdaki ekran görüntüsünde bir şablona bir depolama hesabı kaynağının eklendiği gösterilmektedir:

![Kaynak Yöneticisi araçları uzantı parçacıkları](./media/view-resources/resource-manager-tools-extension-snippets.png)

Uzantı Ayrıca yapılandırma özellikleri için seçeneklerin bir listesini de sağlar.

![Kaynak Yöneticisi Araçları uzantısı yapılandırılabilir değerler](./media/view-resources/resource-manager-tools-extension-configurable-properties.png)

## <a name="use-template-reference"></a>Şablon başvurusu kullanma

Azure Resource Manager şablonu başvurusu, şablon şeması için en kapsamlı kaynaktır. API sürümleri, şablon biçimi ve özellik bilgilerini bulabilirsiniz.

1. [Azure Resource Manager şablonu başvurusuna](/azure/templates/)gidin.
1. Sol gezinti bölmesinde **depolama**' yı seçin ve ardından **tüm kaynaklar**' ı seçin. Tüm Kaynaklar sayfası, kaynak türlerini ve sürümlerini özetler.

    ![Şablon başvurusu kaynak sürümleri](./media/view-resources/resource-manager-template-reference-resource-versions.png)

    Kaynak türünü biliyorsanız, şu URL biçimiyle doğrudan bu sayfaya gidebilirsiniz: `https://docs.microsoft.com/azure/templates/{provider-namespace}/{resource-type}` .

1. En son sürümü seçin. En son API sürümünü kullanmanız önerilir.

    **Şablon biçimi** bölümü, depolama hesabının tüm özelliklerini listeler. **SKU** listede:

    ![Şablon başvuru depolama hesabı biçimi](./media/view-resources/resource-manager-template-reference-storage-account-sku.png)

    **Özellik değerleri** bölümünde **SKU nesnesini** görmek için aşağı kaydırın. Makale, SKU adı için izin verilen değerleri gösterir:

    ![Şablon başvurusu depolama hesabı SKU değerleri](./media/view-resources/resource-manager-template-reference-storage-account-sku-values.png)

    Sayfanın sonunda, **hızlı başlangıç şablonları** bölümü, kaynak türünü Içeren bazı Azure hızlı başlangıç şablonlarını listeler:

    ![Şablon başvuru depolama hesabı hızlı başlangıç şablonları](./media/view-resources/resource-manager-template-reference-quickstart-templates.png)

Şablon başvurusu, her bir Azure hizmet belge sitesinin her birinden bağlantılıdır.  Örneğin, [Key Vault belgeleri sitesi](../../key-vault/general/overview.md):

![Şablon başvurusunu Kaynak Yöneticisi Key Vault](./media/view-resources/resource-manager-template-reference-key-vault.png)

## <a name="use-resource-explorer"></a>Kaynak Gezgini kullan

Kaynak Gezgini Azure portal katıştırılır. Bu yöntemi kullanmadan önce bir depolama hesabınız olması gerekir. Yoksa, bir tane oluşturmak için aşağıdaki düğmeyi seçin:

[![Azure’a dağıtma](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-storage-account-create%2fazuredeploy.json)

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Arama kutusuna **Kaynak Gezgini**' ni girip **Kaynak Gezgini**' ı seçin.

    ![Azure portal Kaynak Gezgini](./media/view-resources/azure-portal-resource-explorer.png)

1. Solda, **abonelikler**' i genişletin ve ardından Azure aboneliğinizi genişletin. Depolama hesabını **sağlayıcılar** ya da **ResourceGroups**altında bulabilirsiniz.

    ![Azure portal Kaynak Gezgini](./media/view-resources/azure-portal-resource-explorer-home.png)

    - **Sağlayıcılar**: **Providers**  ->  **Microsoft. Storage**  ->  **storageaccounts**sağlayıcıları ' nı genişletin ve ardından depolama hesabınızı seçin.
    - **ResourceGroups**: depolama hesabını içeren kaynak grubunu seçin, **kaynaklar**' ı seçin ve ardından Depolama hesabını seçin.

    Sağ tarafta, var olan depolama hesabının SKU yapılandırmasını şuna benzer şekilde görürsünüz:

    ![Azure portal Kaynak Gezgini depolama hesabı SKU 'su](./media/view-resources/azure-portal-resource-explorer-sku.png)

## <a name="use-resourcesazurecom"></a>Resources.azure.com kullanma

Resources.azure.com, bir Azure aboneliğine sahip herkes tarafından genel bir Web sitesine erişilebilir. Önizlemededir.  Bunun yerine [Kaynak Gezgini](#use-resource-explorer) kullanmayı düşünün. Bu araç aşağıdaki işlevleri sağlar:

- Azure Kaynak yönetimi API 'Lerini bulun.
- API belgelerini ve şema bilgilerini alın.
- API çağrılarını doğrudan kendi aboneliklerinizde yapın.

Bu aracı kullanarak şema bilgilerinin nasıl alınacağını göstermek için bir depolama hesabınız olması gerekir. Yoksa, bir tane oluşturmak için aşağıdaki düğmeyi seçin:

[![Azure’a dağıtma](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3a%2f%2fraw.githubusercontent.com%2fAzure%2fazure-quickstart-templates%2fmaster%2f101-storage-account-create%2fazuredeploy.json)

1. [Resources.Azure.com](https://resources.azure.com/)konumuna gidin. Aracın sol bölmeyi popüler bir süre sürer.
1. **Abonelikler**' i seçin.

    ![resource.azure.com API eşlemesi](./media/view-resources/resources-azure-com-api-mapping.png)

    Soldaki düğüm sağ taraftaki API çağrısıyla eşleşir. **Al** DÜĞMESINI seçerek API çağrısını yapabilirsiniz.
1. Solda, **abonelikler**' i genişletin ve ardından Azure aboneliğinizi genişletin. Depolama hesabını **sağlayıcılar** ya da **ResourceGroups**altında bulabilirsiniz.

    - **Sağlayıcılar**: **Providers**  ->  **Microsoft. Storage**  ->  **storageaccounts**sağlayıcıları ' nı genişletin ve depolama hesabına gidin.
    - **ResourceGroups**: depolama hesabını içeren kaynak grubunu seçin ve ardından **kaynaklar**' ı seçin.

    Sağ tarafta, var olan depolama hesabının SKU yapılandırmasını şuna benzer şekilde görürsünüz:

    ![Azure portal Kaynak Gezgini depolama hesabı SKU 'su](./media/view-resources/azure-portal-resource-explorer-sku.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, şablon şeması bilgilerini bulmayı öğrendiniz. Kaynak Yöneticisi şablonları oluşturma hakkında daha fazla bilgi edinmek için bkz. [ARM şablonlarının yapısını ve sözdizimini anlayın](./template-syntax.md).

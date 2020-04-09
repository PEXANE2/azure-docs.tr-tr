---
title: Şablon başvurukullanma
description: Şablon oluşturmak için Azure Kaynak Yöneticisi şablonu başvurularını kullanın.
author: mumian
ms.date: 03/27/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: b713d508a5e28291778d3727c15e12972eea3a77
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878533"
---
# <a name="tutorial-utilize-the-resource-manager-template-reference"></a>Öğretici: Kaynak Yöneticisi şablon uytun

Şablon şema bilgilerini nasıl bulacağınızı öğrenin ve azure kaynak yöneticisi (ARM) şablonları oluşturmak için bilgileri kullanın.

Bu öğreticide Azure Hızlı Başlangıç şablonları arasından temel bir şablon kullanırsınız. Şablon başvuru belgelerini kullanarak şablonu özelleştirebilirsiniz.

![Kaynak Yöneticisi şablon başvuru dağıtma depolama hesabı](./media/template-tutorial-use-template-reference/resource-manager-template-tutorial-deploy-storage-account.png)

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablon biçimini anlama
> * Şablon başvurusunu bulma
> * Şablonu düzenleme
> * Şablonu dağıtma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code ve Resource Manager Araçları uzantısı. Bkz. [ARM şablonları oluşturmak için Görsel Stüdyo Kodunu Kullan.](use-vs-code-to-create-template.md)

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

[Azure Quickstart Şablonları,](https://azure.microsoft.com/resources/templates/) ARM şablonları için bir depodur. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu hızlı başlangıçta kullanılan şablon [Standart depolama hesabı oluşturma](https://azure.microsoft.com/resources/templates/101-storage-account-create/) olarak adlandırılır. Şablon, Azure Depolama hesabı kaynağını tanımlar.

1. Visual Studio Code'dan **Dosya**>**Aç Dosya'yı**seçin.
1. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

1. Dosyayı açmak için **Aç**’ı seçin.
1. Dosyayı **azuredeploy.json** olarak yerel bilgisayarınıza kaydetmek için**Dosya Yı Kaydet'i** **seçin.**>

## <a name="understand-the-schema"></a>Şemayı anlama

1. VS Code'da şablonu kök düzeye daraltın. Aşağıdaki öğelere sahip çok basit bir yapı görürsünüz:

    ![Resource Manager şablonu basit yapısı](./media/template-tutorial-use-template-reference/resource-manager-template-simplest-structure.png)

    * **$schema**: Şablon dilinin sürümünü tanımlayan JSON şema dosyasının konumunu belirtin.
    * **contentVersion**: Şablonunuzdaki önemli değişiklikleri belirtmek için bu öğeye istediğiniz değeri verebilirsiniz.
    * **parameters**: Dağıtım, kaynak dağıtımını özelleştirme amacıyla yürütüldüğünde sağlanan değerleri belirtin.
    * **variables**: Şablon dili ifadelerini basitleştirmek için şablonda JSON parçaları olarak kullanılan değerleri belirtin.
    * **resources**: Kaynak grubunda dağıtılan veya güncelleştirilen kaynak türlerini belirtin.
    * **outputs**: Dağıtım sonrasında döndürülen değerleri belirtin.

1. **resources** bölümünü genişletin. Tanımlı bir `Microsoft.Storage/storageAccounts` kaynağı olduğunu göreceksiniz. SKU adı bir parametre değeri kullanır.  Parametre **storageAccountType**denir.

    ![Resource Manager şablonu depolama hesabı tanımı](./media/template-tutorial-use-template-reference/resource-manager-template-storage-resource.png)

1. **StorageAccountType'ın** nasıl tanımlandığını görmek için **parametreleri** genişletin. Parametrenin izin verilen dört değeri vardır. İzin verilen diğer değerleri bulur ve parametre tanımını gözden geçirirsiniz.

    ![Kaynak Yöneticisi şablon depolama hesabı kaynakları skus](./media/template-tutorial-use-template-reference/resource-manager-template-storage-resources-skus-old.png)

## <a name="find-the-template-reference"></a>Şablon başvurusunu bulma

1. [Azure Şablonu başvurusuna](https://docs.microsoft.com/azure/templates/)göz atın.
1. Başlık kutusuna **göre Filtre'ye** **göre, depolama hesaplarını**girin ve Başvuru > **Depolama**altında ilk **Depolama Hesaplarını** seçin.

    ![Resource Manager şablon başvurusu depolama hesabı](./media/template-tutorial-use-template-reference/resource-manager-template-resources-reference-storage-accounts.png)

    Bir kaynak sağlayıcısının genellikle birkaç API sürümü vardır:

    ![Kaynak Yöneticisi şablon başvuru depolama hesabı sürümleri](./media/template-tutorial-use-template-reference/resource-manager-template-resources-reference-storage-accounts-versions.png)

1. Sol bölmeden **Depolama** altındaki **tüm kaynakları** seçin. Bu sayfa, depolama kaynak sağlayıcısının kaynak türlerini ve sürümlerini listeler. Şablonunuzda tanımlanan kaynak türleri için en son API sürümlerini kullanmanız önerilir.

    ![Kaynak Yöneticisi şablon başvuru depolama hesabı türleri sürümleri](./media/template-tutorial-use-template-reference/resource-manager-template-resources-reference-storage-accounts-types-versions.png)

1. **storageAccount** kaynak türünün en son sürümünü seçin. Bu makalenin yazıldığı son sürüm **2019-06-01'dir.** Bu sürümün şablonunuzdaki depolama hesabı kaynağı için kullanılan sürümle eşleştiğinden emin olun. API sürümünü güncellerseniz, kaynak tanımının şablon başvurusuyla eşleştiğini doğrulayın.

1. Bu sayfa, storageAccount kaynak türünün ayrıntılarını listeler.  Örneğin, **Sku** nesnesi için izin verilen değerleri listeler. Daha önce açtığınız hızlı başlangıç şablonunda listelenenden daha fazla skus vardır. Kullanılabilir tüm depolama türlerini içerecek şekilde hızlı başlangıç şablonu özelleştirebilirsiniz.

    ![Kaynak Yöneticisi şablon başvuru depolama hesabı skus](./media/template-tutorial-use-template-reference/resource-manager-template-resources-reference-storage-accounts-skus.png)

## <a name="edit-the-template"></a>Şablonu düzenleme

Visual Studio Code'dan, aşağıdaki ekran görüntüsünde gösterildiği gibi ek depolama hesabı türlerini ekleyin:

![Kaynak Yöneticisi şablon depolama hesabı kaynakları](./media/template-tutorial-use-template-reference/resource-manager-template-storage-resources-skus.png)

## <a name="deploy-the-template"></a>Şablonu dağıtma

Dağıtım yordamı için Visual Studio Code hızlı başlangıçta [Şablonu dağıtma](quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) bölümüne bakın. Şablonu dağıttığınızda, **örneğin, Premium_ZRS,** yeni eklenen bir değerle **storageAccountType** parametresini belirtin. **Premium_ZRS** izin verilen bir değer olmadığından, özgün quickstart şablonu kullanırsanız dağıtım başarısız olur.  Parametre değerini geçmek için dağıtım komutuna aşağıdaki anahtarı ekleyin:

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli
--parameters storageAccountType='Premium_ZRS'
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell
-storageAccountType "Premium_ZRS"
```

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şablon başvurusunu kullanarak var olan bir şablonu özelleştirmeyi öğrendiniz. Birden çok depolama hesabı örneği oluşturmayı öğrenmek için, bkz:

> [!div class="nextstepaction"]
> [Birden çok örnek oluşturma](./template-tutorial-create-multiple-instances.md)

---
title: Şablon başvurusu kullanma
description: Şablon oluşturmak için Azure Resource Manager şablonu başvurusunu kullanın.
author: mumian
ms.date: 04/23/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: 12990238455046d837b175318225bb4f3d317706
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82185056"
---
# <a name="tutorial-utilize-the-resource-manager-template-reference"></a>Öğretici: Kaynak Yöneticisi şablonu başvurusunu kullanma

Şablon Şeması bilgilerini bulmayı ve bilgileri kullanarak Azure Resource Manager (ARM) şablonları oluşturmayı öğrenin.

Bu öğreticide Azure Hızlı Başlangıç şablonları arasından temel bir şablon kullanırsınız. Şablon başvuru belgelerini kullanarak şablonu özelleştirirsiniz.

![Kaynak Yöneticisi şablonu başvurusunu dağıt depolama hesabı](./media/template-tutorial-use-template-reference/resource-manager-template-tutorial-deploy-storage-account.png)

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablon biçimini anlama
> * Şablon başvurusunu bulma
> * Şablonu düzenleme
> * Şablonu dağıtma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code ve Resource Manager Araçları uzantısı. [ARM şablonları oluşturmak için Visual Studio Code kullanma](use-vs-code-to-create-template.md)konusuna bakın.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

[Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/) , ARM şablonları için bir depodur. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu hızlı başlangıçta kullanılan şablon [Standart depolama hesabı oluşturma](https://azure.microsoft.com/resources/templates/101-storage-account-create/) olarak adlandırılır. Şablon, Azure Depolama hesabı kaynağını tanımlar.

1. Visual Studio Code **Dosya**>**Aç dosya**' yı seçin.
1. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

1. Dosyayı açmak için **Aç**’ı seçin.
1. Dosyayı yerel bilgisayarınıza **azuredeploy. JSON** olarak kaydetmek için **Dosya**>**farklı kaydet** ' i seçin.

## <a name="understand-the-schema"></a>Şemayı anlama

1. VS Code'da şablonu kök düzeye daraltın. Aşağıdaki öğelere sahip çok basit bir yapı görürsünüz:

    ![Resource Manager şablonu basit yapısı](./media/template-tutorial-use-template-reference/resource-manager-template-simplest-structure.png)

    * **$schema**: Şablon dilinin sürümünü tanımlayan JSON şema dosyasının konumunu belirtin.
    * **contentVersion**: Şablonunuzdaki önemli değişiklikleri belirtmek için bu öğeye istediğiniz değeri verebilirsiniz.
    * **parameters**: Dağıtım, kaynak dağıtımını özelleştirme amacıyla yürütüldüğünde sağlanan değerleri belirtin.
    * **variables**: Şablon dili ifadelerini basitleştirmek için şablonda JSON parçaları olarak kullanılan değerleri belirtin.
    * **resources**: Kaynak grubunda dağıtılan veya güncelleştirilen kaynak türlerini belirtin.
    * **outputs**: Dağıtım sonrasında döndürülen değerleri belirtin.

1. **resources** bölümünü genişletin. Tanımlı bir `Microsoft.Storage/storageAccounts` kaynağı olduğunu göreceksiniz. SKU adı bir parametre değeri kullanır.  Parametreye **Storageaccounttype**adı verilir.

    ![Resource Manager şablonu depolama hesabı tanımı](./media/template-tutorial-use-template-reference/resource-manager-template-storage-resource.png)

1. **Storageaccounttype** 'in nasıl tanımlandığını görmek için **Parametreler** ' i genişletin. Parametrenin dört izin verilen değeri vardır. İzin verilen diğer değerleri bulacak ve ardından parametre tanımını düzelteceksiniz.

    ![Kaynak Yöneticisi Şablon depolama hesabı kaynakları SKU 'ları](./media/template-tutorial-use-template-reference/resource-manager-template-storage-resources-skus-old.png)

## <a name="find-the-template-reference"></a>Şablon başvurusunu bulma

1. [Azure şablon başvurusuna](https://docs.microsoft.com/azure/templates/)gidin.
1. **Başlığa göre filtrele** kutusuna **depolama hesapları**' nı girin ve **başvuru > depolama**alanındaki ilk **depolama hesaplarını** seçin.

    ![Resource Manager şablon başvurusu depolama hesabı](./media/template-tutorial-use-template-reference/resource-manager-template-resources-reference-storage-accounts.png)

    Kaynak sağlayıcısı genellikle birkaç API sürümüne sahiptir:

    ![Kaynak Yöneticisi şablonu başvuru depolama hesabı sürümleri](./media/template-tutorial-use-template-reference/resource-manager-template-resources-reference-storage-accounts-versions.png)

1. Sol bölmedeki **depolama** alanındaki **tüm kaynaklar** ' ı seçin. Bu sayfa, depolama kaynak sağlayıcısının kaynak türlerini ve sürümlerini listeler. Şablonunuzda tanımlanan kaynak türleri için en son API sürümlerini kullanmanız önerilir.

    ![Kaynak Yöneticisi şablonu başvuru depolama hesabı türleri sürümleri](./media/template-tutorial-use-template-reference/resource-manager-template-resources-reference-storage-accounts-types-versions.png)

1. **Storageaccount** kaynak türünün en son sürümünü seçin. Bu makale yazıldığında en son sürüm **2019-06-01** ' dir. Bu sürümün, şablonunuzda depolama hesabı kaynağı için kullanılan sürümle eşleştiğinden emin olun. API sürümünü güncelleştirirseniz, kaynak tanımının şablon başvurusuyla eşleştiğini doğrulayın.

1. Bu sayfa, storageAccount kaynak türünün ayrıntılarını listeler.  Örneğin, **SKU** nesnesi için izin verilen değerleri listeler. Daha önce açtığınız hızlı başlangıç şablonunda listelenenlerden daha fazla SKU vardır. Hızlı başlangıç şablonunu tüm kullanılabilir depolama türlerini içerecek şekilde özelleştirebilirsiniz.

    ![Kaynak Yöneticisi şablonu başvuru depolama hesabı SKU 'ları](./media/template-tutorial-use-template-reference/resource-manager-template-resources-reference-storage-accounts-skus.png)

## <a name="edit-the-template"></a>Şablonu düzenleme

Visual Studio Code, aşağıdaki ekran görüntüsünde gösterildiği gibi ek depolama hesabı türlerini ekleyin:

![Kaynak Yöneticisi şablonu depolama hesabı kaynakları](./media/template-tutorial-use-template-reference/resource-manager-template-storage-resources-skus.png)

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. [Azure Cloud Shell](https://shell.azure.com) oturum açın

1. Sol üst köşedeki **PowerShell** veya **Bash** (CLI için) öğesini seçerek tercih ettiğiniz ortamı seçin.  Geçiş yaptığınızda kabuğun yeniden başlatılması gerekir.

    ![Dosyayı karşıya yükleme Cloud Shell Azure portal](./media/template-tutorial-use-template-reference/azure-portal-cloud-shell-upload-file.png)

1. **Dosyaları karşıya yükle/indir**'i seçin ve sonra da **Karşıya Yükle**'yi seçin. Önceki ekran görüntüsüne bakın. Önceki bölümde kaydettiğiniz dosyayı seçin. Dosyayı karşıya yükledikten sonra, dosyanın başarıyla karşıya yüklendiğini doğrulamak için **ls** komutunu ve **Cat** komutunu kullanabilirsiniz.

1. Cloud Shell aşağıdaki komutları çalıştırın. PowerShell kodunu veya CLI kodunu gösteren sekmeyi seçin.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ```azurecli
    echo "Enter a project name that is used to generate resource group name:" &&
    read projectName &&
    echo "Enter the location (i.e. centralus):" &&
    read location &&
    resourceGroupName="${projectName}rg" &&
    az group create --name $resourceGroupName --location "$location" &&
    az deployment group create --resource-group $resourceGroupName --template-file "$HOME/azuredeploy.json" --parameters storageAccountType='Standard_RAGRS'
    ```

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    $projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${projectName}rg"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "$HOME/azuredeploy.json" -storageAccountType "Standard_RAGRS"
    ```

    ---

 Şablonu dağıttığınızda, yeni eklenen bir değerle **Storageaccounttype** parametresini belirtin, örneğin, **Standard_RAGRS**. **Standard_RAGRS** izin verilen bir değer olmadığından, özgün hızlı başlangıç şablonunu kullanırsanız, dağıtım başarısız olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şablon başvurusunu kullanarak var olan bir şablonu özelleştirmeyi öğrendiniz. Birden çok depolama hesabı örneği oluşturmayı öğrenmek için, bkz:

> [!div class="nextstepaction"]
> [Birden çok örnek oluşturma](./template-tutorial-create-multiple-instances.md)

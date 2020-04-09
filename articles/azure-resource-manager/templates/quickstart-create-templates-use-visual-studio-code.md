---
title: Şablon oluşturma - Visual Studio Code
description: Resource Manager şablonları üzerinde çalışmak için Visual Studio Code ve Azure Resource Manager araçları eklentisini kullanın.
author: mumian
ms.date: 03/04/2019
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: a0c80f18e9cd09b765804aaddbd178b4b3e32a9d
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80984461"
---
# <a name="quickstart-create-arm-templates-by-using-visual-studio-code"></a>Quickstart: Visual Studio Code kullanarak ARM şablonları oluşturma

Azure Kaynak Yöneticisi (ARM) şablonları oluşturmak ve bunları yeniden oluşturmak için Visual Studio kodunu ve Azure Kaynak Yöneticisi Araçları uzantısını nasıl kullanacağınızı öğrenin. Uzantı olmadan Visual Studio Code'da ARM şablonları oluşturabilirsiniz, ancak uzantı şablon geliştirmeyi basitleştiren otomatik tamamlama seçenekleri sağlar. Azure çözümlerinizi dağıtma ve yönetme yle ilgili kavramları anlamak için [şablon dağıtımına genel bakış'a](overview.md)bakın.

Bu hızlı başlatmada, bir depolama hesabı dağıtın:

![kaynak yöneticisi şablonu quickstart visual studio kod diyagramı](./media/quickstart-create-templates-use-visual-studio-code/resource-manager-template-quickstart-vscode-diagram.png)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

- [Görsel Stüdyo Kodu](https://code.visualstudio.com/).
- Resource Manager Araçları uzantısı. Yüklemek için şu adımları kullanın:

    1. Visual Studio Code'u açın.
    2. Uzantılar bölmesini açmak için **CTRL+SHIFT+X** tuşlarına basın
    3. **Azure Resource Manager Araçları**’nı arayın ve sonra **Yükle**’yi seçin.
    4. Uzantı yüklemesini tamamlamak için **Yeniden Yükle**’yi seçin.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Sıfırdan şablon oluşturmak yerine, [Azure Hızlı Başlangıç Şablonları](https://azure.microsoft.com/resources/templates/)’ndan bir şablon açarsınız. Azure Quickstart Şablonları, ARM şablonları için bir depodur.

Bu hızlı başlangıçta kullanılan şablon [Standart depolama hesabı oluşturma](https://azure.microsoft.com/resources/templates/101-storage-account-create/) olarak adlandırılır. Şablon, Azure Depolama hesabı kaynağını tanımlar.

1. Visual Studio Code'dan **Dosya**>**Aç Dosya'yı**seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

3. Dosyayı açmak için **Aç**’ı seçin.
4. Dosyayı **azuredeploy.json** olarak yerel bilgisayarınıza kaydetmek için**Dosya Yı Kaydet'i** **seçin.**>

## <a name="edit-the-template"></a>Şablonu düzenleme

Visual Studio Code'u kullanarak şablonun nasıl edinilir olduğunu `outputs` deneyimlemek için, depolama URI'sini göstermek için bölüme bir öğe daha eklersiniz.

1. Bir veya birden çok çıkışı dışarı aktarılan şablona ekleyin:

    ```json
    "storageUri": {
      "type": "string",
      "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
    }
    ```

    Bitirdiğinizde çıkışlar bölümü şöyle görünür:

    ```json
    "outputs": {
      "storageAccountName": {
        "type": "string",
        "value": "[variables('storageAccountName')]"
      },
      "storageUri": {
        "type": "string",
        "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
      }
    }
    ```

    Kodu kopyalayıp Visual Studio Code'un içine yapıştırdıysanız, Resource Manager Araçları uzantısının IntelliSense özelliğini denemek için **value** öğesini bir kez daha yazmayı deneyin.

    ![Resource Manager şablonu Visual Studio Code IntelliSense](./media/quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. Dosyayı kaydetmek için **Dosya**>**Kaydet'i** seçin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Şablonları dağıtmak için birçok yöntem vardır. Azure Bulut kabuğu bu hızlı başlatmada kullanılır. Bulut kabuğu hem Azure CLI'yi hem de Azure PowerShell'i destekler. CLI ve PowerShell arasında seçim yapmak için sekme seçiciyi kullanın.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

1. [Azure Bulutu kabuğunda](https://shell.azure.com) oturum açın

2. Sol üst köşedeki **PowerShell** veya **Bash**(CLI) seçeneğini seçerek tercih ettiğiniz ortamı seçin.  Geçiş yaptığınızda kabuğun yeniden başlatılması gerekir.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ![Azure portal Cloud shell CLI](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-cli.png)

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure portalı Bulut kabuğu PowerShell](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-powershell.png)

    ---

3. **Dosyaları karşıya yükle/indir**'i seçin ve sonra da **Karşıya Yükle**'yi seçin.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ![Azure portal Cloud shell dosya karşıya yükleme](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file.png)

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure portal Cloud shell dosya karşıya yükleme](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file-powershell.png)

    ---

    Önceki bölümde kaydettiğiniz dosyayı seçin. Varsayılan ad **azuredeploy.json** olur. Şablon dosyasına kabuktan erişilebilir olmalıdır.

    Dosyanın başarıyla yüklendiğini doğrulamak için isteğe bağlı olarak **lS** komutunu ve **cat** komutunu kullanabilirsiniz.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ![Azure portal Cloud Shell dosya listeleme](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file.png)

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure portal Cloud Shell dosya listeleme](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file-powershell.png)

    ---
4. Cloud Shell’den aşağıdaki komutları çalıştırın. PowerShell kodunu veya CLI kodunu gösteren sekmeyi seçin.

    # <a name="cli"></a>[CLI](#tab/CLI)

    ```azurecli
    echo "Enter a project name that is used to generate resource group name:" &&
    read projectName &&
    echo "Enter the location (i.e. centralus):" &&
    read location &&
    resourceGroupName="${projectName}rg" &&
    az group create --name $resourceGroupName --location "$location" &&
    az deployment group create --resource-group $resourceGroupName --template-file "$HOME/azuredeploy.json"
    ```

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    $projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${projectName}rg"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "$HOME/azuredeploy.json"
    ```

    ---

    Dosyayı **azuredeploy.json** dışında bir adla kaydederseniz şablon dosyasının adını güncelleştirin.

    Aşağıdaki ekran görüntüsünde bir örnek dağıtım gösterilmektedir:

    # <a name="cli"></a>[CLI](#tab/CLI)

    ![Azure portal Cloud Shell şablon dağıtımı](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure portal Cloud Shell şablon dağıtımı](./media/quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template-powershell.png)

    ---

    Çıktı bölümündeki depolama hesabı adı ve depolama URL'si ekran görüntüsünde vurgulanmıştır. Bir sonraki adımda depolama hesabına ihtiyacınız olacak.

5. Yeni oluşturulan depolama hesabını listelemek için aşağıdaki CLI komutunu çalıştırın:

    # <a name="cli"></a>[CLI](#tab/CLI)

    ```azurecli
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the Storage Account name:" &&
    read storageAccountName &&
    az storage account show --resource-group $resourceGroupName --name $storageAccountName
    ```

    # <a name="powershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $storageAccountName = Read-Host -Prompt "Enter the Storage Account name"
    Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
    ```

    ---

Azure depolama hesaplarını kullanma hakkında daha fazla bilgi edinmek için bkz. [Hızlı başlangıç: Azure portalı kullanarak blobları yükleme, indirme ve listeleme](../../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıcın ana odak noktası, Visual Studio Code kullanarak Azure Hızlı Başlangıç şablonlarındaki mevcut bir şablon düzenlemektir. Ayrıca, Azure Bulutu kabuğundan CLI veya PowerShell'i kullanarak şablonu nasıl dağıtabileceğinizi de öğrendiniz. Azure Hızlı Başlangıç şablonlarındaki şablonlar size ihtiyacınız olan her şeyi sağlamayabilir. Şablon geliştirme hakkında daha fazla bilgi edinmek için yeni başlangıç eğitimi serimize bakın:

> [!div class="nextstepaction"]
> [Başlangıç öğretileri](./template-tutorial-create-first-template.md)

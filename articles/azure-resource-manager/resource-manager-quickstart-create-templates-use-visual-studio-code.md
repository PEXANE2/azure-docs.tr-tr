---
title: Azure Resource Manager şablonu oluşturmak için Visual Studio Code kullanma | Microsoft Docs
description: Kaynak Yöneticisi şablonlarda çalışmak için Visual Studio Code ve Azure Resource Manager araçları uzantısını kullanın.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 03/04/2019
ms.topic: quickstart
ms.author: jgao
ms.openlocfilehash: b62d4f8b43c62ff401b4d74932b131c1bf8fd63f
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72169570"
---
# <a name="quickstart-create-azure-resource-manager-templates-by-using-visual-studio-code"></a>Hızlı başlangıç: Visual Studio Code kullanarak Azure Resource Manager şablonları oluşturma

Azure Resource Manager şablonları oluşturmak ve düzenlemek için Visual Studio Code ve Azure Resource Manager araçları uzantısı 'nın nasıl kullanılacağını öğrenin. Uzantı olmadan Visual Studio Code Kaynak Yöneticisi şablonlar oluşturabilirsiniz, ancak uzantı, şablon geliştirmeyi kolaylaştıran otomatik tamamlama seçenekleri sağlar. Azure çözümlerinizi dağıtma ve yönetme ile ilgili kavramları anlamak için bkz. [Azure Resource Manager genel bakış](resource-group-overview.md).

Bu hızlı başlangıçta bir depolama hesabı dağıtırsınız:

![Resource Manager şablonu hızlı başlangıç Visual Studio kod diyagramı](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/resource-manager-template-quickstart-vscode-diagram.png)

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlayabilmeniz için şunlar gerekir:

- [Visual Studio Code](https://code.visualstudio.com/).
- Kaynak Yöneticisi Araçları uzantısı. Yüklemek için aşağıdaki adımları kullanın:

    1. Visual Studio Code açın.
    2. Uzantılar bölmesini açmak için **CTRL + SHIFT + X** tuşlarına basın
    3. **Azure Resource Manager araçları**araması yapın ve ardından **Install**' ı seçin.
    4. Uzantı yüklemesini sona bırakmak için **yeniden yükle** ' yi seçin.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonu açma

Sıfırdan şablon oluşturmak yerine [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/)bir şablon açarsınız. Azure hızlı başlangıç şablonları Kaynak Yöneticisi şablonlar için bir depodur.

Bu hızlı başlangıçta kullanılan şablona [Standart depolama hesabı oluşturma](https://azure.microsoft.com/resources/templates/101-storage-account-create/)adı verilir. Şablon bir Azure depolama hesabı kaynağını tanımlar.

1. Visual Studio Code, **dosya**> dosya**Aç**' ı seçin.
2. **Dosya adı**' nda aşağıdaki URL 'yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

3. Dosyayı açmak için **Aç** ' ı seçin.
4. Dosyayı yerel bilgisayarınıza **azuredeploy. JSON** olarak kaydetmek için **Dosya**>**farklı kaydet** ' i seçin.

## <a name="edit-the-template"></a>Şablonu düzenleme

Visual Studio Code kullanarak bir şablonu düzenleme konusunda bilgi için, depolama URI 'sini göstermek üzere `outputs` bölümüne bir öğe ekleyin.

1. Çıkarılan şablona bir çıktı daha ekleyin:

    ```json
    "storageUri": {
      "type": "string",
      "value": "[reference(variables('storageAccountName')).primaryEndpoints.blob]"
    }
    ```

    İşiniz bittiğinde, çıktılar bölümü şöyle görünür:

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

    Kodu Visual Studio Code içine kopyalayıp yapıştırdıysanız, Kaynak Yöneticisi Araçları uzantısının IntelliSense özelliğiyle karşılaşabilmesi için **Value** öğesini yeniden yazma işlemi yapmayı deneyin.

    ![Kaynak Yöneticisi Template Visual Studio Code IntelliSense](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/resource-manager-templates-visual-studio-code-intellisense.png)

2. Dosyayı kaydetmek için **dosya**>**Kaydet** ' i seçin.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Şablon dağıtmak için birçok yöntem vardır. Azure Cloud Shell bu hızlı başlangıçta kullanılır. Cloud Shell hem Azure CLı hem de Azure PowerShell destekler. CLı ve PowerShell arasında seçim yapmak için sekme seçiciyi kullanın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. [Azure Cloud Shell](https://shell.azure.com) 'de oturum açın

2. Sol üst köşedeki **PowerShell** veya **Bash**(CLI) öğesini seçerek tercih ettiğiniz ortamı seçin.  ' İ değiştirdiğinizde kabuğu yeniden başlatmanız gerekir.

    # <a name="clitabcli"></a>[CLı](#tab/CLI)

    ![Azure portal Cloud Shell CLı](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-cli.png)

    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure portal Cloud Shell PowerShell](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-choose-powershell.png)

    ---

3. **Dosyaları karşıya yükle/indir**ve sonra **karşıya**Yükle ' yi seçin.

    # <a name="clitabcli"></a>[CLı](#tab/CLI)

    ![Azure portal Cloud Shell karşıya yükleme dosyası](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file.png)

    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure portal Cloud Shell karşıya yükleme dosyası](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-upload-file-powershell.png)

    ---

    Önceki bölümde kaydettiğiniz dosyayı seçin. Varsayılan ad **azuredeploy. JSON**' dir. Şablon dosyası kabuktan erişilebilir olmalıdır.

    Dosyanın başarıyla karşıya yüklendiğini doğrulamak için, isteğe bağlı olarak **ls** komutunu ve **Cat** komutunu da kullanabilirsiniz.

    # <a name="clitabcli"></a>[CLı](#tab/CLI)

    ![Azure portal Cloud Shell liste dosyası](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file.png)

    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure portal Cloud Shell liste dosyası](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-list-file-powershell.png)

    ---
4. Cloud Shell 'den aşağıdaki komutları çalıştırın. PowerShell kodunu veya CLı kodunu göstermek için sekmeyi seçin.

    # <a name="clitabcli"></a>[CLı](#tab/CLI)
    ```azurecli
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the location (i.e. centralus):" &&
    read location &&
    az group create --name $resourceGroupName --location "$location" &&
    az group deployment create --resource-group $resourceGroupName --template-file "$HOME/azuredeploy.json"
    ```

    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "$HOME/azuredeploy.json"
    ```

    ---

    Dosyayı **azuredeploy. JSON**dışında bir ada kaydederseniz şablon dosya adını güncelleştirin.

    Aşağıdaki ekran görüntüsünde örnek bir dağıtım görülmektedir:

    # <a name="clitabcli"></a>[CLı](#tab/CLI)

    ![Azure portal Cloud Shell dağıtım şablonu](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template.png)

    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)

    ![Azure portal Cloud Shell dağıtım şablonu](./media/resource-manager-quickstart-create-templates-use-visual-studio-code/azure-portal-cloud-shell-deploy-template-powershell.png)

    ---

    Çıktı bölümünde depolama hesabı adı ve depolama URL 'SI, ekran görüntüsünde vurgulanır. Sonraki adımda depolama hesabı adının olması gerekir.

5. Yeni oluşturulan depolama hesabını listelemek için aşağıdaki CLı veya PowerShell komutunu çalıştırın:

    # <a name="clitabcli"></a>[CLı](#tab/CLI)
    ```azurecli
    echo "Enter the Resource Group name:" &&
    read resourceGroupName &&
    echo "Enter the Storage Account name:" &&
    read storageAccountName &&
    az storage account show --resource-group $resourceGroupName --name $storageAccountName
    ```

    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)

    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $storageAccountName = Read-Host -Prompt "Enter the Storage Account name"
    Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName
    ```

    ---

Azure depolama hesaplarını kullanma hakkında daha fazla bilgi edinmek için bkz. [hızlı başlangıç: Azure Portal kullanarak Blobları karşıya yükleme, indirme ve listeleme](../storage/blobs/storage-quickstart-blobs-portal.md).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure kaynaklarına artık ihtiyaç duyulmadığında, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubunda toplam altı kaynak görürsünüz.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç 'nin ana odağı, Azure hızlı başlangıç şablonlarından mevcut bir şablonu düzenlemek için Visual Studio Code kullanmaktır. Ayrıca, Azure Cloud Shell 'den CLı veya PowerShell kullanarak şablonu dağıtmayı öğrendiniz. Azure hızlı başlangıç şablonlarından şablonlar, ihtiyacınız olan her şeyi sunmayabilir. Şablon geliştirme hakkında daha fazla bilgi edinmek için bkz. Yeni Başlangıç Öğreticisi serisi:

> [!div class="nextstepaction"]
> [Başlangıç öğreticileri](./template-tutorial-create-first-template.md)

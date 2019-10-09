---
title: Azure Resource Manager şablonlarıyla sanal makine uzantıları dağıtma | Microsoft Docs
description: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtmayı öğrenin
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 11/13/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 338054aadbf04c6c6e2b496677476c2c5634b6ba
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72169295"
---
# <a name="tutorial-deploy-virtual-machine-extensions-with-azure-resource-manager-templates"></a>Öğretici: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtma

Azure VM 'lerinde dağıtım sonrası yapılandırma ve otomasyon görevlerini gerçekleştirmek için [Azure sanal makine uzantıları](../virtual-machines/extensions/features-windows.md) 'nı nasıl kullanacağınızı öğrenin. Birçok farklı VM uzantısı, Azure VM 'Leri ile kullanılabilir. Bu öğreticide, bir Windows sanal makinesinde PowerShell betiğini çalıştırmak için bir Azure Resource Manager şablonundan özel bir betik uzantısı dağıtırsınız.  Betik Web sunucusunu sanal makineye yüklüyor.

Bu öğretici aşağıdaki görevleri ele almaktadır:

> [!div class="checklist"]
> * PowerShell betiği hazırlama
> * Hızlı başlangıç şablonu açma
> * Şablonu düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlayabilmeniz için şunlar gerekir:

* Kaynak Yöneticisi Araçları uzantısıyla [Visual Studio Code](https://code.visualstudio.com/) . Bkz. [uzantıyı yüklemeyi](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Güvenliği artırmak için, sanal makine yöneticisi hesabı için oluşturulan bir parola kullanın. Parola oluşturmak için bir örnek aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Azure Key Vault, şifreleme anahtarlarını ve diğer gizli dizileri korumak için tasarlanmıştır. Daha fazla bilgi için bkz. [öğretici: Kaynak Yöneticisi Şablon dağıtımı Azure Key Vault tümleştirme](./resource-manager-tutorial-use-key-vault.md). Parolanızı her üç ayda bir de güncelleştirmenizi öneririz.

## <a name="prepare-a-powershell-script"></a>PowerShell betiği hazırlama

[GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-vm-extension/installWebServer.ps1)'dan paylaşılan aşağıdaki içeriğe sahip bir PowerShell betiği:

```azurepowershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Dosyayı kendi konumunuzda yayımlamayı seçerseniz, daha sonra öğreticideki `fileUri` öğesini güncelleştirmeniz gerekir.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonu açma

Azure hızlı başlangıç şablonları Kaynak Yöneticisi şablonlar için bir depodur. Sıfırdan şablon oluşturmak yerine, örnek bir şablon bulabilir ve özelleştirebilirsiniz. Bu öğreticide kullanılan şablona [basit bir WINDOWS VM dağıtımı](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/)denir.

1. Visual Studio Code, **dosya** >  dosya**Aç**' ı seçin.
1. **Dosya adı** kutusuna şu URL 'yi yapıştırın: https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json

1. Dosyayı açmak için **Aç**' ı seçin.
    Şablon beş kaynağı tanımlar:

   * **Microsoft. Storage/storageAccounts**. [Şablon başvurusuna](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts)bakın.
   * **Microsoft. Network/Publicıpaddresses**. [Şablon başvurusuna](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses)bakın.
   * **Microsoft. Network/virtualNetworks**. [Şablon başvurusuna](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks)bakın.
   * **Microsoft. Network/NetworkInterfaces**. [Şablon başvurusuna](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces)bakın.
   * **Microsoft. COMPUTE/virtualMachines**. [Şablon başvurusuna](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines)bakın.

     Özelleştirebilmeniz için önce şablon hakkında bazı temel bilgileri almanız yararlı olur.

1. **Dosya @no__t-** 2**farklı kaydet**' i seçerek dosyanın bir kopyasını yerel bilgisayarınıza *azuredeploy. JSON* adıyla kaydedin.

## <a name="edit-the-template"></a>Şablonu düzenleme

Mevcut şablona aşağıdaki içeriğe sahip bir sanal makine uzantısı kaynağı ekleyin:

```json
{
    "apiVersion": "2018-06-01",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "[concat(variables('vmName'),'/', 'InstallWebServer')]",
    "location": "[parameters('location')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/',variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.7",
        "autoUpgradeMinorVersion":true,
        "settings": {
            "fileUris": [
                "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-vm-extension/installWebServer.ps1"
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File installWebServer.ps1"
        }
    }
}
```

Bu kaynak tanımı hakkında daha fazla bilgi için [uzantı başvurusuna](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines/extensions)bakın. Aşağıda bazı önemli öğeler verilmiştir:

* **ad**: uzantı kaynağı, sanal makine nesnesinin bir alt kaynağı olduğundan, adın sanal makine adı ön ekine sahip olması gerekir. Bkz. [alt kaynaklar için ad ve tür ayarlama](child-resource-name-type.md).
* **Bağımlıdson**: sanal makineyi oluşturduktan sonra uzantı kaynağını oluşturun.
* **Fileuris**: betik dosyalarının depolandığı konumlar. Belirtilen konumu kullanmayı tercih ederseniz, değerleri güncelleştirmeniz gerekir.
* **Commandtoexecute**: Bu komut betiği çağırır.

## <a name="deploy-the-template"></a>Şablonu dağıtma

Dağıtım yordamı için öğreticinin "şablonu dağıtma" bölümüne bakın [: bağımlı kaynaklarla Azure Resource Manager şablonları oluşturma](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template). Sanal makine yöneticisi hesabı için oluşturulmuş bir parola kullanmanızı öneririz. Bu makalenin [Önkoşullar](#prerequisites) bölümüne bakın.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

1. Azure portal VM 'yi seçin.
1. VM 'ye Genel Bakış ' da, **kopyalamak Için tıklayın ' ı**seçerek IP adresini kopyalayın ve bir tarayıcı sekmesine yapıştırın. Varsayılan Internet Information Services (IIS) karşılama sayfası açılır:

![Internet Information Services hoş geldiniz sayfası](./media/resource-manager-tutorial-deploy-vm-extensions/resource-manager-template-deploy-extensions-customer-script-web-server.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıttığınız Azure kaynaklarına artık ihtiyacınız kalmadığında, kaynak grubunu silerek bunları temizleyin.

1. Azure portal sol bölmedeki **kaynak grubu**' nu seçin.
2. **Ada göre filtrele** kutusuna kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
    Kaynak grubunda altı kaynak görüntülenir.
4. Üstteki menüden **kaynak grubunu sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir sanal makine ve bir sanal makine uzantısı dağıttınız. Uzantı, sanal makinede IIS Web sunucusunu yükledi. Azure SQL veritabanı uzantısının BACPAC dosyasını içeri aktarmak için nasıl kullanılacağını öğrenmek için bkz.:

> [!div class="nextstepaction"]
> [SQL uzantıları dağıtma](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md)

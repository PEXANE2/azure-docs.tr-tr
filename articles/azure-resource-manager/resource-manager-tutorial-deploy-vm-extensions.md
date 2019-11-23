---
title: Deploy VM extensions with template
description: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtmayı öğrenin
author: mumian
ms.date: 11/13/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: c18e5959a1ec52d9f10f27f456a466669e7b99c9
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74325370"
---
# <a name="tutorial-deploy-virtual-machine-extensions-with-azure-resource-manager-templates"></a>Öğretici: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtma

Azure VM'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri gerçekleştirme amacıyla [Azure sanal makine uzantılarını](../virtual-machines/extensions/features-windows.md) kullanmayı öğrenin. Azure VM'leri ile kullanabileceğiniz birçok farklı VM uzantısı vardır. In this tutorial, you deploy a Custom Script extension from an Azure Resource Manager template to run a PowerShell script on a Windows VM.  Bu betik, VM'ye Web Sunucusu yükler.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * PowerShell betiğini hazırlama
> * Hızlı başlangıç şablonunu açma
> * Şablonu düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code with Resource Manager Tools extension. See [Use Visual Studio Code to create Azure Resource Manager templates](./resource-manager-tools-vs-code.md).
* Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için bkz. [Öğretici: Azure Key Vault'u Resource Manager şablonu dağıtımıyla tümleştirme](./resource-manager-tutorial-use-key-vault.md). We also recommend that you update your password every three months.

## <a name="prepare-a-powershell-script"></a>PowerShell betiğini hazırlama

A PowerShell script with the following content is shared from [Github](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-vm-extension/installWebServer.ps1):

```azurepowershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

If you choose to publish the file to your own location, you must update the `fileUri` element in the template later in the tutorial.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure Quickstart Templates is a repository for Resource Manager templates. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma).

1. In Visual Studio Code, select **File** > **Open File**.
1. In the **File name** box, paste the following URL: https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json

1. To open the file, select **Open**.
    The template defines five resources:

   * **Microsoft.Storage/storageAccounts**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * **Microsoft.Network/publicIPAddresses**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * **Microsoft.Network/virtualNetworks**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * **Microsoft.Network/networkInterfaces**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * **Microsoft.Compute/virtualMachines**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

     It's helpful to get some basic understanding of the template before you customize it.

1. Save a copy of the file to your local computer with the name *azuredeploy.json* by selecting **File** > **Save As**.

## <a name="edit-the-template"></a>Şablonu düzenleme

Aşağıdaki içeriği kullanarak var olan şablona bir sanal makine uzantısı kaynağı ekleyin:

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

For more information about this resource definition, see the [extension reference](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines/extensions). Önemli öğeler şunlardır:

* **name**: Uzantı kaynağı, sanal makine nesnesinin alt kaynağı olduğundan ad alanında sanal makine adı ön ek olarak kullanılmalıdır. See [Set name and type for child resources](child-resource-name-type.md).
* **dependsOn**: Create the extension resource after you've created the virtual machine.
* **fileUris**: The locations where the script files are stored. If you choose not to use the provided location, you need to update the values.
* **commandToExecute**: This command invokes the script.

## <a name="deploy-the-template"></a>Şablonu dağıtma

For the deployment procedure, see the "Deploy the template" section of [Tutorial: Create Azure Resource Manager templates with dependent resources](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template). We recommended that you use a generated password for the virtual machine administrator account. See this article's [Prerequisites](#prerequisites) section.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

1. In the Azure portal, select the VM.
1. In the VM overview, copy the IP address by selecting **Click to copy**, and then paste it in a browser tab. The default Internet Information Services (IIS) welcome page opens:

![The Internet Information Services welcome page](./media/resource-manager-tutorial-deploy-vm-extensions/resource-manager-template-deploy-extensions-customer-script-web-server.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

When you no longer need the Azure resources you deployed, clean them up by deleting the resource group.

1. In the Azure portal, in the left pane, select **Resource group**.
2. In the **Filter by name** box, enter the resource group name.
3. Kaynak grubu adını seçin.
    Six resources are displayed in the resource group.
4. In the top menu, select **Delete resource group**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir sanal makine ve bir sanal makine uzantısı dağıttınız. Uzantı, sanal makineye IIS web sunucusunu yükledi. To learn how to use the Azure SQL Database extension to import a BACPAC file, see:

> [!div class="nextstepaction"]
> [Deploy SQL extensions](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md)

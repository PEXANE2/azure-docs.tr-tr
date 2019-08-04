---
title: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtma | Microsoft Docs
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
ms.openlocfilehash: a6d0c3e9daba6f4f37778fabde161751944e174a
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2019
ms.locfileid: "68774870"
---
# <a name="tutorial-deploy-virtual-machine-extensions-with-azure-resource-manager-templates"></a>Öğretici: Azure Resource Manager şablonlarıyla sanal makine uzantıları dağıtma

Azure VM'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri gerçekleştirme amacıyla [Azure sanal makine uzantılarını](../virtual-machines/extensions/features-windows.md) kullanmayı öğrenin. Azure VM'leri ile kullanabileceğiniz birçok farklı VM uzantısı vardır. Bu öğreticide, bir Windows sanal makinesinde PowerShell betiğini çalıştırmak için bir Azure Resource Manager şablonundan özel bir betik uzantısı dağıtırsınız.  Bu betik, VM'ye Web Sunucusu yükler.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * PowerShell betiğini hazırlama
> * Hızlı başlangıç şablonunu açma
> * Şablonu düzenleme
> * Şablonu dağıtma
> * Dağıtımı doğrulama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* [Visual Studio Code](https://code.visualstudio.com/) ve Resource Manager Araçları uzantısı. Bkz. [uzantıyı yüklemeyi](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```azurecli-interactive
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için bkz [. Öğretici: Azure Key Vault Kaynak Yöneticisi Şablon dağıtımı](./resource-manager-tutorial-use-key-vault.md)tümleştirin. Parolanızı her üç ayda bir de güncelleştirmenizi öneririz.

## <a name="prepare-a-powershell-script"></a>PowerShell betiğini hazırlama

Aşağıdaki içeriğe sahip bir PowerShell betiği, [genel erişime sahip bir Azure depolama hesabından](https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1)paylaşılır:

```azurepowershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Dosyayı kendi konumunuza yayımlamayı seçerseniz, bu `fileUri` öğeyi öğreticide daha sonra güncelleştirmeniz gerekir.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure hızlı başlangıç şablonları Kaynak Yöneticisi şablonlar için bir depodur. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma).

1. Visual Studio Code **Dosya** > **Aç dosya**' yı seçin.
1. **Dosya adı** kutusuna aşağıdaki URL 'yi yapıştırın: https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json

1. Dosyayı açmak için **Aç**' ı seçin.  
    Şablon beş kaynağı tanımlar:

   * **Microsoft. Storage/storageAccounts**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * **Microsoft. Network/Publicıpaddresses**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * **Microsoft. Network/virtualNetworks**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * **Microsoft. Network/NetworkInterfaces**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * **Microsoft. COMPUTE/virtualMachines**. Bkz. [şablon başvurusu](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

     Özelleştirebilmeniz için önce şablon hakkında bazı temel bilgileri almanız yararlı olur.

1. Dosya**farklı kaydet**' i seçerek > dosyanın bir kopyasını *azuredeploy. JSON* adlı yerel bilgisayarınıza kaydedin.

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
                "https://armtutorials.blob.core.windows.net/usescriptextensions/installWebServer.ps1"
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File installWebServer.ps1"
        }
    }
}
```

Bu kaynak tanımı hakkında daha fazla bilgi için [uzantı başvurusuna](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines/extensions)bakın. Önemli öğeler şunlardır:

* **ad**: Uzantı kaynağı, sanal makine nesnesinin bir alt kaynağı olduğundan, adın sanal makine adı ön ekine sahip olması gerekir. Bkz. [alt kaynaklar için ad ve tür ayarlama](child-resource-name-type.md).
* **Bağımlıdson**: Sanal makineyi oluşturduktan sonra uzantı kaynağını oluşturun.
* **Dosya URI 'leri**: Betik dosyalarının depolandığı konumlar. Belirtilen konumu kullanmayı tercih ederseniz, değerleri güncelleştirmeniz gerekir.
* **Commandtoexecute**: Bu komut betiği çağırır.  

## <a name="deploy-the-template"></a>Şablonu dağıtma

Dağıtım yordamı için [öğreticinin "şablonu dağıtma" bölümüne bakın: Bağımlı kaynaklarla](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template)Azure Resource Manager şablonlar oluşturun. Sanal makine yöneticisi hesabı için oluşturulmuş bir parola kullanmanızı öneririz. Bu makalenin [Önkoşullar](#prerequisites) bölümüne bakın.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

1. Azure portal VM 'yi seçin.
1. VM 'ye Genel Bakış ' da, **kopyalamak Için tıklayın ' ı**seçerek IP adresini kopyalayın ve bir tarayıcı sekmesine yapıştırın.  
   Varsayılan Internet Information Services (IIS) karşılama sayfası açılır:

![Internet Information Services hoş geldiniz sayfası](./media/resource-manager-tutorial-deploy-vm-extensions/resource-manager-template-deploy-extensions-customer-script-web-server.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıttığınız Azure kaynaklarına artık ihtiyacınız kalmadığında, kaynak grubunu silerek bunları temizleyin.

1. Azure portal sol bölmedeki **kaynak grubu**' nu seçin.
2. **Ada göre filtrele** kutusuna kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  
    Kaynak grubunda altı kaynak görüntülenir.
4. Üstteki menüden **kaynak grubunu sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir sanal makine ve bir sanal makine uzantısı dağıttınız. Uzantı, sanal makineye IIS web sunucusunu yükledi. Azure SQL veritabanı uzantısının BACPAC dosyasını içeri aktarmak için nasıl kullanılacağını öğrenmek için bkz.:

> [!div class="nextstepaction"]
> [SQL uzantıları dağıtma](./resource-manager-tutorial-deploy-sql-extensions-bacpac.md)

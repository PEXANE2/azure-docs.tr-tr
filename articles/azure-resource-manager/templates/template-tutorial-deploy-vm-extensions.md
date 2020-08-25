---
title: Şablon ile VM uzantıları dağıtma
description: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtmayı öğrenin
author: mumian
ms.date: 04/23/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: fb99babfd53b26874bed62183871d13ae0ae4baf
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "86120135"
---
# <a name="tutorial-deploy-virtual-machine-extensions-with-arm-templates"></a>Öğretici: ARM şablonlarıyla sanal makine uzantıları dağıtma

Azure VM'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri gerçekleştirme amacıyla [Azure sanal makine uzantılarını](../../virtual-machines/extensions/features-windows.md) kullanmayı öğrenin. Azure VM'leri ile kullanabileceğiniz birçok farklı VM uzantısı vardır. Bu öğreticide, bir Windows sanal makinesinde PowerShell betiğini çalıştırmak için bir Azure Resource Manager (ARM) şablonundan özel bir betik uzantısı dağıtırsınız.  Bu betik, VM'ye Web Sunucusu yükler.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * PowerShell betiğini hazırlama
> * Hızlı başlangıç şablonunu açma
> * Şablonu düzenleme
> * Şablonu dağıtma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code ve Resource Manager Araçları uzantısı. Bkz. [hızlı başlangıç: Visual Studio Code Azure Resource Manager şablonlar oluşturma](quickstart-create-templates-use-visual-studio-code.md).
* Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```console
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için bkz. [öğretici: ARM şablon dağıtımında Azure Key Vault tümleştirme](./template-tutorial-use-key-vault.md). Parolanızı her üç ayda bir de güncelleştirmenizi öneririz.

## <a name="prepare-a-powershell-script"></a>PowerShell betiğini hazırlama

Satır içi PowerShell betiği veya bir betik dosyası kullanabilirsiniz.  Bu öğreticide, bir betik dosyasının nasıl kullanılacağı gösterilmektedir. [GitHub](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-vm-extension/installWebServer.ps1)'dan paylaşılan aşağıdaki içeriğe sahip bir PowerShell betiği:

```azurepowershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Dosyayı kendi konumunuzda yayımlamayı seçerseniz, `fileUri` daha sonra öğreticide bulunan öğeyi güncelleştirin.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure hızlı başlangıç şablonları, ARM şablonları için bir depodur. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma).

1. Visual Studio Code **Dosya**  >  **Aç dosya**' yı seçin.
1. **Dosya adı** kutusuna aşağıdaki URL 'yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

1. Dosyayı açmak için **Aç**' ı seçin.
    Şablon beş kaynağı tanımlar:

   * [**Microsoft. Storage/storageAccounts**](/azure/templates/Microsoft.Storage/storageAccounts).
   * [**Microsoft. Network/Publicıpaddresses**](/azure/templates/microsoft.network/publicipaddresses).
   * [**Microsoft. Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups).
   * [**Microsoft. Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks).
   * [**Microsoft. Network/NetworkInterfaces**](/azure/templates/microsoft.network/networkinterfaces).
   * [**Microsoft. COMPUTE/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines).

     Özelleştirebilmeniz için önce şablon hakkında bazı temel bilgileri almanız yararlı olur.

1. Dosya farklı Kaydet ' **i seçerek dosyanın**bir kopyasını *azuredeploy.js* adı ile yerel bilgisayarınıza kaydedin  >  **Save As**.

## <a name="edit-the-template"></a>Şablonu düzenleme

Aşağıdaki içeriği kullanarak var olan şablona bir sanal makine uzantısı kaynağı ekleyin:

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "apiVersion": "2019-12-01",
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

Bu kaynak tanımı hakkında daha fazla bilgi için [uzantı başvurusuna](/azure/templates/microsoft.compute/virtualmachines/extensions)bakın. Önemli öğeler şunlardır:

* **name**: Uzantı kaynağı, sanal makine nesnesinin alt kaynağı olduğundan ad alanında sanal makine adı ön ek olarak kullanılmalıdır. Bkz. [alt kaynaklar için ad ve tür ayarlama](child-resource-name-type.md).
* **Bağımlıdson**: sanal makineyi oluşturduktan sonra uzantı kaynağını oluşturun.
* **Fileuris**: betik dosyalarının depolandığı konumlar. Belirtilen konumu kullanmayı tercih ederseniz, değerleri güncelleştirmeniz gerekir.
* **Commandtoexecute**: Bu komut betiği çağırır.

Satır içi betiği kullanmak için **Fileuris**'i kaldırın ve **commandtoexecute** öğesini şu şekilde güncelleştirin:

```powershell
powershell.exe Install-WindowsFeature -name Web-Server -IncludeManagementTools && powershell.exe remove-item 'C:\\inetpub\\wwwroot\\iisstart.htm' && powershell.exe Add-Content -Path 'C:\\inetpub\\wwwroot\\iisstart.htm' -Value $('Hello World from ' + $env:computername)
```

Bu satır içi betik Ayrıca iisstart.html içeriğini güncelleştirir.

Web sunucusuna erişebilmek için HTTP bağlantı noktasını da açmanız gerekir.

1. Şablonda **SecurityRules** bulun.
1. **Varsayılan-Allow-3389**' ın yanına aşağıdaki kuralı ekleyin.

    ```json
    {
      "name": "AllowHTTPInBound",
      "properties": {
        "priority": 1010,
        "access": "Allow",
        "direction": "Inbound",
        "destinationPortRange": "80",
        "protocol": "Tcp",
        "sourcePortRange": "*",
        "sourceAddressPrefix": "*",
        "destinationAddressPrefix": "*"
      }
    }
    ```

## <a name="deploy-the-template"></a>Şablonu dağıtma

Dağıtım yordamı için öğreticinin "şablonu dağıtma" bölümüne bakın [: bağımlı kaynaklarla ARM şablonları oluşturma](./template-tutorial-create-templates-with-dependent-resources.md#deploy-the-template). Sanal makine yöneticisi hesabı için oluşturulmuş bir parola kullanmanızı öneririz. Bu makalenin [Önkoşullar](#prerequisites) bölümüne bakın.

Cloud Shell, VM 'nin genel IP adresini almak için aşağıdaki komutu çalıştırın:

```azurepowershell
(Get-AzPublicIpAddress -ResourceGroupName $resourceGroupName).IpAddress
```

IP adresini bir Web tarayıcısına yapıştırın. Varsayılan Internet Information Services (IIS) karşılama sayfası açılır:

![Internet Information Services hoş geldiniz sayfası](./media/template-tutorial-deploy-vm-extensions/resource-manager-template-deploy-extensions-customer-script-web-server.png)

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
> [SQL uzantılarını dağıtma](./template-tutorial-deploy-sql-extensions-bacpac.md)

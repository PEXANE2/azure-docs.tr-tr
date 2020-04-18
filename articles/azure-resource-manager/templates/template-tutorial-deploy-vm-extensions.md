---
title: VM uzantılarını şablonla dağıtma
description: Azure Resource Manager şablonlarıyla sanal makine uzantılarını dağıtmayı öğrenin
author: mumian
ms.date: 04/16/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 280b4a9775346c719e82d1fef4162fa6ea666798
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81616884"
---
# <a name="tutorial-deploy-virtual-machine-extensions-with-arm-templates"></a>Öğretici: ARM şablonlarıyla sanal makine uzantılarını dağıtma

Azure VM'lerinde dağıtım sonrası yapılandırma ve otomasyon görevleri gerçekleştirme amacıyla [Azure sanal makine uzantılarını](../../virtual-machines/extensions/features-windows.md) kullanmayı öğrenin. Azure VM'leri ile kullanabileceğiniz birçok farklı VM uzantısı vardır. Bu eğitimde, Windows VM'de PowerShell komut dosyası çalıştırmak için Azure Kaynak Yöneticisi (ARM) şablonundan özel komut dosyası uzantısı dağıtılır.  Bu betik, VM'ye Web Sunucusu yükler.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * PowerShell betiğini hazırlama
> * Hızlı başlangıç şablonunu açma
> * Şablonu düzenleme
> * Şablonu dağıtma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code ve Resource Manager Araçları uzantısı. Bkz. [ARM şablonları oluşturmak için Görsel Stüdyo Kodunu Kullan.](use-vs-code-to-create-template.md)
* Güvenliği artırmak istiyorsanız sanal makine yönetici hesabı için oluşturulmuş bir parola kullanın. Parola oluşturma örneği aşağıda verilmiştir:

    ```console
    openssl rand -base64 32
    ```

    Azure Key Vault şifreleme anahtarları ve diğer gizli dizileri korumak üzere tasarlanmıştır. Daha fazla bilgi için [Bkz. Öğretici: Azure Anahtar Kasası'nı ARM şablon dağıtımına entegre edin.](./template-tutorial-use-key-vault.md) Ayrıca parolanızı her üç ayda bir güncellemenizi öneririz.

## <a name="prepare-a-powershell-script"></a>PowerShell betiğini hazırlama

Sıralı PowerShell komut dosyası veya komut dosyası dosyası kullanabilirsiniz.  Bu öğretici, komut dosyası dosyasının nasıl kullanılacağını gösterir. Aşağıdaki içeriğe sahip bir PowerShell komut dosyası [GitHub'dan](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorial-vm-extension/installWebServer.ps1)paylaşılır:

```azurepowershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Dosyayı kendi konumunuza yayımlamayı seçerseniz, şablondaki öğeyi `fileUri` daha sonra öğreticide güncelleştirin.

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

Azure Quickstart Şablonları, ARM şablonları için bir depodur. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu öğreticide kullanılan şablonun adı: [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Basit bir Windows sanal makinesi dağıtma).

1. Visual Studio Code'da **Dosya** > **Aç Dosya'yı**seçin.
1. Dosya **adı** kutusuna aşağıdaki URL'yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

1. Dosyayı açmak için **Aç'ı**seçin.
    Şablon beş kaynak tanımlar:

   * [**Microsoft.Storage/storageHesapları**](/azure/templates/Microsoft.Storage/storageAccounts).
   * [**Microsoft.Network/publicIPAddresses**](/azure/templates/microsoft.network/publicipaddresses).
   * [**Microsoft.Network/networkSecurityGroups**](/azure/templates/microsoft.network/networksecuritygroups).
   * [**Microsoft.Network/virtualNetworks**](/azure/templates/microsoft.network/virtualnetworks).
   * [**Microsoft.Network/networkInterfaces**](/azure/templates/microsoft.network/networkinterfaces).
   * [**Microsoft.Compute/virtualMachines**](/azure/templates/microsoft.compute/virtualmachines).

     Şablonu özelleştirmeden önce şablonhakkında temel bir anlayış elde etmek yararlı dır.

1. Dosyayı**Kaydet'i**seçerek *azuredeploy.json* adıyla yerel bilgisayarınıza **kaydedin.** > 

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

Bu kaynak tanımı hakkında daha fazla bilgi için [uzantı başvurusuna](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines/extensions)bakın. Önemli öğeler şunlardır:

* **name**: Uzantı kaynağı, sanal makine nesnesinin alt kaynağı olduğundan ad alanında sanal makine adı ön ek olarak kullanılmalıdır. Bkz. [Alt kaynaklar için ad ve yazı yı ayarla.](child-resource-name-type.md)
* **dependsOn**: Sanal makineyi oluşturduktan sonra uzantı kaynağını oluşturun.
* **fileUris**: Komut dosyası dosyalarının depolandığı konumlar. Sağlanan konumu kullanmamayı seçerseniz, değerleri güncelleştirmeniz gerekir.
* **commandToExecute**: Bu komut komut dosyasını çağırır.

Satır satırkomutunu kullanmak için **fileUris'i**kaldırın ve **ToExecute komutunu** şu şekilde güncelleştirin:

```powershell
powershell.exe Install-WindowsFeature -name Web-Server -IncludeManagementTools && powershell.exe remove-item 'C:\\inetpub\\wwwroot\\iisstart.htm' && powershell.exe Add-Content -Path 'C:\\inetpub\\wwwroot\\iisstart.htm' -Value $('Hello World from ' + $env:computername)
```

Bu satır satırlı komut dosyası da iisstart.html içeriğini güncelleştirin.

Web sunucusuna erişebilmeniz için HTTP bağlantı noktasını da açmanız gerekir.

1. Şablonda **güvenlik Kuralları'nı** bulun.
1. **Varsayılan izin-3389**yanına aşağıdaki kuralı ekleyin.

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

Dağıtım yordamı için, Öğretici'nin "Şablonu dağıt" bölümüne [bakın: Bağımlı kaynaklara sahip ARM şablonları oluşturun.](./template-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) Sanal makine yöneticisi hesabı için oluşturulan bir parola kullanmanızı tavsiye ettik. Bu makalenin [Önkoşullar](#prerequisites) bölümüne bakın.

Bulut Kabuğundan, VM'nin genel IP adresini almak için aşağıdaki komutu çalıştırın:

```azurepowershell
(Get-AzPublicIpAddress -ResourceGroupName $resourceGroupName).IpAddress
```

IP adresini bir Web tarayıcısına yapıştırın. Varsayılan Internet Bilgi Hizmetleri (IIS) karşılama sayfası açılır:

![İnternet Bilgi Hizmetleri karşılama sayfası](./media/template-tutorial-deploy-vm-extensions/resource-manager-template-deploy-extensions-customer-script-web-server.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dağıttığınız Azure kaynaklarına artık ihtiyacınız olmadığında, kaynak grubunu silerek bunları temizleyin.

1. Azure portalında, sol bölmede **Kaynak grubunu**seçin.
2. Ad kutusuna **göre Filtre'ye** kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
    Kaynak grubunda altı kaynak görüntülenir.
4. Üst menüde **kaynak grubunu sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir sanal makine ve bir sanal makine uzantısı dağıttınız. Uzantı, sanal makineye IIS web sunucusunu yükledi. BIR BACPAC dosyasını almak için Azure SQL Veritabanı uzantısını nasıl kullanacağınızı öğrenmek için bkz:

> [!div class="nextstepaction"]
> [SQL uzantılarını dağıtma](./template-tutorial-deploy-sql-extensions-bacpac.md)

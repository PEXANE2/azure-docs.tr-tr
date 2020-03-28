---
title: Birden çok kaynak örneği oluşturma
description: Birden çok Azure kaynağı örneği oluşturmak için bir Azure Resource Manager şablonu oluşturmayı öğrenin.
author: mumian
ms.date: 03/04/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 9ed14ce1af6421accccface1b66119057d1c5a30
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80239287"
---
# <a name="tutorial-create-multiple-resource-instances-with-arm-templates"></a>Öğretici: ARM şablonları ile birden çok kaynak örneği oluşturma

Azure kaynağının birden çok örneğini oluşturmak için Azure Kaynak Yöneticisi (ARM) şablonunuzda nasıl yinelenirken öğrenin. Bu öğreticide, bir şablonu değiştirerek üç depolama hesabı örneği oluşturacaksınız.

![Azure Kaynak Yöneticisi birden çok örnek diyagramı oluşturur](./media/template-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances-diagram.png)

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hızlı başlangıç şablonunu açma
> * Şablonu düzenleme
> * Şablonu dağıtma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Visual Studio Code ve Resource Manager Araçları uzantısı. Bkz. [ARM şablonları oluşturmak için Görsel Stüdyo Kodunu Kullan.](use-vs-code-to-create-template.md)

## <a name="open-a-quickstart-template"></a>Hızlı başlangıç şablonunu açma

[Azure QuickStart Şablonları,](https://azure.microsoft.com/resources/templates/) ARM şablonları için bir depodur. Sıfırdan bir şablon oluşturmak yerine örnek bir şablon bulabilir ve bunu özelleştirebilirsiniz. Bu hızlı başlangıçta kullanılan şablon [Standart depolama hesabı oluşturma](https://azure.microsoft.com/resources/templates/101-storage-account-create/) olarak adlandırılır. Şablon, Azure Depolama hesabı kaynağını tanımlar.

1. Visual Studio Code'dan **Dosya**>**Aç Dosya'yı**seçin.
2. **Dosya adı**’na şu URL’yi yapıştırın:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Dosyayı açmak için **Aç**’ı seçin.
4. Şablonda tanımlı bir 'Microsoft.Storage/storageAccounts' kaynağı bulunur. Şablonu, [şablon başvurusu](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts) ile karşılaştırın. Şablonu özelleştirmeden önce temel noktaları kavramak faydalı olacaktır.
5. Dosyayı **azuredeploy.json** olarak yerel bilgisayarınıza kaydetmek için**Dosya Yı Kaydet'i** **seçin.**>

## <a name="edit-the-template"></a>Şablonu düzenleme

Var olan şablon, temel şifrelenmemiş depolama hesabı oluşturur. Şablonu özelleştirerek üç depolama hesabı oluşturacaksınız.

Visual Studio Code’dan aşağıdaki dört değişikliği yapın:

![Azure Resource Manager birden çok örnek oluşturur](./media/template-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances.png)

1. Depolama hesabı kaynak tanımına bir `copy` öğesi ekleyin. Kopyalama öğesinde, bu döngü için yineleme sayısı ve bir değişken belirtin. Sayı değeri pozitif bir tamsayı olmalıdır ve 800’ü aşamaz.
2. `copyIndex()` İşlevi, döngüde geçerli yinelemeyi döndürür. Dizini ad ön eki olarak kullanırsınız. `copyIndex()` sıfır tabanlıdır. Dizin değerini kaydırmak için copyIndex () işlevine bir değer geçirebilirsiniz. Örneğin, *copyIndex(1)*.
3. Artık kullanılmadığı için **değişkenler** öğesini silin.
4. **Çıktılar** öğesini silin. Artık buna ihtiyacınız yoktur.

Tamamlanan şablon aşağıdaki gibi görünür:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "apiVersion": "2018-02-01",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage",
      "properties": {},
      "copy": {
        "name": "storagecopy",
        "count": 3
      }
    }
  ]
}
```

Birden çok örnek oluşturma hakkında daha fazla bilgi için [bkz.](./copy-resources.md)

## <a name="deploy-the-template"></a>Şablonu dağıtma

Dağıtım yordamı için Visual Studio Code hızlı başlangıçta [Şablonu dağıtma](quickstart-create-templates-use-visual-studio-code.md#deploy-the-template) bölümüne bakın.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Üç depolama hesabının tümünü listelemek için --ad parametresini atlayın:

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
```azurecli
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az storage account list --resource-group $resourceGroupName
```

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
Get-AzStorageAccount -ResourceGroupName $resourceGroupName
```

---

Depolama hesabı adlarını, şablondaki ad tanımıyla karşılaştırın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.  Kaynak grubundaki toplam altı kaynak görüyor olmalısınız.
4. Üst menüden **kaynak grubunu sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, birden çok depolama hesabı örneği oluşturmayı öğrendiniz.  Bir sonraki öğreticide, birden fazla kaynağa ve birden çok kaynak türüne sahip bir şablon geliştireceksiniz. Bazı kaynakların bağımlı kaynakları vardır.

> [!div class="nextstepaction"]
> [Bağımlı kaynaklar oluşturma](./template-tutorial-create-templates-with-dependent-resources.md)

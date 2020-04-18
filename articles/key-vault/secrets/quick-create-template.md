---
title: Azure Quickstart - Azure Kaynak Yöneticisi şablonu kullanarak bir Azure anahtar kasası ve bir sır oluşturun | Microsoft Dokümanlar
description: Azure anahtar kasalarının nasıl oluşturulup oluşturulacaklarını hızlı bir şekilde gösterin ve Azure Kaynak Yöneticisi şablonu kullanarak kasalara sır ekleyin.
services: key-vault
author: mumian
manager: dougeby
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: mvc,subject-armqs
ms.date: 02/27/2020
ms.author: jgao
ms.openlocfilehash: 273a467f5db2201015352aaf4a232f5a42e29673
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81618089"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-resource-manager-template"></a>Hızlı başlatma: Kaynak Yöneticisi şablonu kullanarak Azure Key Vault'tan bir sır ayarlayın ve alın

[Azure Key Vault,](../general/overview.md) anahtarlar, parolalar, sertifikalar ve diğer sırlar gibi sırlar için güvenli bir mağaza sağlayan bir bulut hizmetidir. Bu hızlı başlangıç, önemli bir kasa ve bir sır oluşturmak için bir Kaynak Yöneticisi şablonu dağıtma işlemine odaklanır.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Şablonda izinlerin yapılandırılması için Azure AD kullanıcı nesnesi kimliğiniz gerekir. Aşağıdaki yordam nesne kimliğini (GUID) alır.

    1. Aşağıdaki Azure PowerShell veya Azure CLI komutunu **Tseç'i**seçerek çalıştırın ve ardından komut dosyasını kabuk bölmesine yapıştırın. Komut dosyasını yapıştırmak için kabuğu sağ tıklatın ve sonra **Yapıştır'ı**seçin.

        # <a name="cli"></a>[CLI](#tab/CLI)
        ```azurecli-interactive
        echo "Enter your email address that is used to sign in to Azure:" &&
        read upn &&
        az ad user show --id $upn --query "objectId" &&
        echo "Press [ENTER] to continue ..."
        ```

        # <a name="powershell"></a>[PowerShell](#tab/PowerShell)
        ```azurepowershell-interactive
        $upn = Read-Host -Prompt "Enter your email address used to sign in to Azure"
        (Get-AzADUser -UserPrincipalName $upn).Id
        Write-Host "Press [ENTER] to continue..."
        ```

        ---

    2. Nesne kimliğini yazın. Bu hızlı başlangıç sonraki bölümde gerekir.

## <a name="create-a-vault-and-a-secret"></a>Bir kasa ve bir sır oluşturun

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://azure.microsoft.com/resources/templates/101-key-vault-create/)

:::code language="json" source="~/quickstart-templates/101-key-vault-create/azuredeploy.json" range="1-150" highlight="107-148":::

Şablonda iki Azure kaynağı tanımlanır:

* [**Microsoft.KeyVault/vaults**](/azure/templates/microsoft.keyvault/vaults): bir Azure anahtar kasası oluşturun.
* [**Microsoft.KeyVault/vaults/secrets**](/azure/templates/microsoft.keyvault/vaults/secrets): önemli bir kasa sırrı oluşturun.

Daha fazla Azure Key Vault şablon örnekleri [burada](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Keyvault)bulabilirsiniz.

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon bir anahtar kasa ve bir sır oluşturur.

    [![Azure’a dağıtma](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-key-vault-create%2Fazuredeploy.json)

2. Aşağıdaki değerleri seçin veya girin.

    ![Kaynak Yöneticisi şablonu, Key Vault entegrasyonu, dağıtım portalı](../media/quick-create-template/create-key-vault-using-template-portal.png)

    Belirtilmediği sürece, anahtar kasasını ve sırrı oluşturmak için varsayılan değeri kullanın.

    * **Abonelik**: Bir Azure aboneliği seçin.
    * **Kaynak grubu**: **Yeni Oluştur'u**seçin, kaynak grubu için benzersiz bir ad girin ve ardından **Tamam'ı**tıklatın.
    * **Konum**: Bir konum seçin.  Örneğin, **Orta ABD**.
    * **Anahtar Vault Adı**: .vault.azure.net ad alanı içinde küresel olarak benzersiz olması gereken anahtar kasası için bir ad girin. Dağıtımı doğrularken bir sonraki bölümde adın ada ihtiyacı vardır.
    * **Kiracı Kimliği**: şablon işlevi kiracı kimliğinizi otomatik olarak alır.  Varsayılan değeri değiştirmeyin.
    * **Reklam Kullanıcı Kimliği**: [Önkoşullar'dan](#prerequisites)aldığınız Azure AD kullanıcı nesnekimliğinizi girin.
    * **Gizli Ad**: anahtar kasasında sakladığınız gizli nin adını girin.  Örneğin, **adminpassword**.
    * **Gizli Değer**: gizli değeri girin.  Bir parola saklarsanız, oluşturduğunuz parolayı Önkoşullar'da kullanmanız önerilir.
    * **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**: Seçin.
3. **Satın al**'ı seçin. Anahtar kasası başarıyla dağıtıldıktan sonra bir bildirim alırsınız:

    ![Kaynak Yöneticisi şablonu, Key Vault entegrasyonu, portal bildirimi dağıtma](../media/quick-create-template/resource-manager-template-portal-deployment-notification.png)

Azure portalı şablonu dağıtmak için kullanılır. Azure portalına ek olarak, Azure PowerShell, Azure CLI ve REST API'yi de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için [şablonları dağıt'a](../../azure-resource-manager/templates/deploy-powershell.md)bakın.

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Anahtar kasasını ve sırrı denetlemek için Azure portalını kullanabilir veya oluşturulan sırrı listelemek için aşağıdaki Azure CLI veya Azure PowerShell komut dosyasını kullanabilirsiniz.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter your key vault name:" &&
read keyVaultName &&
az keyvault secret list --vault-name $keyVaultName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$keyVaultName = Read-Host -Prompt "Enter your key vault name"
Get-AzKeyVaultSecret -vaultName $keyVaultName
Write-Host "Press [ENTER] to continue..."
```

---

Çıktı aşağıdakilere benzer:

# <a name="cli"></a>[CLI](#tab/CLI)

![Kaynak Yöneticisi şablonu, Key Vault entegrasyonu, dağıtım portalı doğrulama çıktısı](../media/quick-create-template/resource-manager-template-portal-deployment-cli-output.png)

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

![Kaynak Yöneticisi şablonu, Key Vault entegrasyonu, dağıtım portalı doğrulama çıktısı](../media/quick-create-template/resource-manager-template-portal-deployment-powershell-output.png)

---
## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Key Vault hızlı başlangıçları ve öğreticileri bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmanız yararlı olabilir.
Artık gerek kalmadığında kaynak grubunu silin; bunu yaptığınızda Key Vault ve ilgili kaynaklar silinir. Azure CLI veya Azure PowerShell kullanarak kaynak grubunu silmek için:

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure Kaynak Yöneticisi şablonu kullanarak bir anahtar kasa sı ve bir sır oluşturdunuz ve dağıtımı doğruladınız. Key Vault ve Azure Kaynak Yöneticisi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere devam edin.

- Azure [Anahtar Kasasına Genel Bakış](../general/overview.md)
- [Azure Kaynak Yöneticisi](../../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](../general/best-practices.md)

---
title: Azure hızlı başlangıç-Azure Resource Manager şablonu kullanarak bir Azure Anahtar Kasası ve gizli dizi oluşturma | Microsoft Docs
description: Azure Anahtar kasaları oluşturmayı ve Azure Resource Manager şablonu kullanarak kasalarına gizli dizileri eklemeyi gösteren hızlı başlangıç.
services: key-vault
author: mumian
manager: dougeby
tags: azure-resource-manager
ms.service: key-vault
ms.topic: quickstart
ms.custom: mvc
ms.date: 09/03/2019
ms.author: jgao
ms.openlocfilehash: 13ac8b9f6d82cc8b0060f87237ef3140cb245379
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70259200"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-resource-manager-template"></a>Hızlı Başlangıç: Kaynak Yöneticisi şablonu kullanarak Azure Key Vault bir gizli dizi ayarlama ve alma

[Azure Key Vault](./key-vault-overview.md) , anahtarlar, parolalar, sertifikalar ve diğer gizli diziler gibi gizli dizileri için güvenli bir depo sağlayan bir bulut hizmetidir. Bu hızlı başlangıç, bir anahtar kasası ve gizli dizi oluşturmak için bir Kaynak Yöneticisi şablonu dağıtma işlemine odaklanmaktadır. Kaynak Yöneticisi şablonları geliştirme hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi belgeleri](/azure/azure-resource-manager/) ve [şablon başvurusu](/azure/templates/microsoft.keyvault/allversions).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* Şablonda izinlerin yapılandırılması için Azure AD kullanıcı nesnesi kimliğiniz gerekir. Aşağıdaki yordam nesne KIMLIĞINI (GUID) alır.

    1. Aşağıdaki Azure PowerShell veya Azure CLı komutunu çalıştırarak **deneyin**' i seçin ve ardından betiği kabuk bölmesine yapıştırın. Betiği yapıştırmak için, kabuğa sağ tıklayın ve ardından **Yapıştır**' ı seçin. 

        ```azurecli-interactive
        echo "Enter your email address that is used to sign in to Azure:" &&
        read upn &&
        az ad user show --upn-or-object-id $upn --query "objectId" 
        ```

        ```azurepowershell-interactive
        $upn = Read-Host -Prompt "Enter your email address used to sign in to Azure"
        (Get-AzADUser -UserPrincipalName $upn).Id
        ```

    2. Nesne KIMLIĞINI yazın. Bu hızlı başlangıç bölümünün sonraki bölümünde olması gerekir.

## <a name="create-a-vault-and-a-secret"></a>Kasa ve gizli dizi oluşturma

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-key-vault-create/). Daha fazla Azure Key Vault şablon örneği [burada](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Keyvault)bulunabilir.

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon bir anahtar kasası ve gizli dizi oluşturur.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-key-vault-create%2Fazuredeploy.json"><img src="./media/quick-create-template/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Aşağıdaki değerleri seçin veya girin.  

    ![Resource Manager şablonu Key Vault tümleştirmesi portal dağıtımı](./media/quick-create-template/create-key-vault-using-template-portal.png)

    Belirtilmediği takdirde, anahtar kasasını ve bir parolayı oluşturmak için varsayılan değeri kullanın.

    * **Abonelik**: Bir Azure aboneliği seçin.
    * **Kaynak grubu**: **Yeni oluştur**' u seçin, kaynak grubu için benzersiz bir ad girin ve ardından **Tamam**' a tıklayın. 
    * **Konum**: Bir konum seçin.  Örneğin, **Orta ABD**.
    * **Key Vault adı**:. Vault.Azure.net ad alanı içinde genel olarak benzersiz olması gereken Anahtar Kasası için bir ad girin.  
    * **Kiracı Kimliği**: Şablon işlevi kiracı kimliğinizi otomatik olarak alır.  Varsayılan değeri değiştirmeyin.
    * **Ad Kullanıcı kimliği**: [ön koşullardan](#prerequisites)aldığınız Azure AD Kullanıcı nesne kimliğinizi girin.
    * **Gizli dizi adı**: anahtar kasasında depoladığınız gizli dizi için bir ad girin.  Örneğin, **AdminPassword**.
    * **Gizli dizi değeri**: gizli değeri girin.  Bir parola depolukarşılaşırsanız, Önkoşullarda oluşturduğunuz oluşturulan parolanın kullanılması önerilir.
    * **Yukarıdaki hüküm ve koşullar durumunu kabul ediyorum**: Seçin.
3. **Satın al**'ı seçin.

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Anahtar kasasını ve parolayı denetlemek için Azure portal kullanabilir ya da oluşturulan gizli anahtarı listelemek için aşağıdaki Azure CLı veya Azure PowerShell betiğini kullanabilirsiniz.

```azurecli-interactive
echo "Enter your key vault name:" &&
read keyVaultName &&
az keyvault secret list --vault-name $keyVaultName
```

```azurepowershell-interactive
$keyVaultName = Read-Host -Prompt "Enter your key vault name"
Get-AzKeyVaultSecret -vaultName $keyVaultName
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Key Vault hızlı başlangıçları ve öğreticileri bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmanız yararlı olabilir.
Artık gerek kalmadığında kaynak grubunu silin; bunu yaptığınızda Key Vault ve ilgili kaynaklar silinir. Azure CLı veya Azure PowerShell kullanarak kaynak grubunu silmek için:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName 
```
```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName 
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Azure Resource Manager şablonu kullanarak bir anahtar kasası ve gizli dizi oluşturdunuz ve dağıtımı doğruladı. Key Vault ve Azure Resource Manager hakkında daha fazla bilgi edinmek için aşağıdaki makalelere devam edin.

- [Azure Key Vault genel bakışını](key-vault-overview.md) okuyun
- [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) hakkında daha fazla bilgi edinin
- [Anahtarlar, gizlilikler ve sertifikalar](about-keys-secrets-and-certificates.md) hakkında daha fazla bilgi alın
- [En iyi uygulamaları](key-vault-best-practices.md) gözden geçirin Azure Key Vault
---
title: Öğretici-.NET 'teki bir sanal makine ile Azure Key Vault kullanma | Microsoft Docs
description: Bu öğreticide, anahtar kasaınızdan gizli dizi okumak için bir ASP.NET Core uygulaması yapılandırırsınız.
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 07/20/2020
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: e77701e17ef1b47aa6b8e3b8f2d10e93bf5e054e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87101706"
---
# <a name="tutorial-use-azure-key-vault-with-a-virtual-machine-in-net"></a>Öğretici: .NET 'teki bir sanal makine ile Azure Key Vault kullanma

Azure Key Vault, API anahtarları, uygulamalarınıza, hizmetlerinize ve BT kaynaklarınıza erişmeniz gereken veritabanı bağlantı dizeleri gibi gizli dizileri korumanıza yardımcı olur.

Bu öğreticide, Azure Key Vault bilgileri okumak için bir konsol uygulamasının nasıl alınacağını öğreneceksiniz. Uygulama, Key Vault kimlik doğrulaması yapmak için sanal makine tarafından yönetilen kimliği kullanır. 

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * Bir kaynak grubu oluşturun.
> * Anahtar kasası oluşturma.
> * Anahtar kasasına gizli dizi ekleyin.
> * Anahtar kasasından bir gizli dizi alma.
> * Bir Azure sanal makinesi oluşturun.
> * Sanal makine için [yönetilen kimliği](../../active-directory/managed-identities-azure-resources/overview.md) etkinleştirin.
> * VM kimliğine izin atayın.

Başlamadan önce [temel kavramları Key Vault](basic-concepts.md)okuyun. 

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Windows, Mac ve Linux için:
  * [Git](https://git-scm.com/downloads)
  * [.NET Core 3,1 SDK veya üzeri](https://dotnet.microsoft.com/download/dotnet-core/3.1).
  * [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-resources-and-assign-permissions"></a>Kaynak oluşturma ve izin atama

Kodlamaya başlamadan önce bazı kaynaklar oluşturmanız, anahtar kasanıza bir gizli anahtar koymanız ve izinler atamanız gerekir.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure CLı kullanarak Azure 'da oturum açmak için şunu girin:

```azurecli
az login
```

### <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve Anahtar Kasası oluşturma

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-rg-kv-creation.md)]

### <a name="populate-your-key-vault-with-a-secret"></a>Anahtar kasanızı gizli dizi ile doldurma

Şimdi [az keykasasecret set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) komutunu kullanarak anahtar kasanıza gizli dizi ekleyin. **MySecret**adlı anahtar kasasında bir gizli dizi oluşturmak için aşağıdaki komutu girin:

```azurecli
az keyvault secret set --vault-name "<your-unique-key-vault-name>" --name "mySecret" --value "MySecret"
```

Bu gizli anahtar, **MySecret**değerini depolar.

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Aşağıdaki yöntemlerden birini kullanarak bir Windows veya Linux sanal makinesi oluşturun:

| Windows | Linux |
|--|--|
| [Azure CLI](../../virtual-machines/windows/quick-create-cli.md) | [Azure CLI](../../virtual-machines/linux/quick-create-cli.md) |  
| [PowerShell](../../virtual-machines/windows/quick-create-powershell.md) | [PowerShell](../../virtual-machines/linux/quick-create-powershell.md) |
| [Azure Portal](../../virtual-machines/windows/quick-create-portal.md) | [Azure Portal](../../virtual-machines/linux/quick-create-portal.md) |

### <a name="assign-an-identity-to-the-vm"></a>VM 'ye bir kimlik atama
[Az VM Identity Assign](/cli/azure/vm/identity?view=azure-cli-latest#az-vm-identity-assign) komutuyla sanal makine için sistem tarafından atanan bir kimlik oluşturun:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Aşağıdaki kodda görüntülenen sistem tarafından atanan kimliği unutmayın. Yukarıdaki komutun çıkışı şöyle olacaktır: 

```output
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

### <a name="assign-permissions-to-the-vm-identity"></a>VM kimliğine izin atama
[Az keykasası Set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuyla anahtar kasanıza önceden oluşturulmuş kimlik izinlerini atayın:

```azurecli
az keyvault set-policy --name '<your-unique-key-vault-name>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

### <a name="sign-in-to-the-virtual-machine"></a>Sanal makinede oturum açın

Sanal makinede oturum açmak için Bağlan bölümündeki yönergeleri izleyin [ve Windows çalıştıran bir Azure sanal makinesinde oturum açın](../../virtual-machines/windows/connect-logon.md) veya [Linux çalıştıran bir Azure sanal makinesine bağlanın ve oturum açın](../../virtual-machines/linux/login-using-aad.md).

## <a name="set-up-the-console-app"></a>Konsol uygulamasını ayarlama

Bir konsol uygulaması oluşturun ve komutunu kullanarak gerekli paketleri yükler `dotnet` .

### <a name="install-net-core"></a>.NET Core'u yükleyin

.NET Core 'u yüklemek için [.net İndirmeleri](https://www.microsoft.com/net/download) sayfasına gidin.

### <a name="create-and-run-a-sample-net-app"></a>Örnek bir .NET uygulaması oluşturma ve çalıştırma

Bir komut istemi açın.

Aşağıdaki komutları çalıştırarak konsola "Merhaba Dünya" yazdırabilirsiniz:

```console
dotnet new console -n keyvault-console-app
cd keyvault-console-app
dotnet run
```

### <a name="install-the-package"></a>Paketi yükler

Konsol penceresinde, .NET için Azure Key Vault gizli dizi istemci Kitaplığı ' nı yükledikten sonra:

```console
dotnet add package Azure.Security.KeyVault.Secrets
```

Bu hızlı başlangıçta, Azure Key Vault kimlik doğrulaması yapmak için aşağıdaki kimlik paketini yüklemeniz gerekir:

```console
dotnet add package Azure.Identity
```

## <a name="edit-the-console-app"></a>Konsol uygulamasını düzenleme

*Program.cs* dosyasını açın ve şu paketleri ekleyin:

```csharp
using System;
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;
```

Bu satırları ekleyin ve URI 'yi `vaultUri` anahtar kasanızın sayısını yansıtacak şekilde güncelleyerek. Aşağıdaki kod, kimlik doğrulaması için uygulama tarafından yönetilen kimliğin belirtecini kullanan Anahtar Kasası kimlik doğrulaması için [' DefaultAzureCredential () '](/dotnet/api/azure.identity.defaultazurecredential?view=azure-dotnet) kullanıyor. Ayrıca, Anahtar Kasası kısıtlanmadan yeniden denemeler için üstel geri alma özelliği de kullanılıyor.

```csharp
  class Program
    {
        static void Main(string[] args)
        {
            string secretName = "mySecret";

            var kvUri = "https://<your-key-vault-name>.vault.azure.net";
            SecretClientOptions options = new SecretClientOptions()
            {
                Retry =
                {
                    Delay= TimeSpan.FromSeconds(2),
                    MaxDelay = TimeSpan.FromSeconds(16),
                    MaxRetries = 5,
                    Mode = RetryMode.Exponential
                 }
            };

            var client = new SecretClient(new Uri(kvUri), new DefaultAzureCredential(),options);

            Console.Write("Input the value of your secret > ");
            string secretValue = Console.ReadLine();

            Console.Write("Creating a secret in " + keyVaultName + " called '" + secretName + "' with the value '" + secretValue + "` ...");

            client.SetSecret(secretName, secretValue);

            Console.WriteLine(" done.");

            Console.WriteLine("Forgetting your secret.");
            secretValue = "";
            Console.WriteLine("Your secret is '" + secretValue + "'.");

            Console.WriteLine("Retrieving your secret from " + keyVaultName + ".");

            KeyVaultSecret secret = client.GetSecret(secretName);

            Console.WriteLine("Your secret is '" + secret.Value + "'.");

            Console.Write("Deleting your secret from " + keyVaultName + " ...");

            client.StartDeleteSecret(secretName);

            System.Threading.Thread.Sleep(5000);
            Console.WriteLine(" done.");

        }
    }
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında, sanal makineyi ve anahtar kasanızı silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Key Vault REST API](https://docs.microsoft.com/rest/api/keyvault/)

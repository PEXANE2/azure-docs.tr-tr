---
title: Hızlı başlangıç-.NET için Azure Key Vault istemci kitaplığı (v4)
description: .NET istemci kitaplığı 'nı (v4) kullanarak bir Azure Anahtar Kasası 'ndan gizli dizileri oluşturmayı, almayı ve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 09/30/2020
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: devx-track-csharp
ms.openlocfilehash: cc5aa7f10d83edc757e99820b9591a953df72062
ms.sourcegitcommit: 06ba80dae4f4be9fdf86eb02b7bc71927d5671d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2020
ms.locfileid: "91612260"
---
# <a name="quickstart-azure-key-vault-client-library-for-net-sdk-v4"></a>Hızlı başlangıç: .NET için Azure Key Vault istemci kitaplığı (SDK v4)

.NET için Azure Key Vault istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek üzere aşağıdaki adımları izleyin.

Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. .NET için Key Vault istemci kitaplığı 'nı kullanın:

- Anahtarlar ve parolalar üzerinde güvenlik ve denetim düzeyini artırın.
- Şifreleme anahtarlarını dakikalar içinde oluşturun ve içeri aktarın.
- Bulut ölçeği ve küresel yedeklilik ile gecikme süresini azaltın.
- TLS/SSL sertifikaları için görevleri basitleştirme ve otomatikleştirme.
- FIPS 140-2 düzey 2 doğrulanan HSM 'leri kullanın.

[API başvuru belgeleri](/dotnet/api/azure.security.keyvault.secrets?view=azure-dotnet)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault)  |  [Paket (NuGet)](https://www.nuget.org/packages/Azure.Security.KeyVault.Secrets/)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/dotnet)
* [.NET Core 3,1 SDK veya üzeri](https://dotnet.microsoft.com/download/dotnet-core)
* [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell](/powershell/azure/)

## <a name="setup"></a>Kurulum

### <a name="create-a-new-console-app"></a>Yeni bir konsol uygulaması oluşturma

Bir .NET Core konsol uygulaması oluşturmak ve derlemek için aşağıdaki adımları izleyin.

1. Bir komut kabuğunda, aşağıdaki komutu çalıştırarak adında bir proje oluşturun `key-vault-console-app` :

    ```dotnetcli
    dotnet new console --name key-vault-console-app
    ```

1. Yeni oluşturulan *Anahtar Kasası-konsol-uygulama* dizinine geçin ve projeyi derlemek için aşağıdaki komutu çalıştırın:

    ```dotnetcli
    dotnet build
    ```

    Derleme çıktısı hiçbir uyarı veya hata içermemelidir.
    
    ```console
    Build succeeded.
     0 Warning(s)
     0 Error(s)
    ```

### <a name="install-the-packages"></a>Paketleri yükler

Komut kabuğundan .NET için Azure Key Vault istemci Kitaplığı ' nı yükledikten sonra:

```dotnetcli
dotnet add package Azure.Security.KeyVault.Secrets
```

Bu hızlı başlangıçta Azure Identity için Azure SDK istemci kitaplığı 'nı yüklemeniz gerekir:

```dotnetcli
dotnet add package Azure.Identity
```

### <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve Anahtar Kasası oluşturma

[!INCLUDE[Create a resource group and key vault](../../../includes/key-vault-rg-kv-creation.md)]

### <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[!INCLUDE[Create a service principal](../../../includes/key-vault-sp-creation.md)]

#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Anahtar kasanıza hizmet sorumlusu erişimi verin

<!-- TODO: need to pass -g myResourceGroup to this command too -->
[!INCLUDE[Give the service principal access to your key vault](../../../includes/key-vault-sp-kv-access.md)]

#### <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

[!INCLUDE[Set environment variables](../../../includes/key-vault-set-environmental-variables.md)]

## <a name="object-model"></a>Nesne modeli

.NET için Azure Key Vault istemci kitaplığı, anahtarları ve ilgili varlıkları yönetmenizi sağlar. Sertifika ve gizli dizileri içeren ilgili varlık örnekleri. [Kod örnekleri](#code-examples) bölümünde, bir istemci oluşturma, gizli anahtar ayarlama, gizli anahtar alma ve gizli dizi silme işlemlerinin nasıl yapılacağı gösterilir.

Konsol uygulamasının tamamına adresinden ulaşılabilir https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/key-vault-console-app .

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeler ekleme

Aşağıdaki yönergeleri *program.cs*' nin en üstüne ekleyin:

[!code-csharp[](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=directives)]

### <a name="authenticate-and-create-a-client"></a>İstemci kimliğini doğrulama ve oluşturma

Anahtar Kasası kimlik doğrulaması ve istemci oluşturma, [ortam değişkenlerini ayarlama](#set-environment-variables) adımında ortam değişkenlerine bağımlıdır. Anahtar kasanızın adı, Anahtar Kasası URI 'sine, biçiminde genişletilir `https://<your-key-vault-name>.vault.azure.net` . Aşağıdaki kod, erişim belirtecini almak için ortam değişkenlerini okuyan anahtar kasasında kimlik doğrulaması için [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?view=azure-dotnet) kullanır.

[!code-csharp[](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=authenticate)]

### <a name="save-a-secret"></a>Gizli dizi Kaydet

Konsol uygulamasının kimliğinin doğrulandığına göre, anahtar kasasına bir gizli dizi ekleyin. Bu görev için [istemcisini kullanın. SetSecret](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.setsecretasync) yöntemi. Yöntemin ilk parametresi, &mdash; Bu örnekteki "MySecret" gizli anahtarı için bir ad kabul eder.

[!code-csharp[](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=setsecret)]

Parolanın [az keykasası Secret Show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutuyla ayarlandığını doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

```azurepowershell
(Get-AzKeyVaultSecret -VaultName <your-unique-keyvault-name> -Name mySecret).SecretValueText
```

### <a name="retrieve-a-secret"></a>Gizli dizi alma

Artık önceden ayarlanan değeri istemcisiyle elde edebilirsiniz [. GetSecret](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.getsecretasync) yöntemi.

[!code-csharp[](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=getsecret)]

Gizli anahtar artık olarak kaydedilir `secret.Value` .

### <a name="delete-a-secret"></a>Gizli anahtarı silme

Son olarak, anahtar kasanızdan parolayı istemciyle silelim [. DeleteSecret](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.getsecretasync) yöntemi.

[!code-csharp[](~/samples-key-vault-dotnet-quickstart/key-vault-console-app/Program.cs?name=deletesecret)]

Parolanın [az keykasasecret Show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutuyla yapıldığını doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

```azurepowershell
(Get-AzKeyVaultSecret -VaultName <your-unique-keyvault-name> -Name mySecret).SecretValueText
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, anahtar Kasanızı ve ilgili kaynak grubunu kaldırmak için Azure CLı veya Azure PowerShell kullanabilirsiniz.

### <a name="delete-a-key-vault"></a>Key Vault silme

```azurecli
az keyvault delete --name <your-unique-keyvault-name>
```

```azurepowershell
Remove-AzKeyVault -VaultName <your-unique-keyvault-name>
```

### <a name="purge-a-key-vault"></a>Key Vault Temizleme

```azurecli
az keyvault purge --location eastus --name <your-unique-keyvault-name>
```

```azurepowershell
Remove-AzKeyVault -VaultName <your-unique-keyvault-name> -InRemovedState -Location eastus
```

### <a name="delete-a-resource-group"></a>Kaynak grubunu silme

```azurecli
az group delete -g "myResourceGroup"
```

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="sample-code"></a>Örnek kod

Aşağıdaki adımları tamamlayarak, .NET Core konsol uygulamasını Key Vault etkileşimde bulunmak üzere değiştirin:

1. *Program.cs* içindeki kodu aşağıdaki kodla değiştirin:

    ```csharp
    using System;
    using Azure.Identity;
    using Azure.Security.KeyVault.Secrets;
    
    namespace key_vault_console_app
    {
        class Program
        {
            static void Main(string[] args)
            {
                const string secretName = "mySecret";
                var keyVaultName = Environment.GetEnvironmentVariable("KEY_VAULT_NAME");
                var kvUri = $"https://{keyVaultName}.vault.azure.net";
    
                var client = new SecretClient(new Uri(kvUri), new DefaultAzureCredential());
    
                Console.Write("Input the value of your secret > ");
                var secretValue = Console.ReadLine();
    
                Console.Write($"Creating a secret in {keyVaultName} called '{secretName}' with the value '{secretValue}' ...");
                client.SetSecret(secretName, secretValue);
                Console.WriteLine(" done.");
    
                Console.WriteLine("Forgetting your secret.");
                secretValue = string.Empty;
                Console.WriteLine($"Your secret is '{secretValue}'.");
    
                Console.WriteLine($"Retrieving your secret from {keyVaultName}.");
                KeyVaultSecret secret = client.GetSecret(secretName);
                Console.WriteLine($"Your secret is '{secret.Value}'.");
    
                Console.Write($"Deleting your secret from {keyVaultName} ...");
                client.StartDeleteSecret(secretName);
                System.Threading.Thread.Sleep(5000);
                Console.WriteLine(" done.");
            }
        }
    }
    ```

1. Uygulamayı çalıştırmak için aşağıdaki komutu yürütün.

    ```dotnetcli
    dotnet run
    ```

1. İstendiğinde, gizli bir değer girin. Örneğin, mySecretPassword.

    Aşağıdaki çıkışın bir varyasyonu görüntülenir:

    ```console
    Input the value of your secret > mySecretPassword
    Creating a secret in <your-unique-keyvault-name> called 'mySecret' with the value 'mySecretPassword' ... done.
    Forgetting your secret.
    Your secret is ''.
    Retrieving your secret from <your-unique-keyvault-name>.
    Your secret is 'mySecretPassword'.
    Deleting your secret from <your-unique-keyvault-name> ... done.    
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Anahtar Kasası oluşturdunuz, gizli dizi depolandı ve bu gizli dizi alındı. [GitHub 'da konsol uygulamasının tamamına](https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/key-vault-console-app)bakın.

Key Vault ve bunu uygulamalarınızla tümleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Key Vault genel bakışını](../general/overview.md) okuyun
- [Azure Key Vault geliştirici kılavuzuna](../general/developers-guide.md) bakın
- [En iyi uygulamaları](../general/best-practices.md) gözden geçirin Azure Key Vault

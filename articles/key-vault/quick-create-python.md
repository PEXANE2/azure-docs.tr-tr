---
title: Quickstart - Python için Azure Key Vault istemci kitaplığı
description: Python istemci kitaplığını kullanarak Azure anahtar kasasından nasıl sır oluşturup, nasıl silen ve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 10/20/2019
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.openlocfilehash: fccb999be82978073b3db13eba224b08adba2538
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811611"
---
# <a name="quickstart-azure-key-vault-client-library-for-python"></a>Quickstart: Python için Azure Key Vault istemci kitaplığı

Python için Azure Key Vault istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Python için Key Vault istemci kitaplığını kullanın:

- Anahtarlar ve parolalar üzerinde güvenliği ve denetimi artırın.
- Şifreleme anahtarlarını dakikalar içinde oluşturun ve aktarın.
- Bulut ölçeği ve genel artıklık la gecikme süresini azaltın.
- TLS/SSL sertifikaları için görevleri basitleştirin ve otomatikleştirin.
- FIPS 140-2 Düzey 2 onaylı HSM'leri kullanın.

[API başvuru belgeleri](/python/api/overview/azure/key-vault?view=azure-python) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault) | [Paketi (Python Paket Dizini)](https://pypi.org/project/azure-keyvault/)

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Python 2.7, 3.5.3 veya daha sonra
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell](/powershell/azure/overview)

Bu hızlı başlangıç, Bir Linux terminal penceresinde [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) çalıştırdığınızı varsayar.

## <a name="setting-up"></a>Ayarlama

### <a name="install-the-package"></a>Paketi yükleyin

Konsol penceresinden Python için Azure Key Vault sırları kitaplığını yükleyin.

```console
pip install azure-keyvault-secrets
```

Bu hızlı başlangıç için azure.identity paketini de yüklemeniz gerekir:

```console
pip install azure.identity
```

### <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve anahtar kasası oluşturma

Bu hızlı başlatma, önceden oluşturulmuş bir Azure anahtar kasası kullanır. [Azure CLI quickstart,](quick-create-cli.md) [Azure PowerShell quickstart](quick-create-powershell.md)veya Azure portalı [quickstart'taki](quick-create-portal.md)adımları izleyerek önemli bir kasa oluşturabilirsiniz. Alternatif olarak, aşağıdaki Azure CLI komutlarını çalıştırabilirsiniz.

> [!Important]
> Her anahtar kasanın benzersiz bir adı olmalıdır. Aşağıdaki örneklerde benzersiz-keyvault-adınızı> anahtar kasanızın adı ile <değiştirin.

```azurecli
az group create --name "myResourceGroup" -l "EastUS"

az keyvault create --name <your-unique-keyvault-name> -g "myResourceGroup"
```

### <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bulut tabanlı Python uygulamasının kimliğini doğrulamanın en basit yolu yönetilen bir kimliktir; bkz. Ayrıntılar [için Azure Key Vault'a erişmek için Uygulama Hizmeti yönetilen kimliği kullanın.](managed-identity.md) Ancak basitlik adına, bu hızlı başlatma bir Python konsol uygulaması oluşturur. Azure ile bir masaüstü uygulamasının kimliğini doğrulamak için bir hizmet sorumlusu nun ve erişim denetim ilkesinin kullanılması nı gerektirir.

Azure CLI az reklam [sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir hizmet ilkesi oluşturun:

```azurecli
az ad sp create-for-rbac -n "http://mySP" --sdk-auth
```

Bu işlem bir dizi anahtar / değer çifti döndürecektir. 

```console
{
  "clientId": "7da18cae-779c-41fc-992e-0527854c6583",
  "clientSecret": "b421b443-1669-4cd7-b5b1-394d5c945002",
  "subscriptionId": "443e30da-feca-47c4-b68f-1636b75e16b3",
  "tenantId": "35ad10f1-7799-4766-9acf-f2d946161b77",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```

Aşağıdaki [Çevre değişkenini ayarla](#set-environmental-variables) adımadımda kullanacağımız için clientId ve clientSecret'a dikkat edin.

#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Servis müdürüne anahtar kasanıza erişim hakkı verin

MüşteriYi [az keyvault set-ilke](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuna geçirerek servis müdürünüze izin veren anahtar kasanız için bir erişim ilkesi oluşturun. Servis sorumlusuna hem anahtarlar hem de sırlar için izinleri al, listele ve ayarla.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
```

#### <a name="set-environmental-variables"></a>Çevresel değişkenleri ayarlama

Uygulamamızdaki Varsayılan AzureCredential yöntemi üç çevresel değişkene dayanır: `AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET`ve `AZURE_TENANT_ID`. Bu `export VARNAME=VALUE` değişkenleri, biçimi kullanarak [bir hizmet ana adımı oluştur'da](#create-a-service-principal) belirttiğiniz clientId, clientSecret ve tenantId değerlerine ayarlayın. (Bu yöntem yalnızca geçerli kabuğunuz için değişkenleri ve kabuktan oluşturulan işlemleri ayarlar; bu değişkenleri `/etc/environment ` ortamınıza kalıcı olarak eklemek, dosyanızı yeniden düzenlendi.) 

Ayrıca anahtar kasa adınızı bir ortam değişkeni `KEY_VAULT_NAME`olarak kaydetmeniz gerekir.

```console
export AZURE_CLIENT_ID=<your-clientID>

export AZURE_CLIENT_SECRET=<your-clientSecret>

export AZURE_TENANT_ID=<your-tenantId>

export KEY_VAULT_NAME=<your-key-vault-name>
````

## <a name="object-model"></a>Nesne modeli

Python için Azure Key Vault istemci kitaplığı, anahtarları ve sertifikalar ve sırlar gibi ilgili varlıkları yönetmenize olanak tanır. Aşağıdaki kod örnekleri, bir istemci oluşturmak, bir sır ayarlamak, bir sır almak ve bir sırrı silmek nasıl gösterecektir.

Tüm konsol uygulaması . https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/key-vault-console-app

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeekleme

Kodunuzun üst bölümüne aşağıdaki yönergeleri ekleyin:

```python
import os
from azure.keyvault.secrets import SecretClient
from azure.identity import DefaultAzureCredential
```

### <a name="authenticate-and-create-a-client"></a>Kimlik doğrulaması ve istemci oluşturma

Anahtar kasanıza doğrulanması ve anahtar kasa istemcisi oluşturma, yukarıdaki [Set çevresel değişkenler](#set-environmental-variables) adımındaki çevresel değişkenlere bağlıdır. Anahtar kasanızın adı URI anahtar kasasına genişletilir ve "https://<your-key-vault-name>.vault.azure.net" biçiminde genişletilir.

```python
credential = DefaultAzureCredential()

client = SecretClient(vault_url=KVUri, credential=credential)
```

### <a name="save-a-secret"></a>Bir sırrı kaydet

Başvurunuzun kimliği doğrulanabildiği için, istemciyi kullanarak keyvault'unuza bir sır koyabilirsiniz. SetSecret method](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.setsecretasync) Bu gizli için bir ad gerektirir -- bu örnekte "mySecret" kullanıyoruz.  

```python
client.set_secret(secretName, secretValue)
```

Gizlinin [az keyvault gizli gösteri](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutuyla ayarlandığını doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

### <a name="retrieve-a-secret"></a>Bir sır alma

Şimdi istemci ile daha önce ayarlanmış değeri [alabilirsiniz. GetSecret yöntemi](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.getsecretasync).

```python
retrieved_secret = client.get_secret(secretName)
 ```

Sırrın artık `retrieved_secret.value`.

### <a name="delete-a-secret"></a>Gizli anahtarı silme

Son olarak, istemci ile anahtar kasasından sırrı [silelim. DeleteSecret yöntemi](/dotnet/api/microsoft.azure.keyvault.keyvaultclientextensions.getsecretasync).

```python
client.delete_secret(secretName)
```

Gizli [az keyvault gizli gösteri](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutu ile gitti doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, anahtar kasanızı ve ilgili kaynak grubunu kaldırmak için Azure CLI veya Azure PowerShell'i kullanabilirsiniz.

```azurecli
az group delete -g "myResourceGroup"
```

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup"
```

## <a name="sample-code"></a>Örnek kod

```python
import os
import cmd
from azure.keyvault.secrets import SecretClient
from azure.identity import DefaultAzureCredential

keyVaultName = os.environ["KEY_VAULT_NAME"]
KVUri = "https://" + keyVaultName + ".vault.azure.net"

credential = DefaultAzureCredential()
client = SecretClient(vault_url=KVUri, credential=credential)

secretName = "mySecret"

print("Input the value of your secret > ")
secretValue = raw_input()

print("Creating a secret in " + keyVaultName + " called '" + secretName + "' with the value '" + secretValue + "` ...")

client.set_secret(secretName, secretValue)

print(" done.")

print("Forgetting your secret.")
secretValue = ""
print("Your secret is '" + secretValue + "'.")

print("Retrieving your secret from " + keyVaultName + ".")

retrieved_secret = client.get_secret(secretName)

print("Your secret is '" + retrieved_secret.value + "'.")
print("Deleting your secret from " + keyVaultName + " ...")

client.delete_secret(secretName)

print(" done.")
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir anahtar kasası oluşturdunuz, bir sır sakladınız ve bu sırrı aldınız. Key Vault ve uygulamalarınızla nasıl entegre edilebildiğini öğrenmek için aşağıdaki makalelere devam edin.

- Azure [Anahtar Kasasına Genel Bakış](key-vault-overview.md)
- Azure [Key Vault geliştiricisi kılavuzuna](key-vault-developers-guide.md) bakın
- [Anahtarlar, sırlar ve sertifikalar](about-keys-secrets-and-certificates.md) hakkında bilgi edinin
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](key-vault-best-practices.md)

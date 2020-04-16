---
title: Quickstart - Python için Azure Key Vault istemci kitaplığı
description: Python istemci kitaplığını kullanarak Azure anahtar kasasından nasıl anahtar oluşturup, alınve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 3/30/2020
ms.service: key-vault
ms.subservice: keys
ms.topic: quickstart
ms.openlocfilehash: e6120d5961dc31845c1322d052d46b52f4d2be6c
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424184"
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

Konsol penceresinden Python için Azure Anahtar Kasası anahtarkitaplığını yükleyin.

```console
pip install azure-keyvault-keys
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

Bulut tabanlı bir .NET uygulamasının kimliğini doğrulamanın en basit yolu yönetilen bir kimliktir; bkz. Ayrıntılar [için Azure Key Vault'a erişmek için Uygulama Hizmeti yönetilen kimliği kullanın.](../general/managed-identity.md) Ancak basitlik adına, bu hızlı başlatma bir .NET konsol uygulaması oluşturur. Azure ile bir masaüstü uygulamasının kimliğini doğrulamak için bir hizmet sorumlusu nun ve erişim denetim ilkesinin kullanılması nı gerektirir.

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

MüşteriYi [az keyvault set-ilke](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuna geçirerek servis müdürünüze izin veren anahtar kasanız için bir erişim ilkesi oluşturun. Hizmet sorumlusuna anahtarları al, listele ve anahtarlar için izinler oluşturma ver.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --key-permissions delete get list create 
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

Python için Azure Key Vault istemci kitaplığı, anahtarları ve sertifikalar ve sırlar gibi ilgili varlıkları yönetmenize olanak tanır. Aşağıdaki kod örnekleri, bir istemci oluşturmak, bir anahtar oluşturmak, bir anahtar almak ve bir anahtar silmek nasıl gösterecektir.

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeekleme

Kodunuzun üst bölümüne aşağıdaki yönergeleri ekleyin:

```python
import os
from azure.keyvault.keys import KeyClient
from azure.identity import DefaultAzureCredential
```

### <a name="authenticate-and-create-a-client"></a>Kimlik doğrulaması ve istemci oluşturma

Anahtar kasanıza doğrulanması ve anahtar kasa istemcisi oluşturma, yukarıdaki [Set çevresel değişkenler](#set-environmental-variables) adımındaki çevresel değişkenlere bağlıdır. Anahtar kasanızın adı URI anahtar kasasına genişletilir ve "https://<your-key-vault-name>.vault.azure.net" biçiminde genişletilir.

```python
credential = DefaultAzureCredential()

client = KeyClient(vault_url=KVUri, credential=credential)
```

### <a name="save-a-key"></a>Bir anahtarı kaydetme

Başvurunuzun kimliği doğrulanına göre, keyvault'unuza bir anahtar koyabilirsiniz 

```python
rsa_key = client.create_rsa_key(myKey,size=2048)
```

Anahtarın [az keyvault tuş göster](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-show) komutu yla ayarlandığını doğrulayabilirsiniz:

```azurecli
az keyvault key show --vault-name <your-unique-keyvault-name> --name myKey
```

### <a name="retrieve-a-key"></a>Bir anahtar alma

Artık daha önce oluşturulmuş anahtarı alabilirsiniz

```python
retrieved_key = client.get_key(keyName)
print(retrieve_key.name)

 ```

Anahtarınız artık `retrieved_key`.

### <a name="delete-a-key"></a>Bir anahtarı silme

Son olarak, anahtarı kasanızdan silelim

```python
client.delete_key(keyName)
```

Anahtarın [az keyvault tuş göster](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-show) komutu yla gittiğini doğrulayabilirsiniz:

```azurecli
az keyvault key show --vault-name <your-unique-keyvault-name> --name myKey
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
from azure.keyvault.key import KeyClient
from azure.identity import DefaultAzureCredential

keyVaultName = os.environ["KEY_VAULT_NAME"]
KVUri = "https://" + keyVaultName + ".vault.azure.net"

credential = DefaultAzureCredential()
client = KeyClient(vault_url=KVUri, credential=credential)

keyName = "myKey"

print("Creating a key in " + keyVaultName + " called '" + keyName  + "` ...")

rsa_key = client.create_rsa_key(myKey,size=2048)

print(" done.")

print("Retrieving your key from " + keyVaultName + ".")

retrieved_key = client.get_key(keyName)

print("Key with name '{0}' was found'.".format(retrieved_key.name))
print("Deleting your key from " + keyVaultName + " ...")

client.begin_delete_key(keyName).result()

print(" done.")
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir anahtar kasası oluşturdunuz, bir anahtar depolamadınız ve bu anahtarı aldınız. Key Vault ve uygulamalarınızla nasıl entegre edilebildiğini öğrenmek için aşağıdaki makalelere devam edin.

- Azure [Anahtar Kasasına Genel Bakış](../general/overview.md)
- Azure [Key Vault geliştiricisi kılavuzuna](../general/developers-guide.md) bakın
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](../general/best-practices.md)

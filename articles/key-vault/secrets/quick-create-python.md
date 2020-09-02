---
title: Hızlı başlangıç – Azure Key Vault Python istemci kitaplığı – gizli dizileri yönetme
description: Python istemci kitaplığını kullanarak bir Azure Anahtar Kasası 'nda gizli dizileri oluşturmayı, almayı ve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 10/20/2019
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: devx-track-python
ms.openlocfilehash: f1f044eb3af35019eaf010e118bc4a5814269e9e
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89378568"
---
# <a name="quickstart-azure-key-vault-secrets-client-library-for-python"></a>Hızlı başlangıç: Python için Azure Key Vault gizli dizi istemci kitaplığı

Python için Azure Key Vault istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek üzere aşağıdaki adımları izleyin.

Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Python için Key Vault istemci kitaplığını şu şekilde kullanın:

- Anahtarlar ve parolalar üzerinde güvenlik ve denetim düzeyini artırın.
- Şifreleme anahtarlarını dakikalar içinde oluşturun ve içeri aktarın.
- Bulut ölçeği ve küresel yedeklilik ile gecikme süresini azaltın.
- TLS/SSL sertifikaları için görevleri basitleştirme ve otomatikleştirme.
- FIPS 140-2 düzey 2 doğrulanan HSM 'leri kullanın.

[API başvuru belgeleri](/python/api/overview/azure/keyvault-secrets-readme?view=azure-python)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-secrets)  |  [Paket (Python paket dizini)](https://pypi.org/project/azure-keyvault-secrets/)

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Python 2,7, 3.5.3 veya üzeri
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell](/powershell/azure/)

Bu hızlı başlangıçta, [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) 'Yi bir Linux Terminal penceresinde çalıştırdığınız varsayılır.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="install-the-package"></a>Paketi yükler

Konsol penceresinde, Python için Azure Key Vault gizli dizi Kitaplığı ' nı yükler.

```console
pip install azure-keyvault-secrets
```

Bu hızlı başlangıçta, Azure. Identity paketini de yüklemeniz gerekecektir:

```console
pip install azure.identity
```

### <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve Anahtar Kasası oluşturma

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-rg-kv-creation.md)]

### <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[!INCLUDE [Create a service principal](../../../includes/key-vault-sp-creation.md)]

#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Anahtar kasanıza hizmet sorumlusu erişimi verin

[!INCLUDE [Give the service principal access to your key vault](../../../includes/key-vault-sp-kv-access.md)]

#### <a name="set-environmental-variables"></a>Ortam değişkenlerini ayarlama

[!INCLUDE [Set environmental variables](../../../includes/key-vault-set-environmental-variables.md)]

## <a name="object-model"></a>Nesne modeli

Python için Azure Key Vault istemci kitaplığı, sertifikaları ve gizli dizileri gibi anahtarları ve ilgili varlıkları yönetmenizi sağlar. Aşağıdaki kod örnekleri, bir istemci oluşturma, gizli anahtar ayarlama, gizli anahtar alma ve gizli dizi silme işlemlerinin nasıl yapılacağını gösterir.

Bu makalede gösterildiğine benzer işlemleri gösteren örnek uygulama, diğer Key Vault özellikleri de [GitHub](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-secrets/samples)'da mevcuttur.

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeler ekleme

Aşağıdaki yönergeleri kodunuzun en üstüne ekleyin:

```python
import os
from azure.keyvault.secrets import SecretClient
from azure.identity import DefaultAzureCredential
```

### <a name="authenticate-and-create-a-client"></a>İstemci kimliğini doğrulama ve oluşturma

Anahtar kasanıza kimlik doğrulama ve Anahtar Kasası istemcisi oluşturma, yukarıdaki [ortam değişkenlerini ayarla](#set-environmental-variables) adımında bulunan ortam değişkenlerine bağlıdır. Anahtar kasanızın adı, Anahtar Kasası URI 'sine, "https://<-Key-kasa-adı>. vault.azure.net" biçiminde genişletilir.

```python
credential = DefaultAzureCredential()

client = SecretClient(vault_url=KVUri, credential=credential)
```

### <a name="save-a-secret"></a>Gizli dizi Kaydet

Uygulamanızın kimliği doğrulandığına göre, istemciyi kullanarak anahtar kasanıza gizli dizi ekleyebilirsiniz. [set_secret yöntemi](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#set-secret-name--value----kwargs-) Bu, gizli dizi için bir ad gerektirir. Bu örnekteki "mySecret" i kullanıyoruz.  

```python
client.set_secret(secretName, secretValue)
```

Parolanın [az keykasası Secret Show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutuyla ayarlandığını doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

### <a name="retrieve-a-secret"></a>Gizli dizi alma

Artık [get_secret yöntemiyle](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#get-secret-name--version-none----kwargs-)daha önceden ayarlanan değeri alabilirsiniz.

```python
retrieved_secret = client.get_secret(secretName)
 ```

Gizli anahtar artık olarak kaydedilir `retrieved_secret.value` .

### <a name="delete-a-secret"></a>Gizli anahtarı silme

Son olarak, [begin_delete_secret yöntemiyle](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#begin-delete-secret-name----kwargs-)anahtar kasaınızdan parolayı silelim.

```python
client.begin_delete_secret(secretName)
```

Parolanın [az keykasasecret Show](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutuyla yapıldığını doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, anahtar Kasanızı ve ilgili kaynak grubunu kaldırmak için Azure CLı veya Azure PowerShell kullanabilirsiniz.

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
KVUri = f"https://{keyVaultName}.vault.azure.net"

credential = DefaultAzureCredential()
client = SecretClient(vault_url=KVUri, credential=credential)

secretName = "mySecret"

print("Input the value of your secret > ")
secretValue = raw_input()

print(f"Creating a secret in {keyVaultName} called '{secretName}' with the value '{secretValue}` ...")

client.set_secret(secretName, secretValue)

print(" done.")

print("Forgetting your secret.")
secretValue = ""
print(f"Your secret is {secretValue}.")

print(f"Retrieving your secret from {keyVaultName}.")

retrieved_secret = client.get_secret(secretName)

print(f"Your secret is '{retrieved_secret.value}'.")
print(f"Deleting your secret from {keyVaultName} ...")

client.begin_delete_secret(secretName)

print(" done.")
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Anahtar Kasası oluşturdunuz, gizli dizi depolandı ve bu gizli dizi alındı. Key Vault ve uygulamalarınızla tümleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere ilerleyin.

- [Azure Anahtar Kasası'na Genel Bakış](../general/overview.md)
- [Geliştirici Kılavuzu Azure Key Vault](../general/developers-guide.md)
- [En iyi Azure Key Vault uygulamalar](../general/best-practices.md)

---
title: Hızlı başlangıç – Azure Key Vault Python istemci kitaplığı – anahtarları Yönet
description: Python istemci kitaplığını kullanarak Azure Anahtar Kasası 'ndan anahtar oluşturmayı, almayı ve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 3/30/2020
ms.service: key-vault
ms.subservice: keys
ms.topic: quickstart
ms.custom: devx-track-python
ms.openlocfilehash: 1352d3fe4a13202a296b79fe82343c50a9e281d1
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89400424"
---
# <a name="quickstart-azure-key-vault-keys-client-library-for-python"></a>Hızlı başlangıç: Python için Azure Key Vault anahtarlar istemci kitaplığı

Python için Azure Key Vault istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek üzere aşağıdaki adımları izleyin.

Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Python için Key Vault istemci kitaplığını şu şekilde kullanın:

- Anahtarlar ve parolalar üzerinde güvenlik ve denetim düzeyini artırın.
- Şifreleme anahtarlarını dakikalar içinde oluşturun ve içeri aktarın.
- Bulut ölçeği ve küresel yedeklilik ile gecikme süresini azaltın.
- TLS/SSL sertifikaları için görevleri basitleştirme ve otomatikleştirme.
- FIPS 140-2 düzey 2 doğrulanan HSM 'leri kullanın.

[API başvuru belgeleri](/python/api/overview/azure/keyvault-keys-readme?view=azure-python)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-keys)  |  [Paket (Python paket dizini)](https://pypi.org/project/azure-keyvault-keys/)

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Python 2,7, 3.5.3 veya üzeri
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell](/powershell/azure/)

Bu hızlı başlangıçta, [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) 'Yi bir Linux Terminal penceresinde çalıştırdığınız varsayılır.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="install-the-package"></a>Paketi yükler

Konsol penceresinde, Python için Azure Key Vault anahtarları kitaplığını yükler.

```console
pip install azure-keyvault-keys
```

Bu hızlı başlangıçta, Azure. Identity paketini de yüklemeniz gerekecektir:

```console
pip install azure.identity
```

### <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve Anahtar Kasası oluşturma

Bu hızlı başlangıçta önceden oluşturulmuş bir Azure Anahtar Kasası kullanılmaktadır. [Azure CLI hızlı başlangıç](quick-create-cli.md), [Azure PowerShell hızlı](quick-create-powershell.md)başlangıç veya [Azure Portal Hızlı Başlangıç](quick-create-portal.md)adımlarını izleyerek bir Anahtar Kasası oluşturabilirsiniz. Alternatif olarak, aşağıdaki Azure CLı komutlarını çalıştırabilirsiniz.

> [!Important]
> Her Anahtar Kasası benzersiz bir ada sahip olmalıdır. -Unique-keykasa-adı> <aşağıdaki örneklerde anahtar kasanızın adıyla değiştirin.

```azurecli
az group create --name "myResourceGroup" -l "EastUS"

az keyvault create --name <your-unique-keyvault-name> -g "myResourceGroup"
```

### <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bulut tabanlı bir uygulamanın kimlik doğrulamasının en kolay yolu, yönetilen bir kimlikle; Ayrıntılar için bkz. [Azure Key Vault Için kimlik doğrulama](../general/authentication.md) . 

Kolaylık sağlaması için, bu hızlı başlangıç, bir hizmet sorumlusu ve bir erişim denetimi ilkesi kullanılmasını gerektiren bir masaüstü uygulaması oluşturur. Hizmet sorumlusu, "http:// &lt; My-Unique-Service-Principal-Name" biçiminde benzersiz bir ad gerektirir &gt; .

Azure CLı [az ad SP Create-for-RBAC](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir hizmet sorumlusu oluşturun:

```azurecli
az ad sp create-for-rbac -n "http://&lt;my-unique-service-principal-name&gt;" --sdk-auth
```

Bu işlem, bir dizi anahtar/değer çifti döndürür. 

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

Aşağıdaki [ortam değişkenini ayarla](#set-environmental-variables) adımında kullanabilmemiz Için ClientID ve ClientSecret değerlerini göz önünde ayırın.

#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Anahtar kasanıza hizmet sorumlusu erişimi verin

ClientID 'yi [az keykasa Set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuna geçirerek, hizmet sorumlusuna izin veren Anahtar Kasası için bir erişim ilkesi oluşturun. Anahtarlar için hizmet sorumlusu al, Listele ve oluştur izinlerini verin.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --key-permissions delete get list create 
```

#### <a name="set-environmental-variables"></a>Ortam değişkenlerini ayarlama

Uygulamamızda DefaultAzureCredential yöntemi üç çevresel değişkene dayanır: `AZURE_CLIENT_ID` , `AZURE_CLIENT_SECRET` ve `AZURE_TENANT_ID` . Bu değişkenleri biçimini kullanarak [hizmet sorumlusu oluşturma](#create-a-service-principal) adımında not ettiğiniz ClientID, ClientSecret ve tenantıd değerlerine ayarlayın `export VARNAME=VALUE` . (Bu yöntem yalnızca geçerli kabuğunuzun ve kabuktan oluşturulan süreçlerin değişkenlerini ayarlar; bu değişkenleri ortamınıza kalıcı olarak eklemek için `/etc/environment ` dosyanızı düzenleyin.) 

Ayrıca, Anahtar Kasası adınızı adlı bir ortam değişkeni olarak kaydetmeniz gerekir `KEY_VAULT_NAME` .

```console
export AZURE_CLIENT_ID=<your-clientID>

export AZURE_CLIENT_SECRET=<your-clientSecret>

export AZURE_TENANT_ID=<your-tenantId>

export KEY_VAULT_NAME=<your-key-vault-name>
````

## <a name="object-model"></a>Nesne modeli

Python için Azure Key Vault istemci kitaplığı, sertifikaları ve gizli dizileri gibi anahtarları ve ilgili varlıkları yönetmenizi sağlar. Aşağıdaki kod örnekleri, bir istemci oluşturma, anahtar oluşturma, anahtar alma ve bir anahtarı silme işlemlerinin nasıl yapılacağını gösterecektir.

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeler ekleme

Aşağıdaki yönergeleri kodunuzun en üstüne ekleyin:

```python
import os
from azure.keyvault.keys import KeyClient
from azure.identity import DefaultAzureCredential
```

### <a name="authenticate-and-create-a-client"></a>İstemci kimliğini doğrulama ve oluşturma

Anahtar kasanıza kimlik doğrulama ve Anahtar Kasası istemcisi oluşturma, yukarıdaki [ortam değişkenlerini ayarla](#set-environmental-variables) adımında bulunan ortam değişkenlerine bağlıdır. Anahtar kasanızın adı, Anahtar Kasası URI 'sine, "https://<-Key-kasa-adı>. vault.azure.net" biçiminde genişletilir.

```python
credential = DefaultAzureCredential()

client = KeyClient(vault_url=KVUri, credential=credential)
```

### <a name="save-a-key"></a>Anahtar kaydetme

Uygulamanızın kimliğinin doğrulandığına göre, anahtar kasanıza bir anahtar koyabilirsiniz 

```python
rsa_key = client.create_rsa_key(myKey,size=2048)
```

Anahtarın, [az keykasakey Show](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-show) komutuyla ayarlandığını doğrulayabilirsiniz:

```azurecli
az keyvault key show --vault-name <your-unique-keyvault-name> --name myKey
```

### <a name="retrieve-a-key"></a>Anahtar alma

Artık önceden oluşturulan anahtarı alabilirsiniz

```python
retrieved_key = client.get_key(keyName)
print(retrieve_key.name)

 ```

Anahtarınız şu şekilde kaydedilir `retrieved_key` .

### <a name="delete-a-key"></a>Bir anahtarı silme

Son olarak, anahtar kasaınızdan anahtarı silemem

```python
client.delete_key(keyName)
```

Anahtarın [az keykasakey Show](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-show) komutuyla yapıldığını doğrulayabilirsiniz:

```azurecli
az keyvault key show --vault-name <your-unique-keyvault-name> --name myKey
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

Bu hızlı başlangıçta bir Anahtar Kasası oluşturdunuz, bir anahtar depo, ve bu anahtarı almıştır. Key Vault ve uygulamalarınızla tümleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere ilerleyin.

- [Azure Anahtar Kasası'na Genel Bakış](../general/overview.md)
- [Geliştirici Kılavuzu Azure Key Vault](../general/developers-guide.md)
- [En iyi Azure Key Vault uygulamalar](../general/best-practices.md)

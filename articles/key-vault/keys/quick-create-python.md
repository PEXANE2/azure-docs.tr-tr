---
title: Hızlı başlangıç – Azure Key Vault Python istemci kitaplığı – anahtarları Yönet
description: Python istemci kitaplığını kullanarak Azure Anahtar Kasası 'ndan anahtar oluşturmayı, almayı ve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 09/03/2020
ms.service: key-vault
ms.subservice: keys
ms.topic: quickstart
ms.custom: devx-track-python
ms.openlocfilehash: 44942067756f82c224decc218de17bf7dbc69734
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89482154"
---
# <a name="quickstart-azure-key-vault-keys-client-library-for-python"></a>Hızlı başlangıç: Python için Azure Key Vault anahtarlar istemci kitaplığı

Python için Azure Key Vault istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek üzere aşağıdaki adımları izleyin. Şifreleme anahtarlarını depolamak için Key Vault kullanarak, kodunuzda bu anahtarların depolanmasını önleyin ve bu sayede uygulamanızın güvenliği artar.

[API başvuru belgeleri](/python/api/overview/azure/keyvault-keys-readme?view=azure-python)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-keys)  |  [Paket (Python paket dizini)](https://pypi.org/project/azure-keyvault-keys/)

## <a name="set-up-your-local-environment"></a>Yerel ortamınızı ayarlama

[!INCLUDE [Set up your local environment](../../../includes/key-vault-python-qs-setup.md)]

7. Key Vault anahtarları kitaplığını yükler:

    ```terminal
    pip install azure-keyvault-keys
    ```

## <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve Anahtar Kasası oluşturma

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-python-qs-rg-kv-creation.md)]

## <a name="give-the-service-principal-access-to-your-key-vault"></a>Anahtar kasanıza hizmet sorumlusu erişimi verin

Hizmet sorumlusuna silme, Get, listeleme ve oluşturma işlemlerini anahtarlar üzerinde yetkilendirmek için aşağıdaki [az keykasası Set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutunu çalıştırın. 

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az keyvault set-policy --name %KEY_VAULT_NAME% --spn %AZURE_CLIENT_ID% --resource-group KeyVault-PythonQS-rg --key-permissions delete get list create
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
az keyvault set-policy --name $KEY_VAULT_NAME --spn $AZURE_CLIENT_ID --resource-group KeyVault-PythonQS-rg --key-permissions delete get list create
```

---

Bu komut, `KEY_VAULT_NAME` `AZURE_CLIENT_ID` önceki adımlarda oluşturulan ve ortam değişkenlerini kullanır.

Daha fazla bilgi için bkz. [erişim Ilkesi atama-CLI](../general/assign-access-policy-cli.md)

## <a name="create-the-sample-code"></a>Örnek kodu oluşturma

Python için Azure Key Vault istemci kitaplığı, şifreleme anahtarlarını ve sertifikalar ve gizli dizileri gibi ilgili varlıkları yönetmenizi sağlar. Aşağıdaki kod örneği, bir istemci oluşturma, gizli anahtar ayarlama, gizli anahtar alma ve gizli dizi silme işlemlerinin nasıl yapılacağını gösterir.

Bu kodu içeren *kv_keys. Kopyala* adlı bir dosya oluşturun.

```python
import os
from azure.keyvault.keys import KeyClient
from azure.identity import DefaultAzureCredential

keyVaultName = os.environ["KEY_VAULT_NAME"]
KVUri = "https://" + keyVaultName + ".vault.azure.net"

credential = DefaultAzureCredential()
client = KeyClient(vault_url=KVUri, credential=credential)

keyName = input("Input a name for your key > ")

print(f"Creating a key in {keyVaultName} called '{keyName}' ...")

rsa_key = client.create_rsa_key(keyName, size=2048)

print(" done.")

print(f"Retrieving your key from {keyVaultName}.")

retrieved_key = client.get_key(keyName)

print(f"Key with name '{retrieved_key.name}' was found.")
print(f"Deleting your key from {keyVaultName} ...")

poller = client.begin_delete_key(keyName)
deleted_key = poller.result()

print(" done.")
```

## <a name="run-the-code"></a>Kodu çalıştırma

Önceki bölümdeki kodun *kv_keys. Kopyala*adlı bir dosyada bulunduğundan emin olun. Ardından aşağıdaki komutla kodu çalıştırın:

```terminal
python kv_keys.py
```

- İzin hatalarıyla karşılaşırsanız, [ `az keyvault set-policy` komutunu](#give-the-service-principal-access-to-your-key-vault)çalıştırdığınızdan emin olun.
- Kodu aynı anahtar adıyla yeniden çalıştırmak, "(çakışma) anahtarı <name> Şu anda silinmiş ancak kurtarılabilir durumda." hatasını verebilir. Farklı bir anahtar adı kullanın.

## <a name="code-details"></a>Kod ayrıntıları

### <a name="authenticate-and-create-a-client"></a>İstemci kimliğini doğrulama ve oluşturma

Yukarıdaki kodda, [`DefaultAzureCredential`](/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python) nesnesi hizmet sorumlusu için oluşturduğunuz ortam değişkenlerini kullanır. Bu kimlik bilgilerini bir Azure kitaplığından, örneğin [`KeyClient`](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python) , bu istemci aracılığıyla birlikte çalışmak istediğiniz KAYNAĞıN URI 'siyle birlikte her bir istemci nesnesi oluştururken sağlarsınız:

```python
credential = DefaultAzureCredential()
client = KeyClient(vault_url=KVUri, credential=credential)
```

## <a name="save-a-key"></a>Anahtar kaydetme

Anahtar Kasası için istemci nesnesini aldıktan sonra, [create_rsa_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#create-rsa-key-name----kwargs-) yöntemini kullanarak bir anahtar saklayabilirsiniz: 

```python
rsa_key = client.create_rsa_key(keyName, size=2048)
```

[Create_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#create-key-name--key-type----kwargs-) veya [create_ec_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#create-ec-key-name----kwargs-)de kullanabilirsiniz.

Bir yöntemi çağırmak, `create` Anahtar Kasası Için Azure REST API bir çağrı oluşturur.

Azure, isteği gerçekleştirirken arayanın kimliğini (hizmet sorumlusu) istemciye verdiğiniz kimlik bilgisi nesnesini kullanarak doğrular.

Ayrıca, çağıranın istenen eylemi gerçekleştirme yetkisine sahip olup olmadığını denetler. Bu yetkilendirmeyi, [ `az keyvault set-policy` komutunu](#give-the-service-principal-access-to-your-key-vault)kullanarak daha önce hizmet sorumlusuna vermiş olursunuz.

## <a name="retrieve-a-key"></a>Anahtar alma

Key Vault bir anahtarı okumak için [get_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#get-key-name--version-none----kwargs-) yöntemi kullanın:

```python
retrieved_key = client.get_key(keyName)
 ```

Anahtarın, [az keykasakey Show](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-show)Azure CLI komutuyla ayarlandığını da doğrulayabilirsiniz.

### <a name="delete-a-key"></a>Bir anahtarı silme

Bir anahtarı silmek için [begin_delete_key](/python/api/azure-keyvault-keys/azure.keyvault.keys.keyclient?view=azure-python#begin-delete-key-name----kwargs-) yöntemi kullanın:

```python
poller = client.begin_delete_key(keyName)
deleted_key = poller.result()
```

`begin_delete_key`Yöntemi zaman uyumsuzdur ve bir Poller nesnesi döndürür. Poller 'ın metodunu çağırma işleminin `result` tamamlanmasını bekler.

Anahtarın Azure CLı komutuyla silindiğini, [az keykasakey Show](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-show)komutunu doğrulayabilirsiniz.

Silinen bir anahtar, bir süredir silinmiş ancak kurtarılabilir durumunda kalır. Kodu yeniden çalıştırırsanız, farklı bir anahtar adı kullanın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Ayrıca, [Sertifikalar](../certificates/quick-create-python.md) ve [gizli](../secrets/quick-create-python.md)dizileri de denemek istiyorsanız, bu makalede oluşturulan Key Vault yeniden kullanabilirsiniz.

Aksi takdirde, bu makalede oluşturulan kaynaklarla işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silmek için aşağıdaki komutu kullanın:

```azurecli
az group delete --resource-group KeyVault-PythonQS-rg
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Anahtar Kasası'na Genel Bakış](../general/overview.md)
- [Geliştirici Kılavuzu Azure Key Vault](../general/developers-guide.md)
- [En iyi Azure Key Vault uygulamalar](../general/best-practices.md)
- [Key Vault ile kimlik doğrulama](../general/authentication.md)

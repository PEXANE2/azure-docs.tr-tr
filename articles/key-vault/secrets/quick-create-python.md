---
title: Hızlı başlangıç – Azure Key Vault Python istemci kitaplığı – gizli dizileri yönetme
description: Python istemci kitaplığını kullanarak bir Azure Anahtar Kasası 'nda gizli dizileri oluşturmayı, almayı ve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 09/03/2020
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.custom: devx-track-python
ms.openlocfilehash: cd8a5751c018b9b3b3b2ef96765545f2edab685b
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89489213"
---
# <a name="quickstart-azure-key-vault-secrets-client-library-for-python"></a>Hızlı başlangıç: Python için Azure Key Vault gizli dizi istemci kitaplığı

Python için Azure Key Vault istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek üzere aşağıdaki adımları izleyin. Gizli dizileri depolamak için Key Vault kullanarak, kodunuzun içinde gizli dizileri saklamaktan kaçının ve bu, uygulamanızın güvenliğini artırır.

[API başvuru belgeleri](/python/api/overview/azure/keyvault-secrets-readme?view=azure-python)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-secrets)  |  [Paket (Python paket dizini)](https://pypi.org/project/azure-keyvault-secrets/)

## <a name="set-up-your-local-environment"></a>Yerel ortamınızı ayarlama

[!INCLUDE [Set up your local environment](../../../includes/key-vault-python-qs-setup.md)]

7. Key Vault gizli dizileri kitaplığı 'nı yükler:

    ```terminal
    pip install azure-keyvault-secrets
    ```

## <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve Anahtar Kasası oluşturma

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-python-qs-rg-kv-creation.md)]

## <a name="give-the-service-principal-access-to-your-key-vault"></a>Anahtar kasanıza hizmet sorumlusu erişimi verin

Hizmet sorumlusuna Get, List ve gizli dizi işlemlerinde ayarlama işlemlerini yetkilendirmek için aşağıdaki [az keykasası Set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutunu çalıştırın. Bu komut, `KEY_VAULT_NAME` `AZURE_CLIENT_ID` önceki adımlarda oluşturulan ve ortam değişkenlerini kullanır.

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az keyvault set-policy --name %KEY_VAULT_NAME% --spn %AZURE_CLIENT_ID% --resource-group KeyVault-PythonQS-rg --secret-permissions delete get list set 
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
az keyvault set-policy --name $KEY_VAULT_NAME --spn $AZURE_CLIENT_ID --resource-group KeyVault-PythonQS-rg --secret-permissions delete get list set 
```

---

Bu komut, `KEY_VAULT_NAME` `AZURE_CLIENT_ID` önceki adımlarda oluşturulan ve ortam değişkenlerini kullanır.

Daha fazla bilgi için bkz. [erişim Ilkesi atama-CLI](../general/assign-access-policy-cli.md)

## <a name="create-the-sample-code"></a>Örnek kodu oluşturma

Python için Azure Key Vault istemci kitaplığı, sertifikalar ve şifreleme anahtarları gibi gizli dizileri ve ilgili varlıkları yönetmenizi sağlar. Aşağıdaki kod örneği, bir istemci oluşturma, gizli anahtar ayarlama, gizli anahtar alma ve gizli dizi silme işlemlerinin nasıl yapılacağını gösterir.

Bu kodu içeren *kv_secrets. Kopyala* adlı bir dosya oluşturun.

```python
import os
import cmd
from azure.keyvault.secrets import SecretClient
from azure.identity import DefaultAzureCredential

keyVaultName = os.environ["KEY_VAULT_NAME"]
KVUri = f"https://{keyVaultName}.vault.azure.net"

credential = DefaultAzureCredential()
client = SecretClient(vault_url=KVUri, credential=credential)

secretName = input("Input a name for your secret > ")
secretValue = input("Input a value for your secret > ")

print(f"Creating a secret in {keyVaultName} called '{secretName}' with the value '{secretValue}' ...")

client.set_secret(secretName, secretValue)

print(" done.")

print(f"Retrieving your secret from {keyVaultName}.")

retrieved_secret = client.get_secret(secretName)

print(f"Your secret is '{retrieved_secret.value}'.")
print(f"Deleting your secret from {keyVaultName} ...")

poller = client.begin_delete_secret(secretName)
deleted_secret = poller.result()

print(" done.")
```

## <a name="run-the-code"></a>Kodu çalıştırma

Önceki bölümdeki kodun *kv_secrets. Kopyala*adlı bir dosyada bulunduğundan emin olun. Ardından aşağıdaki komutla kodu çalıştırın:

```terminal
python kv_secrets.py
```

- İzin hatalarıyla karşılaşırsanız, [ `az keyvault set-policy` komutunu](#give-the-service-principal-access-to-your-key-vault)çalıştırdığınızdan emin olun.
- Aynı secrete adlı kodu yeniden çalıştırmak, "(çakışma) gizli anahtarı <name> Şu anda silinmiş ancak kurtarılabilir durumda." hatasını verebilir. Farklı bir gizli dizi adı kullanın.

## <a name="code-details"></a>Kod ayrıntıları

### <a name="authenticate-and-create-a-client"></a>İstemci kimliğini doğrulama ve oluşturma

Yukarıdaki kodda, [`DefaultAzureCredential`](/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python) nesnesi hizmet sorumlusu için oluşturduğunuz ortam değişkenlerini kullanır. Bu kimlik bilgilerini bir Azure kitaplığından, örneğin [`SecretClient`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python) , bu istemci aracılığıyla birlikte çalışmak istediğiniz KAYNAĞıN URI 'siyle birlikte her bir istemci nesnesi oluştururken sağlarsınız:

```python
credential = DefaultAzureCredential()
client = SecretClient(vault_url=KVUri, credential=credential)
```

### <a name="save-a-secret"></a>Gizli dizi Kaydet

Anahtar Kasası için istemci nesnesini aldıktan sonra [set_secret](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#set-secret-name--value----kwargs-) yöntemi kullanarak bir gizli dizi saklayabilirsiniz: 

```python
client.set_secret(secretName, secretValue)
```

Çağırma `set_secret` , Anahtar Kasası Için Azure REST API bir çağrı oluşturur.

Azure, isteği gerçekleştirirken arayanın kimliğini (hizmet sorumlusu) istemciye verdiğiniz kimlik bilgisi nesnesini kullanarak doğrular.

Ayrıca, çağıranın istenen eylemi gerçekleştirme yetkisine sahip olup olmadığını denetler. Bu yetkilendirmeyi, [ `az keyvault set-policy` komutunu](#give-the-service-principal-access-to-your-key-vault)kullanarak daha önce hizmet sorumlusuna vermiş olursunuz.

### <a name="retrieve-a-secret"></a>Gizli dizi alma

Key Vault bir gizli dizi okumak için [get_secret](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#get-secret-name--version-none----kwargs-) metodunu kullanın:

```python
retrieved_secret = client.get_secret(secretName)
 ```

Gizli anahtar değeri içinde bulunur `retrieved_secret.value` .

Azure CLı komutu [az keykasa gizli göster](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show)komutuyla da bir gizli dizi alabilirsiniz.

### <a name="delete-a-secret"></a>Gizli anahtarı silme

Bir parolayı silmek için [begin_delete_secret](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#begin-delete-secret-name----kwargs-) yöntemi kullanın:

```python
poller = client.begin_delete_secret(secretName)
deleted_secret = poller.result()
```

`begin_delete_secret`Yöntemi zaman uyumsuzdur ve bir Poller nesnesi döndürür. Poller 'ın metodunu çağırma işleminin `result` tamamlanmasını bekler.

Azure CLı komutu [az keykasa gizli göster](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show)komutuyla parolanın kaldırıldığını doğrulayabilirsiniz.

Silindikten sonra gizli anahtar, bir zaman için silinmiş ancak kurtarılabilir durumda kalır. Kodu yeniden çalıştırırsanız, farklı bir gizli dizi adı kullanın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Ayrıca, [Sertifikalar](../certificates/quick-create-python.md) ve [anahtarlarla](../keys/quick-create-python.md)denemeler yapmak istiyorsanız, bu makalede oluşturulan Key Vault yeniden kullanabilirsiniz.

Aksi takdirde, bu makalede oluşturulan kaynaklarla işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silmek için aşağıdaki komutu kullanın:

```azurecli
az group delete --resource-group KeyVault-PythonQS-rg
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Anahtar Kasası'na Genel Bakış](../general/overview.md)
- [Geliştirici Kılavuzu Azure Key Vault](../general/developers-guide.md)
- [En iyi Azure Key Vault uygulamalar](../general/best-practices.md)
- [Key Vault ile kimlik doğrulama](../general/authentication.md)

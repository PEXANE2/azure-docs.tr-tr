---
title: Hızlı başlangıç – Azure Key Vault Python istemci kitaplığı – sertifikaları yönetme
description: Python istemci kitaplığını kullanarak bir Azure anahtar kasasından sertifika oluşturmayı, almayı ve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 09/03/2020
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: devx-track-python
ms.openlocfilehash: b9ff7397ad29ac681e21c32608ade9c6ce557c37
ms.sourcegitcommit: de2750163a601aae0c28506ba32be067e0068c0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89488635"
---
# <a name="quickstart-azure-key-vault-certificates-client-library-for-python"></a>Hızlı başlangıç: Python için Azure Key Vault sertifikaları istemci kitaplığı

Python için Azure Key Vault istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek üzere aşağıdaki adımları izleyin. Sertifikaları depolamak için Key Vault kullanarak, kodunuzun içinde sertifikaların depolanmasını önleyin, bu da uygulamanızın güvenliğini artırır.

[API başvuru belgeleri](/python/api/overview/azure/keyvault-certificates-readme?view=azure-python)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-certificates)  |  [Paket (Python paket dizini)](https://pypi.org/project/azure-keyvault-certificates)

## <a name="set-up-your-local-environment"></a>Yerel ortamınızı ayarlama

[!INCLUDE [Set up your local environment](../../../includes/key-vault-python-qs-setup.md)]

7. Key Vault sertifikaları kitaplığı 'nı yükler:

    ```terminal
    pip install azure-keyvault-certificates
    ```

## <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve Anahtar Kasası oluşturma

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-python-qs-rg-kv-creation.md)]

## <a name="give-the-service-principal-access-to-your-key-vault"></a>Anahtar kasanıza hizmet sorumlusu erişimi verin

Hizmet sorumlunuzu sertifikalar üzerinde al, Listele ve oluştur işlemleri için yetkilendirmek üzere aşağıdaki [az keykasası Set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutunu çalıştırın.

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az keyvault set-policy --name %KEY_VAULT_NAME% --spn %AZURE_CLIENT_ID% --resource-group KeyVault-PythonQS-rg --certificate-permissions delete get list create
```

# <a name="bash"></a>[Bash](#tab/bash)

```azurecli
az keyvault set-policy --name $KEY_VAULT_NAME --spn $AZURE_CLIENT_ID --resource-group KeyVault-PythonQS-rg --certificate-permissions delete get list create 
```

---

Bu komut, `KEY_VAULT_NAME` `AZURE_CLIENT_ID` önceki adımlarda oluşturulan ve ortam değişkenlerini kullanır.

Daha fazla bilgi için bkz. [erişim Ilkesi atama-CLI](../general/assign-access-policy-cli.md)

## <a name="create-the-sample-code"></a>Örnek kodu oluşturma

Python için Azure Key Vault istemci kitaplığı, sertifikaları ve gizli dizileri ve şifreleme anahtarları gibi ilgili varlıkları yönetmenizi sağlar. Aşağıdaki kod örneği, bir istemci oluşturma, gizli anahtar ayarlama, gizli anahtar alma ve gizli dizi silme işlemlerinin nasıl yapılacağını gösterir.

Bu kodu içeren *kv_certificates. Kopyala* adlı bir dosya oluşturun.

```python
import os
from azure.keyvault.certificates import CertificateClient, CertificatePolicy,CertificateContentType, WellKnownIssuerNames 
from azure.identity import DefaultAzureCredential

keyVaultName = os.environ["KEY_VAULT_NAME"]
KVUri = "https://" + keyVaultName + ".vault.azure.net"

credential = DefaultAzureCredential()
client = CertificateClient(vault_url=KVUri, credential=credential)

certificateName = input("Input a name for your certificate > ")

print(f"Creating a certificate in {keyVaultName} called '{certificateName}' ...")

policy = CertificatePolicy.get_default()
poller = client.begin_create_certificate(certificate_name=certificateName, policy=policy)
certificate = poller.result()

print(" done.")

print(f"Retrieving your certificate from {keyVaultName}.")

retrieved_certificate = client.get_certificate(certificateName)

print(f"Certificate with name '{retrieved_certificate.name}' was found'.")
print(f"Deleting your certificate from {keyVaultName} ...")

poller = client.begin_delete_certificate(certificateName)
deleted_certificate = poller.result()

print(" done.")
```

## <a name="run-the-code"></a>Kodu çalıştırma

Önceki bölümdeki kodun *kv_certificates. Kopyala*adlı bir dosyada bulunduğundan emin olun. Ardından aşağıdaki komutla kodu çalıştırın:

```terminal
python kv_certificates.py
```

- İzin hatalarıyla karşılaşırsanız, [ `az keyvault set-policy` komutunu](#give-the-service-principal-access-to-your-key-vault)çalıştırdığınızdan emin olun.
- Kodu aynı anahtar adıyla yeniden çalıştırmak, "(çakışma) sertifikası <name> Şu anda silinmiş ancak kurtarılabilir durumda." hatasını verebilir. Farklı bir anahtar adı kullanın.

## <a name="code-details"></a>Kod ayrıntıları

### <a name="authenticate-and-create-a-client"></a>İstemci kimliğini doğrulama ve oluşturma

Yukarıdaki kodda, [`DefaultAzureCredential`](/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python) nesnesi hizmet sorumlusu için oluşturduğunuz ortam değişkenlerini kullanır. Bu kimlik bilgilerini bir Azure kitaplığından, örneğin [`CertificateClient`](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificateclient?view=azure-python) , bu istemci aracılığıyla birlikte çalışmak istediğiniz KAYNAĞıN URI 'siyle birlikte her bir istemci nesnesi oluştururken sağlarsınız:

```python
credential = DefaultAzureCredential()
client = CertificateClient(vault_url=KVUri, credential=credential)
```

### <a name="save-a-certificate"></a>Sertifika kaydetme

Anahtar Kasası için istemci nesnesini aldıktan sonra, [begin_create_certificate](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificateclient?view=azure-python#begin-create-certificate-certificate-name--policy----kwargs-) yöntemini kullanarak bir sertifika oluşturabilirsiniz: 

```python
policy = CertificatePolicy.get_default()
poller = client.begin_create_certificate(certificate_name=certificateName, policy=policy)
certificate = poller.result()
```

Burada sertifika, [CertificatePolicy. get_default](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificatepolicy?view=azure-python#get-default--) yöntemiyle elde edilen bir ilke gerektirir.

Bir yöntemi çağırmak, `begin_create_certificate` Anahtar Kasası Için Azure REST API zaman uyumsuz bir çağrı oluşturur. Zaman uyumsuz çağrı bir Poller nesnesi döndürür. İşlemin sonucunu beklemek için, Poller 'in `result` yöntemini çağırın.

Azure, isteği gerçekleştirirken arayanın kimliğini (hizmet sorumlusu) istemciye verdiğiniz kimlik bilgisi nesnesini kullanarak doğrular.

Ayrıca, çağıranın istenen eylemi gerçekleştirme yetkisine sahip olup olmadığını denetler. Bu yetkilendirmeyi, [ `az keyvault set-policy` komutunu](#give-the-service-principal-access-to-your-key-vault)kullanarak daha önce hizmet sorumlusuna vermiş olursunuz.

### <a name="retrieve-a-certificate"></a>Sertifika alma

Key Vault bir sertifikayı okumak için [get_certificate](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificateclient?view=azure-python#get-certificate-certificate-name----kwargs-) yöntemi kullanın:

```python
retrieved_certificate = client.get_certificate(certificateName)
 ```

Sertifikanın, [az keykasa sertifikası Show](/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-show)Azure CLI komutuyla ayarlandığını da doğrulayabilirsiniz.

### <a name="delete-a-certificate"></a>Sertifikayı silme

Bir sertifikayı silmek için [begin_delete_certificate](/python/api/azure-keyvault-certificates/azure.keyvault.certificates.certificateclient?view=azure-python#begin-delete-certificate-certificate-name----kwargs-) yöntemi kullanın:

```python
poller = client.begin_delete_certificate(certificateName)
deleted_certificate = poller.result()
```

`begin_delete_certificate`Yöntemi zaman uyumsuzdur ve bir Poller nesnesi döndürür. Poller 'ın metodunu çağırma işleminin `result` tamamlanmasını bekler.

Sertifikanın, [az keykasa sertifikası Show](/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-show)Azure CLI komutuyla silindiğini doğrulayabilirsiniz.

Silinen bir sertifika, bir süredir silinmiş ancak kurtarılabilir durumda kalır. Kodu yeniden çalıştırırsanız, farklı bir sertifika adı kullanın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[Gizli](../secrets/quick-create-python.md) dizileri ve [anahtarlarla](../keys/quick-create-python.md)de denemek istiyorsanız, bu makalede oluşturulan Key Vault yeniden kullanabilirsiniz.

Aksi takdirde, bu makalede oluşturulan kaynaklarla işiniz bittiğinde, kaynak grubunu ve içerdiği tüm kaynakları silmek için aşağıdaki komutu kullanın:

```azurecli
az group delete --resource-group KeyVault-PythonQS-rg
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Anahtar Kasası'na Genel Bakış](../general/overview.md)
- [Geliştirici Kılavuzu Azure Key Vault](../general/developers-guide.md)
- [En iyi Azure Key Vault uygulamalar](../general/best-practices.md)
- [Key Vault ile kimlik doğrulama](../general/authentication.md)

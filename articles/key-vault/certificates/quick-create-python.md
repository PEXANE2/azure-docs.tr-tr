---
title: Hızlı başlangıç-Python için Azure Key Vault istemci kitaplığı
description: Python istemci kitaplığını kullanarak bir Azure anahtar kasasından sertifika oluşturmayı, almayı ve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 3/30/2020
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: tracking-python
ms.openlocfilehash: 7c67bc77984af8a25c3514931ed0983df1751d5e
ms.sourcegitcommit: 398fecceba133d90aa8f6f1f2af58899f613d1e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2020
ms.locfileid: "85126136"
---
# <a name="quickstart-azure-key-vault-client-library-for-python"></a>Hızlı başlangıç: Python için Azure Key Vault istemci kitaplığı

Python için Azure Key Vault istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek üzere aşağıdaki adımları izleyin.

Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Python için Key Vault istemci kitaplığını şu şekilde kullanın:

- Anahtarlar ve parolalar üzerinde güvenlik ve denetim düzeyini artırın.
- Şifreleme anahtarlarını dakikalar içinde oluşturun ve içeri aktarın.
- Bulut ölçeği ve küresel yedeklilik ile gecikme süresini azaltın.
- TLS/SSL sertifikaları için görevleri basitleştirme ve otomatikleştirme.
- FIPS 140-2 düzey 2 doğrulanan HSM 'leri kullanın.

[API başvuru belgeleri](/python/api/overview/azure/keyvault-certificates-readme?view=azure-python)  |  [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault)  |  [Paket (Python paket dizini)](https://pypi.org/project/azure-keyvault/)

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Python 2,7, 3.5.3 veya üzeri
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell](/powershell/azure/overview)

Bu hızlı başlangıçta, [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) 'Yi bir Linux Terminal penceresinde çalıştırdığınız varsayılır.

## <a name="setting-up"></a>Ayarlanıyor

### <a name="install-the-package"></a>Paketi yükler

Konsol penceresinde, Python için Azure Key Vault sertifikaları kitaplığını yükler.

```console
pip install azure-keyvault-certificates
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

Bulut tabanlı bir .NET uygulamasının kimlik doğrulamasının en kolay yolu, yönetilen bir kimliktir; Ayrıntılar için [Azure Key Vault erişmek üzere App Service yönetilen bir kimlik kullanma](../general/managed-identity.md) konusuna bakın.

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

ClientID 'yi [az keykasa Set-Policy](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuna geçirerek, hizmet sorumlusuna izin veren Anahtar Kasası için bir erişim ilkesi oluşturun. Sertifika için hizmet sorumlusu al, Listele ve oluştur izinlerini verin.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --certificate-permissions delete get list create 
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

Python için Azure Key Vault istemci kitaplığı, sertifikaları ve gizli dizileri gibi anahtarları ve ilgili varlıkları yönetmenizi sağlar. Aşağıdaki kod örnekleri, bir istemci oluşturma, sertifika oluşturma, sertifika alma ve sertifikayı silme işlemlerinin nasıl yapılacağını gösterecektir.

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeler ekleme

Aşağıdaki yönergeleri kodunuzun en üstüne ekleyin:

```python
import os
from azure.keyvault.certificates import CertificateClient, CertificatePolicy,CertificateContentType, WellKnownIssuerNames 
from azure.identity import DefaultAzureCredential
```

### <a name="authenticate-and-create-a-client"></a>İstemci kimliğini doğrulama ve oluşturma

Anahtar kasanıza kimlik doğrulama ve Anahtar Kasası istemcisi oluşturma, yukarıdaki [ortam değişkenlerini ayarla](#set-environmental-variables) adımında bulunan ortam değişkenlerine bağlıdır. Anahtar kasanızın adı, Anahtar Kasası URI 'sine, "https://<-Key-kasa-adı>. vault.azure.net" biçiminde genişletilir.

```python
credential = DefaultAzureCredential()

client = CertificateClient(vault_url=KVUri, credential=credential)
```

### <a name="save-a-certificate"></a>Sertifika kaydetme

Uygulamanızın kimliğinin doğrulandığına göre, anahtar kasanıza kendinden imzalı bir sertifika koyabilirsiniz 

```python
certificate_operation_poller = client.begin_create_certificate(
    certificate_name=certificateName, policy=CertificatePolicy.get_default()
)
certificate = certificate_operation_poller.result()
```

Sertifikanın [az keykasa sertifikası göster](/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-show) komutuyla ayarlandığını doğrulayabilirsiniz:

```azurecli
az keyvault certificate show --vault-name <your-unique-keyvault-name> --name myCertificate
```

### <a name="retrieve-a-certificate"></a>Sertifika alma

Şimdi, önceden oluşturulan sertifikayı alabilirsiniz

```python
retrieved_certificate = client.get_certificate(certificateName)
 ```

Sertifikanız şu şekilde kaydedilir `retrieved_certificate` .

### <a name="delete-a-certificate"></a>Sertifikayı silme

Son olarak, anahtar kasaınızdan sertifikayı silemem

```python
client.delete_certificate(certificateName)
```

Sertifikanın [az keykasa sertifikası göster](/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-show) komutuyla ilgili olduğunu doğrulayabilirsiniz:

```azurecli
az keyvault certifcate show --vault-name <your-unique-keyvault-name> --name myCertificate
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
from azure.keyvault.certificates import CertificateClient, CertificatePolicy,CertificateContentType, WellKnownIssuerNames 
from azure.identity import DefaultAzureCredential

keyVaultName = os.environ["KEY_VAULT_NAME"]
KVUri = "https://" + keyVaultName + ".vault.azure.net"

credential = DefaultAzureCredential()
client = CertificateClient(vault_url=KVUri, credential=credential)

certificateName = "myCertificate"

print("Creating a certificate in " + keyVaultName + " called '" + certificateName  + "` ...")

certificate_operation_poller = client.begin_create_certificate(
    certificate_name=certificateName, policy=CertificatePolicy.get_default()

certificate = certificate_operation_poller.result()

print(" done.")

print("Retrieving your certificate from " + keyVaultName + ".")

retrieved_certificate = client.get_certificate(certificateName)

print("Certificate with name '{0}' was found'.".format(retrieved_certificate.name))
print("Deleting your certificate from " + keyVaultName + " ...")

client.delete_certificate(certificateName)

print(" done.")
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir Anahtar Kasası oluşturdunuz, bir sertifikayı depo, ve bu sertifikayı alırsınız. Key Vault ve uygulamalarınızla tümleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere ilerleyin.

- [Azure Key Vault genel bakışını](../general/overview.md) okuyun
- [Azure Key Vault geliştirici kılavuzuna](../general/developers-guide.md) bakın
- [En iyi uygulamaları](../general/best-practices.md) gözden geçirin Azure Key Vault

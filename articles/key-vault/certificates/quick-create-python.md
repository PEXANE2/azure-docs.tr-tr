---
title: Quickstart - Python için Azure Key Vault istemci kitaplığı
description: Python istemci kitaplığını kullanarak Azure anahtar kasasından sertifikaları nasıl oluşturup, nasıl silen ve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 3/30/2020
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.openlocfilehash: 726cd76916510f38c7c14cb29e64449fb8fd539f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424709"
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

Konsol penceresinden Python için Azure Anahtar Kasası sertifikaları kitaplığını yükleyin.

```console
pip install azure-keyvault-certificates
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

MüşteriYi [az keyvault set-ilke](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuna geçirerek servis müdürünüze izin veren anahtar kasanız için bir erişim ilkesi oluşturun. Hizmet sorumlusuna sertifikalar için izinleri al, listele ve oluştur' verin.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --certificate-permissions delete get list create 
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

Python için Azure Key Vault istemci kitaplığı, anahtarları ve sertifikalar ve sırlar gibi ilgili varlıkları yönetmenize olanak tanır. Aşağıdaki kod örnekleri, bir istemci oluşturma, sertifika oluşturma, sertifika alma ve sertifika silme gibi bilgileri gösterir.

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeekleme

Kodunuzun üst bölümüne aşağıdaki yönergeleri ekleyin:

```python
import os
from azure.keyvault.certificates import CertificateClient, CertificatePolicy,CertificateContentType, WellKnownIssuerNames 
from azure.identity import DefaultAzureCredential
```

### <a name="authenticate-and-create-a-client"></a>Kimlik doğrulaması ve istemci oluşturma

Anahtar kasanıza doğrulanması ve anahtar kasa istemcisi oluşturma, yukarıdaki [Set çevresel değişkenler](#set-environmental-variables) adımındaki çevresel değişkenlere bağlıdır. Anahtar kasanızın adı URI anahtar kasasına genişletilir ve "https://<your-key-vault-name>.vault.azure.net" biçiminde genişletilir.

```python
credential = DefaultAzureCredential()

client = CertificateClient(vault_url=KVUri, credential=credential)
```

### <a name="save-a-certificate"></a>Sertifika yı kaydet

Başvurunuzun kimliği doğrulandığına göre, anahtar kasanıza kendi imzalı bir sertifika koyabilirsiniz 

```python
certificate_operation_poller = client.begin_create_certificate(
    certificate_name=certificateName, policy=CertificatePolicy.get_default()
)
certificate = certificate_operation_poller.result()
```

Sertifikanın [az keyvault sertifika göster](/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-show) komutuyla ayarlandığını doğrulayabilirsiniz:

```azurecli
az keyvault certificate show --vault-name <your-unique-keyvault-name> --name myCertificate
```

### <a name="retrieve-a-certificate"></a>Sertifika alma

Artık daha önce oluşturulmuş sertifikayı alabilirsiniz

```python
retrieved_certificate = client.get_certificate(certificateName)
 ```

Sertifikanız artık ' `retrieved_certificate`olarak kaydedilir.

### <a name="delete-a-certificate"></a>Sertifikayı silme

Son olarak, anahtarı kasanızdan sertifikayı silelim

```python
client.delete_certificate(certificateName)
```

Sertifikanın [az keyvault sertifika göster](/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-show) komutu yla gittiğini doğrulayabilirsiniz:

```azurecli
az keyvault certifcate show --vault-name <your-unique-keyvault-name> --name myCertificate
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

Bu hızlı başlangıçta bir anahtar kasası oluşturdunuz, bir sertifika depolamadınız ve bu sertifikayı aldınız. Key Vault ve uygulamalarınızla nasıl entegre edilebildiğini öğrenmek için aşağıdaki makalelere devam edin.

- Azure [Anahtar Kasasına Genel Bakış](../general/overview.md)
- Azure [Key Vault geliştiricisi kılavuzuna](../general/developers-guide.md) bakın
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](../general/best-practices.md)

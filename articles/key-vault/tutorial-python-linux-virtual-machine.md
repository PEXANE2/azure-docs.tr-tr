---
title: Öğretici - Azure Key Vault'ta sırları saklamak için Linux sanal makinesi ve Python uygulaması kullanın | Microsoft Dokümanlar
description: Bu eğitimde, Azure Key Vault'tan bir sırrı okumak için Python uygulamasını nasıl yapılandırabileceğinizi öğrenirsiniz.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: secrets
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: 59b8abf59212d9cfb0719b6b76e9542249ee4c41
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79472699"
---
# <a name="tutorial-use-a-linux-vm-and-a-python-app-to-store-secrets-in-azure-key-vault"></a>Öğretici: Azure Key Vault'ta sırları depolamak için Linux VM ve Python uygulamasını kullanın

Azure Key Vault, uygulamalarınıza, hizmetlerinize ve BT kaynaklarınıza erişmek için gereken API anahtarları ve veritabanı bağlantı dizeleri gibi sırları korumanıza yardımcı olur.

Bu eğitimde, Azure kaynakları için yönetilen kimlikleri kullanarak Azure Key Vault'tan gelen bilgileri okumak için bir Azure web uygulaması ayarlayın. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma
> * Anahtar kasanızda bir sır saklayın
> * Linux sanal makinesi oluşturma
> * Sanal makine için yönetilen bir [kimliği](../active-directory/managed-identities-azure-resources/overview.md) etkinleştirme
> * Anahtar kasasından verileri okumak için konsol uygulaması için gerekli izinleri verin
> * Anahtar kasanızdan bir sır alın

Daha ileri gitmeden önce [Key Vault hakkındaki temel kavramları](basic-concepts.md)anladığınızdan emin olun.

## <a name="prerequisites"></a>Ön koşullar

* [Git.](https://git-scm.com/downloads)
* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.
* [Azure CLI sürüm 2.0.4 veya sonraki](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) sürüm veya Azure Bulut BulutU.

[!INCLUDE [Azure Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="understand-managed-service-identity"></a>Yönetilen Hizmet Kimliğini Anlama

Azure Key Vault kimlik bilgilerini güvenli bir şekilde depolayabilir, böylece kodunuzda yer almaz. Bunları almak için Azure Key Vault'a kimlik doğrulamanız gerekir. Ancak, Key Vault'a kimlik doğrulamak için bir kimlik bilgisine ihtiyacınız vardır. Klasik bir bot kapanı problemi. Azure ve Azure Etkin Dizin (Azure AD) aracılığıyla Yönetilen Hizmet Kimliği (MSI), bir şeyleri başlatmayı kolaylaştıran bir önyükleme kimliği sağlar.

Sanal Makineler, Uygulama Hizmeti veya İşlevler gibi bir Azure hizmeti için MSI'ı etkinleştirdiğinizde, Azure, Azure AD'deki hizmet örneği için bir hizmet ilkesi oluşturur. Hizmet sorumlusunun kimlik bilgilerini hizmet örneğine enjekte eder.

![MSI](media/MSI.png)

Ardından, kodunuz, bir erişim jetonunu almak için Azure kaynağında bulunan yerel bir meta veri hizmetini çağırır. Kodunuz, yerel MSI bitiş noktasından bir Azure Anahtar Kasası hizmetine kimlik doğrulamasına kadar aldığı erişim belirtecikullanır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açabilmek için şunları girin:

```azurecli-interactive
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki kodla Batı `az group create` ABD konumundaki komutu kullanarak bir kaynak grubu oluşturun. Seçtiğiniz `YourResourceGroupName` bir ad ile değiştirin.

```azurecli-interactive
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Bu kaynak grubunu öğretici boyunca kullanırsınız.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Ardından, önceki adımda oluşturduğunuz kaynak grubunda bir anahtar kasası oluşturursunuz. Şu bilgileri belirtin:

* Anahtar kasa adı: Adı 3-24 karakterden oluşan bir dize olmalı ve yalnızca 0-9, a-z, A-Z ve tire (-) içermelidir.
* Kaynak grubu adı.
* Konum: **West US**.

```azurecli-interactive
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Bunun nasıl çalıştığını göstermemize yardımcı olması için bir gizli dizi ekliyoruz. Bir SQL bağlantı dizesini veya hem güvenli hem de uygulamanız için kullanılabilir tutulması gereken diğer bilgileri depolamak isteyebilirsiniz.

Anahtar kasasında *AppSecret* adlı bir gizli dizi oluşturmak için aşağıdaki komutları yazın. Bu gizli dizi **MySecret** değerini depolar.

```azurecli-interactive
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-linux-virtual-machine"></a>Linux sanal makinesi oluşturma

Komutu `az vm create` kullanarak bir VM oluşturun.

Aşağıdaki örnek, **myVM** adlı bir VM oluşturur ve **azureuser** adlı bir kullanıcı hesabı ekler. `--generate-ssh-keys` Parametre otomatik olarak bir SSH tuşu oluşturur ve varsayılan anahtar konumuna koyar (**~/.ssh).** Bunun yerine belirli bir anahtar kümesi `--ssh-key-value` oluşturmak için seçeneği kullanın.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

VM’yi ve destekleyici kaynakları oluşturmak birkaç dakika sürer. Aşağıdaki örnek çıktı, VM oluşturmanın başarılı olduğunu gösterir:

```output
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-00-00-00-00-00",
  "powerState": "VM running",
  "privateIpAddress": "XX.XX.XX.XX",
  "publicIpAddress": "XX.XX.XXX.XXX",
  "resourceGroup": "myResourceGroup"
}
```

VM'nizden çıktınızda kendi `publicIpAddress` notunuzu belirtin. Daha sonraki adımlarda VM'ye erişmek için bu adresi kullanırsınız.

## <a name="assign-an-identity-to-the-vm"></a>VM'ye kimlik atama

Aşağıdaki komutu çalıştırarak sanal makineye sistem tarafından atanmış bir kimlik oluşturun:

```azurecli-interactive
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Komutun çıktısı aşağıdaki gibidir.

```output
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

Bir not ala. `systemAssignedIdentity` Bir sonraki adımda kullan.

## <a name="give-the-vm-identity-permission-to-key-vault"></a>Key Vault için VM kimlik izni verin

Artık Key Vault'u oluşturduğunuz kimlik için izin verebilirsiniz. Şu komutu çalıştırın:

```azurecli-interactive
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-in-to-the-vm"></a>VM'de oturum açın

Bir terminal kullanarak sanal makineye giriş yapın.

```terminal
ssh azureuser@<PublicIpAddress>
```

## <a name="install-python-library-on-the-vm"></a>VM'de Python kitaplığını yükleme

HTTP GET aramaları yapmak için Python kitaplığı [isteklerini](https://pypi.org/project/requests/2.7.0/) indirin ve yükleyin.

## <a name="create-edit-and-run-the-sample-python-app"></a>Örnek Python uygulamasını oluşturun, edinve çalıştırın

**Sample.py**adlı bir Python dosyası oluşturun.

Sample.py açın ve aşağıdaki kodu içerecek şekilde edin:

```python
# importing the requests library
import requests

# Step 1: Fetch an access token from an MSI-enabled Azure resource      
# Note that the resource here is https://vault.azure.net for the public cloud, and api-version is 2018-02-01
MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"})

# Extracting data in JSON format 
# This request gets an access token from Azure Active Directory by using the local MSI endpoint
data = r.json()

# Step 2: Pass the access token received from the previous HTTP GET call to the key vault
KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})

print(kvSecret.json()["value"])
```

Önceki kod iki adımlı bir işlem gerçekleştirir:

   1. VM'deki yerel MSI bitiş noktasından bir belirteç getirir. Bitiş noktası, Azure Etkin Dizin'den bir belirteç getirir.
   1. Jetonu anahtar kasasına aktarır ve sırrınızı getirir.

Şu komutu çalıştırın. Gizli değeri görmelisin.

```console
python Sample.py
```

Bu eğitimde, Linux sanal makinesinde çalışan bir Python uygulamasıyla Azure Key Vault'u kullanmayı öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu, sanal makineyi ve ilgili tüm kaynakları artık ihtiyacınız olmadığında silin. Bunu yapmak için VM kaynak grubunu ve **Sil** öğesini seçin.

Komutu kullanarak anahtar `az keyvault delete` kasasını silin:

```azurecli-interactive
az keyvault delete --name
                   [--resource-group]
                   [--subscription]
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Anahtar Kasası REST API](https://docs.microsoft.com/rest/api/keyvault/)

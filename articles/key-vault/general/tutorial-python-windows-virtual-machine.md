---
title: Öğretici - Python'da Windows sanal makinesiyle Azure Key Vault'u kullanın | Microsoft Dokümanlar
description: Bu öğreticide, anahtar kasanızdan bir sırrı okumak için ASP.NET bir çekirdek uygulaması yapılandırırsınız.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: 85dc751909f61eff40ad57868bafa7ea2019ed89
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422875"
---
# <a name="tutorial-use-azure-key-vault-with-a-windows-virtual-machine-in-python"></a>Öğretici: Python'da Windows sanal makinesiyle Azure Key Vault'u kullanın

Azure Key Vault, API anahtarları, uygulamalarınıza, hizmetlerinize ve BT kaynaklarınıza erişmek için gereken veritabanı bağlantı dizeleri gibi sırları korumanıza yardımcı olur.

Bu eğitimde, Azure Key Vault'tan bilgileri okumak için bir konsol uygulaması nın nasıl alındığını öğrenirsiniz. Bunu yapmak için Azure kaynakları için yönetilen kimlikleri kullanırsınız. 

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma.
> * Anahtar kasasına bir sır ekle.
> * Anahtar kasasından bir gizli dizi alma.
> * Azure sanal makinesi oluşturun.
> * Yönetilen bir kimliği etkinleştirin.
> * VM kimliğine izin atayın.

Başlamadan önce [Key Vault temel kavramlarını](basic-concepts.md)okuyun. 

Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Windows, Mac ve Linux için:
  * [Git](https://git-scm.com/downloads)
  * Bu öğretici, Azure CLI'yi yerel olarak çalıştırmanızı gerektirir. Azure CLI sürümü 2.0.4 veya daha sonra yüklü olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 2.0’ı yükleme](/cli/azure/install-azure-cli).

## <a name="about-managed-service-identity"></a>Yönetilen Hizmet Kimliği hakkında

Azure Key Vault kimlik bilgilerini güvenli bir şekilde saklar, böylece kodunuzda görüntülenmez. Ancak, anahtarlarınızı almak için Azure Key Vault'a kimlik doğrulamanız gerekir. Key Vault'a kimlik doğrulamak için bir kimlik bilgisine ihtiyacınız var. Klasik bir çizme ikilemi. Yönetilen Hizmet Kimliği (MSI), işlemi basitleştiren bir _bootstrap kimliği_ sağlayarak bu sorunu çözer.

Azure Sanal Makineleri, Azure Uygulama Hizmeti veya Azure Fonksiyonları gibi bir Azure hizmeti için MSI'ı etkinleştirdiğinizde, Azure bir [hizmet ilkesi](basic-concepts.md)oluşturur. MSI bunu Azure Etkin Dizin (Azure AD)'deki hizmet örneği için yapar ve bu örneğin içine hizmet temel kimlik bilgilerini enjekte eder. 

![MSI](../media/MSI.png)

Ardından, bir erişim jetonu almak için kodunuz Azure kaynağında bulunan yerel bir meta veri hizmetini çağırır. Bir Azure Anahtar Kasası hizmetine kimlik doğrulamak için kodunuz, yerel MSI bitiş noktasından aldığı erişim belirteci kullanır. 

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açmak için, şunları girin:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[az group create](/cli/azure/group#az-group-create) komutunu kullanarak bir kaynak grubu oluşturun. 

Kaynak grubu adı seçin ve yer tutucuyu doldurun. Aşağıdaki örnekte Batı ABD konumunda bir kaynak grubu oluşturulur:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Bu öğretici boyunca yeni oluşturduğunuz kaynak grubunu kullanırsınız.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Önceki adımda oluşturduğunuz kaynak grubunda önemli bir kasa oluşturmak için aşağıdaki bilgileri sağlayın:

* Anahtar kasa adı: yalnızca sayılar (0-9), harfler (a-z, A-Z) ve tire (-) içerebilen 3 ila 24 karakterden oluşan bir dize
* Kaynak grubu adı
* Yer: **Batı ABD**

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
Bu noktada, Azure hesabınız bu yeni anahtar kasasında işlemleri gerçekleştirmeyetkisine sahip olan tek hesaptır.

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Bunun nasıl çalıştığını göstermemize yardımcı olması için bir gizli dizi ekliyoruz. Gizli bir SQL bağlantı dizesi veya hem güvenli hem de uygulamanız için kullanılabilir tutmak için gereken diğer bilgiler olabilir.

**AppSecret**adlı anahtar tonozbir sır oluşturmak için, aşağıdaki komutu girin:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Bu gizli değeri **MySecret**depolar .

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Aşağıdaki yöntemlerden birini kullanarak sanal bir makine oluşturabilirsiniz:

* [Azure CLI](../../virtual-machines/windows/quick-create-cli.md)
* [PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
* [Azure portal](../../virtual-machines/windows/quick-create-portal.md)

## <a name="assign-an-identity-to-the-vm"></a>VM'ye kimlik atama
Bu adımda, Azure CLI'de aşağıdaki komutu çalıştırarak sanal makine için sistem tarafından atanmış bir kimlik oluşturursunuz:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Aşağıdaki kodda görüntülenen sistem tarafından atanan kimliğe dikkat edin. Önceki komutun çıktısı olacaktır: 

```output
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="assign-permissions-to-the-vm-identity"></a>VM kimliğine izin atama
Artık aşağıdaki komutu çalıştırarak anahtar kasanıza önceden oluşturulmuş kimlik izinlerini atayabilirsiniz:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-on-to-the-virtual-machine"></a>Sanal makinede oturum açma

Sanal makinede oturum açmak için Connect'teki yönergeleri izleyin [ve Windows çalıştıran bir Azure sanal makinesinde oturum açın.](../../virtual-machines/windows/connect-logon.md)

## <a name="create-and-run-a-sample-python-app"></a>Örnek bir Python uygulaması oluşturma ve çalıştırma

Sonraki bölümde *Sample.py*adlı bir örnek dosya. HTTP GET aramaları yapmak için bir [istek](https://2.python-requests.org/en/master/) kitaplığı kullanır.

## <a name="edit-samplepy"></a>Sample.py'yı edin

*Sample.py*oluşturduktan sonra dosyayı açın ve ardından bu bölümdeki kodu kopyalayın. 

Kod iki adımlı bir işlem sunar:
1. VM'deki yerel MSI bitiş noktasından bir belirteç getirin.  
  Bunu yapmak, Azure AD'den bir belirteç de getirir.
1. Jetonu anahtar kasana ilet ve sırrını getir. 

```python
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from a Managed Identity enabled azure resource.      
    # Note that the resource here is https://vault.azure.net for public cloud and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # extracting data in json format 
    # This request gets an access_token from Azure AD by using the local MSI endpoint.
    data = r.json() 
    
    # Step 2: Pass the access_token received from previous HTTP GET call to your key vault.
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

Aşağıdaki kodu çalıştırarak gizli değeri görüntüleyebilirsiniz: 

```console
python Sample.py
```

Önceki kod, Windows sanal makinesinde Azure Key Vault ile işlemleri nasıl yapacağınızı gösterir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, sanal makineyi ve anahtar kasanızı silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Anahtar Kasası REST API](https://docs.microsoft.com/rest/api/keyvault/)

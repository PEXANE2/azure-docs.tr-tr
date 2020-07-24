---
title: Öğretici-Python 'da bir sanal makine ile Azure Key Vault kullanma | Microsoft Docs
description: Bu öğreticide, anahtar kasaınızdan gizli dizi okumak için bir ASP.NET Core uygulaması yapılandırırsınız.
services: key-vault
author: msmbaldwin
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 07/20/2020
ms.author: mbaldwin
ms.custom: mvc, tracking-python
ms.openlocfilehash: 453307b304c4cb1899b1de31117c944ac66fcddb
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87101881"
---
# <a name="tutorial-use-azure-key-vault-with-a-virtual-machine-in-python"></a>Öğretici: Python 'da bir sanal makine ile Azure Key Vault kullanma

Azure Key Vault, API anahtarları, uygulamalarınıza, hizmetlerinize ve BT kaynaklarınıza erişmeniz gereken veritabanı bağlantı dizeleri gibi gizli dizileri korumanıza yardımcı olur.

Bu öğreticide, Azure Key Vault bilgileri okumak için bir konsol uygulamasının nasıl alınacağını öğreneceksiniz. Bunu yapmak için Azure kaynakları için Yönetilen kimlikler kullanırsınız. 

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * Anahtar kasası oluşturma.
> * Anahtar kasasına gizli dizi ekleyin.
> * Anahtar kasasından bir gizli dizi alma.
> * Bir Azure sanal makinesi oluşturun.
> * Yönetilen bir kimliği etkinleştirin.
> * VM kimliğine izin atayın.

Başlamadan önce [temel kavramları Key Vault](basic-concepts.md)okuyun. 

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Windows, Mac ve Linux için:
  * [Git](https://git-scm.com/downloads)
  * Bu öğretici, Azure CLı 'yı yerel olarak çalıştırmanızı gerektirir. Azure CLı sürüm 2.0.4 veya daha yeni bir sürümün yüklü olması gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 2.0’ı yükleme](/cli/azure/install-azure-cli).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açmak için, şunları girin:

```azurecli
az login
```

### <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve Anahtar Kasası oluşturma

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-rg-kv-creation.md)]

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Bunun nasıl çalıştığını göstermemize yardımcı olması için bir gizli dizi ekliyoruz. Gizli dizi bir SQL bağlantı dizesi veya hem güvenli hem de uygulamanız için kullanılabilir durumda tutmanız gereken diğer bilgiler olabilir.

**Appsecret**adlı anahtar kasasında bir gizli dizi oluşturmak için aşağıdaki komutu girin:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Bu gizli anahtar, **MySecret**değerini depolar.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Aşağıdaki yöntemlerden birini kullanarak bir sanal makine oluşturabilirsiniz:

* [Azure CLI](../../virtual-machines/windows/quick-create-cli.md)
* [PowerShell](../../virtual-machines/windows/quick-create-powershell.md)
* [Azure portal](../../virtual-machines/windows/quick-create-portal.md)

## <a name="assign-an-identity-to-the-vm"></a>VM 'ye bir kimlik atama
Bu adımda, Azure CLı 'de aşağıdaki komutu çalıştırarak sanal makine için sistem tarafından atanan bir kimlik oluşturursunuz:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Aşağıdaki kodda görüntülenen sistem tarafından atanan kimliği unutmayın. Yukarıdaki komutun çıkışı şöyle olacaktır: 

```output
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="assign-permissions-to-the-vm-identity"></a>VM kimliğine izin atama
Artık aşağıdaki komutu çalıştırarak, önceden oluşturulmuş kimlik izinlerini anahtar kasanıza atayabilirsiniz:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-on-to-the-virtual-machine"></a>Sanal makinede oturum açma

Sanal makinede oturum açmak için Bağlan bölümündeki yönergeleri izleyin [ve Windows çalıştıran bir Azure sanal makinesinde oturum açın](../../virtual-machines/windows/connect-logon.md).

## <a name="create-and-run-a-sample-python-app"></a>Örnek bir Python uygulaması oluşturma ve çalıştırma

Sonraki bölümde, *Sample.py*adlı örnek bir dosyadır. HTTP GET çağrısı yapmak için bir [istekler](https://2.python-requests.org/en/master/) kitaplığı kullanır.

## <a name="edit-samplepy"></a>Sample.py Düzenle

*Sample.py*oluşturduktan sonra dosyayı açın ve bu bölümdeki kodu kopyalayın. 

Kod, iki adımlı bir işlem sunar:
1. VM 'deki yerel MSI uç noktasından bir belirteç getirir.  
  Bunun yapılması Azure AD 'den de bir belirteç getirir.
1. Belirteci anahtar kasanıza geçirin ve sonra gizli dizinizi getirin. 

```python
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from a Managed Identity enabled azure resource.
    # Resources with an MSI configured recieve an AAD access token by using the Azure Instance Metadata Service (IMDS)
    # IMDS provides an endpoint accessible to all IaaS VMs using a non-routable well-known IP Address
    # To learn more about IMDS and MSI Authentication see the following link: https://docs.microsoft.com/azure/virtual-machines/windows/instance-metadata-service
    # Note that the resource here is https://vault.azure.net for public cloud and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # extracting data in json format 
    # This request gets an access_token from Azure AD by using the local MSI endpoint.
    data = r.json() 
    
    # Step 2: Pass the access_token received from previous HTTP GET call to your key vault.
    KeyVaultURL = "https://{YOUR KEY VAULT NAME}.vault.azure.net/secrets/{YOUR SECRET NAME}?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

Aşağıdaki kodu çalıştırarak gizli değeri görüntüleyebilirsiniz: 

```console
python Sample.py
```

Yukarıdaki kod, Windows sanal makinesinde Azure Key Vault işlemleri nasıl yapılacağını gösterir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında, sanal makineyi ve anahtar kasanızı silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Key Vault REST API](https://docs.microsoft.com/rest/api/keyvault/)

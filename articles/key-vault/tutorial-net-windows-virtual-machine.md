---
title: Öğretici - .NET'te Windows sanal makinesiyle Azure Key Vault'u kullanın | Microsoft Dokümanlar
description: Bu öğreticide, anahtar kasanızdan bir sırrı okumak için ASP.NET bir çekirdek uygulaması yapılandırırsınız.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 01/02/2019
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: 1bf280ac8b9b01189c306f33e8fcc232a5cec8b6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79472682"
---
# <a name="tutorial-use-azure-key-vault-with-a-windows-virtual-machine-in-net"></a>Öğretici: .NET'te Windows sanal makinesiyle Azure Key Vault'u kullanın

Azure Key Vault, API anahtarları, uygulamalarınıza, hizmetlerinize ve BT kaynaklarınıza erişmek için gereken veritabanı bağlantı dizeleri gibi sırları korumanıza yardımcı olur.

Bu eğitimde, Azure Key Vault'tan bilgileri okumak için bir konsol uygulaması nın nasıl alındığını öğrenirsiniz. Bunu yapmak için Azure kaynakları için yönetilen kimlikleri kullanırsınız. 

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * Bir kaynak grubu oluşturun.
> * Bir anahtar kasası oluşturma.
> * Anahtar kasasına bir sır ekle.
> * Anahtar kasasından bir gizli dizi alma.
> * Azure sanal makinesi oluşturun.
> * Sanal Makine için yönetilen bir [kimliği](../active-directory/managed-identities-azure-resources/overview.md) etkinleştirin.
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

![MSI](media/MSI.png)

Ardından, bir erişim jetonu almak için kodunuz Azure kaynağında bulunan yerel bir meta veri hizmetini çağırır. Bir Azure Anahtar Kasası hizmetine kimlik doğrulamak için kodunuz, yerel MSI bitiş noktasından aldığı erişim belirteci kullanır. 

## <a name="create-resources-and-assign-permissions"></a>Kaynak oluşturma ve izin atama

Kodlamaya başlamadan önce bazı kaynaklar oluşturmanız, anahtar kasanıza bir sır koymanız ve izinler atamanız gerekir.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açabilmek için şunları girin:

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. [az group create](/cli/azure/group#az-group-create) komutunu kullanarak bir kaynak grubu oluşturun. 

Bu örnek, Batı ABD konumunda bir kaynak grubu oluşturur:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Yeni oluşturulan kaynak grubunuz bu öğretici boyunca kullanılacaktır.

### <a name="create-a-key-vault-and-populate-it-with-a-secret"></a>Anahtar bir kasa oluşturun ve bir sır ile doldurmak

Az [keyvault oluşturma](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) komutunu sağlayarak kaynak grubunuzda bir anahtar kasası oluşturun:

* Anahtar kasa adı: yalnızca sayılar (0-9), harfler (a-z, A-Z) ve tire (-) içerebilen 3 ila 24 karakterden oluşan bir dize
* Kaynak grubu adı
* Yer: **Batı ABD**

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
Bu noktada, Azure hesabınız bu yeni anahtar kasasında işlemleri gerçekleştirmeyetkisine sahip olan tek hesaptır.

Şimdi [az keyvault gizli set](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) komutunu kullanarak anahtar kasanıza bir sır ekleyin


**AppSecret**adlı anahtar tonozbir sır oluşturmak için, aşağıdaki komutu girin:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Bu gizli değeri **MySecret**depolar .

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma
Aşağıdaki yöntemlerden birini kullanarak sanal bir makine oluşturun:

* [Azure CLI](../virtual-machines/windows/quick-create-cli.md)
* [Powershell](../virtual-machines/windows/quick-create-powershell.md)
* [Azure portalı](../virtual-machines/windows/quick-create-portal.md)

### <a name="assign-an-identity-to-the-vm"></a>VM'ye kimlik atama
[Az vm kimlik atama](/cli/azure/vm/identity?view=azure-cli-latest#az-vm-identity-assign) komutu ile sanal makine için sistem tarafından atanmış bir kimlik oluşturun:

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

### <a name="assign-permissions-to-the-vm-identity"></a>VM kimliğine izin atama
[Az keyvault set-ilke](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutu ile anahtar kasanıza daha önce oluşturulmuş kimlik izinlerini atayın:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

### <a name="sign-in-to-the-virtual-machine"></a>Sanal makinede oturum açın

Sanal makinede oturum açabilmek için Connect'teki yönergeleri izleyin [ve Windows çalıştıran bir Azure sanal makinesinde oturum açın.](../virtual-machines/windows/connect-logon.md)

## <a name="set-up-the-console-app"></a>Konsol uygulamasını ayarlama

Bir konsol uygulaması oluşturun ve komutu kullanarak gerekli paketleri yükleyin. `dotnet`

### <a name="install-net-core"></a>.NET Core'u yükleyin

.NET Core'u yüklemek için [.NET indirme sayfasına](https://www.microsoft.com/net/download) gidin.

### <a name="create-and-run-a-sample-net-app"></a>Örnek bir .NET uygulaması oluşturma ve çalıştırma

Bir komut istemi açın.

Aşağıdaki komutları çalıştırarak konsola "Hello World" yazdırabilirsiniz:

```console
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

### <a name="install-the-packages"></a>Paketleri yükleyin

Konsol penceresinden, bu hızlı başlatma için gereken .NET paketlerini yükleyin:

```console
dotnet add package System.IO;
dotnet add package System.Net;
dotnet add package System.Text;
dotnet add package Newtonsoft.Json;
dotnet add package Newtonsoft.Json.Linq;
```

## <a name="edit-the-console-app"></a>Konsol uygulamasını edin

*Program.cs* dosyasını açın ve şu paketleri ekleyin:

```csharp
using System;
using System.IO;
using System.Net;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
```

Aşağıdaki üç adımlı işlemde kodu içerecek şekilde sınıf dosyasını edin:

1. VM'deki yerel MSI bitiş noktasından bir belirteç getirin. Bunu yapmak, Azure AD'den bir belirteç de getirir.
2. Jetonu anahtar kasana ilet ve sırrını getir. 
3. İsteğe kasa adını ve gizli adını ekleyin.

```csharp
 class Program
    {
        static void Main(string[] args)
        {
            // Step 1: Get a token from the local (URI) Managed Service Identity endpoint, which in turn fetches it from Azure AD
            var token = GetToken();

            // Step 2: Fetch the secret value from your key vault
            System.Console.WriteLine(FetchSecretValueFromKeyVault(token));
        }

        static string GetToken()
        {
            WebRequest request = WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net");
            request.Headers.Add("Metadata", "true");
            WebResponse response = request.GetResponse();
            return ParseWebResponse(response, "access_token");
        }
        
        static string FetchSecretValueFromKeyVault(string token)
        {
            //Step 3: Add the vault name and secret name to the request.
            WebRequest kvRequest = WebRequest.Create("https://<YourVaultName>.vault.azure.net/secrets/<YourSecretName>?api-version=2016-10-01");
            kvRequest.Headers.Add("Authorization", "Bearer "+  token);
            WebResponse kvResponse = kvRequest.GetResponse();
            return ParseWebResponse(kvResponse, "value");
        }

        private static string ParseWebResponse(WebResponse response, string tokenName)
        {
            string token = String.Empty;
            using (Stream stream = response.GetResponseStream())
            {
                StreamReader reader = new StreamReader(stream, Encoding.UTF8);
                String responseString = reader.ReadToEnd();

                JObject joResponse = JObject.Parse(responseString);    
                JValue ojObject = (JValue)joResponse[tokenName];             
                token = ojObject.Value.ToString();
            }
            return token;
        }
    }
```

Önceki kod, Windows sanal makinesinde Azure Key Vault ile işlemleri nasıl yapacağınızı gösterir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, sanal makineyi ve anahtar kasanızı silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Anahtar Kasası REST API](https://docs.microsoft.com/rest/api/keyvault/)

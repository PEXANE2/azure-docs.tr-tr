---
title: Öğretici - Azure Key Vault'ta sırları saklamak için Linux sanal makinesi ve ASP.NET konsol uygulaması kullanın | Microsoft Dokümanlar
description: Bu eğitimde, Azure Anahtar kasasından bir sırrı okumak için ASP.NET Core uygulamasını nasıl yapılandırabileceğinizi öğreneceksiniz.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.subservice: secrets
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: mbaldwin
ms.custom: mvc
ms.openlocfilehash: 50810dd1fbf2d97989df47b537ef5c574be059d2
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423155"
---
# <a name="tutorial-use-a-linux-vm-and-a-net-app-to-store-secrets-in-azure-key-vault"></a>Öğretici: Azure Key Vault'ta sırları depolamak için Linux VM ve .NET uygulamasını kullanın

Azure Key Vault, uygulamalarınıza, hizmetlerinize ve BT kaynaklarınıza erişmek için gereken API Anahtarları ve veritabanı bağlantı dizeleri gibi sırları korumanıza yardımcı olur.

Bu eğitimde, Azure kaynakları için yönetilen kimlikleri kullanarak Azure Key Vault'tan gelen bilgileri okumak için bir .NET konsol uygulaması ayarlayın. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma
> * Key Vault'ta bir sır saklayın
> * Azure Linux sanal makinesi oluşturma
> * Sanal makine için yönetilen bir [kimliği](../../active-directory/managed-identities-azure-resources/overview.md) etkinleştirme
> * Anahtar Vault'tan verileri okumak için konsol uygulaması için gerekli izinleri verin
> * Key Vault'tan bir sır alın

Daha ileri gitmeden önce, [anahtar tonoz temel kavramları](../general/basic-concepts.md)hakkında okuyun.

## <a name="prerequisites"></a>Ön koşullar

* [Git.](https://git-scm.com/downloads)
* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.
* [Azure CLI 2.0 veya sonraki veya](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Azure Bulut Bulut Uyrc.

[!INCLUDE [Azure Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="understand-managed-service-identity"></a>Yönetilen Hizmet Kimliğini Anlama

Azure Key Vault kimlik bilgilerini kodunuzda olmaması için güvenli bir şekilde depolayabilir, ancak bunları almak için Azure Key Vault'a kimlik doğrulamanız gerekir. Ancak, Key Vault'a kimlik doğrulamak için bir kimlik bilgisine ihtiyacınız vardır. Klasik bir bot kapanı problemi. Azure ve Azure Etkin Dizin (Azure AD) ile Yönetilen Hizmet Kimliği (MSI), bir şeyleri başlatmayı çok daha kolay hale getiren bir önyükleme kimliği sağlayabilir.

Sanal Makineler, Uygulama Hizmeti veya İşlevler gibi bir Azure hizmeti için MSI'ı etkinleştirdiğinizde, Azure Etkin Dizini'ndeki hizmet örneği için bir hizmet ilkesi oluşturur. Hizmet sorumlusunun kimlik bilgilerini hizmet örneğine enjekte eder.

![MSI](../media/MSI.png)

Ardından, kodunuz, bir erişim jetonunu almak için Azure kaynağında bulunan yerel bir meta veri hizmetini çağırır.
Kodunuz yerel MSI_ENDPOINT'ten aldığı erişim belirtecini kullanarak Azure Key Vault hizmetinde kimlik doğrulaması yapar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açabilmek için şunları girin:

```azurecli-interactive
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Komutu `az group create` kullanarak bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Batı ABD konumunda bir kaynak grubu oluşturun. Kaynak grubunuz için bir `YourResourceGroupName` ad seçin ve aşağıdaki örnekte değiştirin:

```azurecli-interactive
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Bu kaynak grubunu öğretici boyunca kullanırsınız.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Ardından, kaynak grubunuzda bir anahtar kasası oluşturun. Şu bilgileri belirtin:

* Anahtar kasa adı: yalnızca sayılar, harfler ve tireler (0-9, a-z, A-Z ve \- ) içerebilen 3 ila 24 karakterden oluşan bir dize.
* Kaynak grubu adı
* Yer: **Batı ABD**

```azurecli-interactive
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

Bu noktada, yalnızca Azure hesabınız bu yeni kasada herhangi bir işlem gerçekleştirme yetkisine bağlıdır.

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Şimdi, bir sır ekle. Gerçek bir senaryoda, bir SQL bağlantı dizesini veya güvenli bir şekilde saklamanız gereken ancak uygulamanız için kullanılabilir hale getirmeniz gereken diğer bilgileri depoluyor olabilirsiniz.

Bu öğretici için, anahtar kasasında bir sır oluşturmak için aşağıdaki komutları yazın. Gizli **AppSecret** denir ve değeri **MySecret**olduğunu.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-linux-virtual-machine"></a>Linux sanal makinesi oluşturma

Komutla `az vm create` bir VM oluşturun.

Aşağıdaki örnek, **myVM** adlı bir VM oluşturur ve **azureuser** adlı bir kullanıcı hesabı ekler. Bize `--generate-ssh-keys` otomatik olarak bir SSH anahtarı oluşturmak ve varsayılan anahtar konumu **(~/.ssh)** koymak için kullanılan parametre. Bunun yerine belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

VM’yi ve destekleyici kaynakları oluşturmak birkaç dakika sürer. Aşağıdaki örnek çıktı, VM oluşturma işleminin başarılı olduğunu gösterir.

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

VM'nizden `publicIpAddress` çıktınızı not edin. Daha sonraki adımlarda VM'ye erişmek için bu adresi kullanırsınız.

## <a name="assign-an-identity-to-the-vm"></a>VM'ye kimlik atama

Aşağıdaki komutu çalıştırarak sanal makineye sistem tarafından atanmış bir kimlik oluşturun:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Komutun çıkışı şöyle olmalıdır:

```output
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

Bir not ala. `systemAssignedIdentity` Bir sonraki adımda kullanırsın.

## <a name="give-the-vm-identity-permission-to-key-vault"></a>Key Vault için VM kimlik izni verin

Artık Key Vault'u oluşturduğunuz kimlik için izin verebilirsiniz. Şu komutu çalıştırın:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-in-to-the-vm"></a>VM'de oturum açın

Bir terminal kullanarak sanal makineye giriş yapın.

```terminal
ssh azureuser@<PublicIpAddress>
```

## <a name="install-net-core-on-linux"></a>Linux'a .NET Core yükle

Linux VM'nizde:

Microsoft ürün anahtarını aşağıdaki komutları çalıştırarak güvenilir olarak kaydedin:

   ```console
   curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
   sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
   ```

İşletim sistemine dayalı istenilen sürüm ana bilgisayar paket beslemesi ayarlama:

```console
   # Ubuntu 17.10
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-artful-prod artful main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
   
   # Ubuntu 17.04
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-zesty-prod zesty main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
   
   # Ubuntu 16.04 / Linux Mint 18
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
   
   # Ubuntu 14.04 / Linux Mint 17
   sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-trusty-prod trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
   sudo apt-get update
```

.NET'i yükleyin ve sürümü kontrol edin:

   ```console
   sudo apt-get install dotnet-sdk-2.1.4
   dotnet --version
   ```

## <a name="create-and-run-a-sample-net-app"></a>Örnek bir .NET uygulaması oluşturma ve çalıştırma

Aşağıdaki komutları çalıştırın. Konsolda "Hello World" yazılı olduğunu görmelisiniz.

```console
dotnet new console -o helloworldapp
cd helloworldapp
dotnet run
```

## <a name="edit-the-console-app-to-fetch-your-secret"></a>Sırrınızı almak için konsol uygulamasını edin

dosya Program.csyı açın ve şu paketleri ekleyin:

   ```csharp
   using System;
   using System.IO;
   using System.Net;
   using System.Text;
   using Newtonsoft.Json;
   using Newtonsoft.Json.Linq;
   ```

Uygulamanın anahtar kasasındaki gizliye erişmesini sağlamak için sınıf dosyasını değiştirmek için iki aşamalı bir işlemdir.

1. VM'deki yerel MSI bitiş noktasından azure etkin dizininden bir belirteç getirin.
1. Jetonu Key Vault'a ilet ve sırrını getir.

   Aşağıdaki kodu içerecek şekilde sınıf dosyasını edin:

   ```csharp
    class Program
       {
           static void Main(string[] args)
           {
               // Step 1: Get a token from local (URI) Managed Service Identity endpoint which in turn fetches it from Azure Active Directory
               var token = GetToken();

               // Step 2: Fetch the secret value from Key Vault
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
               WebRequest kvRequest = WebRequest.Create("https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01");
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

Artık Azure Linux sanal makinesinde çalışan bir .NET uygulamasında Azure Key Vault ile işlemleri nasıl gerçekleştireceklerini öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu, sanal makineyi ve ilgili tüm kaynakları artık ihtiyacınız olmadığında silin. Bunu yapmak için VM kaynak grubunu ve **Sil** öğesini seçin.

Komutu kullanarak anahtar `az keyvault delete` kasasını silin:

```azurecli-interactive
az keyvault delete --name
                   [--resource group]
                   [--subscription]
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Anahtar Kasası REST API](https://docs.microsoft.com/rest/api/keyvault/)

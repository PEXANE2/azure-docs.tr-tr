---
title: Quickstart - Node.js için Azure Key Vault istemci kitaplığı (v4)
description: Node.js istemci kitaplığını kullanarak Azure anahtar kasasından nasıl sır oluşturup, alınve silmeyi öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 10/20/2019
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.openlocfilehash: bda51b71ff1e6c89414bd1540bb38dcfea1d4a3c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79457228"
---
# <a name="quickstart-azure-key-vault-client-library-for-nodejs-v4"></a>Hızlı başlangıç: Node.js için Azure Key Vault istemci kitaplığı (v4)

Node.js için Azure Key Vault istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Node.js için Anahtar Kasa istemci kitaplığını kullanın:

- Anahtarlar ve parolalar üzerinde güvenliği ve denetimi artırın.
- Şifreleme anahtarlarını dakikalar içinde oluşturun ve aktarın.
- Bulut ölçeği ve genel artıklık la gecikme süresini azaltın.
- TLS/SSL sertifikaları için görevleri basitleştirin ve otomatikleştirin.
- FIPS 140-2 Düzey 2 onaylı HSM'leri kullanın.

[API başvuru belgeleri](/javascript/api/overview/azure/key-vault?view=azure-node-latest) | [Kütüphane kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/keyvault) | [Paketi (npm)](https://www.npmjs.com/package/@azure/keyvault-secrets)

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- İşletim sisteminiz için geçerli [Düğüm.js.](https://nodejs.org)
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell](/powershell/azure/overview)

Bu hızlı başlangıç, Bir Linux terminal penceresinde [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) çalıştırdığınızı varsayar.

## <a name="setting-up"></a>Ayarlama

### <a name="install-the-package"></a>Paketi yükleyin

Konsol penceresinden, Node.js için Azure Key Vault sırları kitaplığını yükleyin.

```console
npm install @azure/keyvault-secrets
```

Bu hızlı başlangıç için azure.identity paketini de yüklemeniz gerekir:

```console
npm install @azure/identity
```

### <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve anahtar kasası oluşturma

Bu hızlı başlatma, önceden oluşturulmuş bir Azure anahtar kasası kullanır. [Azure CLI quickstart,](quick-create-cli.md) [Azure PowerShell quickstart](quick-create-powershell.md)veya Azure portalı [quickstart'taki](quick-create-portal.md)adımları izleyerek önemli bir kasa oluşturabilirsiniz. Alternatif olarak, aşağıdaki Azure CLI komutlarını çalıştırmanız yeterlidir.

> [!Important]
> Her anahtar kasanın benzersiz bir adı olmalıdır. Aşağıdaki örneklerde benzersiz-keyvault-adınızı> anahtar kasanızın adı ile <değiştirin.

```azurecli
az group create --name "myResourceGroup" -l "EastUS"

az keyvault create --name <your-unique-keyvault-name> -g "myResourceGroup"
```

### <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

Bulut tabanlı bir uygulamanın kimliğini doğrulamanın en basit yolu yönetilen bir kimliktir; bkz. Ayrıntılar [için Azure Key Vault'a erişmek için Uygulama Hizmeti yönetilen kimliği kullanın.](managed-identity.md) Ancak basitlik uğruna, bu hızlı bir konsol uygulaması oluşturur. Azure ile bir masaüstü uygulamasının kimliğini doğrulamak için bir hizmet sorumlusu nun ve erişim denetim ilkesinin kullanılması nı gerektirir.

Azure CLI az reklam [sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir hizmet ilkesi oluşturun:

```azurecli
az ad sp create-for-rbac -n "http://mySP" --sdk-auth
```

Bu işlem bir dizi anahtar / değer çifti döndürecektir. 

```azurecli
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

MüşteriYi [az keyvault set-ilke](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuna geçirerek servis müdürünüze izin veren anahtar kasanız için bir erişim ilkesi oluşturun. Servis sorumlusuna hem anahtarlar hem de sırlar için izinleri al, listele ve ayarla.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
```

#### <a name="set-environmental-variables"></a>Çevresel değişkenleri ayarlama

Uygulamamızdaki Varsayılan AzureCredential yöntemi üç çevresel değişkene dayanır: `AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET`ve `AZURE_TENANT_ID`. Bu `export VARNAME=VALUE` değişkenleri, biçimi kullanarak [bir hizmet ana adımı oluştur'da](#create-a-service-principal) belirttiğiniz clientId, clientSecret ve tenantId değerlerine ayarlayın. (Bu yalnızca geçerli kabuğunuz için değişkenleri ve kabuktan oluşturulan işlemleri ayarlar; bu değişkenleri `/etc/environment ` ortamınıza kalıcı olarak eklemek, dosyanızı yeniden düzenlendi.) 

Ayrıca anahtar kasa adınızı bir ortam değişkeni `KEY_VAULT_NAME`olarak kaydetmeniz gerekir.

```console
export AZURE_CLIENT_ID=<your-clientID>

export AZURE_CLIENT_SECRET=<your-clientSecret>

export AZURE_TENANT_ID=<your-tenantId>

export KEY_VAULT_NAME=<your-key-vault-name>
````

## <a name="object-model"></a>Nesne modeli

Node.js için Azure Key Vault istemci kitaplığı, anahtarları ve sertifikalar ve sırlar gibi ilgili varlıkları yönetmenize olanak tanır. Aşağıdaki kod örnekleri, bir istemci oluşturmak, bir sır ayarlamak, bir sır almak ve bir sırrı silmek nasıl gösterecektir.

Tüm konsol uygulaması . https://github.com/Azure-Samples/key-vault-dotnet-core-quickstart/tree/master/key-vault-console-app

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeekleme

Kodunuzun üst bölümüne aşağıdaki yönergeleri ekleyin:

```javascript
const { DefaultAzureCredential } = require("@azure/identity");
const { SecretClient } = require("@azure/keyvault-secrets");
```

### <a name="authenticate-and-create-a-client"></a>Kimlik doğrulaması ve istemci oluşturma

Anahtar kasanıza doğrulanması ve anahtar kasa istemcisi oluşturma, yukarıdaki [Set çevresel değişkenler](#set-environmental-variables) adım ve [SecretClient oluşturucusu](/javascript/api/@azure/keyvault-secrets/secretclient?view=azure-node-latest#secretclient-string--tokencredential--pipelineoptions-)çevresel değişkenler bağlıdır. 

Anahtar tonozunuzun adı, formatında, `https://<your-key-vault-name>.vault.azure.net`anahtar kasası URI'ye genişletilir. 

```javascript
const keyVaultName = process.env["KEY_VAULT_NAME"];
const KVUri = "https://" + keyVaultName + ".vault.azure.net";

const credential = new DefaultAzureCredential();
const client = new SecretClient(KVUri, credential);
```

### <a name="save-a-secret"></a>Bir sırrı kaydet

Artık uygulamanızın kimliği doğrulanın, [istemci.setSecret yöntemini](/javascript/api/@azure/keyvault-secrets/secretclient?view=azure-node-latest#setsecret-string--string--setsecretoptions-) kullanarak keyvault'unuza bir sır koyabilirsiniz Bu gizlinin adı gerektirir -- bu örnekte "mySecret" kullanıyoruz.  

```javascript
await client.setSecret(secretName, secretValue);
```

Gizlinin [az keyvault gizli gösteri](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutuyla ayarlandığını doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

### <a name="retrieve-a-secret"></a>Bir sır alma

Şimdi [istemci.getSecret yöntemi](/javascript/api/@azure/keyvault-secrets/secretclient?view=azure-node-latest#getsecret-string--getsecretoptions-)ile önceden ayarlanmış değeri alabilirsiniz.

```javascript
const retrievedSecret = await client.getSecret(secretName);
 ```

Sırrın artık `retrievedSecret.value`.

### <a name="delete-a-secret"></a>Gizli anahtarı silme

Son olarak, [istemci.beginDeleteSecret yöntemi](/javascript/api/@azure/keyvault-secrets/secretclient?view=azure-node-latest#begindeletesecret-string--begindeletesecretoptions-)ile anahtar kasanızdan sırrı silelim.

```javascript
await client.beginDeleteSecret(secretName)
```

Gizli [az keyvault gizli gösteri](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutu ile gitti doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
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

```javascript
const { DefaultAzureCredential } = require("@azure/identity");
const { SecretClient } = require("@azure/keyvault-secrets");

const readline = require('readline');

function askQuestion(query) {
    const rl = readline.createInterface({
        input: process.stdin,
        output: process.stdout,
    });

    return new Promise(resolve => rl.question(query, ans => {
        rl.close();
        resolve(ans);
    }))
}

async function main() {

  const keyVaultName = process.env["KEY_VAULT_NAME"];
  const KVUri = "https://" + keyVaultName + ".vault.azure.net";

  const credential = new DefaultAzureCredential();
  const client = new SecretClient(KVUri, credential);

  const secretName = "mySecret";
  var secretValue = await askQuestion("Input the value of your secret > ");

  console.log("Creating a secret in " + keyVaultName + " called '" + secretName + "' with the value '" + secretValue + "` ...");
  await client.setSecret(secretName, secretValue);

  console.log("Done.");

  console.log("Forgetting your secret.");
  secretValue = "";
  console.log("Your secret is '" + secretValue + "'.");

  console.log("Retrieving your secret from " + keyVaultName + ".");

  const retrievedSecret = await client.getSecret(secretName);

  console.log("Your secret is '" + retrievedSecret.value + "'.");
  console.log("Deleting your secret from " + keyVaultName + " ...");

  await client.beginDeleteSecret(secretName);

  console.log("Done.");

}

main()

```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir anahtar kasası oluşturdunuz, bir sır sakladınız ve bu sırrı aldınız. Key Vault ve uygulamalarınızla nasıl entegre edilebildiğini öğrenmek için aşağıdaki makalelere devam edin.

- Azure [Anahtar Kasasına Genel Bakış](key-vault-overview.md)
- Azure [Key Vault geliştiricisi kılavuzuna](key-vault-developers-guide.md) bakın
- [Anahtarlar, sırlar ve sertifikalar](about-keys-secrets-and-certificates.md) hakkında bilgi edinin
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](key-vault-best-practices.md)
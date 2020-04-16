---
title: Quickstart - Java için Azure Key Vault istemci kitaplığı
description: Azure SDK istemci kitaplıkları için Hızlı Başlangıçlar yazmak için biçim ve içerik ölçütleri sağlar.
author: msmbaldwin
ms.author: mbaldwin
ms.date: 10/20/2019
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.openlocfilehash: 7e0d05ce97def0a255e6ac9909544b04fb13bb37
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81425031"
---
# <a name="quickstart-azure-key-vault-client-library-for-java"></a>Quickstart: Java için Azure Key Vault istemci kitaplığı

Java için Azure Key Vault istemci kitaplığı ile başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek için aşağıdaki adımları izleyin.

Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Java için Key Vault istemci kitaplığını kullanın:

- Anahtarlar ve parolalar üzerinde güvenliği ve denetimi artırın.
- Şifreleme anahtarlarını dakikalar içinde oluşturun ve aktarın.
- Bulut ölçeği ve genel artıklık la gecikme süresini azaltın.
- TLS/SSL sertifikaları için görevleri basitleştirin ve otomatikleştirin.
- FIPS 140-2 Düzey 2 onaylı HSM'leri kullanın.

[Kaynak kodu](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault) | [API referans belgeleri](https://azure.github.io/azure-sdk-for-java) | [Ürün belgeleri](index.yml) | [Örnekleri](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-secrets/src/samples/java/com/azure/security/keyvault/secrets)

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Java Geliştirme Kiti (JDK)](/java/azure/jdk/?view=azure-java-stable) sürüm 8 veya üzeri
- [Apaçi Maven](https://maven.apache.org)
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) veya [Azure PowerShell](/powershell/azure/overview)

Bu hızlı başlangıç, [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) ve [Apache Maven'i](https://maven.apache.org) bir Linux terminal penceresinde çalıştırdığınızı varsayar.

## <a name="setting-up"></a>Ayarlama

### <a name="create-new-java-console-app"></a>Yeni Java konsol uygulaması oluşturma

Konsol penceresinde, adı `mvn` `akv-java`olan yeni bir Java konsol uygulaması oluşturmak için komutu kullanın.

```console
mvn archetype:generate -DgroupId=com.keyvault.quickstart
                       -DartifactId=akv-java
                       -DarchetypeArtifactId=maven-archetype-quickstart
                       -DarchetypeVersion=1.4
                       -DinteractiveMode=false
```

Proje oluşturma çıktısı şuna benzer:

```console
[INFO] ----------------------------------------------------------------------------
[INFO] Using following parameters for creating project from Archetype: maven-archetype-quickstart:1.4
[INFO] ----------------------------------------------------------------------------
[INFO] Parameter: groupId, Value: com.keyvault.quickstart
[INFO] Parameter: artifactId, Value: akv-java
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Parameter: package, Value: com.keyvault.quickstart
[INFO] Parameter: packageInPathFormat, Value: com/keyvault/quickstart
[INFO] Parameter: package, Value: com.keyvault.quickstart
[INFO] Parameter: groupId, Value: com.keyvault.quickstart
[INFO] Parameter: artifactId, Value: akv-java
[INFO] Parameter: version, Value: 1.0-SNAPSHOT
[INFO] Project created from Archetype in dir: /home/user/quickstarts/akv-java
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  38.124 s
[INFO] Finished at: 2019-11-15T13:19:06-08:00
[INFO] ------------------------------------------------------------------------
```

Dizini yeni oluşturulan akv-java/ klasörüne değiştirin.

```console
cd akv-java
```

### <a name="install-the-package"></a>Paketi yükleyin

Metin düzenleyicinizdeki *pom.xml* dosyasını açın. Bağımlılıklar grubuna aşağıdaki bağımlılık öğelerini ekleyin.

```xml
    <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-security-keyvault-secrets</artifactId>
      <version>4.0.0</version>
    </dependency>

    <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-identity</artifactId>
      <version>1.0.0</version>
    </dependency>
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

Bulut tabanlı bir uygulamanın kimliğini doğrulamanın en basit yolu yönetilen bir kimliktir; bkz. Ayrıntılar [için Azure Key Vault'a erişmek için Uygulama Hizmeti yönetilen kimliği kullanın.](../general/managed-identity.md) Ancak basitlik adına, bu hızlı başlangıç bir hizmet ilkesi ve bir erişim denetim ilkesi kullanımını gerektiren bir masaüstü uygulaması oluşturur.

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

Sonraki iki adımda kullanacağımız için clientId, clientSecret ve tenantId'e dikkat edin.

#### <a name="give-the-service-principal-access-to-your-key-vault"></a>Servis müdürüne anahtar kasanıza erişim hakkı verin

MüşteriYi [az keyvault set-ilke](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) komutuna geçirerek servis müdürünüze izin veren anahtar kasanız için bir erişim ilkesi oluşturun. Servis sorumlusuna hem anahtarlar hem de sırlar için izinleri al, listele ve ayarla.

```azurecli
az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get list set --key-permissions create decrypt delete encrypt get list unwrapKey wrapKey
```

#### <a name="set-environmental-variables"></a>Çevresel değişkenleri ayarlama

Uygulamamızdaki Varsayılan AzureCredential yöntemi üç çevresel değişkene dayanır: `AZURE_CLIENT_ID`, `AZURE_CLIENT_SECRET`ve `AZURE_TENANT_ID`. bu değişkenleri yukarıda [bir hizmet anası](#create-a-service-principal) oluştur adımında belirttiğiniz clientId, clientSecret ve tenantId değerlerine ayarlayın. Çevresel `export VARNAME=VALUE` değişkenlerinizi ayarlamak için biçimi kullanın. (Bu yöntem yalnızca geçerli kabuğunuz için değişkenleri ve kabuktan oluşturulan işlemleri ayarlar; bu değişkenleri `/etc/environment ` ortamınıza kalıcı olarak eklemek, dosyanızı yeniden düzenlendi.) 

Ayrıca anahtar kasa adınızı bir ortam değişkeni `KEY_VAULT_NAME`olarak kaydetmeniz gerekir.

```console
export AZURE_CLIENT_ID=<your-clientID>

export AZURE_CLIENT_SECRET=<your-clientSecret>

export AZURE_TENANT_ID=<your-tenantId>

export KEY_VAULT_NAME=<your-key-vault-name>
````

## <a name="object-model"></a>Nesne modeli

Java için Azure Key Vault istemci kitaplığı, anahtarları ve sertifikalar ve sırlar gibi ilgili varlıkları yönetmenize olanak tanır. Aşağıdaki kod örnekleri, bir istemci oluşturmak, bir sır ayarlamak, bir sır almak ve bir sırrı silmek nasıl gösterecektir.

Tüm konsol uygulaması [aşağıdadır.](#sample-code)

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeekleme

Kodunuzun üst bölümüne aşağıdaki yönergeleri ekleyin:

```java
import com.azure.identity.DefaultAzureCredentialBuilder;

import com.azure.security.keyvault.secrets.SecretClient;
import com.azure.security.keyvault.secrets.SecretClientBuilder;
import com.azure.security.keyvault.secrets.models.KeyVaultSecret;
```

### <a name="authenticate-and-create-a-client"></a>Kimlik doğrulaması ve istemci oluşturma

Anahtar kasanıza doğrulanması ve anahtar kasa istemcisi oluşturma, yukarıdaki [Set çevresel değişkenler](#set-environmental-variables) adımındaki çevresel değişkenlere bağlıdır. Anahtar tonozunuzun adı, formatında, `https://<your-key-vault-name>.vault.azure.net`anahtar kasası URI'ye genişletilir.

```java
String keyVaultName = System.getenv("KEY_VAULT_NAME");
String kvUri = "https://" + keyVaultName + ".vault.azure.net";

SecretClient secretClient = new SecretClientBuilder()
    .vaultUrl(kvUri)
    .credential(new DefaultAzureCredentialBuilder().build())
    .buildClient();
```

### <a name="save-a-secret"></a>Bir sırrı kaydet

Uygulamanızın kimliği doğrulanına göre, `secretClient.setSecret` yöntemi kullanarak keyvault'unuza bir sır koyabilirsiniz. Bu, gizli bir ad gerektirir -- bu örnekteki `secretName` değişkene "mySecret" değerini atadık.  

```java
secretClient.setSecret(new KeyVaultSecret(secretName, secretValue));
```

Gizlinin [az keyvault gizli gösteri](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-show) komutuyla ayarlandığını doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

### <a name="retrieve-a-secret"></a>Bir sır alma

Şimdi `secretClient.getSecret` yöntemle önceden ayarlanmış değeri alabilirsiniz.

```java
KeyVaultSecret retrievedSecret = secretClient.getSecret(secretName);
 ```

Artık alınan sırrın değerine `retrievedSecret.getValue()`.

### <a name="delete-a-secret"></a>Gizli anahtarı silme

Son olarak, `secretClient.beginDeleteSecret` yöntemle anahtar kasanızdan sırrı silelim.

```java
secretClient.beginDeleteSecret(secretName);
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

```java
package com.keyvault.quickstart;

import java.io.Console;   

import com.azure.identity.DefaultAzureCredentialBuilder;

import com.azure.security.keyvault.secrets.SecretClient;
import com.azure.security.keyvault.secrets.SecretClientBuilder;
import com.azure.security.keyvault.secrets.models.KeyVaultSecret;

public class App {

    public static void main(String[] args) throws InterruptedException, IllegalArgumentException {

        String keyVaultName = System.getenv("KEY_VAULT_NAME");
        String kvUri = "https://" + keyVaultName + ".vault.azure.net";

        System.out.printf("key vault name = %s and kv uri = %s \n", keyVaultName, kvUri);

        SecretClient secretClient = new SecretClientBuilder()
            .vaultUrl(kvUri)
            .credential(new DefaultAzureCredentialBuilder().build())
            .buildClient();


        Console con = System.console();  

        String secretName = "mySecret";

        System.out.println("Input the value of your secret > ");
        String secretValue = con.readLine();

        System.out.print("Creating a secret in " + keyVaultName + " called '" + secretName + "' with the value '" + secretValue + "` ... ");

        secretClient.setSecret(new KeyVaultSecret(secretName, secretValue));

        System.out.println("done.");

        System.out.println("Forgetting your secret.");
        secretValue = "";
        System.out.println("Your secret is '" + secretValue + "'.");

        System.out.println("Retrieving your secret from " + keyVaultName + ".");

        KeyVaultSecret retrievedSecret = secretClient.getSecret(secretName);

        System.out.println("Your secret is '" + retrievedSecret.getValue() + "'.");
        System.out.print("Deleting your secret from " + keyVaultName + " ... ");

        secretClient.beginDeleteSecret(secretName);

        System.out.println("done.");


    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir anahtar kasası oluşturdunuz, bir sır sakladınız ve bu sırrı aldınız. Key Vault ve uygulamalarınızla nasıl entegre edilebildiğini öğrenmek için aşağıdaki makalelere devam edin.

- Azure [Anahtar Kasasına Genel Bakış](../general/overview.md)
- Azure [Key Vault geliştiricisi kılavuzuna](../general/developers-guide.md) bakın
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](../general/best-practices.md)

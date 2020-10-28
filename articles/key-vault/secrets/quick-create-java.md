---
title: Hızlı başlangıç-Java için Azure Key Vault istemci kitaplığı
description: Azure SDK istemci kitaplıkları için hızlı başlangıçlara yazmak üzere biçim ve içerik ölçütleri sağlar.
author: msmbaldwin
ms.custom: devx-track-java, devx-track-azurecli
ms.author: mbaldwin
ms.date: 10/20/2019
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.openlocfilehash: f4008b43c487f9dd1c8cfe5e5b67a250ff849daf
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92786216"
---
# <a name="quickstart-azure-key-vault-secret-client-library-for-java"></a>Hızlı başlangıç: Java için Azure Key Vault gizli istemci kitaplığı

Java için Azure Key Vault gizli istemci kitaplığı ile çalışmaya başlayın. Paketi yüklemek ve temel görevler için örnek kodu denemek üzere aşağıdaki adımları izleyin.

Ek kaynaklar:

* [Kaynak kod](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault)
* [API başvuru belgeleri](https://azure.github.io/azure-sdk-for-java)
* [Ürün belgeleri](index.yml)
* [Örnekler](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-secrets/src/samples/java/com/azure/security/keyvault/secrets)

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Java Development Kit (JDK)](/java/azure/jdk/) sürüm 8 veya üstü
- [Apache Maven](https://maven.apache.org)
- [Azure CLI](/cli/azure/install-azure-cli)

Bu hızlı başlangıçta, bir Linux Terminal penceresinde [Azure CLI](/cli/azure/install-azure-cli) ve [Apache Maven](https://maven.apache.org) çalıştırdığınız varsayılmaktadır.

## <a name="setting-up"></a>Ayarlanıyor

Bu hızlı başlangıç, Azure 'da kullanıcının kimliğini doğrulamak için Azure CLı ile Azure Identity Library kullanıyor. Geliştiriciler, çağrılarının kimliğini doğrulamak için Visual Studio veya Visual Studio Code de kullanabilir. daha fazla bilgi için bkz [. Azure Identity istemci kitaplığı ile Istemci kimlik doğrulaması](/java/api/overview/azure/identity-readme)

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

1. `login` komutunu çalıştırın.

    ```azurecli-interactive
    az login
    ```

    CLı varsayılan tarayıcınızı açabildiğinden, bunu yapılır ve bir Azure oturum açma sayfası yükler.

    Aksi takdirde, konumunda bir tarayıcı sayfası açın [https://aka.ms/devicelogin](https://aka.ms/devicelogin) ve terminalinizde görünen yetkilendirme kodunu girin.

2. Tarayıcıda hesabınızın kimlik bilgileriyle oturum açın.

### <a name="create-new-java-console-app"></a>Yeni Java konsol uygulaması oluşturma

Konsol penceresinde, `mvn` adıyla yeni bir Java konsol uygulaması oluşturmak için komutunu kullanın `akv-java` .

```console
mvn archetype:generate -DgroupId=com.keyvault.quickstart
                       -DartifactId=akv-java
                       -DarchetypeArtifactId=maven-archetype-quickstart
                       -DarchetypeVersion=1.4
                       -DinteractiveMode=false
```

Projenin üretilme çıktısı şuna benzer şekilde görünür:

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

Dizininizi yeni oluşturulan Akv-Java/klasörüyle değiştirin.

```console
cd akv-java
```

### <a name="install-the-package"></a>Paketi yükler

*pom.xml* dosyasını metin düzenleyicinizde açın. Aşağıdaki bağımlılık öğelerini bağımlılıklar grubuna ekleyin.

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

### <a name="create-a-resource-group-and-key-vault"></a>Kaynak grubu ve Anahtar Kasası oluşturma

[!INCLUDE [Create a resource group and key vault](../../../includes/key-vault-rg-kv-creation.md)]

#### <a name="grant-access-to-your-key-vault"></a>Anahtar kasanıza erişim izni verin

Anahtar kasanız için Kullanıcı hesabınıza gizli izin veren bir erişim ilkesi oluşturun

```console
az keyvault set-policy --name <YourKeyVaultName> --upn user@domain.com --secret-permissions delete get list set
```

#### <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Bu uygulama, Anahtar Kasası adını adında bir ortam değişkeni olarak kullanıyor `KEY_VAULT_NAME` .

Windows
```cmd
set KEY_VAULT_NAME=<your-key-vault-name>
````
Windows PowerShell
```powershell
$Env:KEY_VAULT_NAME=<your-key-vault-name>
```

macOS veya Linux
```cmd
export KEY_VAULT_NAME=<your-key-vault-name>
```

## <a name="object-model"></a>Nesne modeli

Java için Azure Key Vault gizli istemci kitaplığı, gizli dizileri yönetmenizi sağlar. [Kod örnekleri](#code-examples) bölümünde, bir istemci oluşturma, gizli anahtar ayarlama, gizli anahtar alma ve gizli dizi silme işlemlerinin nasıl yapılacağı gösterilir.

Konsol uygulamasının tamamı [aşağıda](#sample-code)verilmiştir.

## <a name="code-examples"></a>Kod örnekleri

### <a name="add-directives"></a>Yönergeler ekleme

Aşağıdaki yönergeleri kodunuzun en üstüne ekleyin:

```java
import com.azure.identity.DefaultAzureCredentialBuilder;

import com.azure.security.keyvault.secrets.SecretClient;
import com.azure.security.keyvault.secrets.SecretClientBuilder;
import com.azure.security.keyvault.secrets.models.KeyVaultSecret;
```

### <a name="authenticate-and-create-a-client"></a>İstemci kimliğini doğrulama ve oluşturma

Bu hızlı başlangıçta oturum açan kullanıcı, yerel geliştirme için tercih edilen yöntem olan Anahtar Kasası kimlik doğrulaması için kullanılır. Azure 'a dağıtılan uygulamalar için, yönetilen kimlik App Service veya sanal makineye atanmalıdır, daha fazla bilgi için bkz. [yönetilen kimliğe genel bakış](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

Aşağıdaki örnekte, anahtar kasanızın adı, "https://. vault.azure.net" biçiminde Anahtar Kasası URI 'sine genişletilir \<your-key-vault-name\> . Bu örnek, kimlik sağlamak için farklı seçeneklere sahip farklı ortamlarda aynı kodun kullanılmasına izin veren  [' DefaultAzureCredential () '](https://docs.microsoft.com/java/api/com.azure.identity.defaultazurecredential) sınıfını kullanıyor. Daha fazla bilgi için bkz. [varsayılan Azure kimlik bilgisi kimlik doğrulaması](https://docs.microsoft.com/java/api/overview/azure/identity-readme). 

```java
String keyVaultName = System.getenv("KEY_VAULT_NAME");
String kvUri = "https://" + keyVaultName + ".vault.azure.net";

SecretClient secretClient = new SecretClientBuilder()
    .vaultUrl(kvUri)
    .credential(new DefaultAzureCredentialBuilder().build())
    .buildClient();
```

### <a name="save-a-secret"></a>Gizli dizi Kaydet

Uygulamanızın kimliği doğrulandığına göre, yöntemini kullanarak anahtar kasanıza gizli dizi ekleyebilirsiniz `secretClient.setSecret` . Bu, gizli dizi için bir ad gerektirir `secretName` . Bu örnekteki değişkene "mySecret" değeri atandık.  

```java
secretClient.setSecret(new KeyVaultSecret(secretName, secretValue));
```

Parolanın [az keykasası Secret Show](/cli/azure/keyvault/secret?#az-keyvault-secret-show) komutuyla ayarlandığını doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
```

### <a name="retrieve-a-secret"></a>Gizli dizi alma

Artık, daha önce ayarlanan değeri yöntemiyle elde edebilirsiniz `secretClient.getSecret` .

```java
KeyVaultSecret retrievedSecret = secretClient.getSecret(secretName);
 ```

Artık alınan gizli dizi değerine ile erişebilirsiniz `retrievedSecret.getValue()` .

### <a name="delete-a-secret"></a>Gizli anahtarı silme

Son olarak, yöntemi ile anahtar kasaınızdan parolayı silelim `secretClient.beginDeleteSecret` .

```java
secretClient.beginDeleteSecret(secretName);
```

Parolanın [az keykasasecret Show](/cli/azure/keyvault/secret?#az-keyvault-secret-show) komutuyla yapıldığını doğrulayabilirsiniz:

```azurecli
az keyvault secret show --vault-name <your-unique-keyvault-name> --name mySecret
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

Bu hızlı başlangıçta bir Anahtar Kasası oluşturdunuz, gizli dizi depolandı ve bu gizli dizi alındı. Key Vault ve uygulamalarınızla tümleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere ilerleyin.

- [Azure Key Vault genel bakışını](../general/overview.md) okuyun
- [Azure Key Vault geliştirici kılavuzuna](../general/developers-guide.md) bakın
- [Anahtar kasasına erişimi güvenli hale getirme](../general/secure-your-key-vault.md)
- [En iyi uygulamaları](../general/best-practices.md) gözden geçirin Azure Key Vault
---
title: Java Bahar Önyükleme uygulamasında Azure App Configuration Key Vault referanslarını kullanmak için öğretici | Microsoft Dokümanlar
description: Bu eğitimde, Bir Java Bahar Önyükleme uygulamasından Azure App Configuration'ın Key Vault referanslarını nasıl kullanacağınızı öğreniyorsunuz
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 12/16/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: b6b6d10165eed331c397e17a18e382b095e1f74f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79216737"
---
# <a name="tutorial-use-key-vault-references-in-a-java-spring-app"></a>Öğretici: Java Spring uygulamasında Anahtar Vault referanslarını kullanma

Bu eğitimde, Azure Anahtar Kasası ile birlikte Azure Uygulama Yapılandırma hizmetini nasıl kullanacağınızı öğreneceksiniz. Uygulama Yapılandırması ve Anahtar Kasası, çoğu uygulama dağıtımında yan yana kullanılan tamamlayıcı hizmetlerdir.

Uygulama Yapılandırması, Key Vault'ta depolanan değerlere başvuru yapan anahtarlar oluşturarak hizmetleri birlikte kullanmanıza yardımcı olur. App Configuration bu tür anahtarları oluşturduğunda, değerlerin kendileri yerine Key Vault değerlerinin URI'lerini depolar.

Uygulamanız, Uygulama Yapılandırması'nda depolanan diğer anahtarlarda olduğu gibi Key Vault başvurularını almak için App Configuration istemci sağlayıcısını kullanır. Bu durumda, Uygulama Yapılandırmasında depolanan değerler, Anahtar Kasası'ndaki değerlere başvuran ÜR'lerdir. Bunlar Key Vault değerleri veya kimlik bilgileri değildir. İstemci sağlayıcı anahtarları Key Vault başvuruları olarak tanıdığından, değerlerini almak için Key Vault'u kullanır.

Uygulamanız, hem Uygulama Yapılandırması hem de Key Vault'a doğru şekilde doğru doğrulamadan sorumludur. İki servis doğrudan iletişim kurmaz.

Bu öğretici, anahtarınıza Anahtar Kasa başvurularının nasıl uygulanacağını gösterir. Bu quickstarts tanıtılan web uygulaması üzerine oluşturur. Devam etmeden önce, [önce App Configuration ile java spring uygulaması oluşturun'u](./quickstart-java-spring-app.md) tamamlayın.

Bu öğreticideki adımları yapmak için herhangi bir kod düzenleyicisini kullanabilirsiniz. Örneğin, [Visual Studio Code,](https://code.visualstudio.com/) Windows, macOS ve Linux işletim sistemleri için kullanılabilen bir çapraz platform kod düzenleyicisidir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Key Vault'ta depolanan bir değere başvuran bir Uygulama Yapılandırması anahtarı oluşturun.
> * Java Spring uygulamasından bu anahtarın değerine erişin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)
* Sürüm 8 ile desteklenen [bir Java Geliştirme Kiti (JDK).](https://docs.microsoft.com/java/azure/jdk)
* [Apache Maven](https://maven.apache.org/download.cgi) sürüm 3.0 veya üzeri.

## <a name="create-a-vault"></a>Kasa oluşturma

1. Azure portalının sol üst köşesinde **kaynak oluştur** seçeneğini seçin:

    ![Anahtar kasası oluşturma tamamlandıktan sonra çıktı](./media/quickstarts/search-services.png)
1. Arama kutusuna, **Anahtar Kasası**girin.
1. Sonuç listesinden soldaki **Anahtar kasalarını** seçin.
1. **Anahtar kasalarında** **Ekle'yi**seçin.
1. Sağdaki **Create anahtar kasasında,** aşağıdaki bilgileri sağlayın:
    * Abonelik seçmek için **Abonelik'i** seçin.
    * **Kaynak Grubu'nda** **yeni oluştur'u** seçin ve bir kaynak grubu adı girin.
    * **Anahtar kasası adında,** benzersiz bir ad gereklidir. Bu öğretici için **Contoso-vault2'yi**girin.
    * **Bölge** açılır listesinde bir konum seçin.
1. Diğer **Create anahtar kasa** seçeneklerini varsayılan değerleriyle bırakın.
1. **Oluştur'u**seçin.

Bu noktada, azure hesabınız bu yeni kasaya erişmeye yetkili tek hesaptır.

![Anahtar kasası oluşturma tamamlandıktan sonra çıktı](./media/quickstarts/vault-properties.png)

## <a name="add-a-secret-to-key-vault"></a>Key Vault’a gizli dizi ekleme

Kasaya bir sır eklemek için birkaç ek adım atmalısınız. Bu durumda, Anahtar Kasa sıyrıksını test etmek için kullanabileceğiniz bir ileti ekleyin. İletinin adı **İleti**ve içinde "Key Vault'tan Merhaba" değerini saklarsınız.

1. Key Vault özellikleri sayfalarından **Sırlar'ı**seçin.
1. **Oluştur/İçe Aktar'ı**seçin.
1. Gizli **bölmeoluştur'da** aşağıdaki değerleri girin:
    * **Yükleme seçenekleri**: **Kılavuzu**Girin .
    * **Adı**: **İleti**girin .
    * **Değer**: **Key Vault'tan Merhaba**girin.
1. Diğer invarsayılan değerleri ile **gizli özellikler oluştur** bırakın.
1. **Oluştur'u**seçin.

## <a name="add-a-key-vault-reference-to-app-configuration"></a>Uygulama Yapılandırmasına Anahtar Kasa sı başvurusu ekleme

1. [Azure portalında](https://portal.azure.com)oturum açın. **Tüm kaynakları**seçin ve ardından hızlı başlatmada oluşturduğunuz Uygulama Yapılandırma mağazası örneğini seçin.

1. **Configuration Explorer'ı**seçin.

1. +**Anahtar kasası başvurusu** **oluştur'u** > seçin ve ardından aşağıdaki değerleri belirtin:
    * **Anahtar**: **Seç /application/config.keyvaultmessage**
    * **Etiket**: Bu değeri boş bırakın.
    * **Abonelik**, **Kaynak grubu**ve **Anahtar kasası**: Önceki bölümde oluşturduğunuz anahtar kasasına değerlere karşılık gelen değerleri girin.
    * **Gizli**: Önceki bölümde oluşturduğunuz **İleti** adlı sırrı seçin.

## <a name="connect-to-key-vault"></a>Anahtar Kasasına Bağlan

1. Bu öğreticide, Key Vault kimlik doğrulaması için bir hizmet ilkesi kullanın. Bu hizmet ilkesini oluşturmak için Azure CLI [az reklam sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanın:

    ```azurecli
    az ad sp create-for-rbac -n "http://mySP" --sdk-auth
    ```

    Bu işlem bir dizi anahtar/değer çifti döndürür:

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

1. Servis müdürünün anahtar kasanıza erişebilsin diye aşağıdaki komutu çalıştırın:

    ```console
    az keyvault set-policy -n <your-unique-keyvault-name> --spn <clientId-of-your-service-principal> --secret-permissions delete get
    ```

1. Nesne kimliğinizi almak için aşağıdaki komutu çalıştırın ve ardından Uygulama Yapılandırması'na ekleyin.

    ```console
    az ad sp show --id <clientId-of-your-service-principal>
    az role assignment create --role "App Configuration Data Reader" --assignee-object-id <objectId-of-your-service-principal> --resource-group <your-resource-group>
    ```

1. Önceki adımda görüntülenen hizmet ilkesi değerlerini kullanarak aşağıdaki ortam değişkenlerini oluşturun:

    * **AZURE_CLIENT_ID**: *clientId*
    * **AZURE_CLIENT_SECRET**: *clientSecret*
    * **AZURE_TENANT_ID**: *tenantId*

> [!NOTE]
> Bu Anahtar Kasa kimlik bilgileri yalnızca uygulamanızda kullanılır.  Uygulamanız, Uygulama Yapılandırma hizmetini dahil etmeden bu kimlik bilgilerini kullanarak doğrudan Key Vault ile kimlik doğrulaması sağlar.  Key Vault, anahtarları paylaşmadan veya açığa çıkarmadan hem uygulamanız hem de Uygulama Yapılandırma hizmetiniz için kimlik doğrulaması sağlar.

## <a name="update-your-code-to-use-a-key-vault-reference"></a>Key Vault referansını kullanmak için kodunuzu güncelleştirin

1. *MessageProperties.java'yı*aç. *keyVaultMessage*adlı yeni bir değişken ekleyin:

    ```java
    private String keyVaultMessage;

    public String getKeyVaultMessage() {
        return keyVaultMessage;
    }

    public void setKeyVaultMessage(String keyVaultMessage) {
        this.keyVaultMessage = keyVaultMessage;
    }
    ```

1. *HelloController.java'yı*açın. Key Vault'tan alınan iletiyi içerecek şekilde *getMessage* yöntemini güncelleştirin.

    ```java
    @GetMapping
    public String getMessage() {
        return "Message: " + properties.getMessage() + "\nKey Vault message: " + properties.getKeyVaultMessage();
    }
    ```

1. *AzureCredentials.java* adında yeni bir dosya oluşturun ve aşağıdaki kodu ekleyin.

    ```java
    package com.example;

    import com.azure.core.credential.TokenCredential;
    import com.azure.identity.EnvironmentCredentialBuilder;
    import com.microsoft.azure.spring.cloud.config.AppConfigurationCredentialProvider;
    import com.microsoft.azure.spring.cloud.config.KeyVaultCredentialProvider;

    public class AzureCredentials implements AppConfigurationCredentialProvider, KeyVaultCredentialProvider{

        @Override
        public TokenCredential getKeyVaultCredential(String uri) {
            return getCredential();
        }

        @Override
        public TokenCredential getAppConfigCredential(String uri) {
            return getCredential();
        }

        private TokenCredential getCredential() {
            return new EnvironmentCredentialBuilder().build();
        }

    }
    ```

1. *AppConfiguration.java*adında yeni bir dosya oluşturun. Ve aşağıdaki kodu ekleyin.

    ```java
    package com.example;

    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;

    @Configuration
    public class AppConfiguration {

        @Bean
        public AzureCredentials azureCredentials() {
            return new AzureCredentials();
        }
    }
    ```

1. Kaynaklarımeta-INF dizininde *spring.factorys* adlı yeni bir dosya oluşturun ve ekleyin.

    ```factories
    org.springframework.cloud.bootstrap.BootstrapConfiguration=\
    com.example.AppConfiguration
    ```

1. Bahar Önyükleme uygulamanızı Maven ile oluşturun ve örneğin çalıştırın:

    ```shell
    mvn clean package
    mvn spring-boot:run
    ```

1. Uygulamanız çalışmaya devam ettikten sonra, örneğin uygulamanızı test etmek için *bukle'yi* kullanın:

      ```shell
      curl -X GET http://localhost:8080/
      ```

    Uygulama Yapılandırma sıcağında girdiğiniz iletiyi görürsünüz. Key Vault'a girdiğiniz iletiyi de görürsünüz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Key Vault'ta depolanan bir değere başvuran bir Uygulama Yapılandırması anahtarı oluşturdunuz. Java Spring uygulamanızda özellik bayraklarını nasıl kullanacağınızı öğrenmek için bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirmesi](./quickstart-feature-flag-spring-boot.md)

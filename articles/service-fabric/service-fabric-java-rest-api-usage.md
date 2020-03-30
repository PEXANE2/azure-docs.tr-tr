---
title: Azure Hizmet Kumaş Java İstemci API'leri
description: Service Fabric istemci API belirtimini kullanarak Service Fabric Java istemci API'larını oluşturun ve kullanın
author: rapatchi
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: rapatchi
ms.openlocfilehash: 0a243c1cd0ab0dcb93a1cc6169c89ba18606f346
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451678"
---
# <a name="azure-service-fabric-java-client-apis"></a>Azure Hizmet Kumaş Java İstemci API'leri

Service Fabric istemci API'leri, Azure'da, şirket içinde, yerel geliştirme makinesinde veya başka bir bulutta Hizmet Kumaşı kümesinde mikro hizmetlere dayalı uygulamaların ve kapsayıcıların dağıtılmasına ve yönetilmesine olanak tanır. Bu makalede, Service Fabric istemci REST API'lerinin üstünde Service Fabric Java istemci API'lerinin nasıl üretilip kullanılacağı açıklanmaktadır

## <a name="generate-the-client-code-using-autorest"></a>Otomatik Rest kullanarak istemci kodunu oluşturma

[AutoRest,](https://github.com/Azure/autorest) YENIDEN web hizmetlerine erişmek için istemci kitaplıkları oluşturan bir araçtır. AutoRest'e giriş, OpenAPI Belirtimi biçimini kullanarak REST API'sini açıklayan bir belirtimdir. [Service Fabric istemci REST API'leri](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/servicefabric/data-plane) bu belirtimi takip edin.

AutoRest aracını kullanarak Service Fabric Java istemci kodunu oluşturmak için aşağıda belirtilen adımları izleyin.

1. Makinenize nodejs ve NPM yükleme

    Linux kullanıyorsanız:
    ```bash
    sudo apt-get install npm
    sudo apt install nodejs
    ```
    Mac OS X kullanıyorsanız:
    ```bash
    brew install node
    ```

2. NPM kullanarak AutoRest'i yükleyin.
    ```bash
    npm install -g autorest
    ```

3. Yerel makinenizde [azure-rest-api-specs](https://github.com/Azure/azure-rest-api-specs) deposunda çatal ve klonlayın ve makinenizin terminalinden klonlanmış konuma gidin.


4. Klonlanmış repo'nuzda aşağıda belirtilen konuma gidin.
    ```bash
    cd specification\servicefabric\data-plane\Microsoft.ServiceFabric\stable\6.0
    ```

    > [!NOTE]
    > Küme sürümünüz 6.0.* değilse, kararlı klasördeki uygun dizine gidin.
    >   

5. Java istemci kodunu oluşturmak için aşağıdaki autorest komutunu çalıştırın.
    
    ```bash
    autorest --input-file= servicefabric.json --java --output-folder=[output-folder-name] --namespace=[namespace-of-generated-client]
    ```
   Aşağıda autorest kullanımını gösteren bir örnektir.
   
    ```bash
    autorest --input-file=servicefabric.json --java --output-folder=java-rest-api-code --namespace=servicefabricrest
    ```
   
   Aşağıdaki komut ``servicefabric.json`` belirtim dosyasını giriş olarak alır ``java-rest-api-     code`` ve klasörde java istemci ``servicefabricrest`` kodu oluşturur ve kodu ad alanına içine alır. Bu adımdan sonra iki ``models``klasör ``implementation`` ve ``ServiceFabricClientAPIs.java`` iki ``package-info.java`` dosya ``java-rest-api-code`` bulmak ve klasörde oluşturulur.


## <a name="include-and-use-the-generated-client-in-your-project"></a>Oluşturulan istemciyi projenize ekleme ve kullanma

1. Oluşturulan kodu projenize uygun şekilde ekleyin. Oluşturulan kodu kullanarak bir kitaplık oluşturmanızı ve bu kitaplığı projenize eklemenizi öneririz.
2. Kitaplık oluşturuyorsanız, kitaplığınızın projesine aşağıdaki bağımlılıkları ekleyin. Farklı bir yaklaşım uyguluyorsanız, bağımlılığı uygun şekilde ekleyin.

    ```
        GroupId:  com.microsoft.rest
        Artifactid: client-runtime
        Version: 1.2.1
    ```
    Örneğin, Maven yapı sistemini kullanıyorsanız dosyanızda ``pom.xml`` aşağıdakileri ekleyin:

    ```xml
        <dependency>
          <groupId>com.microsoft.rest</groupId>
          <artifactId>client-runtime</artifactId>
          <version>1.2.1</version>
        </dependency>
    ```

3. Aşağıdaki kodu kullanarak bir RestClient oluşturun:

    ```java
        RestClient simpleClient = new RestClient.Builder()
            .withBaseUrl("http://<cluster-ip or name:port>")
            .withResponseBuilderFactory(new ServiceResponseBuilder.Factory())
            .withSerializerAdapter(new JacksonAdapter())
            .build();
        ServiceFabricClientAPIs client = new ServiceFabricClientAPIsImpl(simpleClient);
    ```
4. İstemci nesnesini kullanın ve gerektiği gibi uygun aramaları yapın. İstemci nesnesinin kullanımını gösteren bazı örnekler aşağıda verilmiştir. Uygulama paketinin aşağıdaki API'leri kullanmadan önce oluşturulup görüntü deposuna yüklendiğini varsayıyoruz.
    * Bir uygulama sağlama
    
        ```java
            ApplicationTypeImageStorePath imageStorePath = new ApplicationTypeImageStorePath();
            imageStorePath.withApplicationTypeBuildPath("<application-path-in-image-store>");
            client.provisionApplicationType(imageStorePath);
        ```
    * Uygulama oluşturma

        ```java
            ApplicationDescription applicationDescription = new ApplicationDescription();
            applicationDescription.withName("<application-uri>");
            applicationDescription.withTypeName("<application-type>");
            applicationDescription.withTypeVersion("<application-version>");
            client.createApplication(applicationDescription);
        ```

## <a name="understanding-the-generated-code"></a>Oluşturulan kodu anlama
Her API için dört aşırı uygulama yükü bulacaksınız. İsteğe bağlı parametreler varsa, bu isteğe bağlı parametreler de dahil olmak üzere dört varyasyon daha bulabilirsiniz. Örneğin API ``removeReplica``düşünün.
 1. **public void removeReplica(String nodeName, UUID partitionId, String replicaId, Boolean forceRemove, Long timeout)**
    * Bu removeReplica API çağrısının senkron varyantı
 2. **kamu ServiceFuture\<Void> removeReplicaAsync(String nodeName, UUID partitionId, String replicaId, Boolean\<forceRemove, Uzun zaman aşıntı, son ServiceCallback Void> serviceCallback)**
    * Gelecekteki tabanlı asynchronous programlamayı ve geri aramaları kullanmak istiyorsanız, API çağrısının bu varyantı kullanılabilir
 3. **genel Gözlemlenebilir\<Void> removeReplicaAsync(String nodeName, UUID partitionId, String replicaId)**
    * Reaktif asynchronous programlama kullanmak istiyorsanız API çağrısının bu varyantı kullanılabilir
 4. **kamu Gözlemlenebilir\<ServiceResponse\<Void>> removeReplicaWithServiceResponseAsync (String düğümName, UUID partitionId, String replicaId)**
    * Reaktif asynchronöz programlama kullanmak ve RAW dinlenme yanıtı ile başa çıkmak istiyorsanız API çağrısının bu varyantı kullanılabilir

## <a name="next-steps"></a>Sonraki adımlar
* Service [Fabric REST API'leri](https://docs.microsoft.com/rest/api/servicefabric/) hakkında bilgi edinin


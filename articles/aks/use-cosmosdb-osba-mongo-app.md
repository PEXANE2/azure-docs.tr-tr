---
title: Varolan MongoDB uygulamasını MongoDB için Azure Cosmos DB API ve Azure için Açık Servis Aracısı (OSBA) ile tümleştirin
description: Bu makalede, varolan bir Java ve MongoDB uygulamasını Azure için Açık Servis Aracısı (OSBA) kullanarak MongoDB için Azure Cosmos DB API ile nasıl entegre acağınızı öğreneceksiniz.
author: zr-msft
ms.service: azure-dev-spaces
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: zarhoads
ms.custom: mvc
keywords: Cosmos DB, Açık Servis Aracısı, Azure için Açık Servis Brokerı
ms.openlocfilehash: ddaa3b9aa198bc142e1bcbcab6b7b1e028eff2aa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78247929"
---
# <a name="integrate-existing-mongodb-application-with-azure-cosmos-db-api-for-mongodb-and-open-service-broker-for-azure-osba"></a>Varolan MongoDB uygulamasını MongoDB için Azure Cosmos DB API ve Azure için Açık Servis Aracısı (OSBA) ile tümleştirin

Azure Cosmos DB global olarak dağıtılmış, çok modelli bir veritabanıdır. Ayrıca MongoDB için de dahil olmak üzere birçok NoSQL API'si ile tel protokolü uyumluluğu sağlar. MongoDB için Cosmos DB API, uygulamanızın veritabanı sürücülerini veya uygulamasını değiştirmek zorunda kalmadan cosmos DB'yi mevcut MongoDB uygulamanızla birlikte kullanmanıza olanak tanır. Azure için Açık Servis Aracısı'nı kullanarak bir Cosmos DB hizmeti de sağlayabilirsiniz.

Bu makalede, MongoDB veritabanı kullanan varolan bir Java uygulamasını alır ve Azure için Open Service Broker'ı kullanarak cosmos DB veritabanını kullanmak üzere güncelleştirmiş siniz.

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce şunları yapmalısın:
    
* Azure [Kubernetes Hizmet kümesi](kubernetes-walkthrough.md) oluşturuldu.
* [Azure için Açık Servis Aracısı'nı AKS kümenizde yüklü ve yapılandırMış](integrate-azure.md)olun. 
* Hizmet [Kataloğu CLI'yi](https://svc-cat.io/docs/install/) komutları çalıştırmak `svcat` için yüklü ve yapılandırın.
* Varolan bir [MongoDB veritabanına](https://www.mongodb.com/) sahip olun. Örneğin, MongoDB'nin [geliştirme makinenizde](https://docs.mongodb.com/manual/administration/install-community/) veya bir [Azure VM'de](../virtual-machines/linux/install-mongodb.md)çalışmasını sağlayabilirsiniz.
* [Mongo kabuk](https://docs.mongodb.com/manual/mongo/)gibi MongoDB veritabanına bağlanma nın ve sorgulamanın bir yolu vardır.

## <a name="get-application-code"></a>Uygulama kodunu alma
    
Bu makalede, MongoDB veritabanı kullanan bir uygulamayı göstermek için [Cloud Foundry'nin yay](https://github.com/cloudfoundry-samples/spring-music) müziği örnek uygulamasını kullanıyorsunuz.
    
Uygulamayı GitHub'dan klonla ve dizinine gidin:
    
```cmd
git clone https://github.com/cloudfoundry-samples/spring-music
cd spring-music
```

## <a name="prepare-the-application-to-use-your-mongodb-database"></a>Uygulamayı MongoDB veritabanınızı kullanmak üzere hazırlayın

Yay müziği örnek uygulaması veri kaynakları için birçok seçenek sağlar. Bu makalede, varolan bir MongoDB veritabanını kullanacak şekilde yapılandırırsınız. 

*src/main/resources/application.yml*sonuna aşağıdaki YAML'yi ekleyin. Bu ek, *mongodb* adlı bir profil oluşturur ve bir URI ve veritabanı adını yapılandırır. URI'yi mevcut MongoDB veritabanınıza bağlantı bilgileriyle değiştirin. Kullanıcı adı ve parola içeren URI'nin doğrudan bu dosyaya eklenmesi **yalnızca geliştirme kullanımı** içindir ve sürüm **denetimine hiçbir zaman eklenmemelidir.**

```yaml
---
spring:
  profiles: mongodb
  data:
    mongodb:
      uri: "mongodb://user:password@serverAddress:port/musicdb"
      database: musicdb
```



Uygulamanızı başlattığınızda ve *mongodb* profilini kullanmasını söylediğinde, mongoDB veritabanınıza bağlanır ve uygulamanın verilerini depolamak için kullanır.

Uygulamanızı oluşturmak için:

```cmd
./gradlew clean assemble

Starting a Gradle Daemon (subsequent builds will be faster)

BUILD SUCCESSFUL in 10s
4 actionable tasks: 4 executed
```

Başvurunuzu başlatın ve *mongodb* profilini kullanmasını söyleyin:

```cmd
java -jar -Dspring.profiles.active=mongodb build/libs/spring-music-1.0.jar
```

`http://localhost:8080` Tarayıcınızda gidin.

![Varsayılan veri içeren Bahar Müzik uygulaması](media/music-app.png)

Uygulamanın bazı [varsayılan verilerle](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/resources/albums.json)dolduruladığına dikkat edin. Birkaç varolan albümleri silerek ve birkaç yeni albüm oluşturarak onunla etkileşimkurun.

Uygulamanızın MongoDB veritabanınızı kullandığını doğrulayarak ona bağlanabilirsiniz ve *musicdb* veritabanını sorgulayabilirsiniz:

```cmd
mongo serverAddress:port/musicdb -u user -p password
use musicdb
db.album.find()

{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446d"), "title" : "Nevermind", "artist" : "Nirvana", "releaseYear" : "1991", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446e"), "title" : "Pet Sounds", "artist" : "The Beach Boys", "releaseYear" : "1966", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446f"), "title" : "What's Going On", "artist" : "Marvin Gaye", "releaseYear" : "1971", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
...
```

Önceki örnek, MongoDB veritabanına bağlanmak ve sorgulamak için [mongo kabuğunu](https://docs.mongodb.com/manual/mongo/) kullanır. Ayrıca, uygulamanızı durdurarak, yeniden başlatarak ve tarayıcınızda geri gezinerek değişikliklerinizin devam ettiğini de doğrulayabilirsiniz. Yaptığınız değişikliklerin hala orada olduğuna dikkat edin.


## <a name="create-a-cosmos-db-database"></a>Cosmos DB veritabanı oluşturma

Açık Servis Aracısını kullanarak Azure'da bir Cosmos `svcat provision` DB veritabanı oluşturmak için aşağıdaki komutu kullanın:

```cmd
svcat provision musicdb --class azure-cosmosdb-mongo-account --plan account  --params-json '{
  "location": "eastus",
  "resourceGroup": "MyResourceGroup",
  "ipFilters" : {
    "allowedIPRanges" : ["0.0.0.0/0"]
  }
}'
```

Önceki komut, *Eastus* bölgesindeki *MyResourceGroup* kaynak grubunda Azure'da bir Cosmos DB veritabanını hükümler. *ResourceGroup,* *konum*ve diğer Azure'a özgü JSON parametreleri hakkında daha fazla bilgiyi [Cosmos DB modülü başvuru belgelerinde](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/cosmosdb.md#provision-3)edinilebilir.

Veritabanınızın sağlanmasını tamamladığını doğrulamak `svcat get instance` için aşağıdaki komutu kullanın:

```cmd
$ svcat get instance musicdb

   NAME     NAMESPACE              CLASS                PLAN     STATUS
+---------+-----------+------------------------------+---------+--------+
  musicdb   default     azure-cosmosdb-mongo-account   account   Ready
```

STATUS *altında* *Hazır'a* bakınca veritabanınız hazırdır.

Veritabanınız sağlama yı tamamladıktan sonra, meta verilerini bir [Kubernetes sırrına](https://kubernetes.io/docs/concepts/configuration/secret/)bağlamanız gerekir. Diğer uygulamalar daha sonra bir gizli bağlı sonra bu verilere erişebilirsiniz. Veritabanınızın meta verilerini bir gizliye bağlamak için `svcat bind` aşağıdaki komutu kullanın:

```cmd
$ svcat bind musicdb

  Name:        musicdb
  Namespace:   default
  Status:
  Secret:      musicdb
  Instance:    musicdb

Parameters:
  No parameters defined
```


## <a name="use-the-cosmos-db-database-with-your-application"></a>Uygulamanızla Cosmos DB veritabanını kullanın

Uygulamanızla birlikte Cosmos DB veritabanını kullanmak için, uygulamaya bağlanmak için URI bilmeniz gerekir. Bu bilgileri almak için `kubectl get secret` aşağıdaki komutu kullanın:

```cmd
$ kubectl get secret musicdb -o=jsonpath='{.data.uri}' | base64 --decode

mongodb://12345678-90ab-cdef-1234-567890abcdef:aaaabbbbccccddddeeeeffffgggghhhhiiiijjjjkkkkllllmmmmnnnnooooppppqqqqrrrrssssttttuuuuvvvv@098765432-aaaa-bbbb-cccc-1234567890ab.documents.azure.com:10255/?ssl=true&replicaSet=globaldb
```

Önceki komut *musicdb* gizli alır ve sadece URI görüntüler. Sırlar base64 formatında depolanır, böylece önceki komut da şifreyi çözer.

Cosmos DB veritabanının URI'sini kullanarak *src/main/resources/application.yml'yi* kullanarak güncelleyin:

```yaml
...
---
spring:
  profiles: mongodb
  data:
    mongodb:
      uri: "mongodb://12345678-90ab-cdef-1234-567890abcdef:aaaabbbbccccddddeeeeffffgggghhhhiiiijjjjkkkkllllmmmmnnnnooooppppqqqqrrrrssssttttuuuuvvvv@098765432-aaaa-bbbb-cccc-1234567890ab.documents.azure.com:10255/?ssl=true&replicaSet=globaldb"
      database: musicdb
```

Kullanıcı adı ve parola içeren URI'nin güncellenmesi, yalnızca **geliştirme kullanımı** içindir ve sürüm **denetimine hiçbir zaman eklenmemelidir.**

Cosmos DB veritabanını kullanmaya başlamak için uygulamanızı yeniden oluşturve başlatın:

```cmd
./gradlew clean assemble

java -jar -Dspring.profiles.active=mongodb build/libs/spring-music-1.0.jar
```

Uygulamanızın hala *mongodb* profilini ve Cosmos DB veritabanına bağlanmak için *mongodb://* ile başlayan bir URI kullandığına dikkat edin. [MongoDB için Azure Cosmos DB API](../cosmos-db/mongodb-introduction.md) bu uyumluluğu sağlar. Uygulamanızın mongoDB veritabanı kullanıyormuş gibi çalışmaya devam etmesini sağlar, ancak aslında Cosmos DB kullanıyor.

`http://localhost:8080` Tarayıcınızda gidin. Varsayılan verilerin geri yüklendiğini fark edin. Birkaç varolan albümleri silerek ve birkaç yeni albüm oluşturarak onunla etkileşimkurun. Uygulamanızı durdurarak, yeniden başlatarak ve tarayıcınızda geri gezinerek değişikliklerinizin devam ettiğini doğrulayabilirsiniz. Yaptığınız değişikliklerin hala orada olduğuna dikkat edin. Değişiklikler, Azure için Açık Servis Aracısı kullanarak oluşturduğunuz Cosmos DB'de kalıcıdır.


## <a name="run-your-application-on-your-aks-cluster"></a>Uygulamanızı AKS kümenizde çalıştırın

Uygulamayı AKS kümenize dağıtmak için [Azure Dev Spaces'i](../dev-spaces/azure-dev-spaces.md) kullanabilirsiniz. Azure Geliştirme Alanları, Dockerfiles ve Helm grafikleri gibi yapılar oluşturmanıza ve AKS'de bir uygulamayı dağıtmanıza ve çalıştırmanıza yardımcı olur.

AKS kümenizde Azure Dev Alanları'nı etkinleştirmek için:

```azurecli
az aks enable-addons --addons http_application_routing -g MyResourceGroup -n MyAKS
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```

Uygulamanızı AKS'de çalışacak şekilde hazırlamak için Azure Geliştirme Alanları aracını kullanın:

```cmd
azds prep --public
```

Bu komut, projenin kökünde Miğfer grafiğiniz olan bir *grafik/klasör* de dahil olmak üzere çeşitli yapılar oluşturur. Bu komut, bu belirli proje için bir *Dockerfile* oluşturamaz, bu nedenle onu oluşturmanız gerekir.

Bu içerikle *Dockerfile* adlı projenizin kökünde bir dosya oluşturun:

```dockerfile
FROM openjdk:8-jdk-alpine
EXPOSE 8080
WORKDIR /app
COPY build/libs/spring-music-1.0.jar .
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-Dspring.profiles.active=mongodb","-jar","/app/spring-music-1.0.jar"]
```

Buna ek olarak, yapılandırmaları güncellemeniz *gerekir.develop.build* özellik *in azds.yaml* *yanlış*:
```yaml
...
configurations:
  develop:
    build:
      useGitIgnore: false
```

Ayrıca *charts / bahar-müzik / şablonlar / deployment.yaml* *8080* *containerPort* özniteliği güncellemeniz gerekir:

```yaml
...
spec:
  ...
  template:
    ...
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: http
              containerPort: 8080
              protocol: TCP
```

Uygulamanızı AKS'ye dağıtmak için:

```cmd
$ azds up

Using dev space 'default' with target 'MyAKS'
Synchronizing files...1m 18s
Installing Helm chart...5s
Waiting for container image build...23s
Building container image...
Step 1/5 : FROM openjdk:8-jdk-alpine
Step 2/5 : EXPOSE 8080
Step 3/5 : WORKDIR /app
Step 4/5 : COPY build/libs/spring-music-1.0.jar .
Step 5/5 : ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-Dspring.profiles.active=mongodb","-jar","/app/spring-music-1.0.jar"]
Built container image in 21s
Waiting for container...8s
Service 'spring-music' port 'http' is available at http://spring-music.1234567890abcdef1234.eastus.aksapp.io/
Service 'spring-music' port 8080 (TCP) is available at http://localhost:57892
press Ctrl+C to detach
...
```

Günlüklerde görüntülenen URL'ye gidin. Önceki örnekte, *http://spring-music.1234567890abcdef1234.eastus.aksapp.io/*'. 

Değişikliklerinizle birlikte uygulamayı gördüğünüzden doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, MongoDB'yi kullanmaktan MongoDB için Cosmos DB API'nin kullanılmasına kadar varolan bir uygulamanın nasıl güncelleştirileni açıklanmıştır. Bu makalede ayrıca, Azure için Açık Servis Aracısı'nı kullanarak bir Cosmos DB hizmetinin nasıl sağlandığı ve bu uygulamanın Azure Dev Spaces ile AKS'ye nasıl dağıtılacağı da ele alınmıştır.

Cosmos DB, Azure için Açık Servis Aracısı ve Azure Dev Alanları hakkında daha fazla bilgi için bkz:
* [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)
* [Azure için Açık Hizmet Aracısı](https://osba.sh)
* [Dev Spaces ile geliştirme](../dev-spaces/azure-dev-spaces.md)

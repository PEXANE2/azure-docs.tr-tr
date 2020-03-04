---
title: Mevcut MongoDB uygulamasını MongoDB için Azure Cosmos DB API ile tümleştirme ve Azure için Hizmet Aracısı açma (OSBA)
description: Bu makalede, var olan bir Java ve MongoDB uygulamasını Azure için açık Hizmet Aracısı (OSBA) kullanarak MongoDB için Azure Cosmos DB API 'siyle nasıl tümleştirileceğini öğreneceksiniz.
author: zr-msft
ms.service: azure-dev-spaces
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: zarhoads
ms.custom: mvc
keywords: Cosmos DB, Hizmet Aracısı açın, Azure için Hizmet Aracısı açın
ms.openlocfilehash: ddaa3b9aa198bc142e1bcbcab6b7b1e028eff2aa
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78247929"
---
# <a name="integrate-existing-mongodb-application-with-azure-cosmos-db-api-for-mongodb-and-open-service-broker-for-azure-osba"></a>Mevcut MongoDB uygulamasını MongoDB için Azure Cosmos DB API ile tümleştirme ve Azure için Hizmet Aracısı açma (OSBA)

Azure Cosmos DB global olarak dağıtılmış, çok modelli bir veritabanıdır. Ayrıca MongoDB gibi çeşitli NoSQL API 'lerle kablo protokol uyumluluğu sağlar. MongoDB için Cosmos DB API 'SI, uygulamanızın veritabanı sürücülerini veya uygulamasını değiştirmek zorunda kalmadan mevcut MongoDB uygulamanızla Cosmos DB kullanmanıza olanak sağlar. Ayrıca, Azure için açık Hizmet Aracısı kullanarak bir Cosmos DB hizmeti sağlayabilirsiniz.

Bu makalede, bir MongoDB veritabanı kullanan ve Azure için açık Hizmet Aracısı kullanarak bir Cosmos DB veritabanı kullanmak üzere güncelleştiren mevcut bir Java uygulaması alırsınız.

## <a name="prerequisites"></a>Önkoşullar

Devam edebilmeniz için önce şunları yapmanız gerekir:
    
* Oluşturulmuş bir [Azure Kubernetes hizmeti kümesi](kubernetes-walkthrough.md) vardır.
* [Azure Için açık hizmet Aracısı, AKS kümenizde yüklenmiş ve yapılandırılmış](integrate-azure.md)olmalıdır. 
* [Hizmet kataloğu CLI](https://svc-cat.io/docs/install/) 'nın yüklü ve `svcat` komutlarını çalıştıracak şekilde yapılandırılmış olması gerekir.
* Mevcut bir [MongoDB](https://www.mongodb.com/) veritabanına sahip. Örneğin, MongoDB 'yi [geliştirme makinenizde](https://docs.mongodb.com/manual/administration/install-community/) veya BIR [Azure VM](../virtual-machines/linux/install-mongodb.md)'de çalıştırıyor olabilirsiniz.
* [Mongo kabuğu](https://docs.mongodb.com/manual/mongo/)gibi MongoDB veritabanına bağlanma ve sorgulama yapmanın bir yoludur.

## <a name="get-application-code"></a>Uygulama kodunu alma
    
Bu makalede, MongoDB veritabanı kullanan bir uygulamayı göstermek için [Cloud Foundry 'daki Spring Music örnek uygulamasını](https://github.com/cloudfoundry-samples/spring-music) kullanacaksınız.
    
Uygulamayı GitHub 'dan kopyalayıp dizinine gidin:
    
```cmd
git clone https://github.com/cloudfoundry-samples/spring-music
cd spring-music
```

## <a name="prepare-the-application-to-use-your-mongodb-database"></a>MongoDB veritabanınızı kullanmak için uygulamayı hazırlama

Spring Music örnek uygulaması, veri kaynakları için birçok seçenek sunar. Bu makalede, mevcut bir MongoDB veritabanını kullanacak şekilde yapılandırın. 

*Src/Main/Resources/Application. yıml*sonuna aşağıdaki YAML 'yi ekleyin. Bu ek olarak, *MongoDB* adlı bir profil oluşturulur ve bir URI ve veritabanı adı yapılandırılır. URI değerini mevcut MongoDB veritabanınıza bağlantı bilgileriyle değiştirin. Bir Kullanıcı adı ve parola içeren URI 'nin eklenmesi **yalnızca geliştirme amaçlıdır** ve **sürüm denetimine hiçbir şekilde eklenmemelidir**.

```yaml
---
spring:
  profiles: mongodb
  data:
    mongodb:
      uri: "mongodb://user:password@serverAddress:port/musicdb"
      database: musicdb
```



Uygulamanızı başlatıp *MongoDB* profilini kullanmayı söylemeniz durumunda, MongoDB veritabanınıza bağlanır ve uygulamanın verilerini depolamak için onu kullanın.

Uygulamanızı derlemek için:

```cmd
./gradlew clean assemble

Starting a Gradle Daemon (subsequent builds will be faster)

BUILD SUCCESSFUL in 10s
4 actionable tasks: 4 executed
```

Uygulamanızı başlatın ve *MongoDB* profilini kullanmayı söyleyin:

```cmd
java -jar -Dspring.profiles.active=mongodb build/libs/spring-music-1.0.jar
```

Tarayıcınızda `http://localhost:8080` gidin.

![Varsayılan verilerle Spring Music uygulaması](media/music-app.png)

Uygulamanın bazı [varsayılan verilerle](https://github.com/cloudfoundry-samples/spring-music/blob/master/src/main/resources/albums.json)doldurulduğuna dikkat edin. Var olan birkaç albümü silerek ve birkaç yeni tane oluşturarak etkileşime geçin.

Uygulamanızın MongoDB veritabanınızı kullanarak bu ağa bağlanarak ve *musicdb* veritabanını sorgulayarak bu veritabanını kullandığını doğrulayabilirsiniz:

```cmd
mongo serverAddress:port/musicdb -u user -p password
use musicdb
db.album.find()

{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446d"), "title" : "Nevermind", "artist" : "Nirvana", "releaseYear" : "1991", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446e"), "title" : "Pet Sounds", "artist" : "The Beach Boys", "releaseYear" : "1966", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
{ "_id" : ObjectId("5c1bb6f5df0e66f13f9c446f"), "title" : "What's Going On", "artist" : "Marvin Gaye", "releaseYear" : "1971", "genre" : "Rock", "trackCount" : 0, "_class" : "org.cloudfoundry.samples.music.domain.Album" }
...
```

Önceki örnekte [Mongo kabuğu](https://docs.mongodb.com/manual/mongo/) , MongoDB veritabanına bağlanmak ve veritabanını sorgulamak için kullanılır. Ayrıca, uygulamanızı durdurup yeniden başlatarak ve tarayıcınıza geri giderek değişikliklerinizin kalıcı olduğunu doğrulayabilirsiniz. Yaptığınız değişikliklerin hala orada olduğuna dikkat edin.


## <a name="create-a-cosmos-db-database"></a>Cosmos DB veritabanı oluşturma

Azure 'da açık Hizmet Aracısı kullanarak bir Cosmos DB veritabanı oluşturmak için `svcat provision` komutunu kullanın:

```cmd
svcat provision musicdb --class azure-cosmosdb-mongo-account --plan account  --params-json '{
  "location": "eastus",
  "resourceGroup": "MyResourceGroup",
  "ipFilters" : {
    "allowedIPRanges" : ["0.0.0.0/0"]
  }
}'
```

Yukarıdaki komut, *eastus* bölgesindeki *myresourcegroup* kaynak grubunda Azure 'da bir Cosmos DB veritabanı sağlar. *ResourceGroup*, *Location*ve DIĞER Azure 'a özgü JSON parametreleri hakkında daha fazla bilgi [Cosmos DB modülü başvuru belgelerinde](https://github.com/Azure/open-service-broker-azure/blob/master/docs/modules/cosmosdb.md#provision-3)bulunabilir.

Veritabanınızın sağlamayı tamamladığınızı doğrulamak için `svcat get instance` komutunu kullanın:

```cmd
$ svcat get instance musicdb

   NAME     NAMESPACE              CLASS                PLAN     STATUS
+---------+-----------+------------------------------+---------+--------+
  musicdb   default     azure-cosmosdb-mongo-account   account   Ready
```

*Durum durumu*' nun altında, *Veritabanınız hazırlanıyor.*

Veritabanınızın sağlamayı tamamladıktan sonra, meta verilerini bir [Kubernetes gizli](https://kubernetes.io/docs/concepts/configuration/secret/)öğesine bağlamanız gerekir. Daha sonra diğer uygulamalar, bir gizli anahtar eklendikten sonra bu verilere erişebilir. Veritabanınızın meta verilerini gizli bir parolaya bağlamak için `svcat bind` komutunu kullanın:

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


## <a name="use-the-cosmos-db-database-with-your-application"></a>Cosmos DB veritabanını uygulamanızla birlikte kullanma

Cosmos DB veritabanını uygulamanızla birlikte kullanmak için, bağlanılacak URI 'yi bilmeniz gerekir. Bu bilgileri almak için `kubectl get secret` komutunu kullanın:

```cmd
$ kubectl get secret musicdb -o=jsonpath='{.data.uri}' | base64 --decode

mongodb://12345678-90ab-cdef-1234-567890abcdef:aaaabbbbccccddddeeeeffffgggghhhhiiiijjjjkkkkllllmmmmnnnnooooppppqqqqrrrrssssttttuuuuvvvv@098765432-aaaa-bbbb-cccc-1234567890ab.documents.azure.com:10255/?ssl=true&replicaSet=globaldb
```

Yukarıdaki komut, *musicdb* gizliliğini alır ve yalnızca URI 'yi görüntüler. Parolalar Base64 biçiminde depolanır, bu nedenle yukarıdaki komut de bunun kodunu çözer.

Cosmos DB veritabanının URI 'sini kullanarak, bunu kullanmak için *src/Main/Resources/Application. yml* 'yi güncelleştirin:

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

Bir Kullanıcı adı ve parola içeren URI 'yi güncelleştirme **yalnızca geliştirme amaçlıdır** ve **sürüm denetimine hiçbir şekilde eklenmemelidir**.

Cosmos DB veritabanını kullanmaya başlamak için uygulamanızı yeniden derleyin ve başlatın:

```cmd
./gradlew clean assemble

java -jar -Dspring.profiles.active=mongodb build/libs/spring-music-1.0.jar
```

Uygulamanız hala *MongoDB* profilini ve Cosmos db veritabanına bağlanmak için *MongoDB://* ile başlayan bir URI 'yi kullandığından emin olun. [MongoDB için Azure Cosmos DB API 'si](../cosmos-db/mongodb-introduction.md) bu uyumluluğu sağlar. Uygulamanızın MongoDB veritabanı kullanıyor gibi çalışmaya devam etmesine izin verir, ancak gerçekten Cosmos DB kullanıyor.

Tarayıcınızda `http://localhost:8080` gidin. Varsayılan verilerin geri yüklendiğini unutmayın. Var olan birkaç albümü silerek ve birkaç yeni tane oluşturarak etkileşime geçin. Uygulamanızı durdurup yeniden başlatarak ve tarayıcınıza geri giderek değişikliklerinizin kalıcı olduğunu doğrulayabilirsiniz. Yaptığınız değişikliklerin hala orada olduğuna dikkat edin. Değişiklikler, Azure için açık Hizmet Aracısı kullanarak oluşturduğunuz Cosmos DB kalıcı hale getirilir.


## <a name="run-your-application-on-your-aks-cluster"></a>Uygulamanızı AKS kümenizde çalıştırma

Uygulamayı AKS kümenize dağıtmak için [Azure dev Spaces](../dev-spaces/azure-dev-spaces.md) kullanabilirsiniz. Azure Dev Spaces, Dockerfiles ve HELI grafikleri gibi yapıtlar oluşturmanıza ve bir uygulamayı AKS 'de dağıtmanıza ve çalıştırmaya yardımcı olur.

AKS kümenizdeki Azure Dev Spaces etkinleştirmek için:

```azurecli
az aks enable-addons --addons http_application_routing -g MyResourceGroup -n MyAKS
az aks use-dev-spaces -g MyResourceGroup -n MyAKS
```

Uygulamanızı AKS 'de çalışacak şekilde hazırlamak için Azure Dev Spaces araçları 'nı kullanın:

```cmd
azds prep --public
```

Bu komut, projenin kökünde helk grafiğiniz olan bir *grafik/* klasör dahil olmak üzere çeşitli yapıtlar oluşturur. Bu komut, bu belirli proje için bir *Dockerfile* oluşturamaz, bu nedenle oluşturmanız gerekir.

Bu içerikle *Dockerfile* adlı projenizin kökünde bir dosya oluşturun:

```dockerfile
FROM openjdk:8-jdk-alpine
EXPOSE 8080
WORKDIR /app
COPY build/libs/spring-music-1.0.jar .
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-Dspring.profiles.active=mongodb","-jar","/app/spring-music-1.0.jar"]
```

Ayrıca, *azds. YAML* 'de *Configurations. Build* özelliğini *false*olarak güncelleştirmeniz gerekir:
```yaml
...
configurations:
  develop:
    build:
      useGitIgnore: false
```

Ayrıca, *Spring-Music/Templates/Deployment. YAML*Içindeki *containerport* özniteliğini de *8080* olarak güncelleştirmeniz gerekir:

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

Uygulamanızı AKS 'e dağıtmak için:

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

Günlüklerde görüntülenecek URL 'ye gidin. Yukarıdaki örnekte *http://spring-music.1234567890abcdef1234.eastus.aksapp.io/* kullanırsınız. 

Değişiklikleri yaptığınız değişikliklerle birlikte, uygulamayı gördiğinizi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, MongoDB için Cosmos DB API 'sini kullanarak mevcut bir uygulamayı MongoDB kullanarak güncelleştirme yöntemi açıklanmaktadır. Bu makalede ayrıca, Azure için açık Hizmet Aracısı kullanarak Cosmos DB bir hizmetin nasıl sağlanacağı ve bu uygulamanın Azure Dev Spaces ile AKS 'e dağıtılmasının ele alınmaktadır.

Cosmos DB hakkında daha fazla bilgi için Azure Hizmet Aracısı açın ve Azure Dev Spaces, bkz:
* [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/)
* [Azure için Hizmet Aracısı açın](https://osba.sh)
* [Geliştirme alanları ile geliştirme](../dev-spaces/azure-dev-spaces.md)

---
title: Azure Kubernetes Hizmeti (AKS) ile Bir Apache Spark işi çalıştırın
description: Apache Spark işini çalıştırmak için Azure Kubernetes Hizmetini (AKS) kullanın
author: lenadroid
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: alehall
ms.custom: mvc
ms.openlocfilehash: 4b3248cb9ab61a158f70b5a2d6ae9dd846501816
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473634"
---
# <a name="running-apache-spark-jobs-on-aks"></a>AKS'de Apache Spark işleri çalıştırmak

[Apache Spark,][apache-spark] büyük ölçekli veri işleme için hızlı bir motordur. [Spark 2.3.0 sürümü][spark-latest-release]itibariyle, Apache Spark Kubernetes kümeleri ile yerel entegrasyonu destekler. Azure Kubernetes Service (AKS), Azure'da çalışan yönetilen bir Kubernetes ortamıdır. Bu belge, Bir Azure Kubernetes Hizmeti (AKS) kümesinde Apache Spark işlerinin hazırlanması nı ve çalıştırılınır.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için aşağıdakileri yapmanız gerekir.

* Kubernetes ve [Apache Spark][spark-quickstart]temel anlayış .
* [Docker Hub][docker-hub] hesabı veya Azure [Kapsayıcı Kayıt Defteri.][acr-create]
* Azure CLI geliştirme sisteminize [yüklendi.][azure-cli]
* [JDK 8][java-install] sisteminizde yüklü.
* Sisteminizde SBT ([Scala Build Tool][sbt-install]) yüklendi.
* Sisteminizde yüklü komut satırı araçları git.

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Kıvılcım büyük ölçekli veri işleme için kullanılır ve Kubernetes düğümlerinin Spark kaynakları gereksinimlerini karşılamak için boyutlandırılmasını gerektirir. Azure Kubernetes `Standard_D3_v2` Hizmetiniz (AKS) düğümleriniz için minimum boyut öneririz.

Bu minimum öneriyi karşılayan bir AKS kümesine ihtiyacınız varsa, aşağıdaki komutları çalıştırın.

Küme için bir kaynak grubu oluşturun.

```azurecli
az group create --name mySparkCluster --location eastus
```

Küme için bir Hizmet Sorumlusu oluşturun. Oluşturulduktan sonra, bir sonraki komut için Servis Sorumlusu appId ve parolagerekir.

```azurecli
az ad sp create-for-rbac --name SparkSP
```

Hizmet-asıl ve istemci-gizli parametreler olarak `Standard_D3_v2`geçirilen appId ve parola değerleri ve boyutu ve değerleri ile AKS kümesi oluşturun.

```azurecli
az aks create --resource-group mySparkCluster --name mySparkCluster --node-vm-size Standard_D3_v2 --generate-ssh-keys --service-principal <APPID> --client-secret <PASSWORD>
```

AKS kümesine bağlanın.

```azurecli
az aks get-credentials --resource-group mySparkCluster --name mySparkCluster
```

Kapsayıcı görüntülerini depolamak için Azure Kapsayıcı Kayıt Defteri'ni (ACR) kullanıyorsanız, AKS ve ACR arasında kimlik doğrulamasını yapılandırın. Bu adımlar için [ACR kimlik doğrulama belgelerine][acr-aks] bakın.

## <a name="build-the-spark-source"></a>Kıvılcım kaynağını oluşturun

Bir AKS kümesinde Kıvılcım işleri çalıştırmadan önce, Spark kaynak kodunu oluşturmanız ve bir kapsayıcı görüntüsüne paketlemeniz gerekir. Kıvılcım kaynağı, bu işlemi tamamlamak için kullanılabilecek komut dosyaları içerir.

Spark proje deposunu geliştirme sisteminize kopyalayın.

```bash
git clone -b branch-2.4 https://github.com/apache/spark
```

Klonlanan deponun dizinine değiştirin ve Spark kaynağının yolunu bir değişkene kaydedin.

```bash
cd spark
sparkdir=$(pwd)
```

Birden çok JDK sürümünüz yüklüyse, geçerli oturum için sürüm 8'i kullanacak şekilde ayarlayın. `JAVA_HOME`

```bash
export JAVA_HOME=`/usr/libexec/java_home -d 64 -v "1.8*"`
```

Kubernetes desteğiyle Spark kaynak kodunu oluşturmak için aşağıdaki komutu çalıştırın.

```bash
./build/mvn -Pkubernetes -DskipTests clean package
```

Aşağıdaki komutlar Spark kapsayıcı görüntüsünü oluşturur ve bir kapsayıcı görüntü kayıt defterine iter. Konteyner `registry.example.com` kayıt defterinizin adı `v1` ve kullanmayı tercih ettiğiniz etiketle değiştirin. Docker Hub kullanıyorsanız, bu değer kayıt defteri adıdır. Azure Kapsayıcı Kayıt Defteri (ACR) kullanıyorsanız, bu değer ACR giriş sunucusu adıdır.

```bash
REGISTRY_NAME=registry.example.com
REGISTRY_TAG=v1
```

```bash
./bin/docker-image-tool.sh -r $REGISTRY_NAME -t $REGISTRY_TAG build
```

Kapsayıcı görüntüsünü kapsayıcı görüntü kayıt defterinize itin.

```bash
./bin/docker-image-tool.sh -r $REGISTRY_NAME -t $REGISTRY_TAG push
```

## <a name="prepare-a-spark-job"></a>Bir Kıvılcım işi hazırlayın

Sonra, bir Kıvılcım iş hazırlayın. Bir kavanoz dosyası Spark iş tutmak için kullanılır `spark-submit` ve komutu çalıştırırken gereklidir. Kavanoz, genel bir URL üzerinden erişilebilir hale getirilebilir veya bir konteyner görüntüsü içinde önceden paketlenmiş olabilir. Bu örnekte, Pi değerini hesaplamak için bir örnek kavanoz oluşturulur. Bu kavanoz daha sonra Azure depolama alanına yüklenir. Mevcut bir kavanozunuzun varsa,

Bir Kıvılcım işi için proje oluşturmak istediğiniz bir dizin oluşturun.

```bash
mkdir myprojects
cd myprojects
```

Şablondan yeni bir Scala projesi oluşturun.

```bash
sbt new sbt/scala-seed.g8
```

İstendiğinde, `SparkPi` proje adı için girin.

```bash
name [Scala Seed Project]: SparkPi
```

Yeni oluşturulan proje dizinine gidin.

```bash
cd sparkpi
```

Projeyi kavanoz dosyası olarak paketlemeye olanak tanıyan bir SBT eklentisi eklemek için aşağıdaki komutları çalıştırın.

```bash
touch project/assembly.sbt
echo 'addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.14.10")' >> project/assembly.sbt
```

Örnek kodu yeni oluşturulan projeye kopyalamak ve gerekli tüm bağımlılıkları eklemek için bu komutları çalıştırın.

```bash
EXAMPLESDIR="src/main/scala/org/apache/spark/examples"
mkdir -p $EXAMPLESDIR
cp $sparkdir/examples/$EXAMPLESDIR/SparkPi.scala $EXAMPLESDIR/SparkPi.scala

cat <<EOT >> build.sbt
// https://mvnrepository.com/artifact/org.apache.spark/spark-sql
libraryDependencies += "org.apache.spark" %% "spark-sql" % "2.3.0" % "provided"
EOT

sed -ie 's/scalaVersion.*/scalaVersion := "2.11.11"/' build.sbt
sed -ie 's/name.*/name := "SparkPi",/' build.sbt
```

Projeyi bir kavanoza paketlemek için aşağıdaki komutu çalıştırın.

```bash
sbt assembly
```

Başarılı bir şekilde paketledikten sonra, aşağıdakilere benzer çıktı görmeniz gerekir.

```bash
[info] Packaging /Users/me/myprojects/sparkpi/target/scala-2.11/SparkPi-assembly-0.1.0-SNAPSHOT.jar ...
[info] Done packaging.
[success] Total time: 10 s, completed Mar 6, 2018 11:07:54 AM
```

## <a name="copy-job-to-storage"></a>İşi depolamaya kopyalama

Kavanoz dosyasını tutmak için bir Azure depolama hesabı ve kapsayıcı oluşturun.

```azurecli
RESOURCE_GROUP=sparkdemo
STORAGE_ACCT=sparkdemo$RANDOM
az group create --name $RESOURCE_GROUP --location eastus
az storage account create --resource-group $RESOURCE_GROUP --name $STORAGE_ACCT --sku Standard_LRS
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string --resource-group $RESOURCE_GROUP --name $STORAGE_ACCT -o tsv`
```

Aşağıdaki komutlarla kavanoz dosyasını Azure depolama hesabına yükleyin.

```azurecli
CONTAINER_NAME=jars
BLOB_NAME=SparkPi-assembly-0.1.0-SNAPSHOT.jar
FILE_TO_UPLOAD=target/scala-2.11/SparkPi-assembly-0.1.0-SNAPSHOT.jar

echo "Creating the container..."
az storage container create --name $CONTAINER_NAME
az storage container set-permission --name $CONTAINER_NAME --public-access blob

echo "Uploading the file..."
az storage blob upload --container-name $CONTAINER_NAME --file $FILE_TO_UPLOAD --name $BLOB_NAME

jarUrl=$(az storage blob url --container-name $CONTAINER_NAME --name $BLOB_NAME | tr -d '"')
```

Değişken `jarUrl` şimdi kavanoz dosyasına genel olarak erişilebilir yolu içerir.

## <a name="submit-a-spark-job"></a>Bir Kıvılcım işi gönderme

Kube-proxy'yi aşağıdaki kodla ayrı bir komut satırında başlatın.

```bash
kubectl proxy
```

Spark deposunun köküne geri gidin.

```bash
cd $sparkdir
```

Bir işi çalıştırmak için yeterli izinlere sahip bir hizmet hesabı oluşturun.

```bash
kubectl create serviceaccount spark
kubectl create clusterrolebinding spark-role --clusterrole=edit --serviceaccount=default:spark --namespace=default
```

İşi kullanarak `spark-submit`gönder.

```bash
./bin/spark-submit \
  --master k8s://http://127.0.0.1:8001 \
  --deploy-mode cluster \
  --name spark-pi \
  --class org.apache.spark.examples.SparkPi \
  --conf spark.executor.instances=3 \
  --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark \
  --conf spark.kubernetes.container.image=$REGISTRY_NAME/spark:$REGISTRY_TAG \
  $jarUrl
```

Bu işlem, kabuk oturumunuza iş durumunu aktaran Spark işini başlatır. İş çalışırken, kubectl get pods komutunu kullanarak Spark sürücü bölmesini ve yürütme bölmelerini görebilirsiniz. Bu komutları çalıştırmak için ikinci bir terminal oturumu açın.

```console
kubectl get pods
```

```output
NAME                                               READY     STATUS     RESTARTS   AGE
spark-pi-2232778d0f663768ab27edc35cb73040-driver   1/1       Running    0          16s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-1   0/1       Init:0/1   0          4s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-2   0/1       Init:0/1   0          4s
spark-pi-2232778d0f663768ab27edc35cb73040-exec-3   0/1       Init:0/1   0          4s
```

İş çalışırken, Spark UI'ye de erişebilirsiniz. İkinci terminal oturumunda, Spark UI'ye erişim sağlayan komutu `kubectl port-forward` kullanın.

```bash
kubectl port-forward spark-pi-2232778d0f663768ab27edc35cb73040-driver 4040:4040
```

Spark UI'ye erişmek `127.0.0.1:4040` için adresi bir tarayıcıda açın.

![Kıvılcım UI](media/aks-spark-job/spark-ui.png)

## <a name="get-job-results-and-logs"></a>İş sonuçları ve günlükleri alın

İş bittikten sonra, sürücü bölmesi "Tamamlanmış" durumda olacaktır. Aşağıdaki komutla bölmenin adını alın.

```bash
kubectl get pods --show-all
```

Çıktı:

```output
NAME                                               READY     STATUS      RESTARTS   AGE
spark-pi-2232778d0f663768ab27edc35cb73040-driver   0/1       Completed   0          1m
```

Kıvılcım `kubectl logs` sürücü bölmesinden günlükleri almak için komutu kullanın. Pod adını sürücü bölmenizin adıyla değiştirin.

```bash
kubectl logs spark-pi-2232778d0f663768ab27edc35cb73040-driver
```

Bu günlükler içinde, Pi değeri olan Kıvılcım işinin sonucunu görebilirsiniz.

```output
Pi is roughly 3.152155760778804
```

## <a name="package-jar-with-container-image"></a>Konteyner görüntüsü ile paket kavanoz

Yukarıdaki örnekte, Spark kavanoz dosyası Azure depolamasına yüklendi. Başka bir seçenek özel olarak üretilmiştir Docker görüntüleri içine kavanoz dosyası paketi için.

Bunu yapmak için, `dockerfile` `$sparkdir/resource-managers/kubernetes/docker/src/main/dockerfiles/spark/` dizinde bulunan Spark görüntüsünü bulun. Spark `ADD` işi `jar` için deyimini `ENTRYPOINT` ve beyanlar arasında bir `WORKDIR` yer ekleyin.

Jar yolunu geliştirme sisteminizdeki `SparkPi-assembly-0.1.0-SNAPSHOT.jar` dosyanın konumuna güncelleştirin. Ayrıca kendi özel kavanoz dosyanızı kullanabilirsiniz.

```bash
WORKDIR /opt/spark/work-dir

ADD /path/to/SparkPi-assembly-0.1.0-SNAPSHOT.jar SparkPi-assembly-0.1.0-SNAPSHOT.jar

ENTRYPOINT [ "/opt/entrypoint.sh" ]
```

Görüntüyü, birlikte verilen Spark komut dosyalarıyla oluşturun ve itin.

```bash
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> build
./bin/docker-image-tool.sh -r <your container repository name> -t <tag> push
```

İşi çalıştırırken, uzak bir kavanoz URL'si belirtmek yerine, `local://` şema Docker görüntüsündeki kavanoz dosyasına giden yol ile kullanılabilir.

```bash
./bin/spark-submit \
    --master k8s://https://<k8s-apiserver-host>:<k8s-apiserver-port> \
    --deploy-mode cluster \
    --name spark-pi \
    --class org.apache.spark.examples.SparkPi \
    --conf spark.executor.instances=3 \
    --conf spark.kubernetes.authenticate.driver.serviceAccountName=spark \
    --conf spark.kubernetes.container.image=<spark-image> \
    local:///opt/spark/work-dir/<your-jar-name>.jar
```

> [!WARNING]
> Spark [belgelerinden][spark-docs]: "Kubernetes zamanlayıcısı şu anda deneysel. Gelecek sürümlerde yapılandırma, kapsayıcı görüntüleri ve giriş noktaları etrafında davranış değişiklikleri olabilir".

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla ayrıntı için Spark belgelerine göz atın.

> [!div class="nextstepaction"]
> [Belgeleri kıvılcımla][spark-docs]

<!-- LINKS - external -->
[apache-spark]: https://spark.apache.org/
[docker-hub]: https://docs.docker.com/docker-hub/
[java-install]: https://aka.ms/azure-jdks
[sbt-install]: https://www.scala-sbt.org/1.0/docs/Setup.html
[spark-docs]: https://spark.apache.org/docs/latest/running-on-kubernetes.html
[spark-latest-release]: https://spark.apache.org/releases/spark-release-2-3-0.html
[spark-quickstart]: https://spark.apache.org/docs/latest/quick-start.html


<!-- LINKS - internal -->
[acr-aks]: cluster-container-registry-integration.md
[acr-create]: https://docs.microsoft.com/azure/container-registry/container-registry-get-started-azure-cli
[aks-quickstart]: https://docs.microsoft.com/azure/aks/
[azure-cli]: https://docs.microsoft.com/cli/azure/?view=azure-cli-latest
[storage-account]: https://docs.microsoft.com/azure/storage/common/storage-azure-cli

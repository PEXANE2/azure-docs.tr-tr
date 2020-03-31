---
title: HDInsight'ta Kafka ile Azure Kubernetes Hizmetini Kullanma
description: Azure Kubernetes Hizmeti'nde (AKS) barındırılan kapsayıcı görüntülerinden HDInsight'ta Kafka'yı nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/04/2019
ms.openlocfilehash: 6abb4f632535f1bda7e9f337f111ba372a624f2b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80239612"
---
# <a name="use-azure-kubernetes-service-with-apache-kafka-on-hdinsight"></a>HDInsight'ta Apache Kafka ile Azure Kubernetes Hizmetini Kullanma

HDInsight kümesinde [Apache Kafka](https://kafka.apache.org/) ile Azure Kubernetes Hizmetini (AKS) nasıl kullanacağınızı öğrenin. Bu belgedeki adımlar, Kafka ile bağlantıyı doğrulamak için AKS'de barındırılan bir Düğüm.js uygulamasını kullanır. Bu uygulama Kafka ile iletişim kurmak için [kafka-düğüm paketini](https://www.npmjs.com/package/kafka-node) kullanır. Tarayıcı istemcisi ile AKS'de barındırılan arka uç arasındaki olay odaklı mesajlaşma için [Socket.io](https://socket.io/) kullanır.

[Apache Kafka](https://kafka.apache.org), gerçek zamanlı akış verisi işlem hatları ve uygulamaları oluşturmak için kullanılabilen, açık kaynak dağıtılmış akış platformudur. Azure Kubernetes Hizmeti, barındırılan Kubernetes ortamınızı yönetir ve kapsayıcı uygulamaları dağıtmayı hızlı ve kolay hale getirir. Azure Sanal Ağı'nı kullanarak iki hizmeti bağlayabilirsiniz.

> [!NOTE]  
> Bu belgenin odak noktası, Azure Kubernetes Hizmeti'nin HDInsight'ta Kafka ile iletişim kurabilmesi için gereken adımlardır. Örneğin kendisi yapılandırma nın çalıştığını göstermek için sadece temel bir Kafka istemcisidir.

## <a name="prerequisites"></a>Ön koşullar

* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* Bir Azure aboneliği

Bu belge, aşağıdaki Azure hizmetlerini oluşturmaya ve kullanmaya aşina olduğunuzu varsayar:

* HDInsight üzerinde Kafka
* Azure Kubernetes Service
* Azure Sanal Ağları

Bu belge ayrıca [Azure Kubernetes Hizmeti öğreticisini](../../aks/tutorial-kubernetes-prepare-app.md)gözden geçirdiğinizi de varsayar. Bu makalede bir kapsayıcı hizmeti oluşturur, bir Kubernetes kümesi, bir kapsayıcı `kubectl` kayıt defteri oluşturur ve yardımcı programı yapılandırır.

## <a name="architecture"></a>Mimari

### <a name="network-topology"></a>Ağ topolojisi

Hem HDInsight hem de AKS, bilgi işlem kaynakları için kapsayıcı olarak bir Azure Sanal Ağı kullanır. HDInsight ve AKS arasındaki iletişimi etkinleştirmek için ağları arasındaki iletişimi etkinleştirmeniz gerekir. Bu belgedeki adımlar, Sanal Ağ Ağlarına Bakma'yı kullanır. VPN gibi diğer bağlantılar da çalışmalıdır. Eşleme hakkında daha fazla bilgi için [Sanal ağ eşleme](../../virtual-network/virtual-network-peering-overview.md) belgesine bakın.

Aşağıdaki diyagram, bu belgede kullanılan ağ topolojisini göstermektedir:

![Bir sanal ağda HDInsight, diğerinde AKS,](./media/apache-kafka-azure-container-services/kafka-aks-architecture.png)

> [!IMPORTANT]  
> Eşlenen ağlar arasında ad çözümlemesi etkinleştirildiğinden, IP adresi kullanılır. Varsayılan olarak, HDInsight'taki Kafka, istemciler bağlandığında IP adresleri yerine ana bilgisayar adlarını döndürecek şekilde yapılandırılır. Bu belgedeki adımlar Kafka'yı IP reklamcılığı yerine kullanmak üzere değiştirir.

## <a name="create-an-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti (AKS) Oluşturma

Zaten bir AKS kümeniz yoksa, nasıl oluşturulabileceğinizi öğrenmek için aşağıdaki belgelerden birini kullanın:

* [Azure Kubernetes Hizmeti (AKS) kümesini dağıtma - Portal](../../aks/kubernetes-walkthrough-portal.md)
* [Azure Kubernetes Hizmeti (AKS) kümesini dağıtma - CLI](../../aks/kubernetes-walkthrough.md)

> [!IMPORTANT]  
> **AKS, ek** bir kaynak grubunda yükleme sırasında sanal bir ağ oluşturur. Ek kaynak **grubu, MC_resourceGroup_AKSclusterName_location**adlandırma kuralını izler.  
> Bu ağ, bir sonraki bölümde HDInsight için oluşturulan bir bakılır.

## <a name="configure-virtual-network-peering"></a>Sanal ağ eşleme yapılandırma

### <a name="identify-preliminary-information"></a>Ön bilgileri belirleme

1. Azure [portalından,](https://portal.azure.com)AKS kümeniz için sanal ağı içeren ek **Kaynak grubunu** bulun.

2. Kaynak grubundan Sanal __ağ__ kaynağını seçin. Adı daha sonra kullanmak için not edin.

3. **Ayarlar**altında __Adres alanını__seçin. Listelenen adres alanına dikkat edin.

### <a name="create-virtual-network"></a>Sanal ağ oluşturma

1. HDInsight için sanal bir ağ oluşturmak için +__Networking__ > __Sanal ağ__ağı __oluştur' a__ > gidin.

1. Belirli özellikler için aşağıdaki yönergeleri kullanarak ağı oluşturun:

    |Özellik | Değer |
    |---|---|
    |Adres alanı|AKS küme ağı tarafından kullanılan adresle çakışmayan bir adres alanı kullanmanız gerekir.|
    |Konum|AKS kümesi için kullandığınız sanal ağ için aynı __Konumu__ kullanın.|

1. Bir sonraki adıma geçmeden önce sanal ağ oluşturulana kadar bekleyin.

### <a name="configure-peering"></a>Eşlemeyi yapılandırma

1. HDInsight ağı ile AKS küme ağı arasındaki eşlemi yapılandırmak için sanal ağı seçin ve ardından __Peerings'i__seçin.

1. Formu doldurmak için + __Ekle__ ve aşağıdaki değerleri kullanın:

    |Özellik |Değer |
    |---|---|
    |Bu VN> \<uzak sanal ağa bakan adı|Bu bakan yapılandırma için benzersiz bir ad girin.|
    |Sanal ağ|**AKS kümesi**için sanal ağı seçin.|
    |\<AKS VN> \<bu VN>|Benzersiz bir ad girin.|

    Diğer tüm alanları varsayılan değerde bırakın ve ardından eşlemi yapılandırmak için __Tamam'ı__ seçin.

## <a name="create-apache-kafka-cluster-on-hdinsight"></a>HDInsight'ta Apache Kafka kümesi ni oluştur

HDInsight kümesinde Kafka'yı oluştururken, HDInsight için daha önce oluşturulan sanal ağa katılmanız gerekir. Kafka kümesi oluşturma hakkında daha fazla bilgi için [Apache Kafka küme](apache-kafka-get-started.md) oluştur belgesine bakın.

## <a name="configure-apache-kafka-ip-advertising"></a>Apache Kafka IP Reklamını Yapılandır

Kafka'yı etki alanı adları yerine IP adreslerinin reklamını yapacak şekilde yapılandırmak için aşağıdaki adımları kullanın:

1. Web tarayıcısını kullanarak `https://CLUSTERNAME.azurehdinsight.net` adresine gidin. CLUSTERNAME'yi HDInsight kümesinde Kafka adı ile değiştirin.

    İstendiğinde, küme için HTTPS kullanıcı adı ve parolasını kullanın. Kümeiçin Ambari Web UI görüntülenir.

2. Kafka hakkındaki bilgileri görüntülemek için soldaki listeden __Kafka'yı__ seçin.

    ![Kafka ile servis listesi vurgulandı](./media/apache-kafka-azure-container-services/select-kafka-service.png)

3. Kafka yapılandırmasını görüntülemek için, üst ten __Configs'i__ seçin.

    ![Apache Ambari hizmetleri yapılandırması](./media/apache-kafka-azure-container-services/select-kafka-config1.png)

4. __Kafka-env__ yapılandırmasını bulmak `kafka-env` için sağ üstteki __Filtre__ alanına girin.

    ![Kafka konfigürasyonu, kafka-env için](./media/apache-kafka-azure-container-services/search-for-kafka-env.png)

5. Kafka'yı IP adreslerinin reklamını yapacak şekilde yapılandırmak için __kafka-env şablonu__ alanının altına aşağıdaki metni ekleyin:

    ```bash
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Kafka'nın dinlediği arayüzü yapılandırmak `listeners` için sağ üstteki __Filtre__ alanına girin.

7. Kafka'yı tüm ağ arabirimlerinde dinleyecek şekilde __listeners__ yapılandırmak `PLAINTEXT://0.0.0.0:9092`için dinleyici ler alanındaki değeri .

8. Yapılandırma değişikliklerini kaydetmek için __Kaydet__ düğmesini kullanın. Değişiklikleri açıklayan bir kısa mesaj girin. Değişiklikler kaydedildikten sonra __Tamam'ı__ seçin.

    ![Apache Ambari yapılandırmayı kaydedin](./media/apache-kafka-azure-container-services/save-configuration-button.png)

9. Kafka'yı yeniden başlatırken hataları önlemek __için, Servis Eylemleri__ düğmesini kullanın ve __Bakım Modunu Aç'ı__seçin. Bu işlemi tamamlamak için Tamam'ı seçin.

    ![Servis eylemleri, bakım açma özelliği vurgulanır](./media/apache-kafka-azure-container-services/turn-on-maintenance-mode.png)

10. Kafka'yı yeniden başlatmak için __Yeniden Başlat__ düğmesini kullanın ve __Etkilenen Tümünü Yeniden Başlat'ı__seçin. Yeniden başlat'ı onaylayın ve işlem tamamlandıktan sonra __TAMAM__ düğmesini kullanın.

    ![Etkilenen tüm vurgulanan yeniden başlat düğmesi ile yeniden başlat](./media/apache-kafka-azure-container-services/restart-required-button.png)

11. Bakım modunu devre dışı kalmak için __Servis Eylemleri__ düğmesini kullanın ve __Bakım Modunu Kapat'ı__seçin. Bu işlemi tamamlamak için **Tamam'ı** seçin.

## <a name="test-the-configuration"></a>Yapılandırmayı test edin

Bu noktada Kafka ve Azure Kubernetes Service, gözlenen sanal ağlar üzerinden iletişim halindedir. Bu bağlantıyı test etmek için aşağıdaki adımları kullanın:

1. Test uygulaması tarafından kullanılan bir Kafka konusu oluşturun. Kafka konuları oluşturma hakkında daha fazla bilgi için [Apache Kafka küme](apache-kafka-get-started.md) oluştur belgesine bakın.

2. Örnek uygulamayı 'dan [https://github.com/Blackmist/Kafka-AKS-Test](https://github.com/Blackmist/Kafka-AKS-Test)indirin.

3. Dosyayı `index.js` edin ve aşağıdaki satırları değiştirin:

    * `var topic = 'mytopic'`: `mytopic` Bu uygulamanın kullandığı Kafka konusunun adını değiştirin.
    * `var brokerHost = '176.16.0.13:9092`: `176.16.0.13` Kümeniz için broker ana bilgisayarlarından birinin dahili IP adresiyle değiştirin.

        Kümedeki broker ana bilgisayarlarının (işçi düğümleri) iç IP adresini bulmak için [Apache Ambari REST API](../hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-internal-ip-address-of-cluster-nodes) belgesine bakın. Alan adı ile başlayan girişlerden birinin IP `wn`adresini seçin.

4. Dizindeki `src` bir komut satırından, bağımlılıkları yükleyin ve dağıtım için bir görüntü oluşturmak için Docker'ı kullanın:

    ```bash
    docker build -t kafka-aks-test .
    ```

    > [!NOTE]  
    > Bu uygulamanın gerektirdiği paketler depoya işaretlenir, bu nedenle bunları yüklemek `npm` için yardımcı programı kullanmanız gerekmez.

5. Azure Konteyner Kayıt Defterinizde (ACR) oturum açın ve oturum açma Sunucusu adını bulun:

    ```azurecli
    az acr login --name <acrName>
    az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
    ```

    > [!NOTE]  
    > Azure Kapsayıcı Kayıt Defteri adınızı bilmiyorsanız veya Azure Kubernetes Hizmetiile çalışmak için Azure CLI'yi kullanmaya aşina değilseniz, [AKS öğreticilerine](../../aks/tutorial-kubernetes-prepare-app.md)bakın.

6. Yerel `kafka-aks-test` resmi ACR'nizin giriş Sunucusu ile etiketleyin. Ayrıca, `:v1` resim sürümünü belirtmek için sonuna ekleyin:

    ```bash
    docker tag kafka-aks-test <acrLoginServer>/kafka-aks-test:v1
    ```

7. Görüntüyü kayıt defterine itin:

    ```bash
    docker push <acrLoginServer>/kafka-aks-test:v1
    ```

    Bu işlemin tamamlanması birkaç dakika sürer.

8. Kubernetes bildirim dosyasını`kafka-aks-test.yaml`() `microsoft` düzenlemeve adım 4'te alınan ACR loginServer adı ile değiştirin.

9. Uygulama ayarlarını bildirimden dağıtmak için aşağıdaki komutu kullanın:

    ```bash
    kubectl create -f kafka-aks-test.yaml
    ```

10. Uygulamanın `EXTERNAL-IP` izlenmesi için aşağıdaki komutu kullanın:

    ```bash
    kubectl get service kafka-aks-test --watch
    ```

    Harici bir IP adresi atandıktan sonra, saatten çıkmak için __CTRL + C__

11. Bir web tarayıcısı açın ve hizmetin harici IP adresini girin. Aşağıdaki resme benzer bir sayfaya ulaşırsınız:

    ![Apache Kafka test web sayfası görüntüsü](./media/apache-kafka-azure-container-services/test-web-page-image1.png)

12. Alana metin girin ve sonra __Gönder__ düğmesini seçin. Veriler Kafka'ya gönderildi. Daha sonra uygulamadaki Kafka tüketicisi mesajı okur ve __Kafka bölümünden gelen mesajlara__ ekler.

    > [!WARNING]  
    > İletinin birden çok kopyasını alabilirsiniz. Bu sorun genellikle bağlandıktan sonra tarayıcınızı yenilediğinizde veya uygulamaya birden fazla tarayıcı bağlantısı açtığınızda ortaya çıkar.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight’ta Apache Kafka kullanma hakkında bilgi almak için aşağıdaki bağlantıları kullanın:

* [HDInsight'ta Apache Kafka ile başlayın](apache-kafka-get-started.md)

* [HDInsight'ta Apache Kafka'nın bir kopyasını oluşturmak için MirrorMaker'ı kullanın](apache-kafka-mirroring.md)

* [Apache Storm'u HdInsight'ta Apache Kafka ile kullanın](../hdinsight-apache-storm-with-kafka.md)

* [Apache Spark'ı Apache Kafka ile HDInsight'ta kullanın](../hdinsight-apache-spark-with-kafka.md)

* [Azure Sanal Ağı üzerinden Apache Kafka'ya bağlanın](apache-kafka-connect-vpn-gateway.md)

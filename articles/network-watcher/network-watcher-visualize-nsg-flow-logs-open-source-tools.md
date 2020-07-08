---
title: NSG akış günlüklerini görselleştirme-elastik yığın
titleSuffix: Azure Network Watcher
description: Ağ Izleyicisi ve esnek yığın kullanarak Azure 'da ağ güvenlik grubu akış günlüklerini yönetin ve çözümleyin.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 6d2b2fb55a9c23643bbb778ced047e75871ba7f5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84807677"
---
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a>Açık kaynak araçları kullanarak Azure Ağ İzleyicisi NSG akış günlüklerini görselleştirme

Ağ güvenlik grubu akış günlükleri, ağ güvenlik gruplarında giriş ve çıkış IP trafiğini anlamak için kullanılabilecek bilgiler sağlar. Bu akış günlükleri, her kural için giden ve gelen akışları gösterir, akışın geçerli olduğu NIC (kaynak/hedef IP, kaynak/hedef bağlantı noktası, protokol) ve trafiğe izin verildiyse veya reddedildiyse.

Bu akış günlüklerinin el ile ayrıştırılması ve öngörüleri elde edilmesi zor olabilir. Ancak, bu verileri görselleştirmeye yardımcı olabilecek çeşitli açık kaynaklı araçlar vardır. Bu makalede, bir kibana panosunda akış günlüklerinizi hızlı bir şekilde dizinleyecek ve görselleştirmenize olanak tanıyan esnek yığını kullanarak bu günlükleri görselleştirmeye yönelik bir çözüm sağlanmaktadır.

## <a name="scenario"></a>Senaryo

Bu makalede, esnek yığını kullanarak ağ güvenlik grubu akış günlüklerini görselleştirmenizi sağlayacak bir çözüm ayarlayacağız.  Logstash giriş eklentisi akış günlüklerini, akış günlüklerini içerecek şekilde yapılandırılan depolama blobundan doğrudan elde eder. Daha sonra elastik yığını kullanarak, akış günlüklerinin dizini oluşturulur ve bilgileri görselleştirmek üzere bir kibana panosu oluşturmak için kullanılır.

![senaryo][scenario]

## <a name="steps"></a>Adımlar

### <a name="enable-network-security-group-flow-logging"></a>Ağ güvenlik grubu akış günlüğünü etkinleştir
Bu senaryoda, hesabınızdaki en az bir ağ güvenlik grubunda ağ güvenlik grubu akış günlüğü 'nün etkin olması gerekir. Ağ güvenlik akışı günlüklerinin etkinleştirilmesi hakkındaki yönergeler için, [ağ güvenlik grupları için akış günlüğüne giriş konusuna giriş](network-watcher-nsg-flow-logging-overview.md)olarak aşağıdaki makaleye bakın.

### <a name="set-up-the-elastic-stack"></a>Elastik yığını ayarlama
NSG akış günlüklerini elastik Stack ile bağlayarak, günlüklerimize ilişkin neleri araymamızı, grafiklemenizi, analiz etmenizi ve türetmemize olanak sağlayan bir kibana panosu oluşturuyoruz.

#### <a name="install-elasticsearch"></a>Elaa aramasını yükleme

1. 5,0 ve üzeri sürümler için esnek yığın Java 8 gerektirir. `java -version`Sürümünüzü denetlemek için komutunu çalıştırın. Java yüklü değilse, [Azure-suppored JDKs](https://aka.ms/azure-jdks)ile ilgili belgelere başvurun.
2. Sisteminiz için doğru ikili paketi indirin:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
   sudo dpkg -i elasticsearch-5.2.0.deb
   sudo /etc/init.d/elasticsearch start
   ```

   Diğer yükleme yöntemleri, [Elaun Search yüklemesinde](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html) bulunabilir

3. Şu komutla, Elaun aramasının çalıştığını doğrulayın:

    ```bash
    curl http://127.0.0.1:9200
    ```

    Aşağıdakine benzer bir yanıt görmeniz gerekir:

    ```json
    {
    "name" : "Angela Del Toro",
    "cluster_name" : "elasticsearch",
    "version" : {
        "number" : "5.2.0",
        "build_hash" : "8ff36d139e16f8720f2947ef62c8167a888992fe",
        "build_timestamp" : "2016-01-27T13:32:39Z",
        "build_snapshot" : false,
        "lucene_version" : "6.1.0"
    },
    "tagline" : "You Know, for Search"
    }
    ```

Elastik arama yükleme hakkında daha fazla yönerge için [yükleme yönergelerine](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)bakın.

### <a name="install-logstash"></a>Logstash 'i yükler

1. Logstash 'i yüklemek için aşağıdaki komutları çalıştırın:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
2. Daha sonra Flow günlüklerine erişmek ve bunları ayrıştırmak için Logstash 'i yapılandırmanız gerekir. Şunu kullanarak bir logstash. conf dosyası oluşturun:

    ```bash
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

3. Aşağıdaki içeriği dosyaya ekleyin:

   ```
   input {
      azureblob
        {
            storage_account_name => "mystorageaccount"
            storage_access_key => "VGhpcyBpcyBhIGZha2Uga2V5Lg=="
            container => "insights-logs-networksecuritygroupflowevent"
            codec => "json"
            # Refer https://docs.microsoft.com/azure/network-watcher/network-watcher-read-nsg-flow-logs
            # Typical numbers could be 21/9 or 12/2 depends on the nsg log file types
            file_head_bytes => 12
            file_tail_bytes => 2
            # Enable / tweak these settings when event is too big for codec to handle.
            # break_json_down_policy => "with_head_tail"
            # break_json_batch_count => 2
        }
      }

      filter {
        split { field => "[records]" }
        split { field => "[records][properties][flows]"}
        split { field => "[records][properties][flows][flows]"}
        split { field => "[records][properties][flows][flows][flowTuples]"}

     mutate{
      split => { "[records][resourceId]" => "/"}
      add_field => {"Subscription" => "%{[records][resourceId][2]}"
                    "ResourceGroup" => "%{[records][resourceId][4]}"
                    "NetworkSecurityGroup" => "%{[records][resourceId][8]}"}
      convert => {"Subscription" => "string"}
      convert => {"ResourceGroup" => "string"}
      convert => {"NetworkSecurityGroup" => "string"}
      split => { "[records][properties][flows][flows][flowTuples]" => ","}
      add_field => {
                  "unixtimestamp" => "%{[records][properties][flows][flows][flowTuples][0]}"
                  "srcIp" => "%{[records][properties][flows][flows][flowTuples][1]}"
                  "destIp" => "%{[records][properties][flows][flows][flowTuples][2]}"
                  "srcPort" => "%{[records][properties][flows][flows][flowTuples][3]}"
                  "destPort" => "%{[records][properties][flows][flows][flowTuples][4]}"
                  "protocol" => "%{[records][properties][flows][flows][flowTuples][5]}"
                  "trafficflow" => "%{[records][properties][flows][flows][flowTuples][6]}"
                  "traffic" => "%{[records][properties][flows][flows][flowTuples][7]}"
                  "flowstate" => "%{[records][properties][flows][flows][flowTuples][8]}"
                   "packetsSourceToDest" => "%{[records][properties][flows][flows][flowTuples][9]}"
                   "bytesSentSourceToDest" => "%{[records][properties][flows][flows][flowTuples][10]}"
                   "packetsDestToSource" => "%{[records][properties][flows][flows][flowTuples][11]}"
                   "bytesSentDestToSource" => "%{[records][properties][flows][flows][flowTuples][12]}"
                   }
      convert => {"unixtimestamp" => "integer"}
      convert => {"srcPort" => "integer"}
      convert => {"destPort" => "integer"}        
     }

     date{
       match => ["unixtimestamp" , "UNIX"]
     }
    }
   output {
     stdout { codec => rubydebug }
     elasticsearch {
       hosts => "localhost"
       index => "nsg-flow-logs"
     }
   }  
   ```

Logstash 'i yükleme hakkında daha fazla bilgi için [resmi belgelere](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)bakın.

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a>Azure Blob depolama için Logstash giriş eklentisini yükler

Bu Logstash eklentisi, akış günlüklerine belirlenen depolama hesabından doğrudan erişmenizi sağlayacaktır. Bu eklentiyi yüklemek için, varsayılan Logstash yükleme dizininden (Bu durumda/usr/share/logstash/bin) komutunu çalıştırın:

```bash
logstash-plugin install logstash-input-azureblob
```

Logstash 'i başlatmak için şu komutu çalıştırın:

```bash
sudo /etc/init.d/logstash start
```

Bu eklenti hakkında daha fazla bilgi için [belgelerine](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)bakın.

### <a name="install-kibana"></a>Kibana 'i yükler

1. Kibana yüklemek için aşağıdaki komutları çalıştırın:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
   tar xzvf kibana-5.2.0-linux-x86_64.tar.gz
   ```

2. Kibana çalıştırmak için şu komutları kullanın:

   ```bash
   cd kibana-5.2.0-linux-x86_64/
   ./bin/kibana
   ```

3. Kibana Web arabiriminizi görüntülemek için şuraya gidin`http://localhost:5601`
4. Bu senaryoda, akış günlükleri için kullanılan dizin deseninin "NSG-Flow-Logs" olması gerekir. Logstash. conf dosyanızın "çıktı" bölümünde Dizin modelini değiştirebilirsiniz.
5. Kibana panosunu uzaktan görüntülemek istiyorsanız, **5601 numaralı bağlantı noktasına**erişime izin veren bir gelen NSG kuralı oluşturun.

### <a name="create-a-kibana-dashboard"></a>Kibana panosu oluşturma

Uyarılarınızın eğilimlerini ve ayrıntılarını görüntülemek için örnek bir Pano aşağıdaki resimde gösterilmiştir:

![Şekil 1][1]

[Pano dosyasını](https://aka.ms/networkwatchernsgflowlogdashboard), [görselleştirme dosyasını](https://aka.ms/networkwatchernsgflowlogvisualizations)ve [kayıtlı arama dosyasını](https://aka.ms/networkwatchernsgflowlogsearch)indirin.

Kibana 'ın **Yönetim** sekmesi altında, **kayıtlı nesneler** ' e gidin ve üç dosyayı içeri aktarın. Sonra **Pano** sekmesinden örnek panoyu açabilir ve yükleyebilirsiniz.

Kendi görselleştirmelerinizle ilgili ölçümleriniz doğrultusunda kendi görselleştirmelerinizi ve panolarınızı da oluşturabilirsiniz. Kibana 'ın [resmi belgelerinden](https://www.elastic.co/guide/en/kibana/current/visualize.html)kibana görselleştirmeler oluşturma hakkında daha fazla bilgi edinin.

### <a name="visualize-nsg-flow-logs"></a>NSG akış günlüklerini görselleştirme

Örnek Pano, akış günlüklerinin çeşitli görselleştirmelerini sağlar:

1. Zaman aralığı boyunca akış sayısını gösteren zaman zaman serisi grafiklerde kararına/yönlendirmeye göre akar. Bu görselleştirmelerin zaman ve Aralık birimini düzenleyebilirsiniz. Kararlara göre akışlar, yapılan, izin verme veya reddetme kararlarının oranını gösterir. bu sırada akışlara göre akışlar gelen ve giden trafiğin oranını gösterir. Bu görsellerle zaman içinde trafik eğilimlerini inceleyebilir ve tüm ani veya olağandışı desenleri arayabilirsiniz.

   ![Şekil 2][2]

2. Hedef/kaynak bağlantı noktasına göre akışlar – akış dökümlerinin ilgili bağlantı noktalarına dökümünü gösteren pasta grafikler. Bu görünümle, en sık kullanılan bağlantı noktalarınızı görebilirsiniz. Pasta grafiğinin içindeki belirli bir bağlantı noktasına tıkladığınızda panonun geri kalanı bu bağlantı noktasının akışlarına göre filtreedilir.

   ![figure3][3]

3. Akış sayısı ve en erken günlük zamanı – kaydedilen akış sayısını ve yakalanan en erken günlüğün tarihini gösteren ölçümler.

   ![figure4][4]

4. NSG ve kurala göre akar: her NSG içindeki akışların dağıtımını ve her NSG içinde kuralların dağıtımını gösteren bir çubuk grafiği. Buradan, hangi NSG 'lerin ve kuralların en iyi şekilde oluşturulduğunu görebilirsiniz.

   ![figure5][5]

5. İlk 10 kaynak/hedef IP 'Leri – ilk 10 kaynak ve hedef IP 'Leri gösteren çubuk grafikler. Bu grafikleri, daha fazla veya daha az üst IP gösterecek şekilde ayarlayabilirsiniz. Burada, en yaygın olarak gerçekleşen IP 'Lerin yanı sıra her IP 'ye yönelik trafik kararı (izin verme veya reddetme) görebilirsiniz.

   ![figure6][6]

6. Akış Başlıkları – bu tabloda, her bir akış grubu içinde yer alan bilgiler ve ilgili NGS ve kuralları gösterilmektedir.

   ![figure7][7]

Panonun üst kısmındaki sorgu çubuğunu kullanarak, pano KIMLIĞI, kaynak grupları, kural veya herhangi bir ilgi alanı gibi herhangi bir parametreye göre Pano 'ya filtre uygulayabilirsiniz. Kibana 'in sorguları ve filtreleri hakkında daha fazla bilgi için [resmi belgelere](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html) bakın

## <a name="conclusion"></a>Sonuç

Ağ güvenlik grubu akış günlüklerini elastik Stack ile birleştirerek, ağ trafiğinizi görselleştirmenin güçlü ve özelleştirilebilir bir yoluyla karşılaştık. Bu panolar, ağ trafiğiniz hakkında hızlı bir şekilde Öngörüler elde edebilir ve bunları paylaşabilir ve tüm olası bozukluklar üzerinde araştırma ve araştırma yapmanıza olanak sağlar. Kibana kullanarak, bu panoları uyarlayabilirsiniz ve güvenlik, denetim ve uyumluluk ihtiyaçlarını karşılamak için belirli görselleştirmeler oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Power BI ile NSG akış günlüklerinizi görselleştirmeyi öğrenin [Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)

<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png

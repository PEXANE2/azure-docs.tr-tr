---
title: NSG akış günlüklerini görselleştirin - Elastik Yığın
titleSuffix: Azure Network Watcher
description: Ağ İzleyicisi ve Elastik Yığın kullanarak Azure'daki Ağ Güvenliği Grubu Akış Günlüklerini yönetin ve analiz edin.
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: e567994038fb4f71ef86dc577760ecf4699a0b1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840647"
---
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a>Açık kaynak araçları kullanarak Azure Ağ İzleyicisi NSG akış günlüklerini görselleştirme

Ağ Güvenliği Grubu akış günlükleri, Ağ Güvenlik Grupları'ndaki IP trafiğini anlamak ve çıkış yapmak için kullanılabilecek bilgiler sağlar. Bu akış günlükleri, kural bazında giden ve gelen akışları gösterir, akış için geçerli nic, akış hakkında 5 tuple bilgi (Kaynak/Hedef IP, Kaynak/Hedef Bağlantı Noktası, Protokol) ve trafiğe izin verilip verilmediğini veya reddedilmişse.

Bu akış günlüklerini elle ayrışdırmak ve içgörüler elde etmek zor olabilir. Ancak, bu verileri görselleştirmeye yardımcı olabilecek birkaç açık kaynak aracı vardır. Bu makalede, bir Kibana panosunda hızlı bir şekilde dizine ve akış günlükleri görselleştirmek için izin verecek Elastik Stack kullanarak bu günlükleri görselleştirmek için bir çözüm sağlayacaktır.

> [!Warning]  
> Aşağıdaki adımlar akış günlükleri sürüm 1 ile çalışır. Ayrıntılar için, [ağ güvenlik grupları için akış günlüğe giriş](network-watcher-nsg-flow-logging-overview.md)'e bakın. Aşağıdaki yönergeler, değişiklik yapılmadan günlük dosyalarının sürüm 2 ile çalışmaz.

## <a name="scenario"></a>Senaryo

Bu makalede, Elastik Yığın'ı kullanarak Ağ Güvenlik Grubu akış günlüklerini görselleştirmenize olanak sağlayacak bir çözüm kuracağız.  Logstash giriş eklentisi, akış günlüklerini içeren şekilde yapılandırılan depolama blob'undan doğrudan akış günlüklerini elde eder. Daha sonra, Elastik Yığın kullanılarak, akış günlükleri dizine eklenir ve bilgileri görselleştirmek için bir Kibana panosu oluşturmak için kullanılır.

![senaryo][scenario]

## <a name="steps"></a>Adımlar

### <a name="enable-network-security-group-flow-logging"></a>Ağ Güvenliği Grubu akış günlüğe kaydetmeyi etkinleştirme
Bu senaryo için, hesabınızda en az bir Ağ Güvenlik Grubu'nda Ağ Güvenliği Grubu Akış Günlüğe Kaydetme özelliğine sahip olmalısınız. Ağ Güvenliği Akış Günlüklerini etkinleştirme yle ilgili talimatlar için, Ağ Güvenlik Grupları için akış günlüğü için aşağıdaki [makaleye Giriş'e](network-watcher-nsg-flow-logging-overview.md)bakın.

### <a name="set-up-the-elastic-stack"></a>Elastik Yığını Ayarlama
NSG akış günlüklerini Elastik Yığın'a bağlayarak, günlüklerimizden arama yapmamıza, grafik oluşturmamıza, analiz etmemize ve öngörüler elde etmemize olanak tanıyan bir Kibana panosu oluşturabiliriz.

#### <a name="install-elasticsearch"></a>Elasticsearch'u Yükleyin

1. Sürüm 5.0 ve üzeri Elastik Stack Java 8 gerektirir. Sürümünüzü `java -version` kontrol etmek için komutu çalıştırın. Java yüklü değilseniz, [Azure'un desteklenmiş JDK'leri](https://aka.ms/azure-jdks)üzerindeki belgelere bakın.
2. Sisteminiz için doğru ikili paketi indirin:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
   sudo dpkg -i elasticsearch-5.2.0.deb
   sudo /etc/init.d/elasticsearch start
   ```

   Diğer kurulum yöntemleri [Elasticsearch Kurulum](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html) bulunabilir

3. Elasticsearch'un komutla çalıştığını doğrulayın:

    ```bash
    curl http://127.0.0.1:9200
    ```

    Buna benzer bir yanıt görmeniz gerekir:

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

Elastik arama yükleme hakkında daha fazla bilgi için [Kurulum talimatlarına](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)bakın.

### <a name="install-logstash"></a>Logstash yükle

1. Logstash'ı yüklemek için aşağıdaki komutları çalıştırın:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
2. Daha sonra akış günlüklerine erişmek ve ayrıştırmak için Logstash'ı yapılandırmamız gerekir. Kullanarak bir logstash.conf dosyası oluşturun:

    ```bash
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

3. Dosyaya aşağıdaki içeriği ekleyin:

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

Logstash'ın kurulumu hakkında daha fazla talimat için [resmi belgelere](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)bakın.

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a>Azure blob depolama için Logstash giriş eklentisini yükleme

Bu Logstash eklentisi, akış günlüklerine kendi belirlediği depolama hesabından doğrudan erişmenizi sağlar. Bu eklentiyi yüklemek için varsayılan Logstash yükleme dizininden (bu durumda /usr/share/logstash/bin) komutu çalıştırın:

```bash
logstash-plugin install logstash-input-azureblob
```

Logstash başlatmak için komutu çalıştırın:

```bash
sudo /etc/init.d/logstash start
```

Bu eklenti hakkında daha fazla bilgi için [belgelere](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)bakın.

### <a name="install-kibana"></a>Kibana'yı Yükle

1. Kibana'yı yüklemek için aşağıdaki komutları çalıştırın:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
   tar xzvf kibana-5.2.0-linux-x86_64.tar.gz
   ```

2. Kibana çalıştırmak için komutları kullanın:

   ```bash
   cd kibana-5.2.0-linux-x86_64/
   ./bin/kibana
   ```

3. Kibana web arabiriminizi görüntülemek için`http://localhost:5601`
4. Bu senaryo için akış günlükleri için kullanılan dizin deseni "nsg-akış-günlükleri"dir. Logstash.conf dosyanızın "çıktı" bölümündeki dizin deseni değiştirebilirsiniz.
5. Kibana panosunu uzaktan görüntülemek istiyorsanız, **5601 portuna**erişime izin veren gelen bir NSG kuralı oluşturun.

### <a name="create-a-kibana-dashboard"></a>Kibana panosu oluşturma

Uyarılarınızdaki eğilimleri ve ayrıntıları görüntülemek için örnek bir pano aşağıdaki resimde gösterilir:

![Şekil 1][1]

Pano [dosyasını,](https://aka.ms/networkwatchernsgflowlogdashboard) [görselleştirme dosyasını](https://aka.ms/networkwatchernsgflowlogvisualizations)ve [kaydedilen arama dosyasını](https://aka.ms/networkwatchernsgflowlogsearch)indirin.

Kibana'nın **Yönetim** sekmesi altında, **Kaydedilmiş Nesneler'e** gidin ve üç dosyayı da aktarın. Ardından **Pano** sekmesinden örnek panoyu açıp yükleyebilirsiniz.

Ayrıca, kendi ilgi alanınıza uygun görselleştirmelerinizi ve panolarınızı oluşturabilirsiniz. Kibana'nın [resmi belgelerinden](https://www.elastic.co/guide/en/kibana/current/visualize.html)Kibana görselleştirmeleri oluşturma hakkında daha fazla bilgi edinin.

### <a name="visualize-nsg-flow-logs"></a>NSG akış günlüklerini görselleştirin

Örnek pano akış günlüklerinin çeşitli görselleştirmelerini sağlar:

1. Zaman Içinde Karar/Yöne Göre Akışlar - zaman dilimi boyunca akış sayısını gösteren zaman serisi grafikleri. Bu görsellerin zaman ve yayılma birimini de edinebilirsiniz. Karara göre akışlar alınan kararların izin verme veya reddetme oranını gösterirken, Yönlendirmeye Göre Akışlar gelen ve giden trafiğin oranını gösterir. Bu görseller ile zaman içinde trafik trendlerini inceleyebilir ve herhangi bir ani veya sıradışı desenler için bakabilirsiniz.

   ![şekil2][2]

2. Hedefe/Kaynak Bağlantı Noktasına göre akışlar – ilgili bağlantı noktalarına akışların dökümünü gösteren pasta grafikleri. Bu görünümle en sık kullanılan bağlantı noktalarınızı görebilirsiniz. Pasta grafiği içindeki belirli bir bağlantı noktasını tıklatırsanız, panogeri kalanı o bağlantı noktasının akışlarına filtrelenir.

   ![şekil3][3]

3. Akış Sayısı ve En Erken Günlük Süresi – kaydedilen akış sayısını ve yakalanan en erken günlüğün tarihini gösteren ölçümler.

   ![şekil4][4]

4. NSG ve Kural'a göre akışlar – her NSG içindeki akışların dağılımını ve her NSG içindeki kuralların dağılımını gösteren bir çubuk grafik. Buradan hangi NSG'nin ve kuralların en çok trafik oluşturduğunu görebilirsiniz.

   ![şekil5][5]

5. En iyi 10 Kaynak/Hedef IP'ler – en iyi 10 kaynak ve hedef IP'leri gösteren çubuk grafikler. Bu grafikleri daha fazla veya daha az üst IP'leri gösterecek şekilde ayarlayabilirsiniz. Buradan en sık meydana gelen IP'lerin yanı sıra her IP'ye yönelik trafik kararının (izin verme veya reddetme) alındığını görebilirsiniz.

   ![şekil6][6]

6. Akış Tuples - Bu tablo, her akış tuple içinde yer alan bilgileri yanı sıra karşılık gelen NGS ve kural gösterir.

   ![şekil7][7]

Pano'nun üst kısmındaki sorgu çubuğunu kullanarak, abonelik kimliği, kaynak grupları, kural veya ilgi çekici başka bir değişken gibi akışların herhangi bir parametresini temel alan panoya filtre uygulayabilirsiniz. Kibana'nın sorguları ve filtreleri hakkında daha fazla bilgi için [resmi belgelere](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html) bakın

## <a name="conclusion"></a>Sonuç

Ağ Güvenlik Grubu akış günlüklerini Elastik Yığın ile birleştirerek, ağ trafiğimizi görselleştirmek için güçlü ve özelleştirilebilir bir yol bulduk. Bu panolar, ağ trafiğiniz hakkında hızlı bir şekilde bilgi edinmenize ve paylaşmanıza ve olası anormallikleri filtrelemenize ve araştırmanıza olanak tanır. Kibana'yı kullanarak, bu panoları özelleştirebilir ve güvenlik, denetim ve uyumluluk gereksinimlerini karşılamak için belirli görselleştirmeler oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Power BI ile [Visualize NSG akışları günlüklerini](network-watcher-visualize-nsg-flow-logs-power-bi.md) ziyaret ederek NSG akış günlüklerinizi Power BI ile nasıl görselleştirebilirsiniz öğrenin

<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png

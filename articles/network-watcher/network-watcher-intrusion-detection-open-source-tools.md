---
title: Açık kaynak araçlarıyla ağ izinsiz giriş algılama gerçekleştirin
titleSuffix: Azure Network Watcher
description: Bu makalede, ağ izinsiz giriş algılama gerçekleştirmek için Azure Ağ İzleyicisi ve açık kaynak araçlarının nasıl kullanılacağı açıklanmaktadır
services: network-watcher
documentationcenter: na
author: damendo
ms.assetid: 0f043f08-19e1-4125-98b0-3e335ba69681
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: damendo
ms.openlocfilehash: 781f3788c9001276315a2baed7060450fa00d77a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76845032"
---
# <a name="perform-network-intrusion-detection-with-network-watcher-and-open-source-tools"></a>Ağ İzleyicisi ve açık kaynak araçlarıyla ağ izinsiz giriş algılama gerçekleştirin

Paket yakalamalar, ağ izinsiz giriş algılama sistemlerinin (IDS) uygulanması ve Ağ Güvenliği İzleme (NSM) gerçekleştirmesi için önemli bir bileşendir. Paket yakalamaişlemlerini işleyen ve olası ağ ihlalleri nin ve kötü amaçlı etkinliklerin imzalarını arayan birkaç açık kaynak IDS aracı vardır. Ağ İzleyicisi tarafından sağlanan paket yakalamaları kullanarak, ağınızı zararlı saldırılara veya güvenlik açıklarına karşı analiz edebilirsiniz.

Bu tür açık kaynak araçlardan biri, ağ trafiğini izlemek için kural kümelerini kullanan ve şüpheli olaylar olduğunda uyarıları tetikleyen bir IDS motoru olan Suricata'dır. Suricata çok dişli bir motor sunar, bu da ağ trafiği analizini artan hız ve verimlilikle gerçekleştirebileceği anlamına gelir. Suricata ve yetenekleri hakkında daha fazla bilgi https://suricata-ids.org/için, kendi web sitesini ziyaret edin.

## <a name="scenario"></a>Senaryo

Bu makalede, Ağ İzleyicisi, Suricata ve Elastik Yığın kullanarak ağ izinsiz giriş algılama gerçekleştirmek için ortamınızı nasıl ayarlayınız açıklanmaktadır. Ağ İzleyicisi, ağ izinsiz giriş algılama gerçekleştirmek için kullanılan paket yakalamalarını sağlar. Suricata, paket yakalamaişlemlerini işler ve verilen tehdit kuralına uyan paketlere dayalı uyarıları tetikler. Bu uyarılar yerel makinenizdeki bir günlük dosyasında depolanır. Elastik Yığın'ı kullanarak, Suricata tarafından oluşturulan günlükler dizine eklenebilir ve bir Kibana panosu oluşturmak için kullanılabilir, bu da size günlüklerin görsel bir temsilini ve olası ağ güvenlik açıklarına hızlı bir şekilde bilgi edinmenizi sağlar.  

![basit web uygulama senaryosu][1]

Her iki açık kaynak aracı da azure vm'de ayarlanabilir ve bu çözümlemesi kendi Azure ağ ortamınızda gerçekleştirmenize olanak tanır.

## <a name="steps"></a>Adımlar

### <a name="install-suricata"></a>Suricata'yı Yükleyin

Diğer tüm kurulum yöntemleri için,https://suricata.readthedocs.io/en/latest/install.html

1. VM'nizin komut satırı terminalinde aşağıdaki komutları çalıştırın:

    ```
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo sudo apt-get install suricata
    ```

1. Yüklemenizi doğrulamak için komutların tam listesini görmek için komutu `suricata -h` çalıştırın.

### <a name="download-the-emerging-threats-ruleset"></a>Ortaya Çıkan Tehditler kural kümesini indirin

Bu aşamada, Suricata'nın çalışması için herhangi bir kuralımız yok. Ağınıza yönelik algılamak istediğiniz belirli tehditler varsa kendi kurallarınızı oluşturabilir veya Ortaya Çıkan Tehditler veya Snort'un VRT kuralları gibi bir dizi sağlayıcıdan gelişmiş kural kümelerini de kullanabilirsiniz. Burada serbestçe erişilebilen Emerging Threats kuralını kullanıyoruz:

Kural kümesini indirin ve dizine kopyalayın:

```
wget https://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
tar zxf emerging.rules.tar.gz
sudo cp -r rules /etc/suricata/
```

### <a name="process-packet-captures-with-suricata"></a>Suricata ile işlem paketi yakalama

Ek-lefkoşa kullanarak paket yakalamaişlemlerini işlemek için aşağıdaki komutu çalıştırın:

```
sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
```
Ortaya çıkan uyarıları kontrol etmek için fast.log dosyasını okuyun:
```
tail -f /var/log/suricata/fast.log
```

### <a name="set-up-the-elastic-stack"></a>Elastik Yığını Ayarlama

Suricata'nın ürettiği günlükler amızmızda olup bitenler hakkında değerli bilgiler içerse de, bu günlük dosyalarının okunması ve anlaşılması kolay değildir. Suricata'yı Elastik Yığın'a bağlayarak, günlüklerimizden arama yapmamıza, grafik oluşturmamıza, analiz etmemize ve öngörüler elde etmemize olanak tanıyan bir Kibana panosu oluşturabiliriz.

#### <a name="install-elasticsearch"></a>Elasticsearch'u Yükleyin

1. Sürüm 5.0 ve üzeri Elastik Stack Java 8 gerektirir. Sürümünüzü `java -version` kontrol etmek için komutu çalıştırın. Java yüklü değilseniz, [Azure'un desteklenmiş JDK'leri](https://aka.ms/azure-jdks)üzerindeki belgelere bakın.

1. Sisteminiz için doğru ikili paketi indirin:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    Diğer kurulum yöntemleri [Elasticsearch Kurulum](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html) bulunabilir

1. Elasticsearch'un komutla çalıştığını doğrulayın:

    ```
    curl http://127.0.0.1:9200
    ```

    Buna benzer bir yanıt görmeniz gerekir:

    ```
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

Elastik arama yükleme hakkında daha fazla bilgi için [Yükleme](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html) sayfasına bakın

### <a name="install-logstash"></a>Logstash yükle

1. Logstash'ı yüklemek için aşağıdaki komutları çalıştırın:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. Sonra eve.json dosyasının çıktısını okumak için Logstash yapılandırmak gerekir. Kullanarak bir logstash.conf dosyası oluşturun:

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. Aşağıdaki içeriği dosyaya ekleyin (eve.json dosyasına giden yolun doğru olduğundan emin olun):

    ```ruby
    input {
    file {
        path => ["/var/log/suricata/eve.json"]
        codec =>  "json"
        type => "SuricataIDPS"
    }

    }

    filter {
    if [type] == "SuricataIDPS" {
        date {
        match => [ "timestamp", "ISO8601" ]
        }
        ruby {
        code => "
            if event.get('[event_type]') == 'fileinfo'
            event.set('[fileinfo][type]', event.get('[fileinfo][magic]').to_s.split(',')[0])
            end
        "
        }

        ruby{
        code => "
            if event.get('[event_type]') == 'alert'
            sp = event.get('[alert][signature]').to_s.split(' group ')
            if (sp.length == 2) and /\A\d+\z/.match(sp[1])
                event.set('[alert][signature]', sp[0])
            end
            end
            "
        }
    }

    if [src_ip]  {
        geoip {
        source => "src_ip"
        target => "geoip"
        #database => "/opt/logstash/vendor/geoip/GeoLiteCity.dat"
        add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
        add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
        }
        mutate {
        convert => [ "[geoip][coordinates]", "float" ]
        }
        if ![geoip.ip] {
        if [dest_ip]  {
            geoip {
            source => "dest_ip"
            target => "geoip"
            #database => "/opt/logstash/vendor/geoip/GeoLiteCity.dat"
            add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
            add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
            }
            mutate {
            convert => [ "[geoip][coordinates]", "float" ]
            }
        }
        }
    }
    }

    output {
    elasticsearch {
        hosts => "localhost"
    }
    }
    ```

1. Logstash'ın dosyayı yutabilmesi için eve.json dosyasına doğru izinleri verdiğinizden emin olun.
    
    ```
    sudo chmod 775 /var/log/suricata/eve.json
    ```

1. Logstash başlatmak için komutu çalıştırın:

    ```
    sudo /etc/init.d/logstash start
    ```

Logstash'ın kurulumu hakkında daha fazla talimat [için, resmi belgelere](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html) bakın

### <a name="install-kibana"></a>Kibana'yı Yükle

1. Kibana'yı yüklemek için aşağıdaki komutları çalıştırın:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
    tar xzvf kibana-5.2.0-linux-x86_64.tar.gz

    ```
1. Kibana çalıştırmak için komutları kullanın:

    ```
    cd kibana-5.2.0-linux-x86_64/
    ./bin/kibana
    ```

1. Kibana web arabiriminizi görüntülemek için`http://localhost:5601`
1. Bu senaryo için, Suricata günlükleri için kullanılan dizin deseni "logstash-*"

1. Kibana panosunu uzaktan görüntülemek istiyorsanız, **5601 portuna**erişime izin veren gelen bir NSG kuralı oluşturun.

### <a name="create-a-kibana-dashboard"></a>Kibana panosu oluşturma

Bu makale için, uyarılarınızdaki eğilimleri ve ayrıntıları görüntülemeniz için örnek bir pano sağladık.

1. [Burada](https://aka.ms/networkwatchersuricatadashboard)pano dosyasını indirin , visualization dosyası [burada](https://aka.ms/networkwatchersuricatavisualization), ve burada kayıtlı arama [dosyası](https://aka.ms/networkwatchersuricatasavedsearch).

1. Kibana'nın **Yönetim** sekmesi altında, **Kaydedilmiş Nesneler'e** gidin ve üç dosyayı da aktarın. Ardından **Pano** sekmesinden örnek panoyu açıp yükleyebilirsiniz.

Ayrıca, kendi ilgi alanınıza uygun görselleştirmelerinizi ve panolarınızı oluşturabilirsiniz. Kibana'nın [resmi belgelerinden](https://www.elastic.co/guide/en/kibana/current/visualize.html)Kibana görselleştirmeleri oluşturma hakkında daha fazla bilgi edinin.

![kibana pano][2]

### <a name="visualize-ids-alert-logs"></a>IDS uyarı günlüklerini görselleştirin

Örnek pano, Suricata uyarı günlüklerinin çeşitli görselleştirmelerini sağlar:

1. GeoIP tarafından uyarılar - coğrafi konuma göre (IP tarafından belirlenir) kendi ülke / bölge tarafından uyarıların dağılımını gösteren bir harita

    ![coğrafi ip][3]

1. En Sık 10 Uyarılar - 10 en sık tetiklenen uyarıları ve açıklamaları bir özeti. Tek bir uyarıyı tıklatmak, panoyu bu belirli uyarıyla ilgili bilgilere filtreler.

    ![resim 4][4]

1. Uyarı Sayısı – kural kümesi tarafından tetiklenen uyarıların toplam sayısı

    ![resim 5][5]

1. En iyi 20 Kaynak/Hedef IP/Bağlantı Noktası - uyarıların tetiklenen en iyi 20 IP'yi ve bağlantı noktasını gösteren pasta grafikleri. Kaç tane ve ne tür uyarıların tetiklendiğini görmek için belirli IP'lere/bağlantı noktalarına filtre uygulayabilirsiniz.

    ![resim 6][6]

1. Uyarı Özeti – her bir uyarının belirli ayrıntılarını özetleyen bir tablo. Her uyarı için diğer ilgi çekici parametreleri göstermek için bu tabloyu özelleştirebilirsiniz.

    ![resim 7][7]

Özel görselleştirmeler ve panolar oluşturma yla ilgili daha fazla belge [için, Kibana'nın resmi belgelerine](https://www.elastic.co/guide/en/kibana/current/introduction.html)bakın.

## <a name="conclusion"></a>Sonuç

Network Watcher ve Suricata gibi açık kaynak ids araçları tarafından sağlanan paket yakalamaları birleştirerek, çok çeşitli tehditler için ağ izinsiz giriş algılama gerçekleştirebilirsiniz. Bu panolar, ağınızdaki eğilimleri ve anormallikleri hızlı bir şekilde belirlemenize ve kötü amaçlı kullanıcı aracıları veya savunmasız bağlantı noktaları gibi uyarıların kök nedenlerini bulmak için verileri araştırmanıza olanak tanır. Bu çıkarılan verilerle, ağınıza nasıl tepki vereceğiniz ve ağınızı zararlı saldırı girişimlerinden nasıl koruyacağınız konusunda bilinçli kararlar alabilir ve ağınıza gelecekteki izinsiz girişleri önlemek için kurallar oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Azure İşlevleriyle proaktif ağ izleme yapmak için Paket yakalamayı kullan'ı](network-watcher-alert-triggered-packet-capture.md) ziyaret ederek uyarılara dayalı paket yakalamaları nasıl tetiklediğinizi öğrenin

Power BI ile [Visualize NSG akışları günlüklerini](network-watcher-visualize-nsg-flow-logs-power-bi.md) ziyaret ederek NSG akış günlüklerinizi Power BI ile nasıl görselleştirebilirsiniz öğrenin



<!-- images -->
[1]: ./media/network-watcher-intrusion-detection-open-source-tools/figure1.png
[2]: ./media/network-watcher-intrusion-detection-open-source-tools/figure2.png
[3]: ./media/network-watcher-intrusion-detection-open-source-tools/figure3.png
[4]: ./media/network-watcher-intrusion-detection-open-source-tools/figure4.png
[5]: ./media/network-watcher-intrusion-detection-open-source-tools/figure5.png
[6]: ./media/network-watcher-intrusion-detection-open-source-tools/figure6.png
[7]: ./media/network-watcher-intrusion-detection-open-source-tools/figure7.png

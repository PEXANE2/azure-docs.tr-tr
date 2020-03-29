---
title: Grafana'yı kullanarak NSG Akış Günlüklerini Yönetme
titleSuffix: Azure Network Watcher
description: Ağ İzleyicisi ve Grafana'yı kullanarak Azure'daki Ağ Güvenliği Grubu Akış Günlüklerini yönetin ve analiz edin.
services: network-watcher
documentationcenter: na
author: damendo
tags: azure-resource-manager
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: damendo
ms.openlocfilehash: c48d5a02cdb8ef63904642c6c2c76cb5d61e1f9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76840919"
---
# <a name="manage-and-analyze-network-security-group-flow-logs-using-network-watcher-and-grafana"></a>Ağ İzleyicisi ve Grafana'yı kullanarak Ağ Güvenliği Grubu akış günlüklerini yönetme ve analiz

[Ağ Güvenlik Grubu (NSG) akış günlükleri,](network-watcher-nsg-flow-logging-overview.md) ağ arabirimlerindeki IP trafiğini anlamak ve çıkmak için kullanılabilecek bilgiler sağlar. Bu akış günlükleri, NSG kural bazında giden ve gelen akışları gösterir, akış için geçerli nic, akış hakkında 5-tuple bilgi (Kaynak/Hedef IP, Kaynak/Hedef Bağlantı Noktası, Protokol) ve trafiğe izin verildi yse veya reddedildiyse.

> [!Warning]  
> Aşağıdaki adımlar akış günlükleri sürüm 1 ile çalışır. Ayrıntılar için, [ağ güvenlik grupları için akış günlüğe giriş](network-watcher-nsg-flow-logging-overview.md)'e bakın. Aşağıdaki yönergeler, değişiklik yapılmadan günlük dosyalarının sürüm 2 ile çalışmaz.

Akış günlüğü etkinleştirilmiş ağınızda birçok NSG'niz olabilir. Bu miktarda günlük veriayrıştırmak ve günlüklerinizden öngörüler elde etmek hantal hale getirir. Bu makalede, açık kaynak grafik aracı Grafana, dağıtılmış arama ve analiz motoru ElasticSearch ve açık kaynak sunucu tarafı veri işleme ardışık bir ardışık olan Logstash kullanarak bu NSG akış günlüklerini merkezi olarak yönetmek için bir çözüm sağlar.  

## <a name="scenario"></a>Senaryo

NSG akış günlükleri Network Watcher kullanılarak etkinleştirilir ve Azure blob depolama alanında depolanır. Bir Logstash eklentisi blob depolamadan akış günlükleri bağlamak ve işlemek ve ElasticSearch bunları göndermek için kullanılır.  Akış günlükleri ElasticSearch'te depolandıktan sonra, grafana'da analiz edilebilir ve özelleştirilmiş panolara görselleştirilebilir.

![NSG Ağ İzleyicisi Grafana](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig1.png)

## <a name="installation-steps"></a>Yükleme adımları

### <a name="enable-network-security-group-flow-logging"></a>Ağ Güvenliği Grubu akış günlüğe kaydetmeyi etkinleştirme

Bu senaryo için, hesabınızda en az bir Ağ Güvenlik Grubu'nda Ağ Güvenliği Grubu Akış Günlüğe Kaydetme özelliğine sahip olmalısınız. Ağ Güvenliği Akış Günlüklerini etkinleştirme yle ilgili talimatlar için, Ağ Güvenlik Grupları için akış günlüğü için aşağıdaki [makaleye Giriş'e](network-watcher-nsg-flow-logging-overview.md)bakın.

### <a name="setup-considerations"></a>Kurulumda dikkat edilecek noktalar

Bu örnekte Grafana, ElasticSearch ve Logstash, Azure'da dağıtılan bir Ubuntu 16.04 LTS Server üzerinde yapılandırılmıştır. Bu minimum kurulum üç bileşeni çalıştırmak için kullanılır – hepsi aynı VM üzerinde çalışır. Bu kurulum yalnızca sınama ve kritik olmayan iş yükleri için kullanılmalıdır. Logstash, Elasticsearch ve Grafana birçok örnekte bağımsız olarak ölçeklendirilecek şekilde tasarlanabilir. Daha fazla bilgi için bu bileşenlerin her biri için belgelere bakın.

### <a name="install-logstash"></a>Logstash yükle

JSON biçimlendirilmiş akış günlüklerini akış tuple düzeyine düzleştirmek için Logstash'ı kullanırsınız.

1. Logstash'ı yüklemek için aşağıdaki komutları çalıştırın:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```

2. Akış günlüklerini ayrıştırmak ve ElasticSearch'e göndermek için Logstash'ı yapılandırın. Kullanarak bir Logstash.conf dosyası oluşturun:

    ```bash
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

3. Aşağıdaki içeriği dosyaya ekleyin. Depolama hesabı bilgilerinizi yansıtacak şekilde depolama hesabı adını ve erişim anahtarını değiştirin:

   ```bash
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

      mutate {
        split => { "[records][resourceId]" => "/"}
        add_field => { "Subscription" => "%{[records][resourceId][2]}"
          "ResourceGroup" => "%{[records][resourceId][4]}"
          "NetworkSecurityGroup" => "%{[records][resourceId][8]}" 
        }
        convert => {"Subscription" => "string"}
        convert => {"ResourceGroup" => "string"}
        convert => {"NetworkSecurityGroup" => "string"}
        split => { "[records][properties][flows][flows][flowTuples]" => "," }
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
        add_field => {
          "time" => "%{[records][time]}"
          "systemId" => "%{[records][systemId]}"
          "category" => "%{[records][category]}"
          "resourceId" => "%{[records][resourceId]}"
          "operationName" => "%{[records][operationName}}"
          "Version" => "%{[records][properties][Version}}"
          "rule" => "%{[records][properties][flows][rule]}"
          "mac" => "%{[records][properties][flows][flows][mac]}"
        }
        convert => {"unixtimestamp" => "integer"}
        convert => {"srcPort" => "integer"}
        convert => {"destPort" => "integer"}
        add_field => { "message" => "%{Message}" }        
      }
 
      date {
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

Logstash config dosyası sağlanan üç bölümden oluşur: giriş, filtre ve çıktı.
Giriş bölümü, Logstash'ın işleyecek günlüklerinin giriş kaynağını belirtir – bu durumda blob depolamada depolanan NSG akış günlüğü JSON dosyalarına erişmemizi sağlayacak bir "azureblob" giriş eklentisi (sonraki adımlarda yüklü) kullanacağız. 

Filtre bölümü daha sonra her akış günlüğü dosyasını düzleştirir, böylece her bir akış tuple'ı ve ilişkili özellikleri ayrı bir Logstash olayı haline gelir.

Son olarak, çıkış bölümü her Logstash olayını ElasticSearch sunucusuna iletilir. Logstash config dosyasını özel ihtiyaçlarınıza uyacak şekilde değiştirmekten çekinmeyin.

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a>Azure Blob depolama için Logstash giriş eklentisini yükleme

Bu Logstash eklentisi, akış günlüklerine kendi belirlediği blob depolama hesabından doğrudan erişmenizi sağlar. Bu fişi yüklemek için, varsayılan Logstash yükleme dizininden (bu durumda /usr/share/logstash/bin) komutu çalıştırın:

```bash
cd /usr/share/logstash/bin
sudo ./logstash-plugin install logstash-input-azureblob
```

Bu eklenti hakkında daha fazla bilgi için [Azure Depolama Blobs için Logstash giriş eklentisi'ne](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)bakın.

### <a name="install-elasticsearch"></a>ElasticSearch'u Yükleyin

ElasticSearch'u yüklemek için aşağıdaki komut dosyasını kullanabilirsiniz. ElasticSearch'ün yüklenmesi hakkında daha fazla bilgi için [Elastik Yığın'a](https://www.elastic.co/guide/en/elastic-stack/current/index.html)bakın.

```bash
apt-get install apt-transport-https openjdk-8-jre-headless uuid-runtime pwgen -y
wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | apt-key add -
echo "deb https://packages.elastic.co/elasticsearch/5.x/debian stable main" | tee -a /etc/apt/sources.list.d/elasticsearch-5.x.list
apt-get update && apt-get install elasticsearch
sed -i s/#cluster.name:.*/cluster.name:\ grafana/ /etc/elasticsearch/elasticsearch.yml
systemctl daemon-reload
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
```

### <a name="install-grafana"></a>Grafana'yı yükleyin

Grafana'yı yüklemek ve çalıştırmak için aşağıdaki komutları çalıştırın:

```bash
wget https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_4.5.1_amd64.deb
sudo apt-get install -y adduser libfontconfig
sudo dpkg -i grafana_4.5.1_amd64.deb
sudo service grafana-server start
```

Ek yükleme bilgileri için [Debian / Ubuntu'da yükleme](https://docs.grafana.org/installation/debian/)bölümüne bakın.

#### <a name="add-the-elasticsearch-server-as-a-data-source"></a>Elastik Arama sunucusunu veri kaynağı olarak ekleme

Ardından, akış günlüklerini içeren ElasticSearch dizinini veri kaynağı olarak eklemeniz gerekir. **Veri kaynağı ekle'yi** seçerek ve formu ilgili bilgilerle tamamlayarak bir veri kaynağı ekleyebilirsiniz. Bu yapılandırmanın bir örneği aşağıdaki ekran görüntüsünde bulunabilir:

![Veri kaynağı ekleme](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig2.png)

#### <a name="create-a-dashboard"></a>Pano oluşturma

Grafana'yı NSG akış günlüklerini içeren ElasticSearch dizininden okuyacak şekilde başarıyla yapılandırdığınıza göre, panolar oluşturabilir ve kişiselleştirebilirsiniz. Yeni bir pano oluşturmak için **ilk panonu oluştur'u**seçin. Aşağıdaki örnek grafik yapılandırması NSG kuralına göre bölümlenen akışları gösterir:

![Pano grafiği](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig3.png)

Aşağıdaki ekran görüntüsü, üst akışları ve sıklıklarını gösteren bir grafik ve grafiği gösterir. Akışlar da NSG kuralı ve karar ile akışları tarafından gösterilir. Grafana son derece özelleştirilebilir, bu nedenle özel izleme ihtiyaçlarınıza uygun panolar oluşturmanız tavsiye edilir. Aşağıdaki örnekte tipik bir pano gösterilmektedir:

![Pano grafiği](./media/network-watcher-nsg-grafana/network-watcher-nsg-grafana-fig4.png)

## <a name="conclusion"></a>Sonuç

Network Watcher'ı ElasticSearch ve Grafana ile entegre ederek, artık NSG akış günlüklerini ve diğer verileri yönetmek ve görselleştirmek için kullanışlı ve merkezi bir yol elde elabilirsiniz. Grafana, akış günlüklerini daha fazla yönetmek ve ağ trafiğinizi daha iyi anlamak için de kullanılabilecek diğer güçlü grafik özelliklerine sahiptir. Artık Azure'a ayarlanmış ve bağlı bir Grafana örneğiniz olduğuna göre, sunduğu diğer işlevleri keşfetmeye devam edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Ağ İzleyicisi'ni](network-watcher-monitoring-overview.md)kullanma hakkında daha fazla bilgi edinin.


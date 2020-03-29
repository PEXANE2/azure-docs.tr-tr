---
title: Azure ağ güvenlik grubu akış günlüklerini analiz et - Graylog | Microsoft Dokümanlar
description: Ağ İzleyicisi ve Graylog'u kullanarak Azure'daki ağ güvenliği grubu akış günlüğünü nasıl yönetip analiz edeceğiz öğrenin.
services: network-watcher
documentationcenter: na
author: damendo
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2017
ms.author: damendo
ms.openlocfilehash: 1e597a81967a8fb6be2959d53e65ad01135e5e25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76842912"
---
# <a name="manage-and-analyze-network-security-group-flow-logs-in-azure-using-network-watcher-and-graylog"></a>Ağ İzleyicisi ve Graylog'u kullanarak Azure'daki ağ güvenliği grubu akış günlemelerini yönetme ve analiz

[Ağ güvenliği grubu akış günlükleri,](network-watcher-nsg-flow-logging-overview.md) Azure ağ arabirimleri için ip trafiğini anlamak ve çıkmak için kullanabileceğiniz bilgiler sağlar. Akış günlükleri, ağ güvenlik grubu kuralı temelinde giden ve gelen akışları gösterir, akışın uygulandığı ağ arabirimi, akış la ilgili 5-tuple bilgileri (Kaynak/Hedef IP, Kaynak/Hedef Bağlantı Noktası, Protokol) ve trafiğe izin verilip verilmediğini veya reddedildiği.

Akış günlüğe kaydetme etkinleştirilmiş ağınızda birçok ağ güvenlik grubu olabilir. Akış günlüğü etkin olan çeşitli ağ güvenlik grupları ayrıştırmayı ve günlüklerinizden öngörüler elde etmeyi hantal hale getirebilir. Bu makalede, açık kaynak günlük yönetimi ve çözümleme aracı graylog ve açık kaynak sunucu tarafı veri işleme ardışık ardışık bir kanal kullanarak bu ağ güvenlik grubu akış günlüklerini merkezi olarak yönetmek için bir çözüm sağlar.

> [!Warning]
> Aşağıdaki adımlar akış günlükleri sürüm 1 ile çalışır. Ayrıntılar için, [ağ güvenlik grupları için akış günlüğe giriş](network-watcher-nsg-flow-logging-overview.md)'e bakın. Aşağıdaki yönergeler, değişiklik yapılmadan günlük dosyalarının sürüm 2 ile çalışmaz.

## <a name="scenario"></a>Senaryo

Ağ güvenlik grubu akış günlükleri Ağ İzleyicisi kullanılarak etkinleştirilir. Akış günlükleri Azure blob depolama akışı. Bir Logstash eklentisi blob depolamadan akış günlüklerini bağlamak ve işlemek ve bunları Graylog'a göndermek için kullanılır. Akış günlükleri Graylog'da depolandıktan sonra, analiz edilebilir ve özelleştirilmiş panolarda görselleştirilebilir.

![Graylog iş akışı](./media/network-watcher-analyze-nsg-flow-logs-graylog/workflow.png)

## <a name="installation-steps"></a>Kurulum Adımları

### <a name="enable-network-security-group-flow-logging"></a>Ağ güvenliği grubu akış günlüğe kaydetmeyi etkinleştirme

Bu senaryo için, hesabınızdaki en az bir ağ güvenlik grubunda ağ güvenliği grubu günlüğe kaydetmeniz gerekir. Ağ güvenliği grubu akış günlüklerini etkinleştirme yönergeleri için aşağıdaki makaleye bakın [Ağ güvenlik grupları için akış günlüğe giriş.](network-watcher-nsg-flow-logging-overview.md)

### <a name="setting-up-graylog"></a>Graylog'u ayarlama

Bu örnekte, hem Graylog hem de Logstash, Azure'da dağıtılan bir Ubuntu 14.04 Sunucusunda yapılandırılmıştır.

- Ubuntu'ya nasıl yüklenacağına ilişkin adım adım talimatlar için Graylog'daki [belgelere](https://docs.graylog.org/en/2.2/pages/installation/os/ubuntu.html) bakın.
- [Ayrıca belgeleri](https://docs.graylog.org/en/2.2/pages/configuration/web_interface.html#configuring-webif)izleyerek Graylog web arabirimini yapılandırdığından emin olun.

Bu örnek, minimum Graylog kurulumını kullanır (örn. Graylog'un tek bir örneği), ancak Graylog, sisteminize ve üretim gereksinimlerinize bağlı olarak kaynaklar arasında ölçeklendirilecek şekilde tasarlanabilir. Mimari hususlar veya derin bir mimari rehber hakkında daha fazla bilgi için Graylog'un [dokümantasyonu](https://docs.graylog.org/en/2.2/pages/architecture.html) ve [mimari kılavuzuna](https://www.slideshare.net/Graylog/graylog-engineering-design-your-architecture)bakın.

Graylog, platformunuza ve tercihlerinize bağlı olarak birçok şekilde yüklenebilir. Olası yükleme yöntemlerinin tam listesi için Graylog'un resmi [belgelerine](https://docs.graylog.org/en/2.2/pages/installation.html)bakın. Graylog sunucu uygulaması Linux dağıtımları üzerinde çalışır ve aşağıdaki ön koşullara sahiptir:

-  Java SE 8 veya sonrası – [Azul Azure JDK belgeleri](https://aka.ms/azure-jdks)
-  Elastik Arama 2.x (2.1.0 veya sonrası) - [Elasticsearch kurulum belgeleri](https://www.elastic.co/guide/en/elasticsearch/reference/2.4/_installation.html)
-  MongoDB 2.4 veya sonrası – [MongoDB kurulum belgeleri](https://docs.mongodb.com/manual/administration/install-on-linux/)

### <a name="install-logstash"></a>Logstash yükle

Logstash, JSON biçimlendirilmiş akış günlüklerini akış tuple düzeyine düzleştirmek için kullanılır. Akış günlüklerini düzleştirmek, günlüklerin Graylog'da düzenlenmesini ve aranmasını kolaylaştırır.

1. Logstash'ı yüklemek için aşağıdaki komutları çalıştırın:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
   sudo dpkg -i logstash-5.2.0.deb
   ```

2. Akış günlüklerini ayrıştırmak ve Graylog'a göndermek için Logstash'ı yapılandırın. Logstash.conf dosyası oluşturun:

   ```bash
   sudo touch /etc/logstash/conf.d/logstash.conf
   ```

3. Aşağıdaki içeriği dosyaya ekleyin. Vurgulanan değerleri depolama hesabı ayrıntılarınızı yansıtacak şekilde değiştirin:

   ```
    input {
        azureblob
        {
            storage_account_name => "mystorageaccount"
            storage_access_key => "NrUZmx7pJSKaRJzvQbeiZWi5nBRWOTr7Wwr9DrvK7YtDBrADYxT1y0oEExtSlkDnGRt7qcRiZzEBCCyRYND8SxSt"
            container => "insights-logs-networksecuritygroupflowevent"
            registry_create_policy => "start_over"
            codec => "json"
            file_head_bytes => 21
            file_tail_bytes => 9
            # Possible options: `do_not_break`, `with_head_tail`, `without_head_tail`
            break_json_down_policy  => 'with_head_tail'
            break_json_batch_count => 2
            interval => 5
        }
    }
    
    filter {
        split { field => "[records]" }
        split { field => "[records][properties][flows]"}
        split { field => "[records][properties][flows][flows]"}
        split { field => "[records][properties][flows][flows][flowTuples]"
    }
    
     mutate {
        split => { "[records][resourceId]" => "/"}
        add_field =>{
                    "Subscription" => "%{[records][resourceId][2]}"
                    "ResourceGroup" => "%{[records][resourceId][4]}"
                    "NetworkSecurityGroup" => "%{[records][resourceId][8]}"
        }
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
        udp {
            host => "127.0.0.1"
            port => 12201
        }
    }
    ```
   Logstash config dosyası sağlanan üç bölümden oluşur: giriş, filtre ve çıktı. Giriş bölümü, Logstash'ın işleyecek günlüklerinin giriş kaynağını belirtir – bu durumda, blob depolamada depolanan ağ güvenliği grubu akış günlüğüne erişmemize olanak tanıyan bir Azure blog giriş eklentisi (sonraki adımlarda yüklü) kullanacaksınız.

Filtre bölümü daha sonra her akış günlüğü dosyasını düzleştirir, böylece her bir akış tuple'ı ve ilişkili özellikleri ayrı bir Logstash olayı haline gelir.

Son olarak, çıkış bölümü her Logstash olayını Graylog sunucusuna iletilir. Özel ihtiyaçlarınıza uyacak şekilde Logstash config dosyasını gerektiği gibi değiştirin.

   > [!NOTE]
   > Önceki config dosyası, Graylog sunucusunun yerel ana bilgisayar döngü IP adresi 127.0.0.1 üzerinde yapılandırıldığını varsayar. Değilse, çıktı bölümündeki ana bilgisayar parametresini doğru IP adresiyle değiştirdiğinden emin olun.

Logstash'ın yüklenmesiyle ilgili diğer talimatlar için Logstash [belgelerine](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)bakın.

### <a name="install-the-logstash-input-plug-in-for-azure-blob-storage"></a>Azure blob depolama için Logstash giriş eklentisini yükleme

Logstash eklentisi, akış günlüklerine kendi belirlediği blob depolama hesabından doğrudan erişmenizi sağlar. Eklentiyi yüklemek için varsayılan Logstash yükleme dizininden (bu durumda /usr/share/logstash/bin) aşağıdaki komutu çalıştırın:

```bash
cd /usr/share/logstash/bin
sudo ./logstash-plugin install logstash-input-azureblob
```

Bu eklenti hakkında daha fazla bilgi için [belgelere](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)bakın.

### <a name="set-up-connection-from-logstash-to-graylog"></a>Logstash'tan Graylog'a bağlantı ayarlama

Logstash'ı kullanarak akış günlüklerine bir bağlantı kurduğunuzve Graylog sunucusunu kurduğunuza göre, gelen günlük dosyalarını kabul etmek için Graylog'u yapılandırmanız gerekir.

1. Bunun için yapılandırdığınız URL'yi kullanarak Graylog Server web arabiriminize gidin. Tarayıcınızı`http://<graylog-server-ip>:9000/`

2. Yapılandırma sayfasına gitmek için, sağdaki üst gezinti çubuğundaki **Sistem** açılır menüsünü seçin ve ardından **Girişler'i**tıklatın.
   Alternatif olarak,`http://<graylog-server-ip>:9000/system/inputs`

   ![Başlarken](./media/network-watcher-analyze-nsg-flow-logs-graylog/getting-started.png)

3. Yeni girişi başlatmak için, Giriş açılır **düşüşünü seç'te** *GELF UDP'yi* seçin ve ardından formu doldurun. GELF Graylog Genişletilmiş Log Formatı anlamına gelir. GELF formatı Graylog tarafından geliştirilmiştir. Avantajları hakkında daha fazla bilgi edinmek için Graylog [belgelerine](https://docs.graylog.org/en/2.2/pages/gelf.html)bakın.

   Girişi Graylog sunucunuzu yapılandırdığınız IP'ye bağladığınızdan emin olun. IP adresi, Logstash yapılandırma dosyasının UDP çıkışının **ana** alanıyla eşleşmelidir. Varsayılan bağlantı noktası *12201*olmalıdır. Bağlantı **noktasının** Logstash config dosyasında belirtilen UDP çıkışındaki bağlantı noktası alanıyla eşleştiğinden emin olun.

   ![Girişler](./media/network-watcher-analyze-nsg-flow-logs-graylog/inputs.png)

   Girişi başlattıktan sonra, aşağıdaki resimde gösterildiği gibi **Yerel girişler** bölümünün altında görünmelidir:

   ![](./media/network-watcher-analyze-nsg-flow-logs-graylog/local-inputs.png)

   Graylog ileti girişleri hakkında daha fazla bilgi edinmek için [belgelere](https://docs.graylog.org/en/2.2/pages/sending_data.html#what-are-graylog-message-inputs)bakın.

4. Bu yapılandırmalar yapıldıktan sonra, aşağıdaki komutla akış günlüklerinde okumaya başlamak için `sudo systemctl start logstash.service`Logstash'ı başlatabilirsiniz: .

### <a name="search-through-graylog-messages"></a>Graylog iletileri arasında arama yapın

Graylog sunucunuzun ileti toplaması için biraz zaman tanıyan iletileri arayabilirsiniz. Graylog sunucunuza gönderilen iletileri kontrol etmek için **Girişyapılandırma** sayfasından oluşturduğunuz GELF UDP girişinin "**Alınan iletileri göster**" düğmesini tıklayın. Aşağıdaki resme benzeyen bir ekrana yönlendirilirsiniz: 

![Histogram](./media/network-watcher-analyze-nsg-flow-logs-graylog/histogram.png)

Mavi "%{Message}" bağlantısını tıklattığınızda, aşağıdaki resimde gösterildiği gibi, her akış tuple parametrelerini göstermek için her ileti genişletir:

![İletiler](./media/network-watcher-analyze-nsg-flow-logs-graylog/messages.png)

Varsayılan olarak, aranacak belirli bir ileti alanı seçmezseniz, tüm ileti alanları aramaya dahil edilir. Belirli iletileri aramak istiyorsanız (örn. – belirli bir kaynak IP'den akış tuples) [belgelenmiş](https://docs.graylog.org/en/2.2/pages/queries.html) olarak Graylog arama sorgu dilini kullanabilirsiniz

## <a name="analyze-network-security-group-flow-logs-using-graylog"></a>Graylog'u kullanarak ağ güvenliği grubu akış günlüklerini analiz edin

Artık Graylog'un çalışmasını ayarladığına göre, akış günlüğü verilerinizi daha iyi anlamak için bazı işlevlerini kullanabilirsiniz. Bunun bir yolu, verilerinizin belirli görünümlerini oluşturmak için panoları kullanmaktır.

### <a name="create-a-dashboard"></a>Pano oluşturma

1. Üstteki gezinti **çubuğunda, Panolar'ı** seçin veya`http://<graylog-server-ip>:9000/dashboards/`

2. Buradan, yeşil **Pano Oluştur** düğmesini tıklatın ve kısa formu panpanunuzun başlığı ve açıklamasıyla doldurun. Yeni pano oluşturmak için **Kaydet** düğmesine basın. Aşağıdaki resme benzer bir pano görürsünüz:

    ![Panolar](./media/network-watcher-analyze-nsg-flow-logs-graylog/dashboards.png)

### <a name="add-widgets"></a>Widget ekleme

Görmek için panonun başlığına tıklayabilirsiniz, ancak herhangi bir widget eklemediğimiz için şu anda boş. Panoya eklemek için kolay ve kullanışlı bir tür widget'ı, seçili alanın değerlerinin listesini ve bunların dağıtımını görüntüleyen **Hızlı Değerler** grafikleridir.

1. Üstteki gezinti çubuğundan **Arama'yı** seçerek akış günlüklerini alan UDP girişinin arama sonuçlarına geri gidin.

2. Ekranın sol tarafındaki **Arama sonuç** panelinin altında, her gelen akış tuple iletisinin çeşitli alanlarını listeleyen **Alanlar** sekmesini bulun.

3. Görselleştirmek için istediğiniz parametreyi seçin (bu örnekte IP kaynağı seçilir). Olası widget'ların listesini göstermek için, alanın solundaki mavi açılır oka tıklayın ve ardından widget'ı oluşturmak için **Hızlı değerleri** seçin. Aşağıdaki resme benzer bir şey görmeniz gerekir:

   ![Kaynak IP](./media/network-watcher-analyze-nsg-flow-logs-graylog/srcip.png)

4. Buradan, widget'ın sağ üst **köşesindeki panoya Ekle** düğmesini seçebilir ve eklemek için ilgili panoyu seçebilirsiniz.

5. Az önce eklediğiniz widget'ı görmek için panoya geri gidin.

   Aşağıdaki resimde gösterilen örnek pano gibi önemli ölçümleri izlemek için panonuza histogramlar ve sayımlar gibi çeşitli widget'lar ekleyebilirsiniz:

   ![Akış günlüğü panosu](./media/network-watcher-analyze-nsg-flow-logs-graylog/flowlogs-dashboard.png)

    Panolar ve diğer widget türleri hakkında daha fazla açıklama için Graylog'un [belgelerine](https://docs.graylog.org/en/2.2/pages/dashboards.html)bakın.

Ağ İzleyicisi'ni Graylog ile tümleştirerek, artık ağ güvenliği grubu akış günlüklerini yönetmek ve görselleştirmek için kullanışlı ve merkezi bir yolunuz vardır. Graylog, akış günlüklerini daha fazla yönetmek ve ağ trafiğinizi daha iyi anlamak için de kullanılabilecek akışlar ve uyarılar gibi diğer güçlü özelliklere sahiptir. Artık Graylog'u ayarladığınız ve Azure'a bağladığınızda, sunduğu diğer işlevleri keşfetmeye devam edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Power BI ile [Visualize ağ güvenlik grubu akışları günlüklerini](network-watcher-visualize-nsg-flow-logs-power-bi.md)ziyaret ederek Power BI ile ağ güvenlik grubu akış günlüklerinizi nasıl görselleştirebilirsiniz öğrenin.

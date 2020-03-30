---
title: Azure Monitör'de performans sayaçlarını toplama ve analiz edin | Microsoft Dokümanlar
description: Performans sayaçları, Windows ve Linux aracılarının performansını analiz etmek için Azure Monitor tarafından toplanır.  Bu makalede, hem Windows hem de Linux aracıları için Performans sayaçları koleksiyonunun nasıl yapılandırılabildikleri, bunların ayrıntılarının çalışma alanında depolanması ve azure portalında nasıl analiz edilebildikleri açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/28/2018
ms.openlocfilehash: d1a972a1d89066b961f2dcc28fba830e3a04ebc1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274768"
---
# <a name="windows-and-linux-performance-data-sources-in-azure-monitor"></a>Azure Monitor'da Windows ve Linux performans veri kaynakları
Windows ve Linux'taki performans sayaçları, donanım bileşenlerinin, işletim sistemlerinin ve uygulamaların performansı hakkında bilgi sağlar.  Azure Monitor, daha uzun vadeli analiz ve raporlama için performans verilerini toplamanın yanı sıra Yakın Gerçek Zamanlı (NRT) çözümlemesi için sık aralıklarla performans sayaçları toplayabilir.

![Performans sayaçları](media/data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Performans sayaçlarını yapılandırma
[Gelişmiş Ayarlar'da Veri menüsünden](agent-data-sources.md#configuring-data-sources)Performans sayaçlarını yapılandırın.

Yeni bir çalışma alanı için Windows veya Linux Performance sayaçlarını ilk yapılandırdığınızda, size hızlı bir şekilde birkaç ortak sayaç oluşturma seçeneği verilir.  Her birinin yanında bir onay kutusu görüntülenir.  Başlangıçta oluşturmak istediğiniz sayaçların işaretlandığından emin olun ve **ardından seçili performans sayaçlarını ekle'yi**tıklatın.

Windows performans sayaçları için, her performans sayacı için belirli bir örnek seçebilirsiniz. Linux performans sayaçları için, seçtiğiniz her sayaç örneği, ana sayacın tüm alt sayaçları için geçerlidir. Aşağıdaki tablo, hem Linux hem de Windows performans sayaçları için kullanılabilen yaygın örnekleri gösterir.

| Örnek adı | Açıklama |
| --- | --- |
| \_Toplam |Tüm örneklerin toplamı |
| \* |Tüm örnekler |
| (/&#124;/var) |İsimli örneklerle eşleşir: / veya /var |

### <a name="windows-performance-counters"></a>Windows performans sayaçları

![Windows Performans sayaçlarını yapılandırma](media/data-sources-performance-counters/configure-windows.png)

Toplamak için yeni bir Windows performans sayacı eklemek için bu yordamı izleyin.

1. Biçim *nesnesi(instance)\counter'daki*metin kutusuna sayacın adını yazın.  Yazmaya başladığınızda, ortak sayaçların eşleşen bir listesi sunulur.  Listeden bir sayaç seçebilir veya kendi sayaçlarınızı yazabilirsiniz.  Ayrıca *nesne\sayaç*belirterek belirli bir sayaç için tüm örnekleri döndürebilirsiniz.  

    Adlandırılmış örneklerden SQL Server performans sayaçları toplarken, adlandırılmış tüm örnek sayaçları *MSSQL$* ile başlar ve ardından örnek adı alınır.  Örneğin, adlı SQL instance INST2 veritabanı performans nesnesinden tüm veritabanları için Log Cache `MSSQL$INST2:Databases(*)\Log Cache Hit Ratio`Hit Ratio sayacı nı toplamak için, belirtin.

2. Sayacı listeye eklemek için **+** **Enter'a** tıklayın veya basın.
3. Sayaç eklediğinizde, **Örnek Aralığı**için 10 saniye varsayılanını kullanır.  Toplanan performans verilerinin depolama gereksinimlerini azaltmak istiyorsanız, bunu 1800 saniyeye (30 dakika) kadar daha yüksek bir değere değiştirebilirsiniz.
4. Sayaç eklemeyi bitirdiğinizde, yapılandırmayı kaydetmek için ekranın üst kısmındaki **Kaydet** düğmesini tıklatın.

### <a name="linux-performance-counters"></a>Linux performans sayaçları

![Linux Performans sayaçlarını yapılandırma](media/data-sources-performance-counters/configure-linux.png)

Toplamak için yeni bir Linux performans sayacı eklemek için bu yordamı izleyin.

1. Varsayılan olarak, tüm yapılandırma değişiklikleri otomatik olarak tüm aracılara itilir.  Linux aracıları için, bir yapılandırma dosyası Fluentd veri toplayıcısına gönderilir.  Bu dosyayı her Linux aracısı üzerinde el ile değiştirmek isterseniz, aşağıdaki *yapılandırmayı Linux makinelerime uygulayın* kutusunun işaretlerini kaldırın ve aşağıdaki kılavuzu izleyin.
2. Biçim *nesnesi(instance)\counter'daki*metin kutusuna sayacın adını yazın.  Yazmaya başladığınızda, ortak sayaçların eşleşen bir listesi sunulur.  Listeden bir sayaç seçebilir veya kendi sayaçlarınızı yazabilirsiniz.  
3. Nesnenin diğer sayaçları listesine sayacı eklemek için **+** **Enter'u** tıklatın veya tıklatın.
4. Bir nesnenin tüm sayaçları aynı **Örnek Aralığını**kullanır.  Varsayılan değer 10 saniyedir.  Toplanan performans verilerinin depolama gereksinimlerini azaltmak istiyorsanız, bunu 1800 saniyeye (30 dakika) kadar daha yüksek bir değere değiştirirsiniz.
5. Sayaç eklemeyi bitirdiğinizde, yapılandırmayı kaydetmek için ekranın üst kısmındaki **Kaydet** düğmesini tıklatın.

#### <a name="configure-linux-performance-counters-in-configuration-file"></a>Yapılandırma dosyasında Linux performans sayaçlarını yapılandırma
Azure portalını kullanarak Linux performans sayaçlarını yapılandırmak yerine, Yapılandırma dosyalarını Linux aracısı üzerinde düzenleme seçeneğiniz vardır.  Toplanacak performans ölçümleri **/etc/opt/microsoft/omsagent/\<workspace id\>/conf/omsagent.conf'taki**yapılandırma tarafından kontrol edilir.

Toplanacak performans ölçümlerinin her nesnesi veya kategorisi yapılandırma dosyasında `<source>` tek bir öğe olarak tanımlanmalıdır. Sözdizimi aşağıdaki deseni izler.

    <source>
      type oms_omi  
      object_name "Processor"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>


Bu öğedeki parametreler aşağıdaki tabloda açıklanmıştır.

| Parametreler | Açıklama |
|:--|:--|
| nesne\_adı | Koleksiyonun nesne adı. |
| örnek\_regex |  Hangi örneklerin toplanmasını tanımlayan normal bir *ifade.* Değer: `.*` tüm örnekleri belirtir. Yalnızca \_Toplam örneği için işlemci ölçümlerini toplamak `_Total`için. Yalnızca crond veya sshd örnekleri için işlem ölçümlerini toplamak `(crond\|sshd)`için şunları belirtebilirsiniz: . |
| sayaç\_\_adı regex | Hangi sayaçların (nesne için) toplanmasını tanımlayan normal bir *ifade.* Nesne için tüm sayaçları toplamak `.*`için şunları belirtin: . Örneğin, bellek nesnesi için yalnızca alan sayaçlarını değiştirmek için şunları belirtebilirsiniz:`.+Swap.+` |
| interval | Nesnenin sayaçlarının toplanma sıklığı. |


Aşağıdaki tabloda yapılandırma dosyasında belirtebileceğiniz nesneler ve sayaçlar listelendir.  [Azure Monitor'da Linux uygulamaları için Performans Topla sayaçlarında](data-sources-linux-applications.md)açıklandığı gibi belirli uygulamalar için ek sayaçlar mevcuttur.

| Nesne Adı | Sayaç Adı |
|:--|:--|
| Mantıksal Disk | % Ücretsiz Inodes |
| Mantıksal Disk | % Boş Alan |
| Mantıksal Disk | % Kullanılmış Inodes |
| Mantıksal Disk | % Kullanılan Alan |
| Mantıksal Disk | Disk Okuma Bayt/sn |
| Mantıksal Disk | Disk Okuma/sn |
| Mantıksal Disk | Disk Transferleri/sn |
| Mantıksal Disk | Disk Bayt/sn Yazma |
| Mantıksal Disk | Disk Yazma/sn |
| Mantıksal Disk | Ücretsiz Megabayt |
| Mantıksal Disk | Mantıksal Disk Baytları/sn |
| Bellek | % Kullanılabilir Bellek |
| Bellek | % Kullanılabilir Takas Alanı |
| Bellek | % Kullanılan Bellek |
| Bellek | % Kullanılan Takas Alanı |
| Bellek | Kullanılabilir MBytes Bellek |
| Bellek | Mevcut MBytes Swap |
| Bellek | Sayfa Okuma/sn |
| Bellek | Sayfa Yazma/sn |
| Bellek | Sayfalar/sn |
| Bellek | İkinci El MByteS Takas Alanı |
| Bellek | İkinci El Bellek MByte'ları |
| Ağ | Aktarılan Toplam Bayt |
| Ağ | Alınan Toplam Bayt Sayısı |
| Ağ | Toplam Bayt |
| Ağ | Aktarılan Toplam Paketler |
| Ağ | Alınan Toplam Paket Sayısı |
| Ağ | Toplam Rx Hataları |
| Ağ | Toplam Tx Hataları |
| Ağ | Toplam Çarpışmalar |
| Fiziksel Disk | Avg. Disk sn/Oku |
| Fiziksel Disk | Avg. Disk sn/Transfer |
| Fiziksel Disk | Avg. Disk sn/Yazma |
| Fiziksel Disk | Fiziksel Disk Bayt/sn |
| İşleme | Pct Ayrıcalıklı Zaman |
| İşleme | Pct Kullanıcı Saati |
| İşleme | İkinci El Bellek kByte'ları |
| İşleme | Sanal Paylaşılan Bellek |
| İşlemci | % DPC Süresi |
| İşlemci | % Boşta Kalma Süresi |
| İşlemci | % Kesme Süresi |
| İşlemci | % IO Bekleme Süresi |
| İşlemci | % Güzel Zaman |
| İşlemci | % Ayrıcalıklı Zaman |
| İşlemci | % İşlemci zamanı |
| İşlemci | % Kullanıcı Süresi |
| Sistem | Ücretsiz Fiziksel Bellek |
| Sistem | Sayfalama Dosyalarında Boş Alan |
| Sistem | Ücretsiz Sanal Bellek |
| Sistem | İşlemler |
| Sistem | Sayfalama Dosyalarında Depolanan Boyut |
| Sistem | Uptime |
| Sistem | Kullanıcılar |


Performans ölçümleri için varsayılan yapılandırma aşağıda veda eder.

    <source>
      type oms_omi
      object_name "Physical Disk"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 5m
    </source>

    <source>
      type oms_omi
      object_name "Logical Disk"
      instance_regex ".*
      counter_name_regex ".*"
      interval 5m
    </source>

    <source>
      type oms_omi
      object_name "Processor"
      instance_regex ".*
      counter_name_regex ".*"
      interval 30s
    </source>

    <source>
      type oms_omi
      object_name "Memory"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>

## <a name="data-collection"></a>Veri toplama
Azure Monitor, belirtilen tüm performans sayaçlarını, o sayacı yüklü olan tüm aracılarda belirtilen örnek aralıklarında toplar.  Veriler toplanmaz ve ham veriler aboneliğiniz tarafından belirtilen süre boyunca tüm günlük sorgusu görünümlerinde kullanılabilir.

## <a name="performance-record-properties"></a>Performans kayıt özellikleri
Performans kayıtları bir **Perf** türüne sahiptir ve aşağıdaki tablodaki özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olayın toplandığı bilgisayar. |
| Countername |Performans sayacının adı |
| Karşı Yol |Bilgisayar \\ \\ \<>\\nesne(örnek)\\sayacı şeklindeki sayacın tam yolu. |
| Karşı Değer |Sayacın sayısal değeri. |
| Örnekadı |Olay örneğinin adı.  Örnek yoksa boş. |
| ObjectName |Performans nesnesinin adı |
| SourceSystem |Verilerin toplandığı aracı türü. <br><br>OpsManager – Windows aracısı, doğrudan bağlantı veya SCOM <br> Linux – Tüm Linux ajanları  <br> AzureDepolama – Azure Tanılama |
| TimeGenerated |Verilerin örneklendiği tarih ve saat. |

## <a name="sizing-estimates"></a>Tahminleri boyutlandırma
 Belirli bir sayacın 10 saniyelik aralıklarla toplanması için kabaca bir tahmin, örnek başına günde yaklaşık 1 MB'dır.  Aşağıdaki formülle belirli bir sayacın depolama gereksinimlerini tahmin edebilirsiniz.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-queries-with-performance-records"></a>Performans kayıtlarıyla sorguları günlüğe kaydetme
Aşağıdaki tablo, Performans kayıtlarını alan günlük sorgularının farklı örneklerini sağlar.

| Sorgu | Açıklama |
|:--- |:--- |
| Perf |Tüm Performans verileri |
| Perf &#124; nerede Bilgisayar == "MyComputer" |Belirli bir bilgisayardaki tüm Performans verileri |
| Perf &#124; nerede CounterName == "Geçerli Disk Sıra Uzunluğu" |Belirli bir sayaç için tüm Performans verileri |
| Perf &#124; objectname == "İşlemci" ve CounterName == "% İşlemci Süresi" ve InstanceName == "_Total" &#124; AVGCPU = avg(CounterValue) bilgisayar tarafından özetlemek |Tüm bilgisayarlarda ortalama CPU Kullanımı |
| Perf &#124; nerede CounterName == "% İşlemci Süresi" &#124; Bilgisayar tarafından AggregatedValue = max(CounterValue) özetlemek |Tüm bilgisayarlarda maksimum CPU Kullanımı |
| Perf &#124; where ObjectName == "LogicalDisk" ve CounterName == "Current Disk Queue Length" ve Computer == "MyComputerName" &#124; InstanceName'ye göre AggregatedValue = avg(CounterValue) özetlemek |Belirli bir bilgisayarın tüm örneklerinde Ortalama Geçerli Disk Sırası uzunluğu |
| Perf &#124; nerede CounterName == "Disk Transferleri/sn" &#124; Bilgisayar tarafından AggregatedValue = percentile (CounterValue, 95) özetlemek |95. Tüm bilgisayarlarda Disk Aktarımlarının/Sn'nin Yüzdelik Yüzdesi |
| Perf &#124; nerede CounterName == "% İşlemci Süresi" ve InstanceName == "_Total" &#124; özetlemek AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 1h), Bilgisayar |Tüm bilgisayarlarda CPU kullanımının saatlik ortalaması |
| Perf &#124; nerede Bilgisayar == "MyComputer" ve CounterName startswith_cs "%" ve InstanceName == "_Total" &#124; toplamdValue = percentile(CounterValue, 70) tarafından bin (TimeGenerated, 1h), CounterName özetlemek | Belirli bir bilgisayar için her yüzde lik sayacın saatlik yüzde 70'i |
| Perf &#124; nerede CounterName == "% İşlemci Süresi" ve InstanceName == "_Total" ve Bilgisayar == "MyComputer" &#124; özetlemek ["min(CounterValue)"] = min(CounterValue), ["avg(CounterValue)"] = avg(CounterValue),["percentile75(CounterValue)"] = percentile(CounterValue, 75), ["max(CounterValue)"] = max(CounterValue) by bin(TimeGenerated, 1h), Bilgisayar |Belirli bir bilgisayar için saatlik ortalama, minimum, maksimum ve yüzde 75'lik CPU kullanımı |
| Perf &#124; where ObjectName == "MSSQL$INST2:Databases" ve InstanceName == "master" | Adlandırılmış SQL Server instance INST2'den ana veritabanı için Veritabanı performans nesnesinden tüm Performans verileri.  




## <a name="next-steps"></a>Sonraki adımlar
* MySQL ve Apache HTTP Server gibi [Linux uygulamalarından performans sayaçları toplayın.](data-sources-linux-applications.md)
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin.  
* Ek görselleştirmeler ve analizler için toplanan verileri [Power BI'ye](powerbi.md) aktarın.

---
title: Akış olaylarını yakalama - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makale, Azure Etkinlik Hub'ları üzerinden aktarılan olayları yakalamanızı sağlayan Yakalama özelliğine genel bir bakış sağlar.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: e53cdeea-8a6a-474e-9f96-59d43c0e8562
ms.service: event-hubs
ms.workload: na
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2020
ms.author: shvija
ms.openlocfilehash: c166f4cace6a8cc25b36a84f4614033801e69a51
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265018"
---
# <a name="capture-events-through-azure-event-hubs-in-azure-blob-storage-or-azure-data-lake-storage"></a>Azure Blob Depolama veya Azure Veri Gölü Depolamasında Azure Etkinlik Hub'ları aracılığıyla olayları yakalama
Azure Etkinlik Hub'ları, bir zaman veya boyut aralığı belirtme esnekliğiyle, Bir [Azure Blob depolama alanında](https://azure.microsoft.com/services/storage/blobs/) veya Seçtiğiniz Azure Veri Gölü Depolama Gen 1 veya Gen [2](https://azure.microsoft.com/services/data-lake-store/) hesabında Kisi veriakışı verilerini otomatik olarak yakalamanızı sağlar. Capture'i ayarlama hızlıdır, çalıştırmak için hiçbir yönetim maliyeti yoktur ve Olay Hub'ları ile otomatik olarak [ölçeklenir.](event-hubs-scalability.md#throughput-units) Olay Hub'ları Yakalama, akış verilerini Azure'a yüklemenin en kolay yoludur ve veri yakalama yerine veri işlemeye odaklanmanızı sağlar.

Olay Hub'ları Yakalama, aynı akışta gerçek zamanlı ve toplu iş akı temelli boru hatlarını işlemenizi sağlar. Bu, zaman içinde ihtiyaçlarınıza göre büyüyen çözümler oluşturabileceğiniz anlamına gelir. İster gelecekteki gerçek zamanlı işleme ye yönelik bir göz ile bugün toplu iş tabanlı sistemler inşa ediyor olun, ister mevcut gerçek zamanlı bir çözüme verimli bir soğuk yol eklemek istiyorsanız, Event Hubs Capture akış verileriyle çalışmayı kolaylaştırır.


## <a name="how-event-hubs-capture-works"></a>Olay Hub'ları Yakalama nasıl çalışır?

Olay Hub'ları, dağıtılmış bir günlük gibi telemetri girişi için zaman bekletme kalıcı bir arabellektir. Olay Hub'larında ölçeklemenin anahtarı [bölümlenmiş tüketici modelidir.](event-hubs-scalability.md#partitions) Her bölüm bağımsız bir veri bölümüdür ve bağımsız olarak tüketilir. Zaman içinde bu veriler, yapılandırılabilir bekletme dönemine bağlı olarak eskir. Sonuç olarak, belirli bir olay hub'ı asla "çok dolu" olmaz.

Olay Hub'ları Yakalama, yakalanan verileri depolamak için kullanılan kendi Azure Blob depolama hesabınızı ve kapsayıcınızı veya Azure Veri Gölü Depolama hesabınızı belirtmenize olanak tanır. Bu hesaplar, etkinlik hub'ınızla aynı bölgede veya başka bir bölgede olabilir ve bu da Olay Hub'ları Yakalama özelliğinin esnekliğini sağlar.

Yakalanan veriler [Apache Avro][Apache Avro] formatında yazılır: zengin veri yapılarını satır altı şema ile sağlayan kompakt, hızlı, ikili bir biçim. Bu biçim Hadoop ekosistemi, Akış Analizi ve Azure Veri Fabrikası'nda yaygın olarak kullanılır. Avro ile çalışma hakkında daha fazla bilgiyi bu makalenin ilerleyen bilgilerine ulaşabilirsiniz.

### <a name="capture-windowing"></a>Yakalama pencereleme

Olay Hub'ları Yakalama, yakalamayı denetlemek için bir pencere ayarlamanızı sağlar. Bu pencere, "ilk kazanç ilkesi" içeren minimum boyut ve zaman yapılandırmasıdır, yani ilk tetikleyici bir yakalama işlemine neden olur. On beş dakikalık, 100 MB yakalama pencereniz varsa ve saniyede 1 MB gönderiyorsanız, boyut penceresi zaman penceresinden önce tetikler. Her bölüm bağımsız olarak yakalar ve yakalama aralığının karşılandığı zaman adlandırılmış, yakalama sırasında tamamlanmış bir blok blob yazar. Depolama adlandırma kuralı aşağıdaki gibidir:

```
{Namespace}/{EventHub}/{PartitionId}/{Year}/{Month}/{Day}/{Hour}/{Minute}/{Second}
```

Tarih değerlerinin sıfırlarla dolgulu olduğunu unutmayın; bir örnek dosya adı olabilir:

```
https://mystorageaccount.blob.core.windows.net/mycontainer/mynamespace/myeventhub/0/2017/12/08/03/03/17.avro
```

Azure depolama bölmenizin geçici olarak kullanılamaması durumunda, Olay Hub'ları Yakalama, etkinlik merkezinizde yapılandırılan veri saklama süresi için verilerinizi saklar ve depolama hesabınız yeniden kullanılabilir olduğunda verileri geri doldurur.

### <a name="scaling-to-throughput-units"></a>Elde etme birimlerine ölçekleme

Olay Hub'ları [trafiği, iş birimi birimleri](event-hubs-scalability.md#throughput-units)tarafından denetlenir. Tek bir iş birimi saniyede 1 MB veya giriş saniyede 1000 olay ve bu çıkış miktarının iki katına izin verir. Standart Olay Hub'ları 1-20 iş birimiyle yapılandırılabilir ve kota artışı [destek isteği][support request]yle daha fazla satın alabilirsiniz. Satın aldığınız iş birimi birimlerinin dışındaki kullanım lar azaltılır. Olay Hub'ları Capture verileri doğrudan dahili Olay Hub'ları depolamasından kopyalayarak, iş gücü biriminin çıkış kotalarını atlayarak ve çıkışınızı Akış Analizi veya Kıvılcım gibi diğer işlem okuyucularına kaydederek kopyalar.

Yapılandırıldıktan sonra, Olay Hub'ları Yakalama ilk etkinliğinizi gönderdiğinizde otomatik olarak çalışır ve çalışmaya devam eder. Akış aşağı işleminizin işlemin çalıştığını bilmesini kolaylaştırmak için, Olay Hub'ları veri olmadığında boş dosyalar yazar. Bu işlem, toplu işişlemcilerinizi besleyebilen öngörülebilir bir biçim ve işaretçi sağlar.

## <a name="setting-up-event-hubs-capture"></a>Olay Hub'larını Yakalamayı Ayarlama

[Azure portalını](https://portal.azure.com)kullanarak veya Azure Kaynak Yöneticisi şablonlarını kullanarak Olay merkezi oluşturma zamanında Capture'i yapılandırabilirsiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure portalını kullanarak Event Hubs Yakalama özelliğini etkinleştirme](event-hubs-capture-enable-through-portal.md)
- [Bir olay hub'ı ile bir Event Hubs ad alanı oluşturma ve Azure Resource Manager şablonu kullanarak Yakalamayı etkinleştirme](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)


## <a name="exploring-the-captured-files-and-working-with-avro"></a>Yakalanan dosyaları keşfetmek ve Avro ile çalışmak

Olay Hub'ları Yakalama, yapılandırılan zaman penceresinde belirtildiği gibi Avro biçiminde dosyalar oluşturur. Bu dosyaları [Azure Depolama Gezgini][Azure Storage Explorer]gibi herhangi bir araçta görüntüleyebilirsiniz. Üzerinde çalışmak için dosyaları yerel olarak indirebilirsiniz.

Event Hubs Capture tarafından üretilen dosyalar aşağıdaki Avro şemasına sahiptir:

![Avro şeması][3]

Avro dosyalarını keşfetmenin kolay bir yolu, Apache'deki [Avro Tools][Avro Tools] kavanozu kullanmaktır. Ayrıca, sindirilen veriler üzerinde karmaşık dağıtılmış işleme gerçekleştirmek için hafif bir SQL tabanlı deneyim veya [Apache Spark][Apache Spark] için [Apache Drill'i][Apache Drill] de kullanabilirsiniz. 

### <a name="use-apache-drill"></a>Apache Matkap Kullanın

[Apache Drill,][Apache Drill] yapılandırılmış ve yarı yapılandırılmış verileri nerede olursa olsun sorgulayabilen "Büyük Veri araması için açık kaynak kodlu SQL sorgu motorudur". Motor bağımsız bir düğüm olarak veya büyük performans için büyük bir küme olarak çalıştırabilirsiniz.

Azure Blob depolamasına yerel bir destek kullanılabilir, bu da belgelerde açıklandığı gibi avro dosyasındaki verileri sorgulamayı kolaylaştırır:

[Apache Drill: Azure Blob Depolama Eklentisi][Apache Drill: Azure Blob Storage Plugin]

Yakalanan dosyaları kolayca sorgulamak için, Azure Blob depolama alanına erişmek için bir kapsayıcı aracılığıyla etkinleştirilen Apache Drill özellikli bir VM oluşturabilir ve çalıştırabilirsiniz:

https://github.com/yorek/apache-drill-azure-blob

Ölçek deposunda Akış'ta uçtan uca bir örnek kullanılabilir:

[Ölçekte Akış: Olay Hub'ları Yakalama]

### <a name="use-apache-spark"></a>Apache Spark'ı kullanma

[Apache Spark][Apache Spark] , "büyük ölçekli veri işleme için birleşik bir analiz motorudur." SQL de dahil olmak üzere farklı dilleri destekler ve Azure Blob depolama alanına kolayca erişebilir. Azure'da Apache Spark'ı çalıştırmak için birkaç seçenek vardır ve her biri Azure Blob depolama alanına kolay erişim sağlar:

- [HDInsight: Azure depolama alanında adres dosyaları][HDInsight: Address files in Azure storage]
- [Azure Databricks: Azure Blob depolama][Azure Databricks: Azure Blob Storage]
- [Azure Kubernetes Servisi](https://docs.microsoft.com/azure/aks/spark-job) 

### <a name="use-avro-tools"></a>Avro Araçlarını Kullan

[Avro Tools][Avro Tools] kavanoz paketi olarak mevcuttur. Kavanoz dosyasını indirdikten sonra aşağıdaki komutu çalıştırarak belirli bir Avro dosyasının şemasını görebilirsiniz:

```shell
java -jar avro-tools-1.9.1.jar getschema <name of capture file>
```

Bu komut döndürür

```json
{

    "type":"record",
    "name":"EventData",
    "namespace":"Microsoft.ServiceBus.Messaging",
    "fields":[
                 {"name":"SequenceNumber","type":"long"},
                 {"name":"Offset","type":"string"},
                 {"name":"EnqueuedTimeUtc","type":"string"},
                 {"name":"SystemProperties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Properties","type":{"type":"map","values":["long","double","string","bytes"]}},
                 {"name":"Body","type":["null","bytes"]}
             ]
}
```

Dosyayı JSON biçimine dönüştürmek ve diğer işlemleri gerçekleştirmek için Avro Araçları'nı da kullanabilirsiniz.

Daha gelişmiş işlemler gerçekleştirmek için, seçtiğiniz platform için Avro'ya indirin ve yükleyin. Bu yazının yazıldığı sırada C, C++, C,\#Java, NodeJS, Perl, PHP, Python ve Ruby için uygulamalar mevcuttur.

Apache Avro [Java][Java] ve [Python][Python]için başlangıç kılavuzları tam vardır. [Olay Hub'ları Yakalama makalesini](event-hubs-capture-python.md) de okuyabilirsiniz.

## <a name="how-event-hubs-capture-is-charged"></a>Olay Hub'ları Yakalama nasıl yüklenir?

Olay Hub'ları Yakalama, saat başı şarj olarak, iş lenme birimlerine benzer şekilde ölçülür. Ücret, ad alanı için satın alınan iş birimi sayısıyla doğru orantılıdır. Üretim birimleri artırıldıkça ve azaldıkça, Olay Hub'ları Yakalama sayaçları artar ve azalır, eşleşen performans sağlar. Sayaçlar birlikte ortaya çıkar. Fiyatlandırma ayrıntıları için [Olay Hub'ları](https://azure.microsoft.com/pricing/details/event-hubs/)fiyatlandırması'na bakın. 

Capture'in ayrı olarak faturalandırıldıkça çıkış kotasını tüketmediğini unutmayın. 

## <a name="integration-with-event-grid"></a>Olay Izgaraile Entegrasyon 

Kaynak olarak Olay Hub'ları ad alanı içeren bir Azure Olay Ağı aboneliği oluşturabilirsiniz. Aşağıdaki öğretici, kaynak olarak bir olay hub'ı ve lavabo olarak Azure İşlevleri uygulaması olan bir Olay Ağıaboneliği nasıl oluşturabileceğinizi gösterir: [Yakalanan Olay Hub'ları verilerini Olay Grid ve Azure Işlevleri'ni kullanarak bir SQL Veri Ambarı'na geçirin.](store-captured-data-data-warehouse.md)

## <a name="next-steps"></a>Sonraki adımlar
Olay Hub'ları Yakalama, Azure'a veri almanın en kolay yoludur. Azure Veri Gölü, Azure Veri Fabrikası ve Azure HDInsight'ı kullanarak, istediğiniz ölçekte seçtiğiniz tanıdık araçları ve platformları kullanarak toplu işişleme ve diğer analizleri gerçekleştirebilirsiniz.

Azure portalı ve Azure Kaynak Yöneticisi şablonu kullanarak bu özelliği nasıl etkinleştireceklerini öğrenin:

- [Event Hubs Capture için Azure portalını kullanma](event-hubs-capture-enable-through-portal.md)
- [Olay Hub'larının Yakalanmasını etkinleştirmek için Azure Kaynak Yöneticisi şablonu kullanın](event-hubs-resource-manager-namespace-event-hub-enable-capture.md)


[Apache Avro]: https://avro.apache.org/
[Apache Drill]: https://drill.apache.org/
[Apache Spark]: https://spark.apache.org/
[support request]: https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[Azure Storage Explorer]: https://azurestorageexplorer.codeplex.com/
[3]: ./media/event-hubs-capture-overview/event-hubs-capture3.png
[Avro Tools]: https://www.apache.org/dist/avro/stable/java/avro-tools-1.9.2.jar
[Java]: https://avro.apache.org/docs/current/gettingstartedjava.html
[Python]: https://avro.apache.org/docs/current/gettingstartedpython.html
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[HDInsight: Address files in Azure storage]:https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-blob-storage
[Azure Databricks: Azure Blob Storage]:https://docs.databricks.com/spark/latest/data-sources/azure/azure-storage.html
[Apache Drill: Azure Blob Storage Plugin]:https://drill.apache.org/docs/azure-blob-storage-plugin/
[Ölçekte Akış: Olay Hub'ları Yakalama]:https://github.com/yorek/streaming-at-scale/tree/master/event-hubs-capture

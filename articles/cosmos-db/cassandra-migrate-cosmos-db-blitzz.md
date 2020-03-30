---
title: Blitzz kullanarak Verileri Cassandra'dan Azure Cosmos DB Cassandra API'ye geçirin
description: Blitzz kullanarak Apache Cassandra veritabanından Azure Cosmos DB Cassandra API'ye verileri nasıl taşıyacağınız hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/21/2019
ms.author: sngun
ms.reviewer: sngun
ms.openlocfilehash: 746232a85e326f08d44cf2dbe0a4ae718c7cc312
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69984374"
---
# <a name="migrate-data-from-cassandra-to-azure-cosmos-db-cassandra-api-account-using-blitzz"></a>Blitzz kullanarak Verileri Cassandra'dan Azure Cosmos DB Cassandra API hesabına geçirin

Azure Cosmos DB'deki Cassandra API, Apache Cassandra'da çalışan kurumsal iş yükleri için şu gibi çeşitli nedenlerle mükemmel bir seçim haline gelmiştir: 

* **Yönetme ve izleme nin genel yükü yoktur:** Os, JVM ve yaml dosyaları ve etkileşimleri arasında sayısız ayarları yönetme ve izleme yükü ortadan kaldırır.

* **Önemli maliyet tasarrufu:** VM'lerin, bant genişliğinin ve geçerli lisansların maliyetini içeren Azure Cosmos DB ile maliyetten tasarruf edebilirsiniz. Ayrıca, veri merkezlerini, sunucuları, SSD depolamayı, ağ iletişimini ve elektrik maliyetlerini yönetmeniz gerekmez. 

* **Varolan kodu ve araçları kullanabilme:** Azure Cosmos DB, mevcut Cassandra SDK'lar ve araçlarla tel protokolü düzeyi uyumluluğu sağlar. Bu uyumluluk, küçük değişikliklerle Azure Cosmos DB Cassandra API’si ile mevcut kod tabanınızı kullanabilmenizi sağlar.

Veritabanı iş yüklerini bir platformdan diğerine geçirmenin çeşitli yolları vardır. [Blitzz,](https://www.blitzz.io) çeşitli veritabanlarından Azure Cosmos DB'ye sıfır kapalı kalma süresi geçişi gerçekleştirmenin güvenli ve güvenilir bir yolunu sunan bir araçtır. Bu makalede, Blitzz kullanarak Apache Cassandra veritabanından Azure Cosmos DB Cassandra API'ye veri geçirmek için gereken adımlar açıklanmaktadır.

## <a name="benefits-using-blitzz-for-migration"></a>Göç için Blitzz kullanmanın faydaları

Blitzz'in göç çözümü, karmaşık operasyonel iş yüklerini geçirmek için adım adım bir yaklaşım izler. Blitzz'in sıfır kesintili geçiş planının bazı önemli yönleri şunlardır:

* Apache Cassandra veritabanından Azure Cosmos DB'ye iş mantığının (tablolar, dizinler, görünümler) otomatik olarak geçişini sunar. Şemaları elle oluşturmanız gerekmez.

* Blitzz yüksek hacimli ve paralel veritabanı çoğaltma sunuyor. Değişim-Veri Yakalama (CDC) adı verilen bir teknik kullanarak geçiş sırasında hem kaynak hem de hedef platformların senkronize olmasını sağlar. Blitzz, CDC kullanarak sürekli olarak kaynak veritabanından (Apache Cassandra) bir değişiklik akışı çeker ve bunu hedef veritabanına (Azure Cosmos DB) uygular.

* Bu hataya dayanıklı dır ve sistemdeki bir donanım veya yazılım hatası sırasında bile verilerin tesliminde tam olarak bir kez garanti eder.

* SSL, şifreleme gibi çeşitli güvenlik metodolojileri kullanarak aktarım sırasında verileri güvence altına alar.

## <a name="steps-to-migrate-data"></a>Verileri geçirme adımları

Bu bölümde, Blitzz'i ayarlamak için gereken adımlar açıklanır ve verileri Apache Cassandra veritabanından Azure Cosmos DB'ye aktarır.

1. Blitzz çoğaltmasını yüklemeyi planladığınız bilgisayardan bir güvenlik sertifikası ekleyin. Bu sertifika, belirtilen Azure Cosmos DB hesabıyla bir SSL bağlantısı kurmak için Blitzz çoğaltması tarafından gereklidir. Sertifikayı aşağıdaki adımlarla ekleyebilirsiniz:

   ```bash
   wget https://cacert.omniroot.com/bc2025.crt
   mv bc2025.crt bc2025.cer
   keytool -keystore $JAVA_HOME/lib/security/cacerts -importcert -alias bc2025ca -file bc2025.cer
   ```

1. Blitzz kurulum ve ikili dosyaları ya [Blitzz web sitesinde](https://www.blitzz.io)bir demo talep ederek alabilirsiniz. Alternatif olarak, ekibe [de e-posta](mailto:success@blitzz.io) gönderebilirsiniz.

   ![Blitzz çoğaltma aracı indir](./media/cassandra-migrate-cosmos-db-blitzz/blitzz-replicant-download.png)

   ![Blitzz çoğaltma dosyaları](./media/cassandra-migrate-cosmos-db-blitzz/replicant-files.png)

1. CLI terminalinden kaynak veritabanı yapılandırmasını ayarlayın. Komutu kullanarak **`vi conf/conn/cassandra.yml`** yapılandırma dosyasını açın ve Cassandra düğümlerinin IP adreslerinin, bağlantı noktası numarasının, kullanıcı adının, parolanın ve gerekli diğer ayrıntıların virgülle ayrılmış bir listesini ekleyin. Yapılandırma dosyasındaki içeriklerin bir örneği aşağıda verilmiştir:

   ```bash
   type: CASSANDRA
  
   host: 172.17.0.2
   port: 9042

   username: 'cassandra'
   password: 'cassandra'

   max-connections: 30

   ```

   ![Cassandra bağlantı düzenleyicisi açık](./media/cassandra-migrate-cosmos-db-blitzz/open-connection-editor-cassandra.png)

   ![Cassandra bağlantı yapılandırması](./media/cassandra-migrate-cosmos-db-blitzz/cassandra-connection-configuration.png)

   Yapılandırma ayrıntılarını doldurduktan sonra dosyayı kaydedin ve kapatın.

1. İsteğe bağlı olarak, kaynak veritabanı filtre dosyasını ayarlayabilirsiniz. Filtre dosyası, hangi şemaların veya tabloların geçirilenleri belirtir. Komutu kullanarak **`vi filter/cassandra_filter.yml`** yapılandırma dosyasını açın ve aşağıdaki yapılandırma ayrıntılarını girin:

   ```bash

   allow:
   -    schema: “io_blitzz”
   Types: [TABLE]
   ```

   Veritabanı filtre ayrıntılarını doldurduktan sonra dosyayı kaydedin ve kapatın.

1. Daha sonra hedef veritabanı yapılandırmasını ayarlarsınız. Yapılandırmayı tanımlamadan [önce, bir Azure Cosmos DB Cassandra API hesabı oluşturun](create-cassandra-dotnet.md#create-a-database-account) ve ardından bir Keyspace ve geçirilen verileri depolamak için bir tablo oluşturun. Azure Cosmos DB'de Apache Cassandra'dan Cassandra API'ye geçiş yaptığınızdan, Apache cassandra ile kullandığınız bölüm anahtarını kullanabilirsiniz.

1. Verileri geçirmeden önce, kapsayıcı verisini uygulamanızın hızlı bir şekilde geçirilemesi için gereken miktara yükseltin. Örneğin, iş buzunu 100.000 RUs'a yükseltebilirsiniz. Geçişe başlamadan önce iş kaynağının ölçeklenilmesi, verilerinizi daha kısa sürede geçirmenize yardımcı olur.

   ![Azure Cosmos konteynerini tüm yolboyunca ölçeklendirin](./media/cassandra-migrate-cosmos-db-blitzz/scale-throughput.png)

   Geçiş tamamlandıktan sonra iş buzunu azaltın. Her işlem için depolanan veri ve RUS miktarına bağlı olarak, veri geçişinden sonra gereken iş miktarını tahmin edebilirsiniz. Gerekli RUs'ları nasıl tahmin edebilirsiniz hakkında daha fazla bilgi edinmek için, Azure [Cosmos DB kapasite planlayıcısı](estimate-ru-with-capacity-planner.md) makalelerini kullanarak [kapsayıcılar ve veritabanları nda Sağlama veri girişi](set-throughput.md) ve RU/ları tahmin etme konusuna bakın.

1. **Bağlantı String** bölmesinden Azure Cosmos hesabınızın **İletişim Noktasını, Bağlantı Noktasını, Kullanıcı Adını**ve Birincil **Parolasını** alın. Yapılandırma dosyasında bu değerleri kullanırsınız.

1. CLI terminalinden, hedef veritabanı yapılandırmasını ayarlayın. Komutu kullanarak **`vi conf/conn/cosmosdb.yml`** yapılandırma dosyasını açın ve ana bilgisayar URI, bağlantı noktası numarası, kullanıcı adı, parola ve diğer gerekli parametrelerin virgülle ayrılmış bir listesini ekleyin. Aşağıdaki örnekte yapılandırma dosyasının içeriği gösterilmektedir:

   ```bash
   type: COSMOSDB

   host: '<Azure Cosmos account’s Contact point>'
   port: 10350

   username: 'blitzzdemo'
   password: '<Your Azure Cosmos account’s primary password>'

   max-connections: 30
   ```

1. Daha sonra verileri Blitzz kullanarak geçirin. Blizz çoğaltmasını **tam** veya anlık **görüntü** modunda çalıştırabilirsiniz:

   * **Tam modda** – Bu modda, çoğaltma geçişten sonra çalıştırmaya devam eder ve kaynak Apache Cassandra sistemindeki değişiklikleri dinler. Herhangi bir değişiklik algılarsa, bunlar hedef Azure Cosmos hesabında gerçek zamanlı olarak çoğaltılır.

   * **Anlık görüntü modu** – Bu modda şema geçişi ve tek seferlik veri çoğaltma gerçekleştirebilirsiniz. Gerçek zamanlı çoğaltma bu seçenekle desteklenmez.

   Yukarıdaki iki mod kullanılarak, geçiş sıfır kapalı kalma süresi ile gerçekleştirilebilir. 

1. Blitzz çoğaltma CLI terminalinden verileri geçirmek için aşağıdaki komutu çalıştırın:

   ```bash
   ./bin/replicant full conf/conn/cassandra.yaml conf/conn/cosmosdb.yaml --filter filter/cassandra_filter.yaml --replace-existing
   ```

   Çoğaltma UI çoğaltma ilerleme gösterir. Şema geçişi ve anlık görüntü işlemi yapıldıktan sonra ilerleme %100'ü gösterir. Geçiş tamamlandıktan sonra, hedef Azure Cosmos veritabanındaki verileri doğrulayabilirsiniz.

   ![Cassandra veri geçişi çıkışı](./media/cassandra-migrate-cosmos-db-blitzz/cassandra-data-migration-output.png)


1. Geçiş için tam modu kullandığınızdan, kaynak Apache Cassandra veritabanındaki verileri ekleme, güncelleştirme veya silme gibi işlemleri gerçekleştirebilirsiniz. Daha sonra hedef Azure Cosmos veritabanında gerçek zamanlı olarak çoğaltıldıklarını doğrulayın. Geçişten sonra, Azure Cosmos kapsayıcınız için yapılandırılan iş ortasını azalttıklarından emin olun.

1. Çoğaltmayı herhangi bir noktayı durdurabilir ve **--devam** anahtarıyla yeniden başlatabilirsiniz. Çoğaltma, veri tutarlılığından ödün vermeden durduğu noktadan devam eder. Aşağıdaki komut, özgeçmiş anahtarının nasıl kullanılacağını gösterir.

   ```bash
   ./bin/replicant full conf/conn/cassandra.yaml conf/conn/cosmosdb.yaml --filter filter/cassandra_filter.yaml --replace-existing --resume
   ```

Hedefe veri geçişi, gerçek zamanlı geçiş hakkında daha fazla bilgi edinmek için [Blitzz çoğaltma](https://www.youtube.com/watch?v=fsUhF9LUZmM)demosu'na bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md) 
* [Bölüm anahtarı en iyi uygulamalar](partitioning-overview.md#choose-partitionkey)
* [Azure Cosmos DB kapasite planlayıcısı makalelerini kullanarak RU/ları tahmin edin](estimate-ru-with-capacity-planner.md)
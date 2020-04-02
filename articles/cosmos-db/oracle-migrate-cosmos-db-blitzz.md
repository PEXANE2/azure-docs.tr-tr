---
title: Blitzz kullanarak Oracle'dan Azure Cosmos DB Cassandra API'ye veri aktarın
description: Blitzz'i kullanarak Oracle veritabanından Azure Cosmos DB Cassandra API'ye verileri nasıl taşıyarak aktarılamayı öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/21/2019
ms.author: sngun
ms.reviewer: sngun
ms.openlocfilehash: 43d15a7252819a3e4f7635e37458b75e9b7ecca7
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546279"
---
# <a name="migrate-data-from-oracle-to-azure-cosmos-db-cassandra-api-account-using-blitzz"></a>Blitzz'i kullanarak Oracle'dan Azure Cosmos DB Cassandra API hesabına veri aktarın

Azure Cosmos DB'deki Cassandra API, Oracle'da çalışan kurumsal iş yükleri için şu gibi çeşitli nedenlerle mükemmel bir seçim haline gelmiştir:

* **Daha iyi ölçeklenebilirlik ve kullanılabilirlik:** Tek hata noktalarını, daha iyi ölçeklenebilirliği ve uygulamalarınız için kullanılabilirliği ortadan kaldırır.

* **Önemli maliyet tasarrufu:** VM, bant genişliği ve geçerli Oracle lisanslarının maliyetini içeren Azure Cosmos DB ile maliyetten tasarruf edebilirsiniz. Ayrıca, veri merkezlerini, sunucuları, SSD depolamayı, ağ iletişimini ve elektrik maliyetlerini yönetmeniz gerekmez.

* **Yönetme ve izleme nin genel yükü yoktur:** Azure Cosmos DB, tam olarak yönetilen bir bulut hizmeti olarak sayısız ayarı yönetme ve izleme giderini kaldırır.

Veritabanı iş yüklerini bir platformdan diğerine geçirmenin çeşitli yolları vardır. [Blitzz,](https://www.blitzz.io) çeşitli veritabanlarından Azure Cosmos DB'ye sıfır kapalı kalma süresi geçişi gerçekleştirmenin güvenli ve güvenilir bir yolunu sunan bir araçtır. Bu makalede, Blitzz kullanarak Oracle veritabanından Azure Cosmos DB Cassandra API'ye veri geçirmek için gereken adımlar açıklanmaktadır.

## <a name="benefits-using-blitzz-for-migration"></a>Göç için Blitzz kullanmanın faydaları

Blitzz'in göç çözümü, karmaşık operasyonel iş yüklerini geçirmek için adım adım bir yaklaşım izler. Blitzz'in sıfır kesintili geçiş planının bazı önemli yönleri şunlardır:

* Oracle veritabanından Azure Cosmos DB'ye iş mantığının (tablolar, dizinler, görünümler) otomatik olarak geçişini sunar. Şemaları elle oluşturmanız gerekmez.

* Blitzz yüksek hacimli ve paralel veritabanı çoğaltma sunuyor. Değişim-Veri Yakalama (CDC) adı verilen bir teknik kullanarak geçiş sırasında hem kaynak hem de hedef platformların senkronize olmasını sağlar. Blitzz, CDC kullanarak sürekli olarak kaynak veritabanından (Oracle) bir değişiklik akışı çeker ve bunu hedef veritabanına (Azure Cosmos DB) uygular.

* Bu hataya dayanıklı dır ve sistemdeki bir donanım veya yazılım hatası sırasında bile verilerin tesliminde tam olarak bir kez garanti eder.

* TLS/SSL, şifreleme gibi çeşitli güvenlik metodolojileri kullanarak aktarım sırasında verileri güvence altına alar.

* PL/SQL'de yazılı karmaşık iş mantığını Azure Cosmos DB'deki eşdeğer iş mantığına dönüştürmek için hizmetler sunar.

## <a name="steps-to-migrate-data"></a>Verileri geçirme adımları

Bu bölümde, Blitzz'i kurmak için gereken adımlar açıklanır ve verileri Oracle veritabanından Azure Cosmos DB'ye aktarır.

1. Blitzz çoğaltmasını yüklemeyi planladığınız bilgisayardan bir güvenlik sertifikası ekleyin. Bu sertifika, belirtilen Azure Cosmos DB hesabıyla BIR TLS bağlantısı kurmak için Blitzz çoğaltması tarafından gereklidir. Sertifikayı aşağıdaki adımlarla ekleyebilirsiniz:

   ```bash
   wget https://cacert.omniroot.com/bc2025.crt
   mv bc2025.crt bc2025.cer
   keytool -keystore $JAVA_HOME/lib/security/cacerts -importcert -alias bc2025ca -file bc2025.cer
   ```

1. ou [Blitzz web sitesinde](https://www.blitzz.io)bir demo isteyerek ya Blitzz yükleme ve ikili dosyaları alabilirsiniz. Alternatif olarak, ekibe [de e-posta](mailto:success@blitzz.io) gönderebilirsiniz.

   ![Blitzz çoğaltma aracı indir](./media/oracle-migrate-cosmos-db-blitzz/blitzz-replicant-download.png)

   ![Blitzz çoğaltma dosyaları](./media/oracle-migrate-cosmos-db-blitzz/replicant-files.png)

1. CLI terminalinden kaynak veritabanı yapılandırmasını ayarlayın. Komutu kullanarak **`vi conf/conn/oracle.yml`** yapılandırma dosyasını açın ve oracle düğümlerinin IP adreslerinin, bağlantı noktası numarasının, kullanıcı adının, parolanın ve gerekli diğer ayrıntıların virgülden ayrılmış bir listesini ekleyin. Aşağıdaki kod örnek bir yapılandırma dosyasını gösterir:

   ```bash
   type: ORACLE

   host: localhost
   port: 53546

   service-name: IO

   username: '<Username of your Oracle database>'
   password: '<Password of your Oracle database>'

   conn-cnt: 30
   use-ssl: false
   ```

   ![Oracle bağlantı düzenleyicisi açık](./media/oracle-migrate-cosmos-db-blitzz/open-connection-editor-oracle.png)

   ![Oracle bağlantı yapılandırması](./media/oracle-migrate-cosmos-db-blitzz/oracle-connection-configuration.png)

   Yapılandırma ayrıntılarını doldurduktan sonra dosyayı kaydedin ve kapatın.

1. İsteğe bağlı olarak, kaynak veritabanı filtre dosyasını ayarlayabilirsiniz. Filtre dosyası, hangi şemaların veya tabloların geçirilenleri belirtir. Komutu kullanarak **`vi filter/oracle_filter.yml`** yapılandırma dosyasını açın ve aşağıdaki yapılandırma ayrıntılarını girin:

   ```bash

   allow:
   -    schema: “io_blitzz”
   Types: [TABLE]
   ```
 
   Veritabanı filtre ayrıntılarını doldurduktan sonra dosyayı kaydedin ve kapatın.

1. Daha sonra hedef veritabanıyapılandırmasını ayarlarsınız. Yapılandırmayı tanımlamadan önce [bir Azure Cosmos DB Cassandra API hesabı oluşturun.](create-cassandra-dotnet.md#create-a-database-account) Verilerinizden [doğru bölüm anahtarını seçin](partitioning-overview.md#choose-partitionkey) ve ardından bir Keyspace ve geçirilen verileri depolamak için bir tablo oluşturun.

1. Verileri geçirmeden önce, kapsayıcı verisini uygulamanızın hızlı bir şekilde geçirilemesi için gereken miktara yükseltin. Örneğin, iş buzunu 100.000 RUs'a yükseltebilirsiniz. Geçişe başlamadan önce iş kaynağının ölçeklenilmesi, verilerinizi daha kısa sürede geçirmenize yardımcı olur. 

   ![Azure Cosmos konteynerini tüm yolboyunca ölçeklendirin](./media/oracle-migrate-cosmos-db-blitzz/scale-throughput.png)

   Geçiş tamamlandıktan sonra iş buzunu azaltmanız gerekir. Her işlem için depolanan veri ve RUS miktarına bağlı olarak, veri geçişinden sonra gereken iş miktarını tahmin edebilirsiniz. Gerekli RUs'ları nasıl tahmin edebilirsiniz hakkında daha fazla bilgi edinmek için, Azure [Cosmos DB kapasite planlayıcısı](estimate-ru-with-capacity-planner.md) makalelerini kullanarak [kapsayıcılar ve veritabanları nda Sağlama veri girişi](set-throughput.md) ve RU/ları tahmin etme konusuna bakın.

1. **Bağlantı String** bölmesinden Azure Cosmos hesabınızın **İletişim Noktasını, Bağlantı Noktasını, Kullanıcı Adını**ve Birincil **Parolasını** alın. Yapılandırma dosyasında bu değerleri kullanırsınız.

1. CLI terminalinden, hedef veritabanı yapılandırmasını ayarlayın. Komutu kullanarak **`vi conf/conn/cosmosdb.yml`** yapılandırma dosyasını açın ve ana bilgisayar URI, bağlantı noktası numarası, kullanıcı adı, parola ve diğer gerekli parametrelerin virgülle ayrılmış bir listesini ekleyin. Yapılandırma dosyasındaki içeriklerin bir örneği aşağıda verilmiştir:

   ```bash
   type: COSMOSDB

   host: `<Azure Cosmos account’s Contact point>`
   port: 10350

   username: 'blitzzdemo'
   password: `<Your Azure Cosmos account’s primary password>'

   max-connections: 30
   use-ssl: false
   ```

1. Daha sonra verileri Blitzz kullanarak geçirin. Blizz çoğaltmasını **tam** veya anlık **görüntü** modunda çalıştırabilirsiniz:

   * **Tam mod** – Bu modda, çoğaltma geçişten sonra çalıştırmaya devam eder ve kaynak Oracle sistemindeki değişiklikleri dinler. Herhangi bir değişiklik algılarsa, bunlar hedef Azure Cosmos hesabında gerçek zamanlı olarak çoğaltılır.

   * **Anlık görüntü modu** – Bu modda şema geçişi ve tek seferlik veri çoğaltma gerçekleştirebilirsiniz. Gerçek zamanlı çoğaltma bu seçenekle desteklenmez.


   Yukarıdaki iki mod kullanılarak, geçiş sıfır kapalı kalma süresi ile gerçekleştirilebilir.

1. Blitzz çoğaltma CLI terminalinden verileri geçirmek için aşağıdaki komutu çalıştırın:

   ```bash
   ./bin/replicant full conf/conn/oracle.yaml conf/conn/cosmosdb.yaml --filter filter/oracle_filter.yaml --replace-existing
   ```

   Çoğaltma UI çoğaltma ilerleme gösterir. Şema geçişi ve anlık görüntü işlemi yapıldıktan sonra ilerleme %100'ü gösterir. Geçiş tamamlandıktan sonra, hedef Azure Cosmos veritabanındaki verileri doğrulayabilirsiniz.

   ![Oracle veri geçişi çıktısı](./media/oracle-migrate-cosmos-db-blitzz/oracle-data-migration-output.png)

1. Geçiş için tam modu kullandığınızdan, kaynak Oracle veritabanında veri ekleme, güncelleştirme veya silme gibi işlemleri gerçekleştirebilirsiniz. Daha sonra hedef Azure Cosmos veritabanında gerçek zamanlı olarak çoğaltıldıklarını doğrulayabilirsiniz. Geçişten sonra, Azure Cosmos kapsayıcınız için yapılandırılan iş ortasını azalttıklarından emin olun.

1. Çoğaltmayı herhangi bir noktayı durdurabilir ve **--devam** anahtarıyla yeniden başlatabilirsiniz. Çoğaltma, veri tutarlılığından ödün vermeden durduğu noktadan devam eder. Aşağıdaki komut, özgeçmiş anahtarının nasıl kullanılacağını gösterir.

   ```bash
   ./bin/replicant full conf/conn/oracle.yaml conf/conn/cosmosdb.yaml --filter filter/oracle_filter.yaml --replace-existing --resume
   ```

Hedefe veri geçişi, gerçek zamanlı geçiş hakkında daha fazla bilgi edinmek için [Blitzz çoğaltma](https://www.youtube.com/watch?v=y5ZeRK5A-MI)demosu'na bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Kapsayıcı ve veritabanlarına aktarım hızı sağlama](set-throughput.md) 
* [Bölüm anahtarı en iyi uygulamalar](partitioning-overview.md#choose-partitionkey)
* [Azure Cosmos DB kapasite planlayıcısı makalelerini kullanarak RU/ları tahmin edin](estimate-ru-with-capacity-planner.md)
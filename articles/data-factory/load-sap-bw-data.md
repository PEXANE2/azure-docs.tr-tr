---
title: SAP İş Ambarından yük verileri
description: SAP İş Ambarı'ndan (BW) veri kopyalamak için Azure Veri Fabrikası'nı kullanın
services: data-factory
author: linda33wj
ms.author: jingwang
manager: shwang
ms.reviewer: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/22/2019
ms.openlocfilehash: 96b23696164514ad2f16de72f0f76aa237ffce2e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415832"
---
# <a name="copy-data-from-sap-business-warehouse-by-using-azure-data-factory"></a>Azure Veri Fabrikası'nı kullanarak SAP İş Ambarı'ndaki verileri kopyalama
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, SAP İş Ambarı'ndan (BW) Open Hub üzerinden Azure Veri Gölü Depolama Gen2'ye verileri kopyalamak için Azure Veri Fabrikası'nın nasıl kullanılacağı gösterilmektedir. Verileri diğer [desteklenen lavabo veri depolarına](copy-activity-overview.md#supported-data-stores-and-formats)kopyalamak için benzer bir işlem kullanabilirsiniz.

> [!TIP]
> SAP BW Open Hub tümleştirmesi ve delta çıkarma akışı da dahil olmak üzere SAP BW'den veri kopyalama hakkında genel bilgi için, [Azure Veri Fabrikası'nı kullanarak OPEN Hub üzerinden SAP İş Ambarı'ndan veri kopyalama'ya](connector-sap-business-warehouse-open-hub.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

- **Azure Veri Fabrikası**: Varsa, [bir veri fabrikası oluşturmak](quickstart-create-data-factory-portal.md#create-a-data-factory)için aşağıdaki adımları izleyin.

- **HEDEF türü "Veritabanı Tablosu" ile SAP BW Açık Hub Hedefi (OHD):** Bir OHD oluşturmak veya OHD'nizin Veri Fabrikası tümleştirmesi için doğru şekilde yapılandırıldığından kontrol etmek için bu makalenin [SAP BW Open Hub Hedef yapılandırmaları](#sap-bw-open-hub-destination-configurations) bölümüne bakın.

- **SAP BW kullanıcısının aşağıdaki izinlere ihtiyacı vardır:**

  - Uzaktan İşlev Aramaları (RFC) ve SAP BW için yetkilendirme.
  - **S_SDSAUTH** yetkilendirme nesnesinin "Yürüt" etkinliğine izinler.

- **SAP .NET konektörü 3.0 ile [kendi kendine barındırılan tümleştirme çalışma süresi (IR).](concepts-integration-runtime.md#self-hosted-integration-runtime) ** Aşağıdaki kurulum adımlarını izleyin:

  1. Kendi kendine barındırılan tümleştirme çalışma saatini, sürüm 3.13 veya sonraki sürümlerini yükleyin ve kaydedin. (Bu işlem daha sonra bu makalede açıklanmıştır.)

  2. Microsoft [.NET 3.0 için 64 bit SAP Bağlayıcısını](https://support.sap.com/en/product/connectors/msnet.html) SAP'nin web sitesinden indirin ve kendi barındırdığı IR ile aynı bilgisayara yükleyin. Yükleme sırasında, aşağıdaki resimde görüldüğü **gibi, İsteğe bağlı kurulum adımları** iletişim kutusunda **GAC'ye Derlemeleri Yükle'yi** seçtiğinizden emin olun:

     ![SAP .NET Bağlayıcı iletişim kutusunu ayarlama](media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

## <a name="do-a-full-copy-from-sap-bw-open-hub"></a>SAP BW Open Hub'dan tam kopya yapın

Azure portalında veri fabrikanıza gidin. Veri Fabrikası Kullanıcı Çağrı Lerini ayrı bir sekmede açmak için **Yazar & Monitör'ü** seçin.

1. **Başlatalım** sayfasında, Verileri **Kopyala** aracını açmak için Verileri Kopyala'yı seçin.

2. **Özellikler** sayfasında bir **Görev adı**belirtin ve sonra **İleri'yi**seçin.

3. Kaynak **veri deposu** sayfasında **+Yeni bağlantı oluştur'u**seçin. Bağlayıcı galerisinden **SAP BW Aç Hub'ı** seçin ve ardından **Devam et'i**seçin. Bağlayıcılara filtre uygulayacak şekilde arama kutusuna **SAP** yazabilirsiniz.

4. SAP **BW Açık Hub bağlantı** sayfasında, yeni bir bağlantı oluşturmak için aşağıdaki adımları izleyin.

   ![SAP BW Aç Hub'a bağlı hizmet sayfası oluşturma](media/load-sap-bw-data/create-sap-bw-open-hub-linked-service.png)

   1. **Tümleştirme çalışma zamanı** listesinden Bağlan'dan, varolan kendi barındırılan bir IR'yi seçin. Veya henüz bir tane yoksa bir tane oluşturmayı seçin.

      Kendi kendine barındırılan yeni bir IR oluşturmak için **+Yeni'yi**seçin ve ardından **Kendi kendine barındırılan Ip'yi**seçin. Bir **Ad**girin ve sonra **İleri'yi**seçin. Geçerli bilgisayara yüklemek için **Express kurulumünü** seçin veya sağlanan **El ile kurulum** adımlarını izleyin.

      [Önkoşullar'da](#prerequisites)belirtildiği gibi, Microsoft .NET 3.0 için SAP Bağlayıcısı'nın kendi barındırılan IR'nin çalıştığı aynı bilgisayarda yüklü olduğundan emin olun.

   2. SAP BW **Sunucu adı,** **Sistem numarası,** **İstemci Kimliği,** **Dil** **(EN**dışında ise), **Kullanıcı adı**ve **Şifre'yi**doldurun.

   3. Ayarları doğrulamak için **Test bağlantısını** seçin ve ardından **Bitir'i**seçin.

   4. Yeni bir bağlantı oluşturulur. **İleri**’yi seçin.

5. Hub **Hedeflerini Seç** sayfasında SAP BW'nizde bulunan Açık Hub Hedefleri'ne göz atın. Verileri kopyalamak için OHD'yi seçin ve sonra **İleri'yi**seçin.

   ![SAP BW Açık Hub Hedef tablosunu seçin](media/load-sap-bw-data/select-sap-bw-open-hub-table.png)

6. İhtiyacınız varsa bir filtre belirtin. OHD'niz yalnızca tek bir istek kimliğiyle tek bir veri aktarım işleminden (DTP) veri içeriyorsa veya DTP'nizin bittiğinden eminseniz ve verileri kopyalamak istiyorsanız, **Son İstek'i Hariç Tut** onay kutusunu temizleyin.

   Bu makalenin SAP [BW Open Hub Hedef yapılandırmaları](#sap-bw-open-hub-destination-configurations) bölümünde bu ayarlar hakkında daha fazla bilgi edinin. Hangi verilerin döndürüleceğini iki kez denetlemek için **Onu doğrula'yı** seçin. Ardından **İleri'yi**seçin.

   ![SAP BW Open Hub filtresini yapılandırın](media/load-sap-bw-data/configure-sap-bw-open-hub-filter.png)

7. Hedef **veri deposu** sayfasında **+Yeni bağlantı** > oluştur**Azure Veri Gölü Depolama Gen2** > **Devam'ı**seçin.

8. Azure **Veri Gölü Depolama bağlantı** sayfası belirtin, bağlantı oluşturmak için aşağıdaki adımları izleyin.

   ![ADLS Gen2 bağlantılı hizmet sayfası oluşturma](media/load-sap-bw-data/create-adls-gen2-linked-service.png)

   1. **Ad** açılır listesinden Veri Gölü Depolama Gen2 özellikli hesabınızı seçin.
   2. **Son**'u seçerek bağlantıyı oluşturun. Ardından **İleri'yi**seçin.

9. Çıktı **dosyasını veya klasörünü seç** sayfasında, çıktı klasörü adı olarak **copyfromopenhub** girin. Ardından **İleri'yi**seçin.

   ![Çıktı klasörü sayfasını seçin](media/load-sap-bw-data/choose-output-folder.png)

10. Dosya **biçimi ayarı** sayfasında, varsayılan ayarları kullanmak için **İleri'yi** seçin.

    ![Lavabo biçimi sayfasını belirtin](media/load-sap-bw-data/specify-sink-format.png)

11. **Ayarlar** sayfasında Performans **ayarlarını**genişletin. SAP BW'den paralel olarak yüklemek için 5 gibi **kopya paralellik derecesi** için bir değer girin. Ardından **İleri'yi**seçin.

    ![Kopyalama ayarlarını yapılandırma](media/load-sap-bw-data/configure-copy-settings.png)

12. **Özet** sayfasında, ayarları inceleyin. Ardından **İleri'yi**seçin.

13. **Dağıtım** sayfasında, ardışık izlemeyi izlemek için **Monitör'ü** seçin.

    ![Dağıtım sayfası](media/load-sap-bw-data/deployment.png)

14. Sayfanın sol tarafındaki **Monitör** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemler** sütunu, etkinlik tarafından işletilen ayrıntıları görüntülemek ve ardışık hattı yeniden çalıştırmak için bağlantılar içerir.

    ![Boru hattı izleme görünümü](media/load-sap-bw-data/pipeline-monitoring.png)

15. Ardışık işlem hattı çalıştırımıyla ilişkili etkinlik çalışanlarını görüntülemek **için, Eylemler** sütununda **Etkinlik Çalıştırmalarını Görüntüle'yi** seçin. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. Ardışık hatlar çalıştırılanın görünümüne geri dönmek için en üstteki **Ardışık Hatlar** bağlantısını seçin. Listeyi yenilemek için **Yenile**’yi seçin.

    ![Etkinlik izleme ekranı](media/load-sap-bw-data/activity-monitoring.png)

16. Her kopyalama etkinliğinin yürütme ayrıntılarını izlemek için, etkinlik izleme görünümündeki **Eylemler'in** altındaki gözlük simgesi olan **Ayrıntılar** bağlantısını seçin. Kullanılabilir ayrıntılar, kaynaktan lavaboya kopyalanan veri hacmini, veri veri verisini, yürütme adımlarını ve süresini ve kullanılan yapılandırmaları içerir.

    ![Etkinlik izleme ayrıntıları](media/load-sap-bw-data/activity-monitoring-details.png)

17. **Maksimum İstek Kimliğini**görüntülemek için etkinlik izleme görünümüne geri dön ve **Eylemler**altında **Çıktı'yı** seçin.

    ![Etkinlik çıktı ekranı](media/load-sap-bw-data/activity-output.png)

    ![Etkinlik çıktısı ayrıntıları görünümü](media/load-sap-bw-data/activity-output-details.png)

## <a name="incremental-copy-from-sap-bw-open-hub"></a>SAP BW Open Hub'dan artımlı kopya

> [!TIP]
> Veri Fabrikası'ndaki SAP BW Open Hub konnektörünü SAP BW'den artan verileri nasıl kopyalayabildiğini öğrenmek için [SAP BW Open Hub konektör delta çıkarma akışına](connector-sap-business-warehouse-open-hub.md#delta-extraction-flow) bakın. Bu makale, temel bağlayıcı yapılandırması anlamanıza da yardımcı olabilir.

Şimdi, SAP BW Open Hub'Dan artımlı kopya yapılandırmaya devam edelim.

Artımlı **kopya, istek kimliğine**dayalı bir "yüksek filigran" mekanizması kullanır. Bu kimlik, SAP BW Open Hub Hedef'te DTP tarafından otomatik olarak oluşturulur. Aşağıdaki diyagram bu iş akışını gösterir:

![Artımlı kopyalama iş akışı akış şeması](media/load-sap-bw-data/incremental-copy-workflow.png)

Veri fabrikasında **Başlatalım** sayfasında, yerleşik şablonu kullanmak için **şablondan veri aktarını oluştur'u** seçin.

1. **SAP BW'den Azure Veri Gölü Depolama Gen2 şablonuna kadar Artımlı kopyayı** bulmak ve seçmek için **SAP BW'yi** arayın. Bu şablon, verileri Azure Veri Gölü Depolama Gen2'de kopyalar. Diğer lavabo türlerine kopyalamak için benzer bir iş akışı kullanabilirsiniz.

2. Şablonun ana sayfasında, aşağıdaki üç bağlantıyı seçin veya oluşturun ve ardından pencerenin sağ alt köşesinde **bu şablonu kullan'ı** seçin.

   - **Azure Blob depolama**: Bu izbinde, *en yüksek kopyalanan istek kimliği*olan yüksek filigranı depolamak için Azure Blob depolama alanını kullanıyoruz.
   - **SAP BW Open Hub**: Bu kaynaktan veri kopyalamak tır. Ayrıntılı yapılandırma için önceki tam kopya gözden geçirme denetimine bakın.
   - **Azure Veri Gölü Depolama Gen2**: Bu, verileri kopyalamak için lavabodur. Ayrıntılı yapılandırma için önceki tam kopya gözden geçirme denetimine bakın.

   ![SAP BW şablonundan artımlı kopya](media/load-sap-bw-data/incremental-copy-from-sap-bw-template.png)

3. Bu şablon, aşağıdaki üç etkinlikle bir ardışık hatlar oluşturur ve bunları başarı üzerine zincirleme yapar: *Arama,* *Verileri Kopyala*ve *Web*.

   Boru hattı **Parametreleri** sekmesine gidin. Sağlamanız gereken tüm yapılandırmaları görürsünüz.

   ![SAP BW yapılandırmasından artımlı kopya](media/load-sap-bw-data/incremental-copy-from-sap-bw-pipeline-config.png)

   - **SAPOpenHubDestinationName**: Verileri kopyalamak için Hub'ı Aç tablo adını belirtin.

   - **Data_Destination_Container**: Verileri kopyalamak için hedef Azure Veri Gölü Depolama Gen2 kapsayıcısını belirtin. Kapsayıcı yoksa, Veri Fabrikası kopyalama etkinliği yürütme sırasında bir tane oluşturur.
  
   - **Data_Destination_Directory**: Verileri kopyalamak için Azure Veri Gölü Depolama Gen2 kapsayıcısının altındaki klasör yolunu belirtin. Yol yoksa, Veri Fabrikası kopyalama etkinliği yürütme sırasında bir yol oluşturur.
  
   - **HighWatermarkBlobContainer**: Yüksek filigran değerini depolamak için kabı belirtin.

   - **HighWatermarkBlobDirectory**: Yüksek filigran değerini depolamak için kapsayıcının altındaki klasör yolunu belirtin.

   - **HighWatermarkBlobName**: Yüksek filigran değerini depolamak için `requestIdCache.txt`blob adını belirtin. Blob depolama, HighWatermarkBlobContainer + HighWatermarkBlobDirectory + HighWatermarkBlobName, konteyner */ yol / requestIdCache.txt*gibi karşılık gelen yolu gidin. İçerik 0 ile bir leke oluşturun.

      ![Blob içeriği](media/load-sap-bw-data/blob.png)

   - **LogicAppURL**: Bu şablonda, Blob depolamasındaki yüksek filigran değerini ayarlamak için Azure Logic Apps'ı aramak için WebActivity'i kullanıyoruz. Veya depolamak için Azure SQL Veritabanı'nı kullanabilirsiniz. Değeri güncelleştirmek için depolanmış yordam etkinliği kullanın.

      Aşağıdaki resimde görüldüğü gibi, önce bir mantık uygulaması oluşturmanız gerekir. Ardından, **HTTP POST URL'ye**yapıştırın.

      ![Mantık Uygulaması yapılandırması](media/load-sap-bw-data/logic-app-config.png)

      1. Azure portalına gidin. Yeni bir **Logic Apps** hizmeti seçin. **Logic Apps Designer'a**gitmek için **+Boş Mantık Uygulaması'nı** seçin.

      2. Bir HTTP **isteği ne zaman alınır**tetikleyicisi oluşturun. HTTP istek gövdesini aşağıdaki gibi belirtin:

         ```json
         {
            "properties": {
               "sapOpenHubMaxRequestId": {
                  "type": "string"
               }
            },
            "type": "object"
         }
         ```

      3. **Blob** oluşturma eylemi ekleyin. **Klasör yolu** ve **Blob adı için,** daha önce *HighWatermarkBlobContainer+HighWatermarkBlobDirectory* ve *HighWatermarkBlobName'de*yapılandırdığınız değerleri kullanın.

      4. **Kaydet**’i seçin. Ardından, Veri Fabrikası ardışık alanında kullanmak üzere **HTTP POST URL** değerini kopyalayın.

4. Veri Fabrikası ardışık hatlar parametrelerini sağladıktan sonra, yapılandırmayı doğrulamak için bir çalıştırma yı çağırmak için **Hata Ayıklama** > **Bitiş'i** seçin. Veya, tüm değişiklikleri yayımlamak için **Yayımla'yı** seçin ve ardından çalıştırmayı yürütmek için **tetikleyici ekle'yi** seçin.

## <a name="sap-bw-open-hub-destination-configurations"></a>SAP BW Açık Hub Hedef yapılandırmaları

Bu bölümde, verileri kopyalamak için Veri Fabrikası'ndaki SAP BW Open Hub bağlayıcısını kullanmak için SAP BW tarafının yapılandırması tanıtılmaktadır.

### <a name="configure-delta-extraction-in-sap-bw"></a>SAP BW'de delta ekstraksiyonu yapılandırın

Hem geçmiş kopyaya hem de artımlı kopyaya veya yalnızca artımlı kopyaya ihtiyacınız varsa, DELTA ekstraksiyonu SAP BW'de yapılandırın.

1. Açık Hub Hedefini oluşturun. Gerekli dönüştürme ve veri aktarım işlemini otomatik olarak oluşturan SAP İşlem RSA1'de OHD'yi oluşturabilirsiniz. Aşağıdaki ayarları kullanın:

   - **ObjectType**: Herhangi bir nesne türünü kullanabilirsiniz. Burada, bir örnek olarak **InfoCube** kullanın.
   - **Hedef Türü**: **Veritabanı Tablosunu**Seçin.
   - **Tablonun Anahtarı**: **Teknik Anahtarı**seçin.
   - **Ekstraksiyon**: **Verileri Tut'u seçin ve Tabloya Kayıt Ekle'yi**seçin.

   ![SAP BW OHD delta çıkarma iletişim kutusu oluşturma](media/load-sap-bw-data/create-sap-bw-ohd-delta.png)

   ![SAP BW OHD delta2 çıkarma iletişim kutusu oluşturma](media/load-sap-bw-data/create-sap-bw-ohd-delta2.png)

   DTP için paralel çalışan SAP iş süreçlerinin sayısını artırabilirsiniz:

   ![oluşturmak-sap-bw-ohd-delta3](media/load-sap-bw-data/create-sap-bw-ohd-delta3.png)

2. DTP'yi işlem zincirlerinde zamanlayın.

   Küp için delta DTP yalnızca gerekli satırlar sıkıştırılmamışsa çalışır. BW küp sıkıştırmanın DTP'den önce Aç Hub tablosuna çalışmadığından emin olun. Bunu yapmanın en kolay yolu, DTP'yi mevcut işlem zincirlerinize entegre etmektir. Aşağıdaki örnekte, DTP (OHD'ye) *Ayarla* (toplam toplama) ve *Daralt* (küp sıkıştırma) adımları arasındaki işlem zincirine eklenir.

   ![SAP BW proses zinciri akış şeması oluşturma](media/load-sap-bw-data/create-sap-bw-process-chain.png)

### <a name="configure-full-extraction-in-sap-bw"></a>SAP BW'de tam çıkarma yapılandırma

Delta çıkarma ek olarak, aynı SAP BW InfoProvider tam bir çıkarma isteyebilirsiniz. Bu genellikle tam kopyalama yapmak ancak artımlı değil yapmak istiyorsanız veya [delta çıkarma yeniden senkronize](#resync-delta-extraction)etmek istiyorsanız geçerlidir.

Aynı OHD için birden fazla DTP olamaz. Bu nedenle, delta çıkarma önce ek bir OHD oluşturmanız gerekir.

![SAP BW OHD tam oluşturma](media/load-sap-bw-data/create-sap-bw-ohd-full.png)

Tam yük OHD için delta ekstraksiyonundan farklı seçenekler seçin:

- OHD'de: **Verileri Silmek ve Kayıtları Eklemek**için **Çıkarma** seçeneğini ayarlayın. Aksi takdirde, dtp'yi bir BW işlem zincirinde tekrarladiğinizde veriler birçok kez ayıklanır.

- DTP'de: **Çıkarma Modunu** **Tam**olarak ayarlayın. Bu resimde görüldüğü gibi, OHD oluşturulduktan hemen sonra otomatik olarak oluşturulan DTP'yi **Delta'dan** **Full'a** değiştirmeniz gerekir:

   !["Tam" çıkarma için yapılandırılan SAP BW OHD iletişim kutusu oluşturma](media/load-sap-bw-data/create-sap-bw-ohd-full2.png)

- Veri Fabrikası'nın BW Açık Hub bağlayıcısında: **Son isteği hariç**tut.'u kapatın. Aksi takdirde, hiçbir şey ayıklanır.

Genellikle tam DTP'yi el ile çalıştırın. Veya, tam DTP için bir işlem zinciri oluşturabilirsiniz. Genellikle mevcut işlem zincirlerinizden bağımsız ayrı bir zincirdir. Her iki durumda da, *Veri Fabrikası kopyasını kullanarak çıkarma başlatmadan önce DTP'nin tamamlandığından emin olun.* Aksi takdirde, yalnızca kısmi veriler kopyalanır.

### <a name="run-delta-extraction-the-first-time"></a>Delta çıkarma ilk kez çalıştırın

İlk delta çıkarma teknik olarak tam bir *çıkarmadır.* Varsayılan olarak, SAP BW Open Hub bağlayıcısı verileri kopyaladığında son isteği hariç tutar. İlk delta çıkarma işlemi için, sonraki Bir DTP ayrı bir istek kimliği ile tabloda delta veri üretene kadar Veri Fabrikası kopyalama etkinliği tarafından hiçbir veri ayıklanır. Bu senaryoyu önlemenin iki yolu vardır:

- İlk delta çıkarma için **Son İstek Dışıat** seçeneğini kapatın. Delta çıkarma işlemine başlamadan önce ilk delta DTP'nin bittiğine emin olun.
-  Bir sonraki bölümde açıklandığı gibi delta çıkarma yeniden senkronize etmek için yordamı kullanın.

### <a name="resync-delta-extraction"></a>Resync delta çıkarma

Aşağıdaki senaryolar SAP BW küplerinde verileri değiştirir, ancak delta DTP tarafından dikkate alınmaz:

- SAP BW seçici silme (herhangi bir filtre koşulu kullanılarak satırlar)
- SAP BW istek silme (hatalı isteklerin)

SAP Open Hub Hedefi veri mart kontrollü bir veri hedefi değildir (2015'ten beri tüm SAP BW destek paketlerinde). Bu nedenle, OHD'deki verileri değiştirmeden verileri bir küpten silebilirsiniz. Daha sonra küpün verilerini Veri Fabrikası ile yeniden eşitlemelisiniz:

1. Veri Fabrikası'nda tam bir çıkarma çalıştırın (SAP'de tam bir DTP kullanarak).
2. Delta DTP için Aç Hub tablosundaki tüm satırları silin.
3. Delta DTP'nin durumunu **Fetched**olarak ayarlayın.

Bundan sonra, sonraki tüm delta DTP'ler ve Veri Fabrikası delta ekstraksiyonları beklendiği gibi çalışır.

Delta DTP'nin durumunu **Fetched**olarak ayarlamak için delta DTP'yi el ile çalıştırmak için aşağıdaki seçeneği kullanabilirsiniz:

    *No Data Transfer; Delta Status in Source: Fetched*

## <a name="next-steps"></a>Sonraki adımlar

SAP BW Open Hub konektör desteği hakkında bilgi edinin:

> [!div class="nextstepaction"]
>[SAP İş Ambarı Açık Hub konektörü](connector-sap-business-warehouse-open-hub.md)

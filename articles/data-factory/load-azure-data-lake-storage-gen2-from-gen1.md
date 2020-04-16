---
title: Azure Veri Gölü Depolama Gen1'den Gen2'ye verileri kopyalama
description: Azure Veri Gölü Depolama Gen1'den Gen2'ye kadar verileri kopyalamak için Azure Veri Fabrikası'nı kullanın
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 05/13/2019
ms.openlocfilehash: 6655510a4cfdb88e98319c7fc26c7ae83255bb6f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415818"
---
# <a name="copy-data-from-azure-data-lake-storage-gen1-to-gen2-with-azure-data-factory"></a>Azure Veri Fabrikası ile Azure Veri Gölü Depolama Gen1'den Gen2'ye verileri kopyalama

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Azure Veri Gölü Depolama Gen2, [Azure Blob depolamaalanına](../storage/blobs/storage-blobs-introduction.md)dahil edilmiş büyük veri analitiğine adanmış bir dizi özelliktir. Hem dosya sistemi hem de nesne depolama paradigmalarını kullanarak verilerinizi arabirim de kullanabilirsiniz.

Şu anda Azure Veri Gölü Depolama Gen1 kullanıyorsanız, Azure Veri Depolama Gen1'deki verileri Azure Veri Fabrikası'nı kullanarak Gen2'ye kopyalayarak Azure Veri Gölü Depolama Gen2'yi değerlendirebilirsiniz.

Azure Veri Fabrikası, tam olarak yönetilen bulut tabanlı bir veri tümleştirme hizmetidir. Hizmeti, gölü zengin şirket içi ve bulut tabanlı veri depolarından gelen verilerle doldurmak ve analitik çözümlerinizi oluştururken zamandan tasarruf etmek için kullanabilirsiniz. Desteklenen bağlayıcıların listesi için [Desteklenen veri depolarının](copy-activity-overview.md#supported-data-stores-and-formats)tablosuna bakın.

Azure Veri Fabrikası, ölçeklendirilen, yönetilen bir veri hareketi çözümü sunar. Veri Fabrikası'nın ölçekleme mimarisi sayesinde, verileri yüksek bir iş seviyesinde alabilir. Daha fazla bilgi için [bkz.](copy-activity-performance.md)

Bu makalede, Azure Veri Gölü Depolama Gen1'deki verileri Azure Veri Gölü Depolama Gen2'ye kopyalamak için Veri Fabrikası kopyalama veri aracını nasıl kullanacağınızı gösterilmektedir. Diğer veri depolarından veri kopyalamak için benzer adımları izleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
* Azure Veri Gölü Depolama Gen1 hesabı içinde veri bulunan.
* Veri Gölü Depolama Gen2 etkin leştirilmiş Azure Depolama hesabı. Depolama hesabınız yoksa, [bir hesap oluşturun.](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Sol menüde > kaynak**Veri + Analitik** > **Veri Fabrikası** **Oluştur'u**seçin.
   
   ![Yeni bölmede Veri Fabrikası seçimi](./media/quickstart-create-data-factory-portal/new-azure-data-factory-menu.png)

2. Yeni **veri fabrikası** sayfasında, aşağıdaki resimde gösterilen alanlar için değerler sağlayın: 
      
   ![Yeni Veri fabrikası sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/new-azure-data-factory.png)
 
    * **Ad**: Azure veri fabrikanız için benzersiz bir ad girin. "Veri fabrikası adı \"LoadADLSDemo\" kullanılamıyor" hatasını alırsanız, veri fabrikası için farklı bir ad girin. Örneğin, _**adınız**_**ADFTutorialDataFactory** adını kullanın. Veri fabrikasını yeniden oluşturun. Data Factory yapıtlarını adlandırma kuralları için bkz. [Data Factory adlandırma kuralları](naming-rules.md).
    * **Abonelik**: Veri fabrikasını oluşturmak için Azure aboneliğinizi seçin. 
    * **Kaynak Grubu**: Açılan listeden varolan bir kaynak grubu seçin. **Ayrıca yeni oluştur** seçeneğini belirleyebilir ve bir kaynak grubunun adını girebilirsiniz. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md). 
    * **Sürüm**: **V2**seçin.
    * **Konum**: Veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları başka konumlarda ve bölgelerde olabilir. 

3. **Oluştur**’u seçin.
4. Oluşturma tamamlandıktan sonra veri fabrikanıza gidin. Aşağıdaki resimde gösterildiği gibi **Veri Fabrikası** ana sayfasını görürsünüz: 
   
   ![Data factory giriş sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/data-factory-home-page.png)

5. Veri Tümleştirme uygulamasını ayrı bir sekmede başlatmak için **Yazar & Monitör** döşemesini seçin.

## <a name="load-data-into-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. Nesil'e verileri yükleme

1. **Başlat** sayfasında, kopya veri aracını başlatmak için **Veri Kopyala'yı** seçin. 

   ![Veri araç döşemesi kopyalama](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-tile.png)
2. **Özellikler** sayfasında, **Görev adı** alanı için **CopyFromADLSGen1ToGen2'yi** belirtin. **İleri**’yi seçin.

    ![Özellikler sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-data-tool-properties-page.png)
3. Kaynak **veri deposu** sayfasında + **Yeni bağlantı oluştur'u**seçin.

    ![Kaynak veri deposu sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page.png)
    
4. Bağlayıcı galerisinden **Azure Data Lake Storage 1. Nesil**'i seçin ve sonra da **Devam**'ı seçin.
    
    ![Kaynak veri deposu Azure Veri Gölü Depolama Gen1 sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/source-data-store-page-adls-gen1.png)
    
5. Azure **Veri Gölü Depolama Gen1 bağlantı** sayfasında aşağıdaki adımları izleyin:

   a. Hesap adı için Veri Gölü Depolama Gen1'inizi seçin ve **Kiracı'yı**belirtin veya doğrulayın.
  
   b. Ayarları doğrulamak için **Test bağlantısını** seçin. Ardından **Son**’u seçin.
  
   c. Yeni bir bağlantı oluşturulduğunu görüyorsunuz. **İleri**’yi seçin.
   
   > [!IMPORTANT]
   > Bu gezinmede, Azure Veri Gölü Depolama Gen1'inizin kimliğini doğrulamak için Azure kaynakları için yönetilen bir kimlik kullanırsınız. Yönetilen kimliğe Azure Veri Gölü Depolama Gen1'de uygun izinleri vermek için [aşağıdaki yönergeleri](connector-azure-data-lake-store.md#managed-identity)izleyin.
   
   ![Azure Veri Gölü Depolama Gen1 hesabını belirtin](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen1-account.png)
      
6. Giriş **dosyasını veya klasörü** seçin sayfasında, kopyalamak istediğiniz klasöre ve dosyaya göz atın. Klasörü veya dosyayı seçin ve **Seç'i**seçin.

    ![Girdi dosyasını veya klasörünü seçin](./media/load-azure-data-lake-storage-gen2-from-gen1/choose-input-folder.png)

7. **Kopya dosyaları özyinelemeli** ve **İkili kopyalama** seçeneklerini seçerek kopyalama davranışını belirtin. **İleri**’yi seçin.

    ![Çıktı klasörünü belirtin](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-binary-copy.png)
    
8. Hedef **veri deposu** sayfasında, **+ Yeni bağlantı** > oluştur**Azure Veri Gölü Depolama Gen2** > **Devam'ı**seçin.

    ![Hedef veri deposu sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/destination-data-storage-page.png)

9. Azure **Veri Gölü Depolama Gen2 bağlantı** sayfasında aşağıdaki adımları izleyin:

   a. **Depolama hesabı adı** açılan listesinden Veri Gölü Depolama Gen2 özellikli hesabınızı seçin.
   
   b. **Son**'u seçerek bağlantıyı oluşturun. Ardından **İleri'yi**seçin.
   
   ![Azure Veri Gölü Depolama Gen2 hesabını belirtin](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-account.png)

10. Çıktı **dosyasını veya klasörünü seç** sayfasında, çıkış klasörü adı olarak **copyfromlsgen1** girin ve **İleri'yi**seçin. Veri Fabrikası, kopya sırasında ilgili Azure Veri Gölü Depolama Gen2 dosya sistemini ve alt klasörlerini, yoksa oluşturur.

    ![Çıktı klasörünü belirtin](./media/load-azure-data-lake-storage-gen2-from-gen1/specify-adls-gen2-path.png)

11. Varsayılan ayarları kullanmak için **Ayarlar** sayfasında **İleri**'yi seçin.

12. **Özet** sayfasında, ayarları gözden geçirin ve **İleri'yi**seçin.

    ![Özet sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/copy-summary.png)
13. Dağıtım **sayfasında,** ardışık izlemeyi izlemek için **Monitör'ü** seçin.

    ![Dağıtım sayfası](./media/load-azure-data-lake-storage-gen2-from-gen1/deployment-page.png)
14. Soldaki **İzleyici** sekmesinin otomatik olarak seçildiğine dikkat edin. **Eylemler** sütunu, etkinlik çalıştırması ayrıntılarını görüntüleme ve işlem hattını yeniden çalıştırma bağlantılarını içerir.

    ![İşlem hattı çalıştırmalarını izleme](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-pipeline-runs.png)

15. Ardışık işlem hattı çalışmasıyla ilişkili etkinlik çalışanlarını görüntülemek **için, Eylemler** sütunundaki **Etkinlik Çalışır'ı Görüntüle** bağlantısını seçin. İşlem hattında yalnızca bir etkinlik (kopyalama etkinliği) olduğundan tek bir girdi görürsünüz. Ardışık hatlar çalışır görünümüne geri dönmek için, üstteki **Ardışık Hatlar** bağlantısını seçin. Listeyi yenilemek için **Yenile**’yi seçin. 

    ![Etkinlik çalıştırmalarını izleme](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-runs.png)

16. Her kopyalama etkinliğinin yürütme ayrıntılarını izlemek için, etkinlik izleme görünümündeki **Eylemler** altında **Ayrıntılar** bağlantısını (gözlük görüntüsü) seçin. Kaynaktan lavaboya kopyalanan veri hacmi, veri aktarımı, ilgili süreye sahip yürütme adımları ve kullanılan yapılandırmalar gibi ayrıntıları izleyebilirsiniz.

    ![Etkinlik çalıştırma ayrıntılarını izleme](./media/load-azure-data-lake-storage-gen2-from-gen1/monitor-activity-run-details.png)

17. Verilerin Azure Veri Gölü Depolama Gen2 hesabınıza kopyalandığından doğrulayın.

## <a name="best-practices"></a>En iyi uygulamalar

Azure Veri Gölü Depolama Gen1'den Genel Olarak Azure Veri Gölü Depolama Gen2'ye yükseltmeyi değerlendirmek [için](../storage/blobs/data-lake-storage-migrate-gen1-to-gen2.md)bkz. Aşağıdaki bölümler, Veri Gölü Depolama Gen1'den Data Lake Storage Gen2'ye veri yükseltmesi için Veri Fabrikası'nı kullanmak için en iyi uygulamaları tanıyın.

### <a name="data-partition-for-historical-data-copy"></a>Geçmiş veri kopyalamaiçin veri bölümü

- Veri Gölü Depolama Gen1'deki toplam veri boyutunuz 30 TB'den küçükse ve dosya sayısı 1 milyondan azsa, tüm verileri tek bir kopyalama etkinliği çalışmasında kopyalayabilirsiniz.
- Kopyalamanız gereken daha büyük miktarda veri varsa veya toplu olarak veri geçişini yönetme ve her birinin belirli bir zaman dilimi içinde tamamlanmasını sağlamak için esnekliği istiyorsanız, verileri bölümleyin. Bölümleme de beklenmeyen bir sorun riskini azaltır.

Uçtan uca çözümü doğrulamak ve ortamınızdaki kopya iş hacmini test etmek için bir kavram kanıtı kullanın. Kavram kanıtı adımları: 

1. Veri Gölü Depolama Gen1'den Veri Gölü Depolama Gen2'ye kadar birkaç veri TB'sini kopyalamak ve bir kopya performans taban çizgisini elde etmek için tek bir kopyalama etkinliği yle tek bir Veri Fabrikası ardışık hattı oluşturun. Veri [tümleştirme birimleri (DIUs)](copy-activity-performance-features.md#data-integration-units) ile 128 olarak başlayın. 
2. Adım 1'de aldığınız kopya iş başına bağlı olarak, tüm veri geçişi için gereken tahmini süreyi hesaplayın. 
3. (İsteğe bağlı) Bir denetim tablosu oluşturun ve geçirilecek dosyaları bölmek için dosya filtresini tanımlayın. Dosyaları bölmenin yolu: 

    - Joker karakter filtresiyle klasör adına veya klasör adına göre bölümleme. Bu yöntemi öneriyoruz.
    - Bir dosyanın son değiştirilen süresine göre bölümleme.

### <a name="network-bandwidth-and-storage-io"></a>Ağ bant genişliği ve depolama G/Ç 

Data Lake Storage Gen1'den verileri okuyan ve Verileri Veri Gölü Depolama Gen2'ye yazan Veri Fabrikası kopyalama işlerinin eşzamanlılıklarını denetleyebilirsiniz. Bu şekilde, geçiş sırasında Veri Gölü Depolama Gen1'deki normal iş çalışmasını etkilememek için bu depolama G/Ç'deki kullanımı yönetebilirsiniz.

### <a name="permissions"></a>İzinler 

Veri Fabrikası'nda, [Veri Gölü Depolama Gen1 bağlayıcısı](connector-azure-data-lake-store.md) Azure kaynak kimlik doğrulaması için hizmet temeli ve yönetilen kimliği destekler. [Veri Gölü Depolama Gen2 bağlayıcısı,](connector-azure-data-lake-storage.md) Azure kaynak kimlik doğrulaması için hesap anahtarını, hizmet sorumlusu ve yönetilen kimliği destekler. Veri Fabrikası'nın ihtiyacınız olan tüm dosyaları veya erişim kontrol listelerinde (ALA'lar) gezinip kopyalayabilmelerini sağlamak için, sağladığınız hesap için tüm dosyaları erişmek, okumak veya yazmak ve isterseniz ALA'lar ayarlamak için yeterince yüksek izinler verin. Geçiş döneminde bir süper kullanıcı veya sahip rolü ver. 

### <a name="preserve-acls-from-data-lake-storage-gen1"></a>APARA'ları Veri Gölü Depolama Gen1'den Koruyun

Data Lake Storage Gen1'den Data Lake Storage Gen2'ye yükselttirirken ACD'leri veri dosyalarıyla birlikte çoğaltmak istiyorsanız, [Bkz.](connector-azure-data-lake-storage.md#preserve-acls) 

### <a name="incremental-copy"></a>Artımlı kopya 

Yalnızca Veri Gölü Depolama Gen1'den yalnızca yeni veya güncelleştirilmiş dosyaları yüklemek için çeşitli yaklaşımlar kullanabilirsiniz:

- Zaman bölümlenmiş klasör veya dosya adına göre yeni veya güncelleştirilmiş dosyaları yükleyin. Örnek /2019/05/13/*'dur.
- LastModifiedDate'e göre yeni veya güncelleştirilmiş dosyaları yükleyin.
- Yeni veya güncelleştirilmiş dosyaları herhangi bir üçüncü taraf aracı veya çözüme göre tanımlayın. Ardından, dosya veya klasör adını parametre veya tablo veya dosya üzerinden Veri Fabrikası ardışık hattına geçirin. 

Artımlı yükleme yapmak için uygun sıklık, Azure Veri Gölü Depolama Gen1'deki toplam dosya sayısına ve her seferinde yüklenecek yeni veya güncelleştirilmiş dosyaların hacmine bağlıdır. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Etkinlik özeti](copy-activity-overview.md)
> [Azure Veri Gölü Depolama Gen1 konektörü](connector-azure-data-lake-store.md)
> Azure Veri Gölü Depolama[Gen2 konektörü](connector-azure-data-lake-storage.md)
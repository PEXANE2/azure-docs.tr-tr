---
title: Azure SQL Denetimi
description: Veritabanı olaylarını bir denetim günlüğüne izlemek için Azure SQL veritabanı denetimini kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: conceptual
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 03/27/2020
ms.custom: azure-synapse
ms.openlocfilehash: 4e20129502e7538bd2f3354b75b33095970e1595
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81411858"
---
# <a name="azure-sql-auditing"></a>Azure SQL Denetimi

Azure SQL [Veritabanı](sql-database-technical-overview.md) ve [Azure Synapse Analytics](../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) için denetim veritabanı olaylarını izler ve bunları Azure depolama hesabınızda, Log Analytics çalışma alanınızda veya Etkinlik Hub'larınızdaki bir denetim günlüğüne yazar. 

Denetim şunları da sağlar:

- Mevzuatla uyumluluk, veritabanı etkinliğini anlama ve işletme sorunlarını veya şüpheli güvenlik ihlallerini işaret edebilecek farklılıklar ve anormal durumlar hakkında içgörü sahip olmanıza yardımcı olur.

- Uyumluluğu garanti etmese bile uyumluluk standartlarına uymayı sağlar ve kolaylaştırır. Standartlara uygunluğu destekleyen Azure programları hakkında daha fazla bilgi için, SQL Veritabanı uyumluluk sertifikalarının en güncel listesini bulabileceğiniz [Azure Güven Merkezi'ne](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) bakın.

> [!NOTE] 
> Bu konu hem Azure SQL Veritabanı hem de Azure Synapse Analytics veritabanları için geçerlidir. Basitlik için SQL Veritabanı, hem Azure SQL Veritabanı'na hem de Azure Synapse Analytics'e atıfta bulunularak kullanılır.

## <a name="overview"></a><a id="overview"></a>Genel Bakış

SQL veritabanı denetimini kullanarak:

- Seçili olayların denetim izini **koruyun.** Denetlenecek veritabanı eylemi kategorilerini tanımlayabilirsiniz.
- Veritabanı etkinliği hakkında **rapor.** Etkinlik ve olay raporlamaya hızlı bir başlangıç yapmak için önceden yapılandırılmış raporları ve bir panoyu kullanabilirsiniz.
- Raporları **analiz edin.** Şüpheli olayları, alışılmışın dışındaki etkinlikleri ve eğilimleri bulabilirsiniz.

> [!IMPORTANT]
> - Azure SQL Veritabanı denetimi kullanılabilirlik & performansı için optimize edilir. Çok yüksek etkinlik sırasında Azure SQL Veritabanı işlemlerin devam etmesine izin verir ve denetlenen bazı olayları kaydetmeyebilir.

#### <a name="auditing-limitations"></a>Denetim sınırlamaları

- **Premium depolama** şu anda **desteklenmez.**
- **Azure Veri Gölü Depolama Gen2 depolama hesabı** için **hiyerarşik ad alanı** şu anda **desteklenmez.**
- Duraklatılmış bir Azure **SQL Veri Ambarı'nda** denetimi etkinleştirme desteklenmez. Denetimi etkinleştirmek için Veri Ambarı'nı devam ettirin.

## <a name="define-server-level-vs-database-level-auditing-policy"></a><a id="server-vs-database-level"></a>Sunucu düzeyinde ve veritabanı düzeyinde denetim ilkesini tanımlama

Denetim ilkesi belirli bir veritabanı için veya varsayılan sunucu ilkesi olarak tanımlanabilir:

- Sunucu ilkesi, sunucudaki varolan ve yeni oluşturulan tüm veritabanları için geçerlidir.

- *Sunucu blob denetimi etkinse,* *her zaman veritabanıiçin geçerlidir.* Veritabanı denetim ayarlarıne bakılmaksızın veritabanı denetlenir.

- Veritabanı veya veri ambarı üzerinde blob denetimi etkinleştirme, sunucuda etkinleştirmeye ek olarak, geçersiz kılmaz veya sunucu blob denetim ayarlarından herhangi birini değiştirmez. *not* Her iki denetim de yan yana var olacaktır. Başka bir deyişle, veritabanı paralel olarak iki kez denetlenir; bir kez sunucu ilkesi ve bir kez veritabanı ilkesi tarafından.

   > [!NOTE]
   > Aşağıdakiler olmadıkça hem sunucu blob denetimi hem de veritabanı blob denetiminden kaçınmalısınız:
    > - Belirli bir veritabanı için farklı bir *depolama hesabı,* *bekletme süresi* veya *Günlük Analizi Çalışma Alanı* kullanmak istiyorsunuz.
    > - Sunucudaki diğer veritabanlarından farklı belirli bir veritabanı için olay türlerini veya kategorilerini denetlemek istiyorsunuz. Örneğin, yalnızca belirli bir veritabanı için denetleştirilmesi gereken tablo ekler olabilir.
   >
   > Aksi takdirde, yalnızca sunucu düzeyinde blob denetimini etkinleştirmenizi ve veritabanı düzeyinde denetimi tüm veritabanları için devre dışı bırakmanızı tavsiye ettik.

## <a name="set-up-auditing-for-your-server"></a><a id="setup-auditing"></a>Sunucunuz için denetim ayarlama

Varsayılan denetim ilkesi, veritabanına karşı yürütülen tüm sorguları ve depolanmış yordamları ve başarılı ve başarısız girişleri denetleyecek tüm eylemleri ve aşağıdaki eylem gruplarını içerir:
  
  - BATCH_COMPLETED_GROUP
  - SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP
  - FAILED_DATABASE_AUTHENTICATION_GROUP
  
Azure PowerShell bölümünü [kullanarak SQL veritabanı denetimini yönet'te](#manage-auditing) açıklandığı gibi PowerShell'i kullanarak farklı eylem türleri ve eylem grupları için denetim yapılandırabilirsiniz.

Azure SQL Veritabanı Denetimi, bir denetim kaydında karakter alanları için 4000 karakter veri depolar. **Denetlenebilir** bir eylemden döndürülen bildirim veya **data_sensitivity_information** değerleri 4000'den fazla karakter içeriyorsa, ilk 4000 karakterin dışındaki veriler kesilir **ve denetlenmez.**
Aşağıdaki bölümde Azure portalını kullanarak denetim yapılandırması açıklanmaktadır.

1. [Azure portalına](https://portal.azure.com)gidin.
2. SQL veritabanı/sunucu bölmenizde Güvenlik başlığı altında **Denetime** gidin.
3. Bir sunucu denetim ilkesi ayarlamayı tercih ederseniz, veritabanı denetim sayfasındasunucu **ayarlarını görüntüle** bağlantısını seçebilirsiniz. Daha sonra sunucu denetim ayarlarını görüntüleyebilir veya değiştirebilirsiniz. Sunucu denetim ilkeleri, bu sunucudaki varolan ve yeni oluşturulan tüm veritabanları için geçerlidir.

    ![Gezinti bölmesi](./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png)

4. Veritabanı düzeyinde denetimi etkinleştirmeyi tercih ederseniz, **Denetim'i** **ON'a**geçirin. Sunucu denetimi etkinse, veritabanı tarafından yapılandırılan denetim sunucu denetimiyle yan yana bulunur.

5. Denetim günlüklerinin yazıldığı yeri yapılandırmak için birden çok seçeneğiniz vardır. Günlükleri bir Azure depolama hesabına, Azure Monitor günlüklerine (önizleme) tüketim için Günlük Analizi çalışma alanına veya olay hub'ı (önizleme) kullanarak tüketim için etkinlik merkezine yazabilirsiniz. Bu seçeneklerin herhangi bir birleşimini yapılandırabilirsiniz ve denetim günlükleri her birine yazılır.
  
   ![depolama seçenekleri](./media/sql-database-auditing-get-started/auditing-select-destination.png)
   
### <a name=""></a><a id="audit-storage-destination">Depolama hedefine denetim</a>

Denetim günlüklerini bir depolama hesabına yapılandırmak için **Depolama** ve Açık **Depolama ayrıntılarını**seçin. Günlüklerin kaydedilen Azure depolama hesabını seçin ve ardından bekletme dönemini seçin. Ardından **Tamam**'a tıklayın. Bekletme döneminden eski günlükler silinir.

- Bekletme süresi için varsayılan değer 0 'dir (sınırsız bekletme). Denetim için depolama hesabını yapılandırırken **Depolama ayarlarında** **Bekletme (Günler)** kaydırıcısını taşıyarak bu değeri değiştirebilirsiniz.
  - Bekletme süresini 0'dan (sınırsız bekletme) başka bir değere değiştirirseniz, saklamanın yalnızca bekletme değeri değiştirildikten sonra yazılan günlükler için geçerli olacağını (bekletme sınırsız olarak ayarlanan dönemde yazılan günlükler, bekletme etkinleştirildikten sonra bile korunur).

  ![depolama hesabı](./media/sql-database-auditing-get-started/auditing_select_storage.png)

#### <a name="remarks"></a>Açıklamalar

- Denetim günlükleri, Azure aboneliğinizdeki Azure Blob depolama alanında **Append Blobs'a** yazılır
- Sunucu veya veritabanı düzeyindedenetim olayları için değişmez bir günlük deposunu yapılandırmak için [Azure Depolama tarafından sağlanan yönergeleri](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutability-policies-manage#enabling-allow-protected-append-blobs-writes) izleyin (Lütfen sabit blob depolamasını yapılandırırken ek **eklere izin ver'i** seçtiğinizden emin olun).
- Bir Azure Depolama hesabına VNet veya güvenlik duvarının arkasına denetim günlükleri yazabilirsiniz. Belirli talimatlar için [bkz.](create-auditing-storage-account-vnet-firewall.md)
- Denetim ayarlarınızı yapılandırıldıktan sonra, yeni tehdit algılama özelliğini açabilir ve güvenlik uyarıları alacak şekilde e-postaları yapılandırabilirsiniz. Tehdit algılamayı kullandığınızda, olası güvenlik tehditlerini gösterebilen anormal veritabanı etkinlikleri hakkında proaktif uyarılar alırsınız. Daha fazla bilgi için bkz: [Tehdit algılama ile başlarken.](sql-database-threat-detection-get-started.md)
- Günlük biçimi, depolama klasörünün hiyerarşisi ve adlandırma kuralları hakkında ayrıntılı bilgi için [Blob Audit Log Format Reference](https://go.microsoft.com/fwlink/?linkid=829599)bölümüne bakın.
- AAD Kimlik Doğrulaması kullanırken, başarısız oturum açma kayıtları SQL denetim günlüğünde *görünmez.* Başarısız oturum açma denetim kayıtlarını görüntülemek için, bu olayların ayrıntılarını günlüğe kaydeden [Azure Etkin Dizin portalını]( ../active-directory/reports-monitoring/reference-sign-ins-error-codes.md)ziyaret etmeniz gerekir.
- [Salt Okunur Yinelemelerde](sql-database-read-scale-out.md) denetim otomatik olarak etkinleştirilir. Depolama klasörlerinin hiyerarşisi, adlandırma kuralları ve günlük biçimi hakkında daha fazla bilgi için [SQL Veritabanı Denetim Günlüğü Biçimi'ne](sql-database-audit-log-format.md)bakın. 

### <a name=""></a><a id="audit-log-analytics-destination">Log Analytics hedefine denetim</a>
  
Yazma denetim günlüklerini bir Log Analytics çalışma alanına yapılandırmak için **Log Analytics (Önizleme) seçeneğini** seçin ve **Log Analytics ayrıntılarını**açın. Günlüklerin yazıldığı Log Analytics çalışma alanını seçin veya oluşturun ve **ardından Tamam'ı**tıklatın.
    
  > [!WARNING]
   > Log Analytics denetiminin etkinleştirilmesi, yutma oranlarına bağlı olarak maliyete tabi olacaktır. Lütfen bu [seçeneği](https://azure.microsoft.com/pricing/details/monitor/)kullanmanın ilişkili maliyetine dikkat edin veya denetim günlüklerini bir Azure depolama hesabında depolamayı düşünün.
   
   ![LogAnalyticsworkspace](./media/sql-database-auditing-get-started/auditing_select_oms.png)

### <a name=""></a><a id="audit-event-hub-destination">Olay Hub hedefine denetim</a>

> [!WARNING]
> Üzerinde SQL havuzu bulunan bir sunucuda denetimi etkinleştirmek, **SQL havuzunun yeniden başlatılmasına ve yeniden duraklatılmasına** neden olur ve bu da faturalandırma ücretlerine neden olabilir.
> Duraklatılmış bir SQL havuzunda denetimi etkinleştirmek mümkün değildir. Etkinleştirmek için SQL havuzunu duraklatın.

Denetim günlüklerini bir olay hub'ına yapılandırmak için **Olay Hub'ı (Önizleme) ve** Olay Hub ayrıntılarını açın'ı seçin. **Event Hub details** Günlüklerin yazıldığı etkinlik merkezini seçin ve **ardından Tamam'ı**tıklatın. Olay hub'ı veritabanınız ve sunucunuzla aynı bölgede olduğundan emin olun.

   ![Eventhub](./media/sql-database-auditing-get-started/auditing_select_event_hub.png)

## <a name="analyze-audit-logs-and-reports"></a><a id="subheading-3"></a>Denetim günlüklerini ve raporlarını analiz edin

Azure Monitor günlüklerine denetim günlükleri yazmayı seçtiyseniz:

- Azure [portalını](https://portal.azure.com)kullanın.  İlgili veritabanını açın. Veritabanının **Denetim** sayfasının üst kısmında denetim **günlüklerini görüntüle'yi**tıklatın.

    ![denetim günlüklerini görüntüleme](./media/sql-database-auditing-get-started/auditing-view-audit-logs.png)

- Ardından, günlükleri görüntülemenin iki yolu vardır:
    
    **Denetim kayıtları** sayfasının üst **kısmındaki Günlük Analitiği'ni** tıklattığınızda, zaman aralığını ve arama sorgusunu özelleştirebileceğiniz Log Analytics çalışma alanında Günlükler görünümü açılır.
    
    ![Log Analytics çalışma alanında açık](./media/sql-database-auditing-get-started/auditing-log-analytics.png)

    **Denetim kayıtları** sayfasının üst kısmındaki **Görünüm panosunu** tıklattığınızda, Güvenlik İstatistikleri, Hassas Verilere Erişim ve daha fazlasını inceebileceğiniz denetim günlükleri bilgilerini görüntüleyen bir pano açılır. Bu pano, verileriniz için güvenlik öngörüleri elde etmenize yardımcı olmak için tasarlanmıştır.
    Ayrıca zaman aralığını ve arama sorgusunu özelleştirebilirsiniz. 
    ![Günlük Analiz Panosu'ni Görüntüle](media/sql-database-auditing-get-started/auditing-view-dashboard.png)

    ![Günlük Analiz Panosu](media/sql-database-auditing-get-started/auditing-log-analytics-dashboard.png)

    ![Log Analytics Güvenlik Öngörüleri](media/sql-database-auditing-get-started/auditing-log-analytics-dashboard-data.png)
 

- Alternatif olarak, Log Analytics blade'den denetim günlüklerine de erişebilirsiniz. Log Analytics çalışma alanınızı açın ve **Genel** bölümün altında **Günlükler'i**tıklatın. Denetim günlüğünü görüntülemek için *"SQLSecurityAuditEvents" araması* gibi basit bir sorguyla başlayabilirsiniz.
    Buradan, denetim günlüğü verilerinizde gelişmiş aramalar yapmak için [Azure Monitor günlüklerini](../log-analytics/log-analytics-log-search.md) de kullanabilirsiniz. Azure Monitor günlükleri, tüm iş yükleri ve sunucularınızda milyonlarca kaydı kolayca analiz etmek için tümleşik arama ve özel panoları kullanarak size gerçek zamanlı operasyonel öngörüler sağlar. Azure Monitor günlükleri arama dili ve komutları hakkında daha fazla yararlı bilgi için Azure [Monitor günlükleri arama başvurusuna](../log-analytics/log-analytics-log-search.md)bakın.

Denetim günlüklerini Event Hub'a yazmayı seçtiyseniz:

- Olay Hub'ından denetim günlükleri verilerini tüketmek için, olayları tüketmek ve bunları bir hedefe yazmak için bir akış ayarlamanız gerekir. Daha fazla bilgi için Azure [Etkinlik Hub'ları Belgeleri'ne](../event-hubs/index.yml)bakın.
- Event Hub'daki denetim günlükleri [Apache Avro](https://avro.apache.org/) etkinliklerinin gövdesinde yakalanır ve UTF-8 kodlaması ile JSON biçimlendirmesi kullanılarak depolanır. Denetim günlüklerini okumak için, [Avro Araçları'nı](../event-hubs/event-hubs-capture-overview.md#use-avro-tools) veya bu biçimi işleyen benzer araçları kullanabilirsiniz.

Bir Azure depolama hesabına denetim günlükleri yazmayı seçtiyseniz, günlükleri görüntülemek için kullanabileceğiniz birkaç yöntem vardır:

- Denetim günlükleri kurulum sırasında seçtiğiniz hesapta toplanır. [Azure Depolama Gezgini](https://storageexplorer.com/)gibi bir araç kullanarak denetim günlüklerini keşfedebilirsiniz. Azure depolama alanında, denetim günlükleri **sqldbauditlogs**adlı bir kapsayıcı içinde blob dosyaları koleksiyonu olarak kaydedilir. Depolama klasörlerinin hiyerarşisi, adlandırma kuralları ve günlük biçimi hakkında daha fazla bilgi için [SQL Veritabanı Denetim Günlüğü Biçimi'ne](https://go.microsoft.com/fwlink/?linkid=829599)bakın.

- Azure [portalını](https://portal.azure.com)kullanın.  İlgili veritabanını açın. Veritabanının **Denetim** sayfasının üst kısmında denetim **günlüklerini görüntüle'yi**tıklatın.

    ![Gezinti bölmesi](./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png)

    **Denetim kayıtları** açılır ve bu kayıtlar dan görünüm elde edebilirsiniz.

  - **Denetim kayıtları** sayfasının üst kısmında **filtreyi** tıklatarak belirli tarihleri görüntüleyebilirsiniz.
  - **Denetim Kaynağı'nı**değiştirerek sunucu *denetim ilkesi* tarafından oluşturulan denetim kayıtları ile veritabanı *denetim ilkesi* arasında geçiş yapabilirsiniz.
  - **Yalnızca SQL enjeksiyonları** onay kutusunun denetim kayıtlarını göster'i işaretleyerek yalnızca SQL enjeksiyonla ilgili denetim kayıtlarını görüntüleyebilirsiniz.

       ![Gezinti bölmesi]( ./media/sql-database-auditing-get-started/8_auditing_get_started_blob_audit_records.png)

- Denetim günlüğü verilerini tabular biçiminde döndürmek için sistem işlevini **sys.fn_get_audit_file** (T-SQL) kullanın. Bu işlevi kullanma hakkında daha fazla bilgi için [sys.fn_get_audit_file'](/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql)a bakın.

- SQL Server Management Studio'da **Denetim Dosyalarını Birleştir'i** kullanın (SSMS 17 ile başlayarak):
    1. SSMS menüsünden **Dosya** > **Aç** > **Birleştirme Denetim Dosyaları'nı**seçin.

        ![Gezinti bölmesi](./media/sql-database-auditing-get-started/9_auditing_get_started_ssms_1.png)
    2. **Denetim Dosyaları Ekle** iletişim kutusu açılır. Denetim dosyalarını yerel bir diskten birleştirip birleştirmeyeceğinizi veya Azure Depolama'dan içeri aktarıp aktarmayacağını seçmek için **Ekle** seçeneklerinden birini seçin. Azure Depolama bilgilerinizi ve hesap anahtarınızı sağlamanız gerekir.

    3. Birleştirilecek tüm dosyalar eklendikten sonra birleştirme işlemini tamamlamak için **Tamam'ı** tıklatın.

    4. Birleştirilmiş dosya, görüntüleyip çözümlediğiniz ve xel veya CSV dosyasına veya tabloya aktarabileceğiniz SSMS'te açılır.

- Güç BI kullanın. Power BI'de denetim günlüğü verilerini görüntüleyebilir ve çözümleyebilirsiniz. Daha fazla bilgi ve indirilebilir şablona erişmek için [bkz.](https://blogs.msdn.microsoft.com/azuresqldbsupport/20../../sql-azure-blob-auditing-basic-power-bi-dashboard/)
- Günlük dosyalarını Azure Depolama blob kapsayıcınızdan portal üzerinden veya [Azure Depolama Gezgini](https://storageexplorer.com/)gibi bir araç kullanarak indirin.
  - Bir günlük dosyasını yerel olarak indirdikten sonra, SSMS'teki günlükleri açmak, görüntülemek ve çözümlemek için dosyayı çift tıklatın.
  - Azure Depolama Gezgini üzerinden aynı anda birden çok dosya indirebilirsiniz. Bunu yapmak için, belirli bir alt klasörü sağ tıklatın ve yerel bir klasörde kaydetmek için **Kaydet'i** seçin.

- Ek yöntemler:

  - Birkaç dosya veya günlük dosyaları içeren bir alt klasör indirdikten sonra, bunları daha önce açıklanan SSMS Birleştirme Denetim Dosyaları yönergelerinde açıklandığı gibi yerel olarak birleştirebilirsiniz.
  - Blob denetim günlüklerini programlı olarak görüntüleyin:

    - PowerShell'i kullanarak [Genişletilmiş Olaylar Dosyalarını Sorgula.](https://sqlscope.wordpress.com/20../../reading-extended-event-files-using-client-side-tools-only/)

## <a name="production-practices"></a><a id="production-practices"></a>Üretim uygulamaları

<!--The description in this section refers to preceding screen captures.-->

#### <a name="auditing-geo-replicated-databases"></a>Coğrafi olarak çoğaltılan veritabanlarını denetleme

Coğrafi olarak çoğaltılan veritabanlarında, birincil veritabanında denetimi etkinleştirdiğinizde ikincil veritabanı nın aynı denetim ilkesi ne olur. Birincil veritabanından bağımsız olarak **ikincil sunucuda**denetimi etkinleştirerek ikincil veritabanında denetim kurmak da mümkündür.

- Sunucu düzeyi (**önerilir**): Hem **birincil sunucuda** hem de **ikincil sunucuda** denetimi açın - birincil ve ikincil veritabanlarının her biri kendi sunucu düzeyindeki ilkesine göre bağımsız olarak denetlenir.
- Veritabanı düzeyi: İkincil veritabanları için veritabanı düzeyinde denetim yalnızca Birincil veritabanı denetim ayarlarından yapılandırılabilir.
  - Denetim, sunucuda değil, *birincil veritabanının kendisinde*etkinleştirilmelidir.
  - Birincil veritabanında denetim etkinleştirildikten sonra, ikincil veritabanında da etkin hale gelir.

    >[!IMPORTANT]
    >Veritabanı düzeyinde denetim ile, ikincil veritabanı için depolama ayarları birincil veritabanı ile aynı olacak, bölgeler arası trafiğe neden. Yalnızca sunucu düzeyinde denetimi etkinleştirmenizi ve veritabanı düzeyinde denetimi tüm veritabanları için devre dışı bırakmanızı öneririz.

#### <a name="storage-key-regeneration"></a>Depolama anahtarı rejenerasyonu

Üretimde, depolama anahtarlarınızı düzenli aralıklarla yenileme olasılığınız yüksektir. Azure depolama alanına denetim günlükleri yazarken, anahtarlarınızı yenilerken denetim ilkenizi yeniden kaydetmeniz gerekir. İşlem aşağıdaki gibidir:

1. Açık **Depolama Ayrıntıları**. Depolama **Erişim Anahtarı** kutusunda **İkincil'i**seçin ve **Tamam'ı**tıklatın. Ardından denetim yapılandırma sayfasının üst kısmında **Kaydet'i** tıklatın.

    ![Gezinti bölmesi](./media/sql-database-auditing-get-started/5_auditing_get_started_storage_key_regeneration.png)
2. Depolama yapılandırma sayfasına gidin ve birincil erişim anahtarını yeniden oluşturun.

    ![Gezinti bölmesi](./media/sql-database-auditing-get-started/6_auditing_get_started_regenerate_key.png)
3. Denetim yapılandırma sayfasına geri dön, depolama erişim anahtarını ikincilden birincile çevir ve sonra **Tamam'ı**tıklatın. Ardından denetim yapılandırma sayfasının üst kısmında **Kaydet'i** tıklatın.
4. Depolama yapılandırma sayfasına geri dön ve ikincil erişim anahtarını yeniden oluşturun (bir sonraki anahtarın yenileme döngüsüne hazırlık olarak).

## <a name="manage-azure-sql-server-and-database-auditing"></a><a id="manage-auditing"></a>Azure SQL Server ve Veritabanı denetimini yönetme

#### <a name="using-azure-powershell"></a>Azure PowerShell’i kullanma

**PowerShell cmdlets (ek filtreleme için WHERE yan tümcesi desteği dahil)**:

- [Veritabanı Denetim İlkesi Oluşturma veya Güncelleştirme (Set-AzSqlDatabaseAudit)](/powershell/module/az.sql/set-azsqldatabaseaudit)
- [Sunucu Denetim İlkesi Oluştur veya Güncelleştir (Set-AzSqlServerAudit)](/powershell/module/az.sql/set-azsqlserveraudit)
- [Veritabanı Denetim Politikası Alın (Get-AzSqlDatabaseAudit)](/powershell/module/az.sql/get-azsqldatabaseaudit)
- [Sunucu Denetim İlkesi Alın (Get-AzSqlServerAudit)](/powershell/module/az.sql/get-azsqlserveraudit)
- [Veritabanı Denetim İlkesini Kaldır (Remove-AzSqlDatabaseAudit)](/powershell/module/az.sql/remove-azsqldatabaseaudit)
- [Sunucu Denetim İlkesini Kaldır (Remove-AzSqlServerAudit)](/powershell/module/az.sql/remove-azsqlserveraudit)

Komut dosyası örneği için bkz. [PowerShell kullanarak denetimi ve tehdit algılamayı yapılandırın.](scripts/sql-database-auditing-and-threat-detection-powershell.md)

#### <a name="using-rest-api"></a>REST API kullanma

**REST API**:

- [Veritabanı Denetim İlkesi Oluşturma veya Güncelleştirme](/rest/api/sql/database%20auditing%20settings/createorupdate)
- [Sunucu Denetim İlkesi Oluşturma veya Güncelleştirme](/rest/api/sql/server%20auditing%20settings/createorupdate)
- [Veritabanı Denetim İlkesi Alın](/rest/api/sql/database%20auditing%20settings/get)
- [Sunucu Denetim İlkesi Alın](/rest/api/sql/server%20auditing%20settings/get)

Ek filtreleme için WHERE yan tümcesi desteği ile genişletilmiş ilke:

- [Veritabanı *Genişletilmiş* Denetim İlkesi Oluşturma veya Güncelleme](/rest/api/sql/database%20extended%20auditing%20settings/createorupdate)
- [Sunucu *Genişletilmiş* Denetim İlkesi Oluşturma veya Güncelleme](/rest/api/sql/server%20auditing%20settings/createorupdate)
- [Veritabanı *Genişletilmiş* Denetim İlkealın](/rest/api/sql/database%20extended%20auditing%20settings/get)
- [Sunucu *Genişletilmiş* Denetim İlkesi Alın](/rest/api/sql/server%20auditing%20settings/get)

#### <a name="using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma

Azure [Kaynak Yöneticisi](../azure-resource-manager/management/overview.md) şablonlarını kullanarak Azure SQL veritabanı denetimini aşağıdaki örneklerde gösterildiği gibi yönetebilirsiniz:

- [Denetim günlüklerini Azure Blob depolama hesabına yazmak için denetleme özelliğine sahip bir Azure SQL Sunucusu dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-blob-storage)
- [Denetim günlüklerini Log Analytics'e yazmak için denetleme özelliğine sahip bir Azure SQL Sunucusu dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-oms)
- [Denetim günlüklerini Olay Hub'larına yazmak için denetleme özelliğine sahip bir Azure SQL Sunucusu dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-eventhub)

> [!NOTE]
> Bağlantılı örnekler harici bir genel depoda dır ve garanti olmaksızın 'olduğu gibi' sağlanır ve herhangi bir Microsoft destek programı/hizmeti kapsamında desteklenmez.

---
title: Yönetilen örnek denetimi
description: Azure SQL Veritabanı yönetilen örnek denetimi ile T-SQL kullanarak nasıl başlayın
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
f1_keywords:
- mi.azure.sqlaudit.general.f1
author: DavidTrigano
ms.author: datrigan
ms.reviewer: vanto
ms.date: 03/27/2020
ms.openlocfilehash: 405ac27fad3c24d3064f11476f452ad00abb9b02
ms.sourcegitcommit: d0fd35f4f0f3ec71159e9fb43fcd8e89d653f3f2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80387776"
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Azure SQL Veritabanı yönetilen örneğinde denetimle çalışmaya başlama

[Yönetilen örnek](sql-database-managed-instance.md) denetimi veritabanı olaylarını izler ve bunları Azure depolama hesabınızdaki bir denetim günlüğüne yazar. Denetim şunları da sağlar:

- Mevzuatla uyumluluk, veritabanı etkinliğini anlama ve işletme sorunlarını veya şüpheli güvenlik ihlallerini işaret edebilecek farklılıklar ve anormal durumlar hakkında içgörü sahip olmanıza yardımcı olur.
- Uyumluluğu garanti etmese bile uyumluluk standartlarına uymayı sağlar ve kolaylaştırır. Standartlara uygunluğu destekleyen Azure programları hakkında daha fazla bilgi için, SQL Veritabanı uyumluluk sertifikalarının en güncel listesini bulabileceğiniz [Azure Güven Merkezi'ne](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) bakın.

## <a name="set-up-auditing-for-your-server-to-azure-storage"></a>Sunucunuz için denetimi Azure depolama alanına ayarlama

Aşağıdaki bölümde, yönetilen örneğiniz üzerinde denetim yapılandırması açıklanmaktadır.

1. [Azure portalına](https://portal.azure.com)gidin.
2. Denetim günlüklerinin depolandığı bir Azure Depolama **kapsayıcısı** oluşturun.

   1. Denetim günlüklerinizi depolamak istediğiniz Azure Depolama alanına gidin.

      > [!IMPORTANT]
      > Bölgeler arası okuma/yazmalardan kaçınmak için yönetilen örnekle aynı bölgede bir depolama hesabı kullanın.

   1. Depolama hesabında **Genel Bakış'a** gidin ve **Blobs'u**tıklatın.

      ![Azure Blob widget'ı](./media/sql-managed-instance-auditing/1_blobs_widget.png)

   1. Üst menüde, yeni bir kapsayıcı oluşturmak için **+ Kapsayıcı'yı** tıklatın.

      ![Blob kapsayıcı simgesi oluşturma](./media/sql-managed-instance-auditing/2_create_container_button.png)

   1. Kapsayıcı **Adı**sağlayın, Genel erişim düzeyini **Özel'e**ayarlayın ve ardından **Tamam'ı**tıklatın.

      ![Blob konteyner yapılandırması oluşturma](./media/sql-managed-instance-auditing/3_create_container_config.png)
  > [!IMPORTANT]
  > Sunucu veya veritabanı düzeyindeki denetim etkinlikleri için değişmez bir günlük deposu yapılandırmak isteyen müşteriler [Azure Depolama tarafından sağlanan yönergeleri](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutability-policies-manage#enabling-allow-protected-append-blobs-writes) izlemelidir (Lütfen değişmez blob depolamasını yapılandırırken ek **eklere izin ver'i** seçtiğinizden emin olun)
  
3. Denetim günlükleri için kapsayıcıyı oluşturduktan sonra denetim günlükleri için hedef olarak yapılandırmanın iki yolu vardır: [T-SQL kullanmak](#blobtsql) veya [SQL Server Management Studio (SSMS) UI'yi kullanmak:](#blobssms)

   - <a id="blobtsql"></a>T-SQL kullanarak denetim günlükleri için blog depolama yapılandırma:

     1. Kapsayıcılar listesinde, yeni oluşturulan kapsayıcıyı tıklatın ve sonra **Kapsayıcı özelliklerini**tıklatın.

        ![Blob konteyner özellikleri düğmesi](./media/sql-managed-instance-auditing/4_container_properties_button.png)

     1. Kopya simgesine tıklayarak kapsayıcı URL'sini kopyalayın ve URL'yi (örneğin, Not Defteri'nde) ileride kullanmak üzere kaydedin. Kapsayıcı URL biçimi,`https://<StorageName>.blob.core.windows.net/<ContainerName>`

        ![Blob konteyner kopyalama URL'si](./media/sql-managed-instance-auditing/5_container_copy_name.png)

     1. Depolama hesabına yönetilen örnek denetim erişim haklarını vermek için bir Azure Depolama **SAS Belirteci** oluşturun:

        - Kapsayıcıyı önceki adımda oluşturduğunuz Azure Depolama hesabına gidin.

        - Depolama Ayarları menüsündeki **Paylaşılan erişim imzasını** tıklatın.

          ![Depolama ayarları menüsünde paylaşılan erişim imza simgesi](./media/sql-managed-instance-auditing/6_storage_settings_menu.png)

        - SAS'ı aşağıdaki gibi yapılandırın:

          - **İzin verilen hizmetler**: Blob

          - **Başlangıç tarihi**: saat dilimiyle ilgili sorunları önlemek için, dünkü tarihi kullanman önerilir

          - **Bitiş tarihi**: Bu SAS Belirteci'nin süresinin dolduğu tarihi seçin

            > [!NOTE]
            > Denetim hatalarını önlemek için son kullanma tarihinden sonra belirteci yenileyin.

          - **SAS Oluştur**’a tıklayın.
            
            ![SAS yapılandırması](./media/sql-managed-instance-auditing/7_sas_configure.png)

        - SAS Oluştur'a tıkladıktan sonra, SAS Belirteci en altta görünür. Simgeyi kopya simgesine tıklayarak kopyalayın ve ileride kullanmak üzere (örneğin, Not Defteri'nde) kaydedin.

          ![SAS belirteci kopyala](./media/sql-managed-instance-auditing/8_sas_copy.png)

          > [!IMPORTANT]
          > Belirteci başından soru işareti ("?") karakteri kaldırın.

     1. Yönetilen örneğinize SQL Server Management Studio (SSMS) veya desteklenen başka bir araç aracılığıyla bağlanın.

     1. Önceki adımlarda oluşturduğunuz Kapsayıcı URL'sini ve SAS Belirteci'ni kullanarak **yeni bir Kimlik Bilgisi oluşturmak** için aşağıdaki T-SQL deyimini çalıştırın:

        ```SQL
        CREATE CREDENTIAL [<container_url>]
        WITH IDENTITY='SHARED ACCESS SIGNATURE',
        SECRET = '<SAS KEY>'
        GO
        ```

     1. Yeni bir Sunucu Denetimi oluşturmak için aşağıdaki T-SQL deyimini çalıştırın (kendi denetim adınızı seçin, önceki adımlarda oluşturduğunuz Kapsayıcı URL'yi kullanın). Belirtilmemişse, `RETENTION_DAYS` varsayılan değer 0 'dır (sınırsız bekletme):

        ```SQL
        CREATE SERVER AUDIT [<your_audit_name>]
        TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
        GO
        ```

        1. Sunucu [Denetim Belirtimi veya Veritabanı Denetim Belirtimi oluşturarak](#createspec) devam edin

   - <a id="blobssms"></a>SQL Server Management Studio (SSMS) 18 (Önizleme) kullanarak denetim günlükleri için blob depolama yapılandırma:

     1. SQL Server Management Studio (SSMS) UI'ı kullanarak yönetilen örne bağlanın.

     1. Nesne Gezgini'nin kök notunu genişletin.

     1. **Güvenlik** düğümlerini genişletin, **Denetimler** düğümüne sağ tıklayın ve "Yeni Denetim"e tıklayın:

        ![Güvenlik ve denetim düğümlerini genişletme](./media/sql-managed-instance-auditing/10_mi_SSMS_new_audit.png)

     1. **Denetim hedefinde** "URL"nin seçildiğinden emin olun ve **Gözat'a**tıklayın:

        ![Azure Depolama'ya Göz atın](./media/sql-managed-instance-auditing/11_mi_SSMS_audit_browse.png)

     1. (İsteğe bağlı) Azure hesabınızda oturum açın:

        ![Azure'da oturum açma](./media/sql-managed-instance-auditing/12_mi_SSMS_sign_in_to_azure.png)

     1. Açılan alt lardan bir abonelik, depolama hesabı ve Blob kapsayıcısı seçin veya **Oluştur'a**tıklayarak kendi kapsayıcınızı oluşturun. Bitirdikten sonra **Tamam'a**tıklayın:

        ![Azure aboneliğini, depolama hesabını ve blob kapsayıcıyı seçin](./media/sql-managed-instance-auditing/13_mi_SSMS_select_subscription_account_container.png)

     1. "Denetim Oluştur" iletişim kutusunda **Tamam'ı** tıklatın.

4. <a id="createspec"></a>Blob kapsayıcısını denetim günlükleri için hedef olarak yapılandırdıktan sonra, SQL Server'da olduğu gibi bir Sunucu Denetim Belirtimi veya Veritabanı Denetim Belirtimi oluşturun ve etkinleştirin:

   - [Sunucu denetim belirtimi T-SQL kılavuzu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [Veritabanı denetim belirtimi T-SQL kılavuzu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

5. Adım 3'te oluşturduğunuz sunucu denetimini etkinleştirin:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

Ek bilgi için:

- [Azure SQL Veritabanı'nda tek veritabanları, elastik havuzlar ve yönetilen örnekler ile SQL Server'daki veritabanları arasındaki farkları denetleme](#auditing-differences-between-databases-in-azure-sql-database-and-databases-in-sql-server)
- [SUNUCU DENETIMI OLUŞTUR](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
- [SUNUCU DENETIMINI DEĞIŞTIR](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

## <a name="set-up-auditing-for-your-server-to-event-hub-or-azure-monitor-logs"></a>Sunucunuz için denetimi Event Hub veya Azure Monitör günlüklerine ayarlama

Yönetilen bir örnekten denetim günlükleri Bile Hub'larına veya Azure Monitör günlüklerine gönderilebilir. Bu bölümde bu nasıl yapılandırılabilen açıklanmıştır:

1. [Azure Portalı'nda](https://portal.azure.com/) yönetilen örneğe gidin.

2. **Tanılama ayarlarına**tıklayın.

3. **Tanılamayı Aç'a**tıklayın. Tanılama zaten etkinse, bunun yerine *+Ekle tanılama ayarı* gösterilecektir.

4. Günlükler listesinde **SQLSecurityAuditEvents'i** seçin.

5. Denetim etkinlikleri için bir hedef seçin - Olay Hub'ı, Azure Monitör günlükleri veya her ikisi. Her hedef için gerekli parametreleri (örneğin, Log Analytics çalışma alanı) yapılandırın.

6. **Kaydet**'e tıklayın.

    ![Tanılama ayarlarını yapılandırma](./media/sql-managed-instance-auditing/9_mi_configure_diagnostics.png)

7. SQL Server Management **Studio (SSMS) veya** desteklenen başka bir istemciyi kullanarak yönetilen örne bağlanın.

8. Sunucu denetimi oluşturmak için aşağıdaki T-SQL deyimini çalıştırın:

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>] TO EXTERNAL_MONITOR;
    GO
    ```

9. SQL Server'da olduğu gibi bir sunucu denetim belirtimi veya veritabanı denetim belirtimi oluşturun ve etkinleştirin:

   - [Sunucu denetim belirtimi T-SQL kılavuzu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [Veritabanı denetim belirtimi T-SQL kılavuzu oluşturma](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

10. Adım 8'de oluşturulan sunucu denetimini etkinleştirin:
 
    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

## <a name="consume-audit-logs"></a>Denetim günlüklerini tüketin

### <a name="consume-logs-stored-in-azure-storage"></a>Azure Depolama'da depolanan günlükleri tüketin

Blob denetim günlüklerini görüntülemek için kullanabileceğiniz çeşitli yöntemler vardır.

- Denetim günlüğü `sys.fn_get_audit_file` verilerini tabular biçimde döndürmek için sistem işlevini (T-SQL) kullanın. Bu işlevi kullanma hakkında daha fazla bilgi için [sys.fn_get_audit_file belgelerine](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql)bakın.

- [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)gibi bir araç kullanarak denetim günlüklerini keşfedebilirsiniz. Azure depolama alanında denetim günlükleri, denetim günlüklerini depolamak için tanımlanan bir kapsayıcıiçinde blob dosyaları koleksiyonu olarak kaydedilir. Depolama klasörünün hiyerarşisi, adlandırma kuralları ve günlük biçimi hakkında daha fazla bilgi için [Blob Audit Log Format Reference](https://go.microsoft.com/fwlink/?linkid=829599)bölümüne bakın.

- Denetim günlüğü tüketim yöntemlerinin tam listesi için, [SQL veritabanı denetimi ile başlatın](sql-database-auditing.md)bakın.

### <a name="consume-logs-stored-in-event-hub"></a>Olay Hub'ında depolanan günlükleri tüketin

Olay Hub'ından denetim günlükleri verilerini tüketmek için, olayları tüketmek ve bunları bir hedefe yazmak için bir akış ayarlamanız gerekir. Daha fazla bilgi için Azure Etkinlik Hub'ları Belgeleri'ne bakın.

### <a name="consume-and-analyze-logs-stored-in-azure-monitor-logs"></a>Azure Monitor günlüklerinde depolanan günlükleri tüketin ve analiz edin

Denetim günlükleri Azure Monitor günlüklerine yazılmışsa, denetim verilerinde gelişmiş aramalar yapabileceğiniz Log Analytics çalışma alanında kullanılabilir. Başlangıç noktası olarak, Log Analytics çalışma alanına gidin ve *Genel* bölümün altında *Günlükler'i* tıklatın ve denetim günlüklerini görüntülemek için aşağıdaki `search "SQLSecurityAuditEvents"` gibi basit bir sorgu girin.  

Azure Monitor günlükleri, tüm iş yükleri ve sunucularınızda milyonlarca kaydı kolayca analiz etmek için tümleşik arama ve özel panoları kullanarak size gerçek zamanlı operasyonel öngörüler sağlar. Azure Monitor günlükleri arama dili ve komutları hakkında daha fazla yararlı bilgi için Azure [Monitor günlükleri arama başvurusuna](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview)bakın.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="auditing-differences-between-databases-in-azure-sql-database-and-databases-in-sql-server"></a>Azure SQL Veritabanı'ndaki veritabanları ile SQL Server'daki veritabanları arasındaki farkları denetleme

Azure SQL Veritabanı'ndaki veritabanlarında denetim ile SQL Server'daki veritabanları arasındaki temel farklar şunlardır:

- Azure SQL Veritabanı'nda yönetilen örnek dağıtım seçeneğiyle, denetim `.xel` sunucu düzeyinde çalışır ve günlük dosyalarını Azure Blob depolama alanında depolar.
- SQL Server şirket içi / sanal makinelerde, denetim sunucu düzeyinde çalışır, ancak dosyaları sistem/windows olay günlüklerinde depolar.

Yönetilen örnekte XEvent denetimi Azure Blob depolama hedeflerini destekler. Dosya ve windows günlükleri **desteklenmez.**

Azure Blob depolamasına denetim için sözdiziminde `CREATE AUDIT` önemli farklar şunlardır:

- Yeni bir `TO URL` sözdizimi sağlanır ve `.xel` dosyaların yerleştirildiği Azure blob Depolama kapsayıcısının URL'sini belirtmenize olanak tanır.
- Even Hub `TO EXTERNAL MONITOR` ve Azure Monitor günlükleri hedeflerini etkinleştirmek için yeni bir sözdizimi sağlanır.
- SQL Veritabanı `TO FILE` Windows dosya paylaşımlarına erişemediğiiçin sözdizimi **desteklenmez.**
- Kapatma seçeneği **desteklenmez.**
- `queue_delay`0 **desteklenmez.**

## <a name="next-steps"></a>Sonraki adımlar

- Denetim günlüğü tüketim yöntemlerinin tam listesi için, [SQL veritabanı denetimi ile başlatın](sql-database-auditing.md)bakın.
- Standartlara uygunluğu destekleyen Azure programları hakkında daha fazla bilgi için, SQL Veritabanı uyumluluk sertifikalarının en güncel listesini bulabileceğiniz [Azure Güven Merkezi'ne](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) bakın.

<!--Image references-->










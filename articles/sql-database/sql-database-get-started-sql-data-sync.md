---
title: Veri Eşitlemeyi Ayarlama
description: Bu öğretici, Azure SQL Veri Eşitlemeyi'ni nasıl ayarlayabileceğinizi gösterir
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 01/14/2019
ms.openlocfilehash: 019ddbac1900856666b958d90b4395f25eb5ee84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79268970"
---
# <a name="tutorial-set-up-sql-data-sync-between-azure-sql-database-and-sql-server-on-premises"></a>Öğretici: Azure SQL Veritabanı ve SQL Server arasında SQL Veri Eşitlemeyi'ni şirket içinde ayarlama

Bu eğitimde, hem Azure SQL Veritabanı hem de SQL Server örneklerini içeren bir eşitleme grubu oluşturarak Azure SQL Veri Eşitlemeyi'ni nasıl ayarlayabileceğinizi öğrenirsiniz. Eşitleme grubu özel olarak yapılandırılır ve ayarladığınız zamanlamaya göre eşitlenir.

Öğretici, SQL Database ve SQL Server ile ilgili en azından bazı önceden deneyiminiz olduğunu varsayar.

SQL Veri Eşitlemesine genel bakış için, [Azure SQL Veri Eşitleme ile bulut ve şirket içi veritabanları arasında verileri eşitleme](sql-database-sync-data.md)bölümüne bakın.

SQL Veri Eşitleme'nin nasıl yapılandırılılabildiğini anlatan PowerShell örnekleri için, Azure SQL veritabanları veya [Azure SQL Veritabanı ile sql server şirket içi veritabanı](scripts/sql-database-sync-data-between-azure-onprem.md) arasında nasıl [eşitlenenebilin](scripts/sql-database-sync-data-between-sql-databases.md)

> [!IMPORTANT]
> Azure SQL Veri Eşitlemesi şu anda Azure SQL Veritabanı Yönetilen Örneği'ni **desteklemez.**

## <a name="create-sync-group"></a>Eşitleme grubu oluşturma

1. SQL veritabanınızı bulmak için [Azure portalına](https://portal.azure.com) gidin. **SQL veritabanlarını**arayın ve seçin.

    ![SQL veritabanları, Microsoft Azure portalı ara](media/sql-database-get-started-sql-data-sync/search-for-sql-databases.png)

1. Veri Eşitleme için hub veritabanı olarak kullanmak istediğiniz veritabanını seçin.

    ![SQL veritabanı listesinden seçin, Microsoft Azure portalı](media/sql-database-get-started-sql-data-sync/select-sql-database.png)

    > [!NOTE]
    > Hub veritabanı, eşitleme grubunun birden çok veritabanı uç noktası olduğu bir eşitleme topolojisinin merkezi bitiş noktasıdır. Eşitleme grubunda uç noktaları olan diğer tüm üye veritabanları, hub veritabanı ile eşitleme.

1. Seçili veritabanının **SQL veritabanı** menüsünde, **diğer veritabanlarına eşitle'yi**seçin.

    ![Diğer veritabanları, SQL veritabanı, Microsoft Azure portalı ile eşitleme](media/sql-database-get-started-sql-data-sync/sync-to-other-databases.png)

1. Diğer **veritabanlarıyla eşitleme** sayfasında **Yeni Eşitleme Grubu'nu**seçin. **Yeni eşitleme grubu** sayfası, **eşitleme grubu oluştur (adım 1)** vurgulanmış olarak açılır.

   ![Adım 1 ayarları](media/sql-database-get-started-sql-data-sync/stepone.png)

   Veri **Eşitleme Grubu Oluştur** sayfasında aşağıdaki ayarları değiştirin:

   | Ayar                        | Açıklama |
   | ------------------------------ | ------------------------------------------------- |
   | **Eşitleme Grubu Adı** | Yeni eşitleme grubu için bir ad girin. Bu ad, veritabanının adından farklıdır. |
   | **Eşitleme Meta veri veritabanı** | Veritabanı oluşturmayı (önerilir) veya varolan bir veritabanını kullanmayı seçin.<br/><br/>**Yeni veritabanını**seçerseniz, **yeni veritabanı oluştur'u seçin.** Sonra **SQL Veritabanı** sayfasında, adını ve yeni veritabanı yapılandırmak ve **Tamam**seçin.<br/><br/>**Varolan veritabanını kullan'ı**seçerseniz, listeden veritabanını seçin. |
   | **Otomatik Eşitleme** | **A veya** **Kapalı'yı**seçin.<br/><br/>**Açık'ı**seçerseniz, bir sayı girin ve **Eşitleme Sıklığı** bölümünde **Saniye,** **Dakika,** **Saat**veya **Günler'i** seçin. |
   | **Uyuşmazlık Çözümü** | **Hub'ı seçin veya** **Üye kazanın.**<br/><br/>**Hub kazanmak,** çakışmalar oluştuğunda, hub veritabanındaki verilerin üye veritabanında çakışan verilerin üzerine yazması anlamına gelir.<br/><br/>**Üye galibiyeti,** çakışmalar oluştuğunda, üye veritabanındaki verilerin hub veritabanında çakışan verilerin üzerine yazması anlamına gelir. |

   > [!NOTE]
   > Microsoft, **Eşitleme Meta veri veritabanı**olarak kullanılmak üzere yeni, boş bir veritabanı oluşturmanızı önerir. Veri Eşitleme bu veritabanında tablolar oluşturur ve sık sık iş yükü çalıştırın. Bu veritabanı, seçili bölgedeki tüm Eşitleme Grupları için **Eşitleme Meta Veri Veritabanı** olarak paylaşılır ve bölgedeki tüm Eşitleme Grupları ve Eşitleme Aracıları kaldırmadan veritabanını veya adını değiştiremezsiniz.

   **Tamam'ı** seçin ve eşitleme grubunun oluşturulmasını ve dağıtılmasını bekleyin.

## <a name="add-sync-members"></a>Eşitleme üyeleri ekleme

Yeni eşitleme grubu oluşturulduktan ve dağıtıldıktan sonra, **Eşitleme üye ekle (adım 2)** **Yeni eşitleme grubu** sayfasında vurgulanır.

Hub **Veritabanı** bölümünde, hub veritabanının bulunduğu SQL Veritabanı sunucusu için varolan kimlik bilgilerini girin. Bu bölüme *yeni* kimlik bilgileri girmeyin.

![Adım 2 ayarları](media/sql-database-get-started-sql-data-sync/steptwo.png)

### <a name="to-add-an-azure-sql-database"></a>Azure SQL Veritabanı eklemek için

Üye **Veritabanı** bölümünde, azure SQL Veritabanı Ekle'yi seçerek isteğe bağlı olarak eşitleme grubuna **bir Azure SQL Veritabanı**ekleyin. **Azure SQL Veritabanını Yapılandırışla** sayfası açılır.

  ![Adım 2 - veritabanını yapılandırma](media/sql-database-get-started-sql-data-sync/steptwo-configure.png)

  Azure **SQL Veritabanını Yapılandır** sayfasında aşağıdaki ayarları değiştirin:

  | Ayar                       | Açıklama |
  | ----------------------------- | ------------------------------------------------- |
  | **Üye Adını Senkronize Edin** | Yeni eşitleme üyesi için bir ad sağlayın. Bu ad, veritabanı adının kendisinden farklıdır. |
  | **Abonelik** | Faturalandırma amacıyla ilişkili Azure aboneliğini seçin. |
  | **Azure SQL Sunucusu** | Varolan SQL Veritabanı sunucusunu seçin. |
  | **Azure SQL Veritabanı** | Varolan SQL veritabanını seçin. |
  | **Yol Tariflerini Eşitle** | **Çift Yönlü Eşitleme,** **Hub'a**veya **Hub'dan**seçin. |
  | **Kullanıcı Adı** ve **Şifre** | Üye veritabanının bulunduğu SQL Veritabanı sunucusunun varolan kimlik bilgilerini girin. Bu bölüme *yeni* kimlik bilgileri girmeyin. |

  **Tamam'ı** seçin ve yeni eşitleme üyesinin oluşturulmasını ve dağıtılmasını bekleyin.

<a name="add-on-prem"></a>
### <a name="to-add-an-on-premises-sql-server-database"></a>Şirket içi SQL Server veritabanı eklemek için

Üye **Veritabanı** bölümünde, şirket içi **Veritabanı Ekle'yi**seçerek isteğe bağlı olarak eşitleme grubuna şirket içi BIR SQL Server ekleyin. **Şirket Başına Yapılandırış** sayfası aşağıdaki şeyleri yapabileceğiniz yerlerde açılır:

1. **Eşitle Aracısı Ağ Geçidi'ni seçin.** **Aracıyı Seç** sayfası açılır.

   ![Eşitleme aracısı oluşturma](media/sql-database-get-started-sql-data-sync/steptwo-agent.png)

1. **Eşitle Aracısı** seç sayfasında, varolan bir aracıyı kullanıp kullanmamayı veya aracı oluşturup oluşturmayYacağını seçin.

   **Varolan aracıları**seçerseniz, listeden varolan aracıyı seçin.

   **Yeni bir aracı oluştur'u**seçerseniz, aşağıdakileri yapın:

   1. Sağlanan bağlantıdan veri eşitleme aracısını indirin ve SQL Server'ın bulunduğu bilgisayara yükleyin. Aracıyı doğrudan [SQL Azure Veri Eşitleme Aracısı'ndan](https://www.microsoft.com/download/details.aspx?id=27693)da indirebilirsiniz.

      > [!IMPORTANT]
      > İstemci aracısının sunucuyla iletişim kurmasını sağlamak için güvenlik duvarında giden TCP bağlantı noktası 1433'u açmanız gerekir.

   1. Aracı için bir ad girin.

   1. **Oluştur ve Oluştur Anahtarı'nı** seçin ve aracı anahtarını panoya kopyalayın.

   1. **Eşitle Aracısı** seç sayfasını kapatmak için **Tamam'ı** seçin.

1. SQL Server bilgisayarında, İstemci Eşitleme Aracısı uygulamasını bulun ve çalıştırın.

   ![Veri senkronizasyonu istemci aracı uygulaması](media/sql-database-get-started-sql-data-sync/datasync-preview-clientagent.png)

    1. Eşitleme aracısı uygulamasında **Aracı Anahtarı Gönder'i**seçin. **Eşitleme Meta veri veritabanı yapılandırma** iletişim kutusu açılır.

    1. Meta **veri veritabanı yapılandırmasını eşitle** iletişim kutusuna, Azure portalından kopyalanan aracı anahtarına yapıştırın. Ayrıca, meta veri veritabanının bulunduğu Azure SQL Veritabanı sunucusuiçin varolan kimlik bilgilerini de sağlayın. (Bir meta veri veritabanı oluşturduysanız, bu veritabanı hub veritabanıyla aynı sunucudadır.) **Tamam'ı** seçin ve yapılandırmanın tamamlanmasını bekleyin.

        ![Aracı anahtarını ve sunucu kimlik bilgilerini girin](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-enterkey.png)

        > [!NOTE]
        > Güvenlik duvarı hatası alırsanız, SQL Server bilgisayarından gelen trafiğe izin vermek için Azure'da bir güvenlik duvarı kuralı oluşturun. Kuralı portalda veya SQL Server Management Studio'da (SSMS) el ile oluşturabilirsiniz. SSMS'te, adını <hub_database_name hub_database_name>.database.windows.net olarak girerek Azure'daki hub veritabanına bağlanın.

    1. Aracıya bir SQL Server veritabanı kaydetmek için **Kaydet'i** seçin. **SQL Server Configuration** iletişim kutusu açılır.

        ![SQL Server veritabanı ekleme ve yapılandırma](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-adddb.png)

    1. SQL **Server Configuration** iletişim kutusunda, SQL Server kimlik doğrulamasını veya Windows kimlik doğrulamasını kullanarak bağlanmayı seçin. SQL Server kimlik doğrulamasını seçerseniz, varolan kimlik bilgilerini girin. Senkronize etmek istediğiniz SQL Server adını ve veritabanının adını sağlayın ve ayarlarınızı test etmek için **Test bağlantısını** seçin. Ardından **Kaydet'i** seçin ve kayıtlı veritabanı listede görünür.

        ![SQL Server veritabanı artık kayıtlı](media/sql-database-get-started-sql-data-sync/datasync-preview-agent-dbadded.png)

    1. İstemci Eşitleme Aracısı uygulamasını kapatın.

1. Portalda, Şirket **Içi Yapıla** sayfasında **Veritabanını Seç'i**seçin.

1. **Veritabanını Seç** sayfasında, **Üye Adı Eşitle** alanında, yeni eşitleme üyesi için bir ad sağlayın. Bu ad, veritabanının adından farklıdır. Listeden veritabanını seçin. **Eşitleme Yönleri** alanında, **Çift Yönlü Eşitleme,** **Hub'a**veya **Hub'dan'ı**seçin.

    ![Şirket içi veritabanını seçin](media/sql-database-get-started-sql-data-sync/datasync-preview-selectdb.png)

1. **Veritabanı seç** sayfasını kapatmak için **Tamam'ı** seçin. Ardından, **Şirket İçi Yapılandırma** sayfasını kapatmak ve yeni eşitleme üyesinin oluşturulmasını ve dağıtılmasını beklemek için **Tamam'ı** seçin. Son olarak, **eşitle üyeleri** sayfasını kapatmak için **Tamam'ı** seçin.

> [!NOTE]
> SQL Veri Eşitleme'ye ve yerel aracıya bağlanmak için, DataSync_Executor *rolüne*kullanıcı adınızı ekleyin. Veri Eşitleme, SQL Server örneğinde bu rolü oluşturur.

## <a name="configure-sync-group"></a>Eşitleme grubunu yapılandırma

Yeni eşitleme grubu üyeleri oluşturulduktan ve dağıtıldıktan sonra, **Yapılandırma eşitleme grubu (adım 3)** **Yeni eşitleme grubu** sayfasında vurgulanır.

![Adım 3 ayarları](media/sql-database-get-started-sql-data-sync/stepthree.png)

1. **Tablolar** sayfasında, eşitleme grubu üyeleri listesinden bir veritabanı seçin ve **şema'yı yenile'yi**seçin.

1. Listeden eşitlemek istediğiniz tabloları seçin. Varsayılan olarak, tüm sütunlar seçilir, bu nedenle eşitlemek istemediğiniz sütunlar için onay kutusunu devre dışı kaldırın. Birincil anahtar sütunu seçili bıraktığından emin olun.

1. **Kaydet'i**seçin.

1. Varsayılan olarak, veritabanları zamanlanana veya el ile çalıştırılana kadar eşitlenmez. El ile eşitleme çalıştırmak için Azure portalındaki SQL veritabanınıza gidin, **diğer veritabanlarıyla eşitle'yi**seçin ve eşitleme grubunu seçin. **Veri Eşitleme** sayfası açılır. **Eşitle**’yi seçin.

    ![Manuel eşitleme](media/sql-database-get-started-sql-data-sync/datasync-sync.png)

## <a name="faq"></a>SSS

**Veri Eşitleme verilerimi ne sıklıkta eşitleyebilir?**

Eşitlemeler arasındaki minimum süre beş dakikadır.

**SQL Data Sync tabloları tam olarak oluşturuyor mu?**

Hedef veritabanında eşitleme şeması tabloları eksikse SQL Data Sync seçtiğiniz sütunlarla o tabloları oluşturur. Ancak, bu aşağıdaki nedenlerle tam sadakat şema neden olmaz:

- Hedef tabloda yalnızca seçtiğiniz sütunlar oluşturulmuştur. Seçilmeyen sütunlar yoksayılmıştır.
- Hedef tabloda yalnızca seçili sütun dizinleri oluşturulmuştur. Seçili olmayan sütunların dizinleri yoksayılmıştır.
- XML türü sütunlarında dizinler oluşturulmaz.
- CHECK kısıtlamaları oluşturulmaz.
- Kaynak tablolardaki tetikleyiciler oluşturulmaz.
- Görünümler ve depolanan yordamlar oluşturulmaz.

Bu sınırlamalar nedeniyle aşağıdakileri öneririz:

- Üretim ortamları için, tam sadakat şema kendiniz oluşturun.
- Hizmetle denemeler yaparken otomatik sağlama özelliğini kullanın.

**Neden oluşturmadığım tabloları görüyorum?**

Veri Eşitleme, değişiklik izleme için veritabanında ek tablolar oluşturur. Bunları silmeyin veya Veri Eşitleme çalışmayı durdurur.

**Verilerim eşitlendikten sonra yakınsak mı?**

Mutlaka. Eşitlemelerin Hub'dan A'ya, Hub'dan B'ye ve Hub'dan C'ye olduğu hub ve üç sözcülü (A, B ve C) içeren bir eşitleme grubu alın. *Hub'dan* A eşitlemesine sonra Veritabanı A'da değişiklik yapılırsa, bu değişiklik bir sonraki eşitleme görevine kadar B veya veritabanı C veritabanına yazılmaz.

**Şema değişikliklerini eşitleme grubuna nasıl alabilirim?**

Tüm şema değişikliklerini el ile yapın ve çoğaltın.

1. Şema değişikliklerini hub'da ve tüm eşitleme üyelerine el ile çoğaltın.
1. Eşitleme şeasını güncelleştirin.

Yeni tablo ve sütun eklemek için:

Yeni tablolar ve sütunlar geçerli eşitlemeyi etkilemez ve Veri Eşitleme, eşitleme şemasına eklenene kadar bunları yok sayar. Yeni veritabanı nesneleri eklerken sırayı izleyin:

1. Hub'a ve tüm eşitleme üyelerine yeni tablolar veya sütunlar ekleyin.
1. Eşitleme şemasına yeni tablolar veya sütunlar ekleyin.
1. Yeni tablo ve sütunlara değerler eklemeye başlayın.

Bir sütunun veri türünü değiştirmek için:

Varolan bir sütunun veri türünü değiştirdiğinizde, Veri Eşitleme, eşitleme şemasında tanımlanan özgün veri türüne yeni değerler sığdırınca çalışmaya devam eder. Örneğin, kaynak veritabanındaki türü **int'den** **bigint'e**değiştirirseniz, **int** veri türü için çok büyük bir değer ekilene kadar Veri Eşitleme çalışmaya devam edin. Değişikliği tamamlamak için şema değişikliğini hub'a ve tüm eşitleme üyelerine el ile çoğaltın ve ardından eşitleme şemasını güncelleştirin.

**Veri Eşitleme ile veritabanını nasıl dışa aktarıp içe aktarabilirim?**

Bir veritabanını *.bacpac* dosyası olarak dışa aktardıktan ve veritabanı oluşturmak için dosyayı içe aktardıktan sonra, yeni veritabanında Veri Eşitlemeyi'ni kullanmak için aşağıdakileri yapın:

1. [Bu komut dosyasını](https://github.com/vitomaz-msft/DataSyncMetadataCleanup/blob/master/Data%20Sync%20complete%20cleanup.sql)kullanarak yeni veritabanındaki Veri Eşitleme nesnelerini ve ek tabloları temizleyin. Komut dosyası, veritabanından gerekli tüm Veri Eşitleme nesnelerini siler.
1. Eşitleme grubunu yeni veritabanıyla yeniden oluşturun. Eski eşitleme grubuna artık ihtiyacınız yoksa, silin.

**Müşteri temsilcisi hakkında nerede bilgi edinebilirim?**

İstemci aracısı hakkında sık sorulan sorular için [Agent SSS](sql-database-data-sync-agent.md#agent-faq)bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler. Hem SQL Veritabanı örneğini hem de SQL Server veritabanını içeren bir eşitleme grubu oluşturdunuz.

SQL Data Sync hakkında daha fazla bilgi için bkz.:

- [Azure SQL Veri Eşitlemi için Veri Eşitleme Aracısı](sql-database-data-sync-agent.md)
- [En iyi uygulamalar](sql-database-best-practices-data-sync.md) ve [Azure SQL Veri Eşitlemenile ilgili sorunları giderme](sql-database-troubleshoot-data-sync.md)
- [Azure Monitor günlükleriyle SQL Veri Eşitlemesini Izleyin](sql-database-sync-monitor-oms.md)
- [Eşitleme şeasını Transact-SQL](sql-database-update-sync-schema.md) veya [PowerShell](scripts/sql-database-sync-update-schema.md) ile güncelleştirme

SQL Veritabanı hakkında daha fazla bilgi için bkz.:

- [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
- [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)

---
title: Öğretici - SQL Server veritabanlarını Azure'a yedekleme
description: Bu eğitimde, Azure VM'de çalışan bir SQL Server veritabanını Azure Yedekleme Kurtarma Hizmetleri kasasına nasıl yedekleyeceğimiz öğrenin.
ms.topic: tutorial
ms.date: 06/18/2019
ms.openlocfilehash: f1d76fe0dfa428688714b8383c3974ac63195681
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680740"
---
# <a name="back-up-a-sql-server-database-in-an-azure-vm"></a>Azure VM'de SQL Server veritabanını yedekleme

Bu öğretici, Azure VM'de çalışan bir SQL Server veritabanını Azure Yedekleme Kurtarma Hizmetleri kasasına nasıl yedeklediğinizi gösterir. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Bir kasa oluşturun ve yapılandırın.
> * Veritabanlarını keşfedin ve yedeklemeler ayarlayın.
> * Veritabanları için otomatik koruma yı ayarlayın.
> * İsteğe bağlı yedekleme çalıştırın.

## <a name="prerequisites"></a>Ön koşullar

SQL Server veritabanınızı yedeklemeden önce aşağıdaki koşulları kontrol edin:

1. SQL Server örneğini barındıran VM ile aynı bölgede veya yerel olarak bir Kurtarma Hizmetleri kasası tanımlayın veya [oluşturun.](backup-sql-server-database-azure-vms.md#create-a-recovery-services-vault)
2. SQL veritabanlarını yedeklemek için gereken [VM izinlerini denetleyin.](backup-azure-sql-database.md#set-vm-permissions)
3. VM'nin [ağ bağlantısı](backup-sql-server-database-azure-vms.md#establish-network-connectivity)na sahip olduğunu doğrulayın.
4. SQL Server veritabanlarının Azure Yedekleme için [adlandırma yönergelerine](#verify-database-naming-guidelines-for-azure-backup) uygun olarak adlandırılıncaya dabelirtin.
5. Veritabanı için etkin leştirilmiş başka yedekleme çözüme sahip olmadığını doğrulayın. Bu senaryoyu ayarlamadan önce diğer tüm SQL Server yedeklemelerini devre dışı bırak. Herhangi bir çakışma olmaksızın VM üzerinde çalışan bir SQL Server veritabanı için Azure Yedekleme ile birlikte Bir Azure VM için Azure Yedekleme'yi etkinleştirebilirsiniz.

### <a name="establish-network-connectivity"></a>Ağ bağlantısı oluşturma

Tüm işlemler için SQL Server VM sanal makinesinin Azure genel IP adreslerine bağlanması gerekir. VM işlemleri (veritabanı bulma, yedekleme yapılandırma, yedekleme zamanlama, kurtarma noktaları geri yükleme vb) ortak IP adreslerine bağlantı olmadan başarısız olur. Bu seçeneklerden biriyle bağlantı kurun:

* **Azure veri merkezi IP aralıklarına izin verin**: İndirmede [IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653) izin verin. Ağ güvenlik grubuna (NSG) erişmek için **Set-AzureNetworkSecurityRule** cmdlet'ini kullanın.
* **Trafiği yönlendirmek için bir HTTP proxy sunucusu dağıtma**: Bir SQL Server veritabanını Azure VM'de yedeklediğinizde, VM'deki yedekleme uzantısı, yönetim komutlarını Azure Yedekleme'ye ve verileri Azure Depolama'ya göndermek için HTTPS API'lerini kullanır. Yedekleme uzantısı, kimlik doğrulaması için Azure Active Directory (Azure AD) de kullanır. Bu üç hizmetin yedekleme uzantısı trafiğini HTTP proxy üzerinden yönlendirin. Uzantılar, genel internet erişimi için yapılandırılan tek bileşendir.

Her seçeneğin avantajları ve dezavantajları vardır

**Seçeneği** | **Avantaj -ları** | **Dezavantajlar**
--- | --- | ---
IP aralıklarına izin ver | Ek ücret yok. | IP adresi aralıkları zaman içinde değiştiğinden, yönetmek için karmaşıktır. <br/><br/> Yalnızca Azure Depolama'ya değil, Azure'un tamamına erişim sağlar.
HTTP proxy kullanma   | Depolama URL'leri üzerinde proxy'de taneli denetime izin verilir. <br/><br/> VM'lere tek nokta lı internet erişimi. <br/><br/> Azure IP adresi değişikliklerine tabi değildir. | Proxy yazılımı ile bir VM çalıştırmak için ek maliyetler.

### <a name="set-vm-permissions"></a>VM izinlerini ayarlama

Azure Yedekleme, bir SQL Server veritabanı için yedekleme yapılandırdığınızda birçok şey yapar:

* **AzureBackupWindowsİş** uzantısını ekler.
* Azure Yedekleme, sanal makinedeki veritabanlarını keşfetmek için **NT SERVICE\AzureWLBackupPluginSvc**hesabını oluşturur. Bu hesap yedekleme ve geri yükleme için kullanılır ve SQL sysadmin izinleri gerektirir.
* Azure Yedekleme, veritabanı bulma/sorgulama için **NT AUTHORITY\SYSTEM** hesabından yararlanır, bu nedenle bu hesabın SQL'de genel oturum açması gerekir.

SQL Server VM'yi Azure Marketi'nden oluşturmadıysanız, **usererrorSQLNoSysadminMembership**hatası alabilirsiniz. Bu durumda [bu yönergeleri izleyin.](backup-azure-sql-database.md#set-vm-permissions)

### <a name="verify-database-naming-guidelines-for-azure-backup"></a>Azure Yedekleme için veritabanı adlandırma yönergelerini doğrulama

Veritabanı adları için aşağıdakilerden kaçının:

* İzleyen/Giden alanlar
* '!'
* Kare köşeli ']' köşeli köşeyi kapat
* 'F:\' ile başlayan veritabanları adları

Azure tablosunda desteklenmeyen karakterler için takma adlarımız var, ancak bunlardan kaçınmanızı öneririz. [Daha fazla bilgi edinin](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server veritabanlarını keşfedin

VM üzerinde çalışan veritabanlarını keşfedin.

1. Azure [portalında,](https://portal.azure.com)veritabanını yedeklemek için kullandığınız Kurtarma Hizmetleri kasasını açın.

2. Kurtarma **Hizmetleri tonoz** panosunda **Yedekleme'yi**seçin.

   ![Yedekleme Hedefi menüsünü açmak için Yedekleme'yi seçin](./media/backup-azure-sql-database/open-backup-menu.png)

3. **Yedekleme**Hedefi'nde, **iş yükünün** **Azure'da** (varsayılan) nerede çalıştığıkümesi.

4. **Yedeklemek istediğiniz şeyde**Azure **VM'de SQL Server'ı**seçin.

    ![Yedekleme için Azure VM'de SQL Server'ı seçin](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

5. **Yedek Hedef** > **VM'lerde DB'leri Keşfedin'de,** abonelikte korumasız VM'leri aramak için Başlangıç **Bulma'yı** seçin. Abonelikteki korumasız sanal makinelerin sayısına bağlı olarak biraz zaman alabilir.

   * Korumasız VM'ler, ada ve kaynak grubuna göre listelenen keşiften sonra listede görünmelidir.
   * Bir VM beklediğiniz gibi listelenmiyorsa, kasada yedeklenip yedeklenmediğini kontrol edin.
   * Birden çok VM aynı ada sahip olabilir, ancak farklı kaynak gruplarına ait olurlar.

     ![Yedekleme, VM'lerde DB'ler için arama sırasında beklemede](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. VM listesinde, SQL Server veritabanını çalıştıran VM'yi seçin > **DB'leri Keşfedin.**

7. **Bildirimler** alanındaki veritabanı bulmasını izleyin. VM'de kaç veritabanları olduğuna bağlı olarak işin tamamlanması biraz zaman alabilir. Seçili veritabanları keşfedildiğinde, bir başarı iletisi görüntülenir.

    ![Dağıtım başarı iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

8. Azure Yedekleme, VM'deki tüm SQL Server veritabanlarını keşfeder. Keşif sırasında arka planda aşağıdakiler oluşur:

    * Azure Yedekleme, iş yükü yedeklemesi için VM'yi kasaya kaydettirin. Kayıtlı VM'deki tüm veritabanları sadece bu kasaya yedeklenebilir.
    * Azure Yedekleme, VM'de **AzureBackupWindowsİş iş yükü** uzantısını yükler. SQL veritabanında aracı yüklü değil.
    * Azure Yedekleme, VM'de **NT Service\AzureWLBackupPluginSvc** hizmet hesabını oluşturur.
      * Tüm yedekleme ve geri yükleme işlemleri hizmet hesabını kullanır.
      * **NT Service\AzureWLBackupPluginSvc'nin** SQL sysadmin izinlerine ihtiyacı vardır. Azure Marketi'nde oluşturulan tüm SQL Server VM'leri **SqlIaSExtension** yüklü olarak gelir. **AzureBackupWindowsWorkload** uzantısı, gerekli izinleri otomatik olarak almak için **SQLIaaSExtension'ı** kullanır.
    * Eğer pazardan VM oluşturmadıysanız, o zaman VM **SqlIaaSExtension** yüklü değildir ve bulma işlemi hata iletisi **UserErrorSQLNoSysAdminMembership**ile başarısız olur. Bu sorunu gidermek için [yönergeleri](backup-azure-sql-database.md#set-vm-permissions) izleyin.

        ![VM ve veritabanını seçin](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma  

Yedeklemeyi aşağıdaki gibi yapılandırın:

1. **Yedekleme**Hedefi'nde, **Yedeklemeyi Yapılandır'ı**seçin.

   ![Yedeklemeyi Yapılandır'ı seçin](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

2. **Yedeklemeyi Yapılandır'ı**tıklatın, **yedekleme bıçağına öğeleri seç** görüntülenir. Bu, tüm kayıtlı kullanılabilirlik gruplarını ve bağımsız SQL Sunucularını listeler. Bu durumda ki tüm korumasız veritabanlarını veya Her zaman AG'deki tüm veritabanlarını görmek için köşeli sıranın solundaki köşeli genimi genişletin.  

    ![Tüm SQL Server örneklerini bağımsız veritabanları ile görüntüleme](./media/backup-azure-sql-database/list-of-sql-databases.png)

3. > ok korumak istediğiniz tüm veritabanlarını **seçin.**

   ![Veritabanını koruma](./media/backup-azure-sql-database/select-database-to-protect.png)

   Yedekleme yüklerini optimize etmek için Azure Yedekleme, bir yedekleme işinde en fazla veri tabanı sayısını 50'ye ayarlar.

     * Alternatif olarak, **AUTOPROTECT** sütunundaki ilgili açılır dosyadaki **ON** seçeneğini seçerek tüm örnekte veya Her Zaman Kullanılabilirlik grubunda otomatik korumayı etkinleştirebilirsiniz. Otomatik koruma özelliği yalnızca tek bir noktada varolan tüm veritabanlarında koruma sağlamakla kalmıyor, aynı zamanda o örne veya gelecekte kullanılabilirlik grubuna eklenecek yeni veritabanlarını da otomatik olarak korur.  

4. Yedekleme ilkesi bıçağını açmak için **Tamam'ı** tıklatın. **OK**

    ![Her Zaman Kullanılabilirlik grubunda otomatik korumayı etkinleştirin](./media/backup-azure-sql-database/enable-auto-protection.png)

5. **Yedekleme ilkesi seç,** bir ilke seçin ve sonra **Tamam'ı**tıklatın.

   * Varsayılan ilkeyi seçin: HourlyLogBackup.
   * SQL için daha önce oluşturulmuş varolan bir yedekleme ilkesini seçin.
   * RPO ve bekletme aralığınızı temel alan yeni bir ilke tanımlayın.

     ![Yedekleme ilkesini seçin](./media/backup-azure-sql-database/select-backup-policy.png)

6. **Yedekleme** menüsünde **Yedeklemeyi Etkinleştir'i**seçin.

    ![Seçilen yedekleme ilkesini etkinleştirme](./media/backup-azure-sql-database/enable-backup-button.png)

7. Portalın **Bildirimler** alanındaki yapılandırma ilerlemesini izleyin.

    ![Bildirim alanı](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluşturma

Yedekleme ilkesi, yedeklemelerin ne zaman alındığını ve ne kadar süreyle saklanacaklarını tanımlar.

* Kasa düzeyinde bir ilke oluşturulur.
* Birden çok kasa aynı yedekleme ilkesini kullanabilir, ancak yedekleme ilkesini her kasaya uygulamanız gerekir.
* Bir yedekleme ilkesi oluşturduğunuzda, günlük tam yedekleme varsayılandır.
* Bir diferansiyel yedekleme ekleyebilirsiniz, ancak yalnızca tam yedeklemeleri haftalık olarak gerçekleşecek şekilde yapılandırdığınızda.
* Farklı yedekleme ilkeleri türleri [hakkında bilgi edinin.](backup-architecture.md#sql-server-backup-types)

Yedekleme ilkesi oluşturmak için:

1. Kasada, **Yedekleme ilkeleri** > **Ekle'yi**tıklatın.
2. **Ekle** menüsünde, ilke türünü tanımlamak için **Azure VM'de SQL Server'ı** tıklatın.

   ![Yeni yedekleme ilkesi için bir ilke türü seçin](./media/backup-azure-sql-database/policy-type-details.png)

3. **İlke adına,** yeni ilke için bir ad girin.
4. **Tam Yedekleme ilkesinde,** Bir **Yedekleme Frekansı**seçin, **Günlük** veya **Haftalık'ı**seçin.

   * **Günlük**için, yedekleme işinin başladığı saat ve saat dilimini seçin.
   * **Tam Yedekleme** seçeneğini kapatamadığınız için tam bir yedekleme çalıştırmalısınız.
   * İlkeyi görüntülemek için **Tam Yedekleme'yi** tıklatın.
   * Günlük tam yedeklemeler için diferansiyel yedeklemeler oluşturamazsınız.
   * **Haftalık**için, yedekleme işinin başladığı haftanın gününü, saatini ve saat dilimini seçin.

     ![Yeni yedekleme ilkesi alanları](./media/backup-azure-sql-database/full-backup-policy.png)  

5. **Bekletme Aralığı**için varsayılan olarak tüm seçenekler seçilir. İstemediğiniz istenmeyen bekletme aralığı sınırlarını temizleyin ve aralıkları kullanacak şekilde ayarlayın.

    * Herhangi bir yedekleme türü (tam/diferansiyel/günlük) için minimum bekletme süresi yedi gündür.
    * Kurtarma noktaları, bekletme aralıklarına göre bekletme için etiketlenir. Örneğin, günlük tam yedekleme seçerseniz, her gün yalnızca bir tam yedekleme tetiklenir.
    * Belirli bir günün yedeklemesi, haftalık bekletme aralığına ve haftalık bekletme ayarınıza göre etiketlenir ve saklanır.
    * Aylık ve yıllık saklama aralıkları benzer şekilde olur.

   ![Bekletme aralığı aralığı ayarları](./media/backup-azure-sql-database/retention-range-interval.png)

6. Tam **Yedekleme ilkesi** menüsünde, ayarları kabul etmek için **Tamam'ı** seçin.
7. Diferansiyel yedekleme ilkesi eklemek için **Diferansiyel Yedekleme'yi**seçin.

   ![Bekletme aralığı](./media/backup-azure-sql-database/retention-range-interval.png)
   ![aralığı ayarları Diferansiyel yedekleme ilkesi menüsünü açın](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

8. **Diferansiyel Yedekleme ilkesinde,** frekans ve bekletme denetimlerini açmak için **Etkinleştir'i** seçin.

    * En fazla, günde bir diferansiyel yedekleme tetikleyebilirsiniz.
    * Diferansiyel yedeklemeler en fazla 180 gün süreyle saklanabilir. Daha uzun bekletme gerekiyorsa, tam yedekleme kullanmanız gerekir.

9. İlkeyi kaydetmek ve ana **Yedekleme ilkesi** menüsüne dönmek için **Tamam'ı** seçin.

10. İşlem günlüğü yedekleme ilkesi eklemek için **Log Backup'ı**seçin.
11. **Günlük**Yedekleme'de, **Etkinleştir'i**seçin ve ardından sıklık ve bekletme denetimlerini ayarlayın. Günlük yedeklemeleri her 15 dakikada bir sık olarak oluşabilir ve 35 güne kadar saklanabilir.
12. İlkeyi kaydetmek ve ana **Yedekleme ilkesi** menüsüne dönmek için **Tamam'ı** seçin.

    ![Günlük yedekleme ilkesini edin](./media/backup-azure-sql-database/log-backup-policy-editor.png)

13. Yedekleme **ilkesi** menüsünde, SQL **Yedekleme Sıkıştırma'yı**etkinleştirip etkinleştirmeyeceğinizi seçin.
    * Sıkıştırma varsayılan olarak devre dışı bırakılır.
    * Arka uçta, Azure Yedekleme SQL yerel yedekleme sıkıştırma kullanır.

14. Yedekleme ilkesindeki ediyi tamamladıktan sonra **Tamam'ı**seçin.

## <a name="run-an-on-demand-backup"></a>İsteğe bağlı yedekleme çalıştırma

1. Kurtarma Hizmetleri kasanızda Yedek öğeleri seçin.
2. "Azure VM'de SQL"e tıklayın.
3. Veritabanına sağ tıklayın ve "Şimdi Yedekleme"yi seçin.
4. Yedekleme Türünü Seçin (Tam/Diferansiyel/Günlük/Yalnızca Tam Kopyala) ve Sıkıştırma (Etkinleştirme/Devre Dışı)
5. Yedeklemeyi başlatmak için Tamam'ı seçin.
6. Kurtarma Hizmetleri kasanıza giderek ve "Yedekleme İşleri"ni seçerek yedekleme işini izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalı kullanarak şu işlemleri gerçekleştirdiniz:

> [!div class="checklist"]
>
> * Bir kasa oluşturun ve yapılandırın.
> * Veritabanlarını keşfedin ve yedeklemeler ayarlayın.
> * Veritabanları için otomatik koruma yı ayarlayın.
> * İsteğe bağlı yedekleme çalıştırın.

Bir Azure sanal makinesini diskten geri yüklemek için sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure VM'lerde SQL Server veritabanlarını geri yükleme](./restore-sql-database-azure-vm.md)

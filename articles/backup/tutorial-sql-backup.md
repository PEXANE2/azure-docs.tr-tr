---
title: Öğretici - SQL Server veritabanlarını Azure'a yedekleme
description: Bu öğreticide Azure sanal makinesi üzerinde çalışan bir SQL Server veritabanını Azure Backup Kurtarma Hizmetleri kasasına yedeklemeyi öğreneceksiniz.
ms.topic: tutorial
ms.date: 06/18/2019
ms.openlocfilehash: 17a8472da2595c08cb198baaf853faf110a619fa
ms.sourcegitcommit: cd0a1ae644b95dbd3aac4be295eb4ef811be9aaa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88612461"
---
# <a name="back-up-a-sql-server-database-in-an-azure-vm"></a>Azure sanal makinesinde çalışan bir SQL Server veritabanını yedekleme

Bu öğreticide Azure sanal makinesi üzerinde çalışan bir SQL Server veritabanını Azure Backup Kurtarma Hizmetleri kasasına yedekleme adımları gösterilmiştir. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Kasa oluşturma ve yapılandırma.
> * Veritabanlarını bulma ve yedekleme ayarlarını yapma.
> * Veritabanları için otomatik korumayı ayarlama.
> * İsteğe bağlı yedekleme çalıştırma.

## <a name="prerequisites"></a>Önkoşullar

SQL Server veritabanınızı yedeklemeden önce aşağıdaki koşullara göz atın:

1. SQL Server örneğini barındıran sanal makineyle aynı bölgeyi veya yerel ayarı paylaşan bir Kurtarma Hizmetleri kasası bulun veya [oluşturun](backup-sql-server-database-azure-vms.md#create-a-recovery-services-vault).
2. SQL veritabanlarını yedeklemek için gerekli [Sanal makine izinlerini denetleyin](backup-azure-sql-database.md#set-vm-permissions).
3. Sanal makinenin [ağ bağlantısına](backup-sql-server-database-azure-vms.md#establish-network-connectivity) sahip olduğunu doğrulayın.
4. SQL Server veritabanları, Azure Backup [adlandırma kurallarına](#verify-database-naming-guidelines-for-azure-backup) göre adlandırılır.
5. Veritabanı için etkinleştirilmiş farklı yedekleme çözümleri olmadığından emin olun. Bu senaryoyu ayarlamadan önce diğer tüm SQL Server yedekleme çözümlerini devre dışı bırakın. Azure sanal makineleri için Azure Backup ile sanal makine üzerinde çalışan SQL Server veritabanı için Azure Backup hizmetini herhangi bir çakışma olmadan birlikte etkinleştirebilirsiniz.

### <a name="establish-network-connectivity"></a>Ağ bağlantısını kurma

SQL Server VM sanal makinesi tüm işlemler için Azure genel IP adresi üzerinden iletişim kurar. Genel IP adresi bağlantısı olmadığında sanal makine işlemleri (veritabanı bulma, yedekleme yapılandırma, yedekleme zamanlama, kurtarma noktalarını geri yükleme vb.) başarısız olur. Şu seçeneklerden birini kullanarak bağlantı kurun:

* **Azure veri merkezi IP aralıklarına izin verin**: İndirdiğiniz [IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653) izin verin. Ağ güvenlik grubuna (NSG) erişmek için **Set-AzureNetworkSecurityRule** cmdlet'ini kullanın.
* **Trafiği yönlendirmek için bir HTTP ara sunucusu dağıtın**: Bir Azure sanal makinesi üzerinde çalışan SQL Server veritabanını yedeklediğinizde sanal makine üzerindeki yedekleme uzantısı, Azure Backup hizmetine yönetim komutlarını, Azure Depolama hizmetine de verileri göndermek için HTTPS API'lerini kullanır. Yedekleme uzantısı ayrıca kimlik doğrulaması için Azure Active Directory (Azure AD) kullanır. Bu üç hizmet için yedekleme uzantısı trafiğini HTTP ara sunucusu üzerinden yönlendirin. Yalnızca uzantılar genel internet erişimi için yapılandırılır.

Her seçeneğin belirli avantajları ve dezavantajları vardır

**Seçenek** | **Avantajlar** | **Dezavantajlar**
--- | --- | ---
IP aralıklarına izin verme | Ek maliyet yoktur. | IP aralıkları belirli dönemlerde değiştirildiğinden yönetim süreci karmaşıktır. <br/><br/> Yalnızca Azure Depolama'ya değil Azure altyapısının tamamına erişim sağlar.
HTTP ara sunucusu kullanma   | Depolama URL'leri için ara sunucuda ayrıntılı denetim sağlanır. <br/><br/> Sanal makinelere tek noktadan internet erişimi. <br/><br/> Azure IP adresi değişikliklerinden etkilenmez. | Ara sunucu yazılımıyla sanal makine çalıştırmak için ek maliyetler.

### <a name="set-vm-permissions"></a>Sanal makine izinlerini ayarlama

SQL Server veritabanı için yedekleme yapılandırdığınızda Azure Backup birkaç farklı işlem gerçekleştirir:

* **AzureBackupWindowsWorkload** uzantısını ekler.
* Azure Backup, sanal makinedeki veritabanlarını bulmak için **NT SERVICE\AzureWLBackupPluginSvc** hesabını oluşturur. Bu hesap, yedekleme ve geri yükleme için kullanılır ve SQL sysadmin izinlerine ihtiyaç duyar.
* Azure Backup, veritabanı bulma/sorgulama işlemleri için **NT AUTHORITY\SYSTEM** hesabını kullanır. Bu nedenle bu hesabın SQL'de herkese açık kullanıcı olması gerekir.

SQL Server sanal makinesini Azure Market'ten oluşturmadıysanız **UserErrorSQLNoSysadminMembership** hatasıyla karşılaşabilirsiniz. Bu durumda [buradaki yönergeleri izleyin](backup-azure-sql-database.md#set-vm-permissions).

### <a name="verify-database-naming-guidelines-for-azure-backup"></a>Azure Backup için veritabanı adlandırma yönergelerini doğrulama

Veritabanı adında şunları kullanmaktan kaçının:

* Sondaki/öndeki boşluklar
* Sonda "!"
* Köşeli ayraç "]"
* "F:\" ile başlayan veritabanı adları

Azure tablosunda desteklenmeyen karakterler için diğer ad desteği sunulur ancak bu karakterleri kullanmaktan kaçınmanız önerilir. [Daha fazla bilgi edinin](/rest/api/storageservices/understanding-the-table-service-data-model).

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server veritabanlarını bulma

Sanal makine üzerinde çalışan veritabanlarını bulun.

1. [Azure portalında](https://portal.azure.com) veritabanını yedeklemek istediğiniz Kurtarma Hizmetleri kasasını açın.

2. **Kurtarma Hizmetleri kasası** panosunda **Yedekle**'yi seçin.

   ![Yedekle'yi seçerek Yedekleme Hedefi menüsünü açın](./media/backup-azure-sql-database/open-backup-menu.png)

3. **Yedekleme Hedefi** altında **İş yükünüz nerede çalışıyor?** için **Azure**'ı (varsayılan) seçin.

4. **Neleri yedeklemek istiyorsunuz?** bölümünde **Azure VM'de SQL Server**'ı seçin.

    ![Yedekleme için Azure VM'de SQL Server'ı seçme](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

5. **Yedekleme Hedefi** > **VM'lerin içinde VT bul** altında **Bulmayı Başlat**'a tıklayarak abonelikteki korunmayan sanal makineleri arayın. İşlemin süresi abonelikte bulunan korumasız sanal makine sayısına bağlı olarak değişebilir.

   * Bulma işlemi tamamlandığında korunmayan sanal makineler adları ve kaynak gruplarıyla birlikte listelenir.
   * İstediğiniz sanal makine listede yoksa başka bir kasaya yedeklenip yedeklenmediğini kontrol edin.
   * Aynı ada sahip ancak farklı kaynak gruplarına ait olan birden fazla sanal makine olabilir.

     ![VM'lerin içinde VT bulma sırasında Backup bekliyor](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. Sanal makine listesinde SQL Server veritabanını çalıştıran sanal makineyi bulup **Veritabanlarını bul**'u seçin.

7. Veritabanı bulma işlemini **Bildirimler** alanından takip edebilirsiniz. İşlemin tamamlanma süresi sanal makine üzerindeki veritabanı sayısına bağlı olarak değişebilir. Seçilen veritabanları bulunduğunda işlemin başarılı olduğunu belirten bir ileti görüntülenir.

    ![Dağıtım başarılı iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

8. Azure Backup, sanal makine üzerindeki tüm SQL Server veritabanlarını bulur. Bulma sırasında arka planda şu işlemler gerçekleştirilir:

    * Azure Backup, sanal makineyi iş yükünün yedekleneceği kasaya kaydeder. Kayıtlı sanal makine üzerindeki tüm veritabanları yalnızca bu kasaya yedeklenebilir.
    * Azure Backup, sanal makineye **AzureBackupWindowsWorkload** uzantısını yükler. SQL veritabanına herhangi bir aracı yüklenmez.
    * Azure Backup, sanal makinede **NT Service\AzureWLBackupPluginSvc** hesabını oluşturur.
      * Tüm yedekleme ve geri yükleme işlemleri için hizmet hesabı kullanılır.
      * **NT Service\AzureWLBackupPluginSvc**, SQL sysadmin izinlerine ihtiyaç duyar. Azure Market'te oluşturulan tüm SQL Server sanal makinelerinde **SqlIaaSExtension** yüklü gelir. **AzureBackupWindowsWorkload** uzantısı, gerekli izinleri otomatik olarak almak için **SQLIaaSExtension** uzantısını kullanır.
    * Sanal makineyi marketten oluşturmadıysanız **SqlIaaSExtension** uzantısı yüklü değildir ve bulma işlemi **UserErrorSQLNoSysAdminMembership** hata iletisini vererek başarısız olur. Bu sorunu gidermek için [buradaki yönergeleri](backup-azure-sql-database.md#set-vm-permissions) izleyin.

        ![Sanal makineyi ve veritabanını seçme](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma  

Yedeklemeyi şu şekilde yapılandırın:

1. **Yedekleme Hedefi** alanında **Yedeklemeyi Yapılandır**'ı seçin.

   ![Yedeklemeyi Yapılandır'ı seçin](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

2. **Yedeklemeyi Yapılandır**'ı seçtiğinizde **Yedeklenecek öğeleri seçin** bölmesi açılır. Kayıtlı kullanılabilirlik grupları ve tek SQL Server örnekleri burada listelenir. İlgili örnek veya AlwaysOn kullanılabilirlik grubunda bulunan korunmayan tüm veritabanlarını görmek için satırın sol tarafındaki köşeli çift ayraç simgesini genişletin.  

    ![Tek veritabanına sahip tüm SQL Server örnekleri gösteriliyor](./media/backup-azure-sql-database/list-of-sql-databases.png)

3. Korumak istediğiniz veritabanlarını belirledikten sonra **Tamam**'ı seçin.

   ![Veritabanını koruma](./media/backup-azure-sql-database/select-database-to-protect.png)

   Azure Backup, yedekleme yüklerini iyileştirmek için tek bir yedekleme işindeki maksimum veritabanı sayısını 50 ile sınırlar.

     * Alternatif olarak **Otomatik Koruma** sütunundaki ilgili açılan menüde **Açık** seçeneğini belirleyerek örneğin veya AlwaysOn kullanılabilirlik grubunun tamamında otomatik korumayı etkinleştirebilirsiniz. Otomatik koruma özelliği, var olan tüm veritabanlarını tek seferde etkinleştirmekle kalmaz, aynı zamanda örneğe veya kullanılabilirlik grubuna daha sonra eklenecek yeni veritabanlarını da otomatik olarak korur.  

4. **Tamam**'ı seçerek **Yedekleme ilkesi** bölmesini açın.

    ![AlwaysOn kullanılabilirlik grubunda otomatik korumayı etkinleştirme](./media/backup-azure-sql-database/enable-auto-protection.png)

5. **Yedekleme ilkesi seç** bölümünde bir ilke seçtikten sonra **Tamam**'ı seçin.

   * Varsayılan ilkeyi seçin: HourlyLogBackup.
   * SQL için daha önce oluşturulmuş olan bir yedekleme ilkesini seçin.
   * Kurtarma noktası hedefinize ve bekletme aralığınıza uygun yeni bir ilke tanımlayın.

     ![Yedekleme ilkesi seçme](./media/backup-azure-sql-database/select-backup-policy.png)

6. **Yedekleme** menüsünde **Yedeklemeyi etkinleştir**'i seçin.

    ![Seçili yedekleme ilkesini etkinleştirme](./media/backup-azure-sql-database/enable-backup-button.png)

7. Portalın **Bildirimler** alanında yapılandırmanın ilerleme durumunu takip edin.

    ![Bildirim alanı](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluşturma

Yedekleme ilkesi, yedekleme zamanını ve saklama süresini belirler.

* İlkeler, kasa düzeyinde oluşturulur.
* Bir yedekleme ilkesi birden fazla kasa tarafından kullanılabilir ancak ilgili yedekleme ilkesini her kasaya ayrıca uygulamanız gerekir.
* Bir yedekleme ilkesi oluşturduğunuzda varsayılan olarak günlük tam yedek oluşturulur.
* Haftalık tam yedek yapılandırırsanız değişiklik yedeği de ekleyebilirsiniz.
* Farklı yedekleme ilkesi türleri [hakkında bilgi edinin](backup-architecture.md#sql-server-backup-types).

Yedekleme ilkesi oluşturmak için:

1. Kasada **Yedekleme ilkeleri** > **Ekle**'yi seçin.
2. **Ekle** menüsünde ilke türünü tanımlamak için **Azure VM'de SQL Server**'ı seçin.

   ![Yeni yedekleme ilkesi için ilke türü seçme](./media/backup-azure-sql-database/policy-type-details.png)

3. **İlke adı** alanına yeni ilkenin adını girin.
4. **Tam Yedekleme ilkesi** bölümünde **Yedekleme Sıklığı** için **Günlük** veya **Haftalık** seçeneğini belirleyin.

   * **Günlük** için yedekleme işinin başlayacağı saati ve saat dilimini seçin.
   * **Tam Yedekleme** seçeneğini devre dışı bırakamazsınız, tam yedekleme gerçekleştirmeniz şarttır.
   * **Tam Yedekleme**'yi seçerek ilkeyi görüntüleyin.
   * Günlük tam yedeklemeler için değişiklik yedeği oluşturamazsınız.
   * **Haftalık** için yedekleme işinin başlayacağı haftanın gününü, saati ve saat dilimini seçin.

     ![Yeni yedekleme ilkesi alanları](./media/backup-azure-sql-database/full-backup-policy.png)  

5. Varsayılan olarak tüm **Bekletme Aralığı** seçenekleri seçilidir. Kullanmak istemediğiniz bekletme aralığı sınırlarını silin ve kullanılacak aralıkları ayarlayın.

    * Tüm yedekleme türleri (tam/değişiklik/günlük) için minimum bekletme süresi yedi gündür.
    * Kurtarma noktaları, belirtilen bekletme aralığına göre etiketlenir. Örneğin günlük tam yedek seçerseniz her gün yalnızca bir yedekleme işlemi tetiklenir.
    * Belirli bir güne ait yedekleme etiketlenir ve haftalık bekletme aralığı ile haftalık bekletme ayarına göre bekletilir.
    * Aylık ve yıllık bekletme aralıkları da benzer şekilde çalışır.

   ![Bekletme aralığı ayarları](./media/backup-azure-sql-database/retention-range-interval.png)

6. **Tam Yedekleme ilkesi** menüsünde **Tamam**'ı seçerek ayarları kabul edin.
7. Değişiklik yedeği ilkesi eklemek için **Değişiklik Yedeği**'ni seçin.

   ![Bekletme aralığı ayarları](./media/backup-azure-sql-database/retention-range-interval.png)
   ![Değişiklik yedeği ilkesi menüsünü açın](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

8. **Değişiklik Yedeği ilkesi** sayfasında **Etkinleştir**'i seçerek sıklık ve bekletme denetimlerini açın.

    * En yüksek sıklıkta her gün bir değişiklik yedeği tetikleyebilirsiniz.
    * Değişiklik yedekleri en fazla 180 gün bekletilebilir. Daha uzun süre bekletmeniz gerekiyorsa tam yedekleme ilkesini kullanmanız gerekir.

9. **Tamam**'ı seçerek ilkeyi kaydedin ve ana **Yedekleme ilkesi** menüsüne dönün.

10. İşlem günlüğü yedekleme ilkesi eklemek için **Günlük Yedekleme**'yi seçin.
11. **Günlük Yedekleme**'de **Etkinleştir**'i seçip sıklığı ve bekletme denetimlerini ayarlayın. Günlük yedeklemeleri 15 dakikada bir gerçekleştirilebilir ve 35 güne kadar bekletilebilir.
12. **Tamam**'ı seçerek ilkeyi kaydedin ve ana **Yedekleme ilkesi** menüsüne dönün.

    ![Günlük yedekleme ilkesini düzenleme](./media/backup-azure-sql-database/log-backup-policy-editor.png)

13. **Yedekleme ilkesi** menüsünde **SQL Yedekleme Sıkıştırması**'nın etkinleştirilip etkinleştirilmeyeceğini seçin.
    * Sıkıştırma varsayılan olarak devre dışıdır.
    * Azure Backup, arka uçta SQL yerel yedekleme sıkıştırmasını kullanır.

14. Yedekleme ilkesinde yaptığınız değişiklikleri tamamladıktan sonra **Tamam**'ı seçin.

## <a name="run-an-on-demand-backup"></a>İsteğe bağlı yedekleme çalıştırma

1. Kurtarma Hizmetleri kasanızda Yedekleme öğeleri'ni seçin.
2. "Azure VM’de SQL"i seçin.
3. Veritabanlarından birine sağ tıklayıp "Şimdi yedekle"yi seçin.
4. Yedekleme Türü (Tam/Değişiklik/Günlük/Yalnızca Tamamını Kopyala) ve Sıkıştırma (Etkinleştir/Devre Dışı Bırak) ayarını yapın
5. Yedeklemeyi başlatmak için Tamam'ı seçin.
6. Kurtarma Hizmetleri kasanıza gidip "Yedekleme işleri" seçeneğini belirleyerek yedekleme işini izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalı kullanarak şu işlemleri gerçekleştirdiniz:

> [!div class="checklist"]
>
> * Kasa oluşturma ve yapılandırma.
> * Veritabanlarını bulma ve yedekleme ayarlarını yapma.
> * Veritabanları için otomatik korumayı ayarlama.
> * İsteğe bağlı yedekleme çalıştırma.

Bir Azure sanal makinesini diskten geri yüklemek için sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure VM’lerinde SQL Server veritabanlarını geri yükleme](./restore-sql-database-azure-vm.md)

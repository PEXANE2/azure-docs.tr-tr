---
title: Azure VM’lerinde SQL Server veritabanlarını yedekleme
description: Bu makalede, Azure Yedekleme ile Azure sanal makinelerde SQL Server veritabanlarını nasıl yedekleyin gerektiğini öğrenin.
ms.reviewer: vijayts
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 5b10907738feeecbec06669175e82578f2915f92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273338"
---
# <a name="back-up-sql-server-databases-in-azure-vms"></a>Azure VM’lerinde SQL Server veritabanlarını yedekleme

SQL Server veritabanları, düşük kurtarma noktası hedefi (RPO) ve uzun süreli bekletme gerektiren kritik iş yükleridir. [Azure Yedekleme'yi](backup-overview.md)kullanarak Azure sanal makinelerde (VM) çalışan SQL Server veritabanlarını yedekleyebilirsiniz.

Bu makalede, Azure VM'de çalışan bir SQL Server veritabanını Azure Yedekleme Kurtarma Hizmetleri kasasına nasıl yedekleneeceğimiz gösterilmektedir.

Bu makalede, nasıl öğreneceksiniz:

> [!div class="checklist"]
>
> * Bir kasa oluşturun ve yapılandırın.
> * Veritabanlarını keşfedin ve yedeklemeler ayarlayın.
> * Veritabanları için otomatik koruma yı ayarlayın.

>[!NOTE]
>**Azure VM'deki SQL sunucusu için yumuşak silme ve Azure VM iş yüklerinde SAP HANA için yumuşak silme** artık önizlemede kullanılabilir.<br>
>Önizleme için kaydolmak için, bize yazınAskAzureBackupTeam@microsoft.com

## <a name="prerequisites"></a>Ön koşullar

BIR SQL Server veritabanını yedeklemeden önce aşağıdaki ölçütleri denetleyin:

1. SQL Server örneğini barındıran VM ile aynı bölgede ve abonelikte bir [Kurtarma Hizmetleri kasası](backup-sql-server-database-azure-vms.md#create-a-recovery-services-vault) tanımlayın veya oluşturun.
2. VM'nin [ağ bağlantısı](backup-sql-server-database-azure-vms.md#establish-network-connectivity)na sahip olduğunu doğrulayın.
3. SQL Server veritabanlarının [Azure Yedekleme için veritabanı adlandırma yönergelerine](#database-naming-guidelines-for-azure-backup)uyduğundan emin olun.
4. Veritabanı için etkin leştirilmiş başka yedekleme çözümleriniz olup olmadığını denetleyin. Veritabanını yedeklemeden önce diğer tüm SQL Server yedeklemelerini devre dışı kaldırın.

> [!NOTE]
> Azure VM için Azure Yedekleme'yi ve VM'de çakışmadan çalışan bir SQL Server veritabanı için etkinleştirebilirsiniz.

### <a name="establish-network-connectivity"></a>Ağ bağlantısı oluşturma

Tüm işlemler için BIR SQL Server VM, Azure genel IP adreslerine bağlantı gerektirir. VM işlemleri (veritabanı bulma, yedeklemeleri yapılandırma, yedekleme leri zamanlama, kurtarma noktalarını geri yükleme vb.) Azure genel IP adreslerine bağlantı olmadan başarısız olur.

Aşağıdaki seçeneklerden birini kullanarak bağlantı kurun:

#### <a name="allow-the-azure-datacenter-ip-ranges"></a>Azure veri merkezi IP aralıklarına izin ver

Bu seçenek, indirilen dosyadaki [IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653) izin verir. Bir ağ güvenlik grubuna (NSG) erişmek için Set-AzureNetworkSecurityRule cmdlet'ini kullanın. Güvenli alıcılar listeniz yalnızca bölgeye özgü IP'leri içeriyorsa, kimlik doğrulamasını etkinleştirmek için güvenli alıcılar listesini Azure Active Directory (Azure AD) hizmet etiketini de güncelleştirmeniz gerekir.

#### <a name="allow-access-using-nsg-tags"></a>NSG etiketlerini kullanarak erişime izin ver

Bağlantıyı kısıtlamak için NSG kullanıyorsanız, Azure Yedekleme'ye giden erişime izin vermek için AzureBackup hizmet etiketini kullanmanız gerekir. Ayrıca, Azure AD ve Azure Depolama [kurallarını](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) kullanarak kimlik doğrulama ve veri aktarımı için bağlantıya da izin vermelisiniz. Bu işlem Azure portalından veya PowerShell üzerinden yapılabilir.

Portalı kullanarak bir kural oluşturmak için:

  1. **Tüm Hizmetler'de** **Ağ güvenlik gruplarına** gidin ve ağ güvenlik grubunu seçin.
  2. **Ayarlar**altında **Giden güvenlik kurallarını** seçin.
  3. **Ekle'yi**seçin. [Güvenlik kuralı ayarlarında](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#security-rule-settings)açıklandığı gibi yeni bir kural oluşturmak için gerekli tüm ayrıntıları girin. **Hedef** seçeneğinin Hizmet **Etiketi** ne zaman Hedef hizmet etiketi olarak ayarlandığından emin olun, **AzureBackup** olarak ayarlanır. **AzureBackup**
  4. Yeni oluşturulan giden güvenlik kuralını kaydetmek için **Ekle'yi**tıklatın.

PowerShell kullanarak bir kural oluşturmak için:

 1. Azure hesap kimlik bilgileri ekleme ve ulusal bulutları güncelleştirme<br/>
      `Add-AzureRmAccount`<br/>

 2. NSG aboneliğini seçin<br/>
      `Select-AzureRmSubscription "<Subscription Id>"`

 3. NSG'yi seçin<br/>
    `$nsg = Get-AzureRmNetworkSecurityGroup -Name "<NSG name>" -ResourceGroupName "<NSG resource group name>"`

 4. Azure Yedekleme hizmet etiketi için çıkış kuralına izin ekleme<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureBackupAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureBackup" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

 5. Depolama hizmet etiketi için giden kurala izin verme ekleme<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "StorageAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "Storage" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

 6. AzureActiveDirectory hizmet etiketi için çıkış kuralına izin ekleme<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureActiveDirectoryAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureActiveDirectory" -DestinationPortRange 443 -Description "Allow outbound traffic to AzureActiveDirectory service"`

 7. NSG'yi kaydedin<br/>
    `Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg`

**Azure Güvenlik Duvarı etiketlerini kullanarak erişime izin verin.** Azure Güvenlik Duvarı kullanıyorsanız, AzureBackup [FQDN etiketini](https://docs.microsoft.com/azure/firewall/fqdn-tags)kullanarak bir uygulama kuralı oluşturun. Bu, Azure Yedekleme'ye giden erişime olanak tanır.

**Trafiği yönlendirmek için bir HTTP proxy sunucusu dağıtın.** Bir SQL Server veritabanını Azure VM'de yedeklediğinizde, VM'deki yedekleme uzantısı, yönetim komutlarını Azure Yedekleme'ye ve verileri Azure Depolama'ya göndermek için HTTPS API'lerini kullanır. Yedekleme uzantısı, kimlik doğrulaması için Azure AD'yi de kullanır. Bu üç hizmetin yedekleme uzantısı trafiğini HTTP proxy üzerinden yönlendirin. Proxy kurallarınız için izin listesine eklemek için Azure Yedekleme ile birlikte kullanılan joker karakter etki alanı yoktur. Azure tarafından sağlanan bu hizmetler için genel IP aralıklarını kullanmanız gerekir. Uzantılar, genel internet erişimi için yapılandırılan tek bileşendir.

Bağlantı seçenekleri aşağıdaki avantajları ve dezavantajları içerir:

**Seçeneği** | **Avantaj -ları** | **Dezavantajlar**
--- | --- | ---
IP aralıklarına izin ver | Ek ücret yok | IP adresi aralıkları zaman içinde değiştiğinden, yönetilmesi gereken karmaşık <br/><br/> Yalnızca Azure Depolama'ya değil, Azure'un tamamına erişim sağlar
NSG hizmet etiketlerini kullanma | Aralık değişiklikleri otomatik olarak birleştirilerek yönetilmesi daha kolay <br/><br/> Ek ücret yok <br/><br/> | Yalnızca NSG'lerle kullanılabilir <br/><br/> Tüm hizmete erişim sağlar
Azure Güvenlik Duvarı FQDN etiketlerini kullanma | Gerekli FQDN'ler otomatik olarak yönetildikçe yönetilmesi daha kolay | Yalnızca Azure Güvenlik Duvarı ile kullanılabilir
HTTP proxy kullanma | VM'lere tek nokta internet erişimi <br/> | Proxy yazılımı ile VM çalıştırmak için ek maliyetler <br/> Yayınlanmış FQDN adresi yok, izin kuralları Azure IP adresi değişikliklerine tabi olacak

#### <a name="private-endpoints"></a>Özel Uç Noktalar

[!INCLUDE [Private Endpoints](../../includes/backup-private-endpoints.md)]

### <a name="database-naming-guidelines-for-azure-backup"></a>Azure Yedekleme için veritabanı adlandırma yönergeleri

Veritabanı adlarında aşağıdaki öğeleri kullanmaktan kaçının:

* İzleyen ve öncü alanlar
* Sondaki ünlem işaretleri (!)
* Kare ayraçları kapatma (])
* Yarı kolon ';'
* İleri eğik çizgi '/'

Diğer ad desteklenmeyen karakterler için kullanılabilir, ancak bu karakterlerden kaçınmanızı öneririz. Daha fazla bilgi için bkz. [Tablo Hizmeti Veri Modelini anlama](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model?redirectedfrom=MSDN).

>[!NOTE]
>Adlarında "+" veya "&" gibi özel karakterlere sahip veritabanları için **Yapı koruması** işlemi desteklenmez. Veritabanı adını değiştirebilir veya bu veritabanlarını başarıyla koruyabilen **Otomatik Koruma'yı**etkinleştirebilirsiniz.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server veritabanlarını keşfedin

VM üzerinde çalışan veritabanları nasıl keşfedilir:

1. Azure [portalında,](https://portal.azure.com)veritabanını yedeklemek için kullandığınız Kurtarma Hizmetleri kasasını açın.

2. Kurtarma **Hizmetleri tonoz** panosunda **Yedekleme'yi**seçin.

   ![Yedekleme Hedefi menüsünü açmak için Yedekleme'yi seçin](./media/backup-azure-sql-database/open-backup-menu.png)

3. **Yedekleme Hedefi'nde,** **iş yükünüz nerede çalışıyor?** **Azure**

4. **Yedeklemek istediğiniz şeyde**Azure **VM'de SQL Server'ı**seçin.

    ![Yedekleme için Azure VM'de SQL Server'ı seçin](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

5. **Yedek Hedef** > **VM'lerde DB'leri Keşfedin'de,** abonelikte korumasız VM'leri aramak için Başlangıç **Bulma'yı** seçin. Bu arama, abonelikteki korumasız VM sayısına bağlı olarak biraz zaman alabilir.

   * Korumasız VM'ler, ada ve kaynak grubuna göre listelenen keşiften sonra listede görünmelidir.
   * Bir VM beklediğiniz gibi listelenmiyorsa, kasada yedeklenip yedeklenmediğine bakın.
   * Birden çok VM aynı ada sahip olabilir, ancak farklı kaynak gruplarına ait olurlar.

     ![Yedekleme, VM'lerde DB'ler için arama sırasında beklemede](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. VM listesinde, SQL Server veritabanını çalıştıran VM'yi seçin > **DB'leri Keşfedin.**

7. **Bildirimler'deki**veritabanı bulmayı izleme. Bu eylem için gereken süre VM veritabanlarının sayısına bağlıdır. Seçili veritabanları keşfedildiğinde, bir başarı iletisi görüntülenir.

    ![Dağıtım başarı iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

8. Azure Yedekleme, VM'deki tüm SQL Server veritabanlarını keşfeder. Keşif sırasında arka planda aşağıdaki öğeler oluşur:

    * Azure Yedekleme, Iş yükü yedeklemesi için VM'yi kasaya kaydeder. Kayıtlı VM'deki tüm veritabanları yalnızca bu kasaya yedeklenebilir.
    * Azure Yedekleme, VM'de AzureBackupWindowsİş iş yükü uzantısını yükler. SQL veritabanında aracı yüklü değil.
    * Azure Yedekleme, VM'de NT Service\AzureWLBackupPluginSvc hizmet hesabını oluşturur.
      * Tüm yedekleme ve geri yükleme işlemleri hizmet hesabını kullanır.
      * NT Service\AzureWLBackupPluginSvc SQL sysadmin izinleri gerektirir. Market'te oluşturulan tüm SQL Server VM'leri SqlIaSExtension yüklü olarak gelir. AzureBackupWindowsWorkload uzantısı, gerekli izinleri otomatik olarak almak için SQLIaaSExtension'ı kullanır.
    * Marketten VM'yi oluşturmadıysanız veya SQL 2008 ve 2008 R2'deyseniz, VM SqlIaaSExtension yüklü olmayabilir ve bulma işlemi UserErrorSQLNoSysAdminMembership hata iletisiyle başarısız olur. Bu sorunu gidermek için, [VM izni kümesi](backup-azure-sql-database.md#set-vm-permissions)altındaki yönergeleri izleyin.

        ![VM ve veritabanını seçin](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma  

1. **Yedekleme Hedef** > **Adım 2: Yedekleme yapılandırma,** **Yedekleme yapılandırma**seçin.

   ![Yedeklemeyi Yapılandır'ı seçin](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

2. **Yedeklemek için öğeleri**seç'te, tüm kayıtlı kullanılabilirlik gruplarını ve bağımsız SQL Server örneklerini görürsünüz. Bu örnekteki tüm korumasız veritabanlarının listesini veya Her Zaman Kullanılabilirlik Grubunda ki listeyi genişletmek için satırın solundaki oku seçin.  

    ![Tüm SQL Server örneklerini bağımsız veritabanları ile görüntüleme](./media/backup-azure-sql-database/list-of-sql-databases.png)

3. Korumak istediğiniz tüm veritabanlarını seçin ve ardından **Tamam'ı**seçin.

   ![Veritabanını koruma](./media/backup-azure-sql-database/select-database-to-protect.png)

   Yedekleme yüklerini optimize etmek için Azure Yedekleme, bir yedekleme işinde en fazla veri tabanı sayısını 50'ye ayarlar.

     * 50'den fazla veritabanını korumak için birden çok yedekleme yapılandırır.
     * AutoPROTECT açılır listesinde, tüm örneği veya **AUTOPROTECT** Her Zaman Kullanılabilirlik grubunu [etkinleştirmek](#enable-auto-protection) için **A'yı**seçin ve ardından **Tamam'ı**seçin.

    > [!NOTE]
    > [Otomatik koruma](#enable-auto-protection) özelliği yalnızca varolan tüm veritabanlarında aynı anda koruma sağlamakla kalmıyor, aynı zamanda o örne veya kullanılabilirlik grubuna eklenen yeni veritabanlarını da otomatik olarak korur.  

4. **Yedekleme ilkesini**açmak için **Tamam'ı** seçin.

    ![Her Zaman Kullanılabilirlik grubu için otomatik korumayı etkinleştirin](./media/backup-azure-sql-database/enable-auto-protection.png)

5. **Yedekleme ilkesinde,** bir ilke seçin ve sonra **Tamam'ı**seçin.

   * Varsayılan ilkeyi HourlyLogBackup olarak seçin.
   * SQL için daha önce oluşturulmuş varolan bir yedekleme ilkesini seçin.
   * RPO ve bekletme aralığınızı temel alan yeni bir ilke tanımlayın.

     ![Yedekleme ilkesini seçin](./media/backup-azure-sql-database/select-backup-policy.png)

6. **Yedekleme'de,** **Yedeklemeyi Etkinleştir'i**seçin.

    ![Seçilen yedekleme ilkesini etkinleştirme](./media/backup-azure-sql-database/enable-backup-button.png)

7. Portalın **Bildirimler** alanındaki yapılandırma ilerlemesini izleyin.

    ![Bildirim alanı](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluşturma

Yedekleme ilkesi, yedeklemelerin ne zaman alındığını ve ne kadar süreyle saklanacaklarını tanımlar.

* Kasa düzeyinde bir ilke oluşturulur.
* Birden çok kasa aynı yedekleme ilkesini kullanabilir, ancak yedekleme ilkesini her kasaya uygulamanız gerekir.
* Bir yedekleme ilkesi oluşturduğunuzda, günlük tam yedekleme varsayılandır.
* Bir diferansiyel yedekleme ekleyebilirsiniz, ancak yalnızca tam yedeklemeleri haftalık olarak gerçekleşecek şekilde yapılandırdığınızda.
* Farklı [yedekleme ilkeleri türleri](backup-architecture.md#sql-server-backup-types)hakkında bilgi edinin.

Yedekleme ilkesi oluşturmak için:

1. Kasada Yedek **ilkeleri** > **Ekle'yi**seçin.
2. **Ekle'de,** ilke türünü tanımlamak için Azure **VM'de SQL Server'ı** seçin.

   ![Yeni yedekleme ilkesi için bir ilke türü seçin](./media/backup-azure-sql-database/policy-type-details.png)

3. **İlke adına,** yeni ilke için bir ad girin.
4. **Tam Yedekleme ilkesinde,** bir **Yedekleme Frekansı**seçin. **Günlük** veya **Haftalık'ı**seçin.

   * **Günlük**için, yedekleme işinin başladığı saat ve saat dilimini seçin.
   * **Haftalık**için, yedekleme işinin başladığı haftanın gününü, saatini ve saat dilimini seçin.
   * **Tam** Yedekleme seçeneğini kapatamadığınız için tam yedekleme çalıştırın.
   * İlkeyi görüntülemek için **Tam Yedekleme'yi** seçin.
   * Günlük tam yedeklemeler için diferansiyel yedeklemeler oluşturamazsınız.

     ![Yeni yedekleme ilkesi alanları](./media/backup-azure-sql-database/full-backup-policy.png)  

5. **BEKLETME ARALIĞI'nde**tüm seçenekler varsayılan olarak seçilir. İstemediğiniz bekletme aralığı sınırlarını temizleyin ve ardından aralıkları kullanacak şekilde ayarlayın.

    * Herhangi bir yedekleme türü (tam, diferansiyel ve günlük) için minimum bekletme süresi yedi gündür.
    * Kurtarma noktaları, bekletme aralıklarına göre bekletme için etiketlenir. Örneğin, günlük tam yedekleme seçerseniz, her gün yalnızca bir tam yedekleme tetiklenir.
    * Belirli bir günün yedeklemesi, haftalık bekletme aralığına ve haftalık bekletme ayarına göre etiketlenir ve saklanır.
    * Aylık ve yıllık saklama aralıkları benzer şekilde olur.

       ![Bekletme aralığı aralığı ayarları](./media/backup-azure-sql-database/retention-range-interval.png)

6. Tam **Yedekleme ilkesi** menüsünde, ayarları kabul etmek için **Tamam'ı** seçin.
7. Diferansiyel yedekleme ilkesi eklemek için **Diferansiyel Yedekleme'yi**seçin.

   ![Bekletme aralığı](./media/backup-azure-sql-database/retention-range-interval.png)
   ![aralığı ayarları Diferansiyel yedekleme ilkesi menüsünü açın](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

8. **Diferansiyel Yedekleme ilkesinde,** frekans ve bekletme denetimlerini açmak için **Etkinleştir'i** seçin.

    * Günde yalnızca bir diferansiyel yedekleme tetikleyebilirsiniz.
    * Diferansiyel yedeklemeler en fazla 180 gün süreyle saklanabilir. Daha uzun bekletme için tam yedekleme kullanın.

9. İlkeyi kaydetmek ve ana **Yedekleme ilkesi** menüsüne dönmek için **Tamam'ı** seçin.

10. İşlem günlüğü yedekleme ilkesi eklemek için **Log Backup'ı**seçin.
11. **Günlük**Yedekleme'de, **Etkinleştir'i**seçin ve ardından sıklık ve bekletme denetimlerini ayarlayın. Günlük yedeklemeleri her 15 dakikada bir sık olarak oluşabilir ve 35 güne kadar saklanabilir.
12. İlkeyi kaydetmek ve ana **Yedekleme ilkesi** menüsüne dönmek için **Tamam'ı** seçin.

    ![Günlük yedekleme ilkesini edin](./media/backup-azure-sql-database/log-backup-policy-editor.png)

13. Yedekleme **ilkesi** menüsünde, SQL **Yedekleme Sıkıştırma'yı** etkinleştirip etkinleştirmeyeceğinizi seçin. Bu seçenek varsayılan olarak devre dışıdır. Etkinse, SQL Server VDI'ye sıkıştırılmış bir yedekleme akışı gönderir.  Azure Yedekleme'nin, bu denetimin değerine bağlı olarak sıkıştırma / NO_COMPRESSION yan tümcesi ile örnek düzeyi varsayılanlarını geçersiz kdığını lütfen unutmayın.

14. Yedekleme ilkesindeki ediyi tamamladıktan sonra **Tamam'ı**seçin.

> [!NOTE]
> Her günlük yedeklemebir kurtarma zinciri oluşturmak için önceki tam yedekleme zincirlenir. Bu tam yedekleme, son günlük yedeklemesinin bekletilmesi sona erene kadar korunur. Bu, tüm günlüklerin kurtarılabileceğinden emin olmak için tam yedeklemenin ek bir süre için tutulduğu anlamına gelebilir. Kullanıcının haftalık tam yedekleme, günlük diferansiyel ve 2 saatlik günlükleri olduğunu varsayalım. Hepsi 30 gün boyunca saklanır. Ama, haftalık tam gerçekten temizlenebilir / silinmiş sadece bir sonraki tam yedekleme sonra yani, 30 + 7 gün sonra kullanılabilir. Diyelim ki, haftalık tam yedekleme 16 Kasım'da oluyor. Saklama politikası uyarınca, 16 Aralık'a kadar bekletilmelidir. Bu tam için son günlük yedekleme 22 Kasım'da, bir sonraki planlanan tam önce olur. Bu günlük 22 Aralık'a kadar kullanılabilir olana kadar, 16 Kasım tam silinemez. Yani, 16 Kasım tam 22 Aralık'a kadar korunur.

## <a name="enable-auto-protection"></a>Otomatik korumayı etkinleştirme  

Otomatik korumanın, varolan ve gelecekteki tüm veritabanlarını tek başına bir SQL Server örneğine veya Her Zaman Kullanılabilirlik Grubuna otomatik olarak yedeklemesini sağlayabilirsiniz.

* Otomatik koruma için tek seferde seçebileceğiniz veritabanlarının sayısında bir sınır yoktur.
* Otomatik korumayı etkinleştirdiğiniz bir durumda veritabanlarını seçiçle koruyamaz veya korumadan dışlayamayacağız.
* Örneğiniz zaten bazı korumalı veritabanları içeriyorsa, otomatik korumayı açtıktan sonra bile ilgili ilkeleri altında korunurlar. Daha sonra eklenen tüm korumasız veritabanları, **Yedeklemeyi Yapılandır'da**listelenen otomatik korumayı etkinleştirme sırasında tanımladığınız tek bir ilkeye sahip olacaktır. Ancak, otomatik korumalı veritabanıyla ilişkili ilkeyi daha sonra değiştirebilirsiniz.  

Otomatik korumayı etkinleştirmek için:

  1. **Yedeklenene**Öğeler'de, otomatik korumayı etkinleştirmek istediğiniz örneği seçin.
  2. **AUTOPROTECT**altında açılan listeyi seçin, **AÇIK'ı**seçin ve ardından **Tamam'ı**seçin.

      ![Kullanılabilirlik grubunda otomatik korumayı etkinleştirme](./media/backup-azure-sql-database/enable-auto-protection.png)

  3. Yedekleme birlikte tüm veritabanları için yapılandırılır ve **Yedekleme İşler**izlenebilir.

Otomatik korumayı devre dışı balmanız gerekiyorsa, **Yedeklemeyi Yapılandır'ın**altında örnek adını seçin ve ardından örnek için **Otomatik Korumayı Devre Dışı** Nı seçin. Tüm veritabanları yedeklenmeye devam eder, ancak gelecekteki veritabanları otomatik olarak korumaaltına alınamaz.

![Söz gelimi otomatik korumayı devre dışı](./media/backup-azure-sql-database/disable-auto-protection.png)

## <a name="next-steps"></a>Sonraki adımlar

Şunları nasıl yapacağınızı öğrenin:

* [Yedeklenmiş SQL Server veritabanlarını geri yükleme](restore-sql-database-azure-vm.md)
* [Yedeklenmiş SQL Server veritabanlarını yönetme](manage-monitor-sql-database-backup.md)

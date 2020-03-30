---
title: Azure Yedekleme ile BIR SAP HANA veritabanını Azure'a yedekleme
description: Bu makalede, Bir SAP HANA veritabanını Azure Yedekleme hizmetiyle Azure sanal makinelerine nasıl yedekleyeceğimiz öğrenin.
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: deedd4d2553b3b06f76f698fdb2425a8d3878d23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248066"
---
# <a name="back-up-sap-hana-databases-in-azure-vms"></a>Azure VM’lerindeki SAP HANA veritabanlarını yedekleme

SAP HANA veritabanları, düşük kurtarma noktası hedefi (RPO) ve uzun süreli bekletme gerektiren kritik iş yükleridir. Azure Sanal Makinelerde (VM) çalışan SAP HANA veritabanlarını [Azure Yedekleme'yi](backup-overview.md)kullanarak yedekleyebilirsiniz.

Bu makalede, Azure VM'lerde çalışan SAP HANA veritabanlarının Azure Yedekleme Kurtarma Hizmetleri kasasına nasıl yedekleneğini gösterilmektedir.

Bu makalede, nasıl öğreneceksiniz:
> [!div class="checklist"]
>
> * Kasa oluşturma ve yapılandırma
> * Veritabanlarını keşfedin
> * Yedeklemeleri yapılandırma
> * İsteğe bağlı yedekleme işi çalıştırma

>[!NOTE]
>**Azure VM'deki SQL sunucusu için yumuşak silme ve Azure VM iş yüklerinde SAP HANA için yumuşak silme** artık önizlemede kullanılabilir.<br>
>Önizleme için kaydolmak için, bize yazınAskAzureBackupTeam@microsoft.com

## <a name="prerequisites"></a>Ön koşullar

Ön [koşullara](tutorial-backup-sap-hana-db.md#prerequisites) ve ön [kayıt komut dosyasının](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) yedekleme için veritabanını ayarlamak için bölümleri ne işe yaradığına bakın.

### <a name="set-up-network-connectivity"></a>Ağ bağlantısını ayarlama

Sap HANA VM, tüm işlemler için Azure genel IP adreslerine bağlantı gerektirir. VM işlemleri (veritabanı bulma, yedeklemeleri yapılandırma, yedekleme leri zamanlama, kurtarma noktalarını geri yükleme vb.) Azure genel IP adreslerine bağlantı olmadan başarısız olur.

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

**Trafiği yönlendirmek için bir HTTP proxy sunucusu dağıtın.** Bir AZURE VM'de bir SAP HANA veritabanını yedeklediğinizde, VM'deki yedekleme uzantısı, yönetim komutlarını Azure Yedekleme'ye ve verileri Azure Depolama'ya göndermek için HTTPS API'lerini kullanır. Yedekleme uzantısı, kimlik doğrulaması için Azure AD'yi de kullanır. Bu üç hizmetin yedekleme uzantısı trafiğini HTTP proxy üzerinden yönlendirin. Uzantılar, genel internet erişimi için yapılandırılan tek bileşendir.

Bağlantı seçenekleri aşağıdaki avantajları ve dezavantajları içerir:

**Seçeneği** | **Avantaj -ları** | **Dezavantajlar**
--- | --- | ---
IP aralıklarına izin ver | Ek ücret yok | IP adresi aralıkları zaman içinde değiştiğinden, yönetilmesi gereken karmaşık <br/><br/> Yalnızca Azure Depolama'ya değil, Azure'un tamamına erişim sağlar
NSG hizmet etiketlerini kullanma | Aralık değişiklikleri otomatik olarak birleştirilerek yönetilmesi daha kolay <br/><br/> Ek ücret yok <br/><br/> | Yalnızca NSG'lerle kullanılabilir <br/><br/> Tüm hizmete erişim sağlar
Azure Güvenlik Duvarı FQDN etiketlerini kullanma | Gerekli FQDN'ler otomatik olarak yönetildikçe yönetilmesi daha kolay | Yalnızca Azure Güvenlik Duvarı ile kullanılabilir
HTTP proxy kullanma | Depolama URL'leri üzerinde proxy'de taneli denetime izin verilir <br/><br/> VM'lere tek nokta internet erişimi <br/><br/> Azure IP adresi değişikliklerine tabi değildir | Proxy yazılımı ile VM çalıştırmak için ek maliyetler

#### <a name="private-endpoints"></a>Özel Uç Noktalar

[!INCLUDE [Private Endpoints](../../includes/backup-private-endpoints.md)]

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-the-databases"></a>Veritabanlarını keşfedin

1. Kasada, **Başlarken**, **Yedekleme'yi**tıklatın. **İş yükünüz nerede çalışıyor?** **SAP HANA in Azure VM**
2. **Keşfet'i Başlat'ı**tıklatın. Bu, kasa bölgesinde korumasız Linux VM'lerinin keşfini başlatır.

   * Keşiften sonra, ad ve kaynak grubuna göre listelenen portalda korumasız VM'ler görünür.
   * Bir VM beklendiği gibi listelenmiyorsa, kasada yedeklenip yedeklenmediğini kontrol edin.
   * Birden çok VM aynı ada sahip olabilir, ancak farklı kaynak gruplarına aittir.

3. **Sanal Makineler Seç'te,** veritabanı keşfi için SAP HANA VM'lere erişmek için Azure Yedekleme hizmetine izin veren komut dosyasını indirmek için bağlantıyı tıklatın.
4. Yedeklemek istediğiniz SAP HANA veritabanlarını barındıran her VM'deki komut dosyasını çalıştırın.
5. Sanal **Makineleri Seç'te**KOMUT Dosyalarını Çalıştırdıktan sonra Sanal Makineler'i seçin, VM'leri seçin. Sonra **DBs'yi Keşfedin'i**tıklatın.
6. Azure Yedekleme, VM'deki tüm SAP HANA veritabanlarını keşfeder. Azure Yedekleme, keşif sırasında VM'yi kasayla kaydeder ve VM'ye bir uzantı yükler. Veritabanında aracı yüklü değil.

    ![SAP HANA veritabanlarını keşfedin](./media/backup-azure-sap-hana-database/hana-discover.png)

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma  

Şimdi yedeklemeyi etkinleştirin.

1. Adım 2'de, **Yedeklemeyi Yapılandır'ı**tıklatın.

    ![Yedeklemeyi Yapılandır](./media/backup-azure-sap-hana-database/configure-backup.png)
2. **Yedeklemek için öğeleri**seç'te, > **Ok'u**korumak istediğiniz tüm veritabanlarını seçin.

    ![Yedeklenebilmek için öğeleri seçme](./media/backup-azure-sap-hana-database/select-items.png)
3. Yedekleme > **İlkesi'nde yedekleme ilkesini seçin,** aşağıdaki talimatlara uygun olarak veritabanları için yeni bir yedekleme ilkesi oluşturun. **Backup Policy**

    ![Yedekleme ilkesini seçin](./media/backup-azure-sap-hana-database/backup-policy.png)
4. İlkeyi oluşturduktan sonra **Yedekleme** menüsünde **Yedeklemeyi Etkinleştir'i**tıklatın.

    ![Yedeklemeyi etkinleştirme](./media/backup-azure-sap-hana-database/enable-backup.png)
5. Portalın **Bildirimler** alanında yedekleme yapılandırma ilerlemesini izleyin.

### <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluşturma

Yedekleme ilkesi, yedeklemelerin ne zaman alındığını ve ne kadar süreyle saklanacaklarını tanımlar.

* Kasa düzeyinde bir ilke oluşturulur.
* Birden çok kasa aynı yedekleme ilkesini kullanabilir, ancak yedekleme ilkesini her kasaya uygulamanız gerekir.

İlke ayarlarını aşağıdaki gibi belirtin:

1. **İlke adına,** yeni ilke için bir ad girin.

   ![İlke adı girin](./media/backup-azure-sap-hana-database/policy-name.png)
2. **Tam Yedekleme ilkesinde,** Bir **Yedekleme Frekansı**seçin, **Günlük** veya **Haftalık'ı**seçin.
   * **Günlük**: Yedekleme işinin başladığı saat ve saat dilimini seçin.
       * Tam bir yedekleme çalıştırmalısınız. Bu seçeneği kapatamazsınız.
       * İlkeyi görüntülemek için **Tam Yedekleme'yi** tıklatın.
       * Günlük tam yedeklemeler için diferansiyel yedeklemeler oluşturamazsınız.
   * **Haftalık**: Yedekleme işinin çalıştığı haftanın gününü, saatini ve saat dilimini seçin.

   ![Yedekleme frekansı seçin](./media/backup-azure-sap-hana-database/backup-frequency.png)

3. **Bekletme**Aralığı'nda, tam yedekleme için bekletme ayarlarını yapılandırın.
    * Varsayılan olarak tüm seçenekler seçilir. Kullanmak istemediğiniz bekletme aralığı sınırlarını temizleyin ve yaptığınız sınırları ayarlayın.
    * Herhangi bir yedekleme türü (tam/diferansiyel/günlük) için minimum bekletme süresi yedi gündür.
    * Kurtarma noktaları, bekletme aralıklarına göre bekletme için etiketlenir. Örneğin, günlük tam yedekleme seçerseniz, her gün yalnızca bir tam yedekleme tetiklenir.
    * Belirli bir günün yedeklemesi, haftalık bekletme aralığına ve ayarına göre etiketlenir ve saklanır.
    * Aylık ve yıllık saklama aralıkları benzer şekilde olur.

4. Tam **Yedekleme ilkesi** menüsünde, ayarları kabul etmek için **Tamam'ı** tıklatın.
5. Diferansiyel ilkesi eklemek için **Diferansiyel Yedekleme'yi** seçin.
6. **Diferansiyel Yedekleme ilkesinde,** frekans ve bekletme denetimlerini açmak için **Etkinleştir'i** seçin.
    * En fazla, günde bir diferansiyel yedekleme tetikleyebilirsiniz.
    * Diferansiyel yedeklemeler en fazla 180 gün süreyle saklanabilir. Daha uzun bekletme gerekiyorsa, tam yedekleme kullanmanız gerekir.

    ![Diferansiyel yedekleme ilkesi](./media/backup-azure-sap-hana-database/differential-backup-policy.png)

    > [!NOTE]
    > Artımlı yedeklemeler şu anda desteklenmez.

7. İlkeyi kaydetmek ve ana **Yedekleme ilkesi** menüsüne dönmek için **Tamam'ı** tıklatın.
8. İşlem günlüğü yedekleme ilkesi eklemek için **Günlük Yedekleme'yi** seçin,
    * **Günlük Yedekleme'de** **Etkinleştir'i**seçin.  SAP HANA tüm günlük yedeklemelerini yönettiği için bu devre dışı tutulamaz.
    * Frekans ve bekletme denetimlerini ayarlayın.

    > [!NOTE]
    > Günlük yedekleri yalnızca başarılı bir tam yedekleme tamamlandıktan sonra akmaya başlar.

9. İlkeyi kaydetmek ve ana **Yedekleme ilkesi** menüsüne dönmek için **Tamam'ı** tıklatın.
10. Yedekleme ilkesini tanımlamayı bitirdikten sonra **Tamam'ı**tıklatın.

> [!NOTE]
> Her günlük yedeklemebir kurtarma zinciri oluşturmak için önceki tam yedekleme zincirlenir. Bu tam yedekleme, son günlük yedeklemesinin bekletilmesi sona erene kadar korunur. Bu, tüm günlüklerin kurtarılabileceğinden emin olmak için tam yedeklemenin ek bir süre için tutulduğu anlamına gelebilir. Kullanıcının haftalık tam yedekleme, günlük diferansiyel ve 2 saatlik günlükleri olduğunu varsayalım. Hepsi 30 gün boyunca saklanır. Ama, haftalık tam gerçekten temizlenebilir / silinmiş sadece bir sonraki tam yedekleme sonra yani, 30 + 7 gün sonra kullanılabilir. Diyelim ki, haftalık tam yedekleme 16 Kasım'da oluyor. Saklama politikası uyarınca, 16 Aralık'a kadar bekletilmelidir. Bu tam için son günlük yedekleme 22 Kasım'da, bir sonraki planlanan tam önce olur. Bu günlük 22 Aralık'a kadar kullanılabilir olana kadar, 16 Kasım tam silinemez. Yani, 16 Kasım tam 22 Aralık'a kadar korunur.

## <a name="run-an-on-demand-backup"></a>İsteğe bağlı yedekleme çalıştırma

Yedeklemeler ilke zamanlamasına uygun olarak çalışır. İsteğe bağlı yedeklemeyi aşağıdaki gibi çalıştırabilirsiniz:

1. Kasa menüsünde Yedek **öğeleri**tıklatın.
2. **Yedekleme Öğeleri'nde**SAP HANA veritabanını çalıştıran VM'yi seçin ve şimdi **Yedekleme'yi**tıklatın.
3. **Yedekleme Şimdi,** kurtarma noktası nın korunması gereken son günü seçmek için takvim denetimini kullanın. Ardından **Tamam**'a tıklayın.
4. Portal bildirimlerini izleyin. Devam**Eden** **Yedekleme İşleri** > > kasa panosundaki iş ilerlemesini izleyebilirsiniz. Veritabanınızın boyutuna bağlı olarak, ilk yedeklemeyi oluşturmak biraz zaman alabilir.

## <a name="run-sap-hana-studio-backup-on-a-database-with-azure-backup-enabled"></a>Azure Yedekleme etkinleştirilmiş bir veritabanında SAP HANA Studio yedeklemeçalıştırın

Azure Yedekleme ile yedeklenen bir veritabanının yerel bir yedeklemesini (HANA Studio'yu kullanarak) almak istiyorsanız, aşağıdakileri yapın:

1. Veritabanının tamamlanması için tam veya günlük yedeklemelerini bekleyin. SAP HANA Studio / Kokpit'teki durumu kontrol edin.
2. Günlük yedeklemelerini devre dışı bırak ve yedekleme kataloğunu ilgili veritabanı için dosya sistemine ayarlayın.
3. Bunu yapmak için **systemdb** > **Configuration** > **Select Database** > **Filtresi (Log)** çift tıklayın.
4. **enable_auto_log_backup'ı** **Hayır**olarak ayarlayın.
5. **False** **için log_backup_using_backint** ayarlayın.
6. Veritabanının isteğe bağlı tam yedeklemesini alın.
7. Tam yedekleme ve katalog yedeklemesinin tamamlanmasını bekleyin.
8. Önceki ayarları Azure ayarlarına geri döndürün:
    * **enable_auto_log_backup** **Evet**olarak ayarlayın.
    * **log_backup_using_backint** **True**olarak ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM'lerde çalışan SAP HANA veritabanlarını](https://docs.microsoft.com/azure/backup/sap-hana-db-restore) nasıl geri yükleyin
* [Azure Yedekleme'yi kullanarak yedeklenen SAP HANA veritabanlarını nasıl yöneteceğimize](https://docs.microsoft.com/azure/backup/sap-hana-db-manage) öğrenin

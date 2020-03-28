---
title: Öğretici - Azure VM'lerde SAP HANA veritabanlarını yedekleme
description: Bu eğitimde, Azure VM'de çalışan SAP HANA veritabanlarını Azure Yedekleme Kurtarma Hizmetleri kasasına nasıl yedekleyeceğiz öğrenin.
ms.topic: tutorial
ms.date: 02/24/2020
ms.openlocfilehash: f64dd74ad0e038c5cad152e20ae2255de03114e3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79501443"
---
# <a name="tutorial-back-up-sap-hana-databases-in-an-azure-vm"></a>Öğretici: SAP HANA veritabanlarını Azure VM'de yedekleme

Bu öğretici, Azure VM'lerde çalışan SAP HANA veritabanlarını Azure Yedekleme Kurtarma Hizmetleri kasasına nasıl yedeklediğinizi gösterir. Bu makalede nasıl öğreneceksiniz:

> [!div class="checklist"]
>
> * Kasa oluşturma ve yapılandırma
> * Veritabanlarını keşfedin
> * Yedeklemeleri yapılandırma

[Şu](sap-hana-backup-support-matrix.md#scenario-support) anda desteklediğimiz tüm senaryolar şunlardır.

## <a name="prerequisites"></a>Ön koşullar

Yedeklemeleri yapılandırmadan önce aşağıdakileri yaptığınızdan emin olun:

* Aşağıdaki [ayarlanmış ağ bağlantısı](#set-up-network-connectivity) yordamında açıklandığı gibi Azure'a ulaşabilmesi için VM'den Internet'e bağlantıya izin verin.
* Aşağıdaki ölçütleri karşılayan **hdbuserstore'da** bir anahtar bulunmalıdır:
  * Bu varsayılan **hdbuserstore** mevcut olmalıdır
  * MDC için anahtar **NAMESERVER'ın**SQL bağlantı noktasını işaret etmelidir. SDC durumunda **INDEXSERVER'ın** SQL bağlantı noktasını işaret etmelidir.
  * Kullanıcı eklemek ve silmek için kimlik bilgileri olmalıdır
* SAP HANA yedekleme yapılandırma komut dosyasını (ön kayıt komut dosyası) HANA'nın yüklendiği sanal makinede kök kullanıcı olarak çalıştırın. [Bu komut dosyası](https://aka.ms/scriptforpermsonhana) HANA sistemini yedeklemeiçin hazırlar. Ön kayıt komut dosyası hakkında daha fazla bilgi için [ön kayıt komut dosyasının yaptığı](#what-the-pre-registration-script-does) bölüme bakın.

## <a name="set-up-network-connectivity"></a>Ağ bağlantısını ayarlama

Sap HANA VM, tüm işlemler için Azure genel IP adreslerine bağlantı gerektirir. VM işlemleri (veritabanı bulma, yedeklemeleri yapılandırma, yedekleme leri zamanlama, kurtarma noktalarını geri yükleme vb.) Azure genel IP adreslerine bağlantı olmadan başarısız olur.

Aşağıdaki seçeneklerden birini kullanarak bağlantı kurun:

### <a name="allow-the-azure-datacenter-ip-ranges"></a>Azure veri merkezi IP aralıklarına izin ver

Bu seçenek, indirilen dosyadaki [IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653) izin verir. Bir ağ güvenlik grubuna (NSG) erişmek için Set-AzureNetworkSecurityRule cmdlet'ini kullanın. Güvenli alıcılar listeniz yalnızca bölgeye özgü IP'leri içeriyorsa, kimlik doğrulamasını etkinleştirmek için güvenli alıcılar listesini Azure Active Directory (Azure AD) hizmet etiketini de güncelleştirmeniz gerekir.

### <a name="allow-access-using-nsg-tags"></a>NSG etiketlerini kullanarak erişime izin ver

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

## <a name="what-the-pre-registration-script-does"></a>Ön kayıt komut dosyası ne yapar

Ön kayıt komut dosyası nın çalıştırılışı aşağıdaki işlevleri gerçekleştirir:

* Dağıtımınızda Azure Yedekleme aracısı tarafından gerekli olan paketleri yükler veya günceller.
* Azure Yedekleme sunucuları ve Azure Etkin Dizin ve Azure Depolama gibi bağımlı hizmetlerle giden ağ bağlantısı denetimleri gerçekleştirir.
* [Ön koşulların](#prerequisites)bir parçası olarak listelenen kullanıcı anahtarını kullanarak HANA sisteminize giriş yapın. Kullanıcı anahtarı HANA sisteminde bir yedek kullanıcı (AZUREWLBACKUPHANAUSER) oluşturmak için kullanılır ve ön kayıt komut dosyası başarılı bir şekilde çalıştırıldıktan sonra kullanıcı anahtarı silinebilir.
* AZUREWLBACKUPHANAUSER bu gerekli rolleri ve izinleri atanır:
  * DATABASE ADMIN (MDC durumunda) ve BACKUP ADMIN (SDC durumunda): geri yükleme sırasında yeni veritabanları oluşturmak için.
  * KATALOG OKUYUN: yedek kataloğu okumak için.
  * SAP_INTERNAL_HANA_SUPPORT: birkaç özel tabloya erişmek için.
* Komut dosyası, tüm işlemleri (veritabanı sorguları, geri yükleme işlemleri, yapılandırma ve yedekleme çalıştırma) işlemek için HANA yedekleme eklentisi için AZUREWLBACKUPHANAUSER için **hdbuserstore'a** bir anahtar ekler.

>[!NOTE]
> [Önkoşulların](#prerequisites) bir parçası olarak listelenen kullanıcı anahtarını ön kayıt komut dosyasına parametre olarak açıkça geçirebilirsiniz:`-sk SYSTEM_KEY_NAME, --system-key SYSTEM_KEY_NAME` <br><br>
>Komut dosyasının kabul edilebis diğer parametreleri öğrenmek için komutu kullanın`bash msawb-plugin-config-com-sap-hana.sh --help`

Anahtar oluşturmayı onaylamak için, HANA makinesinde SIDADM kimlik bilgileriyle HDBSQL komutunu çalıştırın:

```hdbsql
hdbuserstore list
```

Komut çıktısı {SID}{DBNAME} tuşunu görüntülemeli ve kullanıcı AZUREWLBACKUPHANAUSER olarak gösterilmelidir.

>[!NOTE]
> Altında `/usr/sap/{SID}/home/.hdb/`benzersiz bir SSFS dosyası kümesi olduğundan emin olun. Bu yolda yalnızca bir klasör olmalıdır.

## <a name="create-a-recovery-service-vault"></a>Kurtarma Hizmeti kasası oluşturma

Kurtarma Hizmetleri kasası, zaman içinde oluşturulan yedeklemeleri ve kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri kasası, korumalı sanal makinelerle ilişkili yedekleme ilkelerini de içerir.

Kurtarma Hizmetleri kasası oluşturmak için:

1. [Azure portalında](https://portal.azure.com/)aboneliğinizde oturum açın.

2. Sol menüde **Tüm hizmetler'i** seçin

   ![Tüm hizmetleri seçin](./media/tutorial-backup-sap-hana-db/all-services.png)

3. Tüm **hizmetler** iletişim kutusunda, **Kurtarma Hizmetleri'ni**girin. Kaynak listesi, girişinize göre filtreler. Kaynaklar listesinde Kurtarma **Hizmetleri kasalarını**seçin.

   ![Kurtarma Hizmetleri kasalarını seçin](./media/tutorial-backup-sap-hana-db/recovery-services-vaults.png)

4. Kurtarma **Hizmetleri** kasaları panosunda **Ekle'yi**seçin.

   ![Kurtarma Hizmetleri kasası ekle](./media/tutorial-backup-sap-hana-db/add-vault.png)

   **Kurtarma Hizmetleri kasa** iletişim kutusu açılır. **Ad, Abonelik, Kaynak grubu** ve **Konum** değerleri sağlama

   ![Kurtarma Hizmetleri kasası oluşturma](./media/tutorial-backup-sap-hana-db/create-vault.png)

   * **Ad**: Ad, kurtarma hizmetleri kasasını tanımlamak için kullanılır ve Azure aboneliğine özgü olmalıdır. En az iki, ancak en fazla 50 karakter olan bir ad belirtin. Ad bir harfle başlamalı ve yalnızca harfler, sayılar ve tirelerden oluşmalıdır. Bu öğretici için, biz adı **SAPHanaVault**kullandım.
   * **Abonelik**: Kullanılacak aboneliği seçin. Yalnızca bir aboneliğin üyesiyseniz, bu adı görürsünüz. Hangi aboneliği kullanacağından emin değilseniz, varsayılan (önerilen) aboneliği kullanın. Yalnızca iş veya okul hesabınız birden fazla Azure aboneliğiyle ilişkiliyse birden çok seçenek vardır. Burada **SAP HANA çözüm laboratuvar aboneliğini** kullandık.
   * **Kaynak grubu**: Varolan bir kaynak grubu kullanın veya yeni bir kaynak grubu oluşturun. Burada, **SAPHANADemo**kullandık.<br>
   Aboneliğinizdeki kullanılabilir kaynak gruplarının listesini görmek için **varolan kaynağı kullan'ı**seçin ve ardından açılan liste kutusundan bir kaynak seçin. Yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** seçin ve adı girin. Kaynak grupları hakkında tam bilgi için [Azure Kaynak Yöneticisi'ne genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)bilgisine bakın.
   * **Konum**: Kasa için coğrafi bölgeyi seçin. Kasa, SAP HANA çalıştıran Sanal Makine ile aynı bölgede olmalıdır. Biz **Doğu ABD 2**kullandık.

5. **Gözden Geçir + Oluştur'u**seçin.

   ![Oluştur'& Gözden Geçir'i seçin](./media/tutorial-backup-sap-hana-db/review-create.png)

Kurtarma hizmetleri kasası şimdi oluşturuldu.

## <a name="discover-the-databases"></a>Veritabanlarını keşfedin

1. Kasada, **Başlarken**, **Yedekleme'yi**tıklatın. **İş yükünüz nerede çalışıyor?** **SAP HANA in Azure VM**
2. **Keşfet'i Başlat'ı**tıklatın. Bu, kasa bölgesinde korumasız Linux VM'lerinin keşfini başlatır. Korumak istediğiniz Azure VM'yi görürsünüz.
3. **Sanal Makineler Seç'te,** veritabanı keşfi için SAP HANA VM'lere erişmek için Azure Yedekleme hizmetine izin veren komut dosyasını indirmek için bağlantıyı tıklatın.
4. Yedeklemek istediğiniz SAP HANA veritabanı(lar)ı barındıran VM komut dosyasını çalıştırın.
5. Komut dosyasını VM'de çalıştırdıktan sonra Sanal **Makineleri Seç'te**VM'yi seçin. Sonra **DBs'yi Keşfedin'i**tıklatın.
6. Azure Yedekleme, VM'deki tüm SAP HANA veritabanlarını keşfeder. Azure Yedekleme, keşif sırasında VM'yi kasayla kaydeder ve VM'ye bir uzantı yükler. Veritabanında aracı yüklü değil.

   ![Veritabanlarını keşfedin](./media/tutorial-backup-sap-hana-db/database-discovery.png)

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma

Yedeklemek istediğimiz veritabanları keşfedildiklerine göre yedeklemeyi etkinleştirelim.

1. **Yedeklemeyi Yapılandır'ı**tıklatın.

   ![Yedeklemeyi yapılandırma](./media/tutorial-backup-sap-hana-db/configure-backup.png)

2. **Yedeklemek için öğeleri**seç'te, korumak istediğiniz bir veya daha fazla veritabanını seçin ve ardından **Tamam'ı**tıklatın.

   ![Yedeklenebilmek için öğeleri seçme](./media/tutorial-backup-sap-hana-db/select-items-to-backup.png)

3. **Yedekleme İlkesi > Yedekleme İlkesi seçin,** bir sonraki bölümdeki talimatlara uygun olarak veritabanı için yeni bir yedekleme ilkesi oluşturun.

   ![Yedekleme ilkesini seçin](./media/tutorial-backup-sap-hana-db/backup-policy.png)

4. İlkeyi oluşturduktan sonra **Yedekleme menüsünde** **Yedeklemeyi Etkinleştir'i**tıklatın.

   ![Yedeklemeyi Etkinleştir'i tıklatın](./media/tutorial-backup-sap-hana-db/enable-backup.png)

5. Portalın **Bildirimler** alanında yedekleme yapılandırma ilerlemesini izleyin.

## <a name="creating-a-backup-policy"></a>Yedekleme ilkesi oluşturma

Yedekleme ilkesi, yedeklemelerin ne zaman alındığını ve ne kadar süreyle saklanacaklarını tanımlar.

* Kasa düzeyinde bir ilke oluşturulur.
* Birden çok kasa aynı yedekleme ilkesini kullanabilir, ancak yedekleme ilkesini her kasaya uygulamanız gerekir.

İlke ayarlarını aşağıdaki gibi belirtin:

1. **İlke adına,** yeni ilke için bir ad girin. Bu durumda **SAPHANA**girin.

   ![Yeni ilke için ad girin](./media/tutorial-backup-sap-hana-db/new-policy.png)

2. **Tam Yedekleme ilkesinde,** bir **Yedekleme Frekansı**seçin. **Günlük** veya **Haftalık'ı**seçebilirsiniz. Bu öğretici için, **Günlük** yedekleme seçti.

   ![Yedekleme frekansı seçin](./media/tutorial-backup-sap-hana-db/backup-frequency.png)

3. **Bekletme**Aralığı'nda, tam yedekleme için bekletme ayarlarını yapılandırın.
   * Varsayılan olarak, tüm seçenekler seçilir. Kullanmak istemediğiniz bekletme aralığı sınırlarını temizleyin ve yaptığınız sınırları ayarlayın.
   * Herhangi bir yedekleme türü (tam/diferansiyel/günlük) için minimum bekletme süresi yedi gündür.
   * Kurtarma noktaları, bekletme aralıklarına göre bekletme için etiketlenir. Örneğin, günlük tam yedekleme seçerseniz, her gün yalnızca bir tam yedekleme tetiklenir.
   * Belirli bir günün yedeklemesi, haftalık bekletme aralığına ve ayarına göre etiketlenir ve saklanır.
   * Aylık ve yıllık saklama aralıkları benzer şekilde olur.
4. Tam **Yedekleme ilkesi** menüsünde, ayarları kabul etmek için **Tamam'ı** tıklatın.
5. Ardından, bir diferansiyel ilkesi eklemek için **Diferansiyel Yedekleme'yi** seçin.
6. **Diferansiyel Yedekleme ilkesinde,** frekans ve bekletme denetimlerini açmak için **Etkinleştir'i** seçin. Biz **30 gün**boyunca tutulur, her Pazar **02:00 am**bir diferansiyel yedekleme sağladık. **Sunday**

   ![Diferansiyel yedekleme ilkesi](./media/tutorial-backup-sap-hana-db/differential-backup-policy.png)

   >[!NOTE]
   >Artımlı yedeklemeler şu anda desteklenmez.
   >

7. İlkeyi kaydetmek ve ana **Yedekleme ilkesi** menüsüne dönmek için **Tamam'ı** tıklatın.
8. İşlem günlüğü yedekleme ilkesi eklemek için **Günlük Yedekleme'yi** seçin,
   * **Günlük Yedekleme** **etkinleştirmek**için varsayılan olarak ayarlanır. SAP HANA tüm günlük yedeklemelerini yönettiği için bu devre dışı tutulamaz.
   * Yedekleme zamanlaması olarak **2 saat** ve **15 günlük** bekletme süresi belirledik.

    ![Günlük yedekleme ilkesi](./media/tutorial-backup-sap-hana-db/log-backup-policy.png)

   >[!NOTE]
   > Günlük yedekleri yalnızca başarılı bir tam yedekleme tamamlandıktan sonra akmaya başlar.
   >

9. İlkeyi kaydetmek ve ana **Yedekleme ilkesi** menüsüne dönmek için **Tamam'ı** tıklatın.
10. Yedekleme ilkesini tanımlamayı bitirdikten sonra **Tamam'ı**tıklatın.

Sap HANA veritabanınız(lar) için yedekleme(ler) başarıyla yapılandırıldınız.

## <a name="next-steps"></a>Sonraki Adımlar

* [Azure VM'lerde çalışan SAP HANA veritabanlarında isteğe bağlı yedeklemeleri](backup-azure-sap-hana-database.md#run-an-on-demand-backup) nasıl çalıştırabilirsiniz öğrenin
* [Azure VM'lerde çalışan SAP HANA veritabanlarını](sap-hana-db-restore.md) nasıl geri yükleyin
* [Azure Yedekleme'yi kullanarak yedeklenen SAP HANA veritabanlarını nasıl yöneteceğimize](sap-hana-db-manage.md) öğrenin
* [SAP HANA veritabanlarını yedeklerken sık karşılaşılan sorunları](backup-azure-sap-hana-database-troubleshoot.md) nasıl gidereceklerini öğrenin

---
title: Öğretici-Azure VM 'lerinde SAP HANA veritabanlarını yedekleme
description: Bu öğreticide, Azure VM 'de çalışan SAP HANA veritabanlarını Azure Backup kurtarma hizmetleri kasasına nasıl yedekleyeceğinizi öğrenin.
ms.topic: tutorial
ms.date: 02/24/2020
ms.openlocfilehash: 435668dedc7efa33fd5fbfeea8671f05d070a385
ms.sourcegitcommit: be53e74cd24bbabfd34597d0dcb5b31d5e7659de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79128430"
---
# <a name="tutorial-back-up-sap-hana-databases-in-an-azure-vm"></a>Öğretici: Azure VM 'de SAP HANA veritabanlarını yedekleme

Bu öğreticide, Azure VM 'lerinde çalışan SAP HANA veritabanlarının Azure Backup kurtarma hizmetleri kasasına nasıl yedekleneceği gösterilmektedir. Bu makalede şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
>
> * Kasa oluşturma ve yapılandırma
> * Veritabanlarını bul
> * Yedekleri yapılandırma

Şu anda desteklediğimiz tüm senaryolar [aşağıda](sap-hana-backup-support-matrix.md#scenario-support) verilmiştir.

## <a name="prerequisites"></a>Önkoşullar

Yedeklemeleri yapılandırmadan önce aşağıdakileri yaptığınızdan emin olun:

* Aşağıdaki [ağ bağlantısı kurma](#set-up-network-connectivity) yordamında açıklandığı gibi, Azure 'a ULAŞABILMESI için VM 'den internet 'e bağlantıya izin verin.
* **Hdbuserstore** içinde aşağıdaki ölçütlere uyan bir anahtar bulunmalıdır:
  * Varsayılan **hdbuserstore** içinde mevcut olmalıdır
  * MDC için anahtar, **nameserver**SQL bağlantı noktasını göstermelidir. SDC söz konusu olduğunda, **ındexserver** SQL bağlantı noktasını işaret etmelidir
  * Kullanıcı eklemek ve silmek için kimlik bilgilerine sahip olmalıdır
* Kök kullanıcı olarak, HANA 'nın yüklü olduğu sanal makinede SAP HANA yedekleme yapılandırma betiğini (ön kayıt betiği) çalıştırın. [Bu betik](https://aka.ms/scriptforpermsonhana) , yedekleme IÇIN hazırlanma Hana sistemini alır. Ön kayıt betiği hakkında daha fazla bilgi edinmek için [ön kayıt betiğinin ne olduğunu](#what-the-pre-registration-script-does) öğrenin bölümüne bakın.

## <a name="set-up-network-connectivity"></a>Ağ bağlantısını ayarlama

Tüm işlemler için SAP HANA VM, Azure genel IP adresleri için bağlantı gerektirir. VM işlemleri (veritabanı bulma, yedeklemeleri yapılandırma, yedeklemeleri zamanlama, kurtarma noktalarını geri yükleme vb.), Azure genel IP adreslerine bağlantı olmadan başarısız olur.

Aşağıdaki seçeneklerden birini kullanarak bağlantı kurun:

### <a name="allow-the-azure-datacenter-ip-ranges"></a>Azure veri merkezi IP aralıklarına izin ver

Bu seçenek, indirilen dosyadaki [IP aralıklarının](https://www.microsoft.com/download/details.aspx?id=41653) kullanılmasına izin verir. Bir ağ güvenlik grubuna (NSG) erişmek için set-AzureNetworkSecurityRule cmdlet 'ini kullanın. Güvenli Alıcılar listeniz yalnızca bölgeye özgü IP 'Leri içeriyorsa, kimlik doğrulamasını etkinleştirmek için Azure Active Directory (Azure AD) hizmet etiketini güvenli alıcılar listesini de güncelleştirmeniz gerekir.

### <a name="allow-access-using-nsg-tags"></a>NSG etiketlerini kullanarak erişime izin ver

Bağlantıyı kısıtlamak için NSG kullanıyorsanız, Azure Backup giden erişime izin vermek için AzureBackup Service Tag ' i kullanmanız gerekir. Ayrıca, Azure AD ve Azure Storage [kurallarını](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) kullanarak kimlik doğrulama ve veri aktarımı için bağlantıya de izin vermeniz gerekir. Bu, Azure portal veya PowerShell aracılığıyla yapılabilir.

Portalı kullanarak bir kural oluşturmak için:

  1. **Tüm hizmetler**' de **ağ güvenlik grupları** ' na gidin ve ağ güvenlik grubunu seçin.
  2. **Ayarlar**altında **giden güvenlik kuralları** ' nı seçin.
  3. **Add (Ekle)** seçeneğini belirleyin. [Güvenlik kuralı ayarları](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#security-rule-settings)' nda açıklandığı gibi yeni bir kural oluşturmak için gereken tüm ayrıntıları girin. Seçenek **hedefinin** **hizmet etiketi** olarak ayarlandığından ve **hedef hizmet etiketinin** **AzureBackup**olarak ayarlandığından emin olun.
  4. Yeni oluşturulan giden güvenlik kuralını kaydetmek için **Ekle**' ye tıklayın.

PowerShell kullanarak bir kural oluşturmak için:

 1. Azure hesabı kimlik bilgilerini ekleme ve ulusal bulutları güncelleştirme<br/>
      `Add-AzureRmAccount`<br/>

 2. NSG aboneliğini seçin<br/>
      `Select-AzureRmSubscription "<Subscription Id>"`

 3. NSG 'yi seçin<br/>
    `$nsg = Get-AzureRmNetworkSecurityGroup -Name "<NSG name>" -ResourceGroupName "<NSG resource group name>"`

 4. Azure Backup hizmet etiketi için giden izin verme kuralı ekle<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureBackupAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureBackup" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

 5. Depolama hizmeti için giden kuralı izin ver etiketi ekle<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "StorageAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "Storage" -DestinationPortRange 443 -Description "Allow outbound traffic to Azure Backup service"`

 6. AzureActiveDirectory Service etiketi için giden izin verme kuralı ekle<br/>
    `Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name "AzureActiveDirectoryAllowOutbound" -Access Allow -Protocol * -Direction Outbound -Priority <priority> -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix "AzureActiveDirectory" -DestinationPortRange 443 -Description "Allow outbound traffic to AzureActiveDirectory service"`

 7. NSG 'yi kaydetme<br/>
    `Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg`

**Azure Güvenlik Duvarı etiketlerini kullanarak erişime Izin verin**. Azure Güvenlik duvarı kullanıyorsanız, AzureBackup [FQDN etiketini](https://docs.microsoft.com/azure/firewall/fqdn-tags)kullanarak bir uygulama kuralı oluşturun. Bu, Azure Backup giden erişimine izin verir.

**Trafiği yönlendirmek için BIR http proxy sunucusu dağıtın**. Azure VM 'de bir SAP HANA veritabanını yedeklerken, VM 'deki yedekleme uzantısı, Azure depolama 'ya Azure Backup ve verilere yönetim komutları göndermek için HTTPS API 'Lerini kullanır. Yedekleme uzantısı, kimlik doğrulaması için Azure AD 'yi de kullanır. Bu üç hizmetin yedekleme uzantısı trafiğini HTTP proxy üzerinden yönlendirin. Uzantılar, genel internet erişimi için yapılandırılan tek bileşendir.

Bağlantı seçenekleri aşağıdaki avantajları ve dezavantajları içerir:

**Seçenek** | **Üstünlü** | **Olumsuz**
--- | --- | ---
IP aralıklarına izin ver | Ek maliyet yok | IP adresi aralıkları zaman içinde değiştiğinden yönetilmesi karmaşıktır <br/><br/> Yalnızca Azure Storage değil Azure 'un tamamına erişim sağlar
NSG hizmet etiketlerini kullanma | Aralık değişikliklerinin otomatik olarak birleştirilmesi için daha kolay yönetilmesi <br/><br/> Ek maliyet yok <br/><br/> | Yalnızca NSG 'ler ile kullanılabilir <br/><br/> Hizmetin tamamına erişim sağlar
Azure Güvenlik Duvarı FQDN etiketlerini kullanma | Gerekli FQDN 'Ler otomatik olarak yönetildiğinden yönetimi daha kolay | Yalnızca Azure Güvenlik Duvarı ile kullanılabilir
HTTP proxy kullanma | Depolama URL 'Lerinde ara sunucuya ayrıntılı denetime izin verilir <br/><br/> VM 'lere tek bir internet erişimi noktası <br/><br/> Azure IP adresi değişikliklerine tabi değildir | Proxy yazılımıyla VM çalıştırmak için ek maliyetler

## <a name="what-the-pre-registration-script-does"></a>Ön kayıt betiği ne yapar

Ön kayıt betiğini çalıştırmak aşağıdaki işlevleri gerçekleştirir:

* Dağıtım Azure Backup aracısının gerektirdiği gerekli paketleri kurar veya güncelleştirir.
* Azure Backup sunucularıyla giden ağ bağlantı denetimlerini ve Azure Active Directory ve Azure depolama gibi bağımlı hizmetleri gerçekleştirir.
* [Önkoşulların](#prerequisites)bir parçası olarak listelenen Kullanıcı anahtarını kullanarak Hana sisteminizde oturum açar. Kullanıcı anahtarı, HANA sisteminde bir yedek Kullanıcı (AZUREWLBACKUPHANAUSER) oluşturmak için kullanılır ve ön kayıt betiği başarıyla çalıştıktan sonra Kullanıcı anahtarı silinebilir.
* AZUREWLBACKUPHANAUSER bu gerekli roller ve izinler atanır:
  * VERITABANı YÖNETICISI: geri yükleme sırasında yeni veritabanları oluşturmak için.
  * Katalog okuma: yedekleme kataloğunu okumak için.
  * SAP_INTERNAL_HANA_SUPPORT: birkaç özel tabloya erişmek için.
* Betik, tüm işlemleri (veritabanı sorguları, geri yükleme işlemleri, yedekleme yapılandırması ve çalıştırma) işlemek üzere HANA eklentisi için AZUREWLBACKUPHANAUSER için **hdbuserstore** 'a bir anahtar ekler.

Anahtar oluşturmayı onaylamak için, SIDADDM kimlik bilgileriyle HANA makinesinde HDBSQL komutunu çalıştırın:

```hdbsql
hdbuserstore list
```

Komut çıktısı {SID} {DBNAME} anahtarını, AZUREWLBACKUPHANAUSER olarak gösterilen kullanıcıyla görüntülemelidir.

>[!NOTE]
> `/usr/sap/{SID}/home/.hdb/`altında farklı bir SSFS dosyaları kümesine sahip olduğunuzdan emin olun. Bu yolda yalnızca bir klasör olmalıdır.

## <a name="create-a-recovery-service-vault"></a>Kurtarma hizmeti Kasası oluşturma

Kurtarma Hizmetleri Kasası, zaman içinde oluşturulan yedeklemeleri ve kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri Kasası, korunan sanal makinelerle ilişkili yedekleme ilkelerini de içerir.

Kurtarma Hizmetleri kasası oluşturmak için:

1. [Azure Portal](https://portal.azure.com/)aboneliğinizde oturum açın.

2. Sol taraftaki menüden **tüm hizmetler** ' i seçin.

   ![Tüm hizmetleri seçin](./media/tutorial-backup-sap-hana-db/all-services.png)

3. **Tüm hizmetler** Iletişim kutusunda **Kurtarma Hizmetleri**' ni girin. Giriş listenize göre filtrelerin kaynak listesi. Kaynak listesinde, **Kurtarma Hizmetleri kasaları**' nı seçin.

   ![Kurtarma Hizmetleri kasalarını seçin](./media/tutorial-backup-sap-hana-db/recovery-services-vaults.png)

4. **Kurtarma Hizmetleri** kasaları panosunda **Ekle**' yi seçin.

   ![Kurtarma Hizmetleri Kasası Ekle](./media/tutorial-backup-sap-hana-db/add-vault.png)

   **Kurtarma Hizmetleri Kasası** iletişim kutusu açılır. **Ad, abonelik, kaynak grubu** ve **konum** için değerler sağlayın

   ![Kurtarma Hizmetleri kasası oluşturma](./media/tutorial-backup-sap-hana-db/create-vault.png)

   * **Ad**: ad, kurtarma hizmetleri kasasını tanımlamak için kullanılır ve Azure aboneliğinin benzersiz olması gerekir. En az iki, 50 karakterden daha fazla olmayan bir ad belirtin. Ad bir harfle başlamalı ve yalnızca harf, rakam ve kısa çizgi içermelidir. Bu öğreticide, **Saphanavault**adını kullandık.
   * **Abonelik**: kullanılacak aboneliği seçin. Yalnızca bir aboneliğin üyesiyseniz, bu adı görürsünüz. Hangi aboneliğin kullanılacağı konusunda emin değilseniz, varsayılan (önerilen) aboneliği kullanın. Yalnızca iş veya okul hesabınızın birden fazla Azure aboneliğiyle ilişkilendirilmesi durumunda birden çok seçenek vardır. Burada **SAP HANA çözüm Laboratuvarı abonelik** aboneliğini kullandık.
   * **Kaynak grubu**: var olan bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Burada **Saphanademo**kullandık.<br>
   Aboneliğinizdeki kullanılabilir kaynak gruplarının listesini görmek için **Varolanı kullan**' ı seçin ve ardından aşağı açılan liste kutusundan bir kaynak seçin. Yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' u seçin ve adı girin. Kaynak grupları hakkında tüm bilgiler için bkz. [Azure Resource Manager genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
   * **Konum**: kasa için coğrafi bölgeyi seçin. Kasa, SAP HANA çalıştıran sanal makine ile aynı bölgede olmalıdır. **Doğu ABD 2**kullandık.

5. **Gözden geçir + oluştur**' u seçin.

   ![Gözden geçirme & Oluştur ' u seçin](./media/tutorial-backup-sap-hana-db/review-create.png)

Kurtarma Hizmetleri Kasası artık oluşturulmuştur.

## <a name="discover-the-databases"></a>Veritabanlarını bulma

1. Kasadaki **Başlarken**' de, **Yedekle**' ye tıklayın. **İş yükünüz nerede çalışıyor?** , **Azure VM 'de SAP HANA**' yi seçin.
2. **Bulmayı Başlat**' a tıklayın. Bu işlem, kasa bölgesinde korunmayan Linux VM 'lerinin bulunmasını başlatır. Korumak istediğiniz Azure VM 'yi görürsünüz.
3. **Sanal makineler Seç**bölümünde, veritabanı bulma Için SAP HANA VM 'lerine erişmek üzere Azure Backup hizmetine izinler sağlayan betiği indirmek için bağlantıya tıklayın.
4. Betiği, yedeklemek istediğiniz SAP HANA veritabanlarını barındıran VM üzerinde çalıştırın.
5. Betiği VM üzerinde çalıştırdıktan sonra, **sanal makineler Seç**bölümünde VM 'yi seçin. Ardından veritabanlarını **bul**' a tıklayın.
6. Azure Backup VM 'deki tüm SAP HANA veritabanlarını bulur. Bulma sırasında Azure Backup, VM 'yi kasayla kaydeder ve VM 'ye bir uzantı kurar. Veritabanına hiçbir aracı yüklü değil.

   ![Veritabanlarını bulma](./media/tutorial-backup-sap-hana-db/database-discovery.png)

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma

Şimdi yedeklemek istediğimiz veritabanları keşfedildiğine göre yedeklemeyi etkinleştirelim.

1. **Yedeklemeyi Yapılandır**öğesine tıklayın.

   ![Yedeklemeyi yapılandırma](./media/tutorial-backup-sap-hana-db/configure-backup.png)

2. **Yedeklenecek öğeleri seçin**' de korumak istediğiniz bir veya daha fazla veritabanını seçin ve ardından **Tamam**' a tıklayın.

   ![Yedeklenecek öğeleri seçin](./media/tutorial-backup-sap-hana-db/select-items-to-backup.png)

3. **Yedekleme ilkesi > yedekleme ilkesi**' ni seçin, sonraki bölümdeki yönergelere uygun olarak veritabanları için yeni bir yedekleme ilkesi oluşturun.

   ![Yedekleme ilkesi seçin](./media/tutorial-backup-sap-hana-db/backup-policy.png)

4. İlkeyi oluşturduktan sonra **yedekleme menüsünde** **yedeklemeyi etkinleştir**' e tıklayın.

   ![Yedeklemeyi etkinleştir 'e tıklayın](./media/tutorial-backup-sap-hana-db/enable-backup.png)

5. Yedekleme Yapılandırma ilerlemesini portalın **Bildirimler** alanında izleyin.

## <a name="creating-a-backup-policy"></a>Yedekleme ilkesi oluşturma

Yedekleme ilkesi, yedeklemelerin ne zaman alındığını ve ne kadar süreyle korunduğunu tanımlar.

* Kasa düzeyinde bir ilke oluşturulur.
* Birden çok kasa aynı yedekleme ilkesini kullanabilir, ancak yedekleme ilkesini her kasaya uygulamanız gerekir.

İlke ayarlarını aşağıdaki gibi belirtin:

1. **İlke adı**alanına yeni ilke için bir ad girin. Bu durumda, **Saphana**girin.

   ![Yeni ilke için ad girin](./media/tutorial-backup-sap-hana-db/new-policy.png)

2. **Tam yedekleme ilkesinde**, bir **yedekleme sıklığı**seçin. **Günlük** veya **haftalık**seçeneklerinden birini belirleyebilirsiniz. Bu öğretici için **günlük** yedeklemeyi seçtik.

   ![Yedekleme sıklığı seçin](./media/tutorial-backup-sap-hana-db/backup-frequency.png)

3. **Bekletme aralığı**' nda, tam yedekleme için bekletme ayarlarını yapılandırın.
   * Varsayılan olarak, tüm seçenekler seçilidir. Kullanmak istemediğiniz tüm Bekletme aralığı sınırlarını temizleyin ve yaptığınız değişiklikleri ayarlayın.
   * Herhangi bir yedekleme türü için en düşük saklama süresi (tam/fark/günlük) yedi gündür.
   * Kurtarma noktaları, bekletme aralığına göre bekletme için etiketlenir. Örneğin, günlük tam yedekleme seçerseniz, her gün yalnızca bir tam yedekleme tetiklenir.
   * Belirli bir günün yedeklemesi, haftalık bekletme aralığına ve ayarına göre etiketlenebilir ve korunur.
   * Aylık ve yıllık bekletme aralıkları benzer bir şekilde davranır.
4. **Tam yedekleme ilkesi** menüsünde, ayarları kabul etmek için **Tamam** ' ı tıklatın.
5. Ardından bir fark ilkesi eklemek için **fark yedeklemesi** ' ni seçin.
6. **Değişiklik yedekleme ilkesinde**, sıklık ve bekletme denetimlerini açmak için **Etkinleştir** ' i seçin. **30 gün**boyunca tutulan **2:00**' de her **Pazar günü** bir değişiklik yedeklemesini etkinleştirdik.

   ![Değişiklik yedekleme ilkesi](./media/tutorial-backup-sap-hana-db/differential-backup-policy.png)

   >[!NOTE]
   >Artımlı yedeklemeler Şu anda desteklenmiyor.
   >

7. İlkeyi kaydetmek ve ana **yedekleme ilkesi** menüsüne dönmek için **Tamam** ' ı tıklatın.
8. İşlem günlüğü yedekleme ilkesi eklemek için **günlük yedeklemesi** ' ni seçin,
   * **Günlük yedeklemesi** varsayılan olarak **Enable**olarak ayarlanmıştır. Tüm günlük yedeklemelerini SAP HANA yönettiğinden bu devre dışı bırakılamaz.
   * Yedekleme zamanlaması ve **15 günlük** Bekletme dönemi olarak **2 saat** ayarlandık.

    ![Günlük yedekleme ilkesi](./media/tutorial-backup-sap-hana-db/log-backup-policy.png)

   >[!NOTE]
   > Günlük yedeklemeleri, yalnızca bir başarılı tam yedekleme tamamlandıktan sonra Flow 'a başlar.
   >

9. İlkeyi kaydetmek ve ana **yedekleme ilkesi** menüsüne dönmek için **Tamam** ' ı tıklatın.
10. Yedekleme ilkesini tanımlamayı tamamladıktan sonra, **Tamam**' a tıklayın.

Artık SAP HANA veritabanınız için yedekleme (ler) i başarıyla yapılandırdınız.

## <a name="next-steps"></a>Sonraki Adımlar

* [Azure VM 'lerinde çalışan SAP HANA veritabanlarında isteğe bağlı yedeklemeler çalıştırmayı](backup-azure-sap-hana-database.md#run-an-on-demand-backup) öğrenin
* [Azure VM 'lerinde çalışan SAP HANA veritabanlarını geri yüklemeyi](sap-hana-db-restore.md) öğrenin
* [Kullanarak yedeklenen SAP HANA veritabanlarını yönetmeyi öğrenin Azure Backup](sap-hana-db-manage.md)
* [SAP HANA veritabanlarını yedeklerken sık karşılaşılan sorunları gidermeye nasıl sorun](backup-azure-sap-hana-database-troubleshoot.md) alabileceğinizi öğrenin

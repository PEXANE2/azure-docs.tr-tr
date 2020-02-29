---
title: Azure Backup ile Azure 'da bir SAP HANA veritabanını yedekleme
description: Bu makalede, Azure Backup hizmeti ile SAP HANA bir veritabanını Azure sanal makinelerine nasıl yedekleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: a5fd09e0e487d103e8bd78964c11b572a62e28fa
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78164619"
---
# <a name="back-up-sap-hana-databases-in-azure-vms"></a>Azure VM’lerindeki SAP HANA veritabanlarını yedekleme

SAP HANA veritabanları, düşük kurtarma noktası hedefi (RPO) ve uzun süreli saklama gerektiren kritik iş yükleridir. [Azure Backup](backup-overview.md)kullanarak Azure sanal makinelerinde (VM) çalışan SAP HANA veritabanlarını yedekleyebilirsiniz.

Bu makalede, Azure sanal makinelerinde çalışan SAP HANA veritabanlarının Azure Backup kurtarma hizmetleri kasasına nasıl yedekleneceği gösterilmektedir.

Bu makalede, öğreneceksiniz nasıl yapılır:
> [!div class="checklist"]
>
> * Kasa oluşturma ve yapılandırma
> * Veritabanlarını bul
> * Yedekleri yapılandırma
> * İsteğe bağlı yedekleme işi çalıştırma

>[!NOTE]
>Azure **VM 'de SQL Server Için geçici silme ve Azure VM iş yükleri SAP HANA için geçici silme** , artık önizleme aşamasında kullanıma sunuldu.<br>
>Önizlemeye kaydolmak için AskAzureBackupTeam@microsoft.com adresinden bize yazın

## <a name="prerequisites"></a>Önkoşullar

[Önkoşulları](tutorial-backup-sap-hana-db.md#prerequisites) ve [ön kayıt betiği](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) , yedekleme için veritabanını ayarlama bölümlerine bakın.

### <a name="set-up-network-connectivity"></a>Ağ bağlantısını ayarlama

Tüm işlemler için SAP HANA VM, Azure genel IP adresleri için bağlantı gerektirir. VM işlemleri (veritabanı bulma, yedeklemeleri yapılandırma, yedeklemeleri zamanlama, kurtarma noktalarını geri yükleme vb.), Azure genel IP adreslerine bağlantı olmadan başarısız olur.

Aşağıdaki seçeneklerden birini kullanarak bağlantı kurun:

#### <a name="allow-the-azure-datacenter-ip-ranges"></a>Azure veri merkezi IP aralıklarına izin ver

Bu seçenek, indirilen dosyadaki [IP aralıklarının](https://www.microsoft.com/download/details.aspx?id=41653) kullanılmasına izin verir. Bir ağ güvenlik grubuna (NSG) erişmek için set-AzureNetworkSecurityRule cmdlet 'ini kullanın. Güvenli Alıcılar listeniz yalnızca bölgeye özgü IP 'Leri içeriyorsa, kimlik doğrulamasını etkinleştirmek için Azure Active Directory (Azure AD) hizmet etiketini güvenli alıcılar listesini de güncelleştirmeniz gerekir.

#### <a name="allow-access-using-nsg-tags"></a>NSG etiketlerini kullanarak erişime izin ver

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

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-the-databases"></a>Veritabanlarını bulma

1. Kasadaki **Başlarken**' de, **Yedekle**' ye tıklayın. **İş yükünüz nerede çalışıyor?** , **Azure VM 'de SAP HANA**' yi seçin.
2. **Bulmayı Başlat**' a tıklayın. Bu işlem, kasa bölgesinde korunmayan Linux VM 'lerinin bulunmasını başlatır.

   * Bulma sonrasında, korumasız VM 'Ler ad ve kaynak grubuna göre listelenmiş şekilde portalda görüntülenir.
   * Bir VM beklendiği gibi listelenmiyorsa, bir kasada zaten yedeklenmiş olup olmadığını kontrol edin.
   * Birden çok VM aynı ada sahip olabilir, ancak bunlar farklı kaynak gruplarına aittir.

3. **Sanal makineler Seç**bölümünde, veritabanı bulma Için SAP HANA VM 'lerine erişmek üzere Azure Backup hizmetine izinler sağlayan betiği indirmek için bağlantıya tıklayın.
4. Betiği, yedeklemek istediğiniz her bir sanal makine barındırma SAP HANA veritabanları üzerinde çalıştırın.
5. VM 'lerde betiği çalıştırdıktan sonra, **sanal makineleri seçin**' de VM 'ler ' i seçin. Ardından veritabanlarını **bul**' a tıklayın.
6. Azure Backup VM 'deki tüm SAP HANA veritabanlarını bulur. Bulma sırasında Azure Backup, VM 'yi kasayla kaydeder ve VM 'ye bir uzantı kurar. Veritabanına hiçbir aracı yüklü değil.

    ![SAP HANA veritabanlarını bul](./media/backup-azure-sap-hana-database/hana-discover.png)

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma  

Şimdi yedeklemeyi etkinleştirin.

1. 2\. adımda **yedeklemeyi Yapılandır**' a tıklayın.

    ![Yedeklemeyi Yapılandır](./media/backup-azure-sap-hana-database/configure-backup.png)
2. **Yedeklenecek öğeleri seçin**' de, korumak istediğiniz tüm veritabanlarını seçin > **Tamam**' a tıklayın.

    ![Yedeklenecek öğeleri seçin](./media/backup-azure-sap-hana-database/select-items.png)
3. **Yedekleme ilkesi** > **yedekleme Ilkesi**' ni seçin ve aşağıdaki yönergelere uygun olarak veritabanları için yeni bir yedekleme ilkesi oluşturun.

    ![Yedekleme ilkesi seçin](./media/backup-azure-sap-hana-database/backup-policy.png)
4. İlkeyi oluşturduktan sonra **yedekleme** menüsünde **yedeklemeyi etkinleştir**' e tıklayın.

    ![Yedeklemeyi etkinleştir](./media/backup-azure-sap-hana-database/enable-backup.png)
5. Yedekleme Yapılandırma ilerlemesini portalın **Bildirimler** alanında izleyin.

### <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluştur

Yedekleme ilkesi, yedeklemelerin ne zaman alındığını ve ne kadar süreyle korunduğunu tanımlar.

* Kasa düzeyinde bir ilke oluşturulur.
* Birden çok kasa aynı yedekleme ilkesini kullanabilir, ancak yedekleme ilkesini her kasaya uygulamanız gerekir.

İlke ayarlarını aşağıdaki gibi belirtin:

1. **İlke adı**alanına yeni ilke için bir ad girin.

   ![İlke adını girin](./media/backup-azure-sap-hana-database/policy-name.png)
2. **Tam yedekleme ilkesinde**, bir **yedekleme sıklığı**seçin, **günlük** veya **haftalık**' ı seçin.
   * **Günlük**: yedekleme işinin başladığı saat ve saat dilimini seçin.
       * Tam yedekleme çalıştırmanız gerekir. Bu seçeneği devre dışı bırakabilirsiniz.
       * İlkeyi görüntülemek için **tam yedekleme** ' ye tıklayın.
       * Günlük tam yedeklemeler için fark yedeklemeleri oluşturamazsınız.
   * **Haftalık**: yedekleme işinin çalıştığı haftanın gününü, saatini ve saat dilimini seçin.

   ![Yedekleme sıklığını seçin](./media/backup-azure-sap-hana-database/backup-frequency.png)

3. **Bekletme aralığı**' nda, tam yedekleme için bekletme ayarlarını yapılandırın.
    * Varsayılan olarak tüm seçenekler seçilidir. Kullanmak istemediğiniz tüm Bekletme aralığı sınırlarını temizleyin ve yaptığınız değişiklikleri ayarlayın.
    * Herhangi bir yedekleme türü için en düşük saklama süresi (tam/fark/günlük) yedi gündür.
    * Kurtarma noktaları, bekletme aralığına göre bekletme için etiketlenir. Örneğin, günlük tam yedekleme seçerseniz, her gün yalnızca bir tam yedekleme tetiklenir.
    * Belirli bir günün yedeklemesi, haftalık bekletme aralığına ve ayarına göre etiketlenebilir ve korunur.
    * Aylık ve yıllık bekletme aralıkları benzer bir şekilde davranır.

4. **Tam yedekleme ilkesi** menüsünde, ayarları kabul etmek için **Tamam** ' ı tıklatın.
5. Fark ilkesi eklemek için değişiklik **yedeklemesi** ' ni seçin.
6. **Değişiklik yedekleme ilkesinde**, sıklık ve bekletme denetimlerini açmak için **Etkinleştir** ' i seçin.
    * En çok, her gün bir değişiklik yedeklemesi tetikleyebilirsiniz.
    * Değişiklik yedeklemeleri, en fazla 180 gün boyunca korunabilir. Daha uzun bekletme yapmanız gerekiyorsa tam yedeklemeler kullanmanız gerekir.

    ![Değişiklik yedekleme ilkesi](./media/backup-azure-sap-hana-database/differential-backup-policy.png)

    > [!NOTE]
    > Artımlı yedeklemeler Şu anda desteklenmiyor.

7. İlkeyi kaydetmek ve ana **yedekleme ilkesi** menüsüne dönmek için **Tamam** ' ı tıklatın.
8. İşlem günlüğü yedekleme ilkesi eklemek için **günlük yedeklemesi** ' ni seçin,
    * **Günlük yedeklemesi**' nde **Etkinleştir**' i seçin.  Tüm günlük yedeklemelerini SAP HANA yönettiğinden bu devre dışı bırakılamaz.
    * Sıklık ve bekletme denetimlerini ayarlayın.

    > [!NOTE]
    > Günlük yedeklemeleri, yalnızca başarılı bir tam yedekleme tamamlandıktan sonra Flow 'a başlar.

9. İlkeyi kaydetmek ve ana **yedekleme ilkesi** menüsüne dönmek için **Tamam** ' ı tıklatın.
10. Yedekleme ilkesini tanımlamayı tamamladıktan sonra, **Tamam**' a tıklayın.

> [!NOTE]
> Her günlük yedeklemesi, bir kurtarma zinciri oluşturmak için önceki tam yedeklemeye zincirlenir. Bu tam yedekleme, son günlük yedeklemenin saklama süresi sona erene kadar bekletilecektir. Bu, tüm günlüklerin kurtarılabilmesini sağlamak için ek bir süre boyunca tam yedeklemenin korunduğu anlamına gelebilir. Kullanıcının haftalık tam yedekleme, günlük fark ve 2 saatlik günlük olduğunu varsayalım. Bunların hepsi 30 gün boyunca tutulur. Ancak, haftalık tam yedekleme yalnızca bir sonraki tam yedekleme kullanılabilir olduktan sonra temizlenir/silinebilir (30 + 7 gün sonra). , Haftalık tam yedekleme 16 Kasım 'da gerçekleşir. Bekletme ilkesine göre, 16 Aralık 'a kadar saklanması gerekir. Bu tam için en son günlük yedeklemesi, 22nd tarihinde bir sonraki zamanlanmış tam önce gerçekleşir. Bu günlük, 22nd 'e kadar kullanılabilir olana kadar, 16. ve tam 6. Bu nedenle, 16. dolu, 22nd 'e kadar korunur.

## <a name="run-an-on-demand-backup"></a>İsteğe bağlı yedekleme çalıştırma

Yedeklemeler, ilke zamanlamasına uygun olarak çalışır. İsteğe bağlı bir yedeklemeyi aşağıdaki gibi çalıştırabilirsiniz:

1. Kasa menüsünde, **yedekleme öğeleri**' ne tıklayın.
2. **Yedekleme öğeleri**' nde, SAP HANA VERITABANıNı çalıştıran VM 'yi seçin ve **Şimdi Yedekle**' ye tıklayın.
3. **Şimdi Yedekle**' de, kurtarma noktasının tutulacağı son günü seçmek için Takvim denetimini kullanın. Daha sonra, **Tamam**'a tıklayın.
4. Portal bildirimlerini izleyin. İş ilerlemesini kasa panosunda izleyebilirsiniz > **yedekleme işleri** > **devam**ediyor. Veritabanınızın boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.

## <a name="run-sap-hana-studio-backup-on-a-database-with-azure-backup-enabled"></a>Azure Backup etkin bir veritabanında SAP HANA Studio yedeklemesini çalıştırma

Azure Backup ile yedeklenen bir veritabanının yerel yedeklemesini (HANA Studio 'Yu kullanarak) almak istiyorsanız aşağıdakileri yapın:

1. Veritabanının tamamlaması için herhangi bir tam veya günlük yedeklemesi bekleyin. SAP HANA Studio/Kokpit içindeki durumu denetleyin.
2. Günlük yedeklemelerini devre dışı bırakın ve ilgili veritabanı için yedekleme kataloğunu dosya sistemine ayarlayın.
3. Bunu yapmak için **SystemDB** > **yapılandırması** ' na çift tıklayın > veritabanı > **filtresi (günlük)** **seçeneğini belirleyin** .
4. **Enable_auto_log_backup** **Hayır**olarak ayarlayın.
5. **Log_backup_using_backint** **false**olarak ayarlayın.
6. Veritabanının isteğe bağlı tam yedeklemesini yapın.
7. Tam yedekleme ve Katalog yedeklemesinin bitmesini bekleyin.
8. Önceki ayarları Azure için geri döndürür:
    * **Enable_auto_log_backup** **Evet**olarak ayarlayın.
    * **Log_backup_using_backint** **true**olarak ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM 'lerinde çalışan SAP HANA veritabanlarını geri yüklemeyi](https://docs.microsoft.com/azure/backup/sap-hana-db-restore) öğrenin
* [Kullanarak yedeklenen SAP HANA veritabanlarını yönetmeyi öğrenin Azure Backup](https://docs.microsoft.com/azure/backup/sap-hana-db-manage)

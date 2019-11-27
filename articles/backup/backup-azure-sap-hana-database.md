---
title: Azure Backup ile Azure 'da bir SAP HANA veritabanını yedekleme
description: Bu makalede, Azure Backup hizmeti ile SAP HANA bir veritabanını Azure sanal makinelerine nasıl yedekleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: ed47f18c9dabc685d6fbe02804562ef86a93190a
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74285896"
---
# <a name="back-up-sap-hana-databases-in-azure-vms"></a>Azure VM’lerindeki SAP HANA veritabanlarını yedekleme

SAP HANA veritabanları, düşük kurtarma noktası hedefi (RPO) ve uzun süreli saklama gerektiren kritik iş yükleridir. [Azure Backup](backup-overview.md)kullanarak Azure sanal makinelerinde (VM) çalışan SAP HANA veritabanlarını yedekleyebilirsiniz.

Bu makalede, Azure sanal makinelerinde çalışan SAP HANA veritabanlarının Azure Backup kurtarma hizmetleri kasasına nasıl yedekleneceği gösterilmektedir.

Bu makalede, öğreneceksiniz nasıl yapılır:
> [!div class="checklist"]
>
> * Kasa oluşturma ve yapılandırma
> * Veritabanlarını bul
> * Yedeklemeleri yapılandırma
> * İsteğe bağlı yedekleme işi çalıştırma

## <a name="prerequisites"></a>Önkoşullar

Veritabanını yedekleme için ayarlamak üzere [önkoşulları](tutorial-backup-sap-hana-db.md#prerequisites) ve [izin bölümlerine ayarlama](tutorial-backup-sap-hana-db.md#setting-up-permissions) bölümüne bakın.

### <a name="set-up-network-connectivity"></a>Ağ bağlantısını ayarlama

Tüm işlemler için SAP HANA VM, Azure genel IP adresleri için bağlantı gerektirir. VM işlemleri (veritabanı bulma, yedeklemeleri yapılandırma, yedeklemeleri zamanlama, kurtarma noktalarını geri yükleme vb.) bağlantı olmadan çalışmaz. Azure veri merkezi IP aralıklarına erişime izin vererek bağlantı kurun:

* Azure veri merkezleri için [IP adresi aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653) indirebilir ve ardından bu IP adreslerine erişime izin verebilirsiniz.
* Ağ güvenlik grupları (NSG 'ler) kullanıyorsanız, tüm Azure genel IP adreslerine izin vermek için Azurecsesli [hizmet etiketini](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) kullanabilirsiniz. NSG kurallarını değiştirmek için [set-AzureNetworkSecurityRule cmdlet 'ini](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurenetworksecurityrule?view=azuresmps-4.0.0) kullanabilirsiniz.
* Aktarım, HTTPS üzerinden olduğundan, bağlantı noktası 443, izin verilenler-listesine eklenmelidir.

## <a name="onboard-to-the-public-preview"></a>Genel önizlemeye ekleme

Genel önizlemeye aşağıdaki şekilde katılın:

* Portalda, [Bu makaleyi izleyerek](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal)abonelik kimliğinizi Kurtarma Hizmetleri hizmet sağlayıcısına kaydedin.
* PowerShell için bu cmdlet 'i çalıştırın. "Kayıtlı" olarak tamamlanmalıdır.

    ```powershell
    Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```

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

### <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluşturma

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

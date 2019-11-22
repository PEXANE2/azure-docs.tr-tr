---
title: Öğretici-Azure VM 'lerinde SAP HANA veritabanlarını yedekleme
description: Bu öğreticide, Azure VM 'de çalışan SAP HANA veritabanlarını Azure Backup kurtarma hizmetleri kasasına nasıl yedekleyeceğinizi öğrenin.
ms.topic: tutorial
ms.date: 11/12/2019
ms.openlocfilehash: a622370fca3144aeb6a5d7c071c227b3c21cf135
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74288765"
---
# <a name="tutorial-back-up-sap-hana-databases-in-an-azure-vm"></a>Öğretici: Azure VM 'de SAP HANA veritabanlarını yedekleme

Bu öğreticide, Azure VM 'lerinde çalışan SAP HANA veritabanlarının Azure Backup kurtarma hizmetleri kasasına nasıl yedekleneceği gösterilmektedir. Bu makalede şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
>
> * Kasa oluşturma ve yapılandırma
> * Veritabanlarını bul
> * Yedeklemeleri yapılandırma

Şu anda desteklediğimiz tüm senaryolar [aşağıda](sap-hana-backup-support-matrix.md#scenario-support) verilmiştir.

## <a name="onboard-to-the-public-preview"></a>Genel önizlemeye ekleme

Genel önizlemeye aşağıdaki şekilde katılın:

* Portalda, [Bu makaleyi izleyerek](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal)abonelik kimliğinizi Kurtarma Hizmetleri hizmet sağlayıcısına kaydedin.

* PowerShell için bu cmdlet 'i çalıştırın. "Kayıtlı" olarak tamamlanmalıdır.

    ```powershell
    Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
    ```

## <a name="prerequisites"></a>Önkoşullar

Yedeklemeleri yapılandırmadan önce aşağıdakileri yaptığınızdan emin olun:

1. SAP HANA veritabanını çalıştıran VM 'de, aşağıdaki gibi, zypper kullanarak resmi SLES paketinden/medyasından ODBC sürücü paketlerini yükleyip etkinleştirin:

```bash
sudo zypper update
sudo zypper install unixODBC
```

>[!NOTE]
> Depoları güncelleştirmediğiniz için, unixODBC sürümünün en düşük 2.3.4 olduğundan emin olun. UniXODBC sürümünü bildirmek için `odbcinst -j` kök olarak çalıştırın
>

2. [Aşağıdaki yordamda](#set-up-network-connectivity)açıklandığı gibi, Azure 'a ULAŞABILMESI için VM 'den internet 'e bağlantıya izin verin.

3. HANA 'nın kök kullanıcı olarak yüklendiği sanal makinede ön kayıt betiğini çalıştırın. [Bu betik](https://aka.ms/scriptforpermsonhana) [doğru izinleri](#setting-up-permissions)ayarlayacaktır.

## <a name="set-up-network-connectivity"></a>Ağ bağlantısını ayarlama

Tüm işlemler için SAP HANA VM, Azure genel IP adresleri için bağlantı gerektirir. VM işlemleri (veritabanı bulma, yedeklemeleri yapılandırma, yedeklemeleri zamanlama, kurtarma noktalarını geri yükleme vb.) bağlantı olmadan çalışmaz. Azure veri merkezi IP aralıklarına erişime izin vererek bağlantı kurun:

* Azure veri merkezleri için [IP adresi aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653) indirebilir ve ardından bu IP adreslerine erişime izin verebilirsiniz.
* Ağ güvenlik grupları (NSG 'ler) kullanıyorsanız, tüm Azure genel IP adreslerine izin vermek için Azurecsesli [hizmet etiketini](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) kullanabilirsiniz. NSG kurallarını değiştirmek için [set-AzureNetworkSecurityRule cmdlet 'ini](https://docs.microsoft.com/powershell/module/servicemanagement/azure/set-azurenetworksecurityrule?view=azuresmps-4.0.0) kullanabilirsiniz.
* Aktarım, HTTPS üzerinden olduğundan, bağlantı noktası 443, izin verilenler-listesine eklenmelidir.

## <a name="setting-up-permissions"></a>İzinleri ayarlama

Ön kayıt betiği aşağıdaki eylemleri gerçekleştirir:

1. HANA sisteminde AZUREWLBACKUPHANAUSER oluşturur ve gerekli rolleri ve izinleri ekler:
   * VERITABANı YÖNETICISI: geri yükleme sırasında yeni DBs oluşturmak için.
   * Katalog okuma: yedekleme kataloğunu okumak için.
   * SAP_INTERNAL_HANA_SUPPORT: birkaç özel tabloya erişmek için.
2. Tüm işlemleri işlemek için (veritabanı sorguları, geri yükleme işlemleri, yedek yapılandırma ve çalıştırma), HANA eklentisinin Hdbuserstore öğesine bir anahtar ekler.

Anahtar oluşturmayı onaylamak için, SIDADDM kimlik bilgileriyle HANA makinesinde HDBSQL komutunu çalıştırın:

```hdbsql
hdbuserstore list
```

Komut çıktısı {SID} {DBNAME} anahtarını, AZUREWLBACKUPHANAUSER olarak gösterilen kullanıcıyla görüntülemelidir.

>[!NOTE]
> /Usr/sap/{SID}/home/.hdb/. altında farklı bir SSFS dosyaları kümesine sahip olduğunuzdan emin olun. Bu yolda yalnızca bir klasör olmalıdır.
>

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

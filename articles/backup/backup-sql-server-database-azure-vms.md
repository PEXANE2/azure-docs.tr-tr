---
title: Azure VM’lerinde SQL Server veritabanlarını yedekleme
description: Bu makalede, Azure sanal makinelerinde Azure Backup SQL Server veritabanlarını nasıl yedekleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 16e24ed94d8017d9fb922193bb16a33ec7a9cdfd
ms.sourcegitcommit: 1383842d1ea4044e1e90bd3ca8a7dc9f1b439a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84817542"
---
# <a name="back-up-sql-server-databases-in-azure-vms"></a>Azure VM’lerinde SQL Server veritabanlarını yedekleme

SQL Server veritabanları, düşük kurtarma noktası hedefi (RPO) ve uzun süreli saklama gerektiren kritik iş yükleridir. [Azure Backup](backup-overview.md)kullanarak Azure sanal makinelerinde (VM) çalışan SQL Server veritabanlarını yedekleyebilirsiniz.

Bu makalede, bir Azure VM üzerinde çalışan SQL Server bir veritabanını Azure Backup kurtarma hizmetleri kasasına nasıl yedekleyeceğiniz gösterilmektedir.

Bu makalede aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
>
> * Kasa oluşturun ve yapılandırın.
> * Veritabanları bulun ve yedeklemeleri ayarlayın.
> * Veritabanları için otomatik korumayı ayarlayın.

>[!NOTE]
>Azure **VM 'de SQL Server Için geçici silme ve Azure VM iş yükleri SAP HANA için geçici silme** , artık önizleme aşamasında kullanıma sunuldu.<br>
>Önizlemeye kaydolmak için, şurada bize yazın:AskAzureBackupTeam@microsoft.com

## <a name="prerequisites"></a>Ön koşullar

Bir SQL Server veritabanını yedeklemek için aşağıdaki ölçütleri kontrol edin:

1. SQL Server örneğini barındıran VM ile aynı bölgede ve abonelikte bir [Kurtarma Hizmetleri Kasası](backup-sql-server-database-azure-vms.md#create-a-recovery-services-vault) oluşturun veya oluşturun.
1. VM 'nin [ağ bağlantısı](backup-sql-server-database-azure-vms.md#establish-network-connectivity)olduğunu doğrulayın.
1. SQL Server veritabanlarının [Azure Backup için veritabanı adlandırma yönergelerini](#database-naming-guidelines-for-azure-backup)izlediğinden emin olun.
1. SQL Server VM adının ve kaynak grubu adının Birleşik uzunluğunun Azure Resource Manager (ARM) VM 'Ler için 84 karakteri (veya klasik VM 'Ler için 77 karakter) aşmadığından emin olun. Bu sınırlama, bazı karakterlerin hizmet tarafından ayrıldığı bir kısıtlamadır.
1. Veritabanı için etkinleştirilmiş başka bir yedekleme çözümünden emin olup olmadığınızı denetleyin. Veritabanını kapatmadan önce diğer tüm SQL Server yedeklemelerini devre dışı bırakın.

> [!NOTE]
> Azure VM için Azure Backup ve ayrıca çakışma olmadan VM üzerinde çalışan bir SQL Server veritabanı için etkinleştirebilirsiniz.

### <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma

Tüm işlemler için bir SQL Server VM Azure Backup hizmeti, Azure depolama ve Azure Active Directory bağlantı gerektirir. Bu, Özel uç noktalar kullanılarak veya gerekli genel IP adreslerine veya FQDN 'lere erişim izni vererek elde edilebilir. Gerekli Azure hizmetlerine doğru bağlantının yapılmasına izin verilmemesi, veritabanı bulma, yedeklemeyi yapılandırma, yedeklemeleri gerçekleştirme ve verileri geri yükleme gibi işlemlerde hata oluşmasına yol açabilir.

Aşağıdaki tabloda bağlantı kurmak için kullanabileceğiniz çeşitli alternatifler listelenmektedir:

| **Seçeneği**                        | **Üstünlü**                                               | **Dezavantajlar**                                            |
| --------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Özel uç noktalar                 | Sanal ağ içindeki özel IP 'Lerde yedeklemelere izin ver  <br><br>   Ağ ve kasa tarafında ayrıntılı denetim sağlama | Standart özel uç nokta [maliyetleri](https://azure.microsoft.com/pricing/details/private-link/) |
| NSG hizmet etiketleri                  | Aralık değişikliklerinin otomatik olarak birleştirilmesi için daha kolay yönetilmesi   <br><br>   Ek maliyet yok | Yalnızca NSG 'ler ile kullanılabilir  <br><br>    Hizmetin tamamına erişim sağlar |
| Azure Güvenlik Duvarı FQDN etiketleri          | Gerekli FQDN 'Ler otomatik olarak yönetildiğinden bu yana yönetilmesi daha kolay | Yalnızca Azure Güvenlik Duvarı ile kullanılabilir                         |
| Hizmet FQDN/IP 'lerine erişime izin ver | Ek maliyet yok   <br><br>  Tüm ağ güvenliği araçları ve güvenlik duvarları ile birlikte geçerlidir | Geniş bir IP veya FQDN kümesine erişilmesi gerekebilir   |
| HTTP proxy kullanma                 | VM 'lere tek bir internet erişimi noktası                       | Proxy yazılımıyla VM çalıştırmak için ek maliyetler         |

Bu seçenekleri kullanma hakkında daha fazla ayrıntı aşağıdaki şekilde paylaşılır:

#### <a name="private-endpoints"></a>Özel uç noktalar

Özel uç noktalar, bir sanal ağ içindeki sunuculardan kurtarma hizmetleri kasanıza güvenli bir şekilde bağlanmanıza olanak tanır. Özel uç nokta, kasanızın VNET adres alanından bir IP kullanır. Sanal ağın ve kasadaki kaynaklarınız arasındaki ağ trafiği, sanal ağınız üzerinden ve Microsoft omurga ağında özel bir bağlantı üzerinden dolaşır. Bu, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır. [Azure Backup için](https://docs.microsoft.com/azure/backup/private-endpoints)özel uç noktalar hakkında daha fazla bilgi edinin.

#### <a name="nsg-tags"></a>NSG etiketleri

Ağ güvenlik grupları (NSG) kullanıyorsanız, Azure Backup giden erişime izin vermek için *AzureBackup* Service etiketini kullanın. Azure Backup etiketine ek olarak, *Azure AD* ve *Azure depolama*için benzer [NSG kuralları](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) oluşturarak kimlik doğrulama ve veri aktarımı için de bağlantıya izin vermeniz gerekir.  Aşağıdaki adımlar Azure Backup etiketi için bir kural oluşturma işlemini anlatmaktadır:

1. **Tüm hizmetler**' de **ağ güvenlik grupları** ' na gidin ve ağ güvenlik grubunu seçin.

1. **Ayarlar**altında **giden güvenlik kuralları** ' nı seçin.

1. **Ekle**'yi seçin. [Güvenlik kuralı ayarları](https://docs.microsoft.com/azure/virtual-network/manage-network-security-group#security-rule-settings)' nda açıklandığı gibi yeni bir kural oluşturmak için gereken tüm ayrıntıları girin. Seçenek **hedefinin** *hizmet etiketi* olarak ayarlandığından ve **hedef hizmet etiketinin** *AzureBackup*olarak ayarlandığından emin olun.

1. Yeni oluşturulan giden güvenlik kuralını kaydetmek için **Ekle** ' ye tıklayın.

Benzer şekilde, Azure depolama ve Azure AD için NSG giden güvenlik kuralları oluşturabilirsiniz.

#### <a name="azure-firewall-tags"></a>Azure Güvenlik Duvarı etiketleri

Azure Güvenlik duvarı kullanıyorsanız, *AzureBackup* [Azure Güvenlik Duvarı FQDN etiketini](https://docs.microsoft.com/azure/firewall/fqdn-tags)kullanarak bir uygulama kuralı oluşturun. Bu, Azure Backup tüm giden erişimleri sağlar.

#### <a name="allow-access-to-service-ip-ranges"></a>Hizmet IP aralıklarına erişime izin ver

Erişim hizmeti IP 'lerine izin vermeyi seçerseniz, [burada](https://www.microsoft.com/download/confirmation.aspx?id=56519)KULLANILABILIR olan JSON dosyasındaki IP aralıklarına bakın. Azure Backup, Azure depolama ve Azure Active Directory karşılık gelen IP 'lere erişim izni vermeniz gerekir.

#### <a name="allow-access-to-service-fqdns"></a>Hizmet FQDN 'lere erişime izin ver

Sunucularınızdaki gerekli hizmetlere erişime izin vermek için aşağıdaki FQDN 'leri de kullanabilirsiniz:

| Hizmet    | Erişilecek etki alanı adları                             |
| -------------- | ------------------------------------------------------------ |
| Azure Backup  | `*.backup.windowsazure.com`                             |
| Azure depolama | `*.blob.core.windows.net` <br><br> `*.queue.core.windows.net` |
| Azure AD      | [Bu makaleye](https://docs.microsoft.com/office365/enterprise/urls-and-ip-address-ranges#microsoft-365-common-and-office-online) göre 56 ve 59 bölümlerinde FQDN 'lere erişime izin ver |

#### <a name="use-an-http-proxy-server-to-route-traffic"></a>Trafiği yönlendirmek için bir HTTP proxy sunucusu kullanma

Azure VM 'de bir SQL Server veritabanını yedeklerken, VM 'deki yedekleme uzantısı, Azure depolama 'ya Azure Backup ve verilere yönetim komutları göndermek için HTTPS API 'Lerini kullanır. Yedekleme uzantısı, kimlik doğrulaması için Azure AD 'yi de kullanır. Bu üç hizmetin yedekleme uzantısı trafiğini HTTP proxy üzerinden yönlendirin. Gerekli hizmetlere erişim izni vermek için yukarıda bahsedilen IP 'Ler ve FQDN 'lerin listesini kullanın. Kimliği doğrulanmış proxy sunucuları desteklenmez.

### <a name="database-naming-guidelines-for-azure-backup"></a>Azure Backup için veritabanı adlandırma yönergeleri

Veritabanı adlarında aşağıdaki öğeleri kullanmaktan kaçının:

* Sondaki ve Öndeki boşluklar
* Sondaki ünlem işaretleri (!)
* Kapanış köşeli parantezleri (])
* Noktalı virgül '; '
* Eğik çizgi '/'

Diğer ad, desteklenmeyen karakterler için kullanılabilir, ancak bunlardan kaçınmasını öneririz. Daha fazla bilgi için bkz. [Tablo Hizmeti Veri Modelini anlama](https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model).

>[!NOTE]
>Adında "+" veya "&" gibi özel karakterler içeren veritabanları için **koruma yapılandırma** işlemi desteklenmez. Veritabanı adını değiştirebilir veya **otomatik korumayı**etkinleştirerek bu veritabanlarını başarıyla koruyabilirler.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-sql-server-databases"></a>SQL Server veritabanlarını bulma

VM üzerinde çalışan veritabanlarını bulma:

1. [Azure Portal](https://portal.azure.com), veritabanını yedeklemek Için kullandığınız kurtarma hizmetleri kasasını açın.

2. **Kurtarma Hizmetleri Kasası** panosunda **yedekleme**' yi seçin.

   ![Yedekleme hedefi menüsünü açmak için yedekleme 'yi seçin](./media/backup-azure-sql-database/open-backup-menu.png)

3. **Yedekleme hedefi**' nde, **iş yükünüzün çalıştığı yeri** **Azure**'da belirleyin.

4. **Neleri yedeklemek**istiyorsunuz ' de **Azure VM 'de SQL Server**' yi seçin.

    ![Yedekleme için Azure VM 'de SQL Server seçin](./media/backup-azure-sql-database/choose-sql-database-backup-goal.png)

5. **Yedekleme hedefi**' nde  >  ,**VM 'lerde veritabanlarını bulur**, abonelikteki korumasız VM 'leri aramak için **bulmayı Başlat** ' ı seçin. Bu arama, abonelikteki korumasız VM sayısına bağlı olarak biraz zaman alabilir.

   * Korumasız VM 'Ler, ad ve kaynak grubuna göre listelenmiş bulma sonrasında listede görünmelidir.
   * Bir VM beklendiği gibi listelenmiyorsa, bir kasada zaten yedeklenmiş olup olmadığına bakın.
   * Birden çok VM aynı ada sahip olabilir, ancak bunlar farklı kaynak gruplarına ait olacaktır.

     ![VM 'lerde veritabanları için arama sırasında yedekleme bekliyor](./media/backup-azure-sql-database/discovering-sql-databases.png)

6. VM listesinde, SQL Server veritabanını çalıştıran VM 'yi seçin > **DB 'Leri bulun**.

7. **Bildirimlerde**veritabanı bulmayı izleyin. Bu eylem için gereken süre, VM veritabanlarının sayısına bağlıdır. Seçilen veritabanları bulunduğunda, bir başarı iletisi görüntülenir.

    ![Dağıtım başarılı iletisi](./media/backup-azure-sql-database/notifications-db-discovered.png)

8. Azure Backup VM 'deki tüm SQL Server veritabanlarını bulur. Bulma sırasında, arka planda aşağıdaki öğeler oluşur:

    * Azure Backup, VM 'yi iş yükü yedeklemesi için kasaya kaydeder. Kayıtlı VM 'deki tüm veritabanları yalnızca bu kasaya yedeklenebilir.
    * Azure Backup, AzureBackupWindowsWorkload uzantısını VM 'ye yüklenir. SQL veritabanında hiçbir aracı yüklü değil.
    * Azure Backup VM üzerinde NT Service\AzureWLBackupPluginSvc hizmet hesabını oluşturur.
      * Tüm yedekleme ve geri yükleme işlemleri hizmet hesabını kullanır.
      * NT Service\AzureWLBackupPluginSvc, SQL sysadmin izinleri gerektirir. Market 'te oluşturulan tüm SQL Server VM 'Ler, Sqliaasextenma yüklenmiş olarak gelir. AzureBackupWindowsWorkload uzantısı, gerekli izinleri otomatik olarak almak için Sqliaasexten kullanır.
    * VM 'yi Market 'ten oluşturmadıysanız veya SQL 2008 ve 2008 R2 kullanıyorsanız, sanal makinede Sqliaasextenınulma yüklü olmayabilir ve bulma işlemi UserErrorSQLNoSysAdminMembership hata iletisiyle başarısız olur. Bu sorunu onarmak için [VM Izinleri ayarla](backup-azure-sql-database.md#set-vm-permissions)bölümündeki yönergeleri izleyin.

        ![VM ve veritabanını seçin](./media/backup-azure-sql-database/registration-errors.png)

## <a name="configure-backup"></a>Yedeklemeyi yapılandırma  

1. **Yedekleme hedefi**  >  **Adım 2: yedeklemeyi yapılandırma**' yı seçin, yedeklemeyi **Yapılandır**' ı seçin.

   ![Yedeklemeyi Yapılandır ' ı seçin](./media/backup-azure-sql-database/backup-goal-configure-backup.png)

2. **Yedeklenecek öğeleri seçin**' de, tüm kayıtlı kullanılabilirlik gruplarını ve tek başına SQL Server örneklerini görürsünüz. Söz konusu örnekteki veya her zaman açık kullanılabilirlik grubundaki tüm korumasız veritabanlarının listesini genişletmek için satırın solundaki oku seçin.  

    ![Tek başına veritabanları ile tüm SQL Server örneklerini görüntüleme](./media/backup-azure-sql-database/list-of-sql-databases.png)

3. Korumak istediğiniz tüm veritabanları ' nı seçin ve ardından **Tamam**' ı seçin.

   ![Veritabanını koruma](./media/backup-azure-sql-database/select-database-to-protect.png)

   Yedekleme yüklerini iyileştirmek için Azure Backup bir yedekleme işinde en fazla veritabanı sayısını 50 olarak ayarlar.

     * 50 'den fazla veritabanını korumak için birden çok yedekleme yapılandırın.
     * Tüm örneği veya Always on kullanılabilirlik grubunu [etkinleştirmek](#enable-auto-protection) Için, **oto koru** açılan listesinde **Açık**' ı seçin ve ardından **Tamam**' ı seçin.

    > [!NOTE]
    > [Otomatik koruma](#enable-auto-protection) özelliği, aynı anda var olan tüm veritabanlarında korumayı yalnızca bir kerede etkinleştirmez, ancak aynı zamanda bu örneğe veya kullanılabilirlik grubuna eklenen yeni veritabanlarını otomatik olarak korur.  

4. **Yedekleme ilkesini**açmak için **Tamam ' ı** seçin.

    ![Always on kullanılabilirlik grubu için otomatik korumayı etkinleştir](./media/backup-azure-sql-database/enable-auto-protection.png)

5. **Yedekleme ilkesi**' nde bir ilke seçin ve ardından **Tamam**' ı seçin.

   * Varsayılan ilkeyi HourlyLogBackup olarak seçin.
   * Daha önce SQL için oluşturulmuş mevcut bir yedekleme ilkesi seçin.
   * RPO ve bekletme aralığınızı temel alarak yeni bir ilke tanımlayın.

     ![Yedekleme ilkesi seçin](./media/backup-azure-sql-database/select-backup-policy.png)

6. **Yedekleme**'de **yedeklemeyi etkinleştir**' i seçin.

    ![Seçilen yedekleme ilkesini etkinleştir](./media/backup-azure-sql-database/enable-backup-button.png)

7. Portalın **Bildirimler** alanındaki yapılandırma ilerlemesini izleyin.

    ![Bildirim alanı](./media/backup-azure-sql-database/notifications-area.png)

### <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluşturma

Yedekleme ilkesi, yedeklemelerin ne zaman alındığını ve ne kadar süreyle korunduğunu tanımlar.

* Kasa düzeyinde bir ilke oluşturulur.
* Birden çok kasa aynı yedekleme ilkesini kullanabilir, ancak yedekleme ilkesini her kasaya uygulamanız gerekir.
* Bir yedekleme ilkesi oluşturduğunuzda, varsayılan olarak günlük tam yedekleme varsayılandır.
* Değişiklik yedeklemesi ekleyebilirsiniz, ancak yalnızca tam yedeklemeleri haftalık olarak gerçekleşecek şekilde yapılandırabilirsiniz.
* [Farklı yedekleme ilkesi türleri](backup-architecture.md#sql-server-backup-types)hakkında bilgi edinin.

Bir yedekleme ilkesi oluşturmak için:

1. Kasada, **yedekleme ilkeleri**  >  **Ekle**' yi seçin.
2. **Ekle**' de, ilke türünü tanımlamak IÇIN **Azure VM 'de SQL Server** ' yi seçin.

   ![Yeni yedekleme ilkesi için bir ilke türü seçin](./media/backup-azure-sql-database/policy-type-details.png)

3. **İlke adı**alanına yeni ilke için bir ad girin.
4. **Tam yedekleme ilkesinde**, bir **yedekleme sıklığı**seçin. **Günlük** veya **haftalık**seçeneklerinden birini belirleyin.

   * **Günlük**için, yedekleme işinin başladığı saat ve saat dilimini seçin.
   * **Haftalık**olarak, yedekleme işinin başladığı hafta, saat ve saat diliminin gününü seçin.
   * **Tam yedekleme seçeneğini** kapatamadığı için tam yedekleme çalıştırın.
   * İlkeyi görüntülemek için **tam yedekleme** ' yi seçin.
   * Günlük tam yedeklemeler için fark yedeklemeleri oluşturamazsınız.

     ![Yeni yedekleme ilkesi alanları](./media/backup-azure-sql-database/full-backup-policy.png)  

5. **Saklama aralığı**' nda, tüm seçenekler varsayılan olarak seçilidir. İstemediğiniz tüm Bekletme aralığı sınırlarını temizleyin ve ardından kullanılacak aralıkları ayarlayın.

    * Herhangi bir yedekleme türü (tam, değişiklik ve günlük) için en düşük saklama süresi yedi gündür.
    * Kurtarma noktaları, bekletme aralığına göre bekletme için etiketlenir. Örneğin, günlük tam yedekleme seçerseniz, her gün yalnızca bir tam yedekleme tetiklenir.
    * Belirli bir günün yedeklemesi, haftalık bekletme aralığına ve haftalık bekletme ayarına göre etiketlenebilir ve korunur.
    * Aylık ve yıllık bekletme aralıkları benzer bir şekilde davranır.

       ![Bekletme aralığı aralığı ayarları](./media/backup-azure-sql-database/retention-range-interval.png)

6. **Tam yedekleme ilkesi** menüsünde, ayarları kabul etmek için **Tamam** ' ı seçin.
7. Bir değişiklik yedekleme ilkesi eklemek için, **fark yedeklemesi**' ni seçin.

   ![Bekletme aralığı aralığı ayarları ](./media/backup-azure-sql-database/retention-range-interval.png)
    ![ , fark yedekleme ilkesi menüsünü açın](./media/backup-azure-sql-database/backup-policy-menu-choices.png)

8. **Değişiklik yedekleme ilkesinde**, sıklık ve bekletme denetimlerini açmak için **Etkinleştir** ' i seçin.

    * Günde yalnızca bir değişiklik yedeklemesini tetikleyebilirsiniz.
    * Değişiklik yedeklemeleri, en fazla 180 gün boyunca korunabilir. Daha uzun bekletme için tam yedeklemeler kullanın.

9. İlkeyi kaydetmek ve ana **yedekleme ilkesi** menüsüne dönmek için **Tamam ' ı** seçin.

10. İşlem günlüğü yedekleme ilkesi eklemek için **günlük yedeklemesi**' ni seçin.
11. **Günlük yedeklemesi**bölümünde **Etkinleştir**' i seçin ve ardından sıklık ve bekletme denetimlerini ayarlayın. Günlük yedeklemeleri her 15 dakikada bir gerçekleşebilir ve 35 güne kadar tutulabilir.
12. İlkeyi kaydetmek ve ana **yedekleme ilkesi** menüsüne dönmek için **Tamam ' ı** seçin.

    ![Günlük yedekleme ilkesini düzenleme](./media/backup-azure-sql-database/log-backup-policy-editor.png)

13. **Yedekleme ilkesi** menüsünde, **SQL Backup sıkıştırmasını** etkinleştirilip etkinleştirilmeyeceğini seçin. Bu seçenek varsayılan olarak devre dışıdır. Etkinleştirilirse, SQL Server VDı 'a sıkıştırılmış bir yedekleme akışı gönderir.  Azure Backup bu denetimin değerine bağlı olarak, örnek düzeyi varsayılan değerleri COMPRESSION/NO_COMPRESSION yan tümcesiyle geçersiz kıldığını unutmayın.

14. Yedekleme ilkesinde yapılan düzenlemeleri tamamladıktan sonra **Tamam**' ı seçin.

> [!NOTE]
> Her günlük yedeklemesi, bir kurtarma zinciri oluşturmak için önceki tam yedeklemeye zincirlenir. Bu tam yedekleme, son günlük yedeklemenin saklama süresi sona erene kadar bekletilecektir. Bu, tüm günlüklerin kurtarılabilmesini sağlamak için ek bir süre boyunca tam yedeklemenin korunduğu anlamına gelebilir. Kullanıcının haftalık tam yedekleme, günlük fark ve 2 saatlik günlük olduğunu varsayalım. Bunların hepsi 30 gün boyunca tutulur. Ancak, haftalık tam yedekleme yalnızca bir sonraki tam yedekleme kullanılabilir olduktan sonra temizlenir/silinebilir (30 + 7 gün sonra). , Haftalık tam yedekleme 16 Kasım 'da gerçekleşir. Bekletme ilkesine göre, 16 Aralık 'a kadar saklanması gerekir. Bu tam için en son günlük yedeklemesi, 22nd tarihinde bir sonraki zamanlanmış tam önce gerçekleşir. Bu günlük, 22nd 'e kadar kullanılabilir olana kadar, 16. ve tam 6. Bu nedenle, 16. dolu, 22nd 'e kadar korunur.

## <a name="enable-auto-protection"></a>Otomatik korumayı etkinleştir  

Otomatik korumayı, tüm mevcut ve gelecekteki veritabanlarını tek başına bir SQL Server örneğine veya her zaman açık kullanılabilirlik grubuna otomatik olarak yedekleyecek şekilde etkinleştirebilirsiniz.

* Tek seferde otomatik koruma için seçebileceğiniz veritabanı sayısı için bir sınır yoktur. Bulma genellikle her sekiz saatte bir çalışır. Bununla birlikte, bulmayı **yeniden keşfet** seçeneğini belirleyerek el ile bir keşif çalıştırırsanız yeni veritabanlarını hemen bulabilir ve koruyabilirsiniz.
* Otomatik korumayı etkinleştirdiğiniz sırada bir örnekteki veritabanlarını seçmeli olarak koruyamazsınız veya dışlayabilirsiniz.
* Örneğiniz zaten bazı korumalı veritabanları içeriyorsa, otomatik korumayı etkinleştirdikten sonra bile kendi ilkeleri altında korunmaları gerekir. Daha sonra eklenen tüm korumasız veritabanları, otomatik korumayı etkinleştirme sırasında, **yedeklemeyi Yapılandır**altında listelenen tek bir ilkeye sahip olacaktır. Ancak, bir otomatik korumalı veritabanıyla ilişkili ilkeyi daha sonra değiştirebilirsiniz.  

Otomatik korumayı etkinleştirmek için:

  1. **Yedeklenecek öğeler**' de, otomatik korumayı etkinleştirmek istediğiniz örneği seçin.
  2. **Oto koru**altında aşağı açılan listeyi seçin, **Açık**' ı seçin ve ardından **Tamam**' ı seçin.

      ![Kullanılabilirlik grubunda otomatik korumayı etkinleştir](./media/backup-azure-sql-database/enable-auto-protection.png)

  3. Yedekleme tüm veritabanları için bir arada yapılandırılır ve **yedekleme işlerinde**izlenebilir.

Otomatik korumayı devre dışı bırakmanız gerekirse, **yedeklemeyi Yapılandır**altında örnek adını seçin ve ardından örnek Için **otomatik korumayı devre dışı bırak** ' ı seçin. Tüm veritabanları yedeklenmeye devam eder, ancak gelecekteki veritabanları otomatik olarak korunmaz.

![Bu örnekte otomatik korumayı devre dışı bırak](./media/backup-azure-sql-database/disable-auto-protection.png)

## <a name="next-steps"></a>Sonraki adımlar

Şunları nasıl yapacağınızı öğrenin:

* [Yedeklenen SQL Server veritabanlarını geri yükleme](restore-sql-database-azure-vm.md)
* [Yedeklenen SQL Server veritabanlarını yönetme](manage-monitor-sql-database-backup.md)

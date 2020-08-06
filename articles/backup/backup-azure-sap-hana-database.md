---
title: Azure Backup ile Azure 'da bir SAP HANA veritabanını yedekleme
description: Bu makalede, Azure Backup hizmeti ile SAP HANA bir veritabanını Azure sanal makinelerine nasıl yedekleyeceğinizi öğrenin.
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: 0efd26272bbfc4c8f63f7f02a605d48e53577390
ms.sourcegitcommit: 85eb6e79599a78573db2082fe6f3beee497ad316
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87809145"
---
# <a name="back-up-sap-hana-databases-in-azure-vms"></a>Azure VM’lerindeki SAP HANA veritabanlarını yedekleme

SAP HANA veritabanları, düşük kurtarma noktası hedefi (RPO) ve uzun süreli saklama gerektiren kritik iş yükleridir. [Azure Backup](backup-overview.md)kullanarak Azure sanal makinelerinde (VM) çalışan SAP HANA veritabanlarını yedekleyebilirsiniz.

Bu makalede, Azure sanal makinelerinde çalışan SAP HANA veritabanlarının Azure Backup kurtarma hizmetleri kasasına nasıl yedekleneceği gösterilmektedir.

Bu makalede aşağıdakileri nasıl yapacağınızı öğreneceksiniz:
> [!div class="checklist"]
>
> * Kasa oluşturma ve yapılandırma
> * Veritabanlarını bul
> * Yedeklemeleri yapılandırma
> * İsteğe bağlı yedekleme işi çalıştırma

>[!NOTE]
>1 Ağustos 2020 itibariyle, RHEL için SAP HANA yedekleme (7,4, 7,6, 7,7 & 8,1) genel kullanıma sunulmuştur.

>[!NOTE]
>Azure **VM 'de SQL Server Için geçici silme ve Azure VM iş yükleri SAP HANA için geçici silme** , artık önizleme aşamasında kullanıma sunuldu.<br>
>Önizlemeye kaydolmak için, adresinden bize yazın [AskAzureBackupTeam@microsoft.com](mailto:AskAzureBackupTeam@microsoft.com) .

## <a name="prerequisites"></a>Önkoşullar

[Önkoşulları](tutorial-backup-sap-hana-db.md#prerequisites) ve [ön kayıt betiği](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) , yedekleme için veritabanını ayarlama bölümlerine bakın.

### <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma

Tüm işlemler için, bir Azure VM üzerinde çalışan bir SAP HANA veritabanı, Azure Backup hizmeti, Azure depolama ve Azure Active Directory bağlantı gerektirir. Bu, Özel uç noktalar kullanılarak veya gerekli genel IP adreslerine veya FQDN 'lere erişim izni vererek elde edilebilir. Gerekli Azure hizmetlerine doğru bağlantının yapılmasına izin verilmemesi, veritabanı bulma, yedeklemeyi yapılandırma, yedeklemeleri gerçekleştirme ve verileri geri yükleme gibi işlemlerde hata oluşmasına yol açabilir.

Aşağıdaki tabloda bağlantı kurmak için kullanabileceğiniz çeşitli alternatifler listelenmektedir:

| **Seçenek**                        | **Avantajlar**                                               | **Dezavantajlar**                                            |
| --------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Özel uç noktalar                 | Sanal ağ içindeki özel IP 'Lerde yedeklemelere izin ver  <br><br>   Ağ ve kasa tarafında ayrıntılı denetim sağlama | Standart özel uç nokta [maliyetleri](https://azure.microsoft.com/pricing/details/private-link/) |
| NSG hizmet etiketleri                  | Aralık değişikliklerinin otomatik olarak birleştirilmesi için daha kolay yönetilmesi   <br><br>   Ek maliyet yok | Yalnızca NSG 'ler ile kullanılabilir  <br><br>    Hizmetin tamamına erişim sağlar |
| Azure Güvenlik Duvarı FQDN etiketleri          | Gerekli FQDN 'Ler otomatik olarak yönetildiğinden bu yana yönetilmesi daha kolay | Yalnızca Azure Güvenlik Duvarı ile kullanılabilir                         |
| Hizmet FQDN/IP 'lerine erişime izin ver | Ek maliyet yok   <br><br>  Tüm ağ güvenliği araçları ve güvenlik duvarları ile birlikte geçerlidir | Geniş bir IP veya FQDN kümesine erişilmesi gerekebilir   |
| HTTP proxy kullanma                 | VM 'lere tek bir internet erişimi noktası                       | Proxy yazılımıyla VM çalıştırmak için ek maliyetler         |

Bu seçenekleri kullanma hakkında daha fazla ayrıntı aşağıdaki şekilde paylaşılır:

#### <a name="private-endpoints"></a>Özel uç noktalar

Özel uç noktalar, bir sanal ağ içindeki sunuculardan kurtarma hizmetleri kasanıza güvenli bir şekilde bağlanmanıza olanak tanır. Özel uç nokta, kasanızın VNET adres alanından bir IP kullanır. Sanal ağın ve kasadaki kaynaklarınız arasındaki ağ trafiği, sanal ağınız üzerinden ve Microsoft omurga ağında özel bir bağlantı üzerinden dolaşır. Bu, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır. [Azure Backup için](./private-endpoints.md)özel uç noktalar hakkında daha fazla bilgi edinin.

#### <a name="nsg-tags"></a>NSG etiketleri

Ağ güvenlik grupları (NSG) kullanıyorsanız, Azure Backup giden erişime izin vermek için *AzureBackup* Service etiketini kullanın. Azure Backup etiketine ek olarak, *Azure AD* ve *Azure depolama*için benzer [NSG kuralları](../virtual-network/security-overview.md#service-tags) oluşturarak kimlik doğrulama ve veri aktarımı için de bağlantıya izin vermeniz gerekir.  Aşağıdaki adımlar Azure Backup etiketi için bir kural oluşturma işlemini anlatmaktadır:

1. **Tüm hizmetler**' de **ağ güvenlik grupları** ' na gidin ve ağ güvenlik grubunu seçin.

1. **Ayarlar**altında **giden güvenlik kuralları** ' nı seçin.

1. **Add (Ekle)** seçeneğini belirleyin. [Güvenlik kuralı ayarları](../virtual-network/manage-network-security-group.md#security-rule-settings)' nda açıklandığı gibi yeni bir kural oluşturmak için gereken tüm ayrıntıları girin. Seçenek **hedefinin** *hizmet etiketi* olarak ayarlandığından ve **hedef hizmet etiketinin** *AzureBackup*olarak ayarlandığından emin olun.

1. Yeni oluşturulan giden güvenlik kuralını kaydetmek için **Ekle** ' ye tıklayın.

Benzer şekilde, Azure depolama ve Azure AD için NSG giden güvenlik kuralları oluşturabilirsiniz. Hizmet etiketleri hakkında daha fazla bilgi için [Bu makaleye](../virtual-network/service-tags-overview.md)bakın.

#### <a name="azure-firewall-tags"></a>Azure Güvenlik Duvarı etiketleri

Azure Güvenlik duvarı kullanıyorsanız, *AzureBackup* [Azure Güvenlik Duvarı FQDN etiketini](../firewall/fqdn-tags.md)kullanarak bir uygulama kuralı oluşturun. Bu, Azure Backup tüm giden erişimleri sağlar.

#### <a name="allow-access-to-service-ip-ranges"></a>Hizmet IP aralıklarına erişime izin ver

Erişim hizmeti IP 'lerine izin vermeyi seçerseniz, [burada](https://www.microsoft.com/download/confirmation.aspx?id=56519)KULLANILABILIR olan JSON dosyasındaki IP aralıklarına bakın. Azure Backup, Azure depolama ve Azure Active Directory karşılık gelen IP 'lere erişim izni vermeniz gerekir.

#### <a name="allow-access-to-service-fqdns"></a>Hizmet FQDN 'lere erişime izin ver

Sunucularınızdaki gerekli hizmetlere erişime izin vermek için aşağıdaki FQDN 'leri de kullanabilirsiniz:

| Hizmet    | Erişilecek etki alanı adları                             |
| -------------- | ------------------------------------------------------------ |
| Azure Backup  | `*.backup.windowsazure.com`                             |
| Azure depolama | `*.blob.core.windows.net` <br><br> `*.queue.core.windows.net` |
| Azure AD      | [Bu makaleye](/office365/enterprise/urls-and-ip-address-ranges#microsoft-365-common-and-office-online) göre 56 ve 59 bölümlerinde FQDN 'lere erişime izin ver |

#### <a name="use-an-http-proxy-server-to-route-traffic"></a>Trafiği yönlendirmek için bir HTTP proxy sunucusu kullanma

Azure VM üzerinde çalışan bir SAP HANA veritabanını yedeklerken, VM 'deki yedekleme uzantısı, Azure depolama 'ya Azure Backup ve verilere yönetim komutları göndermek için HTTPS API 'Lerini kullanır. Yedekleme uzantısı, kimlik doğrulaması için Azure AD 'yi de kullanır. Bu üç hizmetin yedekleme uzantısı trafiğini HTTP proxy üzerinden yönlendirin. Gerekli hizmetlere erişim izni vermek için yukarıda bahsedilen IP 'Ler ve FQDN 'lerin listesini kullanın. Kimliği doğrulanmış proxy sunucuları desteklenmez.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="discover-the-databases"></a>Veritabanlarını bulma

1. Kasadaki **Başlarken**' de, **Yedekle**' ye tıklayın. **İş yükünüz nerede çalışıyor?**, **Azure VM 'de SAP HANA**' yi seçin.
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

1. 2. adımda **yedeklemeyi Yapılandır**' a tıklayın.

    ![Yedeklemeyi Yapılandır](./media/backup-azure-sap-hana-database/configure-backup.png)
2. **Yedeklenecek öğeleri seçin**' de, korumak istediğiniz tüm veritabanlarını seçin > **Tamam**' a tıklayın.

    ![Yedeklenecek öğeleri seçin](./media/backup-azure-sap-hana-database/select-items.png)
3. **Yedekleme**İlkesi ' nde  >  **yedekleme ilkesi**' ni seçin ve aşağıdaki yönergelere uygun olarak veritabanları için yeni bir yedekleme ilkesi oluşturun.

    ![Yedekleme ilkesi seçin](./media/backup-azure-sap-hana-database/backup-policy.png)
4. İlkeyi oluşturduktan sonra **yedekleme** menüsünde **yedeklemeyi etkinleştir**' e tıklayın.

    ![Yedeklemeyi etkinleştir](./media/backup-azure-sap-hana-database/enable-backup.png)
5. Yedekleme Yapılandırma ilerlemesini portalın **Bildirimler** alanında izleyin.

### <a name="create-a-backup-policy"></a>Yedekleme ilkesi oluşturma

Yedekleme ilkesi, yedeklemelerin ne zaman alındığını ve ne kadar süreyle korunduğunu tanımlar.

* Kasa düzeyinde bir ilke oluşturulur.
* Birden çok kasa aynı yedekleme ilkesini kullanabilir, ancak yedekleme ilkesini her kasaya uygulamanız gerekir.

>[!NOTE]
>Azure Backup, bir Azure VM 'de çalışan bir SAP HANA Veritabanının yedeklenmesinde gün ışığından yararlanma saati değişikliklerini otomatik olarak ayarlamaz.
>
>İlkeyi gerektiği şekilde el ile değiştirin.

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
    * **Günlük yedeklemesi**' nde **Etkinleştir**' i seçin.  Tüm günlük yedeklemelerini SAP HANA yönettiğinden, bu devre dışı bırakılamaz.
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
3. **Şimdi Yedekle**' de, gerçekleştirmek istediğiniz yedekleme türünü seçin. Daha sonra, **Tamam**'a tıklayın. Bu yedekleme, bu yedekleme öğesiyle ilişkili ilkeye göre saklanacaktır.
4. Portal bildirimlerini izleyin. İş ilerlemesini kasa panosunda izleyebilirsiniz > **yedekleme işleri**  >  **devam**ediyor. Veritabanınızın boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.

## <a name="run-sap-hana-studio-backup-on-a-database-with-azure-backup-enabled"></a>Azure Backup etkin bir veritabanında SAP HANA Studio yedeklemesini çalıştırma

Azure Backup ile yedeklenen bir veritabanının yerel yedeklemesini (HANA Studio 'Yu kullanarak) almak istiyorsanız aşağıdakileri yapın:

1. Veritabanının tamamlaması için herhangi bir tam veya günlük yedeklemesi bekleyin. SAP HANA Studio/Kokpit içindeki durumu denetleyin.
1. Günlük yedeklemelerini devre dışı bırakın ve ilgili veritabanı için yedekleme kataloğunu dosya sistemine ayarlayın.
1. Bunu yapmak için **SystemDB**  >  **yapılandırması**  >  **veritabanı**  >  **filtresi Seç (günlük)** öğesine çift tıklayın.
1. **Enable_auto_log_backup** **Hayır**olarak ayarlayın.
1. **Log_backup_using_backint** **false**olarak ayarlayın.
1. **Catalog_backup_using_backint** **false**olarak ayarlayın.
1. Veritabanının isteğe bağlı tam yedeklemesini yapın.
1. Tam yedekleme ve Katalog yedeklemesinin bitmesini bekleyin.
1. Önceki ayarları Azure için geri döndürür:
    * **Enable_auto_log_backup** **Evet**olarak ayarlayın.
    * **Log_backup_using_backint** **true**olarak ayarlayın.
    * **Catalog_backup_using_backint** **true**olarak ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure VM 'lerinde çalışan SAP HANA veritabanlarını geri yüklemeyi](./sap-hana-db-restore.md) öğrenin
* [Kullanarak yedeklenen SAP HANA veritabanlarını yönetmeyi öğrenin Azure Backup](./sap-hana-db-manage.md)
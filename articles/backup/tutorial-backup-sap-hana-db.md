---
title: Öğretici-Azure VM 'lerinde SAP HANA veritabanlarını yedekleme
description: Bu öğreticide, Azure VM 'de çalışan SAP HANA veritabanlarını Azure Backup kurtarma hizmetleri kasasına nasıl yedekleyeceğinizi öğrenin.
ms.topic: tutorial
ms.date: 02/24/2020
ms.openlocfilehash: f89d21a252870befae7807d2dda96828aaaa1326
ms.sourcegitcommit: 14bf4129a73de2b51a575c3a0a7a3b9c86387b2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87439659"
---
# <a name="tutorial-back-up-sap-hana-databases-in-an-azure-vm"></a>Öğretici: Azure VM 'de SAP HANA veritabanlarını yedekleme

Bu öğreticide, Azure VM 'lerinde çalışan SAP HANA veritabanlarının Azure Backup kurtarma hizmetleri kasasına nasıl yedekleneceği gösterilmektedir. Bu makalede şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
>
> * Kasa oluşturma ve yapılandırma
> * Veritabanlarını bul
> * Yedeklemeleri yapılandırma

Şu anda desteklediğimiz tüm senaryolar [aşağıda](sap-hana-backup-support-matrix.md#scenario-support) verilmiştir.

>[!NOTE]
>1 Ağustos 2020 itibariyle, RHEL için SAP HANA yedekleme (7,4, 7,6, 7,7 & 8,1) genel kullanıma sunulmuştur.

## <a name="prerequisites"></a>Ön koşullar

Yedeklemeleri yapılandırmadan önce aşağıdakileri yaptığınızdan emin olun:

* SAP HANA çalıştıran VM ile aynı bölgede ve abonelikte bir [Kurtarma Hizmetleri Kasası](backup-sql-server-database-azure-vms.md#create-a-recovery-services-vault) oluşturun veya oluşturun.
* Aşağıdaki [ağ bağlantısı kurma](#set-up-network-connectivity) yordamında açıklandığı gibi, Azure 'a ULAŞABILMESI için VM 'den internet 'e bağlantıya izin verin.
* SAP HANA sunucusu VM adı ve kaynak grubu adının toplam uzunluğunun Azure Resource Manager (ARM_ VM 'Ler için (ve klasik VM 'Ler için 77 karakter) 84 karakteri aşmadığından emin olun. Bu sınırlama, bazı karakterlerin hizmet tarafından ayrıldığı bir kısıtlamadır.
* **Hdbuserstore** içinde aşağıdaki ölçütlere uyan bir anahtar bulunmalıdır:
  * Varsayılan **hdbuserstore**içinde mevcut olmalıdır. Varsayılan değer `<sid>adm` SAP HANA yüklendiği hesaptır.
  * MDC için anahtar, **nameserver**SQL bağlantı noktasını göstermelidir. SDC söz konusu olduğunda, **dizin sunucusunun** SQL bağlantı noktasını işaret etmelidir
  * Kullanıcı eklemek ve silmek için kimlik bilgilerine sahip olmalıdır
* Kök kullanıcı olarak, HANA 'nın yüklü olduğu sanal makinede SAP HANA yedekleme yapılandırma betiğini (ön kayıt betiği) çalıştırın. [Bu betik](https://aka.ms/scriptforpermsonhana) , yedekleme IÇIN hazırlanma Hana sistemini alır. Ön kayıt betiği hakkında daha fazla bilgi edinmek için [ön kayıt betiğinin ne olduğunu](#what-the-pre-registration-script-does) öğrenin bölümüne bakın.

>[!NOTE]
>Preregistration betiği, RHEL (7,4, 7,6 ve 7,7) üzerinde çalışan SAP HANA iş yükleri için **COMPAT-unixODBC234** ve rhel 8,1 Için **unixODBC** 'yi yüklüyor. [Bu paket, SAP Solutions (RPMs) deposu için RHEL for SAP HANA (RHEL 7 Server) Güncelleştirme Hizmetleri ' nde bulunur](https://access.redhat.com/solutions/5094721).  Azure Marketi RHEL görüntüsü için deponun **rhuı-RHEL-SAP-HANA-for-RHEL-7-Server-rhuı-E4s-RPMS**olması gerekir.

## <a name="set-up-network-connectivity"></a>Ağ bağlantısını ayarlama

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

### <a name="private-endpoints"></a>Özel uç noktalar

Özel uç noktalar, bir sanal ağ içindeki sunuculardan kurtarma hizmetleri kasanıza güvenli bir şekilde bağlanmanıza olanak tanır. Özel uç nokta, kasanızın VNET adres alanından bir IP kullanır. Sanal ağın ve kasadaki kaynaklarınız arasındaki ağ trafiği, sanal ağınız üzerinden ve Microsoft omurga ağında özel bir bağlantı üzerinden dolaşır. Bu, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır. [Azure Backup için](./private-endpoints.md)özel uç noktalar hakkında daha fazla bilgi edinin.

### <a name="nsg-tags"></a>NSG etiketleri

Ağ güvenlik grupları (NSG) kullanıyorsanız, Azure Backup giden erişime izin vermek için *AzureBackup* Service etiketini kullanın. Azure Backup etiketine ek olarak, *Azure AD* ve *Azure depolama*için benzer [NSG kuralları](../virtual-network/security-overview.md#service-tags) oluşturarak kimlik doğrulama ve veri aktarımı için de bağlantıya izin vermeniz gerekir.  Aşağıdaki adımlar Azure Backup etiketi için bir kural oluşturma işlemini anlatmaktadır:

1. **Tüm hizmetler**' de **ağ güvenlik grupları** ' na gidin ve ağ güvenlik grubunu seçin.

1. **Ayarlar**altında **giden güvenlik kuralları** ' nı seçin.

1. **Ekle**'yi seçin. [Güvenlik kuralı ayarları](../virtual-network/manage-network-security-group.md#security-rule-settings)' nda açıklandığı gibi yeni bir kural oluşturmak için gereken tüm ayrıntıları girin. Seçenek **hedefinin** *hizmet etiketi* olarak ayarlandığından ve **hedef hizmet etiketinin** *AzureBackup*olarak ayarlandığından emin olun.

1. Yeni oluşturulan giden güvenlik kuralını kaydetmek için **Ekle** ' ye tıklayın.

Benzer şekilde, Azure depolama ve Azure AD için NSG giden güvenlik kuralları oluşturabilirsiniz. Hizmet etiketleri hakkında daha fazla bilgi için [Bu makaleye](https://docs.microsoft.com/azure/virtual-network/service-tags-overview)bakın.

### <a name="azure-firewall-tags"></a>Azure Güvenlik Duvarı etiketleri

Azure Güvenlik duvarı kullanıyorsanız, *AzureBackup* [Azure Güvenlik Duvarı FQDN etiketini](../firewall/fqdn-tags.md)kullanarak bir uygulama kuralı oluşturun. Bu, Azure Backup tüm giden erişimleri sağlar.

### <a name="allow-access-to-service-ip-ranges"></a>Hizmet IP aralıklarına erişime izin ver

Erişim hizmeti IP 'lerine izin vermeyi seçerseniz, [burada](https://www.microsoft.com/download/confirmation.aspx?id=56519)KULLANILABILIR olan JSON dosyasındaki IP aralıklarına bakın. Azure Backup, Azure depolama ve Azure Active Directory karşılık gelen IP 'lere erişim izni vermeniz gerekir.

### <a name="allow-access-to-service-fqdns"></a>Hizmet FQDN 'lere erişime izin ver

Sunucularınızdaki gerekli hizmetlere erişime izin vermek için aşağıdaki FQDN 'leri de kullanabilirsiniz:

| Hizmet    | Erişilecek etki alanı adları                             |
| -------------- | ------------------------------------------------------------ |
| Azure Backup  | `*.backup.windowsazure.com`                             |
| Azure depolama | `*.blob.core.windows.net` <br><br> `*.queue.core.windows.net` |
| Azure AD      | [Bu makaleye](/office365/enterprise/urls-and-ip-address-ranges#microsoft-365-common-and-office-online) göre 56 ve 59 bölümlerinde FQDN 'lere erişime izin ver |

### <a name="use-an-http-proxy-server-to-route-traffic"></a>Trafiği yönlendirmek için bir HTTP proxy sunucusu kullanma

Azure VM üzerinde çalışan bir SAP HANA veritabanını yedeklerken, VM 'deki yedekleme uzantısı, Azure depolama 'ya Azure Backup ve verilere yönetim komutları göndermek için HTTPS API 'Lerini kullanır. Yedekleme uzantısı, kimlik doğrulaması için Azure AD 'yi de kullanır. Bu üç hizmetin yedekleme uzantısı trafiğini HTTP proxy üzerinden yönlendirin. Gerekli hizmetlere erişim izni vermek için yukarıda bahsedilen IP 'Ler ve FQDN 'lerin listesini kullanın. Kimliği doğrulanmış proxy sunucuları desteklenmez.

## <a name="what-the-pre-registration-script-does"></a>Ön kayıt betiği ne yapar

Ön kayıt betiğini çalıştırmak aşağıdaki işlevleri gerçekleştirir:

* Komut dosyası, Linux dağılıcmanıza bağlı olarak, Azure Backup aracısının gerektirdiği gerekli paketleri de yüklüyor veya güncelleştirir.
* Azure Backup sunucularıyla giden ağ bağlantı denetimlerini ve Azure Active Directory ve Azure depolama gibi bağımlı hizmetleri gerçekleştirir.
* [Önkoşulların](#prerequisites)bir parçası olarak listelenen Kullanıcı anahtarını kullanarak Hana sisteminizde oturum açar. Kullanıcı anahtarı, HANA sisteminde bir yedek Kullanıcı (AZUREWLBACKUPHANAUSER) oluşturmak için kullanılır ve ön kayıt betiği başarıyla çalıştıktan sonra Kullanıcı anahtarı silinebilir.
* AZUREWLBACKUPHANAUSER bu gerekli roller ve izinler atanır:
  * VERITABANı YÖNETICISI (MDC söz konusu olduğunda) ve yedekleme YÖNETICISI (SDC durumunda): geri yükleme sırasında yeni veritabanları oluşturmak için.
  * Katalog okuma: yedekleme kataloğunu okumak için.
  * SAP_INTERNAL_HANA_SUPPORT: birkaç özel tabloya erişmek için.
* Betik, tüm işlemleri (veritabanı sorguları, geri yükleme işlemleri, yedek yapılandırma ve çalıştırma) işlemek üzere HANA yedekleme eklentisi için AZUREWLBACKUPHANAUSER için **hdbuserstore** öğesine bir anahtar ekler.

>[!NOTE]
> Ön kayıt betiğine, [önkoşulların](#prerequisites) bir parçası olarak listelenen Kullanıcı anahtarını açıkça geçirebilirsiniz:`-sk SYSTEM_KEY_NAME, --system-key SYSTEM_KEY_NAME` <br><br>
>Betiğin kabul ettiği diğer parametreleri öğrenmek için komutunu kullanın`bash msawb-plugin-config-com-sap-hana.sh --help`

Anahtar oluşturmayı onaylamak için, SIDADDM kimlik bilgileriyle HANA makinesinde HDBSQL komutunu çalıştırın:

```hdbsql
hdbuserstore list
```

Komut çıktısı {SID} {DBNAME} anahtarını, AZUREWLBACKUPHANAUSER olarak gösterilen kullanıcıyla görüntülemelidir.

>[!NOTE]
> Altında farklı bir SSFS dosyası kümesine sahip olduğunuzdan emin olun `/usr/sap/{SID}/home/.hdb/` . Bu yolda yalnızca bir klasör olmalıdır.

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
   Aboneliğinizdeki kullanılabilir kaynak gruplarının listesini görmek için **Varolanı kullan**' ı seçin ve ardından aşağı açılan liste kutusundan bir kaynak seçin. Yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' u seçin ve adı girin. Kaynak grupları hakkında tüm bilgiler için bkz. [Azure Resource Manager genel bakış](../azure-resource-manager/management/overview.md).
   * **Konum**: kasa için coğrafi bölgeyi seçin. Kasa, SAP HANA çalıştıran sanal makine ile aynı bölgede olmalıdır. **Doğu ABD 2**kullandık.

5. **Gözden geçir + oluştur**' u seçin.

   ![Gözden geçirme & oluştur ' u seçin](./media/tutorial-backup-sap-hana-db/review-create.png)

Kurtarma Hizmetleri Kasası artık oluşturulmuştur.

## <a name="discover-the-databases"></a>Veritabanlarını bulma

1. Kasadaki **Başlarken**' de, **Yedekle**' ye tıklayın. **İş yükünüz nerede çalışıyor?**, **Azure VM 'de SAP HANA**' yi seçin.
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

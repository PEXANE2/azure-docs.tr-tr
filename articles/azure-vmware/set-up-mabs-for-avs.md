---
title: Azure VMware çözümü için Azure Backup Sunucusu ayarlama
description: Azure Backup Sunucusu kullanarak sanal makineleri yedeklemek için Azure VMware Çözüm ortamınızı ayarlayın.
ms.topic: how-to
ms.date: 06/09/2020
ms.openlocfilehash: c56ebaff6b08f3d6586dfe025fdb2a5bfc708fa4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84816853"
---
# <a name="set-up-azure-backup-server-for-azure-vmware-solution"></a>Azure VMware çözümü için Azure Backup Sunucusu ayarlama

Azure Backup Sunucusu, iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunan sağlam bir kurumsal yedekleme ve kurtarma sistemidir. Azure VMware Çözüm önizlemesi sırasında, Azure Backup Sunucusu kullanarak yalnızca sanal makine (VM) düzeyinde yedekleme yapılandırabilirsiniz. 

Azure Backup Sunucusu, yedekleme verilerini şu şekilde saklayabilir:

- **Disk**: kısa vadeli depolama için, verileri disk havuzlarına Yedekler Azure Backup sunucusu.
- **Azure**: hem kısa vadeli hem de uzun vadeli depolama için şirket içi Azure Backup sunucusu, disk havuzlarında depolanan verileri Azure Backup kullanılarak Microsoft Azure buluta yedeklenebilir.

Kesintiler meydana geldiğinde ve kaynak veriler kullanılamadığında, verileri kaynağa veya alternatif bir konuma kolayca geri yüklemek için Azure Backup Sunucusu kullanabilirsiniz. Bu şekilde, planlanan veya beklenmeyen sorunlar nedeniyle özgün veriler kullanılamıyorsa, verileri farklı bir konuma kolayca geri yükleyebilirsiniz.

Bu makalede, Azure Backup Sunucusu kullanarak VM 'Leri yedeklemek için Azure VMware Çözüm ortamınızı hazırlamanıza yardımcı olduğumuz. Şu adımlarda size kılavuzluk ederiz: 

> [!div class="checklist"]
> * Önerilen VM disk türünü ve kullanılacak boyutu belirleme.
> * Kurtarma noktalarını depolayan bir kurtarma hizmetleri Kasası oluşturun.
> * Kurtarma Hizmetleri Kasası için depolama çoğaltmasını ayarlayın.
> * Azure Backup Sunucusu depolama ekleyin.

## <a name="supported-vmware-features"></a>Desteklenen VMware özellikleri

- **Aracısız yedekleme:** Azure Backup Sunucusu, sanal makineyi yedeklemek için vCenter veya ESXi sunucusuna bir aracının yüklenmesini gerektirmez. Bunun yerine, Azure Backup Sunucusu ile VMware sunucusunun kimliğini doğrulamak için kullanılan IP adresini veya tam etki alanı adını (FQDN) ve oturum açma kimlik bilgilerini sağlamanız yeterlidir.
- **Bulutta tümleşik yedekleme:** Azure Backup Sunucusu, iş yüklerini disk ve buluta korur. Azure Backup Sunucusu yedekleme ve kurtarma iş akışı, uzun süreli saklama ve şirket dışı yedekleme yönetmenize yardımcı olur.
- **VCenter tarafından yönetilen VM 'Leri Algıla ve koru:** Azure Backup Sunucusu vCenter veya ESXi sunucusunda dağıtılan VM 'Leri algılar ve korur. Azure Backup Sunucusu, büyük dağıtımları koruyabilmeniz için vCenter tarafından yönetilen VM 'Leri de algılar.
- **Klasör düzeyinde oto koruma:** vCenter, VM 'lerinizi VM klasörlerinde düzenlemenizi sağlar. Azure Backup Sunucusu, bu klasörleri algılar ve tüm alt klasörleri içeren klasör düzeyinde VM 'Leri korumak için kullanabilirsiniz. Klasörler korunurken, yalnızca söz konusu klasördeki VM 'Leri korumakla kalmaz Azure Backup Sunucusu, daha sonra eklenen VM 'Leri de korur. Azure Backup Sunucusu yeni VM 'Leri her gün algılar ve otomatik olarak korur. Sanal makinelerinizi özyinelemeli klasörlerde düzenlediğinizde, Azure Backup Sunucusu özyinelemeli klasörlerde dağıtılan yeni VM 'Leri otomatik olarak algılar ve korur.
- **Azure Backup sunucusu küme Içindeki sanal makineleri korumaya devam eder:** VM 'Ler küme içinde yük dengeleme için Vmoas olarak Azure Backup Sunucusu, VM korumasını otomatik olarak algılar ve devam ettirir.
- **Gerekli dosyaları daha hızlı kurtarın:** Azure Backup Sunucusu, tüm VM 'leri kurtarmadan bir Windows sanal makinesi 'nden dosya veya klasör kurtarabilir.

## <a name="limitations"></a>Sınırlamalar

- Azure Backup Sunucusu v3 için güncelleştirme paketi 1 ' in yüklü olması gerekir.
- İlk Azure Backup Sunucusu yedeklemeden önce Kullanıcı anlık görüntülerini yedekleyemiyoruz. Azure Backup Sunucusu ilk yedeklemeyi tamamladıktan sonra, Kullanıcı anlık görüntülerini yedekleyebilirsiniz.
- Azure Backup Sunucusu, geçişli disklerle ve fiziksel ham cihaz eşlemeleriyle (pRDMs) VMware VM 'lerini koruyamaz.
- Azure Backup Sunucusu, VMware vApps 'i algılayamaz veya koruyamaz.

Azure VMware çözümü için Azure Backup Sunucusu ayarlamak için aşağıdaki adımları izlemeniz gerekir:

- Önkoşulları ve ortamı ayarlayın.
- Kurtarma Hizmetleri kasası oluşturun.
- Azure Backup Sunucusu indirin ve yükleyin.
- Azure Backup Sunucusu depolama ekleyin.

### <a name="deployment-architecture"></a>Dağıtım mimarisi

Azure Backup Sunucusu, Azure VMware Çözüm VM 'lerini korumak için bir Azure hizmet olarak altyapı (IaaS) VM 'si olarak dağıtılır.

:::image type="content" source="media/avs-backup/deploy-mabs-avs-diagram.png" alt-text="AVS dağıtım mimarisi" border="false":::

## <a name="prerequisites-for-the-azure-backup-server-environment"></a>Azure Backup Sunucusu ortamı önkoşulları

Azure ortamınızda Azure Backup Sunucusu yüklerken bu bölümdeki önerileri göz önünde bulundurun.

### <a name="azure-virtual-network"></a>Azure Sanal Ağ

[Azure 'Da VMware özel bulutunuz için ağ iletişimini yapılandırmadiğinizden](tutorial-configure-networking.md)emin olun.

### <a name="determine-the-size-of-the-virtual-machine"></a>Sanal makinenin boyutunu belirle

Önceki adımda oluşturduğunuz sanal ağda bir Windows sanal makinesi oluşturmanız gerekir. Azure Backup Sunucusu çalıştırmak için bir sunucu seçtiğinizde, Windows Server 2019 Datacenter 'un bir galeri görüntüsü ile başlayın. [Ilk Windows sanal makinenizi Azure Portal oluşturma](../virtual-machines/windows/quick-create-portal.md) öğreticisinde, Azure 'u hiç kullanmamış olsanız bile, Azure 'DA önerilen VM 'yi kullanmaya başlamanızı sağlayabilirsiniz.

Aşağıdaki tabloda her bir Azure Backup Sunucusu sanal makine boyutu için en fazla korumalı iş yükü sayısı özetlenmektedir. Bilgiler, iç performansa ve iş yükü boyutu ile karmaşıklık için kurallı değerlere dayanır. Gerçek iş yükü boyutu daha büyük olabilir, ancak Azure Backup Sunucusu sanal makinesine bağlı diskler tarafından konamelidir.

| En fazla korumalı iş yükü | Ortalama iş yükü boyutu | Ortalama iş yükü karmaşıklığı (günlük) | Minimum depolama ıOPS 'si | Önerilen disk türü/boyutu      | Önerilen VM boyutu |
|-------------------------|-----------------------|--------------------------------|------------------|-----------------------------------|---------------------|
| 20                      | 100 GB                | Net %5 karmaşıklık                   | 2.000             | Standart HDD (disk başına 8 TB veya üzeri boyut)  | A4V2       |
| 40                      | 150 GB                | Net %10 karmaşıklık                  | 4.500             | Premium SSD * (disk başına 1 TB veya üzeri boyut) | DS3_V2     |
| 60                      | 200 GB                | Net %10 karmaşıklık                  | 10.500            | Premium SSD * (disk başına 8 TB veya üzeri boyut) | DS3_V2     |

* Gerekli IOPS 'yi almak için önerilen en düşük veya daha yüksek boyutlu diskleri kullanın. Küçük boyutlu diskler daha düşük IOPS sağlar.

> [!NOTE]
> Azure Backup Sunucusu, özel, tek amaçlı bir sunucuda çalışmak üzere tasarlanmıştır. Azure Backup Sunucusu şu şekilde bir bilgisayara yükleyemezsiniz:
> * Bir etki alanı denetleyicisi olarak çalışır.
> * Uygulama sunucusu rolünü yüklemiştir.
> * Bir System Center Operations Manager yönetim sunucusudur.
> * Exchange Server çalıştırır.
> * , Bir kümenin düğümüdür.

### <a name="disks-and-storage"></a>Diskler ve depolama alanı

Azure Backup Sunucusu, yükleme için, sistem dosyalarını, yükleme dosyalarını, önkoşul olan yazılımı, veritabanı dosyalarını ve depolama havuzu için ayrılmış diskleri içeren diskler gerektirir.

| Gereksinim                      | Önerilen boyut  |
|----------------------------------|-------------------------|
| Azure Backup Sunucusu yükleme                | Yükleme konumu: 3 GB<br />Veritabanı dosyaları sürücüsü: 900 MB<br />Sistem sürücüsü: SQL Server yüklemesi için 1 GB<br /><br />Ayrıca, arşivleme sırasında dosya kataloğunu geçici bir yükleme konumuna kopyalamak için Azure Backup Sunucusu alana ihtiyacınız vardır.      |
| Depolama havuzu için disk<br />(Temel birimleri kullanır, dinamik bir diskte olamaz) | Korunan verilerin boyutunun iki ile üç katı.<br />Ayrıntılı depolama hesaplaması için bkz. [DPM Capacity Planner](https://www.microsoft.com/download/details.aspx?id=54301).   |

Mevcut bir Azure VM 'ye yeni bir yönetilen veri diski iliştirme hakkında bilgi edinmek için, bkz. [Azure Portal kullanarak bir WINDOWS VM 'ye yönetilen veri diski iliştirme](../virtual-machines/windows/attach-managed-disk-portal.md).

> [!NOTE]
> Tek bir Azure Backup Sunucusu, depolama havuzu için geçici 120 TB 'lik sınıra sahiptir.

### <a name="store-backup-data-on-local-disk-and-in-azure"></a>Yedekleme verilerini yerel diskte ve Azure 'da depolayın

Yedekleme verilerinin Azure 'da depolanması, Azure Backup Sunucusu VM 'deki yedekleme altyapısını azaltır. İşletimsel kurtarma (yedekleme) için Azure Backup Sunucusu, yedekleme verilerini VM 'ye bağlı Azure disklerinde depolar. Diskler ve depolama alanı VM 'ye eklendikten sonra, depolamayı sizin için yönetir Azure Backup Sunucusu. Yedekleme veri depolama alanı miktarı, her bir Azure sanal makinesine bağlı disklerin sayısına ve boyutuna bağlıdır. Azure VM 'nin her boyutunun, eklenebilecek en fazla disk sayısı vardır. Örneğin, a2 dört diskdir, a3 sekiz diskdir ve A4 16 diskdir. Yeniden, disk boyutu ve sayısı toplam yedekleme depolama havuzu kapasitesini tespit.

> [!IMPORTANT]
> Beş günden uzun bir süre içinde Azure Backup Sunucusu bağlı disklerde işlemsel kurtarma *verilerini korumamalıdır* . Veriler beş günden eski olursa, bunu bir kurtarma hizmetleri kasasında saklayın.

Yedekleme verilerini Azure 'da depolamak için bir kurtarma hizmetleri Kasası oluşturun veya kullanın. Azure Backup Sunucusu iş yükünü yedeklemeye hazırlandığınızda, [Kurtarma Hizmetleri kasasını yapılandırırsınız](#create-a-recovery-services-vault). Yapılandırıldıktan sonra, bir çevrimiçi yedekleme işi her çalıştığında kasada bir kurtarma noktası oluşturulur. Her kurtarma hizmetleri Kasası en fazla 9.999 kurtarma noktası içerir. Oluşturulan kurtarma noktası sayısına ve ne kadar süreyle saklandığına bağlı olarak, yedekleme verilerini birçok yıl boyunca koruyabilirsiniz. Örneğin, aylık kurtarma noktaları oluşturabilir ve bunları beş yıl boyunca koruyabilirsiniz.

> [!IMPORTANT]
> Yedekleme verilerini Azure 'a göndermenize veya yerel olarak tutmaya bakılmaksızın, Azure Backup Sunucusu kurtarma hizmetleri kasasıyla kaydetmeniz gerekir.

### <a name="scale-deployment"></a>Ölçek dağıtımı

Dağıtımınızı ölçeklendirmek istiyorsanız aşağıdaki seçeneklere sahip olursunuz:

- **Ölçeği**artırma: Azure Backup sunucusu VM 'Nin boyutunu BIR serinin DS3 serisine artırın ve yerel depolamayı arttırın.
- **Yük boşaltma verileri**: eski verileri Azure 'a gönderin ve yalnızca Azure Backup sunucusu makinesine bağlı depolamada bulunan en yeni verileri koruyun.
- **Ölçeği genişletme**: iş yüklerini korumak için daha fazla Azure Backup sunucusu makine ekleyin.

### <a name="net-framework"></a>.NET Framework

VM 'de .NET Framework 3,5 SP1 veya üzeri yüklü olmalıdır.

### <a name="join-a-domain"></a>Bir etki alanına katılma

Azure Backup Sunucusu VM bir etki alanına katılmalıdır ve VM üzerinde yönetici ayrıcalıklarına sahip bir etki alanı kullanıcısının Azure Backup Sunucusu yüklemesi gerekir.

Önizleme sırasında desteklenmese de, bir Azure VM 'de dağıtılan Azure Backup Sunucusu, Azure VMware çözümünde VM 'lerde iş yüklerini yedekleyebilir. Yedekleme işleminin etkinleştirilmesi için iş yüklerinin aynı etki alanında olması gerekir.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri Kasası, zaman içinde oluşturulan kurtarma noktalarını depolayan bir depolama varlıktır. Ayrıca, korunan öğelerle ilişkili yedekleme ilkeleri de içerir.

1. [Azure Portal](https://portal.azure.com/)aboneliğinizde oturum açın.

1. Sol taraftaki menüden **tüm hizmetler**' i seçin.

   ![Tüm hizmetleri seçin](../backup/media/backup-create-rs-vault/click-all-services.png)

1. **Tüm hizmetler** Iletişim kutusunda **Kurtarma Hizmetleri** ' ni girin ve listeden **Kurtarma Hizmetleri kasaları** ' nı seçin.

   ![Kurtarma Hizmetleri kasalarını girin ve seçin](../backup/media/backup-create-rs-vault/all-services.png)

   Abonelikteki kurtarma hizmetleri kasalarının listesi görüntülenir.

1. **Kurtarma Hizmetleri kasaları** panosunda **Ekle**' yi seçin.

   ![Kurtarma Hizmetleri Kasası ekleme](../backup/media/backup-create-rs-vault/add-button-create-vault.png)

   **Kurtarma Hizmetleri Kasası** iletişim kutusu açılır.

1. **Ad**, **abonelik**, **kaynak grubu**ve **konum**için değerler girin.

   ![Kurtarma Hizmetleri kasasını yapılandırma](../backup/media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Ad**: kasayı tanımlamak için bir kolay ad girin. Ad, Azure aboneliğine özgü olmalıdır. En az iki tane 50 karakterden oluşan bir ad belirtin. Ad bir harfle başlamalı ve yalnızca harf, rakam ve kısa çizgi içermelidir.
   - **Abonelik**: kullanılacak aboneliği seçin. Yalnızca bir aboneliğin üyesiyseniz, bu adı görürsünüz. Hangi aboneliğin kullanılacağı konusunda emin değilseniz, varsayılan (önerilen) aboneliği kullanın. Yalnızca iş veya okul hesabınızın birden fazla Azure aboneliğiyle ilişkilendirilmesi durumunda birden çok seçenek vardır.
   - **Kaynak grubu**: var olan bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Aboneliğinizdeki kullanılabilir kaynak gruplarının listesini görmek için **Varolanı kullan**' ı seçin ve ardından açılır listeden bir kaynak seçin. Yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' u seçin ve adı girin.
   - **Konum**: kasa için coğrafi bölgeyi seçin. Azure VMware Çözüm sanal makinelerini korumaya yönelik bir kasa oluşturmak için kasanın Azure VMware Çözüm özel bulutuyla aynı bölgede olması *gerekir* .

1. Kurtarma Hizmetleri kasasını oluşturmaya hazırsanız **Oluştur**' u seçin.

   ![Kurtarma Hizmetleri kasasını oluşturma](../backup/media/backup-create-rs-vault/click-create-button.png)

   Kurtarma Hizmetleri kasasının oluşturulması biraz zaman alabilir. Portalın sağ üst köşesindeki **Bildirimler** alanında durum bildirimlerini izleyin. Kasanızın oluşturulduktan sonra kurtarma hizmetleri kasaları listesinde görünür. Kasanızı görmüyorsanız **Yenile**' yi seçin.

   ![Yedekleme kasaları listesini yenileme](../backup/media/backup-create-rs-vault/refresh-button.png)

## <a name="set-storage-replication"></a>Depolama çoğaltmasını ayarla

Depolama çoğaltma seçeneği, coğrafi olarak yedekli depolama (varsayılan) ve yerel olarak yedekli depolama arasında seçim yapmanızı sağlar. Coğrafi olarak yedekli depolama, Depolama hesabınızdaki verileri ikincil bir bölgeye kopyalar ve bu da verilerinizin dayanıklı olmasını sağlar. Yerel olarak yedekli depolama, dayanıklı olmayan bir ucuz seçeneğidir. Coğrafi olarak yedekli ve yerel olarak yedekli depolama seçenekleri hakkında daha fazla bilgi edinmek için bkz. [Azure depolama artıklığı](../storage/common/storage-redundancy.md).

> [!IMPORTANT]
> Bir kurtarma hizmetleri Kasası için **yerel olarak yedekli/coğrafi olarak yedekli depolama çoğaltma türü** ayarını değiştirmek, kasadaki yedeklemeleri yapılandırmadan önce yapılmalıdır. Yedeklemeleri yapılandırdıktan sonra, bunu değiştirme seçeneği devre dışı bırakılır ve depolama çoğaltma türünü değiştiremezsiniz.

1. **Kurtarma Hizmetleri kasalarından**yeni kasayı seçin. 

1. **Ayarlar** bölümünde **Özellikler**’i seçin. **Yedekleme yapılandırması**altında **Güncelleştir**' i seçin.

1. Depolama çoğaltma türünü seçin ve **Kaydet**' i seçin.

   ![Yeni kasa için depolama yapılandırmasını ayarla](../backup/media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

## <a name="download-and-install-the-software-package"></a>Yazılım paketini indirme ve yükleme

Yazılım paketini indirmek, ayıklamak ve yüklemek için bu bölümdeki adımları izleyin.

### <a name="download-the-software-package"></a>Yazılım paketini indirin

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. Zaten açık bir kurtarma hizmetleri Kasası varsa, bir sonraki adımla devam edin. Bir kurtarma hizmetleri Kasası açık değilse ancak Azure portal, ana menüde, **Araştır**' ı seçin.

   1. Kaynak listesinde **Kurtarma Hizmetleri**' ni girin.

   1. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Kurtarma Hizmetleri kasalarını**gördüğünüzde, bunu seçin.

   ![Kurtarma Hizmetleri Kasası oluşturma adımı 1](../backup/media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png)

1. Kurtarma Hizmetleri kasalarının listesinden bir kasa seçin.

   Seçilen kasa panosu açılır.

   ![Kasa panosunu aç](../backup/media/backup-azure-microsoft-azure-backup/vault-dashboard.png)

   **Ayarlar** seçeneği varsayılan olarak açılır. Kapalıysa, açmak için **Ayarlar** ' ı seçin.

   ![Kasa ayarları seçeneğini aç](../backup/media/backup-azure-microsoft-azure-backup/vault-setting.png)

1. **Başlangıç Sihirbazı 'nı** açmak için **Yedekle** ' yi seçin.

   ![Yedekleme Başlarken Sihirbazı 'nı açar](../backup/media/backup-azure-microsoft-azure-backup/getting-started-backup.png)

1. Açılan pencerede şunları yapın:

   1. **İş yükünüz nerede çalışıyor?** menüsünde **Şirket içi**' ı seçin.

      :::image type="content" source="media/avs-backup/deploy-mabs-on-premises-workload.png" alt-text="İş yükünüz nerede çalışıyor?":::

   1. **Neleri yedeklemek istiyorsunuz?** menüsünde, Azure Backup sunucusu kullanarak korumak istediğiniz iş yüklerini seçin.

   1. Azure Backup Sunucusu ve kasa kimlik bilgilerini indirmek ve yüklemek için **altyapıyı hazırla** ' yı seçin.

      :::image type="content" source="media/avs-backup/deploy-mabs-prepare-infrastructure.png" alt-text="Altyapıyı hazırla":::

1. Açılan **altyapıyı hazırla** penceresinde şunları yapın:

   1. Azure Backup Sunucusu yüklemek için **indirme** bağlantısını seçin.

   1. **Zaten Indirilmiş olan veya en son Azure Backup sunucusu yükleme** onay kutusunu seçerek kasa kimlik bilgilerini indirin ve ardından **İndir**' i seçin. Kurtarma Hizmetleri kasasına Azure Backup Sunucusu kaydı sırasında kasa kimlik bilgilerini kullanırsınız. Bağlantılar sizi, yazılım paketini indirdiğiniz Indirme merkezi 'ne götürür.

   :::image type="content" source="media/avs-backup/deploy-mabs-prepare-infrastructure2.png" alt-text="Altyapıyı hazırla-Azure Backup Sunucusu":::

1. İndirme sayfasında, tüm dosyalar ' ı seçin ve **İleri**' yi seçin.

   > [!NOTE]
   > Tüm dosyaları aynı klasöre indirmeniz gerekir. Dosyaların karşıdan yükleme boyutu 3 GB 'tan büyük olduğundan, indirmenin tamamlanması 60 dakika sürebilir. 

   ![Indirme merkezinde dosya seçme](../backup/media/backup-azure-microsoft-azure-backup/downloadcenter.png)

### <a name="extract-the-software-package"></a>Yazılım paketini Ayıkla

Yazılım paketini farklı bir sunucuya indirdiyseniz, dosyaları dağıtmak için oluşturduğunuz sanal makineye kopyalayın Azure Backup Sunucusu.

> [!WARNING]
> Kurulum dosyalarını ayıklamak için en az 4 GB boş alan gerekir.

1. Tüm dosyaları indirdikten sonra, **Microsoft Azure Backup** Kurulum Sihirbazı 'nı açmak için **MicrosoftAzureBackupInstaller.exe** çift tıklayın ve ardından **İleri**' yi seçin.

1. Dosyaların ayıklanacağı konumu seçin ve **İleri**' yi seçin.

1. Ayıklama işlemini başlatmak için **Ayıkla** ' yı seçin.

   ![Microsoft Azure Backup Kurulum Sihirbazı](../backup/media/backup-azure-microsoft-azure-backup/extract/03.png)

1. Ayıklandıktan sonra **setup.exeyürütme** seçeneğini belirleyip **son**' u seçin.

> [!TIP]
> setup.exe dosyasını, yazılım paketini ayıkladığınız klasörden de bulabilirsiniz.

### <a name="install-the-software-package"></a>Yazılım paketini yükler

1. **Yükleme**altındaki kurulum penceresinde, Kurulum Sihirbazı 'nı açmak için **Microsoft Azure Backup** ' yi seçin.

   ![Microsoft Azure Backup Kurulum Sihirbazı yükleme düğmesi](../backup/media/backup-azure-microsoft-azure-backup/launch-screen2.png)

1. **Karşılama** ekranında, **önkoşul denetimleri** sayfasına devam etmek için **İleri** ' yi seçin.

1. Azure Backup Sunucusu için donanım ve yazılım önkoşullarının karşılanıp karşılanmadığını belirlemek için **yeniden denetle** ' yi seçin. Başarıyla karşılanırsa, **İleri**' yi seçin.

   ![Azure Backup Sunucusu önkoşul denetimi](../backup/media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)

1. Azure Backup Sunucusu yükleme paketi, gerekli olan uygun SQL Server ikilileriyle birlikte sunulur. Yeni bir Azure Backup Sunucusu yüklemesi başlattığınızda, **Bu kurulum ile yeni SQL Server örneğini yükleme** seçeneğini belirleyin. Sonra **Denetle ve yüklensin '** i seçin.

   ![Azure Backup Sunucusu SQL Server denetimi](../backup/media/backup-azure-microsoft-azure-backup/sql/01.png)

   > [!NOTE]
   > Kendi SQL Server örneğinizi kullanmak istiyorsanız desteklenen SQL Server sürümleri SQL Server 2014 SP1 veya üzeri, 2016 ve 2017. Tüm SQL Server sürümleri Standard veya Enterprise 64-bit olmalıdır. Azure Backup Sunucusu uzak bir SQL Server örneğiyle çalışmıyor. Azure Backup Sunucusu tarafından kullanılan örnek yerel olmalıdır. Azure Backup Sunucusu için mevcut bir SQL Server örneğini kullanıyorsanız, kurulum yalnızca SQL Server *adlandırılan örneklerin* kullanımını destekler.

   Makineyi yeniden başlatma önerisiyle ilgili bir hata oluşursa, bunu yapın ve **yeniden denetle**' yi seçin. SQL Server yapılandırma sorunları varsa, SQL Server yönergelerine göre SQL Server yeniden yapılandırın. Ardından Azure Backup Sunucusu SQL Server mevcut örneğini kullanarak yüklemeyi veya yükseltmeyi yeniden deneyin.

   **El ile yapılandırma**

   Kendi SQL Server örneğinizi kullandığınızda ana veritabanına sysadmin rolüne BUILTIN\Administrators eklediğinizden emin olun.

   **SQL Server 2017 ile SSRS yapılandırması**

   2017 SQL Server kendi örneğini kullandığınızda SQL Server 2017 Reporting Services 'ı (SSRS) el ile yapılandırmanız gerekir. SSRS yapılandırmasından sonra SSRS 'nin **IsInitialized** özelliğinin **true**olarak ayarlandığından emin olun. Bu özellik **true**olarak AYARLANDıĞıNDA Azure Backup sunucusu SSRS 'nin zaten yapılandırıldığını VARSAYAR ve SSRS yapılandırmasını atlar.

   SSRS yapılandırma durumunu denetlemek için şu komutu çalıştırın:

   ```powershell
   $configset =Get-WmiObject –namespace 
   "root\Microsoft\SqlServer\ReportServer\RS_SSRS\v14\Admin" -class 
   MSReportServer_ConfigurationSetting -ComputerName localhost

   $configset.IsInitialized
   ```

   SSRS yapılandırması için aşağıdaki değerleri kullanın:

   * **Hizmet hesabı**: **yerleşik hesap kullan** **ağ hizmeti**olmalıdır.
   * **Web hizmeti URL 'si**: **sanal dizin** **ReportServer_ \<SQLInstanceName> **olmalıdır.
   * **Veritabanı**: **DatabaseName** **ReportServer $ \<SQLInstanceName> **olmalıdır.
   * **Web portalı URL 'si**: **sanal dizin** **Reports_ \<SQLInstanceName> **olmalıdır.

   SSRS yapılandırması hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/sql/reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode?view=sql-server-2017) .

   > [!NOTE]
   > [Microsoft Online Services terms](https://www.microsoft.com/licensing/product-licensing/products) (OST), Azure Backup sunucusu veritabanı olarak kullanılan SQL Server lisansını yönetir. Azure Backup Sunucusu ile paketlenmiş SQL Server, OST 'ye göre yalnızca Azure Backup Sunucusu veritabanı olarak kullanılabilir.

1. Yükleme başarılı olduktan sonra **İleri**' yi seçin.

1. Microsoft Azure Backup Server dosyalarının yüklenmesi için bir konum belirtin ve **İleri**' yi seçin.

   > [!NOTE]
   > Azure 'a yedekleme için karalama konumu gereklidir. Karalama konumunun, buluta yedeklenmek üzere planlandığı verilerin en az %5 ' i olduğundan emin olun. Disk koruması için, yükleme tamamlandıktan sonra ayrı disklerin yapılandırılması gerekir. Depolama havuzları hakkında daha fazla bilgi için bkz. [depolama havuzlarını ve Disk depolamayı yapılandırma](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/hh758075(v=sc.12)).

   ![Microsoft Azure Backup kurulum yükleme ayarları](../backup/media/backup-azure-microsoft-azure-backup/space-screen.png)

1. Kısıtlı yerel kullanıcı hesapları için güçlü bir parola sağlayın ve **İleri**' yi seçin.

   ![Microsoft Azure Backup kurulum güvenlik ayarları](../backup/media/backup-azure-microsoft-azure-backup/security-screen.png)

1. Güncelleştirmeleri denetlemek için Microsoft Update kullanmak isteyip istemediğinizi seçin ve **İleri**' yi seçin.

   > [!NOTE]
   > Windows ve Azure Backup Sunucusu gibi diğer ürünler için güvenlik ve önemli güncelleştirmeler sunan Microsoft Update Windows Update yeniden yönlendirmenizi öneririz.

   ![Microsoft Azure Backup kurulum Microsoft Update kabul etme](../backup/media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)

1. **Ayarların özetini**gözden geçirin ve **yükler**' i seçin.

   Yükleme aşamalarda gerçekleşir. İlk aşamada Microsoft Azure Kurtarma Hizmetleri Aracısı yüklenir ve ikinci aşama Internet bağlantısını denetler. Internet bağlantısı varsa, yüklemeye devam edebilirsiniz. Aksi takdirde, internet 'e bağlanmak için proxy ayrıntılarını sağlamanız gerekir. Son aşama Önkoşul yazılımını denetler. Yüklü değilse, eksik yazılımlar Microsoft Azure Kurtarma Hizmetleri aracısıyla birlikte yüklenir.

1. Makineyi kurtarma hizmetleri kasasına kaydetmek için kasa kimlik bilgilerini bulmak üzere **Araştır** ' ı seçin ve ardından **İleri**' yi seçin.

1. Azure ile şirket içi arasında gönderilen verileri şifrelemek veya şifresini çözmek için bir pass ifadesi seçin.

   > [!TIP]
   > Otomatik olarak bir parola oluşturabilir veya en düşük 16 karakterlik parolanızı sağlayabilirsiniz.

1. Geçiş tümceciğinin kaydedileceği konumu girin ve ardından sunucuyu kaydetmek için **İleri** ' yi seçin.

   > [!IMPORTANT]
   > Pass deyimini yerel sunucu dışında bir güvenli konuma kaydedin. Pass tümceciğini depolamak için Azure Key Vault kullanmanızı kesinlikle öneririz.

   Microsoft Azure Kurtarma Hizmetleri Aracısı kurulumu bittikten sonra, yükleme adımı SQL Server ve Azure Backup Sunucusu bileşenleri için yükleme ve yapılandırmaya gider.

   ![Microsoft Azure Backup kurulum yüklemesi](../backup/media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

1. Yükleme adımı tamamlandıktan sonra **Kapat**' ı seçin.

### <a name="install-update-rollup-1"></a>Güncelleştirme paketi 1 ' i yükler

İş yüklerini koruyabilmeniz için Azure Backup Sunucusu v3 için güncelleştirme paketi 1 ' in yüklenmesi zorunludur. Hata düzeltmelerinin listesini ve Azure Backup Sunucusu v3 güncelleştirme paketi 1 için yükleme yönergelerini görüntülemek için bkz. Bilgi Bankası makalesi [4534062](https://support.microsoft.com/en-us/help/4534062/).

## <a name="add-storage-to-azure-backup-server"></a>Azure Backup Sunucusu’na depolama alanı ekleme

Azure Backup Sunucusu v3 şunları sağlayan Modern Yedekleme Alanı destekler:

-  %50 depolama tasarrufu.
-  Üç kat daha hızlı yedeklemeler.
-  Daha verimli depolama.
-  İş yükü kullanan depolama.

### <a name="volumes-in-azure-backup-server"></a>Azure Backup Sunucusu birimler

Önceden eklenmemişse, gerekli depolama kapasitesine sahip veri disklerini Azure Backup Sunucusu sanal makineye ekleyin.

Azure Backup Sunucusu v3 yalnızca depolama birimlerini kabul eder. Bir birim eklediğinizde Azure Backup Sunucusu, birimi Modern Yedekleme Alanı gereken dayanıklı dosya sistemi (ReFS) olarak biçimlendirir.

### <a name="add-volumes-to-azure-backup-server-disk-storage"></a>Azure Backup Sunucusu disk depolamaya birim ekleme

1. **Yönetim** bölmesinde, depolamayı yeniden tarayın ve **Ekle**' yi seçin. 

1. Depolama havuzuna eklemek için kullanılabilir birimler arasından seçim yapın. 

1. Kullanılabilir birimleri ekledikten sonra, bunları yönetmenize yardımcı olacak kolay bir ad verin. 

1. Azure Backup Sunucusu Modern Yedekleme Alanı avantajları kullanabilmesi için bu birimleri ReFS olarak biçimlendirmek üzere **Tamam ' ı** seçin.

![Kullanılabilir birimleri ekleme](../backup/media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Backup Sunucusu kullanarak Azure VMware çözümünde çalışan VMware VM 'lerinin yedeklenmesini yapılandırma hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure VMware Çözüm VM 'lerinin yedeklemesini yapılandırma](backup-avs-vms-with-mabs.md)


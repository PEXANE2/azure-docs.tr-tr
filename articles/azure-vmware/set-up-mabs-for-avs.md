---
title: Azure VMware çözümü (AVS) için Microsoft Azure Backup Server 'ı ayarlama
description: Microsoft Azure Backup sunucusu kullanarak sanal makineleri yedeklemek için Azure VMware Solution (AVS) ortamınızı ayarlayın.
ms.topic: how-to
ms.date: 06/09/2020
ms.openlocfilehash: 23c29f453587ac7a232c21e43fa6fb45193881bc
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84613113"
---
# <a name="set-up-microsoft-azure-backup-server-for-avs"></a>AVS için Microsoft Azure Backup Server 'ı ayarlama

Microsoft Azure Backup sunucusu, Iş sürekliliği ve olağanüstü durum kurtarma (BCDR) stratejinize katkıda bulunan sağlam bir kurumsal yedekleme ve kurtarma sistemidir. AVS önizlemesi sırasında, Azure Backup Sunucusu kullanarak yalnızca sanal makine düzeyinde yedekleme yapılandırabilirsiniz. 

Azure Backup Sunucusu, yedekleme verilerini şu şekilde saklayabilir:

- **Disk**: kısa vadeli depolama için, verileri disk havuzlarına Yedekler Azure Backup sunucusu.

- **Azure**: hem kısa vadeli hem de uzun vadeli depolama için şirket içi Azure Backup sunucusu, disk havuzlarında depolanan verileri Azure Backup hizmeti kullanılarak Microsoft Azure buluta yedeklenebilir.

Kesintiler gerçekleştiğinde ve kaynak veriler kullanılamıyorsa, verileri kaynağa veya alternatif bir konuma kolayca geri yüklemek için Azure Backup Sunucusu kullanabilirsiniz. Bu şekilde, planlanan veya beklenmeyen sorunlar nedeniyle özgün veriler kullanılamıyorsa, verileri farklı bir konuma kolayca geri yükleyebilirsiniz.

Bu makalede, AVS ortamınızı Azure Backup Sunucusu kullanarak sanal makineleri (VM) yedeklemeye hazırlamanızı öneririz.  Şu adımlarda size kılavuzluk ederiz: 

> [!div class="checklist"]
> * Önerilen VM disk türünü ve kullanılacak boyutu belirleme
> * Kurtarma noktalarını depolayan bir kurtarma hizmetleri Kasası oluşturma
> * Kurtarma Hizmetleri Kasası için depolama çoğaltmayı ayarlama
> * Azure Backup Sunucusu’na depolama alanı ekleme

## <a name="supported-vmware-features"></a>Desteklenen VMware özellikleri

- **Aracısız yedekleme:** Azure Backup Sunucusu, sanal makineyi yedeklemek için vCenter veya ESXi sunucusuna bir aracının yüklenmesini gerektirmez. Bunun yerine, Azure Backup Sunucusu ile VMware sunucusunun kimliğini doğrulamak için kullanılan IP adresini veya tam etki alanı adını (FQDN) ve oturum açma kimlik bilgilerini sağlamanız yeterlidir.

- **Bulutta tümleşik yedekleme:** Azure Backup Sunucusu, iş yüklerini disk ve buluta korur. Azure Backup Sunucusu yedekleme ve kurtarma iş akışı, uzun süreli saklama ve şirket dışı yedekleme yönetmenize yardımcı olur.

- **VCenter tarafından yönetilen VM 'Leri Algıla ve koru:** Azure Backup Sunucusu vCenter veya ESXi sunucusunda dağıtılan VM 'Leri algılar ve korur. Azure Backup Sunucusu Ayrıca, vCenter tarafından yönetilen VM 'Leri algılar, böylece büyük dağıtımları koruyabilirsiniz.

- **Klasör düzeyinde otomatik koruma:** vCenter, sanal makinelerinizi VM klasörlerinde düzenlemenizi sağlar. Azure Backup Sunucusu, bu klasörleri algılar ve VM 'Leri klasör düzeyinde korumanıza olanak sağlar ve tüm alt klasörleri içerir. Klasörler korunurken, yalnızca söz konusu klasördeki VM 'Leri korumakla kalmaz Azure Backup Sunucusu, daha sonra eklenen VM 'Leri de korur. Azure Backup Sunucusu yeni VM 'Leri her gün algılar ve otomatik olarak korur. Sanal makinelerinizi özyinelemeli klasörlerde düzenlediğinizde, Azure Backup Sunucusu özyinelemeli klasörlerde dağıtılan yeni VM 'Leri otomatik olarak algılar ve korur.

- **Azure Backup sunucusu küme Içindeki sanal makineleri korumaya devam eder:** VM 'Ler küme içinde yük dengeleme için Vmoas olarak Azure Backup Sunucusu, VM korumasını otomatik olarak algılar ve devam ettirir.

- **Gerekli dosyaları daha hızlı kurtarın:** Azure Backup Sunucusu, tüm VM 'leri kurtarmadan dosyaları/klasörleri bir Windows VM 'sinden kurtarabilir.

## <a name="limitations"></a>Sınırlamalar

- Azure Backup Sunucusu v3 için güncelleştirme paketi 1 ' in yüklü olması gerekir.

- İlk Azure Backup Sunucusu yedeklemeden önce Kullanıcı anlık görüntülerini yedekleyemezsiniz. Azure Backup Sunucusu ilk yedeklemeyi tamamladıktan sonra, Kullanıcı anlık görüntülerini yedekleyebilirsiniz.

- Azure Backup Sunucusu, VMware VM 'lerini doğrudan geçiş diskleri ve fiziksel ham cihaz eşlemeleriyle koruyamaz (pRDM).

- Azure Backup Sunucusu, VMware vApps 'i algılayamaz veya koruyamaz.

**Azure VMware çözümü (AVS) için Microsoft Azure Backup sunucusu ayarlamak için aşağıdaki adımları gerçekleştirmeniz gerekir:**

- Önkoşulları ve ortamı ayarlayın

- Azure kurtarma hizmetleri Kasası oluşturma

- Azure Backup Sunucusu indirin ve yükleyin 

- Azure Backup Sunucusu’na depolama alanı ekleme 

**Dağıtım mimarisi**  
Microsoft Azure Backup sunucusu, AVS VM 'lerini korumak için Azure IaaS VM olarak dağıtılır.

:::image type="content" source="media/avs-backup/deploy-mabs-avs-diagram.png" alt-text="AVS dağıtım mimarisi" border="false":::

## <a name="prerequisites-for-the-azure-backup-server-environment"></a>Azure Backup Sunucusu ortamı önkoşulları

Azure Backup Sunucusu Azure ortamınıza yüklerken bu bölümdeki önerileri göz önünde bulundurun.

### <a name="azure-virtual-network"></a>Azure Sanal Ağ

[Azure 'Da VMware özel bulutunuz için ağ iletişimini yapılandırmadiğinizden](tutorial-configure-networking.md)emin olun.

### <a name="determine-the-size-of-the-virtual-machine"></a>Sanal makinenin boyutunu belirle

Yukarıdaki adımda oluşturduğunuz sanal ağda bir Windows sanal makinesi oluşturmanız gerekir. Azure Backup Sunucusu çalıştırmak için bir sunucu seçerken, Windows Server 2019 Datacenter 'un Galeri görüntüsüyle başlamanız önerilir. [Ilk Windows sanal makinenizi Azure Portal](../virtual-machines/windows/quick-create-portal.md) öğreticide oluşturun, Azure 'u hiç kullanmamış olsanız bile, Azure 'DA önerilen VM 'yi kullanmaya başlamanızı sağlayabilirsiniz.

Aşağıdaki tabloda her bir Azure Backup Sunucusu sanal makine boyutu için en fazla korumalı iş yükü sayısı özetlenmektedir. Bilgiler, iç performansa ve iş yükü boyutu ile karmaşıklık için kurallı değerlere dayanır. Gerçek iş yükü boyutu daha büyük olabilir, ancak Azure Backup Sunucusu sanal makinesine bağlı diskler tarafından konamelidir.

| Maks. korunan iş yükü sayısı | Ortalama iş yükü boyutu | Ortalama iş yükü karmaşıklığı (günlük) | Minimum depolama ıOPS 'si | Önerilen disk türü/boyutu      | Önerilen VM boyutu |
|-------------------------|-----------------------|--------------------------------|------------------|-----------------------------------|---------------------|
| 20                      | 100 GB                | Net %5 karmaşıklık                   | 2000             | Standart HDD (disk başına 8 TB veya üzeri boyut)  | A4V2       |
| 40                      | 150 GB                | Net %10 karmaşıklık                  | 4500             | Premium SSD * (disk başına 1 TB veya üzeri boyut) | DS3_V2     |
| 60                      | 200 GB                | Net %10 karmaşıklık                  | 10500            | Premium SSD * (disk başına 8 TB veya üzeri boyut) | DS3_V2     |

* Gerekli IOPS 'yi almak için önerilen en düşük veya daha yüksek boyutlu diskleri kullanın. Daha küçük boyutta disk daha düşük IOPS sağlar.

> [!NOTE]
> Azure Backup Sunucusu, özel, tek amaçlı bir sunucuda çalışmak üzere tasarlanmıştır. Bir bilgisayara Azure Backup Sunucusu yükleyemezsiniz:
> * Etki alanı denetleyicisi olarak çalıştırma
> * Uygulama sunucusu rolü yüklendi
> * Bu bir System Center Operations Manager yönetim sunucusudur
> * Çalışan Exchange Server
> * Bu bir kümenin bir düğümüdür

### <a name="disks-and-storage"></a>Diskler ve depolama alanı

Azure Backup Sunucusu, yükleme için, sistem dosyaları, yükleme dosyaları, önkoşul yazılımlar, veritabanı dosyaları ve depolama havuzu için ayrılmış diskler dahil olmak üzere diskler gerektirir.

| Gereksinim                      | Önerilen boyut  |
|----------------------------------|-------------------------|
| Azure Backup Sunucusu yükleme                | Yükleme konumu: 3 GB<br />Veritabanı dosyaları sürücüsü: 900 MB<br />Sistem sürücüsü: SQL yüklemesi için 1 GB<br /><br />Ayrıca, arşivleme sırasında dosya kataloğunu geçici bir yükleme konumuna kopyalamak için Azure Backup Sunucusu alana ihtiyaç duyarsınız.      |
| Depolama havuzu için disk<br />(Temel birimleri kullanır, dinamik bir diskte yer alamaz.) | korunan verilerin boyutunun 2-3 katı<br />Ayrıntılı depolama hesaplaması için [DPM Capacity Planner](https://www.microsoft.com/download/details.aspx?id=54301)başvurun.   |

[Azure Portal makalelerini kullanarak bir WINDOWS VM 'sine yönetilen veri diski iliştirme](../virtual-machines/windows/attach-managed-disk-portal.md) , mevcut BIR Azure VM 'ye yeni bir yönetilen veri diski nasıl ekleyebilirim hakkında sizi gösterir.

> [!NOTE]
> Tek bir Azure Backup Sunucusu, depolama havuzu için geçici 120 TB 'lik sınıra sahiptir.

### <a name="store-backup-data-on-local-disk-and-in-azure"></a>Yedekleme verilerini yerel diskte ve Azure 'da depolayın

Yedekleme verilerinin Azure 'da depolanması Azure Backup Sunucusu VM 'deki yedekleme altyapısını azaltır. İşletimsel kurtarma (yedekleme) için Azure Backup Sunucusu, yedekleme verilerini VM 'ye bağlı Azure disklerinde depolar. Diskler ve depolama alanı VM 'ye eklendikten sonra, depolamayı sizin için yönetir Azure Backup Sunucusu. Yedekleme veri depolama alanı miktarı, her bir Azure sanal makinesine bağlı disklerin sayısına ve boyutuna bağlıdır ve Azure VM 'nin her boyutunun eklenebilecek en fazla disk sayısı vardır. Örneğin, a2 dört diskdir. A3 sekiz disk. A4 16 diskdir. Yeniden, disk boyutu ve sayısı toplam yedekleme depolama havuzu kapasitesini tespit.

> [!IMPORTANT]
> Beş günden uzun bir süre içinde Azure Backup Sunucusu bağlı disklerde işlemsel kurtarma **verilerini korumamalıdır** . Veriler beş günden eski olursa Azure kurtarma hizmetleri kasasında saklayın.

Yedekleme verilerini Azure 'da depolamak için bir kurtarma hizmetleri Kasası oluşturun veya kullanın. Azure Backup Sunucusu iş yükünü yedeklemeye hazırlanırken, [Kurtarma Hizmetleri kasasını yapılandırırsınız](#create-a-recovery-services-vault). Yapılandırıldıktan sonra, bir çevrimiçi yedekleme işi her çalıştığında kasada bir kurtarma noktası oluşturulur. Her kurtarma hizmetleri Kasası en fazla 9999 kurtarma noktası içerir. Oluşturulan kurtarma noktası sayısına ve ne kadar süreyle saklandığına bağlı olarak, yedekleme verilerini birçok yıl boyunca koruyabilirsiniz. Örneğin, aylık kurtarma noktaları oluşturabilir ve bunları beş yıl boyunca koruyabilirsiniz.

> [!IMPORTANT]
> Yedekleme verilerini Azure 'a göndermenize veya yerel olarak tutmaya bakılmaksızın, Azure Backup Sunucusu kurtarma hizmetleri kasasıyla kaydetmeniz gerekir.

### <a name="scale-deployment"></a>Ölçek dağıtımı

Dağıtımınızı ölçeklendirmek istiyorsanız aşağıdaki seçeneklere sahip olursunuz:

- **Ölçeği** artırma-Azure Backup sunucusu VM 'Nin boyutunu BIR serinin DS3 serisine artırın ve yerel depolamayı yükseltin.

- **Veri boşaltma** -eski verileri Azure 'a gönderin ve Azure Backup sunucusu bağlı depolamada yalnızca en yeni verileri tutun.

- **Ölçeği genişletme** -iş yüklerini korumak için daha fazla Azure Backup sunucusu ekleyin.

### <a name="net-framework"></a>.NET Framework

VM 'de .NET Framework 3,5 SP1 veya üzeri yüklü olmalıdır.

### <a name="join-a-domain"></a>Bir etki alanına katılma

Azure Backup Sunucusu VM bir etki alanına katılmalıdır ve VM üzerinde yönetici ayrıcalıklarına sahip bir etki alanı kullanıcısının Azure Backup Sunucusu yüklemesi gerekir.

Önizleme sırasında desteklenmese de, bir Azure VM 'de dağıtılan Azure Backup Sunucusu, AVS 'deki VM 'lerde iş yüklerini yedekleyebilir, ancak yedekleme işlemini etkinleştirmek için aynı etki alanında olmaları gerekir.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri Kasası, zaman içinde oluşturulan kurtarma noktalarını depolayan bir depolama varlıktır. Ayrıca, korunan öğelerle ilişkili yedekleme ilkeleri de içerir.

1. [Azure Portal](https://portal.azure.com/)aboneliğinizde oturum açın.

1. Sol taraftaki menüden **tüm hizmetler**' i seçin.

   ![Tüm hizmetleri seçin](../backup/media/backup-create-rs-vault/click-all-services.png)

1. **Tüm hizmetler** Iletişim kutusunda *Kurtarma Hizmetleri* ' ni girin ve listeden **Kurtarma Hizmetleri kasaları** ' nı seçin.

   ![Kurtarma Hizmetleri kasalarını girin ve seçin](../backup/media/backup-create-rs-vault/all-services.png)

   Abonelikteki kurtarma hizmetleri kasalarının listesi görüntülenir.

1. **Kurtarma Hizmetleri kasaları** panosunda **Ekle**' yi seçin.

   ![Kurtarma Hizmetleri Kasası ekleme](../backup/media/backup-create-rs-vault/add-button-create-vault.png)

   **Kurtarma Hizmetleri Kasası** iletişim kutusu açılır.

1. **Ad**, **abonelik**, **kaynak grubu**ve **konum**için değerler sağlayın.

   ![Kurtarma Hizmetleri kasasını yapılandırma](../backup/media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Ad**: kasayı tanımlamak için bir kolay ad girin. Ad, Azure aboneliğine özgü olmalıdır. En az iki tane 50 karakterden oluşan bir ad belirtin. Ad bir harfle başlamalı ve yalnızca harf, rakam ve kısa çizgi içermelidir.

   - **Abonelik**: kullanılacak aboneliği seçin. Yalnızca bir aboneliğin üyesiyseniz, bu adı görürsünüz. Hangi aboneliğin kullanılacağı konusunda emin değilseniz, varsayılan (önerilen) aboneliği kullanın. Yalnızca iş veya okul hesabınızın birden fazla Azure aboneliğiyle ilişkilendirilmesi durumunda birden çok seçenek vardır.

   - **Kaynak grubu**: var olan bir kaynak grubunu kullanın veya yeni bir tane oluşturun. Aboneliğinizdeki kullanılabilir kaynak gruplarının listesini görmek için **Varolanı kullan**' ı seçin ve ardından açılır listeden bir kaynak seçin. Yeni bir kaynak grubu oluşturmak için **Yeni oluştur** ' u seçin ve adı girin.

   - **Konum**: kasa için coğrafi bölgeyi seçin. AVS sanal makinelerini korumaya yönelik bir kasa oluşturmak için kasanın, AVS özel bulutuyla aynı bölgede olması *gerekir* .

1. Kurtarma Hizmetleri kasasını oluşturmaya hazırsanız **Oluştur**' u seçin.

   ![Kurtarma Hizmetleri kasasını oluşturma](../backup/media/backup-create-rs-vault/click-create-button.png)

   Kurtarma Hizmetleri kasasının oluşturulması biraz zaman alabilir. Portalın sağ üst köşesindeki **Bildirimler** alanında durum bildirimlerini izleyin. Kasanızın oluşturulduktan sonra kurtarma hizmetleri kasaları listesinde görünür. Kasanızı görmüyorsanız **Yenile**' yi seçin.

   ![Yedekleme kasaları listesini yenileme](../backup/media/backup-create-rs-vault/refresh-button.png)

## <a name="set-storage-replication"></a>Depolama Çoğaltmayı Ayarlama

Depolama çoğaltma seçeneği, coğrafi olarak yedekli depolama (varsayılan) ve yerel olarak yedekli depolama arasında seçim yapmanızı sağlar. Coğrafi olarak yedekli depolama, Depolama hesabınızdaki verileri ikincil bir bölgeye kopyalar ve böylece verilerinizin dayanıklı olmasını sağlar. Yerel olarak yedekli depolama, dayanıklı olmayan bir ucuz seçeneğidir. [Azure depolama yedekliği](../storage/common/storage-redundancy.md)'nda coğrafi olarak yedekli ve yerel olarak yedekli depolama seçenekleri hakkında daha fazla bilgi edinin.

> [!IMPORTANT]
> Kasadaki yedeklemeleri yapılandırmadan önce, bir kurtarma hizmetleri Kasası için **depolama çoğaltma türünü** (yerel olarak yedekli/coğrafi olarak yedekli) değiştirme yapılmalıdır. Yedeklemeyi yapılandırdıktan sonra, bunu değiştirme seçeneği devre dışı bırakılır ve **depolama çoğaltma türünü**değiştiremezsiniz.

1. **Kurtarma Hizmetleri kasalarından**yeni kasaya tıklayın. 

1. **Ayarlar**' ın altında **Özellikler**' i seçin ve **yedekleme yapılandırması**altında **Güncelleştir**' i tıklatın.

1. Depolama çoğaltma türünü seçin ve **Kaydet**' e tıklayın.

   ![Yeni kasa için depolama yapılandırması ayarlama](../backup/media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

## <a name="download-and-install-software-package"></a>Yazılım paketini indir ve yükle

### <a name="downloading-the-software-package"></a>Yazılım paketi indiriliyor

1. [Azure portalında](https://portal.azure.com/) oturum açın.

1. Zaten açık bir kurtarma hizmetleri Kasası varsa, bir sonraki adımla devam edin. Bir kurtarma hizmetleri Kasası açık değilse, ancak Azure portal, ana menüde, **Araştır**' a tıklayın.

   1. Kaynak listesinde **Kurtarma Hizmetleri** yazın.

   1. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Kurtarma Hizmetleri kasaları** seçeneğini gördüğünüzde buna tıklayın.

   ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](../backup/media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png)

1. Kurtarma Hizmetleri kasalarının listesinden bir kasa seçin.

   Seçilen kasa panosu açılır.

   ![Kasa dikey penceresini açma](../backup/media/backup-azure-microsoft-azure-backup/vault-dashboard.png)

   **Ayarlar** varsayılan olarak açılır. Kapalıysa, açmak için **Ayarlar** ' ı seçin.

   ![Kasa dikey penceresini açma](../backup/media/backup-azure-microsoft-azure-backup/vault-setting.png)

1. Başlangıç Sihirbazı 'nı açmak için **Yedekle** ' ye tıklayın.

   ![Yedekleme başlangıç](../backup/media/backup-azure-microsoft-azure-backup/getting-started-backup.png)

1. Açılan pencerede şunları yapın:

   1. **İş yükünüzün çalıştığı konum** menüsünde **Şirket içi**' ı seçin.

      :::image type="content" source="media/avs-backup/deploy-mabs-on-premises-workload.png" alt-text="İş yükünüz nerede çalışıyor?":::

   1. **Ne yedeklemek** istiyorsunuz menüsünde, Azure Backup sunucusu kullanarak korumak istediğiniz iş yüklerini seçin.

   1. Azure Backup Sunucusu ve kasa kimlik bilgilerini indirmek ve yüklemek için **altyapıyı hazırla** ' ya tıklayın.

      :::image type="content" source="media/avs-backup/deploy-mabs-prepare-infrastructure.png" alt-text="Altyapıyı hazırla":::

1. Açılan **altyapıyı hazırla** penceresinde şunları yapın:

   1. Azure Backup Sunucusu yüklemek için **indirme** bağlantısına tıklayın.

   1. **Zaten Indirilmiş olan veya en son Azure Backup sunucusu yüklemesini** seçerek kasa kimlik bilgilerini indirin ve ardından **İndir**' e tıklayın. Kurtarma Hizmetleri kasasına Azure Backup Sunucusu kaydı sırasında kasa kimlik bilgilerini kullanırsınız. Bağlantılar sizi, yazılım paketini indirdiğiniz Indirme merkezi 'ne götürür.

   :::image type="content" source="media/avs-backup/deploy-mabs-prepare-infrastructure2.png" alt-text="Altyapıyı hazırla-Azure Backup Sunucusu":::

1. İndirme sayfasında, tüm dosyalar ' ı seçin ve **İleri**' ye tıklayın.

   > [!NOTE]
   > Tüm dosyaları aynı klasöre indirmeniz gerekir.  Dosyaların karşıdan yükleme boyutu > 3GB olduğundan, indirmenin tamamlanması 60 dakika sürebilir. 

   ![İndirme Merkezi 1](../backup/media/backup-azure-microsoft-azure-backup/downloadcenter.png)

### <a name="extracting-the-software-package"></a>Yazılım paketi ayıklanıyor

Yazılım paketini farklı bir sunucuya indirdiyseniz, dosyaları Azure Backup Sunucusu dağıtmak için oluşturduğunuz sanal makineye kopyalayın.

> [!WARNING]
> Kurulum dosyalarını ayıklamak için en az 4 GB boş alan gerekir.

1. Tüm dosyaları indirdikten sonra, **MicrosoftAzureBackupInstaller. exe** ' ye çift tıklayarak **Microsoft Azure Backup kurulum sihirbazını** açın ve ardından **İleri**' ye tıklayın.

1. Dosyaların ayıklanacağı konumu seçin ve **İleri**' ye tıklayın.

1. Ayıklama işlemini başlatmak için **Ayıkla** ' ya tıklayın.

   ![Microsoft Azure Backup Kurulum Sihirbazı](../backup/media/backup-azure-microsoft-azure-backup/extract/03.png)

1. Ayıklandıktan sonra **Setup. exe dosyasını yürütme** seçeneğini belirleyip **son**' a tıklayın.

> [!TIP]
> Ayrıca, yazılım paketini ayıkladığınız klasörden Setup. exe dosyasını da bulabilirsiniz.

### <a name="installing-the-software-package"></a>Yazılım paketini yükleme

1. Yükleme altındaki kurulum penceresinde, Kurulum Sihirbazı 'nı açmak için **Microsoft Azure Backup** ' a tıklayın.

   ![Microsoft Azure Backup Kurulum Sihirbazı](../backup/media/backup-azure-microsoft-azure-backup/launch-screen2.png)

1. Karşılama ekranında, önkoşul denetimlerine devam etmek için **İleri** ' ye tıklayın.

1. Azure Backup Sunucusu için donanım ve yazılım önkoşullarının karşılanıp karşılanmadığını öğrenmek için **Denetle** ' ye tıklayın. Başarıyla karşılanırsa, **İleri**' ye tıklayın.

   ![Azure Backup Sunucusu-hoş geldiniz ve önkoşul denetimi](../backup/media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)

1. Azure Backup Sunucusu yükleme paketi, gereken uygun SQL Server ikilileriyle birlikte paketlenmiştir. Yeni bir Azure Backup Sunucusu yüklemesi başlatırken, **Bu kurulum ile yeni SQL Server örneğini yükleme** seçeneğini belirleyin ve **Denetle ve yükleme**' ye tıklayın.

   ![Azure Backup Sunucusu-SQL denetimi](../backup/media/backup-azure-microsoft-azure-backup/sql/01.png)

   > [!NOTE]
   > Kendi SQL Server 'nizi kullanmak istiyorsanız desteklenen SQL Server sürümleri SQL Server 2014 SP1 veya üzeri, 2016 ve 2017. Tüm SQL Server sürümleri Standard veya Enterprise 64-bit olmalıdır. Azure Backup Sunucusu, uzak bir SQL Server örneğiyle birlikte çalışmaz. Azure Backup Sunucusu tarafından kullanılan örneğin yerel olması gerekir. Azure Backup Sunucusu için mevcut bir SQL Server kullanıyorsanız, kurulum yalnızca SQL Server 'ın *adlandırılmış örneklerinin* kullanılmasını destekler.

   Makineyi yeniden başlatma önerisiyle ilgili bir hata oluşursa, bunu yapın ve **yeniden denetle**' ye tıklayın. Herhangi bir SQL yapılandırma sorunu varsa, SQL yönergeleri başına SQL 'i yeniden yapılandırın ve var olan SQL örneğini kullanarak Azure Backup Sunucusu yüklemeyi/yükseltmeyi yeniden deneyin.

   **El ile yapılandırma**

   Kendi SQL örneğinizi kullandığınızda ana DB 'ye sysadmin rolüne BUILTIN\Administrators eklediğinizden emin olun.

   **SQL 2017 ile SSRS yapılandırması**

   Kendi SQL 2017 örneğinizi kullandığınızda SSRS 'yi el ile yapılandırmanız gerekir. SSRS yapılandırmasından sonra, SSRS 'nin *ısınbaşlatılmış* özelliğinin *true*olarak ayarlandığından emin olun. Bu true olarak ayarlandığında, MABS SSRS 'nin zaten yapılandırıldığını varsayar ve SSRS yapılandırmasını atlar.

   SSRS yapılandırma durumunu denetlemek için şu komutu çalıştırın:

   ```powershell
   $configset =Get-WmiObject –namespace 
   "root\Microsoft\SqlServer\ReportServer\RS_SSRS\v14\Admin" -class 
   MSReportServer_ConfigurationSetting -ComputerName localhost

   $configset.IsInitialized
   ```

   SSRS yapılandırması için aşağıdaki değerleri kullanın:
   * Hizmet hesabı: ' yerleşik hesabı kullan ' ağ hizmeti olmalıdır
   * Web hizmeti URL 'SI: ' sanal dizin ' ReportServer_ olmalıdır\<SQLInstanceName>
   * Veritabanı: DatabaseName ReportServer $ olmalıdır\<SQLInstanceName>
   * Web portalı URL 'SI: ' sanal dizin ' Reports_ olmalıdır\<SQLInstanceName>

   SSRS yapılandırması hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/sql/reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode?view=sql-server-2017) .

   > [!NOTE]
   > [Microsoft Online Services terms](https://www.microsoft.com/licensing/product-licensing/products) (OST), Azure Backup sunucusu veritabanı olarak kullanılan SQL Server lisansını yönetir. Azure Backup Sunucusu ile paketlenmiş SQL Server, OST 'ye göre yalnızca Azure Backup Sunucusu veritabanı olarak kullanılabilir.

1. Başarıyla yüklendikten sonra **İleri**' ye tıklayın.

1. Microsoft Azure Backup sunucusu dosyalarının yüklenmesi için bir konum belirtin ve **İleri**' ye tıklayın.

   > [!NOTE]
   > Karalama konumu Azure 'a yedekleme için gereklidir. Karalama konumunun, buluta yedeklenmek üzere planlandığı verilerin en az %5 ' i olduğundan emin olun. Disk koruması için, yükleme tamamlandıktan sonra ayrı disklerin yapılandırılması gerekir. Depolama havuzları hakkında daha fazla bilgi için bkz. [depolama havuzlarını ve Disk depolamayı yapılandırma](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/hh758075(v=sc.12)).

   ![Microsoft Azure Backup PreReq2](../backup/media/backup-azure-microsoft-azure-backup/space-screen.png)

1. Kısıtlanmış yerel kullanıcı hesapları için güçlü bir parola sağlayın ve **İleri**' ye tıklayın.

   ![Microsoft Azure Backup PreReq2](../backup/media/backup-azure-microsoft-azure-backup/security-screen.png)

1. Güncelleştirmeleri denetlemek için Microsoft Update kullanmak isteyip istemediğinizi seçin ve **İleri**' ye tıklayın.

   > [!NOTE]
   > Windows ve Microsoft Azure Backup Server gibi diğer ürünler için güvenlik ve önemli güncelleştirmeler sunan Microsoft Update Windows Update yeniden yönlendirmenizi öneririz.

   ![Microsoft Azure Backup PreReq2](../backup/media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)

1. *Ayarların özetini* gözden geçirin ve **yükler**' e tıklayın.

   Yükleme aşamalarda gerçekleşir. İlk aşamada Microsoft Azure Kurtarma Hizmetleri Aracısı ve ikinci aşama Internet bağlantısını denetler. Internet bağlantısı varsa, yükleme işlemine devam edebilirsiniz; Aksi takdirde, Internet 'e bağlanmak için proxy ayrıntılarını sağlamanız gerekir. Son aşama Önkoşul yazılımını denetler ve yüklü değilse, eksik yazılımlar Microsoft Azure Kurtarma Hizmetleri aracısıyla birlikte yüklenir.

1. Makineyi kurtarma hizmetleri kasasına kaydetmek için kasa kimlik bilgilerini bulmak üzere **Araştır** ' a tıklayın ve ardından **İleri**' ye tıklayın.

1. Azure ile şirket içi arasında gönderilen verileri şifrelemek/şifrelerini çözmek için bir parola seçin.

   > [!TIP]
   > Otomatik olarak bir parola oluşturabilir veya en düşük 16 karakterlik parolanızı sağlayabilirsiniz.

1. Parolayı kaydetmek için konumu girin ve ardından sunucuyu kaydetmek için **İleri** ' ye tıklayın.

   > [!IMPORTANT]
   > Parolayı yerel sunucu dışında bir güvenli konuma kaydetmek de önemlidir. Microsoft, parolayı depolamak için bir Azure Key Vault kullanmayı kesinlikle önerir.

   Microsoft Azure Kurtarma Hizmetleri Aracısı kurulumu tamamlandıktan sonra, SQL Server ve Azure Backup Sunucusu bileşenleri için yükleme ve yapılandırma üzerine gider.

   ![Azure Backup Sunucusu](../backup/media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

1. Yükleme adımı tamamlandıktan sonra **Kapat**' a tıklayın.

### <a name="install-update-rollup-1"></a>Güncelleştirme paketi 1 ' i yükler

Microsoft Azure Backup Server v3 için güncelleştirme paketi 1 ' in yüklenmesi iş yüklerini korumadan önce zorunludur. Hata düzeltmelerinin listesini ve Microsoft Azure Backup Server v3 UR1 için yükleme yönergelerini görüntülemek için bkz. KB makalesi [4534062](https://support.microsoft.com/en-us/help/4534062/).

## <a name="add-storage-to-azure-backup-server"></a>Azure Backup Sunucusu’na depolama alanı ekleme

Azure Backup Sunucusu v3 şunları sağlayan Modern Yedekleme Alanı destekler:

-  %50 depolama tasarrufu

-  Üç kat daha hızlı yedeklemeler

-  Daha verimli depolama

-  İş yüküne duyarlı depolama

### <a name="volumes-in-backup-server"></a>Yedekleme sunucusundaki birimler

Daha önce eklenmemişse, gerekli depolama kapasitesine sahip veri disklerini Azure Backup sunucusu sanal makinesine ekleyin.

Backup Server v3 yalnızca depolama birimlerini kabul eder. Bir birim eklediğinizde, Backup Server, birimi gereken Modern Yedekleme Alanı dayanıklı dosya sistemi (ReFS) olarak biçimlendirir.

### <a name="add-volumes-to-backup-server-disk-storage"></a>Yedekleme sunucusuna birim ekleme disk depolama

1. **Yönetim** bölmesinde, depolamayı yeniden tarayın ve **Ekle**' yi seçin. 

1. Depolama havuzuna eklemek için kullanılabilir birimler arasından seçim yapın. 

1. Kullanılabilir birimleri ekledikten sonra, bunları yönetmenize yardımcı olacak kolay bir ad verin. 

1. Bu birimleri ReFS olarak biçimlendirmek için **Tamam** ' a tıklayın, böylece yedekleme sunucusu modern yedekleme alanı avantajlarını kullanabilir.

![Kullanılabilir birimleri ekleme](../backup/media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="next-steps"></a>Sonraki Adımlar

Azure Backup Sunucusu kullanarak Azure VMware çözümünde (AVS) çalışan VMware VM 'lerinin yedeklenmesini yapılandırma hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [AVS VM 'lerinin yedeklemesini yapılandırma](backup-avs-vms-with-mabs.md)


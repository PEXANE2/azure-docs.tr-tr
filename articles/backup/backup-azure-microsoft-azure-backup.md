---
title: İş yüklerini yedeklemek için Azure Yedekleme Sunucusu'nı kullanma
description: Bu makalede, Microsoft Azure Yedekleme Sunucusu 'nı (MABS) kullanarak iş yüklerini korumak ve yedeklemek için ortamınızı nasıl hazırlayacağınızı öğrenin.
ms.topic: conceptual
ms.date: 11/13/2018
ms.openlocfilehash: dd506668f9d75523ff7494bccb2979bf0785990d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273416"
---
# <a name="install-and-upgrade-azure-backup-server"></a>Azure Yedekleme Sunucusu'nu yükleme ve yükseltme

> [!div class="op_single_selector"]
>
> * [Azure Backup Sunucusu](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
>
>

> Geçerlidir: MABS v3. (MABS v2 artık desteklenmez. MABS v3'ten daha önce bir sürüm kullanıyorsanız, lütfen en son sürüme yükseltin.)

Bu makalede, Microsoft Azure Yedekleme Sunucusu 'nı (MABS) kullanarak iş yüklerini yedeklemek için ortamınızı nasıl hazırlayacağınızı açıklanmaktadır. Azure Yedekleme Sunucusu ile Hyper-V VM,Microsoft SQL Server, SharePoint Server, Microsoft Exchange ve Windows istemcileri gibi uygulama iş yüklerini tek bir konsoldan koruyabilirsiniz.

> [!NOTE]
> Azure Yedekleme Sunucusu artık VMware VM'leri koruyabilir ve gelişmiş güvenlik özellikleri sağlar. Ürünü aşağıdaki bölümlerde ve en son Azure Yedekleme Aracısı'nda açıklandığı şekilde yükleyin. Azure Yedekleme Sunucusu ile VMware sunucularını yedekleme hakkında daha fazla bilgi edinmek için makaleye bakın, [Bir VMware sunucusunu yedeklemek için Azure Yedekleme Sunucusu'nu kullanın.](backup-azure-backup-server-vmware.md) Güvenlik özellikleri hakkında bilgi edinmek için [Azure yedekleme güvenlik özellikleri belgelerine](backup-azure-security-feature.md)bakın.
>
>

Azure VM'de dağıtılan MABS, Azure'da VM'leri yedekleyebilir, ancak yedekleme işlemini etkinleştirmek için aynı etki alanında olmaları gerekir. Azure VM'yi destekleme işlemi, şirket içinde VM'leri yedeklemeyle aynı kalır, ancak Azure'da MABS'yi dağıtmanın bazı sınırlamaları vardır. Sınırlama hakkında daha fazla bilgi için [DPM'yi Azure sanal makinesi olarak](https://docs.microsoft.com/system-center/dpm/install-dpm?view=sc-dpm-1807#setup-prerequisites) görün

> [!NOTE]
> Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki dağıtım modeli vardır: [Kaynak Yöneticisi ve klasik.](../azure-resource-manager/management/deployment-models.md) Bu makalede, Kaynak Yöneticisi modelini kullanarak dağıtılan VM'leri geri alma bilgileri ve yordamları sağlanır.
>
>

Azure Yedekleme Sunucusu, iş yükü yedekleme işlevinin çoğunu Veri Koruma Yöneticisi'nden (DPM) devralır. Bu makalede, paylaşılan işlevlerin bazılarını açıklamak için DPM belgelerine bağlanır. Azure Yedekleme Sunucusu DPM ile aynı işlevselliğin çoğunu paylaşsa da, Azure Yedekleme Sunucusu teyp yedeklemez ve System Center ile tümleştirmeyapmaz.

## <a name="choose-an-installation-platform"></a>Yükleme platformu seçin

Azure Yedekleme Sunucusu'nu çalışır hale getirmek için ilk adım bir Windows Server ayarlamaktır. Sunucunuz Azure'da veya şirket içinde olabilir.

### <a name="using-a-server-in-azure"></a>Azure'da sunucu kullanma

Azure Yedekleme Sunucusu'nun çalıştırılmak üzere bir sunucu seçerken, Windows Server 2016 Datacenter veya Windows Server 2019 Datacenter'ın galeri görüntüsüyle başlamanız önerilir. Makale, [Azure portalında ilk Windows sanal makinenizi oluşturun](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), Azure'da önerilen sanal makineye başlamak için bir öğretici sağlar, daha önce Azure'u hiç kullanmamış olsanız bile. Sunucu sanal makine (VM) için önerilen minimum gereksinimleri olmalıdır: Standard_A4_v2 dört çekirdek ve 8 GB RAM ile.

Azure Yedekleme Sunucusu ile iş yüklerini korumanın birçok nüansı vardır. Makale, [Bir Azure sanal makine olarak DPM yükleyin,](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/jj852163(v=sc.12))bu nüansları açıklamaya yardımcı olur. Makineyi dağıtmadan önce bu makaleyi tamamen okuyun.

### <a name="using-an-on-premises-server"></a>Şirket içi sunucu kullanma

Temel sunucuyu Azure'da çalıştırmak istemiyorsanız, sunucuyu Hyper-V VM, VMware VM veya fiziksel ana bilgisayarda çalıştırabilirsiniz. Sunucu donanımı için önerilen minimum gereksinimler iki çekirdek ve 8 GB RAM'dir. Desteklenen işletim sistemleri aşağıdaki tabloda listelenmiştir:

| İşletim Sistemi | Platform | SKU |
|:--- | --- |:--- |
| Windows Server 2019 |64 bit |Standard, Datacenter, Essentials |
| Windows Server 2016 ve en yeni SP'ler |64 bit |Standard, Datacenter, Essentials  |

Windows Server Deduplication kullanarak DPM depolama sını devre kapatabilirsiniz. Hyper-V VM'lerde dağıtıldığında [DPM ve deduplication'ın](https://docs.microsoft.com/system-center/dpm/deduplicate-dpm-storage?view=sc-dpm-2019) birlikte nasıl çalıştığı hakkında daha fazla bilgi edinin.

> [!NOTE]
> Azure Yedekleme Sunucusu, özel, tek amaçlı bir sunucuda çalışacak şekilde tasarlanmıştır. Azure Yedekleme Sunucusu'nu şu şekilde yükleyemezsiniz:
>
> * Etki alanı denetleyicisi olarak çalıştırılan bir bilgisayar
> * Uygulama Sunucusu rolünün yüklü olduğu bir bilgisayar
> * System Center Operations Manager yönetim grubu olan bir bilgisayar
> * Exchange Server’ın çalıştırıldığı bir bilgisayar
> * Küme düğümü olan bir bilgisayar
>
> Azure Yedekleme Sunucusu'nun yüklenmesi Windows Server Core veya Microsoft Hyper-V Server'da desteklenmez.

Azure Backup Server'a her zaman bir etki alanına katılın. Sunucuyu farklı bir etki alanına taşımayı planlıyorsanız, önce Azure Yedekleme Sunucusu'nu yükleyin, ardından sunucuyu yeni etki alanına birleştirin. Varolan bir Azure Yedekleme Sunucusu makinesini dağıtımdan sonra yeni bir etki alanına taşıma *desteklenmez.*

İster Azure'a yedek veri gönderin, ister yerel olarak tutun, Azure Yedekleme Sunucusu'nun bir Kurtarma Hizmetleri kasasına kayıtlı olması gerekir.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

### <a name="set-storage-replication"></a>Depolama Çoğaltmayı Ayarlama

Depolama çoğaltma seçeneği, coğrafi olarak yedekli depolama ve yerel olarak yedekli depolama arasında seçim yapmanıza olanak sağlar. Varsayılan olarak, Kurtarma Hizmetleri kasaları coğrafi yedekli depolama kullanır. Bu kasa birincil kasanızsa, depolama seçeneğini coğrafi yedekli depolama ya da depolama ya da depolama ya da depolama alanına bırakın. Daha düşük dayanıklılık düzeyinde olan daha uygun maliyetli bir seçenek istiyorsanız yerel olarak yedekli depolamayı seçin. [Azure Depolama çoğaltma genel bakışında](../storage/common/storage-redundancy.md)coğrafi [yedekli](../storage/common/storage-redundancy-grs.md) ve [yerel olarak gereksiz](../storage/common/storage-redundancy-lrs.md) depolama seçenekleri hakkında daha fazla bilgi edinin.

Depolama çoğaltma ayarını düzenlemek için:

1. **Kurtarma Hizmetleri kasaları** dikey penceresinden yeni kasaya tıklayın. **Ayarlar** bölümünün altında **Özellikler'i**tıklatın.
2. **Özellikler'de**, **Yedekleme Yapılandırması**altında, **Güncelleştir'i**tıklatın.

3. Depolama çoğaltma türünü seçin ve **Kaydet'i**tıklatın.

     ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

## <a name="software-package"></a>Yazılım paketi

### <a name="downloading-the-software-package"></a>Yazılım paketini indirme

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. Zaten bir Kurtarma Hizmetleri kasası açıksa, adım 3'e geçin. Kurtarma Hizmetleri kasası açık değilse, ancak Azure portalında, ana menüde **Gözat'ı**tıklatın.

   * Kaynak listesinde **Kurtarma Hizmetleri** yazın.
   * Yazmaya başladığınızda liste, girdinize göre filtrelenir. **Kurtarma Hizmetleri kasaları** seçeneğini gördüğünüzde buna tıklayın.

     ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-azure-microsoft-azure-backup/open-recovery-services-vault.png)

     Kurtarma Hizmetleri kasalarının listesi görünür.
   * Kurtarma Hizmetleri kasalarının listesinden bir kasa seçin.

     Seçilen kasa panosu açılır.

     ![Kasa dikey penceresini açma](./media/backup-azure-microsoft-azure-backup/vault-dashboard.png)
3. **Ayarlar** bıçağı varsayılan olarak açılır. Kapalıysa, ayarlar bıçağını açmak için **Ayarlar'a** tıklayın.

    ![Kasa dikey penceresini açma](./media/backup-azure-microsoft-azure-backup/vault-setting.png)
4. Başlarken sihirbazını açmak için **Yedekleme'yi** tıklatın.

    ![Yedekleme başlıyor](./media/backup-azure-microsoft-azure-backup/getting-started-backup.png)

    Açılan yedek bıçakla **Başlarken'de** **Yedekleme Hedefleri** otomatik olarak seçilecektir.

    ![Yedekleme-hedefler-varsayılan-açıldı](./media/backup-azure-microsoft-azure-backup/getting-started.png)

5. Yedekleme **Hedefi** bıçağında, **iş yükünüz nerede çalıştırıyor** menüsünden **şirket içi'ni**seçin.

    ![hedef olarak şirket içi ve iş yükleri](./media/backup-azure-microsoft-azure-backup/backup-goals-azure-backup-server.png)

    **Yedeklemek istediğiniz nedir?** açılır menüden Azure Yedekleme Sunucusu'nu kullanarak korumak istediğiniz iş yüklerini seçin ve **ardından Tamam'ı**tıklatın.

    **Yedekleme sihirbazıyla Başlarken,** iş yüklerini Azure'a yedeklemek için **altyapı hazırlama** seçeneğini değiştirir.

   > [!NOTE]
   > Yalnızca dosya ve klasörleri yedeklemek istiyorsanız, Azure Yedekleme aracısını kullanmanızı ve makaledeki kılavuzu izlemenizi öneririz, [İlk bakış: dosyaları ve klasörleri yedekleyin.](backup-try-azure-backup-in-10-mins.md) Dosya ve klasörlerden daha fazlasını koruyacaksanız veya gelecekte koruma gereksinimlerini genişletmeyi planlıyorsanız, bu iş yüklerini seçin.
   >
   >

    ![Başlangıç sihirbazı değişikliği başlarken](./media/backup-azure-microsoft-azure-backup/getting-started-prep-infra.png)

6. Açılan **AltyapıYı Hazırla** çubuğunda, Azure Yedekleme Sunucusu Yükle ve Kasa Kimlik Bilgilerini İndir için **İndir** bağlantılarını tıklatın. Azure Yedekleme Sunucusu'nun kurtarma hizmetleri kasasına kaydedilmesi sırasında kasa kimlik bilgilerini kullanırsınız. Bağlantılar sizi yazılım paketinin indirilebileceği İndirme Merkezi'ne götürür.

    ![Azure Yedekleme Sunucusu için altyapı hazırlama](./media/backup-azure-microsoft-azure-backup/azure-backup-server-prep-infra.png)

7. Tüm dosyaları seçin ve **İleri'yi**tıklatın. Microsoft Azure Yedekleme indirme sayfasından gelen tüm dosyaları indirin ve tüm dosyaları aynı klasöre yerleştirin.

    ![İndirme merkezi 1](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    Birlikte tüm dosyaların indirme boyutu 3G > olduğundan, 10 Mbps indirme bağlantısı nda indirme tamamlamak için 60 dakika kadar sürebilir.

### <a name="extracting-the-software-package"></a>Yazılım paketinin ayıklanması

Tüm dosyaları indirdikten sonra **MicrosoftAzureBackupInstaller.exe'yi**tıklatın. Bu, kurulum dosyalarını sizin belirlediğiniz bir konuma ayıklamak için **Microsoft Azure Yedekleme Kurulum Sihirbazı'nı** başlatır. Sihirbaz aracılığıyla devam edin ve çıkarma işlemine başlamak için **Ayıklama** düğmesine tıklayın.

> [!WARNING]
> Kurulum dosyalarını ayıklamak için en az 4 GB boş alan gerekir.
>
>

![Microsoft Azure Yedekleme Kurulumu Sihirbazı](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Çıkarma işlemi tamamlandıktan sonra, Microsoft Azure Yedekleme Sunucusu'nu yüklemeye başlamak için yeni çıkarılan *setup.exe'yi* başlatmak için kutuyu işaretleyin ve **Finish** düğmesine tıklayın.

### <a name="installing-the-software-package"></a>Yazılım paketinin yüklenmesi

1. Kurulum sihirbazını başlatmak için **Microsoft Azure Yedekleme'yi** tıklatın.

    ![Microsoft Azure Yedekleme Kurulumu Sihirbazı](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. Hoş Geldiniz ekranında **İleri** düğmesini tıklatın. Bu sizi *Önkoşul Denetimleri* bölümüne götürür. Bu ekranda, **Check** Azure Yedekleme Sunucusu'na ilişkin donanım ve yazılım ön koşullarının karşılanıp karşılanmadığını belirlemek için Denetle'yi tıklatın. Tüm ön koşullar başarıyla karşılanırsa, makinenin gereksinimleri karşıladığını belirten bir ileti görürsünüz. **İleri** düğmesine tıklayın.

    ![Azure Yedekleme Sunucusu - Hoş geldiniz ve Önkoşullar denetimi](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Azure Yedekleme Sunucusu yükleme paketi, gerekli uygun SQL Server ikilileriyle birlikte gelir. Yeni bir Azure Yedekleme Sunucusu yüklemesini başlatırken, **bu Kurulumla yeni SQL Server Örneği'ni yükley** seçeneğini seçin ve **Onayla ve Yükle** düğmesini tıklatın. Ön koşullar başarıyla yüklendikten sonra **İleri'yi**tıklatın.

    >[!NOTE]
    >Kendi SQL sunucunuzu kullanmak isterseniz, desteklenen SQL Server sürümleri SQL Server 2014 SP1 veya üzeri, 2016 ve 2017'dir.  Tüm SQL Server sürümleri Standart veya Enterprise 64 bit olmalıdır.
    >Azure Yedekleme Sunucusu uzak bir SQL Server örneğiyle çalışmaz. Azure Yedekleme Sunucusu tarafından kullanılan örneğin yerel olması gerekir. MABS için varolan bir SQL sunucusu kullanıyorsanız, MABS kurulumu yalnızca SQL sunucusunun *adlandırılmış örneklerinin* kullanımını destekler.

    ![Azure Yedekleme Sunucusu - SQL denetimi](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Makineyi yeniden başlatma önerisi yle bir hata oluşursa, bunu yapın ve **Yeniden Denetle'yi**tıklatın. Herhangi bir SQL yapılandırma sorunu varsa, SQL yönergelerine göre SQL'i yeniden yapılandırın ve varolan SQL örneğini kullanarak MABS'yi yükleme/yükseltmeyi yeniden deneyin.

   **El ile yapılandırma**

   Kendi SQL örneğini kullandığınızda, DB'de ustalaşmak için sysadmin rolüne yerleşik yöneticiler eklediğinizden emin olun.

    **SQL 2017 ile SSRS Yapılandırması**

    Kendi SQL 2017 örneğini kullanırken, SSRS'yi el ile yapılandırmanız gerekir. SSRS yapılandırması ndan sonra, SSRS'nin *Başlatılması* gereken özelliğinin *True*olarak ayarlandığından emin olun. Bu True olarak ayarlandığında, MABS SSRS'nin zaten yapılandırıldığını ve SSRS yapılandırmasını atladığını varsayar.

    SSRS yapılandırması için aşağıdaki değerleri kullanın:
    * Hizmet Hesabı: 'Yerleşik hesabı kullanın' Ağ Hizmeti olmalıdır
    * Web Hizmeti URL'si: 'Sanal\<Dizin' SQLInstanceName> ReportServer_ olmalıdır
    * Veritabanı: DatabaseName ReportServer$\<SQLInstanceName> olmalıdır
    * Web Portalı URL'si: 'Sanal\<Dizin' SQLInstanceName> Reports_ olmalıdır

    SSRS yapılandırması hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/sql/reporting-services/report-server/configure-and-administer-a-report-server-ssrs-native-mode?view=sql-server-2017)

    > [!NOTE]
    > MABS veritabanı olarak kullanılan SQL Server [lisansı, Microsoft Çevrimiçi Hizmet Koşulları](https://www.microsoft.com/licensing/product-licensing/products) 'na (OST) tabidir. OST'ye göre, MABS ile birlikte verilen SQL Server yalnızca MABS veritabanı olarak kullanılabilir.

4. Microsoft Azure Yedekleme sunucu dosyalarının yüklenmesi için bir konum sağlayın ve **İleri'yi**tıklatın.

    ![Microsoft Azure Yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    Sıfırdan çizilme konumu, Azure'a yedeklemenin bir gereğidir. Kazı kazan konumunun buluta yedeklemesi planlanan verilerin en az %5'i olduğundan emin olun. Disk koruması için, yükleme tamamlandıktan sonra ayrı disklerin yapılandırılması gerekir. Depolama havuzları ile ilgili daha fazla bilgi için [bkz.](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/hh758075(v=sc.12))
5. Kısıtlı yerel kullanıcı hesapları için güçlü bir parola sağlayın ve **İleri'yi**tıklatın.

    ![Microsoft Azure Yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Güncelleştirmeleri denetlemek için *Microsoft Update'i* kullanmak isteyip istemediğinizi seçin ve **İleri'yi**tıklatın.

   > [!NOTE]
   > Windows Update'in, Windows ve Microsoft Azure Yedekleme Sunucusu gibi diğer ürünler için güvenlik ve önemli güncelleştirmeler sunan Microsoft Update'e yönlendirmesini öneririz.
   >
   >

    ![Microsoft Azure Yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. *Ayarların Özetini* gözden geçirin ve **Yükle'yi**tıklatın.

    ![Microsoft Azure Yedekleme PreReq2](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. Yükleme aşamalarında gerçekleşir. İlk aşamada, Microsoft Azure Kurtarma Hizmetleri Aracısı sunucuya yüklenir. Sihirbaz, Internet bağlantısını da denetler. Internet bağlantısı varsa, yüklemeye devam edebilirsiniz, değilse, Internet'e bağlanmak için proxy ayrıntıları sağlamanız gerekir.

    Bir sonraki adım, Microsoft Azure Kurtarma Hizmetleri Aracısını yapılandırmaktır. Yapılandırmanın bir parçası olarak, makineyi kurtarma hizmetleri kasasına kaydettirmek için kasa kimlik bilgilerinizi sağlamanız gerekir. Ayrıca, Azure ile tesisleriniz arasında gönderilen verileri şifrelemek/şifresini çözmek için bir parola öbeği de sağlarsınız. Otomatik olarak bir parola oluşturabilir veya kendi en az 16 karakterli parolanızı sağlayabilirsiniz. Aracı yapılandırılana kadar sihirbazla devam edin.

    ![Azure Yedekleme Sunucusu PreReq2](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. Microsoft Azure Yedekleme sunucusunun kaydı başarıyla tamamlandıktan sonra, genel kurulum sihirbazı SQL Server ve Azure Yedekleme Sunucusu bileşenlerinin yüklenmesine ve yapılandırmasına geçer. SQL Server bileşen yüklemesi tamamlandıktan sonra Azure Yedekleme Sunucusu bileşenleri yüklenir.

    ![Azure Backup Sunucusu](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

Yükleme adımı tamamlandığında, ürünün masaüstü simgeleri de oluşturulmuş olur. Ürünü başlatmak için simgeyi çift tıklatmanız yeterlidir.

### <a name="add-backup-storage"></a>Yedekleme depolama alanı ekleme

İlk yedek kopya Azure Yedekleme Sunucusu makinesine bağlı depolama da tutulur. Disk ekleme hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/backup/backup-mabs-add-storage)

> [!NOTE]
> Azure'a veri göndermeyi planlıyorsanız bile yedekleme depolama alanı eklemeniz gerekir. Azure Yedekleme Sunucusu'nun geçerli mimarisinde, Azure Yedekleme kasası verilerin *ikinci* kopyasını tutarken, yerel depolama alanı ilk (ve zorunlu) yedekleme kopyasını tutar.
>
>

### <a name="install-and-update-the-data-protection-manager-protection-agent"></a>Veri Koruma Yöneticisi koruma aracısını yükleme ve güncelleştirme

MABS, Sistem Merkezi Veri Koruma Yöneticisi koruma aracısını kullanır. Koruma Aracısını Koruma Sunucularınıza yükleme [adımları aşağıda veda edebilirsiniz.](https://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent?view=sc-dpm-1807)

Aşağıdaki bölümlerde istemci bilgisayarlar için koruma aracıları nasıl güncelleştirilen açıklanmıştır.

1. Yedekleme Sunucusu Yönetici Konsolunda **Yönetim** > **Aracıları'nı**seçin.

2. Görüntü bölmesinde, koruma aracısını güncelleştirmek istediğiniz istemci bilgisayarları seçin.

   > [!NOTE]
   > **Aracı Güncelleştirmeleri** sütunu, koruma altındaki her bilgisayar için bir koruma aracısı güncelleştirmesinin ne zaman kullanılabilerolduğunu gösterir. **Eylemler** bölmesinde, **Güncelleştirme** eylemi yalnızca korumalı bir bilgisayar seçildiğinde ve güncelleştirmeler kullanılabilir olduğunda kullanılabilir.
   >
   >

3. Seçili bilgisayarlara güncelleştirilmiş koruma aracıları yüklemek **için, Eylemler** bölmesine **Güncelleştir'i**seçin.

4. Ağa bağlı olmayan bir istemci bilgisayar için, bilgisayar ağa bağlanana kadar, **Aracı Durumu** sütunu **Bekleyen Güncelleştirme**durumunu gösterir.

   İstemci bilgisayar ağa bağlandıktan sonra, istemci bilgisayarın **Aracı Güncelleştirmeleri** sütunu **güncelleştirme**durumunu gösterir.

## <a name="move-mabs-to-a-new-server"></a>MABS'yi yeni bir sunucuya taşıma

Depolama alanını korurken MABS'yi yeni bir sunucuya taşımanız gerekiyorsa, aşağıdaki adımlar aşağıda veda edebilirsiniz. Bu, yalnızca tüm veriler Modern Yedekleme Depolama'da ysa yapılabilir.

  > [!IMPORTANT]
  >
  > * Yeni sunucu adı, özgün Azure Yedekleme Sunucusu örneğiyle aynı adolmalıdır. Kurtarma noktalarını korumak için önceki depolama havuzunu ve MABS Veritabanını (DPMDB) kullanmak istiyorsanız, yeni Azure Yedekleme Sunucusu örneğinin adını değiştiremezsiniz.
  > * MABS Veritabanı (DPMDB) yedeğine sahip olmalısınız. Veritabanını geri yüklemeniz gerekir.

1. Görüntü bölmesinde, koruma aracısını güncelleştirmek istediğiniz istemci bilgisayarları seçin.
2. Orijinal Azure yedekleme sunucusunu kapatın veya kablodan çıkarın.
3. Makine hesabını etkin dizinde sıfırla.
4. Server 2016'yı yeni makineye yükleyin ve orijinal Azure Yedekleme sunucusuyla aynı makine adını adlandırın.
5. Etki Alanına Katılın
6. Azure Yedekleme sunucusu V3 veya daha sonra yükleme (MABS Depolama havuzu disklerini eski sunucudan taşıyın ve içe aktarın)
7. Adım 1'de alınan DPMDB'yi geri yükleyin.
8. Depolama alanını orijinal yedekleme sunucusundan yeni sunucuya takın.
9. SQL'den DPMDB'yi Geri Yükleme
10. Yeni sunucu cd'sindeki yönetici komut satırından Microsoft Azure Yedekleme yükleme konumu ve çöp kutusu klasörüne kadar

    Yol örneği: C:\windows\system32>cd "c:\Program Files\Microsoft Azure Yedekleme\DPM\DPM\bin\"

11. Azure yedeklemesine, DPMSYNC -SYNC'i çalıştırın

    Eskidiskleri taşımak yerine DPM Depolama havuzuna Yenİ diskler eklediyseniz, DPMSYNC -Reallocatereplica'ı çalıştırın

## <a name="network-connectivity"></a>Ağ bağlantısı

Azure Yedekleme Sunucusu, ürünün başarılı bir şekilde çalışması için Azure Yedekleme hizmetine bağlantı gerektirir. Makinenin Azure bağlantısına sahip olup olmadığını ```Get-DPMCloudConnection``` doğrulamak için Azure Yedekleme Sunucusu PowerShell konsolundaki cmdlet'i kullanın. Cmdlet çıkışı TRUE ise, o zaman bağlantı var, aksi takdirde bağlantı yoktur.

Aynı zamanda Azure aboneliğinin sağlıklı bir durumda olması gerekir. Aboneliğinizin durumunu öğrenmek ve yönetmek için [abonelik portalında](https://account.windowsazure.com/Subscriptions)oturum açın.

Azure bağlantısının ve Azure aboneliğinin durumunu öğrendikten sonra, sunulan yedekleme/geri yükleme işlevleri üzerindeki etkisini öğrenmek için aşağıdaki tabloyu kullanabilirsiniz.

| Bağlantı Durumu | Azure Aboneliği | Azure'a yedekleme | Diske yedekleme | Azure'dan geri yükleme | Diskten geri yükleme |
| --- | --- | --- | --- | --- | --- |
| Bağlı |Etkin |İzin Verildi |İzin Verildi |İzin Verildi |İzin Verildi |
| Bağlı |Süresi doldu |Durduruldu |Durduruldu |İzin Verildi |İzin Verildi |
| Bağlı |Hükmün iptali |Durduruldu |Durduruldu |Durduruldu ve Azure kurtarma noktaları silindi |Durduruldu |
| Kayıp bağlantı > 15 gün |Etkin |Durduruldu |Durduruldu |İzin Verildi |İzin Verildi |
| Kayıp bağlantı > 15 gün |Süresi doldu |Durduruldu |Durduruldu |İzin Verildi |İzin Verildi |
| Kayıp bağlantı > 15 gün |Hükmün iptali |Durduruldu |Durduruldu |Durduruldu ve Azure kurtarma noktaları silindi |Durduruldu |

### <a name="recovering-from-loss-of-connectivity"></a>Bağlantı kaybından kurtarma

Azure'a erişimi engelleyen bir güvenlik duvarınız veya proxy'niz varsa, güvenlik duvarı/proxy profilinde aşağıdaki etki alanı adreslerine izin verebilirsiniz:

* `http://www.msftncsi.com/ncsi.txt`
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

ExpressRoute Microsoft'a bakış kullanıyorsanız, lütfen aşağıdaki hizmetleri/bölgeleri seçin:

* Azure Etkin Dizin (12076:5060)
* Microsoft Azure Bölgesi (Kurtarma Hizmetleri kasanızın konumuna göre)
* Azure Depolama (Kurtarma Hizmetleri kasanızın konumuna göre)

Daha fazla bilgi için ExpressRoute yönlendirme gereksinimlerini ziyaret [edin.](https://docs.microsoft.com/azure/expressroute/expressroute-routing)

Azure bağlantısı Azure Yedekleme Sunucusu makinesine geri yüklendikten sonra, gerçekleştirilebilecek işlemler Azure abonelik durumu tarafından belirlenir. Yukarıdaki tabloda, makine "Bağlı" olduğunda izin verilen işlemlerle ilgili ayrıntılar vardır.

### <a name="handling-subscription-states"></a>Abonelik durumlarını işleme

Bir Azure aboneliğini *Süresi Dolmuş* veya Hükmün Kaldırılması durumundan *Etkin* durumuna almak *mümkündür.* Ancak, durum *Etkin*değilken bu ürün davranışı üzerinde bazı etkileri vardır:

* *Hükümden arındırılmış* bir abonelik, hükmün iptal edildiği dönem için işlevselliğini kaybeder. *Etkin'i*döndürüp, yedekleme/geri yükleme nin ürün işlevselliği yeniden canlanır. Yerel diskteki yedekleme verileri, yeterince büyük bir bekletme süresiyle tutulduğunda da alınabilir. Ancak, Azure'daki yedekleme verileri, abonelik *Deprovisioned* durumuna girdiğinde geri alınamaz şekilde kaybolur.
* *Süresi Dolmuş* bir abonelik yalnızca yeniden *Etkin* yapılana kadar işlevselliğini kaybeder. Aboneliğin *Süresi Dolduğu* dönem için zamanlanan yedeklemeler çalışmaz.

## <a name="upgrade-mabs"></a>Yükseltme MABS

MABS'yi yükseltmek için aşağıdaki yordamları kullanın.

### <a name="upgrade-from-mabs-v2-to-v3"></a>MABS V2'den V3'e yükseltme

> [!NOTE]
>
> MABS V2, MABS V3'ün yüklenmesi için bir ön koşul değildir. Ancak, MABS V3'e yalnızca MABS V2'den yükseltebilirsiniz.

MABS'yi yükseltmek için aşağıdaki adımları kullanın:

1. MABS V2'den MABS V3'e yükseltmek için işletim sisteminizi gerekirse Windows Server 2016 veya Windows Server 2019'a yükseltin.

2. Sunucunuzu yükseltin. Adımlar [yüklemeye](#install-and-upgrade-azure-backup-server)benzer. Ancak, SQL ayarları için, SQL örneğini SQL 2017'ye yükseltme veya kendi SQL server 2017 örneğini kullanma seçeneğiniz vardır.

   > [!NOTE]
   >
   > SQL örneğiniz yükseltilirken çıkmayın, çıkış SQL raporlama örneğini kaldıracaktır ve bu nedenle MABS'yi yeniden yükseltme girişimi başarısız olur.

   > [!IMPORTANT]
   >
   >  SQL 2017 yükseltmesinin bir parçası olarak, SQL şifreleme anahtarlarını yedekleyip raporlama hizmetlerini kaldırıyoruz. SQL sunucu yükseltmesi yapıldıktan sonra raporlama hizmeti (14.0.6827.4788) yüklenir & şifreleme anahtarları geri yüklenir.
   >
   > SQL 2017'yi el ile yapılandırırken, Yükleme yönergeleri altındaki *SQL 2017 bölümüne sahip SSRS yapılandırmasına* bakın.

3. Koruma aracılarını koruma sunucularında güncelleştirin.
4. Yedeklemeler, üretim sunucularınızı yeniden başlatmaya gerek kalmadan devam etmelidir.
5. Verilerinizi şimdi korumaya başlayabilirsiniz. Korurken Modern Yedekleme Depolama'ya yükseltiyorsanız, yedeklemeleri saklamak istediğiniz birimleri de seçebilir ve verilen alanın altında denetleyebilirsiniz. [Daha fazla bilgi edinin](backup-mabs-add-storage.md).

## <a name="troubleshooting"></a>Sorun giderme

Microsoft Azure Yedekleme sunucusu kurulum aşamasında (veya yedekleme veya geri yükleme) hatalarıyla başa çıkamazsa, daha fazla bilgi için bu [hata kodları belgesine](https://support.microsoft.com/kb/3041338) bakın.
[Azure Yedekleme ile ilgili SSS'lere](backup-azure-backup-faq.md) de başvurabilirsiniz

## <a name="next-steps"></a>Sonraki adımlar

[DPM için çevrenizi hazırlama](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-2019)hakkında detaylı bilgi buradan ulaşabilirsiniz. Ayrıca, Azure Yedekleme Sunucusu'nun dağıtılabildiği ve kullanılabildiği desteklenen yapılandırmalar hakkında bilgiler de içerir. Çeşitli işlemleri gerçekleştirmek için bir dizi [PowerShell cmdlet](https://docs.microsoft.com/powershell/module/dataprotectionmanager/?view=systemcenter-ps-2016) kullanabilirsiniz.

Microsoft Azure Yedekleme sunucusukullanarak iş yükü koruması hakkında daha derin bir bilgi edinmek için bu makaleleri kullanabilirsiniz.

* [SQL Server yedekleme](backup-azure-backup-sql.md)
* [SharePoint sunucu yedekleme](backup-azure-backup-sharepoint.md)
* [Alternatif sunucu yedekleme](backup-azure-alternate-dpm-server.md)

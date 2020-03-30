---
title: Azure Stack üzerinde Azure Backup Sunucusu'nu yükleme
description: Bu makalede, Azure Yığını'ndaki iş yüklerini korumak veya yedeklemek için Azure Yedekleme Sunucusu'nun nasıl kullanılacağını öğrenin.
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: b78e5a662bdcf23ad38cb33292658d4d2455e579
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77583444"
---
# <a name="install-azure-backup-server-on-azure-stack"></a>Azure Stack üzerinde Azure Backup Sunucusu'nu yükleme

Bu makalede, Azure Yedekleme Sunucusu'nun Azure Yığını'na nasıl yüklenir. Azure Yedekleme Sunucusu ile Azure Yığını'nda çalışan sanal makineler gibi Hizmet Olarak Altyapı (IaaS) iş yüklerini koruyabilirsiniz. İş yüklerinizi korumak için Azure Yedekleme Sunucusu'nun kullanılmasının bir avantajı, tüm iş yükü korumasını tek bir konsoldan yönetebilmektir.

> [!NOTE]
> Güvenlik özellikleri hakkında bilgi edinmek için [Azure Yedekleme güvenlik özellikleri belgelerine](backup-azure-security-feature.md)bakın.
>

## <a name="azure-backup-server-protection-matrix"></a>Azure Backup Sunucusu koruma matrisi

Azure Yedekleme Sunucusu aşağıdaki Azure Yığını sanal makine iş yüklerini korur.

| Korumalı veri kaynağı | Koruma ve kurtarma |
| --------------------- | ----------------------- |
| Windows Server Yarı Yıllık Kanal - Datacenter/Enterprise/Standard | Birimler, dosyalar, klasörler |
| Windows Server 2016 - Datacenter/Enterprise/Standard | Birimler, dosyalar, klasörler |
| Windows Server 2012 R2 - Veri Merkezi/Kurumsal/Standart | Birimler, dosyalar, klasörler |
| Windows Server 2012 - Veri Merkezi/Kurumsal/Standart | Birimler, dosyalar, klasörler |
| Windows Server 2008 R2 - Veri Merkezi/Kurumsal/Standart | Birimler, dosyalar, klasörler |
| SQL Server 2016 | Database |
| SQL Server 2014 | Database |
| SQL Server 2012 SP1 | Database |
| SharePoint 2016 | Çiftlik, veritabanı, frontend, web sunucusu |
| SharePoint 2013 | Çiftlik, veritabanı, frontend, web sunucusu |
| SharePoint 2010 | Çiftlik, veritabanı, frontend, web sunucusu |

## <a name="prerequisites-for-the-azure-backup-server-environment"></a>Azure Yedekleme Sunucusu ortamı için ön koşullar

Azure Yığını ortamınıza Azure Yedekleme Sunucusu yüklerken bu bölümdeki önerileri göz önünde bulundurun. Azure Yedekleme Sunucusu yükleyici, ortamınızın gerekli ön koşullara sahip olduğunu denetler, ancak yüklemeden önce hazırlayarak zamandan tasarruf edersiniz.

### <a name="determining-size-of-virtual-machine"></a>Sanal makinenin boyutunu belirleme

Azure Yedekleme Sunucusu'nu Azure Yığını sanal makinesinde çalıştırmak için A2 veya daha büyük boyutu kullanın. Sanal makine boyutu seçiminde yardım için [Azure Stack VM boyutu hesap makinesini](https://www.microsoft.com/download/details.aspx?id=56832)indirin.

### <a name="virtual-networks-on-azure-stack-virtual-machines"></a>Azure Yığını sanal makinelerde Sanal Ağlar

Azure Yığını iş yükünde kullanılan tüm sanal makinelerin aynı Azure sanal ağına ve Azure Aboneliği'ne ait olması gerekir.

### <a name="azure-backup-server-vm-performance"></a>Azure Yedekleme Sunucusu VM performansı

Diğer sanal makinelerle paylaşılırsa, depolama hesabı boyutu ve IOPS sınırları Azure Yedekleme Sunucusu VM performansını etkiler. Bu nedenle, Azure Yedekleme Sunucusu sanal makinesi için ayrı bir depolama hesabı kullanmanız gerekir. Azure Yedekleme Sunucusu'nda çalışan Azure Yedekleme aracısının aşağıdakiler için geçici depolama alanına ihtiyacı vardır:

- kendi kullanımı (önbellek konumu),
- buluttan geri yüklenen veriler (yerel evreleme alanı)

### <a name="configuring-azure-backup-temporary-disk-storage"></a>Azure Yedekleme geçici disk depolama yapılandırma

Her Azure Stack sanal makine, kullanıcıtarafından birim `D:\`olarak kullanılabilen geçici disk depolama ile birlikte gelir. Azure Yedekleme'nin ihtiyaç duyduğu yerel evreleme alanı `D:\`içinde kalacak şekilde yapılandırılabilir `C:\`ve önbellek konumu . Bu şekilde, Azure Yedekleme Sunucusu sanal makinesine bağlı veri disklerinden hiçbir depolama alanının oyılması na gerek yoktur.

### <a name="storing-backup-data-on-local-disk-and-in-azure"></a>Yedekleme verilerini yerel diskte ve Azure'da depolama

Azure Yedekleme Sunucusu, operasyonel kurtarma için yedekleme verilerini sanal makineye bağlı Azure disklerinde depolar. Diskler ve depolama alanı sanal makineye bağlandıktan sonra Azure Yedekleme Sunucusu depolama alanını sizin için yönetir. Yedekleme veri depolama miktarı, her [Azure Yığını sanal makinesine](/azure-stack/user/azure-stack-storage-overview)iliştirilen disklerin sayısına ve boyutuna bağlıdır. Azure Stack VM'nin her boyutu, sanal makineye bağlanabilen maksimum sayıda diske sahiptir. Örneğin, A2 dört disktir. A3 sekiz disktir. A4 16 disktir. Yine, disklerin boyutu ve sayısı toplam yedekleme depolama havuzubelirler.

> [!IMPORTANT]
> Azure **not** Yedekleme Sunucusu'na bağlı disklerde operasyonel kurtarma (yedekleme) verilerini beş günden uzun süre saklamamalısınız.
>

Yedekleme verilerini Azure'da depolamak, Azure Yığını'ndaki yedekleme altyapısını azaltır. Veriler beş günden eskiyse, Azure'da depolanmalıdır.

Yedekleme verilerini Azure'da depolamak için Bir Kurtarma Hizmetleri kasası oluşturun veya kullanın. Azure Yedekleme Sunucusu iş yükünü yedeklemeye hazırlanırken, [Kurtarma Hizmetleri kasasını yapılandırırsınız.](backup-azure-microsoft-azure-backup.md#create-a-recovery-services-vault) Yapılandırıldıktan sonra, yedekleme işi her çalıştığında kasada bir kurtarma noktası oluşturulur. Her Kurtarma Hizmetleri kasası 9999 kurtarma noktası tutar. Oluşturulan kurtarma noktalarının sayısına ve bunların ne kadar süreyle tutulduğuna bağlı olarak, yedekleme verilerini uzun yıllar saklayabilirsiniz. Örneğin, aylık kurtarma noktaları oluşturabilir ve bunları beş yıl boyunca saklayabilirsiniz.

### <a name="scaling-deployment"></a>DağıtımÖlçekleme

Dağıtımınızı ölçeklendirmek istiyorsanız, aşağıdaki seçeneklere sahipsiniz:

- Ölçeklendir - Azure Yedekleme Sunucusu sanal makinesinin boyutunu A serisinden D serisine artırın ve [Azure Stack sanal makine yönergeleri başına](/azure-stack/user/azure-stack-manage-vm-disks)yerel depolama alanını artırın.
- Verileri boşaltma - Eski verileri Azure'a gönderin ve yalnızca Azure Yedekleme Sunucusu'na bağlı depolama alanındaki en yeni verileri saklar.
- Ölçeklendir - İş yüklerini korumak için daha fazla Azure Yedekleme Sunucusu ekleyin.

### <a name="net-framework"></a>.NET Framework

.NET Framework 3.5 SP1 veya üstü sanal makineye takılmalıdır.

### <a name="joining-a-domain"></a>Etki alanına katılma

Azure Yedekleme Sunucusu sanal makinesi bir etki alanına birleştirilmelidir. Yönetici ayrıcalıklarına sahip bir etki alanı kullanıcısının sanal makineye Azure Yedekleme Sunucusu yüklemesi gerekir.

## <a name="using-an-iaas-vm-in-azure-stack"></a>Azure Yığını'nda IaaS VM kullanma

Azure Yedekleme Sunucusu için bir sunucu seçerken, Windows Server 2012 R2 Datacenter veya Windows Server 2016 Datacenter galeri görüntüsüyle başlayın. Makale, [Azure portalında ilk Windows sanal makine oluşturun](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), önerilen sanal makine ile başlamak için bir öğretici sağlar. Sunucu sanal makine (VM) için önerilen minimum gereksinimleri olmalıdır: İki çekirdek li A2 Standart ve 3.5 GB RAM.

Azure Yedekleme Sunucusu ile iş yüklerini korumanın birçok nüansı vardır. Makale, [Bir Azure sanal makine olarak DPM yükleyin,](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/jj852163(v=sc.12))bu nüansları açıklamaya yardımcı olur. Makineyi dağıtmadan önce bu makaleyi tamamen okuyun.

> [!NOTE]
> Azure Yedekleme Sunucusu, özel, tek amaçlı sanal bir makinede çalışacak şekilde tasarlanmıştır. Azure Yedekleme Sunucusu'nu şu şekilde yükleyemezsiniz:
>
> - Etki alanı denetleyicisi olarak çalıştırılan bir bilgisayar
> - Uygulama Sunucusu rolünün yüklü olduğu bir bilgisayar
> - Exchange Server’ın çalıştırıldığı bir bilgisayar
> - Küme düğümü olan bir bilgisayar

Azure Backup Server'a her zaman bir etki alanına katılın. Azure Yedekleme Sunucusu'nu farklı bir etki alanına taşımanız gerekiyorsa, önce Azure Yedekleme Sunucusu'nu yükleyin ve ardından yeni etki alanına katılın. Azure Yedekleme Sunucusu'na dağıttıktan sonra, onu yeni bir etki alanına taşıyamazsınız.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

### <a name="set-storage-replication"></a>Depolama Çoğaltmayı Ayarlama

Kurtarma Hizmetleri kasa depolama çoğaltma seçeneği, coğrafi yedekli depolama ve yerel olarak yedekli depolama arasında seçim yapmanızı sağlar. Varsayılan olarak, Kurtarma Hizmetleri kasaları coğrafi yedekli depolama kullanır. Bu kasa birincil kasanızsa, depolama seçeneğini coğrafi yedekli depolama ya da depolama ya da depolama ya da depolama alanına bırakın. Daha az dayanıklı daha ucuz bir seçenek istiyorsanız, yerel olarak yedekli depolama alanını seçin. [Azure Depolama çoğaltma genel bakışında](../storage/common/storage-redundancy.md)coğrafi [yedekli](../storage/common/storage-redundancy-grs.md) ve [yerel olarak gereksiz](../storage/common/storage-redundancy-lrs.md) depolama seçenekleri hakkında daha fazla bilgi edinin.

Depolama çoğaltma ayarını düzenlemek için:

1. Kasa panosunu ve Ayarlar menüsünü açmak için kasanızı seçin. **Ayarlar** menüsü açılmıyorsa, kasa panosundaki **Tüm ayarlar'ı** tıklatın.
2. **Ayarlar** menüsünde, **Yedekleme Yapılandırması** menüsünü açmak için **Yedekleme Altyapısı** > **Yedekleme Yapılandırması'nı** tıklatın. Yedekleme **Yapılandırması** menüsünde kasanız için depolama çoğaltma seçeneğini seçin.

    ![Yedekleme kasalarının listesi](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

## <a name="download-azure-backup-server-installer"></a>Azure Yedekleme Sunucusu yükleyicisi indirin

Azure Yedekleme Sunucusu yükleyicisini indirmenin iki yolu vardır. Azure Yedekleme Sunucusu yükleyicisini [Microsoft Download Center'dan](https://www.microsoft.com/download/details.aspx?id=55269)indirebilirsiniz. Kurtarma Hizmetleri kasasını yapılandırırken Azure Backup Server yükleyicisini de indirebilirsiniz. Aşağıdaki adımlar, Kurtarma Hizmetleri kasasını yapılandırırken yükleyiciyi Azure portalından indirmenize yol açar.

1. Azure Yığını sanal makinenizden Azure [portalında Azure aboneliğinizde oturum açın.](https://portal.azure.com/)
2. Sol menüde **Tüm Hizmetler'i**seçin.

    ![Ana menüdeki Tüm Hizmetler seçeneğini seçin](./media/backup-mabs-install-azure-stack/click-all-services.png)

3. Tüm **hizmetler** iletişim kutusunda Kurtarma *Hizmetleri*yazın. Yazmaya başladığınızda, girdiniz kaynak listesini filtreler. Bunu gördüğünüzde, **Kurtarma Hizmetleri kasalarını**seçin.

    ![Tüm hizmetler iletişim kutusunda Kurtarma Hizmetleri Yazın](./media/backup-mabs-install-azure-stack/all-services.png)

    Abonelikteki Kurtarma Hizmetleri kasalarının listesi görüntülenir.

4. Kurtarma Hizmetleri kasaları listesinden, panolarını açmak için kasanızı seçin.

    ![Tüm hizmetler iletişim kutusunda Kurtarma Hizmetleri Yazın](./media/backup-mabs-install-azure-stack/rs-vault-dashboard.png)

5. Kasanın Başlarken menüsünde, Başlarken sihirbazını açmak için **Yedekleme'yi** tıklatın.

    ![Yedekleme başlıyor](./media/backup-mabs-install-azure-stack/getting-started-backup.png)

    Yedek menü açılır.

    ![Yedekleme-hedefler-varsayılan-açıldı](./media/backup-mabs-install-azure-stack/getting-started-menu.png)

6. Yedekleme menüsünde, **iş yükünüz nerede çalıştırıyor** menüsünden **şirket içi'ni**seçin. **Yedeklemek istediğiniz nedir?** açılır menüden Azure Yedekleme Sunucusu'nu kullanarak korumak istediğiniz iş yüklerini seçin. Hangi iş yüklerini seçeceğiniz emin değilseniz, **Hyper-V Sanal Makineler'i** seçin ve ardından **Altyapı Yık'ı**tıklatın.

    ![hedef olarak şirket içi ve iş yükleri](./media/backup-mabs-install-azure-stack/getting-started-menu-onprem-hyperv.png)

    **Altyapı yı hazırla** menüsü açılır.

7. Altyapı **Yıkmenüsünde,** Azure Yedekleme Sunucusu yükleme dosyalarını indirmek için bir web sayfası açmak için **İndir'i** tıklatın.

    ![Başlangıç sihirbazı değişikliği başlarken](./media/backup-mabs-install-azure-stack/prepare-infrastructure.png)

    Azure Yedekleme Sunucusu için indirilebilir dosyaları barındıran Microsoft web sayfası açılır.

8. Microsoft Azure Yedekleme Sunucusu indirme sayfasında bir dil seçin ve **İndir'i**tıklatın.

    ![İndirme merkezi açılır](./media/backup-mabs-install-azure-stack/mabs-download-center-page.png)

9. Azure Yedekleme Sunucusu yükleyicisi sekiz dosyadan oluşur - bir yükleyici ve yedi .bin dosya. Gerekli tüm dosyaları seçmek için **Dosya Adı'nı** işaretleyin ve **İleri'yi**tıklatın. Tüm dosyaları aynı klasöre indirin.

    ![İndirme merkezi 1](./media/backup-mabs-install-azure-stack/download-center-selected-files.png)

    Tüm yükleme dosyalarının indirme boyutu 3 GB'tan büyüktür. 10 Mbps indirme bağlantısında, tüm yükleme dosyalarını indirme60 dakika kadar sürebilir. Dosyaları belirtilen indirme konumuna indirir.

## <a name="extract-azure-backup-server-install-files"></a>Azure Yedekleme Sunucusu yükleme dosyalarını ayıkla

Tüm dosyaları Azure Yığını sanal makinenize indirdikten sonra indirme konumuna gidin. Azure Yedekleme Sunucusu'nu yüklemenin ilk aşaması dosyaları ayıklamaktır.

![İndirme merkezi 1](./media/backup-mabs-install-azure-stack/download-mabs-installer.png)

1. Yüklemeyi başlatmak için indirilen dosyalar listesinden **MicrosoftAzureBackupserverInstaller.exe'yi**tıklatın.

    > [!WARNING]
    > Kurulum dosyalarını ayıklamak için en az 4 GB boş alan gerekir.
    >

2. Azure Yedekleme Sunucusu sihirbazında devam etmek için **İleri'yi** tıklatın.

    ![Microsoft Azure Yedekleme Kurulumu Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wiz-1.png)

3. Azure Yedekleme Sunucusu dosyalarının yolunu seçin ve **İleri'yi**tıklatın.

   ![Microsoft Azure Yedekleme Kurulumu Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-select-destination-1.png)

4. Çıkarma konumunu doğrulayın ve **Ayıkla'yı**tıklatın.

   ![Microsoft Azure Yedekleme Kurulumu Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-extract-2.png)

5. Sihirbaz dosyaları ayıklar ve yükleme işlemini hazırlar.

   ![Microsoft Azure Yedekleme Kurulumu Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-install-3.png)

6. Çıkarma işlemi tamamlandıktan sonra **Finish'i**tıklatın. Varsayılan olarak, **Execute setup.exe** seçilir. **Finish'i**tıklattığınızda, Setup.exe Microsoft Azure Yedekleme Sunucusu'nu belirtilen konuma yükler.

   ![Microsoft Azure Yedekleme Kurulumu Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-finish-4.png)

## <a name="install-the-software-package"></a>Yazılım paketini yükleyin

Önceki adımda, çıkarma aşamasından çıkmak ve Azure Yedekleme Sunucusu kurulum sihirbazını başlatmak için **Bitir'i** tıklattınız.

![Microsoft Azure Yedekleme Kurulumu Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-local-5.png)

Azure Backup Server, Verileri Koruma Yöneticisi ile kod paylaşır. Azure Yedekleme Sunucusu yükleyicisinde Veri Koruma Yöneticisi ve DPM'ye yapılan başvuruları görürsünüz. Azure Yedekleme Sunucusu ve Veri Koruma Yöneticisi ayrı ürünler olsa da, bu ürünler yakından ilişkilidir.

1. Kurulum sihirbazını başlatmak için **Microsoft Azure Yedekleme Sunucusu'nu**tıklatın.

   ![Microsoft Azure Yedekleme Kurulumu Sihirbazı](./media/backup-mabs-install-azure-stack/mabs-install-wizard-local-5b.png)

2. **Hoş Geldiniz** ekranında **İleri**’ye tıklayın.

    ![Azure Yedekleme Sunucusu - Hoş geldiniz ve Önkoşullar denetimi](./media/backup-mabs-install-azure-stack/mabs-install-wizard-setup-6.png)

3. **Önkoşul Denetimleri** ekranında, Azure Yedekleme Sunucusu'nun donanım ve yazılım ön koşullarının karşılanıp karşılanmadığını belirlemek için **Denetle'yi** tıklatın.

    ![Azure Yedekleme Sunucusu - Hoş geldiniz ve Önkoşullar denetimi](./media/backup-mabs-install-azure-stack/mabs-install-wizard-pre-check-7.png)

    Ortamınız gerekli ön koşullara sahipse, makinenin gereksinimleri karşıladığını belirten bir ileti görürsünüz. **İleri**'ye tıklayın.  

    ![Azure Yedekleme Sunucusu - Önkoşullar denetimi geçti](./media/backup-mabs-install-azure-stack/mabs-install-wizard-pre-check-passed-8.png)

    Ortamınız gerekli ön koşulları karşılamazsa, sorunlar belirtilir. Karşılanmayan ön koşullar DpmSetup.log'da da listelenir. Ön koşul hatalarını çözümle ve sonra **Yine Denetle'yi**çalıştırın. Tüm ön koşullar karşılanana kadar yükleme devam edemez.

    ![Azure Yedekleme Sunucusu - yükleme ön koşulları yerine getirilmedi](./media/backup-mabs-install-azure-stack/installation-errors.png)

4. Microsoft Azure Yedekleme Sunucusu SQL Server gerektirir. Azure Yedekleme Sunucusu yükleme paketi, uygun SQL Server ikilileriyle birlikte gelir. Kendi SQL yüklemenizi kullanmak istiyorsanız, kullanabilirsiniz. Ancak, önerilen seçenek yükleyici SQL Server yeni bir örnek eklemek izin verilir. Seçiminizin ortamınızla çalıştığından emin olmak için **Denetle ve Yükle'yi**tıklatın.

   > [!NOTE]
   > Azure Yedekleme Sunucusu uzak bir SQL Server örneğiyle çalışmaz. Azure Yedekleme Sunucusu tarafından kullanılan örnek yerel olmalıdır.
   >

    ![Azure Yedekleme Sunucusu - Hoş geldiniz ve Önkoşullar denetimi](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-install-9.png)

    Sanal makinenin Azure Yedekleme Sunucusu'nu yüklemek için gerekli ön koşullara sahip olup olmadığını kontrol ettikten sonra **İleri'yi**tıklatın.

    ![Azure Yedekleme Sunucusu - Hoş geldiniz ve Önkoşullar denetimi](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-ready-10.png)

    Makineyi yeniden başlatma önerisi yle bir hata oluşursa, makineyi yeniden başlatın. Makineyi yeniden başlattıktan sonra yükleyiciyi yeniden başlatın ve **SQL Ayarları** ekranına girdiğinizde **Yeniden Denetle'yi**tıklatın.

5. Yükleme **Ayarları'nda,** Microsoft Azure Yedekleme sunucu dosyalarının yüklenmesi için bir konum sağlayın ve **İleri'yi**tıklatın.

    ![Microsoft Azure Yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-settings-11.png)

    Sıfırdan çizilme konumu Azure'a yedeklemek için gereklidir. Çizilme konumunun boyutunun Azure'a yedeklemesi planlanan verilerin en az %5'ine eşit olduğundan emin olun. Disk koruması için, yükleme tamamlandıktan sonra ayrı disklerin yapılandırılması gerekir. Depolama havuzları ile ilgili daha fazla bilgi için [bkz.](https://docs.microsoft.com/previous-versions/system-center/system-center-2012-R2/hh758075(v=sc.12))

6. Güvenlik **Ayarları** ekranında, kısıtlı yerel kullanıcı hesapları için güçlü bir parola sağlayın ve **İleri'yi**tıklatın.

    ![Microsoft Azure Yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-security-12.png)

7. Microsoft **Update Opt-In** ekranında, güncelleştirmeleri denetlemek için *Microsoft Update'i* kullanmak isteyip istemediğiniz i seçin ve **İleri'yi**tıklatın.

   > [!NOTE]
   > Windows Update'in, Windows ve Microsoft Azure Yedekleme Sunucusu gibi diğer ürünler için güvenlik ve önemli güncelleştirmeler sunan Microsoft Update'e yönlendirmesini öneririz.
   >

    ![Microsoft Azure Yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-update-13.png)

8. *Ayarların Özetini* gözden geçirin ve **Yükle'yi**tıklatın.

    ![Microsoft Azure Yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-summary-14.png)

    Azure Yedekleme Sunucusu yüklemeyi bitirdiğinde, yükleyici hemen Microsoft Azure Kurtarma Hizmetleri aracısını başlatır.

9. Microsoft Azure Kurtarma Hizmetleri Aracısı yükleyici açılır ve Internet bağlantısını denetler. Internet bağlantısı varsa, yüklemeye devam edin. Bağlantı yoksa, Internet'e bağlanmak için proxy ayrıntıları sağlayın. Proxy ayarlarınızı belirttikten sonra **İleri'yi**tıklatın.

    ![Microsoft Azure Yedekleme PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-proxy-15.png)

10. Microsoft Azure Kurtarma Hizmetleri Aracısını yüklemek için **Yükle'yi**tıklatın.

    ![Azure Yedekleme Sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-mars-agent-16.png)

    Azure Yedekleme aracısı olarak da adlandırılan Microsoft Azure Kurtarma Hizmetleri aracısı, Azure Yedekleme Sunucusu'nu Kurtarma Hizmetleri kasasına yapılandırır. Yapılandırıldıktan sonra, Azure Yedekleme Sunucusu verileri her zaman aynı Kurtarma Hizmetleri kasasına yedekler.

11. Microsoft Azure Kurtarma Hizmetleri aracısı yüklemeyi tamamladıktan sonra, bir sonraki aşamayı başlatmak için **İleri'yi** tıklatın: Azure Yedekleme Sunucusu'nu Kurtarma Hizmetleri kasasına kaydetme.

    ![Azure Yedekleme Sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-complete-16.png)

    Yükleyici **Register Server Sihirbazı**başlattı.

12. Azure aboneliğinize ve Kurtarma Hizmetleri kasanıza geçin. Altyapı **Hazırla** menüsünde kasa kimlik bilgilerini indirmek için **İndir'i** tıklatın. Adım 2'deki **İndir** düğmesi etkin değilse, düğmeyi etkinleştirmek için **zaten indirilen veya en son Azure Yedekleme Sunucusu yüklemesini kullanarak'ı** seçin. Kasa kimlik bilgileri indirmeleri depoladığınız konuma indirilir. Bir sonraki adım için ihtiyacınız olacağından bu konumun farkında olun.

    ![Azure Yedekleme Sunucusu PreReq2](./media/backup-mabs-install-azure-stack/download-mars-credentials-17.png)

13. Vault **Identification** menüsünde, Kurtarma Hizmetleri kasa kimlik bilgilerini bulmak için **Gözat'ı** tıklatın.

    ![Azure Yedekleme Sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-vault-id-18.png)

    Vault **Kimlik Bilgilerini Seç** iletişim kutusunda, indirme konumuna gidin, kasa kimlik bilgilerinizi seçin ve **Aç'ı**tıklatın.

    Kimlik bilgilerine giden yol Vault Identification menüsünde görünür. Şifreleme Ayarı'na ilerlemek için **İleri'yi** tıklatın.

14. Şifreleme **Ayarı** iletişim kutusunda, yedekleme şifrelemesi için bir parola ve parolayı depolamak için bir konum sağlayın ve **İleri'yi**tıklatın.

    ![Azure Yedekleme Sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-encryption-19.png)

    Kendi parolanızı sağlayabilir veya sizin için bir tane oluşturmak için parola oluşturucuyu kullanabilirsiniz. Parola sizindir ve Microsoft bu parolayı kaydetmez veya yönetmez. Bir felakete hazırlanmak için parola ifadenizi erişilebilir bir konuma kaydedin.

    **İleri'yi**tıklattığınızda, Azure Yedekleme Sunucusu Kurtarma Hizmetleri kasasına kaydedilir. Yükleyici SQL Server'ı ve Azure Yedekleme Sunucusu'nu yüklemeye devam ediyor.

    ![Azure Yedekleme Sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-sql-still-installing-20.png)

15. Yükleyici tamamlandığında Durum, tüm yazılımların başarıyla yüklendiğini gösterir.

    ![Azure Yedekleme Sunucusu PreReq2](./media/backup-mabs-install-azure-stack/mabs-install-wizard-done-22.png)

    Yükleme tamamlandığında, Sunucu masaüstünde Azure Yedekleme Sunucusu konsolu ve Azure Backup Server PowerShell simgeleri oluşturulur.

## <a name="add-backup-storage"></a>Yedekleme depolama alanı ekleme

İlk yedek kopya Azure Yedekleme Sunucusu makinesine bağlı depolama da tutulur. Disk ekleme hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/system-center/dpm/add-storage?view=sc-dpm-1801)

> [!NOTE]
> Azure'a veri göndermeyi planlıyorsanız bile yedekleme depolama alanı eklemeniz gerekir. Azure Yedekleme Sunucusu mimarisinde, Kurtarma Hizmetleri kasası verilerin *ikinci* kopyasını tutarken, yerel depolama alanı ilk (ve zorunlu) yedekleme kopyasını tutar.
>
>

## <a name="network-connectivity"></a>Ağ bağlantısı

Azure Yedekleme Sunucusu, ürünün başarılı bir şekilde çalışması için Azure Yedekleme hizmetine bağlantı gerektirir. Makinenin Azure bağlantısına sahip olup olmadığını ```Get-DPMCloudConnection``` doğrulamak için Azure Yedekleme Sunucusu PowerShell konsolundaki cmdlet'i kullanın. Cmdlet çıkışı TRUE ise, o zaman bağlantı var, aksi takdirde bağlantı yoktur.

Aynı zamanda Azure aboneliğinin sağlıklı bir durumda olması gerekir. Aboneliğinizin durumunu öğrenmek ve yönetmek için [abonelik portalında](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)oturum açın.

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

Bir güvenlik duvarı veya proxy Azure'a erişimi engelliyorsa, güvenlik duvarı/proxy profiline izin veren listedeki aşağıdaki etki alanı adreslerini ekleyin:

- `http://www.msftncsi.com/ncsi.txt`
- \*.Microsoft.com
- \*.WindowsAzure.com
- \*.microsoftonline.com
- \*.windows.net

Azure bağlantısı Azure Yedekleme Sunucusu'na geri yüklendikten sonra, Azure abonelik durumu gerçekleştirilebilecek işlemleri belirler. Sunucu **Bağlandıktan**sonra, kullanılabilir işlemleri görmek için [Ağ bağlantısındaki](backup-mabs-install-azure-stack.md#network-connectivity) tabloyu kullanın.

### <a name="handling-subscription-states"></a>Abonelik durumlarını işleme

Bir Azure aboneliğini *Süresi Dolmuş* veya Hükmün Kaldırılması durumundan *Etkin* durumuna değiştirmek *mümkündür.* Abonelik durumu *Etkin*olmasa da:

- Bir abonelik *Deprovisioned*iken, işlevselliğini kaybeder. *Etkin*aboneliği geri yükleme, yedekleme/geri yükleme işlevini canlandırır. Yerel diskteki yedekleme verileri yeterince büyük bir bekletme süresiyle korunursa, bu yedekleme verileri alınabilir. Ancak, Abonelik *Deprovisioned* durumuna girdiğinde Azure'daki yedekleme verileri geri alınamaz şekilde kaybolur.
- Bir aboneliğin *süresi dolmuş*olsa da işlevselliğini kaybeder. Aboneliğin *Süresi Dolduğunda*zamanlanan yedeklemeler çalışmaz.

## <a name="troubleshooting"></a>Sorun giderme

Microsoft Azure Yedekleme sunucusu kurulum aşamasında (veya yedekleme veya geri yükleme) hatalarıyla başa çıkamazsa, [hata kodları belgesine](https://support.microsoft.com/kb/3041338)bakın.
[Azure Yedekleme ile ilgili SSS'lere](backup-azure-backup-faq.md) de başvurabilirsiniz

## <a name="next-steps"></a>Sonraki adımlar

DPM [için ortamınızı hazırlamak](https://docs.microsoft.com/system-center/dpm/prepare-environment-for-dpm?view=sc-dpm-1801)başlıklı makale, desteklenen Azure Yedekleme Sunucusu yapılandırmaları hakkında bilgiler içerir.

Microsoft Azure Yedekleme Sunucusu'nu kullanarak iş yükü koruması hakkında daha iyi bir bilgi edinmek için aşağıdaki makaleleri kullanabilirsiniz.

- [SQL Server yedekleme](https://docs.microsoft.com/azure/backup/backup-mabs-sql-azure-stack)
- [SharePoint sunucu yedekleme](https://docs.microsoft.com/azure/backup/backup-mabs-sharepoint-azure-stack)
- [Alternatif sunucu yedekleme](backup-azure-alternate-dpm-server.md)

---
title: Sistem durumu ve çıplak metal geri kazanım koruması
description: Sistem durumunuzu yedeklemek ve çıplak metal kurtarma (BMR) koruması sağlamak için Azure Yedekleme Sunucusu'ni kullanın.
ms.topic: conceptual
ms.date: 05/15/2017
ms.openlocfilehash: 358a1c96d598788170993fc48e60daae2b6b036c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77505885"
---
# <a name="back-up-system-state-and-restore-to-bare-metal-by-using-azure-backup-server"></a>Azure Yedekleme Sunucusu'ni kullanarak sistem durumunu yedekleyin ve çıplak metale geri yükleyin

Azure Yedekleme Sunucusu sistem durumunu yedekler ve çıplak metal kurtarma (BMR) koruması sağlar.

* **Sistem durumu yedeklemesi**: İşletim sistemi dosyalarını yedekler. Bu yedekleme, bilgisayar başladığında kurtarmanızı sağlar, ancak sistem dosyaları ve kayıt defteri kaybolur. Sistem durumu yedeklemesi aşağıdaki öğeleri içerir:
  * Etki alanı üyesi: Önyükleme dosyaları, COM + sınıf kaydı veritabanı, kayıt defteri
  * Etki alanı denetleyicisi: Windows Server Active Directory (NTDS), önyükleme dosyaları, COM+ sınıf kayıt veritabanı, kayıt defteri, sistem hacmi (SYSVOL)
  * Küme hizmetlerini çalıştıran bilgisayar: Küme sunucusu meta verileri
  * Sertifika hizmetlerini çalıştıran bilgisayar: Sertifika verileri
* **Bare-metal yedekleme**: Kullanıcı verileri hariç, işletim sistemi dosyalarını ve kritik hacimlerle ilgili tüm verileri yedekler. Tanım olarak, Bir BMR yedekleme bir sistem durumu yedekleme içerir. Bir bilgisayar başlatılmaz ve her şeyi kurtarmak zorunda koruma sağlar.

Aşağıdaki tabloda yedekleyip kurtarabileceğiniz özetler. Sistem durumu ve BMR'nin koruyabileceği uygulama sürümleri hakkında bilgi için Azure [Yedekleme Sunucusu'nun neyi yedeklediğini görün?](backup-mabs-protection-matrix.md)

|Backup|Sorun|Azure Yedekleme Sunucusu yedeklemesinden kurtarma|Sistem durumu yedeklemesinden kurtarma|BMR|
|----------|---------|---------------------------|------------------------------------|-------|
|**Dosya verileri**<br /><br />Normal veri yedekleme<br /><br />BMR/Sistem durumu yedeklemesi|Kayıp dosya verileri|E|N|N|
|**Dosya verileri**<br /><br />Dosya verilerinin Azure Yedekleme Sunucusu yedeklemesi<br /><br />BMR/Sistem durumu yedeklemesi|Kayıp veya bozuk işletim sistemi|N|E|E|
|**Dosya verileri**<br /><br />Dosya verilerinin Azure Yedekleme Sunucusu yedeklemesi<br /><br />BMR/Sistem durumu yedeklemesi|Kayıp sunucu (veri hacimleri bozulmadan)|N|N|E|
|**Dosya verileri**<br /><br />Dosya verilerinin Azure Yedekleme Sunucusu yedeklemesi<br /><br />BMR/Sistem durumu yedeklemesi|Kayıp sunucu (Veri birimleri kayıp)|E|N|E<br /><br />BMR, yedeklenmiş dosya verilerinin düzenli olarak kurtarılmasını takip|
|**SharePoint verileri**<br /><br />Çiftlik verilerinin Azure Yedekleme Sunucusu yedeklemesi<br /><br />BMR/Sistem durumu yedeklemesi|Kayıp site, listeler, liste öğeleri, belgeler|E|N|N|
|**SharePoint verileri**<br /><br />Çiftlik verilerinin Azure Yedekleme Sunucusu yedeklemesi<br /><br />BMR/Sistem durumu yedeklemesi|Kayıp veya bozuk işletim sistemi|N|E|E|
|**SharePoint verileri**<br /><br />Çiftlik verilerinin Azure Yedekleme Sunucusu yedeklemesi<br /><br />BMR/Sistem durumu yedeklemesi|Olağanüstü durum kurtarma|N|N|N|
|Windows Server 2012 R2 Hyper-V<br /><br />Hyper-V ana bilgisayar veya konuğun Azure Yedekleme Sunucusu yedeklemesi<br /><br />BMR/konak sistem durumu yedeklemesi|Kayıp VM|E|N|N|
|Hyper-V<br /><br />Hyper-V ana bilgisayar veya konuğun Azure Yedekleme Sunucusu yedeklemesi<br /><br />BMR/konak sistem durumu yedeklemesi|Kayıp veya bozuk işletim sistemi|N|E|E|
|Hyper-V<br /><br />Hyper-V ana bilgisayar veya konuğun Azure Yedekleme Sunucusu yedeklemesi<br /><br />BMR/konak sistem durumu yedeklemesi|Kayıp Hyper-V konağı (VM’ler bozulmamış)|N|N|E|
|Hyper-V<br /><br />Hyper-V ana bilgisayar veya konuğun Azure Yedekleme Sunucusu yedeklemesi<br /><br />BMR/konak sistem durumu yedeklemesi|Kayıp Hyper-V konağı (VM’ler kayıp)|N|N|E<br /><br />BMR, ardından düzenli Azure Yedekleme Sunucusu kurtarma|
|SQL Server/Exchange<br /><br />Azure Yedekleme Sunucusu uygulaması yedekleme<br /><br />BMR/Sistem durumu yedeklemesi|Kayıp uygulama verileri|E|N|N|
|SQL Server/Exchange<br /><br />Azure Yedekleme Sunucusu uygulaması yedekleme<br /><br />BMR/Sistem durumu yedeklemesi|Kayıp veya bozuk işletim sistemi|N|E|E|
|SQL Server/Exchange<br /><br />Azure Yedekleme Sunucusu uygulaması yedekleme<br /><br />BMR/Sistem durumu yedeklemesi|Kayıp sunucu (veritabanı/işlem günlüğü dosyaları bozulmamış)|N|N|E|
|SQL Server/Exchange<br /><br />Azure Yedekleme Sunucusu uygulaması yedekleme<br /><br />BMR/Sistem durumu yedeklemesi|Kayıp sunucu (veritabanı/işlem günlüğü dosyaları kayıp)|N|N|E<br /><br />BMR kurtarma, ardından düzenli Azure Yedekleme Sunucusu kurtarma|

## <a name="how-system-state-backup-works"></a>Sistem durumu yedeklemesinin çalışma şekli

Bir sistem durumu yedeklemesi çalıştığında, Yedekleme Sunucusu sunucunun sistem durumunun yedekini istemek için Windows Server Yedekleme ile iletişim kurar. Varsayılan olarak, Backup Server ve Windows Server Backup en kullanılabilir boş alana sahip sürücüyü kullanır. Bu sürücü yle ilgili bilgiler *PSDataSourceConfig.xml* dosyasına kaydedilir. 

Yedekleme Sunucusu'nun sistem durumu yedeklemesi için kullandığı sürücüyü özelleştirebilirsiniz: 

1. Korumalı sunucuda *C:\Program Files\Microsoft Data Protection Manager\MABS\Datasources*adresine gidin. 
1. Düzenleme için *PSDataSourceConfig.xml* dosyasını açın. 
1. Sürücü \<harfi için\> FilesToProtect değerini değiştirin. 
1. Dosyayı kaydedin ve kapatın. 

Bilgisayarın sistem durumunu korumak için bir koruma grubu ayarlanmışsa, tutarlılık denetimi çalıştırın. Bir uyarı oluşturulursa, uyarıda **koruma grubunu değiştir'i** seçin ve ardından sihirbazdaki sayfaları tamamlayın. Ardından başka bir tutarlılık denetimi çalıştırın.

Koruma sunucusu bir kümedeyse, en boş alana sahip sürücü olarak bir küme sürücüsü seçilebilir. Bu sürücü sahipliği başka bir düğüme geçerse ve sistem durumu yedeklemesi çalışırsa, sürücü kullanılamaz ve yedekleme başarısız olur. Bu senaryoda, *PSDataSourceConfig.xml'i* yerel bir sürücüyü işaret edecek şekilde değiştirin.

Ardından, Windows Server Backup geri yükleme klasörünün kökünde *WindowsImageBackup* adlı bir klasör oluşturur. Windows Server Backup yedeklemeyi oluştururken, tüm veriler bu klasöre yerleştirilir. Yedekleme bittiğinde, dosya Yedekleme Sunucusu bilgisayarına aktarılır. Aşağıdaki bilgileri not edin:

* Yedekleme veya aktarım bittiğinde bu klasör ve içindekiler temizlenmez. Bunu düşünmenin en iyi yolu, bir sonraki yedekleme bitince alan için ayrılmış olmasıdır.
* Klasör her yedekleme için oluşturulur. Saat ve tarih damgası, son sistem durum yedeklemenizin zamanını yansıtır.

## <a name="how-bmr-backup-works"></a>BMR yedekleme nasıl çalışır?

BMR (sistem durumu yedeklemesi dahil) için yedekleme işi doğrudan Yedekleme Sunucusu bilgisayarındaki bir paylaşıma kaydedilir. Korumalı sunucudaki bir klasöre kaydedilmez.

Backup Server, Windows Server Backup'ı çağırır ve bmr yedeklemesinin çoğaltma hacmini paylaşır. Bu durumda, en boş alana sahip sürücüyü kullanmak için Windows Server Yedekleme'ye gerek yok. Bunun yerine, iş için oluşturulan paylaşımı kullanır.

Yedekleme bittiğinde, dosya Yedekleme Sunucusu bilgisayarına aktarılır. Günlükler *C:\Windows\Logs\WindowsServerBackup'da*depolanır.

## <a name="prerequisites-and-limitations"></a>Önkoşullar ve sınırlamalar

* BMR, Windows Server 2003 çalıştıran bilgisayarlar veya istemci işletim sistemi çalıştıran bilgisayarlar için desteklenmez.

* Farklı koruma gruplarındaki aynı bilgisayar için BMR'yi ve sistem durumunu koruyamazsınız.

* Yedek Sunucu bilgisayarı BMR için kendini koruyamaz.

* BMR için teyp için kısa süreli koruma (diskten teybe veya D2T) desteklenmez. Teyp için uzun süreli depolama (diskten teyp diske veya D2D2T) desteklenir.

* BMR koruması için, Windows Server Yedekleme korumalı bilgisayara yüklenmiş olmalıdır.

* BMR koruması için, sistem durumu korumasının aksine, Backup Server'ın korumalı bilgisayarda alan gereksinimleri yoktur. Windows Server Backup yedekleri doğrudan Yedekleme Sunucusu bilgisayarına aktarıyor. Yedekleme aktarım işi Yedek Sunucu **İşleri** görünümünde görünmez.

* Backup Server, BMR için çoğaltma hacminde 30 GB alan ayırır. Bu alan tahsisini Koruma Grubu Değiştir Sihirbazı'ndaki **Disk Ayırma** sayfasında değiştirebilirsiniz. Ya da Get-DatasourceDiskAllocation ve Set-DatasourceDiskAllocation PowerShell cmdlets kullanabilirsiniz. Kurtarma noktası hacminde, BMR koruması beş günlük bir bekletme için yaklaşık 6 GB gerektirir.
  * Çoğaltma birim boyutunu 15 GB'ın en azına indiremezsiniz.
  * Backup Server, BMR veri kaynağının boyutunu hesaplamaz. Tüm sunucular için 30 GB varsayar. Ortamınızda beklediğiniz BMR yedeklemelerinin boyutuna göre değeri değiştirin. Tüm kritik birimlerde kullanılan alanın toplamı olarak bir BMR yedeklemesinin boyutunu kabaca hesaplayabilirsiniz. Kritik birimler = önyükleme hacmi + sistem hacmi + etkin dizin gibi birim barındırma sistemi durumu verileri.

* Sistem durumu korumasından BMR korumasına geçiş ederseniz, BMR koruması *kurtarma noktası hacminde*daha az yer gerektirir. Ancak, birimdeki ekstra alan geri alınamaz. Koruma Grubu **Sihirbazı Değiştir'in Disk Tahsisini Değiştir** sayfasındaki birim boyutunu el ile küçültebilirsiniz. Ya da Get-DatasourceDiskAllocation ve Set-DatasourceDiskAllocation PowerShell cmdlets kullanabilirsiniz.

    Sistem durumu korumasından BMR korumasına geçiş ederseniz, BMR koruması *çoğaltma hacminde*daha fazla alan gerektirir. Birim otomatik olarak uzatılır. Varsayılan alan ayırmalarını değiştirmek istiyorsanız, Değiştir-DiskAllocation PowerShell cmdlet'ini kullanın.

* BMR korumasından sistem durumu korumasına geçiş ederseniz, kurtarma noktası hacminde daha fazla alana ihtiyacınız olur. Backup Server ses düzeyini otomatik olarak artırmayı deneyebilir. Depolama havuzunda yeterli alan yoksa bir hata oluşur.

    BMR korumasından sistem durumu korumasına geçiş yapacaksanız, korumalı bilgisayarda yer almanız gerekir. Sistem durum koruması önce yinelemeyi yerel bilgisayara yazdığından ve sonra yinelemeyi Yedekleme Sunucusu bilgisayarına aktardığından, boşluğa ihtiyacınız vardır.

## <a name="before-you-begin"></a>Başlamadan önce

1. **Azure Yedekleme Sunucusu'na dağıtın.** Yedekleme Sunucusu'nun doğru şekilde dağıtılmış olduğunu doğrulayın. Daha fazla bilgi için bkz.
    * [Azure Yedekleme Sunucusu için sistem gereksinimleri](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites)
    * [Yedek Sunucu koruma matrisi](backup-mabs-protection-matrix.md)

1. **Depolama alanı ayarlayın.** Yedekleme verilerini Azure ile diskte, teypte ve bulutta depolayabilirsiniz. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/system-center/dpm/plan-long-and-short-term-data-storage)

1. **Koruma aracısını ayarla.** Yedeklemek istediğiniz bilgisayara koruma aracısını yükleyin. Daha fazla bilgi için Bkz. [DPM koruma aracısını dağıtın.](https://docs.microsoft.com/system-center/dpm/deploy-dpm-protection-agent)

## <a name="back-up-system-state-and-bare-metal"></a>Sistem durumu yedeklemesi ve tam yedekleme

Sistem durumunu ve çıplak metali yedeklemek için:

1. Yeni Koruma Grubu Oluştur Sihirbazı'nı Yedekleme Sunucusu Yönetici Konsolu'nda açmak için **Koruma** > **Eylemleri** > **Oluşturma Koruma Grubu'nu**seçin.

1. Koruma **Grubu Türü Seç** sayfasında **Sunucular'ı**seçin ve ardından **İleri'yi**seçin.

1. Grup **Üyeleri seç** sayfasında bilgisayarı genişletin ve ardından **BMR** veya **sistem durumunu**seçin.

    Farklı gruplarda aynı bilgisayar için hem BMR hem de sistem durumunu koruyamayacağınızı unutmayın. Ayrıca, BMR'yi seçtiğinizde, sistem durumu otomatik olarak etkinleştirilir. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups)

1. Veri **Koruma Yöntemini Seç** sayfasında, kısa süreli yedekleme ve uzun vadeli yedeklemeyi nasıl işleyeceğinizi seçin. 

    Kısa süreli yedekleme, Azure Yedekleme (kısa veya uzun vadeli) kullanarak diskten Azure'a yedekleme seçeneğiyle her zaman önce diske olur. Bulutun uzun süreli yedeklemesine alternatif olarak, Yedekleme Sunucusu'na bağlı bağımsız bir teyp aygıtına veya teyp kitaplığına uzun süreli yedekleme ayarlamak gerekir.

1. Kısa **Vadeli Hedefler Seç** sayfasında, diskte kısa süreli depolamaya nasıl yedekleneğiseçin:
    * **Bekletme aralığı**için, verileri diskte ne kadar süreyle tutacağınızı seçin.
    * **Eşitleme sıklığı**için, diske artımlı yedeklemenin ne sıklıkta çalıştırılabildiğini seçin. Yedekleme aralığı ayarlamak istemiyorsanız, **kurtarma noktasından hemen öncesini**seçebilirsiniz. Yedekleme Sunucusu, her kurtarma noktası zamanlanmadan hemen önce bir ekspres tam yedekleme çalıştıracaktır.

1. Verileri uzun süreli depolama için teypte depolamak istiyorsanız, **Uzun Vadeli Hedefler Belirt** sayfasında, teyp verilerini ne kadar süreyle tutacağınızı (1 ila 99 yıl) seçin.
    1. **Yedekleme sıklığı**için, teyp için yedekleme çalıştırmak için ne sıklıkta seçin. Sıklık seçtiğiniz bekletme aralığına bağlıdır:
        * Bekletme aralığı 1 ila 99 yıl olduğunda, günlük, haftalık, iki haftada bir, aylık, üç aylık, yarı yıllık veya yıllık olarak yedekleyebilirsiniz.
        * Bekletme aralığı 1 ila 11 ay arasında olduğunda, günlük, haftalık, iki haftada bir veya aylık yedekleyebilirsiniz.
        * Bekletme aralığı 1 ila 4 hafta olduğunda, günlük veya haftalık yedekleme yapabilirsiniz.

    1. Bant **ve Kitaplık Ayrıntılarını Seç** sayfasında, kullanılacak teyp ve kitaplığı seçin. Ayrıca, verilerin sıkıştırılıp şifrelenmemesi gerektiğini de seçin.

1. Disk **Tahsisini Gözden Geçir** sayfasında, koruma grubu için kullanılabilen depolama havuzu disk alanını gözden geçirin.

    * **Toplam Veri boyutu,** yedeklemek istediğiniz verilerin boyutudur.
    * **Azure Yedekleme Sunucusu'nda sağlanacak disk alanı,** Yedekleme Sunucusu'nun koruma grubu için önerdiği alandır. Backup Server ideal yedekleme hacmini seçmek için bu ayarları kullanır. **Disk ayırma ayrıntılarındaki**yedekleme birimi seçeneklerini atabilirsiniz.
    * İş yükleri için açılır menüde tercih edilen depolama alanını seçin. Düzenlemeleriniz, **Kullanılabilir Disk Depolaması** bölmesindeki **Toplam Depolama** ve **Boş Depolama** değerlerini değiştirir. Underprovisioned alan, Yedekleme Sunucusu'nun düzgün yedeklemeler sağlamak için ses birimine eklemenizi önerdiği depolama miktarıdır.

1. Çoğaltma **Oluşturma Yöntemini Seç** sayfasında, ilk tam veri çoğaltma işlemini nasıl işleyeceğinizi seçin. 

    Ağ üzerinden çoğaltmayı seçerseniz, yoğun olmayan bir zaman seçmenizi öneririz. Büyük miktarda veri veya en iyi den daha az olan ağ koşulları için, çıkarılabilir ortam kullanarak verileri çevrimdışı olarak çoğaltmayı düşünün.

1. Tutarlılık **Denetim Seçenekleri seç** sayfasında, tutarlılık denetimlerini nasıl otomatikleştireceğinizi seçin. 

    Denetimi yalnızca yineleme verileri tutarsız olduğunda veya zamanlamada çalıştırmayı seçebilirsiniz. Otomatik tutarlılık denetimini yapılandırmak istemiyorsanız, istediğiniz zaman el ile kontrol çalıştırabilirsiniz. Yedek Sunucu Yöneticisi Konsolunun **Koruma** alanında el ile denetim yapmak için koruma grubuna sağ tıklayın ve ardından **Tutarlılık Denetimini Gerçekleştir'i**seçin.

1. **Çevrimiçi Koruma Verilerini Belirt** sayfasında Azure Yedekleme'yi kullanarak bulutu yedeklemeyi seçtiyseniz, Azure'a yedeklemek istediğiniz iş yüklerini seçin.

1. Çevrimiçi **Yedekleme Çizelgesi'ni belirt** sayfasında, Azure'a ne sıklıkta yedek olarak geri dönülteceklerini seçin. 

    Her gün, hafta, ay ve yıl çalışacak yedeklemeler zamanlayabilirsiniz. Yedeklemelerin çalışması gereken saati ve tarihi de seçebilirsiniz. Yedeklemeler günde iki kez gerçekleşebilir. Yedekleme her çalıştığında, Yedekleme Sunucusu diskinde depolanan yedekleme verilerinin kopyasından Azure'da bir veri kurtarma noktası oluşturulur.

1. Çevrimiçi **Bekletme İlkesi Belirt** sayfasında, günlük, haftalık, aylık ve yıllık yedeklemelerden oluşturulan kurtarma noktalarının Azure'da nasıl tutulduğunu seçin.

1. Çevrimiçi **Çoğaltma** yı seç sayfasında, verilerin ilk tam çoğaltmasının nasıl gerçekleşeceğini seçin. 

    Ağ üzerinden çoğaltabilir veya çevrimdışı yedekleme (çevrimdışı tohumlama) yapabilirsiniz. Çevrimdışı yedekleme, Azure İçe Aktarma özelliğini kullanır. Daha fazla bilgi için [Azure Yedekleme'de Çevrimdışı yedekleme iş akışına](offline-backup-azure-data-box.md)bakın.

1. **Özet** sayfasında ayarlarınızı inceleyin. **Create Group'u**seçtikten sonra, verilerin ilk çoğaltması gerçekleşir. Veri çoğaltma bittiğinde, **Durum** sayfasında, koruma grubu durumu **Tamam.** Yedeklemeler daha sonra koruma grubu ayarlarına göre gerçekleşir.

## <a name="recover-system-state-or-bmr"></a>Sistem durumu veya BMR kurtarma

BMR'yi veya sistem durumu yedeğini ağ konumuna kurtarın. BMR'yi yedeklediyseniz, sisteminizi başlatmak ve ağa bağlamak için Windows Kurtarma Ortamı'nı (WinRE) kullanın. Ardından ağ konumundan kurtarmak için Windows Sunucu Yedeklemesini kullanın. Sistem durumunu yedeklediyseniz, ağ konumundan kurtarmak için Windows Server Yedekleme'yi kullanmanız gereken bir durumdur.

### <a name="restore-bmr"></a>BMR'yi geri yükleme

Yedekleme Sunucusu bilgisayarında kurtarma çalıştırmak için:

1. **Kurtarma** bölmesinde, kurtarmak istediğiniz bilgisayarı bulun. Sonra **Bare Metal Recovery'i**seçin.

1. Kullanılabilir kurtarma noktaları, takvimde kalın olarak gösterilir. Kullanmak istediğiniz kurtarma noktasının tarih ve saatini seçin.

1. Kurtarma **Türü Seç** sayfasında, **ağ klasörüne**Kopyala'yı seçin.

1. Hedef **belirt** sayfasında, kopyalanan verilerin hedefini seçin. 

    Unutmayın, hedef veri için yeterli oda olması gerekir. Hedef için yeni bir klasör oluşturmanızı öneririz.

1. Kurtarma **Seçeneklerini Belirt** sayfasında güvenlik ayarlarını seçin. Daha sonra daha hızlı kurtarma için depolama alanı ağı (SAN) tabanlı donanım anlık görüntülerini kullanıp kullanmayacağımı seçin. Bu seçenek yalnızca:
    * Bu işlevselliği sağlayan bir SAN'ınvar. 
    * Bir klon oluşturabilir ve bölünebilir hale getirebilirsiniz.
    * Korumalı bilgisayar ve Backup Server bilgisayarı aynı ağa bağlıdır.

1. Bildirim seçeneklerini ayarlayın. 
1. **Onay** sayfasında **Kurtar'ı**seçin.

Paylaşım konumunu ayarlamak için:

1. Geri yükleme konumunda, yedeklemenin bulunduğu klasöre gidin.

1. Paylaşılan klasörün kökü *WindowsImageBackup* klasörü olacak şekilde *WindowsImageBackup'ın* bir düzeyi üzerinde olan klasörü paylaşın. 

    Bu klasörü paylaşmazsanız, geri yükleme yedeklemeyi bulamaz. WinRE kullanarak bağlanmak için WinRE'de doğru IP adresi ve kimlik bilgileriyle erişebileceğiniz bir paya ihtiyacınız var.

Sistemi geri yüklemek için:

1. Geri yüklediğiniz sistem için Windows DVD'sini kullanarak görüntüyü geri yüklemek istediğiniz bilgisayarı başlatın.

1. İlk sayfada, dil ve yerel ayar ayarlarını doğrulayın. **Yükle** sayfasında, **bilgisayarınızı onayla'yı**seçin.

1. Sistem **Kurtarma Seçenekleri** sayfasında, **daha önce oluşturduğunuz bir sistem görüntüsünü kullanarak bilgisayarınızı geri yükle'yi**seçin.

1. Sistem görüntüsü**Advanced** > yedekleme sayfasını seç'te,**ağdaki sistem görüntüsü için**gelişmiş arama sistemi görüntüsü **Select a system image backup**  >  **seçin'i**seçin. Bir uyarı görünüyorsa, **Evet'i**seçin. Paylaşım yoluna gidin, kimlik bilgilerini girin ve ardından kurtarma noktasını seçin. Sistem, bu kurtarma noktasında kullanılabilen belirli yedeklemeleri tarar. Kullanmak istediğiniz kurtarma noktasını seçin.

1. Yedekleme sayfasını **nasıl geri yükleyeceğini seç'te** **Biçimlendirme ve yeniden bölme disklerini**seçin. Bir sonraki sayfada, ayarları doğrulayın.

1. Geri yüklemeyi başlatmak için **Finish'i**seçin. Yeniden başlatma gereklidir.

### <a name="restore-system-state"></a>Sistem durumunu geri yükleme

Yedekleme Sunucusu'nda kurtarma çalıştırmak için:

1. **Kurtarma** bölmesinde, kurtarmak istediğiniz bilgisayarı bulun ve ardından **Çıplak Metal Kurtarma'yı**seçin.

1. Kullanılabilir kurtarma noktaları, takvimde kalın olarak gösterilir. Kullanmak istediğiniz kurtarma noktasının tarih ve saatini seçin.

1. Kurtarma **Türü Seç** sayfasında, **ağ klasörüne**Kopyala'yı seçin.

1. Hedef **belirt** sayfasında, verileri nerede kopyalayacağımı seçin. 

    Seçtiğiniz hedefin veriler için yeterli alana sahip olması gerektiğini unutmayın. Hedef için yeni bir klasör oluşturmanızı öneririz.

1. Kurtarma **Seçeneklerini Belirt** sayfasında güvenlik ayarlarını seçin. Daha hızlı kurtarma için SAN tabanlı donanım anlık görüntülerini kullanıp kullanmayacağınızı seçin. Bu seçenek yalnızca: 
    * Bu işlevselliği sağlayan bir SAN'ınvar.
    * Bir klon oluşturabilir ve bölünebilir hale getirebilirsiniz. 
    * Korumalı bilgisayar ve Backup Server sunucusu aynı ağa bağlıdır.

1. Bildirim seçeneklerini ayarlayın. 
1. **Onay** sayfasında **Kurtar'ı**seçin.

Windows Server Yedekleme'yi çalıştırmak için:

1. **Sonraki Bu** > **Sunucukurtarma** > **This Server** > Eylemleri**seçin.**

1. **Başka Bir Sunucu'yu**seçin, Konum **Türünü Belirt** sayfasını seçin ve ardından Uzaktan **paylaşılan klasörü**seçin. Kurtarma noktasını içeren klasöre giden yolu girin.

1. Kurtarma **Türü Seç** sayfasında **Sistem durumunu**seçin.

1. Sistem **Durumu Kurtarma için Konum Seç** sayfasında Özgün **Konum'u**seçin.

1. **Onay** sayfasında **Kurtar'ı**seçin. 

1. Geri yüklemeden sonra sunucuyu yeniden başlatın.

Ayrıca, sistem durumu geri yüklemesini komut istemiyle çalıştırabilirsiniz: 

1. Kurtarmak istediğiniz bilgisayarda Windows Server Yedekleme'yi başlatın. 

1. Sürüm identiferalmak için, bir komut istemi, girin:

   ```wbadmin get versions -backuptarget \<servername\sharename\>```

1. Sistem durumu geri yüklemesini başlatmak için sürüm tanımlayıcısını kullanın. Komut isteminde şunu girin: 

    ```wbadmin start systemstaterecovery -version:<versionidentified> -backuptarget:<servername\sharename>```

1. Kurtarma işlemini başlatmak istediğinizi onaylayın. İşlemi Komut İstemi penceresinde görebilirsiniz. Geri yükleme günlüğü oluşturulur. 

1. Geri yüklemeden sonra sunucuyu yeniden başlatın.

---
title: Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını yükleme
description: Windows makinelerini yedeklemek için Microsoft Azure Kurtarma Hizmetleri (MARS) aracısını nasıl yükleyin öğrenin.
ms.topic: conceptual
ms.date: 03/03/2020
ms.openlocfilehash: d3932b66dbc41ff2631e2cccbe716c0877a509d3
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422929"
---
# <a name="install-the-azure-backup-mars-agent"></a>Azure Yedekleme MARS aracısını yükleme

Bu makalede, Microsoft Azure Kurtarma Hizmetleri (MARS) aracısının nasıl yüklenir. MARS, Azure Yedekleme aracısı olarak da bilinir.

## <a name="about-the-mars-agent"></a>MARS ajanı hakkında

Azure Yedekleme, dosyaları, klasörleri ve sistem durumunu şirket içi makinelerden ve Azure VM'lerden yedeklemek için MARS aracısını kullanır. Bu yedeklemeler Azure'daki Kurtarma Hizmetleri kasasında depolanır. Aracıyı çalıştırabilirsiniz:

* Doğrudan şirket içi Windows makinelerinde. Bu makineler doğrudan Azure'daki Kurtarma Hizmetleri kasasına yedeklenebilir.
* Windows'u Azure VM yedekleme uzantısı ile yan yana çalıştıran Azure VM'lerde. Aracı, VM'deki belirli dosya ve klasörleri yedekler.
* Microsoft Azure Yedekleme Sunucusu (MABS) örneğinde veya System Center Data Protection Manager (DPM) sunucusunda. Bu senaryoda, makineler ve iş yükleri MABS veya Veri Koruma Yöneticisi'ne geri döner. Ardından MABS veya Veri Koruma Yöneticisi, Azure'daki bir kasaya yedeklemek için MARS aracısını kullanır.

Yedekleme için kullanılabilen veriler aracının nerede yüklü olduğuna bağlıdır.

> [!NOTE]
> Genellikle, VM'de bir Azure Yedekleme uzantısı kullanarak bir Azure VM'yi yedeklersiniz. Bu yöntem tüm VM'yi yedekler. VM'deki belirli dosya ve klasörleri yedeklemek istiyorsanız, uzantAyın yanında MARS aracısını yükleyin ve kullanın. Daha fazla bilgi için yerleşik [Azure VM yedeklemesinin Mimarisi'ne](backup-architecture.md#architecture-built-in-azure-vm-backup)bakın.

![Yedekleme işlemi adımları](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a>Başlamadan önce

* Azure [Yedekleme'nin Windows makinelerini yedeklemek için MARS aracısını](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders)nasıl kullandığını öğrenin.
* Mars aracısını ikincil bir MABS veya Veri Koruma Yöneticisi sunucusunda çalıştıran [yedekleme mimarisi](backup-architecture.md#architecture-back-up-to-dpmmabs) hakkında bilgi edinin.
* Mars ajanı tarafından [desteklenenleri ve destekleyebildiklerinizi](backup-support-matrix-mars-agent.md) gözden geçirin.
* Bir sunucuveya istemciyi Azure'a yedeklemeniz gerekiyorsa bir Azure hesabınız olduğundan emin olun. Hesabınız yoksa, birkaç dakika içinde [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturabilirsiniz.
* Yedeklemek istediğiniz makinelerdeki internet erişimini doğrulayın.
* MARS aracısının yükleme ve yapılandırmasını gerçekleştiren kullanıcının sunucuda korunması gereken yerel yönetici ayrıcalıklarına sahip olduğundan emin olun.

[!INCLUDE [How to create a Recovery Services vault](../../includes/backup-create-rs-vault.md)]

## <a name="modify-storage-replication"></a>Depolama çoğaltma değiştirme

Varsayılan olarak, kasalar [coğrafi yedekli depolama (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs)kullanır.

* Kasa birincil yedekleme mekanizmanızsa, GRS kullanmanızı öneririz.
* Azure depolama maliyetlerini azaltmak için [yerel olarak yedekli depolama (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) kullanabilirsiniz.

Depolama çoğaltma türünü değiştirmek için:

1. Yeni kasada **Ayarlar** bölümünün altındaki **Özellikler'i** seçin.

1. **Özellikler** sayfasında, **Yedekleme Yapılandırması** **altında, Güncelleştir'i**seçin.

1. Depolama çoğaltma türünü seçin ve **Kaydet'i**seçin.

    ![Yedekleme Yapılandırması'nı güncelleştir](./media/backup-afs/backup-configuration.png)

> [!NOTE]
> Kasa ayarlandıktan ve yedek öğeler içeren depolama çoğaltma türünü değiştiremezsiniz. Bunu yapmak istiyorsan, kasayı yeniden oluşturman gerekiyor.
>

### <a name="verify-internet-access"></a>İnternet erişimini doğrulama

Makinenizin internet erişimi sınırlıysa, makine veya proxy'deki güvenlik duvarı ayarlarının aşağıdaki URL'lere ve IP adreslerine izin verdiğinden emin olun:

* URL'ler
  * `www\.msftncsi.com`
  * `*.Microsoft.com`
  * `*.WindowsAzure.com`
  * `*.microsoftonline.com`
  * `*.windows.net`
* IP adresleri
  * 20.190.128.0/18
  * 40.126.0.0/18

### <a name="use-azure-expressroute"></a>Azure ExpressRoute'u kullanma

Herkese açık bakış (eski devreler için kullanılabilir) ve Microsoft'u kullanarak verilerinizi Azure ExpressRoute üzerinden yedekleyebilirsiniz. Özel bakışlar üzerinden yedekleme desteklenmez.

Genel bakışları kullanmak için öncelikle aşağıdaki etki alanları ve adreslere erişimi sağlayın:

* `http://www.msftncsi.com/ncsi.txt`
* `microsoft.com`
* `.WindowsAzure.com`
* `.microsoftonline.com`
* `.windows.net`

Microsoft'un bakışını kullanmak için aşağıdaki hizmetleri, bölgeleri ve ilgili topluluk değerlerini seçin:

* Azure Etkin Dizin (12076:5060)
* Kurtarma Hizmetleri kasanızın konumuna göre Azure bölgesi
* Kurtarma Hizmetleri kasanızın konumuna göre Azure Depolama

Daha fazla bilgi için [ExpressRoute yönlendirme gereksinimlerine](https://docs.microsoft.com/azure/expressroute/expressroute-routing)bakın.

> [!NOTE]
> Kamu bakışları yeni devreler için amortismana uyrtu.

Önceki URL'lerin ve IP adreslerinin tümü, 443 bağlantı noktasındaki HTTPS protokolünü kullanır.

### <a name="private-endpoints"></a>Özel Uç Noktalar

[!INCLUDE [Private Endpoints](../../includes/backup-private-endpoints.md)]

## <a name="download-the-mars-agent"></a>MARS aracısını indirin

Yedeklemek istediğiniz makinelere yükleyebilmeniz için MARS aracısını indirin.

Aracıyı herhangi bir makineye zaten yüklediyseniz, aracının en son sürümünü çalıştırdığınızdan emin olun. Portaldaki en son sürümü bulun veya doğrudan [indirmeye](https://aka.ms/azurebackup_agent)gidin.

1. Kasada, **Başlarken**altında **Yedek'i**seçin.

    ![Yedekleme hedefini açma](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

1. **İş yükünüz nerede çalışıyor?** **On-premises** MARS aracısını Azure VM'ye yüklemek isteseniz bile bu seçeneği belirleyin.
1. **Ne yedeklemek istiyorsunuz altında?** **Files and folders** **Sistem Durumu'nun**durumunu da seçebilirsiniz. Başka birçok seçenek kullanılabilir, ancak bu seçenekler yalnızca ikincil bir yedekleme sunucusu çalıştırıyorsanız desteklenir. **Altyapı Hazırla'yı**seçin.

    ![Dosya ve klasörleri yapılandırma](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

1. **Altyapı yı hazırlamak**için, Install Recovery Services **aracısı**altında, MARS aracısını indirin.

    ![Altyapıyı hazırlama](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

1. İndirme menüsünde **Kaydet'i**seçin. Varsayılan olarak, *MARSagentinstaller.exe* dosyası İndirilenler klasörünüze kaydedilir.

1. **Zaten'yi indirin veya en son Kurtarma Hizmetleri Aracısını kullanarak**seçin ve ardından kasa kimlik bilgilerini indirin.

    ![Kasa kimlik bilgilerini indirme](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

1. **Kaydet'i**seçin. Dosya İndirilenler klasörünüze indirilir. Kasa kimlik bilgilerini açamazsın.

## <a name="install-and-register-the-agent"></a>Aracıyı yükleme ve kaydetme

1. Yedeklemek istediğiniz makinelerde *MARSagentinstaller.exe* dosyasını çalıştırın.
1. MARS Aracıkurulum Sihirbazı'nda **Kurulum Ayarları'nı**seçin. Burada, aracıyı nerede yükleyeceğinizi seçin ve önbellek için bir konum seçin. Ardından **İleri'yi**seçin.
   * Azure Yedekleme, veri anlık görüntülerini Azure'a göndermeden önce depolamak için önbelleği kullanır.
   * Önbellek konumu, yedeklediğiniz verilerin boyutunun en az yüzde 5'ine eşit boş alana sahip olmalıdır.

    ![MARS Aracıkurulum Sihirbazı'nda yükleme ayarlarını seçin](./media/backup-configure-vault/mars1.png)

1. **Proxy Yapılandırması**için, Windows makinesinde çalışan aracının internete nasıl bağlanacağını belirtin. Ardından **İleri'yi**seçin.

   * Özel bir proxy kullanıyorsanız, gerekli proxy ayarlarını ve kimlik bilgilerini belirtin.
   * Aracının [belirli URL'lere](#before-you-start)erişmesi gerektiğini unutmayın.

    ![MARS sihirbazında internet erişimi ayarlama](./media/backup-configure-vault/mars2.png)

1. **Yükleme**için ön koşulları gözden geçirin ve **Yükle'yi**seçin.
1. Aracı yüklendikten sonra **Kayda Geç'i**seçin.
1. **Register Server Wizard** > **Vault Identification'da,** indirdiğiniz kimlik bilgileri dosyasına göz atın ve seçin. Ardından **İleri'yi**seçin.

    ![Register Server Sihirbazı'nı kullanarak kasa kimlik bilgilerini ekleme](./media/backup-configure-vault/register1.png)

1. Şifreleme **Ayarı** sayfasında, makinenin yedeklemelerini şifrelemek ve çözmek için kullanılacak bir parola tümceciği belirtin.

    * Parolayı güvenli bir yere kaydedin. Bir yedekleme geri yüklemek için ihtiyacınız var.
    * Parolayı kaybeder veya unutursanız, Microsoft yedekleme verilerini kurtarmanıza yardımcı olamaz.

1. **Bitiş'i**seçin. Aracı şimdi yüklendi ve makineniz kasaya kayıtlı. Yedeklemenizi yapılandırıp zamanlamak için hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Yedekleme MARS aracısını kullanarak Windows makinelerini nasıl yedekleyin](backup-windows-with-mars-agent.md)

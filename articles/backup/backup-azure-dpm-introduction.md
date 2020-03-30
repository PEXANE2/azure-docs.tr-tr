---
title: İş yüklerini yedeklemek için DPM sunucusunu hazırlama
description: Bu makalede, Azure Yedekleme hizmetini kullanarak Sistem Merkezi Veri Koruma Yöneticisi (DPM) yedeklemelerine Azure'a nasıl hazırlanacağınızı öğrenin.
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 2119d46ca6102286ca879777058a49938b501ad6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273468"
---
# <a name="prepare-to-back-up-workloads-to-azure-with-system-center-dpm"></a>System Center DPM ile iş yüklerini Azure'a yedeklemeye hazırlanın

Bu makalede, Azure Yedekleme hizmetini kullanarak Sistem Merkezi Veri Koruma Yöneticisi (DPM) yedeklemelerine Azure'a nasıl hazırlanacağı açıklanmaktadır.

Makale şunları sağlar:

- Azure Yedekleme ile DPM dağıtmaya genel bakış.
- DPM ile Azure Yedekleme'yi kullanmak için ön koşullar ve sınırlamalar.
- Kurtarma Hizmetleri Yedekleme kasası kurma ve isteğe bağlı olarak kasa için Azure depolama alanı türünü değiştirme dahil olmak üzere Azure hazırlama adımları.
- Kasa kimlik bilgilerini indirme, Azure Yedekleme aracısını yükleme ve DPM sunucusunu kasaya kaydetme gibi DPM sunucusunu hazırlama adımları.
- Sık karşılaşılan hatalar için sorun giderme ipuçları.

## <a name="why-back-up-dpm-to-azure"></a>DPM'yi neden Azure'a yedekledin?

[Sistem Merkezi DPM](https://docs.microsoft.com/system-center/dpm/dpm-overview) dosya ve uygulama verilerini yedekler. DPM, Azure Yedekleme ile aşağıdaki gibi etkileşimde direnirken:

- **Fiziksel bir sunucuda veya şirket içi VM'de çalışan DPM** — Disk ve teyp yedeklemesine ek olarak verileri Azure'daki yedekleme kasasına yedekleyebilirsiniz.
- **Bir Azure VM'de çalışan DPM** — System Center 2012 R2'den Güncelleme 3 veya sonraki bir tarihte, DPM'yi bir Azure VM'de dağıtabilirsiniz. Verileri VM'ye bağlı Azure disklerine yedekleyebilir veya verileri Yedekleme kasasına yedeklemek için Azure Yedekleme'yi kullanabilirsiniz.

DPM sunucularını Azure'a yedeklemenin ticari avantajları şunlardır:

- Şirket içi DPM için Azure Yedekleme, teyp için uzun süreli dağıtıma alternatif sağlar.
- Azure VM'de çalışan DPM için Azure Yedekleme, Azure diskinden depolama alanı boşaltmanızı sağlar. Eski verileri Yedekleme kasasında depolamak, yeni verileri diske depolayarak işinizi büyütmenize olanak tanır.

## <a name="prerequisites-and-limitations"></a>Önkoşullar ve sınırlamalar

**Ayar** | **Gereksinim**
--- | ---
Azure VM'de DPM | DPM 2012 R2 Ile Sistem Merkezi 2012 R2 Rollup 3 veya daha sonra.
Fiziksel bir sunucuda DPM | Sistem Merkezi 2012 SP1 veya sonrası; Sistem Merkezi 2012 R2.
Hyper-V VM'de DPM | Sistem Merkezi 2012 SP1 veya sonrası; Sistem Merkezi 2012 R2.
VMware VM üzerinde DPM | Sistem Merkezi 2012 R2 Güncelleme Rollup 5 veya daha sonra.
Bileşenler | DPM sunucusunda Windows PowerShell ve .NET Framework 4.5 yüklü olmalıdır.
Desteklenen uygulamalar | [DPM'nin](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix) neyi yedekleyebilir olduğunu öğrenin.
Desteklenen dosya türleri | Bu dosya türleri Azure Yedekleme: Şifreli (yalnızca tam yedeklemeler) ile yedeklenebilir; Sıkıştırılmış (artımlı yedeklemeler desteklenir); Seyrek (artımlı yedeklemeler desteklenir); Sıkıştırılmış ve seyrek (seyrek olarak kabul edilir).
Desteklenmeyen dosya türleri | Büyük/küçük harf duyarlı dosya sistemlerindeki sunucular; sabit bağlantılar (atlanır); reparse puan (atlanır); şifreli ve sıkıştırılmış (atlanır); şifreli ve seyrek (atlanır); Sıkıştırılmış akış; ayrışdırış akışı.
Yerel depolama | Yedeklemek istediğiniz her makine, yedeklenen verilerin boyutunun en az %5'i kadar yerel ücretsiz depolama alanına sahip olmalıdır. Örneğin, 100 GB veriyi yedeklemek, sıfırdan çizilme konumunda en az 5 GB boş alan gerektirir.
Kasa depolama | Azure Yedekleme kasasına yedeklediğiniz veri miktarı için bir sınır yoktur, ancak bir veri kaynağının (örneğin sanal bir makine veya veritabanı) boyutu 54.400 GB'ı geçmemelidir.
Azure ExpressRoute | Azure ExpressRoute, Private veya Microsoft bakışlarıyla yapılandırılırsa, verileri Azure'a yedeklemek için kullanılamaz.<br/><br/> Azure ExpressRoute, Genel Bakış ile yapılandırılırsa, verileri Azure'a yedeklemek için kullanılabilir.<br/><br/> **Not:** Kamu Peering yeni devreler için amortismana.
Azure Backup aracısı | DPM System Center 2012 SP1'de çalışıyorsa, DPM SP1 için Rollup 2 veya sonraki lerini yükleyin. Bu aracı yükleme için gereklidir.<br/><br/> Bu makalede, Microsoft Azure Kurtarma Hizmeti (MARS) aracısı olarak da bilinen Azure Yedekleme aracısının en son sürümünün nasıl dağıtılancaya kadar dağıtılanınca açıklanmaktadır. Daha önceki bir sürümünüz dağıtılırsa, yedeklemenin beklendiği gibi çalıştığından emin olmak için en son sürüme güncelleştirin.

Başlamadan önce, Azure Yedekleme özelliği etkinleştirilmiş bir Azure hesabına ihtiyacınız vardır. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Azure [Yedekleme fiyatlandırması](https://azure.microsoft.com/pricing/details/backup/)hakkında bilgi edinin.

[!INCLUDE [backup-create-rs-vault.md](../../includes/backup-create-rs-vault.md)]

## <a name="modify-storage-settings"></a>Depolama ayarlarını değiştirme

Coğrafi yedekli depolama ile yerel olarak yedekli depolama arasında seçim yapabilirsiniz.

- Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir.
- Kasa birincil yedeklemenizse, seçeneği coğrafi yedekdepolamaya bırakın. Bu kadar dayanıklı olmayan daha ucuz bir seçenek istiyorsanız, yerel olarak yedekli depolamayı yapılandırmak için aşağıdaki yordamı kullanın.
- Azure [depolama](../storage/common/storage-redundancy.md)ve [coğrafi yedekli](../storage/common/storage-redundancy-grs.md) ve [yerel olarak](../storage/common/storage-redundancy-lrs.md) yedekli depolama seçenekleri hakkında bilgi edinin.
- İlk yedeklemeden önce depolama ayarlarını değiştirin. Bir öğeyi zaten yedeklediyseniz, depolama ayarlarını değiştirmeden önce depoda yedeklemeyi bırakın.

Depolama çoğaltma ayarını düzenlemek için:

1. Kasa panosunu aç.

2. **Yönet'te**Yedek **Altyapı'yı**tıklatın.

3. **Yedekleme Yapılandırması** menüsünde kasa için bir depolama seçeneği seçin.

    ![Yedekleme kasalarının listesi](./media/backup-azure-dpm-introduction/choose-storage-configuration-rs-vault.png)

## <a name="download-vault-credentials"></a>Kasa kimlik bilgilerini indirme

DPM sunucusunu kasaya kaydettirirken kasa kimlik bilgilerini kullanırsınız.

- Kasa kimlik bilgileri dosyası, her bir yedekleme kasası için portal tarafından oluşturulan bir sertifikadır.
- Portal daha sonra ortak anahtarı Access Control Service'e (ACS) yükler.
- Makine kaydı iş akışı sırasında, sertifikanın özel anahtarı kullanıcının kullanımına sunulmuş ve makinenin kimliği doğrulanır.
- Azure Yedekleme hizmeti kimlik doğrulamaya bağlı olarak, verileri tanımlanan kasaya gönderir.

### <a name="best-practices-for-vault-credentials"></a>Kasa kimlik bilgileri için en iyi uygulamalar

Kimlik bilgilerini almak için, Azure portalından güvenli bir kanal üzerinden kasa kimlik bilgilerini indirin:

- Kasa kimlik bilgileri yalnızca kayıt iş akışı sırasında kullanılır.
- Kasa kimlik bilgileri dosyasının güvenli olduğundan ve tehlikeye girmediğinden emin olmak sizin sorumluluğunuzdadır.
  - Kimlik bilgilerinin denetimi kaybolursa, kasa kimlik bilgileri diğer makineleri kasaya kaydetmek için kullanılabilir.
  - Ancak, yedekleme verileri müşteriye ait bir parola kullanılarak şifrelenir, bu nedenle varolan yedekleme verileri tehlikeye atılamaz.
- Dosyanın DPM sunucusundan erişilebilen bir konuma kaydedildiğinden emin olun. Dosya paylaşımında/Kobİ'de depolanırsa, erişim izinlerini denetleyin.
- Vault kimlik bilgileri 48 saat sonra sona erer. Gerektiğinde en çok kez yeni kasa kimlik bilgilerini indirebilirsiniz. Ancak, kayıt iş akışı sırasında yalnızca en son kasa kimlik bilgileri dosyası kullanılabilir.
- Azure Yedekleme hizmeti sertifikanın özel anahtarından haberdar değildir ve özel anahtar portalda veya hizmette kullanılamaz.

Kasa kimlik bilgileri dosyasını aşağıdaki gibi yerel bir makineye indirin:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. DPM sunucusunu kaydetmek istediğiniz kasayı açın.
3. **Ayarlar'da** **Özellikler'i**tıklatın.

    ![Kasa menüsünü açma](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)

4. **Özellikler** > **Yedekleme Kimlik Bilgileri'nde**İndir'i tıklatın. **Download** Portal, kasa adı ve geçerli tarihin bir birleşimini kullanarak kasa kimlik dosyasını oluşturur ve karşıdan yüklemeye hazır hale getirir.

    ![İndirme](./media/backup-azure-dpm-introduction/vault-credentials.png)

5. Kasa kimlik bilgilerini klasöre indirmek için **Kaydet'i** veya **As'ı kaydedip** bir konum belirtin'i tıklatın. Dosyanın oluşturulması bir dakika kadar sürer.

## <a name="install-the-backup-agent"></a>Yedekleme Aracısını Yükleme

Azure Yedekleme tarafından yedeklenen her makinenin üzerinde Yedekleme aracısı (Microsoft Azure Kurtarma Hizmeti (MARS) aracısı olarak da bilinir) olmalıdır. Aracıyı DPM sunucusuna aşağıdaki gibi yükleyin:

1. DPM sunucusunu kaydetmek istediğiniz kasayı açın.
2. **Ayarlar'da** **Özellikler'i**tıklatın.

    ![Kasa menüsünü açma](./media/backup-azure-dpm-introduction/vault-settings-dpm.png)
3. **Özellikler** sayfasında Azure Yedekleme Aracısını indirin.

    ![İndirme](./media/backup-azure-dpm-introduction/azure-backup-agent.png)

4. İndirdikten sonra MARSAgentInstaller.exe'yi çalıştırın. aracıyı DPM makinesine yüklemek için.
5. Aracı için bir yükleme klasörü ve önbellek klasörü seçin. Önbellek konumu boş alanı yedekleme verilerinin en az %5'i olmalıdır.
6. Internet'e bağlanmak için proxy sunucusu kullanıyorsanız, **Proxy yapılandırma** ekranında proxy sunucu ayrıntılarını girin. Kimlik doğrulaması yapılan bir proxy kullanıyorsanız, bu ekrana kullanıcı adı ve parola ayrıntılarını girin.
7. Azure Yedekleme aracısı yüklemeyi tamamlamak için .NET Framework 4.5 ve Windows PowerShell'i (yüklü değillerse) yükler.
8. Aracı yüklendikten sonra pencereyi **kapatın.**

    ![Kapat](../../includes/media/backup-install-agent/dpm_FinishInstallation.png)

## <a name="register-the-dpm-server-in-the-vault"></a>DPM sunucusunu kasaya kaydettirme

1. DPM Administrator > **Yönetimi** **konsolunda, Çevrimiçi'yi**tıklatın. **Kaydol**’u seçin. Register Server Sihirbazı'nı açar.
2. **Proxy Yapılandırması'nda,** proxy ayarlarını gerektiği gibi belirtin.

    ![Ara sunucu yapılandırması](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Proxy.png)
3. **Yedekleme**Vault'ta, indirdiğiniz kasa kimlik bilgileri dosyasına göz atın ve seçin.

    ![Kasa kimlik bilgileri](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Credentials.jpg)

4. **Azaltma Ayarı'nda,** yedeklemeler için bant genişliği azaltmayı isteğe bağlı olarak etkinleştirebilirsiniz. Çalışma saatlerini ve günlerini belirtmek için hız sınırlarını ayarlayabilirsiniz.

    ![Azaltma Ayarı](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Throttling.png)

5. **Kurtarma Klasörü Ayarı'nda,** veri kurtarma sırasında kullanılabilecek bir konum belirtin.

    - Azure Yedekleme, kurtarılan veriler için bu konumu geçici bir saklama alanı olarak kullanır.
    - Veri kurtarmayı tamamladıktan sonra, Azure Yedekleme bu alandaki verileri temizler.
    - Konumun, paralel olarak kurtarmayı beklediğiniz öğeleri tutmak için yeterli alana sahip olması gerekir.

    ![Kurtarma Klasörü Ayarı](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_RecoveryFolder.png)

6. **Şifreleme ayarında,** bir parola oluşturun veya sağlayın.

    - Parola, yedeklemeleri buluta şifrelemek için kullanılır.
    - En az 16 karakter belirtin.
    - Dosyayı güvenli bir yere kaydedin, kurtarma için gereklidir.

    ![Şifreleme](../../includes/media/backup-install-agent/DPM_SetupOnlineBackup_Encryption.png)

    > [!WARNING]
    > Şifreleme parolasına sahipsiniz ve Microsoft'un bu parolada görünürlüğü yoktur.
    > Parola kaybolur veya unutulursa; Microsoft yedekleme verilerini kurtarmada yardımcı olamaz.

7. DPM sunucusunu kasaya kaydetmek için **Kaydol'u** tıklatın.

Sunucu kasaya başarıyla kaydedildikten ve artık Microsoft Azure'a yedeklemeye başlamaya hazır sınız. DPM konsolundaki koruma grubunu iş yüklerini Azure'a yedekedecek şekilde yapılandırmanız gerekir. Koruma gruplarını nasıl dağıtılayın [öğrenin.](https://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups?view=sc-dpm-2019)

## <a name="troubleshoot-vault-credentials"></a>Sorun giderme kasa kimlik bilgileri

### <a name="expiration-error"></a>Sona erme hatası

Kasa kimlik bilgileri dosyası yalnızca 48 saat için geçerlidir (portaldan indirildikten sonra). Bu ekranda herhangi bir hatayla karşılaşırsanız (örneğin, "Vault kimlik bilgileri dosyasının süresi doldu"), Azure portalına giriş yapın ve kasa kimlik bilgileri dosyasını yeniden indirin.

### <a name="access-error"></a>Erişim hatası

Kasa kimlik bilgileri dosyasının kurulum uygulaması tarafından erişilebilen bir konumda bulunduğundan emin olun. Erişimle ilgili hatalarla karşılaşırsanız, kasa kimlik bilgileri dosyasını bu makinedeki geçici bir konuma kopyalayın ve işlemi yeniden deneyin.

### <a name="invalid-credentials-error"></a>Geçersiz kimlik bilgileri hatası

Geçersiz bir kasa kimlik bilgisi hatasıyla karşılaşırsanız (örneğin, "Geçersiz kasa kimlik bilgileri sağlanmıştır") dosya bozuk veya kurtarma hizmetiyle ilişkili en son kimlik bilgilerine sahip değildir.

- Portaldan yeni bir kasa kimlik dosyası indirdikten sonra işlemi yeniden deneyin.
- Bu hata genellikle Azure portalındaki **İndirme kasa kimlik bilgisi** seçeneğini tıklattığınızda, iki kez hızlı bir şekilde görülür. Bu durumda, yalnızca ikinci kasa kimlik bilgisi dosyası geçerlidir.

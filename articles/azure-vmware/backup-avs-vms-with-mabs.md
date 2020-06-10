---
title: Microsoft Azure Backup Server ile Azure VMware Solution (AVS) VM 'lerini yedekleme
description: Azure VMware Solution (AVS) ortamınızı, Microsoft Azure Backup sunucusu kullanarak sanal makineleri yedeklemek üzere yapılandırın.
ms.topic: how-to
ms.date: 06/09/2020
ms.openlocfilehash: 16941753d614bbfacbc63e4ae518986901153662
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84612764"
---
# <a name="back-up-avs-vms-with-microsoft-azure-backup-server"></a>Microsoft Azure Backup Server ile AVS VM 'lerini yedekleme

Bu makalede, Azure VMware çözümü (AVS) üzerinde çalışan VMware sanal makinelerini Azure Backup Sunucusu kullanarak yedekleme yordamlarına geçeceğiz. Başlamadan önce, [AVS için Microsoft Azure Backup sunucusu ayarlama](set-up-mabs-for-avs.md)aşamasından emin olun. 

Ardından, aşağıdakileri yapmak için gereken tüm yordamları inceleyeceğiz:

> [!div class="checklist"] 
> * Azure Backup Sunucusu, HTTPS üzerinden VMware sunucularıyla iletişim kurabilmesi için güvenli bir kanal ayarlayın. 
> * Azure Backup Sunucusu için hesap kimlik bilgilerini ekleyin. 
> * VCenter 'ı Azure Backup Sunucusu ekleyin. 
> * Yedeklemek istediğiniz VMware VM 'lerini içeren bir koruma grubu ayarlayın, yedekleme ayarlarını belirtin ve yedeklemeyi zamanlayın.

## <a name="create-a-secure-connection-to-the-vcenter-server"></a>vCenter Server güvenli bir bağlantı oluşturun

Varsayılan olarak, Azure Backup Sunucusu HTTPS üzerinden VMware sunucularıyla iletişim kurar. HTTPS bağlantısını kurmak için, VMware sertifika yetkilisi (CA) sertifikasını indirin ve Azure Backup Sunucusu alın.

### <a name="set-up-the-certificate"></a>Sertifikayı ayarlama

1. Tarayıcıdaki Azure Backup Sunucusu, vSphere Web Istemcisi URL 'sini girin.

   > [!NOTE] 
   > VMware Başlarken sayfası görünmezse, bağlantı ve tarayıcı ara sunucu ayarlarını doğrulayın ve yeniden deneyin.

1. VMware Başlarken sayfasında, **Güvenilen kök CA sertifikalarını indir**' e tıklayın.

   :::image type="content" source="../backup/media/backup-azure-backup-server-vmware/vsphere-web-client.png" alt-text="vSphere Web Istemcisi":::

1. **Download. zip** dosyasını Azure Backup sunucusu makineye kaydedin ve ardından içeriğini içeren **CERT** klasörüne ayıklayın:

   - .0 ve. 1 gibi numaralandırılmış bir sırayla başlayan bir uzantıya sahip kök sertifika dosyası.

   - CRL dosyası. R0 veya. R1 gibi bir sırayla başlayan bir uzantıya sahiptir.

1. **Sertifikalar** klasöründe, kök sertifika dosyasına sağ tıklayın ve uzantıyı **. CRT**olarak değiştirmek için **Yeniden Adlandır** ' ı seçin.

   Dosya simgesi bir kök sertifikayı temsil eden bir olarak değişir.

1. Kök sertifikaya sağ tıklayın ve **sertifikayı yükler**' i seçin.

1. **Sertifika Içeri aktarma sihirbazında**, sertifika için hedef olarak **yerel makine** ' yi seçin ve **İleri**' ye tıklayın.

   ![Sihirbaza hoş geldiniz](../backup/media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

   > [!NOTE] 
   > İstenirse, bilgisayarda değişikliklere izin vermek istediğinizi onaylayın.    

1. **Tüm sertifikaları aşağıdaki depolama alanına yerleştir**' i seçin ve sertifika deposunu seçmek Için, **Araştır** ' a tıklayın.

   ![Sertifika depolama alanı](../backup/media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

1. Hedef klasör olarak **Güvenilen kök sertifika yetkilileri** ' ni seçin ve **Tamam**' a tıklayın.

1. Ayarları gözden geçirin ve sertifikayı içeri aktarmaya başlamak için **son** ' a tıklayın.

   ![Sertifikanın doğru klasörde olduğunu doğrulama](../backup/media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

1. Sertifika içeri aktarma onaylandıktan sonra, bağlantınızın güvenli olduğunu onaylamak için vCenter Server oturum açın.

### <a name="enable-tls-12-on-azure-backup-server"></a>Azure Backup Sunucusu TLS 1,2 'yi etkinleştirme

VMWare 6,7 üzerinde iletişim kuralı olarak TLS etkinleştirilmiş. 

1. Aşağıdaki kayıt defteri ayarlarını kopyalayıp Not defteri 'ne yapıştırın ve sonra dosyayı TLS olarak kaydedin. . Txt uzantısı olmayan REG.

   ```text
   
   Windows Registry Editor Version 5.00
   
   [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v2.0.50727]
   
      "SystemDefaultTlsVersions"=dword:00000001
   
      "SchUseStrongCrypto"=dword:00000001
   
   [HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
   
      "SystemDefaultTlsVersions"=dword:00000001
   
      "SchUseStrongCrypto"=dword:00000001
   
   [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v2.0.50727]
   
      "SystemDefaultTlsVersions"=dword:00000001
   
      "SchUseStrongCrypto"=dword:00000001
   
   [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
   
      "SystemDefaultTlsVersions"=dword:00000001
   
      "SchUseStrongCrypto"=dword:00000001
   
   ```

1. TLS öğesine sağ tıklayın. Ayarları kayıt defterine eklemek için REG dosyasını açın ve **Birleştir** veya **Aç** ' ı seçin.

## <a name="add-the-provisioning-ip-address-for-avs-esxi-hosts-on-azure-backup-server"></a>Azure Backup Sunucusu üzerindeki AVS ESXi konakları için sağlama IP adresini ekleme

Önizleme süresince, AVS, sanal ağda dağıtılan sanal makineden ESX konağını çözümlemez. Azure Backup Sunucusu sanal makinesine konak dosyası girişi eklemek için ek adımlar gerçekleştirmeniz gerekir.

### <a name="identify-ip-address-for-esxi-hosts"></a>ESXi konakları için IP adresini tanımla

1. Tarayıcıyı açın ve vCenter URL 'Lerinde oturum açın. 

   > [!TIP]
   > URL 'Leri, [özel bulutunuzun yerel vCenter ' a bağlanma](tutorial-access-private-cloud.md#connect-to-the-local-vcenter-of-your-private-cloud) makalesine bulabilirsiniz.

1. VSphere Istemcisinde, yedeklemeyi etkinleştirmeyi planladığınız kümeyi seçin.

   :::image type="content" source="media/avs-backup/vsphere-client-select-host.png" alt-text="VSphere Istemcisi-konak seçin":::

1. **Configure**  >  **Ağ**  >  **vmkernel bağdaştırıcılarını**Yapılandır ' ı seçin ve cihaz listesi altında, **sağlama** rolü etkin olan ağ bağdaştırıcısını belirleyin ve **IP adresini** ve ESXi ana bilgisayar adını unutmayın.

   :::image type="content" source="media/avs-backup/vmkernel-adapters-provisioning-enabled.png" alt-text="VMKernel bağdaştırıcıları-etkinleştirilen cihazları sağlama":::

1. Yedeklemeyi etkinleştirmeyi planladığınız her bir ESXi ana bilgisayarı için önceki adımı tekrarlayın.

### <a name="update-the-host-file-on-the-azure-backup-server"></a>Azure Backup Sunucusu ana bilgisayar dosyasını güncelleştirme

1. **Not defteri 'ni** yönetici olarak açın.

2. **Dosya > aç** ' ı seçin ve C:\windows\system32\drivers\etc\hostiçin arama yapın.

3. Her ESXi konağının girişini, yukarıdaki bölümünde belirlediğiniz IP adresiyle birlikte ekleyin.

4. Değişikliklerinizi kaydedin ve Not defteri 'Ni kapatın.

## <a name="add-the-account-on-azure-backup-server"></a>Hesabı Azure Backup Sunucusu ekleyin


1. Azure Backup sunucusu açın ve Azure Backup sunucusu konsolunda **Yönetim**  >  **üretim sunucuları**  >  **VMware 'yi Yönet**' i seçin.

   ![Azure Backup Sunucusu konsolu](../backup/media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

1. **Kimlik bilgilerini Yönet** Iletişim kutusunda **Ekle**' ye tıklayın.

   ![Kimlik bilgilerini Azure Backup Sunucusu Yönet iletişim kutusu](../backup/media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

1. **Kimlik bilgisi ekle**' de, yeni kimlik bilgisi için bir ad ve açıklama girin ve VMware sunucusunda tanımladığınız Kullanıcı adını ve parolayı belirtin.

   > [!NOTE] 
   > VMware sunucusu ve Azure Backup Sunucusu aynı etki alanında değilse, Kullanıcı adında etki alanını belirtin.

   ![Kimlik bilgisi Ekle iletişim kutusu Azure Backup Sunucusu](../backup/media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

1. Yeni kimlik bilgisini eklemek için **Ekle** ' ye tıklayın.

   ![Kimlik bilgilerini Azure Backup Sunucusu Yönet iletişim kutusu](../backup/media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

## <a name="add-the-vcenter-server-to-azure-backup-server"></a>vCenter Server Azure Backup Sunucusu ekleyin

1. Azure Backup sunucusu konsolunda **Yönetim**  >  **üretim sunucuları**  >  **Ekle**' ye tıklayın.

   ![Üretim sunucusu ekleme Sihirbazı 'Nı aç](../backup/media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

1. **VMware sunucuları** ' nı seçin ve **İleri**' ye tıklayın.

   ![Üretim sunucusu ekleme Sihirbazı](../backup/media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

1. VCenter 'ın IP adresini belirtin.

   ![VMware sunucusu belirtin](../backup/media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

1. **SSL bağlantı noktası**alanına vCenter ile iletişim kurmak için kullanılan bağlantı noktasını girin.

   > [!TIP] 
   > Bağlantı noktası 443 varsayılan bağlantı noktasıdır, ancak vCenter 'niz farklı bir bağlantı noktasında dinliyorsa bunu değiştirebilirsiniz.

1. **Kimlik bilgilerini belirtin**bölümünde, önceki bölümde oluşturduğunuz kimlik bilgisini seçin.

1. **Ekle** ' ye tıklayarak vCenter Server listesine ekleyin ve **İleri**' ye tıklayın.

   ![VMWare sunucusu ve kimlik bilgisi ekleme](../backup/media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

1. **Özet** sayfasında, vCenter Azure Backup sunucusu eklemek için **Ekle** ' ye tıklayın.

   Yeni sunucu hemen eklenir; vCenter 'a bir aracı gerekmez.

   ![VMware sunucusunu Azure Backup Sunucusu Ekle](../backup/media/backup-azure-backup-server-vmware/tasks-screen.png)

1. **Son** sayfasında, ayarları gözden geçirin ve ardından **Kapat**' ı tıklatın.

   ![Son sayfa](../backup/media/backup-azure-backup-server-vmware/summary-screen.png)

   Bir üretim sunucusu altında bulunan vCenter Server 'ı VMware sunucusu olarak Type ve Aracı durumu ise *Tamam*olarak görmeniz gerekir. Aracı durumunu *bilinmiyor*olarak görürseniz **Yenile**' ye tıklayın.

## <a name="configure-a-protection-group"></a>Koruma grubu yapılandırma

Koruma grupları birden çok VM toplar ve aynı veri saklama ve yedekleme ayarlarını gruptaki tüm VM 'lere uygular.

1. Azure Backup sunucusu konsolunda, yeni **koruma**' ya tıklayın  >  **New**.

   ![Yeni koruma grubu oluşturma Sihirbazı 'nı açın](../backup/media/backup-azure-backup-server-vmware/open-protection-wizard.png)

1. **Yeni koruma grubu oluşturma** Sihirbazı 'na hoş geldiniz sayfasında **İleri**' ye tıklayın.

   ![Yeni koruma grubu oluşturma Sihirbazı iletişim kutusu](../backup/media/backup-azure-backup-server-vmware/protection-wizard.png)

1. **Koruma grubu türünü seçin** sayfasında **sunucular** ' ı seçin ve ardından **İleri**' ye tıklayın. **Grup üyelerini seçin** sayfası görüntülenir.

1. **Grup üyelerini seçin**bölümünde yedeklemek Istediğiniz VM 'leri (veya VM klasörlerini) seçin. Ardından **İleri**'ye tıklayın.

   > [!NOTE]
   > Bir klasör veya bu klasörün içindeki klasörler veya klasörler ' i seçtiğinizde yedekleme için de seçilidir. Yedeklemek istemediğiniz klasörlerin veya VM 'Lerin işaretini kaldırabilirsiniz. Bir VM veya klasör zaten yedekleniyorsa, sanal makine için yinelenen kurtarma noktalarının oluşturulmamasını sağlayan bu seçeneği seçemezsiniz.

   ![Grup üyelerini seçin](../backup/media/backup-azure-backup-server-vmware/server-add-selected-members.png)

1. **Veri koruma yöntemini seçin** sayfasında, koruma grubu ve koruma ayarları için bir ad girin. 

1. Kısa vadeli korumayı **diske**ayarlayın, çevrimiçi korumayı etkinleştirin ve ardından **İleri**' ye tıklayın.

   ![Veri koruma yöntemini seçme](../backup/media/backup-azure-backup-server-vmware/name-protection-group.png)

1. Verilerin diske ne kadar süreyle yedeklenmek istediğinizi belirtin.

   - **Bekletme aralığı** -disk kurtarma noktalarının saklanacağı gün sayısı.
   - **Hızlı tam yedekleme** -disk kurtarma noktalarının ne sıklıkta alındığı. Kısa vadeli yedeklemelerin oluşma zamanlarını/tarihleri değiştirmek için **Değiştir**' e tıklayın.

   :::image type="content" source="media/avs-backup/new-protection-group-specify-short-term-goals.png" alt-text="Disk tabanlı koruma için kısa vadeli hedeflerinizi belirtin":::

1. **Disk ayırmayı İncele**' de, VM yedeklemeleri için sunulan disk alanını gözden geçirin.

   - Önerilen disk ayırmaları belirttiğiniz bekletme aralığına, iş yükü türüne ve korunan verilerin boyutuna göre belirlenir. Gerekli değişiklikleri yapın ve ardından **İleri**' ye tıklayın.

   - **Veri boyutu:** Koruma grubundaki verilerin boyutu.

   - **Disk alanı:** Koruma grubu için önerilen miktarda disk alanı. Bu ayarı değiştirmek istiyorsanız, her veri kaynağının büyüyeceğini tahmin ettiğiniz miktardan biraz daha büyük olan toplam alanı ayırmanız gerekir.

   - **Depolama havuzu ayrıntıları:** Toplam ve kalan disk boyutu da dahil olmak üzere depolama havuzunun durumunu gösterir.

   :::image type="content" source="media/avs-backup/review-disk-allocation.png" alt-text="Depolama havuzunda ayrılan disk alanını gözden geçirin":::

   > [!NOTE]
   > Bazı senaryolarda raporlanan veri boyutu gerçek VM boyutundan daha yüksek. Sorunun farkındayız ve şu anda araştırıyoruz.

1. **Çoğaltma oluşturma yöntemini seçin** sayfasında, ilk yedeklemeyi nasıl almak istediğinizi belirtin ve **İleri**' ye tıklayın.

   - Varsayılan değer **otomatik olarak ağ üzerinden** ve **Şimdi**.   Varsayılanı kullanıyorsanız, yoğun olmayan bir zaman belirtmenizi öneririz. **Daha sonra** öğesini seçin ve bir gün ve saat belirtin.

   - Büyük miktarlarda veri veya en iyi olmayan ağ koşulları için, çıkarılabilir medya kullanarak verileri çevrimdışı olarak çoğaltmayı göz önünde bulundurun.

   ![Çoğaltma oluşturma yöntemini seçin](../backup/media/backup-azure-backup-server-vmware/replica-creation.png)

1. **Tutarlılık denetimi seçenekleri**' nde tutarlılık denetimlerinin nasıl ve ne zaman otomatikleştirildiğini seçin ve **İleri**' ye tıklayın.

   - Çoğaltma verileri tutarsız hale geldiğinde veya bir küme zamanlaması üzerinde tutarlılık denetimleri gerçekleştirebilirsiniz.

   - Otomatik tutarlılık denetimlerini yapılandırmak istemiyorsanız, koruma grubuna sağ tıklayarak **tutarlılık denetimi**yapabilirsiniz.

1. **Çevrimiçi koruma verilerini belirtin** sayfasında, yedeklemek Istediğiniz VM 'LERI veya VM klasörlerini seçin ve ardından **İleri**' ye tıklayın. 

   > [!TIP]
   > Üyeleri tek tek seçebilir veya tümünü **Seç** ' e tıklayarak tüm üyeleri seçebilirsiniz.

   ![Çevrimiçi koruma verilerini belirtin](../backup/media/backup-azure-backup-server-vmware/select-data-to-protect.png)

1. **Çevrimiçi yedekleme zamanlamasını belirtin** sayfasında, verileri yerel depolamadan Azure 'a ne sıklıkta yedeklemek istediğinizi belirtin ve **İleri**' ye tıklayın. 

   - Zamanlamaya göre oluşturulacak verilerin bulut kurtarma noktaları. 

   - Kurtarma noktası oluşturulduktan sonra, Azure 'daki kurtarma hizmetleri kasasına aktarılır.

   ![Çevrimiçi Yedekleme zamanlamasını belirtin](../backup/media/backup-azure-backup-server-vmware/online-backup-schedule.png)

1. **Çevrimiçi saklama Ilkesini belirtin** sayfasında, günlük/haftalık/aylık/yıllık yedeklerden Azure 'a oluşturulan kurtarma noktalarını ne kadar süreyle saklamak istediğinizi belirtin ve ardından **İleri**' ye tıklayın.

   - Azure 'da verileri ne kadar süreyle saklayabilmeniz için zaman sınırı yoktur.

   - Tek sınır, korunan örnek başına 9999 taneden fazla kurtarma noktasına sahip olamaz. Bu örnekte, korumalı örnek VMware sunucusudur.

   ![Çevrimiçi bekletme ilkesini belirtin](../backup/media/backup-azure-backup-server-vmware/retention-policy.png)

1. **Özet** sayfasında, ayarları gözden geçirin ve ardından **Grup Oluştur**' a tıklayın.

   ![Koruma grubu üyesi ve ayar Özeti](../backup/media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="monitor-with-azure-backup-server-console"></a>Azure Backup Sunucusu konsolu ile izleme

Bir koruma grubunu AVS VM 'lerini yedekleyecek şekilde yapılandırdıktan sonra, Azure Backup Sunucusu konsolunu kullanarak yedekleme işinin ve uyarının durumunu izleyebilirsiniz. Şunları izleyebilirsiniz.

- Izleme bölmesindeki uyarılar sekmesinde, bir koruma grubu için hataları, uyarıları ve genel bilgileri, belirli bir korumalı bilgisayar veya ileti önem derecesine göre izleyebilirsiniz. Etkin ve etkin olmayan uyarıları görüntüleyebilir ve e-posta bildirimlerini ayarlayabilirsiniz

- Izleme bölmesindeki Işler sekmesinde, belirli bir korumalı veri kaynağı veya koruma grubu için Azure Backup Sunucusu tarafından başlatılan işleri görüntüleyebilirsiniz. İş ilerleme durumunu izleyebilir veya işler tarafından tüketilen kaynakları kontrol edebilirsiniz.

- **Koruma** görev bölmesinde, koruma grubundaki birim ve paylaşımların durumunu denetleyebilir ve kurtarma ayarları, disk ayırma ve yedekleme zamanlaması gibi yapılandırma ayarlarını kontrol edebilirsiniz.

- **Yönetim** görev alanında, **diskler, çevrimiçi**ve **aracılar** sekmesini görüntüleyerek depolama havuzundaki disklerin durumunu DENETLEYEBILIR, Azure 'a kaydedebilir ve DPM aracısının durumunu dağıttınız.

:::image type="content" source="media/avs-backup/monitor-backup-jobs-in-mabs.png" alt-text="Azure Backup Sunucusu 'de yedekleme işlerinin durumunu izleme":::

## <a name="restore-vmware-virtual-machines"></a>VMware sanal makinelerini geri yükleme

Azure Backup Sunucusu Yönetici Konsolu, kurtarılabilir verileri bulmanın iki yolu vardır-arama veya tarama. Verileri kurtarırken, verileri veya bir VM 'yi aynı konuma geri yüklemek isteyebilirsiniz. Bu nedenle Azure Backup Sunucusu, VMware VM yedeklemeleri için üç kurtarma seçeneğini destekler:

- **Özgün konum kurtarma (olr)** -KORUMALı bir VM 'yi özgün konumuna geri yüklemek için olr 'yi kullanın. Bir VM 'yi yalnızca, yedekleme gerçekleşmesinden sonra hiç disk eklenmediyse veya silinmediyse, özgün konumuna geri yükleyebilirsiniz. Diskler eklendiyse veya silinirse, alternatif bir konum kurtarma kullanmanız gerekir.

- **Alternatif Konum Kurtarma (ALR)** -özgün VM eksik olduğunda veya özgün VM 'yi rahatsız etmek ISTEMIYORSANıZ, VM 'yi alternatif bir konuma kurtarın. Bir VM 'yi alternatif bir konuma kurtarmak için bir ESXi konağının, kaynak havuzunun, klasörün ve depolama veri deposunun ve yolun konumunu sağlamanız gerekir. Geri yüklenen VM 'yi özgün VM 'den ayırt etmeye yardımcı olmak için, Azure Backup Sunucusu "-Kurtarılan" öğesini VM adına ekler.

- **Tek dosya konumu kurtarma (ıLR)** -korunan VM bir Windows Server sanal makinesi Ise, sanal makine içindeki tek tek dosyalar/klasörler Azure Backup sunucusu ILR özelliği kullanılarak kurtarılabilir. Tek tek dosyaları kurtarmak için bu makalenin ilerleyen kısımlarında yer alarak bulunan yordama bakın. Bir VM 'den tek bir dosyanın geri yüklenmesi yalnızca Windows VM ve disk kurtarma noktaları için kullanılabilir.

### <a name="restore-a-recovery-point"></a>Kurtarma noktasını geri yükleme

1. Azure Backup Sunucusu Yönetici Konsolu kurtarma görünümü ' ne tıklayın. 

1. Kurtarmak istediğiniz VM 'yi bulmak için Tarama bölmesini, gözatmaya veya filtrelemenizi kullanın. Bir VM veya klasör seçtiğinizde, bölmedeki Kurtarma noktaları kullanılabilir kurtarma noktalarını görüntüler.

   ![Kullanılabilir kurtarma noktaları](../backup/media/restore-azure-backup-server-vmware/recovery-points.png)

1. **Kurtarma noktaları** alanında, bir kurtarma noktasının alındığı tarih seçmek için Takvim ve açılır menüleri kullanın. Kalın yazı tipinde takvim tarihleri kullanılabilir kurtarma noktalarına sahip. Alternatif olarak, VM 'ye sağ tıklayıp **tüm kurtarma noktalarını göster** ' i seçip listeden kurtarma noktasını seçebilirsiniz.

   > [!NOTE] 
   > Kısa süreli koruma için daha hızlı kurtarma için disk tabanlı bir kurtarma noktası seçin. Kısa vadeli kurtarma noktalarının süresi dolduktan sonra, kurtarmak için yalnızca çevrimiçi kurtarma noktaları görürsünüz.

1. Çevrimiçi kurtarma noktasından kurtarmadan önce, hazırlama konumunun kurtarmak istediğiniz sanal makinenin tam sıkıştırılmamış boyutunu barındırmak için yeterli boş alan içerdiğinden emin olun. Hazırlama konumu, **abonelik ayarlarını yapılandırma Sihirbazı**çalıştırılarak görüntülenebilir/değiştirilebilir.

   :::image type="content" source="media/avs-backup/mabs-recovery-folder-settings.png" alt-text="Kurtarma klasörü ayarlarını Azure Backup Sunucusu":::

1. **Kurtarma Sihirbazı 'nı**açmak için **kurtar** ' ı tıklatın.

   ![Kurtarma Sihirbazı, kurtarma seçimini ıncele](../backup/media/restore-azure-backup-server-vmware/recovery-wizard.png)

1. **Kurtarma seçeneklerini belirtin** ekranına Ilerlemek için **İleri** ' ye tıklayın ve **Kurtarma türü seçin** ekranına ilerlemek için **İleri** ' ye tıklayın. 

   > [!NOTE]
   > VMware iş yükleri ağ bant genişliği azaltmayı etkinleştirmeyi desteklemez.

1. **Kurtarma türünü seçin** sayfasında, özgün örneğe mi yoksa yeni bir konuma mi kurtarılacağı seçin ve ardından **İleri**' ye tıklayın.

   - **Özgün örneğe kurtar**' ı seçerseniz, sihirbazda başka seçimler yapmanıza gerek kalmaz. Özgün örnek için veriler kullanılır.

   - **Herhangi bir konakta sanal makine olarak kurtar**' ı seçerseniz, **hedef belirtin** ekranında **ESXi Konağı, kaynak havuzu, klasör**ve **yol**bilgilerini girin.

   ![Kurtarma türünü seçin](../backup/media/restore-azure-backup-server-vmware/recovery-type.png)

1. **Özet** sayfasında, ayarlarınızı gözden geçirin ve kurtarma işlemini başlatmak için **kurtar** ' ı tıklatın. 

   Kurtarma durumu ekranı, kurtarma işleminin ilerlemesini gösterir.

### <a name="restore-an-individual-file-from-a-vm"></a>Bir VM 'den tek bir dosyayı geri yükleme

Korunan bir VM kurtarma noktasından tek tek dosyaları geri yükleyebilirsiniz. Bu özellik yalnızca Windows Server VM 'Leri için kullanılabilir. Tek tek dosyaları geri yükleme, VMDK 'ye gözatıp kurtarma işlemini başlatmadan önce istediğiniz dosya (ler) i bulmanıza benzer şekilde, tüm VM 'yi geri yüklemeyle benzerdir. 

> [!NOTE]
> Bir VM 'den tek bir dosyanın geri yüklenmesi yalnızca Windows VM ve disk kurtarma noktaları için kullanılabilir.

1. Azure Backup Sunucusu Yönetici Konsolu **Kurtarma** görünümü ' ne tıklayın.

1. Kurtarmak istediğiniz VM 'yi bulmak için, **tarama** bölmesini kullanın veya filtre uygulayın. Bir VM veya klasör seçtiğinizde, bölmedeki Kurtarma noktaları kullanılabilir kurtarma noktalarını görüntüler.

   ![Kullanılabilir kurtarma noktaları](../backup/media/restore-azure-backup-server-vmware/vmware-rp-disk.png)

1. **Için kurtarma noktaları** bölmesinde, istenen kurtarma noktası (ler) i içeren tarihi seçmek için takvimi kullanın. Yedekleme ilkesinin nasıl yapılandırıldığına bağlı olarak, tarihler birden fazla kurtarma noktasına sahip olabilir. 

1. Kurtarma noktasının alındığı günü seçtikten sonra, doğru **kurtarma süresini**seçtiğinizden emin olun. 

   > [!NOTE]
   > Seçilen tarihin birden çok kurtarma noktası varsa, kurtarma zamanı açılır menüsünde bunu seçerek kurtarma noktanızı seçin. 

   Kurtarma noktasını seçtikten sonra, kurtarılabilir öğelerin listesi **yol** bölmesinde görünür.  

1. Kurtarmak istediğiniz dosyaları bulmak için, **yol** bölmesinde **kurtarılabilir öğe** sütunundaki öğeye çift tıklayarak dosyayı açın ve kurtarmak istediğiniz dosya veya klasörleri seçin. Birden çok öğe seçmek için, her öğeyi seçerken **CTRL** tuşuna basın. **Kurtarılabilir öğe** sütununda görünen dosya veya klasör listesinde arama yapmak için **yol** bölmesini kullanın.
    
   > [!NOTE]
   > **Aşağıdaki arama listesi** alt klasörlerde arama yapmaz. Alt klasörlerde arama yapmak için klasörü çift tıklayın. Alt bir klasörden üst klasöre geçmek için **yukarı** düğmesini kullanın. Birden çok öğe (dosya ve klasör) seçebilirsiniz, ancak aynı ana klasörde olmaları gerekir. Aynı kurtarma işinde birden çok klasörden öğe kurtaramazsınız.

   ![Kurtarma seçimini İncele](../backup/media/restore-azure-backup-server-vmware/vmware-rp-disk-ilr-2.png)

1. Kurtarma için öğe (ler) i seçtiğinizde, Yönetici Konsolu araç şeridinde **Kurtarma Sihirbazı**'nı açmak için **kurtar** ' ı tıklatın. Kurtarma Sihirbazı 'nda, **Kurtarma seçimini İncele** ekranı, kurtarılacak seçili öğeleri gösterir.

1. **Kurtarma seçeneklerini belirtin** ekranında, aşağıdakilerden birini yapın:

   - Ağ bant genişliği azaltmayı etkinleştirmek için **Değiştir** ' e tıklayın.  Kısıtlama iletişim kutusunda, açmak için **ağ bant genişliği kullanımını azaltmayı etkinleştir** ' i seçin. Etkinleştirildikten sonra **ayarları** ve **iş zamanlamasını**yapılandırın.
    
   - Ağ azaltmayı devre dışı bırakmak için **İleri** 'ye tıklayın.

1. **Kurtarma türünü seçin** ekranında **İleri**' ye tıklayın. Yalnızca dosya veya klasör (ler) i bir ağ klasörüne kurtarabilirsiniz.

1. **Hedef belirtin** ekranında, dosyalarınız veya klasörleriniz için bir ağ konumu bulmak üzere **Araştır** ' a tıklayın. Azure Backup Sunucusu, kurtarılan tüm öğelerin kopyalandığı bir klasör oluşturur. Klasör adı, MABS_day-ay-yıl ön ekine sahiptir. Kurtarılan dosyalar veya klasör için bir konum seçtiğinizde, bu konumun (hedef, hedef yol ve kullanılabilir alan) ayrıntıları sağlanır.

   ![Dosyaların kurtarılacağı konumu belirtin](../backup/media/restore-azure-backup-server-vmware/specify-destination.png)

1. **Kurtarma seçeneklerini belirtin** ekranında, hangi güvenlik ayarının uygulanacağını seçin. Ağ bant genişliği kullanımını azaltmayı tercih edebilirsiniz, ancak daraltma varsayılan olarak devre dışıdır. Ayrıca, **San kurtarma** ve **bildirim** etkin değildir.

1. **Özet** ekranında, ayarlarınızı gözden geçirin ve kurtarma işlemini başlatmak için **kurtar** ' ı tıklatın. **Kurtarma durumu** ekranı, kurtarma işleminin ilerlemesini gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Yedeklemeleri ayarlarken oluşan sorunları gidermek için, Azure Backup Sunucusu için sorun giderme kılavuzunu gözden geçirin.



> [!div class="nextstepaction"]
> [Azure Backup Sunucusu için sorun giderme kılavuzu](../backup/backup-azure-mabs-troubleshoot.md)

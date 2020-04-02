---
title: Azure Yedekleme Sunucusu ile VMware VM'leri yedekleme
description: Bu makalede, VMware vCenter/ESXi sunucusunda çalışan VMware VM'leri yedeklemek için Azure Yedekleme Sunucusu'nun nasıl kullanılacağını öğrenin.
ms.topic: conceptual
ms.date: 12/11/2018
ms.openlocfilehash: 951016d393b095b0329ff18861421402e0e18a1a
ms.sourcegitcommit: c5661c5cab5f6f13b19ce5203ac2159883b30c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80529501"
---
# <a name="back-up-vmware-vms-with-azure-backup-server"></a>Azure Yedekleme Sunucusu ile VMware VM'leri yedekleme

Bu makalede, VMware ESXi hosts/vCenter Server'da çalışan VMware VM'lerin Azure Yedekleme Sunucusu'nü kullanarak Azure'a nasıl yedekleneneğini açıklanmaktadır.

Bu makalede, nasıl açıklanmaktadır:

- Azure Yedekleme Sunucusu'nun HTTPS üzerinden VMware sunucularıyla iletişim kurabilmesi için güvenli bir kanal kurun.
- Azure Yedekleme Sunucusu'nun VMware sunucusuna erişmek için kullandığı bir VMware hesabı ayarlayın.
- Hesap kimlik bilgilerini Azure Yedekleme'ye ekleyin.
- vCenter veya ESXi sunucusunu Azure Yedekleme Sunucusu'na ekleyin.
- Yedeklemek istediğiniz VMware VM'leri içeren bir koruma grubu ayarlayın, yedekleme ayarlarını belirtin ve yedeklemeyi zamanlayın.

## <a name="before-you-start"></a>Başlamadan önce

- VCenter/ESXi'nin yedekleme için desteklenen bir sürümünü çalıştırdığınızı doğrulayın. [Destek](https://docs.microsoft.com/azure/backup/backup-mabs-protection-matrix)matrisine buradan bakın.
- Azure Yedekleme Sunucusu'u ayarladığınızdan emin olun. Eğer yapmadıysan, başlamadan önce [bunu yap.](backup-azure-microsoft-azure-backup.md) En son güncelleştirmelerle Azure Yedekleme Sunucusu'nu çalıştırıyor olmalısınız.

## <a name="create-a-secure-connection-to-the-vcenter-server"></a>vCenter Server'a güvenli bağlantı oluşturma

Varsayılan olarak, Azure Yedekleme Sunucusu HTTPS üzerinden VMware sunucularıyla iletişim kurar. HTTPS bağlantısını kurmak için VMware Sertifika Yetkilisi (CA) sertifikasını indirin ve Azure Yedekleme Sunucusu'ndan aktarın.

### <a name="before-you-begin"></a>Başlamadan önce

- HTTPS kullanmak istemiyorsanız, [tüm VMware sunucuları için HTTPS sertifika doğrulamasını devre dışı kullanabilirsiniz.](backup-azure-backup-server-vmware.md#disable-https-certificate-validation)
- Genellikle Azure Yedekleme Sunucusu makinesindeki bir tarayıcıdan vSphere Web Client'ı kullanarak vCenter/ESXi sunucusuna bağlanırsınız. Bunu ilk yaptığınızda, bağlantı güvenli değildir ve aşağıdakileri gösterir.
- Azure Yedekleme Sunucusu'nun yedeklemeleri nasıl işleyeceğini anlamak önemlidir.
  - İlk adım olarak Azure Yedekleme Sunucusu verileri yerel disk depolamaya yedekler. Azure Yedekleme Sunucusu, azure yedekleme sunucusunun korumalı verileri için disk kurtarma noktalarını depoladığı bir depolama havuzu, bir disk ler ve birimler kümesi kullanır. Depolama havuzu doğrudan depolama (DAS), bir fiber kanal SAN veya iSCSI depolama cihazı veya SAN eklenebilir. VMware VM verilerinizin yerel yedeklemesi için yeterli depolama alanına sahip olduğundan emin olmak önemlidir.
  - Azure Yedekleme Sunucusu daha sonra yerel disk depolamadan Azure'a yedekler.
  - Ne kadar depolama alanına ihtiyacınız olduğunu anlamak için [yardım alın.](https://docs.microsoft.com/system-center/dpm/create-dpm-protection-groups?view=sc-dpm-1807#figure-out-how-much-storage-space-you-need) Bilgiler DPM içindir, ancak Azure Yedekleme Sunucusu için de kullanılabilir.

### <a name="set-up-the-certificate"></a>Sertifikayı ayarlama

Aşağıdaki gibi güvenli bir kanal ayarlayın:

1. Azure Yedekleme Sunucusu'ndaki tarayıcıya vSphere Web İstemci URL'sini girin. Giriş sayfası görünmüyorsa, bağlantı ve tarayıcı proxy ayarlarını doğrulayın.

    ![vSphere Web İstemci](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

2. vSphere Web Client giriş sayfasında, **güvenilir kök CA sertifikalarını karşıdan yükleyin'i**tıklatın.

    ![Güvenilir kök CA sertifikasını indirin](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

3. **İndiradlı** dosya indirilir. Tarayıcınıza bağlı olarak, dosyayı açıp açmamanızı veya kaydetmenizi isteyen bir ileti alırsınız.

    ![CA sertifikasını indirin](./media/backup-azure-backup-server-vmware/download-certs.png)

4. Dosyayı .zip uzantılı Azure Yedekleme Sunucusu makinesine kaydedin.

5. Sağ tıklayın **download.zip** > **Extract Tüm**. .zip dosyası içeriğini **sertifikaklasörüne** ayıklar ve şunları içerir:
   - 0,0 ve .1 gibi numaralanmış bir sırayla başlayan uzantılı kök sertifika dosyası.
   - CRL dosyası,.r0 veya .r1 gibi bir diziyle başlayan bir uzantıya sahiptir. CRL dosyası bir sertifikayla ilişkilidir.

    ![İndirilen sertifikalar](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

6. **Sertifikalar** klasöründe, yeniden **adlandırma>** kök sertifika dosyasına sağ tıklayın.

    ![Kök sertifikasını yeniden adlandır](./media/backup-azure-backup-server-vmware/rename-cert.png)

7. Kök sertifikanın uzantısını .crt olarak değiştirin ve onaylayın. Dosya simgesi kök sertifikayı temsil eden simgeye dönüşür.

8. Kök sertifikayı sağ tıklatın ve açılır menüden **Sertifika Yı yükle'yi**seçin.

9. **Sertifika Alma Sihirbazı'nda,** sertifikanın hedefi olarak **Yerel Makine'yi** seçin ve sonra **İleri'yi**tıklatın. Bilgisayarda değişiklik yapılmasına izin vermek isteyip istemediğiniz sorulur doğrulayın.

    ![Sihirbaz Hoşgeldiniz](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

10. Sertifika **Deposu** sayfasında, **aşağıdaki mağazadaki tüm sertifikaları yerleştir'i**seçin ve ardından sertifika deposunu seçmek için **Gözat'ı** tıklatın.

    ![Sertifika depolama](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

11. **Sertifika Deposu'nu**Seç'te, sertifikalar için hedef klasör olarak **Güvenilir Kök Sertifika Yetkilileri'ni** seçin ve ardından **Tamam'ı**tıklatın.

    ![Sertifika hedef klasörü](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

12. **Sertifika Alma Sihirbazı'nı Tamamlarken,** klasörü doğrulayın ve sonra **Bitir'i**tıklatın.

    ![Sertifikanın doğru klasörde olduğunu doğrula](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

13. Sertifika alma onaylandıktan sonra, bağlantınızın güvenli olduğunu doğrulamak için vCenter Server'da oturum açın.

### <a name="disable-https-certificate-validation"></a>HTTPS sertifika doğrulamayı devre dışı bırakma

Kuruluşunuz içinde güvenli sınırlarınız varsa ve VMware sunucuları ile Azure Yedekleme Sunucusu makinesi arasındaki HTTPS protokolünü kullanmak istemiyorsanız, HTTPS'yi aşağıdaki gibi devre dışı tutun:

1. Aşağıdaki metni bir .txt dosyasına kopyalayıp yapıştırın.

```text
Windows Registry Editor Version 5.00
[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
"IgnoreCertificateValidation"=dword:00000001
```

2. Dosyayı **DisableSecureAuthentication.reg**adıyla Azure Yedekleme Sunucusu makinesine kaydedin.

3. Kayıt defteri girişini etkinleştirmek için dosyayı çift tıklatın.

## <a name="create-a-vmware-role"></a>VMware rolü oluşturma

Azure Yedekleme Sunucusu'nun v-Center Server/ESXi ana bilgisayarına erişmek için izinlere sahip bir kullanıcı hesabına ihtiyacı vardır. Belirli ayrıcalıklara sahip bir VMware rolü oluşturun ve ardından bir kullanıcı hesabını rolile ilişkilendirin.

1. vCenter Server'da oturum açın (veya vCenter Server kullanmıyorsanız ESXi ana bilgisayar).
2. **Navigator** panelinde **Yönetim'i**tıklatın.

    ![Yönetim](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

3. **Yönetim** > **Rolleri'nde**rol ekle simgesini (+ sembolü) tıklatın.

    ![Rol ekleme](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

4.  > **Rol Adı** **Oluştur'da** *BackupAdminRole*girin. Rol adı istediğiniz gibi olabilir, ancak rolün amacı için tanınabilir olmalıdır.

5. Aşağıdaki tabloda özetlenen ayrıcalıkları seçin ve sonra **Tamam'ı**tıklatın.  Yeni rol **Roller** panelindeki listede görünür.
   - Üst öğeyi genişletmek ve alt ayrıcalıkları görüntülemek için üst etiketin yanındaki simgeyi tıklatın.
   - VirtualMachine ayrıcalıklarını seçmek için, üst alt hiyerarşisine birkaç düzey girmeniz gerekir.
   - Ebeveyn ayrıcalığı içindeki tüm alt ayrıcalıkları seçmeniz gerekmez.

    ![Üst alt ayrıcalık hiyerarşisi](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

### <a name="role-permissions"></a>Rol izinleri

| **vCenter 6.7 kullanıcı hesabı için ayrıcalıklar**              | **vCenter 6.5 kullanıcı hesabı için ayrıcalıklar**             |
| --------------------------------------------------------- | -------------------------------------------------------- |
| Datastore.Allocate Alanı                                  | Datastore.Allocate Alanı                                 |
| Global.Log Etkinliği                                          | Global.Log Etkinliği                                         |
| Global.Manage Özel Öznitelikleri                           | Global.Manage Özel Öznitelikleri                          |
| Network.Ata                                            | Network.Ata                                           |
| Kaynak. Sanal Makineyi Kaynak havuzuna atama        | Kaynak. Sanal Makineyi Kaynak havuzuna atama       |
| VirtualMachine.Configuration.AddNewDisk                   | VirtualMachine.Configuration.AddNewDisk                  |
| VirtualMachine.Configuration. Aygıt Ekle veya Kaldır       | VirtualMachine.Configuration. Aygıt Ekle veya Kaldır      |
| VirtualMachine.Configuration.Advanced                     | VirtualMachine.Configuration.Advanced                    |
| VirtualMachine.Configuration.Toggle Disk Değiştirme İzleme | VirtualMachine.Configuration.Disk Değiştirme İzleme       |
| VirtualMachine.Configuration.Configure Host USB Aygıtı   | VirtualMachine.Configuration.Host USB Cihazı            |
| VirtualMachine.Configuration.Query SahipSiz Dosyalar         | VirtualMachine.Configuration.Query SahipSiz Dosyalar        |
| VirtualMachine.Configuration.Change Swapfile Yerleştirme   | VirtualMachine.Configuration.Swapfile Yerleştirme         |
| VirtualMachine.Interaction.Power Off                      | VirtualMachine.Interaction.Power Off                     |
| VirtualMachine.Inventory.Create Yeni                       | VirtualMachine.Inventory.Create Yeni                      |
| VirtualMachine.Provisioning.Allow Disk Erişimi            | VirtualMachine.Provisioning.Allow Disk Erişimi           |
| VirtualMachine.Provisioning.Allow Dosya Erişimi            | VirtualMachine.Provisioning.Allow Dosya Erişimi           |
| VirtualMachine.Provisioning.Allow Read-only Disk Erişimi  | VirtualMachine.Provisioning.Allow Read-only Disk Erişimi |
| VirtualMachine.Snapshot Management.Create Snapshot       | VirtualMachine.Snapshot Management.Create Snapshot      |
| VirtualMachine.Snapshot Management.Remove Snapshot       | VirtualMachine.Snapshot Management.Remove Snapshot      |

<br>

| **vCenter 6.0 kullanıcı hesabı için ayrıcalıklar**                | **vCenter 5.5 kullanıcı hesabı için ayrıcalıklar** |
| ---------------------------------------------------------- | ------------------------------------------- |
| Datastore.AllocateSpace                                    | Network.Ata                              |
| Global.Manage özel öznitelikleri                           | Datastore.AllocateSpace                     |
| Global.Set özel öznitelik                               | VirtualMachine.Config.ChangeTracking        |
| Host.Yerel işlemler. Sanal makine oluşturma              | VirtualMachine.State.RemoveSnapshot         |
| Ağ.  Ağ atama                                   | VirtualMachine.State.CreateSnapshot         |
| Kaynak.  Kaynak havuzuna sanal makine atama         | VirtualMachine.Provisioning.DiskRandomRead  |
| Sanal makine. Configuration.Add new disk                | VirtualMachine.Interact.PowerOff            |
| Sanal makine. Configuration.Advanced                    | VirtualMachine.Inventory.Create             |
| Sanal makine. Configuration.Disk değişikliği izleme        | VirtualMachine.Config.AddNewDisk            |
| Sanal makine. Configuration.Host USB aygıtı             | VirtualMachine.Config.HostUSBDevice         |
| Sanal makine. Configuration.Query sahipsiz dosyaları         | VirtualMachine.Config.AdvancedConfig        |
| Sanal makine. Configuration.Swapfile yerleşimi          | VirtualMachine.Config.SwapPlacement         |
| Sanal makine. Interaction.Power Off                     | Global.ManageCustomFields                   |
| Sanal makine. Stok. Yeni oluştur                     |                                             |
| Sanal makine. Provisioning.Allow disk erişimi            |                                             |
| Sanal makine. Sağlama. Salt okunur disk erişimine izin verme |                                             |
| Sanal makine. Anlık görüntü yönetimi. Anlık görüntü oluşturma       |                                             |
| Sanal makine. Anlık görüntü yönetimi. Anlık Görüntü'yi Kaldırma       |                                             |



## <a name="create-a-vmware-account"></a>Bir VMware hesabı oluşturma

1. vCenter Server **Navigator** panelinde **Kullanıcılar ve Gruplar'ı**tıklatın. vCenter Server kullanmıyorsanız, hesabı uygun ESXi ana bilgisayarda oluşturun.

    ![Kullanıcılar ve Gruplar seçeneği](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    **vCenter Kullanıcıları ve Grupları** paneli görüntülenir.

2. **vCenter Kullanıcıları ve Grupları** panelinde, **Kullanıcılar** sekmesini seçin ve ardından kullanıcı ekle simgesini (+ simgesi) tıklatın.

    ![vCenter Kullanıcıları ve Gruplar paneli](./media/backup-azure-backup-server-vmware/usersandgroups.png)

3. **Yeni Kullanıcı** iletişim kutusunda, **Ok'>** kullanıcı bilgilerini ekleyin. Bu yordamda, kullanıcı adı BackupAdmin'dir.

    ![Yeni Kullanıcı iletişim kutusu](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

4. Kullanıcı hesabını **Navigator** panelindeki rolile ilişkilendirmek için **Genel İzinler'i**tıklatın. Genel **İzinler** panelinde, **Yönet** sekmesini seçin ve ardından ekle simgesini (+ simgesi) tıklatın.

    ![Genel İzinler paneli](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

5. **Genel İzin Kökünde - İzin Ekle**, kullanıcıyı veya grubu seçmek için **Ekle'yi** tıklatın.

    ![Kullanıcı veya grup seçin](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

6. **Select Users/Groups**'da **BackupAdmin** > **Ekle'yi**seçin. **Kullanıcılarda,** kullanıcı hesabı için *etki alanı\kullanıcı adı* biçimi kullanılır. Farklı bir etki alanı kullanmak istiyorsanız, **etki alanı** listesinden seçin. Seçili kullanıcıları **İzin Ekle** iletişim kutusuna eklemek için **Tamam'ı** tıklatın.

    ![BackupAdmin kullanıcı ekle](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

7. **Atanan Rol'da,** açılan listeden **BackupAdminRole** > **Ok'u**seçin.

    ![Kullanıcıyı role atama](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

**Genel İzinler** panelindeki **Yönet** sekmesinde, yeni kullanıcı hesabı ve ilişkili rol listede görünür.

## <a name="add-the-account-on-azure-backup-server"></a>Azure Yedekleme Sunucusu'nda hesap ekleme

1. Azure Yedekleme Sunucusu'nı açın. Simgeyi masaüstünde bulamıyorsanız, uygulamalar listesinden Microsoft Azure Yedekleme'yi açın.

    ![Azure Yedekleme Sunucusu simgesi](./media/backup-azure-backup-server-vmware/mabs-icon.png)

2. Azure Yedekleme Sunucusu konsolunda, **Yönetim** >  **Üretim Sunucuları** > **Yönet VMware'i**tıklatın.

    ![Azure Yedekleme Sunucusu konsolu](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

3. Kimlik **Bilgilerini Yönet** iletişim kutusunda **Ekle'yi**tıklatın.

    ![Azure Yedekleme Sunucusu Kimlik Bilgilerini Yönet iletişim kutusu](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

4. **Kimlik Bilgisi Ekle'de,** yeni kimlik bilgisi için bir ad ve açıklama girin ve VMware sunucusunda tanımladığınız kullanıcı adını ve parolayı belirtin. Adı, *Contoso Vcenter kimlik bu* yordamda kimlik tanımlamak için kullanılır. VMware sunucusu ve Azure Yedekleme Sunucusu aynı etki alanında değilse, etki alanını kullanıcı adına belirtin.

    ![Azure Yedekleme Sunucusu Kimlik Bilgileri ekle iletişim kutusu](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

5. Yeni kimlik bilgisi eklemek için **Ekle'yi** tıklatın.

    ![Azure Yedekleme Sunucusu Kimlik Bilgilerini Yönet iletişim kutusu](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

## <a name="add-the-vcenter-server"></a>vCenter Sunucusu ekle

vCenter Server'ı Azure Yedekleme Sunucusu'na ekleyin.

1. Azure Yedekleme Sunucusu konsolunda **Yönetim** > **Üretim Sunucuları** > **Ekle'yi**tıklatın.

    ![Açık Üretim Sunucu Ekleme Sihirbazı](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

2. **Üretim Sunucusu Ekleme Sihirbazı'nda** > Üretim Sunucusu türü sayfasını**seçin,** **VMware Sunucularını**seçin ve **ardından İleri'yi**tıklatın.

    ![Üretim Sunucusu Ekleme Sihirbazı](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

3. **Select Computers****Server Name/IP Adresi'nde**VMware sunucusunun FQDN veya IP adresini belirtin.   Tüm ESXi sunucuları aynı vCenter tarafından yönetiliyorsa, vCenter adını belirtin. Aksi takdirde, ESXi ana bilgisayar ekleyin.

    ![VMware sunucusubelirtin](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. **SSL Bağlantı**Noktası'nda, VMware sunucusuyla iletişim kurmak için kullanılan bağlantı noktasını girin. 443 varsayılan bağlantı noktasıdır, ancak VMware sunucunuz farklı bir bağlantı noktasında dinliyorsa bunu değiştirebilirsiniz.

5. **Kimlik Bilgisi Belirt'** te, daha önce oluşturduğunuz kimlik bilgisini seçin.

    ![Kimlik bilgisi belirtin](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. VMware sunucusunu sunucular listesine eklemek için **Ekle'yi** tıklatın. Ardından **İleri**’ye tıklayın.

    ![VMWare sunucusu ve kimlik bilgisi ekleme](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. **Özet** sayfasında, VMware sunucusunu Azure Yedekleme Sunucusu'na eklemek için **Ekle'yi** tıklatın. Yeni sunucu hemen eklenir, VMware sunucusunda herhangi bir aracı gerekmez.

    ![Azure Yedekleme Sunucusuna VMware sunucusu ekleme](./media/backup-azure-backup-server-vmware/tasks-screen.png)

8. **Bitiş** sayfasındaki ayarları doğrulayın.

   ![Sayfayı bitir](./media/backup-azure-backup-server-vmware/summary-screen.png)

vCenter sunucusu tarafından yönetilmeyen birden çok ESXi ana bilgisayarnız varsa veya birden çok vCenter Server örneğiniz varsa, sunucuları eklemek için sihirbazı yeniden çalıştırmanız gerekir.

## <a name="configure-a-protection-group"></a>Koruma grubunu yapılandırma

Yedekleme için VMware VM'ler ekleyin. Koruma grupları birden çok VM toplar ve gruptaki tüm VM'lere aynı veri saklama ve yedekleme ayarlarını uygular.

1. Azure Yedekleme Sunucusu konsolunda **Koruma**, **> Yeni'yi**tıklatın.

    ![Yeni Koruma Grubu Oluştur sihirbazını aç](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

1. Yeni **Koruma Grubu Oluştur** sihirbazı karşılama sayfasında **İleri'yi**tıklatın.

    ![Yeni Koruma Grubu sihirbazı iletişim kutusu oluşturma](./media/backup-azure-backup-server-vmware/protection-wizard.png)

1. Koruma **grubu türünü seç** sayfasında **Sunucular'ı** seçin ve **ardından İleri'yi**tıklatın. **Grup üyelerini seç** sayfası görüntülenir.

1. **Grup üyelerini seç'te,** yedeklemek istediğiniz VM'leri (veya VM klasörlerini) seçin. Ardından **İleri**’ye tıklayın.

    - Bir klasör seçtiğinizde veya bu klasörün içindeki VM'ler veya klasörler yedekleme için de seçilir. Yedeklemek istemediğiniz klasörlerin veya VM'lerin denetimini kaldırabilirsiniz.
1. Bir VM veya klasör zaten yedekleniyorsa, onu seçemezsiniz. Bu, yinelenen kurtarma noktalarının bir VM için oluşturulmamasını sağlar.

    ![Grup üyelerini seçin](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

1. **Veri Koruma Yöntemi seç** sayfasında, koruma grubu ve koruma ayarları için bir ad girin. Azure'a yedeklemek için **Disk'e** kısa süreli koruma ayarlayın ve çevrimiçi korumayı etkinleştirin. Ardından **İleri**’ye tıklayın.

    ![Veri koruma yöntemini seçme](./media/backup-azure-backup-server-vmware/name-protection-group.png)

1. **Kısa Vadeli Hedefler Belirt'** te, verileri diske yedeklemek için ne kadar süreyle tutmak istediğinizi belirtin.
   - **Bekletme**Aralığı'nda, disk kurtarma noktalarının kaç gün tutulması gerektiğini belirtin.
   - **Eşitleme sıklığında,** disk kurtarma noktalarının ne sıklıkta alındığını belirtin.
       - Yedekleme aralığı ayarlamak istemiyorsanız, her kurtarma noktası zamanlanmadan hemen önce bir yedeklemenin çalıştırılması için **kurtarma noktasından hemen önce** kontrol edebilirsiniz.
       - Kısa vadeli yedeklemeler tam yedeklemedir ve artımlı değildir.
       - Kısa süreli yedeklemelerin gerçekleştiği saatleri/tarihleri değiştirmek için **Değiştir'i** tıklatın.

         ![Kısa vadeli hedefler belirtin](./media/backup-azure-backup-server-vmware/short-term-goals.png)

1. **Gözden Geçir Disk Ayırma'da,** VM yedeklemeleri için sağlanan disk alanını gözden geçirin. VM'ler için.

   - Önerilen disk ayırmaları, belirttiğiniz bekletme aralığını, iş yükünün türünü ve korunan verilerin boyutunu temel atanır. Gerekli değişiklikleri yapın ve sonra **İleri'yi**tıklatın.
   - **Veri boyutu:** Koruma grubundaki verilerin boyutu.
   - **Disk alanı:** Koruma grubu için önerilen disk alanı miktarı. Bu ayarı değiştirmek istiyorsanız, her veri kaynağının büyüdüğünü tahmin ettiğiniz miktardan biraz daha büyük toplam alan ayırmanız gerekir.
   - **Verileri bir ertele:** Birlikte konumu açarsanız, korumadaki birden çok veri kaynağı tek bir yineleme ve kurtarma noktası hacmiyle eşlenebilir. Birlikte konum tüm iş yükleri için desteklenmez.
   - **Otomatik olarak büyür:** Bu ayarı açarsanız, korumalı gruptaki veriler ilk ayırmayı büyütüyorsa, Azure Yedekleme Sunucusu disk boyutunu yüzde 25 artırmaya çalışır.
   - **Depolama havuzu ayrıntıları:** Toplam ve kalan disk boyutu da dahil olmak üzere depolama havuzunun durumunu gösterir.

    ![Disk tahsisini gözden geçirme](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

1. **Çoğaltma Oluşturma Yöntemini Seç** sayfasında, ilk yedeklemeyi nasıl almak istediğinizi belirtin ve sonra **İleri'yi**tıklatın.
   - Varsayılan **ağ üzerinden otomatik olarak** ve **Şimdi**.
   - Varsayılanı kullanıyorsanız, yoğun olmayan bir saat belirtmenizi öneririz. **Daha sonra** seçin ve bir gün ve saat belirtin.
   - Büyük miktarda veri veya en iyi olmayan ağ koşulları için, çıkarılabilir ortam kullanarak verileri çevrimdışı olarak çoğaltmayı düşünün.

    ![Yineleme oluşturma yöntemini seçin](./media/backup-azure-backup-server-vmware/replica-creation.png)

1. **Tutarlılık Denetim Seçenekleri'nde,** tutarlılık denetimlerini nasıl ve ne zaman otomatikleştireceklerini seçin. Ardından **İleri**’ye tıklayın.
      - Yineleme verileri tutarsız olduğunda veya ayarlanmış bir zamanlamada tutarlılık denetimleri çalıştırabilirsiniz.
      - Otomatik tutarlılık denetimlerini yapılandırmak istemiyorsanız, el ile denetleme yapabilirsiniz. Bunu yapmak **için, Tutarlılık Denetimi Gerçekleştir>** koruma grubuna sağ tıklayın.

1. **Çevrimiçi Koruma Verilerini Belirt** sayfasında, yedeklemek istediğiniz VM veya VM klasörlerini seçin. Üyeleri tek tek seçebilir veya tüm üyeleri seçmek için **Tümünü Seç'i** tıklatabilirsiniz. Ardından **İleri**’ye tıklayın.

    ![Çevrimiçi koruma verilerini belirtin](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

1. Çevrimiçi **Yedekleme Çizelgesi'ni belirt** sayfasında, yerel depolamadan Azure'a verileri ne sıklıkta yedeklemek istediğinizi belirtin.

    - Veriler için bulut kurtarma noktaları zamanlamaya göre oluşturulur. Ardından **İleri**’ye tıklayın.
    - Kurtarma noktası oluşturulduktan sonra, Azure'daki Kurtarma Hizmetleri kasasına aktarılır.

    ![Çevrimiçi yedekleme zamanlamasını belirtin](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

1. Çevrimiçi **Bekletme İlkesi Belirt** sayfasında, Azure'un günlük/haftalık/aylık/yıllık yedeklemelerinden oluşturulan kurtarma noktalarını ne kadar süreyle tutmak istediğinizi belirtin. sonra **İleri'yi**tıklatın.

    - Verileri Azure'da ne kadar süreyle tutabileceğiniz konusunda herhangi bir zaman sınırı yoktur.
    - Tek sınır, korumalı örnek başına 9999'dan fazla kurtarma puanına sahip olamazsınız. Bu örnekte, korumalı örnek VMware sunucusudur.

    ![Çevrimiçi bekletme ilkesini belirtin](./media/backup-azure-backup-server-vmware/retention-policy.png)

1. **Özet** sayfasında, ayarları gözden geçirin ve ardından **Grup Oluştur'u**tıklatın.

    ![Koruma grubu üyesi ve ayar özeti](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="vmware-vsphere-67"></a>VMWare vSphere 6.7

vSphere 6.7'yi yedeklemek için aşağıdakileri yapın:

- DPM Server'da TLS 1.2'yi etkinleştirme

>[!NOTE]
>VMWare 6.7 itibaren iletişim protokolü olarak TLS etkin vardı.

- Kayıt defteri anahtarlarını aşağıdaki gibi ayarlayın:

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

## <a name="next-steps"></a>Sonraki adımlar

Yedeklemeleri ayarlarken sorun giderme sorunları için [Azure Yedekleme Sunucusu için sorun giderme kılavuzunu](./backup-azure-mabs-troubleshoot.md)inceleyin.

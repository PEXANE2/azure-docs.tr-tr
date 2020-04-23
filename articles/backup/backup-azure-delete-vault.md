---
title: Microsoft Azure Kurtarma Hizmetleri kasası silme
description: Bu makalede, bağımlılıkları nasıl kaldırabileceğinizi ve ardından Azure Yedekleme Kurtarma Hizmetleri kasasını nasıl silenöğrenin.
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 69fed6c53914ed7aa16b04b5311ec69966734f25
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82025144"
---
# <a name="delete-an-azure-backup-recovery-services-vault"></a>Azure Yedekleme Kurtarma Hizmetleri kasası silme

Bu makalede, Azure [Yedekleme](backup-overview.md) Kurtarma Hizmetleri kasasının nasıl silinir. Bağımlılıkları kaldırmak ve sonra bir kasa silme için talimatlar içerir.

## <a name="before-you-start"></a>Başlamadan önce

Korumalı sunucular veya yedekleme yönetim sunucuları gibi bağımlılıkları olan bir Kurtarma Hizmetleri kasası silemezsiniz.

- Yedekleme verileri içeren kasalar silinemez (korumayı durdurmuş ancak yedekleme verilerini korumuş olsanız bile).

- Bağımlılıklar içeren bir kasayı silerseniz, aşağıdaki ileti görüntülenir:

  ![Kasa hatasını silin.](./media/backup-azure-delete-vault/error.png)

- Bağımlılıklar içeren bir portaldan şirket içi korumalı bir öğeyi silerseniz, bir uyarı iletisi görüntülenir:

  ![Korumalı sunucu hatasını silin.](./media/backup-azure-delete-vault/error-message.jpg)

- Yedekleme öğeleri uyarı iletisi altında yumuşak silinmiş durumda ise uyarı iletisi görüntülenir ve kalıcı olarak silinene kadar beklemek zorunda kalır. Daha fazla bilgi için bu [makaleye](https://docs.microsoft.com/azure/backup/backup-azure-security-feature-cloud)bakın.

   ![Kasa hatasını silin.](./media/backup-azure-delete-vault/error-message-soft-delete.png)

- Kayıtlı depolama hesapları olan kasalar silinemez. Hesabın kaydını nasıl kınılsüreceğini öğrenmek için [bir depolama hesabının kaydını açma'ya](manage-afs-backup.md#unregister-a-storage-account)bakın.
  
Kasayı silmek için kurulumunuzla eşleşen senaryoyu seçin ve önerilen adımları izleyin:

Senaryo | Kasayı silmek için bağımlılıkları kaldırma adımları |
-- | --
Azure Yedekleme aracısını kullanarak Azure Yedekleme aracısını kullanarak azure'a yedekleme yaparak şirket içi dosyalarım ve klasörlerim var | MARS yönetim [konsolundan yedekleme öğelerini sil adımlarını gerçekleştirin](#delete-backup-items-from-the-mars-management-console)
Azure'da MABS (Microsoft Azure Yedekleme Sunucusu) veya DPM (System Center Data Protection Manager) kullanılarak korunan şirket içi makinelerim var | [MABS yönetim konsolundan yedekleme öğelerini sil adımlarını gerçekleştirin](#delete-backup-items-from-the-mabs-management-console)
Buluttaki öğeleri korudum (örneğin, bir laaS sanal makine veya Azure Dosyaları paylaşımı)  | [Buluttaki korumalı öğeleri sil](#delete-protected-items-in-the-cloud) adımlarını gerçekleştirin
Öğeleri hem tesislerinde hem de bulutta korudum | Aşağıdaki bölümlerin tümündeki adımları aşağıdaki sırayla gerçekleştirin: <br> 1. [Buluttaki korumalı öğeleri silme](#delete-protected-items-in-the-cloud)<br> 2. [Mars yönetim konsolundan yedek öğeleri silme](#delete-backup-items-from-the-mars-management-console) <br> 3. [MABS Management konsolundan yedek öğeleri silme](#delete-backup-items-from-the-mabs-management-console)
Şirket içinde veya bulutta korumalı öğelerim yok; Ancak, hala Vault silme hatası alıyorum | Azure Kaynak [Yöneticisi'ni kullanarak Kurtarma Hizmetleri kasasını sil](#delete-the-recovery-services-vault-by-using-azure-resource-manager) adımlarını gerçekleştirin <br><br> Kasaya kayıtlı depolama hesabı olmadığından emin olun. Hesabın kaydını nasıl kınılsüreceğini öğrenmek için [bir depolama hesabının kaydını açma'ya](manage-afs-backup.md#unregister-a-storage-account)bakın.

## <a name="delete-protected-items-in-the-cloud"></a>Buluttaki korumalı öğeleri silme

İlk olarak, bağımlılıkları ve kasa silme işlemini anlamak için **[başlamadan önce](#before-you-start)** bölümü okuyun.

Korumayı durdurmak ve yedekleme verilerini silmek için aşağıdaki adımları gerçekleştirin:

1. Portaldan, Kurtarma **Hizmetleri kasasına**gidin ve sonra **Yedekleme öğelerine**gidin. Ardından, buluttaki korumalı öğeleri (örneğin, Azure Sanal Makineleri, Azure Depolama [Azure Dosyaları hizmeti] veya Azure Sanal Makinelerde SQL Server' ı seçin.

    ![Yedekleme türünü seçin.](./media/backup-azure-delete-vault/azure-storage-selected.png)

2. Yedekleme öğesini seçmek için sağ tıklatın. Yedekleme öğesinin korunup korunmadığına bağlı olarak, menü **Yedeklemeyi Durdur** bölmesini veya **Yedekleme Verilerini Sil** bölmesini görüntüler.

    - **Yedeklemeyi Durdur** bölmesi görünürse, açılan menüden **Yedekleme Verilerini Sil'i** seçin. Yedekleme öğesinin adını girin (bu alan büyük/küçük harf duyarlıdır) ve ardından açılır menüden bir neden seçin. Varsa yorumlarınızı girin. Ardından **yedeklemeyi durdur'u**seçin.

        ![Yedeklemeyi Durdur bölmesi.](./media/backup-azure-delete-vault/stop-backup-item.png)

    - Yedekleme **Verilerini Sil** bölmesi görünürse, yedekleme öğesinin adını girin (bu alan büyük/küçük harf duyarlıdır) ve ardından açılır menüden bir neden seçin. Varsa yorumlarınızı girin. Ardından **Sil'i**seçin.

         ![Yedekleme Verilerini Sil bölmesi.](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

3. **Bildirim** simgesini ![kontrol edin: Bildirim simgesi.](./media/backup-azure-delete-vault/messages.png) İşlem bittikten sonra, hizmet aşağıdaki iletiyi görüntüler: Yedeklemeyi durdurma ve " Yedekleme Öğesi *"* *için yedekleme verilerini silme.* *İşlemi başarıyla tamamladı.*
4. Yedekleme öğesinin silindiğinden emin olmak için **Yedek Öğeler** menüsünde **Yenile'yi** seçin.

      ![Yedekleme Öğelerini Sil sayfası.](./media/backup-azure-delete-vault/empty-items-list.png)

## <a name="delete-protected-items-on-premises"></a>Şirket içinde korunan öğeleri silme

İlk olarak, bağımlılıkları ve kasa silme işlemini anlamak için **[başlamadan önce](#before-you-start)** bölümü okuyun.

1. Kasa panosu menüsünden **Yedek Altyapı'yı**seçin.
2. Şirket içi senaryonuza bağlı olarak, aşağıdaki seçeneklerden birini seçin:

      - MARS için **Korumalı Sunucular'ı** ve ardından **Azure Yedekleme Aracısı'nı**seçin. Ardından, silmek istediğiniz sunucuyu seçin.

        ![MARS için, panolarını açmak için kasanızı seçin.](./media/backup-azure-delete-vault/identify-protected-servers.png)

      - MABS veya DPM için **Yedek Yönetim Sunucuları'nı**seçin. Ardından, silmek istediğiniz sunucuyu seçin.

          ![MABS için, panolarını açmak için kasanızı seçin.](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. **Sil** bölmesi bir uyarı iletisiyle görüntülenir.

     ![Silme bölmesi.](./media/backup-azure-delete-vault/delete-protected-server.png)

     Uyarı iletisini ve onay onay kutusundaki yönergeleri gözden geçirin.
    > [!NOTE]
    >- Korumalı sunucu Azure hizmetleriyle eşitlenirse ve yedek öğeler varsa, onay onay kutusu bağımlı yedekleme öğelerinin sayısını ve yedek öğeleri görüntülemek için bağlantıyı görüntüler.
    >- Korumalı sunucu Azure hizmetleriyle eşitlenmezse ve yedek öğeler varsa, onay onay kutusu yalnızca yedek öğe sayısını görüntüler.
    >- Yedek öğe yoksa, onay onay kutusu silinmesini ister.

4. İzin onay kutusunu seçin ve sonra **Sil'i**seçin.

5. **Bildirim** simgesini ![kontrol](./media/backup-azure-delete-vault/messages.png)edin yedek verileri silin. İşlem bittikten sonra, hizmet iletiyi görüntüler: *Yedeklemeyi durdurma ve "Yedekleme Öğesi" için yedekleme verilerini silme.* *İşlemi başarıyla tamamladı.*
6. Yedek öğenin silindiğinden emin olmak için **Yedek Öğeler** menüsünde **Yenile'yi** seçin.

Bu işlem bittikten sonra, yedekleme öğelerini yönetim konsolundan silebilirsiniz:

- [MARS yönetim konsolundan yedekleme öğelerini silme](#delete-backup-items-from-the-mars-management-console)
- [Yedekleme öğelerini MABS yönetim konsolundan silme](#delete-backup-items-from-the-mabs-management-console)

### <a name="delete-backup-items-from-the-mars-management-console"></a>MARS yönetim konsolundan yedekleme öğelerini silme

1. MARS yönetim konsolunu açın, **Eylemler** bölmesine gidin ve **YedeklemeYi Zamanla'yı**seçin.
2. Zamanlanmış Yedekleme sayfasını **Değiştir veya Durdur'dan,** **bu yedekleme zamanlamasını kullanmayı durdur'u seçin ve depolanan tüm yedeklemeleri silin.** Ardından **İleri'yi**seçin.

    ![Zamanlanmış bir yedeklemeyi değiştirin veya durdurun.](./media/backup-azure-delete-vault/modify-schedule-backup.png)

3. **Zamanlanmış Yedeklemeyi Durdur** sayfasından **Finish'i**seçin.

    ![Zamanlanmış bir yedeklemeyi durdurun.](./media/backup-azure-delete-vault/stop-schedule-backup.png)
4. El ile oluşturmanız gereken bir güvenlik PIN'i (kişisel kimlik numarası) girmeniz istenir. Bunu yapmak için önce Azure portalında oturum açın.
5. Kurtarma **Hizmetleri kasa** > **Ayarları** > **Özellikleri'ne**gidin.
6. **Güvenlik PIN'i**altında **Oluştur'u**seçin. Bu PIN'i kopyalayın. PIN yalnızca beş dakika için geçerlidir.
7. Yönetim konsolunda PIN'i yapıştırın ve ardından **Tamam'ı**seçin.

    ![Bir güvenlik PIN'i oluşturun.](./media/backup-azure-delete-vault/security-pin.png)

8. Yedekleme **İlerlemesini Değiştir** sayfasında aşağıdaki ileti görüntülenir: *Silinen yedekleme verileri 14 gün boyunca saklanır. Bu süre den sonra, yedekleme verileri kalıcı olarak silinir.*  

    ![Yedekleme altyapısını silin.](./media/backup-azure-delete-vault/deleted-backup-data.png)

Şirket içi yedekleme öğelerini sildikten sonra portaldan sonraki adımları izleyin.

### <a name="delete-backup-items-from-the-mabs-management-console"></a>Yedekleme öğelerini MABS yönetim konsolundan silme

Yedekleme öğelerini MABS yönetim konsolundan silmek için kullanabileceğiniz iki yöntem vardır.

#### <a name="method-1"></a>Yöntem 1

Korumayı durdurmak ve yedekleme verilerini silmek için aşağıdaki adımları yapın:

1. DPM Yönetici Konsolu'nu açın ve ardından gezinti çubuğunda **Koruma'yı** seçin.
2. Ekran bölmesinde, kaldırmak istediğiniz koruma grubu üyesini seçin. **Grup Üyelerinin Korumasını Durdur** seçeneğini seçmek için sağ tıklatın.
3. **Korumayı Durdur** iletişim kutusundan, **korumalı verileri sil'i**ve ardından depolama çevrimiçi onay kutusunu **sil'i** seçin. Ardından, **Korumayı Durdur'u**seçin.

    ![Korumayı Durdur bölmesinden korumalı verileri sil'i seçin.](./media/backup-azure-delete-vault/delete-storage-online.png)

    Korumalı üye *durumu, kullanılabilir Etkin Olmayan yinelemeye*dönüşür.

4. Etkin olmayan koruma grubuna sağ tıklayın ve **etkin olmayan korumayı kaldır'ı**seçin.

    ![Etkin olmayan korumayı kaldırın.](./media/backup-azure-delete-vault/remove-inactive-protection.png)

5. Etkin **Olmayan Korumayı Sil** penceresinden, **çevrimiçi depolama yı sil** onay kutusunu seçin ve ardından **Tamam'ı**seçin.

    ![Çevrimiçi depolama alanını silin.](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

#### <a name="method-2"></a>2. Yöntem

**MABS yönetim** konsolu açın. **Veri koruma yöntemi ni seç altında,** I want çevrimiçi **koruma** onay kutusunu temizleyin.

  ![Veri koruma yöntemini seçin.](./media/backup-azure-delete-vault/data-protection-method.png)

Şirket içi yedekleme öğelerini sildikten sonra portaldan sonraki adımları izleyin.

## <a name="delete-the-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme

1. Tüm bağımlılıklar kaldırıldığında, kasa menüsündeki **Temel Ler** bölmesine gidin.
2. Listede yedek öğeler, yedekleme yönetim sunucuları veya çoğaltılmış öğeler olmadığını doğrulayın. Öğeler hala kasada görünüyorsa, [başlamadan önce](#before-you-start) bölümüne bakın.

3. Kasada başka öğe yoksa, kasa panosunda **Sil'i** seçin.

    ![Kasa panosunda Sil'i seçin.](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Kasayı silmek istediğinizi doğrulamak için **Evet'i** seçin. Kasa silindi. Portal **Yeni** hizmet menüsüne geri döner.

## <a name="delete-the-recovery-services-vault-by-using-powershell"></a>PowerShell'i kullanarak Kurtarma Hizmetleri kasasını silme

İlk olarak, bağımlılıkları ve kasa silme işlemini anlamak için **[başlamadan önce](#before-you-start)** bölümü okuyun.

Korumayı durdurmak ve yedekleme verilerini silmek için:

- Azure VM yedeklemesinde SQL kullanıyorsanız ve SQL örnekleri için otomatik korumayı etkinleştirdiyseniz, önce otomatik korumayı devre dışı edin.

    ```PowerShell
        Disable-AzRecoveryServicesBackupAutoProtection
           [-InputItem] <ProtectableItemBase>
           [-BackupManagementType] <BackupManagementType>
           [-WorkloadType] <WorkloadType>
           [-PassThru]
           [-VaultId <String>]
           [-DefaultProfile <IAzureContextContainer>]
           [-WhatIf]
           [-Confirm]
           [<CommonParameters>]
    ```

  Azure Yedekleme korumalı bir öğe için korumayı nasıl devre dışı dışı ksözenene ne kadar iyi bilgi [edinin.](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupautoprotection?view=azps-2.6.0)

- Buluttaki tüm yedekleme korumalı öğelerin (ör. laaS VM, Azure Dosya Paylaşımı vb.):

    ```PowerShell
       Disable-AzRecoveryServicesBackupProtection
       [-Item] <ItemBase>
       [-RemoveRecoveryPoints]
       [-Force]
       [-VaultId <String>]
       [-DefaultProfile <IAzureContextContainer>]
       [-WhatIf]
       [-Confirm]
       [<CommonParameters>]
    ```

    [Learn more](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackupprotection?view=azps-2.6.0&viewFallbackFrom=azps-2.5.0) Yedekleme korumalı bir öğe için devre dışı koruma hakkında daha fazla bilgi edinin.

- Azure Yedekleme Aracısı (MARS) kullanılarak korunan şirket içi Dosyalar ve Klasörler için, her MARS PowerShell modülünden yedeklenen verileri silmek için aşağıdaki PowerShell komutunu kullanın:

    ```powershell
    Get-OBPolicy | Remove-OBPolicy -DeleteBackup -SecurityPIN <Security Pin>
    ```

    Aşağıdaki komut isteminin görüntülendiği gönderi:

    *Microsoft Azure Yedekleme Bu yedekleme ilkesini kaldırmak istediğinizden emin misiniz? Silinen yedekleme verileri 14 gün boyunca saklanır. Bu süre den sonra, yedekleme verileri kalıcı olarak silinir. <br/> [Y] Evet [A] Evet Hepsine [N] Hayır [L] Hayır Herkese [S] Askıya [?] Yardım (varsayılan "Y"dir):*

- MABS (Microsoft Azure Yedekleme Sunucusu) veya DPM ile Azure (Sistem Merkezi Veri Koruma Yöneticisi) kullanılarak korunan şirket içi makinelerde, Azure'da yedeklenen verileri silmek için aşağıdaki komutu kullanın.

    ```powershell
    Get-OBPolicy | Remove-OBPolicy -DeleteBackup -SecurityPIN <Security Pin>
    ```

    Aşağıdaki komut isteminin görüntülendiği gönderi:

   *Microsoft Azure Yedekleme* Bu yedekleme ilkesini kaldırmak istediğinizden emin misiniz? Silinen yedekleme verileri 14 gün boyunca saklanır. Bu süre den sonra, yedekleme verileri kalıcı olarak silinir. <br/>
   [Y] Evet [A] Evet Hepsine [N] Hayır [L] Hayır Herkese [S] Askıya [?] Yardım (varsayılan "Y"):*

Desteklenen verileri sildikten sonra, şirket içi kapsayıcıları ve yönetim sunucularını kaydını kaldırın.

- Azure Yedekleme Aracısı (MARS) kullanılarak korunan şirket içi Dosyalar ve Klasörler için Azure'a yedekleme:

    ```PowerShell
    Unregister-AzRecoveryServicesBackupContainer
              [-Container] <ContainerBase>
              [-PassThru]
              [-VaultId <String>]
              [-DefaultProfile <IAzureContextContainer>]
              [-WhatIf]
              [-Confirm]
              [<CommonParameters>]
    ```

    Kasadan bir Windows Server'ı veya başka bir kapsayıcıyı kayıt dışı etme hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/powershell/module/az.recoveryservices/unregister-azrecoveryservicesbackupcontainer?view=azps-2.6.0)

- MABS (Microsoft Azure Yedekleme Sunucusu) veya DPM ile Azure 'a (Sistem Merkezi Veri Koruma Yönetimi) kullanılarak korunan şirket içi makineler için:

    ```PowerShell
        Unregister-AzRecoveryServicesBackupManagementServer
          [-AzureRmBackupManagementServer] <BackupEngineBase>
          [-PassThru]
          [-VaultId <String>]
          [-DefaultProfile <IAzureContextContainer>]
          [-WhatIf]
          [-Confirm]
          [<CommonParameters>]
    ```

    Kasadan yedek yönetim konteynerini kaydetme hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/powershell/module/az.recoveryservices/unregister-azrecoveryservicesbackupcontainer?view=azps-2.6.0)

Yedeklenen verileri kalıcı olarak sildikten ve tüm kapsayıcıları kaydettirdikten sonra kasayı silmeye devam edin.

Kurtarma Hizmetleri kasasını silmek için:

   ```PowerShell
       Remove-AzRecoveryServicesVault
      -Vault <ARSVault>
      [-DefaultProfile <IAzureContextContainer>]
      [-WhatIf]
      [-Confirm]
      [<CommonParameters>]
   ```

Kurtarma hizmetleri kasası silme hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/powershell/module/az.recoveryservices/remove-azrecoveryservicesvault)

## <a name="delete-the-recovery-services-vault-by-using-cli"></a>CLI kullanarak Kurtarma Hizmetleri kasasını silme

İlk olarak, bağımlılıkları ve kasa silme işlemini anlamak için **[başlamadan önce](#before-you-start)** bölümü okuyun.

> [!NOTE]
> Şu anda Azure Yedekleme CLI yalnızca Azure VM yedeklemelerini yönetmeyi destekler, bu nedenle kasayı silmek için aşağıdaki komut yalnızca kasa Azure VM yedekleri içeriyorsa çalışır. Kasada Azure VM'ler dışında herhangi bir yedek öğe varsa, Azure Yedekleme CLI'sini kullanarak bir kasayı silemezsiniz.

Varolan Kurtarma hizmetleri kasasını silmek için aşağıdakileri gerçekleştirin:

- Korumayı durdurmak ve yedekleme verilerini silmek için

    ```azurecli
    az backup protection disable --container-name
                             --item-name
                             [--delete-backup-data {false, true}]
                             [--ids]
                             [--resource-group]
                             [--subscription]
                             [--vault-name]
                             [--yes]
    ```

    Daha fazla bilgi için bu [makaleye](/cli/azure/backup/protection#az-backup-protection-disable)bakın.

- Varolan bir Kurtarma hizmetleri kasası silme:

    ```azurecli
    az backup vault delete [--force]
                       [--ids]
                       [--name]
                       [--resource-group]
                       [--subscription]
                       [--yes]
    ```

    Daha fazla bilgi için bu [makaleye](https://docs.microsoft.com/cli/azure/backup/vault?view=azure-cli-latest) bakın

## <a name="delete-the-recovery-services-vault-by-using-azure-resource-manager"></a>Azure Kaynak Yöneticisi'ni kullanarak Kurtarma Hizmetleri kasasını silme

Kurtarma Hizmetleri kasasını silme seçeneği, yalnızca tüm bağımlılıklar kaldırılırsa ve vault *silme hatası*almaya devam ederseniz önerilir. Aşağıdaki ipuçlarından herhangi birini veya tümünün deneyin:

- Kasa menüsündeki **Essentials** bölmesinden, yedek öğelerin, yedekleme yönetim sunucularının veya çoğaltılmış öğelerin listelenmediğini doğrulayın. Yedekleme öğeleri varsa, [başlamadan önce](#before-you-start) bölümüne bakın.
- [Kasayı tekrar geçitten silmeyi](#delete-the-recovery-services-vault) dene.
- Tüm bağımlılıklar kaldırılırsa ve Vault silme *hatasını*almaya devam ediyorsanız, aşağıdaki adımları gerçekleştirmek için ARMClient aracını kullanın (nottan sonra).

1. Chocolatey'i indirmek ve yüklemek için [chocolatey.org](https://chocolatey.org/) gidin. Ardından, aşağıdaki komutu çalıştırarak ARMClient'ı yükleyin:

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Azure hesabınızda oturum açın ve ardından aşağıdaki komutu çalıştırın:

    `ARMClient.exe login [environment name]`

3. Azure portalında, silmek istediğiniz kasa için abonelik kimliği ve kaynak grubu adını toplayın.

ARMClient komutu hakkında daha fazla bilgi için [ARMClient README'ye](https://github.com/projectkudu/ARMClient/blob/master/README.md)bakın.

### <a name="use-the-azure-resource-manager-client-to-delete-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silmek için Azure Kaynak Yöneticisi istemcisini kullanma

1. Abonelik kimliğinizi, kaynak grup adınızı ve kasa adınızı kullanarak aşağıdaki komutu çalıştırın. Herhangi bir bağımlılığınız yoksa, aşağıdaki komutu çalıştırdığınızda kasa silinir:

   ```azurepowershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```

2. Kasa boş değilse, aşağıdaki hata iletisini alırsınız: *Kasa bu kasada var olan kaynaklar olduğu için silinemez.* Korumalı bir öğeyi veya kapsayıcıyı kasadaki kaldırmak için aşağıdaki komutu çalıştırın:

   ```azurepowershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. Azure portalında kasanın silindiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[Kurtarma Hizmetleri kasaları hakkında bilgi edinin](backup-azure-recovery-services-vault-overview.md)<br/>
[Kurtarma Hizmetleri kasalarını izleme ve yönetme hakkında bilgi edinin](backup-azure-manage-windows-server.md)

---
title: Azure 'da bir kurtarma hizmetleri kasasını silme
description: Bir kurtarma hizmetleri kasasının nasıl silineceğini açıklar.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: dacurwin
ms.openlocfilehash: 34484c309cb186aabec519e54269fefae316165e
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639899"
---
# <a name="delete-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme

Bu makalede [Azure Backup](backup-overview.md) kurtarma hizmetleri kasasının nasıl silineceği açıklanır. Bağımlılıkları kaldırmak ve ardından kasayı silmek için yönergeler içerir.


## <a name="before-you-start"></a>Başlamadan önce

Korumalı sunucular veya kasa ile ilişkili yedekleme yönetim sunucuları gibi bağımlılıklara sahip bir kurtarma hizmetleri kasasını silemezsiniz.

- Yedekleme verilerini içeren kasa silinemez (diğer bir deyişle, korumayı durdursanız ancak yedekleme verilerini tutsanız bile).

- Bağımlılıklar içeren bir kasayı silerseniz, şu hata görüntülenir:

  ![Kasa silme hatası](./media/backup-azure-delete-vault/error.png)

- Bir şirket içi korumalı öğeyi (MARS, MABS veya DPM 'den Azure 'a) bağımlılıkları içeren portaldan silerseniz, bir uyarı iletisi görüntülenir:

  ![Korumalı sunucu silme hatası](./media/backup-azure-delete-vault/error-message.jpg)

  
Kasayı düzgün bir şekilde silmek için, kurulumla eşleşen senaryoyu seçin ve önerilen adımları izleyin:

Senaryo | Kasayı silmek için bağımlılıkları kaldırma adımları |
-- | --
Azure 'a yedekleme Azure Backup Aracısı (MARS) kullanılarak korunan şirket içi dosya ve Klasörlerim var | Yedekleme verilerini ve yedekleme öğelerini silme ile Ilgili adımları gerçekleştirin- [Mars Aracısı için](#delete-backup-items-from-mars-management-console)
MABS (Microsoft Azure Backup Server) veya DPM 'yi Azure 'a (System Center Data Protection Manager) kullanarak korunan şirket içi makinelerim var | [MABS Aracısı için](#delete-backup-items-from-mabs-management-console) yedekleme verilerini ve yedekleme öğelerini silme içindeki adımları gerçekleştirin
Bulutta korumalı öğeler var (örn. laaS VM, Azure dosya paylaşma vb.)  | [Bulutta korunan öğeler için](#delete-protected-items-in-cloud) yedekleme verilerini ve yedekleme öğelerini silme ' daki adımları gerçekleştirin
Hem şirket içinde hem de bulutta korumalı Öğelerim var | Yedekleme verilerini ve yedekleme öğelerini silme bölümündeki adımları aşağıdaki sırayla gerçekleştirin: <br> - [Buluttaki korumalı öğeler için](#delete-protected-items-in-cloud)<br> - [MARS Aracısı için](#delete-backup-items-from-mars-management-console) <br> - [MABS Aracısı için](#delete-backup-items-from-mabs-management-console)
Şirket içinde veya bulutta herhangi bir korumalı öğe yok; Ancak yine de kasa silme hatası alıyorum | [Azure Resource Manager istemcisi 'ni kullanarak kurtarma hizmetleri kasasını silme](#delete-the-recovery-services-vault-using-azure-resource-manager-client) bölümündeki adımları gerçekleştirin


## <a name="delete-protected-items-in-cloud"></a>Buluttaki korumalı öğeleri sil

Devam etmeden önce, bağımlılıklar ve kasa silme işlemini anlamak için **[Bu](#before-you-start)** bölümü okuyun.

Korumayı durdurmak ve yedekleme verilerini silmek için aşağıdaki işlemleri gerçekleştirin:

1. Portal > **Kurtarma Hizmetleri Kasası** > **yedekleme öğeleri** bulutta korunan öğeleri seçin (örnek AzureVirtual makinesi, Azure depolama (Azure dosyaları), SQL 'den Azure sanal makinesi vb.).

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/azure-storage-selected.png)

2. Yedekleme öğesinin korumalı olup olmadığına bağlı olarak Yedekleme öğesine sağ tıklayın veya menünün **Yedeklemeyi Durdur** ' u ve **yedekleme verilerini sil**' i görüntüleyin.

    - **Yedeklemeyi Durdur**için açılan listeden **yedekleme verilerini sil** ' i seçin. Yedekleme öğesinin **adını** girin (büyük/küçük harfe duyarlı), bir **neden**seçin, **açıklamalar**girin ve **Yedeklemeyi Durdur**' a tıklayın.

        ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/stop-backup-item.png)

    - **Yedekleme verilerini sil**Için, yedekleme öğesinin adını girin (büyük/küçük harfe duyarlı), bir **neden**seçin, **açıklamalar**girin ve **Sil**' e tıklayın. 

         ![yedekleme verilerini sil](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Yedekleme verilerini ![silmebildirimini](./media/backup-azure-delete-vault/messages.png)denetleyin. Tamamlandıktan sonra hizmet şu iletiyi görüntüler: Yedekleme **durduruluyor ve "*yedekleme öğesi*" için yedekleme verileri siliniyor**. **Işlem başarıyla tamamlandı**.
6. Yedekleme öğesinin kaldırılıp kaldırılmadığını denetlemek için **yedekleme öğeleri** menüsünde **Yenile** ' ye tıklayın.

      ![yedekleme verilerini sil](./media/backup-azure-delete-vault/empty-items-list.png)

## <a name="delete-protected-items-on-premises"></a>Şirket içi korumalı öğeleri sil

Devam etmeden önce, bağımlılıklar ve kasa silme işlemini anlamak için **[Bu](#before-you-start)** bölümü okuyun.

1. Kasa panosu menüsünden **Yedekleme altyapısı**' na tıklayın.
2. Şirket içi senaryonuza bağlı olarak aşağıdaki seçeneği belirleyin:

      - **Azure Backup Aracısı**için, **korumalı sunucular** > **Azure Backup Aracısı** ' nı seçin ve silmek istediğiniz sunucuyu seçin. 

        ![kendi panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/identify-protected-servers.png)

      - **DPM** **Azure Backup sunucusu**/için **yedekleme yönetim sunucuları**' nı seçin. Silmek istediğiniz sunucuyu seçin. 


          ![kendi panosunu açmak için kasa seçin](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Uyarı iletisiyle **silme** dikey penceresi görüntülenir.

     ![yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-protected-server.png)

     Uyarı iletisini ve onay onay kutusunda belirtilen yönergeleri gözden geçirin.
    
    > [!NOTE]
    >- Korumalı sunucu, Azure hizmeti ile eşitlenmiş ve yedekleme öğeleri mevcutsa, onay onay kutusu bağımlı yedekleme öğelerinin sayısını ve yedekleme öğelerini görüntüleme bağlantısını görüntüler.
    >- Korumalı sunucu Azure hizmeti ile eşitlenmemiş ve yedekleme öğeleri varsa, izin onay kutusu, yedekleme öğelerinin sayısını görüntüler.
    >- Yedekleme öğeleri yoksa, izin onay kutusu silme işlemini ister.

4. Onay onay kutusunu seçin ve **Sil**' e tıklayın.




5. Yedekleme verilerini ![silmebildirimini](./media/backup-azure-delete-vault/messages.png)denetleyin. Tamamlandıktan sonra hizmet şu iletiyi görüntüler: Yedekleme **durduruluyor ve "*yedekleme öğesi*" için yedekleme verileri siliniyor**. **Işlem başarıyla tamamlandı**.
6. Yedekleme öğesinin kaldırılıp kaldırılmadığını denetlemek için **yedekleme öğeleri** menüsünde **Yenile** ' ye tıklayın.

Artık yedekleme öğelerini yönetim konsolundan silmeye devam edebilirsiniz:
    
   - [MARS kullanılarak korunan öğeler](#delete-backup-items-from-mars-management-console)
    - [MABS kullanılarak korunan öğeler](#delete-backup-items-from-mabs-management-console)


### <a name="delete-backup-items-from-mars-management-console"></a>Yedekleme öğelerini MARS yönetim konsolundan Sil

MARS yönetim konsolundan yedekleme öğelerini silmek için

- MARS yönetim konsolunu başlatın, **Eylemler** bölmesine gidin ve **yedeklemeyi zamanla**' yı seçin.
- **Zamanlanmış bir yedeklemeyi değiştirme veya durdurma** sihirbazında, **Bu yedekleme zamanlamasını kullanmayı Durdur seçeneğini belirleyin ve tüm depolanan yedeklemeleri silip** **İleri**' ye tıklayın.

    ![Zamanlanmış bir yedeklemeyi değiştirme veya durdurma](./media/backup-azure-delete-vault/modify-schedule-backup.png)

- **Zamanlanmış yedekleme sihirbazını durdur** ' dan **son**' a tıklayın.

    ![Zamanlanmış bir yedeklemeyi durdur](./media/backup-azure-delete-vault/stop-schedule-backup.png)
- Bir güvenlik PIN kodu girmeniz istenir. PIN oluşturmak için aşağıdaki adımları gerçekleştirin:
  - Azure Portal’da oturum açın.
  - **Kurtarma Hizmetleri Kasası** > **ayarları** > **özelliklerine**gidin.
  - **GÜVENLIK PIN**'ı altında **Oluştur**' a tıklayın. Bu PIN 'ı kopyalayın. (Bu PIN yalnızca beş dakika için geçerlidir)
- Yönetim konsolunda (istemci uygulaması) PIN 'ı yapıştırın ve **Tamam**' a tıklayın.

  ![Güvenlik PIN 'ı](./media/backup-azure-delete-vault/security-pin.png)

- **Yedekleme ilerlemesini değiştirme** sihirbazında *, silinen yedekleme verilerinin 14 gün boyunca tutulacaksınız. Bu süreden sonra, yedekleme verileri kalıcı olarak silinir.*  

    ![Yedekleme altyapısını Sil](./media/backup-azure-delete-vault/deleted-backup-data.png)

Yedekleme öğelerini Şirket içinden silmiş olduğunuza göre, portaldan sonraki adımları tamamlayabilirsiniz.

### <a name="delete-backup-items-from-mabs-management-console"></a>MABS yönetim konsolundan yedekleme öğelerini Sil

MABS yönetim konsolundan yedekleme öğelerini silmek için

**Yöntem 1** Korumayı durdurmak ve yedekleme verilerini silmek için aşağıdaki adımları gerçekleştirin:

1.  DPM Yönetici Konsolu, gezinti çubuğunda **koruma** ' ya tıklayın.
2.  Görüntü bölmesinde, kaldırmak istediğiniz koruma grubu üyesini seçin. **Grup üyelerinin korumasını durdur** seçeneğini belirlemek için sağ tıklayın.
3.  **Korumayı Durdur** iletişim kutusunda **korunan verileri** > Sil**çevrimiçi Sil** onay kutusunu seçin ve ardından **Korumayı Durdur**' a tıklayın.

    ![Depolamayı çevrimiçi silme](./media/backup-azure-delete-vault/delete-storage-online.png)

Korunan üye durumu artık **etkin olmayan çoğaltma**olarak değiştirildi.

4. Etkin olmayan koruma grubuna sağ tıklayın ve **etkin olmayan korumayı kaldır**' ı seçin.

    ![Etkin olmayan korumayı kaldır](./media/backup-azure-delete-vault/remove-inactive-protection.png)

5. **Etkin olmayan korumayı Sil** penceresinde **çevrimiçi depolamayı sil** ' i seçin ve **Tamam**' ı tıklatın.

    ![Diskteki ve çevrimiçi çoğaltmaları kaldırma](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

**Yöntem 2** **Mabs yönetim** konsolunu başlatın. **Veri koruma yöntemini seçin** bölümünde **çevrimiçi koruma**istiyorum ' u seçin.

  ![veri koruma yöntemini seçin](./media/backup-azure-delete-vault/data-protection-method.png)

Yedekleme öğelerini Şirket içinden silmiş olduğunuza göre, portaldan sonraki adımları tamamlayabilirsiniz.


## <a name="delete-the-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme

1. Tüm bağımlılıklar kaldırıldığında, kasa menüsündeki **temel** bileşenler bölmesine gidin.
2. Herhangi bir **yedekleme öğesi**, **yedek yönetim sunucusu**veya **yinelenen öğe** olmadığını doğrulayın. Hala kasada yer görünürse, [başlamadan önce](#before-you-start) bölümüne bakın.

3. Kasada daha fazla öğe yoksa, kasa panosunda **Sil**' e tıklayın.

    ![yedekleme verilerini sil](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Kasayı silmek istediğinizi doğrulamak için **Evet**' e tıklayın. Kasa silinir ve Portal **Yeni** hizmet menüsüne geri döner.

## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>Azure Resource Manager istemcisi 'ni kullanarak kurtarma hizmetleri kasasını silme

Kurtarma Hizmetleri kasasını silmek için bu seçenek yalnızca tüm bağımlılıklar kaldırıldığında ve *kasa silme hatası*almaya devam ettiğinizde önerilir.

- Kasa menüsündeki **temel** bileşenler bölmesinden hiçbir **yedekleme öğesi**, **yedekleme yönetim sunucusu**veya **yinelenen öğe** olmadığını doğrulayın. Yedekleme öğeleri varsa, [başlamadan önce](#before-you-start) bölümüne bakın.
- [Kasayı portaldan silmeyi](#delete-the-recovery-services-vault)yeniden deneyin.
- Tüm bağımlılıklar kaldırılırsa ve yine de *kasa silme hatası* alıyorsanız, aşağıda verilen adımları gerçekleştirmek Için ARMClient aracını kullanın;

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Chocolatey 'i [buradan](https://chocolatey.org/) yükleyip ARMClient 'ı yüklemek için aşağıdaki komutu çalıştırın:

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Azure hesabınızda oturum açın ve şu komutu çalıştırın:

    `ARMClient.exe login [environment name]`

3. Azure portal, silmek istediğiniz kasasının abonelik KIMLIĞINI ve kaynak grubu adını toplayın.

ARMClient komutu hakkında daha fazla bilgi için bu [belgeye](https://github.com/projectkudu/ARMClient/blob/master/README.md)bakın.

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silmek için Azure Resource Manager istemci kullanma

1. Abonelik KIMLIĞINIZI, kaynak grubu adını ve kasa adını kullanarak aşağıdaki komutu çalıştırın. Komutu çalıştırdığınızda, hiçbir bağımlılığı yoksa kasayı siler.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Kasa boş değilse, "Bu kasada mevcut kaynaklar olduğundan kasa silinemez" hatasını alırsınız. Bir kasadaki Korunan öğeleri/kapsayıcıyı kaldırmak için aşağıdakileri yapın:

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. Azure portal kasanın silindiğini doğrulayın.


## <a name="next-steps"></a>Sonraki adımlar

[Hakkında bilgi edinin](backup-azure-recovery-services-vault-overview.md) Kurtarma Hizmetleri kasaları.<br/>
Kurtarma Hizmetleri kasalarını izleme ve yönetme [hakkında bilgi edinin](backup-azure-manage-windows-server.md) .

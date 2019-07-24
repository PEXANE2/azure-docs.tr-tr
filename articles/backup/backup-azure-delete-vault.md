---
title: Azure 'da bir kurtarma hizmetleri kasasını silme
description: Bir kurtarma hizmetleri kasasının nasıl silineceğini açıklar.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/11/2019
ms.author: raynew
ms.openlocfilehash: 3d3a77b43babb7fa3666dbbc9e4334f61ae612f8
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68232658"
---
# <a name="delete-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme

Bu makalede [Azure Backup](backup-overview.md) kurtarma hizmetleri kasasının nasıl silineceği açıklanır. Bağımlılıkları kaldırmak ve ardından kasayı silmek için yönergeler içerir.


## <a name="before-you-start"></a>Başlamadan önce

Korumalı sunucular veya kasa ile ilişkili yedekleme yönetim sunucuları gibi bağımlılıklara sahip bir kurtarma hizmetleri kasasını silemezsiniz.<br/>
Yedekleme verilerini içeren kasa silinemez (diğer bir deyişle, korumayı durdursanız ancak yedekleme verilerini tutsanız bile).

Bağımlılıklar içeren bir kasayı silerseniz, şu hata görüntülenir:

![kasa silme hatası](./media/backup-azure-delete-vault/error.png)

Kasayı düzgün bir şekilde silmek için, kurulumla eşleşen senaryoyu seçin ve önerilen adımları izleyin:

Senaryo | Kasayı silmek için bağımlılıkları kaldırma adımları |
-- | --
Azure 'a yedekleme Azure Backup Aracısı (MARS) kullanılarak korunan şirket içi dosya ve Klasörlerim var | Yedekleme verilerini ve yedekleme öğelerini silme ile Ilgili adımları gerçekleştirin- [Mars Aracısı için](#for-mars-agent)
MABS (Microsoft Azure Backup Server) veya DPM 'yi Azure 'a (System Center Data Protection Manager) kullanarak korunan şirket içi makinelerim var | [MABS Aracısı için](#for-mabs-agent) yedekleme verilerini ve yedekleme öğelerini silme içindeki adımları gerçekleştirin
Bulutta korumalı öğeler var (örn. laaS VM, Azure dosya paylaşma vb.)  | [Bulutta korunan öğeler için](#for-protected-items-in-cloud) yedekleme verilerini ve yedekleme öğelerini silme ' daki adımları gerçekleştirin
Hem şirket içinde hem de bulutta korumalı Öğelerim var | Yedekleme verilerini ve yedekleme öğelerini silme bölümündeki adımları aşağıdaki sırayla gerçekleştirin: <br> - [Buluttaki korumalı öğeler için](#for-protected-items-in-cloud)<br> - [MABS Aracısı için](#for-mars-agent) <br> - [MABS Aracısı için](#for-mabs-agent)
Şirket içinde veya bulutta herhangi bir korumalı öğe yok; Ancak yine de kasa silme hatası alıyorum | [Azure Resource Manager istemcisi 'ni kullanarak kurtarma hizmetleri kasasını silme](#delete-the-recovery-services-vault-using-azure-resource-manager-client) bölümündeki adımları gerçekleştirin
Bu şirket içi sunucuya artık sahip olmadım (kayıp/kullanımdan çıkarıldı) ve kurtarma hizmetleri kasasını silmek istiyorum | Microsoft desteği 'ne başvurun.

## <a name="delete-backup-data-and-backup-items"></a>Yedekleme verilerini ve yedekleme öğelerini Sil

Devam etmeden önce, bağımlılıklar ve kasa silme işlemini anlamak için **[Bu](#before-you-start)** bölümü okuyun.

### <a name="for-protected-items-in-cloud"></a>Buluttaki korumalı öğeler için

Korumayı durdurmak ve yedekleme verilerini silmek için aşağıdaki işlemleri gerçekleştirin:

1. Portal > Kurtarma Hizmetleri Kasası > yedekleme öğeleri bulutta korunan öğeleri seçin.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/azure-storage-selected.jpg)

2. Her öğe için sağ tıklayıp **Yedeklemeyi Durdur**' u seçmeniz gerekir.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/stop-backup-item.png)

3. Yedeklemeyi > Durdur ' da**bir seçenek belirleyin**, **yedekleme verilerini sil**' i seçin.
4. Öğenin adını yazın ve **Yedeklemeyi Durdur**' a tıklayın.
   - Bu, öğeyi silmek istediğinizi doğrular.
   - **Yedeklemeyi Durdur** düğmesi, doğruladıktan sonra etkinleştirilir.
   - Verileri korur ve silmezseniz kasayı silemezsiniz.

     ![yedekleme verilerini sil](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Yedekleme verilerini  ![silmebildirimini](./media/backup-azure-delete-vault/messages.png)denetleyin. Tamamlandıktan sonra hizmet şu iletiyi görüntüler: Yedekleme **durduruluyor ve "*yedekleme öğesi*" için yedekleme verileri siliniyor**. **Işlem başarıyla tamamlandı**.
6. Yedekleme öğesinin kaldırılıp kaldırılmadığını denetlemek için **yedekleme öğeleri** menüsünde **Yenile** ' ye tıklayın.

      ![yedekleme verilerini sil](./media/backup-azure-delete-vault/empty-items-list.png)

### <a name="for-mars-agent"></a>MARS Aracısı için

Korumayı durdurmak ve yedekleme verilerini silmek için adımları aşağıda listelenen sırayla gerçekleştirin:

- [1. Adım: Yedekleme öğelerini MARS yönetim konsolundan Sil](#step-1-delete-backup-items-from-mars-management-console)
- [2. Adım: Portaldan Azure Backup aracısını kaldır](#step-1-delete-backup-items-from-mars-management-console)


#### <a name="step-1-delete-backup-items-from-mars-management-console"></a>1\. adım: Yedekleme öğelerini MARS yönetim konsolundan Sil

Sunucu KULLANILAMAMASINDAN dolayı bu adımı gerçekleştiremedik, Microsoft destek 'e başvurun.

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

#### <a name="step-2-from-portal-remove-azure-backup-agent"></a>2\. adım: Portaldan Azure Backup aracısını kaldır

Devam etmeden önce [1. adım](#step-1-delete-backup-items-from-mars-management-console) tamamlandığından emin olun:

1. Kasa panosu menüsünde, **Yedekleme altyapısı**' na tıklayın.
2. Altyapı sunucularını görüntülemek için **korumalı sunucular** ' a tıklayın.

    ![kendi panosunu açmak için kasanızı seçin](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. **Korumalı sunucular** listesinde Azure Backup Aracısı ' na tıklayın.

    ![Yedekleme türünü seçin](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

4. Azure Backup Aracısı kullanılarak korunan sunucular listesinden sunucuya tıklayın.

    ![belirli korumalı sunucuyu seçin](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

5. Seçili sunucu panosunda **Sil**' e tıklayın.

    ![Seçili sunucuyu Sil](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. **Sil** menüsünde, sunucunun adını yazın ve **Sil**' e tıklayın.

     ![yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

> [!NOTE]
> Aşağıdaki hatayı görüyorsanız, önce [Yönetim konsolundan yedekleme öğelerini silme](#step-1-delete-backup-items-from-mars-management-console)bölümünde listelenen adımları uygulayın.
>
>![silme başarısız oldu](./media/backup-azure-delete-vault/deletion-failed.png)
>
>Yönetim konsolundan yedeklemeleri silme adımlarını gerçekleştirebiliyorsanız, örneğin, yönetim konsoluyla sunucu KULLANILAMAMASINDAN dolayı, Microsoft destek 'e başvurun.

7. Yedekleme verilerini  ![silmebildirimini](./media/backup-azure-delete-vault/messages.png)denetleyin. Tamamlandıktan sonra hizmet şu iletiyi görüntüler: Yedekleme **durduruluyor ve "*yedekleme öğesi*" için yedekleme verileri siliniyor**. **Işlem başarıyla tamamlandı**.
8. Yedekleme öğesinin kaldırılıp kaldırılmadığını denetlemek için **yedekleme öğeleri** menüsünde **Yenile** ' ye tıklayın.


### <a name="for-mabs-agent"></a>MABS Aracısı için

Korumayı durdurmak ve yedekleme verilerini silmek için adımları aşağıda listelenen sırayla gerçekleştirin:

- [1. Adım: MABS yönetim konsolundan yedekleme öğelerini Sil](#step-1-delete-backup-items-from-mabs-management-console)
- [2. Adım: Portaldan Azure Backup yönetim sunucularını kaldır](#step-2-from-portal-remove-azure-backup-agent)

#### <a name="step-1-delete-backup-items-from-mabs-management-console"></a>1\. adım: MABS yönetim konsolundan yedekleme öğelerini Sil

Sunucu KULLANILAMAMASINDAN dolayı bu adımı gerçekleştiremedik, Microsoft destek 'e başvurun.

**Yöntem 1** Korumayı durdurmak ve yedekleme verilerini silmek için aşağıdaki adımları gerçekleştirin:

1.  DPM Yönetici Konsolu, gezinti çubuğunda **koruma** ' ya tıklayın.
2.  Görüntü bölmesinde, kaldırmak istediğiniz koruma grubu üyesini seçin. **Grup üyelerinin korumasını durdur** seçeneğini belirlemek için sağ tıklayın.
3.  **Korumayı Durdur** iletişim kutusunda, **korunan verileri** > Sil**çevrimiçi silme** onay kutusunu seçin ve ardından **Korumayı Durdur**' a tıklayın.

    ![Depolamayı çevrimiçi silme](./media/backup-azure-delete-vault/delete-storage-online.png)

Korunan üye durumu artık **etkin olmayan çoğaltma**olarak değiştirildi.

5. Etkin olmayan koruma grubuna sağ tıklayın ve **etkin olmayan korumayı kaldır**' ı seçin.

    ![Etkin olmayan korumayı kaldır](./media/backup-azure-delete-vault/remove-inactive-protection.png)

6. **Etkin olmayan korumayı Sil** penceresinde **çevrimiçi depolamayı sil** ' i seçin ve **Tamam**' ı tıklatın.

    ![Diskteki ve çevrimiçi çoğaltmaları kaldırma](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

**Yöntem 2** **Mabs yönetim** konsolunu başlatın. **Veri koruma yöntemini seçin** bölümünde **çevrimiçi koruma**istiyorum ' u seçin.

  ![veri koruma yöntemini seçin](./media/backup-azure-delete-vault/data-protection-method.png)

Yedekleme öğelerini Şirket içinden silmiş olduğunuza göre, portaldan sonraki adımları tamamlayabilirsiniz.

#### <a name="step-2-from-portal-remove-azure-backup-management-servers"></a>2\. adım: Portaldan Azure Backup yönetim sunucularını kaldır

Devam etmeden önce [1. adım](#step-1-delete-backup-items-from-mabs-management-console) tamamlandığından emin olun:

1. Kasa panosu menüsünde, **Yedekleme altyapısı**' na tıklayın.
2. Sunucuları görüntülemek için **yedekleme yönetim sunucuları** ' na tıklayın.

    ![kendi panosunu açmak için kasa seçin](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. **Silmek**> öğeye sağ tıklayın.
4. **Sil** menüsünde, sunucunun adını yazın ve **Sil**' e tıklayın.

     ![yedekleme verilerini sil](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

> [!NOTE]
> Aşağıdaki hatayı görüyorsanız, önce [Yönetim konsolundan yedekleme öğelerini silme](#step-2-from-portal-remove-azure-backup-management-servers)bölümünde listelenen adımları uygulayın.
>
>![silme başarısız oldu](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Yönetim konsolundan yedeklemeleri silme adımlarını gerçekleştirebiliyorsanız, örneğin, yönetim konsoluyla sunucu KULLANILAMAMASINDAN dolayı, Microsoft destek 'e başvurun.

5. Yedekleme verilerini  ![silmebildirimini](./media/backup-azure-delete-vault/messages.png)denetleyin. Tamamlandıktan sonra hizmet şu iletiyi görüntüler: Yedekleme **durduruluyor ve "*yedekleme öğesi*" için yedekleme verileri siliniyor**. **Işlem başarıyla tamamlandı**.
6. Yedekleme öğesinin kaldırılıp kaldırılmadığını denetlemek için **yedekleme öğeleri** menüsünde **Yenile** ' ye tıklayın.


## <a name="delete-the-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını silme

1. Tüm bağımlılıklar kaldırıldığında, kasa menüsündeki **temel** bileşenler bölmesine gidin.
2. Herhangi bir **yedekleme öğesi**, **yedek yönetim sunucusu**veya **yinelenen öğe** olmadığını doğrulayın. Öğeler kasada görünmeye devam ediyorsa, [bunları kaldırın](#delete-backup-data-and-backup-items).

3. Kasada daha fazla öğe yoksa, kasa panosunda **Sil**' e tıklayın.

    ![yedekleme verilerini sil](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Kasayı silmek istediğinizi doğrulamak için **Evet**' e tıklayın. Kasa silinir ve Portal **Yeni** hizmet menüsüne geri döner.

## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>Azure Resource Manager istemcisi 'ni kullanarak kurtarma hizmetleri kasasını silme

Kurtarma Hizmetleri kasasını silmek için bu seçenek yalnızca tüm bağımlılıklar kaldırıldığında ve *kasa silme hatası*almaya devam ettiğinizde önerilir.



- Kasa menüsündeki **temel** bileşenler bölmesinden hiçbir **yedekleme öğesi**, **yedekleme yönetim sunucusu**veya **yinelenen öğe** olmadığını doğrulayın. Yedekleme öğeleri varsa, [yedekleme verilerini silme ve yedekleme öğelerini silme](#delete-backup-data-and-backup-items)bölümündeki adımları gerçekleştirin.
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

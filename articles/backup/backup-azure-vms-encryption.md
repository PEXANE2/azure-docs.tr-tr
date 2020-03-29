---
title: Şifreli Azure VM'leri yedekleme ve geri yükleme
description: Azure Yedekleme hizmetiyle şifreli Azure VM'leri nasıl yedekleyip geri yükleyin.
ms.topic: conceptual
ms.date: 04/03/2019
ms.openlocfilehash: a3976cc83f749b1abe00cef3f5bf867ffbc30ab6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206699"
---
# <a name="back-up-and-restore-encrypted-azure-vm"></a>Şifreli Azure VM'yi yedekleyin ve geri yükleyin

Bu makalede, [Azure Yedekleme](backup-overview.md) hizmetini kullanarak şifreli disklerle Windows veya Linux Azure sanal makinelerini (VM'ler) nasıl yedekleyip geri yükleyebilirsiniz.

Başlamadan önce Azure Yedekleme'nin Azure VM'lerle nasıl etkileşimde olduğu hakkında daha fazla bilgi edinmek istiyorsanız, şu kaynakları inceleyin:

- Azure VM yedekleme mimarisini [gözden geçirin.](backup-architecture.md#architecture-built-in-azure-vm-backup)
- [Hakkında bilgi edinin](backup-azure-vms-introduction.md) Azure VM yedeklemeve Azure Yedekleme uzantısı.

## <a name="encryption-support"></a>Şifreleme desteği

Azure Yedekleme, işletim sistemi/veri diskleri Azure Disk Şifreleme (ADE) ile şifrelenmiş Olan Azure VM'lerinin yedeklemesini destekler. ADE, Windows VM'lerin şifrelemesi için BitLocker'ı ve Linux VM'leri için dm-crypt özelliğini kullanır. ADE, disk şifreleme anahtarlarını ve sırlarını yönetmek için Azure Key Vault ile tümleşir. Anahtar Vault Anahtar Şifreleme Anahtarları (KEKs) Key Vault bunları yazmadan önce şifreleme sırlarını şifreleme, ek bir güvenlik katmanı eklemek için kullanılabilir.

Azure Yedekleme, aşağıdaki tabloda özetlendiği gibi Azure AD uygulaması yla ve Olmadan ADE kullanarak Azure VM'lerini yedekleyebilir ve geri yükleyebilir.

**VM disk türü** | **ADE (BEK/dm-crypt)** | **ADE ve KEK**
--- | --- | ---
**Yönetilmeyen** | Evet | Evet
**Yönetilen**  | Evet | Evet

- [ADE,](../security/azure-security-disk-encryption-overview.md)Key [Vault](../key-vault/key-vault-overview.md)ve [KEKs](https://docs.microsoft.com/azure/virtual-machine-scale-sets/disk-encryption-key-vault#set-up-a-key-encryption-key-kek)hakkında daha fazla bilgi edinin.
- Azure VM disk şifrelemesi için [SSS'yi](../security/azure-security-disk-encryption-faq.md) okuyun.

### <a name="limitations"></a>Sınırlamalar

- Aynı abonelik ve bölge içinde şifrelenmiş VM'leri yedekleyebilir ve geri yükleyebilirsiniz.
- Azure Yedekleme, bağımsız tuşlar kullanılarak şifrelenmiş VM'leri destekler. VM'yi şifrelemek için kullanılan sertifikanın bir parçası olan anahtar şu anda desteklenmez.
- Kurtarma Hizmetleri Yedekleme kasası ile aynı abonelik ve bölge içinde şifrelenmiş VM'leri yedekleyebilir ve geri yükleyebilirsiniz.
- Şifreli VM'ler dosya/klasör düzeyinde kurtarılamaz. Dosyaları ve klasörleri geri yüklemek için tüm VM kurtarmak gerekir.
- VM'yi geri verirken, şifreli VM'ler için [varolan VM değiştir](backup-azure-arm-restore-vms.md#restore-options) seçeneğini kullanamazsınız. Bu seçenek yalnızca şifrelenmemiş yönetilen diskler için desteklenir.

## <a name="before-you-start"></a>Başlamadan önce

Başlamadan önce aşağıdakileri yapın:

1. ADE etkinleştirilmiş bir veya daha fazla [Windows](../security/azure-security-disk-encryption-windows.md) veya [Linux](../virtual-machines/linux/disk-encryption-overview.md) VM'niz olduğundan emin olun.
2. Azure VM [yedeklemesi için destek matrisini gözden geçirin](backup-support-matrix-iaas.md)
3. Kurtarma Hizmetleri Yedekleme kasası yoksa bir tane [oluşturun.](backup-azure-arm-vms-prepare.md#create-a-vault)
4. Yedekleme için zaten etkin olan VM'ler için şifrelemeyi etkinleştiriseniz, yedeklemelerin kesintiye uğramadan devam edebilmesi için Yedekleme'ye Key Vault'a erişmek için izinler vermeniz yeterlidir. Bu izinleri atama hakkında [daha fazla bilgi edinin.](#provide-permissions)

Buna ek olarak, bazı durumlarda yapmanız gereken birkaç şey vardır:

- **VM'deki VM aracısını yükleyin**: Azure Yedekleme, makinede çalışan Azure VM aracısına bir uzantı yükleyerek Azure VM'leri yedekler. VM'niz Bir Azure pazar yeri görüntüsünden oluşturulduysa, aracı yüklenir ve çalışır. Özel bir VM oluşturursanız veya şirket içi bir makineyi geçiştiriyorsanız, [aracıyı el ile yüklemeniz](backup-azure-arm-vms-prepare.md#install-the-vm-agent)gerekebilir.

## <a name="configure-a-backup-policy"></a>Yedekleme ilkesini yapılandırma

1. Henüz bir Kurtarma Hizmetleri yedekleme kasası oluşturmadıysanız, [aşağıdaki yönergeleri](backup-azure-arm-vms-prepare.md#create-a-vault) izleyin
2. Portaldaki kasayı açın ve **Başlarken** bölümünde **Yedekleme'yi** seçin.

    ![Yedek bıçak](./media/backup-azure-vms-encryption/select-backup.png)

3. **Yedekleme hedefinde** > **iş yükünüz nerede çalışıyor?** **Azure**
4. **Virtual machine** > **OK** **Ne yedeklemek istiyorsunuz?**

      ![Senaryo bıçağı](./media/backup-azure-vms-encryption/select-backup-goal-one.png)

5. **Yedekleme ilkesinde** > **yedekleme ilkesini seçin,** kasayla ilişkilendirmek istediğiniz ilkeyi seçin. Ardından **Tamam**'a tıklayın.
    - Yedekleme ilkesi, yedeklemelerin ne zaman alındığını ve ne kadar süreyle depolanacaklarını belirtir.
    - Varsayılan ilkenin ayrıntıları, açılan menü altında listelenir.

    ![Senaryo dikey penceresini açma](./media/backup-azure-vms-encryption/select-backup-goal-two.png)

6. Varsayılan ilkeyi kullanmak istemiyorsanız, **Yeni Oluştur'u**seçin ve [özel bir ilke oluşturun.](backup-azure-arm-vms-prepare.md#create-a-custom-policy)

7. Select ilkesini kullanarak yedeklemek istediğiniz şifreli VM'leri seçin ve **Tamam'ı**seçin.

      ![Şifreli VM'leri seçin](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)

8. Kasa sayfasında Azure Key Vault kullanıyorsanız, Azure Yedekleme'nin Anahtar Kasası'ndaki anahtarlara ve sırlara salt okunur şekilde erişmesi gerektiğine dair bir ileti görürsünüz.

    - Bu iletiyi alırsanız, herhangi bir eylem gerekmez.

        ![Erişim Tamam](./media/backup-azure-vms-encryption/access-ok.png)

    - Bu iletiyi alırsanız, [izinleri aşağıdaki yordamda](#provide-permissions)açıklandığı şekilde ayarlamanız gerekir.

        ![Erişim uyarısı](./media/backup-azure-vms-encryption/access-warning.png)

9. Yedekleme ilkesini kasaya dağıtmak ve seçili VM'ler için yedeklemeyi etkinleştirmek için **Yedeklemeyi Etkinleştir'i** tıklatın.

## <a name="trigger-a-backup-job"></a>Yedekleme işini tetikleme

İlk yedekleme zamanlamaya uygun olarak çalışır, ancak hemen aşağıdaki gibi çalıştırabilirsiniz:

1. Kasa menüsünde Yedek **öğeleri**tıklatın.
2. **Yedekleme Öğeleri'nde**Azure **Sanal Makine'yi**tıklatın.
3. Yedek **Öğeler** listesinde elipsleri (...) tıklatın.
4. **Şimdi Yedek'i**tıklatın.
5. **Yedekleme Şimdi,** kurtarma noktası nın korunması gereken son günü seçmek için takvim denetimini kullanın. Ardından **Tamam**'a tıklayın.
6. Portal bildirimlerini izleyin. Devam**Eden** **Yedekleme İşleri** > > kasa panosundaki iş ilerlemesini izleyebilirsiniz. VM’nizin boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.

## <a name="provide-permissions"></a>İzinler sağlama

Azure VM'nin, ilişkili VM'lerle birlikte anahtarları ve sırları yedeklemek için salt okunur erişime ihtiyacı vardır.

- Anahtar Kasanız, Azure aboneliğinin Azure AD kiracısıyla ilişkilidir. **Üye kullanıcıysanız,** Azure Yedekleme başka bir işlem yapmadan Key Vault'a erişebilir.
- **Konuk kullanıcıysanız,** anahtar kasasına erişmek için Azure Yedekleme için izin sağlamanız gerekir.

İzinleri ayarlamak için:

1. Azure portalında **Tüm hizmetleri**seçin ve **Anahtar kasalarını**arayın.
2. Yedeklediğiniz şifreli VM ile ilişkili anahtar kasasını seçin.
3. **Erişim ilkelerini** > seçin**Yeni**.
4. **Anapara seç'i**seçin ve ardından **Yedekleme Yönetimi**yazın.
5. **Yedekleme Yönetim Hizmeti** > **Seçin.**

    ![Yedekleme hizmeti seçimi](./media/backup-azure-vms-encryption/select-backup-service.png)

6. **Şablondan erişim ilkesi** > Yapılandırma ekle **'de (isteğe bağlı)** **Azure Yedekleme'yi**seçin.
    - Anahtar izinleri ve Gizli **izinler** için gerekli **izinler**önceden doldurulur.
    - VM'niz **yalnızca BEK**kullanılarak şifrelenmişse, yalnızca sırlar için izinlere ihtiyacınız olduğundan **Anahtar izinleri** seçimini kaldırın.

    ![Azure yedekleme seçimi](./media/backup-azure-vms-encryption/select-backup-template.png)

7. **Tamam**'a tıklayın. **Yedekleme Yönetim Hizmeti,** **Access ilkelerine**eklenir.

    ![Erişim ilkeleri](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

8. Azure Yedekleme'yi izinlerle birlikte sağlamak için **Kaydet'i** tıklatın.

## <a name="restore-an-encrypted-vm"></a>Şifreli VM'yi geri yükleme

Şifrelenmiş VM'leri aşağıdaki gibi geri yüklersiniz:

1. [VM diskini geri yükleyin.](backup-azure-arm-restore-vms.md#restore-disks)
2. Aşağıdakilerden birini yaparak sanal makine örneğini yeniden oluşturun:
    1. VM ayarlarını özelleştirmek ve VM dağıtımını tetiklemek için geri yükleme işlemi sırasında oluşturulan şablonu kullanın. [Daha fazla bilgi edinin](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm).
    2. PowerShell'i kullanarak geri yüklenen disklerden yeni bir VM oluşturun. [Daha fazla bilgi edinin](backup-azure-vms-automation.md#create-a-vm-from-restored-disks).
3. Linux VM'leri için ADE uzantısını yeniden yükleyin, böylece veri diskleri açık ve monte edilebilmektedir.

## <a name="next-steps"></a>Sonraki adımlar

Herhangi bir sorunla karşılaştıysanız, şu makaleleri inceleyin:

- Şifreli Azure VM'leri yedekleyip geri alırken [sık karşılaşılan hatalar.](backup-azure-vms-troubleshoot.md)
- [Azure VM aracısı/yedekleme uzantısı](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) sorunları.

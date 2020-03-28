---
title: Azure Site Kurtarma'yı kullanarak Hyper-V olağanüstü durum kurtarmayı ayarlama
description: Site Kurtarma'yı kullanarak şirket içi Hyper-V V VM'lerin (VMM olmadan) Azure'a olağanüstü durum kurtarmasını nasıl ayarlayay'ı öğrenin.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 11/12/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 741d4718b5e6140f4ddd2bb22e1a2ec830763176
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239850"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Şirket içi Hyper-V sanal makineleri için Azure’da olağanüstü durum kurtarma ayarlama

[Azure Site Kurtarma](site-recovery-overview.md) hizmeti, şirket içi makineleri ve Azure sanal makinelerini (VM) yöneterek ve düzenleyerek olağanüstü durum kurtarma stratejinize katkıda bulunur.

Bu, serideki üçüncü öğreticidir. Şirket içi Hyper-V V M'lerin Azure'a nasıl olağanüstü durum kurtarmasını ayarlayabileceğinizi gösterir. Bu öğretici, Microsoft System Center Virtual Machine Manager (VMM) tarafından yönetilmeyen Hyper-V VM'ler geçerlidir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağınızı ve hedefinizi seçme.
> * Şirket içi Site Kurtarma bileşenleri ve hedef çoğaltma ortamı da dahil olmak üzere kaynak çoğaltma ortamını ayarlayın.
> * Bir çoğaltma ilkesi oluşturma.
> * Sanal makine için çoğaltmayı etkinleştirme.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı talimatlar için, [Site Kurtarma belgelerinin](https://docs.microsoft.com/azure/site-recovery) **Nasıl Yapilir Kılavuzları** bölümündeki makaleleri inceleyin.



## <a name="before-you-begin"></a>Başlamadan önce

Bu, serideki üçüncü öğreticidir. Önceki öğreticilerde görevleri zaten tamamladığınızı varsayar:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. [Şirket içi Hyper-V’leri hazırlama](tutorial-prepare-on-premises-hyper-v.md)

## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme

1. Azure portalında Kurtarma **Hizmetleri kasalarına** gidin ve kasayı seçin. Biz önceki öğretici **kasa ContosoVMVault** hazırladı.
2. **Başlarken,** **Site Kurtarma'yı**seçin ve ardından **Altyapı Hazırla'yı**seçin.
3. **Koruma hedefi** > nde**makineleriniz nerede bulunur?** **On-premises**
4. **To Azure** **Makinelerinizi nerede çoğaltmak istiyorsunuz?**
5. **Makineleriniz sanallaştırılmış mı?**, **Hyper-V ile Evet'i**seçin.
6. **No** **Hyper-V ana bilgisayarlarınızı yönetmek için Sistem Merkezi VMM**kullanıyor musunuz?
7. **Tamam'ı**seçin.

    ![Çoğaltma hedefi](./media/hyper-v-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

1. **Dağıtım planlamasında,** büyük bir dağıtım planlıyorsanız, Sayfadaki bağlantıdan Hyper-V için Dağıtım Planlayıcısını indirin. Hyper-V dağıtım planlaması hakkında [daha fazla bilgi edinin.](hyper-v-deployment-planner-overview.md)
2. Bu öğretici için Dağıtım Planlayıcısı'na ihtiyacımız yoktur. **Dağıtım planlamayı tamamladınız mı?** **I will do it later** **OK**

    ![Dağıtım planlaması](./media/hyper-v-azure-tutorial/deployment-planning.png)

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamını ayarlamak için bir Hyper-V sitesi oluşturur ve çoğaltmak istediğiniz VM'leri içeren Hyper-V ana bilgisayarlarını bu siteye eklersiniz. Ardından, her ana bilgisayara Azure Site Kurtarma Sağlayıcısı'nı ve Azure Kurtarma Hizmetleri aracısını indirip yükler siniz ve Hyper-V sitesini kasaya kaydedin.

1. **AltyapıYı Hazırla**altında **Kaynak'ı**seçin.
2. **Kaynak Hazırlama'da**, **+ Hyper-V Sitesini**seçin.
3. **Hyper-V oluştur sitesinde,** site adını belirtin. **ContosoHyperVSite**kullanıyoruz.

    ![Hyper-V sitesi](./media/hyper-v-azure-tutorial/hyperv-site.png)

4. Site oluşturulduktan sonra kaynak adım 1 **hazırla:** > **Hyper-V sitesini seçin,** oluşturduğunuz siteyi seçin.
5. + **Hyper-V Server'ı**seçin.

    ![Hyper-V sunucusu](./media/hyper-v-azure-tutorial/hyperv-server.png)

6. Microsoft Azure Site Kurtarma Sağlayıcısı için yükleyiciyi indirin.
7. Kasa kayıt anahtarını indirin. Sağlayıcıyı yüklemek için bu tuşa ihtiyacınız var. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.

    ![Sağlayıcı yı ve kayıt anahtarını indirin](./media/hyper-v-azure-tutorial/download.png)
    

### <a name="install-the-provider"></a>Sağlayıcıyı yükleyin

Hyper-V sitesine eklemek istediğiniz her Hyper-V ana bilgisayara indirilen kurulum dosyasını (AzureSiteRecoveryProvider.exe) yükleyin. Kurulum, her Hyper-V ana bilgisayarına Azure Site Kurtarma Sağlayıcısı ve Kurtarma Hizmetleri aracısını yükler.

1. Kurulum dosyasını çalıştırın.
2. Azure Site Kurtarma Sağlayıcısı Kurulum sihirbazındaki **Microsoft Update** bölümünde, Sağlayıcı güncelleştirmelerini denetlemek için Microsoft Update’i kullanmayı kabul edin.
3. **Yükleme'de,** Sağlayıcı ve aracı için varsayılan yükleme konumunu kabul edin ve **Yükle'yi**seçin.
4. Yüklemeden sonra, Microsoft Azure Site Kurtarma Kayıt Sihirbazı > **Vault Ayarları'nda** **Gözat'ı**seçin ve **Anahtar Dosyası'nda**indirdiğiniz kasa anahtar dosyasını seçin.
5. Azure Site Recovery aboneliğini, kasa adını (**ContosoVMVault**) ve Hyper-V sunucusunun ait olduğu Hyper-V sitesini (**ContosoHyperVSite**) belirtin.
6. **Proxy Ayarları** bölümünde **Proxy sunucusu olmadan doğrudan Azure Site Recovery hizmetine bağlan** seçeneğini belirleyin.
7. **Kayıt'ta,** sunucu kasaya kaydedildikten sonra **Finish'i**seçin.

Hyper-V sunucusundan gelen meta veriler Azure Site Kurtarma tarafından alınır ve sunucu **Site Kurtarma Altyapısı** > **Hyper-V Ana Bilgisayarlarında**görüntülenir. Bu işlemin tamamlanması 30 dakika sürebilir.

#### <a name="install-the-provider-on-a-hyper-v-core-server"></a>Sağlayıcıyı Hyper-V çekirdek sunucusuna yükleme

Hyper-V çekirdek sunucusu çalıştırıyorsanız, kurulum dosyasını indirin ve aşağıdaki adımları izleyin:

1. Bu komutu çalıştırarak dosyaları AzureSiteRecoveryProvider.exe'den yerel bir dizine ayıklayın:

    `AzureSiteRecoveryProvider.exe /x:. /q`
 
2. `.\setupdr.exe /i` öğesini çalıştırın. Sonuçlar %Programdata%\ASRLogs\DRASetupWizard.log'a kaydedilir.

3. Bu komutu çalıştırarak sunucuyu kaydedin:

    ```
    cd  "C:\Program Files\Microsoft Azure Site Recovery Provider\DRConfigurator.exe" /r /Friendlyname "FriendlyName of the Server" /Credentials "path to where the credential file is saved"
    ```

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynakları seçin ve doğrulayın:

1. Altyapı**Hedef** **Hazırla'yı** > seçin.
2. Azure VM'lerinin başarısız olduktan sonra oluşturulacağı abonelik ve **ContosoRG** kaynak grubunu seçin.
3. **Kaynak Yöneticisi** dağıtım modelini seçin.

Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.

## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

1. Altyapı**Çoğaltma Ayarlarını** >  **Hazırla** > **+Oluştur'u seçin ve ilişkilendirin.**
2. **Oluştur ve ilişkilendirilkesinde,** bir ilke adı belirtin. **ContosoReplicationPolicy**kullanıyoruz.
3. Bu öğretici için varsayılan ayarları bırakacağız:
    - **Kopyalama sıklığı** delta verilerinin (ilk çoğaltmadan sonra) ne sıklıkta çoğaltılacağını gösterir. Varsayılan sıklık her beş dakikada birdir.
    - **Kurtarma noktası tutma,** kurtarma noktalarının iki saat boyunca tutulacağını gösterir.
    - **Uygulamayla tutarlı anlık görüntü sıklığı**, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının her saat oluşturulacağını belirtir.
    - **İlk çoğaltma başlangıç saati,** ilk çoğaltmanın hemen başlayacağını gösterir.
4. İlke oluşturulduktan sonra **Tamam'ı**seçin. Yeni bir ilke oluşturduğunuzda, bu politika belirtilen Hyper-V sitesiyle otomatik olarak ilişkilendirilmez. Bizim öğretici olarak, bu **ContosoHyperVSite's.**

    ![Çoğaltma ilkesi](./media/hyper-v-azure-tutorial/replication-policy.png)

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

1. **Çoğaltma uygulamasında** **Kaynak'ı**seçin.
2. **Kaynak** bölümünde **ContosoHyperVSite** sitesini seçin. Ardından **Tamam'ı**seçin.
3. **Hedef'te,** hedefi (Azure), kasa aboneliğini ve Kaynak **Yöneticisi** dağıtım modelini doğrulayın.
4. Öğretici ayarları kullanıyorsanız, çoğaltılan veriler için önceki öğreticide oluşturulan **contosovmsacct1910171607** depolama hesabını seçin. Ayrıca, Azure VM'lerinin başarısız olduktan sonra bulunacağı **ContosoASRnet** ağını da seçin.
5. **Sanal makinelerde** > **Seçin**, çoğaltmak istediğiniz VM'yi seçin. Ardından **Tamam'ı**seçin.

   **İşler** > **Site Recovery işleri** bölümünde **Korumayı Etkinleştir** eyleminin ilerleme durumunu izleyebilirsiniz. **Finalize Protection** işi tamamlandıktan sonra, ilk çoğaltma tamamlandı ve VM başarısız olmaya hazır.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)

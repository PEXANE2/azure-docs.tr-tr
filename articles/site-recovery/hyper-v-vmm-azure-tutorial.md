---
title: Azure Site Kurtarma'yı kullanarak Hyper-V (VMM ile) olağanüstü kurtarma yı ayarlama
description: Site Kurtarma'yı kullanarak Sistem Merkezi VMM bulutlarında şirket içi Hyper-V V M'lerin olağanüstü durum kurtarmasını Azure'a nasıl ayarlayayize edin.
ms.topic: tutorial
ms.date: 03/19/2020
ms.custom: MVC
ms.openlocfilehash: a391d8eb3cf7bc43b52883cbf2e76170338c44c6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80067586"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>VMM bulutlarında şirket içi Hyper-V sanal makineleri için Azure’da olağanüstü durum kurtarma ayarlama

Bu öğretici, System Center Virtual Machine Manager (VMM) tarafından yönetilen şirket içi Hyper-V sanal makinelerin (VM) [Azure Site Kurtarma'yı](site-recovery-overview.md)kullanarak Azure'a çoğaltmayı nasıl etkinleştireceğimi açıklar. VMM kullanmıyorsanız, bu [öğreticiizleyin.](hyper-v-azure-tutorial.md)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağınızı ve hedefinizi seçme.
> * Şirket içi Site Kurtarma bileşenleri ve hedef çoğaltma ortamı da dahil olmak üzere kaynak çoğaltma ortamını ayarlayın.
> * VMM VM ağları ve Azure sanal ağları arasında ağ eşlemi ayarlama.
> * Bir çoğaltma ilkesi oluşturma.
> * Sanal makine için çoğaltmayı etkinleştirme.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı talimatlar için, [Site Kurtarma belgelerinin](/azure/site-recovery/) **Nasıl Yapilir Kılavuzları** bölümündeki makaleleri inceleyin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, aşağıdaki öğreticileri zaten tamamladığınızı varsayar:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
1. [Şirket içi Hyper-V sunucuları hazırlayın](hyper-v-prepare-on-premises-tutorial.md)

## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme

1. Azure portalında Kurtarma **Hizmetleri kasalarına** gidin ve [Azure'u Hazırla](tutorial-prepare-azure.md#create-a-recovery-services-vault) öğreticisinde oluşturulan **ContosoVMVault** kasasını seçin.
1. **Başlarken,** **Site Kurtarma** > **Hazırlama Altyapısını** seçin ve aşağıdaki ayarları yapılayın:
    1. **Koruma hedefi** >  **On-premises****Makineleriniz nerede bulunur?**
    1. **Makinelerinizi nerede çoğaltmak istiyorsunuz?** **To Azure**
    1. **Makineleriniz sanallaştırıldı mı?** **Yes, with Hyper-V**
    1. **Hyper-V ana bilgisayarlarınızı yönetmek için System Center VMM**kullanıyor musunuz? **Yes**
1. **Tamam'ı**seçin.

   ![Çoğaltma hedefi](./media/hyper-v-vmm-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

1. **Dağıtım planlamasında,** büyük bir dağıtım planlıyorsanız, Sayfadaki bağlantıdan Hyper-V için Dağıtım Planlayıcısını indirin. Hyper-V dağıtım planlaması hakkında [daha fazla bilgi edinin.](hyper-v-deployment-planner-overview.md)
1. Bu öğretici için Dağıtım Planlayıcısı'na ihtiyacımız yoktur. **Dağıtım planlamayı tamamladınız mı?** **I will do it later** **OK**

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamını ayarladığınızda, Azure Site Kurtarma Sağlayıcısı'nı VMM sunucusuna yükler ve sunucuyu kasaya kaydettirin. Azure Kurtarma Hizmetleri aracısını her Hyper-V ana bilgisayara yüklersiniz.

1. **Altyapı Hazırlayın.** **Kaynak'ı**seçin.
1. **Kaynak hazırlayın.** VMM sunucusu eklemek için **+ VMM'yi** seçin. **Sunucu Ekle** bölümünde **Sunucu türü** olarak **System Center VMM sunucusu** girişinin olduğundan emin olun.
1. Microsoft Azure Site Kurtarma Sağlayıcısı için yükleyiciyi indirin.
1. Kasa kayıt anahtarını indirin. Sağlayıcı kurulumlarını çalıştırırken bu anahtara ihtiyacınız vardır. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.
1. Microsoft Azure Kurtarma Hizmetleri aracısı için yükleyiciyi indirin.

   ![İndir sağlayıcısı, kayıt anahtarı ve aracı](./media/hyper-v-vmm-azure-tutorial/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>Sağlayıcıyı VMM sunucusuna yükleme

1. Azure Site Kurtarma Sağlayıcısı Kurulumu sihirbazında **Microsoft Update**. Sağlayıcı güncelleştirmelerini denetlemek için Microsoft Update'i kullanmayı tercih edin.
1. **Kurulum**. Sağlayıcı için varsayılan yükleme konumunu kabul edin ve **Yükle'yi**seçin.
1. Yüklemeden sonra, Microsoft Azure Site Kurtarma Kayıt Sihirbazı'nda **Vault Ayarları**, **Gözat**ve **Anahtar dosyasında**, indirdiğiniz kasa anahtar dosyasını seçin.
1. Azure Site Kurtarma aboneliğini ve kasa adını **(ContosoVMVault)** belirtin. Kasada tanımlamak için VMM sunucusu için uygun bir ad belirtin.
1. **Proxy Ayarları**. **Proxy olmadan doğrudan Azure Site Kurtarma'ya Bağlan'ı**seçin.
1. Verileri şifrelemek için kullanılan sertifikanın varsayılan konumunu kabul edin. Şifrelenmiş veriler üzerinde başarısız olduğunda şifresi çözülür.
1. **Bulut meta verilerini senkronize edin.** **Site Kurtarma portalına bulut meta verilerini eşitle'yi**seçin. Bu eylemin her sunucuda yalnızca bir kez gerçekleşmesi gerekir. Ardından, **Kaydol'u**seçin.
1. Sunucu kasaya kaydedildikten sonra **Finish'i**seçin.

Kayıt bittikten sonra, sunucudan gelen meta veriler Azure Site Kurtarma tarafından alınır ve VMM sunucusu **Site Kurtarma Altyapısı'nda**görüntülenir.

### <a name="install-the-recovery-services-agent-on-hyper-v-hosts"></a>Kurtarma Hizmetleri aracısını Hyper-V konaklarına yükleme

Çoğaltmak istediğiniz VM'leri içeren her Hyper-V ana bilgisayara aracıyı yükleyin.

Microsoft Azure Kurtarma Hizmetleri Aracı Kurulum Sihirbazı'nda şu ayarları yapılandırın:

1. **Önkoşullar Kontrol**. **Sonraki'ni**seçin. Eksik olan ön koşullar otomatik olarak yüklenir.
1. **Kurulum Ayarları**. Yükleme konumunu ve önbellek konumunu kabul edin. Önbellek sürücüsünün en az 5 GB depolama alanına ihtiyacı vardır. 600 GB veya daha fazla boş alana sahip bir sürücü öneririz. Ardından **Yükle'yi**seçin.
1. **Kurulum**. Yükleme bittiğinde, sihirbazı bitirmek için **Kapat'ı** seçin.

   ![Aracıyı yükle](./media/hyper-v-vmm-azure-tutorial/mars-install.png)

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

1. Altyapı**Hedef** **Hazırla'yı** > seçin.
1. Azure VM'lerinin başarısız olduktan sonra oluşturulacağı abonelik ve kaynak grubunu **(ContosoRG)** seçin.
1. Kaynak **Yöneticisi** dağıtım modelini seçin.

Site Kurtarma, bir veya daha fazla uyumlu Azure depolama hesabı ve ağı olup olmadığını denetler.

## <a name="configure-network-mapping"></a>Ağ eşlemesini yapılandırma

1. **Site Kurtarma Altyapı** > **Ağı eşlemeleri** > **Ağ eşleme.** **+Ağ Eşleme** simgesini seçin.
1. **Ağ eşleme ekle.** Kaynak **Sistem Merkezi VMM** sunucusunu seçin. **Hedef**için Azure'u seçin.
1. Yük devretme işleminden sonra dağıtım modelini ve aboneliği seçin.
1. **Kaynak ağ**. Kaynak şirket içi VM ağını seçin.
1. **Hedef ağ**. Başarısız olduktan sonra oluşturulduklarında azure vm'lerinin çoğaltılacağı Azure ağını seçin. Ardından **Tamam'ı**seçin.

   ![Ağ eşlemesi](./media/hyper-v-vmm-azure-tutorial/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

1. Altyapı**Çoğaltma Ayarlarını** >  **Hazırla** > **+Oluştur'u seçin ve ilişkilendirin.**
1. **Oluştur ve ilişkilendirilkesinde,** bir ilke adı belirtin. **ContosoReplicationPolicy**kullanıyoruz.
1. Varsayılan ayarları kabul edin ve **Tamam'ı**seçin:
   - **Kopyalama sıklığı,** ilk çoğaltmadan sonra delta verilerinin her beş dakikada bir çoğaltılacağını gösterir.
   - **Kurtarma noktası bekletme,** her kurtarma noktasının iki saat boyunca tutulacağını gösterir.
   - **Uygulamayla tutarlı anlık görüntü sıklığı**, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının her saat oluşturulacağını belirtir.
   - **İlk çoğaltma başlangıç saati,** ilk çoğaltmanın hemen başlayacağını gösterir.
   - **Azure'da depolanan verileri şifrelemek** varsayılan **(Kapalı)** olarak ayarlanır ve Azure'daki istirahat verilerinin şifrelenmediğini gösterir.
1. İlke oluşturulduktan sonra **Tamam'ı**seçin. Yeni bir ilke oluşturduğunuzda, otomatik olarak VMM bulutuyla ilişkilendirilsin.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

1. **Uygulamayı çoğaltma .** **Kaynak'ı**seçin.
1. **Kaynak**. VMM bulutu'nu seçin. Ardından **Tamam'ı**seçin.
1. **Hedef**. Hedef (Azure), kasa aboneliğini doğrulayın ve **Kaynak Yöneticisi** modelini seçin.
1. **Contosovmsacct1910171607** depolama hesabını ve **ContosoASRnet** Azure ağını seçin.
1. **Sanal makineler** > **seçin.** Çoğaltmak istediğiniz VM'yi seçin. Ardından **Tamam'ı**seçin.

   **İşler** > **Site Recovery işleri** bölümünde **Korumayı Etkinleştir** eyleminin ilerleme durumunu izleyebilirsiniz. **Finalize Protection** işi tamamlandıktan sonra, ilk çoğaltma tamamlandı ve VM başarısız olmaya hazır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)

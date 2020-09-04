---
title: Azure Site Recovery kullanarak Hyper-V (VMM ile) olağanüstü durum kurtarma ayarlama
description: Site Recovery kullanarak, System Center VMM bulutlarındaki şirket içi Hyper-V VM 'lerinin olağanüstü durum kurtarma ayarlamayı öğrenin.
ms.topic: tutorial
ms.date: 03/19/2020
ms.custom: MVC
ms.openlocfilehash: c806f968bc6530879f64ddbf6fd4c7d45aa7a8d3
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89442829"
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>VMM bulutlarında şirket içi Hyper-V sanal makineleri için Azure’da olağanüstü durum kurtarma ayarlama

Bu öğreticide, [Azure Site Recovery](site-recovery-overview.md)kullanılarak Azure 'a System Center VIRTUAL MACHINE Manager (VMM) tarafından yönetilen şirket içi Hyper-V sanal MAKINELERI (VM) için çoğaltmanın nasıl etkinleştirileceği açıklanmaktadır. VMM kullanmıyorsanız, [Bu öğreticiyi izleyin](hyper-v-azure-tutorial.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çoğaltma kaynağınızı ve hedefinizi seçme.
> * Şirket içi Site Recovery bileşenleri ve hedef çoğaltma ortamı dahil olmak üzere kaynak çoğaltma ortamını ayarlayın.
> * VMM VM ağları ve Azure sanal ağları arasında ağ eşlemesi ayarlayın.
> * Bir çoğaltma ilkesi oluşturma.
> * Sanal makine için çoğaltmayı etkinleştirme.

> [!NOTE]
> Öğreticiler, bir senaryo için en basit dağıtım yolunu gösterir. Mümkün olduğunca varsayılan seçenekleri kullanır ve tüm olası ayarları ve yolları göstermez. Ayrıntılı yönergeler için [Site Recovery belgelerinin](./index.yml) **nasıl yapılır kılavuzlarındaki** makaleleri gözden geçirin.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, aşağıdaki öğreticileri zaten tamamladığınız varsayılmaktadır:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
1. [Şirket içi Hyper-V sunucuları hazırlayın](hyper-v-prepare-on-premises-tutorial.md)

## <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme

1. Azure portal, **Kurtarma Hizmetleri kasaları** ' na gidin ve [Azure 'u hazırla](tutorial-prepare-azure.md#create-a-recovery-services-vault) öğreticisinde oluşturulan **contosovmkasa** kasasını seçin.
1. **Başlarken** **Site Recovery**  >  **altyapıyı hazırla** ' yı seçin ve aşağıdaki ayarları yapılandırın:
    1. **Koruma hedefi**  >  **Makineleriniz nerede bulunuyor?**, **Şirket içi**seçeneğini belirleyin.
    1. **Makinelerinizi nereye çoğaltmak istiyorsunuz?**, **Azure 'a**seçin.
    1. **Makineleriniz sanallaştırılmış mı?**, **Hyper-V ile Evet '** i seçin.
    1. **Hyper-V konaklarınızı yönetmek Için System Center VMM kullanıyor musunuz?**, **Evet**' i seçin.
1. **Tamam**’ı seçin.

   ![Çoğaltma hedefi](./media/hyper-v-vmm-azure-tutorial/replication-goal.png)

## <a name="confirm-deployment-planning"></a>Dağıtım planlamasını onaylama

1. **Dağıtım planlaması**' nda, büyük bir dağıtım planlıyorsanız, Hyper-V ' y i dağıtım planlayıcısı sayfadaki bağlantıdan indirin. Hyper-V dağıtım planlaması hakkında [daha fazla bilgi edinin](hyper-v-deployment-planner-overview.md) .
1. Bu öğretici için Dağıtım Planlayıcısı gerekli değildir. **Dağıtım planlamasını tamamladınız mı?**, **daha sonra yapacağım**' i seçin ve ardından **Tamam**' ı seçin.

## <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama

Kaynak ortamı ayarlarken, VMM sunucusuna Azure Site Recovery sağlayıcıyı yüklersiniz ve sunucuyu kasaya kaydedersiniz. Azure kurtarma hizmetleri Aracısı 'nı her Hyper-V konağına yüklersiniz.

1. **Altyapıyı hazırlayın**. **Kaynak**seçin.
1. **Kaynağı hazırlayın**. VMM sunucusu eklemek için **+ VMM** ' yi seçin. **Sunucu Ekle** bölümünde **Sunucu türü** olarak **System Center VMM sunucusu** girişinin olduğundan emin olun.
1. Microsoft Azure Site Recovery sağlayıcısı için yükleyiciyi indirin.
1. Kasa kayıt anahtarını indirin. Sağlayıcı kurulumunu çalıştırdığınızda bu anahtar gerekir. Anahtar, oluşturulduktan sonra beş gün boyunca geçerlidir.
1. Microsoft Azure Kurtarma Hizmetleri Aracısı için yükleyiciyi indirin.

   ![Sağlayıcıyı, kayıt anahtarını ve aracıyı indir](./media/hyper-v-vmm-azure-tutorial/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>Sağlayıcıyı VMM sunucusuna yükleme

1. Azure Site Recovery sağlayıcısı Kurulum Sihirbazı **Microsoft Update**. Sağlayıcı güncelleştirmelerini denetlemek için Microsoft Update kullanmayı tercih edin.
1. **Yükleme**. Sağlayıcının varsayılan yükleme konumunu kabul edin ve **Yükleme ' yi**seçin.
1. Yüklemeden sonra, Microsoft Azure Site Recovery kayıt sihirbazında **kasa ayarları**' nı seçin ve **anahtar dosyasına** **gidip**, indirdiğiniz kasa anahtarı dosyasını seçin.
1. Azure Site Recovery aboneliğini ve kasa adını (**Contosovmkasa**) belirtin. VMM sunucusu için kasada tanımlamak üzere bir kolay ad belirtin.
1. **Ara sunucu ayarları**. **Proxy olmadan doğrudan Azure Site Recovery Bağlan**' ı seçin.
1. Verileri şifrelemek için kullanılan sertifika için varsayılan konumu kabul edin. Yük devretmek için şifrelenen verilerin şifresi çözülür.
1. **Bulut meta verilerini eşitler**. **Site Recovery Portal 'da bulut meta verilerini Eşitle**' yi seçin. Bu eylemin her bir sunucuda yalnızca bir kez gerçekleşmesi gerekir. Sonra **Kaydet**' i seçin.
1. Sunucu kasada kaydedildikten sonra **son**' u seçin.

Kayıt tamamlandıktan sonra, sunucudan gelen meta veriler Azure Site Recovery tarafından alınır ve VMM sunucusu **Site Recovery altyapısında**görüntülenir.

### <a name="install-the-recovery-services-agent-on-hyper-v-hosts"></a>Kurtarma Hizmetleri aracısını Hyper-V konaklarına yükleme

Aracıyı, çoğaltmak istediğiniz VM 'Leri içeren her Hyper-V konağına yükleyebilirsiniz.

Microsoft Azure Kurtarma Hizmetleri Aracısı kurulum sihirbazında, şu ayarları yapılandırın:

1. **Önkoşul denetimi**. **İleri**’yi seçin. Eksik Önkoşullar otomatik olarak yüklenir.
1. **Yükleme ayarları**. Yükleme konumunu kabul edin. Ardından, **Install**' ı seçin.

    >[!NOTE]
    >Azure Site Recovery için **önbellek konumu** gerekli değildir.

1. **Yükleme**. Yükleme tamamlandığında, Sihirbazı kapatmak için **Kapat** ' ı seçin.

   ![Aracıyı yükler](./media/hyper-v-vmm-azure-tutorial/mars-install.png)

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

1. **Altyapı hedefini hazırla**' yı seçin  >  **Target**.
1. Yük devretmeden sonra Azure VM 'lerinin oluşturulacağı aboneliği ve kaynak grubunu (**ContosoRG**) seçin.
1. **Kaynak Yöneticisi** dağıtım modelini seçin.

Site Recovery, bir veya daha fazla uyumlu Azure depolama hesabı ve ağı olup olmadığını denetler.

## <a name="configure-network-mapping"></a>Ağ eşlemesini yapılandırma

1. **Site Recovery altyapısı**  >  **Ağ eşlemeleri**  >  **Ağ eşleme**. **+ Ağ eşleme** simgesini seçin.
1. **Ağ eşlemesi ekleyin**. **Kaynak System Center VMM** sunucusunu seçin. **Hedef**için Azure ' u seçin.
1. Yük devretme işleminden sonra dağıtım modelini ve aboneliği seçin.
1. **Kaynak ağ**. Kaynak şirket içi VM ağını seçin.
1. **Hedef ağ**. Yük devretmeden sonra oluşturuldukları zaman, çoğaltmanın Azure VM 'lerinin bulunduğu Azure ağını seçin. Ardından **Tamam**' ı seçin.

   ![Ağ eşlemesi](./media/hyper-v-vmm-azure-tutorial/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

1. **Altyapıyı hazırla**  >  **çoğaltma ayarları**  >  **+ Oluştur ve ilişkilendir '** i seçin.
1. **Ilke oluştur ve ilişkilendir**bölümünde bir ilke adı belirtin. **Contosoreplicationpolicy**kullanıyorsunuz.
1. Varsayılan ayarları kabul edin ve **Tamam**' ı seçin:
   - **Kopyalama sıklığı** , ilk çoğaltmadan sonra Delta verilerinin her beş dakikada bir çoğaltılacağını belirtir.
   - **Kurtarma noktası bekletme** , her kurtarma noktasının iki saat boyunca bekletileceği anlamına gelir.
   - **Uygulamayla tutarlı anlık görüntü sıklığı**, uygulamayla tutarlı anlık görüntüleri içeren kurtarma noktalarının her saat oluşturulacağını belirtir.
   - **İlk çoğaltma başlangıç zamanı** , ilk çoğaltmanın hemen başlatılacağını gösterir.
   - **Azure 'da depolanan verileri şifreleme** varsayılan (**kapalı**) olarak ayarlanır ve Azure 'daki bekleyen verilerin şifrelenmediğini belirtir.
1. İlke oluşturulduktan sonra **Tamam**' ı seçin. Yeni bir ilke oluşturduğunuzda, VMM bulutu ile otomatik olarak ilişkilendirilir.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

1. **Uygulamayı çoğaltın**. **Kaynak**seçin.
1. **Kaynak**. VMM bulutunu seçin. Ardından **Tamam**' ı seçin.
1. **Hedef**. Hedefi (Azure), kasa aboneliğini doğrulayın ve **Kaynak Yöneticisi** modeli seçin.
1. **Contosovmsacct1910171607** Storage hesabını ve **contosoasrnet** Azure ağını seçin.
1. **Sanal makineler**  >  Öğesini **seçin**. Çoğaltmak istediğiniz VM 'yi seçin. Ardından **Tamam**' ı seçin.

   **İşler** > **Site Recovery işleri** bölümünde **Korumayı Etkinleştir** eyleminin ilerleme durumunu izleyebilirsiniz. **Korumayı Sonlandır** işi tamamlandıktan sonra, ilk çoğaltma TAMAMLANıR ve VM yük devretme için hazırlayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Olağanüstü durum kurtarma tatbikatı çalıştırma](tutorial-dr-drill-azure.md)

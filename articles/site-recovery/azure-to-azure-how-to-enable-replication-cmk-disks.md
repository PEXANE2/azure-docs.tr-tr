---
title: Azure Site Recovery şifreli Azure VM 'Leri için çoğaltmayı etkinleştirme
description: Bu makalede, Site Recovery kullanarak, müşteri tarafından yönetilen anahtar (CMK) özellikli disklerle, bir Azure bölgesinden diğerine yapılan sanal makineler için çoğaltmanın nasıl yapılandırılacağı açıklanır.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 01/10/2020
ms.author: mayg
ms.openlocfilehash: 367f29237a3f2a634f209026df47b0cbd6ffc97c
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75897968"
---
# <a name="replicate-machines-with-customer-managed-keys-cmk-enabled-disks"></a>Müşteri tarafından yönetilen anahtarlar (CMK) özellikli diskler ile makineleri çoğaltma

Bu makalede, Azure VM 'lerinin bir Azure bölgesinden diğerine, müşteri tarafından yönetilen anahtarlar (CMK) özellikli yönetilen disklerle nasıl çoğaltılacağı açıklanır.

## <a name="prerequisite"></a>Önkoşul
CMK özellikli yönetilen disklere sahip sanal makineleriniz için çoğaltmayı etkinleştirmeden önce hedef aboneliğin hedef bölgesinde disk şifreleme kümesi (ler) i oluşturmanız gerekir.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Bu örnekte, birincil Azure Bölgesi Doğu Asya ve ikincil bölge Güney Doğu Asya ' dir.

1. Kasada **+ Çoğalt**' ı seçin.
2. Aşağıdaki alanlara göz önünde edin.
    - **Kaynak**: Bu örnekte **Azure**olan VM 'lerin kaynak noktası.
    - **Kaynak konumu**: sanal makinelerinizi korumak istediğiniz Azure bölgesi. Bu örnekte, kaynak konumu "Doğu Asya" dir.
    - **Dağıtım modeli**: kaynak makinelerin Azure dağıtım modeli.
    - **Kaynak aboneliği**: kaynak sanal makinelerinizin ait olduğu abonelik. Kurtarma Hizmetleri kasasıyla aynı Azure Active Directory kiracısındaki herhangi bir abonelik olabilir.
    - **Kaynak grubu**: kaynak sanal makinelerinizin ait olduğu kaynak grubu. Seçilen kaynak grubundaki tüm VM 'Ler, bir sonraki adımda koruma için listelenir.

3. Sanal **makineler** > **sanal makineler**' i seçin, çoğaltmak istediğiniz her VM 'yi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Sonra **Tamam**’ı seçin.

4. **Ayarlar**' da, aşağıdaki hedef site ayarlarını yapılandırabilirsiniz.

    - **Hedef konum**: kaynak sanal makine verilerinizin çoğaltılacağı konum. Site Recovery, seçilen makinenin konumuna göre uygun hedef bölgelerin bir listesini sağlar. Kurtarma Hizmetleri kasasının konumuyla aynı konumu kullanmanızı öneririz.
    - **Hedef abonelik**: olağanüstü durum kurtarma için kullanılan hedef abonelik. Varsayılan olarak, hedef abonelik, kaynak abonelikle aynı olur.
    - **Hedef kaynak grubu**: tüm çoğaltılan sanal makinelerinizin ait olduğu kaynak grubu. Varsayılan olarak, Site Recovery hedef bölgede yeni bir kaynak grubu oluşturur. Ad `asr` sonekini alır. Azure Site Recovery tarafından oluşturulan bir kaynak grubu zaten varsa, yeniden kullanılır. Ayrıca, aşağıdaki bölümde gösterildiği gibi, özelleştirmeyi de seçebilirsiniz. Hedef kaynak grubunun konumu, kaynak sanal makinelerin barındırıldığı bölge dışında herhangi bir Azure bölgesi olabilir.
    - **Hedef sanal ağ**: Site Recovery, varsayılan olarak hedef bölgede yeni bir sanal ağ oluşturur. Ad `asr` sonekini alır. Kaynak ağınızla eşlenir ve gelecekte herhangi bir koruma için kullanılır. Ağ eşlemesi hakkında [daha fazla bilgi edinin](site-recovery-network-mapping-azure-to-azure.md) .
    - **Hedef depolama hesapları (kaynak VM 'niz yönetilen diskleri kullanmıyorsa)** : varsayılan olarak Site Recovery, kaynak VM depolama yapılandırmanızı inceleyerek yeni bir hedef depolama hesabı oluşturur. Bir depolama hesabı zaten varsa, yeniden kullanılır.
    - **Yönetilen çoğaltma diskleri (kaynak VM 'niz yönetilen diskler kullanıyorsa)** : Site Recovery, kaynak VM 'nin yönetilen diskleri ile aynı depolama türündeki (Standart veya Premium) kaynak sanal makinenin yönetilen disklerini yansıtmak için hedef bölgede yeni bir çoğaltma yönetilen disk oluşturur.
    - **Önbellek depolama hesapları**: Site Recovery, kaynak bölgede *önbellek depolaması* adlı ek bir depolama hesabı gerektirir. Kaynak VM 'lerdeki tüm değişiklikler izlenir ve önbellek depolama hesabına gönderilir. Daha sonra hedef konuma çoğaltılır.
    - **Kullanılabilirlik kümesi**: Site Recovery, varsayılan olarak hedef bölgede yeni bir kullanılabilirlik kümesi oluşturur. Adın `asr` soneki vardır. Site Recovery tarafından oluşturulan bir kullanılabilirlik kümesi zaten varsa, yeniden kullanılır.
    - **Disk şifreleme kümeleri (DES)** : Site Recovery, çoğaltma ve hedef yönetilen diskler için kullanılacak disk şifreleme kümelerini gerektirir. Çoğaltmayı etkinleştirmeden önce hedef abonelikte ve hedef bölgede DES 'i önceden oluşturmanız gerekir. Varsayılan olarak, DES seçili değildir. Kaynak disk başına DES seçmek için ' Özelleştir ' seçeneğine tıklamanız gerekir.
    - **Çoğaltma İlkesi**: kurtarma noktası bekletme geçmişi ve uygulamayla tutarlı anlık görüntü sıklığı için ayarları tanımlar. Varsayılan olarak Site Recovery, kurtarma noktası bekletmesi için varsayılan ayarları *24 saat* ile, uygulamayla tutarlı anlık görüntü sıklığı için *60 dakika* olan yeni bir çoğaltma ilkesi oluşturur.

    ![CMK özellikli disklere sahip makine için çoğaltmayı etkinleştirme](./media/azure-to-azure-how-to-enable-replication-cmk-disks/cmk-enable-dr.png)

## <a name="customize-target-resources"></a>Hedef kaynakları özelleştirme

Site Recovery varsayılan hedef ayarlarını değiştirmek için aşağıdaki adımları izleyin.

1. Varsayılan hedef aboneliği değiştirmek için "hedef abonelik" yanındaki **Özelleştir** ' i seçin. Azure AD kiracısında bulunan abonelikler listesinden aboneliği seçin.

2. Aşağıdaki varsayılan ayarları değiştirmek için "kaynak grubu, ağ, depolama ve kullanılabilirlik kümeleri" yanındaki **Özelleştir** ' i seçin:
    - **Hedef kaynak grubu**için, aboneliğin hedef konumundaki kaynak grupları listesinden kaynak grubunu seçin.
    - **Hedef sanal ağ**için, hedef konumdaki sanal ağlar listesinden ağı seçin.
    - **Kullanılabilirlik**kümesi için, kaynak bölgedeki bir kullanılabilirlik KÜMESININ parçasıysa VM 'ye kullanılabilirlik kümesi ayarları ekleyebilirsiniz.
    - **Hedef depolama hesapları**için kullanılacak hesabı seçin.

3. Her müşteri tarafından yönetilen anahtar (CMK) tarafından etkinleştirilmiş kaynak yönetilen diskin hedef DES ' i seçmek için "depolama şifreleme ayarları" nın yanındaki **Özelleştir** ' i seçin. Seçim sırasında, DES 'in hangi hedef anahtar kasasıyla ilişkilendirildiğini de görebileceksiniz.

4. **Çoğaltmayı etkinleştirmek** > **hedef kaynak oluştur** ' u seçin.
5. VM 'Ler çoğaltma için etkinleştirildikten sonra, **çoğaltılan öğeler**altında VM 'lerin sistem durumunu kontrol edebilirsiniz.

![CMK özellikli disklere sahip makine için çoğaltmayı etkinleştirme](./media/azure-to-azure-how-to-enable-replication-cmk-disks/cmk-customize-target-disk-properties.png)

>[!NOTE]
>İlk çoğaltma sırasında durum, görünür ilerleme olmadan yenilenmesi biraz zaman alabilir. En son durumu almak için **Yenile** ' ye tıklayın.

## <a name="faqs"></a>SSS

* Var olan bir çoğaltılan öğe üzerinde CMK 'yı etkinleştirdim, bu da CMK 'nin hedef bölgede de uygulandığından nasıl emin olabilirim?

    Çoğaltma yönetilen diskinin adını (hedef bölgede Azure Site Recovery tarafından oluşturulan) bulabilir ve bu çoğaltma diskine DES iliştirebilirsiniz. Bununla birlikte, bir kez ekledikten sonra, diskler dikey penceresinde DES ayrıntılarını göremezsiniz. Alternatif olarak, VM çoğaltmasını devre dışı bırakmayı ve yeniden etkinleştirmeyi de seçebilirsiniz. Bu, çoğaltılan öğenin diskler dikey penceresinde DES ve Anahtar Kasası ayrıntılarını görmenizi sağlayacak.

* Çoğaltılan öğeye yeni bir CMK özellikli disk ekledim. Bu diski Azure Site Recovery nasıl çoğaltabilirim?

    Yeni bir CMK özellikli diskin varolan bir çoğaltılan öğeye eklenmesi desteklenmez. Çoğaltmayı devre dışı bırakın ve sanal makine için çoğaltmayı yeniden etkinleştirin.


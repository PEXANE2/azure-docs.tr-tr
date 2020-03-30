---
title: Azure Site Kurtarma'da şifrelenmiş Azure VM'leri için çoğaltmayı etkinleştirme
description: Bu makalede, Site Kurtarma'yı kullanarak bir Azure bölgesinden müşteri tarafından yönetilen anahtar (CMK) etkinleştirilmiş disklerle VM'ler için çoğaltmanın nasıl yapılandırılabildiğini açıklanmaktadır.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 01/10/2020
ms.author: mayg
ms.openlocfilehash: 367f29237a3f2a634f209026df47b0cbd6ffc97c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75897968"
---
# <a name="replicate-machines-with-customer-managed-keys-cmk-enabled-disks"></a>Müşteri Tarafından Yönetilen Anahtarlar (CMK) özellikli disklerle makineleri çoğaltma

Bu makalede, Azure VM'lerinin Müşteri Tarafından Yönetilen Anahtarlar (CMK) etkinleştirilmiş yönetilen disklerle bir Azure bölgesinden diğerine nasıl çoğaltılacağını açıklanmaktadır.

## <a name="prerequisite"></a>Önkoşul
CMK özellikli yönetilen disklere sahip sanal makineleriniz için çoğaltmayı etkinleştirmeden önce hedef abonelik için hedef bölgede Disk Şifreleme kümesi(ler) oluşturmanız gerekir.

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Bu örnekte, birincil Azure bölgesi Doğu Asya, ikincil bölge ise Güney Doğu Asya'dır.

1. Kasada **+Çoğaltma'yı**seçin.
2. Aşağıdaki alanlara dikkat edin.
    - **Kaynak**: Bu durumda **Azure**olan VM'lerin çıkış noktası.
    - **Kaynak konumu**: Sanal makinelerinizi korumak istediğiniz Azure bölgesi. Bu örnekiçin, kaynak konumu "Doğu Asya" dır.
    - **Dağıtım modeli**: Kaynak makinelerin Azure dağıtım modeli.
    - Kaynak abonelik : Kaynak sanal makinelerinizin ait olduğu **abonelik.** Kurtarma hizmetleri kasanızla aynı Azure Active Directory kiracısında bulunan herhangi bir abonelik olabilir.
    - **Kaynak Grubu**: Kaynak sanal makinelerinizin ait olduğu kaynak grubudur. Seçili kaynak grubundaki tüm VM'ler bir sonraki adımda korunmak üzere listelenir.

3. **Sanal Makinelerde** > **Sanal Makineleri seçin,** çoğaltmak istediğiniz her VM'yi seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Ardından **Tamam'ı**seçin.

4. **Ayarlar'da,** aşağıdaki hedef site ayarlarını yapılandırabilirsiniz.

    - **Hedef konum**: Kaynak sanal makine verilerinizin çoğaltılacağı konum. Site Kurtarma, seçilen makinenin konumuna göre uygun hedef bölgelerin listesini sağlar. Kurtarma Hizmetleri kasasının konumuyla aynı konumu kullanmanızı öneririz.
    - **Hedef abonelik**: Olağanüstü durum kurtarma için kullanılan hedef abonelik. Varsayılan olarak, hedef abonelik kaynak abonelikle aynıdır.
    - **Hedef kaynak grubu**: Çoğaltılan tüm sanal makinelerinizin ait olduğu kaynak grubudur. Varsayılan olarak, Site Kurtarma hedef bölgede yeni bir kaynak grubu oluşturur. Ad sonek `asr` alır. Azure Site Kurtarma tarafından oluşturulan bir kaynak grubu zaten varsa, yeniden kullanılır. Aşağıdaki bölümde gösterildiği gibi özelleştirmeyi de seçebilirsiniz. Hedef kaynak grubunun konumu, kaynak sanal makinelerin barındırıldığı bölge dışında herhangi bir Azure bölgesi olabilir.
    - **Hedef sanal ağ**: Varsayılan olarak, Site Kurtarma hedef bölgede yeni bir sanal ağ oluşturur. Ad sonek `asr` alır. Kaynak ağınıza eşlenir ve gelecekteki herhangi bir koruma için kullanılır. Ağ eşleme hakkında [daha fazla bilgi edinin.](site-recovery-network-mapping-azure-to-azure.md)
    - **Hedef depolama hesapları (kaynağınız VM yönetilen diskleri kullanmıyorsa)**: Varsayılan olarak, Site Kurtarma, kaynak VM depolama yapılandırmanızı taklit ederek yeni bir hedef depolama hesabı oluşturur. Bir depolama hesabı zaten varsa, yeniden kullanılır.
    - **Yineleme yönetilen diskler (kaynağınız VM yönetilen diskler kullanıyorsa)**: Site Kurtarma, vm'nin yönetilen diskleri ile aynı depolama türündeki (standart veya premium) kaynağı nı yansıtmak için hedef bölgede yeni yineleme yönetilen diskler oluşturur.
    - **Önbellek depolama hesapları**: Site Kurtarma kaynak bölgede *önbellek depolama* adı verilen ek bir depolama hesabı gerekir. Kaynak VM'ler üzerindeki tüm değişiklikler izlenir ve önbellek depolama hesabına gönderilir. Daha sonra hedef konuma kopyalanırlar.
    - **Kullanılabilirlik kümesi**: Varsayılan olarak, Site Kurtarma hedef bölgede yeni bir kullanılabilirlik kümesi oluşturur. Adı sonek `asr` vardır. Site Kurtarma tarafından oluşturulan bir kullanılabilirlik kümesi zaten varsa, yeniden kullanılır.
    - **Disk şifreleme kümeleri (DES)**: Site Kurtarma, çoğaltma ve hedef yönetilen diskler için kullanılacak disk şifreleme kümesi(ler) gerekir. Çoğaltmayı etkinleştirmeden önce hedef abonelikte ve hedef bölgede DES'i önceden oluşturmanız gerekir. Varsayılan olarak, bir DES seçili değildir. Kaynak disk başına bir DES seçmek için 'Özelleştir'e tıklamanız gerekir.
    - **Çoğaltma ilkesi**: Kurtarma noktası bekletme geçmişi ve uygulama tutarlı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, Site Kurtarma kurtarma noktası bekletme için *24 saat* ve uygulama tutarlı anlık görüntü sıklığı için *60 dakika* varsayılan ayarları ile yeni bir çoğaltma ilkesi oluşturur.

    ![CMK özellikli disklere sahip makine için Çoğaltmayı etkinleştirme](./media/azure-to-azure-how-to-enable-replication-cmk-disks/cmk-enable-dr.png)

## <a name="customize-target-resources"></a>Hedef kaynakları özelleştirme

Site Kurtarma varsayılan hedef ayarlarını değiştirmek için aşağıdaki adımları izleyin.

1. Varsayılan hedef aboneliğini değiştirmek için "Hedef abonelik"in yanında **Özelleştir'i** seçin. Azure AD kiracısında bulunan abonelikler listesinden aboneliği seçin.

2. Aşağıdaki varsayılan ayarları değiştirmek için "Kaynak grubu, Ağ, Depolama ve Kullanılabilirlik kümeleri"nin yanında **Özelleştir'i** seçin:
    - **Hedef kaynak grubu**için, aboneliğin hedef konumundaki kaynak grupları listesinden kaynak grubunu seçin.
    - **Hedef sanal ağ**için, hedef konumdaki sanal ağlar listesinden ağı seçin.
    - **Kullanılabilirlik kümesi**için, kaynak bölgede ayarlanan kullanılabilirlik kümesinin bir parçasıysa, VM'ye kullanılabilirlik kümesi ayarlarını ekleyebilirsiniz.
    - **Hedef Depolama hesapları**için kullanılacak hesabı seçin.

3. Müşteri tarafından yönetilen her anahtar (CMK) etkin kaynak yönetilen disk için hedef DES'i seçmek için "Depolama şifreleme ayarları"nın yanında **Özelleştir'i** seçin. Seçim sırasında, DES'in hangi hedef tonozla ilişkili olduğunu da görebilirsiniz.

4. **Hedef kaynak** > **Oluştur'u seçin Çoğaltmayı Etkinleştir.**
5. VM'ler çoğaltma için etkinleştirildikten sonra, **Çoğaltılan öğelerin**altında VM'lerin sistem durumu durumunu kontrol edebilirsiniz.

![CMK özellikli disklere sahip makine için Çoğaltmayı etkinleştirme](./media/azure-to-azure-how-to-enable-replication-cmk-disks/cmk-customize-target-disk-properties.png)

>[!NOTE]
>İlk çoğaltma sırasında, durum belirgin ilerleme olmadan, yenilemek için biraz zaman alabilir. En son durumu almak için **Yenile'yi** tıklatın.

## <a name="faqs"></a>SSS

* Varolan bir çoğaltılan öğede CMK'yı etkinleştirdim, cmk'nin hedef bölgeye de uygulanmasını nasıl sağlayabilirim?

    Yineleme yönetilen diskinin adını (hedef bölgede Azure Site Kurtarma tarafından oluşturulan) bulabilir ve des'i bu yineleme diskine ekleyebilirsiniz. Ancak, bir kez taktığınızda Diskler bıçakteki DES ayrıntılarını göremezsiniz. Alternatif olarak, VM'nin çoğaltılmasını devre dışı bırakmayı ve yeniden etkinleştirmeyi seçebilirsiniz. Çoğaltılan öğe için Diskler bıçağında DES ve anahtar kasa ayrıntılarını görmenizi sağlar.

* Çoğaltılan öğeye yeni bir CMK etkin disk ekledim. Bu diski Azure Site Kurtarma ile nasıl çoğaltabilirim?

    Varolan bir yinelenen öğeye yeni cmk etkin disk eklenmesi desteklenmez. Çoğaltmayı devre dışı bırakıp sanal makine için çoğaltmayı yeniden etkinleştirin.


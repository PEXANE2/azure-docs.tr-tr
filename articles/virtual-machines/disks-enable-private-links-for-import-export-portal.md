---
title: Azure portal-özel bağlantılarla yönetilen disklere içeri/dışarı aktarma erişimini kısıtlama
description: Şu anda önizleme aşamasında olan Azure portal yönetilen diskleriniz için özel bağlantıları etkinleştirin. Sanal ağınızda bulunan diskleri güvenli bir şekilde dışa ve içe aktarmanıza olanak tanır.
author: roygara
ms.service: virtual-machines
ms.topic: overview
ms.date: 08/24/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 95c4464d1ab8416f609f75f2b59fb85a578ef5b7
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91979062"
---
# <a name="use-the-azure-portal-to-restrict-importexport-access-for-managed-disks-with-private-links"></a>Özel bağlantılarla yönetilen disklere yönelik içeri/dışarı aktarma erişimini kısıtlamak için Azure portal kullanın

Yönetilen diskler için özel bağlantılar desteği şu anda önizlemededir ve yönetilen disklerin içeri ve dışarı aktarılmasını, yalnızca Azure sanal ağınızda gerçekleşmeleri için kısıtlayabilmenizi sağlar. Eklenmemiş yönetilen diskler ve anlık görüntüler için, verileri bölgesel genişletme, olağanüstü durum kurtarma ve adli analiz için verileri okumak üzere diğer bölgelere aktarmaya yönelik bir zamana bağlı paylaşılan erişim Imzası (SAS) URI 'SI oluşturabilirsiniz. Ayrıca, VHD 'yi şirket içi diskinizdeki boş bir diske doğrudan yüklemek için SAS URI 'sini de kullanabilirsiniz. Sanal ağı ve yönetilen disklerdeki istemciler arasındaki ağ trafiği, Microsoft omurga ağı üzerinde yalnızca sanal ağ ve özel bir bağlantı üzerinden geçiş yaparken, genel İnternet 'te pozlamayı ortadan kaldırır.

Bir özel uç nokta oluşturarak bir disk erişim kaynağı oluşturabilir ve aynı abonelikte sanal ağınızla bağlantı oluşturabilirsiniz. Özel bağlantılar aracılığıyla verileri dışa ve içe aktarmak için bir diski veya anlık görüntüyü disk erişimiyle ilişkilendirmeniz gerekir. Ayrıca, diskin NetworkAccessPolicy özelliğini veya anlık görüntüsünü olarak ayarlamanız gerekir `AllowPrivate` . 

NetworkAccessPolicy özelliğini, `DenyAll` herhangi bir gövdenin bir disk veya anlık görüntü IÇIN SAS URI 'sini oluşturmasını engellemek üzere olarak ayarlayabilirsiniz. NetworkAccessPolicy özelliği için varsayılan değer `AllowAll` .

## <a name="limitations"></a>Sınırlamalar

[!INCLUDE [virtual-machines-disks-private-links-limitations](../../includes/virtual-machines-disks-private-links-limitations.md)]

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik

[!INCLUDE [virtual-machines-disks-private-links-regions](../../includes/virtual-machines-disks-private-links-regions.md)]

## <a name="create-a-disk-access-resource"></a>Disk erişim kaynağı oluşturma

1. Azure portal oturum açın ve [Bu bağlantıyla](https://aka.ms/disksprivatelinks) **disk erişimi** ' ne gidin.

    > [!IMPORTANT]
    > Disk erişimi dikey penceresine gitmek için, [belirtilen bağlantıyı](https://aka.ms/disksprivatelinks) kullanmanız gerekir. Şu anda, bağlantı kullanılmadan ortak portalda görünmez.

1. Yeni bir disk erişim kaynağı oluşturmak için **+ Ekle** ' yi seçin.
1. Oluştur dikey penceresinde aboneliğinizi, bir kaynak grubunu seçin, bir ad girin ve bir bölge seçin.
1. **Gözden geçir + oluştur**’u seçin.

    :::image type="content" source="media/disks-enable-private-links-for-import-export-portal/disk-access-create-basics.png" alt-text="Disk erişimi oluşturma dikey penceresinin ekran görüntüsü. İstenen adı girin, bir bölge seçin, bir kaynak grubu seçin ve devam edin":::

Kaynağınız oluşturulduğunda doğrudan buna gidin.

:::image type="content" source="media/disks-enable-private-links-for-import-export-portal/screenshot-resource-button.png" alt-text="Disk erişimi oluşturma dikey penceresinin ekran görüntüsü. İstenen adı girin, bir bölge seçin, bir kaynak grubu seçin ve devam edin":::

## <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma

Artık bir disk erişim kaynağına sahip olduğunuza göre, bu işlemi kullanarak diskinizin dışa/içeri aktarmalarının erişimini işleyebilir, bu özel uç noktalar aracılığıyla yapılır. Buna uygun olarak, özel bir uç nokta oluşturmanız ve disk erişimi için yapılandırmanız gerekir.

1. Disk erişim kaynağınız ' nden **Özel uç nokta bağlantıları**' nı seçin.
1. **+ Özel uç nokta**' ı seçin.

    :::image type="content" source="media/disks-enable-private-links-for-import-export-portal/disk-access-main-private-blade.png" alt-text="Disk erişimi oluşturma dikey penceresinin ekran görüntüsü. İstenen adı girin, bir bölge seçin, bir kaynak grubu seçin ve devam edin":::

1. Kaynak grubu seçme
1. Adı girin ve disk erişim kaynağınızın oluşturulduğu bölgeyi seçin.
1. **İleri ' yi seçin: kaynak >**

    :::image type="content" source="media/disks-enable-private-links-for-import-export-portal/disk-access-private-endpoint-first-blade.png" alt-text="Disk erişimi oluşturma dikey penceresinin ekran görüntüsü. İstenen adı girin, bir bölge seçin, bir kaynak grubu seçin ve devam edin":::

1. **Kaynak** dikey penceresinde, **Dizinimde bir Azure kaynağına bağlan**' ı seçin.
1. **Kaynak türü** için **Microsoft. COMPUTE/diskeriþerlerine** seçin
1. **Kaynak** için daha önce oluşturduğunuz disk erişimi kaynağını seçin
1. **Hedef alt kaynağını** **disk** olarak bırak
1. **İleri ' yi seçin: yapılandırma >**.

    :::image type="content" source="media/disks-enable-private-links-for-import-export-portal/disk-access-private-endpoint-second-blade.png" alt-text="Disk erişimi oluşturma dikey penceresinin ekran görüntüsü. İstenen adı girin, bir bölge seçin, bir kaynak grubu seçin ve devam edin":::

1. Disk dışarı aktarmayı sınırlamak istediğiniz sanal ağı seçin, diğer sanal ağlar diskinizi dışarı layamaz.

    > [!NOTE]
    > Seçilen alt ağ için etkin bir ağ güvenlik grubunuz (NGS) varsa, bu yalnızca bu alt ağdaki özel uç noktalar için devre dışı bırakılır. Bu alt ağdaki diğer kaynakların NSG zorlaması olmaya devam edecektir.

1. Uygun alt ağı seçin
1. **Gözden geçir + oluştur**’u seçin.

    :::image type="content" source="media/disks-enable-private-links-for-import-export-portal/disk-access-private-endpoint-third-blade.png" alt-text="Disk erişimi oluşturma dikey penceresinin ekran görüntüsü. İstenen adı girin, bir bölge seçin, bir kaynak grubu seçin ve devam edin":::

## <a name="enable-private-endpoint-on-your-disk"></a>Diskinizde özel uç noktayı etkinleştirin

1. Yapılandırmak istediğiniz diske gidin
1. **Ağ** seçin
1. **Özel uç nokta ' ı (disk erişimi aracılığıyla)** seçin ve daha önce oluşturduğunuz disk erişimini seçin.
1. **Kaydet**'i seçin.

    :::image type="content" source="media/disks-enable-private-links-for-import-export-portal/disk-access-managed-disk-networking-blade.png" alt-text="Disk erişimi oluşturma dikey penceresinin ekran görüntüsü. İstenen adı girin, bir bölge seçin, bir kaynak grubu seçin ve devam edin":::

Artık yönetilen diskinizi içeri/dışarı aktarırken kullanabileceğiniz özel bağlantıları yapılandırmayı tamamladınız.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel bağlantılar için SSS](./faq-for-disks.md#private-links-for-securely-exporting-and-importing-managed-disks)
- [Yönetilen anlık görüntüleri, PowerShell ile farklı bölgedeki bir depolama hesabına VHD olarak verme/kopyalama](./scripts/virtual-machines-powershell-sample-copy-snapshot-to-storage-account.md)
---
title: Azure Market 'te kendi görüntünüzü kullanarak Azure sanal makine teklifi oluşturma
description: Kendi görüntünüzü kullanarak bir sanal makine teklifini Azure Market 'e yayımlamayı öğrenin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: emuench
ms.author: krsh
ms.date: 10/20/2020
ms.openlocfilehash: 42022d1204c3b524ee2e9ef2770f616fba89dc8c
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92284803"
---
# <a name="how-to-create-a-virtual-machine-using-your-own-image"></a>Kendi görüntünüzü kullanarak bir sanal makine oluşturma

Bu makalede, Kullanıcı tarafından belirtilen bir sanal makine (VM) görüntüsünün nasıl oluşturulacağı ve dağıtılacağı açıklanır.

> [!NOTE]
> Bu yordama başlamadan önce, sanal sabit disk (VHD) gereksinimleri de dahil olmak üzere Azure VM teklifleri için [teknik gereksinimleri](marketplace-virtual-machines.md#technical-requirements) gözden geçirin.

Bunun yerine onaylanan bir temel görüntü kullanmak için, [onaylanan bir tabandan VM görüntüsü oluşturma](azure-vm-create-using-approved-base.md)' daki yönergeleri izleyin.

## <a name="configure-the-vm"></a>VM'yi yapılandırma

Bu bölümde bir Azure VM 'nin nasıl boyutlandırılacağını, güncelleştirileceğini ve genelleştirirsiniz. Bu adımlar, VM 'nizi Azure Marketi 'nde dağıtılacak şekilde hazırlamak için gereklidir.

### <a name="size-the-vhds"></a>VHD 'leri boyutlandırma

[!INCLUDE [Discussion of VHD sizing](includes/vhd-size.md)]

### <a name="install-the-most-current-updates"></a>En güncel güncelleştirmeleri yükler

[!INCLUDE [Discussion of most current updates](includes/most-current-updates.md)]

### <a name="perform-additional-security-checks"></a>Ek güvenlik denetimleri gerçekleştirme

[!INCLUDE [Discussion of addition security checks](includes/additional-security-checks.md)]

### <a name="perform-custom-configuration-and-scheduled-tasks"></a>Özel yapılandırma ve zamanlanmış görevler gerçekleştirme

[!INCLUDE [Discussion of custom configuration and scheduled tasks](includes/custom-config.md)]

## <a name="upload-the-vhd-to-azure"></a>VHD 'yi Azure 'a yükleyin

[Azure 'a yüklemek için bir WINDOWS VHD veya vhdx hazırlama](../virtual-machines/windows/prepare-for-upload-vhd-image.md) veya [bir Linux VHD oluşturma ve karşıya yükleme](../virtual-machines/linux/create-upload-generic.md)konusunda açıklandığı gibi, VM 'yi karşıya yüklenecek şekilde yapılandırın ve hazırlayın.

## <a name="extract-the-vhd-from-image-if-using-image-building-services"></a>Kümeden VHD 'yi ayıklama (görüntü oluşturma Hizmetleri kullanıyorsanız)

[Packer](https://www.packer.io/)gibi bir görüntü oluşturma hizmeti KULLANıYORSANıZ, VHD 'yi görüntüden ayıklamanız gerekebilir. Bunu yapmanın doğrudan bir yolu yoktur. VM 'yi oluşturmanız ve VHD 'yi VM diskinden ayıklamanız gerekir.

### <a name="create-the-vm-on-the-azure-portal"></a>Azure portal VM 'yi oluşturma

[Azure Portal](https://ms.portal.azure.com/)temel VM görüntüsünü oluşturmak için aşağıdaki adımları izleyin.

1. [Azure portalında](https://ms.portal.azure.com/) oturum açın.
2. **Sanal makineler**'i seçin.
3. **+ Ekle** ' yi seçerek **sanal makine oluştur** ekranını açın.
4. Açılan listeden görüntüyü seçin veya tüm kullanılabilir sanal makine görüntülerini aramak veya taramak için **tüm ortak ve özel görüntülere gözatıp '** yi seçin.
5. **Gen 2** VM oluşturmak için **Gelişmiş** sekmesine gidin ve **Gen 2** seçeneğini belirleyin.

    :::image type="content" source="media/create-vm/vm-gen-option.png" alt-text="Gen 1 veya Gen 2 ' yi seçin.":::

6. Dağıtılacak VM 'nin boyutunu seçin.

    :::image type="content" source="media/create-vm/create-virtual-machine-sizes.png" alt-text="Gen 1 veya Gen 2 ' yi seçin.":::

7. VM oluşturmak için gereken diğer ayrıntıları sağlayın.
8. Seçimlerinizi gözden geçirmek için **gözden geçir + oluştur** ' u seçin. **Doğrulama başarılı** Iletisi göründüğünde **Oluştur**' u seçin.

Azure, belirttiğiniz sanal makineyi sağlamaya başlar. Sol menüdeki **sanal makineler** sekmesini seçerek ilerleme durumunu izleyin. Oluşturulduktan sonra sanal makinenin **çalışması**için değişiklikler durumu.

### <a name="connect-to-your-vm"></a>Sanal makinenize bağlanma

[Windows](../virtual-machines/windows/connect-logon.md) veya [Linux](../virtual-machines/linux/ssh-from-windows.md#connect-to-your-vm) VM 'nize bağlanmak için aşağıdaki belgelere bakın.

[!INCLUDE [Discussion of addition security checks](includes/size-connect-generalize.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Önerilen sonraki adım: Azure Marketi yayımlama gereksinimlerini karşıladığından emin olmak için [VM görüntünüzü test edin](azure-vm-image-test.md) . Bu isteğe bağlıdır.
- VM görüntünüzü test etmeyin, [SAS URI 'Si oluşturma](azure-vm-get-sas-uri.md)ile devam edin.
- Yeni Azure tabanlı VHD 'nizi oluştururken zorluk yaşıyorsanız bkz. [Azure Market Için VM SSS](azure-vm-create-faq.md).

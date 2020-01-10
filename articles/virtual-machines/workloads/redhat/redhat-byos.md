---
title: Kendi susma görüntülerini getir Red Hat Enterprise Linux | Microsoft Docs
description: Azure 'da Red Hat Enterprise Linux için kendi abonelik görüntülerini getir hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
editor: ''
ms.assetid: f495f1b4-ae24-46b9-8d26-c617ce3daf3a
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 6/6/2019
ms.author: alsin
ms.openlocfilehash: afd59c44bea238b7dd6f116e491054f8b4a52d48
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75486514"
---
# <a name="red-hat-enterprise-linux-bring-your-own-subscription-images-in-azure"></a>Azure 'da kendi abonelik görüntülerini getir Red Hat Enterprise Linux
Red Hat Enterprise Linux (RHEL) görüntüleri, Kullandıkça Öde (PAYG) veya kendi abonelik (KCG) modeli aracılığıyla Azure 'da kullanılabilir. Bu belge, Azure 'daki KCG görüntülerine genel bir bakış sağlar.

## <a name="important-points-to-consider"></a>Dikkate alınması gereken önemli nokta

- Bu programda sunulan RHEL BYOS görüntüleri, Azure galerisindeki/Market 'teki RHEL PAYG görüntülerine benzer üretim için kullanılabilir RHEL görüntüleridir. Görüntüleri elde etmek için kayıt işlemi önizlemededir.

- Görüntüler, [Azure 'da Red Hat Enterprise Linux görüntülerde](./redhat-images.md) açıklanan geçerli ilkelerinizi izler

- Bu görüntülerden oluşturulan VM 'lere standart destek ilkeleri uygulanır

- RHEL BYOS görüntülerinden sağlanan VM 'Ler RHEL PAYG görüntüleriyle ilişkili RHEL ücretlerini taşımaz

- Görüntülerin yetkileri yoktur, bu nedenle, Red Hat 'ten güncelleştirmeleri doğrudan almak için VM 'Leri kaydetmek ve abone olmak için abonelik Yöneticisi 'ni kullanmanız gerekir

- Linux görüntüleri için KCG ve PAYG faturalandırma modelleri arasında dinamik olarak geçiş yapmak mümkün değildir. Faturalandırma modelini değiştirmek için VM 'nin ilgili görüntüden yeniden dağıtılması gerekir

- Azure disk şifrelemesi (ADE), bu RHEL BYOS görüntülerinde desteklenir. ADE desteği şu anda önizleme aşamasındadır. ADE 'yi yapılandırmadan önce abonelik-Manager ' i kullanarak Red Hat ile kaydolmalısınız. Kaydolduktan sonra, ADE 'yi yapılandırmak için şunu inceleyin: [Linux IaaS VM 'leri Için Azure disk şifrelemesini etkinleştirme](https://docs.microsoft.com/azure/virtual-machines/linux/disk-encryption-overview)

- Görüntüler değişmeyecektir (Standart güncelleştirmeler ve düzeltme eklerinin ötesinde), kayıt işlemi önizlemededir ve akış, işlemi kolaylaştırmak için daha fazla gelişir.

- Son uygulamadan bağımsız olarak, bu görüntülerden veya anlık görüntülerinden zaten sağlanmış olan VM 'lerin tam denetimine sahipsiniz

## <a name="requirements-and-conditions-to-access-the-rhel-byos-images"></a>RHEL BYOS görüntülerine erişmek için gereksinimler ve koşullar

1. [Red Hat bulut erişim programı](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) hükümlerini öğrenin ve [Red Hat 'in bulut erişim kaydı sayfasında](https://access.redhat.com/cloude/manager/image_imports/new)kaydolun.

1. [RHEL BYOS erişim isteği formunu](https://aka.ms/rhel-byos)doldurun. Red Hat hesabınızın numarasını ve ile RHEL BYOS görüntülerine erişmek için kullandığınız Azure aboneliklerinizi de sahip olmanız gerekir.

1. Hem Red Hat hem de Microsoft tarafından İnceleme ve onay sağlandığında, Azure abonelikleriniz görüntü erişimi için etkinleştirilir.

### <a name="expected-time-for-image-access"></a>Görüntü erişimi için beklenen süre

RHEL BYOS formunu ve koşulları kabul etmeyi tamamladıktan sonra Red hat, KCG görüntülerinin üç iş günü içinde uygunluğunu doğrular ve geçerliyse, daha sonra bir iş günü içinde KCG görüntülerine erişim elde edersiniz.

## <a name="use-the-rhel-byos-images-from-the-azure-portal"></a>Azure portalından RHEL BYOS görüntülerini kullanma

1. Aboneliğiniz RHEL BYOS görüntüleri için etkinleştirildikten sonra, **kaynak oluştur ' a** gidip **Tümünü göster**' e giderek [Azure Portal](https://portal.azure.com) bulabilirsiniz.

1. Sayfanın üst kısmında özel tekliflerle ilgili olduğunu görürsünüz.

    ![Market özel teklifleri](./media/rhel-byos-privateoffers.png)

1. Özel tekliflerinizi görmek için mor bağlantıya tıklayabilir veya sayfanın en altına gidin.

1. Kullanıcı arabirimindeki sağlamanın geri kalanı, var olan herhangi bir Red Hat görüntüsüne göre farklı olmayacaktır. RHEL sürümünüzü seçin ve VM 'nizi sağlamak için istemleri izleyin. Bu işlem, son adımda resmin şartlarını kabul etmenize de olanak tanır.

>[!NOTE]
>Şu ana kadar bu adımlar, RHEL BYOS görüntünüzü programlı dağıtım için etkinleştirmeyecektir. aşağıdaki "ek bilgiler" bölümünde açıklandığı gibi ek bir adım gerekecektir.

Bu belgenin geri kalanı, görüntü üzerinde hüküm sağlamak ve kabul etmek için CLı yöntemine odaklanır. UI ve CLı, son sonuç olarak (sağlanan RHEL BYOS VM 'si) oldukça tamamen değiştirilebilir.

## <a name="use-the-rhel-byos-images-from-the-azure-cli"></a>Azure CLı 'deki RHEL BYOS görüntülerini kullanma
Aşağıdaki yönergeler kümesi, Azure CLı kullanarak bir RHEL VM için ilk dağıtım sürecinde size yol gösterecektir. Bu yönergelerde, [Azure CLI 'nin yüklü](https://docs.microsoft.com/cli/azure/install-azure-cli)olduğu varsayılır.

>[!IMPORTANT]
>Aşağıdaki komutlar için yayımcı, teklif, plan ve görüntü başvurularında tüm küçük harfleri kullandığınızdan emin olun

1. İstediğiniz abonelikte olup olmadığınızı kontrol edin:
    ```azurecli
    az account show -o=json
    ```

1. RHEL BYOS VM 'niz için bir kaynak grubu oluşturun:
    ```azurecli
    az group create --name <name> --location <location>
    ```

1. Görüntü koşullarını kabul edin:
    ```azurecli
    az vm image terms accept --publisher redhat --offer rhel-byos --plan <SKU value here> -o=jsonc

    # Example:
    az vm image terms accept --publisher redhat --offer rhel-byos --plan rhel-lvm75 -o=jsonc

    OR

    az vm image terms accept --urn RedHat:rhel-byos:rhel-lvm8:8.0.20190620
    ```
    >[!NOTE]
    >Bu koşulların *her bir Azure aboneliği için, görüntü SKU 'su başına bir kez*kabul edilmesi gerekir.

1. Seçim Aşağıdaki komutla VM dağıtımınızı doğrulayın:
    ```azurecli
    az vm create -n <VM name> -g <resource group name> --image <image urn> --validate

    # Example:
    az vm create -n rhel-byos-vm -g rhel-byos-group --image RedHat:rhel-byos:rhel-lvm75:7.5.20190620
    ```

1. `--validate` bağımsız değişkeni olmadan yukarıdaki gibi aynı komutu çalıştırarak VM 'nizi sağlayın:
    ```azurecli
    az vm create -n <VM name> -g <resource group name> --image <image urn> --validate
    ```

1. Sanal makinenize SSH ekleyin ve ilgili olmayan bir görüntünüz olduğunu doğrulayın. Bunu yapmak IÇIN `sudo yum repolist`çalıştırın. Bu çıktı, VM 'yi Red Hat ile kaydetmek için abonelik Yöneticisi 'ni kullanmanızı ister.

## <a name="additional-information"></a>Ek Bilgi
- Bu teklif için etkinleştirilmemiş bir abonelikte bir VM sağlamaya çalışırsanız, aşağıdaki hatayı alırsınız ve aboneliğinizi etkinleştirmek için Microsoft veya Red Hat 'e başvurmanız gerekir.
    ```
    "Offer with PublisherId: redhat, OfferId: rhel-byos, PlanId: rhel-lvm75 is private and can not be purchased by subscriptionId: GUID"
    ```

- RHEL BYOS görüntüsünden bir anlık görüntü oluşturup görüntüyü [paylaşılan görüntü galerisinde](https://docs.microsoft.com/azure/virtual-machines/linux/shared-image-galleries)yayımladığınızda, anlık görüntünün özgün kaynağıyla eşleşen plan bilgileri sağlamanız gerekir. Örneğin, komut gibi görünebilir (son satırdaki plan parametrelerine göz atın):
    ```azurecli
    az vm create –image \
    "/subscriptions/GUID/resourceGroups/GroupName/providers/Microsoft.Compute/galleries/GalleryName/images/ImageName/versions/1.0.0" \
    -g AnotherGroupName --location EastUS2 -n VMName \
    --plan-publisher redhat --plan-product rhel-byos --plan-name rhel-lvm75
    ```

- RHEL BYOS görüntülerinden sanal makineler sağlamak için Otomasyon kullanıyorsanız, yukarıda gösterildiğine benzer plan parametreleri sağlamanız gerekir. Örneğin, Terrayform kullanıyorsanız, plan bilgilerini bir [plan bloğunda](https://www.terraform.io/docs/providers/azurerm/r/virtual_machine.html#plan)sağlarsınız.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Red Hat güncelleştirme altyapısı](./redhat-rhui.md)hakkında daha fazla bilgi edinin.
* Azure 'daki Red Hat görüntüleri hakkında daha fazla bilgi edinmek için [Belgeler sayfasına](./redhat-images.md)gidin.
* Tüm RHEL sürümleri için Red Hat destek ilkeleriyle ilgili bilgiler [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfasında bulunabilir.
---
title: Avere vFXT ön koşullar - Azure
description: Azure için Avere vFXT için ön koşullar
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: rohogue
ms.openlocfilehash: a183989cc666f00da4be077c719c40d2524fd6e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252551"
---
# <a name="prepare-to-create-the-avere-vfxt"></a>Avere vFXT oluşturmaya hazırlanma

Bu makalede, bir Avere vFXT kümesi oluşturmak için ön koşul görevleri açıklanmaktadır.

## <a name="create-a-new-subscription"></a>Yeni bir abonelik oluşturma

Yeni bir Azure aboneliği oluşturarak başlayın. Gider izleme ve temizlemeyi basitleştirmek ve küme iş akışını sağlarken diğer projelerin kotalardan veya kaynak azaltmadan etkilenmediğinden emin olmak için her Avere vFXT projesi için ayrı bir abonelik kullanın.

Azure portalında yeni bir Azure aboneliği oluşturmak için:

1. [Abonelikler bıçağına](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) gidin
1. Üstteki **+ Ekle** düğmesine tıklayın
1. İstenirse oturum açma
1. Bir teklif seçin ve yeni bir abonelik oluşturmak için adımları yürüyün

## <a name="configure-subscription-owner-permissions"></a>Abonelik sahibi izinlerini yapılandırma

Abonelik için sahip izinlerine sahip bir kullanıcı vFXT kümesini oluşturmalıdır. Küme oluşturma, bir sahibin yazılım hizmet koşullarını kabul etmesini ve ağ ve depolama kaynaklarında değişiklik yapılmasına izin vermesini gerektirir.

Azure kümesi için bir Avere vFXT oluşturmasına izin vermek için bazı geçici geçici çözüm geçici servis ler vardır. Bu senaryolar, kaynakların kısıtlanması ve oluşturucuya ek rol tabanlı erişim denetimi (RBAC) rolleri atamayı içerir. Tüm bu durumlarda, bir abonelik sahibi de [önceden Avere vFXT yazılım koşullarını kabul](#accept-software-terms) etmelidir.

| Senaryo | Kısıtlamalar | Avere vFXT kümesini oluşturmak için gereken erişim rolleri |
|----------|--------|-------|
| Kaynak grup yöneticisi vFXT oluşturur | Kaynak grubu içinde sanal ağ, küme denetleyicisi ve küme düğümleri oluşturulmalıdır. | [Kullanıcı Erişim Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator) ve [Katılımcı](../role-based-access-control/built-in-roles.md#contributor) rolleri, her ikisi de hedef kaynak grubuna yönelik. |
| Varolan, harici sanal ağ kullanma | Küme denetleyicisi ve küme düğümleri vFXT'nin kaynak grubunda oluşturulur, ancak farklı bir kaynak grubunda varolan bir sanal ağ kullanır. | (1) VFXT kaynak grubuna kapsamlı [Kullanıcı Erişim Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator) ve [Katılımcı](../role-based-access-control/built-in-roles.md#contributor) rolleri; ve (2) [Sanal Makine Katılımcısı](../role-based-access-control/built-in-roles.md#virtual-machine-contributor), [Kullanıcı Erişim Yöneticisi](../role-based-access-control/built-in-roles.md#user-access-administrator)ve [Avere Katılımcısı](../role-based-access-control/built-in-roles.md#avere-contributor) rolleri sanal ağın kaynak grubuna kapsamlıdır. |
| Küme oluşturucuları için özel rol | Kaynak yerleştirme kısıtlaması yok. Bu yöntem, sahiplerine önemli ayrıcalıklar sağlar. | Abonelik [sahibi, bu makalede](avere-vfxt-non-owner.md)açıklandığı gibi özel bir RBAC rolü oluşturur. |

## <a name="quota-for-the-vfxt-cluster"></a>vFXT kümesi için kota

Aşağıdaki Azure bileşenleri için yeterli kotanız olup olmadığını kontrol edin. Gerekirse, [kota artışı isteyin.](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request)

> [!NOTE]
> Burada listelenen sanal makineler ve SSD bileşenleri vFXT kümesinin kendisi içindir. İşlem ekiminizde kullanacağınız VM'ler ve SSD'ler için kotaya da ihtiyacınız olduğunu unutmayın.
>
> İş akışını çalıştırmak istediğiniz bölge için kotanın etkin olduğundan emin olun.

|Azure bileşeni|Kota|
|----------|-----------|
|Sanal makineler|3 veya daha fazla E32s_v3 (küme düğümü başına bir) |
|Premium SSD depolama alanı|200 GB işletim sistemi alanına ek olarak düğüm başına 1 TB-4 TB önbellek alanı |
|Depolama hesabı (isteğe bağlı) |v2|
|Veri arka uç depolama (isteğe bağlı) |Yeni bir LRS Blob konteyner |
<!-- this table also appears in the overview - update it there if updating here -->

## <a name="accept-software-terms"></a>Yazılım koşullarını kabul et

> [!TIP]
> Bir abonelik sahibi Avere vFXT kümesini oluşturacaksa bu adımı atlayın.

Küme oluşturma sırasında, Avere vFXT yazılımının hizmet koşullarını kabul etmelisiniz. Abonelik sahibi değilseniz, bir abonelik sahibinin şartları önceden kabul etmesini gerekir.

Bu adımın abonelik başına yalnızca bir kez yapılması gerekir.

Yazılım koşullarını önceden kabul etmek için:

1. Azure portalında bir bulut kabuğu açın <https://shell.azure.com>veya göz atarak. Abonelik kimliğinizle oturum açın.

   ```azurecli
    az login
    az account set --subscription <subscription ID>
   ```

1. Hizmet koşullarını kabul etmek ve Azure yazılım görüntüsü için Avere vFXT için programlı erişimi etkinleştirmek için bu komutu sorun:

   ```azurecli
   az vm image accept-terms --urn microsoft-avere:vfxt:avere-vfxt-controller:latest
   ```

## <a name="create-a-storage-service-endpoint-in-your-virtual-network-if-needed"></a>Sanal aağınızda bir depolama hizmeti bitiş noktası oluşturma (gerekirse)

Hizmet [bitiş noktası,](../virtual-network/virtual-network-service-endpoints-overview.md) Azure Blob trafiğini sanal ağ dışında yönlendirmek yerine yerel tutar. Arka uç veri depolama için Azure Blob kullanan Azure kümesi için herhangi bir Avere vFXT için önerilir.

Küme oluşturulurken yeni bir sanal ağ oluşturursanız, otomatik olarak bir bitiş noktası oluşturulur. Varolan bir sanal ağ sağlıyorsanız, küme oluşturma sırasında yeni bir Blob depolama kapsayıcısı oluşturmak istiyorsanız, bir Microsoft depolama hizmeti bitiş noktası olmalıdır.<!-- if there is no endpoint in that situation, the cluster creation will fail -->

> [!TIP]
>
>* Küme oluşturmanın bir parçası olarak yeni bir sanal ağ oluşturuyorsanız bu adımı atlayın.
>* Küme oluşturma sırasında Blob depolama oluşturmadeğilseniz, bir bitiş noktası isteğe bağlıdır. Bu durumda, Azure Blob'u kullanmaya karar verirseniz, hizmet bitiş noktasını daha sonra oluşturabilirsiniz.

Azure portalından depolama hizmeti bitiş noktasını oluşturun.

1. Portalda, sanal ağlar listenizi açın.
1. Kümeniz için sanal ağı seçin.
1. Sol menüden **Hizmet bitiş noktalarını** tıklatın.
1. En üstte **Ekle'yi** tıklatın.
1. Hizmeti ``Microsoft.Storage``seçin.
1. Kümenin alt kümesini seçin.
1. En altta **Ekle'yi**tıklatın.

   ![Hizmet bitiş noktası oluşturma adımları için ek açıklamalar içeren Azure portalı ekran görüntüsü](media/avere-vfxt-service-endpoint.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu ön koşulları tamamladıktan sonra küme oluşturabilirsiniz. Talimatlar için [vFXT kümesini dağıt'ı](avere-vfxt-deploy.md) okuyun.

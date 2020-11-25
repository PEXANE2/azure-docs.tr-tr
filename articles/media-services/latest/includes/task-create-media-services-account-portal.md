---
author: IngridAtMicrosoft
ms.service: media-services
ms.topic: include
ms.date: 11/04/2020
ms.author: inhenkel
ms.custom: portal
ms.openlocfilehash: 72857564a6b195353e7bf2a27c8369004d52d3f2
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95971367"
---
<!-- Use the portal to create a media services account. -->

## <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

1. [Azure portalda](https://portal.azure.com/) oturum açın.
1. **+ Kaynak**  >  **medyası** oluştur  >  **Media Services**' a tıklayın.
1. **Media Services hesabı oluştur** bölümünde gerekli değerleri girin.

    | Ad | Açıklama |
    | ---|---|
    |**Hesap Adı**|Yeni Media Services hesabının adını girin. Media Services hesabı adı, boşluk olmadan, tümü sayı ve küçük harften oluşmalı ve 3-24 karakter uzunluğunda olmalıdır.|
    |**Abonelik**|Birden fazla aboneliğiniz varsa, erişiminiz olan Azure abonelikleri listesinden birini seçin.|
    |**Kaynak Grubu**|Yeni veya mevcut kaynağı seçin. Kaynak grubu; yaşam döngüsünü, izinleri ve ilkeleri paylaşan kaynakların bir koleksiyonudur. [Burada](../../../azure-resource-manager/management/overview.md#resource-groups) daha fazla bilgi edinin.|
    |**Konum**|Media Services hesabınız için medya ve meta veri kayıtlarını depolamak üzere kullanılacak coğrafi bölgeyi seçin. Bu bölge medyanızı işlemek ve akışını sağlamak için kullanılır. Yalnızca Media Services kullanılabilen bölgeler açılır listede görüntülenir. |
    |**Depolama hesabı**|Media Services hesabınızdan medya içeriğinin blob depolamasını sağlamak için bir depolama hesabı seçin. Media Services hesabınızla aynı coğrafi bölgede bulunan mevcut bir depolama hesabını seçebilir ya da yeni bir depolama hesabı oluşturabilirsiniz. Aynı bölgede yeni bir depolama hesabı oluşturulur. Depolama hesabı adları için kurallar Media Services hesapları ile aynıdır.<br/><br/>Bir **birincil** depolama hesabınız olmalıdır ve Media Services hesabınızla Ilişkili birkaç **İkincil** depolama hesabı olabilir. İkincil depolama hesaplarını eklemek için Azure portal kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Media Services hesapları Ile Azure depolama hesapları](../storage-account-concept.md).<br/><br/>Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Gecikme süreleri ve veri çıkış maliyetleri ile karşı karşıya kalmamak için mutlaka Media Services ile aynı konumda bulunan depolama hesaplarını kullanmanız önerilir.|
    |**Gelişmiş ayarlar**| Sistem **tarafından yönetilen radyo düğmesini** seçerek sistem tarafından yönetilen kimlik kullanarak bir hesap oluşturabilirsiniz.  Bu seçimin yapılması, müşteri tarafından yönetilen anahtarları kullanmanıza veya kendi anahtarınızı (BYOK) ve Media Services güvenilir depolamayı etkinleştirmek üzere kullanmanıza olanak sağlayacak.  Müşteri tarafından yönetilen anahtarlar hakkında daha fazla bilgi için, [Media Services ile kendi anahtarınızı getirin (müşteri tarafından yönetilen anahtarlar)](../concept-use-customer-managed-keys-byok.md)konusuna bakın. Ayrıca, [Yönetilen kimlikler](../concept-managed-identities.md) de etkinleştirilir.

1. Hesap dağıtımını ilerleme durumunu görmek için **Panoya sabitle**’yi seçin.
1. Formun alt kısmındaki **Oluştur**’a tıklayın.

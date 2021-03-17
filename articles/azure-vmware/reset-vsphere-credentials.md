---
title: Azure VMware çözümü için vSphere kimlik bilgilerini sıfırlama
description: Azure VMware çözümünüz özel bulutunuz için vSphere kimlik bilgilerini sıfırlamayı ve HCX bağlayıcısının en son vSphere kimlik bilgilerine sahip olduğundan emin olun.
ms.topic: how-to
ms.date: 03/16/2021
ms.openlocfilehash: e5a15caa98a46b0ae75b68ee7b568dabdbf1956c
ms.sourcegitcommit: 27cd3e515fee7821807c03e64ce8ac2dd2dd82d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103603274"
---
# <a name="reset-vsphere-credentials-for-azure-vmware-solution"></a>Azure VMware çözümü için vSphere kimlik bilgilerini sıfırlama

Bu makalede, Azure VMware Çözüm özel bulutunuzun vSphere kimlik bilgilerini sıfırlama adımlarında İzlenecek adımları inceleyeceğiz. Bu, HCX bağlayıcısının en son vSphere kimlik bilgilerine sahip olduğundan emin olmanızı sağlar.

## <a name="reset-your-vsphere-credentials"></a>VSphere kimlik bilgilerinizi sıfırlayın

 İlk olarak vSphere kimlik bilgilerinizi sıfırlayalım. VCenter CloudAdmin ve NSX-T yönetici kimlik bilgilerinizin süresi dolmaz; Ancak, bu hesaplar için yeni parolalar oluşturmak üzere aşağıdaki adımları izleyebilirsiniz.

> [!NOTE]
> HCX, vCenter Orchestrator, vCloud Director veya VNET gibi bağlı hizmetler için CloudAdmin kimlik bilgilerinizi kullanıyorsanız, parolanızı güncelleştirdikten sonra bağlantılarınız çalışmayı durduracaktır.  Parola döndürmeyi başlatmadan önce bu hizmetler durdurulmalıdır.  Bunun başarısız olması, bu hizmetler eski kimlik bilgilerinizi kullanarak sürekli olarak çağrı yapabilmeleri için vCenter CloudAdmin ve NSX-T yönetici hesaplarınızdaki geçici kilitlerin oluşmasına neden olabilir.  Bağlı hizmetler için ayrı hesaplar ayarlama hakkında daha fazla bilgi için bkz. [erişim ve kimlik kavramları](https://docs.microsoft.com/azure/azure-vmware/concepts-identity).

1. Azure VMware Çözüm portalınızda bir komut satırı açın.

2. VCenter CloudAdmin parolanızı güncelleştirmek için aşağıdaki komutu çalıştırın.  {SubscriptionID}, {ResourceGroup} ve {PrivateCloudName} değerlerini CloudAdmin hesabının ait olduğu özel bulutun gerçek değerleriyle değiştirmeniz gerekir.

```
az resource invoke-action --action rotateVcenterPassword --ids "/subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroup}/providers/Microsoft.AVS/privateClouds/{PrivateCloudName}" --api-version "2020-07-17-preview"
```
          
3. NSX-T yönetici parolanızı güncelleştirmek için aşağıdaki komutu çalıştırın. {SubscriptionID}, {ResourceGroup} ve {PrivateCloudName} değerlerini NSX-T yönetici hesabının ait olduğu özel bulutun gerçek değerleriyle değiştirmeniz gerekir.

```
az resource invoke-action --action rotateNSXTPassword --ids "/subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroup}/providers/Microsoft.AVS/privateClouds/{PrivateCloudName}" --api-version "2020-07-17-preview"
```

## <a name="ensure-the-hcx-connector-has-your-latest-vsphere-credentials"></a>HCX bağlayıcısının en son vSphere kimlik bilgilerine sahip olduğundan emin olun

Kimlik bilgilerinizi sıfırladıktan sonra, HCX bağlayıcısının güncelleştirilmiş kimlik bilgilerine sahip olduğundan emin olmak için aşağıdaki adımları izleyin.

1. Parolanız değiştirildikten sonra, https://{HCX bağlayıcı gerecinin IP 'sini kullanarak şirket içi HCX Bağlayıcısı Web arabirimine gidin: 443. 443 numaralı bağlantı noktasını kullandığınızdan emin olun. Yeni kimlik bilgilerinizi kullanarak oturum açın.

2. VMware HCX panosunda **site eşleştirme**' yi seçin.
    
    :::image type="content" source="media/reset-vsphere-credentials/hcx-site-pairing.png" alt-text="Site eşleştirmesi vurgulanmış şekilde VMware HCX panosunun ekran görüntüsü.":::
 
3. AVS 'ye doğru bağlantıyı (birden fazla varsa) seçin ve **Bağlantıyı Düzenle**' yi seçin.
 
4. Yeni vSphere kimlik bilgilerini sağlayın ve kimlik bilgilerini kaydeden **Düzenle**' yi seçin. Kaydetme işleminin başarılı olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure VMware çözümü için vSphere kimlik bilgilerini sıfırlamayı kapsadığınıza göre şunları öğrenmek isteyebilirsiniz:

- [Azure VMware çözümünde NSX ağ bileşenlerini yapılandırma](configure-nsx-network-components-azure-portal.md).
- [Azure VMware Çözüm VM 'Lerinin yaşam döngüsü yönetimi](lifecycle-management-of-azure-vmware-solution-vms.md).
- [Azure VMware çözümünü kullanarak sanal makinelerin olağanüstü durum kurtarma dağıtımı](disaster-recovery-for-virtual-machines.md).

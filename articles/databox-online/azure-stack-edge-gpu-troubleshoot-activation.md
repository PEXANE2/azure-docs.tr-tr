---
title: GPU ile Azure Stack Edge Pro ile ilgili etkinleştirme hatalarını gidermek için Azure portal kullanın | Microsoft Docs
description: Azure Stack Edge Pro GPU etkinleştirme ve anahtar kasası ile ilgili sorunların nasıl giderileceği açıklanmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: troubleshooting
ms.date: 10/08/2020
ms.author: alkohli
ms.openlocfilehash: 33254c170c309626ecfa9099bc4d86578148f4c1
ms.sourcegitcommit: 541bb46e38ce21829a056da880c1619954678586
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2020
ms.locfileid: "91941405"
---
# <a name="troubleshoot-activation-issues-on-your-azure-stack-edge-pro-gpu-device"></a>Azure Stack Edge Pro GPU cihazındaki etkinleştirme sorunlarını giderme 

<!--[!INCLUDE [applies-to-skus](../../includes/azure-stack-edge-applies-to-all-sku.md)]-->

Bu makalede Azure Stack Edge Pro GPU cihazındaki etkinleştirme sorunlarını giderme işlemleri açıklanmaktadır. 


## <a name="activation-errors"></a>Etkinleştirme hataları

Aşağıdaki tabloda, cihaz etkinleştirmeyle ilgili hatalar ve ilgili önerilen çözüm özetlenmektedir.

| Hata iletisi| Önerilen çözüm |
|------------------------------------------------------|--------------------------------------|
| Etkinleştirme için kullanılan Azure Key Vault, cihaz etkinleştirme anahtarıyla etkinleştirilmeden önce silinirse, bu hatayı alırsınız. <br> ![Anahtar Kasası hatası 1](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-1.png)  | Anahtar Kasası silinmişse, kasa Temizleme koruma süresinden olursa anahtar kasasını kurtarabilirsiniz. [Anahtar kasasını kurtarma](/azure/key-vault/general/soft-delete-powershell#recovering-a-key-vault)bölümündeki adımları izleyin. <br>Temizleme koruma süresi geçtiğinde, Anahtar Kasası kurtarılamaz. Sonraki adımlar için Microsoft Desteği'ne başvurun. |
| Azure Key Vault cihaz etkinleştirildikten sonra silinirse ve ardından şifrelemeyi içeren herhangi bir işlem gerçekleştirmeyi denerseniz, örneğin: **Kullanıcı ekleme**, **paylaşma ekleme**, **işlem yapılandırma**, daha sonra bu hatayı alırsınız. <br> ![Anahtar Kasası hatası 2](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-2.png)    | Anahtar Kasası silinmişse, kasa Temizleme koruma süresinden olursa anahtar kasasını kurtarabilirsiniz. Anahtar kasasını kurtarma bölümündeki adımları izleyin. <br>Temizleme koruma süresi geçtiğinde, Anahtar Kasası kurtarılamaz. Sonraki adımlar için Microsoft Desteği'ne başvurun. |
| Azure Key Vault kanal bütünlüğü anahtarı silinirse ve bu durumda şifreleme içeren herhangi bir işlem gerçekleştirmeyi deneyin, örneğin: **Kullanıcı ekleme**, **paylaşma ekleme**, **işlem yapılandırma** , daha sonra bu hatayı alırsınız. <br> ![Anahtar Kasası hatası 3](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-3.png) | Anahtar kasasındaki kanal bütünlüğü anahtarı silinirse, ancak hala temizleme süresi içindeyse, [Anahtar Kasası anahtarını kaldırma Işlemini geri alma](/powershell/module/az.keyvault/undo-azkeyvaultkeyremoval)bölümündeki adımları izleyin. <br>Temizleme koruma süresi dolmuşsa ve yedeklenen anahtar varsa, yedeklemeden geri yükleme yapabilirsiniz Aksi takdirde anahtarı kurtaramazsınız. Sonraki adımlar için Microsoft Desteği'ne başvurun. |
| Etkinleştirme anahtarı oluşturma işlemi herhangi bir hata nedeniyle başarısız olursa, bu hatayı alırsınız. Bildirimde ek ayrıntılar mevcuttur. <br> ![Anahtar Kasası hatası 4](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-4.png)   | Birkaç dakika bekleyip işlemi yeniden deneyin. Sorun devam ederse Microsoft Desteği başvurun. |
| Kullanıcı salt okuma izinlerine sahipse, kullanıcının etkinleştirme anahtarı oluşturmasına izin verilmez ve bu hata gösterilir. <br> ![Anahtar Kasası hatası 5](./media/azure-stack-edge-gpu-troubleshoot-activation/key-vault-error-5.png) | Bunun nedeni, doğru erişiminizin olmaması veya  *Microsoft. Keykasasının* kayıtlı olmaması olabilir.<li>Azure Stack Edge kaynağınız için kullanılan kaynak grubu düzeyinde sahip veya katkıda bulunan erişiminiz olduğundan emin olun.</li><li>Microsoft. Keykasası kaynak sağlayıcısının kayıtlı olduğundan emin olun. Bir kaynak sağlayıcısını kaydetmek için Azure Stack Edge kaynağı için kullanılan aboneliğe gidin. **Kaynak sağlayıcıları**' na gidin, *Microsoft. keykasasını* arayın ve seçip **kaydolun**.</li> |

## <a name="next-steps"></a>Sonraki adımlar

- [Cihaz sorunlarını giderme](azure-stack-edge-gpu-troubleshoot.md)hakkında daha fazla bilgi edinin.

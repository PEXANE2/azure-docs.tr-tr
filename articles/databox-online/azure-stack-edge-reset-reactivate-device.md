---
title: Azure Stack Edge Pro Cihazınızı sıfırlayın ve yeniden etkinleştirin | Microsoft Docs
description: Verileri silme ve sonra Azure Stack Edge Pro cihazınızı yeniden etkinleştirme hakkında bilgi edinin.
services: databox
author: v-dalc
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 03/01/2020
ms.author: alkohli
ms.openlocfilehash: 4026bac9818b14c33c05d99caff4052adad196c3
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101746903"
---
# <a name="reset-and-reactivate-your-azure-stack-edge-pro-device"></a>Azure Stack Edge Pro Cihazınızı sıfırlayın ve yeniden etkinleştirin

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Bu makalede, cihazla ilgili sorun yaşıyorsanız veya başka bir nedenden dolayı yeni baştan başlamak için Azure Stack Edge Pro cihazını sıfırlama, yeniden yapılandırma ve yeniden etkinleştirme açıklanmaktadır.

Verileri kaldırmak için cihazı sıfırladıktan sonra, cihazı yeni bir kaynak olarak yeniden etkinleştirmeniz gerekir. Bir cihaz sıfırlandığında cihaz yapılandırması kaldırılır, bu nedenle cihazı yerel Web Kullanıcı arabirimi aracılığıyla yeniden yapılandırmanız gerekir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Cihazdaki veri disklerinin verileri silme
> * Yeni bir sipariş oluşturarak, cihazı yeniden yapılandırarak ve etkinleştirerek cihazı yeniden etkinleştirin

## <a name="reset-data-from-the-device"></a>Cihazdan veri sıfırlama

Cihazınızın veri disklerinin verilerini silmek için cihazınızı sıfırlamanız gerekir. 

Sıfırlamadan önce, gerekirse, cihazdaki yerel verilerin bir kopyasını oluşturun. Cihazdan verileri bir Azure depolama kapsayıcısına kopyalayabilirsiniz.

>[!IMPORTANT]
> Cihazınızın sıfırlanması, cihazınızdaki tüm yerel verileri ve iş yüklerini siler ve bu işlem geri alınamaz. Cihazınızı yalnızca cihazla birlikte başlatmak istiyorsanız sıfırlayın.

Cihazınızı yerel Web Kullanıcı arabiriminde veya PowerShell 'de sıfırlayabilirsiniz. PowerShell yönergeleri için bkz. [cihazınızı sıfırlama](./azure-stack-edge-connect-powershell-interface.md#reset-your-device).

[! Ekle] [cihazdan veri sıfırlama](../../includes/azure-stack-edge-device-reset.md)

## <a name="reactivate-device"></a>Cihazı yeniden etkinleştirme

Cihazı sıfırladıktan sonra, cihazı yeni bir kaynak olarak yeniden etkinleştirmeniz gerekir. Yeni bir sipariş yerleştirdikten sonra yeni kaynağı yeniden yapılandırmanız ve ardından yeniden etkinleştirmeniz gerekir.

Mevcut cihazınızı yeniden etkinleştirmek için şu adımları izleyin:

1. [Yeni kaynak oluşturma](azure-stack-edge-gpu-deploy-prep?tabs=azure-portal#create-a-new-resource)bölümündeki adımları izleyerek mevcut cihaz için yeni bir sipariş oluşturun. **Sevkiyat Adresi** sekmesinde, **zaten bir cihazım varsa ' ı** seçin.

   ![Sevkiyat adresinde yeni cihaz yok belirtin](./media/azure-stack-edge-reset-reactivate-device/create-resource-with-no-new-device.png)

1. [Etkinleştirme anahtarını alın](azure-stack-edge-gpu-deploy-prep?tabs=azure-portal#get-the-activation-key).

1. [Cihaza bağlanın](azure-stack-edge-gpu-deploy-connect.md).

1. [Cihazı için ağı yapılandırın](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md).

1. [Cihaz ayarlarını yapılandırın](azure-stack-edge-gpu-deploy-set-up-device-update-time.md).

1. [Sertifikaları yapılandırın](azure-stack-edge-gpu-deploy-configure-certificates.md).

1. [Cihazı etkinleştirin](databox-online/azure-stack-edge-gpu-deploy-activate.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Edge Pro cihazına bağlanmayı](azure-stack-edge-gpu-deploy-connect.md)öğrenin.

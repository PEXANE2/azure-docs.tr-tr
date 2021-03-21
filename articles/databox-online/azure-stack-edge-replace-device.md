---
title: Azure Stack Edge Pro cihazınızı değiştirme | Microsoft Docs
description: Bir değiştirme Azure Stack Edge Pro cihazının nasıl alınacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 02/22/2021
ms.author: alkohli
ms.openlocfilehash: 2196c9463569dc43092b38de58e0103104efed0c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102443480"
---
# <a name="replace-your-azure-stack-edge-pro-device"></a>Azure Stack Edge Pro cihazınızı değiştirme

[!INCLUDE [applies-to-GPU-and-pro-r-and-mini-r-skus](../../includes/azure-stack-edge-applies-to-gpu-pro-r-mini-r-sku.md)]

Bu makalede Azure Stack Edge Pro cihazınızın nasıl değiştirileceği açıklanır. Mevcut cihazda bir donanım arızası olduğunda veya bir yükseltme gerektiğinde bir değiştirme cihazı gerekir. 


Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Donanım sorunu için bir destek bileti açın
> * Azure portal bir değiştirme aygıtı için yeni bir sipariş oluşturun
> * Değiştirme cihazını yüklemeyi etkinleştir
> * Özgün cihazı döndür

## <a name="open-a-support-ticket"></a>Bir destek bileti açın

Mevcut cihazınızda bir donanım arızası varsa, aşağıdaki adımları izleyerek bir destek bileti açın:

1. Cihazı döndürmek istediğinizi belirten Microsoft Desteği bir destek bileti açın. **Azure Stack Edge Pro donanımı** sorun türünü seçin ve **donanım sorunları** alt türünü seçin.  

    ![Destek bileti açma](media/azure-stack-edge-replace-device/open-support-ticket-1.png)  

2. Bir Microsoft Desteği mühendisi, bir alan değiştirme birimi 'nin (FRU) sorunu düzeltemedi ve bu örnek için kullanılabilir olup olmadığını öğrenmek için sizinle iletişim kuracaktır. FRU yoksa veya cihazın donanım yükseltmesine ihtiyacı varsa, destek yeni bir sipariş yerleştirmeniz ve eski cihazınızı döndürmeye kılavuzluk eder.

## <a name="create-a-new-order"></a>Yeni sipariş oluştur

[Yeni kaynak oluşturma](azure-stack-edge-gpu-deploy-prep.md#create-a-new-resource)bölümündeki adımları izleyerek değiştirme cihazınızın etkinleştirilmesi için yeni bir kaynak oluşturun.

> [!NOTE]
> Mevcut bir kaynağa karşı değiştirme cihazının etkinleştirilmesi desteklenmez. Yeni kaynak yeni bir sipariş olarak değerlendirilir. Cihazın size sevk edildikten 14 gün sonra faturalandırılmaya başlayacaksınız.

## <a name="install-and-activate-the-replacement-device"></a>Değiştirme cihazını yükleyip etkinleştirin

Değiştirme cihazını yüklemek ve etkinleştirmek için aşağıdaki adımları izleyin:

1. [Cihazınızı yükler](azure-stack-edge-deploy-install.md).
2. [Cihazınızı](azure-stack-edge-deploy-connect-setup-activate.md) , daha önce oluşturduğunuz yeni kaynağa karşı etkinleştirin.

## <a name="return-your-existing-device"></a>Mevcut cihazınızı döndürün

Özgün cihazı döndürmek için tüm adımları izleyin:

1. [Cihazdaki verileri silin](azure-stack-edge-return-device.md#erase-data-from-the-device).
2. Özgün cihaz için [cihaz dönüşü başlatın](azure-stack-edge-return-device.md#initiate-device-return) .
3. [Bir toplama zamanlayın](azure-stack-edge-return-device.md#schedule-a-pickup).
4. Cihaz Microsoft 'a alındıktan sonra, döndürülen cihazla ilişkili [kaynağı silebilirsiniz](azure-stack-edge-return-device.md#delete-the-resource) .


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Edge Pro cihazının nasıl döneceğinizi](azure-stack-edge-return-device.md)öğrenin.

---
title: Azure Stack Edge cihazınızı değiştirme | Microsoft Docs
description: Değiştirme Azure Stack Edge cihazının nasıl alınacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 07/20/2020
ms.author: alkohli
ms.openlocfilehash: 5d5e8cc3a959c7800019107d2c682e144449b46c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87100118"
---
# <a name="replace-your-azure-stack-edge-device"></a>Azure Stack Edge cihazınızı değiştirme

Bu makalede, değiştirme Azure Stack Edge cihazının nasıl alınacağı açıklanır. Mevcut cihazda bir donanım arızası olduğunda veya bir yükseltme gerektiğinde bir değiştirme cihazı gerekir. 


Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Donanım sorunu için bir destek bileti açın
> * Azure portal ' de değiştirme cihazı için yeni bir kaynak oluşturun
> * Değiştirme cihazını yüklemeyi etkinleştir
> * Özgün cihazı döndür

## <a name="open-a-support-ticket"></a>Bir destek bileti açın

Mevcut cihazınızda bir donanım arızası varsa, bir destek bileti açın. Microsoft Desteği, bu örnek için bir alan değiştirme birimi (FRU) bulunmadığını veya cihazın bir donanım yükseltmesine ihtiyacı olduğunu belirlemektir. Her iki durumda da destek, bir değiştirme cihazını sıraya alır.

1. Cihazı döndürmek istediğinizi belirten Microsoft Desteği bir destek bileti açın. **Azure Stack Edge donanımı**olarak sorun türünü seçin.

    ![Destek bileti açma](media/azure-stack-edge-replace-device/open-support-ticket-1.png)  

2. Bir Microsoft Desteği mühendisi sizinle iletişim kuracaktır. Sevkıyat ayrıntılarını belirtin.
<!--3. If you need a return shipping box, you can request it. Answer **Yes** to the question **Need an empty box to return**.-->


## <a name="create-a-resource-for-replacement-device"></a>Değiştirme cihazı için bir kaynak oluşturun

Kaynak oluşturmak için aşağıdaki adımları izleyin.

1. Değiştirme cihazı için kaynak oluşturmak üzere [Yeni kaynak oluşturma](azure-stack-edge-deploy-prep.md#create-a-new-resource) bölümündeki adımları izleyin. 

2. **Azure Stack Edge cihazım olmasına**karşı onay kutusunu seçtiğinizden emin olun. 

    ![Değiştirme cihazının kaynağı](media/azure-stack-edge-replace-device/replace-resource-1.png)  

## <a name="install-and-activate-the-replacement-device"></a>Değiştirme cihazını yükleyip etkinleştirin

Değiştirme cihazını yüklemek ve etkinleştirmek için aşağıdaki adımları izleyin:

1. [Cihazınızı yükler](azure-stack-edge-deploy-install.md).

2. [Cihazınızı](azure-stack-edge-deploy-connect-setup-activate.md) , daha önce oluşturduğunuz yeni kaynağa karşı etkinleştirin.

## <a name="return-your-existing-device"></a>Mevcut cihazınızı döndürün

Özgün cihazı döndürmek için tüm adımları izleyin:

1. [Cihazdaki verileri silin](azure-stack-edge-return-device.md#erase-data-from-the-device).
2. Özgün cihaz için [cihaz dönüşü başlatın](azure-stack-edge-return-device.md#initiate-device-return) .
3. [Bir toplama zamanlayın](azure-stack-edge-return-device.md#schedule-a-pickup).
4. Döndürülen cihazla ilişkili [kaynağı silin](azure-stack-edge-return-device.md#delete-the-resource) .


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Edge cihazını döndürmeyi](azure-stack-edge-return-device.md)öğrenin.

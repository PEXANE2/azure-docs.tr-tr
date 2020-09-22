---
title: Azure Stack Edge Pro cihazınızı değiştirme | Microsoft Docs
description: Bir değiştirme Azure Stack Edge Pro cihazının nasıl alınacağını açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 07/20/2020
ms.author: alkohli
ms.openlocfilehash: ec16a2b42b818e96399b8fdbad4a0951f84ef825
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90893904"
---
# <a name="replace-your-azure-stack-edge-pro-device"></a>Azure Stack Edge Pro cihazınızı değiştirme

Bu makalede, Azure Stack Edge Pro cihazının yerini alma işleminin nasıl yapılacağı açıklanır. Mevcut cihazda bir donanım arızası olduğunda veya bir yükseltme gerektiğinde bir değiştirme cihazı gerekir. 


Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
>
> * Donanım sorunu için bir destek bileti açın
> * Azure portal ' de değiştirme cihazı için yeni bir kaynak oluşturun
> * Değiştirme cihazını yüklemeyi etkinleştir
> * Özgün cihazı döndür

## <a name="open-a-support-ticket"></a>Bir destek bileti açın

Mevcut cihazınızda bir donanım arızası varsa, bir destek bileti açın. Microsoft Desteği Alan Değiştirme Biriminin (FRU) bu örnekte kullanılamadığını veya cihazın donanım yükseltmesine ihtiyacı olduğunu belirleyecektir. Her iki durumda da destek, bir değiştirme cihazını sıraya alır.

1. Cihazı döndürmek istediğinizi belirten Microsoft Desteği bir destek bileti açın. **Azure Stack Edge Pro donanımı**olarak sorun türünü seçin.

    ![Destek bileti açma](media/azure-stack-edge-replace-device/open-support-ticket-1.png)  

2. Bir Microsoft Desteği mühendisi sizinle iletişim kuracaktır. Sevkıyat ayrıntılarını belirtin.
<!--3. If you need a return shipping box, you can request it. Answer **Yes** to the question **Need an empty box to return**.-->


## <a name="create-a-resource-for-replacement-device"></a>Değiştirme cihazı için bir kaynak oluşturun

Kaynak oluşturmak için aşağıdaki adımları izleyin.

1. Değiştirme cihazı için kaynak oluşturmak üzere [Yeni kaynak oluşturma](azure-stack-edge-deploy-prep.md#create-a-new-resource) bölümündeki adımları izleyin. 

2. **Azure Stack Edge Pro cihazım olmasına**karşı onay kutusunu seçtiğinizden emin olun. 

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

- [Azure Stack Edge Pro cihazının nasıl döneceğinizi](azure-stack-edge-return-device.md)öğrenin.

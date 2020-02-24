---
title: Azure Kinect DK’yi sıfırlama
description: Azure Kinect DK cihazını fabrika görüntüsüne sıfırlama işlemi açıklanır
author: Teresa-Motiv
ms.author: v-tea
ms.reviewer: jarrettr
ms.prod: kinect-dk
ms.date: 02/11/2020
ms.topic: how-to
keywords: kinect, sıfırlama
ms.custom:
- CI 113492
- CSSTroubleshooting
audience: ITPro
manager: dcscontentpm
ms.localizationpriority: high
ms.openlocfilehash: f851579712a9549c88f1ca652aea4f31457da132
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77201907"
---
# <a name="reset-azure-kinect-dk"></a>Azure Kinect DK’yi sıfırlama

Azure Kinect DK cihazınızı fabrika görüntüsüne sıfırlamanız gereken durumlarla karşılaşabilirsiniz (örneğin, üretici yazılımı güncelleştirmesi doğru yüklenmemiş olabilir).

1. Azure Kinect DK cihazınızın gücünü kapatın. Bunu yapmak için USB kablosunu ve güç kablosunu çıkarın.
  ![Sıfırlama düğmesini kapatan vidanın konumunun gösterildiği diyagram.](media/reset-azure-kinect-dk-diagram.png)
1. Sıfırlama düğmesini bulmak için tripod montaj kilidinde yer alan vidayı çıkarın.
1. Güç kablosunu yeniden takın.
1. Düzleştirilmiş bir atacın ucunu tripod montaj kilidindeki boş vida deliğine sokun.
1. Ataçla hafifçe bastırarak sıfırlama düğmesini basılı tutun.
1. Sıfırlama düğmesini basılı tutarken USB kablosunu yeniden takın.
1. Yaklaşık 3 saniye sonra güç göstergesi ışığı kehribar rengine dönüşür. Işık değiştiğinde sıfırlama düğmesini bırakın.  
   
   Sıfırlama düğmesini bıraktıktan sonra, cihaz sıfırlanırken güç göstergesi ışığı beyaz ve kehribar rengi göz kırpar. 
1. Güç göstergesinin düz beyaza dönüşmesini bekleyin.
1. Tripod montaj kilidinde, sıfırlama düğmesinin üzerine vidayı takın.
1. Azure Kinect Görüntüleyicisi’ni kullanarak üretici yazılımının sıfırlandığını doğrulayın. Bunu yapmak için [Azure Kinect Görüntüleyicisi](azure-kinect-viewer.md)’ni başlatın ve Azure Kinect DK cihazınızda yüklü üretici yazılımı sürümünü görmek için **Cihaz üretici yazılımı sürüm bilgisi**’ni seçin.

Cihazınızda her zaman en son üretici yazılımının yüklü olmasına dikkat edin. En son üretici yazılımı sürümünü almak için Azure Kinect Üretici Yazılımı Aracı’nı kullanın. Üretici yazılımınızın durumunu denetleme hakkında daha fazla bilgi için bkz. [Cihaz üretici yazılımı sürümünü denetleme](azure-kinect-firmware-tool.md#check-device-firmware-version).

## <a name="related-topics"></a>İlgili konular

- [Azure Kinect DK hakkında](about-azure-kinect-dk.md)
- [Azure Kinect DK’yi kurma](set-up-azure-kinect-dk.md)
- [Azure Kinect DK donanım belirtimleri: Çalıştırma ortamı](hardware-specification.md#operating-environment)
- [Azure Kinect Üretici Yazılımı Aracı](azure-kinect-firmware-tool.md)
- [Azure Kinect Görüntüleyicisi](azure-kinect-viewer.md)
- [Birden fazla Azure Kinect DK cihazı arasında eşitleme](multi-camera-sync.md)

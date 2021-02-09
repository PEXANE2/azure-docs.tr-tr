---
author: v-dalc
ms.service: databox
ms.author: alkohli
ms.topic: include
ms.date: 02/05/2021
ms.openlocfilehash: b06b91e972fd07543cf02105360cb0400ef6b0f1
ms.sourcegitcommit: d1b0cf715a34dd9d89d3b72bb71815d5202d5b3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99831556"
---
İşlem ile ilgili hataları gidermek için IoT Edge Aracısı çalışma zamanı yanıtlarını kullanın. Olası yanıtların bir listesi aşağıda verilmiştir:

* 200-TAMAM
* 400-dağıtım yapılandırması hatalı biçimlendirilmiş veya geçersiz.
* 417-cihazda bir dağıtım yapılandırma kümesi yok.
* 412-dağıtım yapılandırmasındaki şema sürümü geçersiz.
* 406-IoT Edge cihaz çevrimdışı veya durum raporları göndermiyor.
* 500-IoT Edge çalışma zamanında bir hata oluştu.

Daha fazla bilgi için bkz. [IoT Edge Aracısı](/azure/iot-edge/iot-edge-runtime?view=iotedge-2018-06&preserve-view=true#iot-edge-agent).

Aşağıdaki hata, Azure Stack Edge Pro 'unuzdaki IoT Edge hizmetiyle ilgilidir<!--/ Data Box Gateway--> cevaplara hızla ulaşın.

### <a name="compute-modules-have-unknown-status-and-cant-be-used"></a>İşlem modüllerinin durumu bilinmiyor ve kullanılamaz

#### <a name="error-description"></a>Hata açıklaması

Cihazdaki tüm modüller bilinmeyen bir durum gösterir ve kullanılamaz. Bilinmeyen durum yeniden başlatma yoluyla devam ettirir.<!--Original Support ticket relates to trying to deploy a container app on a Hub. Based on the work item, I assume the error description should not be that specific, and that the error applies to Azure Stack Edge Devices, which is the focus of this troubleshooting.-->

#### <a name="suggested-solution"></a>Önerilen çözüm

IoT Edge hizmetini silip modülleri yeniden dağıtın. Daha fazla bilgi için bkz. [IoT Edge hizmeti 'Ni kaldırma](../articles/databox-online/azure-stack-edge-j-series-manage-compute.md#remove-iot-edge-service).

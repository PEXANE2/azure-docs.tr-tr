---
author: v-dalc
ms.service: databox
ms.author: alkohli
ms.topic: include
ms.date: 02/05/2021
ms.openlocfilehash: ad981264a99bd48e27f745a789ebe857b7f17d80
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750366"
---
İşlem ile ilgili hataları gidermek için IoT Edge Aracısı çalışma zamanı yanıtlarını kullanın. Olası yanıtların bir listesi aşağıda verilmiştir:

* 200-TAMAM
* 400-dağıtım yapılandırması hatalı biçimlendirilmiş veya geçersiz.
* 417-cihazda bir dağıtım yapılandırma kümesi yok.
* 412-dağıtım yapılandırmasındaki şema sürümü geçersiz.
* 406-IoT Edge cihaz çevrimdışı veya durum raporları göndermiyor.
* 500-IoT Edge çalışma zamanında bir hata oluştu.

Daha fazla bilgi için bkz. [IoT Edge Aracısı](../articles/iot-edge/iot-edge-runtime.md?preserve-view=true&view=iotedge-2018-06#iot-edge-agent).

Aşağıdaki hata, Azure Stack Edge Pro 'unuzdaki IoT Edge hizmetiyle ilgilidir<!--/ Data Box Gateway--> cevaplara hızla ulaşın.

### <a name="compute-modules-have-unknown-status-and-cant-be-used"></a>İşlem modüllerinin durumu bilinmiyor ve kullanılamaz

#### <a name="error-description"></a>Hata açıklaması

Cihazdaki tüm modüller bilinmeyen bir durum gösterir ve kullanılamaz. Bilinmeyen durum yeniden başlatma yoluyla devam ettirir.<!--Original Support ticket relates to trying to deploy a container app on a Hub. Based on the work item, I assume the error description should not be that specific, and that the error applies to Azure Stack Edge Devices, which is the focus of this troubleshooting.-->

#### <a name="suggested-solution"></a>Önerilen çözüm

IoT Edge hizmetini silip modülleri yeniden dağıtın. Daha fazla bilgi için bkz. [IoT Edge hizmeti 'Ni kaldırma](../articles/databox-online/azure-stack-edge-j-series-manage-compute.md#remove-iot-edge-service).
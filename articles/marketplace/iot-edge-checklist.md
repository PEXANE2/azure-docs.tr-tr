---
title: Azure Marketi 'nde IoT Edge modül teklifleri için ön sertifika denetim listesi
description: Azure Marketi 'nde IoT Edge modülü tekliflerini yayımlamaya yönelik belirli sertifika gereksinimleri hakkında bilgi edinin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 03/01/2021
ms.openlocfilehash: c1d4c9be1f76e62be3c17d4dec22479db003b77a
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102608266"
---
# <a name="pre-certification-checklist-for-iot-edge-modules"></a>IoT Edge modülleri için ön sertifika denetim listesi

> [!NOTE]
> Sertifikalara göndermeden önce bu denetim listesini gözden geçirip, modül işlevselliğini doğrulayacağız. Bu, değişiklik ve yeniden gönderme ihtiyacını azaltarak sertifika işleminizi hızlandıracaktır.

## <a name="validation-of-image"></a>Görüntünün doğrulanması

Sınır modülü görüntüsü gönderim için hazırsa, görüntünün Microsoft 'un beklediği şekilde çalıştığından emin olmak için bu adımları gerçekleştirin.

### <a name="steps-to-perform-in-the-azure-portal"></a>Azure portal gerçekleştirme adımları

1. [Azure portalını](https://partner.microsoft.com/) açın.
1. Bir kaynak grubu oluşturun.
1. Bir IoT Hub oluşturma.
1. IoT Edge bir cihaz oluşturun.
1. Bağlantı dizesini kopyalayın ve Not defteri 'ne kaydedin.
1. **Uç cihazındaki modülleri ayarla '** yı seçin.
1. Görüntünün en son sürümünün bulunduğu ACR ayrıntılarını ekleyin.
1. **IoT Edge modülü Ekle** ' yi seçin ve şunları sağlayın:
    - Modül ayarında görüntü URI 'SI
    - Çevresel değişken (Iş Ortağı Merkezi 'ne eklenen ile aynı)
    - Kapsayıcı oluşturma seçenekleri (Iş Ortağı Merkezi 'ne eklenen ile aynı)
    - Module ikizi ayarı (Iş Ortağı Merkezi 'nde eklendikle aynı)
1. Rotalar ekleyin (Iş Ortağı Merkezi 'ne eklenen ile aynı).
1. **Gözden geçir + Oluştur**’u seçin.

Edge modülleri, Azure 'da oluşturulan Edge cihazında dağıtılır.

### <a name="steps-to-perform-on-the-device"></a>Cihazda gerçekleştirilecek adımlar

#### <a name="device-details"></a>Cihaz ayrıntıları

Sertifika ekibi, farklı mimarilerde yansımaları doğrulamak için aşağıdaki donanımları kullanır:

- X64 görüntüleri için yapılandırma boyutuna sahip bir Azure VM, Ubuntu Server 18.04/Ubuntu Server 16,04 çalıştıran standart D2s v3 olarak.
- Azure Resource Manager 32 görüntüleri için, Raspberry PI 3 modeli B.
- Azure Resource Manager 64 görüntüleri için, NVıDıA Jetson nano 4Gb.

#### <a name="steps"></a>Adımlar

1. Oluşturulan cihazların/VM 'nin Putty aracılığıyla erişilebilir olduğundan emin olun.
1. [IoT Edge çalışma zamanını](https://docs.microsoft.com/azure/iot-edge/how-to-install-iot-edge) cihaza indirin.
1. 5. adımda kopyaladığınız bağlantı dizesini config. YAML dosyasına güncelleştirin.
1. Edge modülünü ile yeniden başlatın `sudo systemctl restart iotedge` .
1. Modülün cihazda dağıtılıp dağıtılmadığını kontrol edin `sudo iotedge list` ; çalışır durumda olmalıdır.
1. İle dağıtılan modülün günlüklerinin hata olmadığından emin olun `sudo iotedge logs “Module Name“ -f` . Bilinen hatalar varsa, teklifi göndermeden önce Iş Ortağı Merkezi **notlarındaki bunu gözden geçire açıklama olarak** inceleyin.

## <a name="metadata-validation"></a>Meta veri doğrulama

Aşağıdakileri doğrulayın:

- En son etiket hem Iş ortağı merkezinde hem de Azure Container Registry listelenmiştir.
- Teklif açıklamasına en düşük donanım gereksinimi eklenmiştir.
- Azure Container Registry Kullanıcı adı ve parolası, Iş Ortağı Merkezi 'ne güncelleştirilir ve eklenir.
- *Uygulanabiliyorsa,* Istenen **ikizi özelliğinin** doğruluğu.
- *Varsa,* istenen **çevresel değişkenlerin** doğruluğu.
- Varsa *,* Istenen **oluşturma seçeneklerinin** doğruluğu.
- Lider yönetimi bağlantı dizesi var.
- Gizlilik ilkesi var
- Kullanım koşulları var
- [Azure IoT cihaz kataloğu](https://devicecatalog.azure.com/devices?certificationBadgeTypes=IoTEdgeCompatible) 'ndan desteklenen IoT Edge cihaz bağlantısı ekleme 

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market 'ten modüller dağıtma](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-portal#deploy-from-azure-marketplace)
- [Edge modülünü Iş Ortağı Merkezi 'nde yayımlama](https://docs.microsoft.com/azure/marketplace/partner-center-portal/azure-iot-edge-module-creation)
- [IoT Edge modülünü dağıtma](https://docs.microsoft.com/azure/iot-edge/quickstart-linux)  

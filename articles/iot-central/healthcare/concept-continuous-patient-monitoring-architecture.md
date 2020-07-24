---
title: Azure IoT Central sürekli hasta izleme mimarisi | Microsoft Docs
description: Sürekli hasta izleme çözüm mimarisi hakkında bilgi edinin.
author: philmea
ms.author: philmea
ms.date: 7/23/2020
ms.topic: overview
ms.service: iot-central
services: iot-central
manager: eliotgra
ms.openlocfilehash: 0032f341330ad394241806a4fe61add530253f09
ms.sourcegitcommit: 0820c743038459a218c40ecfb6f60d12cbf538b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87116862"
---
# <a name="continuous-patient-monitoring-architecture"></a>Sürekli hasta izleme mimarisi



Sürekli hasta izleme çözümleri, sunulan uygulama şablonu kullanılarak oluşturulabilir ve aşağıdaki şekilde kılavuz olarak özetlenen mimari kullanılarak oluşturulabilir.

>[!div class="mx-imgBorder"] 
>![CPM mimarisi](media/cpm-architecture.png)

1. Bluetooth düşük enerji (BLE) kullanarak iletişim kuran tıbbi cihazlar
1. Mobil telefon ağ geçidi, verileri alan ve IoT Central gönderme
1. Azure API 'sine FHıR için sürekli veri aktarma&reg;
1. Birlikte çalışabilen verileri temel alan makine öğrenimi
1. FHıR verileri üzerinde oluşturulmuş ekip panosu

## <a name="details"></a>Ayrıntılar
Bu bölümde, mimari diyagramın her bir bölümü daha ayrıntılı olarak özetlenmektedir.

### <a name="ble-medical-devices"></a>BLEME cihazları
Sağlık IoT alanında kullanılan birçok tıp wearables, Bluetooth düşük enerji cihazlarıdır. Doğrudan buluta konuşamazlar ve bir ağ geçidine geçiş yapması gerekecektir. Bu mimaride, bu ağ geçidi olarak bir mobil telefon uygulaması kullanılması önerilir. 

### <a name="mobile-phone-gateway"></a>Cep telefonu ağ geçidi
Cep telefonu uygulamasının birincil işlevi, tıbbi cihazlardan verileri almak ve Azure IoT Central ile iletişim kurmak için kullanılır. Ayrıca, uygulama, bir cihaz kurulumu ve sağlama akışından hastalara rehberlik etmenize yardımcı olabilir ve kişisel sistem durumu verilerinin bir görünümünü görebilirler. Aynı iletişim akışına ulaşmak için bir hastanoda içindeyse, diğer çözümler bir tablet ağ geçidini veya statik ağ geçidini kullanabilir. Android ve iOS için, uygulama geliştirme çabalarınıza hızlı bir başlangıç noktası olarak kullanabileceğiniz açık kaynaklı örnek bir mobil uygulama oluşturduk. IoT Central sürekli hasta Izleme mobil uygulama örneği hakkında daha fazla bilgi için bkz. [Azure örnekleri](https://docs.microsoft.com/samples/iot-for-all/iotc-cpm-sample/iotc-cpm-sample/).

### <a name="export-to-azure-api-for-fhirreg"></a>FHıR için Azure API 'ye dışarı aktarma&reg;
Azure IoT Central HIPAA uyumludur ve HITRUST &reg; sertifikalıdır, ancak sistem durumu ile ilgili verileri fhır Için Azure API 'sine göndermek isteyebilirsiniz. [FHıR Için Azure API](../../healthcare-apis/overview.md) 'si, sistem durumu verilerinize yönelik yeni bir katılım sistemi oluşturmanıza olanak sağlayan, klinik sağlık verilerine yönelik tam olarak yönetilen, standartlara dayalı ve uyumlu bir API 'dir. Bulut ortamında yönetilen bir hizmet olarak platform (PaaS) tarafından desteklenen FHıR API 'Leri aracılığıyla verilerin hızlı bir şekilde değiş tokuşuna izin vermez. IoT Central sürekli veri dışa aktarma işlevini kullanarak fhır için [Azure IoT Bağlayıcısı](https://docs.microsoft.com/azure/healthcare-apis/iot-fhir-portal-quickstart)aracılığıyla Fhır IÇIN Azure API 'sine veri gönderebilirsiniz.

### <a name="machine-learning"></a>Makine öğrenimi
Verilerinizi toplayarak ve FHıR biçimine çevirerek, öngörüleri zenginleştirmek ve bakım ekibiniz için daha akıllı karar verme olanağı sağlayan makine öğrenimi modelleri oluşturabilirsiniz. Makine öğrenimi modellerini derlemek, eğitebilmek ve dağıtmak için kullanılabilecek farklı türlerde hizmetler vardır. Azure 'un makine öğrenimi tekliflerini kullanma hakkında daha fazla bilgi için [makine öğrenimi belgelerimizde](../../machine-learning/index.yml)bulabilirsiniz.

### <a name="provider-dashboard"></a>Sağlayıcı panosu
FHıR için Azure API 'sinde bulunan veriler bir hasta içgörüler panosu oluşturmak için veya ekiplerin hasta durumunu görselleştirmesine yardımcı olmak için doğrudan bir EMR ile tümleştirilebilecek şekilde kullanılabilir. Ekipler bu panoyu, yardım ve daha erken uyarı işaretlerini belirlemek için bu panoyu kullanabilir. Power BI gerçek zamanlı bir sağlayıcı panosu oluşturmayı öğrenmek için [nasıl yapılır kılavuzumuzu](howto-health-data-triage.md)izleyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Sürekli hasta izleme uygulaması şablonunu dağıtmayı öğrenin](tutorial-continuous-patient-monitoring.md)
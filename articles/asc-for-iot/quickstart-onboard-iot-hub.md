---
title: IoT Hub 'da IoT hizmeti için Azure Güvenlik Merkezi 'Ni etkinleştirin | Microsoft Docs
description: IoT Hub IoT hizmeti için Azure Güvenlik Merkezi 'ni nasıl etkinleştirebileceğinizi öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 670e6d2b-e168-4b14-a9bf-51a33c2a9aad
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/16/2019
ms.author: mlottner
ms.openlocfilehash: a794ccea13323f38b20906458e216f85652bfc3e
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68596955"
---
# <a name="quickstart-onboard-azure-security-center-for-iot-service-in-iot-hub"></a>Hızlı Başlangıç: IoT Hub 'de IoT hizmeti için Azure Güvenlik Merkezi 'Ni ekleme

Bu makalede, mevcut IoT Hub IoT hizmeti için Azure Güvenlik Merkezi 'nin nasıl etkinleştirileceği hakkında bir açıklama sunulmaktadır. Şu anda bir IoT Hub yoksa, kullanmaya başlamak için [Azure Portal kullanarak IoT Hub oluşturma](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal) makalesine bakın. 

> [!NOTE]
> IoT için Azure Güvenlik Merkezi şu anda yalnızca Standart katman IoT Hub 'Larını desteklemektedir.
> IoT için Azure Güvenlik Merkezi, tek bir hub çözümüdür. Birden çok hub gerekliyse, IoT çözümleri için birden çok Azure Güvenlik Merkezi gereklidir. 

## <a name="prerequisites-for-enabling-the-service"></a>Hizmeti etkinleştirme önkoşulları

- Log Analytics çalışma alanı
  - İki tür bilgi, IoT için Azure Güvenlik Merkezi tarafından Log Analytics çalışma alanınızda varsayılan olarak saklanır; **güvenlik uyarıları** ve **önerileri**. 
  - Ek bilgi türü, **Ham olaylar**için depolama alanını eklemeyi seçebilirsiniz. **Ham olayların** Log Analytics daha fazla depolama maliyeti taşıdığına göz önünde unutmayın. 
- IoT Hub (Standart katman)
- Tüm [hizmet önkoşullarını](service-prerequisites.md) karşılayın 

|Desteklenen Azure hizmet bölgeleri | ||
|---|---|---|
| Orta ABD |East US |Doğu ABD 2 |
| Batı Orta ABD |Batı ABD |Batı ABD 2 |
| Güney Orta ABD|Orta Kuzey ABD | Orta Kanada|
| Doğu Kanada| Kuzey Avrupa|Güney Brezilya|
| Fransa Orta| Birleşik Krallık Batı|Birleşik Krallık Güney|
|Batı Avrupa|Kuzey Avrupa| Japonya Batı|
|Japonya Doğu | Avustralya Güneydoğu|Avustralya Doğu|
|Doğu Asya| Güneydoğu Asya| Kore Orta|
|Kore Güney| Orta Hindistan| Güney Hindistan|
|

## <a name="enable-azure-security-center-for-iot-on-your-iot-hub"></a>IoT Hub IoT için Azure Güvenlik Merkezi 'ni etkinleştirin 

IoT Hub güvenliği etkinleştirmek için aşağıdakileri yapın: 

1. **IoT Hub** Azure Portal açın. 
1. **Güvenlik** menüsünde, **IoT çözümünüzün güvenliğini sağlama** ' ya tıklayın.
1. Seçileni Varsayılan olarak **Etkinleştir** ' i bırakın. 
1. Log Analytics çalışma alanınızı seçin.
1. Log Analytics çalışma alanınızın ayrıntılarını sağlayın. 
   - **İkizi koleksiyonunu** etkin bırakarak **ikizi toplamayı** etkinleştirmek için **seçin.**
   - **Ham olayları** , Log Analytics depolanan **Ham cihaz güvenlik olaylarını** seçerek depolama varsayılan bilgi türlerine ek olarak depolamayı tercih edin. **Ham olay** geçişi **Açık**bırakın. 
    
1. **Kaydet**’e tıklayın. 

Tebrikler! IoT Hub IoT için Azure Güvenlik Merkezi 'Ni etkinleştirmeyi tamamladınız. 

## <a name="next-steps"></a>Sonraki adımlar

Çözümünüzü yapılandırmak için sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Çözümünüzü yapılandırın](quickstart-configure-your-solution.md)



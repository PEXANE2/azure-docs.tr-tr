---
title: Azure Percept AI modelleri
description: Prototipleme ve dağıtım için kullanılabilen AI modelleri hakkında daha fazla bilgi edinin
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: conceptual
ms.date: 02/16/2021
ms.custom: template-concept
ms.openlocfilehash: e31a696dfb443c20566f13f5a228eefac7af5ecc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104595932"
---
# <a name="azure-percept-ai-models"></a>Azure Percept AI modelleri

Azure Percept, Azure [Percept Studio](https://go.microsoft.com/fwlink/?linkid=2135819)'Dan doğrudan Azure Percept dk 'ye AI modelleri geliştirmenize ve dağıtmanıza olanak sağlar. Model dağıtımı [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) ve [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/#iotedge-overview)kullanır.

## <a name="sample-ai-models"></a>Örnek AI modelleri

Azure Percept Studio, aşağıdaki uygulamalar için örnek modeller içerir:

- insanların algılanması
- araç algılama
- Genel nesne algılama
- ürün-raf üzerinde algılama

Önceden eğitilen modeller sayesinde, kodlama veya eğitim veri koleksiyonu gerekmez. İstediğiniz modeli portalda Azure Percept DK 'ye dağıtmanız yeterlidir ve devkit 'in video akışını açarak model ınkıt 'in işlem içinde olduğunu görürsünüz. Model ınmetriyle sınırlama telemetrisine [Azure IoT gezgin](https://github.com/Azure/azure-iot-explorer/releases) aracı üzerinden de erişilebilir.

## <a name="reference-solutions"></a>Başvuru çözümleri

Bir [kişi sayma başvuru çözümü](https://github.com/microsoft/Azure-Percept-Reference-Solutions/tree/main/people-detection-app) de mevcuttur. Bu başvuru çözümü, Kullanıcı tanımlı bölge giriş/çıkış olayları ile, Edge tabanlı kişiler sağlayan açık kaynaklı bir AI uygulamasıdır. Şirket içi sınır cihazından alınan video ve AI çıkışı, Azure Web sitesi olarak çalışan kullanıcı arabirimiyle [Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/). AI ınsıya özellikleri, insanlar algılaması için açık kaynaklı bir AI modeliyle sağlanır.

:::image type="content" source="./media/overview-ai-models/people-detector.gif" alt-text="Uzamsal analiz önceden oluşturulmuş çözüm GIF 'i.":::

## <a name="custom-no-code-solutions"></a>Özel kod içermeyen çözümler

Azure Percept Studio sayesinde, hiçbir kodlama [gerekmeden özel görüntü](./tutorial-nocode-vision.md) ve konuşma çözümleri geliştirebilirsiniz.

Özel görme çözümleri için, hem nesne algılama hem de sınıflandırma AI modelleri kullanılabilir. İsterseniz eğitim görüntülerinizi karşıya yükleyebilir ve etiketleyerek Azure Percept DK 'nin Azure Percept Vision SoM ile doğrudan alınabilir. Model eğitimi ve değerlendirmesi, Azure bilişsel [Hizmetler](https://azure.microsoft.com/services/cognitive-services/#overview)'in bir parçası olan [özel görüntü işleme](https://www.customvision.ai/)kolayca gerçekleştirilir.

</br>

> [!VIDEO https://www.youtube.com/embed/9LvafyazlJM]

Özel konuşma çözümleri için şu anda aşağıdaki uygulamalar için ses Yardımcısı şablonları kullanılabilir:

- Hosititesi: ses denetimli akıllı cihazlarla birlikte bulunan otel odası.
- Sağlık Hizmetleri: Bu özellik, sesli denetimli akıllı cihazlarla donatılmıştır.
- Envanter: ses denetimli akıllı cihazlarla birlikte bulunan envanter hub 'ı.
- Oto: ses denetimli akıllı cihazlarla donatılmış bir şekilde, oto.

Önceden oluşturulmuş sesli yardım anahtar sözcükleri ve komutları Portal üzerinden doğrudan kullanılabilir. Özel anahtar sözcükler ve komutlar, Azure bilişsel hizmetler 'in de bir parçası olan [konuşma Studio](https://speech.microsoft.com/)'da oluşturulabilir ve eğitilmiş olabilir.

## <a name="advanced-development"></a>Gelişmiş geliştirme

Lütfen güncel rehberlik, öğreticiler ve örnekler için bkz. [Azure PERCEPT dk gelişmiş geliştirme GitHub](https://github.com/microsoft/azure-percept-advanced-development) , şunun gibi şeyler:

* Cihaza özel bir AI modeli getiriliyor
* Aktarım öğrenimi ile zaten desteklediğimiz bir modeli güncelleştirme
* Ve daha fazlası

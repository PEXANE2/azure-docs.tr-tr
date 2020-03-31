---
title: Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış - Azure | Microsoft Docs
description: Bir iş senaryosunda bir hatanın oluşma olasılığını tahmin eden Azure IoT Predictive Maintenance çözüm hızlandırıcısına genel bakış.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: 0661503dce7ac2707065f60c3952da866ce9dcf3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73827425"
---
# <a name="predictive-maintenance-solution-accelerator-overview"></a>Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış

Tahmine Dayalı Bakım çözümü, arızanın oluşma ihtimali olan anı tahmin eden iş senaryosu için uçtan uca bir çözümdür. Bu çözüm hızlandırıcısını, bakım iyileştirmesi gibi etkinlikler için proaktif olarak kullanabilirsiniz. Çözüm, IoT Hub ve [Azure Machine Learning][lnk-machine-learning] çalışma alanı gibi önemli Azure IoT çözüm hızlandırıcılarını birleştirir. Bu çalışma alanı, bir uçak motorunun Kalan Kullanım Ömrü’nü (RUL) öngörmek için genel bir örnek veri kümesini temel alan bir model içerir. Bu çözüm, kendinize özel iş gereksinimlerinizi karşılayacak bir çözümü planlamanız ve uygulamanız amacıyla sizin için bir başlangıç noktası olarak IoT iş senaryosunu tam olarak uygular.

Predictive Bakım çözüm hızlandırıcı [kodu GitHub'da mevcuttur.](https://github.com/Azure/azure-iot-predictive-maintenance)

## <a name="logical-architecture"></a>Mantıksal mimari

Aşağıdaki diyagram, çözüm hızlandırıcısının mantıksal bileşenlerinin ana hatlarını vermektedir:

![Mantıksal mimari][img-architecture]

Mavi öğeler, çözüm hızlandırıcısını dağıttığınız bölgede sağlanan Azure hizmetleridir. Çözüm hızlandırıcısını dağıtabileceğiniz bölgelerin listesi, [sağlama sayfasında][lnk-azureiotsolutions] görüntülenir.

Yeşil öğe simüle uçak motorudur. [Sanal cihazlar](#simulated-devices) bölümde bu sanal cihazlarla ilgili daha fazla bilgiye ulaşabilirsiniz.

Gri öğeler *aygıt yönetimi* yeteneklerini uygulayan bileşenlerdir. Tahmine Dayalı Bakım çözüm hızlandırıcısının geçerli sürümü, bu kaynakları sağlamaz. Cihaz yönetimi hakkında daha fazla bilgi edinmek için [Uzaktan İzleme çözüm hızlandırıcısına][lnk-remote-monitoring]bakın.

## <a name="azure-resources"></a>Azure kaynakları

Azure portalda sağlanan kaynaklarınızı görüntülemek için seçtiğiniz çözüm adına sahip kaynak grubuna gidin.

![Hızlandırıcı kaynakları][img-resource-group]

Çözüm hızlandırıcısını sağladığınızda, Machine Learning çalışma alanına bağlantısı da olan bir e-posta alırsınız. [Ayrıca, Microsoft Azure IoT Çözüm Hızlandırıcıları][lnk-azureiotsolutions] sayfasından Makine Öğrenimi çalışma alanına da gidebilirsiniz. Çözüm **Hazır** durumda olduğunda bu sayfada bir kutucuk kullanılabilir.

![Makine öğrenimi modeli][img-machine-learning]

## <a name="simulated-devices"></a>Sanal cihazlar

Çözüm hızlandırıcıda, simüle edilmiş bir cihaz bir uçak motorudur. Çözüm, tek bir uçakla eşlenen 2 motorla sağlanır. Her motor dört tür telemetri yayar: Algılayıcı 9, Algılayıcı 11, Algılayıcı 14 ve Algılayıcı 15, Machine Learning modelinin bu motorun RUL değerini hesaplaması için gereken verileri sağlar. Her sanal cihaz IoT Hub'ına şu telemetri iletilerini gönderir:

*Döngü sayısı*. Bir döngü iki ila on saat arasında bir süre ile tamamlanmış bir uçuş. Uçuş sırasında telemetri verileri yarım saatte bir yakalanır.

*Telemetri.* Motor özelliklerini kaydeden dört sensör var. Bu algılayıcılar genel olarak Algılayıcı 9, Algılayıcı 11, Algılayıcı 14 ve Algılayıcı 15 olarak etiketlenir. Bu dört sensör, RUL modelinden yararlı sonuçlar almak için yeterli telemetri gönderir. Çözüm hızlandırıcısında kullanılan model, gerçek altyapı algılayıcı verilerinin bulunduğu ortak bir veri kümesinden oluşturulur. Özgün veri kümesinden modelin oluşturulması hakkında daha fazla bilgi için bkz. [Cortana Intelligence Gallery Tahmine Dayalı Bakım Şablonu][lnk-cortana-analytics].

Sanal cihazlar, çözümde IoT hub'ı tarafından gönderilen aşağıdaki komutları işleyebilir:

| Komut | Açıklama |
| --- | --- |
| StartTelemetry |Benzetim durumunu denetler.<br/>Cihazın telemetri göndermesini başlatır |
| StopTelemetry |Benzetim durumunu denetler.<br/>Cihazın telemetri göndermesini durdurur |

IoT hub'ı cihaz komut bildirim sağlar.

## <a name="azure-stream-analytics-job"></a>Azure Stream Analytics işi

**İş: Telemetri**, gelen cihaz telemetrisi akışını iki durumu kullanarak çalıştırır:

* İlki, cihazlardan tüm telemetriyi seçer ve bu verileri blob depolamaya gönderir. Buradan, web uygulamasında görselleştirilmiş.
* İkinciyse iki dakikalık kayan pencere üzerinde ortalama algılayıcı değerlerini ölçer ve bu verileri Olay hub'ı aracılığıyla **olay işlemcisi**’ne gönderir.

## <a name="event-processor"></a>Olay işlemcisi
**Olay işleyicisi konağı** bir Azure Web İşi’nde çalıştırır. **Olay işlemcisi**, tamamlanan bir döngü için ortalama algılayıcı değerlerini alır. Daha sonra bu değerleri, bir motor için RUL'u hesaplayan eğitimli bir modele geçirir. API, çözümün bir parçası olan Makine Öğrenimi çalışma alanında modele erişim sağlar.

## <a name="machine-learning"></a>Machine Learning
Machine Learning bileşeni gerçek uçak motorlarından toplanan verilerden türetilmiş bir model kullanır. [azureiotsolutions.com][lnk-azureiotsolutions] sayfasındaki çözümünüzün döşemesinden Makine Öğrenimi çalışma alanına gidebilirsiniz. Çözüm **Hazır** durumda olduğunda kutucuk kullanılabilir.

Machine Learning modeli, IoT çözüm hızlandırıcı hizmetleri aracılığıyla toplanan telemetri ile nasıl çalışılabildiğini gösteren bir şablon olarak kullanılabilir. Microsoft, genel [regression model][lnk_regression_model] kullanıma açık veriler<sup>\[\]1'e</sup>ve modelin nasıl kullanılacağına ilişkin adım adım kılavuzluyor.

Azure IoT Tahmine Dayalı Bakım çözümü, bu şablondan oluşturulan regresyon modelini kullanır. Model Azure aboneliğinize dağıtılır ve otomatik olarak oluşturulan API aracılığıyla kullanıma sunulur. Çözüm, 4 (toplam 100 toplam) motor ve 4 (toplam 21 toplam) sensör veri akışı için test verilerinin bir alt kümesini içerir. Bu veriler, eğitilmiş modelden doğru bir sonuç elde etmek için yeterlidir.

*\[1\] A. Saxena ve K. Goebel (2008). "Turbofan Motor Bozunma Simülasyonu Veri Seti", NASA Ames Prognostikveri Deposu (https://c3.nasa.gov/dashlink/resources/139/), NASA Ames Araştırma Merkezi, Moffett Field, CA*

## <a name="next-steps"></a>Sonraki adımlar
Tahmine Dayalı Bakım çözüm hızlandırıcısının temel bileşenlerini gördüğünüze göre bunları özelleştirmek isteyebilirsiniz.

Ayrıca, IoT çözüm hızlandırıcılarının diğer özelliklerinden bazılarını da keşfedebilirsiniz:

* [IoT çözüm hızlandırıcıları için sık sorulan sorular][lnk-faq]
* [Baştan sona IoT güvenliği][lnk-security-groundup]

[img-architecture]: media/iot-accelerators-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-accelerators-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-accelerators-predictive-walkthrough/machine-learning.png

[lnk-remote-monitoring]: quickstart-predictive-maintenance-deploy.md
[lnk-cortana-analytics]: https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsolutions]: https://www.azureiotsolutions.com/
[lnk-faq]: iot-accelerators-faq.md
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_regression_model]: https://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3

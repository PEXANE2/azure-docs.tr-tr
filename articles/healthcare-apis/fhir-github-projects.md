---
title: FHıR için Azure API 'SI ile ilgili GitHub projeleri
description: FHıR için Azure API 'SI için tüm açık kaynak (GitHub) depolarını listeleyin.
services: healthcare-apis
author: ginalee-dotcom
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 02/01/2021
ms.author: ginle
ms.openlocfilehash: 9977765183bd567377592631d65f3e548bef4b34
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2021
ms.locfileid: "103548035"
---
# <a name="related-github-projects"></a>İlgili GitHub projeleri

GitHub 'da, çeşitli kullanımlar için Hizmetleri dağıtmaya yönelik kaynak kodu ve yönergeler sağlayan çok sayıda açık kaynaklı proje vardır. Özellik ve ürünlerimizi öğrenmek ve denemek için GitHub depolarımızı ziyaret etmeye her zaman hoş geldiniz. 

## <a name="fhir-server"></a>FHıR sunucusu
* [Microsoft/fhır-Server](https://github.com/microsoft/fhir-server/): Fhır için Azure API 'si temel olan açık kaynaklı Fhır sunucusu
* En son sürümleri görmek için lütfen [sürüm notlarına](https://github.com/microsoft/fhir-server/releases) bakın
* [Microsoft/fhır-Server-Samples](https://github.com/microsoft/fhir-server-samples): örnek ortam

## <a name="data-conversion--anonymization"></a>Veri dönüştürme & anonimleştirme

#### <a name="fhir-converter"></a>FHıR Dönüştürücüsü
* [Microsoft/fhır-Converter](https://github.com/microsoft/FHIR-Converter): eski veri biçimlerini fhır 'ye çevirmek için bir dönüştürme yardımcı programı
* $Convert-veri işlemi biçiminde Azure için FHıR 'nin yanı sıra FHıR sunucusu ile tümleşik
* OSS 'de devam eden geliştirmeler ve FHıR sunucularıyla sürekli tümleştirme
 
#### <a name="fhir-converter---vs-code-extension"></a>FHıR dönüştürücü-VS Code uzantısı
* [Microsoft/FHıR-için-anonimleştirme araçları](https://github.com/microsoft/FHIR-Tools-for-Anonymization): verilerle yardım eden bir araç kümesi (fhır biçiminde) anonimleştirme
* FHAR için Azure API 'SI ile ve Azure için FHıR sunucusu ' de tanımlı dışarı aktarma ' biçiminde tümleşiktir

#### <a name="fhir-tools-for-anonymization"></a>Anonimleştirme için FHıR araçları
* [Microsoft/vscode-azurehealthcareapis-Tools](https://github.com/microsoft/vscode-azurehealthcareapis-tools): Azure sağlık API 'leri ile çalışacak bir araç koleksiyonu içeren vs Code uzantısı
* Visual Studio Market serbest bırakıldı
* FHIR dönüştürücüde kullanılacak likit şablonları yazmak için kullanılır

## <a name="iot-connector"></a>IoT Bağlayıcısı

#### <a name="integration-with-iot-hub-and-iot-central"></a>IoT Hub ve IoT Central tümleştirmesi
* [Microsoft/ıomt-fhır](https://github.com/microsoft/iomt-fhir): veri normalleştirme ve normalleştirilmiş verilerin fhır dönüştürmesi ile fhır 'ye IoT Hub veya IoT Central tümleştirme
* Normalleştirme: cihaz veri bilgileri, daha fazla işleme için ortak bir biçimde ayıklanır
* FHıR dönüştürme: normalleştirilmiş ve gruplanmış veriler FHAR ile eşleştirilir. Gözlemler, yapılandırılmış şablonlara göre oluşturulur veya güncelleştirilir ve cihaz ve hasta ile bağlantılıdır.
* [Konuşma haritasını oluşturmaya yardımcı olacak araçlar](https://github.com/microsoft/iomt-fhir/tree/master/tools/data-mapper): cihaz giriş verilerini normalleşme ve fhır kaynaklarına dönüştürmek için eşleme yapılandırmasını görselleştirin. Geliştiriciler, eşlemeleri, cihaz eşlemesini ve FHıR eşlemesini düzenlemek ve test etmek ve bunları Azure portal IoT bağlayıcısına yüklemek üzere dışarı aktarmak için bu aracı kullanabilir.

#### <a name="healthkit-and-fhir-integration"></a>HealthKit ve FHıR tümleştirmesi
* [Microsoft/HealthKit-on-fhir](https://github.com/microsoft/healthkit-on-fhir): Apple HealthKit verilerinin bir Fhır sunucusuna aktarılmasını otomatikleştiren bir Swift kitaplığı

 
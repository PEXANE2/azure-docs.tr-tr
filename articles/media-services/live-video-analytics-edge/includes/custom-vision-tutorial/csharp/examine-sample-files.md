---
author: russell-cooks
ms.service: media-services
ms.topic: include
ms.date: 10/05/2020
ms.author: russellcooks
ms.openlocfilehash: 359c5f93516ea6f0561865bd86e4f51dedb4c3a5
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "94358292"
---
1. Visual Studio Code, src/Edge öğesine gidin. Oluşturduğunuz. env dosyasını, birkaç dağıtım şablonu dosyası ile birlikte görürsünüz.

    Dağıtım şablonu, kenar cihazının bazı yer tutucu değerleriyle birlikte dağıtım bildirimini ifade eder. . Env dosyası bu değişkenlerin değerlerini içerir.
1. Ardından src/buluttan cihaza-Console-App klasörüne gidin. Burada, oluşturduğunuz dosyada, diğer birkaç dosyayla birlikte appsettings.jsgörürsünüz:

    * C2D-Console-App. csproj: Visual Studio Code için proje dosyasıdır.
    * operations.js: Bu dosya, programın çalıştırmasını istediğiniz farklı işlemleri listeler.
    * Program. CS: Bu örnek program kodu:

        * Uygulama ayarlarını yükler.
        * Topoloji oluşturmak, grafiği örneklemek ve grafiği etkinleştirmek için IoT Edge modülünün doğrudan yöntemlerinde canlı video analizlerini çağırır.
        * , **TERMINAL** penceresinde grafik çıkışını ve **Çıkış** penceresinde IoT Hub 'ına gönderilen olayları incelemenizi sağlar.
        * Grafik örneğini devre dışı bırakır, grafik örneğini siler ve grafik topolojisini siler.

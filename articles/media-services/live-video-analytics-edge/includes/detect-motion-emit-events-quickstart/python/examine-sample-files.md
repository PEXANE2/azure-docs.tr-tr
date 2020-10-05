---
ms.openlocfilehash: 46e8609be2ec57600e666ef9dab33eae2900e1ce
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "88691127"
---
1. Visual Studio Code, *src/Edge*bölümüne gidin. *. Env* dosyası ve birkaç dağıtım şablonu dosyası görürsünüz.

    Dağıtım şablonu, sınır cihazının bazı özellikler için kullanıldığı dağıtım bildirimini ifade eder. *. Env* dosyası bu değişkenlerin değerlerini içerir.
1. *Src/buluttan cihaza-Console-App* klasörüne gidin. Burada dosya ve diğer birkaç dosya *appsettings.js* görürsünüz:

    * ***operations.js*** , programın çalıştırmasını istediğiniz işlemlerin bir listesi.
    * **Main.py** -örnek program kodu. Bu kod:
    
      * Uygulama ayarlarını yükler.
      * IoT Edge modülündeki canlı video analizi tarafından açığa çıkarılan doğrudan yöntemleri çağırır. [Doğrudan yöntemlerini](../../../direct-methods.md)çağırarak canlı video akışlarını çözümlemek için modülünü kullanabilirsiniz.
      * Programın çıkışını **TERMINAL** penceresinde Incelemenize ve **Çıkış** penceresinde modül tarafından oluşturulan olayları incelemenize olanak sağlamak için duraklar.
      * Kaynakları temizlemek için doğrudan yöntemleri çağırır.   


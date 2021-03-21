---
title: Azure Percept DK için taslak deneyimi sırasında sorun giderme
description: Taslak deneyimi sırasında bulunan daha yaygın sorunlardan bazıları için sorun giderme ipuçları alın
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: d71cfa6ba52052e4b68175be84934c8b4294ed25
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "101663814"
---
# <a name="azure-percept-dk-onboarding-experience-troubleshooting-guide"></a>Azure Percept DK ekleme deneyimi sorun giderme kılavuzu

Azure Percept DK ekleme deneyimi sırasında karşılaşabileceğiniz bazı sorunlar aşağıda verilmiştir. Bu kılavuzdaki adımları kullandıktan sonra sorun devam eder. Azure müşteri desteği 'ne başvurun.

|Sorun|Nedeni|Geçici çözüm|
|:-----|:------|:----------|
|Azure hesabı kaydolma sayfalarına veya Azure portal bağlanırken, önbelleğe alınmış hesapla otomatik olarak oturum açabilir. Bu, kullanmayı amaçladığı hesap değilse, belge ile tutarsız bir deneyim oluşmasına neden olabilir.|Bu genellikle tarayıcıda daha önce kullandığınız bir hesabı "hatırlama" gibi bir ayar nedeniyle oluşur.|Azure sayfasında, sayfanın sağ üst köşesine tıklayarak hesap adınızı gösterir ve ardından "Oturumu Kapat" a tıklayın. Bundan sonra doğru hesapla oturum açabileceksiniz.|
|Azure Percept DK erişim noktası (SCZ-xxxx) ağı kullanılabilir Wi-Fi ağları listesinde görünmez.|Bu genellikle, kendisini kısa bir süre içinde çözümleyen geçici bir sorundur.|Ağın görünmesini bekleyin. 15 dakikadan daha uzun bir süre sonra değilse, cihazı yeniden başlatın.|
|Azure Percept DK erişim noktasıyla bağlantı genellikle bağlantıyı keser.|Bunun nedeni, cihazla ana bilgisayar arasında zayıf bir bağlantı olabilir. Ayrıca, ana bilgisayardaki diğer Wi-Fi bağlantılarından kaynaklanan girişim de oluşabilir.|Antenin Dev Kit 'e doğru bir şekilde bağlı olduğundan emin olun. Geliştirme seti ana bilgisayardan uzakta değilse, daha yakınına geçmeyi deneyin. Ana bilgisayarda çalışıyorsa LTE/5G gibi diğer internet bağlantılarını devre dışı bırakın.|
|Ana bilgisayar, Azure Percept DK erişim noktasıyla bağlantı hakkında bir güvenlik uyarısı gösterir.|Bu, sonraki bir güncelleştirmede düzeltilecektir bilinen bir sorundur.|Devkit Wi-Fi erişim noktası üzerinden ekleme deneyiminde ilerlemeniz güvenlidir.|
|Azure Percept DK erişim noktası (SCZ-xxxx) ağı ağ listesinde görüntülenir, ancak bağlantı başarısız olur.|Bunun nedeni, devkit Wi-Fi erişim noktasının geçici bozulması olabilir.|Devkit 'i yeniden başlatın ve yeniden deneyin.|
|Kurulum deneyimi sırasında Wi-Fi ağa bağlanılamıyor.|Azure ile iletişim kurabilmeleri için Wi-Fi ağı şu anda internet bağlantısına sahip olmalıdır. EAP [PEAP/MSCHAP], captive portalları ve kurumsal EAP-TLS bağlantısı şu anda desteklenmiyor.|Bağlanmakta olduğunuz Wi-Fi ağın desteklendiğinden ve internet bağlantısına sahip olduğundan emin olun.|
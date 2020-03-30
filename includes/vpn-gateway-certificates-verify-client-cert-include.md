---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 12/11/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c0ce4e882f270f5e0c789a608aaada5c6c9cba92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188230"
---
Bağlanmada sorun yaşıyorsanız, aşağıdaki öğeleri kontrol edin:

- **Sertifika Verme Sihirbazı**olan bir istemci sertifikası dışa aktarırsanız, bunu .pfx dosyası olarak dışa aktardığınızdan ve **mümkünse tüm sertifikaları sertifika yoluna dahil ettiğinizden**emin olun. Bu değerle dışa aktardığınızda, kök sertifika bilgileri de dışa aktarılır. Sertifikayı istemci bilgisayara yükledikten sonra .pfx dosyasındaki kök sertifika da yüklenir. Kök sertifikanın yüklü olduğunu doğrulamak **için, kullanıcı sertifikalarını yönet'i** açın ve **Güvenilir Kök Sertifika Yetkilileri\Sertifikalar'ı**seçin. Kimlik doğrulamanın çalışması için bulunması gereken kök sertifikanın listelenmiş olduğunu doğrulayın.

- Enterprise CA çözümü tarafından verilmiş bir sertifika kullandıysanız ve kimlik doğrulaması yapamazsanız, istemci sertifikasındaki kimlik doğrulama siparişini doğrulayın. İstemci sertifikasını çift tıklatarak, **Ayrıntılar** sekmesini seçerek ve gelişmiş **anahtar kullanımını**seçerek kimlik doğrulama listesi sırasını kontrol edin. *İstemci Kimlik Doğrulaması'nın* listedeki ilk öğe olduğundan emin olun. Değilse, listedeki ilk öğe olarak *İstemci Kimlik Doğrulaması* olan kullanıcı şablonunu temel alan bir istemci sertifikası düzenleyin.

- P2S hakkında ek sorun giderme bilgileri için bkz. [P2S bağlantılarının sorunlarını giderme](../articles/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
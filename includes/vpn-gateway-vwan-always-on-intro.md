---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/12/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4ea97e2dbee87f7ab129c4295276c9024c0212c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80116896"
---
Windows 10 VPN istemcisinin yeni bir özelliği, Always On, vpn bağlantısını korumak için yeteneğidir. Her Zaman Açık'ta, etkin VPN profili otomatik olarak bağlanabilir ve kullanıcı oturum açma, ağ durumu değişikliği veya aygıt ekranı etkin gibi tetikleyicilere bağlı olarak bağlı kalabilir.

Windows 10 Always On ile ağ geçitlerini kullanarak Azure'a sürekli kullanıcı tünelleri ve aygıt tünelleri oluşturabilirsiniz. Bu makale, her zaman VPN kullanıcı tünelini yapılandırmanıza yardımcı olur.

Her zaman VPN bağlantıları iki tür tüneliçerir:

* **Aygıt tüneli**: Kullanıcılar aygıtta oturum açmadan önce belirtilen VPN sunucularına bağlanır. Oturum açma öncesi bağlantı senaryoları ve aygıt yönetimi bir aygıt tüneli kullanır.

* **Kullanıcı tüneli**: Yalnızca kullanıcılar cihazda oturum açtıktan sonra bağlanır. Kullanıcı tünellerini kullanarak, VPN sunucuları aracılığıyla kuruluş kaynaklarına erişebilirsiniz.

Aygıt tünelleri ve kullanıcı tünelleri VPN profillerinden bağımsız olarak çalışır. Bunlar aynı anda bağlanabilir ve uygun şekilde farklı kimlik doğrulama yöntemlerini ve diğer VPN yapılandırma ayarlarını kullanabilirler.

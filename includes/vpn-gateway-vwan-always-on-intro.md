---
title: dosya dahil etme
description: dosya dahil etme
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/12/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4ea97e2dbee87f7ab129c4295276c9024c0212c7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80116896"
---
Windows 10 VPN istemcisinin yeni bir özelliği olan her zaman açık, bir VPN bağlantısını sürdürme olanağıdır. Her zaman açık olduğunda, etkin VPN profili otomatik olarak bağlanabilir ve Kullanıcı oturum açma, ağ durumu değişikliği veya cihaz ekranı etkin gibi tetikleyicilere göre bağlı kalır.

Azure 'a kalıcı Kullanıcı tüneli ve cihaz tünelleri sağlamak için Windows 10 her zaman açık olan ağ geçitleri kullanabilirsiniz. Bu makale, her zaman açık VPN Kullanıcı tüneli yapılandırmanıza yardımcı olur.

Her zaman VPN bağlantılarında iki tür tünelden biri vardır:

* **Cihaz tüneli**: kullanıcılar cihazda oturum açmadan önce belirtilen VPN sunucularına bağlanır. Oturum açma öncesi bağlantı senaryoları ve cihaz yönetimi bir cihaz tüneli kullanır.

* **Kullanıcı tüneli**: yalnızca kullanıcılar cihazda oturum açtıktan sonra bağlanır. Kullanıcı tünellerini kullanarak, VPN sunucuları üzerinden kuruluş kaynaklarına erişebilirsiniz.

Cihaz tünelleri ve Kullanıcı tünelleri, VPN profillerinin bağımsız olarak çalışır. Bunlar aynı anda birbirine bağlanabilir ve uygun şekilde farklı kimlik doğrulama yöntemleri ve diğer VPN yapılandırma ayarlarını kullanabilirler.

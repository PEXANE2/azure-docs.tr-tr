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
ms.openlocfilehash: 2783c828f96eeb3539e2266eaf1d6d590a6af4c4
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79371024"
---
Windows 10 VPN istemcisinin yeni bir özelliği olan her zaman açık, bir VPN bağlantısını sürdürme olanağıdır. Her zaman açık olduğunda, etkin VPN profili otomatik olarak bağlanabilir ve Kullanıcı oturum açma, ağ durumu değişikliği veya cihaz ekranı etkin gibi tetikleyicilere göre bağlı kalır.

Azure 'da kalıcı Kullanıcı tünellerini ve cihaz tünellerini oluşturmak için Azure sanal ağ geçitleri 'ni her zaman açık Windows 10 ile kullanabilirsiniz. Bu makale, her zaman açık VPN Kullanıcı tüneli yapılandırmanıza yardımcı olur.

Her zaman VPN bağlantılarında iki tür tünelden biri vardır:

* **Cihaz tüneli**: kullanıcılar cihazda oturum açmadan önce belirtilen VPN sunucularına bağlanır. Oturum açma öncesi bağlantı senaryoları ve cihaz yönetimi bir cihaz tüneli kullanır.

* **Kullanıcı tüneli**: yalnızca kullanıcılar cihazda oturum açtıktan sonra bağlanır. Kullanıcı tünellerini kullanarak, VPN sunucuları üzerinden kuruluş kaynaklarına erişebilirsiniz.

Cihaz tünelleri ve Kullanıcı tünelleri, VPN profillerinin bağımsız olarak çalışır. Bunlar aynı anda birbirine bağlanabilir ve uygun şekilde farklı kimlik doğrulama yöntemleri ve diğer VPN yapılandırma ayarlarını kullanabilirler.

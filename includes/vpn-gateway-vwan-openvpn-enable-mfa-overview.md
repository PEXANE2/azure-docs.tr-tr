---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/14/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 3417bf0bd4ae1e0aa670f9fbfcc1fbbfeb372972
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471571"
---
Kullanıcılara erişim vermeden önce ikinci bir kimlik doğrulama faktörü için soru lmasını istiyorsanız, Azure Çok Faktörlü Kimlik Doğrulaması'nı (MFA) yapılandırabilirsiniz. MFA'yı kullanıcı bazında yapılandırabilir veya [Koşullu Erişim](../articles/active-directory/conditional-access/overview.md)ile MFA'dan yararlanabilirsiniz.

* Kullanıcı başına MFA, ek ücret ödemeden etkinleştirilebilir. Kullanıcı başına MFA etkinleştirildiğinde, kullanıcıdan Azure AD kiracısına bağlı tüm uygulamalara karşı ikinci faktör kimlik doğrulaması istenir. Adımlar için [Seçenek 1'e](#peruser) bakın.
* Koşullu Erişim, ikinci bir faktörün nasıl tanıtılması gerektiği üzerinde daha ince taneli denetim sağlar. MFA'nın yalnızca VPN'e atanmasına izin verebilir ve Azure AD kiracısına bağlı diğer uygulamaları hariç tutabilir. Adımlar için [Seçenek 2'ye](#conditional) bakın.
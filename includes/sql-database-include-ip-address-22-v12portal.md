---
title: Sunucu düzeyinde güvenlik duvarı kuralları
description: Sunucu düzeyinde güvenlik duvarı kuralları
keywords: sql bağlantısı,bağlantı dizesi
services: sql-database
author: dalechen
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: include
ms.date: 07/13/2018
ms.author: ninarn
ms.openlocfilehash: 8d0f9899dbb7599340b8d15ca010a0157011fb9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67188450"
---
1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. Soldaki listede, Tüm **hizmetleri**seçin.

3. Kaydırma ve **SQL sunucuları**seçin.

    ![Azure SQL Veritabanı sunucunuzu portalda bulun][b21-FindServerInPortal]
5. Filtre metin kutusuna sunucunuzun adını yazmaya başlayın. Satırınız görüntülenir.

6. Sunucunuzun satırını seçin. Sunucunuz için bir bıçak görüntülenir.

7. Sunucu bıçağınızda **Ayarlar'ı**seçin.

8. **Güvenlik Duvarı'nı**seçin.

    ![Güvenlik Duvarı > Ayarları'nı seçin][b31-SettingsFirewallNavig]
9. **İstemci IP Ekle'yi**seçin. İlk metin kutusuna yeni kuralınız için bir ad yazın.

10. Etkinleştirmek istediğiniz aralık için düşük ve yüksek IP adresi değerlerini yazın.

    * Düşük değerin **0,0** ve yüksek değer in **0,255**ile bitmek olması kullanışlı olabilir.

11. **Kaydet'i**seçin.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->

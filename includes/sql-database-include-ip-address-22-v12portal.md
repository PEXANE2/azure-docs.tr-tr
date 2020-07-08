---
title: Sunucu düzeyinde güvenlik duvarı kuralları
description: Sunucu düzeyinde güvenlik duvarı kuralları
keywords: SQL bağlantısı, bağlantı dizesi
services: sql-database
author: dalechen
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: include
ms.date: 07/13/2018
ms.author: ninarn
ms.openlocfilehash: 8d0f9899dbb7599340b8d15ca010a0157011fb9e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188450"
---
1. [Azure portalında](https://portal.azure.com/) oturum açın.

2. Sol taraftaki listede **tüm hizmetler**' i seçin.

3. **SQL Server**'ı kaydırın ve seçin.

    ![Portalda Azure SQL veritabanı sunucunuzu bulma][b21-FindServerInPortal]
5. Filtre metin kutusuna sunucunuzun adını yazmaya başlayın. Satırınız görüntülenir.

6. Sunucunuzun satırını seçin. Sunucunuz için bir dikey pencere görüntülenir.

7. Sunucu dikey penceresinde **Ayarlar**' ı seçin.

8. **Güvenlik duvarı**' nı seçin.

    ![Güvenlik Duvarı > ayarları seçin][b31-SettingsFirewallNavig]
9. **İstemci IP 'Si Ekle**' yi seçin. İlk metin kutusuna yeni kuralınız için bir ad yazın.

10. Etkinleştirmek istediğiniz aralığın düşük ve yüksek IP adresi değerlerini yazın.

    * En düşük değer olan **0** ve yüksek değer ile bitiş değeri **. 255**olmalıdır.

11. **Kaydet**'i seçin.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->

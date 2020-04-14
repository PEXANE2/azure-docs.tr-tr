---
title: Yavaş sorgu günlüklerine erişin - Azure portalı - MySQL için Azure Veritabanı
description: Bu makalede, Azure portalından MySQL için Azure Veritabanı'ndaki yavaş günlüklerin nasıl yapılandırılabildiğini ve bunlara erişilenler açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 4/13/2020
ms.openlocfilehash: 59faf63312bd7cc657f8b96ca3110707ea997c02
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81273622"
---
# <a name="configure-and-access-slow-query-logs-from-the-azure-portal"></a>Azure portalından yavaş sorgu günlüklerini yapılandırma ve erişim

[MySQL yavaş sorgu günlükleri için Azure Veritabanını](concepts-server-logs.md) Azure portalından yapılandırabilir, listeleyebilir ve indirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu makaledeki adımlar, [MySQL sunucusu için Azure Veritabanı'na](quickstart-create-mysql-server-database-using-azure-portal.md)sahip olduğunuzu gerektirir.

## <a name="configure-logging"></a>Günlük işlemlerini yapılandırma
MySQL yavaş sorgu günlüğüne erişimi yapılandırın. 

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. MySQL sunucusu için Azure Veritabanınızı seçin.

3. Kenar çubuğundaki **İzleme** bölümü altında **Sunucu günlüklerini**seçin. 
   ![Sunucu günlükleri seçeneklerinin ekran görüntüsü](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Sunucu parametrelerini görmek **için, günlükleri etkinleştirmek ve günlük parametrelerini yapılandırmak için buraya tıklayın'ı**seçin.

5. **slow_query_log'ı** **A**

6. log_output kullanarak günlükleri çıktının nerede **yapılacağına**seçin. Günlükleri hem yerel depolama alanına hem de Azure MonitörÜ Tanı Günlüklerine göndermek için **Dosya'yı**seçin. 

7. Gereken diğer parametreleri değiştirin. 

8. **Kaydet'i**seçin. 

   :::image type="content" source="./media/howto-configure-server-logs-in-portal/3-save-discard.png" alt-text="Yavaş sorgu günlüğü parametrelerinin ekran görüntüsü ve kaydedin.":::

Sunucu **Parametreleri** sayfasından, sayfayı kapatarak günlükler listesine dönebilirsiniz.

## <a name="view-list-and-download-logs"></a>Listeyi görüntüleme ve indirme günlüklerini
Günlüğe kaydetme başladıktan sonra, kullanılabilir yavaş sorgu günlüklerinin listesini görüntüleyebilir ve tek tek günlük dosyalarını indirebilirsiniz.

1. Azure portalı açın.

2. MySQL sunucusu için Azure Veritabanınızı seçin.

3. Kenar çubuğundaki **İzleme** bölümü altında **Sunucu günlüklerini**seçin. Sayfada günlük dosyalarınızın bir listesi gösterilmektedir.

   ![Sunucu günlükleri sayfasının ekran görüntüsü, günlüklerin listesi vurgulanmış](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > Günlüğün adlandırma kuralı **mysql-slow-< sunucu adı>-yyyymmddhh.log**. Dosya adında kullanılan tarih ve saat, günlüğün verildiği zamandır. Günlük dosyaları her 24 saat veya 7,5 GB'da bir döndürülür, hangisi önce gelirse. 

4. Gerekirse, tarih ve saate bağlı olarak belirli bir günlüğe hızla daraltmak için arama kutusunu kullanın. Arama günlüğün adı üzerinde.

5. Tek tek günlük dosyalarını indirmek için, tablo satırındaki her günlük dosyasının yanındaki aşağı ok simgesini seçin.

   ![Aşağı ok simgesi vurgulanmış sunucu günlükleri sayfasının ekran görüntüsü](./media/howto-configure-server-logs-in-portal/5-download.png)

## <a name="set-up-diagnostic-logs"></a>Tanılama günlükleri ayarlama

1. Kenar çubuğundaki **İzleme** bölümünün altında **Tanılama ayarlarını** > seçin**Tanılama ayarları ekleyin.**

   ![Tanılama ayarları seçeneklerinin ekran görüntüsü](./media/howto-configure-server-logs-in-portal/add-diagnostic-setting.png)

1. Tanılama ayarı adı sağlayın.

1. Yavaş sorgu günlüklerini (depolama hesabı, olay merkezi veya Log Analytics çalışma alanı) göndermek için hangi verilerin battığını belirtin.

1. Günlük türü olarak **MySqlSlowLogs'u** seçin.
![Tanılama ayarları yapılandırma seçeneklerinin ekran görüntüsü](./media/howto-configure-server-logs-in-portal/configure-diagnostic-setting.png)

1. Veri lavabolarını yavaş sorgu günlüklerini borulamak için yapılandırıldıktan sonra **Kaydet'i**seçin.
![Kaydet vurgulanmış tanılama ayarları yapılandırma seçeneklerinin ekran görüntüsü](./media/howto-configure-server-logs-in-portal/save-diagnostic-setting.png)

1. Yapılandırmayaptığınız veri lavabolarında bunları keşfederek yavaş sorgu günlüklerine erişin. Günlüklerin görünmesi 10 dakika kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
- Yavaş sorgu günlüklerini programlı olarak nasıl indireceklerini öğrenmek için [CLI'deki Yavaş sorgu Günlüklerine Eriş'e](howto-configure-server-logs-in-cli.md) bakın.
- MySQL için Azure Veritabanı'nda [yavaş sorgu günlükleri](concepts-server-logs.md) hakkında daha fazla bilgi edinin.
- Parametre tanımları ve MySQL günlüğü hakkında daha fazla bilgi için [günlüklerde](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)MySQL belgelerine bakın.
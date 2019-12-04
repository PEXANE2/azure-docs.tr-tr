---
title: Yavaş sorgu günlüklerine erişme-Azure portal-MySQL için Azure veritabanı
description: Bu makalede, Azure portal MySQL için Azure veritabanı 'nda yavaş günlüklerin nasıl yapılandırılacağı ve erişebileceği açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 52d219a94676e0f1ce4ceac31b0b50c9d36665ba
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74770586"
---
# <a name="configure-and-access-slow-query-logs-from-the-azure-portal"></a>Azure portal yavaş sorgu günlüklerini yapılandırma ve erişme

[MySQL Için Azure veritabanı yavaş sorgu günlüklerini](concepts-server-logs.md) Azure Portal yapılandırabilir, listeleyebilir ve indirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu makaledeki adımlarda, [MySQL Için Azure veritabanı sunucusu](quickstart-create-mysql-server-database-using-azure-portal.md)gerekir.

## <a name="configure-logging"></a>Günlüğe kaydetmeyi yapılandırma
MySQL yavaş sorgu günlüğüne erişimi yapılandırın. 

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.

2. MySQL için Azure veritabanı sunucunuzu seçin.

3. Kenar çubuğu 'ndaki **izleme** bölümünde **sunucu günlükleri**' ni seçin. 
   Sunucu günlüğü seçeneklerinin ekran görüntüsünü ![](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Sunucu parametrelerini görmek için, **günlükleri etkinleştirmek ve günlük parametrelerini yapılandırmak için buraya tıklayın ' ı**seçin.

5. Ayarlamanız gereken parametreleri değiştirin. Bu oturumda yaptığınız tüm değişiklikler mor renkle vurgulanır. 

   Parametreleri değiştirdikten sonra **Kaydet**' i seçin. Ya da değişikliklerinizi atabilirsiniz.

   ![Sunucu parametreleri seçeneklerinin ekran görüntüsü](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

**Sunucu parametreleri** sayfasında, sayfayı kapatarak Günlükler listesine geri dönebilirsiniz.

## <a name="view-list-and-download-logs"></a>Listeyi görüntüle ve günlükleri indir
Günlüğe kaydetme başladıktan sonra, kullanılabilir yavaş sorgu günlüklerinin bir listesini görüntüleyebilir ve günlük dosyalarını tek tek indirebilirsiniz.

1. Azure portalı açın.

2. MySQL için Azure veritabanı sunucunuzu seçin.

3. Kenar çubuğu 'ndaki **izleme** bölümünde **sunucu günlükleri**' ni seçin. Sayfa, günlük dosyalarınızın bir listesini gösterir.

   ![Günlük listesi vurgulanmış şekilde sunucu günlükleri sayfasının ekran görüntüsü](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > Günlüğün adlandırma kuralı **MySQL-yavaş-sunucu adınızın >-yyyymmddhh. log <** . Dosya adında kullanılan tarih ve saat, günlüğün verildiği zaman. Günlük dosyaları her 24 saatte bir veya 7,5 GB döndürülür, hangisi önce gelir. 

4. Gerekirse, tarih ve saate göre belirli bir günlüğe hızlıca daraltmak için arama kutusunu kullanın. Arama günlüğün adı üzerinde.

5. Ayrı günlük dosyalarını indirmek için tablo satırındaki her bir günlük dosyasının yanındaki aşağı ok simgesini seçin.

   ![Aşağı ok simgesi vurgulanmış şekilde sunucu günlükleri sayfasının ekran görüntüsü](./media/howto-configure-server-logs-in-portal/5-download.png)

## <a name="set-up-diagnostic-logs"></a>Tanılama günlükleri ayarlama

1. Kenar çubuğu 'ndaki **izleme** bölümünde **Tanılama ayarları** > **Tanılama Ayarları Ekle**' yi seçin.

   ![Tanılama ayarları seçeneklerinin ekran görüntüsü](./media/howto-configure-server-logs-in-portal/add-diagnostic-setting.png)

1. Bir tanılama ayarı adı belirtin.

1. Yavaş sorgu günlüklerinin (depolama hesabı, Olay Hub 'ı veya Log Analytics çalışma alanı) hangi veri havuzlarını gönderileceğini belirtin.

1. Günlük türü olarak **Mysqlyavaşlogs** ' u seçin.
Tanılama ayarları yapılandırma seçeneklerinin ekran görüntüsünü ![](./media/howto-configure-server-logs-in-portal/configure-diagnostic-setting.png)

1. Yavaş sorgu günlüklerini kanala yönelten veri havuzlarını yapılandırdıktan sonra **Kaydet**' i seçin.
Tanılama ayarları yapılandırma seçeneklerinin ekran görüntüsünü ![vurgulanmış şekilde kaydet](./media/howto-configure-server-logs-in-portal/save-diagnostic-setting.png)

1. Bunları yapılandırdığınız veri havuzları içinde inceleyerek yavaş sorgu günlüklerine erişin. Günlüklerin görünmesi 10 dakikaya kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
- Yavaş sorgu günlüklerinin programlama yoluyla nasıl indirileceği hakkında bilgi edinmek için bkz. [CLI 'deki yavaş sorgu günlüklerine erişme](howto-configure-server-logs-in-cli.md) .
- MySQL için Azure veritabanı 'nda [yavaş sorgu günlükleri](concepts-server-logs.md) hakkında daha fazla bilgi edinin.
- Parametre tanımları ve MySQL günlüğü hakkında daha fazla bilgi için, [günlüklerdeki](https://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)MySQL belgelerine bakın.
---
title: Azure portal 'de MariaDB için Azure veritabanı için sunucu günlüklerini yapılandırma ve erişme
description: Bu makalede, Azure portal MariaDB için Azure veritabanı 'nda sunucu günlüklerinin nasıl yapılandırılacağı ve erişebileceği açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/30/2019
ms.openlocfilehash: c8be9519d3393330b3022fadd2de6a49e58ecdcf
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703508"
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Azure portal sunucu günlüklerini yapılandırma ve erişme

[MariaDB yavaş sorgu günlükleri Için Azure veritabanı](concepts-server-logs.md) 'nı Azure Portal yapılandırabilir, listeleyebilir ve indirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunda ilerlemek için şunlar gerekir:
- [MariaDB sunucusu için Azure veritabanı](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>Günlüğe kaydetmeyi yapılandırma
Yavaş sorgu günlüğüne erişimi yapılandırın. 

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.

2. MariaDB sunucusu için Azure veritabanınızı seçin.

3. Kenar çubuğu 'ndaki **izleme** bölümünde **sunucu günlükleri**' ni seçin. 
   @no__t-/sunucu günlüklerini seçin, @ no__t-1 yapılandırmak için tıklayın

4. Başlığı seçin ve sunucu parametrelerini görmek için **günlükleri etkinleştirmek ve günlük parametrelerini yapılandırmak için buraya tıklayın** .

5. "Slow_query_log" öğesini "ON" olarak açmak da dahil olmak üzere, ayarlamanız gereken parametreleri değiştirin. Bu oturumda yaptığınız tüm değişiklikler mor renkle vurgulanır. 

   Parametreleri değiştirildikten sonra **Kaydet**' e tıklayabilirsiniz. Ya da değişikliklerinizi **atabilirsiniz** .

   ![Kaydet veya at ' a tıklayın](./media/howto-configure-server-logs-portal/3-save-discard.png)

6. **Sunucu parametreleri** sayfasında **Kapat düğmesine** (X simgesine) tıklayarak Günlükler listesine geri dönün.

## <a name="view-list-and-download-logs"></a>Listeyi görüntüle ve günlükleri indir
Günlüğe kaydetme başladıktan sonra, kullanılabilir yavaş sorgu günlüklerinin bir listesini görüntüleyebilir ve tek tek günlük dosyalarını sunucu günlükleri bölmesinde indirebilirsiniz. 

1. Azure portalı açın.

2. MariaDB sunucusu için Azure veritabanınızı seçin.

3. Kenar çubuğu 'ndaki **izleme** bölümünde **sunucu günlükleri**' ni seçin. Sayfada, günlük dosyalarınızın bir listesi gösterilir.

   ![Günlüklerin listesi](./media/howto-configure-server-logs-portal/4-server-logs-list.png)

   > [!TIP]
   > Günlüğün adlandırma kuralı **MySQL-yavaş-sunucu adınızın >-yyyymmddhh. log <** . Dosya adında kullanılan tarih ve saat, günlüğün verilme zamanı olur. Günlük dosyaları her 24 saatte bir veya 7,5 GB döndürülür, hangisi önce gelir.

4. Gerekirse, tarih/saate göre belirli bir günlüğe hızlıca daraltmak için **arama kutusunu** kullanın. Arama günlüğün adı üzerinde.

5. Tabloda gösterildiği gibi, tablo satırındaki her bir günlük dosyasının yanındaki **İndir** düğmesini (aşağı ok simgesi) kullanarak ayrı günlük dosyalarını indirin:

   ![İndir simgesine tıklayın](./media/howto-configure-server-logs-portal/5-download.png)

## <a name="set-up-diagnostic-logs"></a>Tanılama günlükleri ayarlama

1. Kenar çubuğu 'ndaki **izleme** bölümünde **Tanılama ayarları**' nı seçin.

1. "+ Tanılama ayarı Ekle" seçeneğine tıklayın ![Tanılama ayarı Ekle @ no__t-1

1. Bir tanılama ayarı adı belirtin.

1. Yavaş sorgu günlüklerini (depolama hesabı, Olay Hub 'ı ve/veya Log Analytics çalışma alanı) göndermek için veri havuzlarını belirtin.

1. Günlük türü olarak "Mysqlyavaşgünlükleri" seçeneğini belirleyin.
![tanılama ayarını yapılandırma @ no__t-1

1. Yavaş sorgu günlüklerini için veri havuzlarını yapılandırdıktan sonra **Kaydet**' e tıklayabilirsiniz.
![Save Diagnostic Setting @ no__t-1

1. Bunları yapılandırdığınız veri havuzları içinde inceleyerek yavaş sorgu günlüklerine erişin. Günlüklerin görünmesi 10 dakikaya kadar sürebilir.

## <a name="next-steps"></a>Sonraki adımlar
- Yavaş sorgu günlüklerinin programlama yoluyla nasıl indirileceği hakkında bilgi edinmek için bkz. [CLI 'deki yavaş sorgu günlüklerine erişme](howto-configure-server-logs-cli.md) .
- MariaDB için Azure veritabanı 'nda [yavaş sorgu günlükleri](concepts-server-logs.md) hakkında daha fazla bilgi edinin.
- Parametre tanımları ve günlüğe kaydetme hakkında daha fazla bilgi için [günlüklere](https://mariadb.com/kb/en/library/slow-query-log-overview/)yönelik MariaDB belgelerine bakın.
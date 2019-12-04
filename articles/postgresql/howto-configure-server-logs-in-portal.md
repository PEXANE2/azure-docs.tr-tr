---
title: Günlükleri yönetme-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede, Azure portal 'den PostgreSQL için Azure veritabanı 'nda sunucu günlüklerinin (. log dosyaları) nasıl yapılandırılacağı ve erişebileceği açıklanır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 3da48a9b6d3acb1f2811bc279de7963fa1d83918
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74763700"
---
# <a name="configure-and-access-azure-database-for-postgresql---single-server-logs-from-the-azure-portal"></a>PostgreSQL için Azure veritabanı-tek sunuculu Günlükler Azure portal yapılandırma ve erişme

[PostgreSQL Için Azure veritabanı günlüklerini](concepts-server-logs.md) Azure Portal yapılandırabilir, listeleyebilir ve indirebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Bu makaledeki adımlarda [PostgreSQL Için Azure veritabanı sunucusu](quickstart-create-server-database-portal.md)gerekir.

## <a name="configure-logging"></a>Günlüğe kaydetmeyi yapılandırma
Sorgu günlüklerine ve hata günlüklerine erişimi yapılandırın. 

1. [Azure Portal](https://portal.azure.com/)’ında oturum açın.

2. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

3. Kenar çubuğu 'ndaki **izleme** bölümünde **sunucu günlükleri**' ni seçin. 

   ![Sunucu günlüğü seçeneklerinin ekran görüntüsü](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Sunucu parametrelerini görmek için, **günlükleri etkinleştirmek ve günlük parametrelerini yapılandırmak için buraya tıklayın ' ı**seçin.

5. Ayarlamanız gereken parametreleri değiştirin. Bu oturumda yaptığınız tüm değişiklikler mor renkle vurgulanır.

   Parametreleri değiştirdikten sonra **Kaydet**' i seçin. Ya da değişikliklerinizi atabilirsiniz. 

   ![Sunucu parametreleri seçeneklerinin ekran görüntüsü](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

**Sunucu parametreleri** sayfasında, sayfayı kapatarak Günlükler listesine geri dönebilirsiniz.

## <a name="view-list-and-download-logs"></a>Listeyi görüntüle ve günlükleri indir
Günlüğe kaydetme başladıktan sonra, kullanılabilir günlüklerin bir listesini görüntüleyebilir ve günlük dosyalarını tek tek indirebilirsiniz. 

1. Azure portalı açın.

2. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

3. Kenar çubuğu 'ndaki **izleme** bölümünde **sunucu günlükleri**' ni seçin. Sayfa, günlük dosyalarınızın bir listesini gösterir.

   ![Günlük listesi vurgulanmış şekilde sunucu günlükleri sayfasının ekran görüntüsü](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > Günlüğün adlandırma kuralı **PostgreSQL-yyyy-aa-dd_hh0000. log**' dır. Dosya adında kullanılan tarih ve saat, günlüğün verildiği zaman. Günlük dosyaları her saat veya 100 MB döndürülür, hangisi önce gelir.

4. Gerekirse, tarih ve saate göre belirli bir günlüğe hızlıca daraltmak için arama kutusunu kullanın. Arama günlüğün adı üzerinde.

   ![Arama kutusuyla sonuçları vurgulanmış şekilde sunucu günlükleri sayfasının ekran görüntüsü](./media/howto-configure-server-logs-in-portal/5-search.png)

5. Ayrı günlük dosyalarını indirmek için tablo satırındaki her bir günlük dosyasının yanındaki aşağı ok simgesini seçin.

   ![Aşağı ok simgesi vurgulanmış şekilde sunucu günlükleri sayfasının ekran görüntüsü](./media/howto-configure-server-logs-in-portal/6-download.png)

## <a name="next-steps"></a>Sonraki adımlar
- Günlüklerin programlı olarak nasıl indirileceği hakkında bilgi edinmek için bkz. [CLI 'daki sunucu günlüklerine erişim](howto-configure-server-logs-using-cli.md) .
- PostgreSQL için Azure veritabanı 'nda [sunucu günlükleri](concepts-server-logs.md) hakkında daha fazla bilgi edinin. 
- Parametre tanımları ve PostgreSQL günlüğü hakkında daha fazla bilgi için bkz. [hata raporlama ve günlüğe kaydetme](https://www.postgresql.org/docs/current/static/runtime-config-logging.html)hakkında PostgreSQL belgeleri.


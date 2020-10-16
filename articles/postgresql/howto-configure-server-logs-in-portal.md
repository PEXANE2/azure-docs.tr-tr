---
title: Günlükleri yönetme-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede, Azure portal 'den PostgreSQL için Azure veritabanı 'nda sunucu günlüklerinin (. log dosyaları) nasıl yapılandırılacağı ve erişebileceği açıklanır.
author: lfittl-msft
ms.author: lufittl
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.openlocfilehash: 3b52cea1d440506caf5b8244c9643719edd8755c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91704274"
---
# <a name="configure-and-access-azure-database-for-postgresql---single-server-logs-from-the-azure-portal"></a>PostgreSQL için Azure veritabanı-tek sunuculu Günlükler Azure portal yapılandırma ve erişme

[PostgreSQL Için Azure veritabanı günlüklerini](concepts-server-logs.md) Azure Portal yapılandırabilir, listeleyebilir ve indirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu makaledeki adımlarda [PostgreSQL Için Azure veritabanı sunucusu](quickstart-create-server-database-portal.md)gerekir.

## <a name="configure-logging"></a>Günlüğe kaydetmeyi yapılandırma
Sorgu günlüklerine ve hata günlüklerine erişimi yapılandırın. 

1. [Azure portalında](https://portal.azure.com/) oturum açın.

2. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

3. Kenar çubuğu 'ndaki **izleme** bölümünde **sunucu günlükleri**' ni seçin. 

   :::image type="content" source="./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png" alt-text="Sunucu günlüğü seçeneklerinin ekran görüntüsü":::

4. Sunucu parametrelerini görmek için, **günlükleri etkinleştirmek ve günlük parametrelerini yapılandırmak için buraya tıklayın ' ı**seçin.

5. Ayarlamanız gereken parametreleri değiştirin. Bu oturumda yaptığınız tüm değişiklikler mor renkle vurgulanır.

   Parametreleri değiştirdikten sonra **Kaydet**' i seçin. Ya da değişikliklerinizi atabilirsiniz. 

   :::image type="content" source="./media/howto-configure-server-logs-in-portal/3-save-discard.png" alt-text="Sunucu günlüğü seçeneklerinin ekran görüntüsü":::

**Sunucu parametreleri** sayfasında, sayfayı kapatarak Günlükler listesine geri dönebilirsiniz.

## <a name="view-list-and-download-logs"></a>Listeyi görüntüle ve günlükleri indir
Günlüğe kaydetme başladıktan sonra, kullanılabilir günlüklerin bir listesini görüntüleyebilir ve günlük dosyalarını tek tek indirebilirsiniz. 

1. Azure portalını açın.

2. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

3. Kenar çubuğu 'ndaki **izleme** bölümünde **sunucu günlükleri**' ni seçin. Sayfa, günlük dosyalarınızın bir listesini gösterir.

   :::image type="content" source="./media/howto-configure-server-logs-in-portal/4-server-logs-list.png" alt-text="Sunucu günlüğü seçeneklerinin ekran görüntüsü":::

   > [!TIP]
   > Günlüğün adlandırma kuralı **PostgreSQL-yyyy-aa-dd_hh0000. log**' dır. Dosya adında kullanılan tarih ve saat, günlüğün verildiği zaman. Günlük dosyaları her saat veya 100 MB döndürülür, hangisi önce gelir.

4. Gerekirse, tarih ve saate göre belirli bir günlüğe hızlıca daraltmak için arama kutusunu kullanın. Arama günlüğün adı üzerinde.

   :::image type="content" source="./media/howto-configure-server-logs-in-portal/5-search.png" alt-text="Sunucu günlüğü seçeneklerinin ekran görüntüsü":::

5. Ayrı günlük dosyalarını indirmek için tablo satırındaki her bir günlük dosyasının yanındaki aşağı ok simgesini seçin.

   :::image type="content" source="./media/howto-configure-server-logs-in-portal/6-download.png" alt-text="Sunucu günlüğü seçeneklerinin ekran görüntüsü":::

## <a name="next-steps"></a>Sonraki adımlar
- Günlüklerin programlı olarak nasıl indirileceği hakkında bilgi edinmek için bkz. [CLI 'daki sunucu günlüklerine erişim](howto-configure-server-logs-using-cli.md) .
- PostgreSQL için Azure veritabanı 'nda [sunucu günlükleri](concepts-server-logs.md) hakkında daha fazla bilgi edinin. 
- Parametre tanımları ve PostgreSQL günlüğü hakkında daha fazla bilgi için bkz. [hata raporlama ve günlüğe kaydetme](https://www.postgresql.org/docs/current/static/runtime-config-logging.html)hakkında PostgreSQL belgeleri.


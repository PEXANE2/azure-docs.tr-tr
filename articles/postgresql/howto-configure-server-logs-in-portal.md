---
title: Günlükleri yönetme - Azure portalı - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, Azure portalından PostgreSQL - Single Server için Azure Veritabanı'ndaki sunucu günlüklerinin (.log dosyaları) nasıl yapılandırılabildiğini ve bunlara erişilenler açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 3da48a9b6d3acb1f2811bc279de7963fa1d83918
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74763700"
---
# <a name="configure-and-access-azure-database-for-postgresql---single-server-logs-from-the-azure-portal"></a>PostgreSQL için Azure Veritabanını yapılandırma ve erişim - Azure portalından Tek Sunucu günlükleri

Azure portalından [PostgreSQL günlükleri için Azure Veritabanını](concepts-server-logs.md) yapılandırabilir, listeleyebilir ve indirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu makaledeki adımlar, [PostgreSQL sunucusu için Azure Veritabanı'na](quickstart-create-server-database-portal.md)sahip olduğunuzu gerektirir.

## <a name="configure-logging"></a>Günlük işlemlerini yapılandırma
Sorgu günlüklerine ve hata günlüklerine erişimi yapılandırın. 

1. [Azure portalında](https://portal.azure.com/)oturum açın.

2. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

3. Kenar çubuğundaki **İzleme** bölümü altında **Sunucu günlüklerini**seçin. 

   ![Sunucu günlükleri seçeneklerinin ekran görüntüsü](./media/howto-configure-server-logs-in-portal/1-select-server-logs-configure.png)

4. Sunucu parametrelerini görmek **için, günlükleri etkinleştirmek ve günlük parametrelerini yapılandırmak için buraya tıklayın'ı**seçin.

5. Ayarlamanız gereken parametreleri değiştirin. Bu oturumda yaptığınız tüm değişiklikler mor renkte vurgulanır.

   Parametreleri değiştirdikten sonra **Kaydet'i**seçin. Veya değişikliklerinizi atabilirsiniz. 

   ![Sunucu Parametreleri seçeneklerinin ekran görüntüsü](./media/howto-configure-server-logs-in-portal/3-save-discard.png)

Sunucu **Parametreleri** sayfasından, sayfayı kapatarak günlükler listesine dönebilirsiniz.

## <a name="view-list-and-download-logs"></a>Listeyi görüntüleme ve indirme günlüklerini
Günlüğe kaydetme başladıktan sonra, kullanılabilir günlüklerin listesini görüntüleyebilir ve tek tek günlük dosyalarını indirebilirsiniz. 

1. Azure portalı açın.

2. PostgreSQL için Azure Veritabanı sunucunuzu seçin.

3. Kenar çubuğundaki **İzleme** bölümü altında **Sunucu günlüklerini**seçin. Sayfada günlük dosyalarınızın bir listesi gösterilmektedir.

   ![Sunucu günlükleri sayfasının ekran görüntüsü, günlüklerin listesi vurgulanmış](./media/howto-configure-server-logs-in-portal/4-server-logs-list.png)

   > [!TIP]
   > Günlüğün adlandırma kuralı **postgresql-yyyy-mm-dd_hh0000.log'dur.** Dosya adında kullanılan tarih ve saat, günlüğün verildiği zamandır. Günlük dosyaları her saat veya 100 MB(hangisi önce gelirse) döner.

4. Gerekirse, tarih ve saate bağlı olarak belirli bir günlüğe hızla daraltmak için arama kutusunu kullanın. Arama günlüğün adı üzerinde.

   ![Arama kutusu ve sonuçları vurgulanmış Sunucu günlükleri sayfasının ekran görüntüsü](./media/howto-configure-server-logs-in-portal/5-search.png)

5. Tek tek günlük dosyalarını indirmek için, tablo satırındaki her günlük dosyasının yanındaki aşağı ok simgesini seçin.

   ![Aşağı ok simgesi vurgulanmış sunucu günlükleri sayfasının ekran görüntüsü](./media/howto-configure-server-logs-in-portal/6-download.png)

## <a name="next-steps"></a>Sonraki adımlar
- Günlükleri programlı olarak nasıl karşıdan yükleyin öğrenmek için [CLI'deki Access sunucusu günlüklerine](howto-configure-server-logs-using-cli.md) bakın.
- PostgreSQL için Azure Veritabanı'ndaki [sunucu günlükleri](concepts-server-logs.md) hakkında daha fazla bilgi edinin. 
- Parametre tanımları ve PostgreSQL günlüğü hakkında daha fazla bilgi için [hata raporlama ve günlük leme](https://www.postgresql.org/docs/current/static/runtime-config-logging.html)hakkındaki PostgreSQL belgelerine bakın.


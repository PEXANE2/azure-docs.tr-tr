---
title: Sunucu parametrelerini yapılandırma - Azure portalı - MariaDB için Azure Veritabanı
description: Bu makalede, Azure portalını kullanarak MariaDB için Azure Veritabanı'ndaki MariaDB sunucu parametrelerinin nasıl yapılandırılabildiğini açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 4/1/2020
ms.openlocfilehash: 9d057a4be02d8d93d3ef02ee3153baebe9146ff1
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632704"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mariadb-by-using-the-azure-portal"></a>Azure portalını kullanarak MariaDB için Azure Veritabanı'ndaki sunucu parametrelerini yapılandırma

MariaDB için Azure Veritabanı, bazı sunucu parametrelerinin yapılandırmayı destekler. Bu makalede, Azure portalını kullanarak bu parametrelerin nasıl yapılandırılabildiğini açıklanmaktadır. Tüm sunucu parametreleri ayarlanabilir.

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Azure portalında Sunucu Parametrelerine gidin

1. Azure portalında oturum açın ve ardından MariaDB sunucusu için Azure Veritabanınızı bulun.
2. **AYARLAR** bölümünde, MariaDB sunucusu için Azure Veritabanı için sunucu parametreleri sayfasını açmak için **Sunucu parametrelerini** tıklatın.
![Azure portal sunucu parametreleri sayfası](./media/howto-server-parameters/azure-portal-server-parameters.png)
3. Ayarlamanız gereken ayarları bulun. Amacı ve izin verilen değerleri anlamak için **Açıklama** sütununa gözden geçirin.
![Sayısal bırakma](./media/howto-server-parameters/3-toggle_parameter.png)
4. Değişikliklerinizi kaydetmek için **Kaydet'i** tıklatın.
![Değişiklikleri Kaydetme veya Atma](./media/howto-server-parameters/4-save_parameters.png)
5. Parametreler için yeni değerler kaydettiyseniz, **tümünü varsayılana sıfırla'yı**seçerek her şeyi varsayılan değerlere geri döndürebilirsiniz.
![Tümlerini varsayılan olarak sıfırla](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Yapılandırılabilir sunucu parametreleri listesi

Desteklenen sunucu parametreleri listesi sürekli büyüyor. Tanımı almak ve sunucu parametrelerini uygulama gereksinimlerinize göre yapılandırmak için Azure portalındaki sunucu parametreleri sekmesini kullanın.

## <a name="non-configurable-server-parameters"></a>Yapılandırılamaz sunucu parametreleri

InnoDB Arabellek Havuzu ve Max Connections yapılandırılamaz ve [fiyatlandırma katmanınıza](concepts-pricing-tiers.md)bağlıdır.

|**Fiyatlandırma Katmanı**| **vCore(lar)**|**InnoDB Tampon Havuzu (MB)**|
|---|---|---|
|Temel| 1| 1024|
|Temel| 2| 2560|
|Genel Amaçlı| 2| 3584|
|Genel Amaçlı| 4| 7680|
|Genel Amaçlı| 8| 15360|
|Genel Amaçlı| 16| 31232|
|Genel Amaçlı| 32| 62976|
|Genel Amaçlı| 64| 125952|
|Bellek İçin İyileştirilmiş| 2| 7168|
|Bellek İçin İyileştirilmiş| 4| 15360|
|Bellek İçin İyileştirilmiş| 8| 30720|
|Bellek İçin İyileştirilmiş| 16| 62464|
|Bellek İçin İyileştirilmiş| 32| 125952|

Bu ek sunucu parametreleri sistemde yapılandırılamaz:

|**Parametre**|**Sabit değer**|
| :------------------------ | :-------- |
|Temel katmanda innodb_file_per_table|KAPALI|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512MB|

Burada listelenmemiş diğer sunucu [parametreleri, MariaDB](https://mariadb.com/kb/en/library/xtradbinnodb-server-system-variables/)için mariadb out-of-box varsayılan değerleri ayarlanır.

## <a name="working-with-the-time-zone-parameter"></a>Saat dilimi parametresi ile çalışma

### <a name="populating-the-time-zone-tables"></a>Saat dilimi tablolarının doldurulma

Sunucunuzdaki saat dilimi tabloları, `mysql.az_load_timezone` mysql komut satırı veya MySQL Workbench gibi bir araçtan depolanan yordamı arayarak doldurulabilir.

> [!NOTE]
> MySQL Workbench `mysql.az_load_timezone` komutunu çalıştırıyorsanız, önce güvenli güncelleştirme modunu `SET SQL_SAFE_UPDATES=0;`kapatmanız gerekebilir.

```sql
CALL mysql.az_load_timezone();
```

> [!IMPORTANT]
> Saat dilimi tablolarının düzgün dolduruldığından emin olmak için sunucuyu yeniden başlatmanız gerekir. Sunucuyu yeniden başlatmak için [Azure portalını](howto-restart-server-portal.md) veya [CLI'yi](howto-restart-server-cli.md)kullanın.
Kullanılabilir saat dilimi değerlerini görüntülemek için aşağıdaki komutu çalıştırın:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Genel düzey saat dilimini ayarlama

Genel düzey saat dilimi Azure portalındaki **Sunucu parametreleri** sayfasından ayarlanabilir. Aşağıdaki küresel saat dilimini "ABD/Pasifik" değerine ayarlar.

![Saat dilimi parametreyi ayarlama](./media/howto-server-parameters/timezone.png)

### <a name="setting-the-session-level-time-zone"></a>Oturum düzeyi saat dilimini ayarlama

Oturum düzeyi saat dilimi MySQL `SET time_zone` komut satırı veya MySQL Workbench gibi bir araçtan komut çalıştırılarak ayarlanabilir. Aşağıdaki örnekte saat dilimiNI **ABD/Pasifik** saat dilimi olarak ayarlar.

```sql
SET time_zone = 'US/Pacific';
```

[Tarih ve Saat Fonksiyonları](https://mariadb.com/kb/en/library/convert_tz/)için MariaDB belgelerine bakın.

<!--
## Next steps

- [Connection libraries for Azure Database for MariaDB](concepts-connection-libraries.md).
-->

---
title: Sunucu parametrelerini yapılandırma - Azure portalı - MySQL için Azure Veritabanı
description: Bu makalede, Azure portalını kullanarak MySQL için Azure Veritabanı'ndaki MySQL sunucu parametrelerinin nasıl yapılandırılabildiğini açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 4/1/2020
ms.openlocfilehash: 715f1028597d76915d833b0ade66bc03d939030d
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546439"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Azure portalını kullanarak MySQL için Azure Veritabanı'ndaki sunucu parametrelerini yapılandırma

MySQL için Azure Veritabanı, bazı sunucu parametrelerinin yapılandırmayı destekler. Bu makalede, Azure portalını kullanarak bu parametrelerin nasıl yapılandırılabildiğini açıklanmaktadır. Tüm sunucu parametreleri ayarlanabilir.

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Azure portalında Sunucu Parametrelerine gidin

1. Azure portalında oturum açın ve MySQL sunucusu için Azure Veritabanınızı bulun.
2. **AYARLAR** bölümünde, MySQL sunucusu için Azure Veritabanı için sunucu parametreleri sayfasını açmak için **Sunucu parametrelerini** tıklatın.
![Azure portal sunucu parametreleri sayfası](./media/howto-server-parameters/auzre-portal-server-parameters.png)
3. Ayarlamanız gereken ayarları bulun. Amacı ve izin verilen değerleri anlamak için **Açıklama** sütununa gözden geçirin.
![Sayısal bırakma](./media/howto-server-parameters/3-toggle_parameter.png)
4. Değişikliklerinizi kaydetmek için **Kaydet'i** tıklatın.
![Değişiklikleri Kaydetme veya Atma](./media/howto-server-parameters/4-save_parameters.png)
5. Parametreler için yeni değerler kaydettiyseniz, **tümünü varsayılana sıfırla'yı**seçerek her şeyi varsayılan değerlere geri döndürebilirsiniz.
![Tümlerini varsayılan olarak sıfırla](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Yapılandırılabilir sunucu parametreleri listesi

Desteklenen sunucu parametreleri listesi sürekli büyüyor. Tanımı almak ve sunucu parametrelerini uygulama gereksinimlerinize göre yapılandırmak için Azure portalındaki sunucu parametreleri sekmesini kullanın.

## <a name="non-configurable-server-parameters"></a>Yapılandırılamaz sunucu parametreleri

InnoDB Arabellek Havuzu boyutu yapılandırılamaz ve [fiyatlandırma katmanınıza](concepts-service-tiers.md)bağlıdır.

|**Fiyatlandırma Katmanı**|**vCore(lar)**|**MB'de <br>InnoDB Arabellek Havuzu boyutu (4 TB'a kadar depolama yı destekleyen sunucular)**| **MB'de <br>InnoDB Arabellek Havuzu boyutu (16 TB'a kadar depolama alanını destekleyen sunucular)**|
|:---|---:|---:|---:|
|Temel| 1| 832| |
|Temel| 2| 2560| |
|Genel Amaçlı| 2| 3584| 7168|
|Genel Amaçlı| 4| 7680| 15360|
|Genel Amaçlı| 8| 15360| 30720|
|Genel Amaçlı| 16| 31232| 62464|
|Genel Amaçlı| 32| 62976| 125952|
|Genel Amaçlı| 64| 125952| 251904|
|Bellek İçin İyileştirilmiş| 2| 7168| 14336|
|Bellek İçin İyileştirilmiş| 4| 15360| 30720|
|Bellek İçin İyileştirilmiş| 8| 30720| 61440|
|Bellek İçin İyileştirilmiş| 16| 62464| 124928|
|Bellek İçin İyileştirilmiş| 32| 125952| 251904|

Bu ek sunucu parametreleri sistemde yapılandırılamaz:

|**Parametre**|**Sabit değer**|
| :------------------------ | :-------- |
|Temel katmanda innodb_file_per_table|KAPALI|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512MB|

Burada listelenmemiş diğer sunucu [parametreleri, 5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) ve [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html)sürümleri için MySQL out-of-box varsayılan değerlerine ayarlanır.

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

[Tarih ve Saat Fonksiyonları](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_convert-tz)için MySQL belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [MySQL için Azure Veritabanı için bağlantı kitaplıkları.](concepts-connection-libraries.md)

---
title: Sunucu parametrelerini Yapılandırma-Azure portal-MySQL için Azure veritabanı
description: Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı 'nda MySQL Server parametrelerinin nasıl yapılandırılacağı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: f803b7cccf3520c309e6b33d99b5565cfc4fdd01
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74764925"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı 'nda sunucu parametrelerini yapılandırma

MySQL için Azure veritabanı bazı sunucu parametrelerinin yapılandırılmasını destekler. Bu makalede Azure portal kullanılarak bu parametrelerin nasıl yapılandırılacağı açıklanır. Tüm sunucu parametreleri ayarlanamaz.

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Azure portal sunucu parametrelerine git

1. Azure portal oturum açın ve ardından MySQL Server için Azure veritabanınızı bulun.
2. **Ayarlar** bölümünde **sunucu parametreleri** ' ne tıklayarak MySQL için Azure veritabanı sunucusu için sunucu parametreleri sayfasını açın.
![Azure portal sunucu parametreleri sayfası](./media/howto-server-parameters/auzre-portal-server-parameters.png)
3. Ayarlamanız gereken ayarları bulun. Amacı ve izin verilen değerleri anlamak için **Açıklama** sütununu gözden geçirin.
açılan ![](./media/howto-server-parameters/3-toggle_parameter.png) Sırala
4. Değişikliklerinizi kaydetmek için **Kaydet** ' e tıklayın.
değişiklikleri kaydetme veya atma ![](./media/howto-server-parameters/4-save_parameters.png)
5. Parametreler için yeni değerler kaydettiyseniz, **Tümünü Sıfırla**' yı seçerek her şeyi varsayılan değerlere geri döndürebilirsiniz.
![tümünü Varsayılana sıfırla](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Yapılandırılabilir sunucu parametrelerinin listesi

Desteklenen sunucu parametrelerinin listesi sürekli olarak büyüyordur. Tanım almak ve uygulama gereksinimlerinize göre sunucu parametrelerini yapılandırmak için Azure portal içindeki sunucu parametreleri sekmesini kullanın.

## <a name="non-configurable-server-parameters"></a>Yapılandırılamayan sunucu parametreleri

InnoDB arabellek havuzu boyutu yapılandırılamaz ve [fiyatlandırma katmanınıza](concepts-service-tiers.md)bağlı değildir.

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**MB <br>(en fazla 4 TB depolama alanı destekleyen sunucular) InnoDB arabellek havuzu boyutu**| **MB <br>InnoDB arabellek havuzu boyutu (16 TB 'a kadar depolama alanı destekleyen sunucular)**|
|:---|---:|---:|---:|
|Temel| 1| 832| |
|Temel| 2| 2560| |
|Genel Amaçlı| 2| 3584| 7168|
|Genel Amaçlı| 4| 7680| 15360|
|Genel Amaçlı| 8| 15360| 30720|
|Genel Amaçlı| 16| 31232| 62464|
|Genel Amaçlı| 32| 62976| 125952|
|Genel Amaçlı| 64| 125952| 251904|
|Bellek için İyileştirilmiş| 2| 7168| 14336|
|Bellek için İyileştirilmiş| 4| 15360| 30720|
|Bellek için İyileştirilmiş| 8| 30720| 61440|
|Bellek için İyileştirilmiş| 16| 62464| 124928|
|Bellek için İyileştirilmiş| 32| 125952| 251904|

Bu ek sunucu parametreleri sistemde yapılandırılamaz:

|**Parametresinin**|**Sabit değer**|
| :------------------------ | :-------- |
|Temel katmanda innodb_file_per_table|KAPALI|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512 MB|

Burada listelenmeyen diğer sunucu parametreleri, [5,7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) ve [5,6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html)sürümleri için MySQL varsayılan değerlerine ayarlanır.

## <a name="working-with-the-time-zone-parameter"></a>Saat dilimi parametresiyle çalışma

### <a name="populating-the-time-zone-tables"></a>Saat dilimi tablolarını doldurma

Sunucunuzdaki saat dilimi tabloları, MySQL komut satırı veya MySQL çalışma ekranı gibi bir araçtan `az_load_timezone` saklı yordam çağırarak doldurulabilir.

> [!NOTE]
> MySQL çalışma modundan `az_load_timezone` komutunu çalıştırıyorsanız, önce `SET SQL_SAFE_UPDATES=0;`kullanarak güvenli güncelleştirme modunu kapatmanız gerekebilir.

```sql
CALL mysql.az_load_timezone();
```

Kullanılabilir saat dilimi değerlerini görüntülemek için aşağıdaki komutu çalıştırın:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Genel düzey saat dilimini ayarlama

Genel düzey saat dilimi Azure portal **sunucu parametreleri** sayfasından ayarlanabilir. Aşağıdaki, genel saat dilimini "ABD/Pasifik" değerine ayarlar.

![Saat dilimi parametresini ayarla](./media/howto-server-parameters/timezone.png)

### <a name="setting-the-session-level-time-zone"></a>Oturum düzeyi saat dilimini ayarlama

Oturum düzeyi saat dilimi, MySQL komut satırı veya MySQL çalışma ekranı gibi bir araçla `SET time_zone` komutu çalıştırılarak ayarlanabilir. Aşağıdaki örnek saat dilimini **ABD/Pasifik** saati dilimine göre ayarlar.

```sql
SET time_zone = 'US/Pacific';
```

[Tarih ve saat işlevleri](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_convert-tz)için MySQL belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [MySQL Için Azure veritabanı bağlantı kitaplıkları](concepts-connection-libraries.md).

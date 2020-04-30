---
title: Sunucu parametrelerini Yapılandırma-Azure portal-MySQL için Azure veritabanı
description: Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı 'nda MySQL Server parametrelerinin nasıl yapılandırılacağı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 4/16/2020
ms.openlocfilehash: bd0a867cce9b2a9ad793b491b9042034ef5810f5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81605149"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı 'nda sunucu parametrelerini yapılandırma

MySQL için Azure veritabanı bazı sunucu parametrelerinin yapılandırılmasını destekler. Bu makalede Azure portal kullanılarak bu parametrelerin nasıl yapılandırılacağı açıklanır. Tüm sunucu parametreleri ayarlanamaz.

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Azure portal sunucu parametrelerine git

1. Azure portal oturum açın ve ardından MySQL Server için Azure veritabanınızı bulun.
2. **Ayarlar** bölümünde **sunucu parametreleri** ' ne tıklayarak MySQL için Azure veritabanı sunucusu için sunucu parametreleri sayfasını açın.
![Azure portal sunucu parametreleri sayfası](./media/howto-server-parameters/auzre-portal-server-parameters.png)
3. Ayarlamanız gereken ayarları bulun. Amacı ve izin verilen değerleri anlamak için **Açıklama** sütununu gözden geçirin.
![Açılan listesini numaralandır](./media/howto-server-parameters/3-toggle_parameter.png)
4. Değişikliklerinizi kaydetmek için **Kaydet** ' e tıklayın.
![Değişiklikleri kaydetme veya atma](./media/howto-server-parameters/4-save_parameters.png)
5. Parametreler için yeni değerler kaydettiyseniz, **Tümünü Sıfırla**' yı seçerek her şeyi varsayılan değerlere geri döndürebilirsiniz.
![Tümünü Varsayılana sıfırla](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Yapılandırılabilir sunucu parametrelerinin listesi

Desteklenen sunucu parametrelerinin listesi sürekli olarak büyüyordur. Tanım almak ve uygulama gereksinimlerinize göre sunucu parametrelerini yapılandırmak için Azure portal içindeki sunucu parametreleri sekmesini kullanın.

## <a name="non-configurable-server-parameters"></a>Yapılandırılamayan sunucu parametreleri

InnoDB arabellek havuzu boyutu yapılandırılamaz ve [fiyatlandırma katmanınıza](concepts-service-tiers.md)bağlı değildir.

|**Fiyatlandırma Katmanı**|**Sanal çekirdek**|**MB <br>cinsinden InnoDB arabellek havuzu boyutu (4 TB 'a kadar depolama alanı destekleyen sunucular)**| **MB <br>cinsinden InnoDB arabellek havuzu boyutu (16 TB 'a kadar depolama alanı destekleyen sunucular)**|
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
|innodb_log_file_size|6/E|
|innodb_log_files_in_group|2|

Burada listelenmeyen diğer sunucu parametreleri, [5,7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) ve [5,6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html)sürümleri için MySQL varsayılan değerlerine ayarlanır.

## <a name="working-with-the-time-zone-parameter"></a>Saat dilimi parametresiyle çalışma

### <a name="populating-the-time-zone-tables"></a>Saat dilimi tablolarını doldurma

Sunucunuzdaki saat dilimi tabloları, MySQL komut satırı veya MySQL çalışma ekranı gibi `mysql.az_load_timezone` bir araçtan saklı yordam çağırarak doldurulabilir.

> [!NOTE]
> MySQL çalışma modundan `mysql.az_load_timezone` komutunu çalıştırıyorsanız, önce kullanarak `SET SQL_SAFE_UPDATES=0;`güvenli güncelleştirme modunu kapatmanız gerekebilir.

```sql
CALL mysql.az_load_timezone();
```

> [!IMPORTANT]
> Saat dilimi tablolarının doğru doldurulduğundan emin olmak için sunucuyu yeniden başlatmanız gerekir. Sunucuyu yeniden başlatmak için [Azure Portal](howto-restart-server-portal.md) veya [CLI](howto-restart-server-cli.md)kullanın.

Kullanılabilir saat dilimi değerlerini görüntülemek için aşağıdaki komutu çalıştırın:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Genel düzey saat dilimini ayarlama

Genel düzey saat dilimi Azure portal **sunucu parametreleri** sayfasından ayarlanabilir. Aşağıdaki, genel saat dilimini "ABD/Pasifik" değerine ayarlar.

![Saat dilimi parametresini ayarla](./media/howto-server-parameters/timezone.png)

### <a name="setting-the-session-level-time-zone"></a>Oturum düzeyi saat dilimini ayarlama

Oturum düzeyi saat dilimi, MySQL komut satırı veya MySQL çalışma `SET time_zone` ekranı gibi bir araçtan komutu çalıştırılarak ayarlanabilir. Aşağıdaki örnek saat dilimini **ABD/Pasifik** saati dilimine göre ayarlar.

```sql
SET time_zone = 'US/Pacific';
```

[Tarih ve saat işlevleri](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_convert-tz)için MySQL belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [MySQL Için Azure veritabanı bağlantı kitaplıkları](concepts-connection-libraries.md).

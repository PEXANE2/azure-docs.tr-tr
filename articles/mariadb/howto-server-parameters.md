---
title: Sunucu parametrelerini Yapılandırma-Azure portal-MariaDB için Azure veritabanı
description: Bu makalede, Azure portal kullanılarak MariaDB için Azure veritabanı 'nda MariaDB sunucu parametrelerinin nasıl yapılandırılacağı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 4/16/2020
ms.openlocfilehash: f39e9450fb922e5b93d7b4b809df73cf5ab007c1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81602400"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mariadb-by-using-the-azure-portal"></a>Azure portal kullanarak, MariaDB için Azure veritabanı 'nda sunucu parametrelerini yapılandırma

MariaDB için Azure veritabanı bazı sunucu parametrelerinin yapılandırılmasını destekler. Bu makalede Azure portal kullanılarak bu parametrelerin nasıl yapılandırılacağı açıklanır. Tüm sunucu parametreleri ayarlanamaz.

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Azure portal sunucu parametrelerine git

1. Azure portal oturum açın ve ardından MariaDB sunucusu için Azure veritabanınızı bulun.
2. **Ayarlar** bölümünde **sunucu parametreleri** ' ne tıklayarak MariaDB sunucusu için Azure veritabanı için sunucu parametreleri sayfasını açın.
![Azure portal sunucu parametreleri sayfası](./media/howto-server-parameters/azure-portal-server-parameters.png)
3. Ayarlamanız gereken ayarları bulun. Amacı ve izin verilen değerleri anlamak için **Açıklama** sütununu gözden geçirin.
![Açılan listesini numaralandır](./media/howto-server-parameters/3-toggle_parameter.png)
4. Değişikliklerinizi kaydetmek için **Kaydet** ' e tıklayın.
![Değişiklikleri kaydetme veya atma](./media/howto-server-parameters/4-save_parameters.png)
5. Parametreler için yeni değerler kaydettiyseniz, **Tümünü Sıfırla**' yı seçerek her şeyi varsayılan değerlere geri döndürebilirsiniz.
![Tümünü Varsayılana sıfırla](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Yapılandırılabilir sunucu parametrelerinin listesi

Desteklenen sunucu parametrelerinin listesi sürekli olarak büyüyordur. Tanım almak ve uygulama gereksinimlerinize göre sunucu parametrelerini yapılandırmak için Azure portal içindeki sunucu parametreleri sekmesini kullanın.

## <a name="non-configurable-server-parameters"></a>Yapılandırılamayan sunucu parametreleri

InnoDB arabellek havuzu ve maksimum bağlantı yapılandırılabilir değildir ve [fiyatlandırma katmanınıza](concepts-pricing-tiers.md)bağlı değildir.

|**Fiyatlandırma Katmanı**| **Sanal çekirdek**|**InnoDB arabellek havuzu (MB)**|
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
|innodb_log_file_size|6/E|
|innodb_log_files_in_group|2|

Burada listelenmeyen diğer sunucu parametreleri, [MariaDB](https://mariadb.com/kb/en/library/xtradbinnodb-server-system-variables/)'nin varsayılan dışı varsayılan değerlerine ayarlanır.

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

[Tarih ve saat işlevleri](https://mariadb.com/kb/en/library/convert_tz/)Için MariaDB belgelerine bakın.

<!--
## Next steps

- [Connection libraries for Azure Database for MariaDB](concepts-connection-libraries.md).
-->

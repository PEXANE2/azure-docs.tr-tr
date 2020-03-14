---
title: Sunucu parametrelerini Yapılandırma-Azure portal-MariaDB için Azure veritabanı
description: Bu makalede, Azure portal kullanılarak MariaDB için Azure veritabanı 'nda MariaDB sunucu parametrelerinin nasıl yapılandırılacağı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/9/2019
ms.openlocfilehash: ba091d05aa243fab08138c96827d2f657d9755de
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79251511"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mariadb-by-using-the-azure-portal"></a>Azure portal kullanarak, MariaDB için Azure veritabanı 'nda sunucu parametrelerini yapılandırma

MariaDB için Azure veritabanı bazı sunucu parametrelerinin yapılandırılmasını destekler. Bu makalede Azure portalını kullanarak bu parametreleri yapılandırma açıklanır. Tüm sunucu parametreleri ayarlanabilir.

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Azure portalında sunucu parametrelerini gidin

1. Azure portal oturum açın ve ardından MariaDB sunucusu için Azure veritabanınızı bulun.
2. **Ayarlar** bölümünde **sunucu parametreleri** ' ne tıklayarak MariaDB sunucusu için Azure veritabanı için sunucu parametreleri sayfasını açın.
![Azure portal sunucu parametreleri sayfası](./media/howto-server-parameters/azure-portal-server-parameters.png)
3. Ayarlamak için gereken herhangi bir ayarı bulun. Amacı ve izin verilen değerleri anlamak için **Açıklama** sütununu gözden geçirin.
açılan ![](./media/howto-server-parameters/3-toggle_parameter.png) Sırala
4. Değişikliklerinizi kaydetmek için **Kaydet** ' e tıklayın.
değişiklikleri kaydetme veya atma ![](./media/howto-server-parameters/4-save_parameters.png)
5. Parametreler için yeni değerler kaydettiyseniz, **Tümünü Sıfırla**' yı seçerek her şeyi varsayılan değerlere geri döndürebilirsiniz.
![tümünü Varsayılana sıfırla](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Yapılandırılabilir sunucu parametrelerinin listesi

Desteklenen sunucu parametrelerinin listesi sürekli olarak artmaktadır. Sunucu parametreleri sekmesi tanımını Al ve uygulama gereksinimlerinize göre sunucu parametrelerini yapılandırma Azure portalında kullanın.

## <a name="non-configurable-server-parameters"></a>Yapılandırılabilir olmayan sunucu parametreleri

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
|Bellek için İyileştirilmiş| 2| 7168|
|Bellek için İyileştirilmiş| 4| 15360|
|Bellek için İyileştirilmiş| 8| 30720|
|Bellek için İyileştirilmiş| 16| 62464|
|Bellek için İyileştirilmiş| 32| 125952|

Bu ek sunucu parametreleri sistemde yapılandırılabilir değildir:

|**Parametresinin**|**Sabit değer**|
| :------------------------ | :-------- |
|Temel katmanda innodb_file_per_table|KAPALI|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512 MB|

Burada listelenmeyen diğer sunucu parametreleri, [MariaDB](https://mariadb.com/kb/en/library/xtradbinnodb-server-system-variables/)'nin varsayılan dışı varsayılan değerlerine ayarlanır.

## <a name="working-with-the-time-zone-parameter"></a>Saat dilimi parametresi ile çalışma

### <a name="populating-the-time-zone-tables"></a>Saat dilimi tablolarını doldurma

Sunucunuzdaki saat dilimi tabloları, MySQL komut satırı veya MySQL çalışma ekranı gibi bir araçtan `az_load_timezone` saklı yordam çağırarak doldurulabilir.

> [!NOTE]
> MySQL çalışma modundan `az_load_timezone` komutunu çalıştırıyorsanız, önce `SET SQL_SAFE_UPDATES=0;`kullanarak güvenli güncelleştirme modunu kapatmanız gerekebilir.

```sql
CALL mysql.az_load_timezone();
```

> [!IMPORTANT]
> Saat dilimi tablolarının doğru doldurulduğundan emin olmak için sunucuyu yeniden başlatmanız gerekir. Sunucuyu yeniden başlatmak için [Azure Portal](howto-restart-server-portal.md) veya [CLI](howto-restart-server-cli.md)kullanın.
Kullanılabilir saat dilimi değerlerini görüntülemek için aşağıdaki komutu çalıştırın:

```sql
SELECT name FROM mysql.time_zone_name;
```

### <a name="setting-the-global-level-time-zone"></a>Genel bir düzeyinde saat dilimi ayarlama

Genel düzey saat dilimi Azure portal **sunucu parametreleri** sayfasından ayarlanabilir. Değerine ayarlar genel saat dilimi aşağıda "ABD / Pasifik".

![Saat dilimi parametre kümesi](./media/howto-server-parameters/timezone.png)

### <a name="setting-the-session-level-time-zone"></a>Oturum düzeyi saat dilimi ayarlama

Oturum düzeyi saat dilimi, MySQL komut satırı veya MySQL çalışma ekranı gibi bir araçla `SET time_zone` komutu çalıştırılarak ayarlanabilir. Aşağıdaki örnek saat dilimini **ABD/Pasifik** saati dilimine göre ayarlar.

```sql
SET time_zone = 'US/Pacific';
```

[Tarih ve saat işlevleri](https://mariadb.com/kb/en/library/convert_tz/)Için MariaDB belgelerine bakın.

<!--
## Next steps

- [Connection libraries for Azure Database for MariaDB](concepts-connection-libraries.md).
-->

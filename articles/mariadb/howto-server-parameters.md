---
title: Sunucu parametrelerini Yapılandırma-Azure portal-MariaDB için Azure veritabanı
description: Bu makalede, Azure portal kullanılarak MariaDB için Azure veritabanı 'nda MariaDB sunucu parametrelerinin nasıl yapılandırılacağı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/05/2019
ms.openlocfilehash: 59d18ea11699ed77763c162e4930b159fcd19fe2
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74888674"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mariadb-by-using-the-azure-portal"></a>Azure portal kullanarak, MariaDB için Azure veritabanı 'nda sunucu parametrelerini yapılandırma

MariaDB için Azure veritabanı bazı sunucu parametrelerinin yapılandırılmasını destekler. Bu makalede Azure portalını kullanarak bu parametreleri yapılandırma açıklanır. Tüm sunucu parametreleri ayarlanabilir.

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Azure portalında sunucu parametrelerini gidin

1. Azure portal oturum açın ve ardından MariaDB sunucusu için Azure veritabanınızı bulun.
2. **Ayarlar** bölümünde **sunucu parametreleri** ' ne tıklayarak MariaDB sunucusu için Azure veritabanı için sunucu parametreleri sayfasını açın.
![Azure portal sunucusu parametreleri sayfası](./media/howto-server-parameters/azure-portal-server-parameters.png)
3. Ayarlamak için gereken herhangi bir ayarı bulun. Gözden geçirme **açıklama** amacı ve izin verilen değerler anlamak için sütun.
![Aşağı açılan listeleme](./media/howto-server-parameters/3-toggle_parameter.png)
4. Tıklayın **Kaydet** yaptığınız değişiklikleri kaydedin.
![Kaydet veya değişiklikleri at](./media/howto-server-parameters/4-save_parameters.png)
5. Parametreler için yeni değerler kaydettiyseniz, her zaman varsayılan değerleri dön her şeyi seçerek geri dönebilirsiniz **tümünü Varsayılana Sıfırla**.
![Tümünü Varsayılana sıfırla](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Yapılandırılabilir sunucu parametrelerinin listesi

Desteklenen sunucu parametrelerinin listesi sürekli olarak artmaktadır. Sunucu parametreleri sekmesi tanımını Al ve uygulama gereksinimlerinize göre sunucu parametrelerini yapılandırma Azure portalında kullanın.

## <a name="non-configurable-server-parameters"></a>Yapılandırılabilir olmayan sunucu parametreleri

Innodb arabellek havuzu ve en fazla bağlantı olmayan yapılandırılabilir ve bağlı olduğu için [fiyatlandırma katmanı](concepts-pricing-tiers.md).

|**Fiyatlandırma Katmanı**| **Sanal çekirdek**|**Innodb arabellek havuzu (MB)**| **En fazla bağlantı sayısı**|
|---|---|---|---|
|Temel| 1| 1024| 50|
|Temel| 2| 2560| 100|
|Genel Amaçlı| 2| 3584| 300|
|Genel Amaçlı| 4| 7680| 625|
|Genel Amaçlı| 8| 15360| 1250|
|Genel Amaçlı| 16| 31232| 2500|
|Genel Amaçlı| 32| 62976| 5000|
|Genel Amaçlı| 64| 125952| 10000|
|Bellek için İyileştirilmiş| 2| 7168| 600|
|Bellek için İyileştirilmiş| 4| 15360| 1250|
|Bellek için İyileştirilmiş| 8| 30720| 2500|
|Bellek için İyileştirilmiş| 16| 62464| 5000|
|Bellek için İyileştirilmiş| 32| 125952| 10000|

Bu ek sunucu parametreleri sistemde yapılandırılabilir değildir:

|**Parametre**|**Sabit değer**|
| :------------------------ | :-------- |
|Temel katmanda innodb_file_per_table|KAPALI|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512 MB|

Burada listelenmeyen diğer sunucu parametreleri, [MariaDB](https://mariadb.com/kb/en/library/xtradbinnodb-server-system-variables/)'nin varsayılan dışı varsayılan değerlerine ayarlanır.

## <a name="working-with-the-time-zone-parameter"></a>Saat dilimi parametresi ile çalışma

### <a name="populating-the-time-zone-tables"></a>Saat dilimi tablolarını doldurma

Saat dilimi tabloları sunucunuzdaki çağırarak doldurulabilir `az_load_timezone` saklı yordamdan MySQL komut satırı veya MySQL Workbench gibi bir araç.

> [!NOTE]
> Çalıştırıyorsanız `az_load_timezone` ilk güvenli güncelleştirme modunu kapat gerekebilir MySQL Workbench'ten komutunu kullanarak `SET SQL_SAFE_UPDATES=0;`.

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

Genel bir düzeyinde saat dilimi ayarlanabilir **sunucu parametreleri** Azure portalında sayfası. Değerine ayarlar genel saat dilimi aşağıda "ABD / Pasifik".

![Saat dilimi parametre kümesi](./media/howto-server-parameters/timezone.png)

### <a name="setting-the-session-level-time-zone"></a>Oturum düzeyi saat dilimi ayarlama

Oturum düzeyi saat dilimi çalıştırarak ayarlanabilir `SET time_zone` MySQL komut satırı veya MySQL Workbench gibi bir araçla komutu. Aşağıdaki örnekte saat dilimini ayarlar **ABD / Pasifik** saat dilimi.

```sql
SET time_zone = 'US/Pacific';
```

[Tarih ve saat işlevleri](https://mariadb.com/kb/en/library/convert_tz/)Için MariaDB belgelerine bakın.

<!--
## Next steps

- [Connection libraries for Azure Database for MariaDB](concepts-connection-libraries.md).
-->

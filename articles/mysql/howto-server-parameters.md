---
title: Sunucu parametrelerini Yapılandırma-Azure portal-MySQL için Azure veritabanı
description: Bu makalede, Azure portalını kullanarak MySQL için Azure veritabanı'nda MySQL sunucusu parametrelerini yapılandırılacağını açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/05/2019
ms.openlocfilehash: 8ec6f32d7db0161cef00330aa38601ba9bdb309d
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74893153"
---
# <a name="how-to-configure-server-parameters-in-azure-database-for-mysql-by-using-the-azure-portal"></a>Nasıl MySQL için Azure veritabanı'nda Azure portalını kullanarak sunucu parametrelerini yapılandırma

MySQL için Azure veritabanı, bazı sunucu parametreleri yapılandırılmasını destekler. Bu makalede Azure portalını kullanarak bu parametreleri yapılandırma açıklanır. Tüm sunucu parametreleri ayarlanabilir.

## <a name="navigate-to-server-parameters-on-azure-portal"></a>Azure portalında sunucu parametrelerini gidin

1. Azure portalında oturum açın, sonra MySQL için Azure veritabanınızı bulun.
2. Altında **ayarları** bölümünde **sunucu parametreleri** MySQL sunucusu için Azure veritabanı sunucusu parametreleri sayfasını açın.
![Azure portal sunucusu parametreleri sayfası](./media/howto-server-parameters/auzre-portal-server-parameters.png)
3. Ayarlamak için gereken herhangi bir ayarı bulun. Gözden geçirme **açıklama** amacı ve izin verilen değerler anlamak için sütun.
![Aşağı açılan listeleme](./media/howto-server-parameters/3-toggle_parameter.png)
4. Tıklayın **Kaydet** yaptığınız değişiklikleri kaydedin.
![Kaydet veya değişiklikleri at](./media/howto-server-parameters/4-save_parameters.png)
5. Parametreler için yeni değerler kaydettiyseniz, her zaman varsayılan değerleri dön her şeyi seçerek geri dönebilirsiniz **tümünü Varsayılana Sıfırla**.
![Tümünü Varsayılana sıfırla](./media/howto-server-parameters/5-reset_parameters.png)

## <a name="list-of-configurable-server-parameters"></a>Yapılandırılabilir sunucu parametrelerinin listesi

Desteklenen sunucu parametrelerinin listesi sürekli olarak artmaktadır. Sunucu parametreleri sekmesi tanımını Al ve uygulama gereksinimlerinize göre sunucu parametrelerini yapılandırma Azure portalında kullanın.

## <a name="non-configurable-server-parameters"></a>Yapılandırılabilir olmayan sunucu parametreleri

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

Bu ek sunucu parametreleri sistemde yapılandırılabilir değildir:

|**Parametre**|**Sabit değer**|
| :------------------------ | :-------- |
|Temel katmanda innodb_file_per_table|KAPALI|
|innodb_flush_log_at_trx_commit|1|
|sync_binlog|1|
|innodb_log_file_size|512 MB|

Burada listelenmeyen diğer sunucu parametreleri sürümleri için MySQL kullanıma hazır varsayılan değerlerine ayarlanmış [5.7](https://dev.mysql.com/doc/refman/5.7/en/innodb-parameters.html) ve [5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html).

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

MySQL belgeleri için başvurmak [tarih ve saat işlevleri](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html#function_convert-tz).

## <a name="next-steps"></a>Sonraki adımlar

- [MySQL için Azure veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md).

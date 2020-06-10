---
title: Sunucu parametrelerini Yapılandırma-Azure portal-MariaDB için Azure veritabanı
description: Bu makalede, Azure portal kullanılarak MariaDB için Azure veritabanı 'nda MariaDB sunucu parametrelerinin nasıl yapılandırılacağı açıklanır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 6/8/2020
ms.openlocfilehash: 674ae5c60b7e897f43d28f5813641ddc833b3002
ms.sourcegitcommit: ce44069e729fce0cf67c8f3c0c932342c350d890
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84636079"
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

## <a name="working-with-the-time-zone-parameter"></a>Saat dilimi parametresiyle çalışma

### <a name="populating-the-time-zone-tables"></a>Saat dilimi tablolarını doldurma

Sunucunuzdaki saat dilimi tabloları, `mysql.az_load_timezone` MySQL komut satırı veya MySQL çalışma ekranı gibi bir araçtan saklı yordam çağırarak doldurulabilir.

> [!NOTE]
> `mysql.az_load_timezone`MySQL çalışma modundan komutunu çalıştırıyorsanız, önce kullanarak güvenli güncelleştirme modunu kapatmanız gerekebilir `SET SQL_SAFE_UPDATES=0;` .

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

Oturum düzeyi saat dilimi, `SET time_zone` MySQL komut satırı veya MySQL çalışma ekranı gibi bir araçtan komutu çalıştırılarak ayarlanabilir. Aşağıdaki örnek saat dilimini **ABD/Pasifik** saati dilimine göre ayarlar.

```sql
SET time_zone = 'US/Pacific';
```

[Tarih ve saat işlevleri](https://mariadb.com/kb/en/library/convert_tz/)Için MariaDB belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Sunucu parametreleri](concepts-server-parameters.md) hakkında daha fazla bilgi edinin
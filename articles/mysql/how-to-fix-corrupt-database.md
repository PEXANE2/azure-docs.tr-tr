---
title: Veritabanı bozulmasını çözümle-MySQL için Azure veritabanı
description: MySQL için Azure veritabanı 'nda veritabanı bozulma sorunlarını giderme hakkında bilgi edinin
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 09/21/2020
ms.openlocfilehash: 815b24d25e674e5460cc50d7eb6871f740994893
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941487"
---
# <a name="troubleshoot-database-corruption-on-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda veritabanı bozulması sorunlarını giderme
[!INCLUDE[applies-to-single-flexible-server](includes/applies-to-single-flexible-server.md)]

Veritabanı bozulması, uygulamanız için kapalı kalma süresine neden olabilir ve veri kaybını önlemek için zaman içinde sorunu çözümlemek de önemlidir. Veritabanı bozulması oluştuğunda, sunucu günlüklerinizi görürsünüz bu hata **InnoDB: diskte veritabanı hatası bozulması veya başarısız oldu**.

Bu kılavuzda, veritabanı veya tablo bozuksa nasıl düzeltileceğini öğreneceksiniz. MySQL için Azure veritabanı InnoDB altyapısını kullanır ve BT, otomatik bozulma denetimi ve onarım işlemleri yapar. InnoDB, okuduğu her sayfada sağlama toplamı gerçekleştirerek bozuk sayfaları denetler ve bir sağlama toplamı tutarsızlığı bulursa, MySQL sunucusunu otomatik olarak durdurur.

Veritabanınızın bozulma sorunlarını hızla azaltmaya yardımcı olmak için aşağıdaki seçeneklerden herhangi birini deneyin.

## <a name="restart-your-mysql-server"></a>MySQL sunucunuzu yeniden başlatın

Genellikle, uygulamanız bu tablo ro veritabanına erişirken bir veritabanının veya tablonun bozuk olduğunu fark edersiniz. InnoDB, sunucu yeniden başlatıldığında çoğu sorunu giderebilecek bir kilitlenme kurtarma mekanizması özelliklerine neden olur. Bu nedenle sunucunun yeniden başlatılması, veritabanının hatalı durumda kalmasına neden olan bir kilitlenmeden kurtarılmasına yardımcı olmalıdır.

##  <a name="resolve-data-corruption-with-dump-and-restore-method"></a>Döküm ve geri yükleme yöntemiyle veri bozulmasını çözümleme

Bir **döküm ve geri yükleme yöntemiyle**bozulma sorununun çözülmesi önerilir. Bu, tablo yapısını ve içindeki verileri koruyacak ve ardından tabloyu yeniden veritabanına geri yüklemeden, tablonun mantıksal bir yedeğini oluşturmak için **mysqldump** yardımcı programını kullanarak, bozulan tabloya erişim sağlamayı içerir.

### <a name="backup-your-database-or-tables"></a>Veritabanınızı veya tablolarınızı yedekleme

> [!Important]
> - İstemci makinenizden sunucuya erişmek için bir güvenlik duvarı kuralı yapılandırdığınıza dikkat edin. Esnek sunucuda, tek sunucu ve [güvenlik duvarı kuralında](flexible-server/how-to-connect-tls-ssl.md) [güvenlik duvarı kuralı](howto-manage-firewall-using-portal.md) yapılandırma konusuna bakın.
> - ```--ssl-cert```SSL etkinse **mysqldump** için SSL seçeneğini kullanın

Bu komutu kullanarak mysqldump kullanarak komut satırından bir yedekleme dosyası oluşturun

```
$ mysqldump [--ssl-cert=/path/to/pem] -h [host] -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

Sağlanacak parametreler şunlardır:
- [SSL-CERT =/Path/to/ped] SSL sertifikası 'nı istemci makinenize indirin ve komutta içindeki yolu ayarlayın. SSL 'yi kullanma devre dışı.
- [ana bilgisayar] MySQL sunucusu için Azure veritabanınız
- [uname], sunucu yöneticinizin kullanıcı adıdır
- [Pass] Yönetici kullanıcılarınızın parolasıdır
- [dbname] veritabanınızın adıdır
- [BackupFile. SQL] veritabanı yedeklemenizin dosya adı

> [!Important]
> - Tek sunucu için ```admin-user@servername``` aşağıdaki komutlarda değiştirmek üzere biçimini kullanın ```myserveradmin``` .
> - Esnek sunucu için ```admin-user``` aşağıdaki komutlarda değiştirilecek biçimi kullanın ```myserveradmin``` .

Belirli bir tablo bozuksa, bu örneği kullanarak yedeklemek için veritabanınızda belirli tablolar ' ı seçin
```
$ mysqldump --ssl-cert=</path/to/pem> -h mydemoserver.mysql.database.azure.com -u myserveradmin -p testdb table1 table2 > testdb_tables_backup.sql
```

Bir veya daha fazla veritabanını yedeklemek için--Database anahtarını kullanın ve boşluklarla ayrılmış veritabanı adlarını listeleyin.

```
$ mysqldump --ssl-cert=</path/to/pem>  -h mydemoserver.mysql.database.azure.com -u myserveradmin -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql
```

###  <a name="restore-your-database-or-tables"></a>Veritabanınızı veya tablolarınızı geri yükleme

Aşağıdaki adımlarda, TP 'nin veritabanınızı veya tablolarınızı nasıl geri yükleneceği gösterilmektedir. Yedekleme dosyası oluşturulduktan sonra, ***MySQL** yardımcı programını kullanarak tabloyu veya veritabanlarını geri yükleyebilirsiniz. Komutunu aşağıda gösterildiği gibi çalıştırın:

```
mysql  --ssl-cert=</path/to/pem> -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
```testdb``` **Mysqldump**ile oluşturulan yedekleme dosyasından geri yükleme işleminin bir örneği aşağıda verilmiştir. 

> [!Important]
> - Tek sunucu için ```admin-user@servername``` aşağıdaki komutla değiştirmek üzere biçimini kullanın ```myserveradmin``` .
> - Esnek sunucu için ```admin-user``` aşağıdaki komutta değiştirilecek biçimi kullanın ```myserveradmin``` . 

```
$ mysql --ssl-cert=</path/to/pem> -h mydemoserver.mysql.database.azure.com -u myserveradmin -p testdb < testdb_backup.sql
```

## <a name="next-steps"></a>Sonraki Adımlar
Yukarıdaki adımlar sorunu çözmeye yardımcı değilse, her zaman sunucunun tamamını geri yükleyebilirsiniz.
- [MySQL için Azure veritabanı tek sunucu geri yükleme](howto-restore-server-portal.md)
- [MySQL için Azure veritabanı esnek sunucusunu geri yükleme](flexible-server/how-to-restore-server-portal.md)




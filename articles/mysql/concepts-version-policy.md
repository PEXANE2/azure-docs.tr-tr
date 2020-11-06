---
title: Sürüm oluşturma ilkesi-MySQL için Azure veritabanı-tek sunucu ve esnek sunucu (Önizleme)
description: MySQL için Azure veritabanı 'nda MySQL 'in büyük ve küçük sürümlerinin bulunduğu ilkeyi açıklar
author: sr-msft
ms.author: srranga
ms.service: mysql
ms.topic: conceptual
ms.date: 11/03/2020
ms.custom: fasttrack-edit
ms.openlocfilehash: 0670107d84374589aa60cc18f184b9b3d3facce1
ms.sourcegitcommit: 46c5ffd69fa7bc71102737d1fab4338ca782b6f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94331813"
---
# <a name="azure-database-for-mysql-versioning-policy"></a>MySQL için Azure veritabanı sürüm oluşturma ilkesi

Bu sayfada MySQL için Azure veritabanı sürüm oluşturma ilkesi açıklanmakta ve MySQL için Azure veritabanı-tek sunucu ve MySQL için Azure veritabanı-esnek sunucu (Önizleme) Dağıtım modları için geçerlidir.

## <a name="supported--mysql-versions"></a>Desteklenen MySQL sürümleri

MySQL için Azure veritabanı aşağıdaki veritabanı sürümlerini destekler.

| Sürüm | Tek sunucu | Esnek Sunucu (Önizleme) |
| ----- | :------: | :----: |
| MySQL 8 | X |  | 
| MySQL 5,7 | X | X |
| MySQL 5,6| X |  |


## <a name="major-version-support"></a>Ana sürüm desteği
MySQL için Azure veritabanı, sürüm, sürüm [oluşturma ilkesinde](https://en.wikipedia.org/wiki/mysql)sağlandığı gibi MySQL Community tarafından devre dışı bırakılana kadar, MySQL Için Azure veritabanı tarafından desteklenecek.

## <a name="minor-version-support"></a>İkincil sürüm desteği
MySQL için Azure veritabanı, düzenli bakımın parçası olarak Azure tercih edilen MySQL sürümüne küçük sürüm yükseltmeleri otomatik olarak gerçekleştirir. 

## <a name="major-version-retirement-policy"></a>Ana sürüm kullanımdan kaldırma ilkesi
Aşağıdaki tabloda MySQL ana sürümleri için kullanımdan kaldırma ayrıntıları verilmiştir. Tarihler [MySQL sürüm oluşturma ilkesini](https://www.mysql.com/support/eol-notice.html)izler.

| Sürüm | Yenilikler | Azure desteği Başlangıç tarihi | Emeklilik tarihi|
| ----- | ----- | ------ | ----- |
| [MySQL 5,6](https://dev.mysql.com/doc/relnotes/mysql/5.6/)| [Özellikler](https://dev.mysql.com/doc/relnotes/mysql/5.6/en/news-5-6-49.html)  | 20 Mart 2018 | Şubat 2021
| [MySQL 5,7](https://dev.mysql.com/doc/relnotes/mysql/5.7/) | [Özellikler](https://dev.mysql.com/doc/relnotes/mysql/5.7/en/news-5-7-31.html) | 20 Mart 2018 | 2023 Ekim
| [MySQL 8](https://mysqlserverteam.com/whats-new-in-mysql-8-0-generally-available/) | [Özellikler](https://dev.mysql.com/doc/relnotes/mysql/8.0/en/news-8-0-21.html)) | 11 Aralık 2019 | 2026 Nisan


## <a name="retired-mysql-engine-versions-not-supported-in-azure-database-for-mysql"></a>Kullanımdan kaldırılan MySQL altyapısı sürümleri MySQL için Azure veritabanı 'nda desteklenmiyor

Her MySQL veritabanı sürümü için kullanımdan kaldırma tarihinden sonra, kullanımdan kaldırılan sürümü çalıştırmaya devam ederseniz aşağıdaki kısıtlamalara göz önünde bulabilirsiniz:
- Topluluk başka hata düzeltmeleri veya güvenlik düzeltmelerinin serbest bırakılmayacak, MySQL için Azure veritabanı, herhangi bir hata veya güvenlik sorunu için kullanımdan kaldırılan veritabanı altyapısına düzeltme eki uygulanmaz veya kullanımdan kaldırılan veritabanı altyapısından kaynaklanan güvenlik önlemleri almaz. Ancak Azure, ana bilgisayar, işletim sistemi, kapsayıcılar ve hizmetle ilgili diğer tüm bileşenler için düzenli bakım ve düzeltme eki gerçekleştirmeye devam edecektir.
- Karşılaşabileceğiniz herhangi bir destek sorunu MySQL veritabanıyla ilgili olarak size destek sunamayacak olabilir. Bu gibi durumlarda, size herhangi bir destek sağlayabilmesi için veritabanınızı yükseltmeniz gerekir.
- Kullanımdan kaldırılan sürüm için yeni veritabanı sunucuları oluşturabileceksiniz. Bununla birlikte, mevcut sunucularınız için zaman içinde kurtarmalar gerçekleştirebilir ve okuma çoğaltmaları oluşturabilirsiniz.
- MySQL için Azure veritabanı tarafından geliştirilen yeni hizmet özellikleri yalnızca desteklenen veritabanı sunucusu sürümleri için kullanılabilir olabilir.
- Çalışma süresi SLA 'Ları yalnızca MySQL hizmeti ile ilgili sorunlar için Azure veritabanı 'na, veritabanı altyapısından kaynaklanan hataların neden olması için geçerlidir.  
- Kullanımdan kaldırılmış olan veritabanı sürümünde tanımlanan MySQL veritabanı altyapısı güvenlik açığından kaynaklanan, hizmette ciddi bir tehdit olması durumunda Azure, önce hizmeti güvenli hale getirmek için veritabanı sunucunuzun işlem düğümünü durdurmayı tercih edebilir. Sunucuyu çevrimiçi duruma getirmeden önce sunucuyu yükseltmeniz istenir. Yükseltme işlemi sırasında, verileriniz her zaman, istenirse eski sürüme geri yüklemek için kullanılabilecek otomatik yedeklemeler kullanılarak korunur. 



## <a name="next-steps"></a>Sonraki adımlar
- Bkz. MySQL için Azure veritabanı-tek sunuculu [Desteklenen sürümler](./concepts-supported-versions.md)
- Bkz. MySQL için Azure veritabanı-esnek sunucu (Önizleme) [Desteklenen sürümler](flexible-server/concepts-supported-versions.md)
- Yükseltmeleri gerçekleştirmek için MySQL [dökümünü ve geri yüklemeyi](./concepts-migrate-dump-restore.md) inceleyin.

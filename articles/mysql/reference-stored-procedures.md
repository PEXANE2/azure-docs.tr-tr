---
title: Yönetim saklı yordamları-MySQL için Azure veritabanı
description: MySQL için Azure veritabanı 'nda bulunan saklı yordamların, verileri yapılandırma, saat dilimini ayarlama ve sorguları sonlandırma konusunda ne kadar kullanışlı olduğunu öğrenin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 6a3fa40eaae174d3616fd0318f81576b7c59eac7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80067694"
---
# <a name="azure-database-for-mysql-management-stored-procedures"></a>MySQL için Azure veritabanı yönetimi saklı yordamları

MySQL sunucunuzu yönetmeye yardımcı olmak için MySQL sunucuları için Azure veritabanı 'nda saklı yordamlar bulunur. Bu, sunucunuzun bağlantılarını, sorgularını yönetmeyi ve Gelen Verileri Çoğaltma ayarlamayı içerir.  

## <a name="data-in-replication-stored-procedures"></a>Gelen Verileri Çoğaltma saklı yordamlar

Gelen Verileri Çoğaltma, şirket içinde, sanal makinelerde veya diğer bulut sağlayıcıları tarafından barındırılan veritabanı hizmetlerinde çalışan bir MySQL sunucusundan, MySQL için Azure Veritabanı hizmetine verileri eşitlemenize olanak sağlar.

Ana ve çoğaltma arasında Gelen Verileri Çoğaltma ayarlamak veya kaldırmak için aşağıdaki saklı yordamlar kullanılır.

|**Saklı yordam adı**|**Giriş Parametreleri**|**Çıkış parametreleri**|**Kullanım notunun**|
|-----|-----|-----|-----|
|*MySQL. az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|Yok|SSL moduyla veri aktarmak için CA sertifikasının bağlamını master_ssl_ca parametresine geçirin. </br><br>SSL olmadan veri aktarmak için master_ssl_ca parametresine boş bir dize geçirin.|
|*MySQL. az_replication _start*|Yok|Yok|Çoğaltmayı başlatır.|
|*MySQL. az_replication _stop*|Yok|Yok|Çoğaltmayı durduruyor.|
|*MySQL. az_replication _remove_master*|Yok|Yok|Ana ve çoğaltma arasındaki çoğaltma ilişkisini kaldırır.|
|*MySQL. az_replication_skip_counter*|Yok|Yok|Bir çoğaltma hatasını atlar.|

MySQL için Azure veritabanı 'nda ana ve çoğaltma arasında Gelen Verileri Çoğaltma ayarlamak için, [gelen verileri çoğaltma yapılandırma](howto-data-in-replication.md)konusuna bakın.

## <a name="other-stored-procedures"></a>Diğer saklı yordamlar

Aşağıdaki saklı yordamlar, MySQL için Azure veritabanı 'nda sunucunuzu yönetmek için kullanılabilir.

|**Saklı yordam adı**|**Giriş Parametreleri**|**Çıkış parametreleri**|**Kullanım notunun**|
|-----|-----|-----|-----|
|*MySQL. az_kill*|processlist_id|Yok|[`KILL CONNECTION`](https://dev.mysql.com/doc/refman/8.0/en/kill.html) Komuta eşdeğerdir. , Bağlantının yürütüldüğü tüm deyimleri sonlandırdıktan sonra, belirtilen processlist_id ilişkili bağlantıyı sonlandırır.|
|*MySQL. az_kill_query*|processlist_id|Yok|[`KILL QUERY`](https://dev.mysql.com/doc/refman/8.0/en/kill.html) Komuta eşdeğerdir. Bağlantı şu anda yürütülmekte olan ifadeyi sonlandırır. Bağlantıyı canlı bırakır.|
|*MySQL. az_load_timezone*|Yok|Yok|`time_zone` Parametrenin adlandırılmış değerlere (örn.) ayarlanbilmesini sağlamak için saat dilimi tablolarını yükler. "ABD/Pasifik").|

## <a name="next-steps"></a>Sonraki adımlar
- [Gelen verileri çoğaltma](howto-data-in-replication.md) ayarlamayı öğrenin
- [Saat dilimi tablolarını](howto-server-parameters.md#working-with-the-time-zone-parameter) nasıl kullanacağınızı öğrenin

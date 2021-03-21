---
title: Yönetim saklı yordamları-MariaDB için Azure veritabanı
description: MariaDB için Azure veritabanı 'nda bulunan saklı yordamların, veri çoğaltmayı yapılandırmanıza, saat dilimini ayarlamanıza ve sorguları sonlandırmaya yardımcı olmak için yararlı olduğunu öğrenin.
author: savjani
ms.author: pariks
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 103bba37f5574185f10f5c4e28e66268da0c7f39
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98664640"
---
# <a name="azure-database-for-mariadb-management-stored-procedures"></a>MariaDB için Azure veritabanı yönetimi saklı yordamları

Saklı yordamlar, MariaDB sunucuları için Azure veritabanı 'nda, MariaDB sunucunuzu yönetmeye yardımcı olmak için kullanılabilir. Bu, sunucunuzun bağlantılarını, sorgularını yönetmeyi ve Gelen Verileri Çoğaltma ayarlamayı içerir.  

## <a name="data-in-replication-stored-procedures"></a>Gelen Verileri Çoğaltma saklı yordamlar

Gelen Verileri Çoğaltma, şirket içinde, sanal makinelerde veya diğer bulut sağlayıcıları tarafından barındırılan veritabanı hizmetlerinde çalışan bir MariaDB sunucusundan, MariaDB için Azure Veritabanı hizmetine verileri eşitlemenize olanak sağlar.

Aşağıdaki saklı yordamlar bir kaynak ve çoğaltma arasında Gelen Verileri Çoğaltma ayarlamak veya kaldırmak için kullanılır.

|**Saklı yordam adı**|**Giriş parametreleri**|**Çıkış parametreleri**|**Kullanım notunun**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|Yok|SSL moduyla veri aktarmak için CA sertifikasının bağlamını master_ssl_ca parametresine geçirin. </br><br>SSL olmadan veri aktarmak için master_ssl_ca parametresine boş bir dize geçirin.|
|*mysql.az_replication _start*|Yok|Yok|Çoğaltmayı başlatır.|
|*mysql.az_replication _stop*|Yok|Yok|Çoğaltmayı durduruyor.|
|*mysql.az_replication _remove_master*|Yok|Yok|Kaynak ve çoğaltma arasındaki çoğaltma ilişkisini kaldırır.|
|*mysql.az_replication_skip_counter*|Yok|Yok|Bir çoğaltma hatasını atlar.|

MariaDB için Azure veritabanı 'nda bir kaynak ve çoğaltma arasında Gelen Verileri Çoğaltma ayarlamak için, [gelen verileri çoğaltma yapılandırma](howto-data-in-replication.md)konusuna bakın.

## <a name="other-stored-procedures"></a>Diğer saklı yordamlar

Sunucunuzu yönetmek üzere MariaDB için Azure veritabanı 'nda aşağıdaki saklı yordamlar kullanılabilir.

|**Saklı yordam adı**|**Giriş parametreleri**|**Çıkış parametreleri**|**Kullanım notunun**|
|-----|-----|-----|-----|
|*mysql.az_kill*|processlist_id|Yok|Komuta eşdeğerdir [`KILL CONNECTION`](https://dev.mysql.com/doc/refman/8.0/en/kill.html) . , Bağlantının yürütüldüğü tüm deyimleri sonlandırdıktan sonra, belirtilen processlist_id ilişkili bağlantıyı sonlandırır.|
|*mysql.az_kill_query*|processlist_id|Yok|Komuta eşdeğerdir [`KILL QUERY`](https://dev.mysql.com/doc/refman/8.0/en/kill.html) . Bağlantı şu anda yürütülmekte olan ifadeyi sonlandırır. Bağlantıyı canlı bırakır.|
|*mysql.az_load_timezone*|Yok|Yok|`time_zone`Parametrenin adlandırılmış değerlere (örn.) ayarlanbilmesini sağlamak için saat dilimi tablolarını yükler. "ABD/Pasifik").|

## <a name="next-steps"></a>Sonraki adımlar
- [Gelen verileri çoğaltma](howto-data-in-replication.md) ayarlamayı öğrenin
- [Saat dilimi tablolarını](howto-server-parameters.md#working-with-the-time-zone-parameter) nasıl kullanacağınızı öğrenin
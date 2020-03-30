---
title: Yönetim depolanan yordamlar - MariaDB için Azure Veritabanı
description: MariaDB için Azure Veritabanı'nda hangi yordamların depolanmasının, veri çoğaltmayı yapılandırmanıza, saat dilimini ayarlamanıza ve sorguları öldürmenize yardımcı olmak için yararlı olduğunu öğrenin.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 2f6d1e20db64cb0c2a64771ea26b971b22031fd9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79529999"
---
# <a name="azure-database-for-mariadb-management-stored-procedures"></a>MariaDB yönetimi için Azure Veritabanı depolanmış yordamlar

MariaDB sunucunuzu yönetmenize yardımcı olmak için MariaDB sunucuları için Azure Veritabanı'nda depolanan yordamlar kullanılabilir. Buna sunucunuzun bağlantılarını, sorgularını yönetme ve Veri Çoğaltma'yı ayarlama dahildir.  

## <a name="data-in-replication-stored-procedures"></a>Veri Çoğaltma depolanan yordamlar

Gelen Verileri Çoğaltma, şirket içinde, sanal makinelerde veya diğer bulut sağlayıcıları tarafından barındırılan veritabanı hizmetlerinde çalışan bir MariaDB sunucusundan, MariaDB için Azure Veritabanı hizmetine verileri eşitlemenize olanak sağlar.

Aşağıdaki depolanan yordamlar, bir ana ve yineleme arasında Veri Çoğaltma'yı ayarlamak veya kaldırmak için kullanılır.

|**Depolanan Yordam Adı**|**Giriş Parametreleri**|**Çıkış Parametreleri**|**Kullanım Notu**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_port<br/>master_log_file<br/>master_log_pos<br/>master_ssl_ca|Yok|SSL moduile veri aktarmak için CA sertifikasının bağlamında master_ssl_ca parametresine geçin. </br><br>SSL olmadan veri aktarmak için boş bir dize içinde master_ssl_ca parametreye geçirin.|
|*mysql.az_replication _start*|Yok|Yok|Çoğaltma yı başlatır.|
|*mysql.az_replication _stop*|Yok|Yok|Çoğaltmayı durdurur.|
|*mysql.az_replication _remove_master*|Yok|Yok|Ana ve yineleme arasındaki çoğaltma ilişkisini kaldırır.|
|*mysql.az_replication_skip_counter*|Yok|Yok|Bir çoğaltma hatasini atlar.|

MariaDB için Azure Veritabanı'nda bir ana ve bir yineleme arasında Veri Çoğaltma'yı ayarlamak için, [Veri Çoğaltma'yı nasıl yapılandırılatırınıza](howto-data-in-replication.md)bakın.

## <a name="other-stored-procedures"></a>Diğer depolanan yordamlar

Sunucunuzu yönetmek için MariaDB için Azure Veritabanı'nda aşağıdaki saklı yordamlar kullanılabilir.

|**Depolanan Yordam Adı**|**Giriş Parametreleri**|**Çıkış Parametreleri**|**Kullanım Notu**|
|-----|-----|-----|-----|
|*mysql.az_kill*|processlist_id|Yok|Komuta [`KILL CONNECTION`](https://dev.mysql.com/doc/refman/8.0/en/kill.html) eşdeğeri. Bağlantının yürütüldeceği herhangi bir bildirimi sonlandırdıktan sonra sağlanan processlist_id ilişkili bağlantıyı sonlandırır.|
|*mysql.az_kill_query*|processlist_id|Yok|Komuta [`KILL QUERY`](https://dev.mysql.com/doc/refman/8.0/en/kill.html) eşdeğeri. Bağlantının şu anda yürütmekte olduğu deyimi sonlandırır. Bağlantıyı canlı bırakır.|
|*mysql.az_load_timezone*|Yok|Yok|Parametrenin `time_zone` adlandırılmış değerlere (ör. "ABD/Pasifik").|

## <a name="next-steps"></a>Sonraki adımlar
- [Veri Çoğaltma'yı](howto-data-in-replication.md) nasıl ayarlayatıyarı öğrenin
- [Saat dilimi tablolarını](howto-server-parameters.md#working-with-the-time-zone-parameter) nasıl kullanacağınızı öğrenin
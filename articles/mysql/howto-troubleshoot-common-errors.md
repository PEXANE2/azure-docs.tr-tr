---
title: Yaygın hatalarda sorun giderme-MySQL için Azure veritabanı
description: MySQL için Azure veritabanı hizmetine yönelik yeni kullanıcıların karşılaştığı yaygın geçiş hatalarıyla ilgili sorunları nasıl giderebileceğinizi öğrenin
author: savjani
ms.service: mysql
ms.author: pariks
ms.custom: mvc
ms.topic: overview
ms.date: 8/20/2020
ms.openlocfilehash: f64d4d2b9acbe0e6585ca546c915b82d2d1dbbc4
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92737184"
---
# <a name="common-errors"></a>Sık karşılaşılan hatalar

MySQL için Azure veritabanı, MySQL 'in topluluk sürümü tarafından desteklenen tam olarak yönetilen bir hizmettir. Yönetilen bir hizmet ortamındaki MySQL deneyimi, kendi ortamınızda MySQL 'i çalıştırmanın farklı olabilir. Bu makalede, kullanıcıların MySQL için Azure veritabanı hizmetine geçiş yaparken veya geliştirme sırasında karşılaşabilecekleri yaygın hatalardan bazılarını görürsünüz.

## <a name="errors-due-to-lack-of-super-privilege-and-dba-role"></a>Süper ayrıcalık olmaması ve DBA rolü nedeniyle hatalar oluştu

Süper ayrıcalık ve DBA rolü hizmette desteklenmez. Sonuç olarak, aşağıda listelenen bazı yaygın hatalarla karşılaşabilirsiniz:

#### <a name="error-1419-you-do-not-have-the-super-privilege-and-binary-logging-is-enabled-you-might-want-to-use-the-less-safe-log_bin_trust_function_creators-variable"></a>HATA 1419: süper ayrıcalığa sahip değilsiniz ve ikili günlüğe kaydetme etkin değil (daha az güvenli log_bin_trust_function_creators değişkenini *kullanmak isteyebilirsiniz)*

Bir işlev oluşturulurken yukarıdaki hata oluşabilir, aşağıdaki gibi tetiklenebilir veya bir şema içeri aktarılarak. OLUŞTURMA IşLEVI veya oluşturma TETIKLEYICISI gibi DDL deyimleri ikili günlüğe yazılır, bu nedenle ikincil çoğaltma bunları yürütebilir. Çoğaltma SQL iş parçacığının ayrıcalıkları yükseltmek için yararlanılabilen tam ayrıcalıkları vardır. İkili günlük etkinleştirilmiş sunucular için bu olma tehlikesi 'a karşı koruma sağlamak için, MySQL Engine 'in, gereken olağan oluşturma yordamına ek olarak, saklı işlev oluşturucularının süper ayrıcalığa sahip olması gerekir. 

```sql
CREATE FUNCTION f1(i INT)
RETURNS INT
DETERMINISTIC
READS SQL DATA
BEGIN
  RETURN i;
END;
```

**Çözüm** : hatayı çözümlemek için, Portal 'daki [sunucu parametreleri](howto-server-parameters.md) dikey penceresinden log_bin_trust_function_creators 1 ' i ayarlayın, DDL deyimlerini yürütün veya istenen nesneleri oluşturmak için şemayı içeri aktarın ve oluşturulduktan sonra log_bin_trust_function_creators parametresini önceki değerine geri dönün.

#### <a name="error-1227-42000-at-line-101-access-denied-you-need-at-least-one-of-the-super-privileges-for-this-operation-operation-failed-with-exitcode-1"></a>HATA 1227 (42000), satır 101: erişim engellendi; Bu işlem için süper ayrıcalıklar (en az biri) gereklidir. İşlem, ExitCode 1 ile başarısız oldu

Yukarıdaki hata, bir döküm dosyası içeri aktarılırken veya [definers](https://dev.mysql.com/doc/refman/5.7/en/create-procedure.html)içeren yordam oluşturulurken oluşabilir. 

**Çözüm** : Bu hatayı çözmek için Yönetici Kullanıcı, aşağıdaki örneklerde olduğu gıbı, Grant komutunu çalıştırarak yordam oluşturma veya yürütme ayrıcalıkları verebilir:

```sql
GRANT CREATE ROUTINE ON mydb.* TO 'someuser'@'somehost';
GRANT EXECUTE ON PROCEDURE mydb.myproc TO 'someuser'@'somehost';
```
Alternatif olarak, definers 'ı aşağıda gösterildiği gibi içeri aktarma işlemini çalıştıran yönetici kullanıcının adıyla değiştirebilirsiniz.

```sql
DELIMITER;;
/*!50003 CREATE*/ /*!50017 DEFINER=`root`@`127.0.0.1`*/ /*!50003
DELIMITER;;

/* Modified to */

DELIMITER ;;
/*!50003 CREATE*/ /*!50017 DEFINER=`AdminUserName`@`ServerName`*/ /*!50003
DELIMITER ;
```

## <a name="next-steps"></a>Sonraki Adımlar
Aradığınız yanıtı bulamazsanız aşağıdakileri göz önünde bulundurun:
- Sorunuzu [Microsoft Q&soru sayfası](/answers/topics/azure-database-mysql.html) veya [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-database-mysql)olarak gönderin.
- MySQL için Azure veritabanı ekibine bir e-posta gönderin [ @Ask ](mailto:AskAzureDBforMySQL@service.microsoft.com). Bu e-posta adresi bir teknik destek diğer adı değil.
- Azure desteği 'ne başvurun, [Azure Portal bir bilet dosyası](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)yapın. Hesabınızla ilgili bir sorun gidermek için Azure portalda bir [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) oluşturun.
- Görüş bildirmek veya yeni özellikler istemek için [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql) aracılığıyla bir giriş oluşturun.

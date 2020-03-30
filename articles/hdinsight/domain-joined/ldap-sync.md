---
title: Azure HDInsight'ta Ranger ve Apache Ambari'de LDAP senkronizasyonu
description: Ranger ve Ambari'deki LDAP senkronizasyonuna hitap edin ve genel yönergeler sağlayın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 99bd1ac156b12a5be7b8c5c17eb5b568b7070a25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77463225"
---
# <a name="ldap-sync-in-ranger-and-apache-ambari-in-azure-hdinsight"></a>Azure HDInsight'ta Ranger ve Apache Ambari'de LDAP senkronizasyonu

HDInsight Kurumsal Güvenlik Paketi (ESP) kümeleri yetkilendirme için Ranger'ı kullanır. Apache Ambari ve Ranger hem kullanıcıları hem de grupları bağımsız olarak senkronize eder ve biraz farklı çalışır. Bu makale, Ranger ve Ambari LDAP senkronizasyon adresi içindir.

## <a name="general-guidelines"></a>Genel yönergeler

* Kümeleri her zaman gruplarla dağıtın.
* Ambari ve Ranger'daki grup filtrelerini değiştirmek yerine, tüm bunları Azure AD'de yönetmeye çalışın ve gerekli kullanıcıları getirmek için iç içe geçmiş grupları kullanın.
* Bir kullanıcı eşitlendikten sonra, kullanıcı grupların bir parçası olmasa bile kaldırılmaz.
* LDAP filtrelerini doğrudan değiştirmeniz gerekiyorsa, bazı doğrulamalar içerdiğinden önce UI'yi kullanın.

## <a name="users-are-synced-separately"></a>Kullanıcılar ayrı ayrı senkronize edilir

Ambari ve Ranger iki farklı amaça hizmet ettikleri için kullanıcı veri tabanını paylaşmıyor. Bir kullanıcının Ambari UI'yi kullanması gerekiyorsa, kullanıcının Ambari ile senkronize edilmesi gerekir. Kullanıcı Ambari ile senkronize edilmezse, Ambari UI / API bunu reddeder, ancak sistemin diğer bölümleri çalışır (bunlar Ambari tarafından değil, Ranger veya Resource Manager tarafından korunur). Kullanıcıyı Ranger ilkesine dahil etmek istiyorsanız, kullanıcıyı Ranger ile senkronize edin.

Güvenli bir küme dağıtıldığında, grup üyeleri hem Ambari hem de Ranger ile geçişli olarak (tüm alt gruplar ve üyeleri) senkronize edilir. 

## <a name="ambari-user-sync-and-configuration"></a>Ambari kullanıcı senkronizasyonu ve yapılandırması

Kafa düğümleri, bir cron iş, `/opt/startup_scripts/start_ambari_ldap_sync.py`kullanıcı eşitleme zamanlamak için her saat çalıştırılır. Cron iş eşitleme gerçekleştirmek için Ambari dinlenme API'leri çağırır. Komut dosyası eşitlenmek üzere kullanıcıların ve grupların bir listesini gönderir (kullanıcılar belirtilen gruplara ait olmayabilir, her ikisi de ayrı ayrı belirtilir). Ambari, sAMAccountName'yi kullanıcı adı ve tüm grup üyeleri olarak geçişli olarak senkronize eder.

Günlükler içinde `/var/log/ambari-server/ambari-server.log`olmalıdır. Daha fazla bilgi için [Ambari günlük düzeyini Yapılandır' a](https://docs.cloudera.com/HDPDocuments/Ambari-latest/administering-ambari/content/amb_configure_ambari_logging_level.html)bakın.

Veri Gölü kümelerinde, kullanıcı sonrası oluşturma kancası eşitlenen kullanıcılar için ev klasörleri oluşturmak için kullanılır ve bunlar ev klasörlerinin sahipleri olarak ayarlanır. Kullanıcı Ambari ile doğru şekilde eşitlenmemişse, kullanıcı evreleme ve diğer geçici klasörlere erişmede hatalarla karşı karşıya kalabilir.

### <a name="update-groups-to-be-synced-to-ambari"></a>Ambari ile senkronize edilecek grupları güncelleştirme

Azure AD'deki grup üyeliklerini yönetemezseniz, iki seçeneğiniz vardır:

* [LDAP Kullanıcı larını ve Gruplarını Senkronize Edin'de](https://docs.cloudera.com/HDPDocuments/HDP3/latest/ambari-authentication-ldap-ad/content/authe_ldapad_synchronizing_ldap_users_and_groups.html)daha tam olarak açıklandığı gibi tek seferlik eşitleme gerçekleştirin. Grup üyeliği değiştiğinde, bu eşitlemeyi yeniden yapmanız gerekir.

* Bir cron iş yazın, [ambari API periyodik olarak](https://community.cloudera.com/t5/Support-Questions/How-do-I-automate-the-Ambari-LDAP-sync/m-p/96634) yeni gruplar ile arayın.

## <a name="ranger-user-sync-and-configuration"></a>Ranger Kullanıcı senkronizasyonu ve yapılandırması

Ranger kullanıcıları senkronize etmek için her saat çalışan dahili bir eşitleme motoru vardır. Kullanıcı veritabanını Ambari ile paylaşmaz. HDInsight, arama filtresini yönetici kullanıcıyı, izleme izleme kullanıcısını ve küme oluşturma sırasında belirtilen grup üyelerini eşitlemek için yapılandırır. Grup üyeleri geçişli olarak senkronize edilecektir:

* Artımlı eşitlemeyi devre dışı.
* Kullanıcı grubu eşitleme haritasını etkinleştirin.
* Geçişli grup üyelerini içerecek şekilde arama filtresini belirtin.
* Kullanıcılar için sAMAccountName'yi eşitleme ve gruplar için ad özniteliği.

### <a name="group-or-incremental-sync"></a>Grup veya Artımlı eşitleme

Ranger bir grup eşitleme seçeneğini destekler, ancak kullanıcı filtresi ile bir kesişim olarak çalışır. Grup üyelikleri ve kullanıcı filtresi arasında bir birlik değil. Ranger'da grup eşitleme filtresi için tipik bir kullanım örneği - grup filtresi: (dn=clusteradmingroup), kullanıcı filtresi: (şehir=seattle).

Artımlı eşitleme yalnızca zaten eşitlenmiş kullanıcılar için çalışır (ilk kez). Artımlı ilk eşitlemeden sonra gruplara eklenen yeni kullanıcıları eşitlemez.

### <a name="update-ranger-sync-filter"></a>Ranger eşitleme filtresini güncelleştir

LDAP filtresi Ambari UI'de Ranger kullanıcı eşitleme yapılandırması bölümü altında bulunabilir. Varolan filtre şeklinde `(|(userPrincipalName=bob@contoso.com)(userPrincipalName=hdiwatchdog-core01@CONTOSO.ONMICROSOFT.COM)(memberOf:1.2.840.113556.1.4.1941:=CN=hadoopgroup,OU=AADDC Users,DC=contoso,DC=onmicrosoft,DC=com))`olacaktır. Sonunda yüklem eklediğinizden emin olun ve arama `net ads` komutunu veya ldp.exe veya benzeri bir şeyi kullanarak filtreyi test edin.

## <a name="ranger-user-sync-logs"></a>Ranger kullanıcı eşitleme günlükleri

Ranger kullanıcı eşitleme headnodes ya da olabilir. Günlükler içinde. `/var/log/ranger/usersync/usersync.log` Günlüklerin ayrıntılılığını artırmak için aşağıdaki adımları yapın:

1. Ambari'ye giriş yap.
1. Ranger yapılandırma bölümüne gidin.
1. Gelişmiş **usersync-log4j** bölümüne gidin.
1. Düzeyi `log4j.rootLogger` değiştirin (Değiştirinden sonra `log4j.rootLogger = DEBUG,logFile,FilterLog`gibi görünmelidir). `DEBUG`
1. Yapılandırmayı kaydedin ve ranger'ı yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight'ta kimlik doğrulama sorunları](./domain-joined-authentication-issues.md)
* [Azure AD kullanıcılarını HDInsight kümesine eşitleme](../hdinsight-sync-aad-users-to-cluster.md)

---
title: Azure HDInsight 'ta Ranger ve Apache ambarı 'nda LDAP eşitlemesi
description: Ranger ve ambarı 'nda LDAP eşitlemesini çözün ve genel yönergeler sağlayın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 99bd1ac156b12a5be7b8c5c17eb5b568b7070a25
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77463225"
---
# <a name="ldap-sync-in-ranger-and-apache-ambari-in-azure-hdinsight"></a>Azure HDInsight 'ta Ranger ve Apache ambarı 'nda LDAP eşitlemesi

HDInsight Kurumsal Güvenlik Paketi (ESP) kümeleri yetkilendirme için Ranger kullanır. Apache ambarı ve Ranger, kullanıcıları ve grupları bağımsız olarak eşitler ve biraz farklı çalışır. Bu makale, Ranger ve ambarı 'nda LDAP eşitlemesini ele almak için tasarlanmıştır.

## <a name="general-guidelines"></a>Genel yönergeler

* Kümeleri gruplar ile her zaman dağıtın.
* Ambarı ve Ranger içindeki Grup filtrelerini değiştirmek yerine, tüm bunları Azure AD 'de yönetmeyi ve iç içe grupları kullanarak gerekli kullanıcıları getirin.
* Kullanıcı eşitlendikten sonra, Kullanıcı grupların parçası olmasa bile kaldırılmaz.
* LDAP filtrelerini doğrudan değiştirmeniz gerekiyorsa, bazı doğrulamalar içerdiğinden önce Kullanıcı arabirimini kullanın.

## <a name="users-are-synced-separately"></a>Kullanıcılar ayrı olarak eşitlenir

İki farklı amaca hizmet ettiğinden, ambarı ve Ranger Kullanıcı veritabanını paylaşmaz. Bir kullanıcının, ambarı Kullanıcı ARABIRIMINI kullanması gerekiyorsa, kullanıcının ambarı ile eşitlenmesi gerekir. Kullanıcı ambarı ile eşitlenmemişse, ambarı kullanıcı ARABIRIMI/API 'SI reddeder, ancak sistemin diğer bölümleri çalışacaktır (Bunlar, Ranger veya Kaynak Yöneticisi ve ambarı değil) tarafından korunur. Kullanıcıyı bir Ranger ilkesine eklemek istiyorsanız, kullanıcıyı Ranger ile eşitleyin.

Güvenli bir küme dağıtıldığında, Grup üyeleri geçişli olarak (tüm alt gruplar ve bunların üyeleri) hem ambarı hem de Ranger için eşitlenir. 

## <a name="ambari-user-sync-and-configuration"></a>Ambarı Kullanıcı eşitleme ve yapılandırma

Kullanıcı eşitlemesini zamanlamak için bir cron işi `/opt/startup_scripts/start_ambari_ldap_sync.py`olan baş düğümlerden her saat çalıştırılır. Cron işi, eşitlemeyi gerçekleştirmek için, ambarı REST API 'Lerini çağırır. Betik, eşitlenecek Kullanıcı ve grupların bir listesini gönderir (kullanıcılar belirtilen gruplara ait olmayabilir, her ikisi de ayrı olarak belirtilir). Ambarı, sAMAccountName öğesini Kullanıcı adı ve tüm grup üyeleri olarak eşitler, geçişli.

Günlüklerin içinde `/var/log/ambari-server/ambari-server.log`olması gerekir. Daha fazla bilgi için bkz. [ambarı günlüğü düzeyini yapılandırma](https://docs.cloudera.com/HDPDocuments/Ambari-latest/administering-ambari/content/amb_configure_ambari_logging_level.html).

Data Lake kümelerinde, Kullanıcı sonrası oluşturma kancası, eşitlenen kullanıcılar için giriş klasörleri oluşturmak için kullanılır ve giriş klasörlerinin sahibi olarak ayarlanır. Kullanıcı, doğru şekilde ambarla eşitlenmemişse, hazırlama ve diğer geçici klasörlere erişme sırasında Kullanıcı hatalara karşı başarısız olabilir.

### <a name="update-groups-to-be-synced-to-ambari"></a>Ambarı ile eşitlenecek güncelleştirme grupları

Azure AD 'de grup üyeliklerini yönetebilmeniz için iki seçeneğiniz vardır:

* [LDAP kullanıcılarını ve gruplarını eşitleme](https://docs.cloudera.com/HDPDocuments/HDP3/latest/ambari-authentication-ldap-ad/content/authe_ldapad_synchronizing_ldap_users_and_groups.html)konusunda daha fazla bilgi için bir kerelik eşitleme gerçekleştirin. Grup üyeliği değiştiğinde, bu eşitlemeyi yeniden yapmanız gerekir.

* Bir cron işi yazın, yeni gruplar ile [AMBARı API 'sini düzenli olarak](https://community.cloudera.com/t5/Support-Questions/How-do-I-automate-the-Ambari-LDAP-sync/m-p/96634) çağırın.

## <a name="ranger-user-sync-and-configuration"></a>Ranger Kullanıcı eşitleme ve yapılandırma

Ranger, kullanıcıların eşitlenmesi için saatte bir çalışan, yerleşik bir eşitleme altyapısına sahiptir. Kullanıcı veritabanını ambarı ile paylaşmaz. HDInsight, yönetici kullanıcıyı, izleme kullanıcısını ve küme oluşturma sırasında belirtilen grubun üyelerini eşitlemek için arama filtresini yapılandırır. Grup üyeleri geçişli olarak eşitlenir:

* Artımlı eşitlemeyi devre dışı bırakın.
* Kullanıcı grubu eşitleme haritasını etkinleştirin.
* Geçişli grup üyelerini dahil etmek için arama filtresi belirtin.
* Gruplar için sAMAccountName for Users ve Name özniteliğini eşitleyin.

### <a name="group-or-incremental-sync"></a>Grup veya artımlı eşitleme

Ranger bir grup eşitleme seçeneğini destekler, ancak Kullanıcı filtresiyle kesişme olarak da kullanılır. Grup üyelikleri ve Kullanıcı filtresi arasında bir birleşim değil. Ranger içindeki Grup eşitleme filtresi için tipik kullanım durumu-grup filtresi: (DN = clusteradmingroup), Kullanıcı filtresi: (City = Seattle).

Artımlı eşitleme yalnızca zaten eşitlenmiş olan kullanıcılar (ilk kez) için geçerlidir. Artımlı, ilk eşitlemeden sonra gruplara eklenen yeni kullanıcıları eşitmez.

### <a name="update-ranger-sync-filter"></a>Ranger eşitleme filtresini Güncelleştir

LDAP filtresi, ' ın Ranger Kullanıcı-eşitleme Yapılandırması bölümünün altında bulunan ambarı Kullanıcı arabiriminde bulunabilir. Mevcut filtre formda `(|(userPrincipalName=bob@contoso.com)(userPrincipalName=hdiwatchdog-core01@CONTOSO.ONMICROSOFT.COM)(memberOf:1.2.840.113556.1.4.1941:=CN=hadoopgroup,OU=AADDC Users,DC=contoso,DC=onmicrosoft,DC=com))`olacaktır. Son ' a koşul ekleyip arama komutunu veya Ldp. exe ' yi veya `net ads` benzer bir şeyi kullanarak filtreyi test edin.

## <a name="ranger-user-sync-logs"></a>Ranger Kullanıcı eşitleme günlükleri

Ranger Kullanıcı eşitlemesi, her iki yayın düğümünden de oluşabilir. Günlükler ' de `/var/log/ranger/usersync/usersync.log`bulunur. Günlüklerin ayrıntı düzeyini artırmak için aşağıdaki adımları uygulayın:

1. Ambarı 'nda oturum açın.
1. Ranger yapılandırma bölümüne gidin.
1. Gelişmiş **usersync-Log4J** bölümüne gidin.
1. Öğesini olarak `log4j.rootLogger` `DEBUG` değiştirin (değiştirme sonrasında, şöyle `log4j.rootLogger = DEBUG,logFile,FilterLog`görünmelidir).
1. Yapılandırmayı kaydedin ve Ranger 'ı yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight 'ta kimlik doğrulama sorunları](./domain-joined-authentication-issues.md)
* [Azure AD kullanıcılarını HDInsight kümesine eşitleme](../hdinsight-sync-aad-users-to-cluster.md)

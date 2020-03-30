---
title: Azure Monitör'de Syslog iletilerini toplayın ve analiz edin | Microsoft Dokümanlar
description: Syslog, Linux'ta yaygın olan bir olay günlüğü protokolüdür. Bu makalede, Log Analytics'teki Syslog iletilerinin toplanmasının nasıl yapılandırılabildikleri ve oluşturdukları kayıtların ayrıntılarını açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/22/2019
ms.openlocfilehash: 8d68a8d6d28d79c50a92cd2d18df2abab26c30ec
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274729"
---
# <a name="syslog-data-sources-in-azure-monitor"></a>Azure İzleyici'de Syslog veri kaynakları
Syslog, Linux'ta yaygın olan bir olay günlüğü protokolüdür. Uygulamalar, yerel makinede depolanabilecek veya bir Syslog toplayıcısına teslim edilebilecek iletiler gönderir. Linux için Log Analytics aracısı yüklendiğinde, iletileri temsilciye iletmek için yerel Syslog daemon'u yapılandırır. Aracı, daha sonra ilgili kaydın oluşturulduğu Azure Monitor'a iletiyi gönderir.  

> [!NOTE]
> Azure Monitor, rsyslog veya syslog-ng tarafından gönderilen ve rsyslog'un varsayılan daemon olduğu iletilerin toplanmasını destekler. Red Hat Enterprise Linux, CentOS ve Oracle Linux sürümü (sysklog) sürüm 5 varsayılan syslog daemon syslog olay koleksiyonu için desteklenmez. Bu dağıtımların bu sürümünden syslog verileri toplamak için, [rsyslog daemon](http://rsyslog.com) yüklü olmalı ve sysklog yerine yapılandırılmalıdır.
>
>

![Syslog koleksiyonu](media/data-sources-syslog/overview.png)

Aşağıdaki olanaklar Syslog toplayıcısı ile desteklenir:

* Kaya
* kullanıcı
* posta
* Daemon
* Auth
* syslog
* Lpr
* news
* Uucp
* Cron
* authpriv
* ftp
* yerel0-yerel7

Başka bir tesis için Azure Monitor'da [özel günlükler veri kaynağını yapılandırın.](data-sources-custom-logs.md)
 
## <a name="configuring-syslog"></a>Syslog'u Yapılandırma
Linux için Log Analytics aracısı yalnızca yapılandırmasında belirtilen olanaklar ve önemleri olan olayları toplar. Syslog'u Azure portalı üzerinden veya Linux aracılarınızdaki yapılandırma dosyalarını yöneterek yapılandırabilirsiniz.

### <a name="configure-syslog-in-the-azure-portal"></a>Azure portalında Syslog'u yapılandırma
[Gelişmiş Ayarlar'da Veri menüsünden](agent-data-sources.md#configuring-data-sources)Syslog'u yapılandırın. Bu yapılandırma, her Linux aracısının yapılandırma dosyasına teslim edilir.

Önce **makinelerime aşağıdaki yapılandırmayı uygula** seçeneğini seçerek ve sonra adını yazıp tıklayarak **+** yeni bir tesis ekleyebilirsiniz. Her tesis için yalnızca seçili öneme sahip iletiler toplanır.  Toplamak istediğiniz özel tesisin önemlerini kontrol edin. İletileri filtrelemek için ek ölçüt ler sağlayamazsınız.

![Syslog'u yapılandır](media/data-sources-syslog/configure.png)

Varsayılan olarak, tüm yapılandırma değişiklikleri otomatik olarak tüm aracılara itilir. Syslog'u her Linux aracısına el ile yapılandırmak istiyorsanız, kutuyu işaretleyin *Makinelerime aşağıdaki yapılandırmayı uygulayın.*

### <a name="configure-syslog-on-linux-agent"></a>Linux aracısı üzerinde Syslog yapılandırma
Log [Analytics aracısı bir Linux istemcisi üzerine yüklendiğinde,](../../azure-monitor/learn/quick-collect-linux-computer.md)toplanan iletilerin tesisini ve önem derecesini tanımlayan varsayılan bir syslog yapılandırma dosyası yükler. Yapılandırmayı değiştirmek için bu dosyayı değiştirebilirsiniz. Yapılandırma dosyası, istemcinin yüklediği Syslog daemon'a bağlı olarak farklıdır.

> [!NOTE]
> Syslog yapılandırmasını düzenlemeniz gerekiyorsa, değişikliklerin etkili olması için syslog daemon'u yeniden başlatmanız gerekir.
>
>

#### <a name="rsyslog"></a>rsyslog
rsyslog için yapılandırma dosyası **/etc/rsyslog.d/95-omsagent.conf**adresinde bulunmaktadır. Varsayılan içeriği aşağıda gösterilmiştir. Bu uyarı düzeyi veya daha yüksek olan tüm tesisler için yerel ajandan gönderilen syslog iletileri toplar.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

Yapılandırma dosyasının kendi bölümünü kaldırarak bir tesisi kaldırabilirsiniz. Belirli bir tesis için toplanan önemleri, o tesisin girişini değiştirerek sınırlayabilirsiniz. Örneğin, kullanıcı tesisini hata veya daha yüksek önem derecesine sahip iletilerle sınırlamak için yapılandırma dosyasının bu satırını aşağıdakilerle değiştirirsiniz:

    user.error    @127.0.0.1:25224


#### <a name="syslog-ng"></a>syslog-ng
syslog-ng için yapılandırma dosyası **/etc/syslog-ng/syslog-ng.conf**adresinde yer almaktadır.  Varsayılan içeriği aşağıda gösterilmiştir. Bu, tüm tesisler ve tüm önemleri için yerel ajandan gönderilen syslog iletilerini toplar.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };

    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };

    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };

    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };

    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };

    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };

    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Yapılandırma dosyasının kendi bölümünü kaldırarak bir tesisi kaldırabilirsiniz. Belirli bir tesis için toplanan önemleri, listesinden kaldırarak sınırlandırabilirsiniz.  Örneğin, kullanıcı tesisini yalnızca uyarı ve kritik iletilerle sınırlamak için yapılandırma dosyasının bu bölümünü aşağıdakilerle değiştirirsiniz:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="collecting-data-from-additional-syslog-ports"></a>Ek Syslog bağlantı noktalarından veri toplama
Log Analytics aracısı, 25224 bağlantı noktasındaki yerel istemcideki Syslog iletilerini dinler.  Aracı yüklendiğinde, varsayılan syslog yapılandırması uygulanır ve aşağıdaki konumda bulunur:

* Rsyslog:`/etc/rsyslog.d/95-omsagent.conf`
* Syslog-ng:`/etc/syslog-ng/syslog-ng.conf`

İki yapılandırma dosyası oluşturarak bağlantı noktası numarasını değiştirebilirsiniz: Yüklediğiniz Syslog daemon'a bağlı olarak bir FluentD config dosyası ve rsyslog-or-syslog-ng dosyası.  

* FluentD config dosyası yeni bir dosya içinde `/etc/opt/microsoft/omsagent/conf/omsagent.d` bulunmalı: ve **bağlantı noktası** girişindeki değeri özel bağlantı noktası numaranızla değiştirin.

        <source>
          type syslog
          port %SYSLOG_PORT%
          bind 127.0.0.1
          protocol_type udp
          tag oms.syslog
        </source>
        <filter oms.syslog.**>
          type filter_syslog
        </filter>

* rsyslog için, bulunan yeni bir yapılandırma dosyası `/etc/rsyslog.d/` oluşturmanız gerekir: ve %SYSLOG_PORT değerini özel bağlantı noktası numaranızla değiştirin.  

    > [!NOTE]
    > Yapılandırma dosyasında `95-omsagent.conf`bu değeri değiştirirseniz, aracı varsayılan yapılandırma uyguladığında üzerine yazılır.
    >

        # OMS Syslog collection for workspace %WORKSPACE_ID%
        kern.warning              @127.0.0.1:%SYSLOG_PORT%
        user.warning              @127.0.0.1:%SYSLOG_PORT%
        daemon.warning            @127.0.0.1:%SYSLOG_PORT%
        auth.warning              @127.0.0.1:%SYSLOG_PORT%

* Syslog-ng config aşağıda gösterilen örnek yapılandırma kopyalayarak ve bulunan syslog-ng.conf yapılandırma dosyasının sonuna özel modifiye ayarları ekleyerek `/etc/syslog-ng/`değiştirilmelidir. Varsayılan etiket **%WORKSPACE_ID% _oms** veya **%WORKSPACE_ID_OMS** **kullanmayın,** değişikliklerinizi ayırt etmeye yardımcı olmak için özel bir etiket tanımlayın.  

    > [!NOTE]
    > Yapılandırma dosyasındaki varsayılan değerleri değiştirirseniz, aracı varsayılan yapılandırma uyguladığında bunlar üzerine yazılır.
    >

        filter f_custom_filter { level(warning) and facility(auth; };
        destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
        log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };

Değişiklikleri tamamladıktan sonra, yapılandırma değişikliklerinin etkili olmasını sağlamak için Syslog ve Log Analytics aracısı hizmetinin yeniden başlatılması gerekir.   

## <a name="syslog-record-properties"></a>Syslog kayıt özellikleri
Syslog kayıtları bir **Syslog** türüne sahiptir ve aşağıdaki tablodaki özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olayın toplandığı bilgisayar. |
| Tesis |Sistemin iletiyi oluşturan bölümünü tanımlar. |
| HostIP |İletiyi gönderen sistemin IP adresi. |
| Ana bilgisayar adı |İletiyi gönderen sistemin adı. |
| Önem Düzeyi |Olayın önem düzeyi. |
| SyslogMessage |İletinin metni. |
| ProcessID |İletiyi oluşturan işlemin kimliği. |
| Etkinlik Zamanı |Olayın oluşturulduğu tarih ve saat. |

## <a name="log-queries-with-syslog-records"></a>Syslog kayıtlarıyla günlük sorguları
Aşağıdaki tablo, Syslog kayıtlarını alan günlük sorgularının farklı örneklerini sağlar.

| Sorgu | Açıklama |
|:--- |:--- |
| Syslog |Tüm Sysloglar. |
| Syslog &#124; nerede Önem Düzeyi == "hata" |Hata şiddeti ile tüm Syslog kayıtları. |
| Syslog &#124; Bilgisayara göre AggregatedValue = count() özetlemek |Syslog kayıtlarının bilgisayartarafından sayımı. |
| Syslog &#124; TopluDeğer = count() tarafından Tesise göre özetlemek |Tesise göre Syslog kayıtlarının sayısı. |

## <a name="next-steps"></a>Sonraki adımlar
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../../azure-monitor/log-query/log-query-overview.md) hakkında bilgi edinin.
* Syslog kayıtlarından tek tek alanlara ayrıştamak için [Özel Alanlar'ı](../../azure-monitor/platform/custom-fields.md) kullanın.
* [Linux aracılarını](../../azure-monitor/learn/quick-collect-linux-computer.md) diğer veri türlerini toplamak için yapılandırın.

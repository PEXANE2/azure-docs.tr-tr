---
title: Azure Izleyici 'de syslog iletilerini toplayın ve çözümleyin | Microsoft Docs
description: Syslog, Linux için ortak olan bir olay günlüğü protokolüdür. Bu makalede, oluşturdukları kayıtların Log Analytics ve ayrıntılarında Syslog iletilerinin toplanmasını yapılandırma açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/22/2019
ms.openlocfilehash: 983091ac0d1f9fdcb33e64d2af521ec3442a040b
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86515521"
---
# <a name="syslog-data-sources-in-azure-monitor"></a>Azure İzleyici'de Syslog veri kaynakları
Syslog, Linux için ortak olan bir olay günlüğü protokolüdür. Uygulamalar yerel makinede depolanabilecek veya bir Syslog toplayıcısına teslim edilen iletileri gönderir. Linux için Log Analytics Aracısı yüklendiğinde, iletileri aracıya iletmek için yerel Syslog Daemon programını yapılandırır. Aracı daha sonra iletiyi ilgili kaydın oluşturulduğu Azure Izleyici 'ye gönderir.  

> [!NOTE]
> Azure Izleyici, rsyslog veya Syslog-ng tarafından gönderilen iletilerin toplanmasını destekler; burada rsyslog varsayılan Daemon olur. Red Hat Enterprise Linux, CentOS ve Oracle Linux sürümünün (sysklog) sürüm 5 ' te bulunan varsayılan Syslog Daemon, Syslog olay koleksiyonu için desteklenmez. Bu dağıtımların bu sürümünden Syslog verileri toplamak için [rsyslog arka plan programı](http://rsyslog.com) yüklenmeli ve sysklog ' ı değiştirecek şekilde yapılandırılmalıdır.
>
>

![Syslog koleksiyonu](media/data-sources-syslog/overview.png)

Syslog toplayıcısı ile aşağıdaki olanaklar desteklenir:

* Aralık
* kullanıcı
* posta
* inin
* 'u
* syslog
* LPR
* news
* uucp
* Cron
* authprıv
* ftp
* local0-local7

Diğer herhangi bir tesis için, Azure Izleyici 'de [özel bir günlük veri kaynağı yapılandırın](data-sources-custom-logs.md) .
 
## <a name="configuring-syslog"></a>Syslog yapılandırma
Linux için Log Analytics Aracısı yalnızca kendi yapılandırmasında belirtilen tesisler ve önem derecelerine sahip olayları toplar. Azure portal aracılığıyla Syslog 'yi yapılandırabilir veya Linux aracılarınızda yapılandırma dosyalarını yönetebilirsiniz.

### <a name="configure-syslog-in-the-azure-portal"></a>Azure portal Syslog 'ı yapılandırma
[Gelişmiş ayarlar 'Daki veri menüsünde](agent-data-sources.md#configuring-data-sources)Syslog 'ı yapılandırın. Bu yapılandırma, her bir Linux aracısının yapılandırma dosyasına dağıtılır.

İlk olarak, **aşağıdaki yapılandırmayı Makinelerime Uygula** seçeneğini belirleyip adını yazıp öğesini tıklayarak yeni bir tesis ekleyebilirsiniz **+** . Her tesis için yalnızca seçili önem derecelerine sahip iletiler toplanacaktır.  Toplamak istediğiniz belirli bir tesis için önem derecelerine göz atın. İletileri filtrelemek için herhangi bir ek ölçüt sağlamazsanız.

![Syslog yapılandırma](media/data-sources-syslog/configure.png)

Varsayılan olarak, tüm yapılandırma değişiklikleri otomatik olarak tüm aracılara gönderilir. Syslog 'yi her bir Linux aracısında el ile yapılandırmak istiyorsanız, *aşağıdaki yapılandırmayı Makinelerime Uygula*onay kutusunun işaretini kaldırın.

### <a name="configure-syslog-on-linux-agent"></a>Linux aracısında Syslog yapılandırma
[Log Analytics Aracısı bir Linux istemcisine yüklendiğinde](../../azure-monitor/learn/quick-collect-linux-computer.md), toplanan iletilerin tesis ve önem derecesini tanımlayan bir varsayılan Syslog yapılandırma dosyası yüklenir. Bu dosyayı, yapılandırmayı değiştirmek için değiştirebilirsiniz. Yapılandırma dosyası, istemcinin yüklediği Syslog Daemon öğesine bağlı olarak farklılık gösteren bir yapılandırmadır.

> [!NOTE]
> Syslog yapılandırmasını düzenlerseniz değişikliklerin etkili olması için Syslog Daemon programını yeniden başlatmanız gerekir.
>
>

#### <a name="rsyslog"></a>rsyslog
Rsyslog yapılandırma dosyası **/etc/rsyslog.exe**dizininde bulunur. Varsayılan içerikleri aşağıda gösterilmiştir. Bu, bir uyarı veya daha yüksek düzeydeki tüm tesislerde yerel aracıdan gönderilen syslog iletilerini toplar.

```config
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
```

Yapılandırma dosyasının bölümünü kaldırarak bir tesisi kaldırabilirsiniz. Belirli bir tesis için toplanan önem derecelerine, bu tesis girişini değiştirerek sınırlayabilirsiniz. Örneğin, Kullanıcı olanağını bir hata veya daha yüksek önem derecesine sahip iletilerle sınırlamak için yapılandırma dosyasının bu satırını aşağıdaki şekilde değiştirirsiniz:

```config
user.error    @127.0.0.1:25224
```

#### <a name="syslog-ng"></a>Syslog-ng
Syslog-NG için yapılandırma dosyası, **/etc/syslog-ng/Syslog-ng.exe**yolunda yer aldığı bir konumdur.  Varsayılan içerikleri aşağıda gösterilmiştir. Bu, tüm tesisler ve tüm önem dereceleri için yerel aracıdan gönderilen syslog iletilerini toplar.   

```config
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
```

Yapılandırma dosyasının bölümünü kaldırarak bir tesisi kaldırabilirsiniz. Belirli bir tesis için toplanan önem derecelerine, listesinden kaldırarak sınırlayabilirsiniz.  Örneğin, Kullanıcı olanağını yalnızca uyarı ve kritik iletilerle sınırlamak için yapılandırma dosyasının bu bölümünü aşağıdaki şekilde değiştirirsiniz:

```config
#OMS_facility = user
filter f_user_oms { level(alert,crit) and facility(user); };
log { source(src); filter(f_user_oms); destination(d_oms); };
```

### <a name="collecting-data-from-additional-syslog-ports"></a>Ek Syslog bağlantı noktalarından veri toplama
Log Analytics Aracısı, 25224 numaralı bağlantı noktasındaki yerel istemcide syslog iletilerini dinler.  Aracı yüklendiğinde, varsayılan Syslog yapılandırması uygulanır ve aşağıdaki konumda bulunur:

* Rsyslog`/etc/rsyslog.d/95-omsagent.conf`
* Syslog-NG:`/etc/syslog-ng/syslog-ng.conf`

İki yapılandırma dosyası oluşturarak bağlantı noktası numarasını değiştirebilirsiniz: yüklediğiniz Syslog Daemon öğesine bağlı olarak, bir Floentd yapılandırma dosyası ve bir rsyslog-veya-Syslog-ng dosyası.  

* Floentd yapılandırma dosyası, içinde bulunan yeni bir dosya olmalıdır `/etc/opt/microsoft/omsagent/conf/omsagent.d` ve **bağlantı noktası** girdisindeki değeri özel bağlantı noktası numaranız ile değiştirin.

    ```xml
    <source>
        type syslog
        port %SYSLOG_PORT%
        bind 127.0.0.1
        protocol_type udp
        tag oms.syslog
    </source>
    <filter oms.syslog.**>
        type filter_syslog
    ```

* Rsyslog için, içinde bulunan yeni bir yapılandırma dosyası oluşturmanız `/etc/rsyslog.d/` ve% SYSLOG_PORT% değerini özel bağlantı noktası numaranız ile değiştirmeniz gerekir.  

    > [!NOTE]
    > Yapılandırma dosyasında bu değeri değiştirirseniz `95-omsagent.conf` , aracı varsayılan bir yapılandırma uyguladığı zaman üzerine yazılır.
    >

    ```config
    # OMS Syslog collection for workspace %WORKSPACE_ID%
    kern.warning              @127.0.0.1:%SYSLOG_PORT%
    user.warning              @127.0.0.1:%SYSLOG_PORT%
    daemon.warning            @127.0.0.1:%SYSLOG_PORT%
    auth.warning              @127.0.0.1:%SYSLOG_PORT%
    ```

* Aşağıda gösterilen örnek yapılandırma kopyalanarak ve içinde bulunan Syslog-ng. conf yapılandırma dosyasının sonuna özel değiştirilen ayarlar eklenerek Syslog-ng yapılandırması değiştirilmelidir `/etc/syslog-ng/` . **% WORKSPACE_ID% _oms** veya **% WORKSPACE_ID_OMS**varsayılan **etiketini kullanmayın,** değişikliklerinizi ayırt etmenize yardımcı olmak için özel bir etiket tanımlayın.  

    > [!NOTE]
    > Yapılandırma dosyasındaki varsayılan değerleri değiştirirseniz, aracı varsayılan bir yapılandırma uyguladığı zaman üzerine yazılır.
    >

    ```config
    filter f_custom_filter { level(warning) and facility(auth; };
    destination d_custom_dest { udp("127.0.0.1" port(%SYSLOG_PORT%)); };
    log { source(s_src); filter(f_custom_filter); destination(d_custom_dest); };
    ```

Değişiklikleri tamamladıktan sonra, yapılandırma değişikliklerinin etkili olması için Syslog ve Log Analytics Aracısı hizmetinin yeniden başlatılması gerekir.   

## <a name="syslog-record-properties"></a>Syslog kayıt özellikleri
Syslog kayıtları bir **Syslog** türüne sahiptir ve aşağıdaki tabloda bulunan özelliklere sahiptir.

| Özellik | Açıklama |
|:--- |:--- |
| Bilgisayar |Olayın toplandığı bilgisayar. |
| Olanak |İletiyi oluşturan sistemin parçasını tanımlar. |
| HostIP |İletiyi gönderen sistemin IP adresi. |
| HostName |İletiyi gönderen sistemin adı. |
| Severıtylevel |Etkinliğin önem düzeyi. |
| SyslogMessage |İleti metni. |
| ProcessID |İletiyi oluşturan işlemin KIMLIĞI. |
| EventTime |Olayın oluşturulduğu tarih ve saat. |

## <a name="log-queries-with-syslog-records"></a>Syslog kayıtlarıyla günlük sorguları
Aşağıdaki tabloda, syslog kayıtlarını alan günlük sorgularının farklı örnekleri verilmiştir.

| Sorgu | Description |
|:--- |:--- |
| Syslog |Tüm syslogs 'lar. |
| Syslog &#124; WHERE ıitylevel = = "Error" |Tüm Syslog kayıtları hata önem derecesine sahip. |
| Syslog &#124;, bir bilgisayara göre Aggreg, value = Count () özetleme |Bilgisayara göre Syslog kaydı sayısı. |
| Syslog &#124;, tesis tarafından Aggreg, value = Count () özetleme |Tesis tarafından Syslog kaydı sayısı. |

## <a name="next-steps"></a>Sonraki adımlar
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../../azure-monitor/log-query/log-query-overview.md) hakkında bilgi edinin.
* Syslog kayıtlarından verileri tek tek alanlara ayrıştırmak için [özel alanları](../../azure-monitor/platform/custom-fields.md) kullanın.
* [Linux aracılarını](../../azure-monitor/learn/quick-collect-linux-computer.md) diğer veri türlerini toplayacak şekilde yapılandırın.

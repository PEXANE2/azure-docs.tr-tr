---
title: Azure Log Analytics Linux Aracısı sorunlarını giderme | Microsoft Docs
description: Azure Izleyici 'de Linux için Log Analytics aracısında, en sık karşılaşılan sorunların belirtilerini, nedenlerini ve çözümlemesini tanıtın.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/21/2019
ms.openlocfilehash: 24aa3462aef4f719e93d17389ff342084f6c7864
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77668766"
---
# <a name="how-to-troubleshoot-issues-with-the-log-analytics-agent-for-linux"></a>Linux için Log Analytics Aracısı ile ilgili sorunları giderme 

Bu makalede, Azure Izleyici 'de Linux için Log Analytics aracısında karşılaşabileceğiniz sorunları gidermeye yönelik yardım ve bunları çözmek için olası çözümler sunulmaktadır.

Bu adımların hiçbiri işinize yaramazsa aşağıdaki Destek kanallarını da kullanılabilir:

* Premier destek avantajlarına sahip müşteriler, [Premier](https://premier.microsoft.com/)ile bir destek isteği açabilir.
* Azure destek sözleşmeleri olan müşteriler [Azure Portal](https://manage.windowsazure.com/?getsupport=true)bir destek talebi açabilir.
* OMI sorunlarını, [OMI sorun giderme kılavuzuyla](https://github.com/Microsoft/omi/blob/master/Unix/doc/diagnose-omi-problems.md)tanılayın.
* Bir [GitHub sorunu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)dosyası.
* Gönderilen fikirleri ve hataları [https://aka.ms/opinsightsfeedback](https://aka.ms/opinsightsfeedback) veya yeni bir dosya dosyasını gözden geçirmek Için Log Analytics geri bildirim sayfasını ziyaret edin.  

## <a name="important-log-locations-and-log-collector-tool"></a>Önemli günlük konumları ve günlük Toplayıcı aracı

 Dosya | Yol
 ---- | -----
 Linux günlük dosyası için log Analytics aracısını | `/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`
 Log Analytics Aracısını Yapılandırma günlük dosyası | `/var/opt/microsoft/omsconfig/omsconfig.log`

 Sorun giderme için veya bir GitHub sorunu göndermeden önce önemli günlükleri almak için günlük Toplayıcı aracımızı kullanmanızı öneririz. Araç hakkında daha fazla bilgi edinebilirsiniz ve [buradan](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/tools/LogCollector/OMS_Linux_Agent_Log_Collector.md)çalıştırabilirsiniz.

## <a name="important-configuration-files"></a>Önemli yapılandırma dosyaları

 Kategori | Dosya Konumu
 ----- | -----
 Syslog | `/etc/syslog-ng/syslog-ng.conf` veya `/etc/rsyslog.conf` veya `/etc/rsyslog.d/95-omsagent.conf`
 Nagios, Zabbix, Log Analytics çıkış ve genel aracı performansı | `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`
 Ek yapılandırmalar | `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/*.conf`

 >[!NOTE]
 >Koleksiyon, veri menüsünden yapılandırılmışsa, çalışma alanınızın Azure portal [Gelişmiş ayarları Log Analytics](../../azure-monitor/platform/agent-data-sources.md#configuring-data-sources) , performans sayaçları ve Syslog için yapılandırma dosyalarını düzenlemenin üzerine yazılır. Tüm aracıların yapılandırmasını devre dışı bırakmak için Log Analytics **Gelişmiş ayarlarından** koleksiyonu devre dışı bırakın ya da tek bir aracı için aşağıdakileri çalıştırın:  
> `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable'`

## <a name="installation-error-codes"></a>Yükleme hata kodları

| Hata Kodu | Anlamı |
| --- | --- |
| NOT_DEFINED | Gerekli bağımlılıkları yüklü olmadığından auoms auditd eklentisi yüklü değil | Paket auditd auoms başarısız oldu, yüklemeyi. |
| 2 | Kabuk pakete sağlanan seçeneği geçersiz. Kullanım için `sudo sh ./omsagent-*.universal*.sh --help` Çalıştır |
| 3 | Kabuk pakete sağlanan seçeneği yoktur. Kullanım için `sudo sh ./omsagent-*.universal*.sh --help` çalıştırın. |
| 4 | Geçersiz paket türü veya geçersiz proxy ayarları; omsagent-*RPM*. sh PAKETLERI yalnızca RPM tabanlı sistemlere yüklenebilir ve omsagent-*Deb*. sh paketleri yalnızca, detem tabanlı sistemlere yüklenebilir. [En son sürümden](../../azure-monitor/learn/quick-collect-linux-computer.md#install-the-agent-for-linux)evrensel yükleyiciyi kullanmanız önerilir. Ayrıca, proxy ayarlarınızı doğrulamak için gözden geçirin. |
| 5 | Kabuk paket kök olarak yürütülmelidir veya ekleme sırasında döndürülen 403 hatası oluştu. `sudo`kullanarak komutunuz çalıştırın. |
| 6 | Geçersiz Paket mimarisi veya ekleme sırasında 200 hatası döndürüldü. omsagent-*x64.sh paketleri yalnızca 64 bitlik sistemlere yüklenebilir ve omsagent-* x86.sh paketleri yalnızca 32 bit sistemlere yüklenebilir. [En son sürümden](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/latest)mimariniz için doğru paketi indirin. |
| 17 | OMS paketi yüklemesi başarısız oldu. Komut çıktısı kök hatasına bakın. |
| 19 | OMI paket yüklemesi başarısız oldu. Komut çıktısı kök hatasına bakın. |
| 20 | SCX paket yüklemesi başarısız oldu. Komut çıktısı kök hatasına bakın. |
| 21 | Sağlayıcı Setleri yüklemesi başarısız oldu. Komut çıktısı kök hatasına bakın. |
| 22 | İle birlikte gelen paket yüklemesi başarısız oldu. Komut çıktısı kök hatasına bakın |
| 23 | SCX veya OMI paket zaten yüklü. Kabuk paketini yüklemek için `--install` yerine `--upgrade` kullanın. |
| 30 | Paket iç hata oluştu. Çıkışındaki ayrıntılarla ilgili bir [GitHub sorunu verin](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) . |
| 55 | Desteklenmeyen OpenSSL sürümü veya Azure Izleyici 'ye bağlanılamıyor ya da dpkg, kilitli veya eksik bir kıvrımlı programdır. |
| 61 | Python ctypes kitaplığı eksik. Python ctypes kitaplığı veya paket (python ctypes) yükleyin. |
| 62 | Eksik tar programı, yükleme hedefi. |
| 63 | Yükleme sed. eksik sed programı |
| 64 | Eksik curl programı, yükleme curl. |
| 65 | Eksik gpg programı, yükleme gpg. |

## <a name="onboarding-error-codes"></a>Onboarding hata kodları

| Hata Kodu | Anlamı |
| --- | --- |
| 2 | Geçersiz seçenek omsadmin betiğe sağlanan. Kullanım için `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -h` çalıştırın. |
| 3 | Omsadmin betiği için sağlanan geçersiz yapılandırma. Kullanım için `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -h` çalıştırın. |
| 4 | Geçersiz proxy omsadmin betiğe sağlanan. Proxy 'yi doğrulayın ve [BIR http proxy 'si kullanmaya yönelik belgelerimizi](log-analytics-agent.md#network-firewall-requirements)görüntüleyin. |
| 5 | Azure Izleyici 'den 403 HTTP hatası alındı. Ayrıntılar için omsadmin betik tam çıktıyı görmek. |
| 6 | Azure Izleyici 'den 200 olmayan HTTP hatası alındı. Ayrıntılar için omsadmin betik tam çıktıyı görmek. |
| 7 | Azure Izleyici ile bağlantı kurulamıyor. Ayrıntılar için omsadmin betik tam çıktıyı görmek. |
| 8 | Log Analytics çalışma alanına ekleme hatası. Ayrıntılar için omsadmin betik tam çıktıyı görmek. |
| 30 | İç komut dosyası hata. Çıkışındaki ayrıntılarla ilgili bir [GitHub sorunu verin](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) . |
| 31 | Hata oluşturma Aracısı kimliği Çıkışındaki ayrıntılarla ilgili bir [GitHub sorunu verin](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) . |
| 32 | Sertifika oluşturulurken bir hata oluştu. Ayrıntılar için omsadmin betik tam çıktıyı görmek. |
| 33 | Omsconfig için metaconfiguration oluşturulurken bir hata oluştu. Çıkışındaki ayrıntılarla ilgili bir [GitHub sorunu verin](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) . |
| 34 | Metaconfiguration oluşturma betiği mevcut değil. `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -w <Workspace ID> -s <Workspace Key>`ile ekleme işlemini yeniden deneyin. |

## <a name="enable-debug-logging"></a>Hata ayıklama günlük kaydını etkinleştirme
### <a name="oms-output-plugin-debug"></a>OMS çıkış eklenti hata ayıklama
 FluentD sağlayarak eklentisi özgü günlük düzeylerini giriş ve çıkışları için farklı günlük düzeyleri belirtmek için sağlar. OMS çıkışı için farklı bir günlük düzeyi belirtmek üzere `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`adresindeki genel aracı yapılandırmasını düzenleyin.  

 OMS çıkış eklentisinde, yapılandırma dosyasının sonundan önce, `info` `log_level` özelliğini `debug`olarak değiştirin:

 ```
 <match oms.** docker.**>
  type out_oms
  log_level debug
  num_threads 5
  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
 ```

Hata ayıklama günlüğü, Azure Izleyici 'ye türe, veri öğelerinin sayısına ve gönderilmek üzere harcanan zamana göre ayrılmış toplu karşıya yüklemeleri görmenizi sağlar:

*Örnek hata ayıklama etkin günlüğü:*

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

### <a name="verbose-output"></a>Ayrıntılı çıkış
OMS çıkış eklentisini kullanmak yerine, veri öğelerini doğrudan `stdout`, Linux için Log Analytics Aracısı günlük dosyasında görünür hale getirebilirsiniz.

`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`genel aracı yapılandırma dosyasında, her satırın önüne bir `#` ekleyerek OMS çıkış eklentisine açıklama ekleyin: Log Analytics

```
#<match oms.** docker.**>
#  type out_oms
#  log_level info
#  num_threads 5
#  buffer_chunk_limit 5m
#  buffer_type file
#  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms*.buffer
#  buffer_queue_limit 10
#  flush_interval 20s
#  retry_limit 10
#  retry_wait 30s
#</match>
```

Çıkış eklentisinin altında, her satırın önündeki `#` kaldırarak aşağıdaki bölümün açıklamasını kaldırın:

```
<match **>
  type stdout
</match>
```

## <a name="issue--unable-to-connect-through-proxy-to-azure-monitor"></a>Sorun: proxy ile Azure Izleyici arasında bağlantı kurulamıyor

### <a name="probable-causes"></a>Olası nedenleri
* Ekleme sırasında belirtilen proxy yanlış
* Azure Izleyici ve Azure Otomasyonu hizmet uç noktaları, veri merkezinizde beyaz listede değil 

### <a name="resolution"></a>Çözüm
1. `-v` etkin seçeneği ile aşağıdaki komutu kullanarak, Linux için Log Analytics aracısıyla Azure Izleyici 'ye yeniden giriş yapın. Proxy aracılığıyla Azure Izleyici 'ye bağlanan aracının ayrıntılı çıkışının yapılmasına izin verir. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <Workspace ID> -s <Workspace Key> -p <Proxy Conf> -v`

2. Aracıyı bir ara sunucu üzerinden iletişim kurmak üzere doğru şekilde yapılandırdığınızdan emin olmak için [proxy ayarlarını güncelleştirme](agent-manage.md#update-proxy-settings) bölümüne bakın.    
* Aşağıdaki Azure Izleyici uç noktalarının beyaz listelenmiş olduğunu iki kez denetleyin:

    |Aracı Kaynağı| Bağlantı Noktaları | Yön |
    |------|---------|----------|  
    |*.ods.opinsights.azure.com | Bağlantı noktası 443| Gelen ve giden |  
    |*.oms.opinsights.azure.com | Bağlantı noktası 443| Gelen ve giden |  
    |*.blob.core.windows.net | Bağlantı noktası 443| Gelen ve giden |  

    Ortamınızdaki runbook 'ları veya yönetim çözümlerini kullanmak üzere otomasyon hizmetine bağlanmak ve kaydolmak için Azure Otomasyonu karma Runbook Worker kullanmayı planlıyorsanız, bağlantı noktası numarasına ve [ağınızı karma Runbook Worker Için yapılandırma](../../automation/automation-hybrid-runbook-worker.md#network-planning)bölümünde açıklanan URL 'lere erişimi olmalıdır. 

## <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Sorun: Bir 403 hatası ekleme çalışırken alıyorsunuz

### <a name="probable-causes"></a>Olası nedenleri
* Tarih ve saat Linux sunucusu üzerinde yanlış 
* Çalışma alanı kimliği ve çalışma alanı anahtarı doğru değil

### <a name="resolution"></a>Çözüm

1. Komut tarih ile Linux sunucunuzdaki zamanını kontrol edin. Saati geçerli saatten 15 dakika +/-ise, ardından ekleme başarısız olur. İçin doğru Bu güncelleştirme tarih ve/veya saat dilimi Linux sunucunuzun. 
2. Linux için Log Analytics aracısını en son sürümünü yüklediğinizi doğrulayın.  En yeni sürümü artık zaman farkı, onboarding hataya neden olduğunu bildirir.
3. Doğru çalışma alanı Kimliğiniz ve çalışma alanı anahtarı bu makalenin önceki kısımlarında yükleme yönergelerini kullanarak Reonboard.

## <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Sorun: Sağ ekledikten sonra bir 500 ve 404 hatası günlük dosyasına bakın
Log Analytics çalışma alanına ilk Linux veri yükleme oluşma zamanı bilinen bir sorundur. Bu, gönderilen veya hizmet deneyimi olan verileri etkilemez.


## <a name="issue-you-see-omiagent-using-100-cpu"></a>Sorun: omıagent 'ı %100 CPU 'SU ile görüyorsunuz

### <a name="probable-causes"></a>Olası nedenleri
NSS-pek paketi [v 1.0.3 -5. EL7](https://centos.pkgs.org/7/centos-x86_64/nss-pem-1.0.3-7.el7.x86_64.rpm.html) ' de bir gerileme, önemli bir performans sorununa neden oldu. Bu, Redhat/CentOS 7. x dağıtımları içinde çok fazla şey görüyor. Bu sorun hakkında daha fazla bilgi edinmek için, aşağıdaki belgelere bakın: hata [1667121 performans gerileme, Libböcek](https://bugzilla.redhat.com/show_bug.cgi?id=1667121).

Performansla ilgili hatalar her zaman gerçekleşmez ve yeniden oluşturulması çok zordur. Omıagent ile ilgili bir sorunla karşılaşırsanız, belirli bir eşiği aşdığınızda omıagent yığın izlemesini toplayacak olan betik omiHighCPUDiagnostics.sh kullanmanız gerekir.

1. Betiği indir <br/>
`wget https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/tools/LogCollector/source/omiHighCPUDiagnostics.sh`

2. Tanılamayı %30 saat sonra %30 CPU eşiğine göre Çalıştır <br/>
`bash omiHighCPUDiagnostics.sh --runtime-in-min 1440 --cpu-threshold 30`

3. Omiagent_trace dosyasında Callstack dökülecektir, çok sayıda kıvrımlı ve NSS işlev çağrısı fark ederseniz aşağıdaki çözüm adımlarını izleyin.

### <a name="resolution-step-by-step"></a>Çözüm (adım adım)

1. NSS-pek paketini [v 1.0.3-5. el7_6.1](https://centos.pkgs.org/7/centos-x86_64/nss-pem-1.0.3-7.el7.x86_64.rpm.html)' e yükseltin. <br/>
`sudo yum upgrade nss-pem`

2. NSS-PEI yükseltme için uygun değilse (genellikle CentOS üzerinde gerçekleşir), daha sonra 7.29.0-46 olarak kıvma düşürme olur. "Yıum Güncelleştir" i çalıştırırsanız, 7.29.0-51 ' e yükseltilecektir ve sorun yeniden gerçekleşir. <br/>
`sudo yum downgrade curl libcurl`

3. OMı 'yi yeniden Başlat: <br/>
`sudo scxadmin -restart`

## <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Sorun: Azure portalında herhangi bir veri görmediğinizden

### <a name="probable-causes"></a>Olası nedenleri

- Azure Izleyici 'ye ekleme başarısız oldu
- Azure Izleyici bağlantısı engellendi
- Linux veri için log Analytics aracısını yedeklenir

### <a name="resolution"></a>Çözüm
1. Aşağıdaki dosyanın mevcut olup olmadığını denetleyerek, ekleme Azure Izleyici 'nin başarılı olup olmadığını denetleyin: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. `omsadmin.sh` komut satırı yönergelerini kullanarak yeniden ekleme
3. Bir ara sunucu kullanıyorsanız, daha önce sağlanan proxy çözümleme adımlarına bakın.
4. Linux için Log Analytics aracısını hizmetiyle iletişim kuramadığında bazı durumlarda, aracı üzerinde veri 50 MB'tır tam arabellek boyutu için sıraya alınır. Aracı şu komut çalıştırılarak yeniden başlatılmalıdır: `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`. 

    >[!NOTE]
    >Aracı sürümü 1.1.0-28 ve daha sonra bu sorun düzeltilmiştir.


## <a name="issue-you-are-not-seeing-forwarded-syslog-messages"></a>Sorun: İletilen Syslog iletilerini görmediğinizden 

### <a name="probable-causes"></a>Olası nedenleri
* Linux sunucusuna uygulanan yapılandırma gönderilen özellikleri ve/veya günlük düzeyleri koleksiyonunu izin vermiyor
* Syslog doğru bir şekilde Linux sunucusuna iletilmez değil
* Saniye başına iletilen ileti sayısını işlemek Linux için Log Analytics aracısını temel yapılandırması için çok büyük

### <a name="resolution"></a>Çözüm
* Syslog yapılandırmasını Log Analytics çalışma alanındaki tüm özellikleri ve doğru günlük düzeyleri olduğunu doğrulayın. [Azure Portal Syslog koleksiyonunu yapılandırma '](../../azure-monitor/platform/data-sources-syslog.md#configure-syslog-in-the-azure-portal) yı gözden geçirin
* Yerel Syslog mesajlaşma Daemon 'ları (`rsyslog`, `syslog-ng`) iletilen iletileri alabildiğini doğrulama
* İletileri engellenmediğinden emin olmak için Syslog sunucusunda güvenlik duvarı ayarlarını kontrol edin
* `logger` komutunu kullanarak Log Analytics Syslog iletisinin benzetimini yapın
  * `logger -p local0.err "This is my test message"`

## <a name="issue-you-are-receiving-errno-address-already-in-use-in-omsagent-log-file"></a>Sorun: Errno adresi zaten kullanımda omsagent günlük dosyası görüntüleniyor
Omsagent. log içinde `[error]: unexpected error error_class=Errno::EADDRINUSE error=#<Errno::EADDRINUSE: Address already in use - bind(2) for "127.0.0.1" port 25224>` görüyorsanız.

### <a name="probable-causes"></a>Olası nedenleri
Linux tanılama uzantısı (LAD) Log Analytics Linux VM uzantısı ile yan yana yüklenir ve syslog verileri toplama omsagent olarak aynı bağlantı noktasını kullanıyorsa bu hatayı gösterir.

### <a name="resolution"></a>Çözüm
1. Kök olarak (25224 bir örnektir ve ortamınızda LAD tarafından kullanılan farklı bir bağlantı noktası gördüğünüzü mümkündür. Not) aşağıdaki komutları yürütün:

    ```
    /opt/microsoft/omsagent/bin/configure_syslog.sh configure LAD 25229

    sed -i -e 's/25224/25229/' /etc/opt/microsoft/omsagent/LAD/conf/omsagent.d/syslog.conf
    ```

    Ardından doğru `rsyslogd` veya `syslog_ng` yapılandırma dosyasını düzenlemeniz ve 25229 numaralı bağlantı noktasına yazmak üzere LAD ile ilgili yapılandırmayı değiştirmeniz gerekir.

2. VM `rsyslogd`çalışıyorsa, değiştirilecek dosya: `/etc/rsyslog.d/95-omsagent.conf` (varsa, değilse `/etc/rsyslog`). VM `syslog_ng`çalışıyorsa, değiştirilecek dosya: `/etc/syslog-ng/syslog-ng.conf`.
3. Omsagent `sudo /opt/microsoft/omsagent/bin/service_control restart`yeniden başlatın.
4. Syslog hizmeti yeniden başlatın.

## <a name="issue-you-are-unable-to-uninstall-omsagent-using-purge-option"></a>Sorun: Omsagent Temizleme seçeneğini kullanarak kaldırmak oluşturulamıyor

### <a name="probable-causes"></a>Olası nedenleri

* Linux tanılama uzantısı yüklenir
* Linux tanılama uzantısı yüklenir ve kaldırılır, ancak yine de mdsd tarafından kullanılan omsagent hakkında bir hata iletisi görür ve kaldırılamaz.

### <a name="resolution"></a>Çözüm
1. Linux tanılama uzantısı (LAD) kaldırın.
2. Linux Tanılama uzantısı dosyalarını şu konumda mevcutsa makineden kaldırın: `/var/lib/waagent/Microsoft.Azure.Diagnostics.LinuxDiagnostic-<version>/` ve `/var/opt/microsoft/omsagent/LAD/`.

## <a name="issue-you-cannot-see-data-any-nagios-data"></a>Sorun: Veri Nagios verileri göremez 

### <a name="probable-causes"></a>Olası nedenleri
* Omsagent kullanıcı Nagios günlük dosyasından okumak için gerekli izinlere sahip değil
* Nagios kaynak ve filtre omsagent.conf dosyasından açıklamalı olmayan verilmemiş

### <a name="resolution"></a>Çözüm
1. Aşağıdaki [yönergeleri](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts)Izleyerek Nagıos dosyasından okumak için omsagent kullanıcısı ekleyin.
2. Linux genel yapılandırma dosyası için Log Analytics Aracısı `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`adresinde, Nagios kaynak ve filtresinin **her ikisinin de** açıklama kaldırmamakta olduğundan emin olun.

    ```
    <source>
      type tail
      path /var/log/nagios/nagios.log
      format none
      tag oms.nagios
    </source>

    <filter oms.nagios>
      type filter_nagios_log
    </filter>
    ```

## <a name="issue-you-are-not-seeing-any-linux-data"></a>Sorun: Herhangi bir Linux veri görmediğinizden 

### <a name="probable-causes"></a>Olası nedenleri
* Azure Izleyici 'ye ekleme başarısız oldu
* Azure Izleyici bağlantısı engellendi
* Sanal makine yeniden başlatıldı
* OMI paket el ile karşılaştırıldığında ne Linux paket için Log Analytics aracısını tarafından yüklü olduğu için daha yeni bir sürüme yükseltildi
* `omsconfig.log` günlük dosyasında DSC kaynak günlükleri *sınıfı bulunamadı* hatası
* Verilerin log Analytics aracısını yedeklenir
* DSC günlükleri *geçerli yapılandırması yok. Bir yapılandırma dosyası belirtmek ve önce geçerli bir yapılandırma oluşturmak için-Path parametresiyle birlikte start-DscConfiguration komutunu yürütün.* `omsconfig.log` günlük dosyasında, ancak `PerformRequiredConfigurationChecks` işlemler hakkında bir günlük iletisi yok.

### <a name="resolution"></a>Çözüm
1. Auditd paket gibi tüm bağımlılıkları yükleyin.
2. Aşağıdaki dosyanın mevcut olup olmadığını denetleyerek Azure Izleyici 'ye ekleme işleminin başarılı olup olmadığını denetleyin: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`.  Aksi takdirde, omsadmin.sh komut satırı [yönergeleri](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line)kullanılarak yeniden ekleyin.
4. Bir ara sunucu kullanıyorsanız, proxy sorun giderme adımları yukarıdaki denetleyin.
5. Sanal makine yeniden başlatıldıktan sonra bazı Azure dağıtım sistemlerinde OMID OMI sunucusu arka plan programı başlamıyor. Bu denetim, değişiklik izleme veya UpdateManagement çözümü ile ilgili verileri görmüyor içinde neden olur. Geçici çözüm, `sudo /opt/omi/bin/service_control restart`çalıştırarak OMI sunucusunu el ile başlatsın.
6. OMI paket el ile yeni bir sürüme yükselttikten sonra çalışmaya devam etmesi için Log Analytics aracısını el ile başlatılması gerekir. Bu adım yükseltildikten sonra nerede OMI sunucusuna otomatik olarak başlamaz bazı işlemleri için gereklidir. OMı 'yi yeniden başlatmak için `sudo /opt/omi/bin/service_control restart` çalıştırın.
7. Omsconfig. log dosyasında DSC kaynak *sınıfı bulunamadı* hatası görürseniz, `sudo /opt/omi/bin/service_control restart`çalıştırın.
8. Bazı durumlarda, Linux için Log Analytics Aracısı Azure Izleyici ile iletişim kuramadığı zaman aracıdaki veriler tam arabellek boyutuna yedeklenir: 50 MB. Aşağıdaki komut `/opt/microsoft/omsagent/bin/service_control restart`çalıştırılarak aracının yeniden başlatılması gerekir.

    >[!NOTE]
    >Aracı sürümü 1.1.0-28 veya daha sonra bu sorun düzeltilene
    >

* `omsconfig.log` günlük dosyası `PerformRequiredConfigurationChecks` işlemlerinin sistemde düzenli olarak çalıştığını belirtmezse, cron işi/hizmeti ile ilgili bir sorun olabilir. `/etc/cron.d/OMSConsistencyInvoker`altında cron işinin bulunduğundan emin olun. Sıralanmış iş oluşturmak için aşağıdaki komutları çalıştırmanız varsa:

    ```
    mkdir -p /etc/cron.d/
    echo "*/15 * * * * omsagent /opt/omi/bin/OMSConsistencyInvoker >/dev/null 2>&1" | sudo tee /etc/cron.d/OMSConsistencyInvoker
    ```

    Ayrıca cron hizmetinin çalıştığından emin olun. Bu hizmetin durumunu denetlemek için debir, Ubuntu, SUSE veya `service crond status` RHEL, CentOS Oracle Linux ile `service cron status` kullanabilirsiniz. Hizmet yok, ikili dosyaları yüklemek ve aşağıdakileri kullanarak hizmeti başlatın:

    **Ubuntu/dene**

    ```
    # To Install the service binaries
    sudo apt-get install -y cron
    # To start the service
    sudo service cron start
    ```

    **SUSE**

    ```
    # To Install the service binaries
    sudo zypper in cron -y
    # To start the service
    sudo systemctl enable cron
    sudo systemctl start cron
    ```

    **RHEL/CeonOS**

    ```
    # To Install the service binaries
    sudo yum install -y crond
    # To start the service
    sudo service crond start
    ```

    **Oracle Linux**

    ```
    # To Install the service binaries
    sudo yum install -y cronie
    # To start the service
    sudo service crond start
    ```

## <a name="issue-when-configuring-collection-from-the-portal-for-syslog-or-linux-performance-counters-the-settings-are-not-applied"></a>Sorun: Koleksiyon Syslog ya da Linux performans sayaçları için portalından yapılandırırken ayarları uygulanmaz

### <a name="probable-causes"></a>Olası nedenleri
* Linux için Log Analytics aracısını en son yapılandırmayı çekilen değil
* Portalda değiştirilen ayarlar uygulanmadı

### <a name="resolution"></a>Çözüm
**Arka plan:** `omsconfig`, beş dakikada bir yeni Portal-tarafı yapılandırma için arama yapan Linux yapılandırma aracısına yönelik Log Analytics aracısıdır. Bu yapılandırma, ardından /etc/opt/microsoft/omsagent/conf/omsagent.conf bulunan Linux yapılandırma dosyaları için Log Analytics aracısını uygulanır.

* Bazı durumlarda, Linux yapılandırma aracı için Log Analytics aracısını en son yapılandırma uygulanmıyor portal yapılandırması hizmet ile iletişim kurmak mümkün olmayabilir.
  1. `dpkg --list omsconfig` veya `rpm -qi omsconfig`çalıştırarak `omsconfig` aracısının yüklü olduğundan emin olun.  Yüklenmezse, Linux için Log Analytics aracısını en son sürümünü yeniden yükleyin.

  2. `omsconfig` aracısının Azure Izleyici ile iletişim kurabildiğini denetleyin `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'`aşağıdaki komutu çalıştırın. Bu komut, bu aracı yapılandırmasını döndürür Syslog ayarları, Linux performans sayaçları ve özel günlükler de dahil olmak üzere hizmetinden alır. Bu komut başarısız olursa, aşağıdaki komutu `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py'`çalıştırın. Bu komut omsmsconfig aracısının Azure Izleyici ile konuştuğunu ve en son yapılandırmayı almasına zorlar.

## <a name="issue-you-are-not-seeing-any-custom-log-data"></a>Sorun: Herhangi bir özel günlük veri görmediğinizden 

### <a name="probable-causes"></a>Olası nedenleri
* Azure Izleyici 'ye ekleme başarısız oldu.
* Bu ayar **Linux sunucularıma aşağıdaki yapılandırmayı uygular** .
* omsconfig en son özel günlük yapılandırması hizmetinden çekilen değil.
* Linux Kullanıcı `omsagent` Log Analytics Aracısı, izinler nedeniyle özel günlüğe erişemiyor veya bulunamadı.  Aşağıdaki iletileri görebilirsiniz:
 * `[DATETIME] [warn]: file not found. Continuing without tailing it.`
 * `[DATETIME] [error]: file not accessible by omsagent.`
* Bilinen sorun ile Linux sürümü 1.1.0-217 için Log Analytics aracısını düzeltilen yarış durumu

### <a name="resolution"></a>Çözüm
1. Şu dosyanın mevcut olup olmadığını kontrol ederek Azure Izleyici 'ye ekleme işleminin başarılı olduğunu doğrulayın: `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`. Değilse, ya da varsa:  

  1. Omsadmin.sh komut satırı [yönergeleri](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line)kullanılarak yeniden ekleme.
  2. Azure portal **Gelişmiş ayarlar** altında, **aşağıdaki yapılandırmayı Linux sunucularıma Uygula** ayarının etkinleştirildiğinden emin olun.  

2. `omsconfig` aracısının Azure Izleyici ile iletişim kurabildiğini denetleyin `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'`aşağıdaki komutu çalıştırın.  Bu komut, bu aracı yapılandırmasını döndürür Syslog ayarları, Linux performans sayaçları ve özel günlükler de dahil olmak üzere hizmetinden alır. Bu komut başarısız olursa, aşağıdaki komutu `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py'`çalıştırın. Bu komut omsmsconfig aracısının Azure Izleyici ile konuştuğunu ve en son yapılandırmayı almasına zorlar.

**Arka plan:** Ayrıcalıklı kullanıcı `root`olarak çalışan Linux için Log Analytics Aracısı yerine, aracı `omsagent` Kullanıcı olarak çalışır. Çoğu durumda, okumak için bu kullanıcı için belirli dosyaları için sırayla yönelik açık izinlerinin verilmesi gerekir. `omsagent` kullanıcıya izin vermek için aşağıdaki komutları çalıştırın:

1. `omsagent` kullanıcıyı belirli bir gruba ekleyin `sudo usermod -a -G <GROUPNAME> <USERNAME>`
2. Gerekli dosyaya evrensel okuma erişimi verin `sudo chmod -R ugo+rx <FILE DIRECTORY>`

Bir yarış durumu ile Log Analytics aracısını 1.1.0-217'den önceki Linux sürümü için bilinen bir sorun yoktur. En son aracıya güncelleştirdikten sonra, `sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`çıkış eklentisinin en son sürümünü almak için aşağıdaki komutu çalıştırın.

## <a name="issue-you-are-trying-to-reonboard-to-a-new-workspace"></a>Sorun: Yeni bir çalışma alanına reonboard çalıştığınız
Yeni bir çalışma alanı için bir aracı için reonboard çalıştığınızda, Log Analytics Aracısı yapılandırması önce reonboarding temizlenmesi gerekir. Aracıdan eski yapılandırmayı temizlemek için, kabuk paketini `--purge` ile çalıştırın

```
sudo sh ./omsagent-*.universal.x64.sh --purge
```
Veya

```
sudo sh ./onboard_agent.sh --purge
```

`--purge` seçeneğini kullanarak yeniden eklemeye devam edebilirsiniz

## <a name="log-analytics-agent-extension-in-the-azure-portal-is-marked-with-a-failed-state-provisioning-failed"></a>Azure portalında log Analytics Aracısı uzantısı durumu ile başarısız olarak işaretlenir: sağlama başarısız oldu

### <a name="probable-causes"></a>Olası nedenleri
* Log Analytics aracısını işletim sisteminden kaldırıldı
* Log Analytics Aracısı hizmeti kapalı, devre dışı veya yapılandırılmamış

### <a name="resolution"></a>Çözüm 
Bu sorunu çözmek için aşağıdaki adımları gerçekleştirin.
1. Azure Portalı'ndan uzantısını kaldırın.
2. [Yönergeleri](../../azure-monitor/learn/quick-collect-linux-computer.md)izleyerek aracıyı yükler.
3. Şu komutu çalıştırarak aracıyı yeniden başlatın: `sudo /opt/microsoft/omsagent/bin/service_control restart`.
* Birkaç dakika bekleyin ve sağlama durumu, **sağlama başarılı**olarak değişir.


## <a name="issue-the-log-analytics-agent-upgrade-on-demand"></a>Sorun: Log Analytics aracısını isteğe bağlı yükseltme

### <a name="probable-causes"></a>Olası nedenleri

Log Analytics aracısını paketleri konaktaki geçmiştir.

### <a name="resolution"></a>Çözüm 
Bu sorunu çözmek için aşağıdaki adımları gerçekleştirin.

1. [Sayfada](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/)en son sürümü denetleyin.
2. Yükleme betiğini indirin (1.4.2-124 örnek sürüm olarak):

    ```
    wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.2-124/omsagent-1.4.2-124.universal.x64.sh
    ```

3. `sudo sh ./omsagent-*.universal.x64.sh --upgrade`yürüterek paketleri yükseltin.

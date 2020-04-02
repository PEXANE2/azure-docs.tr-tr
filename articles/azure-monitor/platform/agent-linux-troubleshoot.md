---
title: Sorun Giderme Azure Log Analytics Linux Agent | Microsoft Dokümanlar
description: Azure Monitor'da Linux için Log Analytics aracısıyla en sık karşılaşılan sorunların belirtilerini, nedenlerini ve çözümünü açıklayın.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/21/2019
ms.openlocfilehash: 2343de97d06abdefed2c2977a7341aa411429319
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80520735"
---
# <a name="how-to-troubleshoot-issues-with-the-log-analytics-agent-for-linux"></a>Linux için Log Analytics aracısıyla ilgili sorunları giderme 

Bu makalede, Azure Monitor'da Linux için Log Analytics aracısıyla karşılaşabileceğiniz sorun giderme hataları sağlar ve bunları çözmek için olası çözümler önerir.

Bu adımlardan hiçbiri sizin için çalışmıyorsa, aşağıdaki destek kanalları da kullanılabilir:

* Premier destek avantajları olan müşteriler [Premier](https://premier.microsoft.com/)ile bir destek isteği açabilir.
* Azure destek anlaşmaları olan [müşteriler, Azure portalında](https://manage.windowsazure.com/?getsupport=true)bir destek isteği açabilir.
* [OMI sorun giderme kılavuzuyla](https://github.com/Microsoft/omi/blob/master/Unix/doc/diagnose-omi-problems.md)OMI Sorunlarını tanıla.
* Bir [GitHub Sorunu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues)dosya.
* Gönderilen fikirleri ve hataları [https://aka.ms/opinsightsfeedback](https://aka.ms/opinsightsfeedback) incelemek veya yeni sini dosyalamak için Günlük Analitiği Geri Bildirimi sayfasını ziyaret edin.  

## <a name="important-log-locations-and-log-collector-tool"></a>Önemli günlük konumları ve Log Collector aracı

 Dosya | Yol
 ---- | -----
 Linux günlük dosyası için Log Analytics aracısı | `/var/opt/microsoft/omsagent/<workspace id>/log/omsagent.log`
 Log Analytics aracısı yapılandırma günlüğü dosyası | `/var/opt/microsoft/omsconfig/omsconfig.log`

 Sorun giderme için önemli günlükleri almak veya Bir GitHub sorunu göndermeden önce günlük toplayıcı aracımızı kullanmanızı öneririz. Araç hakkında daha fazla bilgi edinebilirsiniz ve nasıl [burada](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/tools/LogCollector/OMS_Linux_Agent_Log_Collector.md)çalıştırmak için.

## <a name="important-configuration-files"></a>Önemli yapılandırma dosyaları

 Kategori | Dosya Konumu
 ----- | -----
 Syslog | `/etc/syslog-ng/syslog-ng.conf`veya `/etc/rsyslog.conf` veya`/etc/rsyslog.d/95-omsagent.conf`
 Performans, Nagios, Zabbix, Log Analytics çıktısı ve genel ajan | `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`
 Ek yapılandırmalar | `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/*.conf`

 >[!NOTE]
 >Performans sayaçları ve Syslog için yapılandırma dosyalarını düzenleme, koleksiyon çalışma alanınız için Azure portalındaki [Veri Menüsü'nden](../../azure-monitor/platform/agent-data-sources.md#configuring-data-sources) Log Analytics Advanced Settings yapılandırılırsa üzerine yazılır. Tüm aracılar için yapılandırmayı devre dışı kardık için Log Analytics **Advanced Settings** veya tek bir temsilci nin tahsilatını devre dışlamak için şeyi:  
> `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/OMS_MetaConfigHelper.py --disable'`

## <a name="installation-error-codes"></a>Yükleme hata kodları

| Hata Kodu | Anlamı |
| --- | --- |
| NOT_DEFINED | Gerekli bağımlılıklar yüklenmediği için, auoms denetlenen eklenti yüklenmez | Auoms kurulumu başarısız oldu, paket denetimli yükleyin. |
| 2 | Kabuk paketine sağlanan geçersiz seçenek. Kullanım `sudo sh ./omsagent-*.universal*.sh --help` için çalıştırın |
| 3 | Kabuk paketine seçenek sunulmaz. Kullanım `sudo sh ./omsagent-*.universal*.sh --help` için çalıştırın. |
| 4 | Geçersiz paket türü VEYA geçersiz proxy ayarları; omsagent-*rpm*.sh paketleri yalnızca RPM tabanlı sistemlere, omsagent-*deb*.sh paketleri ise yalnızca Debian tabanlı sistemlere yüklenebilir. Bu [son sürümünden](../../azure-monitor/learn/quick-collect-linux-computer.md#install-the-agent-for-linux)evrensel yükleyici kullanmanız tavsiye edilir. Proxy ayarlarınızı doğrulamak için de gözden geçirin. |
| 5 | Kabuk demeti kök olarak yürütülmelidir VEYA biniş sırasında döndürülen 403 hata vardı. Komutunuzu kullanarak `sudo`çalıştırın. |
| 6 | Geçersiz paket mimarisi VEYA onboarding sırasında döndürülen hata 200 hata vardı; omsagent-*x64.sh paketleri sadece 64-bit sistemlere, omsagent-x86.sh*paketleri ise sadece 32-bit sistemlere kurulabilir. En son sürümden mimariniz için doğru paketi [indirin.](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/latest) |
| 17 | OMS paketinin yüklenmesi başarısız oldu. Kök hatası için komut çıktısı üzerinden bakın. |
| 19 | OMI paketinin yüklenmesi başarısız oldu. Kök hatası için komut çıktısı üzerinden bakın. |
| 20 | SCX paketinin yüklenmesi başarısız oldu. Kök hatası için komut çıktısı üzerinden bakın. |
| 21 | Sağlayıcı kitleri yükleme başarısız oldu. Kök hatası için komut çıktısı üzerinden bakın. |
| 22 | Paketlenmiş paketin yüklenmesi başarısız oldu. Kök hatası için komut çıktısı üzerinden bakın |
| 23 | SCX veya OMI paketi zaten yüklü. Kabuk `--upgrade` paketini yüklemek `--install` yerine kullanın. |
| 30 | İç paket hatası. Çıktıdaki ayrıntıları içeren bir [GitHub Sorunu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) dosyala. |
| 55 | Desteklenmeyen openssl sürümü VEYA Azure Monitor'a bağlanamıyor VEYA dpkg kilitli veya eksik kıvırma programı. |
| 61 | Eksik Python ctypes kitaplığı. Python ctypes kitaplığını veya paketini (python-ctypes) yükleyin. |
| 62 | Eksik katran programı, katran yükleyin. |
| 63 | Eksik sed programı, sed yükleyin. |
| 64 | Eksik kıvırma programı, kıvırmak yükleyin. |
| 65 | Eksik gpg programı, gpg yükleyin. |

## <a name="onboarding-error-codes"></a>Onboarding hata kodları

| Hata Kodu | Anlamı |
| --- | --- |
| 2 | Omsadmin komut dosyasına sağlanan geçersiz seçenek. Kullanım `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -h` için çalıştırın. |
| 3 | Omsadmin komut dosyasına sağlanan geçersiz yapılandırma. Kullanım `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -h` için çalıştırın. |
| 4 | Geçersiz proxy omsadmin komut dosyasına sağlanan. Proxy'yi doğrulayın ve [http proxy kullanmak için belgelerimize](log-analytics-agent.md#firewall-requirements)bakın. |
| 5 | Azure Monitor'dan alınan 403 HTTP hatası. Ayrıntılar için omsadmin komut dosyasının tam çıktısını görün. |
| 6 | Azure Monitor'dan alınan 200'lü olmayan HTTP hatası. Ayrıntılar için omsadmin komut dosyasının tam çıktısını görün. |
| 7 | Azure Monitör'e bağlanamıyor. Ayrıntılar için omsadmin komut dosyasının tam çıktısını görün. |
| 8 | Log Analytics çalışma alanına binme hatası. Ayrıntılar için omsadmin komut dosyasının tam çıktısını görün. |
| 30 | İç komut dosyası hatası. Çıktıdaki ayrıntıları içeren bir [GitHub Sorunu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) dosyala. |
| 31 | Hata üreten aracı kimliği. Çıktıdaki ayrıntıları içeren bir [GitHub Sorunu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) dosyala. |
| 32 | Hata üreten sertifikalar. Ayrıntılar için omsadmin komut dosyasının tam çıktısını görün. |
| 33 | Omsconfig için hata üreten metayapılandırma. Çıktıdaki ayrıntıları içeren bir [GitHub Sorunu](https://github.com/Microsoft/OMS-Agent-for-Linux/issues) dosyala. |
| 34 | Metaconfiguration oluşturma komut dosyası mevcut değil. Ile onboarding `sudo sh /opt/microsoft/omsagent/bin/omsadmin.sh -w <Workspace ID> -s <Workspace Key>`yeniden deneyin. |

## <a name="enable-debug-logging"></a>Hata ayıklama günlüğe kaydetmeyi etkinleştirme
### <a name="oms-output-plugin-debug"></a>OMS çıkış eklentisi hata ayıklama
 FluentD, giriş ve çıkışlar için farklı günlük düzeyleri belirtmenize olanak tanıyan eklentiye özel günlük düzeylerine olanak tanır. OMS çıkışı için farklı bir günlük düzeyi belirtmek için `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`genel aracı yapılandırmasını .  

 OMS çıkış eklentisinde, yapılandırma dosyasının bitiminden `log_level` önce, `info` özelliği `debug`aşağıdakilerden

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

Hata ayıklama günlüğü, toplu yüklemelerin Azure Monitor'a türüne, veri öğesi sayısına ve göndermek için geçen süreye göre ayrıldığını görmenizi sağlar:

*Örnek hata ayıklama etkin günlüğü:*

```
Success sending oms.nagios x 1 in 0.14s
Success sending oms.omi x 4 in 0.52s
Success sending oms.syslog.authpriv.info x 1 in 0.91s
```

### <a name="verbose-output"></a>Verbose çıkışı
OMS çıkış eklentisini kullanmak yerine, linux günlük `stdout`dosyası için Log Analytics aracısında görünen veri öğelerini doğrudan çıktı olarak da çıkarabilirsiniz.

Log Analytics genel ajan yapılandırma `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`dosyasında, her satırın önüne `#` bir tane ekleyerek OMS çıktı eklentisini yorumlayın:

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

Çıkış eklentisinin altında, her satırın önündeki `#` niyazları kaldırarak aşağıdaki bölümü yorumsuz bırakın:

```
<match **>
  type stdout
</match>
```

## <a name="issue--unable-to-connect-through-proxy-to-azure-monitor"></a>Sorun: Azure Monitor'a proxy ile bağlanamıyor

### <a name="probable-causes"></a>Olası nedenler
* Onboarding sırasında belirtilen proxy yanlıştı
* Azure Monitörü ve Azure Otomasyon Hizmeti Bitiş Noktaları veri merkezinizde beyaz listeye alınmaz 

### <a name="resolution"></a>Çözüm
1. Aşağıdaki komutu `-v` kullanarak Linux için Log Analytics aracısı ile Azure Monitor'a reonboard seçeneği etkinleştirilmiş. Proxy üzerinden Azure Monitor'a bağlanan aracının ayrıntılı çıktısına izin verir. 
`/opt/microsoft/omsagent/bin/omsadmin.sh -w <Workspace ID> -s <Workspace Key> -p <Proxy Conf> -v`

2. Aracıyı bir proxy sunucusu üzerinden iletişim kurmak için düzgün şekilde yapılandırdığınızı doğrulamak için bölümü [güncelleştir proxy ayarlarını](agent-manage.md#update-proxy-settings) gözden geçirin.    

3. Azure Monitor [ağ güvenlik duvarı gereksinimleri](log-analytics-agent.md#firewall-requirements) listesinde özetlenen uç noktaların izin listesine doğru şekilde eklendiğini iki kez kontrol edin. Azure Otomasyonu kullanıyorsanız, gerekli ağ yapılandırma adımları da yukarıda bağlanır.

## <a name="issue-you-receive-a-403-error-when-trying-to-onboard"></a>Sorun: Gemiye çalışırken 403 hata alırsınız

### <a name="probable-causes"></a>Olası nedenler
* Linux Server'da Tarih ve Saat yanlış 
* Kullanılan Çalışma Alanı Kimliği ve Çalışma Alanı Anahtarı doğru değil

### <a name="resolution"></a>Çözüm

1. Komut tarihi ile Linux sunucunuzdaki saati kontrol edin. Geçerli zamana +/- 15 dakika ise, onboarding başarısız olur. Bu güncelleştirmeyi düzeltmek için Linux sunucunuzun tarihi ve/veya saat dilimi. 
2. Linux için Log Analytics aracısının en son sürümünü yüklediğinizi doğrulayın.  En yeni sürüm, zaman çarpıklığı onboarding arızaya neden oluyorsa şimdi size bunu size iletir.
3. Bu makalenin daha önceki yükleme yönergelerini izleyerek doğru Çalışma Alanı Kimliği ve Çalışma Alanı Anahtarını kullanarak reonboard.

## <a name="issue-you-see-a-500-and-404-error-in-the-log-file-right-after-onboarding"></a>Sorun: Bindikten hemen sonra günlük dosyasında 500 ve 404 hatası görürsünüz
Bu, Linux verilerinin ilk kez log analytics çalışma alanına yüklenmesinde ortaya çıkan bilinen bir sorundur. Bu, gönderilen verileri veya hizmet deneyimini etkilemez.


## <a name="issue-you-see-omiagent-using-100-cpu"></a>Sorun: %100 CPU kullanarak omiagent'ı görürsünüz

### <a name="probable-causes"></a>Olası nedenler
nss-pem paketi [v1.0.3-5.el7](https://centos.pkgs.org/7/centos-x86_64/nss-pem-1.0.3-7.el7.x86_64.rpm.html) bir gerileme ciddi bir performans sorunu neden oldu, biz Redhat / Centos 7.x dağıtımları çok gelip görmek oldum. Bu sorun hakkında daha fazla bilgi edinmek için aşağıdaki belgeleri kontrol edin: Bug [1667121 Libcurl'de performans regresyonu.](https://bugzilla.redhat.com/show_bug.cgi?id=1667121)

Performansla ilgili hataların her zaman gerçekleşmez ve yeniden oluşturulması çok zordur. Eğer omiagent ile böyle bir sorun yaşıyorsanız belirli bir eşik aşıldığında omiagent yığın iz toplayacak komut dosyası omiHighCPUDiagnostics.sh kullanmalısınız.

1. Komut dosyasını indirin <br/>
`wget https://raw.githubusercontent.com/microsoft/OMS-Agent-for-Linux/master/tools/LogCollector/source/omiHighCPUDiagnostics.sh`

2. %30 CPU eşiği yle tanılamayı 24 saat boyunca çalıştırın <br/>
`bash omiHighCPUDiagnostics.sh --runtime-in-min 1440 --cpu-threshold 30`

3. Callstack omiagent_trace dosyaya atılır, Birçok Curl ve NSS fonksiyon çağrıları fark ederseniz, aşağıdaki çözüm adımlarını izleyin.

### <a name="resolution-step-by-step"></a>Çözünürlük (adım adım)

1. nss-pem paketini [v1.0.3-5.el7_6.1'e](https://centos.pkgs.org/7/centos-x86_64/nss-pem-1.0.3-7.el7.x86_64.rpm.html)yükseltin. <br/>
`sudo yum upgrade nss-pem`

2. nss-pem yükseltme için mevcut değilse (çoğunlukla Centos olur), sonra 7.29.0-46 için kıvırmak düşürün. Yanlışlıkla "yum güncelleme" çalıştırırsanız, o zaman kıvırmak 7.29.0-51 yükseltilir ve sorun tekrar olur. <br/>
`sudo yum downgrade curl libcurl`

3. OMI'yi yeniden başlatın: <br/>
`sudo scxadmin -restart`

## <a name="issue-you-are-not-seeing-any-data-in-the-azure-portal"></a>Sorun: Azure portalında veri görmüyorsunuz

### <a name="probable-causes"></a>Olası nedenler

- Azure Monitör'e binme başarısız oldu
- Azure Monitörbağlantısı engellendi
- Linux verileri için Log Analytics aracısı yedeklenir

### <a name="resolution"></a>Çözüm
1. Azure Monitor'da bulunanların aşağıdaki dosyanın var olup olmadığını kontrol ederek başarılı olup olmadığını denetleyin:`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`
2. Komut satırı yönergelerini `omsadmin.sh` kullanarak reonboard
3. Proxy kullanıyorsanız, daha önce sağlanan proxy çözümleme adımlarına bakın.
4. Bazı durumlarda, Linux için Log Analytics aracısı hizmetle iletişim kuramadığı zaman, aracıdaki veriler 50 MB olan tam arabellek boyutuna doğru sıraya alınır. Aracı aşağıdaki komutu çalıştırarak yeniden `/opt/microsoft/omsagent/bin/service_control restart [<workspace id>]`başlatılmalıdır: . 

    >[!NOTE]
    >Bu sorun aracı sürüm 1.1.0-28 ve sonraki düzeltilir.


## <a name="issue-you-are-not-seeing-forwarded-syslog-messages"></a>Sorun: İletilenmiş Syslog iletilerini görmüyorsunuz 

### <a name="probable-causes"></a>Olası nedenler
* Linux sunucusuna uygulanan yapılandırma, gönderilen tesislerin ve/veya günlük düzeylerinin toplanmasına izin vermez
* Syslog Linux sunucusuna doğru iletiliyor değil
* Saniyede iletilen ileti lerin sayısı, Linux için Log Analytics aracısının temel yapılandırmasının işlemesi için çok fazladır

### <a name="resolution"></a>Çözüm
* Syslog için Log Analytics çalışma alanında yapılandırmayı doğrulayın tüm olanaklar ve doğru günlük düzeyleri vardır. [Azure portalında Syslog koleksiyonunu yapılandırmayı](../../azure-monitor/platform/data-sources-syslog.md#configure-syslog-in-the-azure-portal) gözden geçirin
* Yerel syslog mesajlaşma daemons`rsyslog`doğrulayın ( , `syslog-ng`) iletilen iletileri almak mümkün
* İletilerin engellenmediğinden emin olmak için Syslog sunucusundaki güvenlik duvarı ayarlarını denetleme
* Komutu kullanarak Log Analytics'e `logger` bir Syslog iletisi benzetme
  * `logger -p local0.err "This is my test message"`

## <a name="issue-you-are-receiving-errno-address-already-in-use-in-omsagent-log-file"></a>Sorun: Omsagent günlük dosyasında halihazırda kullanılmakta olan Errno adresi alasınız
Omsagent.log'da görürseniz. `[error]: unexpected error error_class=Errno::EADDRINUSE error=#<Errno::EADDRINUSE: Address already in use - bind(2) for "127.0.0.1" port 25224>`

### <a name="probable-causes"></a>Olası nedenler
Bu hata, Linux Tanılama uzantısının (LAD) Log Analytics Linux VM uzantısı ile yan yana yüklenmiş olduğunu ve syslog veri toplama için omsagent olarak aynı bağlantı noktasını kullandığını gösterir.

### <a name="resolution"></a>Çözüm
1. Kök olarak, aşağıdaki komutları uygulayın (25224'ün bir örnek olduğunu ve çevrenizde LAD tarafından kullanılan farklı bir bağlantı noktası numarası görmenizin mümkün olduğunu unutmayın):

    ```
    /opt/microsoft/omsagent/bin/configure_syslog.sh configure LAD 25229

    sed -i -e 's/25224/25229/' /etc/opt/microsoft/omsagent/LAD/conf/omsagent.d/syslog.conf
    ```

    Daha sonra doğru `rsyslogd` veya `syslog_ng` config dosyasını düzenlemeniz ve 25229 portuna yazmak için LAD ile ilgili yapılandırmayı değiştirmeniz gerekir.

2. VM çalışıyorsa, `rsyslogd`değiştirilecek dosya şudur: `/etc/rsyslog.d/95-omsagent.conf` (varsa, `/etc/rsyslog`başka). VM çalışıyorsa, `syslog_ng`değiştirilecek dosya: `/etc/syslog-ng/syslog-ng.conf`.
3. Omsagent'ı `sudo /opt/microsoft/omsagent/bin/service_control restart`yeniden başlatın.
4. Syslog hizmetini yeniden başlatın.

## <a name="issue-you-are-unable-to-uninstall-omsagent-using-purge-option"></a>Sorun: Temizleme seçeneğini kullanarak omsagent'ı kaldıramazsınız

### <a name="probable-causes"></a>Olası nedenler

* Linux Tanı Uzantısı yüklendi
* Linux Diagnostic Extension yüklendi ve kaldırıldı, ancak yine de mdsd tarafından kullanılan omsagent hakkında bir hata görüyorsunuz ve kaldırılamaz.

### <a name="resolution"></a>Çözüm
1. Linux Tanı uzantısını (LAD) kaldırın.
2. Aşağıdaki konumda bulunuyorsa Linux Diagnostic Extension dosyalarını `/var/lib/waagent/Microsoft.Azure.Diagnostics.LinuxDiagnostic-<version>/` makineden kaldırın: ve `/var/opt/microsoft/omsagent/LAD/`.

## <a name="issue-you-cannot-see-data-any-nagios-data"></a>Sorun: Herhangi bir Nagios verisi göremezsiniz 

### <a name="probable-causes"></a>Olası nedenler
* Omsagent kullanıcı Nagios günlük dosyasından okumak için izinleri yok
* Nagios kaynak ve filtre omsagent.conf dosyasından yorumsuz olmamıştır

### <a name="resolution"></a>Çözüm
1. Bu [yönergeleri](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#nagios-alerts)izleyerek Nagios dosyasından okumak için omsagent kullanıcı ekleyin.
2. Linux genel yapılandırma dosyası için Log `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`Analytics aracısında, Hem Nagios kaynağının **hem de** filtrenin yorumsuz olduğundan emin olun.

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

## <a name="issue-you-are-not-seeing-any-linux-data"></a>Sorun: Herhangi bir Linux verisi görmüyorsunuz 

### <a name="probable-causes"></a>Olası nedenler
* Azure Monitör'e binme başarısız oldu
* Azure Monitörbağlantısı engellendi
* Sanal makine yeniden başlatıldı
* OMI paketi, Log Analytics aracısı tarafından Linux paketi için yüklenenpakete kıyasla daha yeni bir sürüme el ile yükseltildi
* DSC kaynak günlükleri *sınıfı* `omsconfig.log` günlük dosyasında hata bulunamadı
* Veriler için Log Analytics aracısı yedeklenir
* DSC günlükleri *Geçerli yapılandırma yok. Bir yapılandırma dosyası belirtmek ve önce geçerli bir yapılandırma oluşturmak için -Yol parametresi ile Başlat-DscConfiguration komutunu çalıştırın.* günlük `omsconfig.log` dosyasında, ancak işlemler `PerformRequiredConfigurationChecks` hakkında hiçbir günlük iletisi vardır.

### <a name="resolution"></a>Çözüm
1. Denetlenmiş paket gibi tüm bağımlılıkları yükleyin.
2. Azure Monitor'a bininin aşağıdaki dosyanın var `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`olup olmadığını kontrol ederek başarılı olup olmadığını denetleyin: .  Değilse, reonboard omsadmin.sh komut satırı [yönergeleri](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line)ni kullanarak .
4. Proxy kullanıyorsanız, proxy sorun giderme adımlarını yukarıda denetleyin.
5. Bazı Azure dağıtım sistemlerinde, sanal makine yeniden başlatıldıktan sonra omid OMI sunucusu daemon başlamaz. Bu, Denetim, ChangeTracking veya UpdateManagement çözümüyle ilgili verileri görmemeyle sonuçlanır. Geçici çözüm, omi sunucusunu çalıştırarak `sudo /opt/omi/bin/service_control restart`el ile başlatmaktır.
6. OMI paketi el ile yeni bir sürüme yükseltildikten sonra, Log Analytics aracının çalışmaya devam edemesi için el ile yeniden başlatılması gerekir. Bu adım, OMI sunucusunun yükseltildikten sonra otomatik olarak başlatılmayan bazı dağıtımlar için gereklidir. OMI'yi yeniden başlatmak için çalıştırın. `sudo /opt/omi/bin/service_control restart`
7. DSC kaynak sınıfı omsconfig.log hata *bulunamadı* görürseniz, çalıştırın. `sudo /opt/omi/bin/service_control restart`
8. Bazı durumlarda, Linux için Log Analytics aracısı Azure Monitor ile konuşamıyorsa, aracıdaki veriler tam arabellek boyutuna kadar yedeklenir: 50 MB. Aracı aşağıdaki komutu `/opt/microsoft/omsagent/bin/service_control restart`çalıştırarak yeniden başlatılmalıdır.

    >[!NOTE]
    >Bu sorun Agent sürüm 1.1.0-28 veya sonraki düzeltilir
    >

* `omsconfig.log` Günlük dosyası, işlemlerin `PerformRequiredConfigurationChecks` sistemde düzenli aralıklarla çalıştığını göstermiyorsa, cron işi/hizmetiyle ilgili bir sorun olabilir. Cron iş altında `/etc/cron.d/OMSConsistencyInvoker`var olduğundan emin olun. Gerekirse cron iş oluşturmak için aşağıdaki komutları çalıştırın:

    ```
    mkdir -p /etc/cron.d/
    echo "*/15 * * * * omsagent /opt/omi/bin/OMSConsistencyInvoker >/dev/null 2>&1" | sudo tee /etc/cron.d/OMSConsistencyInvoker
    ```

    Ayrıca, cron hizmetinin çalışmadığından emin olun. Bu hizmetin durumunu kontrol etmek için Debian, `service crond status` Ubuntu, SUSE veya RHEL, CentOS, Oracle Linux ile kullanabilirsiniz. `service cron status` Hizmet yoksa, ikilileri yükleyebilir ve aşağıdakileri kullanarak hizmeti başlatabilirsiniz:

    **Ubuntu/Debian**

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

## <a name="issue-when-configuring-collection-from-the-portal-for-syslog-or-linux-performance-counters-the-settings-are-not-applied"></a>Sorun: Syslog veya Linux performans sayaçları için portaldan toplama yapılandırma yaparken, ayarlar uygulanmaz

### <a name="probable-causes"></a>Olası nedenler
* Linux için Log Analytics aracısı en son yapılandırmayı almadı
* Portalda değiştirilen ayarlar uygulanmadı

### <a name="resolution"></a>Çözüm
**Amaç:** `omsconfig` Her beş dakikada bir yeni portal tarafı yapılandırması arayan Linux yapılandırma aracısı için Log Analytics aracısıdır. Bu yapılandırma daha sonra /etc/opt/microsoft/omsagent/conf/omsagent.conf adresinde bulunan Linux yapılandırma dosyaları için Log Analytics aracısına uygulanır.

* Bazı durumlarda, Linux yapılandırma aracısının Log Analytics aracısı portal yapılandırma hizmetiyle iletişim kuramayabilir ve bu da en son yapılandırmanın uygulanmamasıyla sonuçlanır.
  1. Aracının `omsconfig` çalıştırılarak `dpkg --list omsconfig` veya `rpm -qi omsconfig`' t'ye yüklenerek yüklendiğinden veya .  Yüklenmezse, Linux için Log Analytics aracısının en son sürümünü yeniden yükleyin.

  2. Aracının `omsconfig` aşağıdaki komutu `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'`çalıştırarak Azure Monitor ile iletişim kurabileceğinden denetlemeyin. Bu komut, Aracının Hizmetten aldığı yapılandırmayı(Syslog ayarları, Linux performans sayaçları ve özel günlükler dahil) döndürür. Bu komut başarısız olursa, `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py'`aşağıdaki komutu çalıştırın. Bu komut, omsconfig aracısını Azure Monitor ile konuşmaya ve en son yapılandırmayı almaya zorlar.

## <a name="issue-you-are-not-seeing-any-custom-log-data"></a>Sorun: Özel günlük verileri görmüyorsunuz 

### <a name="probable-causes"></a>Olası nedenler
* Azure Monitor'a binme başarısız oldu.
* Ayarı **Linux Sunucularıma aşağıdaki yapılandırmayı uygulayın** seçilmedi.
* omsconfig hizmetten en son özel günlük yapılandırmasını almadı.
* Linux kullanıcısı `omsagent` için Log Analytics aracısı, izinler nedeniyle veya bulunamadıktan dolayı özel günlük lere erişemiyor.  Aşağıdaki hataları görebilirsiniz:
 * `[DATETIME] [warn]: file not found. Continuing without tailing it.`
 * `[DATETIME] [error]: file not accessible by omsagent.`
* Linux sürümü 1.1.0-217 için Log Analytics temsilcisinde Düzeltilen Race Condition ile Bilinen Sorun

### <a name="resolution"></a>Çözüm
1. Azure Monitor'a bininin aşağıdaki dosyanın var `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsadmin.conf`olup olmadığını denetleyerek başarılı olduğunu doğrulayın: . Değilse, ya:  

  1. omsadmin.sh komut satırı [yönergelerini](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/OMS-Agent-for-Linux.md#onboarding-using-the-command-line)kullanarak reonboard.
  2. Azure portalındaki **Gelişmiş Ayarlar** altında, ayarın **Linux Sunucularıma aşağıdaki yapılandırmayı uyguladığından** emin olun.  

2. Aracının `omsconfig` aşağıdaki komutu `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/GetDscConfiguration.py'`çalıştırarak Azure Monitor ile iletişim kurabileceğinden denetlemeyin.  Bu komut, Aracının Hizmetten aldığı yapılandırmayı(Syslog ayarları, Linux performans sayaçları ve özel günlükler dahil) döndürür. Bu komut başarısız olursa, `sudo su omsagent -c 'python /opt/microsoft/omsconfig/Scripts/PerformRequiredConfigurationChecks.py'`aşağıdaki komutu çalıştırın. Bu komut, omsconfig aracısını Azure Monitor ile konuşmaya ve en son yapılandırmayı almaya zorlar.

**Arka plan:** Ayrıcalıklı bir kullanıcı olarak çalışan Linux için Log `root`Analytics aracısı `omsagent` yerine , aracı kullanıcı olarak çalışır. Çoğu durumda, belirli dosyaların okunabilmesi için bu kullanıcıya açık izin verilmesi gerekir. Kullanıcıya izin `omsagent` vermek için aşağıdaki komutları çalıştırın:

1. Kullanıcıyı `omsagent` belirli bir gruba ekleme`sudo usermod -a -G <GROUPNAME> <USERNAME>`
2. Gerekli dosyaya evrensel okuma erişimi verme`sudo chmod -R ugo+rx <FILE DIRECTORY>`

1.1.0-217'den önce Linux sürümü için Log Analytics aracısıyla bir yarış koşuluyla ilgili bilinen bir sorun vardır. En son aracıyı güncelledikten sonra, çıktı eklentisinin `sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf`en son sürümünü almak için aşağıdaki komutu çalıştırın.

## <a name="issue-you-are-trying-to-reonboard-to-a-new-workspace"></a>Sorun: Yeni bir çalışma alanına yeniden dahil etmeye çalışıyorsunuz
Bir aracıyı yeni bir çalışma alanına yeniden kaydetmeye çalıştığınızda, Log Analytics aracısı yapılandırmasının yeniden gemiye binmeden önce temizlenmesi gerekir. Eski yapılandırmayı aracıdan temizlemek için, kabuk paketini`--purge`

```
sudo sh ./omsagent-*.universal.x64.sh --purge
```
Veya

```
sudo sh ./onboard_agent.sh --purge
```

Seçeneği kullandıktan sonra reonboard devam `--purge` edebilirsiniz

## <a name="log-analytics-agent-extension-in-the-azure-portal-is-marked-with-a-failed-state-provisioning-failed"></a>Azure portalındaki Günlük Analytics aracıuzantısı başarısız bir durumla işaretlenir: Sağlama başarısız oldu

### <a name="probable-causes"></a>Olası nedenler
* Log Analytics aracısı işletim sisteminden kaldırıldı
* Log Analytics aracısı hizmeti kapatıldı, devre dışı bırakıldı veya yapılandırılmadı

### <a name="resolution"></a>Çözüm 
Sorunu düzeltmek için aşağıdaki adımları gerçekleştirin.
1. Uzantıyı Azure portalından kaldırın.
2. [Yönergeleri](../../azure-monitor/learn/quick-collect-linux-computer.md)izleyerek aracıyı yükleyin.
3. Aşağıdaki komutu çalıştırarak aracıyı `sudo /opt/microsoft/omsagent/bin/service_control restart`yeniden başlatın: .
* Birkaç dakika bekleyin ve Sağlanması için hükmü devlet değişiklikleri **başarılı oldu.**


## <a name="issue-the-log-analytics-agent-upgrade-on-demand"></a>Sorun: Log Analytics aracısı isteğe bağlı yükseltme

### <a name="probable-causes"></a>Olası nedenler

Ana bilgisayardaki Log Analytics aracı paketlerinin modası geçmiş.

### <a name="resolution"></a>Çözüm 
Sorunu düzeltmek için aşağıdaki adımları gerçekleştirin.

1. [Sayfadaki](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/)en son sürümü kontrol edin.
2. Komut dosyasını indirin (örnek sürüm olarak 1.4.2-124):

    ```
    wget https://github.com/Microsoft/OMS-Agent-for-Linux/releases/download/OMSAgent_GA_v1.4.2-124/omsagent-1.4.2-124.universal.x64.sh
    ```

3. Yürüterek paketleri `sudo sh ./omsagent-*.universal.x64.sh --upgrade`yükseltin.

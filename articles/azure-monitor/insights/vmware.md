---
title: Azure Monitör'de VMware İzleme çözümü | Microsoft Dokümanlar
description: VMware İzleme çözümünü günlüklerin yönetilmesine ve ESXi ana bilgisayarlarının izlenmesine nasıl yardımcı olabileceği hakkında bilgi edinin.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/04/2018
ms.openlocfilehash: c1622ef16155206d779c6d703fc7da568d233e7e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77664788"
---
# <a name="vmware-monitoring-deprecated-solution-in-azure-monitor"></a>Azure Monitör'de VMware İzleme (Amortismana Uğradı) çözümü

![VMware simgesi](./media/vmware/vmware-symbol.png)

> [!NOTE]
> VMware İzleme çözümü amortismana kaldırıldı.  Çözümü zaten yüklemiş olan müşteriler bu çözümü kullanmaya devam edebilir, ancak VMware İzleme yeni çalışma alanlarına eklenemez.

Azure Monitor'daki VMware İzleme çözümü, büyük VMware günlükleri için merkezi bir günlük ve izleme yaklaşımı oluşturmanıza yardımcı olan bir çözümdür. Bu makalede, çözümü kullanarak ESXi ana bilgisayarlarını tek bir konumda nasıl sorun giderebileceğiniz, yakaabileceğiniz ve yönetebileceğiniz açıklanmaktadır. Çözümle, tüm ESXi ana bilgisayarlarınız için ayrıntılı verileri tek bir konumda görebilirsiniz. ESXi ana bilgisayar günlükleri aracılığıyla sağlanan VM ve ESXi ana bilgisayarlarının en iyi olay sayılarını, durumlarını ve eğilimlerini görebilirsiniz. Merkezi ESXi ana bilgisayar günlüklerini görüntüleyerek ve arayarak sorun giderebilirsiniz. Ayrıca, günlük arama sorgularını temel alan uyarılar oluşturabilirsiniz.

Çözüm, verileri Log Analytics aracısına sahip hedef VM'ye itmek için ESXi ana bilgisayarının yerel syslog işlevini kullanır. Ancak, çözüm hedef VM içinde syslog içine dosya yazmaz. Log Analytics aracısı 1514 bağlantı noktasını açar ve bunu dinler. Verileri aldıktan sonra, Log Analytics aracısı verileri Azure Monitor'a iter.

## <a name="install-and-configure-the-solution"></a>Çözümü yükleme ve yapılandırma
Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın.

* Bir izleme çözümü yükle'de açıklanan işlemi kullanarak Aboneliğinize VMware İzleme [çözümünü](../insights/solutions.md#install-a-monitoring-solution)ekleyin.

#### <a name="supported-vmware-esxi-hosts"></a>Desteklenen VMware ESXi ana bilgisayarları
vSphere ESXi Host 5.5, 6.0 ve 6.5

#### <a name="prepare-a-linux-server"></a>Linux sunucusu hazırlama
ESXi ana bilgisayarlarından tüm syslog verilerini almak için bir Linux işletim sistemi VM oluşturun. [Log Analytics Linux aracısı](../learn/quick-collect-linux-computer.md) tüm ESXi ana bilgisayar syslog verilerinin toplama noktasıdır. Aşağıdaki örnekte olduğu gibi günlükleri tek bir Linux sunucusuna iletmek için birden çok ESXi ana bilgisayarını kullanabilirsiniz.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]  

   ![syslog akışı](./media/vmware/diagram.png)

### <a name="configure-syslog-collection"></a>Syslog koleksiyonunu yapılandırma
1. VSphere için syslog yönlendirmeyi ayarlayın. Syslog iletme kurulumuna yardımcı olacak ayrıntılı bilgi için [ESXi 5.0 ve üzeri (2003322) üzerinde yapılandırma syslog'una](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003322)bakın. **ESXi Host Yapılandırma** > **Yazılımı** > **Gelişmiş Ayarlar** > **Syslog**gidin.
   ![vsphereconfig](./media/vmware/vsphere1.png)  
1. *Syslog.global.logHost* alanında, Linux sunucunuzu ve *1514*numaralı bağlantı noktasını ekleyin. Örneğin `tcp://hostname:1514` veya `tcp://123.456.789.101:1514` olabilir.
1. ESXi ana bilgisayar güvenlik duvarını syslog için açın. **ESXi Host Yapılandırma** > **Yazılımı** > **Güvenlik Profili** > **Güvenlik Duvarı** ve açık **Özellikleri**.  

    ![vspherefw](./media/vmware/vsphere2.png)  

    ![vspherefwproperties](./media/vmware/vsphere3.png)  
1. Syslog'un düzgün şekilde ayarlı olduğunu doğrulamak için vSphere Console'u kontrol edin. ESXI ana bilgisayarda **1514** bağlantı noktasının yapılandırıldığına onaylayın.
1. Linux için Log Analytics aracısını Linux sunucusuna indirin ve kurun. Daha fazla bilgi [için, Linux için Log Analytics aracısı için Belgeler bölümüne](https://github.com/Microsoft/OMS-Agent-for-Linux)bakın.
1. Linux için Log Analytics aracısı yüklendikten sonra /etc/opt/microsoft/omsagent/sysconf/omsagent.d dizinine gidin ve vmware_esxi.conf dosyasını /etc/opt/microsoft/omsagent/conf/omsagent.d dizinine kopyalayın ve dosyasahibi/grubu ve izinlerini değiştirin. Örnek:

    ```
    sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/vmware_esxi.conf /etc/opt/microsoft/omsagent/conf/omsagent.d
   sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf
    ```
1. Linux için Log Analytics aracısını `sudo /opt/microsoft/omsagent/bin/service_control restart`çalıştırarak yeniden başlatın.
1. ESXi Ana Bilgisayar'daki komutu `nc` kullanarak Linux sunucusu ile ESXi ana bilgisayarı arasındaki bağlantıyı test edin. Örnek:

    ```
    [root@ESXiHost:~] nc -z 123.456.789.101 1514
    Connection to 123.456.789.101 1514 port [tcp/*] succeeded!
    ```

1. Azure portalında, '' için `VMware_CL`bir günlük sorgusu gerçekleştirin. Azure Monitor syslog verilerini topladığı zaman, syslog biçimini korur. Portalda, *Hostname* ve *ProcessName*gibi bazı özel alanlar yakalanır.  

    ![type](./media/vmware/type.png)  

    Görünüm günlüğü arama sonuçlarınız yukarıdaki resme benziyorsa, VMware İzleme çözüm panosunu kullanmaya hazırsınız.  

## <a name="vmware-data-collection-details"></a>VMware veri toplama ayrıntıları
VMware İzleme çözümü, etkinleştirdiğiniz Linux için Log Analytics aracılarını kullanarak ESXi ana bilgisayarlarından çeşitli performans ölçümleri ve günlük verileri toplar.

Aşağıdaki tabloda veri toplama yöntemleri ve verilerin nasıl toplandığıyla ilgili diğer ayrıntılar gösterilmektedir.

| platform | Linux için Log Analytics aracısı | SCOM temsilcisi | Azure Storage | SCOM gerekli mi? | Yönetim grubu aracılığıyla gönderilen SCOM aracı verileri | toplama sıklığı |
| --- | --- | --- | --- | --- | --- | --- |
| Linux |&#8226; |  |  |  |  |her 3 dakikada bir |

Aşağıdaki tablovware izleme çözümü tarafından toplanan veri alanlarının örneklerini gösterir:

| alan adı | açıklama |
| --- | --- |
| Device_s |VMware depolama cihazları |
| ESXIFailure_s |hata türleri |
| EventTime_t |olayın meydana geldiği zaman |
| HostName_s |ESXi ana bilgisayar adı |
| Operation_s |VM oluşturma veya VM'yi silme |
| ProcessName_s |olay adı |
| ResourceId_s |VMware ana bilgisayar ının adı |
| ResourceLocation_s |VMware |
| ResourceName_s |VMware |
| ResourceType_s |Hyper-V |
| SCSIStatus_s |VMware SCSI durumu |
| SyslogMessage_s |Syslog verileri |
| UserName_s |VM'yi oluşturan veya silen kullanıcı |
| VMName_s |VM adı |
| Bilgisayar |ana bilgisayar |
| TimeGenerated |verilerin oluşturulduğu saat |
| DataCenter_s |VMware veri merkezi |
| StorageLatency_s |depolama gecikmesi (ms) |

## <a name="vmware-monitoring-solution-overview"></a>VMware İzleme çözümüne genel bakış
VMware döşemesi Log Analytics çalışma alanınızda görünür. Herhangi bir hatanın üst düzey bir görünümünü sağlar. Döşemeyi tıklattığınızda, pano görünümüne gidersiniz.

![tile (döşeme)](./media/vmware/tile.png)

#### <a name="navigate-the-dashboard-view"></a>Pano görünümünde gezinme
**VMware** pano görünümünde bıçaklar aşağıdakilere göre düzenlenir:

* Hata Durum Sayısı
* Etkinlik Sayılarını En İyi Ev Sahibi
* En İyi Etkinlik Sayıları
* Sanal Makine Aktiviteleri
* ESXi Ana Bilgisayar Disk Etkinlikleri

![çözüm1](./media/vmware/solutionview1-1.png)

![çözüm2](./media/vmware/solutionview1-2.png)

Bladee özel ayrıntılı bilgileri gösteren Log Analytics arama bölmesini açmak için herhangi bir bıçağı tıklatın.

Buradan, günlük sorgusunu belirli bir şey için değiştirmek için değiştirebilirsiniz. Günlük sorguları oluşturma yla ilgili ayrıntılar için Azure [Monitor'da günlük sorgularını kullanarak veri bulma bilgisine](../log-query/log-query-overview.md)bakın.

#### <a name="find-esxi-host-events"></a>ESXi ev sahibi etkinliklerini bulma
Tek bir ESXi ana bilgisayarı, işlemlerine göre birden çok günlük oluşturur. VMware İzleme çözümü bunları merkezileştirir ve olay sayılarını özetler. Bu merkezi görünüm, hangi ESXi ana bilgisayar biriminin yüksek sayıda olaya sahip olduğunu ve ortamınızda en sık hangi olayların meydana geldiğini anlamanıza yardımcı olur.

![event](./media/vmware/events.png)

Bir ESXi ana bilgisayar ahost'una veya etkinlik türünü tıklatarak daha fazla sondaj yapabilirsiniz.

Bir ESXi ana bilgisayar adını tıklattığınızda, bu ESXi ana bilgisayardan gelen bilgileri görüntüleyebilirsiniz. Olay türüyle sonuçları daraltmak istiyorsanız, arama sorgunuza ekleyin. `“ProcessName_s=EVENT TYPE”` Arama filtresinde **ProcessName'yi** seçebilirsiniz. Bu senin için bilgiyi daraltır.

![Matkap](./media/vmware/eventhostdrilldown.png)

#### <a name="find-high-vm-activities"></a>Yüksek VM etkinliklerini bulun
Sanal bir makine herhangi bir ESXi ana bilgisayarda oluşturulabilir ve silinebilir. Bir yöneticinin bir ESXi ana bilgisayar alanının kaç VM oluşturduğunu belirlemesi yararlıdır. Bu sırayla, performans ve kapasite planlama anlamak için yardımcı olur. Ortamınızı yönetirken VM etkinlik etkinliklerini izlemek çok önemlidir.

![Matkap](./media/vmware/vmactivities1.png)

Ek ESXi ana bilgisayar VM oluşturma verilerini görmek istiyorsanız, bir ESXi ana bilgisayar adını tıklatın.

![Matkap](./media/vmware/createvm.png)

#### <a name="common-log-queries"></a>Sık kullanılan günlük sorguları
Çözüm, yüksek depolama alanı, depolama gecikmesi ve yol hatası gibi ESXi ana bilgisayarlarınızı yönetmenize yardımcı olabilecek diğer yararlı sorguları içerir.

![Sorgu](./media/vmware/queries.png)


#### <a name="save-queries"></a>Sorguları kaydet
Günlük sorgularını kaydetmek Azure Monitor'da standart bir özelliktir ve bulduğunuz sorguları yararlı tutmanıza yardımcı olabilir. Yararlı bulduğunuz bir sorgu oluşturduktan sonra, **Sık Kullanılanlar'ı**tıklatarak sorguyu kaydedin. Kaydedilen bir sorgu, kendi özel panolarınızı oluşturabileceğiniz [Panom](../learn/tutorial-logs-dashboards.md) sayfasından daha sonra kolayca yeniden kullanmanıza olanak tanır.

![DockerDashboardView](./media/vmware/dockerdashboardview.png)

#### <a name="create-alerts-from-queries"></a>Sorgulardan uyarı oluşturma
Sorgularınızı oluşturduktan sonra, belirli olaylar meydana geldiğinde sizi uyarmak için sorguları kullanmak isteyebilirsiniz. Uyarıların nasıl oluşturulabildiğini öğrenmek için [Günlük Analizi'ndeki Uyarılar'a](../platform/alerts-overview.md) bakın. Uyarı sorguları ve diğer sorgu örnekleri için Log Analytics blog gönderisini [kullanarak Monitor VMware'e](https://blogs.technet.microsoft.com/msoms/2016/06/15/monitor-vmware-using-oms-log-analytics) bakın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
### <a name="what-do-i-need-to-do-on-the-esxi-host-setting-what-impact-will-it-have-on-my-current-environment"></a>ESXi ana bilgisayar ayarında ne yapmam gerekiyor? Şu anki çevrem üzerinde nasıl bir etkisi olacak?
Çözüm, yerel ESXi Host Syslog yönlendirme mekanizmasını kullanır. Günlükleri yakalamak için ESXi Ana Bilgisayar'da ek Microsoft yazılımına ihtiyacınız yoktur. Mevcut ortamınız üzerinde düşük bir etkisi olmalıdır. Ancak, ESXI işlevselliği olan syslog iletme ayarlamanız gerekir.

### <a name="do-i-need-to-restart-my-esxi-host"></a>ESXi ana bilgisayarımı yeniden başlatmam gerekiyor mu?
Hayır. Bu işlem yeniden başlatma gerektirmez. Bazen, vSphere düzgün syslog güncellemez. Böyle bir durumda, ESXi ana bilgisayara oturum açın ve sislog'u yeniden yükleyin. Yine, ana bilgisayarı yeniden başlatmanız gerekmediği için bu işlem ortamınızı bozmaz.

### <a name="can-i-increase-or-decrease-the-volume-of-log-data-sent-to-log-analytics"></a>Log Analytics'e gönderilen günlük verilerinin hacmini artırabilir veya azaltabilir miyim?
Evet yapabilirsin. vSphere'deki ESXi Ana Bilgisayar Günlük Düzeyi ayarlarını kullanabilirsiniz. Günlük toplama *bilgi* düzeyine bağlıdır. Bu nedenle, VM oluşturma veya silme denetlemek istiyorsanız, *Hostd'da bilgi* düzeyini korumanız gerekir. Daha fazla bilgi için [VMware Bilgi Bankası'na](https://kb.vmware.com/selfservice/microsites/search.do?&cmd=displayKC&externalId=1017658)bakın.

### <a name="why-is-hostd-not-providing-data-to-log-analytics-my-log-setting-is-set-to-info"></a>Hostd neden Log Analytics'e veri sağmıyor? Günlük ayarım bilgi olarak ayarlandı.
Syslog zaman damgası için bir ESXi ana bilgisayar hatası vardı. Daha fazla bilgi için [VMware Bilgi Bankası'na](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2111202)bakın. Geçici çözüm uyguladıktan sonra, Ana Bilgisayar normal çalışması gerekir.

### <a name="can-i-have-multiple-esxi-hosts-forwarding-syslog-data-to-a-single-vm-with-omsagent"></a>Birden çok ESXi ana bilgisayarı syslog verilerini omsagent ile tek bir VM'ye iletebilir miyim?
Evet. Omsagent ile tek bir VM'ye iletilen birden çok ESXi ana bilgisayara sahip olabilirsiniz.

### <a name="why-dont-i-see-data-flowing-into-log-analytics"></a>Log Analytics'e akan verileri neden göremiyorum?
Birden çok nedeni olabilir:

* ESXi ana bilgisayarı, verileri doğru bir şekilde vm çalışan omsagent'a itmiyor. Sınamak için aşağıdaki adımları gerçekleştirin:

  1. Onaylamak için, ssh kullanarak ESXi ana bilgisayarda oturum açın ve aşağıdaki komutu çalıştırın:`nc -z ipaddressofVM 1514`

      Bu başarılı olmazsa, Gelişmiş Yapılandırma'daki vSphere ayarları büyük olasılıkla doğru değildir. EsXi ana bilgisayarını syslog iletme için nasıl ayarlayabildiğini öğrenmek için [syslog koleksiyonunu yapılandırın.](#configure-syslog-collection)
  1. Syslog bağlantı noktası bağlantısı başarılı ysa, ancak hala veri görmüyorsanız, aşağıdaki komutu çalıştırmak için ssh kullanarak ESXi ana bilgisayarda syslog'u yeniden yükleyin:`esxcli system syslog reload`
* Log Analytics aracılı VM doğru ayarlanmaz. Bunu test etmek için aşağıdaki adımları gerçekleştirin:

  1. Log Analytics bağlantı noktası 1514'u dinler. Açık olduğunu doğrulamak için aşağıdaki komutu çalıştırın:`netstat -a | grep 1514`
  1. Bağlantı noktasının `1514/tcp` açık olduğunu görmelisiniz. Bunu yapmazsanız, omsagent'un doğru yüklendiğinden doğrulayın. Bağlantı noktası bilgilerini görmüyorsanız, Syslog bağlantı noktası VM'de açık değildir.

    a. Log Analytics aracısının kullanarak `ps -ef | grep oms`çalıştığını doğrulayın. Çalışmıyorsa, komutu çalıştırarak işlemi başlatın`sudo /opt/microsoft/omsagent/bin/service_control start`

     b. `/etc/opt/microsoft/omsagent/conf/omsagent.d/vmware_esxi.conf` dosyasını açın.

     c. Aşağıdakilere benzer şekilde uygun kullanıcı ve grup ayarının geçerli olduğunu doğrulayın:`-rw-r--r-- 1 omsagent omiusers 677 Sep 20 16:46 vmware_esxi.conf`

     d. Dosya yoksa veya kullanıcı ve grup ayarı yanlışsa, [bir Linux sunucusu hazırlayarak](#prepare-a-linux-server)düzeltici eylemde bulunun.

## <a name="next-steps"></a>Sonraki adımlar
* Ayrıntılı VMware ana bilgisayar verilerini görüntülemek için Log Analytics'teki [günlük sorgularını](../log-query/log-query-overview.md) kullanın.
* VMware ana bilgisayar verilerini gösteren [kendi panolarınızı oluşturun.](../learn/tutorial-logs-dashboards.md)
* Belirli VMware ana bilgisayar olayları oluştuğunda [uyarılar oluşturun.](../platform/alerts-overview.md)

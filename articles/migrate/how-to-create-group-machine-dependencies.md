---
title: Azure Geçir Sunucusu Değerlendirmesi'nde aracı tabanlı bağımlılık çözümlemesi ayarlama
description: Bu makalede, Azure Geçir Sunucusu Değerlendirmesi'nde aracı tabanlı bağımlılık çözümlemesi nasıl ayarlanır.
ms.topic: how-to
ms.date: 2/24/2020
ms.openlocfilehash: e61b7b4e6c3e566aa67d2bd585d2049ae885083b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79453624"
---
# <a name="set-up-dependency-visualization"></a>Bağımlılık görselleştirmeyi ayarlama

Bu makalede, Azure Geçiş:Sunucu Değerlendirmesi'nde aracı tabanlı bağımlılık çözümlemesi nasıl ayarlanır. [Bağımlılık çözümlemesi,](concepts-dependency-visualization.md) değerlendirmek ve Azure'a geçirmek istediğiniz makinelerdeki bağımlılıkları belirlemenize ve anlamanıza yardımcı olur.

## <a name="before-you-start"></a>Başlamadan önce

- Aracı tabanlı bağımlılık çözümlemesi [hakkında bilgi edinin.](concepts-dependency-visualization.md#agent-based-analysis)
- [VMware VM'ler,](migrate-support-matrix-vmware.md#agent-based-dependency-analysis-requirements) [fiziksel sunucular](migrate-support-matrix-physical.md#agent-based-dependency-analysis-requirements)ve [Hyper-VM'ler](migrate-support-matrix-hyper-v.md#agent-based-dependency-analysis-requirements)için aracı tabanlı bağımlılık görselleştirmesi ayarlamak için ön koşulları ve destek gereksinimlerini gözden geçirin.
- Bir Azure Geçiş projesi [oluşturduğunuzdan](how-to-add-tool-first-time.md) emin olun.
- Bir proje zaten oluşturduysanız, Azure Geçir:Sunucu Değerlendirmesi aracını [eklediğinizden](how-to-assess.md) emin olun.
- Şirket içi makinelerinizi keşfetmek için bir [Azure Geçiş cihazı](migrate-appliance.md) ayarladığınızdan emin olun. [VMware,](how-to-set-up-appliance-vmware.md) [Hyper-V](how-to-set-up-appliance-hyper-v.md)veya [fiziksel sunucular](how-to-set-up-appliance-physical.md)için nasıl bir cihaz kuracağınızı öğrenin. Cihaz şirket içi makineleri keşfeder ve Azure Geçiş:Sunucu Değerlendirmesi'ne meta veriler, performans verileri gönderir.
- Bağımlılık görselleştirmesini kullanmak için, bir [Günlük Analizi çalışma alanını](../azure-monitor/platform/manage-access.md) bir Azure Geçiş projesiyle ilişkilendirin:
    - Bir çalışma alanı ekleyebilirsiniz, ancak Azure Geçir cihazını kurduktan ve Azure Geçir projesinde makineleri keşfettikten sonra.
    - Abonelikte Azure Geçiş projesini içeren bir çalışma alanınız olduğundan emin olun.
    - Çalışma alanı Doğu ABD, Güneydoğu Asya veya Batı Avrupa bölgelerinde ikamet etmelidir. Diğer bölgelerdeki çalışma alanları projeyle ilişkilendirilemez.
    - Çalışma [alanı, Hizmet Haritası'nın desteklendiği](../azure-monitor/insights/vminsights-enable-overview.md#prerequisites)bir bölgede olmalıdır.
    - Yeni veya varolan bir Log Analytics çalışma alanını bir Azure Geçiş projesiyle ilişkilendirebilirsiniz.
    - Bir makine için bağımlılık görselleştirmesini ilk kez ayarladığınız da çalışma alanını bağlarsınız. Azure Geçir projesinin çalışma alanı eklendikten sonra değiştirilemez.
    - Günlük Analizi'nde, Azure Geçişi ile ilişkili çalışma alanı Geçiş Projesi anahtarı ve proje adı ile etiketlenir.

## <a name="associate-a-workspace"></a>Çalışma alanını ilişkilendirme

1. Değerlendirme için makineleri keşfettikten sonra, **Sunucularda** > **Azure Geçiş: Sunucu Değerlendirmesi**, Genel **Bakış'ı**tıklatın.  
2. **Azure Geçir: Sunucu Değerlendirmesi'nde**Temel **Bilgiler'i**tıklatın.
3. **OMS Çalışma Alanı'nda**yapılandırma **gerektirir'ı**tıklatın.

     ![Günlük Analizi çalışma alanını yapılandırma](./media/how-to-create-group-machine-dependencies/oms-workspace-select.png)   

4. **OMS çalışma alanını yapılandırıldığında,** yeni bir çalışma alanı oluşturmak mı yoksa varolan bir çalışma alanı mı kullanmak istediğinizi belirtin.
    - Geçiş projesi aboneliğindeki tüm çalışma alanlarından varolan bir çalışma alanı seçebilirsiniz.
    - Ilişkilendirmek için Reader'ın çalışma alanına erişmeniz gerekir.
5. Yeni bir çalışma alanı oluşturursanız, bunun için bir konum seçin.

    ![Yeni bir çalışma alanı ekleme](./media/how-to-create-group-machine-dependencies/workspace.png)


## <a name="download-and-install-the-vm-agents"></a>Sanal makine aracılarını indirip yükleme

Analiz etmek istediğiniz her makineye aracıları yükleyin.

> [!NOTE]
> System Center Operations Manager 2012 R2 veya sonraki tarafından izlenen makineler için MMA aracısını yüklemeniz gerekmez. Hizmet Haritası, Operasyon Yöneticisi ile tümleşir. Tümleştirme kılavuzunu [izleyin.](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-scom#prerequisites)

1. **Azure Geçir: Sunucu**Değerlendirmesi'nde, **Keşfedilen sunucular'ı**tıklatın.
2. Bağımlılık görselleştirmeile çözümlemek istediğiniz her makine **için, Bağımlılıklar** sütununda **aracı yüklemegerektirir'i**tıklatın.
3. **Bağımlılıklar** sayfasında, Windows veya Linux için MMA ve Bağımlılık aracısını indirin.
4. **Yapılandırılan MMA aracısı**altında, çalışma alanı kimliğini ve anahtarı kopyalayın. MMA aracısını yüklerken bunlara ihtiyacınız var.

    ![Aracıları yükleyin](./media/how-to-create-group-machine-dependencies/dependencies-install.png)


## <a name="install-the-mma"></a>MMA’yı yükleme

Analiz etmek istediğiniz her Windows veya Linux makinesine MMA'yı yükleyin.

### <a name="install-mma-on-a-windows-machine"></a>Windows makinesine MMA yükleme

Aracıyı Bir Windows makinesine yüklemek için:

1. İndirilen aracıya çift tıklayın.
2. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın. **Lisans Koşulları** sayfasında **Kabul Ediyorum**’a tıklayarak lisansı kabul edin.
3. **Hedef Klasör'de,** Varsayılan yükleme klasörünü > **Sonraki'de**tutun veya değiştirin.
4. **Aracı Kurulum Seçenekleri'nde,****Sonraki**Azure **Günlük Analizi'ni** > seçin.
5. Yeni bir Log Analytics çalışma alanı eklemek için **Ekle'yi** tıklatın. Portaldan kopyaladığınız çalışma alanı kimliğine ve anahtarına yapıştırın. **İleri**'ye tıklayın.

Aracıyı komut satırından veya Configuration Manager veya [Intigua](https://go.microsoft.com/fwlink/?linkid=2104196)gibi otomatik bir yöntem kullanarak yükleyebilirsiniz.
- MMA aracısını yüklemek için bu yöntemleri kullanma hakkında [daha fazla bilgi edinin.](../azure-monitor/platform/log-analytics-agent.md#installation-and-configuration)
- MMA aracısı bu [betik](https://go.microsoft.com/fwlink/?linkid=2104394) kullanılarak da yüklenebilir.
- MMA tarafından desteklenen Windows işletim sistemleri hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems)

### <a name="install-mma-on-a-linux-machine"></a>Linux makinesine MMA yükleme

MMA'yı bir Linux makinesine yüklemek için:

1. Scp/sftp kullanarak uygun paketi (x86 veya x64) Linux bilgisayarınıza aktarın.
2. --install bağımsız değişkenini kullanarak paketi yükleyin.

    ```sudo sh ./omsagent-<version>.universal.x64.sh --install -w <workspace id> -s <workspace key>```

MMA'nın Linux işletim sistemleri desteği listesi hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-linux-operating-systems) 

## <a name="install-the-dependency-agent"></a>Bağımlılık aracısını yükleme

1. Bağımlılık aracısını bir Windows makinesine yüklemek için kurulum dosyasını çift tıklatın ve sihirbazı izleyin.
2. Bağımlılık aracısını bir Linux makinesine yüklemek için aşağıdaki komutu kullanarak kök olarak yükleyin:

    ```sh InstallDependencyAgent-Linux64.bin```

- Bağımlılık aracısını yüklemek için komut dosyalarını nasıl kullanabileceğiniz hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-hybrid-cloud#installation-script-examples)
- Bağımlılık aracısı tarafından desteklenen işletim sistemleri hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-enable-overview#supported-operating-systems)


## <a name="create-a-group-using-dependency-visualization"></a>Bağımlılık görselleştirmekullanarak bir grup oluşturma

Şimdi değerlendirme için bir grup oluşturun. 


> [!NOTE]
> Bağımlılıkları görselleştirmek istediğiniz gruplar 10'dan fazla makine içermemelidir. 10'dan fazla makineniz varsa, bunları daha küçük gruplara bölün.

1. **Azure Geçir: Sunucu**Değerlendirmesi'nde, **Keşfedilen sunucular'ı**tıklatın.
2. **Bağımlılıklar** sütununda, gözden geçirmek istediğiniz her makine için **bağımlılıkları** görüntüle'yi tıklatın.
3. Bağımlılık haritasında aşağıdakileri görebilirsiniz:
    - Gelen (istemciler) ve giden (sunucular) TCP bağlantıları, ve makineden.
    - Bağımlılık aracıları yüklü olmayan bağımlı makineler bağlantı noktası numaralarına göre gruplandırılır.
    - Bağımlılık aracıları yüklü bağımlı makineler ayrı kutular olarak gösterilir.
    - Makinenin içinde çalışan süreçler. İşlemleri görüntülemek için her makine kutusunu genişletin.
    - Makine özellikleri (FQDN, işletim sistemi, MAC adresi dahil). Ayrıntıları görüntülemek için her makine kutusuna tıklayın.

4. Zaman aralığı etiketindeki zaman süresine tıklayarak farklı zaman süreleri için bağımlılıklara bakabilirsiniz.
    - Varsayılan olarak aralık bir saattir. 
    - Zaman aralığını değiştirebilir veya başlangıç ve bitiş tarihlerini ve süresini belirtebilirsiniz.
    - Zaman aralığı bir saate kadar olabilir. Daha uzun bir kapsama alanına ihtiyacınız varsa, bağımlı verileri daha uzun bir süre sorgulamak için Azure Monitor'u kullanın.

5. Gruplandırmak istediğiniz bağımlı makineleri belirledikten sonra, haritada birden fazla makine seçmek için Ctrl+Click'i kullanın ve **Makineleri Grupla'yı**tıklatın.
6. Bir grup adı belirtin.
7. Bağımlı makinelerin Azure Geçiş tarafından keşfedildiğini doğrulayın.

    - Bağımlı bir makine Azure Geçir: Sunucu Değerlendirmesi tarafından keşfedilmemişse, gruba ekemezsiniz.
    - Bir makine eklemek için, keşfi yeniden çalıştırın ve makinenin keşfedildiğini doğrulayın.

8. Bu grup için bir değerlendirme oluşturmak istiyorsanız, grup için yeni bir değerlendirme oluşturmak için onay kutusunu seçin.
8. Grubu kaydetmek için **Tamam'ı** tıklatın.

Grubu oluşturduktan sonra, gruptaki tüm makinelere aracıyüklemenızı ve ardından tüm grup için bağımlılıkları görselleştirmenizi öneririz.

## <a name="query-dependency-data-in-azure-monitor"></a>Azure Monitör'de bağımlılık verilerini sorgula

Hizmet Haritası tarafından yakalanan bağımlılık verilerini Azure Geçiş projesiyle ilişkili Günlük Analizi çalışma alanında sorgulayabilirsiniz. Log Analytics, Azure Monitor günlük sorgularını yazmak ve çalıştırmak için kullanılır.

- Log Analytics'te Hizmet Haritası verilerini [nasıl arayacağınızı öğrenin.](../azure-monitor/insights/service-map.md#log-analytics-records)
- [Log Analytics'te](../azure-monitor/log-query/get-started-portal.md)günlük sorguları yazmaya [genel bir bakış alın.](../azure-monitor/log-query/get-started-queries.md)

Bağımlılık verileri için bir sorgu aşağıdaki gibi çalıştırın:

1. Aracıları yükledikten sonra portala gidin ve **Genel Bakış'ı**tıklatın.
2. **Azure Geçir: Sunucu Değerlendirmesi'nde**Genel **Bakış'ı**tıklatın. **Essentials'ı**genişletmek için aşağı ok'u tıklatın.
3. **OMS Çalışma Alanı'nda**çalışma alanı adını tıklatın.
3. **Genel**> Log Analytics çalışma alanı sayfasında **Günlükler'i**tıklatın.
4. Sorgunuzu yazın ve **Çalıştır'ı**tıklatın.

### <a name="sample-queries"></a>Örnek sorgular

Bağımlılık verilerini ayıklamak için kullanabileceğiniz birkaç örnek sorgu aşağıda veda edebilirsiniz.

- Tercih ettiğiniz veri noktalarını ayıklamak için sorguları değiştirebilirsiniz.
- Bağımlılık veri kayıtlarının tam listesini [gözden geçirin.](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#log-analytics-records)
- Ek örnek sorguları [gözden geçirin.](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#sample-log-searches)

#### <a name="sample-review-inbound-connections"></a>Örnek: Gelen bağlantıları gözden geçirme

Bir dizi VM için gelen bağlantıları gözden geçirin.

- Bağlantı ölçümleri (VMConnection) için tablodaki kayıtlar tek tek fiziksel ağ bağlantılarını temsil etmez.
- Birden çok fiziksel ağ bağlantısı mantıksal bir bağlantı halinde gruplandırılır.
- VMConnection'da fiziksel ağ bağlantısı verilerinin nasıl toplanmış olduğu hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/azure-monitor/insights/service-map#connections)

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z);
VMConnection
| where Direction == 'inbound'
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(LinksEstablished) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

#### <a name="sample-summarize-sent-and-received-data"></a>Örnek: Gönderilen ve alınan verileri özetle

Bu örnek, bir makine kümesi arasındaki gelen bağlantılarda gönderilen ve alınan verilerin hacmini özetler.

```
// the machines of interest
let ips=materialize(ServiceMapComputer_CL
| summarize ips=makeset(todynamic(Ipv4Addresses_s)) by MonitoredMachine=ResourceName_s
| mvexpand ips to typeof(string));
let StartDateTime = datetime(2019-03-25T00:00:00Z);
let EndDateTime = datetime(2019-03-30T01:00:00Z);
VMConnection
| where Direction == 'inbound'
| where TimeGenerated > StartDateTime and TimeGenerated  < EndDateTime
| join kind=inner (ips) on $left.DestinationIp == $right.ips
| summarize sum(BytesSent), sum(BytesReceived) by Computer, Direction, SourceIp, DestinationIp, DestinationPort
```

## <a name="next-steps"></a>Sonraki adımlar

Bir grup için [bir değerlendirme oluşturun.](how-to-create-assessment.md)



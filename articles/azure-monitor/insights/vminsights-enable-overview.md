---
title: VM'ler için Azure İzleyici etkinleştir (Önizleme) genel bakış | Microsoft Docs
description: VM'ler için Azure İzleyici dağıtmayı ve yapılandırmayı öğrenin. Sistem gereksinimlerini öğrenin.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/30/2019
ms.author: magoedte
ms.openlocfilehash: d2aa25af4ebf721d04ada46ffab37107bf73113b
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70861124"
---
# <a name="enable-azure-monitor-for-vms-preview-overview"></a>VM'ler için Azure İzleyici etkinleştir (Önizleme) genel bakış

Bu makalede VM'ler için Azure İzleyici ayarlamak için kullanılabilen seçeneklere bir genel bakış sunulmaktadır. Sistem durumunu ve performansı izlemek için VM'ler için Azure İzleyici kullanın. Azure sanal makineleri (VM) ve sanal makine ölçek kümeleri, şirket içi VM 'Ler veya başka bir bulut ortamında barındırılan VM 'Ler üzerinde çalışan uygulama bağımlılıklarını bulun.  

VM'ler için Azure İzleyici ayarlamak için:

* Doğrudan VM veya sanal makine ölçek kümesinden **Öngörüler (Önizleme)** seçeneğini belirleyerek tek BIR Azure VM veya sanal makine ölçek kümesini etkinleştirin.
* Azure Ilkesi 'ni kullanarak iki veya daha fazla Azure VM ve sanal makine ölçek kümesini etkinleştirin. Bu yöntem, mevcut ve yeni VM 'lerde ve ölçek kümelerinde gerekli bağımlılıkların yüklü ve düzgün şekilde yapılandırıldığından emin olmanızı sağlar. Uyumsuz VM 'Ler ve ölçek kümeleri raporlanır, bu sayede onları etkinleştirip etkinleştirmemeye karar verebilirsiniz.
* İki etkinleştirmek veya PowerShell kullanarak belirtilen abonelik veya kaynak grubu üzerinde daha fazla Azure sanal makineleri veya sanal makine ölçek kümeleri.
* Şirket ağınızda veya diğer bulut ortamınızda barındırılan VM 'Leri veya fiziksel bilgisayarları izlemek için VM'ler için Azure İzleyici etkinleştirin.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki bölümlerde yer alan bilgiler anladığınızdan emin olun. 

>[!NOTE]
>Bu bölümde açıklanan aşağıdaki bilgiler [hizmet eşlemesi çözümü](service-map.md)için de geçerlidir.  

### <a name="log-analytics"></a>Log Analytics

VM'ler için Azure İzleyici, aşağıdaki bölgelerde bir Log Analytics çalışma alanını destekler:

- Batı Orta ABD
- Batı ABD 2<sup>1</sup>
- East US
- Orta Kanada
- Birleşik Krallık Güney
- Batı Avrupa
- Güneydoğu Asya
- Avustralya Doğu<sup>1</sup>
- Avustralya Güneydoğu<sup>1</sup>

<sup>1</sup> VM'ler için bu bölgede şu anda Azure İzleyici'nın sistem durumu özelliğini desteklemiyor.

>[!NOTE]
>Azure VM 'Leri herhangi bir bölgeden dağıtabilirsiniz. Bu VM 'Ler Log Analytics çalışma alanı tarafından desteklenen bölgelerle sınırlı değildir.
>

Bir çalışma alanınız yoksa, aşağıdaki kaynaklardan birini kullanarak bir tane oluşturabilirsiniz:
* [Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [Azure portalı](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

Ayrıca, Azure portal tek bir Azure VM veya sanal makine ölçek kümesi için izlemeyi etkinleştirirken bir çalışma alanı oluşturabilirsiniz.

Log Analytics çalışma alanınızda Azure Ilkesi, Azure PowerShell veya Azure Resource Manager şablonları kullanan ölçekli bir senaryo ayarlamak için:

* ServiceMap ve InfrastructureInsights çözümlerini yükleyin. Bu yüklemeyi, belirtilen bir Azure Resource Manager şablonu kullanarak tamamlayabilirsiniz. Veya **Başlarken** sekmesinde, **çalışma alanını Yapılandır**' ı seçin.
* Performans sayaçları toplamak için Log Analytics çalışma alanı yapılandırın.

Çalışma alanınızı ölçekteki senaryoya göre yapılandırmak için aşağıdaki yöntemlerden birini kullanın:

* [Azure PowerShell](vminsights-enable-at-scale-powershell.md#set-up-a-log-analytics-workspace)kullanın.
* VM'ler için Azure İzleyici [**Ilkesi kapsamı**](vminsights-enable-at-scale-policy.md#manage-policy-coverage-feature-overview) sayfasında, **çalışma alanını Yapılandır**' ı seçin. 

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki tabloda VM'ler için Azure İzleyici desteklediği Windows ve Linux işletim sistemleri listelenmektedir. Bu bölümde daha sonra, birincil ve ikincil Linux işletim sistemi sürümünün ve desteklenen çekirdek sürümlerinin ayrıntılarını içeren tam bir liste bulacaksınız.

|İşletim sistemi sürümü |Performans |Haritalar |Durum |
|-----------|------------|-----|-------|
|Windows Server 2019 | X | X | X |
|Windows Server 2016 1803 | X | X | X |
|Windows Server 2016 | X | X | X |
|Windows Server 2012 R2 | X | X | X |
|Windows Server 2012 | X | X | |
|Windows Server 2008 R2 | X | X|  |
|Windows 10, 1803 | X | X | |
|Windows 8.1 | X | X | |
|Windows 8 | X | X | |
|Windows 7 SP1 | X | X | |
|Red Hat Enterprise Linux (RHEL) 6, 7| X | X| X |
|Ubuntu 14,04, 16,04, 18,04 | X | X | X |
|CentOS Linux 6, 7 | X | X | X |
|SUSE Linux Enterprise Server (SLES) 12 | X | X | X |
|8, 9,4 | X<sup>1</sup> | | X |

<sup>1</sup> VM'ler için Azure İzleyici performans özelliği yalnızca Azure İzleyici'deki kullanılabilir. Doğrudan Azure VM 'nin sol bölmesinden kullanılamaz.

>[!NOTE]
>VM'ler için Azure İzleyici sistem durumu özelliği bir Azure VM 'de [iç içe sanallaştırmayı](../../virtual-machines/windows/nested-virtualization.md) desteklemez.
>

>[!NOTE]
>Linux işletim sisteminde:
> - Yalnızca varsayılan ve SMP Linux çekirdek sürümleri desteklenir.
> - Fiziksel Adres Uzantısı (PAE) ve Xen, desteklenmeyen bir Linux dağıtımı için gibi standart olmayan çekirdek serbest bırakır. Örneğin, bir sürüm dizesi sistemiyle *2.6.16.21-0.8-xen* desteklenmiyor.
> - Standart kernels 'lerin yeniden derlemesi dahil olmak üzere özel kernels desteklenmez.
> - CentOSPlus Kernel destekleniyor.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |
| 7,6 | 3.10.0-957 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 6.9 | 2.6.32-696 |
| 6.10 | 2.6.32-754 |

#### <a name="centosplus"></a>CentOSPlus
| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 6.9 | 2.6.32-696.18.7<br>2.6.32-696.30.1 |
| 6.10 | 2.6.32-696.30.1<br>2.6.32-754.3.5 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| Ubuntu 18.04 | Çekirdek 4,15.\*<br>4,18 * |
| Ubuntu 16.04.3 | Çekirdek 4.15. * |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 kuruluş sunucusu

| İşletim sistemi sürümü | Çekirdek sürümü
|:--|:--|
|12 SP2 | 4.4. * |
|12 SP3 | 4.4. * |
|12 SP4 | 4.4. * |
|12 SP4 | Azure tarafından ayarlanan çekirdek |

### <a name="the-microsoft-dependency-agent"></a>Microsoft Dependency aracı

VM'ler için Azure İzleyici harita özelliği, verileri Microsoft bağımlılık Aracısı 'ndan alır. Log Analytics aracısını Log Analytics bağlantısını için bağımlılık Aracısı'nı kullanır. Bu nedenle, sisteminizde Log Analytics aracısının yüklü olması ve bağımlılık aracısıyla yapılandırılmış olması gerekir.

Tek bir Azure VM için VM'ler için Azure İzleyici etkinleştirmenize veya genişleme dağıtım yöntemini kullanmanıza bakılmaksızın aracıyı deneyimin bir parçası olarak yüklemek için Azure VM bağımlılık Aracısı uzantısı 'nı kullanın.

>[!NOTE]
>Bu bölümde açıklanan aşağıdaki bilgiler [hizmet eşlemesi çözümü](service-map.md)için de geçerlidir.  

Karma ortamda, bağımlılık aracısını el ile indirip yükleyebilirsiniz. VM 'niz Azure dışında barındırılıyorsa, bir otomatik dağıtım yöntemi kullanın.

Aşağıdaki tabloda, karma bir ortamda, eşleme özelliğini destekleyen bağlı kaynaklar açıklanmaktadır.

| Bağlı kaynak | Desteklenen | Açıklama |
|:--|:--|:--|
| Windows aracıları | Evet | [Windows aracıların Log Analytics aracı](../../azure-monitor/platform/log-analytics-agent.md)Ile birlikte bağımlılık aracısına ihtiyacı vardır. Daha fazla bilgi için bkz. [desteklenen işletim sistemleri](#supported-operating-systems). |
| Linux aracıları | Evet | Linux aracısının [Log Analytics aracısıyla](../../azure-monitor/platform/log-analytics-agent.md)birlikte, Linux aracılarının bağımlılık aracısına ihtiyacı vardır. Daha fazla bilgi için bkz. [desteklenen işletim sistemleri](#supported-operating-systems). |
| System Center Operations Manager yönetim grubu | Hayır | |

Bağımlılık aracısını şu konumlardan indirebilirsiniz:

| Dosya | İşletim Sistemi | Sürüm | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.9.1 | FCF9C1D9B20AD414051B49EE79144E595CCC411EB6D444D6D5B5A7B1874DCDEC |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.9.1 | 1CB447EF30FC042FE7499A686638F3F9B4F449692FB9D80096820F8024BE4D7C |

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

VM'ler için Azure İzleyici özellikleri etkinleştirmek ve erişmek için, *Log Analytics katkıda* bulunan rolüne sahip olmanız gerekir. Performansı, sistem durumunu ve eşleme verilerini görüntülemek için, Azure VM için *izleme okuyucu* rolüne sahip olmanız gerekir. Log Analytics çalışma alanı için Azure İzleyici VM'ler için yapılandırılmış olması gerekir.

Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../../azure-monitor/platform/manage-access.md).

## <a name="how-to-enable-azure-monitor-for-vms-preview"></a>VM'ler için Azure İzleyici etkinleştirme (Önizleme)

Bu tabloda açıklanan yöntemlerden birini kullanarak VM'ler için Azure İzleyici etkinleştirin:

| Dağıtım durumu | Yöntem | Açıklama |
|------------------|--------|-------------|
| Tek Azure VM veya sanal makine ölçek kümesi | [VM 'den etkinleştir](vminsights-enable-single-vm.md) | Doğrudan VM veya sanal makine ölçek kümesinden **Öngörüler (Önizleme)** öğesini seçerek tek bir Azure VM 'yi etkinleştirebilirsiniz. |
| Birden çok Azure VM veya sanal makine ölçek kümesi | [Azure Ilkesi aracılığıyla etkinleştir](vminsights-enable-at-scale-policy.md) | Azure Ilkesi ve kullanılabilir ilke tanımlarını kullanarak birden fazla Azure VM 'yi etkinleştirebilirsiniz. |
| Birden çok Azure VM veya sanal makine ölçek kümesi | [Azure PowerShell veya Azure Resource Manager şablonları aracılığıyla etkinleştir](vminsights-enable-at-scale-powershell.md) | Azure PowerShell veya Azure Resource Manager şablonlarını kullanarak, belirtilen bir abonelik veya kaynak grubu genelinde birden fazla Azure VM veya sanal makine ölçek kümesi etkinleştirebilirsiniz. |
| Karma bulut | [Karma ortam için etkinleştir](vminsights-enable-hybrid-cloud.md) | Veri merkezinizde veya diğer bulut ortamlarında barındırılan VM 'lere veya fiziksel bilgisayarlara dağıtım yapabilirsiniz. |

## <a name="performance-counters-enabled"></a>Performans sayaçları etkinleştirildi 

VM'ler için Azure İzleyici, kullandığı performans sayaçlarını toplamak için bir Log Analytics çalışma alanı yapılandırır. Aşağıdaki tablolarda, her 60 saniyede toplanan nesneler ve sayaçlar listelenmektedir.

### <a name="windows-performance-counters"></a>Windows performans sayaçları

|Nesne adı |Sayaç adı |
|------------|-------------|
|MantıksalDisk |% Boş alan |
|MantıksalDisk |Ort. Disk sn/Okuma |
|MantıksalDisk |Ort. Disk sn/Aktarım |
|MantıksalDisk |Ort. Disk sn/yazma |
|MantıksalDisk |Disk Bayt/sn |
|MantıksalDisk |Disk Okuma Bayt/sn |
|MantıksalDisk |Disk Okuma/sn |
|MantıksalDisk |Disk aktarımı/sn |
|MantıksalDisk |Disk Yazma Bayt/sn |
|MantıksalDisk |Disk Yazma/sn |
|MantıksalDisk |Boş megabayt |
|Bellek |Kullanılabilir MBayt |
|Ağ bağdaştırıcısı |Alınan Bayt/sn |
|Ağ bağdaştırıcısı |Gönderilen bayt/sn |
|İşlemci |% İşlemci zamanı |

### <a name="linux-performance-counters"></a>Linux performans sayaçları

|Nesne adı |Sayaç adı |
|------------|-------------|
|Mantıksal Disk |% Kullanılan alan |
|Mantıksal Disk |Disk Okuma Bayt/sn |
|Mantıksal Disk |Disk Okuma/sn |
|Mantıksal Disk |Disk aktarımı/sn |
|Mantıksal Disk |Disk Yazma Bayt/sn |
|Mantıksal Disk |Disk Yazma/sn |
|Mantıksal Disk |Boş megabayt |
|Mantıksal Disk |Mantıksal Disk Bayt/sn |
|Bellek |Kullanılabilir MBayt belleği |
|Ağ |Alınan toplam bayt sayısı |
|Ağ |Aktarılan toplam bayt |
|İşlemci |% İşlemci zamanı |

## <a name="management-packs"></a>Yönetim paketleri

VM'ler için Azure İzleyici etkinleştirildiğinde ve bir Log Analytics çalışma alanıyla yapılandırıldığında, bir yönetim paketi, bu çalışma alanına rapor veren tüm Windows bilgisayarlarına iletilir. [System Center Operations Manager yönetim grubunuzu](../../azure-monitor/platform/om-agents.md) Log Analytics çalışma alanıyla tümleştirdiyseniz, hizmet eşlemesi yönetim paketi Yönetim grubundan yönetim grubuna rapor veren Windows bilgisayarlarına dağıtılır.  

Yönetim Paketi *Microsoft. ıntelligencepacks. ApplicationDependencyMonitor*olarak adlandırılır. Yazılmış `%Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs\` klasörü. Yönetim paketinin kullandığı `%Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources\<AutoGeneratedID>\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll`veri kaynağı.

## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri

Microsoft, Azure İzleyici hizmeti kullanımınız vasıtasıyla kullanım ve performans verilerini otomatik olarak toplar. Microsoft bu verileri hizmetin kalitesini, güvenliğini ve bütünlüğünü geliştirmek için kullanır. 

Doğru ve verimli sorun giderme özellikleri sağlamak için, Map özelliği yazılımınızın yapılandırmasıyla ilgili verileri içerir. Veriler, işletim sistemi ve sürümü, IP adresi, DNS adı ve iş istasyonu adı gibi bilgiler sağlar. Microsoft, ad, adres veya diğer iletişim bilgilerinizi toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için bkz: [Microsoft Online Services gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

VM 'niz için izlemeyi etkinleştirmiş olduğunuza göre, izleme bilgileri VM'ler için Azure İzleyici analiz için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Sistem durumu özelliği kullanmayı öğrenmek için bkz: [görünümü VM sistem durumu için Azure İzleyici](vminsights-health.md). Bulunan Uygulama bağımlılıklarını görüntülemek için bkz: [Vm'leri harita görünümü Azure İzleyici](vminsights-maps.md).

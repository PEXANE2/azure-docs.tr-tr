---
title: VM'ler için Azure İzleyici genel bakış 'ı etkinleştir
description: VM'ler için Azure İzleyici dağıtmayı ve yapılandırmayı öğrenin. Sistem gereksinimlerini öğrenin.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: e3c5f6d7e04620cf36f6cd952467d47afd775b19
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87824775"
---
# <a name="enable-azure-monitor-for-vms-overview"></a>VM'ler için Azure İzleyici genel bakış 'ı etkinleştir

Bu makalede, aşağıdakilerin sistem durumunu ve performansını izlemeye VM'ler için Azure İzleyici olanak tanımak için kullanılabilen seçeneklere genel bir bakış sunulmaktadır:

- Azure sanal makineleri 
- Azure sanal makine ölçek kümeleri
- Azure Arc ile bağlantılı karma sanal makineler
- Şirket içi sanal makineler
- Başka bir bulut ortamında barındırılan sanal makineler.  

VM'ler için Azure İzleyici ayarlamak için:

* Azure portal doğrudan menüsünden **Öngörüler** ' i seçerek tek BIR Azure VM, Azure VMSS veya Azure yay makinesini etkinleştirin.
* Azure Ilkesi 'ni kullanarak birden fazla Azure VM 'yi, Azure VMSS 'yi veya Azure yay makinelerini etkinleştirin. Bu yöntem, mevcut ve yeni VM 'lerde ve ölçek kümelerinde gerekli bağımlılıkların yüklü ve düzgün şekilde yapılandırıldığından emin olmanızı sağlar. Uyumsuz VM 'Ler ve ölçek kümeleri raporlanır, bu sayede onları etkinleştirip etkinleştirmemeye karar verebilirsiniz.
* PowerShell kullanarak, belirtilen bir abonelik veya kaynak grubundaki birden fazla Azure VM 'yi, Azure Arc VM 'Leri, Azure VMSS veya Azure yay makinelerini etkinleştirin.
* Şirket ağınızda veya diğer bulut ortamınızda barındırılan VM 'Leri veya fiziksel bilgisayarları izlemek için VM'ler için Azure İzleyici etkinleştirin.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, aşağıdaki bölümlerde yer alan bilgileri anladığınızdan emin olun. 

>[!NOTE]
>Bu bölümde açıklanan aşağıdaki bilgiler [hizmet eşlemesi çözümü](service-map.md)için de geçerlidir.  

### <a name="log-analytics"></a>Log Analytics

VM'ler için Azure İzleyici, aşağıdaki bölgelerde bir Log Analytics çalışma alanını destekler:

- Orta Batı ABD
- Batı ABD
- Batı ABD 2
- Orta Güney ABD
- Doğu ABD
- Doğu ABD 2
- Central US
- Orta Kuzey ABD
- US Gov az
- US Gov VA
- Orta Kanada
- Güney Birleşik Krallık
- Kuzey Avrupa
- West Europe
- Doğu Asya
- Güneydoğu Asya
- Central India
- Doğu Japonya
- Doğu Avustralya
- Güneydoğu Avustralya

>[!NOTE]
>Azure VM 'Leri herhangi bir bölgede izleyebilirsiniz. VM 'Ler, Log Analytics çalışma alanı tarafından desteklenen bölgelerle sınırlı değildir.
>

Log Analytics bir çalışma alanınız yoksa, kaynaklardan birini kullanarak bir tane oluşturabilirsiniz:
* [Azure CLI](../learn/quick-create-workspace-cli.md)
* [PowerShell](../platform/powershell-workspace-configuration.md)
* [Azure Portal](../learn/quick-create-workspace.md)
* [Azure Resource Manager](../platform/template-workspace-configuration.md)

- Azure sanal makine
- Azure sanal makine ölçek kümesi
- Azure Arc ile bağlantılı karma sanal makine

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki tabloda VM'ler için Azure İzleyici desteklediği Windows ve Linux işletim sistemleri listelenmektedir. Bu bölümde daha sonra, birincil ve ikincil Linux işletim sistemi sürümünün ve desteklenen çekirdek sürümlerinin ayrıntılarını içeren tam bir liste bulacaksınız.

|İşletim sistemi sürümü |Performans |Haritalar |
|-----------|------------|-----|
|Windows Server 2019 | X | X |
|Windows Server 2016 1803 | X | X |
|Windows Server 2016 | X | X |
|Windows Server 2012 R2 | X | X |
|Windows Server 2012 | X | X |
|Windows Server 2008 R2 | X | X|
|Windows 10 1803 | X | X |
|Windows 8.1 | X | X |
|Windows 8 | X | X |
|Windows 7 SP1 | X | X |
|Red Hat Enterprise Linux (RHEL) 6, 7| X | X| 
|Ubuntu 18,04, 16,04 | X | X |
|CentOS Linux 7, 6 | X | X |
|SUSE Linux Enterprise Server (SLES) 12 | X | X |
|Deyi 9,4, 8 | X<sup>1</sup> | |

<sup>1</sup> VM'ler için Azure izleyici performans özelliği yalnızca Azure izleyici 'den kullanılabilir. Doğrudan Azure VM 'nin sol bölmesinden kullanılamaz.

>[!NOTE]
>Linux işletim sisteminde:
> - Yalnızca varsayılan ve SMP Linux çekirdek sürümleri desteklenir.
> - Fiziksel adres uzantısı (PAE) ve Xen gibi standart olmayan çekirdek sürümleri, hiçbir Linux dağıtımı için desteklenmez. Örneğin, *2.6.16.21-0.8-Xen* yayın dizesiyle bir sistem desteklenmez.
> - Standart kernels 'lerin yeniden derlemesi dahil olmak üzere özel kernels desteklenmez.
> - CentOSPlus Kernel destekleniyor.
> - Linux çekirdeğinin Spectre güvenlik açığı için düzeltme eki uygulanmış olması gerekir. Daha fazla ayrıntı için lütfen Linux dağıtım satıcınıza başvurun.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 7,6 | 3.10.0-957 |
| 7,5 | 3.10.0-862 |
| 7.4 | 3.10.0-693 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 6,10 | 2.6.32-754 |
| 6,9 | 2.6.32-696 |

#### <a name="centosplus"></a>CentOSPlus

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 6,10 | 2.6.32-754.3.5<br>2.6.32-696.30.1 |
| 6,9 | 2.6.32-696.30.1<br>2.6.32-696.18.7 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 18,04 | 5.3.0-1020<br>5,0 (Azure tarafından ayarlanmış çekirdek dahil)<br>4,18* <br> 4,15* |
| 16.04.3 | 4,15. * |
| 16,04 | 4,13.\*<br>4,11.\*<br>4,10.\*<br>4,8.\*<br>4,4.\* |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
|12 SP4 | 4,12. * (Azure tarafından ayarlanmış çekirdek dahil) |
|12 SP3 | 4,4. * |
|12 SP2 | 4,4. * |

#### <a name="debian"></a>Debian 

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 9 | 4,9 | 

## <a name="supported-azure-arc-machines"></a>Desteklenen Azure yay makineleri
VM'ler için Azure İzleyici, Arc uzantı hizmetinin kullanılabildiği bölgelerde Azure Arc etkin sunucuları için kullanılabilir. Arc aracısının 0,9 veya sonraki bir sürümünü çalıştırıyor olmanız gerekir.

| Bağlı kaynak | Desteklenir | Açıklama |
|:--|:--|:--|
| Windows aracıları | Evet | [Windows aracıların Log Analytics aracı](../platform/log-analytics-agent.md)Ile birlikte bağımlılık aracısına ihtiyacı vardır. Daha fazla bilgi için bkz. [desteklenen işletim sistemleri](#supported-operating-systems). |
| Linux aracıları | Evet | Linux aracısının [Log Analytics aracısıyla](../platform/log-analytics-agent.md)birlikte, Linux aracılarının bağımlılık aracısına ihtiyacı vardır. Daha fazla bilgi için bkz. [desteklenen işletim sistemleri](#supported-operating-systems). |
| System Center Operations Manager yönetim grubu | Hayır | |

## <a name="agents"></a>Aracılar
VM'ler için Azure İzleyici, her bir sanal makinede veya sanal makine ölçek kümesi 'nin izlenmesi için aşağıdaki iki aracısının yüklü olmasını gerektirir. Bu aracıları yüklemek ve bu aracıları çalışma alanına bağlamak, kaynağı eklemek için tek gereksinimdir.

- [Log Analytics Aracısı](../platform/log-analytics-agent.md). Sanal makineden veya sanal makine ölçek kümesinden olayları ve performans verilerini toplar ve Log Analytics çalışma alanına gönderir. Azure kaynaklarında Log Analytics aracısına yönelik dağıtım yöntemleri [Windows](../../virtual-machines/extensions/oms-windows.md) ve [Linux](../../virtual-machines/extensions/oms-linux.md)için VM uzantısını kullanır.
- Bağımlılık Aracısı. Sanal makinede çalışan işlemler ve [VM'ler için Azure izleyici Içindeki eşleme özelliği](vminsights-maps.md)tarafından kullanılan dış işlem bağımlılıkları hakkında bulunan verileri toplar. Bağımlılık Aracısı, verilerini Azure Izleyici 'ye teslim etmek için Log Analytics aracısına bağımlıdır. Azure kaynaklarında bağımlılık aracısının dağıtım yöntemleri [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) ve [Linux](../../virtual-machines/extensions/agent-dependency-linux.md)için VM uzantısını kullanır.

> [!NOTE]
> Log Analytics Aracısı, System Center Operations Manager tarafından kullanılan aracıdır. VM'ler için Azure İzleyici, doğrudan bağlı olmaları durumunda Operations Manager tarafından izlenen aracıları izleyebilir ve bunlara bağımlılık aracısını yüklersiniz. Bir [Yönetim grubu bağlantısı](../tform/../platform/om-agents.md) aracılığıyla Azure izleyici 'ye bağlı aracılar VM'ler için Azure izleyici tarafından izlenemez.

Aşağıda, bu aracıları dağıtmaya yönelik birden çok yöntem verilmiştir. 

| Yöntem | Açıklama |
|:---|:---|
| [Azure Portal](./vminsights-enable-portal.md) | Her iki aracıyı tek bir sanal makineye, sanal makine ölçek kümesine veya Azure Arc ile bağlantılı karma sanal makinelere kurun. |
| [Kaynak Yöneticisi şablonları](vminsights-enable-powershell.md) | CLı ve PowerShell dahil Kaynak Yöneticisi şablonu dağıtmak için desteklenen yöntemlerden herhangi birini kullanarak her iki aracıyı de yükler. |
| [Azure İlkesi](./vminsights-enable-policy.md) | Bir sanal makine veya sanal makine ölçek kümesi oluşturulduğunda aracıları otomatik olarak yüklemek için Azure Ilke girişimi atayın. |
| [El ile yüklemesi](./vminsights-enable-hybrid.md) | Veri merkezinize veya diğer bulut ortamlarınıza dahil olmak üzere Azure dışında barındırılan bilgisayarlarda, aracıları Konuk işletim sistemine yükler. |




## <a name="management-packs"></a>Yönetim paketleri
VM'ler için Azure İzleyici için bir Log Analytics çalışma alanı yapılandırıldığında, bu çalışma alanına bağlı tüm Windows bilgisayarlarına iki yönetim paketi iletilir. Yönetim paketleri *Microsoft. ıntelligencepacks. ApplicationDependencyMonitor* ve *Microsoft. ıntelligencepacks. Vminsıghts* olarak adlandırılır ve *%ProgramFiles%\Microsoft Monitoring Fılest\sistem sağlığı hizmeti State\Management paketlerine yazılır \* . 

*Applicationdependencymonitor* yönetim paketi tarafından kullanılan veri kaynağı **% Program Files%\Microsoft Monitoring Fılest\sistem sağlığı hizmeti state\resources \<AutoGeneratedID>\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*. *Vminsıghts* yönetim paketi tarafından kullanılan veri kaynağı *% Program Files%\Microsoft Monitoring Fılest\sistem sağlığı hizmeti state\resources \<AutoGeneratedID> \ Microsoft.VirtualMachineMonitoringModule.dll*.

## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri

Microsoft, Azure Izleyici hizmetini kullanımınız aracılığıyla kullanım ve performans verilerini otomatik olarak toplar. Microsoft bu verileri hizmetin kalitesini, güvenliğini ve bütünlüğünü geliştirmek için kullanır. 

Doğru ve verimli sorun giderme özellikleri sağlamak için, Map özelliği yazılımınızın yapılandırmasıyla ilgili verileri içerir. Veriler, işletim sistemi ve sürümü, IP adresi, DNS adı ve iş istasyonu adı gibi bilgiler sağlar. Microsoft ad, adres veya diğer iletişim bilgileri toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için [Microsoft Online Services gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=512132)' ne bakın.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Sonraki adımlar

Performans izleme özelliğini nasıl kullanacağınızı öğrenmek için bkz. [VM'ler için Azure izleyici performansını görüntüleme](vminsights-performance.md). Bulunan uygulama bağımlılıklarını görüntülemek için bkz. [VM'ler için Azure izleyici haritasını görüntüleme](vminsights-maps.md).
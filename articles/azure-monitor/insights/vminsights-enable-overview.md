---
title: VM'ler için Azure İzleyici genel bakış 'ı etkinleştir
description: VM'ler için Azure İzleyici dağıtmayı ve yapılandırmayı öğrenin. Sistem gereksinimlerini öğrenin.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/25/2020
ms.openlocfilehash: 261e5f17e787fd96697b06a9b338e74ea0409454
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85507084"
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

## <a name="prerequisites"></a>Ön koşullar

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
- Orta ABD
- Orta Kuzey ABD
- US Gov az
- US Gov VA
- Orta Kanada
- Güney Birleşik Krallık
- Kuzey Avrupa
- Batı Avrupa
- Doğu Asya
- Güneydoğu Asya
- Orta Hindistan
- Doğu Japonya
- Doğu Avustralya
- Güneydoğu Avustralya

>[!NOTE]
>Azure VM 'Leri herhangi bir bölgede izleyebilirsiniz. VM 'Ler, Log Analytics çalışma alanı tarafından desteklenen bölgelerle sınırlı değildir.
>

Log Analytics bir çalışma alanınız yoksa, kaynaklardan birini kullanarak bir tane oluşturabilirsiniz:
* [Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [Azure portalındaki](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

Ayrıca, Azure portal tek bir Azure VM veya sanal makine ölçek kümesi için izlemeyi etkinleştirirken bir çalışma alanı oluşturabilirsiniz.

Azure Ilkesi, Azure PowerShell veya Azure Resource Manager şablonları kullanan ölçekli bir senaryo ayarlamak için, *Vminsıghts* çözümünü yüklemelisiniz. Bunu aşağıdaki yöntemlerden biriyle yapabilirsiniz:

* [Azure PowerShell](vminsights-enable-at-scale-powershell.md#set-up-a-log-analytics-workspace)kullanın.
* VM'ler için Azure İzleyici [**Ilkesi kapsamı**](vminsights-enable-at-scale-policy.md#manage-policy-coverage-feature-overview) sayfasında, **çalışma alanını Yapılandır**' ı seçin. 

### <a name="azure-arc-machines"></a>Azure yay makineleri
VM'ler için Azure İzleyici, Arc uzantı hizmetinin kullanılabildiği bölgelerde Azure Arc etkin sunucuları için kullanılabilir. Kullanıcıların, Arc özellikli sunucularında VM'ler için Azure İzleyici etkinleştirmek için Arc aracısının sürüm 0,9 veya üzeri bir sürümü çalıştırması gerekir.

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

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

### <a name="the-microsoft-dependency-agent"></a>Microsoft bağımlılık Aracısı

VM'ler için Azure İzleyici harita özelliği, verileri Microsoft bağımlılık Aracısı 'ndan alır. Bağımlılık Aracısı, Log Analytics bağlantısı için Log Analytics aracısına bağımlıdır. Bu nedenle, sisteminizde Log Analytics aracısının yüklü olması ve bağımlılık aracısıyla yapılandırılmış olması gerekir.

Tek bir Azure VM için VM'ler için Azure İzleyici etkinleştirmenize veya genişleme dağıtım yöntemini kullanmanıza bakılmaksızın, aracıyı deneyimin bir parçası olarak yüklemek için [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) veya [LINUX](../../virtual-machines/extensions/agent-dependency-linux.md) için Azure VM bağımlılık Aracısı uzantısı ' nı kullanın.

>[!NOTE]
>Bu bölümde açıklanan aşağıdaki bilgiler [hizmet eşlemesi çözümü](service-map.md)için de geçerlidir.  

Karma bir ortamda, bağımlılık aracısını el ile indirip yükleyebilir veya otomatikleştirilmiş bir yöntemi kullanabilirsiniz.

Aşağıdaki tabloda, bir karma ortamda harita özelliğinin desteklediği bağlı kaynaklar açıklanmaktadır.

| Bağlı kaynak | Destekleniyor | Açıklama |
|:--|:--|:--|
| Windows aracıları | Evet | [Windows aracıların Log Analytics aracı](../../azure-monitor/platform/log-analytics-agent.md)Ile birlikte bağımlılık aracısına ihtiyacı vardır. Daha fazla bilgi için bkz. [desteklenen işletim sistemleri](#supported-operating-systems). |
| Linux aracıları | Evet | Linux aracısının [Log Analytics aracısıyla](../../azure-monitor/platform/log-analytics-agent.md)birlikte, Linux aracılarının bağımlılık aracısına ihtiyacı vardır. Daha fazla bilgi için bkz. [desteklenen işletim sistemleri](#supported-operating-systems). |
| System Center Operations Manager yönetim grubu | Hayır | |

Bağımlılık aracısını şu konumlardan indirebilirsiniz:

| Dosya | İşletim Sistemi | Sürüm | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.10.4.10090 | B4E1FF9C1E5CD254AA709AEF9723A81F04EC0763C327567C582CE99C0C5A0BAE  |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.10.4.10090 | A56E310D297CE3B343AE8F4A6F72980F1C3173862D6169F1C713C2CA09660A9F |

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

VM'ler için Azure İzleyici özellikleri etkinleştirmek ve erişmek için, *Log Analytics katkıda* bulunan rolüne sahip olmanız gerekir. Performansı, sistem durumunu ve eşleme verilerini görüntülemek için, Azure VM için *izleme okuyucu* rolüne sahip olmanız gerekir. Log Analytics çalışma alanı VM'ler için Azure İzleyici için yapılandırılmış olmalıdır.

Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../../azure-monitor/platform/manage-access.md).

## <a name="how-to-enable-azure-monitor-for-vms"></a>VM'ler için Azure İzleyici etkinleştirme

Bu tabloda açıklanan yöntemlerden birini kullanarak VM'ler için Azure İzleyici etkinleştirin:

| Dağıtım durumu | Yöntem | Açıklama |
|------------------|--------|-------------|
| Tek Azure VM, Azure VMSS veya Azure Arc makinesi | [Portaldan etkinleştir](vminsights-enable-single-vm.md) | Azure portal menüden doğrudan **Öngörüler** ' i seçin. |
| Birden çok Azure VM, Azure VMSS veya Azure Arc makinesi | [Azure Ilkesi aracılığıyla etkinleştir](vminsights-enable-at-scale-policy.md) | Bir VM veya VMSS oluşturulduğunda otomatik olarak etkinleştirmek için Azure Ilkesini kullanın. |
| | [Azure PowerShell veya Azure Resource Manager şablonları aracılığıyla etkinleştir](vminsights-enable-at-scale-powershell.md) | Belirtilen bir abonelik veya kaynak grubu genelinde birden çok Azure VM, Azure Arc VM veya Azure VMSS 'yi etkinleştirmek için Azure PowerShell veya Azure Resource Manager şablonlarını kullanın. |
| Karma bulut | [Karma ortam için etkinleştir](vminsights-enable-hybrid-cloud.md) | Veri merkezinizde veya diğer bulut ortamlarında barındırılan VM 'lere veya fiziksel bilgisayarlara dağıtın. |

## <a name="management-packs"></a>Yönetim paketleri

VM'ler için Azure İzleyici etkinleştirildiğinde ve bir Log Analytics çalışma alanıyla yapılandırıldığında, bir yönetim paketi, bu çalışma alanına rapor veren tüm Windows bilgisayarlarına iletilir. [System Center Operations Manager yönetim grubunuzu](../../azure-monitor/platform/om-agents.md) Log Analytics çalışma alanıyla tümleştirdiyseniz, hizmet eşlemesi yönetim paketi Yönetim grubundan yönetim grubuna rapor veren Windows bilgisayarlarına dağıtılır.  

Yönetim Paketi *Microsoft. ıntelligencepacks. ApplicationDependencyMonitor*olarak adlandırılır. Yazılmış `%Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs\` klasörü. Yönetim paketinin kullandığı veri kaynağı `%Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources\<AutoGeneratedID>\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll` .

## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri

Microsoft, Azure Izleyici hizmetini kullanımınız aracılığıyla kullanım ve performans verilerini otomatik olarak toplar. Microsoft bu verileri hizmetin kalitesini, güvenliğini ve bütünlüğünü geliştirmek için kullanır. 

Doğru ve verimli sorun giderme özellikleri sağlamak için, Map özelliği yazılımınızın yapılandırmasıyla ilgili verileri içerir. Veriler, işletim sistemi ve sürümü, IP adresi, DNS adı ve iş istasyonu adı gibi bilgiler sağlar. Microsoft ad, adres veya diğer iletişim bilgileri toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için [Microsoft Online Services gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=512132)' ne bakın.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

VM 'niz için izlemeyi etkinleştirmiş olduğunuza göre, izleme bilgileri VM'ler için Azure İzleyici analiz için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Performans izleme özelliğini nasıl kullanacağınızı öğrenmek için bkz. [VM'ler için Azure izleyici performansını görüntüleme](vminsights-performance.md). Bulunan uygulama bağımlılıklarını görüntülemek için bkz. [VM'ler için Azure izleyici haritasını görüntüleme](vminsights-maps.md).

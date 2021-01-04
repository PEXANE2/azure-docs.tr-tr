---
title: VM'ler için Azure İzleyici genel bakış 'ı etkinleştir
description: VM'ler için Azure İzleyici dağıtmayı ve yapılandırmayı öğrenin. Sistem gereksinimlerini öğrenin.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 12/22/2020
ms.custom: references_regions
ms.openlocfilehash: ce90ab160696e2c38d917a391eecb0d51a31282f
ms.sourcegitcommit: 44844a49afe8ed824a6812346f5bad8bc5455030
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/23/2020
ms.locfileid: "97740598"
---
# <a name="enable-azure-monitor-for-vms-overview"></a>VM'ler için Azure İzleyici genel bakış 'ı etkinleştir

Bu makalede, aşağıdakilerin sistem durumunu ve performansını izlemeye VM'ler için Azure İzleyici olanak tanımak için kullanılabilen seçeneklere genel bir bakış sunulmaktadır:

- Azure sanal makineleri 
- Azure sanal makine ölçek kümeleri
- Azure Arc ile bağlantılı karma sanal makineler
- Şirket içi sanal makineler
- Başka bir bulut ortamında barındırılan sanal makineler.  

VM'ler için Azure İzleyici ayarlamak için:

* Azure portal doğrudan menüsünden **Öngörüler** ' i seçerek tek bir Azure sanal makinesini, Azure sanal makine ölçek kümesini veya Azure yay makinesini etkinleştirin.
* Azure Ilkesi 'ni kullanarak birden çok Azure sanal makinesini, Azure sanal makinelerini veya Azure yay makinelerini etkinleştirin. Bu yöntem, mevcut ve yeni VM 'lerde ve ölçek kümelerinde gerekli bağımlılıkların yüklü ve düzgün şekilde yapılandırıldığından emin olmanızı sağlar. Uyumsuz sanal makineler ve ölçek kümeleri raporlanır, bu sayede onları etkinleştirip etkinleştirmemeye karar verebilirsiniz.
* PowerShell kullanarak, belirtilen bir abonelik veya kaynak grubundaki birden çok Azure sanal makinesini, Azure Arc sanal makinelerini, Azure sanal makine ölçek kümelerini veya Azure Arc makinelerini etkinleştirin.
* Şirket ağınızda veya diğer bulut ortamınızda barındırılan VM 'Leri veya fiziksel bilgisayarları izlemek için VM'ler için Azure İzleyici etkinleştirin.

## <a name="supported-machines"></a>Desteklenen makineler
VM'ler için Azure İzleyici aşağıdaki makineleri destekler:

- Azure sanal makine
- Azure sanal makine ölçek kümesi
- Azure Arc ile bağlantılı karma sanal makine


## <a name="supported-azure-arc-machines"></a>Desteklenen Azure yay makineleri
VM'ler için Azure İzleyici, Arc uzantı hizmetinin kullanılabildiği bölgelerde Azure Arc etkin sunucuları için kullanılabilir. Arc aracısının 0,9 veya sonraki bir sürümünü çalıştırıyor olmanız gerekir.

| Bağlı kaynak | Desteklenir | Açıklama |
|:--|:--|:--|
| Windows aracıları | Yes | [Windows aracıların Log Analytics aracı](../platform/log-analytics-agent.md)Ile birlikte bağımlılık aracısına ihtiyacı vardır. Daha fazla bilgi için bkz. [desteklenen işletim sistemleri](../platform/agents-overview.md#supported-operating-systems). |
| Linux aracıları | Yes | Linux aracısının [Log Analytics aracısıyla](../platform/log-analytics-agent.md)birlikte, Linux aracılarının bağımlılık aracısına ihtiyacı vardır. Daha fazla bilgi için bkz. [desteklenen işletim sistemleri](#supported-operating-systems). |
| System Center Operations Manager yönetim grubu | Hayır | |

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

VM'ler için Azure İzleyici, Log Analytics Aracısı ve bağımlılık aracısını destekleyen tüm işletim sistemlerini destekler. Tüm liste için bkz. [Azure izleyici aracılarına genel bakış ](../platform/agents-overview.md#supported-operating-systems) .

VM'ler için Azure İzleyici destekleyen bağımlılık aracısının Linux desteği hakkında aşağıdaki noktalara göz atın:

- Yalnızca varsayılan ve SMP Linux çekirdek sürümleri desteklenir.
- Fiziksel adres uzantısı (PAE) ve Xen gibi standart olmayan çekirdek sürümleri, hiçbir Linux dağıtımı için desteklenmez. Örneğin, *2.6.16.21-0.8-Xen* yayın dizesiyle bir sistem desteklenmez.
- Standart kernels 'lerin yeniden derlemesi dahil olmak üzere özel kernels desteklenmez.
- Sürüm 9,4 ' den başka bir deyişle, harita özelliği desteklenmez ve performans özelliği yalnızca Azure Izleyici menüsünden kullanılabilir. Doğrudan Azure VM 'nin sol bölmesinden kullanılamaz.
- CentOSPlus Kernel destekleniyor.
- Linux çekirdeğinin Spectre güvenlik açığı için düzeltme eki uygulanmış olması gerekir. Daha fazla ayrıntı için lütfen Linux dağıtım satıcınıza başvurun.
## <a name="log-analytics-workspace"></a>Log Analytics çalışma alanı
VM'ler için Azure İzleyici, Log Analytics bir çalışma alanı gerektirir. Bu çalışma alanının ayrıntıları ve gereksinimleri için bkz. [VM'ler için Azure İzleyici Log Analytics çalışma alanını yapılandırma](vminsights-configure-workspace.md) .
## <a name="agents"></a>Aracılar
VM'ler için Azure İzleyici, her bir sanal makinede veya sanal makine ölçek kümesi 'nin izlenmesi için aşağıdaki iki aracısının yüklü olmasını gerektirir. Bu aracıları yüklemek ve bu aracıları çalışma alanına bağlamak, kaynağı eklemek için tek gereksinimdir.

- [Log Analytics Aracısı](../platform/log-analytics-agent.md). Sanal makineden veya sanal makine ölçek kümesinden olayları ve performans verilerini toplar ve Log Analytics çalışma alanına gönderir. Azure kaynaklarında Log Analytics aracısına yönelik dağıtım yöntemleri [Windows](../../virtual-machines/extensions/oms-windows.md) ve [Linux](../../virtual-machines/extensions/oms-linux.md)için VM uzantısını kullanır.
- Bağımlılık Aracısı. Sanal makinede çalışan işlemler ve [VM'ler için Azure izleyici Içindeki eşleme özelliği](vminsights-maps.md)tarafından kullanılan dış işlem bağımlılıkları hakkında bulunan verileri toplar. Bağımlılık Aracısı, verilerini Azure Izleyici 'ye teslim etmek için Log Analytics aracısına bağımlıdır. Azure kaynaklarında bağımlılık aracısına yönelik dağıtım yöntemleri [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) ve [Linux](../../virtual-machines/extensions/agent-dependency-linux.md)için VM uzantısını kullanır.

> [!NOTE]
> Log Analytics Aracısı, System Center Operations Manager tarafından kullanılan aracıdır. VM'ler için Azure İzleyici, doğrudan bağlı olmaları durumunda Operations Manager tarafından izlenen aracıları izleyebilir ve bunlara bağımlılık aracısını yüklersiniz. Bir [Yönetim grubu bağlantısı](../tform/../platform/om-agents.md) aracılığıyla Azure izleyici 'ye bağlı aracılar VM'ler için Azure izleyici tarafından izlenemez.

Aşağıda, bu aracıları dağıtmaya yönelik birden çok yöntem verilmiştir. 

| Yöntem | Açıklama |
|:---|:---|
| [Azure Portal](./vminsights-enable-portal.md) | Her iki aracıyı tek bir sanal makineye, sanal makine ölçek kümesine veya Azure Arc ile bağlantılı karma sanal makinelere kurun. |
| [Resource Manager şablonları](vminsights-enable-resource-manager.md) | CLı ve PowerShell dahil Kaynak Yöneticisi şablonu dağıtmak için desteklenen yöntemlerden herhangi birini kullanarak her iki aracıyı de yükler. |
| [Azure İlkesi](./vminsights-enable-policy.md) | Bir sanal makine veya sanal makine ölçek kümesi oluşturulduğunda aracıları otomatik olarak yüklemek için Azure Ilke girişimi atayın. |
| [El ile yüklemesi](./vminsights-enable-hybrid.md) | Veri merkezinize veya diğer bulut ortamlarınıza dahil olmak üzere Azure dışında barındırılan bilgisayarlarda, aracıları Konuk işletim sistemine yükler. |




## <a name="management-packs"></a>Yönetim paketleri
VM'ler için Azure İzleyici için bir Log Analytics çalışma alanı yapılandırıldığında, bu çalışma alanına bağlı tüm Windows bilgisayarlarına iki yönetim paketi iletilir. Yönetim paketleri *Microsoft. ıntelligencepacks. ApplicationDependencyMonitor* ve *Microsoft. ıntelligencepacks. Vminsıghts* olarak adlandırılır ve *%ProgramFiles%\Microsoft Monitoring Fılest\sistem sağlığı hizmeti State\Management paketlerine* yazılır. 

*Applicationdependencymonitor* yönetim paketi tarafından kullanılan veri kaynağı **% Program Files%\Microsoft Monitoring Fılest\sistem sağlığı hizmeti state\resources \<AutoGeneratedID>\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*. *Vminsıghts* yönetim paketi tarafından kullanılan veri kaynağı *% Program Files%\Microsoft Monitoring Fılest\sistem sağlığı hizmeti state\resources \<AutoGeneratedID> \ Microsoft.VirtualMachineMonitoringModule.dll*.

## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri

Microsoft, Azure Izleyici hizmetini kullanımınız aracılığıyla kullanım ve performans verilerini otomatik olarak toplar. Microsoft bu verileri hizmetin kalitesini, güvenliğini ve bütünlüğünü geliştirmek için kullanır. 

Doğru ve verimli sorun giderme özellikleri sağlamak için, Map özelliği yazılımınızın yapılandırmasıyla ilgili verileri içerir. Veriler, işletim sistemi ve sürümü, IP adresi, DNS adı ve iş istasyonu adı gibi bilgiler sağlar. Microsoft ad, adres veya diğer iletişim bilgileri toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için [Microsoft Online Services gizlilik bildirimi](https://go.microsoft.com/fwlink/?LinkId=512132)' ne bakın.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Sonraki adımlar

Performans izleme özelliğini nasıl kullanacağınızı öğrenmek için bkz. [VM'ler için Azure izleyici performansını görüntüleme](vminsights-performance.md). Bulunan uygulama bağımlılıklarını görüntülemek için bkz. [VM'ler için Azure izleyici haritasını görüntüleme](vminsights-maps.md).

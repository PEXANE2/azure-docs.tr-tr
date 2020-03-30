---
title: VM'lere genel bakış için Azure Monitörünü etkinleştirme
description: VM'ler için Azure Monitörünü nasıl dağıtıp yapılandırıştırmayı öğrenin. Sistem gereksinimlerini öğrenin.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/27/2020
ms.openlocfilehash: 26ed33e967aff1714d2a6fb174eab623e71534c2
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80382741"
---
# <a name="enable-azure-monitor-for-vms-overview"></a>VM'lere genel bakış için Azure Monitörünü etkinleştirme

Bu makalede, sanal makinelerinizdeki Sanal Makinelerde Azure Monitor'un sistem durumu ve performansı izlemesi için kullanılabilir seçeneklere genel bir bakış sunulmaktadır. Azure sanal makinelerde (VM' ler) ve sanal makine ölçek kümelerinde, şirket içi VM'lerde veya başka bir bulut ortamında barındırılan SANAL M'lerde çalışan uygulama bağımlılıklarını keşfedin.  

VM'ler için Azure Monitör'ü ayarlamak için:

* Doğrudan VM veya sanal makine ölçeği kümesinden **Öngörüler** seçerek tek bir Azure VM veya sanal makine ölçeği kümesini etkinleştirin.
* Azure İlkesi'ni kullanarak iki veya daha fazla Azure VM'sini ve sanal makine ölçek kümelerini etkinleştirin. Bu yöntem, varolan ve yeni VM'lerde ve ölçek kümelerinde gerekli bağımlılıkların yüklenmesini ve düzgün şekilde yapılandırılmasını sağlar. Uyumlu olmayan VM'ler ve ölçek kümeleri bildirilir, böylece bunları etkinleştirip etkinleştirmeyeceğiniz ve bunları düzeltip düzeltmeyeceğinizkonusunda karar verebilirsiniz.
* PowerShell'i kullanarak belirli bir abonelik veya kaynak grubunda iki veya daha fazla Azure VM veya sanal makine ölçeği kümesini etkinleştirin.
* Kurumsal ağınızda veya diğer bulut ortamında barındırılan VM'leri veya fiziksel bilgisayarları izlemek için VM'ler için Azure Monitörünü etkinleştirin.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdaki bölümlerdeki bilgileri anladığınızdan emin olun. 

>[!NOTE]
>Bu bölümde açıklanan aşağıdaki bilgiler Hizmet [Haritası çözümü](service-map.md)için de geçerlidir.  

### <a name="log-analytics"></a>Log Analytics

VM'ler için Azure Monitor, aşağıdaki bölgelerde bir Log Analytics çalışma alanını destekler:

- Orta Batı ABD
- Batı ABD
- Batı ABD 2
- Orta Güney ABD
- Doğu ABD
- Doğu ABD 2
- Orta ABD
- Orta Kuzey ABD
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
>Azure VM'leri herhangi bir bölgede izleyebilirsiniz. VM'lerin kendileri, Log Analytics çalışma alanı tarafından desteklenen bölgelerle sınırlı değildir.
>

Log Analytics çalışma alanınız yoksa, kaynaklardan birini kullanarak bir tane oluşturabilirsiniz:
* [Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [Powershell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [Azure portalında](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

Azure portalında ayarlanmış tek bir Azure VM veya sanal makine ölçeği için izleme sağlarken bir çalışma alanı da oluşturabilirsiniz.

Log Analytics çalışma alanınızda Azure İlkesi, Azure PowerShell veya Azure Kaynak Yöneticisi şablonlarını kullanan ölçekte bir senaryo ayarlamak için:

* *ServiceMap* ve *InfrastructureInsights* çözümlerini yükleyin. Sağlanan Azure Kaynak Yöneticisi şablonunu kullanarak bu yüklemeyi tamamlayabilirsiniz. Veya Azure portalındaki **Başlat** sekmesinde, **Çalışma Alanını Yapılandır'ı**seçin.
* Günlük Analizi çalışma alanını performans sayaçları toplamak için yapılandırın.

Çalışma alanınızı ölçekdüzeyinde senaryo için yapılandırmak için aşağıdaki yöntemlerden birini kullanın:

* [Azure PowerShell'i](vminsights-enable-at-scale-powershell.md#set-up-a-log-analytics-workspace)kullanın.
* VM [**İlke Kapsamı**](vminsights-enable-at-scale-policy.md#manage-policy-coverage-feature-overview) için Azure Monitörü **sayfasında, Çalışma Alanını Yapılandır'ı**seçin. 

### <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri

Aşağıdaki tabloda, Azure Monitor for VM'lerin desteklediği Windows ve Linux işletim sistemleri listelenmektedir. Daha sonra bu bölümde, büyük ve küçük Linux işletim sistemi sürümü ve desteklenen çekirdek sürümleri ayrıntıları tam bir liste bulacaksınız.

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
|Kırmızı Şapka Kurumsal Linux (RHEL) 6, 7| X | X| 
|Ubuntu 18.04, 16.04 | X | X |
|CentOS Linux 7, 6 | X | X |
|SUSE Linux Kurumsal Sunucu (SLES) 12 | X | X |
|Debian 9.4, 8 | X<sup>1</sup> | |

<sup>1</sup> VM'ler için Azure Monitor'un Performans özelliği yalnızca Azure Monitor'dan edinilebilir. Doğrudan Azure VM'nin sol bölmesinden kullanılamaz.

>[!NOTE]
>Linux işletim sisteminde:
> - Yalnızca varsayılan ve SMP Linux çekirdek sürümleri desteklenir.
> - Fiziksel Adres Uzantısı (PAE) ve Xen gibi standart dışı çekirdek sürümleri herhangi bir Linux dağıtımı için desteklenmez. Örneğin, *2.6.16.21-0.8-xen* sürüm dizesini olan bir sistem desteklenmez.
> - Standart çekirdeklerin yeniden derlemeleri de dahil olmak üzere özel çekirdekler desteklenmez.
> - CentOSPlus çekirdeği desteklenir.
> - Linux çekirdeği Spectre güvenlik açığı için yamalı olmalıdır. Daha fazla bilgi için lütfen Linux dağıtım satıcınıza danışın.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 7.6 | 3.10.0-957 |
| 7,5 | 3.10.0-862 |
| 7.4 | 3.10.0-693 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 6.10 | 2.6.32-754 |
| 6.9 | 2.6.32-696 |

#### <a name="centosplus"></a>CentOSPlus

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 6.10 | 2.6.32-754.3.5<br>2.6.32-696.30.1 |
| 6.9 | 2.6.32-696.30.1<br>2.6.32-696.18.7 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 18.04 | 5.0 (Azure ayarlı çekirdek içerir)<br>4,18*<br>4,15* |
| 16.04.3 | 4.15.* |
| 16.04 | 4.13.\*<br>4.11.\*<br>4.10.\*<br>4.8.\*<br>4.4.\* |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Kurumsal Sunucu

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
|12 SP4 | 4.12.* (Azure ayarlı çekirdek içerir) |
|12 SP3 | 4.4.* |
|12 SP2 | 4.4.* |

#### <a name="debian"></a>Debian 

| İşletim sistemi sürümü | Çekirdek sürümü |
|:--|:--|
| 9 | 4.9 | 

### <a name="the-microsoft-dependency-agent"></a>Microsoft Bağımlılık aracısı

SANAL'lar için Azure Monitor'daki Harita özelliği, verilerini Microsoft Bağımlılık aracısından alır. Bağımlılık aracısı, Log Analytics'e bağlantısı için Log Analytics temsilcisine güvenir. Bu nedenle sisteminizin Log Analytics aracısını Yükletmesi ve Bağımlılık aracısıyla yapılandırması gerekir.

İster tek bir Azure VM için Azure Monitor'u etkinleştirin, ister ölçekte dağıtım yöntemini kullanın, aracıyı deneyimin bir parçası olarak yüklemek için [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) veya [Linux](../../virtual-machines/extensions/agent-dependency-linux.md) için Azure VM Bağımlılık aracısı uzantısını kullanın.

>[!NOTE]
>Bu bölümde açıklanan aşağıdaki bilgiler Hizmet [Haritası çözümü](service-map.md)için de geçerlidir.  

Karma bir ortamda, Bağımlılık aracısını el ile veya otomatik bir yöntem kullanarak indirebilir ve yükleyebilirsiniz.

Aşağıdaki tabloda, Harita özelliğinin karma bir ortamda desteklediği bağlı kaynaklar açıklanmaktadır.

| Bağlı kaynak | Destekleniyor | Açıklama |
|:--|:--|:--|
| Windows aracıları | Evet | [Windows için Log Analytics aracısı](../../azure-monitor/platform/log-analytics-agent.md)ile birlikte, Windows aracıları Bağımlılık aracısı gerekir. Daha fazla bilgi için [desteklenen işletim sistemlerine](#supported-operating-systems)bakın. |
| Linux aracıları | Evet | [Linux için Log Analytics aracısı](../../azure-monitor/platform/log-analytics-agent.md)ile birlikte, Linux ajanları Bağımlılık aracısı gerekir. Daha fazla bilgi için [desteklenen işletim sistemlerine](#supported-operating-systems)bakın. |
| System Center Operations Manager yönetim grubu | Hayır | |

Bağımlılık aracısını aşağıdaki konumlardan indirebilirsiniz:

| Dosya | İşletim Sistemi | Sürüm | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.10.2.9060 | B7725B6B205CF8C336D9AAD87956336C816412740E9D6499BCACB6F862AE3896  |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.10.2.9060 | C6995A67A782AEC312647D4A99C3C8823F68F5FFA490FD4BB6006A2FF2941B0 |

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

VM'ler için Azure Monitor'daki özellikleri etkinleştirmek ve bu özelliklere erişmek için *Log Analytics katılımcısı* rolüne sahip olmalısınız. Performansı, sistem durumunu ve harita verilerini görüntülemek için Azure VM için *izleme okuyucu* rolüne sahip olmalısınız. VM'ler için Azure Monitor için Log Analytics çalışma alanı yapılandırılmalıdır.

Günlük Analizi çalışma alanına erişimi nasıl denetleyecekleri hakkında daha fazla bilgi için [bkz.](../../azure-monitor/platform/manage-access.md)

## <a name="how-to-enable-azure-monitor-for-vms"></a>VM'ler için Azure Monitörü nasıl etkinleştirilir?

Bu tabloda açıklanan yöntemlerden birini kullanarak VM'ler için Azure Monitörünü etkinleştirin:

| Dağıtım durumu | Yöntem | Açıklama |
|------------------|--------|-------------|
| Tek Azure VM veya sanal makine ölçeği kümesi | [VM'den etkinleştirme](vminsights-enable-single-vm.md) | Doğrudan VM veya sanal makine ölçeği kümesinden **Öngörüler'i** seçerek tek bir Azure VM'yi etkinleştirebilirsiniz. |
| Birden çok Azure VM veya sanal makine ölçek kümesi | [Azure İlkesi ile etkinleştirme](vminsights-enable-at-scale-policy.md) | Azure İlkesi ve kullanılabilir ilke tanımlarını kullanarak birden çok Azure VM'si etkinleştirebilirsiniz. |
| Birden çok Azure VM veya sanal makine ölçek kümesi | [Azure PowerShell veya Azure Kaynak Yöneticisi şablonları aracılığıyla etkinleştirme](vminsights-enable-at-scale-powershell.md) | Azure PowerShell veya Azure Kaynak Yöneticisi şablonlarını kullanarak belirli bir abonelik veya kaynak grubunda birden çok Azure VM'si veya sanal makine ölçeği kümesi ni etkinleştirebilirsiniz. |
| Karma bulut | [Karma ortam için etkinleştirme](vminsights-enable-hybrid-cloud.md) | Veri merkezinizde veya diğer bulut ortamlarınızda barındırılan Sanal Bilgisayarlara veya fiziksel bilgisayarlara dağıtabilirsiniz. |

## <a name="management-packs"></a>Yönetim paketleri

VM'ler için Azure Monitor etkinleştirildiğinde ve bir Log Analytics çalışma alanıyla yapılandırıldığında, yönetim paketi bu çalışma alanına rapor veren tüm Windows bilgisayarlarına iletilir. [Sistem Merkezi Operasyon Yöneticisi yönetim grubunuzu](../../azure-monitor/platform/om-agents.md) Log Analytics çalışma alanıyla tümleştirmişseniz, Hizmet Haritası yönetim paketi yönetim grubundan yönetim grubuna rapor bildiren Windows bilgisayarlarına dağıtılır.  

Yönetim paketi *Microsoft.IntelligencePacks.ApplicationDependencyMonitor*adlı. Klasöre `%Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs\` yazılır. Yönetim paketinin kullandığı veri `%Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources\<AutoGeneratedID>\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll`kaynağı.

## <a name="diagnostic-and-usage-data"></a>Tanılama ve kullanım verileri

Microsoft, Azure Monitor hizmetini kullanarak kullanım ve performans verilerini otomatik olarak toplar. Microsoft bu verileri hizmetin kalitesini, güvenliğini ve bütünlüğünü geliştirmek için kullanır. 

Doğru ve verimli sorun giderme özellikleri sağlamak için Harita özelliği, yazılımınızın yapılandırması hakkında veriler içerir. Veriler işletim sistemi ve sürüm, IP adresi, DNS adı ve iş istasyonu adı gibi bilgiler sağlar. Microsoft adlar, adresler veya diğer iletişim bilgileri toplamaz.

Veri toplama ve kullanım hakkında daha fazla bilgi için [Microsoft Çevrimiçi Hizmetler Gizlilik Bildirimi'ne](https://go.microsoft.com/fwlink/?LinkId=512132)bakın.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

VM'niz için izlemeyi etkinleştirdiğinize göre, izleme bilgileri VM'ler için Azure Monitor'da analiz edilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Performans izleme özelliğini nasıl kullanacağınızı öğrenmek [için VM Performansı için Azure Monitörünü Görüntüle'ye](vminsights-performance.md)bakın. Keşfedilen uygulama bağımlılıklarını görüntülemek [için, VM'ler Haritası için Azure Monitörünü Görüntüle'ye](vminsights-maps.md)bakın.

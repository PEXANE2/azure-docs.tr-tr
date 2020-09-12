---
title: Azure Izleyici aracısına genel bakış
description: Sanal makinelerin Konuk işletim sisteminden izleme verilerini toplayan Azure Izleyici aracısına (AMA) genel bakış.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/10/2020
ms.openlocfilehash: 0fc9139e9456a62bf3586fb358046e7c868b834a
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "90005233"
---
# <a name="azure-monitor-agent-overview-preview"></a>Azure Izleyici aracısına genel bakış (Önizleme)
Azure Izleyici Aracısı (AMA), sanal makinelerin Konuk işletim sisteminden izleme verilerini toplar ve Azure Izleyici 'ye gönderir. Bu makaleler, Azure Izleyici aracısına nasıl yükleneceğine ve veri toplamayı nasıl yapılandıracağınızı içeren bir genel bakış sağlar.

## <a name="relationship-to-other-agents"></a>Diğer aracılarla ilişki
Azure Izleyici Aracısı, şu anda Azure Izleyici tarafından sanal makinelerden gelen Konuk verileri toplamak için kullanılan aşağıdaki aracıların yerini alır:

- [Log Analytics Aracısı](log-analytics-agent.md) -verileri Log Analytics çalışma alanına gönderir ve VM'ler için Azure izleyici ve izleme çözümlerini destekler.
- [Tanılama uzantısı](diagnostics-extension-overview.md) -Azure Izleyici ölçümlerine (yalnızca Windows), Azure Event Hubs ve Azure Storage 'a veri gönderir.
- [Telegraf Agent](collect-custom-metrics-linux-telegraf.md) -verileri Azure Izleyici ölçümlerine gönderir (yalnızca Linux).

Azure Izleyici Aracısı, bu işlevselliği tek bir aracıda birleştirmenin yanı sıra mevcut aracılar üzerinde aşağıdaki avantajları sağlar:

- İzleme kapsamı. Farklı VM kümelerinden farklı veri kümeleri için koleksiyon merkezi olarak yapılandırın.  
- Linux çoklu giriş: Linux sanal makinelerinden birden çok çalışma alanına veri gönderme.
- Windows olay filtreleme: hangi Windows olaylarının toplanacağını filtrelemek için XPATH sorguları kullanın.
- İyileştirilmiş uzantı yönetimi: Azure Izleyici Aracısı, geçerli Log Analytics aracılarında yönetim paketlerinden ve Linux eklentilerine göre daha saydam ve denetlenebilir bir genişletilebilirlik işlemek için yeni bir yöntem kullanır.

### <a name="changes-in-data-collection"></a>Veri koleksiyonundaki değişiklikler
Mevcut aracılar için veri toplamayı tanımlamaya yönelik yöntemler birbirinden ayrı olarak birbirinden farklıdır ve her birinin Azure Izleyici aracısında ele alınan sorunları vardır.

- Log Analytics Aracısı, yapılandırmasını bir Log Analytics çalışma alanından alır. Bu, merkezi olarak yapılandırmak kolaydır, ancak farklı sanal makineler için bağımsız tanımları tanımlamaya zordur. Yalnızca bir Log Analytics çalışma alanına veri gönderebilir.

- Tanılama uzantısının her bir sanal makine için bir yapılandırması vardır. Bu, farklı sanal makineler için bağımsız tanımları tanımlamak ve merkezi olarak yönetmek zor bir işlemdir. Yalnızca Azure Izleyici ölçümleri, Azure Event Hubs veya Azure Storage 'a veri gönderebilir. Linux aracıları için, Azure Izleyici ölçümlerine veri göndermek için açık kaynak telegraf Aracısı gerekir.

Azure Izleyici Aracısı, verileri her aracıdan toplanacak şekilde yapılandırmak için [veri toplama kuralları 'nı (DCR)](data-collection-rule-overview.md) kullanır. Veri toplama kuralları, makinelerin alt kümeleri için benzersiz ve kapsamlı yapılandırmaların etkinleştirilmesinde, koleksiyonda koleksiyon ayarlarının yönetilebilirliğini etkinleştirir. Bunlar, çalışma alanından bağımsızdır ve sanal makineden bağımsızdır. Bu, bir kez tanımlanmalarını ve makineler ve ortamlar arasında yeniden kullanılmasını sağlar. Bkz. [Azure izleyici Aracısı (Önizleme) için veri toplamayı yapılandırma](data-collection-rule-azure-monitor-agent.md).



## <a name="current-limitations"></a>Geçerli sınırlamalar
Azure Izleyici aracısının genel önizlemesi sırasında aşağıdaki sınırlamalar geçerlidir:

- Azure Izleyici Aracısı, VM'ler için Azure İzleyici ve Azure Güvenlik Merkezi gibi çözümleri ve öngörüleri desteklemez. Şu anda desteklenen tek senaryo, yapılandırdığınız veri toplama kurallarını kullanarak veri topluyor. 
- Veri toplama kurallarının, hedef olarak kullanılan Log Analytics çalışma alanıyla aynı bölgede oluşturulması gerekir.
- Şu anda yalnızca Azure sanal makineleri desteklenmektedir. Şirket içi sanal makineler, sanal makine ölçek kümeleri, sunucular için yay, Azure Kubernetes hizmeti ve diğer işlem kaynağı türleri şu anda desteklenmemektedir.
- Sanal makinenin aşağıdaki HTTPS uç noktalarına erişimi olmalıdır:
  - *.ods.opinsights.azure.com
  - *. ingest.monitor.azure.com
  - *. control.monitor.azure.com


## <a name="coexistence-with-other-agents"></a>Diğer aracılarla birlikte bulunma
Azure Izleyici Aracısı mevcut aracılarla birlikte çalışabilir, böylece değerlendirme veya geçiş sırasında mevcut işlevlerini kullanmaya devam edebilirsiniz. Bu özellikle, var olan çözümleri destekleme genel önizlemesindeki sınırlamalar nedeniyle önemlidir. Bu, sorgu sonuçlarını çarpıtabileceğinden ve verilerin alımı ve saklanması için ek ücretler elde edilmesine neden olduğundan yinelenen verileri toplamayla ilgili dikkatli olmanız gerekir.

Örneğin VM'ler için Azure İzleyici, performans verilerini bir Log Analytics çalışma alanına göndermek için Log Analytics aracısını kullanır. Ayrıca, çalışma alanını aracılardan Windows olaylarını ve Syslog olaylarını toplayacak şekilde yapılandırmış olabilirsiniz. Azure Izleyici aracısını yükler ve aynı olaylar ve performans verileri için bir veri toplama kuralı oluşturursanız, bu yinelenen verilerin oluşmasına neden olur.


## <a name="costs"></a>Maliyetler
Azure Izleyici Aracısı için maliyet yoktur, ancak alınan veriler için ücret ödemeniz gerekebilir. Log Analytics veri toplama ve bekletme ve müşteri ölçümleri hakkında ayrıntılı bilgi için bkz. [Azure izleyici fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/) .

## <a name="data-sources-and-destinations"></a>Veri kaynakları ve hedefler
Aşağıdaki tabloda, veri toplama kurallarını kullanarak şu anda Azure Izleyici aracısında toplayabileceğiniz veri türleri ve bu verileri gönderebilirsiniz. Diğer veri türlerini toplamak üzere Azure izleyici aracısını kullanan Öngörüler, çözümler ve diğer çözümlerin listesi için bkz. [Azure izleyici tarafından Izlenen nedir?](../monitor-reference.md) .


Azure Izleyici Aracısı verileri Azure Izleyici ölçümleri 'ne veya Azure Izleyici günlüklerini destekleyen bir Log Analytics çalışma alanına gönderir.

| Veri Kaynağı | Hedefler | Description |
|:---|:---|:---|
| Performans        | Azure Izleyici ölçümleri<br>Log Analytics çalışma alanı | Sayısal değerler, işletim sistemi ve iş yüklerinin farklı yönlerinin performansını ölçerek. |
| Windows olay günlükleri | Log Analytics çalışma alanı | Windows olay günlüğü sistemine gönderilen bilgiler. |
| Syslog             | Log Analytics çalışma alanı | Linux olay günlüğü sistemine gönderilen bilgiler. |


## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Aşağıdaki işletim sistemleri şu anda Azure Izleyici Aracısı tarafından desteklenmektedir.

### <a name="windows"></a>Windows 
  - Windows Server 2019
  - Windows Server 2016
  - Windows Server 2012
  - Windows Server 2012 R2

### <a name="linux"></a>Linux
  - CentOS 6<sup>1</sup>, 7
  - Borçlu 9, 10
  - Oracle Linux 6<sup>1</sup>, 7
  - RHEL 6<sup>1</sup>, 7
  - SLES 11, 12, 15
  - Ubuntu 14,04 LTS, 16,04 LTS, 18,04 LTS

> [!IMPORTANT]
> <sup>1</sup> Bu dağıtımların Syslog verileri gönderebilmesi için, aracı yüklendikten sonra rsyslog hizmetini bir kez yeniden başlatmanız gerekir.


## <a name="security"></a>Güvenlik
Azure Izleyici Aracısı herhangi bir anahtar gerektirmez, bunun yerine [sistem tarafından atanan yönetilen kimlik](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#system-assigned-managed-identity)gerektirir. Aracıyı dağıtabilmeniz için, her bir sanal makinede bir sistem tarafından atanmış yönetilen kimliğinizin etkinleştirilmiş olması gerekir.


## <a name="install-the-azure-monitor-agent"></a>Azure Izleyici aracısını yükler
Azure Izleyici Aracısı, aşağıdaki tablodaki ayrıntılarla bir [Azure VM Uzantısı](../../virtual-machines/extensions/overview.md) olarak uygulanır. 

| Özellik | Windows | Linux |
|:---|:---|:---|
| Publisher | Microsoft. Azure. Monitor  | Microsoft. Azure. Monitor |
| Tür      | AzureMonitorWindowsAgent | AzureMonitorLinuxAgent  |
| TypeHandlerVersion  | 1.0 | 1,5 |

PowerShell veya CLı kullanarak aşağıdakini içeren sanal makine aracılarını yüklemek için yöntemlerden birini kullanarak Azure Izleyici aracısını yükler. Alternatif olarak, [Azure izleyici Aracısı (Önizleme) için veri toplamayı yapılandırma](data-collection-rule-azure-monitor-agent.md#create-using-the-azure-portal)bölümünde açıklanan yordamla portalı kullanarak Azure aboneliğinizdeki sanal makinelerde aracı yükleyebilir ve veri toplamayı yapılandırabilirsiniz.

### <a name="windows"></a>Windows

# <a name="cli"></a>[CLI](#tab/CLI1)

```azurecli
az vm extension set --name AzureMonitorWindowsAgent --publisher Microsoft.Azure.Monitor --ids {resource ID of the VM}

```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell1)

```powershell
Set-AzVMExtension -Name AMAWindows -ExtensionType AzureMonitorWindowsAgent -Publisher Microsoft.Azure.Monitor -ResourceGroupName {Resource Group Name} -VMName {VM name} -Location eastus
```
---


### <a name="linux"></a>Linux

# <a name="cli"></a>[CLI](#tab/CLI2)

```azurecli
az vm extension set --name AzureMonitorLinuxAgent --publisher Microsoft.Azure.Monitor --ids {resource ID of the VM}

```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell2)

```powershell
Set-AzVMExtension -Name AMALinux -ExtensionType AzureMonitorLinuxAgent -Publisher Microsoft.Azure.Monitor -ResourceGroupName {Resource Group Name} -VMName {VM name} -Location eastus
```
---

## <a name="next-steps"></a>Sonraki adımlar

- Aracıdan veri toplamak ve Azure Izleyici 'ye göndermek için [bir veri toplama kuralı oluşturun](data-collection-rule-azure-monitor-agent.md) .

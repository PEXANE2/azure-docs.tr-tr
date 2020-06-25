---
title: Kod değişikliği olmadan uygulamalarınızı izleyin-Azure Izleyici için otomatik izleme Application Insights | Microsoft Docs
description: Azure izleyici Application Insights-kodsuz kullanacaksınız uygulama performansı yönetimi için otomatik izleme 'ye Genel Bakış
ms.topic: conceptual
author: MS-jgol
ms.author: jgol
ms.date: 05/31/2020
ms.reviewer: mbullwin
ms.openlocfilehash: 87342dcd316b0364522baa01e632b704665c998e
ms.sourcegitcommit: f98ab5af0fa17a9bba575286c588af36ff075615
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85363771"
---
# <a name="what-is-auto-instrumentation-or-codeless-attach---azure-monitor-application-insights"></a>Otomatik izleme veya kodsuz kullanacaksınız Attach-Azure izleyici Application Insights nedir?

Otomatik izleme veya kodsuz kullanacaksınız iliştirme, kodunuzu değiştirmeden Application Insights ile uygulama izlemeyi etkinleştirmenizi sağlar.  

Application Insights çeşitli kaynak sağlayıcılarıyla tümleşiktir ve farklı ortamlarda çalışmaktadır. Temelde, tek yapmanız gereken, ve bazı durumlarda, Telemetriyi kutudan çıkar. Zaman içinde, Application Insights kaynağınızda ölçümleri, verileri ve bağımlılıkları görürsünüz. Bu, olası sorunların kaynağını yapmadan önce bu kaynakları ortaya çıkarabilir ve bu durum, uçtan uca işlem görünümüyle ilgili asıl nedeni analiz etmenize olanak tanır.

## <a name="supported-environments-languages-and-resource-providers"></a>Desteklenen ortamlar, diller ve kaynak sağlayıcıları

Daha fazla ve daha fazla tümleştirme eklediğimiz için, otomatik izleme özelliği matrisi karmaşık hale gelir. Aşağıdaki tabloda, çeşitli kaynak sağlayıcılarının, dillerin ve ortamların destek alanının geçerli durumu gösterilmektedir.

|Ortam/kaynak sağlayıcısı | .NET            | .NET Core       | Java            | Node.js         |
|------------------------------|-----------------|-----------------|-----------------|-----------------|
|Windows üzerinde Azure App Service  | GA, OnBD *       | GA, kabul etme      | Özel Önizleme | Özel Önizleme |
|Linux üzerinde Azure App Service    | Yok             | Desteklenmiyor   | Genel Önizleme  | Genel Önizleme  |
|AKS üzerinde Azure App Service      | Yok             | Tasarımda       | Tasarımda       | Tasarımda       |
|Azure Işlevleri-temel       | GA, OnBD *       | GA, OnBD *       | GA, OnBD *       | GA, OnBD *       |
|Azure Işlevleri-bağımlılıklar| Desteklenmiyor   | Desteklenmiyor   | Genel Önizleme  | Desteklenmiyor   |
|Azure Kubernetes Service      | Yok             | Tasarımda       | Aracı üzerinden   | Tasarımda       |
|Azure VM 'Leri Windows             | Genel Önizleme  | Desteklenmiyor   | Desteklenmiyor   | Desteklenmiyor   |
|Şirket Içi VM 'Ler Windows       | GA, kabul etme      | Desteklenmiyor   | Aracı üzerinden   | Desteklenmiyor   |
|Tek başına aracı-tüm env.   | Desteklenmiyor   | Desteklenmiyor   | Genel Önizleme  | Desteklenmiyor   |

* OnBD varsayılan olarak açık için kısaysa, uygulamanızı desteklenen ortamlarda dağıttıktan sonra Application Insights otomatik olarak etkinleştirilir. 

## <a name="azure-app-service"></a>Azure App Service

### <a name="windows"></a>Windows

[Azure App Service üzerinde uygulama izleme](https://docs.microsoft.com/azure/azure-monitor/app/azure-web-apps?tabs=net) .NET uygulaması için kullanılabilir ve varsayılan olarak etkindir, .NET Core tek tıklamayla etkinleştirilebilir ve Java ve Node.js özel önizlemededir.

### <a name="linux"></a>Linux 

Java ve Node.js uygulamalarının App Service izleme genel önizlemede ve tüm bölgelerde kullanılabilen Azure portal etkinleştirilebilir.

## <a name="azure-functions"></a>Azure İşlevleri

Azure Işlevleri için temel izleme, günlük, performans, hata verileri ve HTTP isteklerini toplamak üzere varsayılan olarak etkinleştirilmiştir. Java uygulamaları için, dağıtılmış izleme ile daha zengin izlemeyi etkinleştirebilir ve uçtan uca işlem ayrıntılarını alabilirsiniz. Java için bu işlev genel önizlemeye sunuldu ve [Azure Portal ' de etkinleştirebilirsiniz](https://docs.microsoft.com/azure/azure-monitor/app/monitor-functions).

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Azure Kubernetes hizmetinin ortak olmayan araçları, [tek başına aracı](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent)aracılığıyla Java uygulamalarında kullanılabilir. 

## <a name="azure-windows-vms-and-virtual-machine-scale-set"></a>Azure Windows VM 'Leri ve sanal makine ölçek kümesi

[Azure VM 'leri ve sanal makine ölçek kümesi Için otomatik izleme](https://docs.microsoft.com/azure/azure-monitor/app/azure-vm-vmss-apps) , .NET uygulamaları için kullanılabilir 

## <a name="on-premises-servers"></a>Şirket içi sunucular
.NET uygulamaları ve [Java uygulamaları](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent)için şirket [içi Windows sunucularınız](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) için izlemeyi kolayca etkinleştirebilirsiniz.

## <a name="other-environments"></a>Diğer ortamlar
Çok yönlü Java tek başına Aracısı her türlü ortamda çalışarak, kodunuzu işaretlememeye gerek yoktur. Application Insights etkinleştirmek ve Java aracısının harika özellikleri hakkında bilgi edinmek için [kılavuzu izleyin](https://docs.microsoft.com/azure/azure-monitor/app/java-in-process-agent) . Aracı genel önizlemede ve tüm bölgelerde kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Application Insights genel bakış](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)
* [Uygulama eşlemesi](./../../azure-monitor/app/app-map.md)
* [Uçtan uca performans izleme](./../../azure-monitor/learn/tutorial-performance.md)

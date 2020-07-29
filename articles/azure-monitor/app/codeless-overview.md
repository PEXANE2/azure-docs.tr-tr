---
title: Kod değişikliği olmadan uygulamalarınızı izleyin-Azure Izleyici için otomatik izleme Application Insights | Microsoft Docs
description: Azure izleyici Application Insights-kodsuz kullanacaksınız uygulama performansı yönetimi için otomatik izleme 'ye Genel Bakış
ms.topic: conceptual
author: MS-jgol
ms.author: jgol
ms.date: 05/31/2020
ms.reviewer: mbullwin
ms.openlocfilehash: 234f772b4a77b780f0b0c1faf307c93847e5caf2
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87310727"
---
# <a name="what-is-auto-instrumentation-or-codeless-attach---azure-monitor-application-insights"></a>Otomatik izleme veya kodsuz kullanacaksınız Attach-Azure izleyici Application Insights nedir?

Otomatik izleme veya kodsuz kullanacaksınız iliştirme, kodunuzu değiştirmeden Application Insights ile uygulama izlemeyi etkinleştirmenizi sağlar.  

Application Insights çeşitli kaynak sağlayıcılarıyla tümleşiktir ve farklı ortamlarda çalışmaktadır. Temelde, tek yapmanız gereken, ve bazı durumlarda, Telemetriyi kutudan çıkar. Zaman içinde, Application Insights kaynağınızda ölçümleri, verileri ve bağımlılıkları görürsünüz. Bu, olası sorunların kaynağını yapmadan önce bu kaynakları ortaya çıkarabilir ve bu durum, uçtan uca işlem görünümüyle ilgili asıl nedeni analiz etmenize olanak tanır.

## <a name="supported-environments-languages-and-resource-providers"></a>Desteklenen ortamlar, diller ve kaynak sağlayıcıları

Daha fazla ve daha fazla tümleştirme eklediğimiz için, otomatik izleme özelliği matrisi karmaşık hale gelir. Aşağıdaki tabloda, çeşitli kaynak sağlayıcılarının, dillerin ve ortamların destek alanının geçerli durumu gösterilmektedir.

|Ortam/kaynak sağlayıcısı | .NET            | .NET Core       | Java            | Node.js         |
|------------------------------|-----------------|-----------------|-----------------|-----------------|
|Windows üzerinde Azure App Service  | GA, OnBD *       | GA, kabul etme      | Özel Önizleme | Özel Önizleme |
|Linux üzerinde Azure App Service    | Yok             | Desteklenmez   | Genel Önizleme  | Genel Önizleme  |
|AKS üzerinde Azure App Service      | Yok             | Tasarımda       | Tasarımda       | Tasarımda       |
|Azure Işlevleri-temel       | GA, OnBD *       | GA, OnBD *       | GA, OnBD *       | GA, OnBD *       |
|Azure Işlevleri-bağımlılıklar| Desteklenmez   | Desteklenmez   | Genel Önizleme  | Desteklenmez   |
|Azure Kubernetes Service      | Yok             | Tasarımda       | Aracı üzerinden   | Tasarımda       |
|Azure VM 'Leri Windows             | Genel Önizleme  | Desteklenmez   | Desteklenmez   | Desteklenmez   |
|Şirket Içi VM 'Ler Windows       | GA, kabul etme      | Desteklenmez   | Aracı üzerinden   | Desteklenmez   |
|Tek başına aracı-tüm env.   | Desteklenmez   | Desteklenmez   | Genel Önizleme  | Desteklenmez   |

* OnBD varsayılan olarak açık için kısaysa, uygulamanızı desteklenen ortamlarda dağıttıktan sonra Application Insights otomatik olarak etkinleştirilir. 

## <a name="azure-app-service"></a>Azure App Service

### <a name="windows"></a>Windows

[Azure App Service üzerinde uygulama izleme](./azure-web-apps.md?tabs=net) .NET uygulaması için kullanılabilir ve varsayılan olarak etkindir, .NET Core tek tıklamayla etkinleştirilebilir ve Java ve Node.js özel önizlemededir.

### <a name="linux"></a>Linux 

Java ve Node.js uygulamalarının App Service izleme genel önizlemede ve tüm bölgelerde kullanılabilen Azure portal etkinleştirilebilir.

## <a name="azure-functions"></a>Azure İşlevleri

Azure Işlevleri için temel izleme, günlük, performans, hata verileri ve HTTP isteklerini toplamak üzere varsayılan olarak etkinleştirilmiştir. Java uygulamaları için, dağıtılmış izleme ile daha zengin izlemeyi etkinleştirebilir ve uçtan uca işlem ayrıntılarını alabilirsiniz. Java için bu işlev genel önizlemeye sunuldu ve [Azure Portal ' de etkinleştirebilirsiniz](./monitor-functions.md).

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Azure Kubernetes hizmetinin ortak olmayan araçları, [tek başına aracı](./java-in-process-agent.md)aracılığıyla Java uygulamalarında kullanılabilir. 

## <a name="azure-windows-vms-and-virtual-machine-scale-set"></a>Azure Windows VM 'Leri ve sanal makine ölçek kümesi

[Azure VM 'leri ve sanal makine ölçek kümesi Için otomatik izleme](./azure-vm-vmss-apps.md) , .NET uygulamaları için kullanılabilir 

## <a name="on-premises-servers"></a>Şirket içi sunucular
.NET uygulamaları ve [Java uygulamaları](./java-in-process-agent.md)için şirket [içi Windows sunucularınız](./status-monitor-v2-overview.md) için izlemeyi kolayca etkinleştirebilirsiniz.

## <a name="other-environments"></a>Diğer ortamlar
Çok yönlü Java tek başına Aracısı her türlü ortamda çalışarak, kodunuzu işaretlememeye gerek yoktur. Application Insights etkinleştirmek ve Java aracısının harika özellikleri hakkında bilgi edinmek için [kılavuzu izleyin](./java-in-process-agent.md) . Aracı genel önizlemede ve tüm bölgelerde kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar

* [Application Insights genel bakış](./app-insights-overview.md)
* [Uygulama eşlemesi](./app-map.md)
* [Uçtan uca performans izleme](../learn/tutorial-performance.md)


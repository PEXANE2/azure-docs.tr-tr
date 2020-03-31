---
title: VM'ler için Azure Monitör'de izlemeyi devre dışı
description: Bu makalede, SANAL Makineler için Azure Monitor'da sanal makinelerinizi izlemeyi nasıl durduranAçıklanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: 80473aa494b8fbcea5e43870b7717cd3472dd7d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480530"
---
# <a name="disable-monitoring-of-your-vms-in-azure-monitor-for-vms"></a>VM'ler için Azure Monitör'de VM'lerinizin izlenmesini devre dışı

Sanal makinelerinizin (VM) izlenmesini etkinleştirdikten sonra, daha sonra VM'ler için Azure Monitor'da izlemeyi devre dışı bırakmayı seçebilirsiniz. Bu makalede, bir veya daha fazla VM için izleme devre dışı nasıl gösterilmektedir.  

Şu anda, VM'ler için Azure Monitor, VM izlemenin seçici olarak devre dışı bırakılmasını desteklemez. Log Analytics çalışma alanınız, VM'ler ve diğer çözümler için Azure Monitor'u destekleyebilir. Ayrıca diğer izleme verileri toplayabilir. Log Analytics çalışma alanınız bu hizmetleri sağlıyorsa, başlamadan önce izlemeyi devre dışı bırakmanın etkisini ve yöntemlerini anlamanız gerekir.

Sanal Ayarlar için Azure Monitör, deneyimini sunmak için aşağıdaki bileşenlere güvenir:

* VM'lerden ve diğer kaynaklardan gelen izleme verilerini depolayan bir Log Analytics çalışma alanı.
* Çalışma alanında yapılandırılan performans sayaçları koleksiyonu. Koleksiyon, çalışma alanına bağlı tüm VM'lerde izleme yapılandırmasını güncelleştirir.
* `VMInsights`, çalışma alanında yapılandırılan bir izleme çözümüdür. Bu çözüm, çalışma alanına bağlı tüm VM'lerde izleme yapılandırmasını güncelleştirir.
* `MicrosoftMonitoringAgent`ve `DependencyAgent`, Azure VM uzantıları vardır. Bu uzantılar veri toplar ve çalışma alanına gönderir.

VM'lerinizin izlenmesini devre dışı kizlemeye hazırlanırken, şu hususları göz önünde bulundurun:

* Tek bir VM ile değerlendirildiyseniz ve önceden seçilmiş varsayılan Log Analytics çalışma alanını kullandıysanız, Bağımlılık aracısını VM'den katarak ve Log Analytics aracısını bu çalışma alanından keserek izlemeyi devre dışı kullanabilirsiniz. VM'yi başka amaçlarla kullanmayı ve daha sonra farklı bir çalışma alanına yeniden bağlamaya karar vermek istiyorsanız, bu yaklaşım uygundur.
* Diğer izleme çözümlerini ve diğer kaynaklardan veri toplamayı destekleyen önceden varolan bir Log Analytics çalışma alanı seçtiyseniz, çözüm bileşenlerini çalışma alanınızı kesintiye uğratmadan veya etkilemeden çalışma alanından kaldırabilirsiniz.  

>[!NOTE]
> Çözüm bileşenlerini çalışma alanınızdan kaldırdıktan sonra, Azure Sanal M'leriniziçin performans ve harita verilerini görmeye devam edebilirsiniz. Veriler sonunda **Performans** ve **Harita** görünümlerinde görünmeyi durdurur. **İleride** izlemeyi yeniden etkinleştirebilmeniz için Seçili Azure VM'den Etkinleştir seçeneği kullanılabilir.  

## <a name="remove-azure-monitor-for-vms-completely"></a>VM'ler için Azure Monitörünü tamamen kaldırma

Log Analytics çalışma alanına hala ihtiyacınız varsa, VM'ler için Azure Monitörünü tamamen kaldırmak için aşağıdaki adımları izleyin. `VMInsights` Çözümü çalışma alanından kaldırırsınız.  

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Azure portalında **Tüm hizmetler'i**seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda, liste girişinize göre önerileri filtreler. **Log Analytics**’i seçin.
3. Günlük Analitiği çalışma alanları listenizde, VM'ler için Azure Monitörünü etkinleştirdiğinizde seçtiğiniz çalışma alanını seçin.
4. Solda **Çözümler'i**seçin.  
5. Çözümler listesinde **VMInsights(çalışma alanı adı)** seçeneğini belirleyin. Çözüm için **Genel Bakış** sayfasında **Sil'i**seçin. Onaylamak istendiğinde **Evet'i**seçin.

## <a name="disable-monitoring-and-keep-the-workspace"></a>İzlemeyi devre dışı bırakıp çalışma alanını koruyun  

Log Analytics çalışma alanınızın, Azure Monitor'u VM'ler için değerlendirmek için kullandığınız VM izlemeyi devre dışı katmak için aşağıdaki adımları izleyerek diğer kaynaklardan gelen izlemeyi desteklemesi gerekiyorsa. Azure VM'ler için bağımlılık aracısı VM uzantısını ve Windows veya Linux için Log Analytics aracısı VM uzantısını doğrudan VM'den kaldırırsınız. 

>[!NOTE]
>Aşağıdakiler varsa Log Analytics aracısını kaldırmayın 
>
> * Azure Otomasyonu, süreçleri düzenlemek veya yapılandırma veya güncelleştirmeleri yönetmek için VM'yi yönetir. 
> * Azure Güvenlik Merkezi, güvenlik ve tehdit algılama için VM'yi yönetir. 
>
> Log Analytics aracısını kaldırırsanız, bu hizmetlerin ve çözümlerin VM'nizi proaktif olarak yönetmesini engellersiniz. 

1. [Azure portalında](https://portal.azure.com)oturum açın. 
2. Azure portalında **Sanal Makineler'i**seçin. 
3. Listeden bir VM seçin. 
4. Solda, **Uzantılar'ı**seçin. **Uzantılar** sayfasında **Bağımlılık Temsilcisi'ni**seçin.
5. Uzantı özellikleri sayfasında **Kaldır'ı**seçin.
6. **Uzantılar** sayfasında **MicrosoftMonitoringAgent'ı**seçin. Uzantı özellikleri sayfasında **Kaldır'ı**seçin.  

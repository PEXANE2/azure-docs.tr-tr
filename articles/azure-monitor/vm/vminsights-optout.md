---
title: VM Öngörüler 'de izlemeyi devre dışı bırak
description: Bu makalede, sanal makinelerinizi VM öngörülerine izlemenin nasıl durdurulacağı açıklanır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/12/2020
ms.openlocfilehash: 7eca08abf1ef3bed1aa7fdd806853b94d5615854
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101717069"
---
# <a name="disable-monitoring-of-your-vms-in-vm-insights"></a>VM öngörülerinin sanal makinelerini izlemeyi devre dışı bırakın

Sanal makinelerinizin (VM 'Ler) izlenmesini etkinleştirdikten sonra, daha sonra VM öngörülerine izlemeyi devre dışı bırakmayı tercih edebilirsiniz. Bu makalede bir veya daha fazla VM için izlemenin nasıl devre dışı bırakılacağı gösterilmektedir.  

VM öngörüleri Şu anda VM izlemenin seçmeli devre dışı bırakılmasını desteklemez. Log Analytics çalışma alanınız, VM öngörülerini ve diğer çözümleri destekleyebilir. Diğer izleme verilerini de toplayabilir. Log Analytics çalışma alanınız bu hizmetleri sağlıyorsa, başlamadan önce izlemeyi devre dışı bırakmanın etkisini ve yöntemlerini anlamanız gerekir.

VM öngörüleri, deneyimini sunmak için aşağıdaki bileşenleri kullanır:

* VM 'lerden ve diğer kaynaklardan izleme verilerini depolayan Log Analytics çalışma alanı.
* Çalışma alanında yapılandırılan performans sayaçları koleksiyonu. Koleksiyon, çalışma alanına bağlı tüm VM 'lerde izleme yapılandırmasını güncelleştirir.
* `VMInsights`, çalışma alanında yapılandırılmış bir izleme çözümüdür. Bu çözüm, çalışma alanına bağlı tüm VM 'lerde izleme yapılandırmasını güncelleştirir.
* `MicrosoftMonitoringAgent``DependencyAgent`Azure VM uzantıları olan ve. Bu uzantılar, çalışma alanına veri toplar ve gönderir.

VM 'lerinizi izlemeyi devre dışı bırakmayı hazırlarken şu noktaları göz önünde bulundurun:

* Tek bir VM ile değerlendirilemez ve önceden seçilmiş varsayılan Log Analytics çalışma alanını kullandıysanız, bağımlılık aracısını VM 'den kaldırıp ve Log Analytics aracısının bağlantısını bu çalışma alanından kaldırarak izlemeyi devre dışı bırakabilirsiniz. Bu yaklaşım, VM 'yi başka amaçlar için kullanmayı ve daha sonra farklı bir çalışma alanına yeniden bağlamayı tercih ediyorsanız uygundur.
* Diğer kaynaklardan diğer izleme çözümlerini ve veri toplamayı destekleyen önceden var olan bir Log Analytics çalışma alanını seçtiyseniz, çalışma alanınızı kesintiye uğramadan veya etkilemeden çözüm bileşenlerini çalışma alanından kaldırabilirsiniz.  

>[!NOTE]
> Çalışma alanınızdan çözüm bileşenlerini kaldırdıktan sonra, Azure VM 'leriniz için performans ve eşleme verilerini görmeye devam edebilirsiniz. Veriler sonunda **performans** ve **harita** görünümlerinde görünür. Daha sonra izlemeyi yeniden etkinleştirebilmeniz için seçili Azure VM 'den **Etkinleştir** seçeneği kullanılabilir.  

## <a name="remove-vm-insights-completely"></a>VM öngörülerini tamamen kaldırma

Hala Log Analytics çalışma alanına ihtiyacınız varsa, VM öngörülerini tamamen kaldırmak için aşağıdaki adımları izleyin. `VMInsights`Çözümü çalışma alanından kaldıracaksınız.  

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Azure portal, **tüm hizmetler**' i seçin. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, giriş listenize göre öneriler uygular. **Log Analytics**’i seçin.
3. Log Analytics çalışma alanları listenizde, VM öngörülerini etkinleştirdiğinizde seçtiğiniz çalışma alanını seçin.
4. Sol tarafta **çözümler**' i seçin.  
5. Çözümler listesinde, **Vminsıghts (çalışma alanı adı)** öğesini seçin. Çözümün **genel bakış** sayfasında **Sil**' i seçin. Onaylamanız istendiğinde **Evet**' i seçin.

## <a name="disable-monitoring-and-keep-the-workspace"></a>İzlemeyi devre dışı bırak ve çalışma alanını koru  

Log Analytics çalışma alanınızın diğer kaynaklardan izlemeyi desteklemesi gerekiyorsa, VM öngörülerini değerlendirmek için kullandığınız sanal makinede izlemeyi devre dışı bırakmak için bu adımları takip edin. Azure VM 'Leri için, bağımlılık Aracısı VM uzantısını ve Windows veya Linux için Log Analytics Aracısı VM uzantısını doğrudan sanal makineden kaldıracaksınız. 

>[!NOTE]
>Şu durumlarda Log Analytics aracısını kaldırmayın: 
>
> * Azure Otomasyonu, işlem düzenlemek veya yapılandırma ya da güncelleştirmeleri yönetmek için VM 'yi yönetir. 
> * Azure Güvenlik Merkezi, güvenlik ve tehdit algılama için VM 'yi yönetir. 
>
> Log Analytics aracısını kaldırırsanız, bu hizmetlerin ve çözümlerin sanal makinenizin yönetimini proaktif olarak yönetmesini önlemektir. 

1. [Azure portalında](https://portal.azure.com) oturum açın. 
2. Azure portal **sanal makineler**' i seçin. 
3. Listeden bir VM seçin. 
4. Sol tarafta, **Uzantılar**' ı seçin. **Uzantılar** sayfasında **DependencyAgent**' yi seçin.
5. Uzantı özellikleri sayfasında **Kaldır**' ı seçin.
6. **Uzantılar** sayfasında, **microsoftmonitoringagent**' ı seçin. Uzantı özellikleri sayfasında **Kaldır**' ı seçin.  

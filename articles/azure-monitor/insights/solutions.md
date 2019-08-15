---
title: Azure Izleyici 'de çözümleri izleme | Microsoft Docs
description: Azure Izleyici 'de izleme çözümleri, belirli bir sorun alanı etrafında ölçüm sağlayan bir mantık, görselleştirme ve veri alma kuralları koleksiyonudur.  Bu makale, izleme çözümlerini yükleme ve kullanma hakkında bilgi sağlar.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: f029dd6d-58ae-42c5-ad27-e6cc92352b3b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 08/13/2019
ms.author: bwren
ms.openlocfilehash: 6932f9f302bf1816842d78a662f01ebaafed8686
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68989192"
---
# <a name="monitoring-solutions-in-azure-monitor"></a>Azure Izleyici 'de çözümleri izleme
İzleme çözümleri, belirli bir uygulama veya hizmetin işlemine ek Öngörüler sağlamak için Azure 'daki hizmetlerden yararlanır. Bu makalede, Azure 'daki çözümleri izleme hakkında kısa bir genel bakış ve bunları kullanma ve yükleme hakkındaki ayrıntılar sunulmaktadır.

> [!NOTE]
> İzleme çözümleri daha önce yönetim çözümleri olarak adlandırılmıştı.

İzleme çözümleri genellikle günlük verilerini toplar ve toplanan verileri çözümlemek için sorgular ve görünümler sağlar. Ayrıca uygulama veya hizmetin ilgili eylemleri gerçekleştirmek için Azure Otomasyonu gibi diğer hizmetlerin yararlanarak.

Azure Izleyici 'ye, kullandığınız tüm uygulama ve hizmetler için izleme çözümleri ekleyebilirsiniz. Bunlar genellikle, kullanım ücretleri çağırabilirsiniz maliyet ancak toplama veri yok kullanılabilir. Microsoft tarafından sağlanan çözümleri yanı sıra, iş ortaklarımız ve müşterilerimiz için [yönetim çözümleri oluşturma](solutions-creating.md) kendi ortamında kullanılabilir veya topluluk aracılığıyla kullanıma sunulan için.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="use-monitoring-solutions"></a>İzleme çözümlerini kullanma
Çalışma alanında yüklü her bir çözümün kutucuğunu görüntülemek için Azure Izleyici 'de **genel bakış** sayfasını açın. 

1. Azure portalında oturum açın.
1. Açık **tüm hizmetleri** bulun **İzleyici**.
1. Altında **Insights** menüsünde **daha fazla**.
1. Açılır liste kutusu ekranın en üstünde çalışma alanı ya da kutucuklar için kullanılan zaman aralığını değiştirmek için kullanın.
1. Toplanan verileri bir çözümün daha ayrıntılı analiz içeren onun görünümünü açmak kutucuğa tıklayın.

![Genel Bakış](media/solutions/overview.png)

İzleme çözümleri birden çok Azure kaynağı türü içerebilir ve bir çözüme dahil olan tüm kaynakları diğer kaynaklar gibi görüntüleyebilirsiniz. Örneğin, çözüme dahil edilen herhangi bir günlük sorgusu, [sorgu Gezgini](../log-query/get-started-portal.md#load-queries) 'Ndeki **çözüm sorguları** altında listelenmiştir ve [günlük sorgularıyla](../log-query/log-query-overview.md)geçici analiz gerçekleştirirken bu sorguları kullanabilirsiniz.

## <a name="list-installed-monitoring-solutions"></a>Yüklü izleme çözümlerini Listele 
Aboneliğinizde yüklü izleme çözümlerini listelemek için aşağıdaki yordamı kullanın.

1. Azure portalında oturum açın.
1. Açık **tüm hizmetleri** bulun **çözümleri**.
4. Tüm çalışma alanlarında yüklü çözümleri listelenmiştir. Çözümün adının ardından yüklendiği çalışma alanının adı gelir.
1. Açılır liste kutusu ekranın en üstünde, abonelik veya kaynak grubuna göre filtrele için kullanın.


![Tüm çözümler listesi](media/solutions/list-solutions-all.png)

Kendi Özet sayfasını açmak için bir çözümün adına tıklayın. Bu sayfa, Çözümdeki tüm görünümleri görüntüler ve kendisi ve çalışma alanı çözümü farklı seçenekler sunar. Listesi çözümlere yordamlardan birini kullanarak bir çözüm için Özet sayfasında görüntüleyin ve ardından çözümün adına tıklayın.

![Çözüm özellikleri](media/solutions/solution-properties.png)



## <a name="install-a-monitoring-solution"></a>İzleme çözümü yüklemesi
Microsoft ve iş ortaklarından izleme çözümlerini [Azure Marketi](https://azuremarketplace.microsoft.com)' nden edinebilirsiniz. Kullanılabilir çözümler arayabilir ve bunları aşağıdaki yordamı kullanarak yükleyin. Bir çözüm yüklediğinizde seçmelisiniz bir [Log Analytics çalışma alanı](../platform/manage-access.md) çözüm yükleneceği ve verilerinin nerede toplanacağını.

1. Gelen [aboneliğiniz için çözümlerin listesini](#list-installed-monitoring-solutions), tıklayın **Ekle**.
1. Bir çözüme gözatıp arayın. Ayrıca, [Bu arama bağlantısından](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/management-tools?page=1&subcategories=management-solutions)çözümlere da gidebilirsiniz.
1. İstediğiniz izleme çözümünü bulun ve açıklamasını okuyun.
1. Tıklayın **Oluştur** yükleme işlemini başlatmak için.
1. Yükleme işlemi başladığında Log Analytics çalışma alanını belirtmeniz ve çözüm için gerekli tüm yapılandırmaları sağlamanız istenir.

![Çözüm yükleme](media/solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>Topluluktan bir çözüm yükleyin
Topluluk üyeleri, yönetim çözümleri Azure hızlı başlangıç şablonları gönderebilirsiniz. Bu çözümler doğrudan yükleyebilir veya bunları daha sonra yükleme için şablonları indirin.

1. Açıklanan işlemi izleyin [Log Analytics çalışma alanını ve Otomasyon hesabı](#log-analytics-workspace-and-automation-account) bir çalışma alanı ve hesabı bağlamak için.
2. Git [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates/). 
3. İlginizi çeken bir çözüm arayın.
4. Çözüm sonuçları ayrıntılarını görüntülemek için seçin.
5. Tıklayın **azure'a Dağıt** düğmesi.
6. Çözümdeki herhangi bir parametre için bir kaynak grubu ve konum değerlere ek olarak gibi bilgiler sağlamanız istenir.
7. Tıklayın **satın alma** çözümü yüklemek için.


## <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics çalışma alanı ve Otomasyon hesabı
Tüm izleme çözümleri, çözüm tarafından toplanan verileri depolamak ve günlük aramalarını ve görünümlerini barındırmak için bir [Log Analytics çalışma alanı](../platform/manage-access.md) gerektirir. Ayrıca bazı çözümler gerektiren bir [Otomasyon hesabı](../../automation/automation-security-overview.md#automation-account-overview) runbook'ları ve ilgili kaynakları içerecek şekilde. Çalışma alanı ve hesabı aşağıdaki gereksinimleri karşılaması gerekir.

* Her bir çözümün yüklenmesi yalnızca bir Log Analytics çalışma alanı ve bir Otomasyon hesabı kullanabilirsiniz. Çözüm birden çok çalışma alanı içinde ayrı olarak yükleyebilirsiniz.
* Bir çözüm bir Otomasyon hesabı gerektiriyorsa, ardından Log Analytics çalışma alanını ve Otomasyon hesabı birbirine bağlı olmalıdır. Bir Log Analytics çalışma alanı yalnızca bir Otomasyon hesabına bağlı ve bir Otomasyon hesabı yalnızca bir Log Analytics çalışma alanına bağlı.
* Bağlanacak, Log Analytics çalışma alanını ve Otomasyon hesabı aynı kaynak grubunda ve bölgede olması gerekir. Özel durum, Doğu ABD bölgesinde bir çalışma alanı ve Otomasyon hesabı Doğu ABD 2 ' dir.

### <a name="create-a-link-between-a-log-analytics-workspace-and-automation-account"></a>Log Analytics çalışma alanını ve Otomasyon hesabı arasında bir bağlantı oluşturun
Log Analytics çalışma alanını ve Otomasyon hesabının nasıl belirttiğiniz çözümünüz için yükleme yöntemine bağlıdır.

* Azure Marketi aracılığıyla bir çözüm yükleme sırasında bir çalışma alanı ve Otomasyon hesabı için istenir. Zaten bağlı değilseniz, bunlar arasında bir bağlantı oluşturulur.
* Azure Marketi dışında çözümler için çözüm yüklenmeden önce Log Analytics çalışma alanını ve Otomasyon hesabı bağlamalısınız. Azure Market'te herhangi bir çözüm ve Log Analytics çalışma alanını ve Otomasyon hesabı seçerek bunu yapabilirsiniz. Log Analytics çalışma alanını ve Otomasyon hesabı seçili hemen sonra bağlantıyı oluşturulduğundan çözüm gerçekten yüklemeniz gerekmez. Ardından bağlantıyı oluşturulduktan sonra herhangi bir çözümü, Log Analytics çalışma alanını ve Otomasyon hesabı kullanabilirsiniz.

### <a name="verify-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Log Analytics çalışma alanını ve Otomasyon hesabı arasındaki bağlantıyı doğrulayın
Aşağıdaki yordamı kullanarak bir Otomasyon hesabı ile Log Analytics çalışma alanı arasındaki bağlantı doğrulayabilirsiniz.

1. Azure portalında Otomasyon hesabı seçin.
1. Kaydırma **ilgili kaynakları** menüsünün bölümünde.
1. Varsa **çalışma** ayarı etkinse, sonra bu hesabı bir Log Analytics çalışma alanına bağlı. Tıklayabilirsiniz **çalışma** çalışma alanının ayrıntılarını görüntülemek için.

## <a name="remove-a-monitoring-solution"></a>İzleme çözümünü kaldırma
Yüklü çözümünü kaldırmak için bulmak [yüklü çözümlerinin listesini](#list-installed-monitoring-solutions). Kendi Özet sayfasını açın ve ardından çözümün adına tıklayarak **Sil**.


## <a name="next-steps"></a>Sonraki adımlar
* [Microsoft 'un izleme çözümlerinin bir listesini](solutions-inventory.md)alın.
* İzleme çözümleri tarafından toplanan verileri analiz etmek için [sorgular oluşturmayı](../log-query/log-query-overview.md) öğrenin.


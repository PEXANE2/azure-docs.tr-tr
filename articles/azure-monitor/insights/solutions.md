---
title: Azure Monitör'de izleme çözümleri | Microsoft Dokümanlar
description: Azure Monitor'daki izleme çözümleri, belirli bir sorun alanı etrafında döndürülen ölçümler sağlayan bir mantık, görselleştirme ve veri toplama kuralları koleksiyonudur.  Bu makalede, izleme çözümlerinin yüklenmesi ve kullanılması hakkında bilgi verilmektedir.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/13/2019
ms.openlocfilehash: a04ca3768ade6058c59393591c252bc4347a3663
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79275210"
---
# <a name="monitoring-solutions-in-azure-monitor"></a>Azure Monitor'da izleme çözümleri
İzleme çözümleri, belirli bir uygulamanın veya hizmetin işleyişi hakkında ek bilgiler sağlamak için Azure'daki hizmetlerden yararlanır. Bu makalede, Azure'daki izleme çözümlerine ve bunları kullanma ve yükleme ayrıntılarına kısa bir genel bakış sağlanmaktadır.

> [!NOTE]
> İzleme çözümleri daha önce yönetim çözümleri olarak adlandırılıyordu.

İzleme çözümleri genellikle günlük verilerini toplar ve toplanan verileri çözümlemek için sorgular ve görünümler sağlar. Uygulama veya hizmetle ilgili eylemleri gerçekleştirmek için Azure Otomasyonu gibi diğer hizmetlerden de yararlanabilirler.

Kullandığınız tüm uygulamalar ve hizmetler için Azure Monitor'a izleme çözümleri ekleyebilirsiniz. Genellikle hiçbir ücret ödemeden kullanılabilirler, ancak kullanım ücretleri ne kadar gerektirebilecek veriler toplarlar. Microsoft tarafından sağlanan çözümlere ek olarak, iş ortakları ve müşteriler kendi ortamlarında kullanılmak üzere veya topluluk aracılığıyla müşterilerin kullanımına sunulacak [yönetim çözümleri oluşturabilir.](solutions-creating.md)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="use-monitoring-solutions"></a>İzleme çözümlerini kullanma
Çalışma alanında yüklü her çözüm için bir döşeme görüntülemek için Azure Monitor'da **Genel Bakış** sayfasını açın. 

1. [Azure portalına](https://ms.portal.azure.com)gidin. **Monitör'ü**arayın ve seçin.
1. **Öngörüler** menüsüaltında **Daha Fazla'yı**seçin.
1. Çalışma alanını veya kutucuklar için kullanılan zaman aralığını değiştirmek için ekranın üst kısmındaki açılır kutuları kullanın.
1. Toplanan verilerin daha ayrıntılı analizini içeren görünümünü açmak için bir çözüm için döşemeyi tıklatın.

![Genel Bakış](media/solutions/overview.png)

İzleme çözümleri birden çok Azure kaynağı türü içerebilir ve diğer kaynaklar gibi bir çözümle birlikte verilen kaynakları görüntüleyebilirsiniz. Örneğin, çözüme dahil olan tüm günlük sorguları Sorgu [gezgininde](../log-query/get-started-portal.md#load-queries) **Çözüm Sorguları** altında listelenir [Günlük sorgularıyla](../log-query/log-query-overview.md)özel çözümleme yaparken bu sorguları kullanabilirsiniz.

## <a name="list-installed-monitoring-solutions"></a>Yüklü izleme çözümlerini listele 
Aboneliğinizde yüklü izleme çözümlerini listelemek için aşağıdaki yordamı kullanın.

1. [Azure portalına](https://ms.portal.azure.com)gidin. **Çözümleri**arayın ve seçin.
1. Tüm çalışma alanlarınızda yüklü çözümler listelenir. Çözümün adı, yüklü olduğu çalışma alanının adı tarafından izlenir.
1. Abonelik veya kaynak grubuna göre filtre lemek için ekranın üst kısmındaki açılır kutuları kullanın.


![Tüm çözümleri listele](media/solutions/list-solutions-all.png)

Özet sayfasını açmak için çözümün adını tıklayın. Bu sayfa, çözümde yer alan tüm görünümleri görüntüler ve çözümün kendisi ve çalışma alanı için farklı seçenekler sağlar. Çözümleri listelemek için yukarıdaki yordamlardan birini kullanarak bir çözüm için özet sayfasını görüntüleyin ve ardından çözümün adını tıklatın.

![Çözüm özellikleri](media/solutions/solution-properties.png)



## <a name="install-a-monitoring-solution"></a>İzleme çözümü yükleme
Microsoft ve iş ortaklarından gelen izleme çözümleri [Azure Marketi'nden](https://azuremarketplace.microsoft.com)edinilebilir. Kullanılabilir çözümleri arayabilir ve aşağıdaki yordamı kullanarak bunları yükleyebilirsiniz. Bir çözüm yüklediğinizde, çözümün yüklendiği ve verilerinin toplandığı bir [Log Analytics çalışma alanı](../platform/manage-access.md) seçmeniz gerekir.

1. [Aboneliğiniz için çözümler listesinden](#list-installed-monitoring-solutions) **Ekle'yi**tıklatın.
1. Bir çözüme göz atın veya arayın. Ayrıca [bu arama bağlantısından](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/management-tools?page=1&subcategories=management-solutions)çözümlere göz atabilirsiniz.
1. İstediğiniz izleme çözümünü bulun ve açıklamasını okuyun.
1. Yükleme işlemini başlatmak için **Oluştur'u** tıklatın.
1. Yükleme işlemi başladığında, Log Analytics çalışma alanını belirtmeniz ve çözüm için gerekli yapılandırmayı sağlamanız istenir.

![Çözüm yükleme](media/solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>Topluluktan bir çözüm yükleme
Topluluk üyeleri Azure Quickstart Şablonlarına yönetim çözümleri gönderebilir. Bu çözümleri doğrudan yükleyebilir veya daha sonraki yükleme için şablonları indirebilirsiniz.

1. Bir çalışma alanı ve hesabı bağlamak için [Log Analytics çalışma alanı ve Otomasyon hesabında](#log-analytics-workspace-and-automation-account) açıklanan işlemi izleyin.
2. Azure [Quickstart Şablonlarına](https://azure.microsoft.com/documentation/templates/)gidin. 
3. İlgilendiğiniz bir çözüm arayın.
4. Ayrıntıları görüntülemek için sonuçlardan çözümü seçin.
5. **Azure'a Dağıt düğmesini** tıklatın.
6. Çözümdeki herhangi bir parametre için değerlere ek olarak kaynak grubu ve konum gibi bilgiler sağlamanız istenir.
7. Çözümü yüklemek için **Satın Al'ı** tıklatın.


## <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics çalışma alanı ve Otomasyon hesabı
Tüm izleme çözümleri, çözüm tarafından toplanan verileri depolamak ve günlük aramalarını ve görünümlerini barındırmak için bir [Log Analytics çalışma alanı](../platform/manage-access.md) gerektirir. Bazı çözümler, runbook'ları ve ilgili kaynakları içerecek bir [Otomasyon hesabı](../../automation/automation-security-overview.md#automation-account-overview) da gerektirir. Çalışma alanı ve hesap aşağıdaki gereksinimleri karşılamalıdır.

* Bir çözümün her yüklemesi yalnızca bir Log Analytics çalışma alanı ve bir Otomasyon hesabı kullanabilir. Çözümü birden çok çalışma alanlarına ayrı ayrı yükleyebilirsiniz.
* Bir çözüm bir Otomasyon hesabı gerektiriyorsa, Log Analytics çalışma alanı ve Otomasyon hesabının birbirine bağlanması gerekir. Log Analytics çalışma alanı yalnızca bir Otomasyon hesabına bağlanabilir ve Bir Otomasyon hesabı yalnızca bir Log Analytics çalışma alanına bağlanabilir.
* Bağlanabilmek için Log Analytics çalışma alanı ve Otomasyon hesabının aynı kaynak grubunda ve bölgede olması gerekir. Bunun istisnası, Doğu ABD bölgesinde bir çalışma alanı ve Doğu ABD 2'deki Otomasyon hesabıdır.

### <a name="create-a-link-between-a-log-analytics-workspace-and-automation-account"></a>Log Analytics çalışma alanı ve Otomasyon hesabı arasında bağlantı oluşturma
Log Analytics çalışma alanı ve Otomasyon hesabını nasıl belirtdiğiniz, çözümünüz için yükleme yöntemine bağlıdır.

* Azure Marketi üzerinden bir çözüm yüklediğinizde, bir çalışma alanı ve Otomasyon hesabı için istenirsiniz. Aralarındaki bağlantı, zaten bağlı değillerse oluşturulur.
* Azure Marketi dışındaki çözümler için, çözümü yüklemeden önce Log Analytics çalışma alanı ve Otomasyon hesabını bağlamanız gerekir. Bunu, Azure Marketi'ndeki herhangi bir çözümü seçerek ve Log Analytics çalışma alanı ve Otomasyon hesabını seçerek yapabilirsiniz. Log Analytics çalışma alanı ve Otomasyon hesabı seçilir seçilmez bağlantı oluşturulduğu için çözümü gerçekten yüklemeniz gerekmez. Bağlantı oluşturulduktan sonra, herhangi bir çözüm için Log Analytics çalışma alanı ve Otomasyon hesabını kullanabilirsiniz.

### <a name="verify-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Log Analytics çalışma alanı ile Otomasyon hesabı arasındaki bağlantıyı doğrulayın
Log Analytics çalışma alanı ile Otomasyon hesabı arasındaki bağlantıyı aşağıdaki yordamı kullanarak doğrulayabilirsiniz.

1. Azure portalındaki Otomasyon hesabını seçin.
1. Menünün **İlgili Kaynaklar** bölümüne gidin.
1. Çalışma **Alanı** ayarı etkinse, bu hesap bir Log Analytics çalışma alanına bağlanır. **Çalışma alanının** ayrıntılarını görüntülemek için Çalışma Alanı'nı tıklatabilirsiniz.

## <a name="remove-a-monitoring-solution"></a>İzleme çözümünü kaldırma
Yüklü bir çözümü kaldırmak için, [yüklü çözümler listesinde](#list-installed-monitoring-solutions)bulun. Özet sayfasını açmak için çözümün adını tıklatın ve sonra **Sil'e**tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
* [Microsoft'tan izleme çözümlerinin bir listesini](solutions-inventory.md)alın.
* İzleme çözümleri yle toplanan verileri çözümlemek için [sorguoluşturmayı](../log-query/log-query-overview.md) öğrenin.


---
title: Azure'da bir yönetim çözümü oluşturun | Microsoft Dokümanlar
description: Yönetim çözümleri, Azure'da müşterilerin Günlük Analizi çalışma alanına ekleyebileceği paketlenmiş yönetim senaryolarını içerir.  Bu makalede, kendi ortamınızda kullanılmak üzere veya müşterilerinizin kullanımına sunulacak yönetim çözümlerini nasıl oluşturabileceğiniz hakkında ayrıntılı bilgi verilmektedir.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/20/2017
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f1605597c7716ba6a896c7ecdae968f07d66027b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663224"
---
# <a name="design-and-build-a-management-solution-in-azure-preview"></a>Azure'da bir yönetim çözümü tasarlama ve oluşturma (Önizleme)
> [!NOTE]
> Bu, Şu anda önizlemede olan Azure'da yönetim çözümleri oluşturmak için ön belgelerdir. Aşağıda açıklanan herhangi bir şema değişebilir.

[Yönetim çözümleri,]( solutions.md) müşterilerin Log Analytics çalışma alanına ekleyebileceği paketlenmiş yönetim senaryoları sağlar.  Bu makalede, en yaygın gereksinimler için uygun bir yönetim çözümü tasarlamak ve oluşturmak için temel bir süreç sunar.  Yönetim çözümleri oluşturmakta yeniyseniz, bu işlemi bir başlangıç noktası olarak kullanabilir ve gereksinimleriniz geliştikçe daha karmaşık çözümler için kavramlardan yararlanabilirsiniz.

## <a name="what-is-a-management-solution"></a>Yönetim çözümü nedir?

Yönetim çözümleri, belirli bir yönetim senaryosuna ulaşmak için birlikte çalışan Azure kaynakları içerir.  Bunlar, çözüm yüklendiğinde içerdiği kaynakları nasıl yükleyeceklerine ve yapılandıracaklarına ilişkin ayrıntıları içeren [Kaynak Yönetimi şablonları](../../azure-resource-manager/templates/quickstart-create-templates-use-the-portal.md) olarak uygulanır.

Temel strateji, Azure ortamınızda tek tek bileşenleri oluşturarak yönetim çözümünüzü başlatmaktır.  İşlevselliğin düzgün çalışmasını yaptıktan sonra, bunları bir [yönetim çözüm dosyasında]( solutions-solution-file.md)paketlemeye başlayabilirsiniz. 


## <a name="design-your-solution"></a>Çözümünüzü tasarlayın
Bir yönetim çözümü için en yaygın desen aşağıdaki diyagramda gösterilmiştir.  Bu desendeki farklı bileşenler aşağıda ele alınmıştır.

![Yönetim çözümüne genel bakış](media/solutions-creating/solution-overview.png)


### <a name="data-sources"></a>Veri kaynakları
Çözüm tasarlamanın ilk adımı, Log Analytics deposundan istediğiniz verileri belirlemektir.  Bu veriler bir veri [kaynağı](../../azure-monitor/platform/agent-data-sources.md) veya başka bir [çözüm]( solutions.md)tarafından toplanabilir veya çözümünüzün bu verileri toplamak için işlemi sağlaması gerekebilir.

[Log Analytics'teki Veri kaynaklarında](../../azure-monitor/platform/agent-data-sources.md)açıklandığı gibi, Günlük Analizi deposunda toplanabilir veri kaynaklarının çeşitli yolları vardır.  Buna, Windows Olay Günlüğü'ndeki veya Syslog tarafından oluşturulan ve hem Windows hem de Linux istemcileri için performans sayaçlarına ek olarak oluşturulan olaylar dahildir.  Azure Monitor tarafından toplanan Azure kaynaklarından veri de toplayabilirsiniz.  

Kullanılabilir veri kaynaklarından herhangi biri aracılığıyla erişilemeyen verilere ihtiyaç duyarsanız, REST API'yi çağırabilen herhangi bir istemciden Log Analytics deposuna veri yazmanıza olanak tanıyan [HTTP Veri Toplayıcı API'sini](../../azure-monitor/platform/data-collector-api.md) kullanabilirsiniz.  Bir yönetim çözümünde özel veri toplamanın en yaygın yolu, [Azure Otomasyonu'nda](../../automation/automation-runbook-types.md) gerekli verileri Azure veya dış kaynaklardan toplayan ve depoya yazmak için Veri Toplayıcı API'sini kullanan bir runbook oluşturmaktır.  

### <a name="log-searches"></a>Günlük aramaları
[Günlük aramaları,](../../azure-monitor/log-query/log-query-overview.md) Log Analytics deposundaki verileri ayıklamak ve çözümlemek için kullanılır.  Bunlar, kullanıcının depodaki verilerin geçici analizini yapmasına izin vermenin yanı sıra görünümler ve uyarılar tarafından da kullanılır.  

Herhangi bir görünüm veya uyarı tarafından kullanılmasalar bile kullanıcıya yararlı olacağını düşündüğünüz sorguları tanımlamanız gerekir.  Bunlar portalda Kaydedilmiş Aramalar olarak kullanılabilir ve bunları özel görünümünüze [Sorgular görselleştirme bölümüne](../../azure-monitor/platform/view-designer-parts.md#list-of-queries-part) de ekleyebilirsiniz.

### <a name="alerts"></a>Uyarılar
[Log Analytics'teki uyarılar,](../../azure-monitor/platform/alerts-overview.md) [günlük aramaları](#log-searches) yoluyla depodaki verilere karşı sorunları tanımlar.  Kullanıcıyı bilgilendirir veya yanıt olarak otomatik olarak bir eylem çalıştırın. Uygulamanız için farklı uyarı koşullarını belirlemeli ve çözüm dosyanıza ilgili uyarı kuralları eklemeniz gerekir.

Sorun otomatik bir işlemle düzeltilebilirse, bu düzeltmeyi gerçekleştirmek için genellikle Azure Otomasyonu'nda bir runbook oluşturursunuz.  Azure hizmetlerinin çoğu, runbook'un bu tür işlevleri gerçekleştirmek için yararlanacağı [cmdlet'lerle](/powershell/azure/overview) yönetilebilir.

Çözümünüz bir uyarıya yanıt olarak harici işlevsellik gerektiriyorsa, [webhook yanıtı](../../azure-monitor/platform/alerts-metric.md)kullanabilirsiniz.  Bu, uyarıdan bilgi gönderen harici bir web hizmetini aramanızı sağlar.

### <a name="views"></a>Görünümler
Log Analytics'teki görünümler, Log Analytics deposundaki verileri görselleştirmek için kullanılır.  Her çözüm genellikle kullanıcının ana panosunda görüntülenen bir [döşeme](../../azure-monitor/platform/view-designer-tiles.md) ile tek bir görünüm içerir.  Görünüm, toplanan verilerin kullanıcıya farklı görselleştirmelerini sağlamak için herhangi bir sayıda [görselleştirme parçası](../../azure-monitor/platform/view-designer-parts.md) içerebilir.

Daha sonra çözüm dosyanıza eklenmesi için dışa aktarabileceğiniz [Görünüm Tasarımcısı'nı kullanarak özel görünümler oluşturursunuz.](../../azure-monitor/platform/view-designer.md)  


## <a name="create-solution-file"></a>Çözüm dosyası oluşturma
Çözümünüzün bir parçası olacak bileşenleri yapılandırıp test ettikten sonra [çözüm dosyanızı oluşturabilirsiniz.]( solutions-solution-file.md)  Çözüm bileşenlerini, dosyadaki diğer kaynaklarla ilişkisi olan bir [çözüm kaynağı]( solutions-solution-file.md#solution-resource) içeren bir Kaynak [Yöneticisi şablonunda](../../azure-resource-manager/templates/template-syntax.md) uygularsınız.  


## <a name="test-your-solution"></a>Çözümünüzü test edin
Çözümünüzü geliştirirken, çözümünüzü çalışma alanınızda yüklemeniz ve test etmeniz gerekir.  Bunu, [Kaynak Yöneticisi şablonlarını sınamak ve yüklemek](../../azure-resource-manager/templates/deploy-powershell.md)için kullanılabilir yöntemlerden herhangi birini kullanarak yapabilirsiniz.

## <a name="publish-your-solution"></a>Çözümünüzü yayımlayın
Çözümünüzü tamamladıktan ve test ettikten sonra, aşağıdaki kaynaklardan herhangi biri aracılığıyla müşterilerinizin kullanımına sunabilirsiniz.

- **Azure Quickstart şablonları.**  [Azure Quickstart şablonları,](https://azure.microsoft.com/resources/templates/) GitHub aracılığıyla topluluk tarafından katkıda bulunulan Kaynak Yöneticisi şablonları kümesidir.  [Katkı kılavuzundaki](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE)bilgileri takip ederek çözümünüzü kullanıma sunabilirsiniz.
- **Azure Marketi**.  [Azure Marketi,](https://azuremarketplace.microsoft.com/marketplace/) çözümünüzü diğer geliştiricilere, ISV'lere ve BT uzmanlarına dağıtmanıza ve satmanıza olanak tanır.  Azure Marketi'nde bir teklifi yayınlama ve yönetme konusunda çözümünüzü Azure [Marketi'nde](../../marketplace/marketplace-publishers-guide.md)nasıl yayımlayacağınızı öğrenebilirsiniz.



## <a name="next-steps"></a>Sonraki adımlar
* Yönetim çözümünüz için [nasıl bir çözüm dosyası oluşturabilirsiniz]( solutions-solution-file.md) öğrenin.
* Azure Kaynak [Yöneticisi şablonları yazma](../../azure-resource-manager/templates/template-syntax.md)ayrıntılarını öğrenin.
* Farklı Kaynak Yöneticisi şablonlarından örnekler için [Azure Quickstart Şablonlarında](https://azure.microsoft.com/documentation/templates) arama yapın.

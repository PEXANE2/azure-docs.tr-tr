---
title: Team Data Science Process nedir?
description: Tahmine dayalı analiz çözümlerini ve akıllı uygulamalar sunmak için bir veri bilimi metodolojisidir sağlar.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: overview
ms.date: 1/10/2020
ms.author: tdsp
ms.custom: previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 10d6e562301e089700940ac5dfb212bcc4e09653
ms.sourcegitcommit: 20429bc76342f9d365b1ad9fb8acc390a671d61e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79088076"
---
# <a name="what-is-the-team-data-science-process"></a>Team Data Science Process nedir?

Team Data Science işlem (TDSP), Tahmine dayalı analiz çözümlerini ve akıllı uygulamaları etkili bir şekilde sunmak için bir Çevik, yinelemeli bir veri bilimi metodolojisidir olur. TDSP, takım rollerinin en iyi şekilde nasıl çalıştığını önererek ekip işbirliğini ve öğrenilmesine yardımcı olur. TDSP, veri bilimi girişimlerinin başarıyla uygulanmasına yardımcı olmak için Microsoft ve diğer sektör liderlerinden en iyi uygulamaları ve yapıları içerir. Hedef, şirketlerin kendi analiz programlarının avantajlarını tam olarak hayata geçirmesine yardımcı olmaktır.

Bu makalede TDSP ve ana bileşenlerini genel bir bakış sağlar. Burada, farklı araç türleriyle uygulanabilecek işlemin genel bir açıklamasını sunuyoruz. Proje görevlerini ve rolleri işlem yaşam döngüsü içinde ilgili daha ayrıntılı bir açıklama ek bağlantılı konularda sağlanır. Microsoft araçları ve TDSP ekiplerimiz uygulamak için kullandığımız altyapısı belirli bir kümesini kullanarak TDSP uygulama konusunda yönergeler de verilmektedir.

## <a name="key-components-of-the-tdsp"></a>TDSP temel bileşenleri

TDSP anahtar aşağıdaki bileşenlerden oluşur:

- **Veri bilimi yaşam döngüsü** tanımı
- **Standartlaştırılmış bir proje yapısı**
- Veri bilimi projeleri için **altyapı ve kaynaklar**
- Proje yürütmesi için **Araçlar ve yardımcı programlar**


## <a name="data-science-lifecycle"></a>Veri bilimi yaşam döngüsü

Team Data Science işlem (TDSP), veri bilimi proje geliştirme yapısı için bir yaşam döngüsü sağlar. Yaşam döngüsü, başarılı projelerin izlediği tam adımları özetler.

[Net DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [kdd](https://wikipedia.org/wiki/Data_mining#Process) veya kuruluşunuzun kendi özel süreci gibi başka bir veri bilimi yaşam döngüsü kullanıyorsanız, görev tabanlı TDSP 'yi bu geliştirme ömürleri bağlamında kullanmaya devam edebilirsiniz. Yüksek düzeyde, bu farklı yöntemleri çok ortaktır. 

Bu yaşam döngüsünü, akıllı uygulamalar bir parçası olarak gönderilen veri bilimi projeleri için tasarlanmıştır. Bu uygulamalar, Tahmine dayalı analiz için makine öğrenme veya yapay zeka modelleri dağıtın. Araştırmacı veri bilimi projeleri veya improvised Analytics projeleri, bu işlemi kullanmanın da avantajlarından yararlanabilir. Ancak, bu gibi durumlarda, bazı açıklanan adımları gerekmeyebilir.    

Yaşam döngüsü projeleri genellikle genellikle yinelemeli olarak yürütme, önemli aşamalar açıklanmaktadır:

* **İş anlama**
* **Veri alma ve anlama**
* **Oluşturmanın**
* **Dağıtım**
* **Müşteri kabulü**

**Team Data Science işlem yaşam döngüsünün**görsel bir gösterimi aşağıda verilmiştir. 

![TDSP Lifecycle2](./media/overview/tdsp-lifecycle2.png) 

TDSP yaşam döngüsünün her aşamasına yönelik hedefler, görevler ve belge yapıtları, [Team Data Science işlem yaşam döngüsü](lifecycle.md) konusunda açıklanmaktadır. Bu görevler ve yapıtları proje rolleri ile ilişkilendirilmiş:

- Çözüm Mimarı
- Proje Yöneticisi
- Veri bilimcisi
- Proje lideri 

Aşağıdaki diyagramda görevlerinde (mavi) ve bu rolleri (dikey eksende) için yaşam döngüsü (yatay eksende) her aşaması ilişkili yapıları (yeşil içinde) bir kılavuz görünümünü sağlar. 

[![TDSP-roller-ve-görevler](./media/overview/tdsp-tasks-by-roles.png)](./media/overview/tdsp-tasks-by-roles.png#lightbox)

## <a name="standardized-project-structure"></a>Standart Proje yapısı

Bir dizin yapısına paylaşın ve proje belgelerini şablonlarını kullanma tüm projeleri sahip, onların projeleri hakkında bilgi, takım üyeleri için kolaylaştırır. Tüm kod ve belgelere işbirliğini etkinleştirmek için Git, TFS veya Subversion gibi bir sürüm denetim sistemi (VC) depolanır. İzleme görevleri ve izleme sistemi Jıra, yarışı ve Azure DevOps gibi bir Çevik proje özelliklerinde, tek tek özellikler için kodun yakın izleme sağlar. Bu tür izleme de daha iyi maliyet tahminlerini almak takımların imkan tanır. TDSP, sürüm oluşturma, bilgi güvenliği ve işbirliği için VC'ler her proje için ayrı bir depo oluşturmanızı önerir. Tüm projeler için standartlaştırılmış yapısı kuruluş genelinde kurumsal bilgi yapı yardımcı olur.

Klasör yapısını ve standart olmayan konumlara gerekli belgeleri için şablonlar sağlar. Veri keşfi ve özellik ayıklama kodunu içeren ve bu modeli yinelemeler kayıt dosyaları bu klasör yapısını düzenler. Bu şablonlar, başkaları tarafından yapılan iş anlamak ve takıma yeni üyeler eklemek için takım üyeleri için kolaylaştırır. Belge şablonları markdown biçiminde görebilecek ve kolay bir işlemdir. Her bir proje için önemli sorulara denetim listeleri sağlamak üzere şablonlar kullanın ve bu da sorunun iyi tanımlandığını ve teslim edilebilirler beklenen kaliteyi karşıladığından emin olur. Örneklere şunlar dahildir:

- projenin kapsamını ve iş sorununu belge için bir proje kurucu
- Belge yapısı ve istatistikleri ham verilerin veri raporlarını
- belge türetilmiş özellikleri için model raporları
- ROC eğrileri veya MSE gibi performans ölçümlerini modeli


[![TDSP-dizinler](./media/overview/tdsp-dir-structure.png)](./media/overview/tdsp-dir-structure.png#lightbox)

Dizin yapısı [GitHub](https://github.com/Azure/Azure-TDSP-ProjectTemplate)'dan kopyalanabilir.

## <a name="infrastructure-and-resources-for-data-science-projects"></a>Altyapı ve veri bilimi projeleri için kaynaklar  

TDSP paylaşılan analiz ve depolama altyapısı gibi yönetmek için öneriler sağlar:

- veri kümelerini depolamak için bulut dosya sistemleri 
- veritabanları
- büyük veri (Hadoop veya Spark) kümeleri 
- Machine learning hizmeti 

Ham ve işlenen veri kümelerinin depolandığı analiz ve depolama altyapısı, bulutta veya şirket içinde olabilir. Bu Altyapı yeniden üretilebilen analizi sağlar. Ayrıca, tutarsızlıklar ve gereksiz altyapı maliyetlerinden açabilir çoğaltma önler. Bu kaynaklara güvenli bir şekilde bağlanmak her takım üyesinin izin paylaşılan kaynakları sağlamak ve bunları izlemek için Araçlar sağlanır. Proje üyelerini tutarlı işlem ortamı oluşturmak için de iyi bir uygulamadır. Farklı ekip üyelerinin çoğaltabilir ve denemeleri doğrulayın.

Birden fazla proje üzerinde çalışma ve çeşitli bulut analiz altyapısı bileşenleriyle paylaşımı takım örneği aşağıda verilmiştir.

[![TDSP-altyapı](./media/overview/tdsp-analytics-infra.png)](./media/overview/tdsp-analytics-infra.png#lightbox) 


## <a name="tools-and-utilities-for-project-execution"></a>Araçlar ve yardımcı programlar için proje yürütme

Çoğu kuruluş işlemde giriş zordur. Veri bilimi işlemi ve yaşam döngüsü Yardım düşük önündeki engelleri uygulamak ve kendi benimseme tutarlılığını artırmak için sağlanan araçları. TDSP araçları ve betikleri TDSP benimsenmesini ekip içinde hızlı giriş yapmak için başlangıç kümesi sağlar. Ayrıca, veri bilimi yaşam döngüsünü veri keşfi ve modelleme temel gibi ortak görevlerin bazıları otomatikleştirilmesine yardımcı olur. Kişilerin, ekip paylaşılan kod deposunda paylaşılan araçlar ve yardımcı programlar katkıda bulunmak için iyi tanımlanmış bir yapı mevcuttur. Bu kaynaklar, sonra ekip veya kuruluş içindeki diğer projeleri tarafından yararlanılabilir. TDSP araçları ve yardımcı programlar için bütün topluluk Katkıları etkinleştirmek de planları. TDSP yardımcı programları [GitHub](https://github.com/Azure/Azure-TDSP-Utilities)'dan kopyalanabilir.


## <a name="next-steps"></a>Sonraki adımlar

[Ekip veri bilimi işlemi: roller ve görevler](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/roles-tasks.md) Bu işlemi standartlaştırın bir veri bilimi ekibi için anahtar personeli rollerini ve bunlarla ilişkili görevleri özetler. 

---
title: Ekip Veri Bilimi Süreci rolleri ve görevleri
description: Veri bilimi grubu için temel bileşenlerin, personel rollerinin ve ilişkili görevlerin anahatlarını.
author: marktab
manager: marktab
editor: marktab
services: machine-learning
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: c1ed731943abf0efdd99ea54d2318fa402835e08
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720019"
---
# <a name="team-data-science-process-roles-and-tasks"></a>Ekip Veri Bilimi Süreci rolleri ve görevleri

Ekip Veri Bilimi Süreci (TDSP), Microsoft tarafından geliştirilen ve tahmine dayalı analitik çözümleri ve akıllı uygulamalar oluşturmak için yapılandırılmış bir metodoloji sağlayan bir çerçevedir. Bu makalede, bu işlemi standartlaştıran bir veri bilimi ekibinin temel personel rolleri ve ilişkili görevleri özetlenir.

Bu giriş makalesi, TDSP ortamının nasıl ayarlanılacaya ilişkin öğreticilere bağlantılar kurar. Öğreticiler, Azure DevOps Projeleri, Azure Depoları ve Azure Panolarını kullanmak için ayrıntılı kılavuz sağlar.  Motive edici hedef, kavramdan modellemeye ve dağıtıma geçmektir.

Öğreticiler Azure DevOps'leri kullanır, çünkü TDSP Microsoft'ta bu şekilde uygulanır. Azure DevOps, rol tabanlı güvenlik, iş öğesi yönetimi ve izleme ve kod barındırma, paylaşma ve kaynak denetimini tümleştirerek işbirliğini kolaylaştırır. Öğreticiler ayrıca, Microsoft yazılımları ve Azure hizmetleriyle önceden yapılandırılmış ve entegre edilmiş birkaç popüler veri bilimi aracı olan analytics masaüstü olarak Azure [Veri Bilimi Sanal Makinesi](https://aka.ms/dsvm) (DSVM) kullanır. 

Öğreticileri diğer kod barındırma, çevik planlama ve geliştirme araçları ve ortamlarını kullanarak TDSP'yi uygulamak için kullanabilirsiniz, ancak bazı özellikler kullanılamayabilir.

## <a name="structure-of-data-science-groups-and-teams"></a>Veri bilimi gruplarının ve ekiplerinin yapısı

İşletmelerdeki veri bilimi işlevleri genellikle aşağıdaki hiyerarşide düzenlenir:

- Veri bilimi grubu
  - Grup içindeki veri bilimi ekibi/ler

Böyle bir yapıda, grup müşteri adayları ve takım adayları vardır. Genellikle, bir veri bilimi projesi bir veri bilimi ekibi tarafından yapılır. Veri bilimi ekipleri, proje yönetimi ve yönetim görevleri için proje ipuçlarına ve projenin veri bilimi ve veri mühendisliği bölümlerini gerçekleştirmek için bireysel veri bilimcileri ve mühendislerine sahiptir. İlk proje kurulumu ve yönetimi grup, takım veya proje müşteri adayları tarafından yapılır.

## <a name="definition-and-tasks-for-the-four-tdsp-roles"></a>Dört TDSP rolü için tanım ve görevler
Veri bilimi biriminin bir grup içindeki ekiplerden oluştuğu varsayımıyla, TDSP personeli için dört farklı rol vardır:

1. **Grup Yöneticisi**: Bir kuruluştaki tüm veri bilimi birimini yönetir. Bir veri bilimi biriminin birden çok ekibi olabilir ve bunların her biri farklı iş dikeylerinde birden fazla veri bilimi projesi üzerinde çalışıyor olabilir. Grup Yöneticisi görevlerini bir vekil olarak devredebilir, ancak rolile ilişkili görevler değişmez.
   
2. **Takım Lideri**: Bir işletmenin veri bilimi biriminde bir ekibi yönetir. Bir ekip birden fazla veri bilim adamlarından oluşur. Küçük bir veri bilimi birimi için Grup Yöneticisi ve Takım Müşteri Adayı aynı kişi olabilir.
   
3. **Proje Lideri**: Belirli bir veri bilimi projesinde bireysel veri bilimcilerin günlük faaliyetlerini yönetir.
   
4. **Proje Bireysel Katkıda Bulunanlar**: Veri Bilimcileri, İş Analistleri, Veri Mühendisleri, Mimarlar ve veri bilimi projesini yürüten diğer kişiler.

> [!NOTE]
> Bir işletmenin yapısına ve boyutuna bağlı olarak, tek bir kişi birden fazla rol oynayabilir veya bir rolü birden fazla kişi doldurabilir.

### <a name="tasks-to-be-completed-by-the-four-roles"></a>Dört rol tarafından tamamlanacak görevler

Aşağıdaki diyagram, her Takım Veri Bilimi Süreci rolü için üst düzey görevleri gösterir. Bu şema ve aşağıdaki, her TDSP rolü için görevlerin daha ayrıntılı anahat sorumluluklarına göre ihtiyacınız olan öğretici seçmenize yardımcı olabilir.

![Roller ve görevlere genel bakış](./media/roles-tasks/overview-tdsp-top-level.png)

## <a name="group-manager-tasks"></a>Grup Yöneticisi görevleri

Grup Yöneticisi veya atanmış bir TDSP sistem yöneticisi, TDSP'yi benimsemek için aşağıdaki görevleri tamamlar:

- Kuruluş içinde bir Azure DevOps **organizasyonu** ve grup projesi oluşturur. 
- Azure DevOps grup projesinde bir **proje şablonu deposu** oluşturur ve microsoft TDSP ekibi tarafından geliştirilen proje şablon deposundan tohumlar. Microsoft TDSP proje şablon deposu şunları sağlar:
  - Veri, kod ve belgeler için dizinler de dahil olmak üzere **standart laştırılmış dizin yapısı.**
  - Verimli bir veri bilimi işlemine rehberlik etmek için **standartlaştırılmış belge şablonları** kümesi.
- Bir **yardımcı program deposu**oluşturur ve Microsoft TDSP ekibi tarafından geliştirilen yardımcı program deposundan tohumlar. Microsoft'un TDSP yardımcı programı deposu, bir veri bilimcinin çalışmasını daha verimli hale getirmek için bir dizi yararlı yardımcı program sağlar. Microsoft yardımcı program deposu, etkileşimli veri arama, analiz, raporlama ve temel modelleme ve raporlama için yardımcı programları içerir.
- Kuruluş hesabı için **güvenlik denetimi ilkesini** ayarlar.

Ayrıntılı talimatlar [için, veri bilimi ekibi için Grup Yöneticisi görevlerine](group-manager-tasks.md)bakın.

## <a name="team-lead-tasks"></a>Takım Müşteri Adayı görevleri

Takım Müşteri Adayı veya atanmış proje yöneticisi, TDSP'yi benimsemek için aşağıdaki görevleri tamamlar:

- Grubun Azure DevOps organizasyonunda bir ekip **projesi** oluşturur.
- **Projedeki proje şablon deposunu** oluşturur ve Grup Yöneticisi veya temsilci tarafından ayarlanan grup proje şablonu deposundan tohumlar.
- **Takım yardımcı deposu oluşturur,** grup yardımcı programı deposundan tohumlar ve depoya takıma özgü yardımcı programlar ekler.
- Takım için yararlı veri varlıklarını depolamak için isteğe bağlı olarak [Azure dosya depolama](https://azure.microsoft.com/services/storage/files/) alanı oluşturur. Diğer ekip üyeleri bu paylaşılan bulut dosya mağazalarını analitik masaüstülerine monte edebilir.
- İsteğe bağlı olarak, Azure dosya depolama alanını takımın **DSVM'sine** bağlar ve ona ekip veri varlıkları ekler.
- Ekip üyeleri ekleyerek ve izinlerini yapılandırarak **güvenlik denetimini** ayarlar.

Ayrıntılı yönergeler [için, bir veri bilimi ekibi için Takım İş İleş görevlerine](team-lead-tasks.md)bakın.


## <a name="project-lead-tasks"></a>Proje Müşteri Adayı görevleri

Proje Müşteri Adayı, TDSP'yi benimsemek için aşağıdaki görevleri tamamlar:

- Takım projesinde bir **proje deposu** oluşturur ve proje şablondeposundan tohumlar.
- Projenin veri varlıklarını depolamak için isteğe bağlı olarak **Azure dosya depolama** alanı oluşturur.
- İsteğe bağlı olarak Azure dosya depolama alanını **DSVM'ye** bağlar ve projeye proje veri varlıkları ekler.
- Proje üyelerini ekleyerek ve izinlerini yapılandırarak **güvenlik denetimini** ayarlar.

Ayrıntılı yönergeler [için, veri bilimi ekibi için Proje İş Kaynağı görevlerine](project-lead-tasks.md)bakın.

## <a name="project-individual-contributor-tasks"></a>Proje Bireysel Katılımcı görevleri

Genellikle Veri Bilimcisi olan Project Individual Katılımcısı, TDSP'yi kullanarak aşağıdaki görevleri yapar:

- Proje müşteri adayı tarafından ayarlanan **proje deposunu** klonlar.
- İsteğe bağlı olarak paylaşılan ekibi bağlar ve **Azure dosya depolamasını** **Veri Bilimi Sanal Makinesi'ne** (DSVM) bağlar.
- Projeyi yürütür.

Bir projeye devam etmek için ayrıntılı talimatlar için, [veri bilimi ekibi için Project Individual Katılımcı görevlerine](project-ic-tasks.md)bakın.

## <a name="data-science-project-execution-workflow"></a>Veri bilimi proje yürütme iş akışı

İlgili öğreticileri izleyerek, veri bilimciler, proje müşteri adayları ve ekip müşteri adayları, proje için baştan sona tüm görevleri ve aşamaları izlemek için iş öğeleri oluşturabilir. Azure Repos'un kullanılması, veri bilimciler arasında işbirliğini destekler ve proje yürütülmesi sırasında oluşturulan yapıtların tüm proje üyeleri tarafından sürüm kontrolüne ve paylaşılmasına yardımcı olur. Azure DevOps, Azure Kurulları iş öğelerinizi Azure Repos depo şubelerinize bağlamanızı ve bir iş öğesi için neler yapıldığını kolayca izlemenizi sağlar.

Aşağıdaki şekilde proje yürütülmesi için TDSP iş akışı özetler:

![Tipik veri bilimi proje iş akışı](./media/roles-tasks/overview-project-execute.png)

İş akışı adımları üç faaliyethalinde gruplandırılabilir:

- Proje Müşteri Adayları sprint planlaması yapar
- Veri Bilim Adamları `git` iş öğeleri adrese dallarda eserler geliştirmek
- Proje Adayları veya diğer ekip üyeleri kod incelemeleri yapar ve çalışma dallarını ana dalda birleştirir

Proje yürütme iş akışı hakkında ayrıntılı talimatlar için veri [bilimi projelerinin Çevik gelişimine](agile-development.md)bakın.

## <a name="tdsp-project-template-repository"></a>TDSP proje şablondeposu deposu

Verimli proje yürütme ve işbirliğini desteklemek için Microsoft TDSP ekibinin [proje şablon deposunu](https://github.com/Azure/Azure-TDSP-ProjectTemplate) kullanın. Depo, size kendi TDSP projeleriniz için kullanabileceğiniz standartlaştırılmış bir dizin yapısı ve belge şablonları sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Ekip Veri Bilimi Süreci tarafından tanımlanan rollerin ve görevlerin daha ayrıntılı açıklamalarını keşfedin:

- [Veri bilimi ekibi için Grup Yöneticisi görevleri](group-manager-tasks.md)
- [Veri bilimi ekibi için Takım Müşteri Adayı görevleri](team-lead-tasks.md)
- [Veri bilimi ekibi için Proje Müşteri Adayı görevleri](project-lead-tasks.md)
- [Veri bilimi ekibi için Project Individual Katılımcı görevleri](project-ic-tasks.md)

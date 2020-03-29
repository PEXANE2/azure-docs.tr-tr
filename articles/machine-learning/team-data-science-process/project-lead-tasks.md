---
title: Ekip Veri Bilimi Sürecinde proje liderliği için görevler
description: Ekip Veri Bilimi Süreci ekibinde proje lideri için görevlerin ayrıntılı bir şekilde gözden geçirimi
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 4a4d3a1480a852218e698862a509c4af45e49eb8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76714422"
---
# <a name="project-lead-tasks-in-the-team-data-science-process"></a>Ekip Veri Bilimi Sürecinde Proje müşteri adayı görevleri

Bu makalede, ekip [Veri Bilimi Süreci](overview.md) 'nde (TDSP) proje ekibi için bir depo ayarlamak için proje müşteri *adayının* tamamladığını belirten görevler açıklanmaktadır. TDSP, Microsoft tarafından geliştirilen ve bulut tabanlı, tahmine dayalı analitik çözümlerini verimli bir şekilde yürütmek için yapılandırılmış bir etkinlik dizisi sağlayan bir çerçevedir. TDSP, işbirliği ve ekip öğrenimini geliştirmeye yardımcı olmak için tasarlanmıştır. TDSP'de standartlaştırılan bir veri bilimi ekibinin personel rollerinin ve ilişkili görevlerinin ana hatlarını öğrenmek [için, Ekip Veri Bilimi Süreci rolleri ve görevleri](roles-tasks.md)bölümüne bakın.

Bir proje kurşun TDSP belirli bir veri bilimi projesi üzerinde bireysel veri bilim adamlarının günlük faaliyetlerini yönetir. Aşağıdaki diyagram, proje iş adayı görevleri için iş akışını gösterir:

![Proje iş gücü görev iş akışı](./media/project-lead-tasks/project-leads-1-tdsp-creating-projects.png)

Bu öğretici Adım 1: Proje deposu oluşturma ve Adım 2: Takım ProjectTemplate deposundan tohum proje deposunu kapsar. 

Adım 3 için: Proje için Özellik iş öğesi oluşturma ve Adım 4: Proje aşamaları için Öyküler ekleme, [bkz.](agile-development.md)

Adım 5 için: Depolama/analiz varlıkları oluşturun ve özelleştirin ve gerekirse ekip [verileri ve analiz kaynakları oluşturun'a](team-lead-tasks.md#create-team-data-and-analytics-resources)bakın.

Adım 6 için: Proje deposunun güvenlik denetimini ayarlayın, [bkz.](team-lead-tasks.md#add-team-members-and-configure-permissions)

> [!NOTE] 
> Bu makalede, TDSP projesi ayarlamak için Azure Repos'u kullanmaktadır, çünkü TDSP Microsoft'ta bu şekilde uygulanacağıdır. Ekibiniz başka bir kod barındırma platformu kullanıyorsa, proje müşteri adayı görevleri aynıdır, ancak bunları tamamlama yolu farklı olabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, [grup yöneticinizin](group-manager-tasks.md) ve [ekip müşteri adayınızın](team-lead-tasks.md) aşağıdaki kaynakları ve izinleri ayarladığını varsayar:

- Veri biriminiz için Azure DevOps **organizasyonu**
- Veri bilimi ekibiniz için bir ekip **projesi**
- Takım şablonu ve yardımcı program **depoları**
- Projeniz için depo oluşturmanız ve düzenlemeniz için kuruluş hesabınızdaki **izinler**

Yerel makinenizde veya Veri Bilimi Sanal Makinenizde (DSVM) depoları klonlamak ve içeriği değiştirmek veya Azure dosya depolamasını ayarlayıp DSVM'nize monte etmek için bu denetim listesini de göz önünde bulundurmanız gerekir:

- Azure aboneliği.
- Git makinenize yüklendi. DSVM kullanıyorsanız, Git önceden yüklenmiş. Aksi takdirde, [Platformlar ve araçlar ekine](platforms-and-tools.md#appendix)bakın.
- Bir DSVM kullanmak istiyorsanız, Windows veya Linux DSVM oluşturulan ve Azure'da yapılandırılan. Daha fazla bilgi ve talimatlar için [Veri Bilimi Sanal Makine Dokümantasyonu'na](/azure/machine-learning/data-science-virtual-machine/)bakın.
- Windows DSVM için makinenizde [Git Kimlik Bilgisi Yöneticisi (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) yüklenir. *README.md* dosyasında, **İndir ve Yükle** bölümüne gidin ve en son **yükleyiyi**seçin. *.exe* yükleyicisini yükleyici sayfasından indirin ve çalıştırın. 
- Bir Linux DSVM için, DSVM'nizde ayarlanmış ve Azure DevOps'lere eklenen bir SSH ortak anahtarı. Daha fazla bilgi ve talimatlar için Platformlar ve [araçlar ekindeki](platforms-and-tools.md#appendix) **SSH ortak anahtar oluşturma** bölümüne bakın. 

## <a name="create-a-project-repository-in-your-team-project"></a>Ekip projenizde proje deposu oluşturma

Ekibinizin **MyTeam** projesinde bir proje deposu oluşturmak için:

1. Https'de ekibinizin proje **Özeti** sayfasına *\//\<gidin:\<sunucu adı\<>/ kuruluş adı>/ takım adı>*, örneğin, **https:\//dev.azure.com/DataScienceUnit/MyTeam**, ve sol navigasyondan **Repos'u** seçin. 
   
1. Sayfanın üst kısmındaki depo adını seçin ve açılan sayfadan **Yeni depoyu** seçin.
   
   ![Yeni depo seçin](./media/project-lead-tasks/project-leads-9-select-repos.png)
   
1. Yeni **bir depo oluştur** iletişim kutusunda, **Git'in Türü**altında seçildiğinden emin olun. **Git** **Depo adı**altında *DSProject1'i* girin ve ardından **Oluştur'u**seçin.
   
   ![Depo oluşturma](./media/project-lead-tasks/project-leads-3-create-project-repo-2.png)
   
1. Proje ayarları sayfanızda yeni **DSProject1** deposunu görebileceğinizi doğrulayın. 
   
   ![Proje Ayarlarında Proje deposu](./media/project-lead-tasks/project-leads-4-create-project-repo-3.png)

## <a name="import-the-team-template-into-your-project-repository"></a>Takım şablonunu proje deponuza alma

Proje deponuzu ekip şablonu deponuzun içeriğiyle doldurmak için:

1. Ekibinizin proje **Özeti** sayfasından sol navigasyonda **Depolar'ı** seçin. 
   
1. Sayfanın üst kısmındaki depo adını seçin ve açılır sayfadan **DSProject1'i** seçin.
   
1. **DSProject1 boş** sayfada, **Içe Aktar'ı**seçin. 
   
   ![Alma'yı seçin](./media/project-lead-tasks/project-leads-5-create-project-repo-4.png)
   
1. Git **deposu** iletişim kutusunda, **Kaynak türü**olarak **Git'i** seçin ve **TeamTemplate** deponuzun URL'sini **Klon URL'si**altında girin. URL *\//\<https: sunucu adı\<>/ kuruluş\<adı>/\<takım adı>/_git/ takım şablonu deposu adı>. * Örneğin: **https:\//dev.azure.com/DataScienceUnit/MyTeam/_git/TeamTemplate**. 
   
1. **İçeri aktar**'ı seçin. Takım şablonu deponuzun içeriği proje deponuza aktarılır. 
   
   ![Takım şablondeposunu içe aktarma](./media/project-lead-tasks/project-leads-6-create-project-repo-5.png)

Projenizin özel gereksinimlerini karşılayacak şekilde proje deponuzun içeriğini özelleştirmeniz gerekiyorsa, depo dosya ve klasörleri ekleyebilir, silebilir veya değiştirebilirsiniz. Doğrudan Azure Deposu'nda çalışabilir veya depoyu yerel makinenize veya DSVM'nize kopyalayabilir, değişiklik yapabilir ve güncelleştirmelerinizi paylaşılan proje deposuna işleyip itebilirsiniz. [Takım depolarının içeriğini Özelleştir'deki](team-lead-tasks.md#customize-the-contents-of-the-team-repositories)yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, Ekip Veri Bilimi Süreci tarafından tanımlanan diğer rollerin ve görevlerin ayrıntılı açıklamalarına bağlantılar verilmiştir:

- [Veri bilimi ekibi için Grup Yöneticisi görevleri](group-manager-tasks.md)
- [Veri bilimi ekibi için Takım Müşteri Adayı görevleri](team-lead-tasks.md)
- [Veri bilimi ekibi için bireysel Katılımcı görevleri](project-ic-tasks.md)

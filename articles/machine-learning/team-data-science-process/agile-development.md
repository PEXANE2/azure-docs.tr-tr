---
title: Veri bilimi projelerinin çevik gelişimi - Ekip Veri Bilimi Süreci
description: Ekip Veri Bilimi İşlemi'ni kullanarak proje ekibi içinde sistematik, sürüm kontrollü ve işbirlikçi bir şekilde bir veri bilimi projesini yürütün.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: c097c14406349d973e905fadb806cc159d9b16d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76722110"
---
# <a name="agile-development-of-data-science-projects"></a>Veri bilimi projelerinin çevik gelişimi

Bu belge, ekiplerin [Veri Bilimi Süreci'ni](overview.md) (TDSP) kullanarak proje ekibi içinde sistemli, sürüm kontrollü ve işbirliğine dayalı bir şekilde bir veri bilimi projesini nasıl yürütebildiğini açıklar. TDSP, Microsoft tarafından geliştirilen ve bulut tabanlı, tahmine dayalı analitik çözümlerini verimli bir şekilde yürütmek için yapılandırılmış bir etkinlik dizisi sağlayan bir çerçevedir. TDSP'yi standartlaştıran bir veri bilimi ekibi tarafından gerçekleştirilen rollerin ve görevlerin ana hatlarını öğrenmek [için, Ekip Veri Bilimi Süreci rolleri ve görevleri bölümüne](roles-tasks.md)bakın. 

Bu makalede, nasıl talimatlar içerir: 

- Projede yer alan iş öğeleri için *sprint planlaması* yapın.
- *Sprint'lere iş öğeleri* ekleyin.
- Özellikle TDSP yaşam döngüsü aşamalarıyla uyumlu *çevik türetilmiş* bir iş öğesi şablonu oluşturun ve kullanın.

Aşağıdaki yönergeler, Azure DevOps'lerde Azure Panoları ve Azure Repos'larını kullanarak bir TDSP ekip ortamı oluşturmak için gereken adımları özetler. Yönergeler Azure DevOps'leri kullanır, çünkü Microsoft'ta TDSP böyle uygulanır. Grubunuzun farklı bir kod barındırma platformu kullanıyorsa, takım müşteri adayı görevleri genellikle değişmez, ancak görevleri tamamlama yolu farklıdır. Örneğin, bir iş öğesini Git dalı ile bağlamak, GitHub ile Azure Deposu'nda olduğu gibi aynı olmayabilir.

Aşağıdaki şekil, bir veri bilimi projesi için tipik bir sprint planlama, kodlama ve kaynak denetimi iş akışını göstermektedir:

![Team Data Science Process](./media/agile-development/1-project-execute.png)

##  <a name="work-item-types"></a><a name='Terminology-1'></a>İş öğesi türleri

TDSP sprint planlama çerçevesinde, sık kullanılan dört *iş öğesi* türü vardır: *Özellikler,* *Kullanıcı Hikayeleri,* *Görevler*ve *Hatalar*. Tüm iş öğelerinin biriktirme listesi, Git deposu düzeyinde değil, proje düzeyindedir. 

İş öğesi türleri için tanımlar şunlardır:

- **Özellik**: Bir Özellik, proje etkileşimine karşılık gelir. Bir istemci ile farklı etkileşimler farklı Özelliklerdir ve projenin farklı aşamalarını farklı Özellikler olarak değerlendirmek en iyisidir. Özelliklerinizi adlandırmak için * \<ClientName>-\<EngagementName>* gibi bir şema seçerseniz, proje ve etkileşim bağlamını adlarından kolayca tanıyabilirsiniz.
  
- **Kullanıcı Hikayesi**: Kullanıcı Hikayeleri, Bir Özelliği uçuça tamamlamak için gereken iş öğeleridir. Kullanıcı Hikayeleri'ne örnek olarak şunlar verilebilir:
  - Verileri alma 
  - Verileri inceleme 
  - Özellikler oluşturma
  - Modelleri oluşturma
  - Modelleri kullanıma hazır hale getirin 
  - Modelleri yeniden eğitme
  
- **Görev**: Görevler, belirli bir Kullanıcı Öyküsü'nu tamamlamak için yapılması gereken atayılabilir iş öğeleridir. Örneğin, Kullanıcı Hikayesi *Veri Alma'daki* görevler aşağıdakiler olabilir:
  - SQL Server kimlik bilgilerini alın
  - SQL Veri Ambarına veri yükleme
  
- **Hata**: Hatalar, bir Görevi tamamlamak için düzeltilmesi gereken varolan kod veya belgelerdeki sorunlardır. Hataların eksik iş öğeleri neden olması durumunda, kullanıcı hikayeleri veya görevler olarak artabilir. 

Veri bilimciler, Özellikler, Kullanıcı Hikayeleri ve Görevler'in yerine TDSP yaşam döngüsü aşamaları ve alt aşamalarla geçen çevik bir şablon u kullanarak kendilerini daha rahat hissedebilirler. Özellikle TDSP yaşam döngüsü aşamalarıyla uyumlu çevik türetilmiş [bir](#set-up-agile-dsp-6)şablon oluşturmak için bkz.

> [!NOTE]
> TDSP, yazılım kodu yönetiminden (SCM) Özellikler, Kullanıcı Hikayeleri, Görevler ve Hatalar kavramlarını ödünç almaktadır. TDSP kavramları geleneksel SCM tanımlarından biraz farklı olabilir.

## <a name="plan-sprints"></a><a name='SprintPlanning-2'></a>Sprint'leri planla

Birçok veri bilim adamları tamamlamak ve farklı hızlarda devam etmek aylar sürebilir birden fazla proje ile meşgul. Sprint planlama, proje önceliklendirmesi ve kaynak planlama ve ayırma için yararlıdır. Azure Panoları'nda projeleriniz için iş öğelerini kolayca oluşturabilir, yönetebilir ve izleyebilir ve projelerin beklendiği gibi ilerlemesini sağlamak için sprint planlaması gerçekleştirebilirsiniz.

Sprint planlama hakkında daha fazla bilgi için [Bkz. Scrum sprintleri.](https://en.wikipedia.org/wiki/Scrum_(software_development)#Sprint) 

Azure Panolarında sprint planlaması hakkında daha fazla bilgi için [bkz.](/azure/devops/boards/sprints/assign-work-sprint) 

## <a name="add-a-feature-to-the-backlog"></a><a name='AddFeature-3'></a>Biriktirme listesine özellik ekleme 

Projeniz ve proje kodu deponuz oluşturulduktan sonra, projenizin çalışmasını temsil etmek üzere biriktirme listesine bir Özellik ekleyebilirsiniz.

1. Proje sayfanızdan, sol gezintide **Boards** > **Biriktirme Listesi'ni** seçin. 
   
1. Biriktirme **Listesi** sekmesinde, üst çubuktaki iş öğesi türü **Öyküler**ise, aşağı doğru açılır ve **Özellikler'i**seçin. Ardından **Yeni Çalışma Öğesi'ni seçin.**
   
   ![Yeni Çalışma Öğesi Seçin](./media/agile-development/2-sprint-team-overview.png)
   
1. Özellik için genellikle proje adınız için bir başlık girin ve ardından **en üste Ekle'yi**seçin. 
   
   ![Bir başlık girin ve en üste Ekle'yi seçin](./media/agile-development/3-sprint-team-add-work.png)
   
1. Biriktirme **Listesi** listesinden yeni Özelliği seçin ve açın. Açıklamayı doldurun, bir ekip üyesi atayın ve planlama parametrelerini ayarlayın. 
   
   **Geliştirme** bölümü altındaki **Ekle bağlantısını** seçerek Özelliği projenin Azure Deposu kodu deposuna da bağlayabilirsiniz. 
   
   Özelliği ni yaptıktan sonra **Kaydet & Kapat'ı**seçin.
   
   ![Özelliği Edet ve Kapat'& Kaydet'i seçin](./media/agile-development/3a-add-link-repo.png)

## <a name="add-a-user-story-to-the-feature"></a><a name='AddStoryunderfeature-4'></a>Özelliğe Kullanıcı Hikayesi Ekleme 

Özellik altında, projeyi tamamlamak için gereken önemli adımları açıklamak için Kullanıcı Hikayeleri ekleyebilirsiniz. 

Bir Özelliğe yeni bir Kullanıcı Hikayesi eklemek için:

1. Biriktirme **Listesi** sekmesinde, **+** Özelliğin solundaki bölümü seçin. 
   
   ![Özellik altında yeni bir Kullanıcı Hikayesi ekleme](./media/agile-development/4-sprint-add-story.png)
   
1. Kullanıcı Hikayesi'ne bir başlık verin ve atama, durum, açıklama, yorumlar, planlama ve öncelik gibi ayrıntıları edin. 
   
   **Ayrıca, Geliştirme** bölümü altındaki **Ekle bağlantısını** seçerek Kullanıcı Hikayesini projenin Azure Deposu kodu deposunun bir dalına bağlayabilirsiniz. İş öğesini bağlamak istediğiniz depoyu ve dalı seçin ve ardından **Tamam'ı**seçin.
   
   ![Bağlantı ekle](./media/agile-development/5-sprint-edit-story.png)
   
1. Kullanıcı Hikayesini düzenlemeyi bitirdikten sonra **Kaydet & Kapat'ı**seçin. 

## <a name="add-a-task-to-a-user-story"></a><a name='AddTaskunderstory-5'></a>Kullanıcı Hikayesine Görev Ekleme 

Görevler, her Kullanıcı Hikayesini tamamlamak için gereken belirli ayrıntılı adımlardır. Bir Kullanıcı Hikayesi'nin tüm Görevleri tamamlandıktan sonra, Kullanıcı Hikayesi de tamamlanmalıdır. 

Bir Kullanıcı Hikayesine Görev eklemek **+** için, Kullanıcı Hikayesi öğesinin yanındakini seçin ve **Görev'i**seçin. Görev'deki başlığı ve diğer bilgileri doldurun.

![Kullanıcı Hikayesine Görev Ekleme](./media/agile-development/7-sprint-add-task.png)

Özellikler, Kullanıcı Hikayeleri ve Görevler oluşturduktan sonra, durumlarını izlemek için bunları **Biriktirme Listesi** veya **Panolar** görünümlerinde görüntüleyebilirsiniz.

![Biriktirme listesi görünümü](./media/agile-development/8-sprint-backlog-view.png)

![Panolar görünümü](./media/agile-development/8a-sprint-board-view.png)

## <a name="use-an-agile-tdsp-work-template"></a><a name='set-up-agile-dsp-6'></a>Çevik bir TDSP çalışma şablonu kullanma

Veri bilimciler, Özellikler, Kullanıcı Hikayeleri ve Görevler'in yerine TDSP yaşam döngüsü aşamaları ve alt aşamalarla geçen çevik bir şablon u kullanarak kendilerini daha rahat hissedebilirler. Azure Panoları'nda, iş öğeleri oluşturmak ve izlemek için TDSP yaşam döngüsü aşamalarını kullanan çevik türetilmiş bir şablon oluşturabilirsiniz. Aşağıdaki adımlar, veri bilimine özgü çevik bir işlem şablonu oluşturma ve şablona dayalı veri bilimi çalışma öğeleri oluşturma yoluyla yürür.

### <a name="set-up-an-agile-data-science-process-template"></a>Çevik Veri Bilimi Süreci şablonu ayarlama

1. Azure DevOps kuruluş ana sayfanızdan sol daki gezinmeden **Kuruluş ayarlarını** seçin. 
   
1. Kuruluş **Ayarları'nda** sol navigasyonda, **Panolar**altında **İşlem'i**seçin. 
   
1. Tüm **işlemler** bölmesinde, **Çevik'in**yanındaki **...** seçeneğini seçin ve ardından **devralınan işlemi oluştur'u**seçin.
   
   ![Çevik'ten devralınan işlem oluşturma](./media/agile-development/10-settings.png) 
   
1. Çevik iletişim **kutusundan devralınan işlemi** oluştur'da, *AgileDataScienceProcess*adını girin ve **süreci oluştur'u**seçin.
   
   ![AgileDataScienceProcess işlemi oluşturun](./media/agile-development/11-agileds.png)
   
1. **Tüm süreçlerde,** yeni **AgileDataScienceProcess**seçin. 
   
1. İş **öğesi türleri** sekmesinde, **Epik,** **Özellik,** **Kullanıcı Hikayesi**ve **Görev'i** her öğenin yanında **...** seçerek devre dışı bırakıp Devre **Dışı Atmayı**seçin. 
   
   ![İş öğesi türlerini devre dışı](./media/agile-development/12-disable.png)
   
1. **Tüm işlemlerde**Biriktirme **Listesi düzeyleri** sekmesini seçin. **Portföy biriktirme listelerinde,** **Epic 'in yanındaki** **...** seçeneğini seçin ve ardından **Edit/Rename'yi**seçin. 
   
1. Biriktirme listesi düzeyi iletişim **kutusunda:**
   1. **Adı**altında, *TDSP Projeleri*ile **Epic** değiştirin. 
   1. **Bu biriktirme listesi düzeyinde Kisi madde türleri**altında, Yeni iş öğesi **türünü**seçin, *TDSP Project'i*girin ve **Ekle'yi**seçin. 
   1. **Varsayılan iş öğesi türü**altında, açılır ve **TDSP Project'i**seçin. 
   1. **Kaydet'i**seçin.
   
   ![Portföy biriktirme listesi düzeyini ayarlama](./media/agile-development/13-rename.png)  
   
1. **Özellikleri** *TDSP Aşamalarına*yeniden adlandırmak için aynı adımları izleyin ve aşağıdaki yeni iş öğesi türlerini ekleyin:
   
   - *İş Anlayışı*
   - *Veri Toplama*
   - *Modelleme*
   - *Dağıtım*
   
1. **Gereksinim biriktirme listesi**altında, **Hikayeleri** *TDSP Alt Aşamalarına*yeniden adlandırın, yeni iş öğesi türü *TDSP Alt Aşama'yı*ekleyin ve varsayılan iş öğesi türünü **TDSP Alt Aşaması'na**ayarlayın.
   
1. **Yineleme biriktirme listesinin**altında, yeni bir iş öğesi türü *TDSP Görevi*ekleyin ve varsayılan iş öğesi türü olarak ayarlayın. 
   
Adımları tamamladıktan sonra biriktirme listesi düzeyleri aşağıdaki gibi görünmelidir:
   
 ![TDSP şablon biriktirme listesi düzeyleri](./media/agile-development/14-template.png)  

### <a name="create-agile-data-science-process-work-items"></a>Çevik Veri Bilimi Süreci iş öğeleri oluşturma

TDSP projeleri oluşturmak ve TDSP yaşam döngüsü aşamalarına karşılık gelen iş öğelerini izlemek için veri bilimi süreci şablonunu kullanabilirsiniz.

1. Azure DevOps kuruluş ana sayfanızdan **Yeni proje'yi**seçin. 
   
1. Yeni **proje oluştur** iletişim kutusunda, projenize bir ad verin ve ardından **Gelişmiş'i**seçin. 
   
1. **Çalışma öğesi işlemi**altında, aşağı bırakın ve **AgileDataScienceProcess**seçin ve sonra **Oluştur'u**seçin.
   
   ![TDSP projesi oluşturma](./media/agile-development/15-newproject.png)
   
1. Yeni oluşturulan projede, sol gezintide **Boards** > **Biriktirme Listesi'ni** seçin.
   
1. TDSP Projelerini görünür kılmak için Yapıl takım **ayarlarını** simgesini seçin. **Ayarlar** ekranında **TDSP Projeleri** onay kutusunu seçin ve ardından **Kaydet ve kapat'ı**seçin.
   
   ![TDSP Projeleri onay kutusunu seçin](./media/agile-development/16-enabledsprojects1.png)
   
1. Veri bilimine özgü bir TDSP Projesi oluşturmak için üst çubuktaki **TDSP Projeleri'ni** seçin ve ardından **Yeni iş öğesi'ni**seçin. 
   
1. Açılır pencerede, TDSP Project iş öğesine bir ad verin ve **en üste ekle'yi**seçin.
   
   ![Veri bilimi proje iş öğesi oluşturma](./media/agile-development/17-dsworkitems0.png)
   
1. TDSP Projesi'nin altına bir iş **+** öğesi eklemek için projenin yanındaki öğeyi seçin ve ardından oluşturulacak iş öğesi türünü seçin. 
   
   ![Veri bilimi iş öğesi türünü seçin](./media/agile-development/17-dsworkitems1.png)
   
1. Yeni iş öğesindeki ayrıntıları doldurun ve **& Kapat'ı kaydet'i**seçin.
   
1. Yeni TDSP Aşamaları, Alt Aşamalar ve Görevler eklemek için iş öğelerinin **+** yanındaki sembolleri seçmeye devam edin. 
   
Veri bilimi proje çalışma öğelerinin **Biriktirme Listesi** görünümünde nasıl görünmesi gerektiğine bir örnek aşağıda verilmiştir:

![18](./media/agile-development/18-workitems1.png)


## <a name="next-steps"></a>Sonraki adımlar

Git ile işbirliği ne kadar [işbirlikli kodlama,](collaborative-coding-with-git.md) paylaşılan kod geliştirme çerçevesi olarak Git'i kullanarak veri bilimi projeleri için nasıl işbirliğine dayalı kod geliştirme yapılacağını ve bu kodlama etkinliklerinin çevik süreçle planlanan işe nasıl bağlanacağını açıklar.

[Örnek walkthroughs](walkthroughs.md) bağlantıları ve küçük resim açıklamaları ile belirli senaryoların gözden geçirme listelerini listeler. Bağlantılı senaryolar, bulut ve şirket içi araçları ve hizmetleri akıllı uygulamalar oluşturmak için iş akışları veya ardışık işlerde nasıl birleştirilen gösteriş.
  
Çevik işlemlerle ilgili ek kaynaklar:

- [Çevik süreç](/azure/devops/boards/work-items/guidance/agile-process)
  
- [Çevik proses iş öğesi türleri ve iş akışı](/azure/devops/boards/work-items/guidance/agile-process-workflow)


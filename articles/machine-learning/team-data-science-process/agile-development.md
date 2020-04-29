---
title: Veri bilimi projelerinin çevik geliştirmesi-Team Data Science süreci
description: Ekip veri bilimi Işlemini kullanarak bir proje ekibinin içindeki bir sistematik, sürüm denetimli ve işbirliğine dayalı bir şekilde bir veri bilimi projesi yürütün.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76722110"
---
# <a name="agile-development-of-data-science-projects"></a>Veri bilimi projelerinin çevik geliştirmesi

Bu belgede, geliştiricilerin bir veri bilimi projesini, [ekip veri bilimi işlemini](overview.md) (TDSP) kullanarak bir proje ekibi içinde bir sistematik, sürüm denetimli ve işbirliğine dayalı bir şekilde nasıl yürütebileceği açıklanmaktadır. TDSP, bulut tabanlı, tahmine dayalı analiz çözümlerini verimli bir şekilde yürütmek için yapılandırılmış bir etkinlik dizisi sağlayan Microsoft tarafından geliştirilen bir çerçevedir. TDSP üzerinde standartlaştırarak bir veri bilimi ekibi tarafından işlenen rol ve görevlerin bir özeti için bkz. [Team Data Science işlem rolleri ve görevleri](roles-tasks.md). 

Bu makalede şunlar hakkında yönergeler yer almaktadır: 

- Bir projede yer alan iş öğeleri için *Sprint planlaması* yapın.
- Sprint 'lere *iş öğeleri* ekleyin.
- Özel olarak TDSP yaşam döngüsü aşamaları ile hizalanan *çevik türetilmiş bir iş öğesi şablonu* oluşturun ve kullanın.

Aşağıdaki yönergeler, Azure DevOps 'da Azure Boards ve Azure Repos kullanarak bir TDSP ekip ortamı ayarlamak için gereken adımları özetler. Yönergeler, Microsoft 'ta TDSP 'nin uygulanması nedeniyle Azure DevOps kullanır. Grubunuz farklı bir kod barındırma platformu kullanıyorsa, ekip sağlama görevleri genellikle değişmez, ancak görevleri tamamlamaya yönelik yol farklıdır. Örneğin, bir iş öğesini git dalına bağlamak, Azure Repos ile olduğu için GitHub ile aynı olmayabilir.

Aşağıdaki şekilde, bir veri bilimi projesi için tipik bir sprint planlama, kodlama ve kaynak denetimi iş akışı gösterilmektedir:

![Team Data Science Process](./media/agile-development/1-project-execute.png)

##  <a name="work-item-types"></a><a name='Terminology-1'></a>İş öğesi türleri

TDSP sprint planlama çerçevesinde, sık kullanılan dört *iş öğesi* türü vardır: *Özellikler*, *Kullanıcı hikayeleri*, *Görevler*ve *hatalar*. Tüm iş öğeleri için biriktirme listesi, git deposu düzeyi değil, proje düzeyidir. 

İşte iş öğesi türleri için tanımlar:

- **Özellik**: bir özellik proje katılımını karşılık gelir. Bir istemciye sahip farklı görevlendirmeler farklı özelliklerdir ve projenin farklı aşamalarını farklı özellikler olarak düşünmek en iyisidir. Özelliklerinizi adlandırmak için * \<clientname>-\<EngagementName>* gibi bir şema seçerseniz, proje bağlamını kolayca tanıyabilir ve adların kendilerini kolayca ayırt edebilirsiniz.
  
- **Kullanıcı hikayesi**: Kullanıcı hikayeleri, bir özelliği uçtan uca tamamlayabilmeniz için gereken iş öğeleridir. Kullanıcı hikayeleri örnekleri şunları içerir:
  - Verileri alma 
  - Verileri inceleme 
  - Özellik Oluştur
  - Modelleri oluşturma
  - Modelleri kullanıma hazır hale getirin 
  - Modelleri yeniden eğitme
  
- **Görev**: görevler, belirli bir kullanıcı hikayesini tamamlamaya yönelik yapılması gereken atanabilir iş öğeleridir. Örneğin, Kullanıcı hikayesinden *veri al* görevleri şu şekilde olabilir:
  - SQL Server kimlik bilgilerini al
  - SQL veri ambarı 'na veri yükleme
  
- **Hata**: hatalar, var olan koddaki veya bir görevi tamamlaması için düzeltilmesi gereken belgelerdeki sorunlardır. Hatalar eksik iş öğelerinden kaynaklanıyorsa, bu kişiler Kullanıcı hikayeleri veya görevleri için de bu Işlemleri yapabilir. 

Veri bilimcileri, özellikleri, kullanıcı hikayelerini ve Işleri TDSP yaşam döngüsü aşamaları ve alt aşamalarıyla değiştiren çevik bir şablonu kullanarak daha rahat bir fikir verebilir. TDSP yaşam döngüsü aşamalarıyla özel olarak hizalanan çevik türetilmiş bir şablon oluşturmak için bkz. [çevık TDSP iş şablonu kullanma](#set-up-agile-dsp-6).

> [!NOTE]
> TDSP, yazılım kod yönetimi 'nden (SCM) özellikler, Kullanıcı hikayeleri, görevler ve hatalar kavramlarını sıralar. TDSP kavramları, geleneksel SCM tanımlarından biraz farklı görünebilir.

## <a name="plan-sprints"></a><a name='SprintPlanning-2'></a>Sprint planlaması

Birçok veri bilimcileri, tamamlanması ve farklı bir şekilde ilerlemek için ay sürebilen birden çok projeyle sahip olur. Sprint planlama, proje önceliği belirleme ve kaynak planlama ve ayırma için faydalıdır. Azure Boards, projeleriniz için iş öğelerini kolayca oluşturabilir, yönetebilir ve izleyebilir ve projelerin beklendiği gibi ilerlemelerini sağlamak için Sprint planlaması gerçekleştirebilirsiniz.

Sprint planlama hakkında daha fazla bilgi için bkz. [Scrum Sprint](https://en.wikipedia.org/wiki/Scrum_(software_development)#Sprint)'leri. 

Azure Boards sprint planlama hakkında daha fazla bilgi için bkz. [bir sprint 'e biriktirme listesi öğeleri atama](/azure/devops/boards/sprints/assign-work-sprint). 

## <a name="add-a-feature-to-the-backlog"></a><a name='AddFeature-3'></a>Biriktirme listesine özellik ekleme 

Projeniz ve proje kod deponuz oluşturulduktan sonra, projenize yönelik işi göstermek için biriktirme listesine bir özellik ekleyebilirsiniz.

1. Proje sayfanızda, sol gezinti bölmesinde **panolar** > **biriktirme** listeleri ' ni seçin. 
   
1. **Kapsam** sekmesinde, üstteki çubukta iş öğesi türü **hikayeler**Ise, açılan ve **Özellikler**' i seçin. Ardından **Yeni Iş öğesi** ' ni seçin.
   
   ![Yeni Iş Öğesi Seç](./media/agile-development/2-sprint-team-overview.png)
   
1. Özellik için genellikle proje adınız olarak bir başlık girin ve ardından **üste Ekle**' yi seçin. 
   
   ![Bir başlık girin ve en üste Ekle ' yi seçin](./media/agile-development/3-sprint-team-add-work.png)
   
1. **Kapsam** listesinden yeni özelliği seçin ve açın. Açıklamayı girin, bir takım üyesi atayın ve planlama parametrelerini ayarlayın. 
   
   Ayrıca, **geliştirme** bölümünün altında **bağlantı ekle** ' ye tıklayarak özelliği projenin Azure Repos kod deposuna bağlayabilirsiniz. 
   
   Özelliği düzenledikten sonra **kaydet & kapat**' ı seçin.
   
   ![Özelliği Düzenle ve Kaydet & kapat 'ı seçin](./media/agile-development/3a-add-link-repo.png)

## <a name="add-a-user-story-to-the-feature"></a><a name='AddStoryunderfeature-4'></a>Özelliğe bir kullanıcı hikayesi ekleyin 

Özelliği altında, projenin tamamlanabilmesi için gereken önemli adımları anlatan Kullanıcı hikayeleri ekleyebilirsiniz. 

Bir özelliğe yeni bir kullanıcı hikayesi eklemek için:

1. **Kapsam** sekmesinde, özelliğin solundaki ' ı **+** seçin. 
   
   ![Özelliğin altına yeni bir kullanıcı hikayesi ekleyin](./media/agile-development/4-sprint-add-story.png)
   
1. Kullanıcı hikayesine bir başlık verin ve atama, durum, açıklama, Yorumlar, planlama ve öncelik gibi ayrıntıları düzenleyin. 
   
   Ayrıca, **geliştirme** bölümünün altında **bağlantı ekle** ' yi seçerek, Kullanıcı hikayesini projenin Azure Repos kod deposunun bir dalına bağlayabilirsiniz. İş öğesini bağlamak istediğiniz depoyu ve dalı seçin ve ardından **Tamam**' ı seçin.
   
   ![Bağlantı ekle](./media/agile-development/5-sprint-edit-story.png)
   
1. Kullanıcı hikayesini düzenledikten sonra **kaydet & kapat**' ı seçin. 

## <a name="add-a-task-to-a-user-story"></a><a name='AddTaskunderstory-5'></a>Kullanıcı hikayesine görev ekleme 

Görevler, her kullanıcı hikayesini tamamlaması için gereken belirli ayrıntılı adımlardır. Kullanıcı hikayesinin tüm görevleri tamamlandıktan sonra, Kullanıcı hikayesi de tamamlanmalıdır. 

Bir kullanıcı hikayesine bir görev eklemek için, Kullanıcı hikayesi öğesinin **+** yanındaki ' ı seçin ve **görev**' i seçin. Başlığı ve görevdeki diğer bilgileri girin.

![Kullanıcı hikayesine görev ekleme](./media/agile-development/7-sprint-add-task.png)

Özellikler, Kullanıcı hikayeleri ve görevler oluşturduktan sonra, durumlarını izlemek için bunları **biriktirme** listeleri veya **panolar** görünümlerinde görüntüleyebilirsiniz.

![Biriktirme listeleri görünümü](./media/agile-development/8-sprint-backlog-view.png)

![Panolar görünümü](./media/agile-development/8a-sprint-board-view.png)

## <a name="use-an-agile-tdsp-work-template"></a><a name='set-up-agile-dsp-6'></a>Çevik TDSP iş şablonu kullanma

Veri bilimcileri, özellikleri, kullanıcı hikayelerini ve Işleri TDSP yaşam döngüsü aşamaları ve alt aşamalarıyla değiştiren çevik bir şablonu kullanarak daha rahat bir fikir verebilir. Azure Boards, iş öğeleri oluşturmak ve izlemek için TDSP yaşam döngüsü aşamalarını kullanan çevik türetilmiş bir şablon oluşturabilirsiniz. Aşağıdaki adımlarda, veri bilimi özgü çevik işlem şablonu ayarlama ve şablona göre veri bilimi iş öğeleri oluşturma işlemleri gösterilmektedir.

### <a name="set-up-an-agile-data-science-process-template"></a>Çevik bir veri bilimi Işlem şablonu ayarlama

1. Azure DevOps kuruluş ana sayfasından sol gezinti bölmesinde **kuruluş ayarları** ' nı seçin. 
   
1. **Kuruluş ayarları** sol gezinti bölmesinde, **panolar**altında **işlem**' i seçin. 
   
1. **Tüm işlemler** bölmesinde, **çevik**' ın yanındaki **...** öğesini seçin ve ardından **devralınan işlem oluştur**' u seçin.
   
   ![Çevik 'ten devralınan işlem oluşturma](./media/agile-development/10-settings.png) 
   
1. **Çevik 'dan devralınan Işlem oluştur** iletişim kutusunda, *Agiledatabilimenceprocess*adlı adı girin ve **işlem oluştur**' u seçin.
   
   ![Agiledatabilimenceprocess işlemi oluşturma](./media/agile-development/11-agileds.png)
   
1. **Tüm işlemler**' de, yeni **Agiledatabilimenceprocess**' i seçin. 
   
1. **İş öğesi türleri** sekmesinde, her bir öğenin yanındaki **...** ' i seçip **devre dışı bırak**' ı seçerek **Epic**, **özelliği**, **Kullanıcı hikayesini**ve **görevi** devre dışı bırakın. 
   
   ![İş öğesi türlerini devre dışı bırak](./media/agile-development/12-disable.png)
   
1. **Tüm Işlemlerde** **kapsam düzeyleri** sekmesini seçin. **Portföy biriktirme listeleri**altında **Epic (devre dışı)** seçeneğinin yanındaki **...** ' ı seçin ve ardından **Düzenle/yeniden adlandır**' ı seçin. 
   
1. **Kapsam düzeyini Düzenle** iletişim kutusunda:
   1. **Ad**' ın altında, **Epic** ile *TDSP projelerini*değiştirin. 
   1. **Bu kapsam düzeyindeki iş öğesi türleri**altında **yeni iş öğesi türü**' nü seçin, *TDSP projesi*girin ve **Ekle**' yi seçin. 
   1. **Varsayılan iş öğesi türü**altında, açılır ve **TDSP projesi**' ni seçin. 
   1. **Kaydet**’i seçin.
   
   ![Portföy biriktirme listesi düzeyini ayarla](./media/agile-development/13-rename.png)  
   
1. **Özellikleri** *TDSP aşamalarına*yeniden adlandırmak için aynı adımları izleyin ve aşağıdaki yeni iş öğesi türlerini ekleyin:
   
   - *İş anlama*
   - *Veri alımı*
   - *Modelleme*
   - *Dağıtım*
   
1. **Gereksinim biriktirme listesi**altında, **hikayeleri** *TDSP alt aşamalarına*yeniden adlandırın, yeni Iş öğesi türü *TDSP alt aşamasını*ekleyin ve varsayılan iş öğesi türünü **TDSP alt aşamasına**ayarlayın.
   
1. **Yineleme biriktirme listesi**altında, yeni bir iş öğesi türü *TDSP görevi*ekleyin ve varsayılan iş öğesi türü olarak ayarlayın. 
   
Adımları tamamladıktan sonra, kapsam düzeyleri şöyle görünmelidir:
   
 ![TDSP şablonu kapsam düzeyleri](./media/agile-development/14-template.png)  

### <a name="create-agile-data-science-process-work-items"></a>Çevik veri bilimi oluşturma iş öğelerini Işleme

TDSP projelerini oluşturmak ve TDSP yaşam döngüsü aşamasına karşılık gelen iş öğelerini izlemek için veri bilimi işlem şablonunu kullanabilirsiniz.

1. Azure DevOps kuruluş ana sayfasından **Yeni proje**' yi seçin. 
   
1. **Yeni proje oluştur** iletişim kutusunda projenize bir ad verin ve **Gelişmiş**' i seçin. 
   
1. **Çalışma öğesi işlemi**altında, açılır ve **Agiledatabilimenceprocess**' i seçin ve ardından **Oluştur**' u seçin.
   
   ![TDSP projesi oluşturma](./media/agile-development/15-newproject.png)
   
1. Yeni oluşturulan projede, sol gezinti bölmesinde **panolar** > **biriktirme** listeleri ' ni seçin.
   
1. TDSP projelerini görünür yapmak için **Ekip ayarlarını yapılandır** simgesini seçin. **Ayarlar** ekranında, **TDSP projeleri** onay kutusunu seçin ve ardından **Kaydet ve Kapat**' ı seçin.
   
   ![TDSP projelerini seç onay kutusu](./media/agile-development/16-enabledsprojects1.png)
   
1. Veri bilimi 'e özgü bir TDSP projesi oluşturmak için üstteki çubukta **TDSP projeleri** ' ni seçin ve ardından **yeni iş öğesi**' ni seçin. 
   
1. Açılan pencerede TDSP proje iş öğesine bir ad verin ve **en üste Ekle**' yi seçin.
   
   ![Veri bilimi projesi iş öğesi oluştur](./media/agile-development/17-dsworkitems0.png)
   
1. TDSP projesinin altına bir iş öğesi eklemek için projenin **+** yanındaki öğesini seçin ve ardından oluşturulacak iş öğesi türünü seçin. 
   
   ![Veri bilimi iş öğesi türünü seçin](./media/agile-development/17-dsworkitems1.png)
   
1. Yeni iş öğesindeki ayrıntıları doldurup **kaydet & kapat**' ı seçin.
   
1. Yeni TDSP aşamaları **+** , alt aşamalar ve görevler eklemek için iş öğelerinin yanındaki sembolleri seçerken devam edin. 
   
Veri bilimi proje iş öğelerinin **biriktirme** listesi görünümünde nasıl görünmeli bir örnek aşağıda verilmiştir:

![18](./media/agile-development/18-workitems1.png)


## <a name="next-steps"></a>Sonraki adımlar

[Git Ile Işbirliğine dayalı kodlama](collaborative-coding-with-git.md) , paylaşılan kod geliştirme çerçevesi olarak git kullanan veri bilimi projeleri için işbirliğine dayalı kod geliştirmenin nasıl yapılacağını ve bu kodlama etkinliklerinin çevik işlemle planlanmış iş 'e nasıl bağlanacağını açıklar.

[Örnek izlenecek yollar](walkthroughs.md) , bağlantılar ve küçük resim açıklamalarıyla belirli senaryolara yönelik izlenecek yolları listeler. Bağlantılı senaryolar, akıllı uygulamalar oluşturmak için bulut ve şirket içi araçların ve hizmetlerin iş akışları veya işlem hatları halinde nasıl birleştirileceğini gösterir.
  
Çevik işlemlerde ek kaynaklar:

- [Çevik işlem](/azure/devops/boards/work-items/guidance/agile-process)
  
- [Çevik işlem iş öğesi türleri ve iş akışı](/azure/devops/boards/work-items/guidance/agile-process-workflow)


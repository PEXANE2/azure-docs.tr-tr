---
title: Git ile İşbirlikçi kodlama - Ekip Veri Bilimi Süreci
description: Çevik planlama ile Git kullanarak veri bilimi projeleri için işbirlikçi kod geliştirme nasıl.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 0708e395eff90ff5b889c05f0fd5e7a98205c5bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721906"
---
# <a name="collaborative-coding-with-git"></a>Git ile işbirliği içinde kodlama

Bu makalede, veri bilimi projeleri için ortak kod geliştirme çerçevesi olarak Git nasıl kullanılacağı açıklanmaktadır. Makale, Azure Repos'taki kodun Azure Panolarındaki [çevik geliştirme](agile-development.md) iş öğelerine nasıl bağlanılmayı, kod incelemeleri nasıl yapılacağını ve değişiklik için çekme isteklerinin nasıl oluşturulup birleştirilen nasıl birleştirilebildiğini kapsar.

## <a name="link-a-work-item-to-an-azure-repos-branch"></a><a name='Linkaworkitemwithagitbranch-1'></a>İş öğesini Azure Repos dalına bağlama 

Azure DevOps, bir Azure Boards Kullanıcı Hikayesi veya Görev iş öğesini Azure Repos Git deposu şubesine bağlamak için kullanışlı bir yol sağlar. Kullanıcı Hikayenizi veya Görevinizi doğrudan onunla ilişkili koda bağlayabilirsiniz. 

Bir iş öğesini yeni bir dala bağlamak için, iş öğesinin yanındaki **Eylemler** elipsisi **(...**) öğesini ve bağlam menüsünde **Yeni dala**kaydırın ve seçin.  

![1](./media/collaborative-coding-with-git/1-sprint-board-view.png)

Dal **oluştur** iletişim kutusunda, yeni şube adını ve temel Azure Repos Git deposunu ve dalını sağlayın. Temel depo, iş öğesiyle aynı Azure DevOps projesinde olmalıdır. Temel dal ana dal veya varolan başka bir dal olabilir. **Oluştur dalLarını**seçin. 

![2](./media/collaborative-coding-with-git/2-create-a-branch.png)

Windows veya Linux'ta aşağıdaki Git bash komutunu kullanarak yeni bir dal da oluşturabilirsiniz:

```bash
git checkout -b <new branch name> <base branch name>

```
\<Bir temel dal adı> belirtmiyorsanız, yeni dal `master`. 

Çalışma dalınıza geçmek için aşağıdaki komutu çalıştırın: 

```bash
git checkout <working branch name>
```

Çalışma dalına geçtikten sonra, iş öğesini tamamlamak için kod veya belge yapıları geliştirmeye başlayabilirsiniz. Koşmak `git checkout master` sizi şubeye `master` geri döndürecek.

Her Kullanıcı Hikayesi iş öğesi için bir Git dalı oluşturmak iyi bir uygulamadır. Ardından, her Görev iş öğesi için Kullanıcı Hikayesi dalını temel alan bir dal oluşturabilirsiniz. Aynı proje için farklı Kullanıcı Hikayeleri üzerinde veya aynı Kullanıcı Hikayesi için farklı Görevlerde çalışan birden çok kişi varsa, dalları Kullanıcı Hikayesi-Görev ilişkisine karşılık gelen bir hiyerarşiiçinde düzenleyin. Her ekip üyesinin farklı bir dalda veya bir dalı paylaşırken farklı kod veya diğer yapılar üzerinde çalışmasını sağlayarak çakışmaları en aza indirebilirsiniz. 

Aşağıdaki diyagram, TDSP için önerilen dallanma stratejisini gösterir. Burada gösterildiği kadar çok dala ihtiyacınız olmayabilir, özellikle bir projede yalnızca bir veya iki kişi çalışıyorsa veya bir Kullanıcı Hikayesi'nin tüm Görevleri'nde yalnızca bir kişi çalışıyorsa. Ancak geliştirme dalını ana daldan ayırmak her zaman iyi bir uygulamadır ve sürüm dalının geliştirme etkinlikleri tarafından kesintiye uğramasını önlemeye yardımcı olabilir. Git dal modelinin tam açıklaması için [Başarılı Git Dallanma Modeli'ne](https://nvie.com/posts/a-successful-git-branching-model/)bakın.

![3](./media/collaborative-coding-with-git/3-git-branches.png)

Ayrıca, bir iş öğesini varolan bir dala da bağlayabilirsiniz. Bir çalışma öğesinin **Ayrıntı** sayfasında **bağlantı ekle'yi**seçin. Ardından iş öğesini bağlamak için varolan bir dalı seçin ve **Tamam'ı**seçin. 

![4](./media/collaborative-coding-with-git/4-link-to-an-existing-branch.png)

## <a name="work-on-the-branch-and-commit-changes"></a><a name='WorkonaBranchandCommittheChanges-2'></a>Dal üzerinde çalışın ve değişiklikler gerçekleştirin 

Yerel makinenizin `script` dalına R komut dosyası dosyası eklemek gibi iş öğeniz için bir değişiklik yaptıktan sonra, aşağıdaki Git bash komutlarını kullanarak yerel şubenizden yukarı akış çalışma dalına değişiklik yapabilirsiniz:

```bash
git status
git add .
git commit -m "added an R script file"
git push origin script
```

![5](./media/collaborative-coding-with-git/5-sprint-push-to-branch.png)

## <a name="create-a-pull-request"></a><a name='CreateapullrequestonVSTS-3'></a>Çekme isteği oluşturma

Bir veya daha fazla taahhüt ve itme den sonra, geçerli çalışma dalınızı temel dalıyla birleştirmeye hazır olduğunuzda, Azure Repos'ta bir *çekme isteği* oluşturabilir ve gönderebilirsiniz. 

Azure DevOps projenizin ana sayfasından, sol navigasyonda Çekme**isteklerini** depolayabakın. **Repos** >  Ardından **Yeni çekme isteği** düğmelerinden birini veya çekme isteği bağlantısı **oluştur'u** seçin.

![6](./media/collaborative-coding-with-git/6-spring-create-pull-request.png)

Yeni **Çekme İsteği** ekranında, gerekirse, değişikliklerinizi birleştirmek istediğiniz Git deposuna ve dalına gidin. İstediğinize başka bilgiler ekleyin veya değiştirin. **Gözden Geçirenler**altında, gözden geçirenlerin adlarını ekleyin ve sonra **Oluştur'u**seçin. 

![7](./media/collaborative-coding-with-git/7-spring-send-pull-request.png)

## <a name="review-and-merge"></a><a name='ReviewandMerge-4'></a>İnceleme ve birleştirme

Çekme isteğini oluşturduğunuzda, gözden geçirenleriniz çekme isteğini gözden geçirmek için bir e-posta bildirimi alır. Gözden geçirenler değişikliklerin çalışıp çalışmadığını sınar ve değişiklikleri mümkünse istekte bulundurucuyla denetler. Gözden geçirenler, değerlendirmelerine dayanarak yorum yapabilir, değişiklik isteyebilir ve çekme isteğini onaylayabilir veya reddedebilir. 

![8](./media/collaborative-coding-with-git/8-add_comments.png)

Gözden geçirenler değişiklikleri onayladıktan sonra, siz veya birleştirme izinleri olan başka biri çalışma dalını temel dalı ile birleştirebilirsiniz. **Tamamla'yı**seçin ve ardından **Komple çekme isteği** iletişim kutusunda birleştirme'yi **tamamla'yı** seçin. Çalışma dalını birleştirdikten sonra silmeyi seçebilirsiniz. 

![10](./media/collaborative-coding-with-git/10-spring-complete-pullrequest.png)

İsteğin **TAMAMLANMIŞ**olarak işaretlendiğini doğrulayın. 

![11](./media/collaborative-coding-with-git/11-spring-merge-pullrequest.png)

Sol navigasyonda **Repos'a** geri döndüğünüzde, `script` şube silindiğinden beri ana dala geçiş yaptığınızı görebilirsiniz.

![12](./media/collaborative-coding-with-git/12-spring-branch-deleted.png)

Çalışma dalını temel dalıile birleştirmek `script` ve birleştirdikten sonra çalışma dalını silmek için aşağıdaki Git bash komutlarını da kullanabilirsiniz:

```bash
git checkout master
git merge script
git branch -d script
```

![13](./media/collaborative-coding-with-git/13-spring-branch-deleted-commandline.png)

## <a name="next-steps"></a>Sonraki adımlar

[Veri bilimi görevlerini yürütmek,](execute-data-science-tasks.md) etkileşimli veri arama, veri analizi, raporlama ve model oluşturma gibi çeşitli yaygın veri bilimi görevlerini tamamlamak için yardımcı programların nasıl kullanılacağını gösterir.

[Örnek walkthroughs](walkthroughs.md) bağlantıları ve küçük resim açıklamaları ile belirli senaryoların gözden geçirme listelerini listeler. Bağlantılı senaryolar, bulut ve şirket içi araçları ve hizmetleri akıllı uygulamalar oluşturmak için iş akışları veya ardışık işlerde nasıl birleştirilen gösteriş. 


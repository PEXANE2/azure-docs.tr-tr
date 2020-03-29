---
title: Ekip Veri Bilimi Sürecinde bireysel bir katılımcının görevleri
description: Bir veri bilimi ekibi projesinde bireysel katılımcı nın görevlerinin ayrıntılı bir şekilde gözden geçirimi.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: d9942c31b63de77196b8b51b88376cb8ef74b990
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721260"
---
# <a name="tasks-for-an-individual-contributor-in-the-team-data-science-process"></a>Ekip Veri Bilimi Sürecinde bireysel bir katılımcının görevleri

Bu konu, tek bir *katılımcının* [Ekip Veri Bilimi Süreci'nde](overview.md) (TDSP) bir proje ayarlamak için tamamladığını n için özetlemektedir. Amaç, TDSP'de standartlaştırılabilen ortak bir ekip ortamında çalışmaktır. TDSP, işbirliği ve ekip öğrenimini geliştirmeye yardımcı olmak için tasarlanmıştır. TDSP'de standartlaştırılan bir veri bilimi ekibi tarafından gerçekleştirilen personel rollerinin ve bunların ilişkili görevlerinin ana hatlarını öğrenmek [için, Takım Veri Bilimi Süreci rolleri ve görevleri bölümüne](roles-tasks.md)bakın.

Aşağıdaki diyagram, bireysel katkıda bulunanların (veri bilimcilerin) ekip ortamlarını ayarlamak için tamamladığını gösteren görevleri gösterir. TDSP altında bir veri bilimi projesinin nasıl yürütüldüne ilişkin talimatlar için [bkz.](project-execution.md) 

![Bireysel katılımcı görevleri](./media/project-ic-tasks/project-ic-1-tdsp-data-scientist.png)

- **ProjectRepository,** proje ekibinizin proje şablonlarını ve varlıklarını paylaşmak için koruduğu depodur.
- **TeamUtilities,** ekibinizin özellikle takımınız için koruduğu yardımcı program deposudur. 
- **GroupUtilities,** grubunuzun tüm grup genelinde yararlı yardımcı programları paylaşmak için koruduğu depodur. 

> [!NOTE] 
> Bu makalede, TDSP ortamı nı ayarlamak için Azure Repos ve Veri Bilimi Sanal Makinesi (DSVM) kullanılır, çünkü Microsoft'ta TDSP böyle uygulanır. Ekibiniz diğer kod barındırma veya geliştirme platformlarını kullanıyorsa, tek tek katkıda bulunan ların görevleri aynıdır, ancak bunları tamamlama nın yolu farklı olabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, aşağıdaki kaynakların ve izinlerin [grup yöneticiniz,](group-manager-tasks.md) [takım müşteri](team-lead-tasks.md)adayınız ve proje [müşteri adayınız](project-lead-tasks.md)tarafından ayarlandığını varsayar:

- Veri bilimi biriminiz için Azure DevOps **organizasyonu**
- Projeniz tarafından ayarlanan proje **deposu,** proje şablonlarını ve varlıklarını paylaşmak için
- Varsa grup yöneticisi ve takım lideri tarafından kurulan **GroupUtilities** ve **TeamUtilities** depoları
- Varsa, ekibiniz veya projeniz için paylaşılan varlıklar için ayarlanan Azure **dosya depolama**
- Klonlama ve proje deponuza geri itme **izinleri** 

Depoları klonlamak ve içeriği yerel makinenizde veya DSVM'de değiştirmek veya Azure dosya depolama alanını DSVM'nize monte etmek için şu denetim listesini göz önünde bulundurmanız gerekir:

- Azure aboneliği.
- Git makinenize yüklendi. DSVM kullanıyorsanız, Git önceden yüklenmiş. Aksi takdirde, [Platformlar ve araçlar ekine](platforms-and-tools.md#appendix)bakın.
- Bir DSVM kullanmak istiyorsanız, Windows veya Linux DSVM oluşturulan ve Azure'da yapılandırılan. Daha fazla bilgi ve talimatlar için [Veri Bilimi Sanal Makine Dokümantasyonu'na](/azure/machine-learning/data-science-virtual-machine/)bakın.
- Windows DSVM için makinenizde [Git Kimlik Bilgisi Yöneticisi (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) yüklenir. *README.md* dosyasında, **İndir ve Yükle** bölümüne gidin ve en son **yükleyiyi**seçin. *.exe* yükleyicisini yükleyici sayfasından indirin ve çalıştırın. 
- Bir Linux DSVM için, DSVM'nizde ayarlanmış ve Azure DevOps'lere eklenen bir SSH ortak anahtarı. Daha fazla bilgi ve talimatlar için Platformlar ve [araçlar ekindeki](platforms-and-tools.md#appendix) **SSH ortak anahtar oluşturma** bölümüne bakın. 
- DSVM'nize takmanız gereken herhangi bir Azure dosya depolama alanı için Azure dosya depolama bilgileri. 

## <a name="clone-repositories"></a>Klon depoları

Yerel olarak depolarla çalışmak ve değişikliklerinizi paylaşılan takım ve proje depolarına itmek için, önce depoları yerel makinenize kopyalar veya *klonlarsınız.* 

1. Azure DevOps'lerde, ekibinizin proje Özeti *sayfasına\//\<https:\<sunucu adı\<>/ kuruluş adı>/ takım adı>*, **örneğin, https:\//dev.azure.com/DataScienceUnit/MyTeam**.
   
1. Sol gezinmede **Depolar'ı** seçin ve sayfanın üst kısmında klonlamak istediğiniz depoyu seçin.
   
1. Repo sayfasında sağ üstteki **Klon'u** seçin.
   
1. Klon **deposu** iletişim kutusunda, HTTP bağlantısı için **HTTPS** veya SSH bağlantısı için **SSH'yi** seçin ve **Komut satırıaltındaki** klon URL'sini panonuza kopyalayın.
   
   ![Klon repo](./media/project-ic-tasks/clone.png)
   
1. Yerel makinenizde veya DSVM'nizde aşağıdaki dizinleri oluşturun:
   
   - Windows için: **C:\GitRepos**
   - Linux için: **$home/GitRepos**
   
1. Oluşturduğunuz dizine değiştirin.
   
1. Git Bash'te, `git clone <clone URL>` klonlamak istediğiniz her depo için komutu çalıştırın. 
   
   Örneğin, aşağıdaki komut **TeamUtilities** deposunu yerel makinenizdeki *MyTeam* dizinine klonlar. 
   
   **HTTPS bağlantısı:**
   
   ```bash
   git clone https://DataScienceUnit@dev.azure.com/DataScienceUnit/MyTeam/_git/TeamUtilities
   ```
   
   **SSH bağlantısı:**
   
   ```bash
   git clone git@ssh.dev.azure.com:v3/DataScienceUnit/MyTeam/TeamUtilities
   ```
   
1. Yerel proje dizininizdeki klonlanmış depoların klasörlerini görebileceğinizi doğrulayın.
   
   ![Üç yerel depo klasörü](./media/project-ic-tasks/project-ic-5-three-repo-cloned-to-ic-linux.png)

## <a name="mount-azure-file-storage-to-your-dsvm"></a>Azure dosya depolamasını DSVM'nize dağıl

Ekibiniz veya projeniz Azure dosya depolamasında varlıkları paylaştıysa, dosya depolama alanını yerel makinenize veya DSVM'ye monte edin. [Yerel makinenizdeki Veya DSVM'deki Azure Dağı dosya depolama adresindeki](team-lead-tasks.md#mount-azure-file-storage-on-your-local-machine-or-dsvm)yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, Ekip Veri Bilimi Süreci tarafından tanımlanan diğer rollerin ve görevlerin ayrıntılı açıklamalarına bağlantılar verilmiştir:

- [Veri bilimi ekibi için Grup Yöneticisi görevleri](group-manager-tasks.md)
- [Veri bilimi ekibi için Takım Müşteri Adayı görevleri](team-lead-tasks.md)
- [Veri bilimi ekibi için Proje Müşteri Adayı görevleri](project-lead-tasks.md)


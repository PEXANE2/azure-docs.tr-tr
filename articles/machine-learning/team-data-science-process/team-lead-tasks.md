---
title: Ekip Veri Bilimi İşlem Ekibi'nde ekip liderliği için görevler
description: Ekip Veri Bilimi Süreci ekibinde bir ekip liderliği için görevlerin ayrıntılı bir şekilde gözden geçirilme
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: d099d7c233c3f4b5e65bfdb7d4b875a0e4098499
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75864290"
---
# <a name="tasks-for-the-team-lead-on-a-team-data-science-process-team"></a>Ekip Veri Bilimi Süreci ekibinde ekip liderliği için görevler

Bu makalede, bir *takım müşteri adayının* veri bilimi ekibi için tamamladığını görevler açıklanır. Ekip liderliğinin amacı, [Team Data Science Process](overview.md) (TDSP) üzerinde standartlaştırılabilen ortak bir ekip ortamı oluşturmaktır. TDSP, işbirliği ve ekip öğrenimini geliştirmeye yardımcı olmak için tasarlanmıştır. 

TDSP, tahmine dayalı analitik çözümleri ve akıllı uygulamalar verimli bir şekilde sunmak için çevik, yinelemeli bir veri bilimi metodolojisidir. İşlem, Microsoft ve endüstrideki en iyi uygulamaları ve yapıları damıtıyor.  Amaç, veri bilimi girişimlerinin başarılı bir şekilde uygulanması ve analiz programlarının faydalarını tam olarak gerçekleştirmektir. TDSP'de standartlaştırılan bir veri bilimi ekibinin personel rollerinin ve ilişkili görevlerinin ana hatlarını öğrenmek [için, Ekip Veri Bilimi Süreci rolleri ve görevleri](roles-tasks.md)bölümüne bakın.

Bir ekip lideri, bir işletmenin veri bilimi biriminde bulunan birkaç veri bilimciden oluşan bir ekibi yönetir. Veri bilimi biriminin boyutuna ve yapısına bağlı olarak, [grup yöneticisi](group-manager-tasks.md) ve takım lideri aynı kişi olabilir veya görevlerini vekillere devredebilirler. Ama görevlerin kendileri değişmez. 

Aşağıdaki diyagram, takım ortamı nı ayarlamak için takım adayının tamamladaçalıştığı görevlerin iş akışını gösterir:

![Takım müşteri adayı görev iş akışı](./media/team-lead-tasks/team-leads-1-creating-teams.png)

1. Azure DevOps'te grubun organizasyonunda bir **ekip projesi** oluşturun. 
  
1. Varsayılan takım deposunu **TeamUtilities**olarak yeniden adlandırın.
  
1. Takım projesinde yeni bir **TeamTemplate** deposu oluşturun. 
  
1. Grubun **GroupUtilities** ve **GroupProjectTemplate** depolarının içeriğini **TeamUtilities** ve **TeamTemplate** depolarına aktarın. 
  
1. Ekip üyeleri ekleyerek ve izinlerini yapılandırarak **güvenlik denetimini** ayarlayın.
  
1. Gerekirse, ekip verileri ve analiz kaynakları oluşturun:
   - **TeamUtilities** deposuna takıma özel yardımcı programlar ekleyin. 
   - Tüm takım için yararlı olabilecek veri varlıklarını depolamak için **Azure dosya depolama** alanı oluşturun. 
   - Azure dosya depolama alanını ekip adayının **Veri Bilimi Sanal Makinesi'ne** (DSVM) monte edin ve buna veri varlıkları ekleyin.

Aşağıdaki öğretici adımları ayrıntılı olarak gözden geçirir.

> [!NOTE] 
> Bu makalede, TDSP takım ortamını ayarlamak için Azure DevOps ve DSVM kullanır, çünkü Microsoft'ta TDSP böyle uygulanır. Ekibiniz diğer kod barındırma veya geliştirme platformlarını kullanıyorsa, takım müşteri adayı görevleri aynıdır, ancak bunları tamamlama nın yolu farklı olabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, aşağıdaki kaynakların ve izinlerin [grup yöneticiniz](group-manager-tasks.md)tarafından ayarlandığını varsayar:

- Veri biriminiz için Azure DevOps **organizasyonu**
- Microsoft TDSP ekibinin **ProjectTemplate** ve **Yardımcı Programlar** depolarının içeriğiyle doldurulan **GroupProjectTemplate** ve **GroupUtilities** depoları
- Ekibiniz için proje ve depo oluşturmanız için kuruluş hesabınızdaki izinler

Depoları klonlayabilmek ve içeriğini yerel makinenizde veya DSVM'nizde değiştirebilmek veya Azure dosya depolama alanını kurup DSVM'nize monte edebilmek için aşağıdakilere ihtiyacınız vardır:

- Azure aboneliği.
- Git makinenize yüklendi. DSVM kullanıyorsanız, Git önceden yüklenmiş. Aksi takdirde, [Platformlar ve araçlar ekine](platforms-and-tools.md#appendix)bakın.
- Bir DSVM kullanmak istiyorsanız, Windows veya Linux DSVM oluşturulan ve Azure'da yapılandırılan. Daha fazla bilgi ve talimatlar için [Veri Bilimi Sanal Makine Dokümantasyonu'na](/azure/machine-learning/data-science-virtual-machine/)bakın.
- Windows DSVM için makinenizde [Git Kimlik Bilgisi Yöneticisi (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) yüklenir. *README.md* dosyasında, **İndir ve Yükle** bölümüne gidin ve en son **yükleyiyi**seçin. *.exe* yükleyicisini yükleyici sayfasından indirin ve çalıştırın. 
- Bir Linux DSVM için, DSVM'nizde ayarlanmış ve Azure DevOps'lere eklenen bir SSH ortak anahtarı. Daha fazla bilgi ve talimatlar için Platformlar ve [araçlar ekindeki](platforms-and-tools.md#appendix) **SSH ortak anahtar oluşturma** bölümüne bakın. 

## <a name="create-a-team-project-and-repositories"></a>Ekip projesi ve depoları oluşturma

Bu bölümde, grubunuzun Azure DevOps organizasyonunda aşağıdaki kaynakları oluşturursunuz:

- Azure DevOps'ta **MyTeam** projesi
- **TeamTemplate** deposu
- **TeamUtilities** deposu

Bu öğreticide depolar ve dizinler için belirtilen adlar, daha büyük veri bilimi kuruluşunuz içinde kendi ekibiniz için ayrı bir proje oluşturmak istediğinizi varsayar. Ancak, tüm grup grup yöneticisi veya kuruluş yöneticisi tarafından oluşturulan tek bir proje altında çalışmayı seçebilir. Daha sonra, tüm veri bilimi ekipleri bu tek proje kapsamında depolar oluşturur. Bu senaryo şu lar için geçerli olabilir:
- Birden fazla veri bilimi ekibi olmayan küçük bir veri bilimi grubu. 
- Yine de grup düzeyinde sprint planlama gibi etkinliklerle ekipler arası işbirliğini optimize etmek isteyen birden fazla veri bilimi ekibine sahip daha büyük bir veri bilimi grubu. 

Takımlar tek bir grup projesi altında takıma özel depolarına sahip olmayı seçerse, takım müşteri adayları * \<TeamName>Şablonu* ve * \<TeamName>Yardımcı Programları*gibi adlara sahip depolar oluşturmalı. Örneğin: *TeamATemplate* ve *TeamAUtilities*. 

Her durumda, takım müşteri adaylarının ekip üyelerinin hangi şablon ve yardımcı program depolarını kurmak ve klonlamak için bildirmeleri gerekir. Proje müşteri adayları, ister ayrı projeler de ister tek bir proje altında olsun, proje depoları oluşturmak [için bir veri bilimi ekibinin proje müşteri adayı görevlerini](project-lead-tasks.md) izlemelidir. 

### <a name="create-the-myteam-project"></a>MyTeam projesini oluşturma

Ekibiniz için ayrı bir proje oluşturmak için:

1. Web tarayıcınızda, URL https adresinden grubunuzun Azure DevOps kuruluş ana sayfasına *gidin:\//\<sunucu adı>/\<kuruluş adı>* ve Yeni **projeyi**seçin. 
   
   ![Yeni proje seçin](./media/team-lead-tasks/team-leads-2-create-new-team.png)
   
1. Proje **Oluştur** iletişim kutusunda, *MyTeam*gibi takım **adınızı, Project adı**altında girin ve sonra **Gelişmiş'i**seçin. 
   
1. **Sürüm denetimi**altında, **Git'i**seçin ve **İş öğesi işlemi**altında **Çevik'i**seçin. Ardından **Oluştur**’u seçin. 
   
   ![Proje oluşturma](./media/team-lead-tasks/team-leads-3-create-new-team-2.png)
   
Ekip projesi **Özet** sayfası açılır, sayfa *URL'si https:\//\<sunucu adı>/\<kuruluş adı>/\<takım adı>. *

### <a name="rename-the-myteam-default-repository-to-teamutilities"></a>MyTeam varsayılan deposunu TeamUtilities olarak yeniden adlandırın

1. **MyTeam** proje **Özeti** sayfasında, **hangi hizmetle başlamak istersiniz?** **Repos** 
   
   ![Repos'u Seçin](./media/team-lead-tasks/team-leads-6-rename-team-project-repo.png)
   
1. **MyTeam** repo sayfasında, sayfanın üst kısmındaki **MyTeam** deposunu seçin ve ardından açılan sayfadan **depoları yönet'i** seçin. 
   
   ![Depoları Yönet'i seçin](./media/team-lead-tasks/team-leads-7-rename-team-project-repo-2.png)
1. Proje **Ayarları** sayfasında, **MyTeam** deposunun yanındaki **...** seçeneğini seçin ve ardından **Rename deposunu**seçin. 
   
   ![Rename deposunu seçin](./media/team-lead-tasks/team-leads-8-rename-team-project-repo-3.png)
   
1. **MyTeam deposunun** açılır penceresini yeniden adlandırın, *TeamUtilities'i*girin ve ardından **Yeniden Adlandır'ı**seçin. 

### <a name="create-the-teamtemplate-repository"></a>TeamTemplate deposunu oluşturma

1. Proje **Ayarları** sayfasında **Yeni depoyu seçin.** 
   
   ![Yeni depo seçin](./media/team-lead-tasks/team-leads-9-create-team-utilities.png)
   
   Veya **MyTeam** proje **Özeti** sayfasının sol daki gezintisinden **Repos'u** seçin, sayfanın üst kısmında bir depo seçin ve ardından açılır noktadan **Yeni depoyu** seçin.
   
1. Yeni **bir depo oluştur** iletişim kutusunda, **Git'in Türü**altında seçildiğinden emin olun. **Git** *TeamTemplate'i* **Depo adı**altında girin ve ardından **Oluştur'u**seçin.
   
   ![Depo oluşturma](./media/team-lead-tasks/team-leads-10-create-team-utilities-2.png)
   
1. Proje ayarları sayfanızda **teamutilities** ve **TeamTemplate** olmak için iki depoyu görebileceğinizi doğrulayın. 
   
   ![İki takım deposu](./media/team-lead-tasks/team-leads-11-two-repo-in-team.png)

### <a name="import-the-contents-of-the-group-common-repositories"></a>Grup ortak depolarının içeriğini alma

Takım depolarınızı grup yöneticiniz tarafından ayarlanan grup ortak depolarının içeriğiyle doldurmak için:

1. **MyTeam** proje giriş sayfanızdan sol navigasyonda **Repos'u** seçin. **MyTeam** şablonunun bulunmadığına dair bir ileti alırsanız, Aksi takdirde bağlantıyı seçin **ve varsayılan TeamTemplate deponuza gidin.** 
   
   Varsayılan **TeamTemplate** deposu açılır. 
   
1. **TeamTemplate'te boş sayfa,** **İçe Aktar'ı**seçin. 
   
   ![Alma'yı seçin](./media/team-lead-tasks/import-repo.png)
   
1. Git **deposu** iletişim kutusunda, **Kaynak türü**olarak **Git'i** seçin ve Grup ortak şablon deponuzun URL'sini **Klon URL'si**altında girin. URL *https:\//\<sunucu adı\<> / kuruluş\<adı>/_git/ depo adı>. * Örneğin: *https:\//dev.azure.com/DataScienceUnit/GroupCommon/_git/GroupProjectTemplate*. 
   
1. **İçeri aktar**'ı seçin. Grup şablonu deponuzun içeriği takım şablonu deponuza aktarılır. 
   
   ![Grup ortak şablon deposunu içe aktarma](./media/team-lead-tasks/import-repo-2.png)
   
1. Projenizin **Depolar** sayfasının üst kısmında, aşağı inin ve **TeamUtilities** deposunu seçin.
   
1. Grup ortak yardımcı programları deposunun içeriğini (örneğin *GroupUtilities)* **TeamUtilities** deponuza almak için alma işlemini yineleyin. 
   
İki takım deponuzun her biri artık ilgili grup ortak deposundaki dosyaları içerir. 

### <a name="customize-the-contents-of-the-team-repositories"></a>Takım depolarının içeriğini özelleştirme

Takımınızın özel gereksinimlerini karşılayacak şekilde takım depolarınızın içeriğini özelleştirmek istiyorsanız, bunu şimdi yapabilirsiniz. Dosyaları değiştirebilir, dizin yapısını değiştirebilir veya dosya ve klasörler ekleyebilirsiniz.

Doğrudan Azure DevOps'ta dosya veya klasörleri değiştirmek, yüklemek veya oluşturmak için:

1. **MyTeam** proje **Özeti** sayfasında **Depoları**seçin. 
   
1. Sayfanın üst kısmında, özelleştirmek istediğiniz depoyu seçin.

1. Repo dizini yapısında, değiştirmek istediğiniz klasöre veya dosyaya gidin. 
   
   - Yeni klasörler veya dosyalar oluşturmak için **Yeni'nin**yanındaki oku seçin. 
     
     ![Yeni dosya oluşturma](./media/team-lead-tasks/new-file.png)
     
   - Dosya yüklemek için **Dosya(lar) dosyasını yükleyin'i**seçin. 
     
     ![Dosyaları karşıya yükleme](./media/team-lead-tasks/upload-files.png)
     
   - Varolan dosyaları yeniden sağlamak için dosyaya gidin ve ardından **Edit'i**seçin. 
     
     ![Dosyayı edin](./media/team-lead-tasks/edit-file.png)
     
1. Dosya ekledikten veya düzenlemeden sonra **Commit'i**seçin.
   
   ![Değişiklikler gerçekleştirme](./media/team-lead-tasks/commit.png)

Yerel makinenizde veya DSVM'nizdeki depolarla çalışmak için, önce depoları yerel makinenize kopyalar veya *klonlarsınız* ve sonra değişikliklerinizi paylaşılan takım depolarına bağlayıp itersiniz, 

Depoları klonlamak için:

1. **MyTeam** proje **Özeti** **sayfasında, Depolar'ı**seçin ve sayfanın üst kısmında klonlamak istediğiniz depoyu seçin.
   
1. Repo sayfasında sağ üstteki **Klon'u** seçin.
   
1. Komut **satırı** **altında, Klon deposu** iletişim kutusunda, HTTP bağlantısı için HTTPS **veya** SSH bağlantısı için **HTTPS'yi** seçin ve klon URL'sini panonuza kopyalayın.
   
   ![Klon URL'sini kopyalama](./media/team-lead-tasks/clone.png)
   
1. Yerel makinenizde aşağıdaki dizinleri oluşturun:
   
   - Windows için: **C:\GitRepos\MyTeam**
   - Linux için, **$home/GitRepos/MyTeam** 
   
1. Oluşturduğunuz dizine değiştirin.
   
1. Git Bash'te, `git clone <clone URL>` \<klon URL'sinin> **Klon** iletişim kutusundan kopyaladığınız URL olduğu komutu çalıştırın.
   
   Örneğin, **TeamUtilities** deposunu yerel makinenizdeki *MyTeam* dizinine klonlamak için aşağıdaki komutlardan birini kullanın. 
   
   **HTTPS bağlantısı:**
   
   ```bash
   git clone https://DataScienceUnit@dev.azure.com/DataScienceUnit/MyTeam/_git/TeamUtilities
   ```
   
   **SSH bağlantısı:**
   
   ```bash
   git clone git@ssh.dev.azure.com:v3/DataScienceUnit/MyTeam/TeamUtilities
   ```

Deponuzun yerel klonunda istediğiniz değişiklikleri yaptıktan sonra, değişiklikleri paylaşılan takım depolarına işleyip itin. 

Yerel **GitRepos\MyTeam\TeamTemplate** veya **GitRepos\MyTeam\TeamUtilities** dizininden aşağıdaki Git Bash komutlarını çalıştırın.

```bash
git add .
git commit -m "push from local"
git push
```

> [!NOTE]
> İlk defa bir Git deposuna söz ediyorsanız, `git commit` komutu çalıştırmadan önce genel parametreleri *user.name* ve *user.email'i* yapılandırmanız gerekebilir. Aşağıdaki iki komutu çalıştırın:
> 
> `git config --global user.name <your name>`
> 
> `git config --global user.email <your email address>`
> 
> Birkaç Git deposuna taahhütte bulunduysanız, hepsi için aynı adı ve e-posta adresini kullanın. Git etkinliklerinizi birden çok depoda izlemek için Power BI panoları inşa ederken aynı adı ve e-posta adresini kullanmak uygundur.

## <a name="add-team-members-and-configure-permissions"></a>Ekip üyeleri ekleme ve izinleri yapılandırma

Takıma üye eklemek için:

1. Azure DevOps'te, **MyTeam** proje giriş sayfasından sol gezintiden **Proje ayarlarını** seçin. 
   
1. Proje **Ayarları'nda** sol navigasyondan **Takımlar'ı**seçin, ardından **Takımlar** sayfasında **MyTeam Team'i**seçin. 
   
   ![Takımları Yapılandır](./media/team-lead-tasks/teams.png)
   
1. Takım **Profili** sayfasında **Ekle'yi**seçin.
   
   ![MyTeam Ekibine Ekle](./media/team-lead-tasks/add-to-team.png)
   
1. Kullanıcı **ekle ve gruplar** iletişim kutusunda, gruba eklenecek üyeleri arayın ve seçin ve ardından **değişiklikleri kaydet'i**seçin. 
   
   ![Kullanıcı ve grup ekleme](./media/team-lead-tasks/add-users.png)
   

Ekip üyeleri için izinleri yapılandırmak için:

1. Proje **Ayarları'nda** sol gezintiden **İzinler'i**seçin. 
   
1. **İzinler** sayfasında, üye eklemek istediğiniz grubu seçin. 
   
1. Bu grubun sayfasında **Üyeler'i**seçin ve sonra **Ekle'yi**seçin. 
   
1. Davet **üyeleri** açılır, arama ve gruba eklemek için üyeleri seçin ve sonra **Kaydet**seçin. 
   
   ![Üyelere izin verme](./media/team-lead-tasks/grant-permissions.png)

## <a name="create-team-data-and-analytics-resources"></a>Ekip verileri ve analiz kaynakları oluşturun

Bu adım isteğe bağlıdır, ancak verileri ve analiz kaynaklarını tüm ekibinizle paylaşmak performans ve maliyet avantajlarına sahiptir. Ekip üyeleri projelerini paylaşılan kaynaklarda yürütebilir, bütçelerden tasarruf edebilir ve daha verimli işbirliği yapabilir. Azure dosya depolama alanı oluşturabilir ve ekip üyeleriyle paylaşmak için DSVM'nize monte edebilirsiniz. 

Azure HDInsight Spark kümeleri gibi ekibinizle diğer kaynakları paylaşma hakkında bilgi için [Platformlara ve araçlara](platforms-and-tools.md)bakın. Bu konu, ihtiyaçlarınıza uygun kaynakları seçme konusunda veri bilimi perspektifinden ve ürün sayfalarına ve diğer alakalı ve yararlı öğreticilere bağlantılar hakkında rehberlik sağlar.

>[!NOTE]
> Yavaş ve maliyetli olabilecek veri merkezleri arasında veri aktarımının önüne geçmek için Azure kaynak grubunuzun, depolama hesabınızın ve DSVM'nizin aynı Azure bölgesinde barındırıldığından emin olun. 

### <a name="create-azure-file-storage"></a>Azure dosya depolama alanı oluşturma

1. Tüm ekibiniz için yararlı olan veri varlıkları için Azure dosya depolaması oluşturmak için aşağıdaki komut dosyasını çalıştırın. Komut dosyası sizi Azure abonelik bilgileriniz için ister, bu nedenle girmeye hazır olun. 

   - Windows makinesinde, komut dosyasını PowerShell komut isteminden çalıştırın:
     
     ```powershell
     wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.ps1" -outfile "CreateFileShare.ps1"
     .\CreateFileShare.ps1
     ```
     
   - Bir Linux makinesinde, komut dosyasını Linux kabuğundan çalıştırın:
     
     ```shell
     wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/CreateFileShare.sh"
     bash CreateFileShare.sh
     ```
   
1. İstendiğinde Microsoft Azure hesabınızda oturum açın ve kullanmak istediğiniz aboneliği seçin.
   
1. Kullanmak üzere depolama hesabını seçin veya seçtiğiniz aboneliğin altında yeni bir hesap oluşturun. Azure dosya depolama adı için küçük harf karakterleri, sayılar ve tireler kullanabilirsiniz.
   
1. Depolamanın montajını ve paylaşımını kolaylaştırmak için, Azure dosya depolama bilgilerini geçerli dizininizdeki bir metin dosyasına kaydetmek için *Y* Girin veya girin. Ekibinizdeki tüm projelerin erişebilmeleri için bu metin dosyasını Ideal olarak **Dokümanlar\DataDictionaries**altında **teamTemplate** deponuza iade edebilirsiniz. Ayrıca, Azure dosya depolama alanınızı bir sonraki bölümde Azure DSVM'inize monte etmek için dosya bilgilerine de ihtiyacınız vardır. 
   
### <a name="mount-azure-file-storage-on-your-local-machine-or-dsvm"></a>Azure dosya depolamayı yerel makinenize veya DSVM'e monte edin

1. Azure dosya depolama alanınızı yerel makinenize veya DSVM'nize monte etmek için aşağıdaki komut dosyasını kullanın.
   
   - Windows makinesinde, komut dosyasını PowerShell komut isteminden çalıştırın:
     
     ```powershell
     wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.ps1" -outfile "AttachFileShare.ps1"
     .\AttachFileShare.ps1
     ```
     
   - Bir Linux makinesinde, komut dosyasını Linux kabuğundan çalıştırın:
     
     ```shell
     wget "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/TDSP/AttachFileShare.sh"
     bash AttachFileShare.sh
     ```
   
1. Bir önceki adımda bir Azure dosya depolama bilgi dosyasını kaydettiyseniz, devam etmek için *Y* Girin veya girin' e basın. Oluşturduğunuz dosyanın tam yolunu ve adını girin. 
   
   Azure dosya depolama bilgi dosyanız yoksa, aboneliğinizi, Azure depolama hesabınızı ve Azure dosya depolama bilgilerinizi girmek için yönergeleri *girin*ve yönergeleri izleyin.
   
1. Dosya paylaşımını takmak için yerel veya TDSP sürücüsünün adını girin. Ekranda varolan sürücü adlarının bir listesi görüntülenir. Zaten var olmayan bir sürücü adı sağlayın.
   
1. Yeni sürücünün ve depolama alanının makinenize başarıyla monte edilebiyi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, Ekip Veri Bilimi Süreci tarafından tanımlanan diğer rollerin ve görevlerin ayrıntılı açıklamalarına bağlantılar verilmiştir:

- [Veri bilimi ekibi için Grup Yöneticisi görevleri](group-manager-tasks.md)
- [Veri bilimi ekibi için Proje Müşteri Adayı görevleri](project-lead-tasks.md)
- [Veri bilimi ekibi için Project Individual Katılımcı görevleri](project-ic-tasks.md)

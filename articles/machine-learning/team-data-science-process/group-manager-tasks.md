---
title: Takım Veri Bilimi Süreci grup yöneticisi görevleri
description: Bir grup yöneticisinin veri bilimi ekibi projesinde tamamlar görevlerin bu ayrıntılı walkthrough izleyin.
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 4ec7f4242e5046e90fdf0eb8c6c0579f402e4f55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721362"
---
# <a name="team-data-science-process-group-manager-tasks"></a>Takım Veri Bilimi Süreci grup yöneticisi görevleri

Bu makalede, bir *grup yöneticisinin* bir veri bilimi organizasyonu için tamamladığını görevler açıklanır. Grup yöneticisi, bir kuruluştaki tüm veri bilimi birimini yönetir. Bir veri bilimi biriminin, her biri farklı iş dikeylerinde birçok veri bilimi projesi üzerinde çalışan birkaç ekibi olabilir. Grup yöneticisinin amacı, [Team Data Science Process](overview.md) (TDSP) üzerinde standartlaştırılabilen ortak bir grup ortamı oluşturmaktır. TDSP'yi standartlaştıran bir veri bilimi ekibi tarafından gerçekleştirilen tüm personel rollerinin ve ilişkili görevlerin ana hatlarını öğrenmek [için, Ekip Veri Bilimi Süreci rolleri ve görevleri bölümüne](roles-tasks.md)bakın.

Aşağıdaki diyagramaltı ana grup yöneticisi kurulum görevlerini gösterir. Grup yöneticileri görevlerini vekillere devredebilir, ancak rolile ilişkili görevler değişmez.

![Grup yöneticisi görevleri](./media/group-manager-tasks/tdsp-group-manager.png)

1. Grup için bir **Azure DevOps organizasyonu** ayarlayın.
2. Azure DevOps kuruluşunda varsayılan **GroupCommon projesini** oluşturun.
3. Azure Depolarında **GroupProjectTemplate** deposunu oluşturun.
4. Azure Depolarında **GroupUtilities** deposunu oluşturun.
5. Microsoft TDSP ekibinin **ProjectTemplate** ve **Yardımcı Programlar** depolarının içeriğini grup ortak depolarına aktarın.
6. Ekip üyelerinin gruba erişmeleri için **üyelik** ve **izinler** ayarlayın.

Aşağıdaki öğretici adımları ayrıntılı olarak gözden geçirir. 

> [!NOTE] 
> Bu makalede, TDSP grup ortamını ayarlamak için Azure DevOps kullanır, çünkü Microsoft'ta TDSP böyle uygulanır. Grubunuz diğer kod barındırma veya geliştirme platformlarını kullanıyorsa, Grup Yöneticisi'nin görevleri aynıdır, ancak bunları tamamlama yolu farklı olabilir.

## <a name="create-an-organization-and-project-in-azure-devops"></a>Azure DevOps'lerde bir kuruluş ve proje oluşturma

1. [visualstudio.microsoft.com](https://visualstudio.microsoft.com)gidin, sağ üstte **Oturum Aç'ı** seçin ve Microsoft hesabınızda oturum açın. 
   
   ![Microsoft hesabınızda oturum açma](./media/group-manager-tasks/signinvs.png)
   
   Microsoft hesabınız yoksa, **şimdi Kaydol'** u, bir Microsoft hesabı oluşturun ve bu hesabı kullanarak oturum açın'ı seçin. Kuruluşunuzun Visual Studio aboneliği varsa, bu aboneliğin kimlik bilgileriyle oturum açın.
   
1. Oturum açmadan sonra, Azure DevOps sayfasında sağ üstte **yeni kuruluş oluştur'u**seçin.
   
   ![Yeni kuruluş oluşturma](./media/group-manager-tasks/create-organization.png)
   
1. Hizmet Koşullarını, Gizlilik Bildirimini ve Davranış Kurallarını kabul etmeniz istenirse, **Devam et'i**seçin.
   
1. Kayıt iletişim kutusunda, Azure DevOps kuruluşunuzun adını belirleyin ve ana bilgisayar bölge atamasını kabul edin veya aşağı inip farklı bir bölge seçin. Daha sonra **Devam** seçeneğini belirleyin. 

1. **Başlamak için proje oluştur** *altında, GroupCommon'ı*girin ve ardından **proje oluştur'u**seçin. 
   
   ![Proje oluşturma](./media/group-manager-tasks/create-project.png)

**GroupCommon** proje **Özeti** sayfası açılır. Sayfa URL *https:\//\<servername\<>/ organizasyon adı>/GroupCommon*.

![Proje özeti sayfası](./media/group-manager-tasks/project-summary.png)

## <a name="set-up-the-group-common-repositories"></a>Grup ortak depolarını ayarlama

Azure Repos, grubunuz için aşağıdaki depo türlerini barındırar:

- **Grup ortak depoları**: Veri bilimi birimi içindeki birden çok ekibin birçok veri bilimi projesi için benimseyebileceği genel amaçlı depolardır. 
- **Takım depoları**: Veri bilimi birimi içindeki belirli ekipler için depolar. Bu depolar bir takımın gereksinimlerine özgüdir ve bu ekip içindeki birden çok proje için kullanılabilir, ancak bir veri bilimi birimi içindeki birden çok ekip arasında kullanılacak kadar genel değildir.
- **Proje depoları**: Belirli projeler için depolar. Bu tür depolar, bir takım içindeki birden çok proje veya veri bilimi birimindeki diğer ekipler için yeterince genel olmayabilir.

Projenizdeki grup ortak depolarını ayarlamak için şunları yapacaksınız: 
- Varsayılan **GroupCommon** deposunu **GroupProjectTemplate** olarak yeniden adlandırın
- Yeni bir **GroupUtilities** deposu oluşturma

### <a name="rename-the-default-project-repository-to-groupprojecttemplate"></a>Varsayılan proje deposunu GroupProjectTemplate olarak yeniden adlandırın

Varsayılan **GroupCommon** proje deposunu **GroupProjectTemplate**olarak yeniden adlandırmak için:

1. **GroupCommon** proje **Özeti** sayfasında **Depoları**seçin. Bu eylem, şu anda boş olan GroupCommon projesinin varsayılan **GroupCommon** deposuna götürür.
   
1. Sayfanın üst kısmında, **GroupCommon'un** yanındaki oku bırakın ve **depoları yönet'i**seçin.
   
   ![Depoları yönetme](./media/group-manager-tasks/rename-groupcommon-repo-3.png)
   
1. Proje **Ayarları** sayfasında, **GroupCommon'un**yanındaki **...** seçeneğini seçin ve ardından **Resay deposunu**seçin. 
   
   ![Seçin... ve sonra Resay deposunu seçin](./media/group-manager-tasks/rename-groupcommon-repo-4.png)
   
1. **GroupCommon deposuaçılır penceresini yeniden adlandırın,** *GroupProjectTemplate'i*girin ve ardından **Yeniden Adlandır'ı**seçin. 
   
   ![Rede deposu](./media/group-manager-tasks/rename-groupcommon-repo-6.png)

### <a name="create-the-grouputilities-repository"></a>GroupUtilities deposunu oluşturma

**GroupUtilities** deposunu oluşturmak için:

1. **GroupCommon** proje **Özeti** sayfasında **Depoları**seçin. 
   
1. Sayfanın üst kısmında, **GroupProjectTemplate'in** yanındaki oku bırakın ve **Yeni depoyu**seçin.
   
   ![Yeni depo seçin](./media/group-manager-tasks/create-grouputilities-repo-1.png)
   
1. Yeni **bir depo oluştur** iletişim kutusunda, **Tür**olarak **Git'i** seçin, Depo **adı**olarak *GroupUtilities'i* girin ve sonra **Oluştur'u**seçin.
   
   ![GroupUtilities deposu oluşturma](./media/group-manager-tasks/create-grouputilities-repo-2.png)
   
1. Proje **Ayarları** sayfasında, iki grup deposunu görmek için sol navigasyonda **Depolar'ın** altındaki **Depoları** seçin: **GroupProjectTemplate** ve **GroupUtilities.**
   
   ![İki grup deposu](./media/group-manager-tasks/two-repositories.png)

## <a name="import-the-microsoft-tdsp-team-repositories"></a>Microsoft TDSP takım depolarını alma

Öğreticinin bu bölümünde, Microsoft TDSP ekibi tarafından yönetilen **ProjectTemplate** ve **Yardımcı Programlar** depolarının içeriğini **GroupProjectTemplate** ve **GroupUtilities** depolarınıza aktarırsınız. 

TDSP takım depolarını almak için:

1. **GroupCommon** proje ana sayfasından, sol navigasyonda **Repos'u** seçin. Varsayılan **GroupProjectTemplate** repo açılır. 
   
1. **GroupProjectTemplate** boş sayfada, **Içe Aktar'ı**seçin. 
   
   ![Alma'yı seçin](./media/group-manager-tasks/import-repo.png)
   
1. Git **deposu iletişim** kutusunda, Kaynak türü olarak **Git'i** seçin ve Klon **URL'si**için *https:\//github.com/Azure/Azure-TDSP-ProjectTemplate.git* girin. **Source type** Sonra **İçe Aktar'ı**seçin. Microsoft TDSP ekibi ProjectTemplate deposunun içeriği GroupProjectTemplate deponuza aktarılır. 
   
   ![Microsoft TDSP takım deposunu içe aktarma](./media/group-manager-tasks/import-repo-2.png)
   
1. **Deposayfasının** üst kısmında, aşağı bırakın ve **GroupUtilities** deposunu seçin.
   
1. Microsoft TDSP ekibi **Yardımcı Programlar** deposunun içeriğini almak için alma işlemini yineleyin, *https:\//github.com/Azure/Azure-TDSP-Utilities.git*, **GroupUtilities** deponuza. 
   
İki grup deponuzun her biri, Microsoft TDSP ekibinin ilgili deposundaki *.git* dizinindekiler hariç tüm dosyaları içerir. 

## <a name="customize-the-contents-of-the-group-repositories"></a>Grup depolarının içeriğini özelleştirme

Grubunuzun özel gereksinimlerini karşılayacak şekilde grup depolarınızın içeriğini özelleştirmek istiyorsanız, bunu şimdi yapabilirsiniz. Dosyaları değiştirebilir, dizin yapısını değiştirebilir veya grubunuzun geliştirdiği veya grubunuz için yararlı olan dosyaları ekleyebilirsiniz.

### <a name="make-changes-in-azure-repos"></a>Azure Repos'ta değişiklik yapma

Depo içeriğini özelleştirmek için:

1. **GroupCommon** proje **Özeti** sayfasında **Depoları**seçin. 
   
1. Sayfanın üst kısmında, özelleştirmek istediğiniz depoyu seçin.

1. Repo dizini yapısında, değiştirmek istediğiniz klasöre veya dosyaya gidin. 
   
   - Yeni klasörler veya dosyalar oluşturmak için **Yeni'nin**yanındaki oku seçin. 
     
     ![Yeni dosya oluşturma](./media/group-manager-tasks/new-file.png)
     
   - Dosya yüklemek için **Dosya(lar) dosyasını yükleyin'i**seçin. 
     
     ![Dosyaları karşıya yükleme](./media/group-manager-tasks/upload-files.png)
     
   - Varolan dosyaları yeniden sağlamak için dosyaya gidin ve ardından **Edit'i**seçin. 
     
     ![Dosyayı edin](./media/group-manager-tasks/edit-file.png)
     
1. Dosya ekledikten veya düzenlemeden sonra **Commit'i**seçin.
   
   ![Değişiklikler gerçekleştirme](./media/group-manager-tasks/commit.png)

### <a name="make-changes-using-your-local-machine-or-dsvm"></a>Yerel makinenizi veya DSVM'nizi kullanarak değişiklik yapın

Yerel makinenizi veya DSVM'nizi kullanarak değişiklik yapmak ve değişiklikleri grup depolarına itmek istiyorsanız, Git ve DSVM'lerle çalışmak için ön koşullara sahip olduğunuzdan emin olun:

- Bir DSVM oluşturmak istiyorsanız, Azure aboneliği.
- Git makinenize yüklendi. DSVM kullanıyorsanız, Git önceden yüklenmiş. Aksi takdirde, [Platformlar ve araçlar ekine](platforms-and-tools.md#appendix)bakın.
- Bir DSVM kullanmak istiyorsanız, Windows veya Linux DSVM oluşturulan ve Azure'da yapılandırılan. Daha fazla bilgi ve talimatlar için [Veri Bilimi Sanal Makine Dokümantasyonu'na](/azure/machine-learning/data-science-virtual-machine/)bakın.
- Windows DSVM için makinenizde [Git Kimlik Bilgisi Yöneticisi (GCM)](https://github.com/Microsoft/Git-Credential-Manager-for-Windows) yüklenir. *README.md* dosyasında, **İndir ve Yükle** bölümüne gidin ve en son **yükleyiyi**seçin. *.exe* yükleyicisini yükleyici sayfasından indirin ve çalıştırın. 
- Bir Linux DSVM için, DSVM'nizde ayarlanmış ve Azure DevOps'lere eklenen bir SSH ortak anahtarı. Daha fazla bilgi ve talimatlar için Platformlar ve [araçlar ekindeki](platforms-and-tools.md#appendix) **SSH ortak anahtar oluşturma** bölümüne bakın. 

İlk olarak, depoyu yerel makinenize kopyalayın veya *klonlayın.* 
   
1. **GroupCommon** proje **Özeti** **sayfasında, Depolar'ı**seçin ve sayfanın üst kısmında klonlamak istediğiniz depoyu seçin.
   
1. Repo sayfasında sağ üstteki **Klon'u** seçin.
   
1. Klon **deposu** iletişim kutusunda, HTTP bağlantısı için **HTTPS** veya SSH bağlantısı için **SSH'yi** seçin ve **Komut satırıaltındaki** klon URL'sini panonuza kopyalayın.
   
   ![Klon repo](./media/group-manager-tasks/clone.png)
   
1. Yerel makinenizde aşağıdaki dizinleri oluşturun:
   
   - Windows için: **C:\GitRepos\GroupCommon**
   - Linux için, **$/GitRepos/GroupCommon** ev dizininizde 
   
1. Oluşturduğunuz dizine değiştirin.
   
1. Git Bash'te, komutu çalıştırın`git clone <clone URL>.`
   
   Örneğin, aşağıdaki komutlardan **biri, GroupUtilities** deposunu yerel makinenizdeki *GroupCommon* dizinine klonlar. 
   
   **HTTPS bağlantısı:**
   
   ```bash
   git clone https://DataScienceUnit@dev.azure.com/DataScienceUnit/GroupCommon/_git/GroupUtilities
   ```
   
   **SSH bağlantısı:**
   
   ```bash
   git clone git@ssh.dev.azure.com:v3/DataScienceUnit/GroupCommon/GroupUtilities
   ```

Deponuzun yerel klonunda istediğiniz değişiklikleri yaptıktan sonra, değişiklikleri paylaşılan grup ortak depolarına itebilirsiniz. 

Yerel **GroupProjectTemplate** veya **GroupUtilities** dizininizden aşağıdaki Git Bash komutlarını çalıştırın.

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

## <a name="add-group-members-and-configure-permissions"></a>Grup üyeleri ekleme ve izinleri yapılandırma

Gruba üye eklemek için:

1. Azure DevOps'te, **GroupCommon** proje giriş sayfasından sol gezintiden **Proje ayarlarını** seçin. 
   
1. Proje **Ayarları'nda** sol gezintiden **Takımlar'ı**seçin, ardından **Takımlar** sayfasında **GroupCommon Team'i**seçin. 
   
   ![Takımları Yapılandır](./media/group-manager-tasks/teams.png)
   
1. Takım **Profili** sayfasında **Ekle'yi**seçin.
   
   ![GroupCommon Ekibine Ekle](./media/group-manager-tasks/add-to-team.png)
   
1. Kullanıcı **ekle ve gruplar** iletişim kutusunda, gruba eklenecek üyeleri arayın ve seçin ve ardından **değişiklikleri kaydet'i**seçin. 
   
   ![Kullanıcı ve grup ekleme](./media/group-manager-tasks/add-users.png)
   

Üyeler için izinleri yapılandırmak için:

1. Proje **Ayarları'nda** sol gezintiden **İzinler'i**seçin. 
   
1. **İzinler** sayfasında, üye eklemek istediğiniz grubu seçin. 
   
1. Bu grubun sayfasında **Üyeler'i**seçin ve sonra **Ekle'yi**seçin. 
   
1. Davet **üyeleri** açılır, arama ve gruba eklemek için üyeleri seçin ve sonra **Kaydet**seçin. 
   
   ![Üyelere izin verme](./media/group-manager-tasks/grant-permissions.png)

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda, Ekip Veri Bilimi Sürecindeki diğer rol ve görevlerin ayrıntılı açıklamalarına bağlantılar verilmiştir:

- [Veri bilimi ekibi için Takım Müşteri Adayı görevleri](team-lead-tasks.md)
- [Veri bilimi ekibi için Proje Müşteri Adayı görevleri](project-lead-tasks.md)
- [Veri bilimi ekibi için Project Individual Katılımcı görevleri](project-ic-tasks.md)

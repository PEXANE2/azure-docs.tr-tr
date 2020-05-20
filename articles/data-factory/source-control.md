---
title: Kaynak denetimi
description: Azure Data Factory 'da kaynak denetimini yapılandırmayı öğrenin
services: data-factory
ms.service: data-factory
ms.workload: data-services
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.reviewer: ''
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 04/30/2020
ms.openlocfilehash: 2d6f667b6a49520dfe210fd797a828328899b634
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83674599"
---
# <a name="source-control-in-azure-data-factory"></a>Azure Data Factory kaynak denetimi
[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Varsayılan olarak, Azure Data Factory Kullanıcı arabirimi deneyimi (UX) yazarları doğrudan Data Factory hizmetine göre yapılır. Bu deneyim aşağıdaki sınırlamalara sahiptir:

- Data Factory hizmeti, değişikliklerinizin JSON varlıklarını depolamak için bir depo içermez. Değişiklikleri kaydetmek için tek yol **Tümünü Yayımla** düğmesi ile tüm değişiklikler doğrudan Data Factory hizmetine yayımlanır.
- Data Factory hizmeti işbirliği ve sürüm denetimi için en iyi duruma getirilmemiştir.

Daha iyi bir yazma deneyimi sağlamak için Azure Data Factory, bir Git deposunu Azure Repos veya GitHub ile yapılandırmanıza olanak tanır. Git, daha kolay değişiklik izleme ve işbirliğine olanak tanıyan bir sürüm denetim sistemidir. Bu öğretici, en iyi yöntemler ve sorun giderme kılavuzu ile birlikte bir git deposundaki yapılandırmayı ve çalışmayı özetler.

> [!NOTE]
> Azure Data Factory git tümleştirmesi, Azure Kamu Bulutu 'nda bulunmamaktadır.

## <a name="advantages-of-git-integration"></a>Git tümleştirmesinin avantajları

Aşağıda, git tümleştirmesinin yazma deneyimine sağladığı avantajların bir listesi verilmiştir:

-   **Kaynak denetimi:** Data Factory iş yükleriniz önemli olmaya yönelik olarak, aşağıdaki gibi çeşitli kaynak denetimi avantajlarından yararlanmak için fabrikanızı git ile tümleştirmeniz gerekir:
    -   Değişiklikleri izleme/denetleme özelliği.
    -   Hataları ortaya çıkan değişiklikleri döndürmenize olanak tanır.
-   **Kısmi kaydetme:** Data Factory hizmetine göre yazarken, değişiklikleri taslak olarak kaydedemez ve tüm yayınlar Veri Fabrikası doğrulaması ' nı iletmelidir. İşlem hatlarınız bitmediği veya bir bilgisayar kilitlenmesiyle ilgili değişiklikleri kaybetmek istemiyorsanız, git tümleştirmesi, Veri Fabrikası kaynakları üzerinde bulundukları durum ne olursa olsun artımlı değişikliklere izin verir. Bir git deposunun yapılandırılması, değişiklikleri kaydetmenizi sağlayarak yalnızca yaptığınız değişiklikleri sizin için test ettiğinizde yayımlamanıza izin verir.
-   **İşbirliği ve denetim:** Aynı fabrikaya katkıda bulunan birden fazla ekip üyesine sahipseniz, bir kod inceleme işlemi aracılığıyla ekip malarınızın birbirleriyle işbirliği yapmasına izin vermek isteyebilirsiniz. Ayrıca, fabrikanızın her katkıda bulunan eşit izinleri olmadığı şekilde ayarlayabilirsiniz. Bazı takım üyelerinin yalnızca git aracılığıyla değişiklik yapmasına izin verilebilir ve yalnızca ekipteki belirli kişilerin değişiklikleri fabrikada yayımlamasına izin verilir.
-   **Daha ıyı CI/CD:**  [Sürekli teslim işlemiyle](continuous-integration-deployment.md)birden çok ortama dağıtıyorsanız, git tümleştirmesi belirli eylemleri daha kolay hale getirir. Bu eylemlerin bazıları şunlardır:
    -   Yayın işlem hattınızı, ' dev ' fabrikasında yapılan herhangi bir değişiklik olduğu anda otomatik olarak tetiklenecek şekilde yapılandırın.
    -   Fabrikanızın Kaynak Yöneticisi şablonunda parametreler olarak kullanılabilen özellikleri özelleştirin. Yalnızca gerekli özellikler kümesini parametre olarak tutmak ve her şeyin sabit olarak kodlanmasını sağlamak yararlı olabilir.
-   **Daha Iyi performans:** Git tümleştirmesinin bulunduğu ortalama bir fabrika, Data Factory hizmetinde bir yazma işlemiyle 10 kat daha hızlı yükler. Bu performans geliştirmesi, kaynakların git aracılığıyla indirilmesinden kaynaklanır.

> [!NOTE]
> Git deposu yapılandırıldığında Data Factory hizmeti ile doğrudan yazma Azure Data Factory UX içinde devre dışıdır. Değişiklikler doğrudan hizmete PowerShell veya SDK aracılığıyla yapılabilir.

## <a name="author-with-azure-repos-git-integration"></a>Azure Repos Git tümleştirmesi ile yazma

Azure Repos git tümleştirmesi ile görsel yazma, Veri Fabrikası işlem hatlarınız üzerinde iş için kaynak denetimi ve işbirliğini destekler. Kaynak denetimi, işbirliği, sürüm oluşturma vb. için bir Azure Repos git kuruluş deposu ile bir veri fabrikası ilişkilendirebilirsiniz. Tek bir Azure Repos git kuruluşunda birden çok depo bulunabilir, ancak bir Azure Repos git deposu yalnızca bir veri fabrikası ile ilişkilendirilebilir. Azure Repos kuruluşunuz veya deponuz yoksa kaynaklarınızı oluşturmak için [Bu yönergeleri](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student) izleyin.

> [!NOTE]
> Betik ve veri dosyalarını bir Azure Repos git deposunda saklayabilirsiniz. Ancak, dosyaları Azure depolama 'ya el ile yüklemeniz gerekir. Data Factory işlem hattı, bir Azure Repos git deposunda depolanan betiği veya veri dosyalarını Azure depolama 'ya otomatik olarak yüklemez.

### <a name="configure-an-azure-repos-git-repository-with-azure-data-factory"></a>Azure Data Factory ile Azure Repos git deposu yapılandırma

Bir Azure Repos Git deposunu, veri fabrikasının iki yöntemi aracılığıyla yapılandırabilirsiniz.

#### <a name="configuration-method-1-azure-data-factory-home-page"></a>Yapılandırma yöntemi 1: Azure Data Factory giriş sayfası

Azure Data Factory giriş sayfasında **kod deposunu ayarla**' yı seçin.

![Azure Repos kod deposu yapılandırma](media/author-visually/configure-repo.png)

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Yapılandırma yöntemi 2: UX yazma tuvali
Azure Data Factory UX yazma tuvalinde **Data Factory** açılan menüsünü ve ardından **kod deposunu ayarla**' yı seçin.

![UX yazma için kod deposu ayarlarını yapılandırma](media/author-visually/configure-repo-2.png)

Her iki yöntem de depo ayarları yapılandırma bölmesini açar.

![Kod deposu ayarlarını yapılandırma](media/author-visually/repo-settings.png)

Yapılandırma bölmesinde aşağıdaki Azure Repos kod deposu ayarları gösterilmektedir:

| Ayar | Açıklama | Değer |
|:--- |:--- |:--- |
| **Depo türü** | Azure Repos kod deposunun türü.<br/> | Azure DevOps git veya GitHub |
| **Azure Active Directory** | Azure AD kiracı adınız. | `<your tenant name>` |
| **Azure Repos kuruluş** | Azure Repos kuruluşunuzun adı. Azure Repos kuruluşunuzun adını adresinde bulabilirsiniz `https://{organization name}.visualstudio.com` . Visual Studio profilinize erişmek ve depolarınızı ve projelerinizi görmek için [Azure Repos kuruluşunuzda oturum](https://www.visualstudio.com/team-services/git/) açabilirsiniz. | `<your organization name>` |
| **ProjectName** | Azure Repos projenizin adı. Azure Repos projenizin adını adresinde bulabilirsiniz `https://{organization name}.visualstudio.com/{project name}` . | `<your Azure Repos project name>` |
| **RepositoryName** | Azure Repos kod deposu adınız. Azure Repos projeler, projeniz büyüdükçe kaynak kodunuzu yönetmek için Git depoları içerir. Yeni bir depo oluşturabilir veya projenizde zaten var olan bir depoyu kullanabilirsiniz. | `<your Azure Repos code repository name>` |
| **İşbirliği dalı** | Yayımlamak için kullanılan Azure Repos işbirliği dalı. Varsayılan olarak, `master` . Başka bir daldan kaynak yayınlamak istiyorsanız bu ayarı değiştirin. | `<your collaboration branch name>` |
| **Kök klasör** | Azure Repos işbirliği dalınızdaki kök klasörünüz. | `<your root folder name>` |
| **Mevcut Data Factory kaynaklarını depoya aktar** | UX **yazma tuvalinden** bir Azure Repos git deposuna var olan veri fabrikası kaynaklarının içeri aktarılmayacağını belirtir. Veri Fabrikası kaynaklarınızı JSON biçiminde ilişkili git deposuna aktarmak için kutuyu seçin. Bu eylem, her kaynağı ayrı ayrı dışarı aktarır (yani, bağlı hizmetler ve veri kümeleri ayrı Jdönemleri içine aktarılır). Bu kutu seçili olmadığında, mevcut kaynaklar içeri aktarılmaz. | Seçili (varsayılan) |
| **Kaynağı içeri aktarma dalı** | Veri Fabrikası kaynaklarının (işlem hatları, veri kümeleri, bağlı hizmetler vs.) içe aktarıldığı dalı belirtir. Kaynakları şu dallardan birine içeri aktarabilirsiniz: a. İşbirliği b. Yeni c oluştur. Mevcut olanı kullan |  |

> [!NOTE]
> Microsoft Edge kullanıyorsanız ve Azure DevOps hesabı açılır listenizde herhangi bir değer görmüyorsanız, güvenilen siteler listesine https://*. VisualStudio. com ekleyin.

### <a name="use-a-different-azure-active-directory-tenant"></a>Farklı bir Azure Active Directory kiracı kullanın

Azure Repos git deposu, farklı bir Azure Active Directory kiracısında olabilir. Farklı bir Azure AD kiracısı belirtmek için kullandığınız Azure aboneliğinde yönetici izinlerine sahip olmanız gerekir.

### <a name="use-your-personal-microsoft-account"></a>Kişisel Microsoft hesabı kullanın

Git tümleştirmesi için kişisel Microsoft hesabı kullanmak istiyorsanız, kişisel Azure deponuzu kuruluşunuzun Active Directory bağlayabilirsiniz.

1. Kişisel Microsoft hesabı kuruluşunuzun Active Directory Konuk olarak ekleyin. Daha fazla bilgi için, bkz. [Azure portal Azure ACTIVE DIRECTORY B2B işbirliği kullanıcıları ekleme](../active-directory/b2b/add-users-administrator.md).

2. Kişisel Microsoft hesabı Azure portal oturum açın. Sonra kuruluşunuzun Active Directory geçin.

3. Kişisel deponuzu şimdi gördüğünüz Azure DevOps bölümüne gidin. Depoyu seçin ve Active Directory bağlanın.

Bu yapılandırma adımlarında sonra, Data Factory Kullanıcı arabiriminde git tümleştirmesini ayarlarken kişisel deponuz kullanılabilir.

Kuruluşunuzun Active Directory Azure Repos bağlama hakkında daha fazla bilgi için bkz. [Azure DevOps kuruluşunuzu Azure Active Directory 'e bağlama](/azure/devops/organizations/accounts/connect-organization-to-azure-ad).

## <a name="author-with-github-integration"></a>GitHub tümleştirmesi ile yazma

GitHub tümleştirmesiyle Visual Authoring, Veri Fabrikası işlem hatlarınız üzerinde iş için kaynak denetimi ve işbirliğini destekler. Kaynak denetimi, işbirliği ve sürüm oluşturma için bir GitHub hesap deposuyla bir veri fabrikasını ilişkilendirebilirsiniz. Tek bir GitHub hesabının birden çok deposu olabilir, ancak bir GitHub deposu yalnızca bir veri fabrikası ile ilişkilendirilebilir. GitHub hesabınız veya deponuz yoksa kaynaklarınızı oluşturmak için [Bu yönergeleri](https://github.com/join)izleyin   .

Data Factory ile GitHub tümleştirmesi hem genel GitHub (yani,) hem de [https://github.com](https://github.com) GitHub Enterprise 'u destekler. GitHub 'daki depoya okuma ve yazma izniniz olduğu sürece hem ortak hem de özel GitHub depolarını Data Factory ile birlikte kullanabilirsiniz.

Bir GitHub deposunu yapılandırmak için, kullanmakta olduğunuz Azure aboneliği için yönetici izinlerinizin olması gerekir.

Bu özelliği dokuz dakikalık bir giriş ve tanıtım için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Azure-Data-Factory-visual-tools-now-integrated-with-GitHub/player]

### <a name="configure-a-github-repository-with-azure-data-factory"></a>Azure Data Factory ile GitHub deposu yapılandırma

Bir GitHub deposunu, veri fabrikasının iki yöntemi aracılığıyla yapılandırabilirsiniz.

#### <a name="configuration-method-1-azure-data-factory-home-page"></a>Yapılandırma yöntemi 1: Azure Data Factory giriş sayfası

Azure Data Factory giriş sayfasında **kod deposunu ayarla**' yı seçin.

![Azure Repos kod deposu yapılandırma](media/author-visually/configure-repo.png)

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Yapılandırma yöntemi 2: UX yazma tuvali

Azure Data Factory UX yazma tuvalinde **Data Factory** açılan menüsünü ve ardından **kod deposunu ayarla**' yı seçin.

![UX yazma için kod deposu ayarlarını yapılandırma](media/author-visually/configure-repo-2.png)

Her iki yöntem de depo ayarları yapılandırma bölmesini açar.

![GitHub depo ayarları](media/author-visually/github-integration-image2.png)

Yapılandırma bölmesinde aşağıdaki GitHub deposu ayarları gösterilmektedir:

| **Ayar** | **Açıklama**  | **Değer**  |
|:--- |:--- |:--- |
| **Depo türü** | Azure Repos kod deposunun türü. | GitHub |
| **GitHub Enterprise kullanma** | GitHub Enterprise 'ı seçme onay kutusu | seçilmemiş (varsayılan) |
| **GitHub Enterprise URL 'SI** | GitHub Enterprise root URL 'SI (yerel GitHub Enterprise Server için HTTPS olmalıdır). Örneğin: `https://github.mydomain.com`. Yalnızca **GitHub Enterprise kullan** seçilirse gereklidir | `<your GitHub enterprise url>` |                                                           
| **GitHub hesabı** | GitHub hesabınızın adı. Bu ad https: \/ /GitHub.exe com/{Account Name}/{Repository Name} öğesinden bulunabilir. Bu sayfaya gidildiğinde GitHub hesabınıza GitHub OAuth kimlik bilgilerini girmeniz istenir. | `<your GitHub account name>` |
| **Depo adı**  | GitHub kod deposu adınız. GitHub hesapları, kaynak kodunuzu yönetmek için Git depoları içerir. Yeni bir depo oluşturabilir veya hesabınızda zaten var olan bir depoyu kullanabilirsiniz. | `<your repository name>` |
| **İşbirliği dalı** | Yayımlamak için kullanılan GitHub işbirliği dalınız. Varsayılan olarak, ana. Başka bir daldan kaynak yayınlamak istiyorsanız bu ayarı değiştirin. | `<your collaboration branch>` |
| **Kök klasör** | GitHub işbirliği dalınızdaki kök klasörünüz. |`<your root folder name>` |
| **Mevcut Data Factory kaynaklarını depoya aktar** | UX yazma tuvalinden bir GitHub deposuna var olan veri fabrikası kaynaklarının içeri aktarılmayacağını belirtir. Veri Fabrikası kaynaklarınızı JSON biçiminde ilişkili git deposuna aktarmak için kutuyu seçin. Bu eylem, her kaynağı ayrı ayrı dışarı aktarır (yani, bağlı hizmetler ve veri kümeleri ayrı Jdönemleri içine aktarılır). Bu kutu seçili olmadığında, mevcut kaynaklar içeri aktarılmaz. | Seçili (varsayılan) |
| **Kaynağı içeri aktarma dalı** | Veri Fabrikası kaynaklarının (işlem hatları, veri kümeleri, bağlı hizmetler vs.) içe aktarıldığı dalı belirtir. Kaynakları şu dallardan birine içeri aktarabilirsiniz: a. İşbirliği b. Yeni c oluştur. Mevcut olanı kullan |  |

### <a name="known-github-limitations"></a>Bilinen GitHub sınırlamaları

- Betik ve veri dosyalarını bir GitHub deposunda saklayabilirsiniz. Ancak, dosyaları Azure depolama 'ya el ile yüklemeniz gerekir. Data Factory işlem hattı, GitHub deposunda depolanan betiği veya veri dosyalarını Azure depolama 'ya otomatik olarak yüklemez.

- 2.14.0 'den eski bir sürüme sahip GitHub Enterprise, Microsoft Edge tarayıcısında çalışmaz.

- Data Factory görsel yazma araçları ile GitHub tümleştirmesi yalnızca, Data Factory genel kullanıma sunulan sürümünde çalışmaktadır.

- Her kaynak türü için (işlem hatları ve veri kümeleri gibi) en fazla 1.000 varlık tek bir GitHub dalında getirilebilir. Bu sınıra ulaşıldığında, kaynaklarınızı ayrı fabrikalar halinde bölmek için önerilir. Azure DevOps git 'in bu sınırlaması yoktur.

## <a name="version-control"></a>Sürüm denetimi

Sürüm denetimi sistemleri ( _kaynak denetimi_olarak da bilinir), geliştiricilerin kod üzerinde işbirliği yapmasına ve kod tabanında yapılan değişiklikleri izlemesine izin verir. Kaynak denetimi, çoklu geliştirici projelerine yönelik temel bir araçtır.

### <a name="creating-feature-branches"></a>Özellik dalları oluşturma

Bir veri fabrikası ile ilişkili her bir Azure Repos git deposu bir işbirliği dalına sahiptir. ( `master` varsayılan işbirliği daldır). Kullanıcılar, dal açılan menüsünde **+ yeni dal** ' a tıklayarak da özellik dalları oluşturabilir. Yeni dal bölmesi göründüğünde, özellik dalınızın adını girin.

![Yeni dal oluştur](media/author-visually/new-branch.png)

Özellik dalınızdaki değişiklikleri işbirliği dalınızla birleştirmeye hazırsanız, dal açılan listesine tıklayın ve **çekme Isteği oluştur**' u seçin. Bu eylem, çekme istekleri oluşturabileceğiniz, kod İncelemeleri yapabileceğiniz ve değişiklikleri işbirliği dalınızda birleştirebileceğiniz git Azure Repos için sizi yönlendirir. ( `master` varsayılan). Yalnızca Data Factory hizmetine işbirliği dalınızdan yayımlama izni verilir. 

![Yeni bir çekme isteği oluştur](media/author-visually/create-pull-request.png)

### <a name="configure-publishing-settings"></a>Yayımlama ayarlarını yapılandırma

Data Factory, varsayılan olarak, yayımlanan fabrikasının Kaynak Yöneticisi şablonlarını oluşturur ve bunları adlı bir dala kaydeder `adf_publish` . Özel bir yayımlama dalı yapılandırmak için, `publish_config.json` işbirliği dalında kök klasöre bir dosya ekleyin. Yayımlarken, ADF bu dosyayı okur, alanı arar `publishBranch` ve tüm kaynak yöneticisi şablonlarını belirtilen konuma kaydeder. Dal yoksa, Data Factory tarafından otomatik olarak oluşturulur. Ve bu dosyanın nasıl göründüğüne ilişkin örnek aşağıda verilmiştir:

```json
{
    "publishBranch": "factory/adf_publish"
}
```

Tek seferde yalnızca bir yayımlama dalı olabilir Azure Data Factory. Yeni bir yayımlama dalı belirttiğinizde, Data Factory önceki yayımlama dalını silmez. Önceki yayımlama dalını kaldırmak istiyorsanız el ile silin.

> [!NOTE]
> Data Factory yalnızca `publish_config.json` fabrikası yüklediğinde dosyayı okur. Portalda zaten fabrika yüklüyse, değişikliklerinizin geçerli olması için tarayıcıyı yenileyin.

### <a name="publish-code-changes"></a>Kod değişikliklerini Yayımla

Değişiklikleri işbirliği dalında ( `master` Varsayılan olarak) birleştirdikten sonra, kod değişikliklerinizi ana dalda Data Factory hizmetine el ile yayımlamak Için **Yayımla** ' ya tıklayın.

![Değişiklikleri Data Factory hizmette yayımlayın](media/author-visually/publish-changes.png)

Yayın dalının ve bekleyen değişikliklerin doğru olduğunu onaylamak için bir yan bölme açılır. Değişikliklerinizi doğruladıktan sonra, yayımlamayı onaylamak için **Tamam** ' ı tıklatın.

![Doğru yayımlama dalını onaylayın](media/author-visually/configure-publish-branch.png)

> [!IMPORTANT]
> Ana dal, Data Factory hizmetine dağıtılan özellikleri temsil etmez. Ana dalın Data Factory hizmetine el ile yayımlanması *gerekir* .

## <a name="best-practices-for-git-integration"></a>Git tümleştirmesi için en iyi uygulamalar

### <a name="permissions"></a>İzinler

Genellikle, her bir takım üyesinin fabrikası güncelleştirme izinlerine sahip olmasını istemezsiniz. Aşağıdaki izin ayarları önerilir:

*   Tüm takım üyeleri, Veri Fabrikası için okuma izinlerine sahip olmalıdır.
*   Yalnızca bir SELECT kişinin fabrikada yayımlamasına izin verilmelidir. Bunu yapmak için, fabrikasının içinde bulunduğu kaynak grubunda **Data Factory katkıda** bulunan rolüne sahip olmaları gerekir. İzinler hakkında daha fazla bilgi için bkz. [Azure Data Factory roller ve izinler](concepts-roles-permissions.md).
   
İşbirliği dalında doğrudan iadelere izin vermeniz önerilir. Bu kısıtlama, her iade etme işleminin [özellik dalları oluşturma](source-control.md#creating-feature-branches)bölümünde açıklanan bir çekme isteği gözden geçirme süreci boyunca ilerlemesinin önlenmesi için hataları önlemeye yardımcı olabilir.

### <a name="using-passwords-from-azure-key-vault"></a>Azure Key Vault parolaları kullanma

Data Factory bağlı hizmetler için herhangi bir bağlantı dizesini veya parolayı ya da yönetilen kimlik kimlik doğrulamasını depolamak üzere Azure Key Vault kullanmanız önerilir. Veri Fabrikası, güvenlik nedenleriyle gizli dizileri git 'te depolamaz. Bağlı hizmetlerde parolalar gibi gizli dizileri içeren tüm değişiklikler Azure Data Factory hizmetine anında yayımlanır.

Key Vault veya MSI kimlik doğrulamasının kullanılması, Kaynak Yöneticisi Şablon dağıtımı sırasında bu gizli dizileri sağlamanız gerektiği için sürekli tümleştirme ve dağıtımı kolaylaştırır.

## <a name="troubleshooting-git-integration"></a>Git tümleştirmesi sorunlarını giderme

### <a name="stale-publish-branch"></a>Eski yayımlama dalı

Yayımla dalı ana Dalla eşitlenmemiş ve son bir yayımlamaya rağmen güncel olmayan kaynaklar içeriyorsa, aşağıdaki adımları izleyin:

1. Geçerli git deponuzu kaldırma
1. Git 'i aynı ayarlarla yeniden yapılandırın, ancak **mevcut Data Factory kaynaklarını depoya Içeri aktarma** işleminin seçili olduğundan emin olun ve **yeni dal** seçin
1. İşbirliği dalında yapılan değişiklikleri birleştirmek için bir çekme isteği oluşturun 

Aşağıda, eski bir yayımlama dalına neden olabilecek durumlara ilişkin bazı örnekler verilmiştir:
- Bir kullanıcının birden çok dalı vardır. Tek bir özellik dalında, AKV ile ilişkili olmayan bağlı bir hizmeti sildi (gitmeyen veya olmayan bağlı hizmetler, git ' te olup olmadığı ve özellik dalından işbirliği dalına birleştirildiğine bakılmaksızın hemen yayımlanır).
- Kullanıcı SDK veya PowerShell kullanarak veri fabrikasını değiştirdi
- Bir Kullanıcı tüm kaynakları yeni bir dala taşıdı ve ilk kez yayımlamayı denedi. Bağlı hizmetler, kaynakları içeri aktarırken el ile oluşturulmalıdır.
- Kullanıcı, bir AKG bağlı hizmetini veya bir JSON Integration Runtime el ile karşıya yükler. Veri kümesi, bağlantılı hizmet veya işlem hattı gibi başka bir kaynaktan bu kaynağa başvurur. UX aracılığıyla oluşturulan bir AKıŞ olmayan bağlantılı hizmet, kimlik bilgilerinin şifrelenmesi gerektiği için hemen yayımlanır. Bağlı hizmete başvuran bir veri kümesini karşıya yükler ve yayımlamayı denerseniz, git ortamında mevcut olduğundan UX buna izin verir. Veri Fabrikası hizmetinde mevcut olmadığından, yayımlama sırasında reddedilir.

## <a name="switch-to-a-different-git-repository"></a>Farklı bir git deposuna geçiş yap

Farklı bir git deposuna geçiş yapmak için Data Factory Genel Bakış sayfasının sağ üst köşesindeki **Git deposu ayarları** simgesine tıklayın. Simgeyi göremiyorsanız, yerel tarayıcı önbelleğinizi temizleyin. Geçerli depoyla ilişkilendirmeyi kaldırmak için simgeyi seçin.

![Git simgesi](media/author-visually/remove-repo.png)

Depo ayarları bölmesi göründüğünde **Git 'ı kaldır**' ı seçin. Data Factory adınızı girin ve veri fabrikanızın ilişkili Git deposunu kaldırmak için **Onayla** ' ya tıklayın.

![Geçerli git deposundan ilişkilendirmeyi kaldırma](media/author-visually/remove-repo2.png)

Geçerli depoyla ilişkilendirmeyi kaldırdıktan sonra, git ayarlarınızı farklı bir depoyu kullanacak şekilde yapılandırabilir ve ardından mevcut Data Factory kaynaklarını yeni depoya içeri aktarabilirsiniz.

> [!IMPORTANT]
> Bir veri fabrikasından git yapılandırmasını kaldırma, depodan herhangi bir şeyi silmez. Fabrika, yayımlanan tüm kaynakları içerecektir. Fabrikaya doğrudan hizmete karşı düzenleme yapmaya devam edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* İşlem hatlarını izleme ve yönetme hakkında daha fazla bilgi edinmek için bkz. işlem [hatlarını programlama yoluyla izleme ve yönetme](monitor-programmatically.md).
* Sürekli tümleştirme ve dağıtım uygulamak için [Azure Data Factory Içinde sürekli tümleştirme ve teslim (CI/CD)](continuous-integration-deployment.md)konusuna bakın.

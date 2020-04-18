---
title: Kaynak denetimi
description: Azure Veri Fabrikası'nda kaynak denetimini nasıl yapılandırılabildiğini öğrenin
services: data-factory
ms.service: data-factory
ms.workload: data-services
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.reviewer: ''
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 01/09/2019
ms.openlocfilehash: 3007865c15ceb03b104282c29179ec59a8196b38
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81604596"
---
# <a name="source-control-in-azure-data-factory"></a>Azure Veri Fabrikası'nda kaynak denetimi
[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Azure Veri Fabrikası kullanıcı arabirimi deneyimi (UX) görsel yazarlık için iki deneyime sahiptir:

- Doğrudan Veri Fabrikası hizmetiile yazar
- Azure Repos Git veya GitHub tümleştirmesi ile yazar

> [!NOTE]
> Azure Kamu Bulutu'nda yalnızca doğrudan Veri Fabrikası hizmetiyle yazma desteklenir.

## <a name="author-directly-with-the-data-factory-service"></a>Doğrudan Veri Fabrikası hizmetiile yazar

Doğrudan Veri Fabrikası hizmetiyle yazarken, değişiklikleri kaydetmenin tek yolu **Tümlerini Yayımla** düğmesi üzerinden dir. Tıkladıktan sonra yaptığınız tüm değişiklikler doğrudan Veri Fabrikası hizmetine yayınlanır. 

![Yayımlama modu](media/author-visually/data-factory-publish.png)

Doğrudan Veri Fabrikası hizmetiyle yazmanın aşağıdaki sınırlamalar vardır:

- Veri Fabrikası hizmeti, değişiklikleriniz için JSON varlıklarını depolamak için bir depo içermez.
- Veri Fabrikası hizmeti işbirliği veya sürüm denetimi için optimize edilmiş değildir.

> [!NOTE]
> Git deposu yapılandırıldığında Azure Veri Fabrikası UX'de doğrudan Veri Fabrikası hizmetiyle yazma devre dışı bırakılır. Değişiklikler powershell veya SDK üzerinden doğrudan hizmete yapılabilir.

## <a name="author-with-azure-repos-git-integration"></a>Azure Repos Git tümleştirmesi ile yazma

Azure Repos Git tümleştirmesi ile görsel yazma, veri fabrikası ardışık hatlarınızda çalışmak için kaynak denetimini ve işbirliğini destekler. Bir veri fabrikasını kaynak denetimi, işbirliği, sürümleme ve benzeri için bir Azure Repos Git kuruluş deposuyla ilişkilendirebilirsiniz. Tek bir Azure Repos Git kuruluşunun birden çok deposu olabilir, ancak Azure Depoları Git deposu yalnızca bir veri fabrikasıyla ilişkilendirilebilir. Bir Azure Deposu kuruluşunuz veya deponuz yoksa, kaynaklarınızı oluşturmak için [aşağıdaki yönergeleri](https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization-msa-or-work-student) izleyin.

> [!NOTE]
> Komut dosyası ve veri dosyalarını Azure Repos Git deposunda depolayabilirsiniz. Ancak, dosyaları el ile Azure Depolama'ya yüklemeniz gerekir. Veri Fabrikası ardışık hattı, Azure Deposu'nda depolanan komut dosyalarını veya veri dosyalarını otomatik olarak Azure Depolama'ya yüklemez.

### <a name="configure-an-azure-repos-git-repository-with-azure-data-factory"></a>Azure Veri Fabrikası ile Azure Repos Git deposunu yapılandırma

Bir Azure Repos Git deposunu veri fabrikasıyla iki yöntemle yapılandırabilirsiniz.

#### <a name="configuration-method-1-azure-data-factory-home-page"></a>Yapılandırma yöntemi 1: Azure Veri Fabrikası ana sayfası

Azure Veri Fabrikası ana sayfasında **Kod Deposu Ayarla'yı**seçin.

![Azure Deposu kod deposunu yapılandırma](media/author-visually/configure-repo.png)

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Yapılandırma yöntemi 2: UX yazma tuval
Azure Veri Fabrikası UX yazma tuvalinde, **Veri Fabrikası** açılır menüsünü seçin ve ardından **Kod Deposu'nu Ayarla'yı**seçin.

![UX yazarlık için kod deposu ayarlarını yapılandırın](media/author-visually/configure-repo-2.png)

Her iki yöntem de depo ayarları yapılandırma bölmesini açar.

![Kod deposu ayarlarını yapılandırma](media/author-visually/repo-settings.png)

Yapılandırma bölmesi aşağıdaki Azure Repos kod deposu ayarlarını gösterir:

| Ayar | Açıklama | Değer |
|:--- |:--- |:--- |
| **Depo Tipi** | Azure Depoları kod deposunun türü.<br/> | Azure DevOps Git veya GitHub |
| **Azure Active Directory** | Azure AD kiracı adınız. | `<your tenant name>` |
| **Azure Repos Organizasyonu** | Azure Repos kuruluş adınız. Azure Repos kuruluş adınızı ' `https://{organization name}.visualstudio.com`da bulabilirsiniz. Visual Studio profilinize erişmek ve depolarınızı ve projelerinizi görmek için [Azure Repos kuruluşunuzda oturum](https://www.visualstudio.com/team-services/git/) açabilirsiniz. | `<your organization name>` |
| **Projeadı** | Azure Repos proje adınız. Azure Repos proje adınızı ' `https://{organization name}.visualstudio.com/{project name}`da bulabilirsiniz. | `<your Azure Repos project name>` |
| **DepoAdı** | Azure Depo kodu depo adınız. Azure Repos projeleri, projeniz büyüdükçe kaynak kodunuzu yönetmek için Git depoları içerir. Yeni bir depo oluşturabilir veya projenizde zaten bulunan varolan bir depoyı kullanabilirsiniz. | `<your Azure Repos code repository name>` |
| **İşbirliği şubesi** | Yayımlama için kullanılan Azure Repos işbirliği dalınız. Varsayılan olarak, `master`bu. Başka bir daldan kaynak yayımlamak istiyorsanız bu ayarı değiştirin. | `<your collaboration branch name>` |
| **Kök klasörü** | Azure Repos işbirliği dalınızdaki kök klasörünüz. | `<your root folder name>` |
| **Varolan Veri Fabrikası kaynaklarını depoya aktarma** | Varolan veri fabrikası kaynaklarını UX Yazma **tuvalinden** Azure Repos Git deposuna aktarıp aktarmayacağı belirtir. Veri fabrikası kaynaklarınızı JSON formatında ilişkili Git deposuna almak için kutuyu seçin. Bu eylem her kaynağı ayrı ayrı dışa aktarıyor (diğer bir süre, bağlı hizmetler ve veri kümeleri ayrı JSON'lara dışa aktarılır). Bu kutu seçilmediğinde, varolan kaynaklar içe aktarılmaz. | Seçili (varsayılan) |
| **Kaynak almak için şube** | Veri fabrikası kaynaklarının (boru hatları, veri kümeleri, bağlantılı hizmetler vb.) hangi dalda alındığını belirtir. Kaynakları aşağıdaki dallardan birine aktarabilirsiniz: a. İşbirliği b. Yeni c oluşturun. Mevcut Kullanımı |  |

> [!NOTE]
> Microsoft Edge kullanıyorsanız ve Azure DevOps Hesabı açılır bırakma nızda herhangi bir değer görmüyorsanız, https://*.visualstudio.com adresini güvenilir siteler listesine ekleyin.

### <a name="use-a-different-azure-active-directory-tenant"></a>Farklı bir Azure Etkin Dizin kiracısı kullanma

Azure Repos Git deposunu farklı bir Azure Active Directory kiracısında oluşturabilirsiniz. Farklı bir Azure AD kiracısı belirtmek için kullandığınız Azure aboneliğinde yönetici izinlerine sahip olmanız gerekir.

### <a name="use-your-personal-microsoft-account"></a>Kişisel Microsoft hesabınızı kullanma

Git tümleştirmesi için kişisel bir Microsoft hesabı kullanmak için, kişisel Azure Repo'nuzu kuruluşunuzun Etkin Dizini'ne bağlayabilirsiniz.

1. Kişisel Microsoft hesabınızı kuruluşunuzun Etkin Dizini'ne konuk olarak ekleyin. Daha fazla bilgi için azure [portalındaki Azure Etkin Dizin B2B işbirliği kullanıcıları ekle'ye](../active-directory/b2b/add-users-administrator.md)bakın.

2. Kişisel Microsoft hesabınızla Azure portalında oturum açın. Ardından kuruluşunuzun Etkin Dizini'ne geçin.

3. Azure DevOps bölümüne gidin ve burada artık kişisel reponuzu görün. Repo'yu seçin ve Active Directory'ye bağlanın.

Bu yapılandırma adımlarını takiben, Veri Fabrikası UI'sinde Git tümleştirmesini ayarladığınızda kişisel repo'nuz kullanılabilir.

Azure Repos'u kuruluşunuzun Etkin Dizinine bağlama hakkında daha fazla bilgi için Azure [DevOps kuruluşunuzu Azure Etkin Dizine Bağlayın'a](/azure/devops/organizations/accounts/connect-organization-to-azure-ad)bakın.

## <a name="author-with-github-integration"></a>GitHub tümleştirmesi ile yazma

GitHub tümleştirmesi ile görsel yazma, veri fabrikası ardışık hatlarınızda çalışmak için kaynak denetimini ve işbirliğini destekler. Bir veri fabrikasını kaynak denetimi, işbirliği ve sürüm için github hesap deposuyla ilişkilendirebilirsiniz. Tek bir GitHub hesabının birden çok deposu olabilir, ancak GitHub deposu yalnızca bir veri fabrikasıyla ilişkilendirilebilir. GitHub hesabınız veya deponuz yoksa, kaynaklarınızı oluşturmak için [aşağıdaki yönergeleri](https://github.com/join) izleyin.

Veri Fabrikası ile GitHub entegrasyonu hem ortak GitHub (yani) [https://github.com](https://github.com)ve GitHub Enterprise destekler. GitHub'daki depoya izin okuduğunuz ve yazdığınız sürece Veri Fabrikası ile hem genel hem de özel GitHub depolarını kullanabilirsiniz.

Bir GitHub repo'su yapılandırmak için, kullanmakta olduğunuz Azure aboneliği için yönetici izinlerine sahip olmalısınız.

Bu özelliğin dokuz dakikalık tanıtımı ve gösterimi için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Azure-Data-Factory-visual-tools-now-integrated-with-GitHub/player]

### <a name="configure-a-github-repository-with-azure-data-factory"></a>Azure Veri Fabrikası ile GitHub deposunu yapılandırma

Bir GitHub deposunu veri fabrikasıyla iki yöntemle yapılandırabilirsiniz.

#### <a name="configuration-method-1-azure-data-factory-home-page"></a>Yapılandırma yöntemi 1: Azure Veri Fabrikası ana sayfası

Azure Veri Fabrikası ana sayfasında **Kod Deposu Ayarla'yı**seçin.

![Azure Deposu kod deposunu yapılandırma](media/author-visually/configure-repo.png)

#### <a name="configuration-method-2-ux-authoring-canvas"></a>Yapılandırma yöntemi 2: UX yazma tuval

Azure Veri Fabrikası UX yazma tuvalinde, **Veri Fabrikası** açılır menüsünü seçin ve ardından **Kod Deposu'nu Ayarla'yı**seçin.

![UX yazarlık için kod deposu ayarlarını yapılandırın](media/author-visually/configure-repo-2.png)

Her iki yöntem de depo ayarları yapılandırma bölmesini açar.

![GitHub depo ayarları](media/author-visually/github-integration-image2.png)

Yapılandırma bölmesi aşağıdaki GitHub depo ayarlarını gösterir:

| **Ayar** | **Açıklama**  | **Değer**  |
|:--- |:--- |:--- |
| **Depo Tipi** | Azure Depoları kod deposunun türü. | GitHub |
| **GitHub Kurumsal'ı kullanma** | GitHub Enterprise'ı seçmek için onay kutusu | seçilmemiş (varsayılan) |
| **GitHub Kurumsal URL** | GitHub Enterprise kök URL'si (yerel GitHub Enterprise sunucusu için HTTPS olmalıdır). Örneğin: https://github.mydomain.com. Yalnızca **GitHub Kurumsal'ı Kullan** seçilirse gereklidir | `<your GitHub enterprise url>` |                                                           
| **GitHub hesabı** | GitHub hesap adınız. Bu ad https:\//github.com/{account name}/{repository name} adresindebulunabilir. Bu sayfada gezinmek, GitHub OAuth kimlik bilgilerini GitHub hesabınıza girmenizi ister. | `<your GitHub account name>` |
| **Depo Adı**  | GitHub kod depo adınız. GitHub hesapları, kaynak kodunuzu yönetmek için Git depoları içerir. Yeni bir depo oluşturabilir veya hesabınızda zaten bulunan varolan bir depoyı kullanabilirsiniz. | `<your repository name>` |
| **İşbirliği şubesi** | Yayımlama için kullanılan GitHub işbirliği dalın. Varsayılan olarak, bu ana. Başka bir daldan kaynak yayımlamak istiyorsanız bu ayarı değiştirin. | `<your collaboration branch>` |
| **Kök klasörü** | GitHub işbirliği dalınızdaki kök klasörünüz. |`<your root folder name>` |
| **Varolan Veri Fabrikası kaynaklarını depoya aktarma** | Varolan veri fabrikası kaynaklarının UX yazma tuvalinden GitHub deposuna aktarılıp aktarılmayacağını belirtir. Veri fabrikası kaynaklarınızı JSON formatında ilişkili Git deposuna almak için kutuyu seçin. Bu eylem her kaynağı ayrı ayrı dışa aktarıyor (diğer bir süre, bağlı hizmetler ve veri kümeleri ayrı JSON'lara dışa aktarılır). Bu kutu seçilmediğinde, varolan kaynaklar içe aktarılmaz. | Seçili (varsayılan) |
| **Kaynak almak için şube** | Veri fabrikası kaynaklarının (boru hatları, veri kümeleri, bağlantılı hizmetler vb.) hangi dalda alındığını belirtir. Kaynakları aşağıdaki dallardan birine aktarabilirsiniz: a. İşbirliği b. Yeni c oluşturun. Mevcut Kullanımı |  |

### <a name="known-github-limitations"></a>Bilinen GitHub sınırlamaları

- Komut dosyası ve veri dosyalarını GitHub deposunda depolayabilirsiniz. Ancak, dosyaları el ile Azure Depolama'ya yüklemeniz gerekir. Veri Fabrikası ardışık hattı, GitHub deposunda depolanan komut dosyalarını veya veri dosyalarını otomatik olarak Azure Depolama'ya yüklemez.

- 2.14.0'dan eski bir sürümü olan GitHub Enterprise, Microsoft Edge tarayıcısında çalışmaz.

- Veri Fabrikası görsel yazma araçları ile GitHub entegrasyonu yalnızca Veri Fabrikası'nın genel sürümünde çalışır.

- Tek bir GitHub dalından kaynak türü başına en fazla 1.000 varlık (ardışık hatlar ve veri kümeleri gibi) alınabilir. Bu sınıra ulaşılırsa, kaynaklarınızı ayrı fabrikalara bölmeniz önerilir. Azure DevOps Git'in bu sınırlaması yoktur.

## <a name="switch-to-a-different-git-repo"></a>Farklı bir Git repo'ya geçiş

Farklı bir Git repo'ya geçmek için, Veri Fabrikası genel bakış sayfasının sağ üst köşesindeki **Git Repo Ayarları** simgesine tıklayın. Simgeyi göremiyorsanız, yerel tarayıcı önbelleğinizi temizleyin. Geçerli repo ile ilişkilendirme kaldırmak için simgeyi seçin.

![Git simgesi](media/author-visually/remove-repo.png)

Depo Ayarları bölmesi görüntüledikten sonra **Git'i kaldır'ı**seçin. Veri fabrikanızla ilişkili Git deposunu kaldırmak için veri fabrika adınızı girin ve **onayla'yı** tıklatın.

![Geçerli Git reposuyla ilişkilendirme kaldırma](media/author-visually/remove-repo2.png)

Geçerli repo ile ilişkilendirme kaldırdıktan sonra, git ayarlarınızı farklı bir repo kullanacak şekilde yapılandırabilir ve sonra varolan Veri Fabrikası kaynaklarını yeni repo'ya aktarabilirsiniz. 

## <a name="version-control"></a>Sürüm denetimi

Sürüm denetim sistemleri _(kaynak denetimi_olarak da bilinir), geliştiricilerin kod üzerinde işbirliği yapmasına ve kod tabanında yapılan değişiklikleri izlemesine izin verir. Kaynak denetimi, çok geliştiricili projeler için önemli bir araçtır.

### <a name="creating-feature-branches"></a>Özellik dalları oluşturma

Bir veri fabrikasıyla ilişkili her Azure Repos Git deposunun bir işbirliği dalı vardır. (`master` varsayılan işbirliği dalıdır). Kullanıcılar ayrıca şube açılır ayında **+ Yeni Şube'yi** tıklayarak özellik dalları oluşturabilirler. Yeni dal bölmesi görüntüledikten sonra özellik dalının adını girin.

![Yeni bir dal oluşturma](media/author-visually/new-branch.png)

Özellik dalınızdaki değişiklikleri işbirliği dalınıza birleştirmeye hazır olduğunuzda, şube açılır düğmesine tıklayın ve **çekme isteği oluştur'u**seçin. Bu eylem, çekme isteklerini yükseltebileceğiniz, kod incelemeleri yapabileceğiniz ve işbirliği dalınızdaki değişiklikleri birleştirebileceğiniz Azure Repos Git'e götürür. (`master` varsayılan dır). Yalnızca işbirliği şubenizden Veri Fabrikası hizmetinde yayımlama izniniz vardır. 

![Yeni çekme isteği oluşturma](media/author-visually/create-pull-request.png)

### <a name="configure-publishing-settings"></a>Yayımlama ayarlarını yapılandırma

Yayımlama dalını yapılandırmak için - yani Kaynak Yöneticisi şablonlarının `publish_config.json` kaydedildiği dal - işbirliği dalındaki kök klasörüne bir dosya ekleyin. Veri Fabrikası bu dosyayı okur, alanı `publishBranch`arar ve sağlanan değerle birlikte yeni bir dal (zaten yoksa) oluşturur. Ardından, tüm Kaynak Yöneticisi şablonlarını belirtilen konuma kaydeder. Örneğin:

```json
{
    "publishBranch": "factory/adf_publish"
}
```

Yeni bir yayımlama dalı belirttiğiniz zaman, Veri Fabrikası önceki yayımlama dalını silmez. Önceki yayımlama dalını kaldırmak istiyorsanız, el ile silin.

> [!NOTE]
> Veri Fabrikası dosyayı yalnızca fabrikayı `publish_config.json` yüklerken okur. Fabrikayı zaten portala yüklediyseniz, değişikliklerinizin etkili olması için tarayıcıyı yenileyin.

### <a name="publish-code-changes"></a>Kod değişikliklerini yayımlama

İşbirliği dalındaki değişiklikleri birleştirdikten sonra`master` (varsayılan değerdir), ana daldaki kod değişikliklerinizi Veri Fabrikası hizmetinde el ile yayımlamak için **Yayımla'yı** tıklatın.

![Veri Fabrikası hizmetinde değişiklik yayımlama](media/author-visually/publish-changes.png)

Yayımlama dalı ve bekleyen değişikliklerin doğru olduğunu onayladığınız bir yan bölme açılır. Değişikliklerinizi doğruladıktan sonra yayımlamayı onaylamak için **Tamam'ı** tıklatın.

![Doğru yayımlama dalını onaylama](media/author-visually/configure-publish-branch.png)

> [!IMPORTANT]
> Ana şube, Veri Fabrikası hizmetinde dağıtılanları temsil etmez. Ana *şube,* Veri Fabrikası hizmetine el ile yayınlanmalıdır.

## <a name="advantages-of-git-integration"></a>Git tümleştirmesinin avantajları

-   **Kaynak Denetimi**. Veri fabrikası iş yüklerin önemli hale geldikçe, aşağıdaki gibi çeşitli kaynak denetimi avantajlarından yararlanmak için fabrikanızı Git ile bütünleştirmek isteyebilirsiniz:
    -   Değişiklikleri izleme/denetleme becerisi.
    -   Hataları ortaya çıkan değişiklikleri geri alabilme yeteneği.
-   **Kısmi Kaydeder.** Fabrikanızda birçok değişiklik yaptığınızda, normal LIVE modunda, değişikliklerinizi taslak olarak kaydedemediğinizi, hazır olmadığınız veya bilgisayarınızın çökmesi durumunda değişikliklerinizi kaybetmek istemediğiniz için fark esiniz. Git tümleştirmesi ile değişikliklerinizi aşamalı olarak kaydetmeye devam edebilir ve yalnızca hazır olduğunuzda fabrikada yayımlayabilirsiniz. Git, memnuniyetinizdeki değişikliklerinizi test edene kadar işiniz için bir sahneleme yeri görevi görür.
-   **İşbirliği ve Kontrol**. Aynı fabrikaya katılan birden fazla ekip üyeniz varsa, bir kod inceleme işlemi yoluyla takım arkadaşlarınızın birbiriyle işbirliği yapmasına izin vermek isteyebilirsiniz. Ayrıca, fabrikaya katkıda bulunan her katılımcının fabrikaya dağıtım izni olmayacak şekilde fabrikanızı da ayarlayabilirsiniz. Ekip üyelerinin Git aracılığıyla değişiklik yapmasına izin verilebilir, ancak yalnızca takımdaki belirli kişilerin değişiklikleri fabrikada "Yayımlamasına" izin verilebilir.
-   **Diffs gösteriliyor.** Git modunda, fabrikaya yayınlanmak üzere olan yükün güzel bir kısmını görebilirsiniz. Bu diff, fabrikanızda son yayınladığınızdan beri değiştirilen/eklenen/silinen tüm kaynakları/varlıkları gösterir. Bu diff dayanarak, ya yayımlama ile daha fazla devam edebilir, ya da geri dönüp değişikliklerinizi kontrol ve sonra geri gel.
-   **Daha iyi CI / CD**. Git modunu kullanıyorsanız, geliştirme fabrikasında herhangi bir değişiklik yapılır yapılmaz sürüm ardışık sisteminizi otomatik olarak tetiklemek üzere yapılandırabilirsiniz. Ayrıca, Fabrikanızda Kaynak Yöneticisi şablonunda parametre olarak kullanılabilen özellikleri özelleştirebilirsiniz. Yalnızca gerekli özellik kümesini parametreler olarak tutmak ve diğer her şeyin sabit kodlanmış olması yararlı olabilir.
-   **Daha Iyi Performans**. Kaynaklar Git üzerinden indirilir, çünkü ortalama bir fabrika, git modunda normal LIVE modundan on kat daha hızlı yükler.

## <a name="best-practices-for-git-integration"></a>Git entegrasyonu için en iyi uygulamalar

### <a name="permissions"></a>İzinler

Genellikle her ekip üyesinin fabrikayı güncelleştirme izinlerine sahip olmasını istemezsiniz. Aşağıdaki izin ayarları önerilir:

*   Tüm ekip üyelerinin veri fabrikasına izinleri okumuş olması gerekir.
*   Yalnızca belirli bir kişi kümesinin fabrikada yayımlamasına izin verilmelidir. Bunu yapmak için, fabrikanın içinde olduğu kaynak grubunda **Veri Fabrikası katılımcısıfatıne** sahip olmaları gerekir. İzinler hakkında daha fazla bilgi için [Azure Veri Fabrikası'nın Rolleri ve izinlerine](concepts-roles-permissions.md)bakın.
   
İşbirliği şubesine doğrudan check-in'lere izin vermemek önerilir. Bu kısıtlama, her iade [özelliği daloluşturma'da](source-control.md#creating-feature-branches)açıklanan bir çekme isteği gözden geçirme sürecinden geçeceği için hataları önlemeye yardımcı olabilir.

### <a name="using-passwords-from-azure-key-vault"></a>Azure Key Vault'tan parola kullanma

Veri Fabrikası Bağlantılı Hizmetler için bağlantı dizelerini veya parolalarını depolamak için Azure Key Vault'un kullanılması önerilir. Güvenlik nedeniyle, bu tür gizli bilgileri Git'de saklamayız, bu nedenle Bağlantılı Hizmetlerdeki değişiklikler hemen Azure Veri Fabrikası hizmetine yayınlanır.

Anahtar Kasa'yı kullanmak, Kaynak Yöneticisi şablonu dağıtımı sırasında bu sırları sağlamak zorunda kalmadığınız için sürekli tümleştirme ve dağıtımı da kolaylaştırır.

## <a name="troubleshooting-git-integration"></a>Sorun giderme Git tümleştirmesi

### <a name="stale-publish-branch"></a>Bayat yayımlama dalı

Yayımlama dalı ana dalla eşitlenmemişse ve son yayımlamaya rağmen güncel olmayan kaynaklar içeriyorsa, aşağıdaki adımları izleyin:

1. Geçerli Git deponuzu kaldırın
1. Git'i aynı ayarlarla yeniden yapılandırın, ancak **varolan Veri Fabrikası kaynaklarını depoya aktarıldığından** emin olun ve **Yeni şubeyi** seçin
1. Değişiklikleri işbirliği dalında birleştirmek için çekme isteği oluşturma 

Aşağıda, eski bir yayımlama dalı neden olabilecek durumlara bazı örnekler verilmiştir:
- Bir kullanıcının birden çok şubesi vardır. Bir özellik dalında, AKV ilişkili olmayan bir bağlantılı hizmeti sildiler (AKV bağlantılı olmayan hizmetler Git'de olup olmadıklarına bakılmaksızın hemen yayınlanır) ve özellik dalını işbirliği brnach'ında birleştirmediler.
- Bir kullanıcı SDK veya PowerShell kullanarak veri fabrikasını değiştirdi
- Bir kullanıcı tüm kaynakları yeni bir dala taşıdı ve ilk kez yayımlamaya çalıştı. Bağlantılı hizmetler kaynakları aktarırken el ile oluşturulmalıdır.
- Kullanıcı, AKV'ye bağlı olmayan bir hizmeti veya Integration Runtime JSON'u el ile yükler. Bu kaynağa veri kümesi, bağlantılı hizmet veya ardışık sistem gibi başka bir kaynaktan başvururlar. Kimlik bilgilerinin şifrelenilmesi gerektiğinden, UX aracılığıyla oluşturulan AKV bağlantısı olmayan bir hizmet hemen yayımlanır. Bağlı hizmete atıfta bulunan bir veri kümesi yükler ve yayımlamaya çalışırsanız, UX buna izin verir, çünkü git ortamında bulunur. Veri fabrikası hizmetinde bulunmadığından yayımlama zamanında reddedilir.

## <a name="provide-feedback"></a>Geri bildirimde bulunma
Özellikler hakkında yorum yapmak veya araçla ilgili sorunlar hakkında Microsoft'a bilgi vermek için **Geri Bildirim'i** seçin:

![Geri Bildirim](media/author-visually/provide-feedback.png)

## <a name="next-steps"></a>Sonraki adımlar

* Boru hatlarını izleme ve yönetme hakkında daha fazla bilgi edinmek [için, denetim kanallarını programlı olarak izleyin ve yönetin.](monitor-programmatically.md)
* Sürekli tümleştirme ve dağıtım uygulamak için [Azure Veri Fabrikası'nda Sürekli tümleştirme ve teslim (CI/CD)](continuous-integration-deployment.md)bakın.

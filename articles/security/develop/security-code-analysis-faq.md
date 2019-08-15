---
title: Microsoft Azure Güvenlik kodu çözümleme belgeleri SSS
description: Bu makale, güvenlik kodu analiz uzantısı hakkında SSS içerir
author: vharindra
manager: sukhans
ms.author: terrylan
ms.date: 07/31/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 8038b7bd60ac771c798a1a8645022b0bf9e142a9
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68934841"
---
# <a name="frequently-asked-questions"></a>Sık sorulan sorular
Sorularınız mı var? Daha fazla bilgi için aşağıdaki SSS bölümüne bakın.

## <a name="general-faqs"></a>Genel SSS

### <a name="can-i-install-the-extension-on-my-tfs-not-azure-devops-server"></a>Uzantıyı TFS (Azure DevOps) sunucusuna yükleyebilir miyim? 

Hayır, uzantı, TFS için indirme ve yükleme için kullanılamaz.

### <a name="do-i-have-to-run-microsoft-security-code-analysis-with-my-build"></a>Microsoft Güvenlik kodu çözümlemesini benim yapımda çalıştırdım mı? 

Evet ve hayır. Analiz Aracı türüne bağlı olarak, kaynak kodun kendisi gereken tek şey veya derleme çıktısı gerekli olabilir. Örneğin, kimlik bilgisi tarayıcısı kod deposunun klasör yapısındaki dosyaları analiz eder, böylece sonuçları almak için kimlik bilgisi tarayıcısını çalıştırabilir ve güvenlik Analizi günlüklerini tek başına derlemede yayımlayabilirsiniz.
Binskım gibi derleme yapıtlarını analiz eden diğer araçlar için öncelikle derleme gerekecektir.

### <a name="can-i-break-my-build-when-results-are-found"></a>Sonuçlar bulunduğunda derlemeden bölebilir miyim? 
Evet, herhangi bir araç günlük dosyasında bir sorun (bulma) bildirdiğinde, derleme kesmeyi ortaya çıkarabilir. Analiz sonrası derleme görevini eklemeniz ve derlemeyi bölmek istediğiniz herhangi bir araçla ilgili onay kutusunu işaretlemeniz yeterlidir. Analiz sonrası görev Kullanıcı arabiriminde herhangi bir araç hata veya uyarı ve hata bildirdiğinde derlemeyi kesebilirsiniz seçeneğini belirleyebilirsiniz.

### <a name="how-are-the-command-line-arguments-different-in-azure-devops-than-they-are-in-the-standalone-desktop-tools"></a>Azure DevOps 'daki komut satırı bağımsız değişkenleri, tek başına masaüstü araçlarında olduklarından farklı midir? 

Çoğu bölümde, Azure DevOps derleme görevleri, güvenlik araçlarının komut satırı bağımsız değişkenleri etrafında doğrudan sarmalayıcılardır. Her şey, masaüstünüzdeki komut satırındaki araca doğrudan geçirebilmeniz için, yapı görevinin bağımsız değişkenler girişine geçirebilirsiniz.
Belirgin farklılıkların bir listesi aşağıda verilmiştir:
 - Araç, $ (Build. SourcesDirectory) veya% BUILD_SOURCESDIRECTORY% aracının kaynak klasöründen yürütülecektir. Örnek: C:\agent\_work\1\s 
 - Bağımsız değişkenlerdeki yollar, yukarıda listelenen kaynak dizinin köküne veya yerel kaynakların bilinen dağıtım konumlarına sahip şirket içi bir aracı çalıştırılarak ya da Azure DevOps derleme değişkenlerini kullanarak mutlak olabilir
 - Araçlar, bir çıkış yolu sağlandığında otomatik olarak bir çıkış dosyası yolu veya klasörü sağlar, bu, kaldırılacak ve derleme aracısındaki iyi bilinen günlüklerimizin bir yolu ile değiştirilirler
 - Bazı ek komut satırı parametreleri, GUI 'nin başlatılmadığından emin olmak için seçeneklerin eklenmesi veya kaldırılması gibi bazı araçlarda ayıklanmış ve kaldırılır.

### <a name="can-i-run-a-build-task-for-example-credential-scanner-across-multiple-repositories-in-an-azure-devops-build"></a>Bir Azure DevOps derlemesinde birden çok depoda derleme görevi (örneğin, kimlik bilgisi tarayıcısı) çalıştırabilir miyim? 

Hayır, tek bir işlem hattında birden fazla depoda güvenli geliştirme araçlarının çalıştırılması şu anda desteklenmiyor.

###  <a name="the-output-file-i-specified-is-not-being-created--i-cant-find-the-output-file-i-specified"></a>Belirtdiğim çıkış dosyası oluşturulamıyor/belirtidiğim çıkış dosyasını bulamıyorum 

Derleme görevleri şu anda Kullanıcı girişini temizler ve derleme aracısında ortak bir konuma oluşturulan çıkış dosyasının konumunu güncelleştirmez. Bu konum hakkında daha fazla bilgi için aşağıdaki sorulara bakın.

### <a name="where-are-the-output-files-generated-by-the-tools-saved"></a>Araçlar tarafından oluşturulan çıkış dosyaları nerede kaydedilir? 

Yapı görevleri, derleme aracısı $ (Agent. BuildDirectory)\_sdt\logs. içindeki aşağıdaki iyi bilinen konuma çıkış yollarını otomatik olarak ekler. Bu konumda standartlaştırarak, kod analizi günlükleri üreten veya bunları kullanan diğer takımların erişime sahip olacağını garanti edebiliriz.

### <a name="can-i-queue-a-build-to-run-these-tasks-on-a-hosted-build-agent"></a>Bu görevleri barındırılan bir yapı aracısında çalıştırmak için bir derlemeyi sıraya alabilir miyim? 

Evet, uzantıdaki tüm görevler ve araçlar barındırılan bir yapı aracısında yürütülebilir.

>[!NOTE]
> Kötü amaçlı yazılımdan koruma oluşturma görevi, "barındırılan VS2017" veya sonraki derleme aracılarında doğru olan Windows Defender 'ın etkinleştirildiği bir derleme Aracısı gerektirir. (Eski/VS2015 "barındırılan" aracısında çalışmaz.) İmzalar bu aracılarda güncelleştirilemez, ancak imza her zaman görece geçerli olmalıdır ve 3 saatten daha eski olmalıdır.
>

### <a name="can-i-run-these-build-tasks-as-part-of-a-release-pipeline-as-opposed-to-a-build-pipeline"></a>Bu derleme görevlerini bir yayın işlem hattının parçası olarak çalıştırabilir miyim (bir derleme işlem hattının aksine)? 
Çoğu durumda, evet. Ancak, yapıtları yayınlayan görevler, Azure DevOps tarafından, yayın işlem hatları içinden çalıştırılacak şekilde desteklenmez: "Yayın ile çalışması beklenmediği tek görev kategorisi, yapıtlar yayımladıklardır. Bunun nedeni, şu andan itibaren yayın içinde yapıtları yayımlamayı desteklememiz değildir.
Bu, "güvenlik çözümlemesi günlüklerini Yayımla" görevinin bir yayın ardışık düzeninde başarıyla çalışmasını önler; açıklayıcı bir hata iletisiyle başarısız olur.

### <a name="from-where-do-the-build-tasks-download-the-tools"></a>Derleme görevleri nereden araçlar indirir? 
Yapı görevleri a) aşağıdaki [Azure DevOps paket yönetimi akışından](https://securitytools.pkgs.visualstudio.com/_packaging/SecureDevelopmentTools/nuget/v3/index.json) veya yapı aracısında önceden yüklenmiş olması gereken düğüm Paket Yöneticisi ' ni kullanarak araçlar için NuGet paketlerini indirin (örnek: "NPM install tslint").

### <a name="what-effect-will-installing-the-extension-have-on-my-azure-devops-organization"></a>Uzantıyı hangi etkimin yükleyeceği, Azure DevOps Kuruluşum üzerinde mi? 

Yükleme sonrasında, uzantı tarafından sağlanan güvenlik derleme görevleri kuruluşunuzdaki tüm kullanıcılar tarafından kullanılabilir hale gelir. Bir Azure işlem hattı oluştururken veya düzenlenirken, bu görevler yapı görev koleksiyonu listesinden eklemek için kullanılabilir olacaktır. Aksi halde, uzantıyı Azure DevOps kuruluşunuzda yüklemek hiçbir etkiye sahip değildir. Herhangi bir hesap veya proje ayarını veya işlem hatlarını değiştirmez.

### <a name="will-installing-the-extension-modify-my-existing-azure-pipelines"></a>Uzantıyı yüklemek var olan Azure Pipelines değiştirilsin mi? 

Hayır. Uzantının yüklenmesi, güvenlik yapı görevlerini Azure Pipelines eklemek için kullanılabilir hale getirir. Araçları yapı sürecinizdeki tümleştirmeye yönelik derleme tanımları eklemek veya güncelleştirmek için yine de gereklidir.

## <a name="task-specific-faqs"></a>Göreve özgü SSS

Derleme görevlerine özgü SSS 'ler, bu bölümde listelenecektir.

### <a name="credential-scanner-faqs"></a>Kimlik bilgisi tarayıcısı SSS

#### <a name="what-are-common-suppressions-scenarios-and-examples"></a>Yaygın gizlemeler senaryoları ve örnekleri nelerdir? 
En yaygın gizleme senaryolarından ikisi aşağıda ayrıntılı olarak verilmiştir:
##### <a name="suppress-all-occurrences-of-a-given-secret-within-the-specified-path"></a>Belirli bir gizli dizi içindeki tüm oluşumları belirtilen yolda gösterme 
Aşağıdaki örnekte gösterildiği gibi, kimlik bilgisi tarayıcısı çıkış dosyasındaki gizli dizinin karma anahtarı gereklidir
   
        {
            "tool": "Credential Scanner",
            "suppressions": [
            {
                "hash": "CLgYxl2FcQE8XZgha9/UbKLTkJkUh3Vakkxh2CAdhtY=",
                "_justification": "Secret used by MSDN sample, it is fake."
            }
          ]
        }

>[!WARNING]
> Karma anahtar, eşleşen değerin veya dosya içeriğinin bir kısmı tarafından oluşturulur. Herhangi bir kaynak kodu düzeltmesi, karma anahtarı değiştirebilir ve gizleme kuralını devre dışı bırakabilir. 

##### <a name="to-suppress-all-secrets-in-a-specified-file-or-to-suppress-the-secrets-file-itself"></a>Belirli bir dosyadaki tüm gizli dizileri bastırmak için (veya gizli dizileri dosyanın kendisini bastırmak için) 
Dosya ifadesi bir dosya adı veya tam dosya yolu/adının herhangi bir sonek kısmı olabilir. Joker karakterler desteklenmez. 

**Örnek** 

Bastırılacak dosya: [ınputpath] \src\JS\lib\angular.js 

Geçerli gizleme kuralları: 
- [Inputpath] \src\JS\lib\angular.js--belirtilen yoldaki dosyayı gösterme
- \src\JS\lib\angular.js
- \JS\lib\angular.js
- \lib\angular.js
- Angular. js--aynı ada sahip herhangi bir dosyayı gösterme

        {
            "tool": "Credential Scanner",
            "suppressions": [
            {
                "file": "\\files\\AdditonalSearcher.xml", 
                "_justification": "Additional CredScan searcher specific to my team"
            },
            {
                "file": "\\files\\unittest.pfx", 
                "_justification": "Legitimate UT certificate file with private key"
            }
          ]
        }      

>[!WARNING] 
> Dosyaya eklenen tüm sonraki parolalar da otomatik olarak bastırılır. 

#### <a name="what-are-recommended-secrets-management-guidelines"></a>Önerilen gizli dizi yönetimi yönergeleri nelerdir? 
Sabit kodlanmış gizli dizileri zamanında algılarken, riskleri azaltmak çok daha iyi bir şekilde tespit edilirken, bir parola, parolaların tamamen iade edilme zorunluluğunu ortadan kaldırsa bile daha iyidir. Bu şekilde, Microsoft, Visual Studio için [Microsoft DevLabs uzantısının](https://marketplace.visualstudio.com/items?itemName=VSIDEDevOpsMSFT.ContinuousDeliveryToolsforVisualStudio) bir parçası olarak Credscan Code Analyzer ' ı yayımlamıştır. Erken önizlemede, geliştiricilere bu sorunların gerçek zamanlı olarak düzeltilmesi için olası gizli dizileri algılamaya yönelik bir satır içi deneyim sağlar. Daha fazla bilgi için lütfen bulutta güvenli şekilde gizli anahtarları yönetirken [Bu](https://devblogs.microsoft.com/visualstudio/managing-secrets-securely-in-the-cloud/) bloga başvurun. Aşağıda, gizliliklerinizi yönetmenize ve gizli bilgilere uygulamalarınızın içinden güvenli bir şekilde erişmenize yardımcı olacak birkaç ek kaynak verilmiştir: 
 - [Azure Anahtar Kasası.](../../key-vault/index.yml)
 - [Azure Active Directory](../../sql-database/sql-database-aad-authentication.md)
 - [Azure AD Yönetilen Hizmet Kimliği](https://azure.microsoft.com/blog/keep-credentials-out-of-code-introducing-azure-ad-managed-service-identity/)
 - [Azure kaynakları için Yönetilen Hizmet Kimliği (MSI)](../../active-directory/managed-identities-azure-resources/overview.md)
 - [Azure Yönetilen Hizmet Kimliği](../../app-service/overview-managed-identity.md)
 - [AppAuthentication kitaplığı](../../key-vault/service-to-service-authentication.md)

#### <a name="can-i-write-my-own-custom-searchers"></a>Kendi özel aramalarımı yazabilir miyim?

Kimlik bilgisi tarayıcısı, **buildsearchers. xml** dosyasında yaygın olarak tanımlanan bir dizi içerik arayanlardan yararlanır. Dosya, bir ContentSearcher nesnesini temsil eden bir XML serileştirilmiş nesneler dizisi içerir. Program iyi sınanmış bir dizi arayla dağıtılır, ancak kendi özel aramalarınızı da uygulamanıza olanak tanır. 

Bir içerik arayici aşağıdaki gibi tanımlanır: 

- **Name** : kimlik bilgisi tarayıcısı çıkış dosyasında kullanılacak açıklayıcı Arayıcının adı. Arayıcının adları için ortası Case adlandırma kuralının kullanılması önerilir. 
- **RuleId** : Searcher 'ın KARARLı donuk kimliği. 
    - Kimlik bilgisi tarayıcısı varsayılan aramacılar CSCAN0010, CSCAN0020, CSCAN0030 gibi Ruleıds ile atanır. Son basamak, olası Arayıcının Regex grubu birleştirme veya bölme için ayrılmıştır.
    - Özelleştirilmiş aramacılar için RuleId 'nin biçiminde kendi ad alanı olmalıdır: CSCAN-{Namespace} 0010, CSCAN-{Namespace} 0020, CSCAN-{Namespace} 0030, vb.
    - Tam araya adı, RuleId ve Arayıcının adının birleşimidir. Örnek CSCAN0010. KeyStoreFiles, CSCAN0020. Base64EncodedCertificate, vb.
- **Resourcematchmodel** : Arayıcının 'a karşı Denetlenecek dosya uzantılarının Regex
- **Contentsearchpatterns** : eşleştirilecek Regex deyimlerini içeren dizelerin dizisi. Arama desenleri tanımlanmamışsa, kaynak eşleşmesi düzeniyle eşleşen tüm dosyalar döndürülür.
- **Contentsearchfilters** : Arayıcının 'a özgü hatalı pozitifleri filtrelemek için Regex deyimlerini içeren dizelerin dizisi.
- **Matchdetails** : her bir araya eşleşmesi için eklenen açıklayıcı bir ileti ve/veya azaltma yönergeleri.
- **Öneri** : önceden hızlı rapor biçimi kullanan bir eşleşme için öneriler alan içeriğini sağlar.
- **Önem derecesi** : sorunun önem derecesini yansıtan bir tamsayı (en yüksek = 1).
![Kimlik bilgisi tarayıcı kurulumu](./media/security-tools/6-credscan-customsearchers.png)

### <a name="roslyn-analyzers-faqs"></a>Roslyn Çözümleyicileri SSS

#### <a name="what-are-the-most-common-errors-when-using-the-roslyn-analyzers-task"></a>Roslyn çözümleyiciler görevi kullanılırken en yaygın hatalar nelerdir?

**Hata: Proje Microsoft. NETCore. App version x. x. x kullanılarak geri yüklendi, ancak şu anki ayarlarla, bunun yerine y. y. y sürümü kullanılacak. Bu sorunu çözmek için, geri yükleme ve derleme ya da yayımlama gibi sonraki işlemler için aynı ayarların kullanıldığından emin olun. Genellikle bu sorun, Runtimeıdentifier özelliği derleme sırasında ayarlandıysa ve geri yükleme sırasında yayınlanmadığında ortaya çıkabilir:**

Roslyn Çözümleyicileri derlemenin bir parçası olarak çalışır, bu nedenle derleme makinesindeki kaynak ağacının oluşturulabilir bir durumda olması gerekir. Ana derlemelerinizin yanı sıra Roslyn çözümleyiciler arasında bir adım (büyük olasılıkla "DotNet. exe Publish"), kaynak ağacı kayıt edilmemiş bir duruma koymamasından kaynaklanıyor olabilir. Yalnızca Roslyn Çözümleyicileri adımından önce bir NuGet geri yüklemesi yapan adımı çoğaltıp, kaynak ağacı bir oluşturulabilir duruma geri yerleştirmeyecektir.

**"csc. exe", 1 hata koduyla çıkıldı--C:\BBBB.dll öğesinden bir Analyzer AAAA örneği oluşturulamıyor: Dosya veya derleme ' Microsoft. CodeAnalysis, Version = X. x. x. X, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' veya bağımlılıklarından biri yüklenemedi. Sistem belirtilen dosyayı bulamıyor.**

Derleyicisinin Roslyn çözümleyicilerinin desteklediğinden emin olun. "csc. exe/Version" en az v 2.6. x rapor etmelidir. Bazı durumlarda, tek bir. csproj dosyası, Microsoft.Net. derleyiciler tarafından bir pakete başvurarak derleme makinesinin Visual Studio yüklemesini geçersiz kılabilir. Derleyicinin belirli bir sürümünü kullanmak istenilolduysa, Microsoft.Net. derleyicilere başvuruları kaldırın. Aksi takdirde, başvurulan paketin da en az v 2.6. x olduğundan emin olun. Csc. exe komut satırındaki (Roslyn derleme görevinin günlüğünde bulunan)/Errorlog: parametresinde bulabileceğiniz hata günlüğünü almayı deneyin. Şöyle bir şey görünebilir:/Errorlog: f:\ts-Services-123\_work\456\s\Some\Project\Code\Code.csproj.Sarif

**C# Derleyici en son yeterince değil (> = 2,6 olmalıdır)**

C# Derleyicinin en son sürümleri buradan yayımlanır: https://www.nuget.org/packages/Microsoft.Net.Compilers. Çalıştıran `C:\>csc.exe /version` yüklü sürümü almak için komut isteminden komutunu kullanın. < V 2.6 olan bir Microsoft.Net. compilers NuGet paketine başvurunuz olmadığından emin olun.

**MSBuild/VSBuild günlükleri bulunamadı**

Görevin çalışma biçimi nedeniyle, bu görevin MSBuild derleme görevinin MSBuild günlüğü için Azure DevOps 'u sorgulaması gerekir. Bu görev, MSBuild derleme görevinin hemen sonrasında çalışırsa, günlük henüz kullanılamaz; MSBuild Build görevi ve Roslyn Çözümleyicileri derleme görevi arasında, Bınskim, kötü amaçlı yazılımdan koruma taraması ve diğerleri gibi SecDevTools derleme görevleri dahil diğer derleme görevlerini yerleştirin. 

## <a name="next-steps"></a>Sonraki adımlar

Ek yardıma ihtiyacınız varsa, Microsoft Güvenlik kodu analizi desteği 9:00, Pasifik Standart Saati ' den Cuma 'Ya kadar 5:00 00

  - Ekleme-başlamak için Teknik Hesap yöneticileriniz ile Iletişim kurun. 
  >[!NOTE] 
  >Microsoft ile ücretli destek ilişkiniz yoksa veya Phoenix kataloğundan hizmet satın almanıza izin veren bir destek teklifiniz varsa, daha fazla bilgi için lütfen [Destek Hizmetleri giriş sayfasını](https://www.microsoft.com/enterprise/services/support) ziyaret edin.

  - Destek-e-posta [Microsoft Güvenlik kodu analizi desteğiyle](mailto:mscahelp@microsoft.com?Subject=Microsoft%20Security%20Code%20Analysis%20Support%20Request)
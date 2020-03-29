---
title: Microsoft Güvenlik Kodu Analizi belgeleri SSS
description: Bu makale, Microsoft Güvenlik Kodu Çözümlemesi uzantısı hakkında bir SSS içerir
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
ms.openlocfilehash: de76467cc741a65a851e5f3a7ec424d0326aebb2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74851528"
---
# <a name="frequently-asked-questions"></a>Sık sorulan sorular
Sorularınız mı var? Daha fazla bilgi için aşağıdaki SSS'ye göz atın.

## <a name="general-faq"></a>Genel SSS

### <a name="can-i-install-the-extension-on-my-visual-studio-team-foundation-server-instance-instead-of-on-an-azure-devops-instance"></a>Uzantıyı Azure DevOps örneği yerine Visual Studio Team Foundation Server örneğime yükleyebilir miyim?

Hayır. Uzantı, Visual Studio Team Foundation Server için indirme ve yükleme için kullanılamaz.

### <a name="do-i-have-to-run-microsoft-security-code-analysis-with-my-build"></a>Yapımla Microsoft Güvenlik Kodu Çözümlemesi'ni çalıştırmam gerekiyor mu? 

Belki. Analiz aracının türüne bağlıdır. Kaynak kodu gerekli olan tek şey olabilir veya yapı çıktısı gerekebilir.

Örneğin, Kimlik Bilgisi Tarayıcısı (CredScan) kod deposunun klasör yapısındaki dosyaları analiz eder. Bu çözümleme nedeniyle, sonuç almak için Tek başına bir yapıda CredScan ve Publish Security Analysis Logs oluşturma görevlerini çalıştırabilirsiniz.

BinSkim gibi yapı sonrası yapıları analiz eden diğer araçlar için, önce yapının oluşturulması gerekir.

### <a name="can-i-break-my-build-when-results-are-found"></a>Sonuçlar bulunduğunda yapımı bozabilir miyim?

Evet. Herhangi bir araç günlük dosyasında bir sorun veya sorun bildirdiğinde bir yapı sonu tanıtabilirsiniz. Analiz Sonrası oluşturma görevini eklemeniz ve yapıyı kırmak istediğiniz herhangi bir araç için onay kutusunu seçmeniz.

Çözümleme Sonrası görevin UI'sinde, herhangi bir araç yalnızca hataları veya her iki hata ve uyarı yı bildirdiğinde yapıyı kesmeyi seçebilirsiniz.

### <a name="how-do-the-command-line-arguments-in-azure-devops-differ-from-those-arguments-in-the-standalone-desktop-tools"></a>How do the command-line arguments in Azure DevOps differ from those arguments in the standalone desktop tools? 

Azure DevOps yapı görevleri çoğunlukla, güvenlik araçlarının komut satırı bağımsız değişkenlerinin etrafındaki doğrudan sarmalayıcılardır. Normalde bir komut satırı aracına geçtiğiniz her şeyi oluşturma görevine bağımsız değişken olarak geçirebilirsiniz.

Fark edilir:

- Araçlar $(Build.SourcesDirectory) aracısının kaynak klasöründen veya %BUILD_SOURCESDIRECTORY'den çalışır. Örnek C:\agent\_work\1\s'dir.
- Bağımsız değişkenlerde yollar daha önce listelenen kaynak dizinin köküne göreli olabilir. Yollar da mutlak olabilir. Azure DevOps Yapı Değişkenleri'ni kullanarak veya yerel kaynakların bilinen dağıtım konumlarına sahip bir şirket içi aracı çalıştırarak mutlak yollar elde elabilirsiniz.
- Araçlar otomatik olarak bir çıktı dosya yolu veya klasör sağlar. Bir yapı görevi için bir çıktı konumu sağlarsanız, bu konum yapı aracısı üzerindeki günlüklerin iyi bilinen konumuna giden bir yol ile değiştirilir
- Bazı ek komut satırı bağımsız değişkenleri bazı araçlar için değiştirilir. Bir örnek, GUI'nin başlatılmamasından emin olan seçeneklerin eklenmesi veya kaldırılmasıdır.

### <a name="can-i-run-a-build-task-like-credential-scanner-across-multiple-repositories-in-an-azure-devops-build"></a>Bir Azure DevOps Yapısı'ndaki birden çok depoda Kimlik Bilgisi Tarayıcısı gibi bir yapı görevi çalıştırabilir miyim?

Hayır. Güvenli geliştirme araçlarını tek bir ardışık ardışık alanda birden çok depoda çalıştırmak desteklenmez.

### <a name="the-output-file-i-specified-isnt-being-created-or-i-cant-find-the-output-file-i-specified"></a>Belirttiğim çıktı dosyası oluşturulmuyor veya belirttiğim çıktı dosyasını bulamıyorum

Yapı görevleri bazı kullanıcı girişlerini filtreler. Özellikle bu soru için, oluşturulan çıktı dosyasının konumunu yapı aracısı üzerinde ortak bir konum olarak güncelleştirin. Bu konum hakkında daha fazla bilgi için aşağıdaki sorulara bakın.

### <a name="where-are-the-output-files-generated-by-the-tools-saved"></a>Kaydedilen araçlar tarafından oluşturulan çıktı dosyaları nerede? 

Yapı görevleri, yapı aracısı üzerinde bu iyi bilinen konuma otomatik olarak çıktı yolları\_ekler: $(Agent.BuildDirectory) sdt\logs. Bu konumda standartlaştırdığımız için, kod çözümleme günlükleri üreten veya tüketen tüm takımlar çıktıya erişebilir.

### <a name="can-i-queue-a-build-to-run-these-tasks-on-a-hosted-build-agent"></a>Barındırılan bir yapı aracısı üzerinde bu görevleri çalıştırmak için bir yapı sıraya miyim? 

Evet. Uzantıdaki tüm görevler ve araçlar barındırılan bir yapı aracısında yürütülebilir.

>[!NOTE]
> Kötü Amaçlı Yazılımdan Koruma Tarayıcısı oluşturma görevi, Windows Defender etkinleştirilmiş bir yapı aracısı gerektirir. Hosted Visual Studio 2017 ve daha sonra böyle bir ajan sağlar. Yapı görevi Visual Studio 2015 barındırılan temsilcide çalışmaz.
>
> Bu aracılar üzerinde imzalar güncelleştirilemese de, imzalar her zaman üç saatten daha eski olmalıdır.

### <a name="can-i-run-these-build-tasks-as-part-of-a-release-pipeline-as-opposed-to-a-build-pipeline"></a>Bu yapı görevlerini, yapı ardışık bir yapı nın aksine, bir sürüm ardışık hattının parçası olarak çalıştırabilir miyim?

Çoğu durumda, evet.

Ancak Azure DevOps, bu görevler yapılarını yayımladığında sürüm ardışık hatlarında görev çalıştırmayı desteklemez. Bu destek eksikliği, Yayımlama Güvenlik Çözümlemesi Günlükleri görevinin bir sürüm ardışık alanında başarılı bir şekilde çalışmasını engeller. Görev bunun yerine açıklayıcı bir hata iletisi ile başarısız olur.

### <a name="from-where-do-the-build-tasks-download-the-tools"></a>Yapı görevleri araçları nereden indirmektedir?

Yapı görevleri, araçların NuGet paketlerini [Azure DevOps Paket Yönetimi akışından](https://securitytools.pkgs.visualstudio.com/_packaging/SecureDevelopmentTools/nuget/v3/index.json)indirebilir. Yapı görevleri, yapı aracısının üzerine önceden yüklenmesi gereken Düğüm Paket Yöneticisi'ni de kullanabilir. Bu tür yükleme bir örnek komut **u pm tslint yükleyin.**

### <a name="what-effect-does-installing-the-extension-have-on-my-azure-devops-organization"></a>Uzantıyı yüklemenin Azure DevOps kuruluşum üzerinde ne gibi bir etkisi var? 

Bunların yüklenmesi nden sonra, uzantı tarafından sağlanan güvenlik yapısı görevleri kuruluşunuzdaki tüm kullanıcılar tarafından kullanılabilir hale gelir. Bir Azure Ardışık Alanı oluşturduğunuzda veya bunları yaptığınızda, bu görevler yapı görev koleksiyonu listesinden kullanılabilir. Aksi takdirde, uzantını Azure DevOps kuruluşunuza yüklemenin hiçbir etkisi olmaz. Yükleme, hesap ayarlarını, proje ayarlarını veya ardışık hatları değiştirmez.

### <a name="does-installing-the-extension-modify-my-existing-azure-pipelines"></a>Uzantıyı yüklemek varolan Azure Ardışık Hatlarımı değiştirir mi? 

Hayır. Uzantıyı yüklemek, güvenlik yapısı görevlerini ardışık hatlarınıza ek olarak kullanılabilir hale getirir. Araçların yapı işleminizle çalışabilmesi için yapı tanımları eklemeniz veya güncelleştirmeniz gerekir.

## <a name="task-specific-faq"></a>Göreve özel SSS

Görevler oluşturmaya özgü sorular bu bölümde listelenmiştir.

### <a name="credential-scanner"></a>Kimlik Bilgisi Tarayıcı

#### <a name="what-are-common-suppression-scenarios-and-examples"></a>Ortak bastırma senaryoları ve örnekleri nelerdir?

Aşağıda, en yaygın iki bastırma senaryosunun ayrıntıları verilmiştir.

##### <a name="to-suppress-all-occurrences-of-a-given-secret-within-the-specified-path"></a>Belirtilen yol içinde belirli bir sırrın tüm oluşumlarını bastırmak için

Aşağıdaki örnekte gösterildiği gibi, CredScan çıktı dosyasından gelen sırrın karma anahtarı gereklidir.

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
> Karma anahtar, eşleşen değerin veya dosya içeriğinin bir bölümü tarafından oluşturulur. Herhangi bir kaynak kodu düzeltmesi karma anahtarı değiştirebilir ve bastırma kuralını devre dışı edebilir.

##### <a name="to-suppress-all-secrets-in-a-specified-file-or-to-suppress-the-secrets-file-itself"></a>Belirli bir dosyadaki tüm sırları bastırmak veya sırları dosyanın kendisini bastırmak için

Dosya ifadesi bir dosya adı olabilir. Ayrıca, tam bir dosya yolunun veya dosya adının temel adı parçası da olabilir. Joker karakterler desteklenmez.

Aşağıdaki örnekler, InputPath \<>\src\JS\lib\angular.js dosyasının nasıl bastırılabildiğini gösterir

Geçerli bastırma kurallarına örnekler:

- \<InputPath>\src\JS\lib\angular.js - belirtilen yolda dosyayı bastırır
- \src\JS\lib\angular.js
- \JS\lib\angular.js
- \lib\angular.js
- açısal.js - aynı ada sahip herhangi bir dosyayı bastırır

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
> Dosyaya eklenen gelecekteki tüm sırlar da otomatik olarak bastırılır.

#### <a name="what-are-recommended-guidelines-for-managing-secrets"></a>Sırları yönetmek için önerilen yönergeler nelerdir?

Aşağıdaki kaynaklar, sırları güvenli bir şekilde yönetmenize ve uygulamalarınız içinden hassas bilgilere erişmenize yardımcı olur:

 - [Azure Key Vault](../../key-vault/index.yml)
 - [Azure Etkin Dizin (Azure AD)](../../sql-database/sql-database-aad-authentication.md)
 - [Azure AD Yönetilen Hizmet Kimliği (MSI)](https://azure.microsoft.com/blog/keep-credentials-out-of-code-introducing-azure-ad-managed-service-identity/)
 - [Azure kaynakları için yönetilen kimlikler](../../active-directory/managed-identities-azure-resources/overview.md)
 - [Azure Uygulama Hizmeti ve Azure İşlerinde yönetilen kimlikler](../../app-service/overview-managed-identity.md)
 - [AppAuthentication kitaplığı](../../key-vault/service-to-service-authentication.md)


Daha fazla bilgi için, [Bulut'ta Sırları Güvenli Bir Şekilde Yönetme](https://devblogs.microsoft.com/visualstudio/managing-secrets-securely-in-the-cloud/)blog yazısına bakın.

#### <a name="can-i-write-my-own-custom-searchers"></a>Kendi özel arama ekibimi yazabilir miyim?

Kimlik Bilgisi Tarayıcı, buildsearchers.xml dosyasında yaygın olarak tanımlanan bir içerik arama kümesine dayanır. Dosya, **ContentSearcher** nesnesini temsil eden bir Dizi XML seri nesnelerini içerir. Program iyi test edilmiş arama bir dizi dağıtılır. Ama çok kendi özel arama uygulayabilirsiniz.

İçerik arayan bir kullanıcı aşağıdaki gibi tanımlanır:

- **Adı**: Kimlik Bilgisi Tarayıcı çıktı dosyalarında kullanılacak açıklayıcı arama adı. Arama adlarını kullanmak için deve kasa adlandırma kuralını kullanmanızı tavsiye ettik.
- **RuleId**: Arama yapılanın kararlı opak kimliği:
    - Bir Kimlik Bilgisi Tarayıcı varsayılan arayıcısı CSCAN0010, CSCAN0020 veya CSCAN0030 gibi bir **RuleId** değeri atanır. Son basamak, arama grubu gruplarının normal ifadeler (regex) aracılığıyla birleştirilmesi veya bölünmesi için ayrılmıştır.
    - Özelleştirilmiş bir arama nın **RuleId** değerinin kendi ad alanına sahip olması gerekir. Örnekler arasında\<CSCAN-\>Namespace 0010,\<CSCAN- Namespace\>0020 ve CSCAN-\<Namespace\>0030 sayılabilir.
    - Tam nitelikli arama **adı, RuleId** değeri ve arama adının birleşimidir. Örnekler CSCAN0010 içerir. KeyStoreFiles ve CSCAN0020. Base64EncodedCertificate.
- **ResourceMatchPattern**: Arama üzerinde kontrol etmek için dosya uzantıları Regex.
- **ContentSearchPatterns**: Eşleşen regex ifadeleri içeren dizeleri bir dizi. Arama deseni tanımlanmamışsa, **ResourceMatchPattern** değeriyle eşleşen tüm dosyalar döndürülür.
- **ContentSearchFilters**: Arama cıvık yanlış pozitifleri filtrelemek için regex ifadeleri içeren bir dizi dize.
- **MatchDetails**: Açıklayıcı bir ileti, azaltma yönergeleri veya her ikisi de aramanın her eşleşmesi için eklenecek.
- **Öneri**: PREfast rapor biçimini kullanarak bir eşleşmeiçin öneri alanı içeriği.
- **Önem Derecesi**: Bir sorunun önem düzeyini yansıtan bir karşıcı. En yüksek önem düzeyi 1 değerine sahiptir.

  ![Kimlik Bilgisi Tarayıcı kurulumlarını gösteren XML](./media/security-tools/6-credscan-customsearchers.png)

### <a name="roslyn-analyzers"></a>Roslyn Çözümleyicileri

#### <a name="what-are-common-errors-when-using-the-roslyn-analyzers-task"></a>Roslyn Çözümleyicileri görevini kullanırken sık karşılaşılan hatalar nelerdir?

##### <a name="the-project-was-restored-using-a-wrong-microsoftnetcoreapp-version"></a>Proje yanlış bir Microsoft.NETCore.App sürümü kullanılarak geri yüklendi

Tam hata iletisi:

"Hata: Proje Microsoft.NETCore.App sürümü *x.x.x*kullanılarak geri yüklendi, ancak geçerli ayarları ile, sürüm *y.y.y* yerine kullanılacak. Bu sorunu gidermek için, aynı ayarların geri yükleme ve yapı veya yayımlama gibi sonraki işlemler için kullanıldığından emin olun. RuntimeIdentifier özelliği yapı veya yayımlama sırasında ayarlanır, ancak geri yükleme sırasında değil, genellikle bu sorun oluşabilir."

Roslyn Çözümleyicileri görevleri derlemenin bir parçası olarak çalıştırıldığı için, yapı makinesindeki kaynak ağacın inşa edilebilir durumda olması gerekir.

Ana yapınız ile Roslyn Çözümleyiciadımları adımları arasındaki bir adım, kaynak ağacı nı binayı engelleyen bir duruma sokmuş olabilir. Bu ekstra adım muhtemelen **dotnet.exe yayımlamak**. Roslyn Analyzers adımından hemen önce NuGet restorasyonu yapan adımı çoğaltmayı deneyin. Bu yinelenen adım, kaynak ağacı tekrar inşa edilebilir duruma sokabilir.

##### <a name="cscexe-cant-create-an-analyzer-instance"></a>csc.exe çözümleyici örneği oluşturamaz

Tam hata iletisi:

"'csc.exe' hata kodu 1 ile çıktı - Analizör *AAAA* bir\\örnek C oluşturulamaz:*BBBB*.dll : Dosya veya derleme 'Microsoft.CodeAnalysis, Sürüm =*X.X.X.X*, Kültür = nötr, PublicKeyToken=31bf3856ad364e35' veya bağımlılıklarından biri yükleyemedi. Sistem belirtilen dosyayı bulamıyor."

Derleyicinizin Roslyn Analizörlerini desteklediğinden emin olun. **komut csc.exe /sürüm** çalıştıran 2.6 veya daha sonra bir sürüm değeri bildirmelidir.

Bazen bir .csproj dosyası, Microsoft.Net.Compilers'dan bir pakete başvurarak yapı makinesinin Visual Studio yüklemesini geçersiz kılabilir. Derleyicinin belirli bir sürümünü kullanmak istemiyorsanız, Microsoft.Net.Compilers'a yapılan başvuruları kaldırın. Aksi takdirde, başvurulan paketin sürümünün de 2,6 veya daha sonra olduğundan emin olun.

**csc.exe /errorlog** seçeneğinde belirtilen hata günlüğü yolunu almaya çalışın. Seçenek ve yol, Roslyn Çözümleyicileri oluşturma görevinin günlüğünde görünür. Onlar /errorlog gibi bir şey **görünebilir:F:\ts-services-123\_iş\456\s\Some\Project\Code\Code.csproj.sarif**

##### <a name="the-c-compiler-version-isnt-recent-enough"></a>C# derleyici sürümü yeterince yeni değil

C# derleyicisinin en son sürümlerini almak için [Microsoft.Net.Compilers'a](https://www.nuget.org/packages/Microsoft.Net.Compilers)gidin. Yüklü sürüm almak için komut istemi **csc.exe /sürümünü** çalıştırın. Sürüm 2.6 veya sonraki sürüm olan bir Microsoft.Net.Compilers NuGet paketine başvurup başvurulduğuzdan emin olun.

##### <a name="msbuild-and-vsbuild-logs-arent-found"></a>MSBuild ve VSBuild günlükleri bulunamadı

Roslyn Çözümleyicileri oluşturma görevi, MSBuild yapı görevinden MSBuild günlüğü için Azure DevOps'leri sorgulamak gerekir. Çözümleyici görevi MSBuild görevinden hemen sonra çalışırsa, günlük henüz kullanılamayacak. MSBuild görevi ile Roslyn Çözümleyicileri görevi arasına başka görevler yerleştirin. Diğer görevlere örnek olarak BinSkim ve Malware Scanner verilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Ek yardıma ihtiyacınız varsa, Microsoft Güvenlik Kodu Analizi Desteği Pazartesi'den Cuma'ya 09:00-17:00 Pasifik Standart Saati arasında kullanılabilir.

- Onboarding: [Onboarding belgelerimize](security-code-analysis-onboard.md) bakın
  
- Destek: [Ekibimize Microsoft Güvenlik Kodu Analizi Desteği'nde](mailto:mscahelp@microsoft.com?Subject=Microsoft%20Security%20Code%20Analysis%20Support%20Request) e-posta gönder
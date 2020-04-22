---
title: Microsoft Azure'da güvenli uygulamalar geliştirin
description: Bu makalede, web uygulama projenizin uygulama ve doğrulama aşamalarında göz önünde bulundurulması gereken en iyi uygulamalar açıkılmaktadır.
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/12/2019
ms.topic: article
ms.service: security
ms.subservice: security-develop
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 03f5b0124f95465c4a5da5043364a2f5816dae62
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81685742"
---
# <a name="develop-secure-applications-on-azure"></a>Azure üzerinde güvenli uygulamalar geliştirme
Bu makalede, bulut için uygulamalar geliştirirken göz önünde bulundurulması gereken güvenlik etkinlikleri ve denetimleri sıyoruz. Microsoft [Güvenlik Geliştirme Yaşam Döngüsü'nün (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) uygulama ve doğrulama aşamalarında göz önünde bulundurulması gereken güvenlik soruları ve kavramları ele alınır. Amaç, daha güvenli bir uygulama geliştirmek için kullanabileceğiniz etkinlikleri ve Azure hizmetlerini tanımlamanıza yardımcı olmaktır.

Aşağıdaki SDL aşamaları bu makalede ele alınmıştır:

- Uygulama
- Doğrulama

## <a name="implementation"></a>Uygulama
Uygulama aşamasının odak noktası, erken önleme için en iyi uygulamaları oluşturmak ve güvenlik sorunlarını koddan tespit etmek ve kaldırmaktır.
Uygulamanızın kullanılmasını istemediğiniz şekillerde kullanılacağını varsayalım. Bu, uygulamanızın yanlışlıkla veya kasıtlı olarak kötüye kullanılmasına karşı korumanıza yardımcı olur.

### <a name="perform-code-reviews"></a>Kod incelemelerini gerçekleştirin

Kodu iade etmeden önce, genel kod kalitesini artırmak ve hata oluşturma riskini azaltmak için [kod incelemeleri](https://docs.microsoft.com/azure/devops/learn/devops-at-microsoft/code-reviews-not-primarily-finding-bugs) gerçekleştirin. Kod gözden geçirme işlemini yönetmek için [Visual Studio'yu](https://docs.microsoft.com/azure/devops/repos/tfvc/get-code-reviewed-vs?view=vsts) kullanabilirsiniz.

### <a name="perform-static-code-analysis"></a>Statik kod analizini gerçekleştirme

[Statik kod çözümlemesi](https://www.owasp.org/index.php/Static_Code_Analysis) *(kaynak kodu çözümlemesi*olarak da bilinir) genellikle kod incelemesinin bir parçası olarak gerçekleştirilir. Statik kod çözümlemesi genellikle [kusurlu denetimi](https://en.wikipedia.org/wiki/Taint_checking) ve [veri akışı analizi](https://en.wikipedia.org/wiki/Data-flow_analysis)gibi teknikler kullanarak çalışmayan koddaki olası güvenlik açıklarını bulmak için statik kod çözümleme araçlarının çalıştırılacağı anlamına gelir.

Azure Marketi, statik kod analizi gerçekleştiren ve kod incelemelerine yardımcı olan [geliştirici araçları](https://azuremarketplace.microsoft.com/marketplace/apps/category/developer-tools?page=1&search=code%20review) sunar.

### <a name="validate-and-sanitize-every-input-for-your-application"></a>Uygulamanız için her girişi doğrulayın ve dezenfekte edin

Uygulamanızı en yaygın web uygulaması güvenlik açıklarından korumak için tüm girdileri güvenilmeyen olarak değerlendirin. Güvenilmeyen veriler enjeksiyon saldırıları için bir araçtır. Uygulamanız için giriş, URL'deki parametreleri, kullanıcıdan gelen girişi, veritabanından veya API'den gelen verileri ve kullanıcının işleyebilir şekilde işleyebileceği her şeyi içerir. Uygulama verileri [validate](https://owasp.org/www-project-proactive-controls/v3/en/c5-validate-inputs) herhangi bir şekilde kullanmadan önce (kullanıcıya geri görüntülemek de dahil olmak üzere) verilerin sözdizimsel ve anlamsal olarak geçerli olduğunu doğrulamalıdır.

Yalnızca düzgün biçimlendirilmiş verilerin iş akışına girdiğinden emin olmak için veri akışında girişi erken doğrulayın. Veritabanınızda hatalı biçimlendirilmiş verilerin kalıcı olmasını veya akış aşağı bileşeninde bir arızayı tetiklemelerini istemezsiniz.

Kara liste ve beyaz liste, giriş sözdizimi doğrulaması gerçekleştirmek için iki genel yaklaşımdır:

  - Kara liste, belirli bir kullanıcı girişinin "kötü amaçlı olduğu bilinen" içerik içermediğini denetlemeye çalışır.

  - Beyaz liste, belirli bir kullanıcı girişinin bir dizi "bilinen iyi" girişle eşleşinden denetlemeye çalışır. Karakter tabanlı beyaz liste, bir uygulamanın kullanıcı girişinin yalnızca "bilinen iyi" karakterleri içerdiğini veya girişin bilinen bir biçimle eşleştiğini denetlediği beyaz liste biçimidir.
    Örneğin, bu, bir kullanıcı adının yalnızca alfasayısal karakterler içerdiğini veya tam olarak iki sayı içerdiğini denetlemeyi içerebilir.

Beyaz liste, güvenli yazılım oluşturmak için tercih edilen bir yaklaşımdır.
Kara liste hataya açıktır, çünkü potansiyel olarak kötü girişin tam bir listesini düşünmek imkansızdır.

Bu işi istemci tarafında değil, sunucuda (veya sunucuda ve istemci tarafında) yapın.

### <a name="verify-your-applications-outputs"></a>Uygulamanızın çıktılarını doğrulama

Görsel olarak veya belge içinde sunduğunuz çıktılar her zaman kodlanmalıdır ve kaçılmalıdır. *Çıkış kodlamaolarak*da bilinen [kaçış,](https://www.owasp.org/index.php/Injection_Theory#Escaping_.28aka_Output_Encoding.29)güvenilmeyen verilerin enjeksiyon saldırısı için bir araç olmamasını sağlamaya yardımcı olmak için kullanılır. Kaçış, veri doğrulama ile birlikte, bir bütün olarak sistemin güvenliğini artırmak için katmanlı savunma sağlar.

Kaçmak, her şeyin çıktı olarak görüntülenmesini *sağlar.* Kaçmak ayrıca yorumlayıcıya verilerin yürütülmesi için tasarlanmadığını bildirir ve bu da saldırıların çalışmasını engeller. Bu, *siteler arası komut dosyası* (XSS) adı verilen başka bir yaygın saldırı tekniğidir.

Üçüncü bir tarafın bir web çerçevesi kullanıyorsanız, [OWASP XSS önleme hile sayfasını](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.md)kullanarak web sitelerinde çıktı kodlama seçeneklerinizi doğrulayabilirsiniz.

### <a name="use-parameterized-queries-when-you-contact-the-database"></a>Veritabanına başvururken parametreli sorguları kullanma

Kodunuzda asla "anında" bir satır içinde veritabanı sorgusu oluşturmayın ve doğrudan veritabanına gönderin. Uygulamanıza eklenen kötü amaçlı kod, veritabanınızın çalınmasına, silinmesine veya değiştirilmesine neden olabilir. Uygulamanız, veritabanınızı barındıran işletim sisteminde kötü amaçlı işletim sistemi komutlarını çalıştırmak için de kullanılabilir.

Bunun yerine, parametreli sorguları veya depolanmış yordamları kullanın. Parametreli sorgular kullandığınızda, kodunuzdan yordamı güvenli bir şekilde çağırabilir ve sorgu deyiminin bir parçası olarak kabul edildiğinden endişe etmeden bir dize geçirebilirsiniz.

### <a name="remove-standard-server-headers"></a>Standart sunucu üstbilgilerini kaldırma

Server, X-Powered-By ve X-AspNet-Version gibi başlıklar sunucu ve temel teknolojiler hakkında bilgi ortaya çıkarır. Uygulamanın parmak izini almamak için bu üstbilgileri bastırmanızı öneririz.
[Azure web sitelerindeki standart sunucu üstbilgilerini kaldırma](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/)ya bakın.

### <a name="segregate-your-production-data"></a>Üretim verilerinizi ayırın

Üretim verileriniz veya "gerçek" verileriniz geliştirme, sınama veya işletmenin amaçladığı amaç dışında başka bir amaç için kullanılmamalıdır. Tüm geliştirme ve testler için maskeli[(anonimleştirilmiş)](https://en.wikipedia.org/wiki/Data_anonymization)veri kümesi kullanılmalıdır.

Bu, gerçek verilerinize daha az kişinin erişebilen saldırı yüzeyinizi azalttığı anlamına gelir. Bu aynı zamanda daha az sayıda çalışanın kişisel verileri görmesi anlamına gelir ve bu da gizlilikteki olası bir ihlali ortadan kaldırır.

### <a name="implement-a-strong-password-policy"></a>Güçlü bir parola ilkesi uygulama

Kaba kuvvete ve sözlük tabanlı tahmine karşı savunmak için, kullanıcıların karmaşık bir parola oluşturmasını sağlamak için güçlü bir parola ilkesi uygulamanız gerekir (örneğin, en az 12 karakter uzunluğunda ve alfasayısal ve özel karakterler gerektirir).

Parola ilkeleri oluşturmak ve uygulamak için bir kimlik çerçevesi kullanabilirsiniz. Azure AD B2C, [yerleşik ilkeler](../../active-directory-b2c/tutorial-create-user-flows.md#create-a-password-reset-user-flow), self servis [parola sıfırlama](../../active-directory-b2c/user-flow-self-service-password-reset.md)ve daha fazlasını sağlayarak parola yönetimi konusunda size yardımcı olur.

Varsayılan hesaplardaki saldırılara karşı savunmak için, tüm anahtarların ve parolaların değiştirilebilir olduğunu ve kaynakları yükledikten sonra oluşturulduğunu veya değiştirildiğini doğrulayın.

Uygulamaparolaları otomatik olarak oluşturması gerekiyorsa, oluşturulan parolaların rasgele olduğundan ve entropi olup olmadığından emin olun.

### <a name="validate-file-uploads"></a>Dosya yüklemelerini doğrulama

Uygulamanız dosya yüklemelerine izin [veriyorsa,](https://www.owasp.org/index.php/Unrestricted_File_Upload)bu riskli etkinlik için alabileceğiniz önlemleri göz önünde bulundurun. Birçok saldırının ilk adımı, saldırı altında olan bir sisteme bazı kötü amaçlı kodlar almaktır. Dosya yüklemesi kullanmak, saldırganın bunu başarmesine yardımcı olur. OWASP, yüklediğiniz dosyanın güvenli olduğundan emin olmak için bir dosyayı doğrulamak için çözümler sunar.

Kötü amaçlı yazılımdan koruma, virüsleri, casus yazılımları ve diğer kötü amaçlı yazılımları tanımlamaya ve kaldırmaya yardımcı olur. [Microsoft Antimalware](../fundamentals/antimalware.md) veya Microsoft iş ortağının uç nokta koruma çözümlerini[(Trend Micro,](https://www.trendmicro.com/azure/) [Broadcom,](https://www.broadcom.com/products) [McAfee,](https://www.mcafee.com/us/products.aspx) [Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10)ve [Endpoint Protection)](https://docs.microsoft.com/configmgr/protect/deploy-use/endpoint-protection)yükleyebilirsiniz.

[Microsoft Antimalware](../fundamentals/antimalware.md) gerçek zamanlı koruma, zamanlanmış tarama, kötü amaçlı yazılım düzeltme, imza güncelleştirmeleri, motor güncelleştirmeleri, örnek raporlama ve dışlama olay toplama gibi özellikler içerir. Dağıtım kolaylığı ve yerleşik algılamalar (uyarılar ve olaylar) için Microsoft Kötü Amaçlı Yazılımdan Koruma ve iş ortağı çözümlerini [Azure Güvenlik Merkezi](../../security-center/security-center-partner-integration.md) ile tümleştirebilirsiniz.

### <a name="dont-cache-sensitive-content"></a>Hassas içeriği önbelleğe alamayın

Tarayıcıda hassas içeriği önbelleğe alanmayın. Tarayıcılar önbelleğe alma ve geçmiş için bilgi depolayabilir. Önbelleğe alınan dosyalar, Internet Explorer durumunda Geçici İnternet Dosyaları klasörü gibi bir klasörde depolanır. Bu sayfalara yeniden atıfta bulunulduğunda, tarayıcı sayfaları önbelleğinden görüntüler. Hassas bilgiler (adres, kredi kartı bilgileri, Sosyal Güvenlik numarası, kullanıcı adı) kullanıcıya görüntülenirse, bilgiler tarayıcının önbelleğinde saklanabilir ve tarayıcının önbelleğini inceleyerek veya yalnızca tarayıcının **Geri Düğmesine** basarak alınabilir.

## <a name="verification"></a>Doğrulama
Doğrulama aşaması, kodun önceki aşamalarda oluşturulan güvenlik ve gizlilik ilkelerini karşıladığından emin olmak için kapsamlı bir çaba içerir.

### <a name="find-and-fix-vulnerabilities-in-your-application-dependencies"></a>Uygulama bağımlılıklarınızdaki güvenlik açıklarını bulma ve düzeltme

Bilinen güvenlik açığı olan bileşenleri tanımlamak için uygulamanızı ve bağımlı kitaplıklarını tarayıp. Bu tama gerçekleştirmek için kullanılabilir ürünler [OWASP Bağımlılık Denetimi,](https://www.owasp.org/index.php/OWASP_Dependency_Check)[Snyk](https://snyk.io/)ve [Black Duck](https://www.blackducksoftware.com/)içerir.

Azure App Service Web Apps'ta [Tinfoil Security](https://www.tinfoilsecurity.com/) tarafından desteklenen güvenlik açığı taraması mevcuttur. [Tinfoil Security, Uygulama Hizmeti aracılığıyla tarama,](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) geliştiricilere ve yöneticilere kötü niyetli bir aktörün bu güvenlik açıklarından yararlanmadan önce güvenlik açıklarını hızlı, entegre ve ekonomik bir şekilde keşfetmeve bunlara çözüm bulma nızı sağlar.

> [!NOTE]
> [Tinfoil Security'yi Azure AD ile tümleştirebilirsiniz.](../../active-directory/saas-apps/tinfoil-security-tutorial.md) Tinfoil Security'yi Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:
>  - Azure AD'de, Tinfoil Security'ye kimlerin erişebileceğini kontrol edebilirsiniz.
>  - Kullanıcılarınız Azure AD hesaplarını kullanarak Otomatik olarak Tinfoil Security'de (tek oturum açma) oturum açabilir.
>  - Hesaplarınızı tek bir merkezi konumda, Azure portalında yönetebilirsiniz.

### <a name="test-your-application-in-an-operating-state"></a>Uygulamanızı işletim durumunda test edin

Dinamik uygulama güvenliği testi (DAST), güvenlik açıklarını bulmak için bir uygulamayı işletim durumunda sınama işlemidir. DAST araçları, bellek bozulması, güvenli olmayan sunucu yapılandırması, site ler arası komut dosyası yürütme, kullanıcı gizliliği sorunları, SQL enjeksiyonu ve diğer kritik güvenlik sorunları gibi güvenlik açıklarını bulmak için çalışırken programları analiz ederler.

DAST statik uygulama güvenliği testinden (SAST) farklıdır. SAST araçları, güvenlik açıklarını bulmak için kod yürütülmediğinde kaynak kodu veya kod derlenmiş sürümlerini analiz eder.

Tercihen bir güvenlik uzmanının [(penetrasyon testörü](../fundamentals/pen-testing.md) veya güvenlik açığı denetçisi) yardımıyla DAST'yi gerçekleştirin. Bir güvenlik uzmanı yoksa, bir web proxy tarayıcı ve bazı eğitim ile DAST kendiniz gerçekleştirebilirsiniz. Kodunuza bariz güvenlik sorunları getirmemeniz için bir DAST tarayıcısını erkenden takın. Web uygulaması güvenlik açığı tarayıcılarının listesi için [OWASP](https://www.owasp.org/index.php/Category:Vulnerability_Scanning_Tools) sitesine bakın.

### <a name="perform-fuzz-testing"></a>Bulanık test gerçekleştirin

[Bulanık lık testinde,](https://cloudblogs.microsoft.com/microsoftsecure/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/)bir uygulamaya kasıtlı olarak biçimsiz veya rasgele veri sunarak program başarısızlığına neden olursunuz. Program hatası namına, uygulama yayımlanmadan önce olası güvenlik sorunlarını ortaya çıkarmasına yardımcı olur.

[Güvenlik Risk Algılama,](https://docs.microsoft.com/security-risk-detection/) yazılımda güvenlik açısından kritik hataları bulmak için Microsoft'un benzersiz bulanıklık test hizmetidir.

### <a name="conduct-attack-surface-review"></a>Saldırı yüzey incelemesi yapın

Kod tamamlandıktan sonra saldırı yüzeyinin gözden geçirilmesi, bir uygulama veya sistemde yapılan herhangi bir tasarım veya uygulama değişikliğinin dikkate alınmasına yardımcı olur. Tehdit modelleri de dahil olmak üzere değişiklikler sonucunda oluşturulan yeni saldırı vektörlerinin gözden geçirilmesini ve azaltılmasını sağlamaya yardımcı olur.

Uygulamayı tarayarak saldırı yüzeyinin bir resmini oluşturabilirsiniz. Microsoft, [Attack Surface Analyzer](https://www.microsoft.com/download/details.aspx?id=24487)adlı bir saldırı yüzey analiz aracı sunar. Birçok ticari dinamik test ve güvenlik açığı tarama araçları veya hizmetleri arasından seçim yapabilirsiniz, [OWASP Zed Attack Proxy Projesi](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project)dahil, [Arachni](http://arachni-scanner.com/), [Skipfish](https://code.google.com/p/skipfish/), ve [w3af](http://w3af.sourceforge.net/). Bu tarama araçları uygulamanızı tarıyor ve uygulamanın web üzerinden erişilebilen bölümlerini eşler. Azure Marketi'nde de benzer [geliştirici araçları](https://azuremarketplace.microsoft.com/marketplace/apps/category/developer-tools?page=1)için arama yapabilirsiniz.

### <a name="perform-security-penetration-testing"></a>Güvenlik penetrasyon testi gerçekleştirin

Uygulamanızın güvenli olmasını sağlamak, diğer işlevleri test etmek kadar önemlidir. [Penetrasyon testini](../fundamentals/pen-testing.md) oluşturma ve dağıtım işleminin standart bir parçası haline getirin. Dağıtılan uygulamalarda düzenli güvenlik testleri ve güvenlik açığı taraması zamanlayın ve açık bağlantı noktaları, uç noktalar ve saldırılar için izlemeyi izleyin.

### <a name="run-security-verification-tests"></a>Güvenlik doğrulama testlerini çalıştırın

[Azure için Secure DevOps Kit](https://azsk.azurewebsites.net/index.html) (AzSK), Azure platformunun birden çok hizmeti için SVT içerir. Azure aboneliğinizin ve uygulamanızı oluşturan farklı kaynakların güvenli durumda olduğundan emin olmak için bu SVT'leri düzenli aralıklarla çalıştırın. Ayrıca, SVT'leri Visual Studio uzantısı olarak kullanılabilir hale getiren AzSK'nın sürekli tümleştirme/sürekli dağıtım (CI/CD) uzantıları özelliğini kullanarak bu testleri otomatikleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde, güvenli uygulamalar tasarlamanıza ve dağıtmanıza yardımcı olabilecek güvenlik denetimleri ve etkinlikleri öneririz.

- [Güvenli uygulamalar tasarla](secure-design.md)
- [Güvenli uygulamaları dağıtma](secure-deploy.md)

---
title: Microsoft Azure Güvenlik kodu analiz belgelerine genel bakış
description: Bu makale, güvenlik kodu analiz uzantısı 'na genel bakış
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
ms.openlocfilehash: 283d63bafc583f2ac9da3294644aaebd17d7c950
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718294"
---
# <a name="about-microsoft-security-code-analysis"></a>Microsoft Güvenlik kodu analizi hakkında

**Microsoft Güvenlik kodu analiz uzantısı** , Microsoft 'Ta [güvenli geliştirme yaşam döngüsü (SDL)](https://www.microsoft.com/securityengineering/sdl/practices) uzmanlarının önerildiği şekilde güvenlik kodu ANALIZINI Azure DEVOPS CI/CD işlem hatları ile sorunsuz bir şekilde tümleştirmenizi sağlar. Güvenlik, çeşitli araçların çalıştırıldığı karmaşıklıkları soyutlayan tutarlı bir UX aracılığıyla basitleştirilmiştir. Araçların NuGet tabanlı teslimi sayesinde, takımların yükleme veya güncelleştirme araçlarını yönetmesi gerekmez. Komut satırı ve temel yapı görev arabirimleri sayesinde, bilgili bir araç olan tüm kullanıcılar, gündelik geliştiricilere kadar çok daha az veya istedikleri gibi araçlar üzerinde çok fazla denetime sahip olabilir. Ekipler Ayrıca, bekletme için günlükleri yayımlama, işlem yapılabilir geliştirici odaklı raporlar oluşturma ve gerilemede derleme sonlarını yapılandırma &.

## <a name="why-microsoft-security-code-analysis"></a>Neden Microsoft Güvenlik kodu Analizi

### <a name="security-simplified"></a>Basitleştirilmiş güvenlik

Azure DevOps işlem hattınızda güvenlik kodu analiz araçları eklemek, yeni görevler eklemek kadar basittir. Bunları özelleştirin veya varsayılanlara gidin. Görevler DevOps işlem hattınızı bir parçası olarak çalışır ve her türlü bulguları ayrıntılandıran Günlükler oluşturur.

### <a name="clean-builds"></a>Derlemeleri temizle

Araçların bildirdiği ilk sorunları çözdükten sonra, uzantıyı yeni sorunlarda derlemeleri bölmek için yapılandırabilirsiniz. Her çekme isteğinde sürekli tümleştirme derlemesini ayarlamak hiç bu kadar kolay olmamıştır!

### <a name="set-it-and-forget-it"></a>Ayarlayın ve unutmayın

Derleme görevleri ve araçları güncel kalmak için ayarlanabilir (ve varsayılan olarak). Aracın güncelleştirilmiş bir sürümü varsa, indirmek ve yüklemek gerekmez; Bu uzantı sizin için bunu sizin yerinize gerçekleştirir. 

>>>
### <a name="under-the-hood"></a>Üzerinde

Microsoft Güvenlik kodu çözümleme uzantısı derleme görevleri için karmaşıklıklar şunlardır:
  - Güvenlik statik analiz araçlarını çalıştırma ve
  - Özet raporu oluşturmak veya derlemeyi bölmek için günlük dosyalarından sonuçları işleme.
>>>

## <a name="security-code-analysis-toolset"></a>Güvenlik kodu analiz araç takımı

Microsoft Güvenlik kodu çözümleme uzantısı, önemli Analysis Tools 'un en son sürümleri için size hazır hale gelir. Uzantı hem Microsoft Iç hem de açık kaynak araçlarını içerir. Araçlar, ilgili derleme görevini kullanarak işlem hattını çalıştırmak & yapılandırdıktan sonra bulutta barındırılan aracıya otomatik olarak indirilir. Bu, bugün uzantısında kullanılabilen araçların bir kümesidir. Daha fazla bilgi alın ve eklenebilecek araçlar için önerilerinizi bize gönderin.

### <a name="anti-malware-scanner"></a>Kötü amaçlı yazılımdan koruma tarayıcısı

Kötü amaçlı yazılımdan koruma tarayıcı oluşturma görevi, Microsoft Güvenlik kodu çözümleme uzantısına eklenmiştir. Windows Defender 'ın zaten yüklü olduğu bir yapı aracısında çalıştırılmalıdır. Daha fazla bilgi için [Defender Web sitesini](https://aka.ms/defender) ziyaret edin 

### <a name="binskim"></a>Binskım

BinSkim, derleyici/bağlayıcı ayarlarını ve güvenlikle ilgili diğer ikili özellikleri doğrulayan bir taşınabilir yürütülebilir (PE) hafif tarayıcıdır. Yapı görevi, BinSkim. exe uygulamasının etrafında bir komut satırı sarmalayıcı sağlar. BinSkim, açık kaynaklı bir araçtır ve daha fazla bilgi için [GitHub 'Da Binskım 'yi](https://github.com/Microsoft/binskim) ziyaret edin

### <a name="credential-scanner"></a>Kimlik bilgisi tarayıcısı

Kaynak kodda depolanan parolalar ve diğer gizli dizileri Şu anda önemli bir sorundur. Kimlik bilgisi tarayıcısı, kaynak kodunuzda ve yapı çıkışındaki kimlik bilgilerini, parolaları, sertifikaları ve diğer hassas içeriği algılayan özel bir statik analiz aracıdır.

### <a name="microsoft-security-risk-detection"></a>Microsoft güvenlik riski algılama

Güvenlik riski algılama, Microsoft 'un yazılımda yararlanabilecek güvenlik hatalarını belirlemek için bulut tabanlı benzersiz bir test hizmetidir. Bu hizmet ayrı bir ekleme işlemi gerektirir. Daha fazla bilgi için [docs.Microsoft.com adresindeki Msrd](https://docs.microsoft.com/security-risk-detection/) adresini ziyaret edin

### <a name="roslyn-analyzers"></a>Roslyn Çözümleyicileri

Yönetilen kodu çözümlemek için Microsoft 'un derleyici ile tümleşik Statik Analiz Aracı (C# ve vb). Daha fazla bilgi için [docs.Microsoft.com adresindeki Roslyn Çözümleyicileri](https://docs.microsoft.com/dotnet/standard/analyzers/) sitesini ziyaret edin

### <a name="tslint"></a>TSLint

TSLint, okunabilirlik, bakım ve işlevsellik hataları için TypeScript kodunu denetleyen genişletilebilir bir statik analiz aracıdır. Modern düzenleyiciler ve derleme sistemleri genelinde yaygın olarak desteklenir ve kendi LINT kurallarınız, yapılandırmanız ve formatlamalarınızla özelleştirilebilir. TSLint, açık kaynaklı bir araçtır ve daha fazla bilgi için [GitHub 'Da tslint](https://github.com/palantir/tslint) adresini ziyaret edin

## <a name="analysis-and-post-processing-of-results"></a>Sonuçları analiz ve sonrası Işleme

Microsoft Güvenlik kodu analiz uzantısı 'nın Ayrıca, güvenlik araçları görevlerinde bulunan sonuçları işleyebilmeniz ve analiz etmenize yardımcı olmak için üç yardımcı işlem sonrası görevi vardır. Bunlar genellikle diğer araç görevlerinin ardından işlem hattına eklenir.

### <a name="publish-security-analysis-logs"></a>Güvenlik analizi günlüklerini yayımlama
Güvenlik analizi günlük oluşturmayı Yayımla görevi, araştırma ve izleme için derleme sırasında çalıştırılan güvenlik araçlarının günlük dosyalarını korur.

Bunlar Azure sunucu yapılarına (zip dosyası olarak) yayımlanabilir veya özel derleme aracıınızdan erişilebilir bir dosya paylaşımında yer alabilir.

### <a name="security-report"></a>Güvenlik raporu
Güvenlik raporu oluşturma görevi, derleme sırasında çalıştırılan güvenlik araçları tarafından oluşturulan günlük dosyalarını ayrıştırır ve analiz araçları tarafından bulunan tüm sorunları içeren tek bir Özet rapor dosyası oluşturur.

Görev, belirli araçlara veya tüm araçlara yönelik bulguları bildirmek üzere yapılandırılabilir ve ayrıca ne tür bir sorun (hata veya hata ve uyarı) bildirilmesi gerektiğini de seçebilirsiniz.

### <a name="post-analysis-build-break"></a>Analiz sonrası (derleme kesmesi)
Analiz sonrası derleme görevi, bir veya daha fazla analiz aracının koddaki bulguları veya sorunları rapor etmesini sağlamak için müşterinin bir yapı kesmesi eklemesine ve derlemeyi başarısız olmasına olanak sağlar.

Görev, belirli araçlar veya tüm araçlar tarafından bulunan sorunlar için derlemeyi bölmek üzere yapılandırılabilir ve ayrıca bulunan sorunların önem derecesine (hatalar veya uyarılar) göre.

>[!NOTE]
>Araç başarıyla tamamlandığında, tüm araçların çalışmasına izin vermek için derleme işleminin tamamlanmasını sağlamak için tek tek derleme görevleri tasarım tarafından başarılı olur.

## <a name="next-steps"></a>Sonraki adımlar

Güvenlik kodu analizini ekleme ve yükleme yönergeleri için [ekleme ve yükleme](security-code-analysis-onboard.md) kılavuzumuza bakın

Yapı görevlerini yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma](security-code-analysis-customize.md) kılavuzumuzu

Uzantı ve sunulan araçlar hakkında başka sorularınız varsa, [SSS sayfamızı inceleyin.](security-code-analysis-faq.md)
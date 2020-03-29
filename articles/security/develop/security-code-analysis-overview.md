---
title: Microsoft Güvenlik Kodu Analizi belgelerine genel bakış
description: Bu makale, Microsoft Güvenlik Kodu Çözümlemesi uzantısına genel bir bakıştır
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
ms.openlocfilehash: 963bc909b69962cded0a50d717e3a653d3d69769
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74851477"
---
# <a name="about-microsoft-security-code-analysis"></a>Microsoft Güvenlik Kodu Analizi Hakkında

Microsoft Güvenlik Kodu Çözümlemesi uzantısı ile takımlar, Azure DevOps sürekli tümleştirme ve teslim (CI/CD) boru hatlarına güvenlik kodu çözümlemesi ekleyebilir. Bu çözümleme, Microsoft'taki [Güvenli Geliştirme Yaşam Döngüsü (SDL)](https://www.microsoft.com/securityengineering/sdl/practices) uzmanları tarafından önerilir.

Tutarlı bir UX, çalışan araçların karmaşıklığını gizleyerek güvenliği kolaylaştırır. Araçların NuGet tabanlı teslimi ile ekiplerin artık takım yüklemesini veya güncellenmesini yönetmesine gerek kalmaz. Yapı görevleri için hem komut satırı hem de temel arabirimler sayesinde, tüm kullanıcılar araçlar üzerinde istedikleri kadar denetime sahip olabilir.

Takımlar, şu gibi güçlü işlem sonrası yetenekleri de kullanabilir:

- Bekletme için günlükleri yayımlama.
- Eyleme geçirilebilir, geliştirici odaklı raporlar oluşturma.
- Regresyon testlerinde yapı molalarını yapılandırma.

## <a name="why-should-i-use-microsoft-security-code-analysis"></a>Microsoft Güvenlik Kodu Çözümlemesi'ni neden kullanmalıyım?

### <a name="security-simplified"></a>Güvenlik basitleştirildi

Azure DevOps ardınıza Microsoft Güvenlik Kodu Çözümleme araçları eklemek, yeni görevler eklemek kadar kolaydır. Görevleri özelleştirin veya varsayılan davranışlarını kullanın. Görevler Azure DevOps ardışık alanınızın bir parçası olarak çalışır ve birçok sonuç türünde ayrıntı veren günlükler üretir.

### <a name="clean-builds"></a>Temiz yapılar

Araçlar tarafından bildirilen ilk sorunları ele aldıktan sonra, uzantıyı yeni sorunlarda yapıyı bozacak şekilde yapılandırabilirsiniz.Her çekme isteğine sürekli tümleştirme oluşturma kolaydır.

### <a name="set-it-and-forget-it"></a>Ayarlayın ve unutun

Varsayılan olarak, yapı görevleri ve araçları güncel kalır. Bir aracın güncelleştirilmiş bir sürümü varsa, bunu karşıdan yüklemeniz ve yüklemeniz gerekmez. Uzantı, sizin için güncelleştirme ilgilenir.

### <a name="under-the-hood"></a>Başlık altında

Uzantının yapı görevleri aşağıdakilerin karmaşıklığını gizler:
  - Güvenlik statik analiz araçlarını çalıştırıyor.
  - Özet rapor oluşturmak veya yapıyı kırmak için günlük dosyalarından elde edilen sonuçları işleme.

## <a name="microsoft-security-code-analysis-tool-set"></a>Microsoft Güvenlik Kodu Çözümleme araç kümesi

Microsoft Güvenlik Kodu Çözümlemesi uzantısı, önemli analiz araçlarının en son sürümlerini kolayca kullanabileceğinizi sağlar. Uzantı, hem Microsoft tarafından yönetilen araçları hem de açık kaynak kodlu araçları içerir.

Bu araçlar, ardışık yapıyı yapılandırmak ve çalıştırmak için ilgili yapı görevini kullandıktan sonra bulut barındırılan aracıya otomatik olarak indirilir.

Bu bölümde, uzantıda şu anda kullanılabilen araçlar kümesi listelenir. Daha fazla araç eklenmesine dikkat edin. Ayrıca, eklememizi istediğiniz araçlar için önerilerinizi bize gönderin.

### <a name="anti-malware-scanner"></a>Kötü Amaçlı Yazılımdan Koruma Tarayıcısı

Kötü Amaçlı Yazılımdan Koruma Tarayıcısı oluşturma görevi artık Microsoft Güvenlik Kodu Çözümlemesi uzantısına dahildir. Bu görev, Windows Defender zaten yüklü olan bir yapı aracısı üzerinde çalıştırılmalıdır. Daha fazla bilgi için [Windows Defender web sitesine](https://aka.ms/defender)bakın.

### <a name="binskim"></a>BinSkim

BinSkim, derleyici ayarlarını, bağlayıcı ayarlarını ve ikili dosyaların güvenlikle ilgili diğer özelliklerini doğrulayan Taşınabilir Çalıştırılabilir (PE) hafif tarayıcıdır. Bu yapı görevi, binskim.exe konsol uygulamasının etrafında bir komut satırı sarıcı sağlar. BinSkim açık kaynak kodlu bir araçtır. Daha fazla bilgi için [GitHub'daki BinSkim'e](https://github.com/Microsoft/binskim)bakın.

### <a name="credential-scanner"></a>Kimlik Bilgisi Tarayıcı

Kaynak kodda depolanan parolalar ve diğer sırlar önemli bir sorundur. Kimlik Bilgisi Tarayıcı, bu sorunu çözmeye yardımcı olan özel bir statik çözümleme aracıdır. Araç, kaynak kodunuzdaki ve yapı çıktınızdaki kimlik bilgilerini, sırları, sertifikaları ve diğer hassas içeriği algılar.

### <a name="microsoft-security-risk-detection"></a>Microsoft Güvenlik Risk Algılama

Microsoft Security Risk Detection (MSRD), bulanıklık testi için bulut tabanlı bir hizmettir. Yazılımdaki kullanılabilir güvenlik hatalarını tanımlar. Bu hizmet ayrı bir abonelik ve etkinleştirme gerektirir. Daha fazla bilgi için [MSRD Geliştirici Merkezi'ne](https://docs.microsoft.com/security-risk-detection/)bakın.

### <a name="roslyn-analyzers"></a>Roslyn Çözümleyicileri

Roslyn Analyzers, yönetilen C# ve Visual Basic kodunu statik olarak analiz etmek için Microsoft'un derleyici yle tümleşik aracıdır. Daha fazla bilgi için [Roslyn tabanlı analizörlere](https://docs.microsoft.com/dotnet/standard/analyzers/)bakın.

### <a name="tslint"></a>TSLint

TSLint, TypeScript kodunu okunabilirlik, skorun ve işlevsellik hataları için denetleyen genişletilebilir bir statik çözümleme aracıdır. Modern editörler ve yapı sistemleri tarafından yaygın olarak desteklenir. Kendi tiftik kuralları, yapılandırmaları ve formatters ile özelleştirebilirsiniz. TSLint açık kaynak kodlu bir araçtır. Daha fazla bilgi için [GitHub'daki TSLint'e](https://github.com/palantir/tslint)bakın.

## <a name="analysis-and-post-processing-of-results"></a>Sonuçların analizi ve post-processing

Microsoft Güvenlik Kodu Çözümlemesi uzantısı da üç postprocessing görevleri vardır. Bu görevler, güvenlik aracı görevleri tarafından bulunan sonuçları çözümlemenize yardımcı olur. Bir ardışık izlemeye eklendiğinde, bu görevler genellikle diğer tüm araç görevlerini izler.

### <a name="publish-security-analysis-logs"></a>Güvenlik Analizi Günlüklerini Yayımla

Güvenlik Çözümlemesi Günlükleri oluşturma görevi, yapı sırasında çalıştırılabilen güvenlik araçlarının günlük dosyalarını korur. Soruşturma ve takip için bu günlükleri okuyabilirsiniz.

Günlük dosyalarını Azure Yapıları'nda .zip dosyası olarak yayımlayabilirsiniz. Bunları özel yapı aracınızdan erişilebilir bir dosya paylaşımına da kopyalayabilirsiniz.

### <a name="security-report"></a>Güvenlik Raporu

Güvenlik Raporu oluşturma görevi günlük dosyalarını ayrıştirır. Bu dosyalar, yapı sırasında çalışan güvenlik araçları tarafından oluşturulur. Yapı görevi daha sonra tek bir özet rapor dosyası oluşturur. Bu dosya, çözümleme araçları tarafından bulunan tüm sorunları gösterir.

Bu görevi, belirli araçlar veya tüm araçlar için sonuçları raporlamak için yapılandırabilirsiniz. Yalnızca hatalar veya her iki hata ve uyarı gibi hangi sorun düzeyinin rapor edeceğini de seçebilirsiniz.

### <a name="post-analysis-build-break"></a>Post-Analysis (yapı sonu)

Çözümleme Sonrası oluşturma göreviyle, bir yapının kasıtlı olarak başarısız olması yla ilgili bir yapı sonu enjekte edebilirsiniz. Bir veya daha fazla çözümleme aracı koddaki sorunları rapor ederse, bir yapı sonu enjekte ekarsınız.

Belirli araçlar veya tüm araçlar tarafından bulunan sorunlar için yapıyı kırmak için bu görevi yapılandırabilirsiniz. Ayrıca, hatalar veya uyarılar gibi bulunan sorunların önem derecesine göre yapılandırabilirsiniz.

>[!NOTE]
>Tasarım gereği, görev başarıyla biterse, her yapı görevi başarılı olur. Bu, bir aracın sorunları bulup bulmadığı, böylece yapının tüm araçların çalışmasına izin vererek tamamlanabilmesi için geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar

Microsoft Güvenlik Kodu Analizi'nin gemiye nasıl yüklenir ve yüklenir, ilgili talimatları için [Onboarding ve yükleme kılavuzumuza](security-code-analysis-onboard.md)bakın.

Yapı görevlerini yapılandırma hakkında daha fazla bilgi için [Yapılandırma kılavuzumuza](security-code-analysis-customize.md) veya [YAML Yapılandırma kılavuzumuza](yaml-configuration.md)bakın.

Uzantı ve sunulan araçlar la ilgili daha fazla sorunuz varsa, [SSS sayfamıza](security-code-analysis-faq.md)göz atın.

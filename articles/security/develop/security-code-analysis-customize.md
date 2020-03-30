---
title: Microsoft Güvenlik Kodu Çözümlemesi görevlerini özelleştirme
titleSuffix: Azure
description: Bu makalede, Microsoft Güvenlik Kodu Çözümlemesi uzantısında görevleri özelleştirme açıklanmaktadır
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
ms.openlocfilehash: 6cdf892651407defc21f359a8e3b326b4af63b62
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77499998"
---
# <a name="configure-and-customize-the-build-tasks"></a>Yapı görevlerini yapılandırma ve özelleştirme

Bu makalede, yapı görevlerinin her birinde bulunan yapılandırma seçenekleri ayrıntılı olarak açıklanmaktadır. Makale, güvenlik kodu çözümleme araçlarının görevleriyle başlar. İşlem sonrası görevlerle sona erer.

## <a name="anti-malware-scanner-task"></a>Anti-Malware Tarayıcı görev

>[!NOTE]
> Kötü Amaçlı Yazılımdan Koruma Tarayıcısı oluşturma görevi, Windows Defender etkinleştirilmiş bir yapı aracısı gerektirir. Hosted Visual Studio 2017 ve daha sonra böyle bir ajan sağlar. Yapı görevi Visual Studio 2015 barındırılan temsilcide çalışmaz.
>
> Bu aracılar üzerinde imzalar güncelleştirilemese de, imzalar her zaman üç saatten daha eski olmalıdır.

Görev yapılandırmasının ayrıntıları aşağıdaki ekran görüntüsü nde ve metinde gösterilir.

![Kötü Amaçlı Yazılımdan Koruma Tarayıcısı oluşturma görevini yapılandırma](./media/security-tools/5-add-anti-malware-task600.png)

Ekran görüntüsünün **Tür** listesi kutusunda **Temel** seçilir. Taramayı özelleştiren komut satırı bağımsız değişkenleri sağlamak için **Özel'i** seçin.

Windows Defender imzaları indirmek ve yüklemek için Windows Update istemcisini kullanır. Yapı aracınızda imza güncelleştirmesi başarısız olursa, **HRESULT** hata kodu büyük olasılıkla Windows Update'ten gelir.

Windows Update hataları ve bunların azaltılması hakkında daha fazla bilgi için, [bileşene göre Windows Update hata kodlarına](https://docs.microsoft.com/windows/deployment/update/windows-update-error-reference) ve TechNet makalesi [Windows Update Agent - Hata Kodları'na](https://social.technet.microsoft.com/wiki/contents/articles/15260.windows-update-agent-error-codes.aspx)bakın.

Bu görev için YAML yapılandırması hakkında bilgi için lütfen [Kötü Amaçlı Yazılımdan Koruma Seçeneklerimizi](yaml-configuration.md#anti-malware-scanner-task) kontrol edin

## <a name="binskim-task"></a>BinSkim görevi

> [!NOTE]
> BinSkim görevini çalıştıramadan önce yapınızın aşağıdaki koşullardan birini karşılaması gerekir:
>  - Yapınız yönetilen koddan ikili yapılar üretir.
>  - BinSkim ile analiz etmek istediğiniz ikili eserler emretmişsinizdir.

Görev yapılandırmasının ayrıntıları aşağıdaki ekran görüntüsü ve listede gösterilir.

![BinSkim yapı görevini yapılandırma](./media/security-tools/7-binskim-task-details.png)

- Yapı yapılandırmasını.pdb hata ayıklama dosyalarının üretilen hata ayıklama olarak ayarlayın. BinSkim, çıktı ikililerinde bulunan sorunları kaynak koduna eşlemek için bu dosyaları kullanır.
- Kendi komut satırınızı araştırmaktan ve oluşturmaktan kaçınmak için:
     - **Tür** listesinde **Temel'i**seçin.
     - **İşlev** listesinde **Çözümle'yi**seçin.
- **Hedef'te,** dosya, dizin veya filtre deseni için bir veya daha fazla belirtici girin. Bu belirteçler, çözümlenecek bir veya daha fazla ikiliye çözümlenir:
    - Birden çok belirtilen hedef bir yarı kolon (;) ile ayrılmalıdır.
    - Bir belirtiç tek bir dosya olabilir veya joker karakter içerebilir.
    - Dizin belirtimleri \\her zaman *ile bitmelidir.
    - Örnekler:

           *.dll;*.exe
           $(BUILD_STAGINGDIRECTORY)\*
           $(BUILD_STAGINGDIRECTORY)\*.dll;$(BUILD_STAGINGDIRECTORY)\*.exe;

- **Tür** listesinde **Komut Satırı'nı** seçerseniz, binskim.exe çalıştırmanız gerekir:
     - binskim.exe için ilk bağımsız değişkenler bir veya daha fazla yol özellikleri tarafından izlenen fiil **çözümleme** olduğundan emin olun. Her yol tam bir yol veya kaynak dizine göre bir yol olabilir.
     - Birden çok hedef yolu bir boşlukla ayrılmalıdır.
     - **/o** veya **/output** seçeneğini atlayabilirsiniz. Çıktı değeri sizin için eklenir veya değiştirilir.
     - Standart komut satırı yapılandırmaları aşağıdaki gibi gösterilir.

           analyze $(Build.StagingDirectory)\* --recurse --verbose
           analyze *.dll *.exe --recurse --verbose

          > [!NOTE]
          > Hedef için \\dizinler belirtirseniz, sondaki * önemlidir.

BinSkim komut satırı bağımsız değişkenleri, kimlik kuralları veya çıkış kodları hakkında daha fazla bilgi için [BinSkim Kullanım Kılavuzu'na](https://github.com/Microsoft/binskim/blob/master/docs/UserGuide.md)bakın.

Bu görev için YAML yapılandırması hakkında bilgi için lütfen [BinSkim YAML seçeneklerimizi](yaml-configuration.md#binskim-task) kontrol edin

## <a name="credential-scanner-task"></a>Kimlik Bilgisi Tarayıcı görevi

Görev yapılandırmasının ayrıntıları aşağıdaki ekran görüntüsü ve listede gösterilir.

![Kimlik Bilgisi Tarayıcı oluşturma görevini yapılandırma](./media/security-tools/3-taskdetails.png)

Mevcut seçenekler şunlardır:

  - **Çıkış Biçimi**: Kullanılabilir değerler **Arasında TSV**, **CSV**, **SARIF**, ve **PREfast**.
  - **Araç Sürümü**: **En Son'u**seçmenizi öneririz.
  - **Tarama Klasörü**: Taranacak depo klasörü.
  - **Arama Yapanlar Dosya Türü**: Tarama için kullanılan arama yapanların dosyasını bulma seçenekleridir.
  - **Bastırma Dosyası**: Bir [JSON](https://json.org/) dosyası çıktı günlüğündeki sorunları bastırabilir. Bastırma senaryoları hakkında daha fazla bilgi için bu makalenin SSS bölümüne bakın.
  - **Verbose Çıkış**: Kendini açıklayıcı.
  - **Toplu İşlem Boyutu**: Kimlik Bilgisi Tarayıcısını çalıştırmak için kullanılan eşzamanlı iş parçacığı sayısı. Varsayılan değer 20'dir. Olası değerler 1 ile 2.147.483.647 arasında değişir.
  - **Maç Zaman Dilimi**: Çeki terk etmeden önce bir arama eşleşmesi denemek için saniye cinsinden zaman dilimi.
  - **Dosya Taramasite Boyutunu Oku**: İçerik okunurken kullanılan arabelleğebaytboyutu. Varsayılan değer 524.288'dir.  
  - **Maksimum Dosya Tarama Byte'u Oku**: İçerik analizi sırasında bir dosyadan okunacak maksimum bayt sayısı. Varsayılan değer 104.857.600'dür.
  - **Denetim Seçenekleri** > **Bu görevi çalıştırın**: Görevin ne zaman çalıştırılacağını belirtir. Daha karmaşık koşullar belirtmek için **Özel koşulları** seçin.
  - **Sürüm**: Azure DevOps içindeki yapı görevi sürümü. Bu seçenek sık kullanılmaz.

Bu görev için YAML yapılandırması hakkında daha fazla bilgi için lütfen [Kimlik Bilgisi Tarayıcımız YAML seçeneklerini](yaml-configuration.md#credential-scanner-task) kontrol edin

## <a name="microsoft-security-risk-detection-task"></a>Microsoft Güvenlik Risk Algılama görevi

> [!NOTE]
> MSRD görevini kullanmadan önce Microsoft Güvenlik Risk Algılama (MSRD) hizmetinde bir hesap oluşturmalı ve yapılandırmanız gerekir. Bu hizmet ayrı bir onboarding işlemi gerektirir. Bu uzantıdaki diğer görevlerin aksine, bu görev MSRD ile ayrı bir abonelik gerektirir.
>
> Lütfen [Microsoft Güvenlik Risk Algılama](https://aka.ms/msrddocs) ve Microsoft Güvenlik Risk Algılama: Talimatlar için nasıl [olunabilir.](https://docs.microsoft.com/security-risk-detection/how-to/)

Bu görevi yapılandırmaya ilişkin ayrıntılar aşağıdaki listede gösterilir. Herhangi bir UI öğesi için, yardım almak için bu öğenin üzerinde gezinmek olabilir.

   - **MSRD için Azure DevOps Hizmet Bitiş Noktası Adı**: Genel bir Azure DevOps hizmeti bitiş noktası türü, yerleşik MSRD örnek URL'nizi ve REST API erişim belirtecinizi saklar. Böyle bir bitiş noktası oluşturduysanız, burada belirtebilirsiniz. Aksi takdirde, bu MSRD görevi için yeni bir hizmet bitiş noktası oluşturmak ve yapılandırmak için **Yönet** bağlantısını seçin.
   - **Hesap Kimliği**: MSRD hesap URL'sinden alınabilen bir GUID.
   - **URL'ler - İkili :** Kamuya açık URL'lerin yarı kolon lu sınırlı listesi. Bulanıklama makinesi ikilileri indirmek için bu URL'leri kullanır.
   - **Tohum Dosyalarının URL'leri**: Herkese açık URL'lerin yarı kolon lu listesi. Bulanıklama makinesi tohumları indirmek için bu URL'leri kullanır. Tohum dosyaları ikili dosyalarla birlikte karşıdan yüklenirse, bu değeri belirtmek isteğe bağlıdır.
   - **İşletim Sistemi Platform Türü**: Bulanıklık işini çalıştıran makinelerin işletim sistemi (OS) platformudur. Kullanılabilir değerler **Windows** ve **Linux'tür.**
   - **Windows Edition / Linux Edition**: Bulanık lık işini çalıştıran makinelerin işletim sistemi sürümü. Makinelerinizde farklı bir işletim sistemi sürümü varsa varsayılan değerin üzerine yazabilirsiniz.
   - **Paket Yükleme Komut Dosyası**: Komut dosyanız bir test makinesinde çalıştırılacak. Bu komut dosyası, bulanık iş gönderilmeden önce test hedef programını ve bağımlılıklarını yükler.
   - **İş Gönderme Parametreleri**:
       - **Tohum Dizini**: Tohumları içeren bulanıklık makinesindeki dizin yolu.
       - **Tohum Uzantısı**: Tohumların dosya adı uzantısı.
       - **Test Sürücüsü Çalıştırılabilir**: Bulanıkmakinede hedef çalıştırılabilir dosyaya giden yol.
       - **Test Sürücüsü Çalıştırılabilir Mimari**: Hedef yürütülebilir dosyanın mimarisi. Kullanılabilir değerler **x86** ve **amd64'tür.**
       - **Test Sürücüsü Bağımsız Değişkenleri**: Test yürütülebilir dosyasına geçirilen komut satırı bağımsız değişkenleri. Tırnak işaretleri de dahil olmak üzere "%test dosyası%" bağımsız değişkeni otomatik olarak hedef dosyaya giden tam yol ile değiştirilir. Bu dosya test sürücüsü tarafından ayrıştırılır ve gereklidir.
       - **Test Sürücü İşlemi Sınav Tamamlandıktan Sonra Çıkış : Test**sürücüsü nün tamamlanmasından sonra sonlandırılması gerekiyorsa bu onay kutusunu seçin. Test sürücüsünün zorla kapatılması gerekiyorsa temizleyin.
       - **Maksimum Süre (saniye cinsinden)**: Hedef programın bir giriş dosyasını ayrıştırmak için gerektirdiği en uzun makul beklenen sürenin tahmini. Tahmin ne kadar doğru sayılsa, bulanık uygulama o kadar verimli çalışır.
       - **Test Sürücüsü Tekrar Tekrar Çalıştırılabilir**: Test sürücüsü kalıcı veya paylaşılan genel duruma bağlı olmadan tekrar tekrar çalıştırabiliyorsa bu onay kutusunu seçin.
       - **Test Sürücüsü Yeniden Adlandırılabilir**: Test sürücüsü çalıştırılan dosya yeniden adlandırılabilir ve hala doğru çalışıyorsa bu onay kutusunu seçin.
       - **Fuzzing Application Tek Bir Işletim Sistemi İşlemi Olarak Çalışır**: Test sürücüsü tek bir işletim sistemi işlemi altında çalışıyorsa bu onay kutusunu seçin. Test sürücüsü ek işlemler doğurursa temizleyin.

Bu görev için YAML yapılandırması hakkında daha fazla bilgi için lütfen [Microsoft Security Risk Detection YAML seçeneklerimizi](yaml-configuration.md#microsoft-security-risk-detection-task) kontrol edin

## <a name="roslyn-analyzers-task"></a>Roslyn Analizörler görev

> [!NOTE]
> Roslyn Çözümleyicileri görevini çalıştırmadan önce, yapınızın bu koşulları karşılaması gerekir:
>
> - Yapı tanımınız, C# veya Visual Basic kodunu derlemek için yerleşik MSBuild veya VSBuild oluşturma görevini içerir. Çözümleyicilerin görevi, MSBuild derlemesini Roslyn çözümleyicileri etkinleştirilmiş olarak çalıştırmak için yerleşik görevin girdi ve çıktısına dayanır.
> - Bu yapı görevini çalıştıran yapı aracısı Visual Studio 2017 sürümü 15.5 veya daha sonra yüklenmiş, böylece derleyici sürüm 2.6 veya daha sonra kullanır.

Görev yapılandırmasının ayrıntıları aşağıdaki listede ve notta gösterilir.

Mevcut seçenekler şunlardır:

- **Kural Kümesi**: Değerler **SDL Gerekli**, **SDL Önerilen**, veya kendi özel kural kümesi.
- **Çözümleyiciler Sürüm**: **En Son**seçeneğini seçmenizi öneririz.
- **Derleyici Uyarılar Bastırma Dosyası**: Bastırılan uyarı ların listesini içeren bir metin dosyası.
- **Denetim Seçenekleri** > **Bu görevi çalıştırın**: Görevin ne zaman çalıştırılacağını belirtir. Daha karmaşık koşullar belirtmek için **Özel koşulları** seçin.

> [!NOTE]
>
> - Roslyn Analizörlerderleyici ile entegre ve csc.exe derleme bir parçası olarak çalıştırılabilir. Bu nedenle, bu görev, yapıda daha önce çalışan derleyici komutunyeniden oynatılmasını veya yeniden çalıştırılmasını gerektirir. Bu yeniden oynatma veya çalıştırma, MSBuild yapı görev günlükleri için Visual Studio Team Services (VSTS) sorgulayarak yapılır.
>
>   Görevin MSBuild derleme komut satırını yapı tanımından güvenilir bir şekilde almanın başka bir yolu yoktur. Kullanıcıların komut satırlarını girmelerine izin vermek için serbest biçimli bir metin kutusu eklemeyi kabul ettik. Ama o zaman bu komut satırlarını güncel ve ana yapıyla senkronize tutmak zor olurdu.
>
>   Özel yapılar, sadece derleyici komutlarının değil, tüm komut kümesinin yeniden oynatılmasını gerektirir. Bu gibi durumlarda, Roslyn Çözümleyicileri etkinleştirme önemsiz veya güvenilir değildir.
>
> - Roslyn Analizörlerderleyici ile entegre edilmiştir. Çağrılması için, Roslyn Çözümleyicileri derleme gerektirir.
>
>   Bu yeni yapı görevi, zaten oluşturulmuş olan C# projeleri yeniden derlenerek uygulanır. Yeni görev, özgün görevle aynı yapı veya yapı tanımında yalnızca MSBuild ve VSBuild yapı görevlerini kullanır. Ancak, bu durumda yeni görev Roslyn Çözümleyicileri etkin bunları kullanır.
>
>   Yeni görev özgün görevle aynı aracıda çalışırsa, yeni görevin çıktısı özgün görevin çıktısının üzerine *s* kaynakları klasöründe yazar. Yapı çıktısı aynı olmasına rağmen, MSBuild'i çalıştırmanızı, çıktıyı yapı evreleme dizinine kopyalamanızı ve sonra Roslyn Çözümleyicileri çalıştırmanızı tavsiye ederiz.

Roslyn Çözümleyicileri görevi için ek kaynaklar için Microsoft Dokümanlar'daki [Roslyn tabanlı Çözümleyiciler'e](https://docs.microsoft.com/dotnet/standard/analyzers/) göz atın.

NuGet sayfasında [microsoft.codeanalysis.FxCopAnalyzeers](https://www.nuget.org/packages/Microsoft.CodeAnalysis.FxCopAnalyzers)bu yapı görevi tarafından yüklenen ve kullanılan analizör paketini bulabilirsiniz.

Bu görev için YAML yapılandırması hakkında bilgi için lütfen [Roslyn Analizörlerimiz YAML seçeneklerimizi](yaml-configuration.md#roslyn-analyzers-task) kontrol edin

## <a name="tslint-task"></a>TSLint görevi

TSLint hakkında daha fazla bilgi için [TSLint GitHub repo'ya](https://github.com/palantir/tslint)gidin.

>[!NOTE] 
>Bildiğiniz gibi, [TSLint GitHub repo](https://github.com/palantir/tslint) ana sayfası TSLint'in 2019 yılında bir ara amortismana tabi tutulacağını söylüyor. Microsoft, ALTERNATIF bir görev olarak [ESLint'i](https://github.com/eslint/eslint) araştırmaktadır.

Bu görev için YAML yapılandırması hakkında bilgi için lütfen [TSLint YAML seçeneklerimizi](yaml-configuration.md#tslint-task) kontrol edin

## <a name="publish-security-analysis-logs-task"></a>Güvenlik Analizi Günlükleri görev yayımla

Görev yapılandırmasının ayrıntıları aşağıdaki ekran görüntüsü ve listede gösterilir.

![Yayımlama Güvenlik Analizi Günlükleri oluşturma görevini yapılandırma](./media/security-tools/9-publish-security-analsis-logs600.png)  

- **Artefakt Adı**: Herhangi bir dize tanımlayıcısı.
- **Yapı Türü**: Seçiminize bağlı olarak, günlükleri Azure DevOps Sunucunuzda veya yapı aracısı tarafından erişilebilen paylaşılan bir dosyada yayımlayabilirsiniz.
- **Araçlar**: Belirli araçlar için günlükleri korumayı seçebilir veya tüm günlükleri korumak için **Tüm Araçlar'ı** seçebilirsiniz.

Bu görev için YAML yapılandırması hakkında daha fazla bilgi için lütfen [Güvenlik Günlükleri Yayımla yaml seçeneklerimizi](yaml-configuration.md#publish-security-analysis-logs-task) kontrol edin

## <a name="security-report-task"></a>Güvenlik Raporu görevi

Güvenlik Raporu yapılandırmasının ayrıntıları aşağıdaki ekran görüntüsü ve listede gösterilir.

![Güvenlik Raporu oluşturma görevini yapılandırma](./media/security-tools/4-createsecurityanalysisreport600.png)

- **Raporlar**: Pipeline **Console,** **TSV Dosyası**ve **Html Dosya** biçimlerinden herhangi birini seçin. Seçili her biçim için bir rapor dosyası oluşturulur.
- **Araçlar**: Algılanan sorunların özetini istediğiniz yapı tanımınızdaki araçları seçin. Seçilen her araç için, yalnızca hataları görüp görmediğiniz veya özet raporda hem hataları hem de uyarıları görüp görmediğiniz seçeneğiniz olabilir.
- **Gelişmiş Seçenekler**: Seçilen araçlardan biri için günlük yoksa, bir uyarı veya hata günlüğe kaydetmeyi seçebilirsiniz. Bir hata günlüğe kaydederseniz, görev başarısız olur.
- **Temel Günlükler Klasörü**: Günlüklerin bulunduğu temel günlükler klasörünü özelleştirebilirsiniz. Ancak bu seçenek genellikle kullanılmaz.

Bu görev için YAML yapılandırması hakkında bilgi için lütfen [Güvenlik raporumuz YAML seçeneklerini](yaml-configuration.md#security-report-task) kontrol edin

## <a name="post-analysis-task"></a>Analiz Sonrası görev

Görev yapılandırmasının ayrıntıları aşağıdaki ekran görüntüsü ve listede gösterilir.

![Analiz Sonrası yapı görevinin yapılandırılması](./media/security-tools/a-post-analysis600.png)

- **Araçlar**: Yapı tanımınızda koşullu olarak bir yapı sonu enjekte etmek istediğiniz araçları seçin. Seçilen her araç için, yalnızca hataları veya hem hataları hem de uyarıları kırmak isteyip istemediğinizi seçmek için bir seçenek olabilir.
- **Rapor**: İsteğe bağlı olarak yapı nın kırılmasına neden olan sonuçları yazabilirsiniz. Sonuçlar Azure DevOps konsol penceresine ve günlük dosyasına yazılır.
- **Gelişmiş Seçenekler**: Seçilen araçlardan biri için günlük yoksa, bir uyarı veya hata günlüğe kaydetmeyi seçebilirsiniz. Bir hata günlüğe kaydederseniz, görev başarısız olur.

Bu görev için YAML yapılandırması hakkında bilgi için lütfen [Post Analysis YAML seçeneklerimizi](yaml-configuration.md#post-analysis-task) kontrol edin

## <a name="next-steps"></a>Sonraki adımlar

YAML tabanlı yapılandırma hakkında daha fazla bilgi için [YAML Yapılandırma kılavuzumuza](yaml-configuration.md)bakın.

Güvenlik Kodu Analizi uzantısı ve sunulan araçlar hakkında başka sorularınız varsa, [SSS sayfamıza](security-code-analysis-faq.md)göz atın.

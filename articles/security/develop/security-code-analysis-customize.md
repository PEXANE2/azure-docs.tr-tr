---
title: Microsoft Azure Güvenlik kodu analiz görevi Özelleştirme Kılavuzu
description: Bu makale, güvenlik kodu analiz uzantısı 'ndaki görevleri özelleştirme hakkında
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
ms.openlocfilehash: ab219b71eb8cd6f6172b7d02a639301c67811b49
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718359"
---
# <a name="how-to-configure--customize-the-build-tasks"></a>Nasıl Yapılır: Yapı görevlerini yapılandırma & özelleştirme

Bu sayfa, her derleme görevinin her birinde kullanılabilen yapılandırma seçeneklerini ayrıntılı olarak açıklar ve güvenlik kodu çözümleme araçları görevlerine ve sonrasında işleme sonrası görevleri izler

## <a name="anti-malware-scanner-task"></a>Kötü amaçlı yazılımdan koruma tarayıcı görevi

> [!NOTE]
> Kötü amaçlı yazılımdan koruma oluşturma görevi, Windows Defender 'ın etkinleştirildiği, "barındırılan VS2017" ve daha sonraki derleme aracılarında doğru olan bir derleme Aracısı gerektirir. (Eski/VS2015 "barındırılan" aracısında çalıştırılmayacak) İmzalar bu aracılarda güncelleştirilemez, ancak imza her zaman görece geçerli olmalıdır ve 3 saatten daha eski olmalıdır.

Aşağıda yapılandırma ile ilgili ekran görüntüsü ve ayrıntılar.

![Kötü amaçlı yazılımdan koruma tarayıcı oluşturma görevini özelleştirme](./media/security-tools/5-add-anti-malware-task600.png) 

- Type = **Basic** için ayarlar
- Tür = **Custom**ile, taramayı özelleştirmek için komut satırı bağımsız değişkenleri sağlanmış olabilir.

Windows Defender, imzaları indirmek ve yüklemek için Windows Update istemcisini kullanır. Derleme aracısında imza güncellemesi başarısız olursa, HRESULT hata kodu muhtemelen Windows Update geliyor. Windows Update hataları ve hafifletme hakkında daha fazla bilgi için [Bu sayfayı](https://docs.microsoft.com/windows/deployment/update/windows-update-error-reference) ve bu [TechNet sayfasını](https://social.technet.microsoft.com/wiki/contents/articles/15260.windows-update-agent-error-codes.aspx) ziyaret edin

## <a name="binskim-task"></a>Binskım görevi

> [!NOTE]
> BinSkim görevini çalıştırmak için bir önkoşul olarak, derlemeniz aşağıdaki koşullardan birini karşılamalıdır.
>    - Derlemeniz yönetilen koddan ikili yapıtlar üretir.
>    - Binskım ile analiz etmek istediğiniz ikili yapıtlara sahip olursunuz.

Aşağıda yapılandırma ile ilgili ekran görüntüsü ve ayrıntılar. 

![Binskım kurulumu](./media/security-tools/7-binskim-task-details.png)  
1. * **. Pdb** hata ayıklama dosyalarını oluşturmak için derleme yapılandırmasını hata ayıklama olarak ayarlayın. Çıktı ikilisinde bulunan sorunları kaynak koduna geri eşlemek için Binskım tarafından kullanılır. 
2. Araştırmamak ve kendi Komut satırlarınızı oluşturmak için Type = **temel** & Function = **Çözümle** öğesini seçin. 
3. **Target** -analiz edilecek bir veya daha fazla ikiliye çözümlenen bir dosya, dizin veya filtre düzenine sahip bir veya daha fazla tanımlayıcı. 
    - Birden çok hedef noktalı virgülle ayrılmalıdır **(;)** . 
    - Tek bir dosya olabilir veya joker karakterler içerebilir.
    - Dizinler her zaman ile bitmelidir\*
    - Örnekler:

           *.dll;*.exe
           $(BUILD_STAGINGDIRECTORY)\*
           $(BUILD_STAGINGDIRECTORY)\*.dll;$(BUILD_STAGINGDIRECTORY)\*.exe;
4. Tür = **komut satırı**' nı seçerseniz, 
     - **Binskim. exe** ' nin ilk bağımsız değişkeninin tam yolları kullanarak **analiz** edin veya kaynak dizine göreli yollar olduğundan emin olun.
     - **Komut satırı** girişi için, birden çok hedefin boşlukla ayrılması gerekir.
     - **/O** veya **/output** dosya parametresini atlayabilirsiniz; Bu, sizin veya değiştirdiğiniz için eklenecektir. 
     - **Standart komut satırı yapılandırması** 

           analyze $(Build.StagingDirectory)\* --recurse --verbose
           analyze *.dll *.exe --recurse --verbose
          > [!NOTE]
          > Hedefin bir \* dizinini veya dizinlerini belirtirken sonda çok önemlidir. 

Komut satırı bağımsız değişkenleri, KIMLIĞE veya çıkış kodlarına göre kurallar hakkında Binskım hakkında daha fazla bilgi için, [Binskım Kullanıcı kılavuzunu](https://github.com/Microsoft/binskim/blob/master/docs/UserGuide.md) ziyaret edin

## <a name="credential-scanner-task"></a>Kimlik bilgisi tarayıcı görevi
Aşağıda yapılandırma ile ilgili ekran görüntüsü ve ayrıntılar.
 
![Kimlik bilgisi tarayıcısı özelleştirmesi](./media/security-tools/3-taskdetails.png)

Kullanılabilir seçenekler şunlardır 
  - **Çıkış biçimi** – TSV/CSV/Sarif/PREfast
  - **Araç sürümü** Önerilen Sürümü
  - **Tarama klasörü** : deponuzdaki Taranacak klasör
  - **Aramacılar dosya türü** -tarama için kullanılan Searchers dosyasını bulma seçenekleri.
  - Gizlemeler **dosyası** : bir [JSON](https://json.org/) dosyası, çıkış günlüğündeki sorunları (Kaynaklar bölümünde daha fazla ayrıntı) için kullanılabilir. 
  - **Ayrıntılı çıkış** -kendi kendine açıklama 
  - **Toplu Iş boyutu** -kimlik bilgisi tarayıcılarını paralel olarak çalıştırmak için kullanılan eşzamanlı iş parçacıklarının sayısı. Varsayılan değer 20 ' dir (değer, 1 ile 2147483647 arasında olmalıdır).
  - **Kaçıncı zaman aşımı** -denetim silinmeden önce bir arama ile eşleşme gerçekleştirmeye harcanan zaman miktarı. 
  - **Dosya taraması okuma arabelleği** boyutu-içerik bayt cinsinden okunurken arabellek boyutu. (Varsayılan olarak 524288) 
  - **En fazla dosya taraması okuma baytı** -içerik analizi sırasında belirli bir dosyadan okunan en fazla bayt sayısı. (Varsayılan olarak 104857600) 
  - **Bu görevi çalıştır** ( **Denetim seçenekleri**altında)-görevin ne zaman çalışacağını belirtir. Daha karmaşık koşullar belirtmek için "özel koşullar" ı seçin. 
  - Azure DevOps içinde **Sürüm** oluşturma görevi sürümü. Sık kullanılmayan. 

## <a name="microsoft-security-risk-detection-task"></a>Microsoft güvenlik riski algılama görevi
> [!NOTE]
> Bu görevi kullanabilmeniz için risk algılama hizmeti ile bir önkoşul olarak bir hesap oluşturmanız ve yapılandırmanız gerekir. Bu hizmet ayrı bir ekleme işlemi gerektirir; Bu uzantıdaki diğer görevlerin çoğu için ' Tak ve Çalıştır ' değildir. Lütfen [Microsoft güvenlik riski algılama](https://aka.ms/msrddocs) ve Microsoft Güvenlik [riski algılama ' ye başvurun: ](https://docs.microsoft.com/security-risk-detection/how-to/) Yönergeler için.

Aşağıda yapılandırma hakkında ayrıntılı bilgi.

Gerekli verileri girin; Her seçenekte, üzerine gelme metni yardımı bulunur.
   - **MSRD Için Azure DevOps hizmeti uç noktası adı**: MSRD örnek URL 'sini (eklendi) depolamak için genel bir Azure DevOps hizmeti uç noktası oluşturduysanız ve REST API erişim belirteci oluşturduysanız, bu hizmet uç noktasını seçebilirsiniz. Aksi takdirde, bu MSRD görevi için yeni bir hizmet uç noktası oluşturmak ve yapılandırmak üzere Yönet bağlantısına tıklayın. 
   - **Hesap kimliği**: MSRD hesap URL 'sinden alınabilecek bir GUID 'dir.
   - **Ikililerin URL 'leri**: Genel olarak kullanılabilir URL 'lerin (ikili dosyaları indirmek için kullanılan makine tarafından kullanılmak üzere) noktalı virgülle ayrılmış bir listesi.
   - **Çekirdek dosyalarının URL 'leri**: Genel olarak kullanılabilir URL 'lerin (Seeds 'yi indirmek için kullanılan makine tarafından kullanılmak üzere) noktalı virgülle ayrılmış bir listesi. Çekirdek dosyalar ikili dosyalarla birlikte indirildiyse Bu alan isteğe bağlıdır.
   - **Işletim sistemi platformu türü**: Üzerinde belirsizlik işini çalıştıracak makinelerin işletim sistemi platform türü (Windows veya Linux).
   - **Windows sürümü/Linux sürümü**: Üzerinde belirsizlik işini çalıştıracak makinelerin işletim sistemi sürümü. Makinelerinizde farklı bir işletim sistemi sürümü varsa varsayılan değerin üzerine yazabilirsiniz.
   - **Paket yükleme betiği**: Belirsizlik işini göndermeden önce test hedef programını ve bağımlılıklarını yüklemek için komut dosyanızı bir test makinesinde çalıştırılacak şekilde sağlayın.
   - **Iş gönderme parametreleri**:
       - **Çekirdek Dizin**: Seeds 'yi içeren belirsizlik makinesindeki dizinin yolu.
       - **Çekirdek uzantısı**: Çekirdekler dosya uzantısı
       - **Test sürücüsü yürütülebilir dosyası**: Hedeflenen makinedeki hedef yürütülebilir dosyanın yolu.
       - **Test sürücüsü yürütülebilir mimarisi**: Hedef yürütülebilir dosya mimarisi (x86 veya amd64).
       - **Test sürücüsü bağımsız değişkenleri**: Test hedefi yürütülebilir dosyasına geçirilen komut satırı bağımsız değişkenleri. Çift tırnak işaretleri dahil olmak üzere "% TestFile%" simgesinin, test sürücüsünün ayrıştırması beklenen hedef dosyanın tam yolu ile otomatik olarak değiştirileceğini ve gerekli olduğunu unutmayın.
       - Test **sürücüsü Işlemi test tamamlandıktan sonra çıkıyor**: Tamamlandıktan sonra test sürücüsünü sonlandırmayı denetle; Test sürücüsünün zorla kapatılması gerekiyorsa işaretini kaldırın.
       - **En uzun süre (saniye cinsinden)** : Hedef programın bir giriş dosyasını ayrıştırabilmesi için gereken en uzun tahmini süreyi tahmin edin. Bu tahmine göre daha doğru, daha verimli bir şekilde çalışır.
       - **Test sürücüsü sürekli çalıştırılabilir**: Kalıcı/paylaşılan küresel duruma bağlı kalmadan test sürücüsünün tekrar tekrar çalıştırılabilirler.
       - **Test sürücüsü yeniden adlandırılabilir**: Test sürücüsü yürütülebilir dosyasının yeniden adlandırıldığını ve yine de düzgün çalışıp çalışmadığını denetleyin.
       - **Uygulama, tek bir işletim sistemi işlemi olarak çalışır**: Test sürücüsünün tek bir işletim sistemi işlemi altında çalışıp çalışmamakta olup olmadığını denetleyin; Test sürücüsünün ek işlemler atamasını kaldırma onay işaretini kaldırın.


## <a name="roslyn-analyzers-task"></a>Roslyn Çözümleyicileri görevi
> [!NOTE]
> Roslyn Çözümleyicisi görevini çalıştırmak için bir önkoşul olarak, derlemeniz aşağıdaki koşullara uymalıdır.
>  - Derleme tanımınız derlemek C# Için yerleşik MSBuild veya vsbuild derleme görevini içerir (veya vb.) kodu. Bu görev, Roslyn Çözümleyicileri etkin olan MSBuild derlemesini yeniden çalıştırmak için söz konusu derleme görevinin giriş ve çıktısına bağımlıdır.
>  - Bu derleme görevinin çalıştırıldığı derleme aracısı, Visual Studio 2017 v 15.5 veya üzeri yüklü (derleyici sürümü 2.6. x).
>

Aşağıda yapılandırma hakkında ayrıntılı bilgi.

Kullanılabilir seçenekler şunlardır 
- **RuleSet** -SDL gereklı, SDL önerilir ya da kendi özel RuleSet 'i kullanabilirsiniz.
- **Çözümleyiciler sürümü** Önerilen Sürümü
- **Derleyici uyarıları gizlemeleri dosyası** -gizlenmesi gereken uyarı kimlikleri listesini içeren bir metin dosyası. 
- **Bu görevi çalıştır** ( **Denetim seçenekleri**altında)-görevin ne zaman çalışacağını belirtir. Daha karmaşık koşullar belirtmek için "**özel koşullar**" ı seçin. 

> [!NOTE]
> - Roslyn Çözümleyicileri derleyici ile tümleşiktir ve yalnızca CSC. exe derlemesinin bir parçası olarak çalıştırılabilir. Bu nedenle, bu görev, derlemede daha önce çalıştırılan derleyici komutunu yeniden çalıştırmayı/yeniden çalıştırmayı gerektirir. Bu, MSBuild derleme görev günlükleri için VSTS sorgulanarak yapılır (görevin, derleme tanımından MSBuild derlemesi komut satırını güvenilir bir şekilde alması için başka bir yol yoktur; kullanıcıların komut satırlarını girmesini sağlamak için bir serbest biçim metin kutusu eklemeyi düşünyoruz , ancak bu durum, ana yapıyla güncel ve eşitlenmiş olarak devam etmek zor olabilir. Özel derlemeler yalnızca derleyici komutlarının değil, tüm komut kümesinin yeniden kullanılmasını gerektirir ve bu durumlarda basit/güvenilir değildir. 
> - Roslyn Çözümleyicileri derleyici ile tümleşiktir ve derlemenin çağrılmasını gerektirir. Bu derleme görevi, yalnızca MSBuild/ C# vsbuild Build görevi kullanılarak oluşturulmuş projeleri yeniden derleyerek aynı derleme/yapı tanımında, ancak bu durumda çözümleyiciler etkin olarak yeniden derlenerek uygulanır. Bu derleme görevi özgün derleme göreviyle aynı aracıda çalışıyorsa, özgün MSBuild/VSBuild derleme görevinin çıktısının bu derleme görevinin çıkışıyla ' kaynaklar klasöründe üzerine yazılır. Yapı çıkışı aynı olacaktır, ancak MSBuild 'i çalıştırmanız, çıkışları yapıt hazırlama dizinine kopyalamanız ve sonra Roslyn 'yi çalıştırmanız önerilir.
>

Roslyn çözümleyicilerine yönelik ek kaynaklar için [Microsoft docs ' de Roslyn çözümleyicilerine](https://docs.microsoft.com/dotnet/standard/analyzers/) bakın

Bu derleme görevi tarafından yüklenen ve kullanılan çözümleyici paketi [burada](https://www.nuget.org/packages/Microsoft.CodeAnalysis.FxCopAnalyzers) bulunabilir 

## <a name="tslint-task"></a>Tslınt görevi

TSLint hakkında daha fazla bilgi için [Tslint GitHub deposunun](https://github.com/palantir/tslint) adresini ziyaret edin
>[!NOTE] 
>Farkında olabilirsiniz. TSLint, 2019 (Kaynak: [Tslınt GitHub deposu](https://github.com/palantir/tslint)) Takım şu anda [eslint](https://github.com/eslint/eslint) 'i alternatif olarak araştırmakta ve eslint için yeni bir görev oluşturmak yol haritasında.

## <a name="publish-security-analysis-logs-task"></a>Güvenlik analizi günlüklerini Yayımla görevi
Aşağıda yapılandırma ile ilgili ekran görüntüsü ve ayrıntılar.

![Yayımla güvenlik analizini özelleştirme](./media/security-tools/9-publish-security-analsis-logs600.png)  

- **Yapıt adı** -herhangi bir dize tanımlayıcı olabilir
- **Yapıt türü** -Azure-DevOps sunucusunda veya yapı Aracısı tarafından erişilebilen bir dosya paylaşımında günlükleri yayımlayabilirsiniz. 
- **Araçlar** -her bir/belirli araç için günlükleri korumayı veya tüm günlükleri korumak Için **tüm araçları** seçmenizi seçebilirsiniz. 

## <a name="security-report-task"></a>Güvenlik raporu görevi
Aşağıda yapılandırma ile ilgili ekran görüntüsü ve ayrıntılar.  
![Analiz sonrası kurulum](./media/security-tools/4-createsecurityanalysisreport600.png) 
- **Raporlar** -oluşturulacak rapor dosyalarını seçin; Her bir **konsol**, **TSV**ve/veya **HTML** biçiminde bir oluşturulur 
- **Araçlar** -Derleme tanımınızda, algılanan sorunların özetini istediğiniz araçları seçin. Her bir araç seçili olduğunda, raporda yalnızca hataları mı yoksa hem hata hem de uyarıları görmek isteyip istemediğinizi belirlemek için bir seçenek olabilir. 
- **Gelişmiş Seçenekler** -seçili araçlardan biri için günlük olmaması olasılığına karşı bir uyarı veya hata günlüğü (ve görevin başarısız olması) seçeneğini belirleyebilirsiniz.
Günlüklerin bulunacağı temel Günlükler klasörünü özelleştirebilirsiniz, ancak bu tipik bir senaryo değildir. 

## <a name="post-analysis-task"></a>Analiz sonrası görev
Aşağıda yapılandırma ile ilgili ekran görüntüsü ve ayrıntılar.

![Analiz sonrası özelleştirme](./media/security-tools/a-post-analysis600.png) 
- **Araçlar** -Derleme tanımınızda, bulgularına göre yapı kesmesi eklemek istediğiniz araçları seçin. Her bir araç seçili olduğunda, yalnızca hatalara mi yoksa hata ve uyarılara mi bölmek istediğinizi belirlemek için bir seçenek olabilir. 
- **Rapor** -isteğe bağlı olarak bulunan sonuçları yazabilir ve derleme kesmesine Azure DevOps konsol penceresine ve günlük dosyasına neden olabilirsiniz. 
- **Gelişmiş Seçenekler** -seçili araçlardan biri için günlük olmaması olasılığına karşı bir uyarı veya hata günlüğü (ve görevin başarısız olması) seçeneğini belirleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uzantı ve sunulan araçlar hakkında başka sorularınız varsa, [SSS sayfamızı inceleyin.](security-code-analysis-faq.md)



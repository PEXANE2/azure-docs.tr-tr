---
title: Bilgi İşlem Emülatör'de Yerel Olarak Bir Bulut Hizmetiprofilleme | Microsoft Dokümanlar
services: cloud-services
description: Visual Studio profil oluşturucusuyla bulut hizmetlerindeki performans sorunlarını araştırın
documentationcenter: ''
author: mikejo
manager: jillfra
editor: ''
tags: ''
ms.assetid: 25e40bf3-eea0-4b0b-9f4a-91ffe797f6c3
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/18/2016
ms.author: mikejo
ms.openlocfilehash: 21270d3c7143ce063ffe30d939368b9813e9072e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70094113"
---
# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>Visual Studio Profiler'ı Kullanarak Azure İşlem Emülatör'ünde Bulut Hizmetinin Performansını Yerel Olarak Test Etme
Bulut hizmetlerinin performansını test etmek için çeşitli araçlar ve teknikler mevcuttur.
Bir bulut hizmetini Azure'da yayımladığınızda, Visual Studio'nun profil oluşturma verilerini toplamasını ve ardından [Azure Uygulamasını Profil Oluşturma'da][1]açıklandığı gibi yerel olarak analiz etmesini sağlayabilirsiniz.
[Azure'da performans sayaçlarını kullanma'da][2]açıklandığı gibi, çeşitli performans sayaçlarını izlemek için tanılama yı da kullanabilirsiniz.
Ayrıca, uygulamanızı buluta dağıtmadan önce işlem emülatöründe yerel olarak profilini çıkarmak isteyebilirsiniz.

Bu makale, emülatörde yerel olarak yapılabilecek cpu örnekleme profiloluşturma yöntemini kapsar. CPU örnekleme çok müdahaleci olmayan bir profil oluşturma yöntemidir. Belirli bir örnekleme aralığında, profil oluşturucu arama yığınının anlık görüntüsünü alır. Veriler belirli bir süre içinde toplanır ve bir raporda gösterilir. Bu profil oluşturma yöntemi, hesaplama açısından yoğun bir uygulamada CPU çalışmalarının çoğunun nerede yapıldığını gösterme eğilimindedir.  Bu, uygulamanızın en çok zaman harcadığı "sıcak yola" odaklanma fırsatı verir.

## <a name="1-configure-visual-studio-for-profiling"></a>1: Profil oluşturma için Visual Studio'yı yapılandırın
İlk olarak, profil oluşturma da yararlı olabilecek birkaç Visual Studio yapılandırma seçeneği vardır. Profil oluşturma raporlarını anlamak için, uygulamanız için sembollere (.pdb dosyaları) ve sistem kitaplıkları için de sembollere ihtiyacınız vardır. Kullanılabilir sembol sunucularına başvuru yaptığınızdan emin olmak istersiniz. Bunu yapmak için, Visual Studio **Araçlar** menüsünde, **Seçenekler**seçin , sonra **Hata Ayıklama**seçin , sonra **Semboller**. Microsoft Symbol **Sunucuları'nın Simge dosyası (.pdb) konumları**altında listelenmiş olduğundan emin olun.  Ayrıca, ek https://referencesource.microsoft.com/symbolssembol dosyaları olabilir başvuru yapabilirsiniz.

![Sembol seçenekleri][4]

İstenirse, Profiloluşturcun oluşturduğu raporları Yalnızca Kodum ayarlayarak basitleştirebilirsiniz. Just My Code etkinleştirildiğinde, işlev çağrı yığınları basitleştirildiğinde, çağrıların kitaplıklara tamamen dahili olması ve .NET Framework'ün raporlardan gizlenmesi sağlanır. **Araçlar** menüsünde **Seçenekler'i**seçin. Ardından **Performans Araçları** düğümlerini genişletin ve **Genel'i**seçin. **Profil oluşturucu raporları için Yalnızca Kodumu Etkinleştir için**onay kutusunu seçin.

![Sadece Kod seçeneklerim][17]

Bu yönergeleri varolan bir projede veya yeni bir projeyle kullanabilirsiniz.  Aşağıda açıklanan teknikleri denemek için yeni bir proje oluşturursanız, bir C# **Azure Bulut Hizmeti** projesi seçin ve bir Web **Rolü** ve **İşçi Rolü**seçin.

![Azure Bulut Hizmeti proje rolleri][5]

Örneğin amaçlar için, projenize çok fazla zaman alan ve bazı belirgin performans sorunu gösteren bazı kodlar ekleyin. Örneğin, bir alt rol projesine aşağıdaki kodu ekleyin:

```csharp
public class Concatenator
{
    public static string Concatenate(int number)
    {
        int count;
        string s = "";
        for (count = 0; count < number; count++)
        {
            s += "\n" + count.ToString();
        }
        return s;
    }
}
```

Bu kodu, alt rolün RoleEntryPoint türetilmiş sınıfındaki RunAsync yönteminden çağırın. (Eşzamanlı olarak çalışan yöntem le ilgili uyarıyı yoksayın.)

```csharp
private async Task RunAsync(CancellationToken cancellationToken)
{
    // TODO: Replace the following with your own logic.
    while (!cancellationToken.IsCancellationRequested)
    {
        Trace.TraceInformation("Working");
        Concatenator.Concatenate(10000);
    }
}
```

Hata ayıklama (Ctrl+F5) olmadan bulut hizmetinizi yerel olarak oluşturun ve çalıştırın ve çözüm yapılandırması **Serbest Bırak'a**ayarlandı. Bu, uygulamanın yerel olarak çalıştırılması için tüm dosya ve klasörlerin oluşturulmasını ve tüm emülatörlerin başlatılmasını sağlar. Çalışan rolünüzün çalıştığını doğrulamak için görev çubuğundan İşlem Emülatör UI'sini başlatın.

## <a name="2-attach-to-a-process"></a>2: Bir işleme iliştirin
Visual Studio 2010 IDE'den başlatarak uygulamanın profilini çıkarmak yerine, profil oluşturucuyu çalışan bir işleme eklemeniz gerekir. 

Profil oluşturucuyu bir işleme eklemek **için, Çözümle** menüsünde **Profiler'ı** seçin ve **Ekle/Ayır.**

![Profil seçeneği ekle][6]

Bir işçi rolü için, WaWorkerHost.exe işlemini bulun.

![WaWorkerHost süreci][7]

Proje klasörünüz bir ağ sürücüsündeyse, profil oluşturucu sizden profil oluşturma raporlarını kaydetmek için başka bir konum sağlamanızı ister.

 Ayrıca WaIISHost.exe ekleyerek bir web rolü ekleyebilirsiniz.
Uygulamanızda birden çok alt rol işlemi varsa, bunları ayırt etmek için işlem kimliğini kullanmanız gerekir. İşlem nesnesi erişerek işlem kimliğini programlı olarak sorgulayabilirsiniz. Örneğin, bu kodu role role RoleEntryPoint türetilmiş sınıfın Çalıştır yöntemine eklerseniz, hangi işleme bağlanacağınıbilmek için Compute Emulator UI'deki günlüklere bakabilirsiniz.

```csharp
var process = System.Diagnostics.Process.GetCurrentProcess();
var message = String.Format("Process ID: {0}", process.Id);
Trace.WriteLine(message, "Information");
```

Günlüğü görüntülemek için İşlem Emülatör UI'sini başlatın.

![İşlem Emülatör UI'sini Başlat][8]

Konsol penceresinin başlık çubuğuna tıklayarak İşlem Emülatör UI'deki alt rol günlüğü konsol penceresini açın. İşlem kimliğini günlükte görebilirsiniz.

![İşlem kimliğini görüntüleme][9]

Eklediğiniz bir senaryoyu yeniden oluşturmak için uygulamanızın Kullanıcı Arabirimi'ndeki (gerekirse) adımları gerçekleştirin.

Profil oluşturmayı durdurmak istediğinizde, **Profil Oluşturmayı Durdur** bağlantısını seçin.

![Profil Oluşturmayı Durdurma seçeneği][10]

## <a name="3-view-performance-reports"></a>3: Performans raporlarını görüntüleme
Uygulamanızın performans raporu görüntülenir.

Bu noktada, profil oluşturucu yürütmeyi durdurur, verileri bir .vsp dosyasına kaydeder ve bu verilerin çözümlemesi gösteren bir rapor görüntüler.

![Profil oluşturucu raporu][11]

Hot Path'te String.wstrcpy'yi görürseniz, yalnızca kullanıcı kodunu göstermek için görünümü değiştirmek için Kodum'u değiştirin.  String.Concat'i görürseniz, Tüm Kodu Göster düğmesine basarak deneyin.

Concatenate yöntemini ve String.Concat'in yürütme süresinin büyük bir kısmını aldığını görmelisiniz.

![Raporun analizi][12]

Bu makalede dize concatenation kodunu eklediyseniz, bunun için Görev Listesi'nde bir uyarı görmeniz gerekir. Ayrıca, oluşturulan ve bertaraf edilen dizelerin sayısına bağlı olarak aşırı miktarda çöp toplama olduğuna dair bir uyarı da görebilirsiniz.

![Performans uyarıları][14]

## <a name="4-make-changes-and-compare-performance"></a>4: Değişiklik yapın ve performansı karşılaştırın
Kod değişikliğinden önceki ve sonraki performansı da karşılaştırabilirsiniz.  Çalışma işlemini durdurun ve string concatenation işlemini StringBuilder kullanımıyla değiştirmek için kodu düzenlediniz:

```csharp
public static string Concatenate(int number)
{
    int count;
    System.Text.StringBuilder builder = new System.Text.StringBuilder("");
    for (count = 0; count < number; count++)
    {
        builder.Append("\n" + count.ToString());
    }
    return builder.ToString();
}
```

Başka bir performans çalışması yapın ve ardından performansı karşılaştırın. Performans Gezgini'nde, çalıştırmalar aynı oturumdaysa, her iki raporu da seçebilir, kısayol menüsünü açabilir ve **Performans Raporlarını Karşılaştır'ı**seçebilirsiniz. Başka bir performans oturumundaki bir çalışmayla karşılaştırmak istiyorsanız, **Analiz menüsünü** açın ve **Performans Raporlarını Karşılaştır'ı**seçin. Görünen iletişim kutusunda her iki dosyayı belirtin.

![Performans raporları seçeneğini karşılaştırma][15]

Raporlar, iki çalıştırma arasındaki farkları vurgular.

![Karşılaştırma raporu][16]

Tebrikler! Profilciyle başladın.

## <a name="troubleshooting"></a>Sorun giderme
* Bir Sürüm yapısının profilini oluşturduğunuzdan emin olun ve hata ayıklamadan başlayın.
* Profiler menüsünde Ekle/Ayırma seçeneği etkin değilse, Performans Sihirbazı'nı çalıştırın.
* Uygulamanızın durumunu görüntülemek için Bilgi İşlem Emülatör UI'sini kullanın. 
* Emülatörde uygulamaları başlatmakta veya profil oluşturucuya takmakta sorun yaşıyorsanız, işlem emülatörü kapatın ve yeniden başlatın. Bu sorunu çözmezse, yeniden başlatmayı deneyin. Çalışan dağıtımları askıya almak ve kaldırmak için İşlem Emülatörü'nü kullanırsanız bu sorun oluşabilir.
* Komut satırındaki profil oluşturma komutlarından herhangi birini, özellikle de genel ayarlardan herhangi birini kullandıysanız, VSPerfClrEnv /globaloff'un çağrıldığından ve VsPerfMon.exe'nin kapatıldığından emin olun.
* Örnekleme yaparken "PRF0025: Veri toplanmadı" iletisini görüyorsanız, iliştirdiğiniz işlemin CPU etkinliği olup olmadığını denetleyin. Herhangi bir hesaplama işi yapmayan uygulamalar herhangi bir örnekleme verisi oluşturmayabilir.  Herhangi bir örnekleme yapmadan önce işlemin çıkmış olması da mümkündür. Profil oluşturma yaptığınız bir rolün Çalıştır yönteminin sonlandırmadığını denetleyin.

## <a name="next-steps"></a>Sonraki Adımlar
Emülatördeki Azure ikililerini emülatöre bağlama, Visual Studio profil oluşturucusunda desteklenmez, ancak bellek ayırmayı test etmek istiyorsanız, profil oluşturma sırasında bu seçeneği seçebilirsiniz. Ayrıca, iş parçacıklarının kilitler için rekabet ederek zaman kaybı olup olmadığını belirlemenize yardımcı olan eşzamanlılık profil oluşturmayı veya bir uygulamakatmanları arasında etkileşim kurarken performans sorunlarını izlemenize yardımcı olan katman etkileşimprofiloluşturmayı da seçebilirsiniz, çoğu genellikle veri katmanı ve bir alt rol arasında.  Uygulamanızın oluşturduğu veritabanı sorgularını görüntüleyebilir ve veritabanı nı kullanımınızı geliştirmek için profil oluşturma verilerini kullanabilirsiniz. Katman etkileşimi profil oluşturma hakkında daha fazla bilgi [için, Visual Studio Team System 2010'da Seviye Etkileşim Profilleyicisini Kullanma: Blog][3]gönderisi Walkthrough'a bakın.

[1]: https://docs.microsoft.com/azure/application-insights/app-insights-profiler
[2]: https://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: https://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png

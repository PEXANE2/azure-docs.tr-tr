---
title: Visual Studio sorunlarını giderme
description: Visual Studio 2013'te yerleşik uzaktan hata ayıklama, izleme ve günlük araçları kullanarak bir Uygulama Hizmeti uygulamasını nasıl gidereceklerini öğrenin.
ms.assetid: def8e481-7803-4371-aa55-64025d116c97
ms.devlang: dotnet
ms.topic: article
ms.date: 08/29/2016
ms.custom: seodec18
ms.openlocfilehash: 3305cfb81980984574961b2a84a056f5d1879ead
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79280059"
---
# <a name="troubleshoot-an-app-in-azure-app-service-using-visual-studio"></a>Visual Studio'yu kullanarak Azure Uygulama Hizmeti'ndeki bir uygulamayı sorun giderme
## <a name="overview"></a>Genel Bakış
Bu öğretici, [Uygulama Hizmeti'ndeki](https://go.microsoft.com/fwlink/?LinkId=529714)bir uygulamayı hata ayıklama moduna [debug mode](https://docs.microsoft.com/visualstudio/debugger/) alarak veya uygulama günlüklerini ve web sunucusu günlüklerini görüntüleyerek hata ayıklama ya da hata ayıklama yardımcı olmak için Visual Studio araçlarının nasıl kullanılacağını gösterir.

Şunları öğreneceksiniz:

* Visual Studio'da hangi uygulama yönetimi işlevleri mevcuttur.
* Uzak bir uygulamada hızlı değişiklikler yapmak için Visual Studio uzaktan görünümü nasıl kullanılır.
* Bir proje Azure'da çalışırken hata ayıklama modunu hem uygulama hem de WebJob için uzaktan çalıştırma.
* Uygulama izleme günlükleri oluşturmak ve uygulama bunları oluştururken bunları görüntülemek için nasıl.
* Ayrıntılı hata iletileri ve başarısız istek izleme dahil olmak üzere web sunucusu günlükleri görüntülemek için nasıl.
* Tanılama günlüklerini bir Azure Depolama hesabına gönderme ve bunları burada görüntüleme.

Visual Studio Ultimate'iniz varsa hata ayıklama için [IntelliTrace'i](/visualstudio/debugger/intellitrace) de kullanabilirsiniz. IntelliTrace bu öğretici kapsamında değildir.

## <a name="prerequisites"></a><a name="prerequisites"></a>Ön koşullar
Bu öğretici, [Azure Uygulama Hizmeti'nde ASP.NET bir uygulama oluşturun'da](app-service-web-get-started-dotnet-framework.md)kurduğunuz geliştirme ortamı, web projesi ve Uygulama Hizmeti uygulamasıyla çalışır. Web İşler bölümleri için Azure [Web İşleri SDK ile Başlat'ta][GetStartedWJ]oluşturduğunuz uygulamaya ihtiyacınız vardır.

Bu öğreticide gösterilen kod örnekleri Bir C# MVC web uygulaması içindir, ancak sorun giderme yordamları Visual Basic ve Web Forms uygulamaları için aynıdır.

Öğretici, Visual Studio 2019'u kullandığınızı varsayar. 

Akış günlükleri özelliği yalnızca .NET Framework 4 veya sonraki leri hedefleyen uygulamalar için çalışır.

## <a name="app-configuration-and-management"></a><a name="sitemanagement"></a>Uygulama yapılandırması ve yönetimi
Visual Studio, [Azure portalında](https://go.microsoft.com/fwlink/?LinkId=529715)bulunan uygulama yönetimi işlevlerinin ve yapılandırma ayarlarının bir alt kümesine erişim sağlar. Bu bölümde, **Server Explorer'ı**kullanarak nelerin kullanılabileceğini göreceksiniz. En son Azure tümleştirme özelliklerini görmek için **Cloud Explorer'ı** da deneyin. **Görünüm** menüsünden her iki pencereyi de açabilirsiniz.

1. Visual Studio'da Azure'da oturum açmadıysanız, **Azure'a** sağ tıklayın ve **Server Explorer'da** **Microsoft Azure Aboneliğine** Bağlan'ı seçin.

    Alternatif olarak hesabınıza erişim sağlayan bir yönetim sertifikası yüklemek tir. Bir sertifika yüklemeyi seçerseniz, **Server Explorer'da** **Azure** düğümüne sağ tıklayın ve ardından bağlam menüsünde **Abonelikleri Yönet ve Filtrele'yi** seçin. Microsoft **Azure Aboneliklerini Yönet** iletişim **kutusunda, Sertifikalar** sekmesini tıklatın ve sonra **Içe Aktar'ı**tıklatın. Azure hesabınız için bir abonelik dosyası *(.publishsettings* dosyası olarak da adlandırılır) indirmek ve sonra içe aktarma yönergelerini izleyin.

   > [!NOTE]
   > Bir abonelik dosyası indirirseniz, dosyayı kaynak kod dizinlerinizin dışındaki bir klasöre kaydedin (örneğin, Karşıdan Yüklemeler klasöründe) ve alma işlemi tamamlandıktan sonra silin. Abonelik dosyasına erişim elde eden kötü niyetli bir kullanıcı Azure hizmetlerinizi düzenleyebilir, oluşturabilir ve silebilir.
   >
   >

    Visual Studio'dan Azure kaynaklarına bağlanma hakkında daha fazla bilgi için [hesapları, abonelikleri ve Yönetimsel Rolleri Yönet'e](https://go.microsoft.com/fwlink/?LinkId=324796#BKMK_AccountVCert)bakın.
2. **Sunucu Gezgini'nde** **Azure'u** genişletin ve **Uygulama Hizmeti'ni**genişletin.
3. Azure Uygulama Hizmeti'nde ASP.NET uygulaması [oluştur'da](app-service-web-get-started-dotnet-framework.md)oluşturduğunuz uygulamayı içeren kaynak grubunu genişletin ve ardından uygulama düğümüne sağ tıklayın ve Ayarları **Görüntüle'yi**tıklatın.

    ![Sunucu Gezgini'nde Ayarları Görüntüle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewsettings.png)

    **Azure Web Uygulaması** sekmesi görüntülenir ve Visual Studio'da kullanılabilen uygulama yönetimi ve yapılandırma görevlerini orada görebilirsiniz.

    ![Azure Web Uygulaması penceresi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configtab.png)

    Bu eğitimde, günlüğe kaydetme ve açılır bırakmaları izleme yi kullanırsınız. Uzaktan hata ayıklama da kullanırsınız, ancak bunu etkinleştirmek için farklı bir yöntem kullanırsınız.

    Bu penceredeki Uygulama Ayarları ve Bağlantı Dizeleri kutuları hakkında bilgi için [Azure Uygulama Hizmeti: Uygulama Dizeleri ve Bağlantı Dizeleri Nasıl Çalışır.](https://azure.microsoft.com/blog/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)

    Bu pencerede yapılabilen bir uygulama yönetimi görevi gerçekleştirmek istiyorsanız, Azure portalına bir tarayıcı penceresi açmak için **Yönetim Portalı'nda Aç'ı** tıklatın.

## <a name="access-app-files-in-server-explorer"></a><a name="remoteview"></a>Server Explorer'daki uygulama dosyalarına erişin
Genellikle Web.config dosyasında `customErrors` bayrak la birlikte bir web `On` `RemoteOnly`projesi dağıtırsınız veya bir sorun olduğunda yararlı bir hata iletisi almadığınız anlamına gelir. Birçok hata için, tüm olsun aşağıdakilerden biri gibi bir sayfa:

**'/' Uygulamasında Sunucu Hatası:**

![Yardımcı olmayan hata sayfası](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror.png)

**Bir hata oluştu:**

![Yardımcı olmayan hata sayfası](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror1.png)

**Web sitesi sayfayı görüntüleyemez**

![Yardımcı olmayan hata sayfası](./media/web-sites-dotnet-troubleshoot-visual-studio/genericerror2.png)

Genellikle hatanın nedenini bulmanın en kolay yolu, önceki ekran görüntülerinin ilkinin nasıl yapılacağını açıkladığı ayrıntılı hata iletilerini etkinleştirmektir. Bu, dağıtılan Web.config dosyasında bir değişiklik gerektirir. Projedeki *Web.config* dosyasını düzenlemeve projeyi yeniden dağıtabilir veya bir [Web.config dönüşümü](https://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/web-config-transformations) oluşturabilir ve hata ayıklama oluşturmayı dağıtabilirsiniz, ancak daha hızlı bir yol vardır: **Solution Explorer'da** *uzak görünüm* özelliğini kullanarak uzak uygulamada dosyaları doğrudan görüntüleyebilir ve düzenlemeyapabilirsiniz.

1. **Sunucu Gezgini'nde,** **Azure'u**genişletin, **Uygulama Hizmeti'ni**genişletin, uygulamanızın bulunduğu kaynak grubunu genişletin ve ardından uygulamanızın düğümlerini genişletin.

    Uygulamanın içerik dosyalarına ve günlük dosyalarına erişmenizi sağlayan düğümler görürsünüz.
2. **Dosyalar** düğümlerini genişletin ve *Web.config* dosyasını çift tıklatın.

    ![Web.config'i aç](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfig.png)

    Visual Studio, uzak uygulamadan Web.config dosyasını açar ve başlık çubuğundaki dosya adının yanında [Remote] gösterir.
3. `system.web` Öğeye aşağıdaki satırı ekleyin:

    `<customErrors mode="Off"></customErrors>`

    ![Web.config'i edit](./media/web-sites-dotnet-troubleshoot-visual-studio/webconfigedit.png)
4. Yardımcı olmayan hata iletisini gösteren tarayıcıyı yenileyin ve şimdi aşağıdaki örnek gibi ayrıntılı bir hata iletisi alırsınız:

    ![Ayrıntılı hata iletisi](./media/web-sites-dotnet-troubleshoot-visual-studio/detailederror.png)

    (Gösterilen hata *Görünümler\Home\Index.cshtml*kırmızı gösterilen satır eklenerek oluşturuldu .)

Web.config dosyasını düzenlemek, Uygulama Hizmeti uygulamanızdaki dosyaları okuma ve düzenleme olanağının sorun gidermeyi kolaylaştırdığı senaryolara yalnızca bir örnektir.

## <a name="remote-debugging-apps"></a><a name="remotedebug"></a>Uzaktan hata ayıklama uygulamaları
Ayrıntılı hata iletisi yeterli bilgi sağlemiyorsa ve hatayı yerel olarak yeniden oluşturamıyorsanız, sorun gidermenin başka bir yolu hata ayıklama modunda uzaktan çalışmaktır. Kesme noktaları ayarlayabilir, belleği doğrudan işleyebilir, koda basabilir ve hatta kod yolunu değiştirebilirsiniz.

Uzaktan hata ayıklama Visual Studio Express sürümlerinde çalışmaz.

Bu bölümde, Azure Uygulama Hizmeti'nde ASP.NET [bir uygulama oluşturun'da](app-service-web-get-started-dotnet-framework.md)oluşturduğunuz projeyi uzaktan nasıl kullanarak hata ayıklama gösterilmektedir.

1. Azure Uygulama Hizmeti'nde [ASP.NET uygulaması oluştur'da](app-service-web-get-started-dotnet-framework.md)oluşturduğunuz web projesini açın.

1. Açık *Denetleyiciler\HomeController.cs*.

1. `About()` Yöntemi silin ve yerine aşağıdaki kodu ekleyin.

    ```csharp
    public ActionResult About()
    {
        string currentTime = DateTime.Now.ToLongTimeString();
        ViewBag.Message = "The current time is " + currentTime;
        return View();
    }
    ```

1. Hatta [bir kesme noktası ayarlayın.](https://docs.microsoft.com/visualstudio/debugger/) `ViewBag.Message`

1. **Çözüm Gezgini'nde**projeyi sağ tıklatın ve **Yayımla'yı**tıklatın.

1. **Profil** açılır listesinde, [Azure Uygulama Hizmeti'nde ASP.NET bir uygulama oluşturma'da](app-service-web-get-started-dotnet-framework.md)kullandığınız profilin aynısını seçin. Ardından Ayarlar'ı tıklatın.

1. **Yayımla** iletişim kutusunda Ayarlar **sekmesini** tıklatın ve ardından **Yapılandırmayı** **Hata Ayıklama**olarak değiştirin ve sonra **Kaydet'i**tıklatın.

    ![Hata ayıklama modunda yayımlama](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-publishdebug.png)

1. **Yayımla**’ta tıklayın. Dağıtım bittikten ve tarayıcınız uygulamanızın Azure URL'sine açıldıktan sonra tarayıcıyı kapatın.

1. **Server Explorer'da**uygulamanızı sağ tıklatın ve ardından **Hata Ayıklama'yı**takın'ı tıklatın.

    ![Hata ayıklama ekleme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-attachdebugger.png)

    Tarayıcı otomatik olarak Azure'da çalışan ana sayfanıza açılır. Azure sunucuyu hata ayıklama için ayarlarken 20 saniye kadar beklemeniz gerekebilir. Bu gecikme, yalnızca 48 saatlik bir süre içinde bir uygulamada hata ayıklama modunda ilk kez çalıştırdığınızda gerçekleşir. Aynı dönemde yeniden hata ayıklamaya başladığınızda, gecikme olmaz.

    > [!NOTE] 
    > Hata ayıklamayı başlatmakta sorun yaşıyorsanız, **Sunucu Gezgini**yerine **Cloud Explorer'ı** kullanarak bunu yapmaya çalışın.
    >

1. Menüde **Hakkında'yı** tıklatın.

    Visual Studio kesme noktasında durur ve kod yerel bilgisayarınızda değil, Azure'da çalışır.

1. Zaman değerini `currentTime` görmek için değişkenin üzerine titreyin.

    ![Azure'da çalışan hata ayıklama modunda değişkeni görüntüleme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugviewinwa.png)

    Gördüğünüz saat, yerel bilgisayarınızdan farklı bir saat diliminde olabilecek Azure sunucu saatidir.

1. `currentTime` Değişken için "Şimdi Azure'da çalışıyor" gibi yeni bir değer girin.

1. Çalışmaya devam etmek için F5 tuşuna basın.

     Azure'da çalışan Hakkında sayfası, geçerli Zaman değişkenine girdiğiniz yeni değeri görüntüler.

     ![Yeni değeri olan sayfa hakkında](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugchangeinwa.png)

## <a name="remote-debugging-webjobs"></a><a name="remotedebugwj"></a>Uzaktan hata ayıklama Webİşler
Bu bölümde, [Azure Web İşleri SDK ile Başlat'ta](https://github.com/Azure/azure-webjobs-sdk/wiki)oluşturduğunuz proje ve uygulamayı uzaktan nasıl kullanarak hata ayıklama yapılacağını gösterir.

Bu bölümde gösterilen özellikler yalnızca Visual Studio 2013'te Güncelleme 4 veya sonraki özelliklerle birlikte kullanılabilir.

Uzaktan hata ayıklama yalnızca sürekli Webİşler ile çalışır. Zamanlanmış ve isteğe bağlı Webİşler hata ayıklamayı desteklemez.

1. [Azure Webİşler SDK ile Başlat'ta][GetStartedWJ]oluşturduğunuz web projesini açın.

2. ContosoAdsWebJob projesinde, açık *Functions.cs.*

3. Yöntemdeki ilk deyimde [bir kesme noktası ayarlayın.](https://docs.microsoft.com/visualstudio/debugger/) `GnerateThumbnail`

    ![Kesme noktasını ayarlama](./media/web-sites-dotnet-troubleshoot-visual-studio/wjbreakpoint.png)

4. **Çözüm Gezgini'nde**web projesine (WebJob projesi değil) sağ tıklayın ve **Yayımla'yı**tıklatın.

5. **Profil** açılır listesinde, [Azure Web İşleri SDK ile Başlat'ta](https://github.com/Azure/azure-webjobs-sdk/wiki)kullandığınız profilin aynısını seçin.

6. **Ayarlar** sekmesini tıklatın ve **Yapılandırmayı** **Hata Ayıklama**olarak değiştirin ve sonra **Yayımla'yı**tıklatın.

    Visual Studio web ve WebJob projelerini dağıtır ve tarayıcınız uygulamanızın Azure URL'sine açılır.

7. **Server Explorer'da**Azure > Uygulama Hizmeti'ni > **kaynak grubunuza > genişletin > Sürekli, uygulamanız > Webİşler >,** ve ardından **ContosoAdsWebJob'a**sağ tıklayın.

8. **Hata Ayıklama Ekle'yi**tıklatın.

    ![Hata ayıklama ekleme](./media/web-sites-dotnet-troubleshoot-visual-studio/wjattach.png)

    Tarayıcı otomatik olarak Azure'da çalışan ana sayfanıza açılır. Azure sunucuyu hata ayıklama için ayarlarken 20 saniye kadar beklemeniz gerekebilir. Bu gecikme, yalnızca 48 saatlik bir süre içinde bir uygulamada hata ayıklama modunda ilk kez çalıştırdığınızda gerçekleşir. Aynı dönemde yeniden hata ayıklamaya başladığınızda, gecikme olmaz.

9. Contoso Ads giriş sayfasına açılan web tarayıcısında yeni bir reklam oluşturun.

    Reklam oluşturmak, WebJob tarafından alınan ve işlenen bir sıra iletisinin oluşturulmasına neden olur. WebJobs SDK sıra iletisini işlemek için işlevi aradığında, kod kesme noktanıza vurur.

10. Hata ayıklama kesme noktanızda kırıldığında, program bulutu çalıştırırken değişken değerlerini inceleyebilir ve değiştirebilirsiniz. Aşağıdaki resimde, hata ayıklama `GenerateThumbnail` yönteme geçirilen blobInfo nesnesinin içeriğini gösterir.

     ![debugger blobInfo nesne](./media/web-sites-dotnet-troubleshoot-visual-studio/blobinfo.png)

11. Çalışmaya devam etmek için F5 tuşuna basın.

     Yöntem `GenerateThumbnail` küçük resmi oluşturmayı bitirir.

12. Tarayıcıda, Dizin sayfasını yenileyin ve küçük resmi görün.

13. Visual Studio'da hata ayıklamayı durdurmak için SHIFT+F5 tuşuna basın.

14. **Server Explorer'da**ContosoAdsWebJob düğümüne sağ tıklayın ve **Panoyu Görüntüle'yi**tıklatın.

15. Azure kimlik bilgilerinizle oturum açın ve ardından WebJob'unuzun sayfasına gitmek için WebJob adını tıklatın.

     ![ContosoAdsWebJob'u tıklatın](./media/web-sites-dotnet-troubleshoot-visual-studio/clickcaw.png)

     Pano, işlevin `GenerateThumbnail` yakın zamanda yürütüldettiğini gösterir.

     **(Panoyu Görüntüle'yi**bir sonraki tıklattığınızda oturum açmanız gerekmez ve tarayıcı doğrudan WebJob'unuzun sayfasına gider.)

16. İşlev yürütmesi ile ilgili ayrıntıları görmek için işlev adını tıklatın.

     ![Fonksiyon ayrıntıları](./media/web-sites-dotnet-troubleshoot-visual-studio/funcdetails.png)

Işleviniz [günlükleri yazdıysa,](https://github.com/Azure/azure-webjobs-sdk/wiki)bunları görmek için **ToggleOutput'u** tıklatabilirsiniz.

## <a name="notes-about-remote-debugging"></a>Uzaktan hata ayıklama hakkında notlar

* Üretimde hata ayıklama modunda çalıştırmak önerilmez. Üretim uygulamanız birden çok sunucu örneğine ölçeklendirilemezse, hata ayıklama web sunucusunun diğer isteklere yanıt vermesini engeller. Birden çok web sunucusu örneğiniz varsa, hata ayıklayıcıya iliştirdiğinizde rasgele bir örnek alırsınız ve sonraki tarayıcı isteklerinin aynı örne gittiğinden emin olmak için hiçbir yolunuz yoktur. Ayrıca, genellikle üretime hata ayıklama yapısı dağıtmazsınız ve sürüm yapılarının derleyici optimizasyonları kaynak kodunuzda neler olduğunu satır satır göstermeyi imkansız hale getirebilir. Üretim sorunlarını gidermek için en iyi kaynağınız uygulama izleme ve web sunucusu günlükleridir.
* Uzaktan hata ayıklama sırasında kesme noktalarında uzun duraklardan kaçının. Azure, birkaç dakikadan uzun süre durdurulan bir işlemi yanıt vermeyen bir işlem olarak ele verir ve kapatır.
* Siz hata ayıklarken, sunucu Visual Studio'ya bant genişliği ücretlerini etkileyebilecek veri gönderiyor. Bant genişliği oranları hakkında daha fazla bilgi için [Azure Fiyatlandırması'na](https://azure.microsoft.com/pricing/calculator/)bakın.
* `debug` *Web.config* dosyasındaki `compilation` öğenin özniteliğinin doğru ayarlandığından emin olun. Hata ayıklama yapı yapılandırması yayımladığınızda varsayılan olarak doğru olarak ayarlanır.

    ```xml
    <system.web>
      <compilation debug="true" targetFramework="4.5" />
      <httpRuntime targetFramework="4.5" />
    </system.web>
    ```
* Hata ayıklayıcının hata ayıklamak istediğiniz koda adım atmadığını fark ederseniz, Just My Code ayarını değiştirmeniz gerekebilir.  Daha fazla bilgi için bkz. Visual [Studio'da Yalnızca Kodum'u kullanarak yalnızca kullanıcı kodunu hata ayıklayıp ayırmayacağınızda belirtin.](https://docs.microsoft.com/visualstudio/debugger/just-my-code)
* Uzaktan hata ayıklama özelliğini etkinleştirdiğinizde sunucuda bir zamanlayıcı başlar ve 48 saat sonra özellik otomatik olarak kapatılır. Bu 48 saatlik sınır, güvenlik ve performans nedenleriyle yapılır. Özelliği istediğiniz kadar kolayca açabilirsiniz. Etkin olarak hata ayıklama dığınızda devre dışı bırakmanızı öneririz.
* Hata ayıklama işlemini yalnızca uygulama işlemine (w3wp.exe) değil, herhangi bir işleme el ile ekleyebilirsiniz. Visual Studio'da hata ayıklama modunun nasıl kullanılacağı hakkında daha fazla bilgi için [Visual Studio'da Hata Ayıklama](/visualstudio/debugger/debugging-in-visual-studio)bölümüne bakın.

## <a name="diagnostic-logs-overview"></a><a name="logsoverview"></a>Tanılama günlüklerine genel bakış
Bir Uygulama Hizmeti uygulamasında çalışan bir ASP.NET uygulaması aşağıdaki tür günlükleri oluşturabilir:

* **Uygulama izleme günlükleri**<br/>
  Uygulama [System.Diagnostics.Trace](/dotnet/api/system.diagnostics.trace) sınıfının metotlarını arayarak bu günlükleri oluşturur.
* **Web sunucusu günlükleri**<br/>
  Web sunucusu, uygulamaya yönelik her HTTP isteği için bir günlük girişi oluşturur.
* **Ayrıntılı hata ileti günlükleri**<br/>
  Web sunucusu, başarısız HTTP istekleri (durum kodu 400 veya daha büyük sonuçlanan istekler) için bazı ek bilgileri içeren bir HTML sayfası oluşturur.
* **Başarısız istek izleme günlükleri**<br/>
  Web sunucusu, başarısız HTTP istekleri için ayrıntılı izleme bilgileri içeren bir XML dosyası oluşturur. Web sunucusu ayrıca bir tarayıcıda XML biçimlendirmek için bir XSL dosyası sağlar.

Günlüğe kaydetme uygulama performansını etkiler, bu nedenle Azure size her günlük türünü gerektiği gibi etkinleştirme veya devre dışı etme olanağı sağlar. Uygulama günlükleri için, yalnızca belirli bir önem düzeyinin üzerindeki günlüklerin yazılması gerektiğini belirtebilirsiniz. Yeni bir uygulama oluşturduğunuzda, varsayılan olarak tüm günlük ler devre dışı bırakılır.

Günlükler uygulamanızın dosya sistemindeki *LogFiles* klasöründeki dosyalara yazılır ve FTP üzerinden erişilebilir. Web sunucusu günlükleri ve uygulama günlükleri bir Azure Depolama hesabına da yazılabilir. Bir depolama hesabında, dosya sisteminde mümkün olandan daha fazla günlük hacmi ni tutabilirsiniz. Dosya sistemini kullandığınızda en fazla 100 megabayt lık günlükle sınırlısınız. (Dosya sistem günlükleri yalnızca kısa süreli bekletme içindir. Azure, sınıra ulaşıldıktan sonra yenilerine yer açmak için eski günlük dosyalarını siler.)  

## <a name="create-and-view-application-trace-logs"></a><a name="apptracelogs"></a>Uygulama izleme günlükleri oluşturma ve görüntüleme
Bu bölümde, aşağıdaki görevleri yapın:

* [Azure ve ASP.NET ile başlayın'da](app-service-web-get-started-dotnet-framework.md)oluşturduğunuz web projesine izleme bildirimleri ekleyin.
* Projeyi yerel olarak çalıştırdığınızda günlükleri görüntüleyin.
* Azure'da çalışan uygulama tarafından oluşturulan günlükleri görüntüleyin.

Web İşler'de uygulama günlüklerinin nasıl oluşturulabildiğini öğrenmek için, Webİşler SDK 'yi kullanarak Azure sıra depolama alanında [nasıl çalışılabilene bakın - Günlükleri nasıl yazılır.](https://github.com/Azure/azure-webjobs-sdk/wiki) Günlükleri görüntülemek ve Azure'da nasıl depolanırlarını denetlemek için aşağıdaki yönergeler, WebJobs tarafından oluşturulan uygulama günlükleri için de geçerlidir.

### <a name="add-tracing-statements-to-the-application"></a>Uygulamaya izleme ifadeleri ekleme
1. Açık *Denetleyiciler\HomeController.cs*, `Index`ve `About`için `Contact` deyimler ve bir `Trace` `using` ifade eklemek için `System.Diagnostics`aşağıdaki kodu ile , ve yöntemleri değiştirin:

    ```csharp
    public ActionResult Index()
    {
        Trace.WriteLine("Entering Index method");
        ViewBag.Message = "Modify this template to jump-start your ASP.NET MVC application.";
        Trace.TraceInformation("Displaying the Index page at " + DateTime.Now.ToLongTimeString());
        Trace.WriteLine("Leaving Index method");
        return View();
    }
    
    public ActionResult About()
    {
        Trace.WriteLine("Entering About method");
        ViewBag.Message = "Your app description page.";
        Trace.TraceWarning("Transient error on the About page at " + DateTime.Now.ToShortTimeString());
        Trace.WriteLine("Leaving About method");
        return View();
    }
    
    public ActionResult Contact()
    {
        Trace.WriteLine("Entering Contact method");
        ViewBag.Message = "Your contact page.";
        Trace.TraceError("Fatal error on the Contact page at " + DateTime.Now.ToLongTimeString());
        Trace.WriteLine("Leaving Contact method");
        return View();
    }        
    ```

1. Dosyanın `using System.Diagnostics;` üstüne bir deyim ekleyin.

### <a name="view-the-tracing-output-locally"></a>İzleme çıktısını yerel olarak görüntüleme
1. Uygulamayı hata ayıklama modunda çalıştırmak için F5 tuşuna basın.

    Varsayılan izleme dinleyicisi, diğer Hata Ayıklama çıktısıyla birlikte tüm izleme çıktısını **Çıktı** penceresine yazar. Aşağıdaki resimde `Index` yönteme eklediğiniz izleme deyimlerinden çıktı gösterilmektedir.

    ![Hata Ayıklama penceresinde izleme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-debugtracing.png)

    Aşağıdaki adımlar, hata ayıklama modunda derleme den önce bir web sayfasında izleme çıktısının nasıl görüntülenebildiğini gösterir.
1. Uygulama Web.config dosyasını (proje klasöründe bulunan dosya) `<system.diagnostics>` açın ve kapatma `</configuration>` öğesihemen önce dosyanın sonuna bir öğe ekleyin:

    ``` xml
    <system.diagnostics>
    <trace>
      <listeners>
        <add name="WebPageTraceListener"
            type="System.Web.WebPageTraceListener,
            System.Web,
            Version=4.0.0.0,
            Culture=neutral,
            PublicKeyToken=b03f5f7f11d50a3a" />
      </listeners>
    </trace>
    </system.diagnostics>
    ```

'ye `WebPageTraceListener` `/trace.axd`göz atarak izleme çıktısını görüntülemenizi sağlar.
1. Aşağıdaki <a href="https://msdn.microsoft.com/library/vstudio/6915t83k(v=vs.100).aspx">trace element</a> örnek gibi `<system.web>` Web.config dosyasında altında bir izleme öğesi ekleyin:

    ``` xml
    <trace enabled="true" writeToDiagnosticsTrace="true" mostRecent="true" pageOutput="false" />
    ```

1. Uygulamayı çalıştırmak için CTRL+F5'e basın.
1. Tarayıcı penceresinin adres çubuğuna URL'ye *trace.axd* ekleyin ve Enter tuşuna `http://localhost:53370/trace.axd`basın (URL benzer).
1. Uygulama **İzleme** sayfasında, ilk satırdaki **Ayrıntıları Görüntüle'yi** tıklatın (BrowserLink satırında değil).

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd1.png)

    **İstek Ayrıntıları** sayfası görüntülenir ve **İzleme Bilgileri** bölümünde `Index` yönteme eklediğiniz izleme ifadelerinden çıktı görürsünüz.

    ![Trace.axd](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-traceaxd2.png)

    Varsayılan olarak, `trace.axd` yalnızca yerel olarak kullanılabilir. Uzaktan bir uygulamadan kullanılabilir hale getirmek istiyorsanız, `localOnly="false"` aşağıdaki `trace` örnekte gösterildiği gibi *Web.config* dosyasındaki öğeye ekleyebilirsiniz:

        <trace enabled="true" writeToDiagnosticsTrace="true" localOnly="false" mostRecent="true" pageOutput="false" />

    Ancak, bir `trace.axd` üretim uygulamasında etkinleştirme, güvenlik nedenleriyle önerilmez. Aşağıdaki bölümlerde, bir Uygulama Hizmeti uygulamasındaki izleme günlüklerini okumanın daha kolay bir yolunu görürsünüz.

### <a name="view-the-tracing-output-in-azure"></a>Azure'da izleme çıktısını görüntüleme
1. **Çözüm Gezgini'nde**, web projesine sağ tıklayın ve **Yayımla'yı**tıklatın.
2. Web **Yayımla** iletişim kutusunda **Yayımla'yı**tıklatın.

    Visual Studio güncelleştirmenizi yayımladıktan sonra, ana sayfanıza bir tarayıcı penceresi açar **(Bağlantı** sekmesinde **Hedef URL'yi** temizlemediğinizi varsayarsak).
3. **Server Explorer'da**uygulamanızı sağ tıklatın ve **Akış Günlüklerini Görüntüle'yi**seçin.

    ![İçerik menüsünde Akış Günlüklerini Görüntüle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-viewlogsmenu.png)

    **Çıktı** penceresi, günlük akışı hizmetine bağlı olduğunuzu gösterir ve görüntülenecek bir günlük olmadan geçen her dakika bir bildirim satırı ekler.

    ![İçerik menüsünde Akış Günlüklerini Görüntüle](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-nologsyet.png)
4. Uygulama giriş sayfanızı gösteren tarayıcı penceresinde **İletişim'i**tıklatın.

    Birkaç saniye içinde, `Contact` yönteme eklediğiniz hata düzeyindeki izlemeden çıktı **Çıkış** penceresinde görüntülenir.

    ![Çıkış penceresinde hata izleme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-errortrace.png)

    Visual Studio yalnızca hata düzeyinde izlemeler gösteriyor, çünkü günlük izleme hizmetini etkinleştirdiğinizde varsayılan ayar bu. Yeni bir Uygulama Hizmeti uygulaması oluşturduğunuzda, ayarlar sayfasını daha önce açtığınızda gördüğünüz gibi, tüm günlük ler varsayılan olarak devre dışı bırakılır:

    ![Uygulama Oturumu Kapatma](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-apploggingoff.png)

    Ancak, Akış **Günlüklerini Görüntüle'yi**seçtiğinizde, Visual Studio uygulama **günlüğünü (Dosya Sistemi)** otomatik olarak **Hata**olarak değiştirir, bu da hata düzeyi günlüklerinin raporlandığı anlamına gelir. Tüm izleme günlüklerinizi görmek için bu ayarı **Verbose**olarak değiştirebilirsiniz. Hatadan daha düşük bir önem düzeyi seçtiğinizde, daha yüksek önem düzeyleri için tüm günlükleri de bildirilir. Bu nedenle, ayrıntılı olarak seçtiğinizde, bilgi, uyarı ve hata günlüklerini de görürsünüz.  

5. **Server Explorer'da**uygulamayı sağ tıklatın ve ardından daha önce yaptığınız gibi **Ayarları Görüntüle'yi** tıklatın.
6. **Uygulama Günlüğe Kaydetme (Dosya Sistemi)** **verbose**olarak değiştirin ve sonra **Kaydet'i**tıklatın.

    ![İzleme düzeyini Verbose'a ayarlama](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-applogverbose.png)
7. **İletişim** sayfanızı gösteren tarayıcı penceresinde Ana **Sayfa'yı**tıklatın, ardından **Hakkında'yı**tıklatın ve ardından **İletişim'i**tıklatın.

    Birkaç saniye **içinde, Çıktı** penceresi tüm izleme çıktınızı gösterir.

    ![Verbose izleme çıkışı](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-verbosetraces.png)

    Bu bölümde, uygulama ayarlarını kullanarak günlük özelliğini etkinleştirdiniz ve devre dışı bıraktınız. Ayrıca, Web.config dosyasını değiştirerek izleme dinleyicilerini etkinleştirebilir ve devre dışı kullanabilirsiniz. Ancak, Web.config dosyasını değiştirmek uygulama etki alanının geri dönüştürülmesine neden olurken, uygulama yapılandırması üzerinden günlüğe kaydetmeyi etkinleştirmek bunu yapmaz. Sorunun yeniden oluşturulması uzun zaman alıyorsa veya aralıklı ysa, uygulama etki alanının geri dönüşümü onu "düzeltebilir" ve yeniden gerçekleşene kadar beklemenizi sağlayabilir. Azure'da tanılamayı etkinleştirmek, uygulama etki alanını geri dönüştürmeden hata bilgilerini hemen yakalamaya başlamanızı sağlar.

### <a name="output-window-features"></a>Çıkış penceresi özellikleri
**Çıktı** Penceresi'nin **Microsoft Azure Günlükleri** sekmesinde birkaç düğme ve bir metin kutusu vardır:

![Günlükler sekmesi düğmeleri](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-icons.png)

Bunlar aşağıdaki işlevleri gerçekleştirir:

* **Çıktı** penceresini temizleyin.
* Sözcük kaydırmayı etkinleştirin veya devre dışı.
* İzleme günlüklerini başlatın veya durdurun.
* Hangi günlüklerin izlendiğini belirtin.
* Günlükleri indirin.
* Filtre günlükleri bir arama dizesi veya normal bir ifade dayalı.
* **Çıkış** penceresini kapatın.

Bir arama dizesi veya normal ifade girerseniz, Visual Studio istemcide günlüğe kaydetme bilgilerini filtreler. Bu, günlükler **Çıktı** penceresinde görüntülendikten sonra ölçütleri girebileceğiniz ve günlükleri yeniden oluşturmanız gerekmeden filtreleme ölçütlerini değiştirebileceğiniz anlamına gelir.

## <a name="view-web-server-logs"></a><a name="webserverlogs"></a>Web sunucusu günlüklerini görüntüleme
Web sunucusu günlükleri uygulama için tüm HTTP etkinliğini kaydeder. **Çıktı** penceresinde bunları görmek için, bunları uygulama için etkinleştirmeniz ve Visual Studio'ya bunları izlemek istediğinizi söylemeniz gerekir.

1. **Server Explorer'dan**açtığınız **Azure Web Uygulaması Yapılandırma** sekmesinde, Web Sunucusu Günlüğe Kaydetme'yi değiştirin ve ardından **Kaydet'i**tıklatın. **On**

    ![Web sunucusu günlüğe kaydetmeyi etkinleştirme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-webserverloggingon.png)
2. **Çıktı** Penceresinde, izlemek **için hangi Microsoft Azure günlüğünü belirt'i** tıklatın.

    ![Hangi Azure günlüğünün izlendiğini belirtin](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-specifylogs.png)
3. Microsoft **Azure Günlüğe Kaydetme Seçenekleri** iletişim kutusunda, **Web sunucusu günlüklerini**seçin ve ardından **Tamam'ı**tıklatın.

    ![Web sunucusu günlüklerini izleme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorwslogson.png)
4. Uygulamayı gösteren tarayıcı penceresinde Ana **Sayfa'yı**tıklatın, ardından **Hakkında'yı**tıklatın ve ardından **İletişim'i**tıklatın.

    Uygulama günlükleri genellikle ilk olarak görünür, ardından web sunucusu günlükleri. Günlüklerin görünmesi için bir süre beklemeniz gerekebilir.

    ![Çıktı penceresinde web sunucusu günlükleri](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-wslogs.png)

Varsayılan olarak, Visual Studio'yu kullanarak web sunucusu günlüklerini ilk etkinleştirdiğinizde, Azure günlükleri dosya sistemine yazar. Alternatif olarak, web sunucusu günlüklerinin bir depolama hesabındaki bir blob kapsayıcısına yazılması gerektiğini belirtmek için Azure portalını kullanabilirsiniz.

Portalı, web sunucusunun bir Azure depolama hesabına günlüğe kaydolmasını ve Visual Studio'da oturum açmayı devre dışı bırakıp, Visual Studio'da yeniden oturum açmanızı etkinleştirdiğinizde depolama hesabı ayarlarınız geri yüklenir.

## <a name="view-detailed-error-message-logs"></a><a name="detailederrorlogs"></a>Ayrıntılı hata ileti günlüklerini görüntüleme
Ayrıntılı hata günlükleri, hata yanıt kodlarıyla (400 veya üzeri) sonuçlanan HTTP istekleri hakkında bazı ek bilgiler sağlar. **Çıktı** penceresinde bunları görmek için, uygulama için bunları etkinleştirmeniz ve Visual Studio'ya bunları izlemek istediğinizi söylemeniz gerekir.

1. **Server Explorer'dan**açtığınız **Azure Web Uygulaması Yapılandırma** sekmesinde, Ayrıntılı Hata **İletilerini** **Açık**olarak değiştirin ve ardından **Kaydet'i**tıklatın.

    ![Ayrıntılı hata iletilerini etkinleştirme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailedlogson.png)

2. **Çıktı** Penceresinde, izlemek **için hangi Microsoft Azure günlüğünü belirt'i** tıklatın.

3. Microsoft **Azure Günlüğe Kaydetme Seçenekleri** iletişim kutusunda, **Tüm günlükleri**tıklatın ve ardından **Tamam'ı**tıklatın.

    ![Tüm günlükleri izleyin](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-monitorall.png)

4. Tarayıcı penceresinin adres çubuğuna, 404 hatasına neden olmak için URL'ye `http://localhost:53370/Home/Contactx`fazladan bir karakter ekleyin (örneğin, ) ve Enter tuşuna basın.

    Birkaç saniye sonra, ayrıntılı hata günlüğü Visual Studio **Output** penceresinde görüntülenir.

    ![Ayrıntılı hata günlüğü - Çıkış penceresi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorlog.png)

    Denetim+tarayıcıda biçimlendirilmiş günlük çıkışını görmek için bağlantıyı tıklatın:

    ![Ayrıntılı hata günlüğü - tarayıcı penceresi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-detailederrorloginbrowser.png)

## <a name="download-file-system-logs"></a><a name="downloadlogs"></a>Dosya sistem günlüklerini karşıdan yükleme
**Çıktı** penceresinde izleyebileceğiniz tüm günlükler *.zip* dosyası olarak da indirilebilir.

1. **Çıktı** penceresinde, **Akış Günlüklerini İndir'i**tıklatın.

    ![Günlükler sekmesi düğmeleri](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadicon.png)

    Dosya Gezgini, indirilen dosya seçilerek *İndirilenler* klasörünüze açılır.

    ![İndirilen dosya](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-downloadedfile.png)
2. *.zip* dosyasını ayıklayın ve aşağıdaki klasör yapısını görürsünüz:

    ![İndirilen dosya](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilefolders.png)

   * Uygulama izleme günlükleri *LogFiles\Application* klasöründe *.txt* dosyalarında bulunmaktadır.
   * Web sunucusu günlükleri *LogFiles\http\RawLogs* klasöründe *.log* dosyalarında dır. Bu dosyaları görüntülemek ve işlemek için [Log Parser](https://www.microsoft.com/download/details.aspx?displaylang=en&id=24659) gibi bir araç kullanabilirsiniz.
   * Ayrıntılı hata ileti günlükleri *LogFiles\DetailedErrors* klasöründe *.html* dosyalarında dır.

     *(Dağıtımlar* klasörü kaynak denetimi yayımlama tarafından oluşturulan dosyalar içindir; Visual Studio yayımlama ile ilgili hiçbir şey yok. *Git* klasörü kaynak denetimi yayımlama ve günlük dosyası akış hizmeti ile ilgili izlemeler içindir.)  

<!-- ## <a name="storagelogs"></a>View storage logs
Application tracing logs can also be sent to an Azure storage account, and you can view them in Visual Studio. To do that you'll create a storage account, enable storage logs in the Azure portal, and view them in the **Logs** tab of the **Azure Web App** window.

You can send logs to any or all of three destinations:

* The file system.
* Storage account tables.
* Storage account blobs.

You can specify a different severity level for each destination.

Tables make it easy to view details of logs online, and they support streaming; you can query logs in tables and see new logs as they are being created. Blobs make it easy to download logs in files and to analyze them using HDInsight, because HDInsight knows how to work with blob storage. For more information, see **Hadoop and MapReduce** in [Data Storage Options (Building Real-World Cloud Apps with Azure)](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options).

You currently have file system logs set to verbose level; the following steps walk you through setting up information level logs to go to storage account tables. Information level means all logs created by calling `Trace.TraceInformation`, `Trace.TraceWarning`, and `Trace.TraceError` will be displayed, but not logs created by calling `Trace.WriteLine`.

Storage accounts offer more storage and longer-lasting retention for logs compared to the file system. Another advantage of sending application tracing logs to storage is that you get some additional information with each log that you don't get from file system logs.

1. Right-click **Storage** under the Azure node, and then click **Create Storage Account**.

![Create Storage Account](./media/web-sites-dotnet-troubleshoot-visual-studio/createstor.png)

1. In the **Create Storage Account** dialog, enter a name for the storage account.

    The name must be must be unique (no other Azure storage account can have the same name). If the name you enter is already in use you'll get a chance to change it.

    The URL to access your storage account will be *{name}*.core.windows.net.
2. Set the **Region or Affinity Group** drop-down list to the region closest to you.

    This setting specifies which Azure datacenter will host your storage account. For this tutorial your choice won't make a noticeable difference, but for a production web app you want your web server and your storage account to be in the same region to minimize latency and data egress charges. The web app (which you'll create later) should run in a region as close as possible to the browsers accessing your web app in order to minimize latency.
3. Set the **Replication** drop-down list to **Locally redundant**.
   
    When geo-replication is enabled for a storage account, the stored content is replicated to a secondary datacenter to enable failover to that location in case of a major disaster in the primary location. Geo-replication can incur additional costs. For test and development accounts, you generally don't want to pay for geo-replication. For more information, see [Create, manage, or delete a storage account](../storage/common/storage-create-storage-account.md).
4. Click **Create**.

    ![New storage account](./media/web-sites-dotnet-troubleshoot-visual-studio/newstorage.png)    
5. In the Visual Studio **Azure Web App** window, click the **Logs** tab, and then click **Configure Logging in Management Portal**.

     <!-- todo:screenshot of new portal if the VS page link goes to new portal -- >
    ![Configure logging](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-configlogging.png)

    This opens the **Configure** tab in the portal for your web app.
6. In the portal's **Configure** tab, scroll down to the application diagnostics section, and then change **Application Logging (Table Storage)** to **On**.
7. Change **Logging Level** to **Information**.
8. Click **Manage Table Storage**.

    ![Click Manage TableStorage](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-stgsettingsmgmtportal.png)

    In the **Manage table storage for application diagnostics** box, you can choose your storage account if you have more than one. You can create a new table or use an existing one.

    ![Manage table storage](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-choosestorageacct.png)
9. In the **Manage table storage for application diagnostics** box, click the check mark to close the box.
10. In the portal's **Configure** tab, click **Save**.
11. In the browser window that displays the application web app, click **Home**, then click **About**, and then click **Contact**.

     The logging information produced by browsing these web pages is written to the storage account.
12. In the **Logs** tab of the **Azure Web App** window in Visual Studio, click **Refresh** under **Diagnostic Summary**.

     ![Click Refresh](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-refreshstorage.png)

     The **Diagnostic Summary** section shows logs for the last 15 minutes by default. You can change the period to see more logs.

     (If you get a "table not found" error, verify that you browsed to the pages that do the tracing after you enabled **Application Logging (Storage)** and after you clicked **Save**.)

     ![Storage logs](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-storagelogs.png)

     Notice that in this view you see **Process ID** and **Thread ID** for each log, which you don't get in the file system logs. You can see additional fields by viewing the Azure storage table directly.
13. Click **View all application logs**.

     The trace log table appears in the Azure storage table viewer.

     (If you get a "sequence contains no elements" error, open **Server Explorer**, expand the node for your storage account under the **Azure** node, and then right-click **Tables** and click **Refresh**.)

     ![Storage logs in table view](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracelogtableview.png)

     This view shows additional fields you don't see in any other views. This view also enables you to filter logs by using special Query Builder UI for constructing a query. For more information, see Working with Table Resources - Filtering Entities in [Browsing Storage Resources with Server Explorer](https://msdn.microsoft.com/library/ff683677.aspx).
14. To look at the details for a single row, double-click one of the rows.

     ![Trace table in Server Explorer](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-tracetablerow.png)
 -->
## <a name="view-failed-request-tracing-logs"></a><a name="failedrequestlogs"></a>Başarısız istek izleme günlüklerini görüntüleme
Başarısız istek izleme günlükleri, URL yeniden yazma veya kimlik doğrulama sorunları gibi senaryolarda IIS'nin bir HTTP isteğini nasıl ele aldığına ilişkin ayrıntıları anlamanız gerektiğinde yararlıdır.

Uygulama Hizmeti uygulamaları, IIS 7.0 ve sonraki saatlerde kullanılabilen aynı başarısız istek izleme işlevini kullanır. Ancak, hangi hataların günlüğe kaydedildiğini yapılandıran IIS ayarlarına erişiminiz yoktur. Başarısız istek izleme etkinleştirdiğinizde, tüm hatalar yakalanır.

Visual Studio'yı kullanarak başarısız istek izlemeyi etkinleştirebilirsiniz, ancak bunları Visual Studio'da görüntüleyemezsiniz. Bu günlükler XML dosyalarıdır. Akış günlüğü hizmeti yalnızca düz metin modunda okunabilir kabul edilen dosyaları izler: *.txt*, *.html*, ve *.log* dosyaları.

Başarısız istek izleme günlüklerini doğrudan FTP üzerinden veya yerel olarak yerel bilgisayarınıza indirmek için bir FTP aracı nı kullandıktan sonra görüntüleyebilirsiniz. Bu bölümde, bunları doğrudan bir tarayıcıda görüntüleceksiniz.

1. **Sunucu Gezgini'nden**açtığınız **Azure Web Uygulaması** penceresinin **Yapılandırma** sekmesinde, Başarısız **İstek İzleme'yi** **Açık**olarak değiştirin ve ardından **Kaydet'i**tıklatın.

    ![Başarısız istek izlemesini etkinleştirme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequeston.png)
2. Uygulamayı gösteren tarayıcı penceresinin adres çubuğunda, URL'ye fazladan bir karakter ekleyin ve 404 hatasına neden olmak için Enter'u tıklatın.

    Bu, başarısız bir istek izleme günlüğü oluşturulmasına neden olur ve aşağıdaki adımlar günlük görüntülemek veya indirmek nasıl gösterir.

3. Visual Studio'da, **Azure Web Uygulaması** penceresinin **Yapılandırma** **sekmesinde Yönetim Portalı'nda Aç'ı**tıklatın.

4. Uygulamanızın [Azure portalı](https://portal.azure.com) **Ayarları** sayfasında **Dağıtım kimlik bilgilerini**tıklatın ve ardından yeni bir kullanıcı adı ve parola girin.

    ![Yeni FTP kullanıcı adı ve şifresi](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-enterftpcredentials.png)

    > [!NOTE]
    > Oturum açtığınızda, uygulama adı önceden belirlenmiş tam kullanıcı adını kullanmanız gerekir. Örneğin, kullanıcı adı olarak "myid" girerseniz ve site "myexample" ise, "myexample\myid" olarak oturum açarsınız.
    >

5. Yeni bir tarayıcı penceresinde, uygulamanızın **Genel Bakış** sayfasında **FTP ana bilgisayar adı** veya **FTPS ana bilgisayar adı** altında gösterilen URL'ye gidin.

6. Daha önce oluşturduğunuz FTP kimlik bilgilerini kullanarak oturum açın (kullanıcı adı için uygulama adı öneki dahil).

    Tarayıcı, uygulamanın kök klasörünü gösterir.

7. *LogFiles* klasörünü açın.

    ![LogFiles klasörünü aç](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-logfilesfolder.png)

8. W3SVC artı sayısal değer adlı klasörü açın.

    ![W3SVC klasörünü aç](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfolder.png)

    Klasör, başarısız istek izleme özelliğini etkinleştirdikten sonra günlüğe kaydedilmiş hatalar için XML dosyaları ve bir tarayıcının XML'yi biçimlendirmek için kullanabileceği bir XSL dosyası içerir.

    ![W3SVC klasörü](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-w3svcfoldercontents.png)

9. İzleme bilgilerini görmek istediğiniz başarısız istek için XML dosyasını tıklatın.

    Aşağıdaki resimde, örnek bir hata için izleme bilgilerinin bir bölümü gösterilmektedir.

    ![Tarayıcıda başarısız istek izleme](./media/web-sites-dotnet-troubleshoot-visual-studio/tws-failedrequestinbrowser.png)

## <a name="next-steps"></a><a name="nextsteps"></a>Sonraki Adımlar
Visual Studio'nun bir Uygulama Hizmeti uygulaması tarafından oluşturulan günlükleri görüntülemeyi nasıl kolaylaştırdığını gördünüz. Aşağıdaki bölümler, ilgili konularda daha fazla kaynağa bağlantılar sağlar:

* Uygulama Hizmeti sorun giderme
* Visual Studio'da Hata Ayıklama
* Azure'da uzaktan hata ayıklama
* ASP.NET uygulamalarında izleme
* Web sunucusu günlüklerini analiz etme
* Başarısız istek izleme günlüklerini çözümleme
* Bulut Hizmetlerini Hata Ayıklama

### <a name="app-service-troubleshooting"></a>Uygulama Hizmeti sorun giderme
Azure Uygulama Hizmeti'ndeki sorun giderme uygulamaları hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Uygulamaları izleme](web-sites-monitor.md)
* [Visual Studio 2013 ile Azure Uygulama Hizmetinde Bellek Sızıntılarını Araştırma](https://blogs.msdn.com/b/visualstudioalm/archive/2013/12/20/investigating-memory-leaks-in-azure-web-sites-with-visual-studio-2013.aspx). Yönetilen bellek sorunlarını çözümleme için Visual Studio özellikleri hakkında Microsoft ALM blog yazısı.
* [Azure Uygulama Hizmeti hakkında bilmeniz gereken çevrimiçi araçlar.](https://azure.microsoft.com/blog/2014/03/28/windows-azure-websites-online-tools-you-should-know-about-2/) Amit Apple tarafından Blog yazısı.

Belirli bir sorun giderme sorusuna yardımcı olmak için aşağıdaki forumlardan birinde bir iş parçacığı başlatın:

* [ASP.NET sitesindeki Azure forumu.](https://forums.asp.net/1247.aspx/1?Azure+and+ASP+NET)
* [Microsoft Q&A'daki Azure forumu.](https://docs.microsoft.com/answers/topics/azure-webapps.html)
* [StackOverflow.com](https://www.stackoverflow.com).

### <a name="debugging-in-visual-studio"></a>Visual Studio'da Hata Ayıklama
Visual Studio'da hata ayıklama modunun nasıl kullanılacağı hakkında daha fazla bilgi için Visual [Studio'da Hata Ayıklama](/visualstudio/debugger/debugging-in-visual-studio) ve [Visual Studio 2010 ile](https://weblogs.asp.net/scottgu/archive/2010/08/18/debugging-tips-with-visual-studio-2010.aspx)Hata Ayıklama İpuçları bölümüne bakın.

### <a name="remote-debugging-in-azure"></a>Azure'da uzaktan hata ayıklama
Uygulama Hizmeti uygulamaları ve Web İşleri için uzaktan hata ayıklama hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Uzaktan Hata Ayıklama Azure Uygulama Hizmetine Giriş](https://azure.microsoft.com/blog/2014/05/06/introduction-to-remote-debugging-on-azure-web-sites/).
* [Uzaktan Hata Ayıklama Azure Uygulama Hizmeti bölüm 2'ye Giriş 2 - Uzaktan Hata Ayıklama İçinde](https://azure.microsoft.com/blog/2014/05/07/introduction-to-remote-debugging-azure-web-sites-part-2-inside-remote-debugging/)
* [Azure Uygulama Hizmeti bölüm 3'te Uzaktan Hata Ayıklama'ya Giriş - Çok Örnekli ortam ve GIT](https://azure.microsoft.com/blog/2014/05/08/introduction-to-remote-debugging-on-azure-web-sites-part-3-multi-instance-environment-and-git/)
* [WebJobs Hata Ayıklama (video)](https://www.youtube.com/watch?v=ncQm9q5ZFZs&list=UU_SjTh-ZltPmTYzAybypB-g&index=1)

Uygulamanız bir Azure Web API veya Mobil Hizmetler arka uç kullanıyorsa ve bunu hata ayıklamanız gerekiyorsa, [Visual Studio'da Hata Ayıklama .NET Arka Uç](https://blogs.msdn.com/b/azuremobile/archive/2014/03/14/debugging-net-backend-in-visual-studio.aspx)bölümüne bakın.

### <a name="tracing-in-aspnet-applications"></a>ASP.NET uygulamalarında izleme
Internet'te ASP.NET izleme için kapsamlı ve güncel bir giriş yoktur. Yapabileceğiniz en iyi MVC henüz yoktu çünkü Web Formları için yazılmış eski tanıtım malzemeleri ile başlamak ve belirli konulara odaklanmak yeni blog gönderileri ile ek. Başlamak için bazı iyi yerler aşağıdaki kaynaklardır:

* [İzleme ve Telemetri (Azure ile Gerçek Dünya Bulut Uygulamaları Oluşturma)](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).<br>
  Azure bulut uygulamalarında izleme önerileri içeren e-kitap bölümü.
* [ASP.NET İzleme](/previous-versions/dotnet/articles/ms972204(v=msdn.10))<br/>
  Konuya temel bir giriş için eski ama yine de iyi bir kaynak.
* [İz Dinleyicileri](/dotnet/framework/debug-trace-profile/trace-listeners)<br/>
  İz dinleyiciler hakkında bilgi ama [WebPageTraceListener](/dotnet/api/system.web.webpagetracelistener)söz etmez.
* [İzleyici: System.Diagnostics İzleme ile ASP.NET İzlemeyi Tümleştirme](/previous-versions/b0ectfxd(v=vs.140))<br/>
  Bu makale de eskidir, ancak giriş maddesinin kapsamadığı bazı ek bilgiler içerir.
* [ASP.NET MVC Jilet Görünümlerde İzleme](https://blogs.msdn.com/b/webdev/archive/2013/07/16/tracing-in-asp-net-mvc-razor-views.aspx)<br/>
  Gönderi, Razor görünümlerinde izlemenin yanı sıra, bir MVC uygulamasında işlenmemiş tüm özel durumları günlüğe kaydetmek için hata filtresinin nasıl oluşturulacağını da açık Bir Web Forms uygulamasında işlenmemiş tüm özel durumları nasıl günlüğe kaydedilebildiğiniz hakkında bilgi için, MSDN'deki [Hata İşleyicileri Için Tam Örnek'teki](/previous-versions/bb397417(v=vs.140)) Global.asax örneğine bakın. MVC veya Web Formlar'da, belirli özel durumları günlüğe kaydetmek, ancak varsayılan çerçeve işlemenin bunlar için etkili olmasına izin vermek istiyorsanız, aşağıdaki örnekte olduğu gibi yakalayabilir ve yeniden atabilirsiniz:

    ```csharp
    try
    {
       // Your code that might cause an exception to be thrown.
    }
    catch (Exception ex)
    {
        Trace.TraceError("Exception: " + ex.ToString());
        throw;
    }
    ```

* [Azure Komut Satırından Akış Tanılama İzleme Günlüğü (artı Glimpse!)](https://www.hanselman.com/blog/StreamingDiagnosticsTraceLoggingFromTheAzureCommandLinePlusGlimpse.aspx)<br/>
  Bu öğretici Visual Studio nasıl yapılacağını gösterir ne yapmak için komut satırı nasıl kullanılır. [Glimpse,](https://www.hanselman.com/blog/IfYoureNotUsingGlimpseWithASPNETForDebuggingAndProfilingYoureMissingOut.aspx) ASP.NET uygulamaları hata ayıklama için bir araçtır.
* David Ebbo ve [Web Apps'tan Akış Günlükleri ile](https://azure.microsoft.com/documentation/videos/log-streaming-with-azure-web-sites/) [Web Apps Günlüklerini ve Tanılamayı](https://azure.microsoft.com/documentation/videos/azure-web-site-logging-and-diagnostics/) Kullanma<br>
  Scott Hanselman ve David Ebbo tarafından Videolar.

Hata günlüğe kaydetme için, kendi izleme kodunuzu yazmaya alternatif olarak [ELMAH](https://nuget.org/packages/elmah/)gibi açık kaynak kodlu bir günlük çerçevesi kullanmak gerekir. Daha fazla bilgi için [Scott Hanselman'ın ELMAH hakkındaki blog gönderilerine](https://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)bakın.

Ayrıca, Azure'dan akış günlükleri `System.Diagnostics` almak için ASP.NET kullanmanız veya izlemeniz gerekmez. Uygulama Hizmeti uygulaması akış günlüğü hizmeti, *LogFiles* klasöründe bulduğu herhangi bir *.txt*, *.html*veya *.log* dosyasını akışı sağlar. Bu nedenle, uygulamanın dosya sistemine yazan kendi günlük sisteminizi oluşturabilirsiniz ve dosyanız otomatik olarak akışlanır ve indirilir. Tek yapmanız gereken *d:\home\logfiles* klasöründe dosya oluşturan uygulama kodu yazmaktır.

### <a name="analyzing-web-server-logs"></a>Web sunucusu günlüklerini analiz etme
Web sunucusu günlüklerini çözümleme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Logparser](https://www.microsoft.com/download/details.aspx?id=24659)<br/>
  Web sunucusu günlüklerinde *(.log* dosyaları) verileri görüntülemek için bir araçtır.
* [LogParser kullanarak Sorun Giderme IIS Performans Sorunları veya Uygulama Hataları](https://www.iis.net/learn/troubleshoot/performance-issues/troubleshooting-iis-performance-issues-or-application-errors-using-logparser)<br/>
  Web sunucusu günlüklerini analiz etmek için kullanabileceğiniz Log Parser aracına giriş.
* [LogParser kullanarak Robert McMurray tarafından Blog yazıları](https://blogs.msdn.com/b/robert_mcmurray/archive/tags/logparser/)<br/>
* [IIS 7.0, IIS 7.5 ve IIS 8.0'daki HTTP durum kodu](https://support.microsoft.com/kb/943891)

### <a name="analyzing-failed-request-tracing-logs"></a>Başarısız istek izleme günlüklerini çözümleme
Microsoft TechNet web sitesi, bu günlüklerin nasıl kullanılacağını anlamak için yararlı olabilecek başarısız [istek izleme](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing) bölümünü kullanma içerir. Ancak bu dokümantasyon ağırlıklı olarak Azure Uygulama Hizmeti'nde yapamayacağınız IIS'de başarısız istek izlemesini yapılandırmaya odaklanır.

[GetStarted]: app-service-web-get-started-dotnet.md
[GetStartedWJ]: https://github.com/Azure/azure-webjobs-sdk/wiki

---
title: "Hızlı başlangıç: Visual Studio 'Yu kullanarak Azure 'da ilk işlevinizi oluşturma"
description: Bu hızlı başlangıçta, Visual Studio kullanarak bir HTTP tetikleyici Azure Işlevi oluşturmayı ve yayımlamayı öğreneceksiniz.
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.topic: quickstart
ms.date: 09/30/2020
ms.custom: devx-track-csharp, mvc, devcenter, vs-azure, 23113853-34f2-4f
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 9e3144738bd259ab9be75059af00f125581bb37c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102050129"
---
# <a name="quickstart-create-your-first-function-in-azure-using-visual-studio"></a>Hızlı başlangıç: Visual Studio 'Yu kullanarak Azure 'da ilk işlevinizi oluşturma

Bu makalede, HTTP isteklerine yanıt veren C# sınıf kitaplığı tabanlı bir işlev oluşturmak için Visual Studio 'Yu kullanırsınız. Kodu yerel olarak test ettikten sonra, <abbr title="Sunucunun tüm ayrıntılarının uygulama geliştiricileri için saydam olduğu ve kodu dağıtma ve yönetme sürecini kolaylaştıran bir çalışma zamanı bilgi işlem ortamı.">sunucusuz</abbr> ortam <abbr title="Uygulamalar için düşük maliyetli bir sunucusuz bilgi işlem ortamı sağlayan bir Azure hizmeti.">Azure İşlevleri</abbr>.

Bu hızlı başlangıcı tamamlamak, birkaç ABD Doları veya daha kısa bir ücret doğurur <abbr title="Azure kullanımı için fatura bilgilerini tutan profil.">Azure hesabı</abbr>.

## <a name="1-prepare-your-environment"></a>1. ortamınızı hazırlayın

+ Azure oluşturma <abbr title="Azure kullanımı için fatura bilgilerini tutan profil.">account</abbr> etkin bir <abbr title="Azure 'da, genellikle bir kuruluş içindeki bir birey veya departmanla ilişkili kaynakları yönettiğiniz temel kuruluş yapısıdır.">aboneliği</abbr>. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ [Visual Studio 2019](https://azure.microsoft.com/downloads/) ' yi yükleyip yükleme sırasında **Azure geliştirme** iş yükünü seçin. 

![Azure geliştirme iş yüküyle Visual Studio 'Yu yükler](media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)

<br/>
<details>
<summary><strong>Bunun yerine bir Azure Işlevleri projesi kullanın</strong></summary>
Şunu oluşturmak istiyorsanız <abbr title="Birlikte dağıtılabilecek ve yönetilebilen bir veya daha fazla bağımsız işlev için mantıksal bir kapsayıcı.">Azure Işlevleri projesi</abbr> Bunun yerine Visual Studio 2017 kullanarak, önce [en son Azure işlevleri araçlarını](functions-develop-vs.md#check-your-tools-version)yüklemeniz gerekir.
</details>

## <a name="2-create-a-function-app-project"></a>2. bir işlev uygulaması projesi oluşturma

1. Visual Studio menüsünden **Dosya**  >  **Yeni**  >  **Proje**' yi seçin.

1. **Yeni proje oluştur**' da, arama *kutusuna Işlevler* girin, **Azure Işlevleri** şablonunu seçin ve ardından **İleri**' yi seçin.

1. **Yeni projenizi yapılandırın** bölümünde, **<abbr title=" uygulama adının bir C# ad alanı olarak geçerli olması gerekir, bu nedenle alt çizgi, kısa çizgi veya alfasayısal olmayan karakterler kullanmayın. "> Projeniz için </abbr> Proje adı** ve sonra **Oluştur**' u seçin. 

1. **Yeni Azure işlevleri uygulama** ayarları için aşağıdaki bilgileri sağlayın:

    + **<abbr title=" Bu değeri seçin, .NET Core 3. x 'ı destekleyen Azure işlevlerinin sürüm 3. x çalışma zamanını kullanan bir işlev projesi oluşturur. Azure işlevleri 1. x .NET Framework destekler. "> Işlevler çalışma zamanı açılan menüsünden Azure Işlevleri </abbr> v3 (.NET Core)** . (Daha fazla bilgi için bkz. [Azure işlevleri çalışma zamanı sürümlerine genel bakış](functions-versions.md).)
    
    + **<abbr title=" Bu değeri SEÇIN bir http isteği tarafından tetiklenen bir işlev oluşturur. "> İşlev şablonu </abbr> olarak http tetikleyicisi** .
    
    + Bir **<abbr title=" Azure işlevi için bir depolama hesabı gerektiğinden, projenizi Azure 'da yayımladığınızda bir tane atanması veya oluşturulması gerekir. HTTP tetikleyicisi, Azure depolama hesabı bağlantı dizesi kullanmaz; diğer tüm tetikleyici türleri için geçerli bir Azure depolama hesabı bağlantı dizesi gerekir. "> </abbr>** Depolama hesabı açılır listesinden depolama öykünücüsü.
        
    + Buradan **anonim** seçin <abbr title="Oluşturulan işlev, anahtar gerektirmeden herhangi bir istemci tarafından tetiklenebilir. Bu yetkilendirme ayarı yeni işlevinizi test etmenizi kolaylaştırır.">Yetkilendirme düzeyi</abbr> Listenin. (Anahtarlar ve yetkilendirme hakkında daha fazla bilgi için bkz. [Yetkilendirme anahtarları](functions-bindings-http-webhook-trigger.md#authorization-keys) ve [http ve Web kancası bağlamaları](functions-bindings-http-webhook.md).)

    + **Oluştur**’u seçin
        
## <a name="3-rename-the-function"></a>3. işlevi yeniden adlandırın

`FunctionName`Yöntem özniteliği, varsayılan olarak olarak oluşturulan işlevin adını ayarlar `Function1` . Araç, projenizi oluştururken varsayılan işlev adını geçersiz kılmanıza izin vermediğinden, işlev sınıfı, dosya ve meta veriler için daha iyi bir ad oluşturmak üzere bir dakikanızı alın.

1. **Dosya Gezgini**'nde işlev1. cs dosyasına sağ tıklayın ve *httpexample. cs* olarak yeniden adlandırın.

1. Kodda, Işlev1 sınıfını ' HttpExample ' olarak yeniden adlandırın.

1. `HttpTrigger`Adlı yöntemde `Run` , `FunctionName` yöntemi özniteliğini olarak yeniden adlandırın `HttpExample` .


## <a name="4-run-the-function-locally"></a>4. işlevi yerel olarak çalıştırın

1. İşlevinizi çalıştırmak için, Visual Studio 'da <kbd>F5</kbd> tuşuna basın.

1. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![Azure yerel çalışma zamanı](../../includes/media/functions-run-function-test-local-vs/functions-debug-local-vs.png)

1. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. Sorgu dizesini ekleyin **? Name =<YOUR_NAME** bu URL 'ye>ve isteği çalıştırın. 

    ![Tarayıcıdaki işlev localhost yanıtı](../../includes/media/functions-run-function-test-local-vs/functions-run-browser-local-vs.png)

1. Hata ayıklamayı durdurmak için, <kbd></kbd> + Visual Studio 'da SHIFT<kbd>F5</kbd> tuşuna basın.

<br/>
<details>
<summary><strong>Sorun giderme</strong></summary>
 Araçların HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz gerekebilir. Yerel olarak bir işlev çalıştırdığınızda yetkilendirme düzeyleri hiçbir zaman zorlanmaz.
</details>

## <a name="5-publish-the-project-to-azure"></a>5. projeyi Azure 'da yayımlayın

1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

1. **Hedef** bölümünde **Azure** ' u seçin.

    :::image type="content" source="../../includes/media/functions-vstools-publish/functions-visual-studio-publish-profile-step-1.png" alt-text="Azure hedefini seçin":::

1. **Belirli bir hedefte** **Azure işlev uygulaması (Windows)** öğesini seçin

    :::image type="content" source="../../includes/media/functions-vstools-publish/functions-visual-studio-publish-profile-step-2.png" alt-text="Azure İşlev Uygulaması seçin":::

1. **Işlev örneği**' nde **Yeni bir Azure işlevi oluştur...** öğesini seçin ve ardından aşağıda belirtilen değerleri kullanın:

    + **Ad** için şunu girin <abbr title="Yeni işlev uygulamanızı benzersiz bir şekilde tanımlayan bir ad kullanın. Bu adı kabul edin veya yeni bir ad girin. Geçerli karakterler: `a-z` , `0-9` , ve `-` ..">Genel olarak benzersiz bir ad</abbr>
    
    + Açılan listeden bir abonelik **seçin** .
    
    + Mevcut olanı **seçin** <abbr title="Birim olarak yönetebileceğiniz ilgili Azure kaynakları için mantıksal bir kapsayıcı.">kaynak grubu</abbr> veya yeni bir kaynak grubu oluşturmak için aşağı açılan listeden **Yeni** ' yi seçin.
    
    + **Seç** <abbr title="Projenizi bir tüketim planında çalışan bir işlev uygulamasında yayımladığınızda yalnızca işlevler uygulamanızın yürütmeleri için ödeme yaparsınız. Diğer barındırma planları daha yüksek maliyetler doğurur.">Tüketim</abbr> Oynatma türü açılır. (Daha fazla bilgi için bkz. [Tüketim planı](consumption-plan.md).)
    
    + Bir **seçin**  <abbr title="Kaynakların ayrıldığı belirli bir Azure veri merkezine coğrafi başvuru. Kullanılabilir bölgelerin listesi için bkz. [bölgeler](https://azure.microsoft.com/regions/) .">location</abbr> açılır.
    
    + **Bir <** abbr = "işlevler çalışma zamanı Için bir Azure depolama hesabı gereklidir. Genel amaçlı bir depolama hesabı yapılandırmak için yeni ' yi seçin. Depolama hesabı gereksinimlerini karşılayan mevcut bir hesabı da seçebilirsiniz. " Azure depolama ></abbr> açılır listeden hesap

    ![App Service Oluştur iletişim kutusu](../../includes/media/functions-vstools-publish/functions-visual-studio-publish.png)

1. **Oluştur**’u seçin 

1. **İşlevler örneğinde**, **paket dosyasından Çalıştır** ' ın işaretli olduğundan emin olun. 

    :::image type="content" source="../../includes/media/functions-vstools-publish/functions-visual-studio-publish-profile-step-4.png" alt-text="Profil oluşturmayı tamamlama":::

    <br/>
    <details>
    <summary><strong>Bu ayar ne yapar?</strong></summary>
    Paket dosyasından **Çalıştır** kullanılırken, işlev uygulamanız, [paket Çalıştır](run-functions-from-deployment-package.md) modu etkin olan [ZIP dağıtımı](functions-deployment-technologies.md#zip-deploy) kullanılarak dağıtılır. Bu, daha iyi performans ile sonuçlandığından, işlevler projeniz için önerilen dağıtım yöntemidir.    
    </details>   

1. **Son**'u seçin.

1. Yayımla sayfasında **Yayımla**’yı seçin.

1. Yayımla sayfasında, işlev uygulamasının kök URL 'sini gözden geçirin.

1. Yayımla sekmesinde, **Cloud Explorer 'Da Yönet ' i seçin <abbr title=" . site içeriğini görüntülemek Için Visual Studio 'yu kullanmanıza, işlev uygulamasını başlatıp durdurmanıza ve Azure 'daki ve Azure Portal doğrudan işlev uygulama kaynaklarına gözatmanıza imkan tanır. "> Bulut Gezgini </abbr>**.
    
    :::image type="content" source="../../includes/media/functions-vstools-publish/functions-visual-studio-publish-complete.png" alt-text="Başarı iletisi yayımlama":::
    

## <a name="6-test-your-function-in-azure"></a>6. işlevinizi Azure 'da test etme

1. Cloud Explorer 'da yeni işlev uygulamanız seçilmelidir. Aksi takdirde, aboneliğinizi genişletin, **uygulama hizmetleri**' ni genişletin ve yeni işlev uygulamanızı seçin.

1. İşlev uygulamasına sağ tıklayın ve **tarayıcıda aç**' ı seçin. Bu, işlev uygulamanızın kökünü varsayılan Web tarayıcınızda açar ve işlev uygulamanızın çalıştığını gösteren sayfayı görüntüler. 

    :::image type="content" source="media/functions-create-your-first-function-visual-studio/function-app-running-azure.png" alt-text="İşlev uygulaması çalışıyor":::

1. Tarayıcıdaki Adres çubuğunda, **/api/httpexample? Name = işlevleri** DIZESINI temel URL 'ye ekleyin ve isteği çalıştırın.

    HTTP tetikleyici işlevinizi çağıran URL aşağıdaki biçimdedir:

    `http://<APP_NAME>.azurewebsites.net/api/HttpExample?name=Functions`

2. Bu URL 'ye gidin ve aşağıdaki örnekte olduğu gibi işlev tarafından döndürülen uzaktan GET isteğine tarayıcıda bir yanıt görürsünüz:

    :::image type="content" source="media/functions-create-your-first-function-visual-studio/functions-create-your-first-function-visual-studio-browser-azure.png" alt-text="Tarayıcıda işlev yanıtı":::

## <a name="7-clean-up-resources"></a>7. Kaynakları Temizleme

Daha fazla maliyet ödemeden kaçınmak için işlev uygulamasını ve kaynaklarını silin.

1. Cloud Explorer 'da aboneliğinizi genişletin, **uygulama hizmetleri**' ni genişletin, işlev uygulamanıza sağ tıklayın ve **portalda aç**' ı seçin. 

1. İşlev uygulaması sayfasında **genel bakış** sekmesini seçin ve **kaynak grubu**' nun altındaki bağlantıyı seçin.

   :::image type="content" source="media/functions-create-your-first-function-visual-studio/functions-app-delete-resource-group.png" alt-text="İşlev uygulaması sayfasından silinecek kaynak grubunu seçin":::

1. **Kaynak grubu** sayfasında, dahil edilen kaynakların listesini gözden geçirin ve silmek istedikleriniz olduklarından emin olun.
 
1. **Kaynak grubunu sil**’i seçin ve yönergeleri izleyin.

    Silme işlemi birkaç dakika sürebilir. İşlem tamamlandığında, birkaç saniye boyunca bir bildirim görüntülenir. Bildirimi görüntülemek için sayfanın üst kısmındaki zil simgesini de seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl ekleneceğini öğrenmek için bir sonraki makaleye ilerleyin <abbr title="Bir işlevi bir depolama kuyruğu ile ilişkilendirmekte, böylece kuyrukta ileti oluşturabilmesi anlamına gelir. Bağlamalar, bir işlev ve diğer kaynaklar arasında bildirime dayalı bağlantılardır. Giriş bağlama işlevine veri sağlar; çıkış bağlama, işlevden diğer kaynaklara veri sağlar.">Azure depolama kuyruğu çıktı bağlama</abbr> işleviniz:

> [!div class="nextstepaction"]
> [İşleviniz için bir Azure depolama kuyruğu bağlama ekleme](functions-add-output-binding-storage-queue-vs.md)

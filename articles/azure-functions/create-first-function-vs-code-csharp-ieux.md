---
title: Visual Studio Code-Azure Işlevleri kullanarak C# işlevi oluşturma
description: C# işlevi oluşturmayı öğrenin ve ardından Visual Studio Code içindeki Azure Işlevleri uzantısını kullanarak yerel projeyi Azure Işlevleri 'nde sunucusuz barındırma için yayımlayın.
ms.topic: quickstart
ms.date: 11/03/2020
ms.custom: devx-track-csharp
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 2848ce6214d59ba2732dcfc148ccaf9936497f17
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102121040"
---
# <a name="quickstart-create-a-c-function-in-azure-using-visual-studio-code"></a>Hızlı başlangıç: Visual Studio Code kullanarak Azure 'da C# işlevi oluşturma

> [!div class="op_single_selector" title1="İşlev dilinizi seçin: "]
> - [C#](create-first-function-vs-code-csharp.md)
> - [Java](create-first-function-vs-code-java.md)
> - [JavaScript](create-first-function-vs-code-node.md)
> - [PowerShell](create-first-function-vs-code-powershell.md)
> - [Python](create-first-function-vs-code-python.md)
> - [TypeScript](create-first-function-vs-code-typescript.md)
> - [Diğer (Go/Rust)](create-first-function-vs-code-other.md)

Bu makalede, HTTP isteklerine yanıt veren bir C# sınıf kitaplığı tabanlı işlev oluşturmak için Visual Studio Code kullanırsınız. Kodu yerel olarak test ettikten sonra, <abbr title="Sunucunun tüm ayrıntılarının uygulama geliştiricileri için saydam olduğu ve kodu dağıtma ve yönetme sürecini basitleştirecek bir çalışma zamanı bilgi işlem ortamı.">sunucusuz</abbr> ortam <abbr title="Uygulamalar için düşük maliyetli bir sunucusuz bilgi işlem ortamı sağlayan Azure hizmeti.">Azure İşlevleri</abbr>.

Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.

Bu makalenin [CLI tabanlı bir sürümü](create-first-function-cli-csharp.md) de vardır.
    
## <a name="1-configure-your-environment"></a>1. ortamınızı yapılandırın

Başlamadan önce, aşağıdaki gereksinimlerin yerinde olduğundan emin olun:

+ Azure <abbr title="Azure kullanımı için fatura bilgilerini tutan profil.">account</abbr> etkin bir <abbr title="Azure 'da, genellikle bir kuruluş içindeki bir birey veya departmanla ilişkili kaynakları yönettiğiniz temel kuruluş yapısıdır.">aboneliği</abbr>. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools) sürüm 3. x.

+ [Desteklenen platformlardan](https://code.visualstudio.com/docs/supporting/requirements#_platforms) birinde çalışan [Visual Studio Code](https://code.visualstudio.com/).

+ Visual Studio Code için [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).  

+ Visual Studio Code için [Azure İşlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions).

## <a name="2-create-your-local-project"></a><a name="create-an-azure-functions-project"></a>2. yerel projenizi oluşturun

Bu bölümde, yerel bir kayıt oluşturmak için Visual Studio Code kullanacaksınız <abbr title="Birlikte dağıtılabilecek ve yönetilebilen bir veya daha fazla bağımsız işlev için mantıksal bir kapsayıcı.">Azure Işlevleri projesi</abbr> C# dilinde. Bu makalenin ilerleyen bölümlerinde işlev kodunu Azure'da yayımlayacaksınız.

1. İçindeki Azure simgesini seçin <abbr title="Visual Studio Code penceresinin sol tarafındaki dikey simgeler grubu.">Etkinlik çubuğu</abbr>, ardından **Azure: Functions** alanında **Yeni proje oluştur...** simgesini seçin.

    ![Yeni proje oluştur'u seçin](./media/functions-create-first-function-vs-code/create-new-project.png)

1. Projenizin çalışma alanı için bir dizin konumu seçtikten sonra **Seç**'i belirleyin.

    > [!NOTE]
    > Bu adımlar, bir çalışma alanının dışında tamamlanacak şekilde tasarlandı. Bu örnekte, bir çalışma alanının parçası olan bir proje klasörünü seçmeyin.

1. İstemlerde aşağıdaki bilgileri sağlayın:

    + **İşlev projeniz için bir dil seçin**: `C#` öğesini seçin.

    + **Projenizin ilk işlevi için bir şablon seçin**: `HTTP trigger` öğesini seçin.

    + **Bir işlev adı sağlayın**: `HttpExample` yazın.

    + **Bir ad alanı sağlayın**: `My.Functions` yazın.

    + **Yetkilendirme düzeyi**: `Anonymous` herhangi bir kişinin işlev uç noktanızı çağırmasını sağlayan öğesini seçin. Yetkilendirme düzeyleri hakkında bilgi edinmek için bkz. [Yetkilendirme anahtarları](functions-bindings-http-webhook-trigger.md#authorization-keys).

    + **Projenizi nasıl açmak istediğinizi seçin**: `Add to workspace` öğesini seçin.

1. Bu bilgileri kullanarak, Visual Studio Code HTTP ile bir Azure Işlevleri projesi oluşturur <abbr title="Bir HTTP isteği, kuyruk iletisi veya belirli bir zaman gibi işlevin kodunu çağıran olay türü.">Tetikleyicinin</abbr>. Yerel proje dosyalarını Gezgin içinde görüntüleyebilirsiniz. Oluşturulan dosyalar hakkında daha fazla bilgi edinmek için bkz. [oluşturulan proje dosyaları](functions-develop-vs-code.md#generated-project-files).

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi doğrudan Azure 'da yayımlamak için Visual Studio Code kullanma zamanı vardır.

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

## <a name="5-publish-the-project-to-azure"></a>5. projeyi Azure 'da yayımlayın

Bu bölümde, Azure aboneliğinizde bir işlev uygulaması ve ilgili kaynaklar oluşturup kodunuzu dağıtırsınız. 

> [!IMPORTANT]
> Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır. 

1. Etkinlik çubuğunda Azure simgesini seçin, sonra **Azure: işlevler** alanında, **işlev uygulamasına dağıt uygulama...** düğmesini seçin.

    

1. İstemlerde aşağıdaki bilgileri sağlayın:

    + **Klasör seçin**: çalışma alanınızdan bir klasör seçin veya işlev uygulamanızı içeren bir klasöre gidin. Zaten açılmış geçerli bir işlev uygulamanız varsa bunu göremezsiniz.

    + **Abonelik seçin**: kullanılacak aboneliği seçin. Yalnızca bir aboneliğiniz varsa bunu göremezsiniz.

    + **Azure'daki işlev uygulamasını seçin**: `+ Create new Function App` öğesini seçin. ( `Advanced` Bu makalede kapsanmayan seçeneğini seçmeyin.)

    + **İşlev uygulaması için genel olarak benzersiz bir ad girin**: URL yolunda geçerli olan bir ad yazın. Yazdığınız ad, olduğundan emin olmak için onaylanır <abbr title="Ad tüm Azure müşterileri tarafından genel olarak kullanılan tüm Işlevler projelerinde benzersiz olmalıdır. Genellikle, contoso-bizapp-Func-20 ' de olduğu gibi kişisel veya şirket adı, uygulama adı ve sayısal bir tanımlayıcı birleşimini kullanırsınız">Azure Işlevlerinde benzersiz</abbr>. 

    + **Yeni kaynaklar için bir konum seçin**: daha iyi performans için, size yakın bir [bölge](https://azure.microsoft.com/regions/) seçin.

    Uzantı, bildirim alanında Azure 'da oluşturulmakta olan ayrı kaynakların durumunu gösterir.

    :::image type="content" source="../../includes/media/functions-publish-project-vscode/resource-notification.png" alt-text="Azure kaynak oluşturma bildirimi":::

1. Tamamlandığında, aşağıdaki Azure kaynakları, işlev uygulamanızın adına göre adlar kullanılarak aboneliğinizde oluşturulur:

    + İlgili kaynaklar için mantıksal kapsayıcı olan bir **kaynak grubu**.
    + Projeleriniz hakkında durum ve diğer bilgileri tutan standart bir **Azure depolama hesabı**.
    + Sunucusuz işlev uygulamanız için temel Konağı tanımlayan bir **Tüketim planı**. 
    + İşlev kodunuzu yürütmek için ortam sağlayan bir **işlev uygulaması**. İşlev uygulaması, aynı barındırma planı içindeki kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.
    + Sunucusuz işlevinizin kullanımını izleyen işlev uygulamasına bağlı bir **Application Insights örneği** .

    İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. 

    > [!TIP]
    > Varsayılan olarak, işlev uygulamanız tarafından gereken Azure kaynakları, sağladığınız işlev uygulaması adına göre oluşturulur. Varsayılan olarak, aynı zamanda işlev uygulamasıyla aynı yeni kaynak grubunda de oluşturulur. Bu kaynakların adlarını özelleştirmek veya mevcut kaynakları yeniden kullanmak istiyorsanız, [projeyi gelişmiş oluşturma seçenekleriyle yayımlamanız](functions-develop-vs-code.md#enable-publishing-with-advanced-create-options)gerekir.


1. Oluşturduğunuz Azure kaynakları dahil olmak üzere oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimde **çıktıyı görüntüle** ' yi seçin. Bildirimi kaçırırsanız, sağ alt köşedeki zil simgesini seçerek tekrar görüntüleyin.

    ![Tüm bildirimi oluştur](./media/functions-create-first-function-vs-code/function-create-notifications.png)

## <a name="6-run-the-function-in-azure"></a>6. işlevi Azure 'da çalıştırın

1. Yan çubuktaki **Azure: Functions** alanına geri döndüğünüzde aboneliğinizi, yeni işlev uygulamanızı ve **işlevlerinizi** genişletin. İşlevi sağ tıklatın (Windows) veya <kbd>CTRL</kbd> + tıklatın (MacOS) `HttpExample` ve **Şimdi işlevi Çalıştır...** seçeneğini belirleyin.

    :::image type="content" source="../../includes/media/functions-vs-code-run-remote/execute-function-now.png" alt-text="İşlevi Azure 'da şimdi çalıştırın Visual Studio Code":::

1. **İstek gövdesi gir** bölümünde istek iletisi gövdesi değerini görürsünüz `{ "name": "Azure" }` .

    Bu istek iletisini işlevinizde göndermek için ENTER tuşuna basın.  

1. İşlev Azure 'da yürütüldüğünde ve bir yanıt döndürdüğünde, Visual Studio Code bir bildirim oluşturulur.

## <a name="5-clean-up-resources"></a>5. Kaynakları Temizleme

Bir [sonraki adıma](#next-steps) devam edip bir <abbr title="Bir işlevi bir depolama kuyruğu ile ilişkilendirmekte, böylece kuyrukta ileti oluşturabilmesi anlamına gelir.">Azure depolama kuyruğu çıktı bağlama</abbr> işlevinizde, daha önce yapmış olduğunuz şeyi oluşturmak için tüm kaynaklarınızı saklamanız gerekir.

Aksi takdirde, daha fazla maliyet ödemeden kaçınmak için işlev uygulamasını ve ilgili kaynaklarını silmek için aşağıdaki adımları kullanabilirsiniz.

[!INCLUDE [functions-cleanup-resources-vs-code-inner.md](../../includes/functions-cleanup-resources-vs-code-inner.md)]

Işlev maliyetleri hakkında daha fazla bilgi edinmek için bkz. [Tüketim planı maliyetlerini tahmin](functions-consumption-costs.md)etme.

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen basit bir işlevi kullanarak bir işlev uygulaması oluşturmak için Visual Studio Code’u kullandınız. Sonraki makalede, bir çıkış ekleyerek bu işlevi genişletmelisiniz <abbr title="Bir işlev ve diğer kaynaklar arasında bildirime dayalı bağlantı. Giriş bağlama işlevine veri sağlar; çıkış bağlama, işlevden diğer kaynaklara veri sağlar.">bağlama</abbr>. Bu bağlama, HTTP isteğinden dizeyi bir Azure kuyruk depolama kuyruğundaki bir iletiye yazar. 

> [!div class="nextstepaction"]
> [Azure depolama kuyruğuna bağlanma](functions-add-output-binding-storage-queue-vs-code.md?pivots=programming-language-csharp)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions

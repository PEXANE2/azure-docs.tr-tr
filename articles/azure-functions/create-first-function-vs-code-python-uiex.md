---
title: Visual Studio Code-Azure Işlevleri kullanarak bir Python işlevi oluşturma
description: Visual Studio Code ' deki Azure Işlevleri uzantısını kullanarak, bir Python işlevi oluşturmayı ve ardından yerel projeyi Azure Işlevleri 'nde sunucusuz barındırma için yayımlamayı öğrenin.
ms.topic: quickstart
ms.date: 11/04/2020
ms.custom: devx-track-python
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 19a8a73428bef3c29c5bf9e56b45750adf5fb2dd
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101748946"
---
# <a name="quickstart-create-a-function-in-azure-with-python-using-visual-studio-code"></a>Hızlı başlangıç: Visual Studio Code kullanarak Azure 'da Python ile bir işlev oluşturma

> [!div class="op_single_selector" title1="İşlev dilinizi seçin: "]
> - [Python](create-first-function-vs-code-python.md)
> - [C#](create-first-function-vs-code-csharp.md)
> - [Java](create-first-function-vs-code-java.md)
> - [JavaScript](create-first-function-vs-code-node.md)
> - [PowerShell](create-first-function-vs-code-powershell.md)
> - [TypeScript](create-first-function-vs-code-typescript.md)
> - [Diğer (Go/Rust)](create-first-function-vs-code-other.md)

Bu makalede, HTTP isteklerine yanıt veren bir Python işlevi oluşturmak için Visual Studio Code kullanırsınız. Kodu yerel olarak test ettikten sonra, <abbr title="Sunucunun tüm ayrıntılarının uygulama geliştiricileri için saydam olduğu ve kodu dağıtma ve yönetme sürecini kolaylaştıran bir çalışma zamanı bilgi işlem ortamı.">sunucusuz</abbr> ortam <abbr title="Uygulamalar için düşük maliyetli bir sunucusuz bilgi işlem ortamı sağlayan bir Azure hizmeti.">Azure İşlevleri</abbr>.

Bu hızlı başlangıcı tamamlamak, Azure hesabınızda birkaç ABD Doları veya daha kısa bir ücret doğurur.

Bu makalenin [CLI tabanlı bir sürümü](create-first-function-cli-python.md) de vardır.

## <a name="1-prepare-your-environment"></a>1. ortamınızı hazırlayın

Başlamadan önce, aşağıdaki gereksinimlerin yerinde olduğundan emin olun:

+ Azure <abbr title="Azure kullanımı için fatura bilgilerini tutan profil.">account</abbr> etkin bir <abbr title="Azure 'da, genellikle bir kuruluş içindeki bir birey veya departmanla ilişkili kaynakları yönettiğiniz temel kuruluş yapısıdır.">aboneliği</abbr>. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ [Azure Functions Core Tools](functions-run-local.md#install-the-azure-functions-core-tools) sürüm 3. x.

+ Python [3,8](https://www.python.org/downloads/release/python-381/), [Python 3,7](https://www.python.org/downloads/release/python-375/), Python [3,6](https://www.python.org/downloads/release/python-368/) , Azure işlevleri (x64) tarafından desteklenir.

+ [Desteklenen platformlardan](https://code.visualstudio.com/docs/supporting/requirements#_platforms) birinde çalışan [Visual Studio Code](https://code.visualstudio.com/).

+ Visual Studio Code için [Python uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-python.python) .  

+ Visual Studio Code için [Azure İşlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions).

<hr/>
<br/>

## <a name="2-create-your-local-project"></a>2. <a name="create-an-azure-functions-project"></a> Yerel projenizi oluşturun

1. İçindeki Azure simgesini seçin <abbr title="Visual Studio Code penceresinin sol tarafındaki dikey simgeler grubu.">Etkinlik çubuğu</abbr>, ardından **Azure: Functions** alanında **Yeni proje oluştur...** simgesini seçin.

    ![Yeni proje oluştur'u seçin](./media/functions-create-first-function-vs-code/create-new-project.png)

1. Projenizin çalışma alanı için bir dizin konumu seçtikten sonra **Seç**'i belirleyin.

    > [!NOTE]
    > Bu adımlar, bir çalışma alanının dışında tamamlanacak şekilde tasarlandı. Bu örnekte, bir çalışma alanının parçası olan bir proje klasörünü seçmeyin.

1. İstemlerde aşağıdaki bilgileri sağlayın:

    + **İşlev projeniz için bir dil seçin**: `Python` öğesini seçin.

    + **Sanal ortam oluşturmak için bir Python diğer adı seçin**: Python yorumlayıcının konumunu seçin. Konum gösterilmemişse, Python ikilisinin tam yolunu yazın.  

    + **Projenizin ilk işlevi için bir şablon seçin**: `HTTP trigger` öğesini seçin.

    + **Bir işlev adı sağlayın**: `HttpExample` yazın.

    + **Yetkilendirme düzeyi**: `Anonymous` herhangi bir kişinin işlev uç noktanızı çağırmasını sağlayan öğesini seçin. Yetkilendirme düzeyleri hakkında bilgi edinmek için bkz. [Yetkilendirme anahtarları](functions-bindings-http-webhook-trigger.md#authorization-keys).

    + **Projenizi nasıl açmak istediğinizi seçin**: `Add to workspace` öğesini seçin.

<br/>
<details>
<summary><strong>Bir işlev projesi oluşturulamıyor mu?</strong></summary>

Yerel Işlevler projesi oluştururken çözülecek en yaygın sorunlar şunlardır:
* Azure Işlevleri uzantısı yüklü değil. 
</details>

<hr/>
<br/>

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

1. İşlev uygulaması projesini başlatmak için <kbd>F5</kbd> tuşuna basın.

1. **Terminal** panelinde, yerel olarak çalışan IŞLEVINIZIN URL uç noktası ' na bakın.

    ![Yerel işlev VS Code çıkışı](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)


1. Temel araçlar çalışırken **Azure: Functions** alanına gidin. **İşlevler** altında **Yerel proje**  >  **işlevleri**' ni genişletin. İşlevi sağ tıklatın (Windows) veya <kbd>CTRL</kbd> + tıklatın (MacOS) `HttpExample` ve **Şimdi işlevi Çalıştır...** seçeneğini belirleyin.

    :::image type="content" source="media/functions-run-function-test-local-vs-code/execute-function-now.png" alt-text="İşlevi şimdi Visual Studio Code Çalıştır":::

1. **İstek gövdesi gir** bölümünde istek iletisi gövdesi değerini görürsünüz `{ "name": "Azure" }` . Bu istek iletisini işlevinizde göndermek için ENTER tuşuna basın.  

1. İşlev yerel olarak yürütüldüğünde ve bir yanıt döndürdüğünde Visual Studio Code bir bildirim tetiklenir. İşlev yürütme hakkında bilgi, **Terminal** panelinde gösterilir.

1. Temel araçları durdurmak ve hata ayıklayıcının bağlantısını kesmek için <kbd>CTRL + C</kbd> tuşlarına basın.

<br/>
<details>
<summary><strong>İşlev yerel olarak çalıştırılamıyor musunuz?</strong></summary>

Yerel Işlevler projesi çalıştırılırken çözülmesi için en yaygın sorunlar şunlardır:
* Temel araçlar yüklü değil. 
*  Windows üzerinde çalışırken sorun yaşıyorsanız, Visual Studio Code için varsayılan Terminal kabuğunun **WSL Bash** olarak ayarlandığından emin olun. 
</details>

<hr/>
<br/>

## <a name="4-sign-in-to-azure"></a>4. Azure 'da oturum açın

Uygulamanızı yayımlamak için Azure 'da oturum açın. Zaten oturum açtıysanız sonraki bölüme gidin.

1. Etkinlik çubuğunda Azure simgesini seçin, sonra **Azure: işlevler** alanında **Azure 'da oturum aç ' ı seçin...**

    ![VS Code içinde Azure 'da oturum açın](../../includes/media/functions-sign-in-vs-code/functions-sign-into-azure.png)

1. Tarayıcıda istendiğinde, **Azure hesabınızı seçin** ve Azure hesabı kimlik bilgilerinizi kullanarak **oturum açın** .

1. Başarıyla oturum açtıktan sonra, yeni tarayıcı penceresini kapatıp Visual Studio Code ' a geri dönün. 

<hr/>
<br/>

## <a name="5-publish-the-project-to-azure"></a>5. projeyi Azure 'da yayımlayın

Kodunuzun ilk dağıtımı, Azure aboneliğinizde bir Işlev kaynağı oluşturmayı içerir.

1. Etkinlik çubuğunda Azure simgesini seçin, sonra **Azure: işlevler** alanında, **işlev uygulamasına dağıt uygulama...** düğmesini seçin.

    ![Projenizi Azure'da yayımlama](../../includes/media/functions-publish-project-vscode/function-app-publish-project.png)

1. İstemlerde aşağıdaki bilgileri sağlayın:

    + **Klasör seçin**: işlev uygulamanızı içeren klasörü seçin.

    + **Abonelik seçin**: kullanılacak aboneliği seçin. Yalnızca bir aboneliğiniz varsa bunu göremezsiniz.

    + **Azure'daki işlev uygulamasını seçin**: `+ Create new Function App` öğesini seçin.

    + **İşlev uygulaması için genel olarak benzersiz bir ad girin**: URL yolunda geçerli olan bir ad yazın. Yazdığınız ad, olduğundan emin olmak için onaylanır <abbr title="Ad tüm Azure müşterileri genelinde küresel olarak benzersiz olmalıdır. Örneğin, contoso-bizapp-Func-20 ' de olduğu gibi kişisel veya kuruluş adı, uygulama adı ve sayısal bir tanımlayıcı birleşimini kullanabilirsiniz.">Azure genelinde benzersiz</abbr>. 

    + **Çalışma zamanı seçin**: yerel olarak çalıştırdığınız Python sürümünü seçin. `python --version`Sürümünüzü denetlemek için komutunu kullanabilirsiniz.

    + **Yeni kaynaklar için bir konum seçin**: daha iyi performans için, size yakın bir [bölge](https://azure.microsoft.com/regions/) seçin.

    Uzantı, bildirim alanında Azure 'da oluşturulmakta olan ayrı kaynakların durumunu gösterir.

    :::image type="content" source="../../includes/media/functions-publish-project-vscode/resource-notification.png" alt-text="Azure kaynak oluşturma bildirimi":::

1. İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. Oluşturma ve dağıtım sonuçlarını görüntülemek için **çıktıyı görüntüle** ' yi seçin. 

    ![Tüm bildirimi oluştur](./media/functions-create-first-function-vs-code/function-create-notifications.png)

<br/>
<details>
<summary><strong>İşlev yayımlanamıyor mu?</strong></summary>

Bu bölüm, Azure kaynaklarını oluşturdu ve yerel kodunuzu Işlev uygulamasına dağıttı. Bu başarılı olmadıysa:

* Hata bilgileri için çıktıyı gözden geçirin. Sağ alt köşedeki zil simgesi çıktıyı görüntülemenin başka bir yoludur. 
* Var olan bir işlev uygulamasına yayımladınız mı? Bu eylem, Azure 'daki uygulamanın içeriğinin üzerine yazar.
</details>


<br/>
<details>
<summary><strong>Hangi kaynaklar oluşturuldu?</strong></summary>

Tamamlandığında, aşağıdaki Azure kaynakları, işlev uygulamanızın adına göre adlar kullanılarak aboneliğinizde oluşturulur: 
* **Kaynak grubu**: kaynak grubu, aynı bölgedeki ilgili kaynaklar için bir mantıksal kapsayıcıdır.
* **Azure depolama hesabı**: bir depolama kaynağı, projeniz hakkındaki durumu ve diğer bilgileri korur.
* **Tüketim planı**: bir tüketim planı, sunucusuz işlev uygulamanız için temel ana bilgisayarı tanımlar.
* **İşlev uygulaması**: bir işlev uygulaması, işlev kodunuzu ve grup işlevlerinizi mantıksal birim olarak yürütmek için ortam sağlar.
* **Application Insights**: Application Insights sunucusuz işlevinizin kullanımını izler.

</details>

<hr/>
<br/>

## <a name="6-run-the-function-in-azure"></a>6. işlevi Azure 'da çalıştırın

1. **Azure: Functions** yan çubuğuna geri dönerek yeni işlev uygulamasını genişletin.
1. **İşlevler**' i genişletin, ardından işlevi sağ tıklatın (Windows) veya <kbd>CTRL</kbd> + tıklatın (MacOS) `HttpExample` ve **Şimdi işlevi Çalıştır...** seçeneğini belirleyin.

    :::image type="content" source="../../includes/media/functions-vs-code-run-remote/execute-function-now.png" alt-text="İşlevi Azure 'da şimdi çalıştırın Visual Studio Code":::

1. **İstek gövdesi gir** bölümünde istek iletisi gövdesi değerini görürsünüz `{ "name": "Azure" }` .

    Bu istek iletisini işlevinizde göndermek için ENTER tuşuna basın.  

1. İşlev Azure 'da yürütüldüğünde ve bir yanıt döndürdüğünde, Visual Studio Code bir bildirim oluşturulur.

## <a name="7-clean-up-resources"></a>7. Kaynakları Temizleme

Bir [sonraki adıma](#next-steps) devam edip bir <abbr title="Bir işlevi bir depolama kuyruğu ile ilişkilendirmekte, böylece kuyrukta ileti oluşturabilmesi anlamına gelir.">Azure depolama kuyruğu çıktı bağlama</abbr> işlevinizde, daha önce yapmış olduğunuz şeyi oluşturmak için tüm kaynaklarınızı saklamanız gerekir.

Aksi takdirde, daha fazla maliyet ödemeden kaçınmak için işlev uygulamasını ve ilgili kaynaklarını silmek için aşağıdaki adımları kullanabilirsiniz.

[!INCLUDE [functions-cleanup-resources-vs-code-inner.md](../../includes/functions-cleanup-resources-vs-code-inner.md)]

Işlev maliyetleri hakkında daha fazla bilgi edinmek için bkz. [Tüketim planı maliyetlerini tahmin](functions-consumption-costs.md)etme.

## <a name="next-steps"></a>Sonraki adımlar

Bir çıkış ekleyerek bu işlevi genişletme <abbr title="Bir işlev ve diğer kaynaklar arasında bildirime dayalı bağlantı. Giriş bağlama işlevine veri sağlar; çıkış bağlama, işlevden diğer kaynaklara veri sağlar.">bağlama</abbr>. Bu bağlama, HTTP isteğinden dizeyi bir Azure kuyruk depolama kuyruğundaki bir iletiye yazar. 

> [!div class="nextstepaction"]
> [Azure depolama kuyruğuna bağlanma](functions-add-output-binding-storage-queue-vs-code.md?pivots=programming-language-python)

[Sorun mu yaşıyorsunuz? Bize bilgi verin.](https://aka.ms/python-functions-qs-survey)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions

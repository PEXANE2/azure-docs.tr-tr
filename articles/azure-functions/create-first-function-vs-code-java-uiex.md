---
title: Visual Studio Code-Azure Işlevleri kullanarak bir Java işlevi oluşturma
description: Java işlevi oluşturmayı öğrenin ve ardından Visual Studio Code içindeki Azure Işlevleri uzantısını kullanarak yerel projeyi Azure Işlevleri 'nde sunucusuz barındırma için yayımlayın.
ms.topic: quickstart
ms.date: 11/03/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 6c84bf4ebc73857fa0280ffbcbf46b68c2da630f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102031727"
---
# <a name="quickstart-create-a-java-function-in-azure-using-visual-studio-code"></a>Hızlı başlangıç: Visual Studio Code kullanarak Azure 'da Java işlevi oluşturma

[!INCLUDE [functions-language-selector-quickstart-vs-code](../../includes/functions-language-selector-quickstart-vs-code.md)]

HTTP isteklerine yanıt veren bir Java işlevi oluşturmak için Visual Studio Code kullanın. Kodu yerel olarak test edin ve daha sonra Azure Işlevlerinin sunucusuz ortamına dağıtın.

Bu hızlı başlangıcı tamamlamak, birkaç ABD Doları veya daha kısa bir ücret doğurur <abbr title="Azure kullanımı için fatura bilgilerini tutan profil.">Azure hesabı</abbr>.

Tercih ettiğiniz geliştirme aracınız Visual Studio Code, [Maven](create-first-function-cli-java.md), [Gradle](./functions-create-first-java-gradle.md) ve [IntelliJ fikrini](/azure/developer/java/toolkit-for-intellij/quickstart-functions)kullanarak Java geliştiricileri için benzer öğreticilerimize göz atın.

## <a name="1-prepare-your-environment"></a>1. ortamınızı hazırlayın

Başlamadan önce, aşağıdaki gereksinimlerin yerinde olduğundan emin olun:

+ Etkin bir Azure hesabı <abbr title="Azure 'da, genellikle bir kuruluş içindeki bir birey veya departmanla ilişkili kaynakları yönettiğiniz temel kuruluş yapısıdır.">aboneliği</abbr>. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ [Java geliştirici seti](/azure/developer/java/fundamentals/java-jdk-long-term-support), sürüm 8 veya 11.

+ [Apache Maven](https://maven.apache.org), sürüm 3,0 veya üzeri.

+ [Desteklenen platformlardan](https://code.visualstudio.com/docs/supporting/requirements#_platforms) birinde çalışan [Visual Studio Code](https://code.visualstudio.com/).

+ [Java Uzantı paketi](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)  

+ Visual Studio Code için [Azure İşlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions).

<br/>
<hr/>

## <a name="2-create-your-local-functions-project"></a>2. <a name="create-an-azure-functions-project"></a> yerel işlevler projenizi oluşturun

1. **Etkinlik çubuğunda** Azure simgesini seçin, sonra **Azure: işlevler** alanında **Yeni proje oluştur...** simgesini seçin.

    ![Yeni proje oluştur'u seçin](./media/functions-create-first-function-vs-code/create-new-project.png)

1. Proje çalışma alanınız için **bir dizin konumu seçin** ve ardından **Seç**' i seçin.

1. İstemlerde aşağıdaki bilgileri sağlayın:

    + **İşlev projeniz için bir dil seçin**: `Java` öğesini seçin.

    + **Java sürümü seçin**: `Java 8` `Java 11` Işlevlerinizin Azure 'da çalıştığı Java sürümünü seçin veya seçin. Yerel olarak doğruladığınızı bir Java sürümü seçin.

    + **Bir grup kimliği sağlayın**: Seç `com.function` .

    + **YAPıT kimliği sağlayın**: seçin `myFunction` .

    + **Bir sürüm belirtin**: Seç `1.0-SNAPSHOT` .

    + **Bir paket adı belirtin**: Seç `com.function` .

    + **Bir uygulama adı belirtin**: Seç `myFunction-12345` .

    + **Yetkilendirme düzeyi**: `Anonymous` herhangi bir kişinin işlev uç noktanızı çağırmasını sağlayan öğesini seçin.

    + **Projenizi nasıl açmak istediğinizi seçin**: `Add to workspace` öğesini seçin.

<br/>

<details>
<summary><strong>Bir işlev projesi oluşturulamıyor mu?</strong></summary>

Yerel Işlevler projesi oluştururken çözülecek en yaygın sorunlar şunlardır:
* Azure Işlevleri uzantısı yüklü değil. 
</details>

<br/>
<hr/>

## <a name="3-run-the-function-locally"></a>3. işlevi yerel olarak çalıştırın

1. İşlev uygulaması projesini başlatmak için <kbd>F5</kbd> tuşuna basın.

1. **Terminalde**, işlevinizin yerel olarak çalıştırıldığı URL uç noktasına bakın.

    ![Yerel işlev VS Code çıkışı](media/functions-create-first-function-vs-code/functions-vscode-f5.png)

1. Temel araçlar çalışırken **Azure: Functions** alanına gidin. **İşlevler** altında **Yerel proje**  >  **işlevleri**' ni genişletin. İşlevi sağ tıklatın (Windows) veya <kbd>CTRL</kbd> + tıklatın (MacOS) `HttpExample` ve **Şimdi işlevi Çalıştır...** seçeneğini belirleyin.

    :::image type="content" source="../../includes/media/functions-run-function-test-local-vs-code/execute-function-now.png" alt-text="İşlevi şimdi Visual Studio Code Çalıştır":::

1. **İstek gövdesi gir** bölümünde istek iletisi gövdesi değerini görürsünüz `{ "name": "Azure" }` . Bu istek iletisini işlevinizde göndermek için <kbd>ENTER</kbd> tuşuna basın.  

1. İşlev yerel olarak yürütüldüğünde ve bir yanıt döndürdüğünde Visual Studio Code bir bildirim tetiklenir. İşlev yürütme hakkında bilgi, **Terminal** panelinde gösterilir.

1. Temel araçları durdurmak ve hata ayıklayıcının bağlantısını kesmek için <kbd>CTRL + C</kbd> tuşlarına basın.

<br/>

<details>
<summary><strong>İşlev yerel olarak çalıştırılamıyor musunuz?</strong></summary>

Yerel Işlevler projesi çalıştırılırken çözülmesi için en yaygın sorunlar şunlardır:
* Temel araçlar yüklü değil. 
*  Windows üzerinde çalışırken sorun yaşıyorsanız, Visual Studio Code için varsayılan Terminal kabuğunun WSL Bash olarak ayarlandığından emin olun. 
</details>

<br/>
<hr/>

## <a name="4-sign-in-to-azure"></a>4. Azure 'da oturum açın

Uygulamanızı yayımlamak için Azure 'da oturum açın. Zaten oturum açtıysanız sonraki bölüme gidin.

1. Etkinlik çubuğunda Azure simgesini seçin, sonra **Azure: işlevler** alanında **Azure 'da oturum aç ' ı seçin...**

    ![VS Code içinde Azure 'da oturum açın](../../includes/media/functions-sign-in-vs-code/functions-sign-into-azure.png)

1. Tarayıcıda istendiğinde, **Azure hesabınızı seçin** ve Azure hesabı kimlik bilgilerinizi kullanarak **oturum açın** .

1. Başarıyla oturum açtıktan sonra, yeni tarayıcı penceresini kapatıp Visual Studio Code ' a geri dönün.

<br/>
<hr/>

## <a name="5-publish-the-project-to-azure"></a>5. projeyi Azure 'da yayımlayın

Kodunuzun ilk dağıtımı, Azure aboneliğinizde bir Işlev kaynağı oluşturmayı içerir.

1. Etkinlik çubuğunda Azure simgesini seçin, sonra **Azure: işlevler** alanında, **işlev uygulamasına dağıt uygulama...** düğmesini seçin.

    ![Projenizi Azure'da yayımlama](../../includes/media/functions-publish-project-vscode/function-app-publish-project.png)

1. İstemlerde aşağıdaki bilgileri sağlayın:

    + **Klasör seçin**: işlev uygulamanızı içeren klasörü seçin. 

    + **Abonelik seçin**: kullanılacak aboneliği seçin. Yalnızca bir aboneliğiniz varsa bunu göremezsiniz.

    + **Azure'daki işlev uygulamasını seçin**: `Create new Function App` öğesini seçin.

    + **İşlev uygulaması için genel olarak benzersiz bir ad girin**: bir URL yolunda Azure genelinde benzersiz bir ad yazın. Yazdığınız ad, genel benzersizlik sağlamak için onaylanır.

    - **Yeni kaynaklar için bir konum seçin**: daha iyi performans için, size yakın bir [bölge](https://azure.microsoft.com/regions/) seçin.

1. İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. Oluşturma ve dağıtım sonuçlarını görmek için **çıktıyı görüntüle** ' yi seçin.

    ![Tüm bildirimi oluştur](../../includes/media/functions-publish-project-vscode/function-create-notifications.png)

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

<br/>
<hr/>

## <a name="6-run-the-function-in-azure"></a>6. işlevi Azure 'da çalıştırın

1. Yan çubuktaki **Azure: Functions** alanına geri döndüğünüzde aboneliğinizi, yeni işlev uygulamanızı ve **işlevlerinizi** genişletin. İşlevi sağ tıklatın (Windows) veya <kbd>CTRL</kbd> + tıklatın (MacOS) `HttpExample` ve **Şimdi işlevi Çalıştır...** seçeneğini belirleyin.

    :::image type="content" source="../../includes/media/functions-vs-code-run-remote/execute-function-now.png" alt-text="İşlevi Azure 'da şimdi çalıştırın Visual Studio Code":::

1. **İstek gövdesi gir** bölümünde istek iletisi gövdesi değerini görürsünüz `{ "name": "Azure" }` . Bu istek iletisini işlevinizde göndermek için ENTER tuşuna basın.  

1. İşlev Azure 'da yürütüldüğünde ve bir yanıt döndürdüğünde, Visual Studio Code bir bildirim oluşturulur.

<br/>
<hr/>

## <a name="7-clean-up-resources"></a>7. Kaynakları Temizleme

[Sonraki adıma](#next-steps)devam etmeyi planlamıyorsanız, daha fazla maliyet ödemeden kaçınmak için işlev uygulamasını ve kaynaklarını silin.

1. Visual Studio Code ' de, etkinlik çubuğundan Azure simgesini seçin ve ardından yan çubukta Işlevler alanını seçin.
1. İşlev uygulamasını seçin, sonra sağ tıklayıp **işlev uygulamasını Sil...** seçeneğini belirleyin.

<br/>
<hr/>

## <a name="next-steps"></a>Sonraki adımlar

Şunu ekleyerek işlevi genişletin <abbr title="Azure depolama 'da bir işlevi bir depolama kuyruğu ile ilişkilendirmekte, bu sayede sırada ileti oluşturabilmesi anlamına gelir.">çıkış bağlama</abbr>. Bu bağlama, HTTP isteğinden dizeyi bir Azure kuyruk depolama kuyruğundaki bir iletiye yazar.

> [!div class="nextstepaction"]
> [Azure depolama kuyruğuna bağlanma](functions-add-output-binding-storage-queue-vs-code.md?pivots=programming-language-java)

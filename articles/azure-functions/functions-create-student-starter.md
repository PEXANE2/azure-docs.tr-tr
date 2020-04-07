---
title: Öğrenciler Başlangıç Için Azure'u kullanarak bir işlev oluşturma
description: Öğrenci Başlangıç aboneliği için Azure'un içinden azure işlevi oluşturmayı öğrenin
Customer intent: As a student, I want to be able to create an HTTP triggered Function App within the Student Starter plan so that I can easily add APIs to any project.
author: alexkarcher-msft
ms.topic: how-to
ms.date: 02/22/2019
ms.author: alkarche
ms.openlocfilehash: c7dd88bf0ead558a0c4951baf38543566d805caa
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756466"
---
# <a name="create-a-function-using-azure-for-students-starter"></a>Öğrenciler Başlangıç Için Azure'u kullanarak bir işlev oluşturma

Bu eğitimde, Öğrenciler Başlangıç aboneliği için bir Azure'da bir "merhaba dünyası" HTTP işlevi oluşturacağız. Ayrıca, bu abonelik türünde Azure İşlevlerinde mevcut olanları da gözden geçiririz.

Öğrenciler için Microsoft *Azure,* bulutta geliştirmeniz gereken Azure ürünlerini size hiçbir ücret ödemeden başlatıyor. [Bu teklif hakkında daha fazla bilgi için burada.](https://azure.microsoft.com/offers/ms-azr-0144p/)

Azure İşlevleri, öncelikle bir VM oluşturmak veya bir web uygulaması yayımlamak zorunda kalmadan kodunuzu [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir ortamda yürütmenize olanak tanır. [Fonksiyonlar hakkında daha fazla bilgi için burada.](./functions-overview.md)

## <a name="create-a-function"></a>İşlev oluşturma

 Bu konuda, Azure portalında http tetiklenen bir "merhaba dünyası" işlevi oluşturmak için Işlevleri nasıl kullanacağınızı öğrenin.

![Azure portalında işlev uygulaması oluşturma](./media/functions-create-student-starter/function-app-in-portal-editor.png)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla Azure portalında <https://portal.azure.com> sayfasında oturum açın.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlevleri daha kolay yönetim, dağıtım, ölçekleme ve kaynakların paylaşımı için mantıksal bir birim olarak gruplandırmanıza olanak tanır.

1. Azure portalının sol üst köşesinde bulunan **kaynak oluştur** düğmesini seçin. Sonra **İşlem** > **Fonksiyonu Uygulaması'nı**seçin.

    ![Azure portalında işlev uygulaması oluşturma](./media/functions-create-student-starter/function-app-create-flow.png)

2. Görüntünün altındaki tabloda belirtilen işlev uygulaması ayarlarını kullanın.

    <img src="./media/functions-create-student-starter/Function-create-start.png" width="315">

    | Ayar      | Önerilen değer  | Açıklama                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Uygulama adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler: `a-z`, `0-9`, ve `-`.  | 
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. | 
    | **[Kaynak Grubu](../azure-resource-manager/management/overview.md)** |  myResourceGroup | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. |
   | **[Uygulama Hizmet Planı/Konumu](./functions-scale.md)** | Yeni | İşlev uygulamanızın hangi bölgeye dağıtılan bölgeye ve kaynaklarınızın yoğunluğunu kontrol eden barındırma planı. Aynı plana dağıtılan Birden Çok Fonksiyon Uygulaması nın tümü aynı tek ücretsiz örneği paylaşır. Bu, Öğrenci Başlangıç planının bir kısıtlamasIdır. Tam barındırma seçenekleri [burada açıklanmıştır.](./functions-scale.md)|
    | **Çalışma zamanı yığını** | Tercih edilen dil | Tercih ettiğiniz işlev programlama dilini destekleyen bir çalışma zamanı seçin. C# ve F# için **.NET** işlevlerini seçin. |
    |**[Application Insights](./functions-monitoring.md)**| Etkin | Application Insights, işlev uygulamanızın günlüklerini depolamak ve analiz etmek için kullanılır. Uygulama Öngörülerini destekleyen bir konum seçerseniz varsayılan olarak etkinleştirilir. Uygulama Öngörüleri, Uygulama Öngörülerini dağıtmak için yakındaki bir bölgeyi el ile seçerek herhangi bir işlev için etkinleştirilebilir. Application Insights olmadan yalnızca canlı akış günlüklerini görüntüleyebilirsiniz.

3. Farklı bir konum seçmek için yukarıdaki **Uygulama Hizmeti planını/Konumunu** seçin

4. **Yeni Oluştur'u** seçin ve ardından planınıza benzersiz bir ad verin.

5. Size en yakın konumu seçin. [Azure bölgelerinin tam haritasını burada görebilirsiniz.](https://azure.microsoft.com/global-infrastructure/regions/) 

    <img src="./media/functions-create-student-starter/Create-ASP.png" width="800">

6. İşlev uygulamasını sağlamak ve dağıtmak için **Oluştur**'u seçin.

    <img src="./media/functions-create-student-starter/Function-create-end.png" width="315">

7. Portalın sağ üst köşesindeki Bildirim simgesini seçin ve **Dağıtım başarılı** iletisini bekleyin.

    ![Yeni işlev uygulaması ayarlarını tanımlama](./media/functions-create-student-starter/function-app-create-notification.png)

8. Yeni işlev uygulamanızı görüntülemek için **Kaynağa git**’i seçin.

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

## <a name="create-an-http-triggered-function"></a><a name="create-function"></a>HTTP ile tetiklenen bir işlev oluşturma

1. Yeni işlev uygulamanızı genişletin, ardından **+** **Fonksiyonlar'ın**yanındaki düğmeyi seçin, Portal **Içi'ni**seçin ve **Devam et'i**seçin.

    ![İşlevler hızlı başlangıcı platform seçimi.](./media/functions-create-student-starter/function-app-quickstart-choose-portal.png)

1. **Web Kancası + API**'yi ve ardından **Oluştur**'u seçin.

    ![Azure portalındaki İşlevler hızlı başlangıcı.](./media/functions-create-student-starter/function-app-quickstart-node-webhook.png)

Dile özgü HTTP ile tetiklenen işlev şablonu kullanılarak bir işlev oluşturulur.

Artık bir HTTP isteği göndererek yeni işlevi çalıştırabilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Yeni işlevinizde sağ üst kısımdaki **</> İşlev URL'sini al**'a tıklayın, **varsayılan (İşlev anahtarı)** seçeneğini belirleyin ve ardından **Kopyala**'ya tıklayın. 

    ![Azure portalından işlev URL’sini kopyalama](./media/functions-create-student-starter/function-app-develop-tab-testing.png)

2. İşlev URL'sini tarayıcınızın adres çubuğuna yapıştırın. `&name=<yourname>` sorgu dizesi değerini bu URL’nin sonuna ekleyin ve isteği yürütmek için klavyenizdeki `Enter` tuşuna basın. İşlev tarafından döndürülen yanıtın tarayıcıda gösterildiğini görürsünüz.  

    Aşağıdaki örnekte tarayıcıdaki yanıt gösterilmektedir:

    ![Tarayıcıdaki işlev yanıtı.](./media/functions-create-student-starter/function-app-browser-testing.png)

    İstek URL’si, işlevinize HTTP üzerinden erişmek için varsayılan olarak gerekli olan bir anahtar içerir.

3. İşleviniz çalıştığında, izleme bilgileri günlüklere yazılır. Önceki yürütme işleminden alınan izleme çıktısını görmek için, portalda işlevinize geri dönün ve ekranın altındaki oka tıklayarak **Günlükler**’i genişletin.

   ![Azure portalında İşlevler günlük görüntüleyicisi.](./media/functions-create-student-starter/function-view-logs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="supported-features-in-azure-for-students-starter"></a>Öğrenciler Başlangıç için Azure'da desteklenen özellikler

Azure for Students Starter'da Azure İşlevleri çalışma zamanının çoğuna erişebilirsiniz ve aşağıda birkaç temel sınırlama lar sıralanmıştır:

* HTTP tetikleyicisi desteklenen tek tetikleyici türüdür.
    * Tüm giriş ve tüm çıkış bağlamaları desteklenir! [Tam listeye buradan bakınız.](functions-triggers-bindings.md)
* Desteklenen Diller: 
    * C# (.NET Çekirdek 2)
    * JavaScript (Düğüm.js 8 & 10)
    * F# (.NET Çekirdek 2)
    * [Daha yüksek planlarda desteklenen dilleri burada görün](supported-languages.md)
* Windows desteklenen tek işletim sistemidir.
* Ölçek, her gün 60 dakikaya kadar çalışan [bir serbest katman örneğiyle](https://azure.microsoft.com/pricing/details/app-service/windows/) sınırlıdır. HTTP trafiği alındıkça 0'dan 1'e otomatik olarak ölçeklendirilir, ancak daha fazla olmaz.
* Yalnızca [sürüm 2.x ve daha sonra](functions-versions.md) Fonksiyonlar çalışma süresi desteklenir.
* Tüm geliştirici araç düzenleme ve yayımlama işlevleri için desteklenir. Buna VS Kodu, Visual Studio, Azure CLI ve Azure portalı dahildir. Portal dışında bir şey kullanmak istiyorsanız, önce portalda bir uygulama oluşturmanız ve ardından tercih ettiğiniz araçta bu uygulamayı dağıtım hedefi olarak seçmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Basit bir HTTP tetikleme işlevi ile bir işlev uygulaması oluşturduk! Artık yerel araç, daha fazla dil, izleme ve tümleştirmeleri keşfedebilirsiniz.

 * [Visual Studio kullanarak ilk işlevinizi oluşturma](./functions-create-your-first-function-visual-studio.md)
 * [Visual Studio Code kullanarak ilk işlevinizi oluşturma](./functions-create-first-function-vs-code.md)
 * [Azure İşlevler JavaScript geliştirici kılavuzu](./functions-reference-node.md)
 * [Azure SQL Veritabanına bağlanmak için Azure Işlevlerini kullanma](./functions-scenario-database-table-cleanup.md)
 * [Azure İşlerİ HTTP ciltleri hakkında daha fazla bilgi edinin.](./functions-bindings-http-webhook.md)
 * [Azure İşlerinizi İzleyebilirsiniz](./functions-monitoring.md)

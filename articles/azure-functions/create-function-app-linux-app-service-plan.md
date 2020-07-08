---
title: Linux üzerinde Azure portal bir işlev uygulaması oluşturma
description: Azure portalını kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin.
ms.topic: how-to
ms.date: 04/29/2020
ms.openlocfilehash: 5aae60900a61c28f6c53f89b8e273daccb047eef
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83116204"
---
# <a name="create-a-function-app-on-linux-in-an-azure-app-service-plan"></a>Azure App Service planında Linux üzerinde bir işlev uygulaması oluşturma

Azure İşlevleri, işlevlerinizi Linux’ta varsayılan bir Azure App Service kapsayıcısında barındırmanıza olanak sağlar. Bu makalede, bir [App Service planında](functions-scale.md#app-service-plan)çalışan Linux ile barındırılan bir işlev uygulaması oluşturmak için [Azure Portal](https://portal.azure.com) nasıl kullanılacağı açıklanmaktadır. Ayrıca [kendi özel kapsayıcınızı getirebilirsiniz](functions-create-function-linux-custom-image.md).

![Azure portalında işlev uygulaması oluşturma](./media/create-function-app-linux-app-service-plan/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla Azure portalında <https://portal.azure.com> sayfasında oturum açın.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Linux’ta işlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun yürütülmesine yönelik bir ortam sağlar. Kaynakların daha kolay yönetilmesi, dağıtılması, ölçeklendirilmesi ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanızı sağlar. Bu makalede, işlev uygulamanızı oluştururken bir App Service planı oluşturacaksınız.

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**'u seçin.

1. **Yeni** sayfasında, **işlem**  >  **işlev uygulaması**' yi seçin.

    :::image type="content" source="./media/create-function-app-linux-app-service-plan/function-app-create-flow.png" alt-text="Azure portalında işlev uygulaması oluşturma":::

1. **Temel bilgiler** sayfasında, aşağıdaki tabloda belirtilen işlev uygulaması ayarlarını kullanın.

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. |
    | **[Kaynak Grubu](../azure-resource-manager/management/overview.md)** |  *myResourceGroup* | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. |
    | **İşlev Uygulaması adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler `a-z` (büyük/küçük harf duyarsız), `0-9` ve `-` .  |
    |**Yayımlama**| **Kod** (varsayılan) | Kod dosyalarını veya Docker kapsayıcısını yayımlama seçeneği. |
    | **Çalışma zamanı yığını** | Tercih edilen dil | Tercih ettiğiniz işlev programlama dilini destekleyen bir çalışma zamanı seçin. C# ve F # işlevleri için **.NET Core** ' u seçin. |
    |**Sürüm**| Sürüm numarası | Yüklü çalışma zamanının sürümünü seçin.  |
    |**Geli**| Tercih edilen bölge | Kendinize veya işlevinizin erişeceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |

    :::image type="content" source="./media/create-function-app-linux-app-service-plan/function-app-create-basics-linux.png" alt-text="Temel bilgiler sayfası":::

1. Ileri 'yi seçin **: barındırma**. **Barındırma** sayfasında, aşağıdaki ayarları girin.

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **[Depolama hesabı](../storage/common/storage-account-create.md)** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan bir depolama hesabı oluşturun. Depolama hesabı adları 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakamlar ve küçük harfler içerebilir. Ayrıca, [depolama hesabı gereksinimlerini](../azure-functions/functions-scale.md#storage-account-requirements)karşılaması gereken mevcut bir hesabı da kullanabilirsiniz. |
    |**İşletim sistemi**| **Linux** | Çalışma zamanı yığını seçiminize göre sizin için bir işletim sistemi önceden seçilmiştir, ancak gerekirse ayarı değiştirebilirsiniz. |
    | **[Planlama](../azure-functions/functions-scale.md)** | **Tüketim (sunucusuz)** | Kaynakların işlev uygulamanıza nasıl ayrılacağını tanımlayan barındırma planı. Varsayılan **Tüketim** planında, kaynaklar işlevlerinizin gerektirdiği şekilde dinamik olarak eklenir. Bu [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) barındırmada yalnızca işlevlerinizin çalıştığı süre için ödeme yaparsınız. Bir App Service planı içinde çalıştırdığınızda [işlev uygulamanızın ölçeklendirmesini](../azure-functions/functions-scale.md) yönetmeniz gerekir.  |

    :::image type="content" source="./media/create-function-app-linux-app-service-plan/function-app-create-hosting-linux.png" alt-text="Barındırma sayfası":::

1. **İleri: izleme**öğesini seçin. **İzleme** sayfasında, aşağıdaki ayarları girin.

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](../azure-functions/functions-monitoring.md)** | **Evet** (varsayılan) | En yakın desteklenen bölgede aynı *uygulama adının* Application Insights kaynağını oluşturur. Bu ayarı genişleterek veya **Yeni oluştur**' u seçerek, Application Insights adını değiştirebilir veya verilerinizi depolamak istediğiniz [Azure Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) 'da farklı bir bölge seçebilirsiniz. |

   :::image type="content" source="./media/create-function-app-linux-app-service-plan/function-app-create-monitoring-linux.png" alt-text="İzleme sayfası":::

1. Uygulama yapılandırma seçimlerini gözden geçirmek için **gözden geçir + oluştur** ' u seçin.

1. **Gözden geçir + oluştur** sayfasında ayarlarınızı gözden geçirin ve sonra işlev uygulamasını sağlamak ve dağıtmak için **Oluştur** ' u seçin.

1. Portalın sağ üst köşesindeki **Bildirimler** simgesini seçin ve **dağıtım başarılı** iletisini izleyin.

1. Yeni işlev uygulamanızı görüntülemek için **Kaynağa git**’i seçin. **Panoya sabitle ' yi**de seçebilirsiniz. Sabitleme, panonuzdan bu işlev uygulama kaynağına döndürülmesini kolaylaştırır.

    ![Dağıtım bildirimi](./media/create-function-app-linux-app-service-plan/function-app-create-notification2.png)

    İşlev uygulamanız kullanılabilir olsa bile, tam olarak başlatılması birkaç dakika sürebilir.

Ardından, yeni işlev uygulamasında bir işlev oluşturun.

## <a name="create-an-http-trigger-function"></a><a name="create-function"></a>HTTP tetikleyici işlevi oluşturma

Bu bölümde, portalda yeni işlev uygulamanızda nasıl bir işlev oluşturacağınız gösterilmektedir.

> [!NOTE]
> Portal geliştirme deneyimi, Azure Işlevleri 'ni denemek için yararlı olabilir. Çoğu senaryoda, işlevlerinizi yerel olarak geliştirmeyi ve [Visual Studio Code](functions-create-first-function-vs-code.md#create-an-azure-functions-project) ya da [Azure Functions Core Tools](functions-run-local.md#create-a-local-functions-project)kullanarak projeyi işlev uygulamanızda yayımlamayı düşünün.  

1. **İşlevler** penceresinin sol menüsünde **işlevler**' i seçin ve ardından üst menüden **Ekle** ' yi seçin. 
 
1. **Yeni işlev** penceresinden **http tetikleyicisi**' ni seçin.

    ![HTTP tetikleyici işlevini seçin](./media/create-function-app-linux-app-service-plan/function-app-select-http-trigger.png)

1. **Yeni işlev** penceresinde, **yeni işlev**için varsayılan adı kabul edin veya yeni bir ad girin. 

1. **Yetkilendirme düzeyi** açılan listesinden **anonim** ' i seçin ve ardından **işlev oluştur**' u seçin.

    Azure, HTTP tetikleyici işlevini oluşturur. Artık bir HTTP isteği göndererek yeni işlevi çalıştırabilirsiniz.

## <a name="test-the-function"></a>İşlevi test etme

1. Yeni HTTP tetikleyici işlevinizde, sol menüden **Code + test** ' i seçin ve ardından üstteki menüden **Işlev URL 'sini al** ' ı seçin.

    ![İşlev URL 'sini Al ' ı seçin](./media/create-function-app-linux-app-service-plan/function-app-select-get-function-url.png)

1. **İşlev URL 'Sini al** iletişim kutusunda, açılan listeden **varsayılan** ' ı seçin ve ardından **Panoya Kopyala** simgesini seçin. 

    ![Azure portalından işlev URL’sini kopyalama](./media/create-function-app-linux-app-service-plan/function-app-develop-tab-testing.png)

1. İşlev URL'sini tarayıcınızın adres çubuğuna yapıştırın. Sorgu dizesi değerini `?name=<your_name>` Bu URL 'nin sonuna ekleyin ve isteği çalıştırmak Için ENTER 'a basın. 

    Aşağıdaki örnekte tarayıcıdaki yanıt gösterilmektedir:

    ![Tarayıcıdaki işlev yanıtı.](./media/create-function-app-linux-app-service-plan/function-app-browser-testing.png)

    İstek URL’si, işlevinize HTTP üzerinden erişmek için varsayılan olarak gerekli olan bir anahtar içerir.

1. İşleviniz çalıştığında, izleme bilgileri günlüklere yazılır. İzleme çıktısını görmek için, portalda **Code + test** sayfasına dönüp sayfanın altındaki **Günlükler** okunu genişletin.

   ![Azure portalında İşlevler günlük görüntüleyicisi.](./media/create-function-app-linux-app-service-plan/function-view-logs.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Basit bir HTTP tetikleyici işlevi olan bir işlev uygulaması oluşturdunuz.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Daha fazla bilgi için bkz. [Azure İşlevleri HTTP bağlamaları](functions-bindings-http-webhook.md).

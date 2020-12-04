---
title: Postman ile istekte bulunma
titleSuffix: Azure Digital Twins
description: Azure dijital TWINS API 'Lerini test etmek için Postman yapılandırma ve kullanma hakkında bilgi edinin.
ms.author: baanders
author: baanders
ms.service: digital-twins
services: digital-twins
ms.topic: how-to
ms.date: 11/10/2020
ms.openlocfilehash: 5dbe161af172b65919328fca0b272f3b658f1b32
ms.sourcegitcommit: c4246c2b986c6f53b20b94d4e75ccc49ec768a9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96600498"
---
# <a name="how-to-use-postman-to-send-requests-to-the-azure-digital-twins-apis"></a>Azure dijital TWINS API 'Lerine istek göndermek için Postman kullanma

[Postman](https://www.getpostman.com/) , bir masaüstü ve EKLENTI tabanlı GUI 'de önemli http isteği işlevleri sağlayan bir rest test aracıdır. Bunu kullanarak HTTP istekleri oluşturabilir ve bunları [Azure Digital TWINS REST API 'lerine](how-to-use-apis-sdks.md)gönderebilirsiniz.

Bu makalede, [Postman Rest Istemcisinin](https://www.getpostman.com/) Azure dijital TWINS API 'leri ile etkileşime geçmek için aşağıdaki adımlarla nasıl yapılandırılacağı açıklanmaktadır:

1. Postman 'da API istekleri yapmak için kullanacağınız bir taşıyıcı belirteci almak için [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) 'yi kullanın.
1. Bir Postman koleksiyonu kurun ve Postman REST istemcisini, kimlik doğrulaması için taşıyıcı belirtecinizi kullanacak şekilde yapılandırın.
1. Azure dijital TWINS API 'Lerine bir istek oluşturmak ve göndermek için yapılandırılmış Postman 'ı kullanın.

## <a name="prerequisites"></a>Önkoşullar

Azure dijital TWINS API 'Lerine erişmek için Postman kullanmaya devam etmek için bir Azure dijital TWINS örneği ayarlamanız ve Postman 'yı indirmeniz gerekir. Bu bölümün geri kalanında bu adımlarda adım adım gösterilmektedir.

### <a name="set-up-azure-digital-twins-instance"></a>Azure dijital TWINS örneğini ayarlama

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

### <a name="download-postman"></a>Postman indirin

Ardından, Postman istemcisinin Masaüstü sürümünü indirin. [*Www.getpostman.com/apps*](https://www.getpostman.com/apps) adresine gidin ve uygulamayı indirmek için yönergeleri izleyin.

## <a name="get-bearer-token"></a>Taşıyıcı belirtecini al

Postman 'ı ve Azure dijital TWINS örneğinizi ayarlamış olduğunuza göre, Postman isteklerinin Azure dijital TWINS API 'Lerinde yetkilendirmek için kullanabileceği bir taşıyıcı belirteç almanız gerekir.

Bu belirteci almanın birkaç olası yolu vardır. Bu makalede, Azure hesabınızda oturum açmak ve bu şekilde bir belirteç almak için [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) kullanılmaktadır.

[Yerel olarak yüklü](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true)BIR Azure CLI 'niz varsa, aşağıdaki komutları çalıştırmak için makinenizde bir komut istemi başlatabilirsiniz.
Aksi takdirde, tarayıcınızda bir [Azure Cloud Shell](https://shell.azure.com) penceresi açabilir ve komutları burada çalıştırabilirsiniz.

1. İlk olarak, şu komutu çalıştırarak Azure 'da uygun kimlik bilgileriyle oturum açtığınızdan emin olun:

    ```azurecli-interactive
    az login
    ```

1. Daha sonra, Azure dijital TWINS hizmetine erişimi olan bir taşıyıcı belirteci almak için [az Account Get-Access-Token](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest&preserve-view=true#az_account_get_access_token) komutunu kullanın.

    ```azurecli-interactive
    az account get-access-token --resource 0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    ```

1. Sonuç olarak değerini kopyalayın `accessToken` ve sonraki bölümde kullanmak üzere kaydedin. Bu, isteklerinizin kimliğini doğrulamak için Postman 'a sağlayacağınızı **belirtebilmeniz gereken belirteç değeridir** .

    :::image type="content" source="media/how-to-use-postman/console-access-token.png" alt-text="Az Account Get-Access-Token komutunun sonucunu gösteren bir yerel konsol penceresinin görünümü. Sonuç içindeki alanlardan birine accessToken, örnek değeri ise Ey ile başlayan bir değer vurgulanır.":::

>[!TIP]
>Bu belirteç en az beş dakika ve en fazla 60 dakika için geçerlidir. Geçerli belirteç için ayrılan süre dışında çalıştırırsanız, yeni bir tane almak için bu bölümdeki adımları tekrarlayabilirsiniz.

## <a name="set-up-postman-collection-and-authorization"></a>Postman toplamayı ve yetkilendirmeyi ayarlama

Sonra, API istekleri oluşturmak için Postman 'ı ayarlayın.
Bu adımlar yerel Postman uygulamanızda gerçekleşir, bu nedenle devam edin ve bilgisayarınızda Postman uygulamasını açın.

### <a name="create-a-postman-collection"></a>Postman koleksiyonu oluşturma

Postman 'daki istekler **koleksiyonlara** (istek grupları) kaydedilir. İsteklerinizi gruplamak için bir koleksiyon oluşturduğunuzda, aynı anda birçok isteğe ortak ayarları uygulayabilirsiniz. Bu, Azure dijital TWINS API 'Lerinde birden fazla istek oluşturmayı planlıyorsanız, tüm koleksiyon için yalnızca bir kez kimlik doğrulaması yapılandırmanız gerektiği için Yetkilendirmeyi büyük ölçüde basitleştirebilir.

1. Bir koleksiyon oluşturmak için *+ Yeni koleksiyon* düğmesine basın.

    :::image type="content" source="media/how-to-use-postman/postman-new-collection.png" alt-text="Yeni açılan Postman penceresinin görünümü. ' Yeni koleksiyon ' düğmesi vurgulandı":::

1. Aşağıdaki *yenı koleksiyon oluştur* penceresinde koleksiyonunuz Için bir **ad** ve isteğe bağlı bir **Açıklama** sağlayın.

Daha sonra, yetkilendirme için koleksiyona bir taşıyıcı belirteci eklemek üzere bir sonraki bölüme devam edin.

### <a name="add-authorization-token-and-finish-collection"></a>Yetkilendirme belirteci ve bitiş koleksiyonu Ekle

1. *Yenı koleksiyon oluştur* Iletişim kutusunda *Yetkilendirme* sekmesine geçin. Bu, topladığınız **belirteç değerini** , koleksiyonunuzdaki tüm API istekleri için kullanmak üzere, [taşıyıcı belirtecini al](#get-bearer-token) bölümünde yerleştireceğiniz yerdir.

    :::image type="content" source="media/how-to-use-postman/postman-authorization.png" alt-text="' Yetkilendirme ' sekmesini gösteren ' yenı koleksıyon oluştur ' Postman penceresi.":::

1. *Türü* _**OAuth 2,0**_ olarak ayarlayın ve erişim belirtecinizi *erişim belirteci* kutusuna yapıştırın.

    :::image type="content" source="media/how-to-use-postman/postman-paste-token.png" alt-text="' Yetkilendirme ' sekmesini gösteren ' yenı koleksıyon oluştur ' Postman penceresi. ' OAuth 2,0 ' türü seçilidir ve erişim belirteci değerinin yapıştırılabileceği erişim belirteci kutusu vurgulanır.":::

1. Taşıyıcı belirtecinize yapıştırdıktan sonra, koleksiyonunuzu oluşturmayı tamamladıktan sonra *Oluştur* ' a basın.

Yeni koleksiyonunuzda artık *koleksiyonlar* altında ana Postman görünüminizden görünebilirler.

:::image type="content" source="media/how-to-use-postman/postman-post-collection.png" alt-text="Ana Postman penceresinin görünümü. Yeni oluşturulan koleksiyon ' koleksiyonlar ' sekmesinde vurgulanır.":::

## <a name="create-a-request"></a>İstek oluştur

Önceki adımları tamamladıktan sonra Azure Digital Ikizi API 'Lerine istek oluşturabilirsiniz.

1. İstek oluşturmak için *+ Yeni* düğmesine basın.

    :::image type="content" source="media/how-to-use-postman/postman-new-request.png" alt-text="Ana Postman penceresinin görünümü. ' Yeni ' düğmesi vurgulandı":::

1. *İstek* seçin.

    :::image type="content" source="media/how-to-use-postman/postman-new-request-2.png" alt-text="Yeni bir şey oluşturmak için seçebileceğiniz seçeneklerin görünümü. ' Istek ' seçeneği vurgulanır":::

1. Bu eylem, isteğiniz için bir ad girebileceğiniz *Istek kaydet* penceresini açar, isteğe bağlı bir açıklama verebilir ve bir parçası olan koleksiyonu seçebilir. Ayrıntıları girin ve isteği daha önce oluşturduğunuz koleksiyona kaydedin.

    :::row:::
        :::column:::
            :::image type="content" source="media/how-to-use-postman/postman-save-request.png" alt-text="Açıklanan alanları dolduracakları ' isteği Kaydet ' penceresinin görünümü. ' Azure dijital TWINS koleksiyonuna Kaydet ' düğmesi vurgulanır":::
        :::column-end:::
        :::column:::
        :::column-end:::
    :::row-end:::

Artık isteğinizi koleksiyon altında görüntüleyebilir ve düzenlenebilir ayrıntılarını almak için bu seçeneği belirleyebilirsiniz.

:::image type="content" source="media/how-to-use-postman/postman-request-details.png" alt-text="Ana Postman penceresinin görünümü. Azure dijital TWINS koleksiyonu genişletilir ve ' sorgu TWINS ' isteği vurgulanır. İsteğin ayrıntıları sayfanın merkezinde gösterilir." lightbox="media/how-to-use-postman/postman-request-details.png":::

### <a name="set-request-details"></a>İstek ayrıntılarını ayarla

Azure dijital TWINS API 'Lerinden birine bir Postman isteği yapmak için API 'nin URL 'SI ve gerektirdiği ayrıntıları hakkında bilgi almanız gerekir. Bu bilgileri [Azure Digital TWINS REST API başvuru belgelerinde](/rest/api/azure-digitaltwins/)bulabilirsiniz.

Örnek sorgu ile devam etmek için bu makalede sorgu API 'SI (ve [başvuru belgeleri](/rest/api/digital-twins/dataplane/query/querytwins)) kullanılarak bir örnekteki tüm dijital TWINS 'leri sorgulama yapılır.

1. Başvuru belgelerinden istek URL 'sini ve türünü alın. Sorgu API 'SI için bu *gönderi `https://digitaltwins-name.digitaltwins.azure.net/query?api-version=2020-10-31`* olur.
1. Postman 'da, istek için türü ayarlayın ve URL 'de yer tutucuları gerektiği şekilde doldurarak istek URL 'sini girin. Bunun nedeni, [*Önkoşul*](#prerequisites) bölümünde örneğinizin **ana bilgisayar adını** kullanacaksınız.
    
   :::image type="content" source="media/how-to-use-postman/postman-request-url.png" alt-text="Yeni isteğin ayrıntılarında, başvuru belgelerindeki sorgu URL 'SI istek URL 'si kutusuna doldurulmuştur." lightbox="media/how-to-use-postman/postman-request-url.png":::
    
1. *Params* sekmesindeki istek için gösterilen parametrelerin başvuru belgelerinde açıklananlar ile eşleşip eşleşmediğinden emin olun. Postman 'daki bu istek için, `api-version` önceki adımda istek URL 'si girildiğinde parametre otomatik olarak doldurulmuştur. Sorgu API 'SI için bu tek gerekli parametredir, bu nedenle bu adım yapılır.
1. *Yetkilendirme* sekmesinde, *türü* üst öğeden *kimlik doğrulamasını devralacak* şekilde ayarlayın. Bu, bu isteğin, daha önce tüm koleksiyon için ayarladığınız kimlik doğrulamasını kullanacağını gösterir.
1. *Üst bilgiler* sekmesindeki istek için gösterilen üst bilgilerin, başvuru belgelerinde açıklananlarla eşleştiğinden emin olun. Bu istek için, birkaç üst bilgi otomatik olarak doldurulmuştur. Sorgu API 'SI için üst bilgi seçeneklerinden hiçbiri gerekli değildir, bu nedenle bu adım yapılır.
1. *Gövde* sekmesindeki istek için gösterilen gövdenin, başvuru belgelerinde açıklanan gereksinimle eşleşip eşleşmediğini denetleyin. Sorgu API 'SI için, sorgu metnini sağlamak üzere bir JSON gövdesi gerekir. İşte, örnekteki tüm dijital TWINS sorgularını sorgulayan bu istek için örnek bir gövde:

   :::image type="content" source="media/how-to-use-postman/postman-request-body.png" alt-text="Yeni isteğin ayrıntılarında gövde sekmesi gösterilir. ' SELECT * FROM DIGITALTWıNS ' sorgusuyla bir ham JSON gövdesi içerir."lightbox="media/how-to-use-postman/postman-request-body.png":::

   Azure dijital TWINS sorguları oluşturma hakkında daha fazla bilgi için bkz. [*nasıl yapılır: ikizi grafiğini sorgulama*](how-to-query-graph.md).

1. İstek türü için gerekebilecek diğer alanlar için başvuru belgelerini denetleyin. Sorgu API 'SI için, tüm gereksinimler Postman isteğinde karşılanmıştır, bu nedenle bu adım yapılır.
1. Tamamlanan isteğinizi göndermek için *Gönder* düğmesini kullanın.
   :::image type="content" source="media/how-to-use-postman/postman-request-send.png" alt-text="Yeni isteğin ayrıntılarına yaklaşarak Gönder düğmesi vurgulanır." lightbox="media/how-to-use-postman/postman-request-send.png":::

İsteği gönderdikten sonra, yanıt ayrıntıları isteğin altındaki Postman penceresinde görüntülenir. Yanıtın durum kodunu ve tüm gövde metinlerini görüntüleyebilirsiniz.

:::image type="content" source="media/how-to-use-postman/postman-request-response.png" alt-text="Gönderilen isteğin ayrıntılarının altında yanıtın ayrıntıları vurgulanır. Sorgu tarafından döndürülen dijital TWINS 'i açıklayan 200 Tamam ve gövde metni durumu vardır." lightbox="media/how-to-use-postman/postman-request-response.png":::

Ayrıca, sonucu doğrulamak veya ortaya çıkan hatalar hakkında daha fazla bilgi edinmek için başvuru belgelerinde verilen beklenen yanıt verileriyle yanıtı karşılaştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Dijital TWINS API 'Leri hakkında daha fazla bilgi edinmek için, bkz. [*nasıl yapılır: Azure dijital TWINS API 'leri ve SDK 'Larını kullanma*](how-to-use-apis-sdks.md)ya da [REST API 'lerinin başvuru belgelerini](/rest/api/azure-digitaltwins/)görüntüleme.
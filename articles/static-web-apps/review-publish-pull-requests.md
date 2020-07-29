---
title: Azure statik Web Apps ön üretim ortamlarında çekme isteklerini gözden geçirin
description: Azure statik Web Apps çekme isteği değişikliklerini gözden geçirmek için üretim öncesi ortamların nasıl kullanılacağını öğrenin.
services: static-web-apps
author: sinedied
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: yolasors
ms.openlocfilehash: 61c5917c1e4cb9dbf96e90af9a30777ea7c2e66c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83597033"
---
# <a name="review-pull-requests-in-pre-production-environments-in-azure-static-web-apps-preview"></a>Azure Static Web Apps Önizlemesinde üretim öncesi ortamlarında çekme isteklerini gözden geçirme

Bu makalede, [Azure statik Web Apps](overview.md)ile dağıtılan uygulamalardaki değişiklikleri gözden geçirmek için üretim öncesi ortamların nasıl kullanılacağı gösterilmektedir.

Üretim öncesi (hazırlama) ortamı, uygulamanızın üretimde bulunmayan değişiklikler içeren tam işlevli hazırlanmış bir sürümüdür.

Azure static Web Apps, depoda bir GitHub eylemleri iş akışı oluşturur. İş akışının izleyen bir dala karşı bir çekme isteği oluşturulduğunda, üretim öncesi ortam oluşturulur. Üretim öncesi ortamı, uygulamanın aşamalarını, üretime göndermeden önce incelemeler gerçekleştirmenizi sağlar.

Birden çok üretim öncesi ortam, Azure statik Web Apps kullanırken aynı anda birlikte bulunabilir. İzlenen dalda bir çekme isteği oluşturduğunuzda, yaptığınız değişikliklerle hazırlanan bir sürüm ayrı bir üretim öncesi ortama dağıtılır.

Üretim öncesi ortamları kullanmanın birçok avantajı vardır. Örneğin, şunları yapabilirsiniz:

- Üretim ve hazırlama arasındaki görsel değişiklikleri gözden geçirin. Örneğin, içerik ve düzen güncelleştirmelerini görüntüleme.
- Takımınızda yapılan değişiklikleri gösterir.
- Uygulamanızın farklı sürümlerini karşılaştırın.
- Kabul testlerini kullanarak değişiklikleri doğrulayın.
- Üretime dağıtım yapmadan önce tasdiklik denetimleri yapın.

> [!NOTE]
> Önizleme süresince, bir seferde [en fazla yalnızca bir hazırlık ortamına](quotas.md) izin verilir.

## <a name="prerequisites"></a>Ön koşullar

- Azure statik Web Apps ile yapılandırılmış mevcut bir GitHub deposu. Bkz. bir tane yoksa, [ilk statik uygulamanızı oluşturma](getting-started.md) .

## <a name="make-a-change"></a>Değişiklik yap

Deponuzda bir değişiklik yaparak başlayın. Aşağıdaki adımlarda gösterildiği gibi doğrudan GitHub üzerinde yapabilirsiniz.

1. GitHub 'daki projenizin deposuna gidin ve ardından **dal** düğmesine tıklayarak yeni bir dal oluşturun.

    :::image type="content" source="./media/review-publish-pull-requests/create-branch.png" alt-text="GitHub arabirimini kullanarak yeni dal oluşturma":::]

    Bir dal adı yazın ve **dal oluştur**' a tıklayın.

1. _Uygulama_ klasörünüze gidin ve metin içeriğini değiştirin. Örneğin, bir başlığı veya paragrafı değiştirebilirsiniz. Düzenlemek istediğiniz dosyayı bulduktan sonra, değişikliği yapmak için **Düzenle** ' ye tıklayın.

    :::image type="content" source="./media/review-publish-pull-requests/edit-file.png" alt-text="GitHub arabirimindeki dosya Düzenle düğmesi":::

1. Değişiklikleri yaptıktan sonra, yaptığınız değişiklikleri yürütmek için **Değişiklikleri Kaydet** ' e tıklayın.

    :::image type="content" source="./media/review-publish-pull-requests/commit-changes.png" alt-text="GitHub arabirimindeki değişiklikleri Yürüt düğmesi":::

## <a name="create-a-pull-request"></a>Çekme isteği oluşturma

Sonra, bu değişiklikten bir çekme isteği oluşturun.

1. GitHub 'da projenizin **çekme isteği** sekmesini açın:

    :::image type="content" source="./media/review-publish-pull-requests/tab.png" alt-text="GitHub deposundaki çekme isteği sekmesi":::

1. Dalınızın **& çekme Isteğini Karşılaştır** düğmesine tıklayın.

1. İsteğe bağlı olarak, yaptığınız değişikliklerle ilgili bazı ayrıntıları doldurarak **çekme Isteği oluştur**' a tıklayabilirsiniz.

    :::image type="content" source="./media/review-publish-pull-requests/open.png" alt-text="GitHub 'da çekme isteği oluşturma":::

Gözden geçirenler atayabilir ve gerekirse yaptığınız değişiklikleri tartışmak için yorum ekleyebilirsiniz.

> [!NOTE]
> Dalınıza yeni işlemeler göndererek birden fazla değişiklik yapabilirsiniz. Çekme isteği, tüm değişiklikleri yansıtacak şekilde otomatik olarak güncelleştirilir.

## <a name="review-changes"></a>Değişiklikleri gözden geçir

Çekme isteği oluşturulduktan sonra, [GitHub eylemleri](https://github.com/features/actions) dağıtım iş akışı çalışır ve yaptığınız değişiklikleri üretim öncesi bir ortama dağıtır.

İş akışı uygulamanızı oluşturmayı ve dağıtımını tamamladıktan sonra, GitHub bot, çekme isteğinize üretim öncesi ortamın URL 'sini içeren bir açıklama ekler. Hazırlanan değişikliklerinizi görmek için bu bağlantıya tıklayabilirsiniz.

:::image type="content" source="./media/review-publish-pull-requests/bot-comment.png" alt-text="Ön üretim URL 'SI ile çekme isteği yorumu":::

Değişiklikleri görmek için oluşturulan URL 'ye tıklayın.

URL 'ye daha yakından bakıyorsanız, şöyle oluştuğunu görebilirsiniz: `https://<SUBDOMAIN-PULL_REQUEST_ID>.<AZURE_REGION>.azurestaticapps.net` .

Belirli bir çekme isteği için yeni güncelleştirmeler gönderseniz bile URL aynı kalır. URL 'nin sabitine ek olarak, çekme isteğinin ömrü için aynı üretim öncesi ortam yeniden kullanılır.

## <a name="publish-changes"></a>Değişiklikleri yayımlama

Değişiklikler onaylandığında, çekme isteğini birleştirerek yaptığınız değişiklikleri üretime yayımlayabilirsiniz.

**Birleştirme çekme isteğine**tıklayın:

:::image type="content" source="./media/review-publish-pull-requests/merge.png" alt-text="GitHub arabirimindeki çekme isteğini birleştirme düğmesi":::

Birleştirme yaptığınız değişiklikleri izlenen dalda ("üretim" dalı) kopyalar. Ardından, dağıtım iş akışı izlenen dalda başlatılır ve değişiklikler uygulamanız yeniden oluşturulduktan sonra canlı bir uygulamadır.

Üretimdeki değişiklikleri doğrulamak için, Web sitesinin Live sürümünü yüklemek üzere üretim URL 'nizi açın.

## <a name="limitations"></a>Sınırlamalar

GitHub deponuz özel olsa bile, uygulamanızın hazırlanan sürümlerinin URL 'SI tarafından şu anda herkese açık bir şekilde erişilebilir olması gerekir.

> [!WARNING]
> Üretim öncesi ortamlara erişim sınırlı olmadığından, hazırlanan sürümlere hassas içerik yayımlarken dikkatli olun.

Statik Web Apps dağıtılan her bir uygulama için kullanılabilir olan üretim öncesi ortamların sayısı, kullanmakta olduğunuz SKU katmanına bağlıdır. Örneğin, ücretsiz katmanla birlikte üretim ortamına ek olarak 1 üretim öncesi ortamınız olabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel etki alanı ayarlama](custom-domain.md)

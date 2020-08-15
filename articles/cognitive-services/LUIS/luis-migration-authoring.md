---
title: Azure Kaynak yazma anahtarına geçiş
titleSuffix: Azure Cognitive Services
description: Bu makalede, Language Understanding (LUSıS) yazma kimlik doğrulamasının bir e-posta hesabından Azure kaynağına geçirilmesi açıklanmaktadır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: how-to
ms.date: 08/13/2020
ms.author: diberry
ms.openlocfilehash: 4c9dc04770e5fadd72c5460a4b44c05ffda47cb7
ms.sourcegitcommit: c293217e2d829b752771dab52b96529a5442a190
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2020
ms.locfileid: "88245460"
---
# <a name="migrate-to-an-azure-resource-authoring-key"></a>Azure Kaynak yazma anahtarına geçiş

Language Understanding (LUSıS) yazma kimlik doğrulaması, bir e-posta hesabından Azure kaynağına değişti. Şu anda gerekli olmasa da, bir Azure kaynağına geçiş daha sonra zorlanabilir.


## <a name="what-is-migration"></a>Geçiş nedir?

Geçiş, bir e-posta hesabından Azure kaynağına yazma kimlik doğrulamasını değiştirme işlemidir. Hesabınız, geçiş işleminden sonra bir Azure aboneliğine ve bir Azure yazma kaynağına bağlanır. *Tüm LUYA kullanıcıları (sahipler veya ortak çalışanlar) sonunda geçiş yapması gerekecektir.*

Geçişin, LUıN portalından gerçekleştirilmesi gerekiyor. Örneğin, LUSıS CLı 'yı kullanarak yazma anahtarlarını oluşturursanız, HALSıS portalındaki geçiş işlemini gerçekleştirmeniz gerekir. Geçiş işleminden sonra uygulamalarınızda hala coyazarlarınız olabilir, ancak bunlar uygulama düzeyi yerine Azure Kaynak düzeyine eklenecektir.

> [!Note]
> Geçişten önce, coyazarlar, LUSıS uygulama düzeyinde _ortak çalışanlar_ olarak bilinir. Geçişten sonra, Azure Kaynak düzeyinde aynı işlevsellik için _katılımcı_ Azure rolü kullanılır.

## <a name="note-before-you-migrate"></a>Geçirmeden önce göz önüne alın

* Yazma deneyiminizi **Kasım, 2, 2020 '** ye geçirmeniz gerekir. 
* Geçiş tek yönlü bir işlemdir. Geçiş yaptıktan sonra geri gidemezsiniz.
* Uygulama sahibiyseniz uygulamalar sizinle otomatik olarak geçiş yapılır.
* Sahip, geçirilecek uygulamaların bir alt kümesini seçemez ve işlem geri alınamaz.
* Uygulamalar, sahip geçirildikten sonra ortak çalışanın tarafında kaybolacaktır.
* Sahipleri, geçişe bildirmek üzere ortak kişilere e-posta göndermeleri istenir.
* Uygulama üzerinde ortak çalışan bir çalışmasanız, uygulamalar sizinle geçiş uygulamacaktır.
* Bir sahibin, ortak çalışanların geçirildiğini bilmesi için bir yol yoktur.
* Geçiş, otomatik olarak ortak bir şekilde toplanmaz ve Azure yazma kaynağına taşıyabilir veya eklemez. Uygulama sahibi, geçişten sonra bu adımı tamamlaması gereken bir uygulamadır. Bu adım [, Azure yazma kaynağı için izinler](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-collaborate)gerektirir.
* Azure kaynağına ortak çalışanlar atandıktan sonra, uygulamalara erişmek için geçirilmesi gerekir. Aksi takdirde, uygulamaları yazmak için erişimleri olmaz.
* Geçirilen bir Kullanıcı, uygulamanın ortak çalışan olarak eklenemez.
* Başka bir kullanıcının sahip olduğu uygulamalara atanan tahmin anahtarlarınız varsa, bu, hem sahip hem de ortak çalışan için geçişi engeller. Bu makalenin ilerleyen kısımlarındaki önerilere bakın.

> [!Note]
> Bir tahmin çalışma zamanı kaynağı oluşturmanız gerekiyorsa, bunu oluşturmak için [ayrı bir işlem](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) vardır.

## <a name="migration-prerequisites"></a>Geçiş önkoşulları

* Geçerli bir Azure aboneliğiyle ilişkilendirilmesi gerekir. Kiracı yöneticinizden aboneliğinizi size eklemesini isteyin veya [ücretsiz bir abonelik için kaydolun](https://azure.microsoft.com/free/cognitive-services).
* LUSıS portalından veya Azure portal bir LUSıS Azure yazma kaynağı oluşturmanız gerekir. LUO portalından bir yazma kaynağı oluşturma, sonraki bölümde ele alınan geçiş akışının bir parçasıdır.
* Uygulamalar üzerinde ortak çalışan değilseniz, uygulamalar otomatik olarak geçirilmez. Bu uygulamaları dışa aktararak veya [dışa aktarma API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40)'sini kullanarak yedeklemenizi öneririz. Geçişten sonra uygulamayı LUO 'ya geri aktarabilirsiniz. İçeri aktarma işlemi, sahip olduğunuz yeni bir uygulama KIMLIĞIYLE yeni bir uygulama oluşturur.
* Uygulamanın sahibiyseniz, otomatik olarak geçiş yapacağından uygulamalarınızı dışarı aktarmanız gerekmez. Uygulamanın ortak çalışan listesini kaydetmenizi öneririz. Bu listeye sahip e-posta şablonu, isteğe bağlı olarak geçiş işleminin bir parçası olarak sağlanır.


|Portal|Amaç|
|--|--|
|[Azure](https://azure.microsoft.com/free/cognitive-services)| Tahmin ve yazma kaynakları oluşturun.<br> Kaynaklara katkıda bulunanlar atayın.|
|[LUIS](https://www.luis.ai)| Yeni yazma kaynaklarına geçiş yapın.<br> Geçiş akışında yeni yazma kaynakları oluşturun.<br> **Manage**  >  **Azure kaynaklarını** Yönet sayfasından, tahmine ve yazma kaynaklarını uygulamalara atayın veya atamayı kaldırma. <br> Uygulamaları bir yazma kaynağından diğerine taşıyın.  |

> [!Note]
> F0 katmanında gösterildiği gibi, LUSıS uygulamanızı yazma işlemi ücretsizdir. [Fiyatlandırma katmanları hakkında daha fazla](luis-limits.md#key-limits)bilgi edinin.


## <a name="migration-steps"></a>Geçiş adımları

1. Üzerinde çalıştığınız LUU portalında, en üstteki araç çubuğundaki **Azure** simgesinden geçiş işlemini başlatabilirsiniz.

   > [!div class="mx-imgBorder"]
   > ![Geçiş simgesi](./media/migrate-authoring-key/migration-button.png)

2. Geçiş açılır penceresi geçişe devam etmenize veya daha sonra geçiş yapmanıza olanak sağlar. **Şimdi geçir**' i seçin.

   > [!div class="mx-imgBorder"]
   > ![Geçiş işleminde, şimdi geçir ' i seçtiğiniz ilk açılan pencere.](./media/migrate-authoring-key/prompt-when-migrating-2.png)

3. Uygulamalarınızdan herhangi birine ortak çalışanlar varsa, onlara geçiş hakkında bilgi veren bir e-posta göndermeniz istenir. Bu, isteğe bağlı bir adımdır.

   Her ortak çalışan ve uygulama için varsayılan e-posta uygulaması, hafif biçimli bir e-posta ile açılır. Göndermeden önce e-postayı düzenleyebilirsiniz. E-posta şablonu, tam uygulama KIMLIĞI ve uygulama adını içerir.

   ```html
   Dear Sir/Madam,

   I will be migrating my LUIS account to Azure. Consequently, you will no longer have access to the following app:

   App Id: <app-ID-omitted>
   App name: Human Resources

   Thank you
   ```

   > [!Note]
   > Hesabınızı Azure 'a geçirdikten sonra uygulamalarınız artık ortak olmayan uygulamalar tarafından kullanılamaz.

4. Herhangi bir uygulamada ortak çalışan değilseniz, geçiş akışı sırasında bu seçeneği belirleyerek uygulamaların bir kopyasını dışarı aktarmanız istenir. Bu, isteğe bağlı bir adımdır.

   Seçeneğini belirlerseniz, aşağıdaki sayfa görüntülenir. İstediğiniz uygulamaları dışa aktarmak için soldaki indir düğmelerini seçin. Bu uygulamaları geçirdikten sonra geri aktarabilirsiniz, çünkü bu uygulamalar sizinle otomatik olarak geçirilmeyecektir.

   > [!div class="mx-imgBorder"]
   > ![Uygulamalarınızı dışarı aktarmak için istemde bulun.](./media/migrate-authoring-key/export-app-for-collabs-2.png)

5. Azure 'dan zaten bir tane oluşturduysanız, yeni bir LUYA yazma kaynağı oluşturmayı veya var olan bir yazma kaynağına geçiş yapabilirsiniz. Aşağıdaki düğmelerden birini seçerek istediğiniz seçeneği belirleyin.

   > [!div class="mx-imgBorder"]
   > ![Yeni bir yazma kaynağı oluşturma ve var olan bir yazma kaynağı kullanma düğmeleri](./media/migrate-authoring-key/choose-existing-authoring-resource.png)

### <a name="create-new-authoring-resource-from-luis-to-migrate"></a>Geçiş için LUSıS 'den yeni yazma kaynağı oluşturma

Yeni bir yazma kaynağı oluşturmak istiyorsanız **Yeni yazma kaynağı oluştur** ' u seçin ve sonraki pencerede aşağıdaki bilgileri sağlayın. Ardından **Bitti**'yi seçin.

> [!div class="mx-imgBorder"]
> ![Yazma kaynağı oluşturmaya yönelik pencere](./media/migrate-authoring-key/create-new-authoring-resource-2.png)

* **Kiracı adı**: Azure aboneliğinizin ilişkilendirildiği kiracı. Bu, varsayılan olarak, kullanmakta olduğunuz kiracıya ayarlanır. Baş harflerinizi içeren en sağdaki avatarını seçerek kiracılar arasında geçiş yapabilirsiniz.
* **Kaynak adı**: seçtiğiniz özel bir ad. Yazma ve tahmin uç noktası sorgularınız için URL 'nin bir parçası olarak kullanılır.
* **Abonelik adı**: kaynakla ilişkilendirilecek abonelik. Kiracınıza ait birden fazla aboneliğiniz varsa, açılan listeden istediğiniz birini seçin.
* **Azure Kaynak grubu adı**: açılan listeden seçtiğiniz özel bir kaynak grubu adı. Kaynak grupları, erişim ve yönetim için Azure kaynaklarını gruplandıreklemenize olanak tanır.

Her bir abonelik için bölge başına 10 ücretsiz yazma kaynağınız olabileceğini unutmayın. Aboneliğiniz aynı bölgede 10 ' dan fazla yazma kaynağına sahipse yeni bir tane oluşturamazsınız.

Yazma kaynağı oluşturulduğunda, başarı iletisi gösterilir. Açılır pencereyi kapatmak için **Kapat** ' ı seçin.

  > [!div class="mx-imgBorder"]
  > ![Yazma kaynağınızın başarıyla oluşturulduğunu belirten ileti](./media/migrate-authoring-key/migration-success-2.png)


### <a name="use-existing-authoring-resource-to-migrate"></a>Geçirmek için mevcut yazma kaynağını kullanın

Aboneliğiniz zaten bir LUSıS Authoring Azure kaynağıyla ilişkili ise veya Azure portal bir kaynak oluşturduysanız ve yeni bir tane oluşturmak yerine buna geçiş yapmak istiyorsanız, **mevcut yazma kaynağını kullan**' ı seçin. Sonraki pencerede aşağıdaki bilgileri sağlayın ve ardından **bitti**' yi seçin.

> [!div class="mx-imgBorder"]
>![Mevcut bir yazma kaynağını değiştirme penceresi](./media/migrate-authoring-key/choose-existing-authoring-resource-2.png)

* **Kiracı adı**: Azure aboneliğinizin ilişkilendirildiği kiracı. Bu, varsayılan olarak, kullanmakta olduğunuz kiracıya ayarlanır.
* **Abonelik adı**: kaynakla ilişkilendirilecek abonelik. Kiracınıza ait birden fazla aboneliğiniz varsa, açılan listeden istediğiniz birini seçin.
* **Kaynak adı**: geçiş yapmak istediğiniz yazma kaynağı.

> [!Note]
> Yazma kaynağınızı aşağı açılan listede göremiyorsanız, oturum açtığınız LUE portalına göre doğru konumda oluşturulmuş olduğunuzdan emin olun. Ayrıca, oluşturduğunuz öğenin bir tahmin kaynağı değil, yazma kaynağı olduğundan emin olun.


Yazma kaynak adınızı doğrulayıp **geçir** düğmesini seçin.

> [!div class="mx-imgBorder"]
> ![Geçiş düğmesi artık kullanılabilir olan bir yazma kaynağı seçme penceresi](./media/migrate-authoring-key/choose-authoring-resource-and-migrate-2.png)

Başarılı iletisi görüntülenir. Açılır pencereyi kapatmak için **Kapat** ' ı seçin.

> [!div class="mx-imgBorder"]
> ![Yazma kaynağınızın başarıyla geçirildiğini belirten ileti](./media/migrate-authoring-key/migration-success-2.png)

## <a name="using-apps-after-migration"></a>Geçişten sonra uygulamaları kullanma

Geçiş işleminden sonra, sahibi olduğunuz tüm LUO uygulamalarınız artık tek bir LUıN yazma kaynağına atanır.

**Uygulamalarım** listesinde, yeni yazma kaynağına geçirilen uygulamalar gösterilir. Uygulamalarınıza erişmeden önce, yazarlarınızın bulunduğu uygulamaları görmek için abonelik ve LUSıS yazma kaynağını seçin.

 > [!div class="mx-imgBorder"]
 > ![Abonelik ve yazma kaynağı için kutular](./media/create-app-in-portal-select-subscription-luis-resource.png)

Uygulamalarınızı LUE portalında düzenlemeyle çalışmaya devam etmek için yazma kaynağının anahtarını bilmeniz gerekmez.

Uygulamalarınızı programlama yoluyla düzenlemeyi planlıyorsanız, yazma anahtarı değerleri gerekir. Bu değerler, **Manage**  >  LUA portalındaki**Azure kaynaklarını** Yönet sayfasında görüntülenir. Bunlar ayrıca kaynağın **anahtarlar** sayfasındaki Azure Portal de mevcuttur. Ayrıca, daha fazla yazma kaynağı oluşturabilir ve bunları aynı sayfadan atayabilirsiniz.

 > [!div class="mx-imgBorder"]
 > ![Yazma kaynaklarını yönetmek için sayfa](./media/migrate-authoring-key/manage-authoring-resource-2.png)

## <a name="adding-contributors-to-authoring-resources"></a>Yazma kaynaklarına katkıda bulunanlar ekleme

[!INCLUDE [Manage contributors for the Azure authoring resource for language understanding](./includes/manage-contributors-authoring-resource.md)]

Yazma kaynağınız üzerinde [katkıda bulunanlar eklemeyi](luis-how-to-collaborate.md) öğrenin. Katkıda bulunanlar, bu kaynaktaki tüm uygulamalara erişebilir.

Kaynak için **Access Control (IAM)** sayfasında Azure Portal yazma kaynağına katkıda bulunanlar ekleyebilirsiniz. Daha fazla bilgi için bkz. [katılımcı erişimi ekleme](luis-migration-authoring-steps.md#after-the-migration-process-add-contributors-to-your-authoring-resource).

> [!Note]
> LUSıS uygulamasının sahibi geçirilmiş ve Azure kaynağına katkıda bulunan çalışanı olarak eklediyseniz, ortak çalışan, aynı zamanda geçirilmediği sürece uygulamaya erişemez.

## <a name="luis-portal-migration-reminders"></a>LUSıS portalı geçiş anımsatıcıları

[LUA portalı](https://www.luis.ai) , geçiş işlemini sağlar.

Bu koşulların her ikisi de doğruysa geçiş yapmanız istenir:
* Yazma için e-posta kimlik doğrulama sisteminde uygulamalarınız var.
* Uygulama sahibisiniz.

Haftalık olarak, uygulamalarınızı geçirmeniz istenir. Geçirmeden bu pencereyi kapatabilirsiniz. Bir sonraki zamanlanan dönemden önce geçiş yapmak istiyorsanız, LUO portalının üst araç çubuğundaki **Azure** simgesinden geçiş işlemini başlatabilirsiniz.

## <a name="prediction-resources-blocking-migration"></a>Tahmin kaynakları geçişi engelliyor
Geçişiniz herhangi bir uygulamanın çalışma zamanını olumsuz etkiler. Geçiş yaptığınızda, tüm ortak çalışanlar uygulamalarınızdan kaldırılır ve diğer uygulamalardan ortak çalışan olarak kaldırılır. Bu işlem, bir ortak çalışan tarafından oluşturulan anahtarların de kaldırılabileceği anlamına gelir. Bu, üretim süreciyle uygulamanızı bozabilirler. Bu, bunlara atanan ortak çalışanları veya anahtarları el ile kaldırana kadar geçişi engellemenin nedenidir.

Bu koşullardan biri doğru ise geçiş engellenir:

* Sahip olmadığınız uygulamalarda tahmin/çalışma zamanı kaynakları atadınız.
* Size ait olan uygulamalara tahmin/çalışma zamanı kaynakları atayan başka kullanıcılarınız vardır.

### <a name="recommended-steps-if-youre-the-owner-of-the-app"></a>Uygulamanın sahibiyseniz önerilen adımlar
Bazı uygulamaların sahibiyseniz ve bu uygulamalara bir tahmin/çalışma zamanı anahtarı atanmışsa, geçiş yaptığınızda bir hata oluşur. Hata, başka kullanıcılara ait olan tahmin anahtarlarının atandığı uygulama kimliklerini listeler.

Şunları yapmanızı öneririz:
* Geçiş hakkındaki ortak çalışanları bilgilendirin.
* Hatada gösterilen uygulamalardan tüm ortak çalışanları kaldırın.
* Ortak çalışanları el ile kaldırdığınızda başarılı olması gereken geçiş sürecini daha az yapın.
* Yeni yazma kaynağınız için ortak çalışanlar katkıda bulunan olarak atayın. Ortak çalışanlar, tahmine kaynakları geçirilir ve uygulamalara geri atanır. Bu, tahmin kaynakları yeniden atanana kadar uygulamada geçici olarak bir kesintiye neden olacağını unutmayın.

Burada başka bir olası çözüm vardır. Sahip geçişten önce, ortak çalışanlar uygulama sahiplerini Azure portal Azure aboneliklerinde katkıda bulunanlar olarak ekleyebilir. Bu adım, çalışma zamanı tahmin kaynağına sahip erişimine izin verir. Sahip, eklendiği yeni aboneliği (yeni bir kiracı altında bulunur) kullanarak geçerse, bu adım hem ortak çalışan hem de uygulama sahibi için geçiş işleminin engelini kaldırın. Ayrıca, uygulamanın sorunsuz bir şekilde geçirilmesine izin verir ve tahmin anahtarı yine de uygulamaları bozmayan uygulamalara atanır.


### <a name="recommended-steps-if-youre-a-collaborator-on-an-app"></a>Uygulama üzerinde ortak çalışan olduğunuzda önerilen adımlar
Uygulamalar üzerinde işbirliği yapıyorsanız ve bu uygulamalara bir tahmin/çalışma zamanı anahtarı atadıktan sonra geçirdiğinizde bir hata görüntülenir. Hata, geçişi engelleyen uygulama kimliklerini ve anahtar yollarını listeler.

Şunları yapmanızı öneririz:
* Uygulamaları bir yedekleme olarak dışarı aktarın. Bu, geçiş işleminde isteğe bağlı bir adımdır.
* **Manage**  >  **Azure kaynaklarını** Yönet sayfasından tahmin kaynaklarının atamasını kaldırma.
* Geçiş işlemini daha az yapın.
* Geçişten sonra uygulamaları geri alın.
* **Manage**  >  **Azure kaynaklarını** Yönet sayfasından, tahmin anahtarlarını uygulamalarınıza yeniden atayın.

> [!Note]
> Taşıdıktan sonra uygulamalarınızı geri aktardığınızda farklı uygulama kimliklerine sahip olurlar. Ayrıca, üretimde vurdıklardan farklı olacak. Artık bu uygulamaların sahibi olursunuz.

## <a name="troubleshooting-the-migration-process"></a>Geçiş işlemi sorunlarını giderme

Geçiş yapmayı denediğinizde, ancak açılan listede Azure aboneliğinizi bulamıyorsanız:
* Bilişsel hizmetler kaynakları oluşturma yetkisine sahip geçerli bir Azure aboneliğiniz olduğundan emin olun. [Azure Portal](https://ms.portal.azure.com) gidin ve aboneliğin durumunu denetleyin. Bir tane yoksa, [ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free/cognitive-services/).
* Geçerli aboneliğinizle ilişkili doğru kiracıda olduğunuzdan emin olun. Bu araç çubuğunda, Kiracılarınızın Avatarınızın soluna kadar olan kiracılar arasında geçiş yapabilirsiniz: bir ![ araç çubuğu, kiracılar arasında geçiş yapabilirsiniz](./media/migrate-authoring-key/switch-user-tenant-2.png)

Var olan bir yazma kaynağınız varsa ancak **mevcut yazma kaynağını kullan** seçeneğini belirlediğinizde bu kaynağı bulamıyorsanız:
* Kaynağınız, oturum açtığınız portaldan farklı bir konumda oluşturulmuş olabilir. [Lusıs yazma bölgelerini ve portallarını](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-authoring-regions)denetleyin.
* Bunun yerine LUO portalından yeni bir kaynak oluşturun.

**Yeni yazma kaynağı oluştur** seçeneğini belirlerseniz geçiş işlemi "kullanıcının Azure bilgileri alınamadı, daha sonra yeniden deneyin" hata iletisiyle başarısız olur.
* Aboneliğiniz, her abonelik için bölge başına 10 veya daha fazla yazma kaynağına sahip olabilir. Bu durumda, yeni bir yazma kaynağı oluşturamayacak.
* **Mevcut yazma kaynağını kullan** seçeneğini belirleyerek ve aboneliğinizdeki mevcut kaynaklardan birini seçerek geçiş yapın.

Aşağıdaki hatayı görürseniz, [uygulamanın sahibiyseniz önerilen adımları](#recommended-steps-if-youre-the-owner-of-the-app)denetleyin.
![Sahipler için geçişin başarısız olduğunu gösteren hata](./media/migrate-authoring-key/migration-failed-for-owner-2.png)

Aşağıdaki hatayı görürseniz, [bir uygulamada ortak çalışan kullanıyorsanız önerilen adımları](#recommended-steps-if-youre-a-collaborator-on-an-app)denetleyin.
![Ortak çalışanlar için geçişin başarısız olduğunu gösteren hata](./media/migrate-authoring-key/migration-failed-for-collab-2.png)


## <a name="next-steps"></a>Sonraki adımlar

* [Yazma ve çalışma zamanı anahtarları hakkındaki kavramları](luis-how-to-azure-subscription.md)gözden geçirin.
* [Anahtar atamayı](luis-how-to-azure-subscription.md) ve [katkıda bulunanlar eklemeyi](luis-how-to-collaborate.md)inceleyin.

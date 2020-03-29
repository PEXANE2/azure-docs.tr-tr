---
title: Gözden Geçirme araç ayarlarını yapılandır - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: İçerik Moderatörü için ekibinizi, etiketlerinizi, bağlayıcılarınızı, iş akışlarını ve kimlik bilgilerini yapılandırmak veya almak için Gözden Geçirme aracını kullanın.
services: cognitive-services
author: PatrickFarley
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/15/2019
ms.author: pafarley
ms.openlocfilehash: 2d685683bdc359b31a5a6c550c19e8c0d858f12a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220132"
---
# <a name="configure-the-review-tool"></a>Gözden Geçirme aracını yapılandırma

[Gözden Geçirme aracı,](https://contentmoderator.cognitive.microsoft.com) panodaki **Ayarlar** menüsünden erişebileceğiniz birkaç önemli özelliğe sahiptir.

![İçerik Moderatör İnceleme çok ayarlar menüsü](images/settings-1.png)

## <a name="manage-team-and-subteams"></a>Ekibi ve alt takımları yönetme

**Takım** sekmesi, belirli insan&mdash; [incelemeleri](../review-api.md#reviews) başlatıldığında haberdar edilebilen ekip ve alt kullanıcı gruplarını yönetmenize olanak tanır. Yalnızca bir takımınız olabilir (Gözden Geçirme aracına kaydolduğunuzda oluşturduğunuz) ancak birden çok alt takım oluşturabilirsiniz. Takım yöneticisi üyeleri davet edebilir, izinlerini ayarlayabilir ve farklı alt takımlara atayabilir.

![Araç ekibi ayarlarını gözden geçirme](images/settings-2-team.png)

Alt ekipler, belirli içerik kategorilerini gözden geçirmeye adanmış yükseltme ekipleri veya ekipler oluşturmak için yararlıdır. Örneğin, daha fazla inceleme için yetişkinlere uygun içeriği ayrı bir ekibe gönderebilirsiniz.

Bu bölümde, alt takımların nasıl oluşturulup anında inceleme atayılabildiğiniz açıklanmaktadır. Ancak, belirli ölçütlere göre incelemeler atamak için [İş Akışlarını](workflows.md) kullanabilirsiniz.

### <a name="create-a-subteam"></a>Alt takım oluşturma

**Alt Takımlar** bölümüne gidin ve Alt **Takım Ekle'yi**tıklatın. Alt takım adınızı iletişim kutusuna girin ve **Kaydet'i**tıklatın.

![Alt Takım Adı](images/1-Teams-2.PNG)

#### <a name="invite-teammates"></a>Takım arkadaşlarını davet et

Varsayılan takımın zaten bir üyesi değilse, bir alt takıma birini atayamazsınız, bu nedenle önce varsayılan takıma gözden geçirenleri eklemeniz gerekir. **Takım** sekmesinde **Davet et'i** tıklatın.

![Kullanıcıları davet etme](images/invite-users.png)

#### <a name="assign-teammates-to-subteam"></a>Alt takıma takım arkadaşları atama

Varsayılan ekibinizden bir veya daha fazla alt takıma üye atamak için **Üye Ekle** düğmesini tıklatın. Varolan kullanıcıları yalnızca bir alt takıma ekleyebilirsiniz. İnceleme aracında olmayan yeni kullanıcılar eklemek için, Takım Ayarları sayfasındaki "Davet Et" düğmesini kullanarak onları davet edin.

![Alt takım üyelerini atama](images/1-Teams-3.PNG)

### <a name="assign-reviews-to-subteams"></a>Alt takımlara yorum atama

Alt takımlarınızı oluşturduktan ve üyeleri atadıktan sonra, bu alt takımlara içerik [incelemeleri](../review-api.md#reviews) atamaya başlayabilirsiniz. Bu, sitenin **Gözden Geçirme** sekmesinden yapılır.
Bir alt takıma içerik atamak için sağ üst köşedeki elipsleri tıklatın, **"Taşı"** seçeneğini belirleyin ve bir alt takım seçin.

![Alt takıma resim incelemesi atama](images/3-review-image-subteam-1.png)

### <a name="switch-between-subteams"></a>Alt takımlar arasında geçiş

Birden fazla alt takımın üyesiyseniz, sizin için hangi içerik yorumlarının görüntülendiğini değiştirmek için bu alt takımlar arasında geçiş yapabilirsiniz. Gözden **Geçir** sekmesinde, **Varsayılan** etiketli açılır menüyü seçin ve **Alt Takımı Seçin'i**seçin. Farklı alt takımların içerik incelemelerini, ancak yalnızca sizin üye olduğunuz kişileri görüntüleyebilirsiniz.

![Alt takımlar arasında geçiş](images/3-review-image-subteam-2.png)

## <a name="tags"></a>Etiketler

**Etiketler** sekmesi, iki varsayılan ılımlılık etiketleri&mdash;**isadult** (**a**) ve **isracy** (**r)** ek olarak özel ılımlılık etiketleri tanımlamanızı sağlar. Özel bir etiket oluşturduğunuzda, varsayılan etiketlerin yanında incelemelerde kullanılabilir hale gelir. Görünürlük ayarlarını değiştirerek incelemelerde hangi etiketlerin görüneceğini değiştirebilirsiniz.

![Etiketler görünüm, "Görünür" onay kutuları da dahil olmak üzere](images/tags-4-disable.png)

### <a name="create-custom-tags"></a>Özel etiketler oluşturma

Yeni bir etiket oluşturmak için ilgili alanlara kısa bir kod, ad ve açıklama girmeniz gerekir.

- **Kısa kod**: Etiketiniz için iki harfli bir kod girin. Örnek: **cb**
- **Adı**: Boşluksuz küçük harfe kısa ve açıklayıcı bir etiket adı girin. Örnek: **isbullying**.
- **Açıklama**: (isteğe bağlı) Etiketinizin hedeflenedığı içeriğin açıklamasını girin. Örnek: **Resim veya siber zorbalık örnekleri.**

Etiket eklemek için **Ekle'yi** tıklatın ve etiket oluşturmayı bitirdiğinizde **Kaydet'i** tıklatın.

![İnceleme aracı yeni etiket iletişim kutusu oluşturma](images/settings-3-tags.png)

### <a name="delete-tags"></a>Etiketleri silme

Etiketler listesindeki girişlerinin yanındaki çöp kutusu simgesini seçerek özel etiketleri silebilirsiniz, ancak varsayılan etiketleri silemezsiniz.

## <a name="connectors"></a>Bağlayıcılar

**Bağlayıcılar** sekmesi, içeriği [iş akışlarının](../review-api.md#workflows)bir parçası olarak farklı şekillerde işleyebilen hizmete özel eklentiler olan bağlayıcılarınızı yönetmenize olanak tanır.

Bir iş akışı oluşturduğunuzda varsayılan bağlayıcı, içeriği **yetişkinlere uygun** veya **müstehcen**olarak işaretleyebilen, küfür bulabileceğiniz ve benzeri İçerik yöneticisi olan İçerik Moderatörü bağlayıcısıdır. Ancak, burada listelenen diğer bağlayıcıları, ilgili hizmetleri için kimlik bilgilerine sahip olduğunuz sürece (örneğin Yüz konektörünü kullanmak için bir [Yüz](https://docs.microsoft.com/azure/cognitive-services/face/overview) abonelik anahtarı almanız gerekir) kullanabilirsiniz.

[Gözden Geçirme aracı](./human-in-the-loop.md) aşağıdaki bağlayıcıları içerir:

- Duygu Tanıma
- Yüz
- PhotoDNA Bulut Hizmeti
- Metin Analizi

### <a name="add-a-connector"></a>Bağlayıcı ekleme

Bağlayıcı eklemek (ve içerik [iş akışlarında](../review-api.md#workflows)kullanıma hazır hale getirmek) için uygun **Bağlan** düğmesini seçin. Bir sonraki iletişim kutusunda, bu hizmet için abonelik anahtarınızı girin. Işiniz bittiğinde, yeni bağlayıcınız sayfanın üst kısmında görünmelidir.

![İçerik Moderatör Bağlayıcıları Ayarları](images/settings-4-connectors.png)

## <a name="workflows"></a>İş akışları

**İş akışları** [sekmesi, iş akışlarınızı](../review-api.md#workflows)yönetmenize olanak tanır. İş akışları içerik için bulut tabanlı filtrelerdir ve içeriği farklı şekillerde sıralamak ve uygun eylemleri yapmak için bağlayıcılarla birlikte çalışırlar. Burada, iş akışlarınızı tanımlayabilir, edinebilir ve sınayabilirsiniz. Bkz. İş akışlarını nasıl yapılacağını belirlemek için [tanımlayın ve kullanın.](Workflows.md)

![İçerik Moderatör İş Akışı Ayarları](images/settings-5-workflows.png)

## <a name="credentials"></a>Kimlik Bilgileri

**Kimlik Bilgileri** sekmesi, Bir REST çağrısından veya istemci SDK'dan herhangi bir Denetleme hizmetine erişmeniz gereken İçerik Moderatörabonelik anahtarınıza hızlı erişim sağlar.

![İçerik Moderatör Kimlik Bilgileri](images/settings-6-credentials.png)

### <a name="use-external-credentials-for-workflows"></a>İş akışları için dış kimlik bilgilerini kullanma

[İnceleme aracı,](https://contentmoderator.cognitive.microsoft.com) kaydolduğunuzda Azure İçerik Yöneticisi hizmetleri için ücretsiz bir deneme anahtarı oluşturur, ancak Azure hesabınızdaki varolan bir anahtarı kullanacak şekilde de yapılandırabilirsiniz. Ücretsiz deneme anahtarları sıkı kullanım limitlerine[(Fiyatlandırma ve sınırlar)](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/)sahip olduğundan, bu büyük ölçekli senaryolar için önerilir.

Azure'da bir [İçerik Moderatörü kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) oluşturduysanız, Azure portalında bu kaynakiçin gidin ve **Keys** bıçağını seçin. Anahtarlarından birini kopyala.

![Azure portalında İçerik Moderatör tuşları](images/credentials-azure-portal-keys.PNG)

Gözden [Geçirme aracının](https://contentmoderator.cognitive.microsoft.com) **Kimlik Bilgileri** sekmesinde, **İş Akışı Ayarları** bölmesine gidin, **Düzenleme'yi**seçin ve anahtarınızı **Ocp-Apim-Abonelik-Anahtar** alanına yapıştırın. Şimdi, ılımlılık API'lerini arayan iş akışları Azure kimlik bilgilerinizi kullanır.

> [!NOTE]
> **İş Akışı Ayarları** bölmesindeki diğer iki alan özel terim ve resim listeleri içindir. Bunlar hakkında bilgi edinmek için [Özel terimlere](../try-terms-list-api.md) veya [Özel resimler](../try-image-list-api.md) kılavuzlarına bakın.

### <a name="use-your-azure-account-with-the-review-apis"></a>Azure hesabınızı inceleme API'leri ile kullanma

Azure anahtarınızı inceleme API'leri ile kullanmak için Kaynak Kimliğinizi almanız gerekir. Azure portalındaki İçerik Yöneticisi kaynağınıza gidin ve **Özellikler** bıçağını seçin. Kaynak Kimliği değerini kopyalayın ve Gözden Geçirme aracının **Kimlik Bilgileri** sekmesinin **Beyaz Listelenmiş Kaynak Kimliği(ler)** alanına yapıştırın.

![Azure portalında İçerik Moderatör Kaynak Kimliği](images/credentials-azure-portal-resourceid.PNG)

Abonelik anahtarınızı her iki yerde de girdiyseniz, İnceleme aracı hesabınızla birlikte gelen deneme anahtarı kullanılmaz, ancak kullanılabilir kalır.

## <a name="next-steps"></a>Sonraki adımlar

İçerik Denetleme senaryolarında İnceleme aracını kullanmaya başlamak için [Gözden Geçirme aracını hızlı](../quick-start.md) bir şekilde izleyin.
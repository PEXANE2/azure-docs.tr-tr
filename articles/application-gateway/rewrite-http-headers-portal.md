---
title: PORTALDA HTTP istek ve yanıt üstbilgilerini yeniden yazma - Azure Uygulama Ağ Geçidi
description: Ağ geçidinden geçen istek ve yanıtlardaki HTTP üstbilgilerini yeniden yazmak için Azure Uygulama Ağ Geçidi'ni yapılandırmak için Azure portalını nasıl kullanacağınızı öğrenin
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 11/13/2019
ms.author: absha
ms.custom: mvc
ms.openlocfilehash: b90736b3ed1c1f69488fde4a386cf215d751c362
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74012851"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-portal"></a>Azure Application Gateway - Azure portalı ile HTTP istek ve yanıt üstbilgilerini yeniden yazma

Bu makalede, taleplerde ve yanıtlarda HTTP üstbilgilerini yeniden yazmak için bir [Uygulama Ağ Geçidi v2 SKU](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) örneğini yapılandırmak için Azure portalının nasıl kullanılacağı açıklanmaktadır.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlamak için bir Uygulama Ağ Geçidi v2 SKU örneği olması gerekir. Başlıkların yeniden yazılması v1 SKU'da desteklenmez. v2 SKU'nuz yoksa başlamadan önce bir [Uygulama Ağ Geçidi v2 SKU](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps) örneği oluşturun.

## <a name="create-required-objects"></a>Gerekli nesneleri oluşturma

HTTP üstbilgi yeniden yapılandırmak için, bu adımları tamamlamanız gerekir.

1. HTTP üstbilgi yeniden yazmak için gerekli olan nesneleri oluşturun:

   - **Yeniden Yazma Eylemi**: Yeniden yazmak istediğiniz istek ve istek üstbilgi alanlarını ve üstbilgi için yeni değeri belirtmek için kullanılır. Bir veya daha fazla yeniden yazma koşullarını yeniden yazma eylemiyle ilişkilendirebilirsiniz.

   - **Yeniden yazma koşulu**: İsteğe bağlı yapılandırma. Yeniden yazma koşulları, HTTP(S) istek ve yanıtlarının içeriğini değerlendirir. HTTP(S) isteği veya yanıtı yeniden yazma koşuluyla eşleşirse yeniden yazma eylemi gerçekleşir.

     Birden fazla koşulu bir eylemle ilişkilendirirseniz, eylem yalnızca tüm koşullar karşılandığında oluşur. Başka bir deyişle, işlem mantıksal ve işlemdir.

   - **Yeniden yazma kuralı**: Birden çok yeniden yazma eylemi / yeniden yazma koşul birleşimlerini içerir.

   - **Kural sırası**: Yeniden yazma kurallarının yürütülme sırasını belirlemeye yardımcı olur. Bu yapılandırma, yeniden yazma kümesinde birden çok yeniden yazma kuralınız olduğunda yararlıdır. Daha düşük kural sırası değeriolan bir yeniden yazma kuralı önce çalışır. Aynı kural sırası değerini iki yeniden yazma kuralına atarsanız, yürütme sırası belirleyici değildir.

   - **Yeniden Yazma Kümesi**: İstek yönlendirme kuralıyla ilişkilendirilecek birden çok yeniden yazma kuralı içerir.

2. Yeniden yazma kümesini yönlendirme kuralına takın. Yeniden yazma yapılandırması yönlendirme kuralı ile kaynak dinleyiciye eklenir. Temel bir yönlendirme kuralı kullandığınızda, üstbilgi yeniden yazma yapılandırması bir kaynak dinleyiciyle ilişkilidir ve genel bir üstbilgi yeniden yazmaktır. Yol tabanlı yönlendirme kuralı kullandığınızda, üstbilgi yeniden yazma yapılandırması URL yol haritasında tanımlanır. Bu durumda, yalnızca bir sitenin belirli yol alanı için geçerlidir.

Birden çok HTTP üstbilgi yeniden yazma kümeleri oluşturabilir ve her yeniden yazma kümesini birden çok dinleyiciye uygulayabilirsiniz. Ancak belirli bir dinleyiciye yalnızca bir yeniden yazma kümesi uygulayabilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla [Azure portalında](https://portal.azure.com/) oturum açın.

## <a name="configure-header-rewrite"></a>Üstbilgi yeniden yapılandırma

Bu örnekte, bir arka uç uygulaması tarafından gönderilen HTTP yanıtındaki konum üstbilgisini yeniden yazarak yeniden yönlendirme URL'sini değiştiririz.

1. **Tüm kaynakları**seçin ve ardından uygulama ağ geçidinizi seçin.

2. Sol bölmede **Yeniden Yazma'yı** seçin.

3. **Yeniden Yazma kümesini**seçin:

   ![Yeniden yazma kümesi ekle](media/rewrite-http-headers-portal/add-rewrite-set.png)

4. Yeniden yazma kümesi için bir ad sağlayın ve yönlendirme kuralıyla ilişkilendirin:

   - **Ad** kutusuna yeniden yazma kümesinin adını girin.
   - **İlişkili yönlendirme kuralları** listesinde listelenen kurallardan birini veya birkaçını seçin. Yalnızca diğer yeniden yazma kümeleriyle ilişkilendirilmemiş kuralları seçebilirsiniz. Diğer yeniden yazma kümeleriyle zaten ilişkili olan kurallar soluk.
   - **Sonraki'ni**seçin.
   
     ![Ad ve ilişkilendirme ekleme](media/rewrite-http-headers-portal/name-and-association.png)

5. Yeniden yazma kuralı oluşturun:

   - **Yeniden Yaz kuralını ekle'yi**seçin.

     ![Yeniden yazma kuralı ekle](media/rewrite-http-headers-portal/add-rewrite-rule.png)

   - **Yeniden Yazma kuralı adı** kutusuna yeniden yazma kuralı için bir ad girin. **Kural sırakutusuna** bir sayı girin.

     ![Kural adını yeniden yazma ekleme](media/rewrite-http-headers-portal/rule-name.png)

6. Bu örnekte, konum üstbilgisini yalnızca azurewebsites.net bir başvuru içerdiğinde yeniden yazacağız. Bunu yapmak için, yanıttaki konum üstbilgisinin azurewebsites.net içerip içermediğini değerlendirmek için bir koşul ekleyin:

   - **Koşul Ekle'yi** seçin ve ardından genişletmek için **"If** yönergeleri" içeren kutuyu seçin.

     ![Koşul ekleme](media/rewrite-http-headers-portal/add-condition.png)

   - Kontrol listesi için **değişken türünde,** **HTTP üstbilgisini**seçin.

   - **Üstbilgi türü** listesinde **Yanıt'ı**seçin.

   - Çünkü bu örnekte, ortak bir üstbilgi olan konum üstbilgisini değerlendiriyoruz, **üstbilgi adı**altında **Ortak üstbilgi'yi** seçin.

   - Ortak **üstbilgi** listesinde **Konum'u**seçin.

   - **Büyük/Küçük Harf duyarlı**altında, **Hayır'ı**seçin.

   - **Operatör** listesinde eşit **(=)** seçeneğini belirleyin.

   - Normal bir ifade deseni girin. Bu örnekte, desen `(https?):\/\/.*azurewebsites\.net(.*)$`kullanacağız.

   - **Tamam'ı**seçin.

     ![Bir If koşulunu yapılandırma](media/rewrite-http-headers-portal/condition.png)

7. Konum üstbilgisini yeniden yazmak için bir eylem ekleyin:

   - Eylem **türü** **listesinde, Ayarla'yı**seçin.

   - **Üstbilgi türü** listesinde **Yanıt'ı**seçin.

   - **Üstbilgi adı altında,** **Ortak üstbilgi**seçin.

   - Ortak **üstbilgi** listesinde **Konum'u**seçin.

   - Üstbilgi değerini girin. Bu örnekte, üstbilgi `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` değeri olarak kullanacağız. Bu değer, *azurewebsites.net* yerine konum üstbilgisinde *contoso.com.*

   - **Tamam'ı**seçin.

     ![Eylem ekleme](media/rewrite-http-headers-portal/action.png)

8. Yeniden yazma kümesini oluşturmak için **Oluştur'u** seçin:

   ![Oluştur’u seçin](media/rewrite-http-headers-portal/create.png)

9. Yeniden Yazma kümesi görünümü açılır. Oluşturduğunuz yeniden yazma kümesinin yeniden yazma kümeleri listesinde olduğunu doğrulayın:

   ![Set görünümünü yeniden yazma](media/rewrite-http-headers-portal/rewrite-set-list.png)

## <a name="next-steps"></a>Sonraki adımlar

Bazı yaygın kullanım örneklerinin nasıl ayarlanılabildiği hakkında daha fazla bilgi edinmek için, [ortak üstbilgisenaryolarını yeniden yazma tarihine](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers)bakın.
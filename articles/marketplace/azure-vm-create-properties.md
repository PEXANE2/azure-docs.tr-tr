---
title: Azure Marketi 'nde sanal makine teklifi özelliklerini yapılandırma
description: Azure Marketi 'nde sanal makine teklifi özelliklerini yapılandırmayı öğrenin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: emuench
ms.author: mingshen
ms.date: 10/19/2020
ms.openlocfilehash: cc5702d973150b80188354719333fb7cf22d58b3
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92284882"
---
# <a name="how-to-configure-virtual-machine-offer-properties"></a>Sanal Makine teklifi özelliklerini yapılandırma

**Özellikler** sayfasında, Azure Marketi 'nde sanal makıne (VM) teklifinizi, uygulama sürümünüzü ve teklifinizi destekleyen yasal sözleşmeleri gruplamak için kullanılan kategorileri tanımlarsınız.

## <a name="select-a-category"></a>Kategori seçin

Teklifinizi uygun Azure Marketi arama alanlarında yerleştirmek için kategoriler ve alt kategoriler ' i seçin. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğini açıkladığınızdan emin olun. Seçin:

- Birincil ve ikincil kategori dahil olmak üzere en az bir ve en fazla iki kategori (isteğe bağlı).
- Her birincil ve/veya ikincil kategori için en fazla iki alt kategori. Teklifiniz için geçerli bir alt kategori yoksa, **geçerli değil**' i seçin.

[Teklif listesi En Iyi uygulamalarında](gtm-offer-listing-best-practices.md)kategorilerin ve alt kategorilerin tam listesini görebilirsiniz. Sanal makine teklifleri her zaman Azure Marketi 'ndeki **işlem** kategorisi altında görünür.

## <a name="provide-terms-and-conditions"></a>Hüküm ve koşullar sağlama

**Yasal**bölümünde teklifiniz için hüküm ve koşullar sağlayın. İki seçeneğiniz vardır:

- [İsteğe bağlı düzeltme ile standart sözleşmeyi kullanın](#use-the-standard-contract)
- [Kendi hüküm ve koşullarınızı kullanın](#use-your-own-terms-and-conditions)

Standart Sözleşme ve isteğe bağlı düzeltme hakkında bilgi edinmek için bkz. [Microsoft Commercial Market Için standart sözleşme](standard-contract.md). [Standart sözleşme](https://go.microsoft.com/fwlink/?linkid=2041178) PDF 'sini indirebilirsiniz (açılır pencere engelinizin kapalı olduğundan emin olun).

### <a name="use-the-standard-contract"></a>Standart sözleşmeyi kullanma

Müşteriler için satın alma sürecini basitleştirmek ve yazılım satıcılarının yasal karmaşıklığını azaltmak için, Microsoft, ticari Market 'teki tekliflerinizi kullanabileceğiniz standart bir sözleşme sunar. Yazılımınızı standart sözleşme kapsamında sunışınızda, müşterilerin yalnızca bir kez okuyup kabul etmesi ve özel hüküm ve koşullar oluşturmanız gerekmez.

1. **Microsoft 'un ticari Market Için standart sözleşmeyi kullan** onay kutusunu seçin.

   ![Microsoft 'un ticari Market için standart sözleşmeyi kullan onay kutusunu gösterir.](partner-center-portal/media/use-standard-contract.png)
1. **Onay** Iletişim kutusunda **kabul et**' i seçin. Bunu görmek için yukarı kaydırmanız gerekebilir.
1. Devam etmeden önce **Taslağı kaydet** ' i seçin.

   > [!NOTE]
   > Ticari Market için standart sözleşmeyi kullanarak bir teklifi yayımladıktan sonra, kendi özel hüküm ve koşullarınızı kullanamazsınız. Çözümünüzü standart sözleşme altında, isteğe bağlı değişikliğe göre veya kendi hüküm ve koşullarınız altında sunun.

### <a name="add-amendments-to-the-standard-contract-optional"></a>Standart Sözleşmeye düzeltme ekleme (isteğe bağlı)

İki çeşit değişiklik bulunur: *evrensel* ve *özel*.

#### <a name="add-universal-amendment-terms"></a>Evrensel düzeltme koşulları Ekle

**Microsoft 'un ticari Market 'e yönelik standart sözleşmeye yönelik evrensel değişiklik koşulları** kutusunda, evrensel değişiklik koşullarınızı girin. Bu kutuya sınırsız sayıda karakter girebilirsiniz. Bu terimler, bulma ve satın alma akışında AppSource, Azure Marketi 'nde müşterilere ve/veya Azure portal görüntülenir.

#### <a name="add-one-or-more-custom-amendments"></a>Bir veya daha fazla özel değişiklik ekleyin

1. **Microsoft 'un ticari Market 'e yönelik standart sözleşmeye yönelik özel değişiklik koşulları**' nın altında, **özel düzeltme dönemi Ekle (en fazla 10)** bağlantısını seçin.
1. Özel değişiklik **Koşulları** kutusunda, düzeltme koşullarınızı girin.
1. **KIRACı kimliği** kutusuna BIR Kiracı kimliği girin. Yalnızca bu özel şartlar için belirttiğiniz kiracı kimlikleri ile ilişkili müşteriler, Azure portal teklifin satın alma akışında görür.
   > [!TIP]
   > Bir Kiracı kimliği, Azure 'da müşterinizin kimliğini tanımlar. Müşterinize bu kimliği sorabilir ve [**https://portal.azure.com**](https://portal.azure.com)  >  **Azure Active Directory**  >  **özellikleri**' ne giderek bulabilirler. Dizin KIMLIĞI değeri kiracı KIMLIĞIDIR (örneğin, `50c464d3-4930-494c-963c-1e951d15360e` ). Ayrıca, [Microsoft Azure ve Office 365 KIRACı kimliği olan](https://www.whatismytenantid.com/)etki alanı adı URL 'sini kullanarak KURULUŞUNUZUN Kiracı kimliğini de arayabilirsiniz.
1. **Açıklama** kutusunda, isteğe bağlı olarak Kiracı kimliği için kolay bir açıklama girin. Bu açıklama, değişiklik yaparken hedeflediğiniz müşteriyi belirlemenize yardımcı olur.
1. Başka bir kiracı KIMLIĞI eklemek için **müşterinin KIRACı kimliğini Ekle** bağlantısını seçin ve 3. ve 4. adımları yineleyin. En fazla 20 Kiracı kimliği ekleyebilirsiniz.
1. Başka bir düzeltme dönemi eklemek için 1 ile 5 arasındaki adımları yineleyin. Teklif başına en fazla on özel düzeltme terimi sağlayabilirsiniz. 
2. Devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="use-your-own-terms-and-conditions"></a>Kendi hüküm ve koşullarınızı kullanın

Standart sözleşme yerine kendi hüküm ve koşullarınızı sağlamayı tercih edebilirsiniz. Müşterilerinizin teklifinizi deneyebilmeleri için önce bu koşulları kabul etmesi gerekir.

1. **Yasal**' ın altında, **Microsoft 'un ticari Market Için standart sözleşmeyi kullan** onay kutusunun temizlenmiş olduğundan emin olun.
1. **Hüküm ve koşullar** kutusunda en fazla 10.000 karakter girin.

   > [!NOTE]
   > Daha uzun bir açıklamaya ihtiyacınız varsa, hüküm ve koşullarınızın nerede bulunmalarından işaret eden tek bir Web adresi girin. Bu, müşterilere etkin bir bağlantı olarak görüntülenecektir.

1. Bir sonraki sekmeye devam etmeden önce **Taslağı kaydet** ' i seçin, **teklif listesi**.

## <a name="next-steps"></a>Sonraki adımlar

- [VM teklifi listesini yapılandırma](azure-vm-create-listing.md)

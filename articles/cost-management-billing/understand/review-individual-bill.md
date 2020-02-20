---
title: Bireysel Azure faturanızı gözden geçirme
description: Bireysel Azure aboneliğinize ait faturanızı ve kaynak kullanımınızı anlama ve ücretleri doğrulama konusunda bilgi edinin.
author: bandersmsft
ms.reviewer: judupont
tags: billing
ms.service: cost-management-billing
ms.topic: tutorial
ms.date: 02/13/2020
ms.author: banders
ms.openlocfilehash: 8ebda9716df2e9e45050760ee8d0434c12838370
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77199135"
---
# <a name="tutorial-review-your-individual-azure-bill"></a>Öğretici: Bireysel Azure faturanızı gözden geçirme

Bu makale, Azure faturanızı anlamanıza ve gözden geçirmenize yardımcı olur. Her faturalama döneminde e-posta ile bir fatura gönderilir. Faturanız Azure ücretlerinizin gösterimidir. Faturadaki maliyet bilgileri Azure portalında da mevcuttur. Bu öğreticide Azure portalında faturanızı ayrıntılı günlük kullanım dosyası ve maliyet analizi ile karşılaştıracaksınız.

Bu öğretici yalnızca bireysel aboneliğe sahip olan Azure müşterileri için geçerlidir. Bireysel abonelikler genellikle doğrudan Azure web sitesinden satın alınan ve kullandıkça öde ücretlerini kullanan aboneliklerdir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Faturadaki ücretleri kullanım dosyasıyla karşılaştırma
> * Ücretleri ve kullanımı maliyet analiziyle karşılaştırma

## <a name="prerequisites"></a>Ön koşullar

Ücretli bir *Microsoft Online Services Program* ödeme hesabına sahip olmanız gerekir. Bu hesap, Azure web sitesi üzerinden Azure’a kaydolduğunuzda oluşturulur. Örneğin [kullandıkça öde ücretlerine sahip bir hesabınız](https://azure.microsoft.com/offers/ms-azr-0003p/) veya [Visual Studio aboneliğiniz](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) olabilir.

[Azure Ücretsiz Hesapları](https://azure.microsoft.com/offers/ms-azr-0044p/) için faturalar yalnızca aylık kredi tutarı aşıldığında düzenlenir.

Azure'a abone olmanızın üzerinden en az 30 gün geçmiş olmalıdır. Azure sizi fatura döneminizin sonunda faturalar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

- [https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="compare-invoiced-charges-with-usage-file"></a>Faturadaki ücretleri kullanım dosyasıyla karşılaştırma

<a name="charges"></a>

Kullanım verilerini ve maliyetleri karşılaştırmanın ilk adımı, faturanızı ve kullanım dosyalarınızı indirmektir. Ayrıntılı kullanım CSV dosyası faturalama dönemine ve günlük kullanıma göre ücretlerinizi gösterir. Vergi konusunda bilgi içermez. Dosyaları indirmek için hesap yöneticisi olmanız veya Sahip rolüne sahip olmanız gerekir.

Azure portalında arama kutusuna *abonelikler* yazın ve [Abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)'e tıklayın.

![Abonelikler'e gidin](./media/review-individual-bill/navigate-subscriptions.png)

Abonelikler listesinde aboneliğe tıklayın.

**Faturalama**'nın altında **Faturalar**'a tıklayın.

Faturalar listesinde indirmek istediğiniz faturayı bulun ve indir simgesine tıklayın. Eski faturaları görüntülemek için zaman aralığı değerini değiştirmeniz gerekebilir. Kullanım ayrıntıları dosyasının ve faturanın oluşturulması birkaç dakika sürebilir.

![Faturalama dönemini, indirme seçeneğini ve her faturalama dönemi için toplam ücretleri gösteren ekran görüntüsü](./media/review-individual-bill/download-invoice.png)

Kullanımı + Ücretleri İndir penceresinde **CSV dosyasını indir**'e ve **Faturayı indir**'e tıklayın.

![Faturayı indir'i ve kullanım sayfasını gösteren ekran görüntüsü](./media/review-individual-bill/usageandinvoice.png)

**Kullanılamıyor** iletisiyle karşılaşırsanız kullanım verilerinin veya faturanın mevcut olmamasının birkaç nedeni olabilir:

- Azure'a abone olmanızın üzerinden 30 gün geçmemiştir.
- Fatura dönemine ait kullanım yoktur.
- Fatura henüz düzenlenmemiştir. Fatura döneminin sonuna kadar bekleyin.
- Faturaları görüntüleme izniniz yoktur. Hesap Yöneticisi değilseniz eski faturaları göremezsiniz. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](../manage/manage-billing-access.md).
- Aboneliğinizde Ücretsiz Deneme sürümünüz veya aşmadığınız bir aylık kredi tutarınız varsa, Microsoft Müşteri Sözleşmeniz olmadığı sürece bir fatura almazsınız.

Sonraki adımda ücretleri gözden geçirin. Faturanızda vergiler ve kullanım ücretleriniz gösterilir.

![Azure Faturası Örneği](./media/review-individual-bill/invoice-usage-charge.png)

İndirdiğiniz CSV kullanım dosyasını açın. Dosyanın sonunda, *Maliyet* sütunundaki tüm satırların değerini toplayın.

![Toplam maliyetin gösterildiği örnek kullanım dosyası](./media/review-individual-bill/usage-file-usage-charges.png)

 Toplam *Maliyet* değeri, faturanızdaki *kullanım ücretleri* maliyeti ile tam olarak aynı olmalıdır.

Kullanım ücretleriniz ölçüm düzeyinde görüntülenir. Aşağıdaki terimler hem faturada hem de ayrıntılı kullanım dosyasında aynı anlama gelir. Örneğin faturadaki faturalama dönemi, ayrıntılı kullanım dosyasındaki faturalama dönemiyle aynıdır.

| Fatura (PDF) | Ayrıntılı kullanım (CSV)|
| --- | --- |
|Fatura döngüsü | BillingPeriodStartDate BillingPeriodEndDate |
|Adı |Ölçüm Kategorisi |
|Tür |Ölçüm Alt Kategorisi |
|Kaynak |MeterName |
|Bölge |MeterRegion |
|Kullanılan | Miktar |
|Dahil |Dahil Edilen Miktar |
|Faturalanabilir |Kapasite Aşım Miktarı |
|Fiyat | EffectivePrice|
| Değer | Maliyet |

Faturanızın **Kullanım Ücretleri** bölümü, faturalama döneminiz boyunca kullanılan her ölçümün toplam değerini (maliyet) gösterir. Örneğin, aşağıdaki resimde Azure Depolama hizmetinin *P10 Diskleri* kaynağı için kullanım ücreti gösterilir.

![Fatura kullanım ücretleri](./media/review-individual-bill/invoice-usage-charges.png)

CSV kullanım bilgileri dosyanıza *MeterName* filtresi uygulayarak faturanızda gösterilen kaynağa belirtin. Ardından sütundaki öğelerin *Maliyet* değerini toplayın. Bir örneği aşağıda verilmiştir.

![Kullanım dosyasının MeterName için toplam değeri](./media/review-individual-bill/usage-file-usage-charge-resource.png)

Özetlenen *Maliyet* değeri, faturanıza yansıtılan tek kaynağın *kullanım maliyetleri* ile aynı olmalıdır.

Daha fazla bilgi için bkz. [Azure faturanızı anlama](understand-invoice.md) ve [Azure ayrıntılı kullanımınızı anlama](understand-usage.md).

## <a name="compare-charges-and-usage-in-cost-analysis"></a>Ücretleri ve kullanımı maliyet analiziyle karşılaştırma

Azure portalındaki maliyet analizi de ücretlerinizi doğrulamanıza yardımcı olabilir. Faturaya yansıtılan kullanım ve ücretlerle ilgili genel bakış için Azure portalındaki [Abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) aboneliğinizi seçin. Ardından **Maliyet analizi**'ne ve daha sonra görünüm listesinde **Fatura ayrıntıları**'na tıklayın.

![Fatura ayrıntıları seçimini gösteren örnek](./media/review-individual-bill/cost-analysis-select-invoice-details.png)

Ardından tarih aralığı listesinde faturanız için bir tarih aralığı belirtin. Fatura numarasını filtre olarak ekleyin ve faturanızla eşleşen InvoiceNumber değerini seçin. Maliyet analizi, faturalanmış öğelerinizin maliyet ayrıntılarını gösterir.

![Maliyet analizinde faturalanmış maliyet ayrıntılarını gösteren örnek](./media/review-individual-bill/cost-analysis-service-usage-charges.png)

Maliyet analizinde gösterilen maliyetler, faturanıza yansıtılan tek kaynağın *kullanım maliyetleri* ile aynı olmalıdır.

![Fatura kullanım ücretleri](./media/review-individual-bill/invoice-usage-charges.png)

## <a name="external"></a>Dış hizmetler ayrı faturalanır

Dış hizmetler veya market ücretleri, üçüncü taraf yazılımı satıcıları tarafından oluşturulan kaynaklar için geçerlidir. Bu kaynakları Azure Market’ten sağlayabilirsiniz. Örneğin Barracuda Güvenlik Duvarı, üçüncü tarafın kullanıma sunduğu bir Azure Market kaynağıdır. Güvenlik duvarının tüm ücretleri ve bunlara karşılık gelen ölçümler dış hizmet ücretleri olarak görünür.

Dış hizmetlerin ücretleri ayrı faturalanır. Ücretler Azure faturanızda gösterilmez. Daha fazla bilgi edinmek için bkz. [Azure dış hizmet ücretlerinizi anlama](understand-azure-marketplace-charges.md).

### <a name="resources-billed-by-usage-meters"></a>Kullanım ölçümleri tarafından faturalanan kaynaklar

Azure doğrudan kaynak maliyetine göre faturalama yapmaz. Kaynağın ücretleri bir veya birden fazla ölçüm kullanılarak hesaplanır. Ölçümler, kaynağın yaşam süresi boyunca tüketilen kaynak kullanımını izlemek için kullanılır. Bu ölçümler daha sonra faturayı hesaplamak için kullanılır.

Sanal makine gibi tek bir Azure kaynağı oluşturduğunuzda bu kaynağın bir veya birden fazla ölçüm örneği oluşturulur. Ölçümler, kaynağın zaman içinde kullanımını izlemek için kullanılır. Her ölçüm, Azure tarafından faturayı hesaplamak için kullanılan kullanım kayıtlarını üretir.

Örneğin, Azure’da oluşturulan tek bir sanal makinenin (VM) kullanımını izlemek için aşağıdaki ölçümler oluşturulmuş olabilir:

- İşlem Saatleri
- IP Adresi Saatleri
- Gelen Veri Aktarımı
- Giden Veri Aktarımı
- Standart Yönetilen Diskler
- Standart Yönetilen Disk İşlemleri
- Standart GÇ-Disk
- Standart GÇ-Blok Blobu Okuma
- Standart GÇ-Blok Blobu Yazma
- Standart GÇ-Blok Blobu Silme

VM oluşturulduğunda her ölçüm kullanım kayıtları üretmeye başlar. Bu kullanım ve ölçümün fiyatı Azure ölçüm sisteminde izlenir.

CSV biçimindeki kullanım dosyanızda faturanızın hesaplanması için kullanılan ölçümleri görebilirsiniz.

## <a name="payment"></a>Faturanızı ödeme

Ödeme yönteminiz olarak bir kredi kartı ayarladıysanız ödeme faturalama döneminin bitişinden sonraki 10 gün içinde otomatik olarak ücretlendirilir. Kredi kartı hesap özetinizde satır öğesi **MSFT Azure** olarak görünür.

Ücretlendirilen kredi kartını değiştirmek için bkz. [Azure’da kredi kartı ekleme, güncelleştirme veya kaldırma](../manage/change-credit-card.md).

[Fatura ile ödemeyi](../manage/pay-by-invoice.md) seçerseniz, ödemenizi faturanızın en altında bulunan konuma gönderin.

Ödemenizin durumunu denetlemek için [bir destek bileti oluşturun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Faturadaki ücretleri kullanım dosyasıyla karşılaştırma
> * Ücretleri ve kullanımı maliyet analiziyle karşılaştırma

Maliyet analizini kullanmaya başlamak için hızlı başlangıcı tamamlayın.

> [!div class="nextstepaction"]
> [Maliyet analizini başlatma](../costs/quick-acm-cost-analysis.md)

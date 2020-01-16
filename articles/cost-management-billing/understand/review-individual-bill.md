---
title: Bireysel Azure faturanızı gözden geçirin
description: Faturanızı ve kaynak kullanımınızı nasıl anlayacağınızı ve bireysel Azure aboneliğiniz için ücretleri doğrulamanızı öğrenin.
author: bandersmsft
manager: dougeby
tags: billing
ms.service: cost-management-billing
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2019
ms.author: banders
ms.openlocfilehash: 1249758e5dd5b7e823c11890e57549195da22529
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75987351"
---
# <a name="tutorial-review-your-individual-azure-bill"></a>Öğretici: bireysel Azure faturanızı gözden geçirin

Bu makale, Azure faturanızı anlamanıza ve incelemenizi sağlar. Her faturalandırma döneminde, normalde e-postada bir fatura alırsınız. Fatura, Azure faturanızı temsil eden bir gösterimidir. Azure portal faturada aynı maliyet bilgileri kullanılabilir. Bu öğreticide faturanızı ayrıntılı günlük kullanım dosyasıyla ve Azure portal maliyet analiziyle karşılaştırırsınız.

Bu öğretici yalnızca bireysel aboneliğe sahip Azure müşterileri için geçerlidir. Ortak ayrı abonelikler, doğrudan Azure Web sitesinden satın alınan Kullandıkça Öde tarifelerine sahip olanlardır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Kullanım dosyası ile faturalanmış ücretleri karşılaştırın
> * Maliyet analizinde ücretleri ve kullanımı karşılaştırın

## <a name="prerequisites"></a>Ön koşullar

Ücretli bir *Microsoft Online Services programı* faturalandırma hesabına sahip olmanız gerekir. Hesap, Azure 'a Azure Web sitesi üzerinden kaydolduğunuzda oluşturulur. Örneğin, [Kullandıkça Öde tarifesine](https://azure.microsoft.com/offers/ms-azr-0003p/) sahip bir hesabınız varsa veya bir [Visual Studio abonesi](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)olduğunuzda.

[Ücretsiz Azure hesabı](https://azure.microsoft.com/offers/ms-azr-0044p/) faturaları yalnızca aylık kredi miktarı aşıldığında oluşturulur.

Azure 'a abone olduğunuz günden 30 günden daha uzun olmalıdır. Azure sizi fatura döneminizin sonunda faturalar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

- https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="compare-invoiced-charges-with-usage-file"></a>Kullanım dosyası ile faturalanmış ücretleri karşılaştırın

<a name="charges"></a>

Kullanım ve maliyetlerin karşılaştırılacağı ilk adım faturanızı ve kullanım dosyalarınızı indirmaktır. Ayrıntılı kullanım CSV dosyası faturalama dönemine ve günlük kullanıma göre ücretlerinizi gösterir. Herhangi bir vergi bilgisi içermez. Dosyaları indirmek için bir hesap yöneticisi olmanız veya sahip rolüne sahip olmanız gerekir.

Azure portal, arama kutusuna *abonelikler* yazın ve ardından [abonelikler](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)' e tıklayın.

![Aboneliklerde gezin](./media/review-individual-bill/navigate-subscriptions.png)

Abonelikler listesinde aboneliğe tıklayın.

**Faturalama**altında **faturalar**' ı tıklatın.

Faturalar listesinde, indirmek istediğiniz olanı bulun ve indir simgesine tıklayın. Eski faturaları görüntülemek için TimeSpan değeri değiştirmeniz gerekebilir. Kullanım ayrıntıları dosyasını ve faturasını oluşturmak birkaç dakika sürebilir.

![Faturalama dönemini, indirme seçeneğini ve her faturalama dönemi için toplam ücretleri gösteren ekran görüntüsü](./media/review-individual-bill/download-invoice.png)

Kullanımı ve ücretleri Indir penceresinde, **CSV 'Yi indir** ve **faturayı indir**' i tıklatın.

![Faturayı indir'i ve kullanım sayfasını gösteren ekran görüntüsü](./media/review-individual-bill/usageandinvoice.png)

**Kullanılabilir değilse** , kullanım ayrıntılarını veya bir faturayı görmemenizi belirten birkaç neden vardır:

- Azure'a abone olmanızın üzerinden 30 gün geçmemiştir.
- Fatura dönemi için kullanım yok.
- Henüz bir fatura oluşturulmaz. Fatura döneminin sonuna kadar bekleyin.
- Faturaları görüntüleme izniniz yoktur. Hesap yöneticisi olmadığınız takdirde eski faturaları görmeyebilirsiniz. Faturalama bilgilerine erişme hakkında daha fazla bilgi için bkz. [Rolleri kullanarak Azure faturalamasına erişimi yönetme](../manage/manage-billing-access.md).
- Aboneliğinizde Ücretsiz Deneme sürümünüz veya aşmadığınız bir aylık kredi tutarınız varsa, Microsoft Müşteri Sözleşmeniz olmadığı sürece bir fatura almazsınız.

Ardından, ücretleri gözden geçirin. Faturanızda vergiler ve kullanım ücretlerinizi gösteren değerler gösterilir.

![Örnek Azure faturası](./media/review-individual-bill/invoice-usage-charge.png)

İndirdiğiniz CSV kullanım dosyasını açın. Dosyanın sonunda, *Maliyet* sütunundaki tüm öğeler için değeri toplayın.

![Toplanan maliyetli örnek kullanım dosyası](./media/review-individual-bill/usage-file-usage-charges.png)

 Toplanan *Maliyet* değeri, faturanızda *kullanım ücretleri* maliyetiyle tam olarak eşleşmelidir.

Kullanım ücretleriniz ölçüm düzeyinde görüntülenir. Aşağıdaki terimler hem faturada hem de ayrıntılı kullanım dosyasında aynı anlama gelir. Örneğin faturadaki faturalama dönemi, ayrıntılı kullanım dosyasındaki faturalama dönemiyle aynıdır.

| Fatura (PDF) | Ayrıntılı kullanım (CSV)|
| --- | --- |
|Fatura döngüsü | BillingPeriodStartDate BillingPeriodEndDate |
|Ad |Ölçüm Kategorisi |
|Tür |Ölçüm Alt Kategorisi |
|Kaynak |MeterName |
|Bölge |MeterRegion |
|Tüketilen | Miktar |
|Var |Dahil Edilen Miktar |
|Faturalanabilir |Kapasite Aşım Miktarı |
|Kur | EffectivePrice|
| Değer | Maliyet |

Faturanızda **kullanım ücretleri** bölümü, fatura döneminize göre tüketilen her bir ölçüm için toplam değeri (maliyet) gösterir. Örneğin, aşağıdaki görüntüde *P10 diskler* kaynağı Için Azure Storage hizmeti 'nin kullanım ücreti gösterilmektedir.

![Fatura kullanım ücretleri](./media/review-individual-bill/invoice-usage-charges.png)

CSV kullanım dosyanızda, faturada gösterilen ilgili kaynak için *MeterName* göre filtreleyin. Ardından, sütundaki öğelerin *Maliyet* değerini toplayın. Bir örneği aşağıda verilmiştir.

![MeterName için kullanım dosyası toplamı değeri](./media/review-individual-bill/usage-file-usage-charge-resource.png)

Toplanan *Maliyet* değeri, faturanızda ücretlendirilen tek kaynak için *kullanım ücretleri* maliyetine tam olarak eşleşmelidir.

Daha fazla bilgi için bkz. [Azure faturanızı anlayın](understand-invoice.md) ve [Azure ayrıntılı kullanımınızı anlayın](understand-usage.md).

## <a name="compare-charges-and-usage-in-cost-analysis"></a>Maliyet analizinde ücretleri ve kullanımı karşılaştırın

Azure portal maliyet analizi, ücretlerinizi doğrulamanıza da yardımcı olabilir. Faturalanmış kullanımınız ve ücretlerinize hızlı bir genel bakış almak için Azure portal [abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) aboneliğinizi seçin. Ardından, **Maliyet Analizi** ' ne tıklayın ve ardından görünümler listesinden **Fatura Ayrıntıları**' na tıklayın.

![Fatura Ayrıntıları seçimini gösteren örnek](./media/review-individual-bill/cost-analysis-select-invoice-details.png)

Sonra, tarih aralığı listesinde faturanızda bir zaman aralığı seçin. Fatura numarası için bir filtre ekleyin ve ardından faturanızdan eşleşen faturaya karşılık gelen fatura numarasını seçin. Maliyet analizi, faturalanmış öğelerinizin maliyet ayrıntılarını gösterir.

![Maliyet analizinde faturalanmış maliyet ayrıntılarını gösteren örnek](./media/review-individual-bill/cost-analysis-service-usage-charges.png)

Maliyet analizinde gösterilen maliyetlerin, faturanızda ücretlendirilen tek kaynak için *kullanım ücretleri* maliyetiyle tam olarak eşleşmesi gerekir.

![Fatura kullanım ücretleri](./media/review-individual-bill/invoice-usage-charges.png)

## <a name="external"></a>Dış hizmetler ayrı faturalanır

Dış hizmetler veya market ücretleri, üçüncü taraf yazılımı satıcıları tarafından oluşturulan kaynaklar için geçerlidir. Bu kaynakları Azure Market’ten sağlayabilirsiniz. Örneğin Barracuda Güvenlik Duvarı, üçüncü tarafın kullanıma sunduğu bir Azure Market kaynağıdır. Güvenlik duvarının tüm ücretleri ve bunlara karşılık gelen ölçümler dış hizmet ücretleri olarak görünür.

Dış hizmetlerin ücretleri ayrı faturalanır. Ücretler Azure faturanızda gösterilmez. Daha fazla bilgi edinmek için bkz. [Azure dış hizmet ücretlerinizi anlama](understand-azure-marketplace-charges.md).

### <a name="resources-billed-by-usage-meters"></a>Kullanım ölçümleri tarafından faturalanan kaynaklar

Azure doğrudan kaynak maliyetine göre faturalama yapmaz. Kaynağın ücretleri bir veya birden fazla ölçüm kullanılarak hesaplanır. Ölçümler, kaynağın yaşam süresi boyunca tüketilen kaynak kullanımını izlemek için kullanılır. Bu ölçümler daha sonra faturayı hesaplamak için kullanılır.

Sanal makine gibi tek bir Azure kaynağı oluşturduğunuzda, bir veya daha fazla ölçüm örneği oluşturulur. Ölçümler, kaynağın zaman içinde kullanımını izlemek için kullanılır. Her ölçüm, Azure tarafından faturayı hesaplamak için kullanılan kullanım kayıtlarını üretir.

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

Kullanım CSV dosyasında faturanızı hesaplamak için kullanılan ölçüleri görebilirsiniz.

## <a name="payment"></a>Faturanızı ödeme

Ödeme yönteminiz olarak bir kredi kartı ayarlarsanız, faturalandırma dönemi sona erdikten sonra ödeme 10 gün içinde otomatik olarak ücretlendirilir. Kredi kartı hesap özetinizde satır öğesi **MSFT Azure** olarak görünür.

Ücretlendirilen kredi kartını değiştirmek için bkz. [Azure’da kredi kartı ekleme, güncelleştirme veya kaldırma](../manage/change-credit-card.md).

[Fatura ile ödemeyi](../manage/pay-by-invoice.md) seçerseniz, ödemenizi faturanızın en altında bulunan konuma gönderin.

Ödemenizin durumunu denetlemek için [bir destek bileti oluşturun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Kullanım dosyası ile faturalanmış ücretleri karşılaştırın
> * Maliyet analizinde ücretleri ve kullanımı karşılaştırın

Maliyet analizini kullanmaya başlamak için hızlı başlangıcı doldurun.

> [!div class="nextstepaction"]
> [Maliyet analizini başlatma](../costs/quick-acm-cost-analysis.md)

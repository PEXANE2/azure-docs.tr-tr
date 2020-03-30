---
title: Satıcı Öngörüleri SSS
description: Bulut İş Ortağı Portalı'nın Satıcı Öngörüleri özelliği hakkında sık sorulan sorular.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: dsindona
ms.openlocfilehash: 011558baa43ee3db2803e9229d1d15df5158d668
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285393"
---
<a name="seller-insights-faq"></a>Satıcı Öngörüleri SSS
===================

Bu makalede, satıcı istatistikleri içinde ortak kullanıcı yordamları ve sorular için rehberlik sağlar.


<a name="find-definitions-for-the-values-in-the-downloaded-transaction-file"></a>İndirilen işlem dosyasındaki değerler için tanımları bulma
------------------------------------------------------------------

İşlem dosyasındaki metrik değerlerin tanımları [Satıcı Öngörüleri Tanımları](./si-insights-definitions-v4.md)makalesinde bulunur.


<a name="see-customer-details-of-transactions-for-which-ive-been-paid"></a>Ödeme yaptığım işlemlerin müşteri ayrıntılarını görün
-------------------------------------------------------------

Ödeme modülünden işlemlerinizi indirdikten sonra **Ödeme Durumu**etiketli sütunu bulun ve filtreyi yalnızca "Ödenmiş Çıkış" değerini görüntülemek için uygulayın. Aşağıdaki sütunlar müşteri bilgilerini içeren görünür: **Şirket Adı,** **Müşteri E-postası,** **Müşteri Ülkesi,** **Müşteri Durumu**ve Müşteri **Posta Kodu.**


<a name="calculate-my-open-accounts-receivable"></a>Açık hesaplarımın alacağı hesaplama
-------------------------------------

Ödeme modülünden işlemlerinizi indirdikten sonra **Ödeme Durumu**etiketli sütunu bulun ve filtreyi yalnızca "Yaklaşan Ödeme" ve "Ödemeye Hazır Değil" değerini görüntülemek için uygulayın. Daha sonra Ödeme **Tutarı (PC)** etiketli sütunu toplamı.


<a name="calculate-revenue-by-customer-usage-period"></a>Müşteri kullanım dönemine göre geliri hesaplama
------------------------------------------

İşlemlerinizi Ödeme modülünden indirdikten **sonra, Hareket Durumu**etiketli sütunu bulun ve "Ücretli" değerini filtreleyin.   Listelenen her hareket için **Ödeme Tutarı (PC)** etiketli sütun, size ödenen tutarı temsil eder.  Hareketle ilişkili kullanım süresini tahmin etmek için, işlemin uygulandığı döneme ait son kullanım gününün yakın bir tahmini olan **ÜcretLendirme Tarihi**sütununu kullanın.


<a name="calculate-your-bad-debt"></a>Kötü borcunu hesapla
---------------------

İşlemlerinizi Ödeme modülünden indirdikten sonra, Son **Tahsilat Durumu**etiketli sütunu bulun ve filtreyi yalnızca "Sil" değerini görüntülemek için uygulayın. Daha sonra Ödeme **Tutarı (PC)** etiketli sütunu toplamı.


<a name="view-payout-or-customer-contact-information"></a>Ödeme veya müşteri iletişim bilgilerini görüntüleme
-------------------------------------------

"Katılımcı" rolüne değil, "sahibi" rolüne sahip bir kullanıcı olarak oturum açın. Yalnızca sahip rolü ödeme ve müşteri bilgilerini görür. [Kullanıcıları Yönet](./cloud-partner-portal-manage-users.md)makalesinde kullanıcı rolleri hakkında daha fazla bilgi edinebilirsiniz.


<a name="calculate-my-advance-payouts"></a>Avans ödemelerimi hesapla
----------------------------

İşlemlerinizi Ödeme modülünden indirdikten **sonra, Hareket Türü**etiketli sütunu bulun ve filtreyi yalnızca "Ücret" değerini görüntülemek için uygulayın. Ardından, Son Toplama **Durumu**etiketli sütunu bulun ve filtreyi yalnızca "Devam Ediyor" değerini görüntülemek için uygulayın. Son olarak, müşteriden tahsilat tan önce size ödenen tüm avansları hesaplamak için **Ödeme Tutarı (PC)** sütununu toplayın.


<a name="calculate-customer-refunds"></a>Müşteri geri ödemelerini hesaplama
--------------------------

İşlemlerinizi Ödeme modülünden indirdikten sonra, Son **Tahsilat Durumu**etiketli sütunu bulun ve filtreyi yalnızca "Geri Ödeme" değerini görüntülemek için uygulayın. Müşterileriniz için işlenen tüm geri ödemeleri hesaplamak için **Ücret Tutarı (PC)** sütununu toplamı.


<a name="identify-which-transactions-involved-a-microsoft-channel-partner"></a>Microsoft Kanal İş Ortağı'nı içeren hareketleri belirleme
----------------------------------------------------------------

"Kurumsal üzerinden Satıcı" ve "Bulut Çözüm Sağlayıcısı" değerlerini görüntülemek üzere filtre uygulanmış **Azure Lisans Türü** sütunundaki tüm hareketler bir Microsoft Kanal İş Ortağı içerir. İş ortağı hakkında daha fazla bilgi için, Ödeme modülü indirme ve Müşteri modülü indirme onların **Bayi Adı** ve **Bayi E-posta** bulabilirsiniz.


<a name="identify-trial-usage-and-trial-conversions"></a>Deneme kullanımını ve deneme dönüşümlerini belirleme
------------------------------------------

Sipariş, Kullanım ve Ödeme modülü indirmeleri artık deneme süresinin geçerli olduğu durumlarda belirli bir sipariş için ne zaman sona erdiğini anlamanıza yardımcı olmak için **Deneme Bitiş Tarihi** içerir. Deneme kullanımını ve siparişleri görmek için, indirmelerde **SKU Fatura Türü** sütununa yer bulun ve filtreyi yalnızca "Deneme" değerini görüntülemek için uygulayın. Deneme dönüşümlerini görmek için, indirmelerde **Deneme Bitiş Tarihi** sütununu bulun ve filtreyi yalnızca Deneme Bitiş Tarihi bugünün tarihini geçtiğinde ve İptal **Tarihi** sütunu Boşsa veya **Deneme Bitiş Tarihi'nden**sonra siparişleri görüntülemek için uygulayın. **Cancel Date**


<a name="when-is-my-monthly-payout-calculated"></a>Aylık ödemem ne zaman hesaplanır?
------------------------------------

Ödemeleriniz, bir önceki ayın son takvim gününe kadar ödemeye hazır olan tüm tutarlar için her ayın 15'inde size verilir. Ayın üçüncü gününde, Microsoft bir önceki ayın ödeme tutarını hesaplar ve ödeme **durumu** sütunundaki "Yaklaşan Ödeme" ile indirmeişleminizdeki tüm geçerli ücret hareketlerini günceller. Bu işlemler, ödeme isteği banka hesabınıza gönderilene kadar bu durumda kalır ve ödeme **durumları** "Ödeme Süresi" olarak güncellenir ve "Ödeme Tarihi" ödeme isteğini bankanıza gönderdiğimiz tarihi gösterecek şekilde güncellenir.


<a name="calculate-customer-acquisition-and-loss"></a>Müşteri edinme ve kaybını hesaplama
---------------------------------------

Müşteri karşıdan yüklemede **Edinilen Tarih** sütununu bularak müşterinin tekliflerinizden birini ilk satın aldığı tarihi görebilirsiniz. Benzer şekilde, müşteri karşıdan yüklemesinde **Kayıp Tarihi** sütunu bularak artık sizin yayınladığınız herhangi bir teklifin olmadığı tarihi görebilirsiniz.


<a name="finding-more-help"></a>Daha fazla yardım bulma
-----------------

- [Satıcı Öngörüleri Tanımları](./si-insights-definitions-v4.md) - Ölçümler ve veriler için tanımları bulun

- Satıcı Öngörüleri - Satıcı Öngörüleri özelliğine giriş [ile başlarken.](./si-getting-started.md)


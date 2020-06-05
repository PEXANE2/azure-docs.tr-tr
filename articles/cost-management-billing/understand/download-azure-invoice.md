---
title: Azure faturanızı görüntüleme ve indirme
description: Azure faturanızı görüntüleme ve indirme işlemleri açıklanır.
keywords: fatura, fatura indirme, azure faturası, azure kullanımı
author: bandersmsft
ms.reviewer: amberb
tags: billing
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 05/28/2020
ms.author: banders
ms.openlocfilehash: 4e77b167f00e2cfa3838439143c6074bd4122976
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84191352"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Microsoft Azure faturanızı görüntüleme ve indirme

Faturanızı [Azure portalından](https://portal.azure.com/) indirebilir veya e-postayla gönderilmesini sağlayabilirsiniz. Kurumsal Anlaşmalı bir Azure müşterisiyseniz (EA müşterisi), kuruluşunuzun faturasını indiremezsiniz. Bunun yerine faturalar, kayıt için faturaları alacak kişi olarak ayarlanan kişiye gönderilir.

## <a name="when-invoices-are-generated"></a>Faturalar ne zaman oluşturulur?

Faturalar, faturalandırma hesabı türünüze göre oluşturulur. Faturalar Microsoft Çevrimiçi Hizmet Programı (MOSP), Microsoft Müşteri Sözleşmesi (MCA) ve Microsoft İş Ortağı Sözleşmesi (MPA) faturalama hesapları için oluşturulur. Faturalar, Kurumsal Anlaşma (EA) faturalama hesapları için de oluşturulur. Ancak, EA faturalama hesaplarına ait faturalar Azure portalında gösterilmez.

Ödeme hesapları hakkında daha fazla bilgi edinmek ve ödeme hesabınızın türünü belirlemek için bkz. [Azure portalında ödeme hesaplarını görüntüleme](../manage/view-all-accounts.md).

## <a name="invoices-for-mosp-billing-accounts"></a>MOSP faturalama hesaplarına ait faturalar

MOSP faturalama hesabı, Azure web sitesi üzerinden Azure’a kaydolduğunuzda oluşturulur. Örneğin, bir [Ücretsiz Azure Hesabına](https://azure.microsoft.com/offers/ms-azr-0044p/), [kullandıkça öde fiyatlarına tabi hesaba](https://azure.microsoft.com/offers/ms-azr-0003p/) veya [Visual Studio abonesi](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/) olarak kaydolduğunuzda.

Azure web sitesinden [kullandıkça öde fiyatlarına tabi hesaba](https://azure.microsoft.com/offers/ms-azr-0003p/) veya [Ücretsiz Azure Hesabına](https://azure.microsoft.com/offers/ms-azr-0044p/) kaydolan belirli bölgelerdeki müşterilerin MCA için faturalama hesabı da olabilir.

Faturalama hesabınızın türünden emin değilseniz bu makaledeki yönergeleri izlemeden önce [Faturalama hesabınızın türünü denetleyin](../manage/view-all-accounts.md#check-the-type-of-your-account). 

Bir MOSP faturalama hesabı aşağıdaki faturalara sahip olabilir:

**Azure hizmet ücretleri**: Abonelik tarafından kullanılan Azure kaynaklarını içeren her bir Azure aboneliği için bir fatura oluşturulur. Fatura, bir faturalama dönemine tabi olan ücretleri içerir. Faturalama dönemi, aboneliğin oluşturulduğu ayın gününe göre belirlenir.

Örneğin Burak, 5 Mart’ta *Azure sub 01*, 10 Mart’ta *Azure sub 02* aboneliklerini oluşturuyor. *Azure sub 01* aboneliğinin faturasına bir ayın beşinci gününden diğer ayın dördüncü gününe kadar olan ücretler dahil edilir. *Azure sub 02* aboneliğinin faturasına ise bir ayın onuncu gününden diğer ayın dokuzuncu gününe kadar olan ücretler dahil edilir. Tüm Azure aboneliklerine ilişkin faturalar normalde hesabın oluşturulduğu ayın gününde oluşturulur, ancak iki gün sonraya kadar ertelenebilir. Bu örnekte Burak, hesabını 2 Şubat’ta oluşturduysa, hem *Azure sub 01* hem de *Azure sub 02* aboneliği için faturalar normalde her ayın ikinci gününde oluşturulur, ancak iki gün sonraya kadar ertelenebilir.

**Azure Market, rezervasyonlar ve spot VM’ler**: Abonelik kullanılarak satın alınan rezervasyonlar, market ürünleri ve spot VM’ler için bir fatura oluşturulur. Fatura önceki aya ait tüm ilgili ücretleri gösterir. Örneğin Burak, 1 Mart’ta bir rezervasyon, 30 Mart’ta başka bir rezervasyon satın aldı. Her iki rezervasyon için Nisan ayında tek bir fatura oluşturulur. Azure Market, rezervasyonlar ve spot VM’lere yönelik fatura yaklaşık olarak her zaman ayın dokuzuncu gününde oluşturulur.

Azure için bir kredi kartıyla ödeme yapıp rezervasyon satın alırsanız Azure hemen bir fatura oluşturur. Ancak bir faturayla faturalandırıldığında, rezervasyonun ücreti sonraki aya ait faturanıza yansıtılır.

**Azure destek planı**: Destek planı aboneliğiniz için her ay bir fatura oluşturulur. İlk fatura, satın aldığınız günde veya sonraki iki gün içinde oluşturulur. Sonraki destek planı faturaları normalde ay içinde hesabın oluşturulduğu günde oluşturulur, ancak iki gün sonraya kadar ertelenebilir.

## <a name="download-your-mosp-azure-subscription-invoice"></a>MOSP Azure aboneliği faturanızı indirme

Fatura, yalnızca MOSP’a ait bir faturalama hesabına ait abonelik için oluşturulur. [MOSP hesabına erişiminizi denetleyin](../manage/view-all-accounts.md#check-the-type-of-your-account). 

Faturasını indirebilmek için abonelikte hesap yöneticisi rolüne sahip olmanız gerekir. Hesap yöneticisi izin verirse sahip, katkıda bulunan veya okuyucu rollerine sahip kullanıcılar da faturayı indirebilir. Daha fazla bilgi için bkz. [Kullanıcıların faturaları indirmesine izin verme](../manage/manage-billing-access.md#opt-in).

1. Azure portalındaki [Abonelikler sayfasından](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) aboneliğinizi seçin.
1. Faturalama bölümünde **Faturalar**’ı seçin.  
    ![Bir aboneliğe ait faturalar seçeneğini belirleyen kullanıcıyı gösteren ekran görüntüsü](./media/download-azure-invoice/select-subscription-invoice.png)
1. Faturanızın PDF biçimindeki halini indirmek **İndir**’i seçin ve ardından fatura bölümü altından **İndir**’i seçin.  
    ![Faturalama dönemini, indirme seçeneğini ve her faturalama dönemi için toplam ücretleri gösteren ekran görüntüsü](./media/download-azure-invoice/downloadinvoice-subscription.png)
1. Dilerseniz kullanım ayrıntıları bölümü altından **İndir**’i seçerek tüketilen miktar ve kullanıma ait günlük dökümünüzü de indirebilirsiniz. CSV dosyasının hazırlanması birkaç dakika sürebilir.  
    ![Faturayı indir seçeneğini ve kullanım sayfasını gösteren ekran görüntüsü](./media/download-azure-invoice/usage-and-invoice-subscription.png)

Faturanız hakkında daha fazla bilgi için bkz. [Microsoft Azure faturanızı anlama](../understand/review-individual-bill.md). Maliyetlerinizi yönetme konusunda yardım için bkz. [Azure fatura ve maliyet yönetimi ile beklenmeyen maliyetleri engelleme](../manage/getting-started.md).

## <a name="download-your-mosp-support-plan-invoice"></a>MOSP destek planı faturanızı indirme

Fatura, yalnızca bir MOSP faturalama hesabına ait destek planı aboneliği için oluşturulur. [MOSP hesabına erişiminizi denetleyin](../manage/view-all-accounts.md#check-the-type-of-your-account).

Faturasını indirebilmek için destek planı aboneliğinde hesap yöneticisi rolüne sahip olmanız gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.  
    ![Portalda maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/download-azure-invoice/search-cmb.png)
1. Sol taraftan **Faturalar**’ı seçin.
1. Destek planı aboneliğinizi ve ardından **İndir**’i seçin.  
    [![Faturalama profili listesini gösteren ekran görüntüsü](./media/download-azure-invoice/cmb-invoices.png)](./media/download-azure-invoice/cmb-invoices-zoomed-in.png#lightbox)
1. Faturanızın PDF biçimindeki halini indirmek için **İndir**’i seçin.  
    ![Faturalama dönemini, indirme seçeneğini ve her faturalama dönemi için toplam ücretleri gösteren ekran görüntüsü](./media/download-azure-invoice/download-invoice-support-plan.png)

## <a name="allow-others-to-download-the-your-subscription-invoice"></a>Başkalarının aboneliğinizin faturasını indirmesine izin verme

Fatura indirmek için:

1.  [Azure portalında](https://portal.azure.com) aboneliğin hesap yöneticisi olarak oturum açın.

2.  **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Portalda maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/download-azure-invoice/search-cmb.png)

3.  Sol taraftan **Faturalar**’ı seçin.

4.  Azure aboneliğinizi seçin ve **Başkalarının fatura indirmesine izin ver**’e tıklayın.

    [![Faturaya erişimin seçilmesini gösteren ekran görüntüsü](./media/download-azure-invoice/cmb-select-access-to-invoice.png)](./media/download-azure-invoice/cmb-select-access-to-invoice-zoomed-in.png#lightbox)
1.  **Açık** seçeneğini, ardından sayfanın üstünde bulunan **Kaydet** seçeneğini belirleyin.  
    ![Faturaya erişim için Açık seçeneğinin belirlenmesini gösteren ekran görüntüsü](./media/download-azure-invoice/cmb-access-to-invoice.png)

## <a name="get-mosp-subscription-invoice-in-email"></a>MOSP abonelik faturasını e-posta ile alma

Faturayı e-posta ile almayı kabul etmek için bir abonelikte veya destek planında hesap yöneticisi rolüne sahip olmanız gerekir. E-posta faturaları yalnızca abonelikler ve destek planları için geçerlidir, rezervasyonlar veya Azure Market satın alımlarında kullanılmaz. Faturayı e-posta ile almayı kabul ettikten sonra, faturayı alacak diğer alıcıları ekleyebilirsiniz.

1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  **Maliyet Yönetimi + Faturalama** araması yapın.  
3.  Sol taraftan **Faturalar**’ı seçin.
4.  Azure aboneliğinizi veya destek planı aboneliğinizi belirleyin ve sonra **Faturayı e-posta ile alın**’ı seçin.  
    [![Faturalama profili listesini gösteren ekran görüntüsü](./media/download-azure-invoice/cmb-email-invoice.png)](./media/download-azure-invoice/cmb-email-invoice-zoomed-in.png#lightbox)
5. **E-posta faturası**’na tıklayın ve koşulları kabul edin.  
    ![Kabul etme akışının 2. adımını gösteren ekran görüntüsü](./media/download-azure-invoice/invoicearticlestep02.png)
6. Fatura, tercih ettiğiniz iletişim e-posta adresinize gönderilir. E-postayı güncelleştirmek için **Profili güncelleştir**’i seçin.  
    ![Kabul etme akışının 3. adımını gösteren ekran görüntüsü](./media/download-azure-invoice/invoicearticlestep03-verifyemail.png)

## <a name="share-subscription-and-support-plan-invoices"></a>Abonelik paylaşma ve destek planı faturaları

Aboneliğinize ve destek planınıza ait faturaları her ay muhasebe ekibinizle paylaşmak veya başka bir e-posta adresinize göndermek isteyebilirsiniz.

1. [Aboneliğinize ve destek planınıza ait faturaları e-posta ile alma](#get-mosp-subscription-invoice-in-email) sayfasındaki adımları izleyin ve **Alıcıları yapılandır**’ı seçin.  
    ![Kullanıcının Alıcıları yapılandır seçeneğini belirlediğin ekran görüntüsü](./media/download-azure-invoice/invoice-article-step03.png)
1. Bir e-posta adresi girin ve **Alıcı ekle**’yi seçin. Birden çok e-posta adresi ekleyebilirsiniz.  
    ![Kullanıcının başka alıcılar eklediğini gösteren ekran görüntüsü](./media/download-azure-invoice/invoice-article-step04.png)
1. Tüm e-posta adreslerini ekledikten sonra, ekranın alt kısmından **Bitti**’yi seçin.

## <a name="invoices-for-mca-and-mpa-billing-accounts"></a>MCA ve MOSP faturalama hesaplarına ait faturalar

Kuruluşunuz, MCA imzalamak için bir Microsoft temsilcisiyle birlikte çalıştığında, MCA faturalama hesabı oluşturulur. Azure web sitesinden [kullandıkça öde fiyatlarına tabi hesaba](https://azure.microsoft.com/offers/ms-azr-0003p/) veya [Ücretsiz Azure Hesabına](https://azure.microsoft.com/offers/ms-azr-0044p/) kaydolan belirli bölgelerdeki bazı müşterilerin MCA için de faturalama hesabı olabilir. Daha fazla bilgi için bkz. [MCA faturalama hesabınızı kullanmaya başlayın](../understand/mca-overview.md).

Müşterilerini yeni ticaret deneyiminde yönetmelerine olanak tanımak amacıyla Bulut Çözümü Sağlayıcısı (CSP) iş ortakları için bir MPA faturalama hesabı oluşturulur. Azure portalındaki faturalama hesabını yönetmek için iş ortaklarının en az bir müşterisi olan bir [Azure planına](https://docs.microsoft.com/partner-center/purchase-azure-plan) sahip olması gerekir. Daha fazla bilgi için bkz. [MPA faturalama hesabınızı kullanmaya başlayın](../understand/mpa-overview.md).

Hesabınızdaki her bir faturalama profili için ay başında aylık bir fatura oluşturulur. Fatura, önceki aya ait tüm Azure aboneliklerinin ve diğer satın alımların ilgili ücretlerini içerir. Örneğin Burak, 5 Mart’ta *Azure sub 01*, 10 Mart’ta *Azure sub 02* aboneliklerini oluşturuyor. *Azure support 01* aboneliğini 28 Mart’ta *Billing profile 01* adlı faturalama profiliyle satın alıyor. Burak, Nisan’ın başında hem Azure aboneliklerine hem de destek planına ait ücretleri içeren tek bir fatura alır.

##  <a name="download-an-mca-or-mpa-billing-profile-invoice"></a>MCA veya MPA faturalama profili faturasını indirme

Faturasını Azure portalından indirmek için faturalama profilinde sahip, katkıda bulunan, okuyucu veya fatura yöneticisi rollerinden birine sahip olmanız gerekir. Faturalama hesabında sahip, katkıda bulunan veya okuyucu rolüne sahip olan kullanıcılar hesaptaki tüm faturalama profiline ait faturaları indirebilir.

1.  [Azure Portal](https://portal.azure.com) oturum açın.

2.  **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Portalda maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/download-azure-invoice/search-cmb.png)

3. Sol taraftan **Faturalar**’ı seçin.

    [![MCA faturalama hesabı için Faturalar sayfasını gösteren ekran görüntüsü](./media/download-azure-invoice/mca-billingprofile-invoices.png)](./media/download-azure-invoice/mca-billingprofile-invoices-zoomed-in.png#lightbox)

4. Faturalar tablosunda, indirmek istediğiniz faturayı seçin.

5. Sayfanın üst kısmındaki **Fatura PDF’sini indir**’e tıklayın.

    [![Faturanın PDF olarak indirilmesini gösteren ekran görüntüsü](./media/download-azure-invoice/mca-billingprofile-download-invoice.png)](./media/download-azure-invoice/mca-billingprofile-download-invoice-zoomed-in.png#lightbox)

6. Ayrıca **Azure kullanımını indir**’e tıklayarak günlük tüketilen miktar dökümünüzü ve tahmini ücretleri de indirebilirsiniz. CSV dosyasının hazırlanması birkaç dakika sürebilir.

## <a name="get-your-billing-profiles-invoice-in-email"></a>Faturalama profilinizin faturasını e-posta ile alma

E-posta fatura tercihini güncelleştirmek için faturalama profilinde veya bu profile ait faturalama hesabında sahip veya katkıda bulunan rolüne sahip olmanız gerekir. Kabul ettikten sonra, faturalama profilinde sahip, katkıda bulunan, okuyucu veya fatura yöneticisi rolüne sahip olan tüm kullanıcılara e-posta ile fatura gönderilir. 

1.  [Azure Portal](https://portal.azure.com) oturum açın.
1.  **Maliyet Yönetimi + Faturalama** araması yapın.  
1.  Sol taraftaki **Faturalar**’ı seçtikten sonra sayfanın üst tarafından **Faturayı E-posta ile Gönder**’i seçin.  
    [![MCA faturalama hesabı için Faturalar sayfasını gösteren ekran görüntüsü](./media/download-azure-invoice/mca-billing-profile-select-email-invoice.png)](./media/download-azure-invoice/mca-billing-profile-select-email-invoice-zoomed.png#lightbox)
1.  Birden fazla faturalama profiliniz varsa bunlardan birini seçin ve ardından **Kabul et**’i seçin.  
    ![MCA faturalama hesabı için Faturalar sayfasını gösteren ekran görüntüsü](./media/download-azure-invoice/mca-billing-profile-email-invoice.png)
1.  **Güncelleştir**’i seçin.

2.  **Maliyet Yönetimi + Faturalama** araması yapın.

    ![Portalda maliyet yönetimi + faturalama aramasını gösteren ekran görüntüsü](./media/download-azure-invoice/search-cmb.png)

3.  Sol taraftaki **Faturalar**’ı seçtikten sonra sayfanın üst tarafından **Faturayı E-posta ile Gönder**’i seçin.

    [![MCA faturalama hesabı için Faturalar sayfasını gösteren ekran görüntüsü](./media/download-azure-invoice/mca-billingprofile-select-emailinvoice.png)](./media/download-azure-invoice/mca-billingprofile-select-emailinvoice-zoomed-in.png)

4.  Birden fazla faturalama profiliniz varsa bunlardan birini seçin ve ardından **Kabul et**’i seçin.

Bir MCA veya MPA faturalama profilinde fatura yöneticisi rolünü atayarak başkalarına faturaları görüntüleme, indirme ve ödeme erişimi verirsiniz. Faturanın e-posta ile gönderilmesini kabul ettiyseniz faturalar bu kullanıcılara da e-posta ile gönderilir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. **Maliyet Yönetimi + Faturalama** araması yapın.  
1. Soldaki **Faturalama profilleri** seçeneğini belirleyin. Faturalama profilleri listesinden fatura yöneticisi rolünü atamak istediğiniz faturalama profilini seçin.  
   ![Faturalama profili listesini gösteren ekran görüntüsü](./media/download-azure-invoice/mca-select-profile-zoomed-in.png)
1. Sol taraftan **Erişim Denetimi (IAM)** girişini seçtikten sonra sayfanın üst tarafından **Ekle**'yi seçin.  
    ![Erişim denetimi sayfasını gösteren ekran görüntüsü](./media/download-azure-invoice/mca-select-access-control-zoomed-in.png)
1. Rol açılan listesinde **Fatura Yöneticisi**'ni seçin. Erişim izni vermek için kullanıcının e-posta adresini girin. Rolü atamak için **Kaydet**’i seçin.  
   ![Bir kullanıcıyı fatura yöneticisi olarak eklemeyi gösteren ekran görüntüsü](./media/download-azure-invoice/mca-added-invoice-manager.png)

1. **Maliyet Yönetimi + Faturalama** araması yapın.

   ![Portalda abonelik aramasını gösteren ekran görüntüsü](./media/download-azure-invoice/search-cmb.png)

1. Soldaki **Faturalama profilleri** seçeneğini belirleyin. Faturalama profilleri listesinden fatura yöneticisi rolünü atamak istediğiniz faturalama profilini seçin.

   ![Faturalama profili listesini gösteren ekran görüntüsü](./media/download-azure-invoice/mca-select-profile-zoomed-in.png)

1. Sol taraftan **Erişim Denetimi (IAM)** girişini seçtikten sonra sayfanın üst tarafından **Ekle**'yi seçin.

   [![Erişim denetimi sayfasını gösteren ekran görüntüsü](./media/download-azure-invoice/mca-select-access-control-zoomed-in.png)

1. Rol açılan listesinde **Fatura Yöneticisi**'ni seçin. Erişim izni vermek için kullanıcının e-posta adresini girin. Rolü atamak için **Kaydet**’i seçin.

   [![Bir kullanıcıyı fatura yöneticisi olarak eklemeyi gösteren ekran görüntüsü](./media/download-azure-invoice/mca-added-invoice-manager.png)](./media/download-azure-invoice/mca-added-invoice-manager.png#lightbox)
   
   
##  <a name="why-you-might-not-see-an-invoice"></a>Fatura neden göremiyorsunuz?

<a name="noinvoice"></a>

Bir faturayı görmemenizin birden fazla nedeni olabilir:

- Fatura henüz hazır değil
    
    - Azure'a abone olmanızın üzerinden 30 gün geçmemiştir. 

    - Azure, fatura döneminizin bitişinden birkaç gün sonra faturanızı iletir. Bu nedenle fatura henüz oluşturulmamış olabilir.

- Faturaları görüntüleme izniniz yoktur. 
    
    - MCA veya MPA faturalama hesabınız varsa faturaları görüntülemek için bir faturalama profilinde Sahip, Katkıda Bulunan, Okuyucu veya Fatura Yöneticisi rolüne veya faturalama hesabında Sahip, Katkıda Bulunan veya Okuyucu rolüne sahip olmanız gerekir. 
    
    - Diğer faturalama hesapları için Hesap Yöneticisi değilseniz faturaları göremeyebilirsiniz.

- Hesabınız faturayı desteklemiyor.

    - Microsoft Çevrimiçi Abonelik Programı (MOSP) için faturalama hesabınız varsa ve Azure Ücretsiz Hesabına veya aylık kredi miktarına sahip bir aboneliğe kaydolduysanız yalnızca aylık kredi miktarı kullanımını aştığınızda fatura alırsınız.

    - Microsoft Müşteri Sözleşmesi (MCA) veya Microsoft İş Ortağı Sözleşmesi (MPA) için bir faturalama hesabınız varsa her zaman bir fatura alırsınız.

- Diğer hesaplarınızdan biri üzerinden faturaya erişebilirsiniz.

    - Bu durum genellikle, portaldaki faturanızı görüntülemenizi isteyen bir e-posta bağlantısına tıkladığınızda olur. Bağlantıya tıkladığınızda şu hata iletisini görürsünüz: `We can't display your invoices. Please try again`. Faturaları görme izinlerine sahip bir e-posta adresiyle oturum açtığınızı doğrulayın.

- Farklı bir kimlik üzerinden faturaya erişebilirsiniz. 

    - Bazı müşterilerin aynı e-posta adresinde iki kimliği vardır: iş hesabı ve Microsoft hesabı. Genellikle, kimliklerden yalnızca birinin faturaları görüntüleme izni vardır. İzni olmayan kimlikle oturum açarlarsa faturaları göremezler. Oturum açmak için doğru kimliği kullandığınızı doğrulayın.

- Yanlış Azure Active Directory (AAD) kiracısında oturum açtınız. 

    - Faturalama hesabınız bir AAD kiracısıyla ilişkili. Yanlış bir kiracıda oturum açtıysanız faturalama hesabınızdaki aboneliklerin faturasını görmezsiniz. Doğru Azure Active Directory (AAD) kiracısında oturum açtığınızı doğrulayın. Doğru kiracıda oturum açmadıysanız Azure portalından kiracıyı değiştirmek için aşağıdakileri kullanın:

        1. Sayfanın sağ üst kısmından e-postanızı seçin.

        2. **Dizini değiştir**’i seçin.

           ![Portalda dizini değiştirme adımının seçildiğini gösteren ekran görüntüsü](./media/download-azure-invoice/select-switch-directory.png)

        3. **Tüm dizinler** bölümünden bir dizin seçin.

           ![Portalda dizin seçme adımını gösteren ekran görüntüsü](./media/download-azure-invoice/select-directory.png)

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bize ulaşın.

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Faturanız ve ücretleriniz hakkında daha fazla bilgi edinmek için bkz:

- [Microsoft Azure kullanımınızı ve ücretlerinizi görüntüleme ve indirme](download-azure-daily-usage.md)
- [Microsoft Azure faturanızı anlama](review-individual-bill.md)
- [Azure faturanızdaki terimleri anlama](understand-invoice.md)

MCA’ya sahipseniz bkz.

- [Faturalama profilinizin faturasındaki ücretleri anlama](review-customer-agreement-bill.md)
- [Faturalama profilinizin faturasındaki terimleri anlama](mca-understand-your-invoice.md)
- [Faturalama profilinizin Azure kullanımı ve ücretleri dosyasını anlama](mca-understand-your-usage.md)
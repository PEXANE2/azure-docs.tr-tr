---
title: Ticari Market 'te Azure uygulamaları teklifi için gözden geçirme geri bildirimini işleme
description: Azure uygulamaları için, Microsoft Iş Ortağı Merkezi 'nde ticari Market portalı 'nı kullanarak Azure Market, AppSource veya bulut çözümü sağlayıcısı (CSP) aracılığıyla listeleme veya satma için gözden geçirme ile ilgili geri bildirimleri ele alma.
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/11/2019
ms.openlocfilehash: 7406a6e35929e3df5d342affbcb4b740efa0cab8
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74281226"
---
# <a name="handling-review-feedback"></a>Gözden geçirme geri bildirimini işleme

Bu makalede, Microsoft Azure Market gözden geçirme ekibi tarafından kullanılan Azure DevOps ortamına nasıl erişebileceğiniz açıklanır. **Microsoft gözden geçirme** adımı sırasında Azure Uygulama teklifinizdeki kritik sorunlar bulunursa, bu sorunlarla ilgili ayrıntılı bilgileri görüntülemek için bu sistemde oturum açabilirsiniz (geri bildirim gözden geçirin). Tüm bu sorunları düzelttikten sonra, Azure Marketi 'nde yayımlamaya devam etmek için teklifinizi yeniden göndermeniz gerekir. Aşağıdaki diyagramda, bu geri bildirim işleminin yayımlama işlemiyle nasıl ilişkili olduğu gösterilmektedir.

![Geri bildirim işlemini gözden geçir](./media/review-feedback-process.png)

Genellikle, gözden geçirme sorunları çekme isteği (PR) olarak başvurulur. Her bir çekme isteği, sorunla ilgili ayrıntıları içeren çevrimiçi bir [Azure DevOps](https://azure.microsoft.com/services/devops/) (daha önce adlandırılmış VISUAL STUDIO Team SERVICES (VSTS)) öğesine bağlıdır. Aşağıdaki görüntüde, İncelemeler sırasında sorunlar bulunursa Iş Ortağı Merkezi deneyiminin bir örneği görüntülenir. 

![Yayın durumu](./media/publishing-status.png)

Gönderim hakkındaki belirli ayrıntıları içeren PR, "sertifika raporunu görüntüle" bağlantısında bahsedilir. Karmaşık durumlarda, gözden geçirme ve destek takımları da size e-posta verebilir.

## <a name="azure-devops-access"></a>Azure DevOps erişimi

Iş Ortağı Merkezi 'nde "Geliştirici" rolüne erişimi olan tüm kullanıcıların, gözden geçirme geri bildirimde başvurulan PR öğelerini görüntüleme erişimi olur.

<!---
To view the PR items referenced in review feedback, publishers must first be granted proper authorization. Otherwise, new publishers receive a 401 - Not Authorized response page when trying to view PRs. To request access to this Azure DevOps repository, perform the following steps:

1. Collect the following information:
    * Your publisher name and ID
    * Offer type (Azure app), offer name, and SKU ID
    * The pull request link, for example: `https://solutiontemplates.visualstudio.com/marketplacesolutions/_git/contoso/pullrequest/<number>`. This URL can be retrieved from the notification message or the address of the 401 response page.
    * The email address(es) of the individuals from your publishing organization that you want access granted to. This list should include the owner address(es) you provided when registering as a publisher on the Cloud Partner Portal (or Partner Center??).
1. Create a support incident. In the title bar of the Cloud Partner Portal (??), select the **Help** button, then choose **Support** from the menu. Your default web browser should launch and navigate to the Microsoft new support incident page. (You may have to sign in first.)
1. Specify the **Problem type** as **marketplace onboarding** and **Category** as **Access problem**, then select **Start request**.

    ![Create an incident](./media/create-an-incident.png)

1. In **Step 1 of 2** page, supply your contact information and select **Continue**.
1. In **Step 2 of 2** page, specify an incident title (for example Request Azure DevOps access) and supply the information you collected in the first step (above). Read and accept the agreement, then select **Submit**.

If the incident creation was successful, a confirmation page is displayed. Save the confirmation information on this page for your reference. The Microsoft Support Team should reply to your access request within a few business days. --->

## <a name="reviewing-the-pull-request"></a>Çekme isteğini gözden geçirme

Çekme isteğinde belgelenen sorunları gözden geçirmek için aşağıdaki yordamı kullanın.

1. Yayımlama adımlarının **Microsoft gözden geçirme** bölümlerinde, tarayıcınızı başlatmak IÇIN bir PR bağlantısına tıklayın ve bu çekme Isteği Için **genel bakış** (giriş) sayfasına gidin. Aşağıdaki görüntüde, contoso örnek uygulama teklifinin kritik sorun giriş sayfasına bir örnek gösterilmektedir. Bu sayfa, Azure uygulamasında bulunan gözden geçirme sorunlarıyla ilgili yararlı Özet bilgiler içerir.

    [çekme isteği giriş sayfası
   ![](./media/pr-home-page-thumb.png)](./media/pr-home-page.png)  <br/> *Genişletilecek resme tıklayın.*

1. Seçim Pencerenin sağ tarafında, bölüm **ilkeleri**' nde, sorunun alt düzey ayrıntılarını araştırmak için, ilgili günlük dosyaları da dahil olmak üzere, sorun iletisi ' ne (Bu örnekte, **ilke doğrulaması başarısız oldu**) tıklayın. Hatalar genellikle günlük dosyalarının altında görüntülenir.
1. Ana sayfanın sol tarafındaki menüde, bu teklif için teknik varlıkları oluşturan liste dosyalarını göstermek için **dosyalar** ' ı seçin. Microsoft gözden geçirenler, bulunan kritik sorunları açıklayan açıklamalar eklemiş olmalıdır. Aşağıdaki örnekte, iki sorun keşfedilmiştir.

    [çekme isteği giriş sayfası
   ![](./media/pr-files-page-thumb.png)](./media/pr-files-page.png)  <br/> *Genişletilecek resme tıklayın.*

1. Çevredeki kodun bağlamında açıklamaya gitmek için sol ağaçtaki her bir açıklama düğümüne tıklayın. Açıklama tarafından açıklanan sorunu gidermek için takımınızın projesindeki kaynak kodunuzu düzeltin.

>[!Note]
>Teklifinizin teknik varlıklarını gözden geçirme ekibinin Azure DevOps ortamında düzenleyemezsiniz. Yayımcılar için, bu, içerilen kaynak kodu için salt okunurdur. Ancak, Microsoft gözden geçirme ekibinin avantajı için açıklamalara yanıt verebilirsiniz.

   Aşağıdaki örnekte, yayımcı ilk sorunu inceetti, düzeltti ve yanıtladı.

   ![İlk düzelme ve açıklama yanıtı](./media/first-comment-reply.png)

## <a name="next-steps"></a>Sonraki adımlar

Gözden geçirme çekme isteği 'nde belgelenen kritik sorunları düzelttikten sonra [Azure uygulama teklifinizi yeniden yayımlamanız](./create-new-azure-apps-offer.md#publish)gerekir.

---
title: Ticari Pazar'da Azure Apps teklifi için inceleme geri bildirimi işleme
description: Microsoft İş Ortağı Merkezi'ndeki Ticari Pazar Portalı'nı kullanarak Azure Marketi, AppSource veya Bulut Çözüm Sağlayıcısı (CSP) programı aracılığıyla giriş yapmak veya satmak için Azure Apps teklifiiçin gözden geçirme geri bildiriminasıl işlenir?
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/11/2019
ms.openlocfilehash: 3932f3fc71dc6427b6cdf93d3a7bc58534a9981c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80279802"
---
# <a name="handling-review-feedback"></a>Gözden geçirme geri bildirimini işleme

Bu makalede, Microsoft Azure Marketi inceleme ekibi tarafından kullanılan Azure DevOps ortamına nasıl erişilen açıklanmaktadır. **Microsoft inceleme** adımı sırasında Azure uygulama teklifinizde kritik sorunlar bulunursa, bu sorunlarla ilgili ayrıntılı bilgileri görüntülemek için bu sistemde oturum açabilirsiniz (geri bildirimi gözden geçirin). Tüm bu sorunları giderdikten sonra, teklifiniAzure Marketi'nde yayımlamaya devam etmek için yeniden göndermeniz gerekir. Aşağıdaki diyagram, bu geri bildirim işleminin yayımlama işlemiyle nasıl ilişkili olduğunu göstermektedir.

![Geri bildirim sürecini gözden geçirme](./media/review-feedback-process.png)

Genellikle, gözden geçirme sorunları çekme isteği (PR) olarak başvurur. Her PR, konuyla ilgili ayrıntıları içeren çevrimiçi bir [Azure DevOps](https://azure.microsoft.com/services/devops/) (daha önce Visual Studio Team Services (VSTS) olarak adlandırılır. Aşağıdaki resimde, incelemeler sırasında sorunlar bulunursa İş Ortağı Merkezi deneyiminin bir örneği görüntülenir. 

![Yayın durumu](./media/publishing-status.png)

Gönderim le ilgili özel ayrıntıları içeren PR, "Sertifika Raporunu Görüntüle" bağlantısında belirtilecektir. Karmaşık durumlar için, inceleme ve destek ekipleri de size e-posta gönderebilirsiniz.

## <a name="azure-devops-access"></a>Azure DevOps erişimi

İş Ortağı Merkezi'ndeki "geliştirici" rolüne erişimi olan tüm kullanıcılar, gözden geçirme geri bildiriminde başvurulan PR öğelerini görüntüleme erişimine sahip olacaktır.

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

1. Yayımlama adımları formunun **Microsoft inceleme** bölümlerinde, tarayıcınızı başlatmak için bir PR bağlantısını tıklayın ve bu PR için **Genel Bakış** (ana sayfa) sayfasına gidin. Aşağıdaki resimde, Contoso örnek uygulama teklifi için kritik sorun ana sayfasının bir örneği gösterilmiştir. Bu sayfa, Azure uygulamasında bulunan inceleme sorunları hakkında yararlı özet bilgiler içerir.

    [![İstek giriş sayfasını çekme](./media/pr-home-page-thumb.png)](./media/pr-home-page.png)
    <br/> *Genişletmek için resmin üzerine tıklayın.*

1. (İsteğe bağlı) Pencerenin sağ tarafında, **İlkeler**bölümünde, ilgili günlük dosyaları da dahil olmak üzere sorunun alt düzey ayrıntılarını araştırmak için sorun iletisini (bu örnekte: **İlke Doğrulama başarısız oldu)** tıklatın. Hatalar genellikle günlük dosyalarının alt kısmında görüntülenir.
1. Ana sayfanın sol tarafındaki menüde, bu teklifin teknik varlıklarını oluşturan liste dosyalarını görüntülemek için **Dosyalar'ı** seçin. Microsoft gözden geçirenler, keşfedilen kritik sorunları açıklayan yorumlar eklemeli. Aşağıdaki örnekte, iki sorun keşfedilmiştir.

    [![İstek giriş sayfasını çekme](./media/pr-files-page-thumb.png)](./media/pr-files-page.png)
    <br/> *Genişletmek için resmin üzerine tıklayın.*

1. Çevreleyen kod bağlamında yoruma gitmek için sol ağaçtaki her yorum düğümüne tıklayın. Yorumtarafından açıklanan sorunu düzeltmek için ekibinizin projesinde kaynak kodunuzu düzeltin.

>[!Note]
>Teklifinizin teknik varlıklarını inceleme ekibinin Azure DevOps ortamında yapamazsınız. Yayımcılar için bu, içerdiği kaynak kodu için salt okunur bir ortamdır. Ancak, Microsoft inceleme ekibinin yararına yorumlara yanıt bırakabilirsiniz.

   Aşağıdaki örnekte, yayımcı ilk sorunu gözden geçirdi, düzeltmiş ve yanıtlamıştır.

   ![İlk düzeltme ve yorum yanıtı](./media/first-comment-reply.png)

## <a name="next-steps"></a>Sonraki adımlar

İnceleme PR(leri) belgelenen kritik sorunları düzelttinsonra, [Azure uygulama teklifinizi yeniden](./create-new-azure-apps-offer.md#publish)yayımlamalısınız.

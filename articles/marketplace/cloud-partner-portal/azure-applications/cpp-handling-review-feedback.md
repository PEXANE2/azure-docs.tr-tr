---
title: Azure uygulama inceleme geri bildirimini işleme | Azure Marketi
description: Azure Marketi için Azure uygulama teklifleri için gözden geçirme geri bildirimlerini işlemek için Azure DevOps'lerin nasıl kullanılacağını açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 02/05/2019
ms.author: dsindona
ms.openlocfilehash: b5f290bae908ac753801eef2c9b8394ca1bb7a40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285308"
---
# <a name="handling-review-feedback"></a>Gözden geçirme geri bildirimini işleme

Bu makalede, Microsoft Azure Marketi inceleme ekibi tarafından kullanılan Azure DevOps ortamına nasıl erişilen açıklanmaktadır.  **Microsoft inceleme** adımı sırasında Azure uygulama teklifinizde kritik sorunlar bulunursa, bu sorunlarla ilgili ayrıntılı bilgileri görüntülemek için bu sistemde oturum açabilirsiniz (geri bildirimi gözden geçirin).  Tüm bu sorunları giderdikten sonra, teklifiniAzure Marketi'nde yayımlamaya devam etmek için yeniden göndermeniz gerekir.  Aşağıdaki diyagram, bu geri bildirim işleminin yayımlama işlemiyle nasıl ilişkili olduğunu göstermektedir.

![Azure DevOps geri bildirimi yle adımları yayımlama](./media/pub-flow-vsts-access.png)

Genellikle, gözden geçirme sorunları çekme isteği (PR) olarak başvurur.  Her PR, konuyla ilgili ayrıntıları içeren çevrimiçi bir [Azure DevOps](https://azure.microsoft.com/services/devops/) (daha önce Visual Studio Team Services (VSTS) olarak adlandırılır.  Aşağıdaki resimde bir gözden geçirme PR başvurusu örneği görüntülenir.  Karmaşık durumlar için, inceleme ve destek ekipleri de size e-posta gönderebilirsiniz. 

![İnceleme geri bildirimini görüntüleyen durum sekmesi](./media/status-tab-ms-review.png)


## <a name="azure-devops-access"></a>Azure DevOps erişimi

İnceleme geri bildiriminde başvurulan PR öğelerini görüntülemek için öncelikle yayıncılara uygun yetkilendirme nin verilmesi gerekir.  Aksi takdirde, yeni `401 - Not Authorized` yayımcılar PR'ları görüntülemeye çalışırken bir yanıt sayfası alır.  Bu Azure DevOps deposuna erişim isteğinde bulunmak için aşağıdaki adımları gerçekleştirin:

1. Aşağıdaki bilgileri toplayın:
    - Yayıncı adınız ve kimliğiniz
    - Teklif türü (Azure uygulaması), teklif adı ve SKU Kimliği
    - Çekme isteği bağlantısı, örneğin: `https://solutiontemplates.visualstudio.com/marketplacesolutions/_git/contoso/pullrequest/<number>` Bu URL bildirim iletisinden veya 401 yanıt sayfasının adresinden alınabilir.
    - Erişim izni verilmesini istediğiniz yayımkuruluşunuzdan kişilerin e-posta adresi(es).  Bu liste, Bulut İş Ortağı Portalı'na yayımcı olarak kaydolurken sağladığınız sahip adresini(es) içermelidir.
2. Bir destek olayı oluşturun.  Bulut İş Ortağı Portalı'nın başlık çubuğunda **Yardım düğmesini** seçin ve menüden **Destek'i** seçin.  Varsayılan web tarayıcısı başlatmak ve Microsoft yeni destek olay sayfasına gitmek gerekir.  (Önce oturum açmanız gerekebilir.)
3. Sorun **türünü** **pazar daki biniş** ve **Kategori** olarak **Access sorunu**olarak belirtin, ardından Başlat **isteğini**seçin.

    ![Destek bileti kategorisi](./media/support-incident1.png)

4. **Adım 1 2** sayfa, iletişim bilgilerinizi kaynağı ve **Devam'ı**seçin.
5. **2 sayfanın 2.adımında,** bir olay `Request Azure DevOps access`başlığı belirtin (örneğin) ve ilk adımda (yukarıda) topladığınız bilgileri toplayın.  Anlaşmayı okuyun ve kabul edin, ardından **Gönder'i**seçin.

Olay oluşturma başarılı olduysa, bir onay sayfası görüntülenir.  Onay bilgilerini referansınız için bu sayfaya kaydedin.  Microsoft Destek Ekibi erişim isteğinizi birkaç iş günü içinde yanıtlamalıdır.


## <a name="reviewing-the-pull-request"></a>Çekme isteğini gözden geçirme 

Çekme isteğinde belgelenen sorunları gözden geçirmek için aşağıdaki yordamı kullanın.

1. **Yayımlama adımları** formunun **Microsoft inceleme** bölümünde, tarayıcınızı başlatmak için bir PR bağlantısını tıklayın ve bu PR için Genel **Bakış** (ana sayfa) sayfasına gidin.  Aşağıdaki resimde, Contoso örnek uygulama teklifi için örnek bir kritik konu giriş sayfası gösterilmiştir.  Bu sayfa, Azure uygulamasında bulunan inceleme sorunları hakkında yararlı özet bilgiler içerir.  

    [![İstek giriş sayfasını çekme](./media/pr-home-page-thumb.png)](./media/pr-home-page.png)
    <br/> *Genişletmek için resmin üzerine tıklayın.*
    
2. (İsteğe bağlı) Pencerenin sağ tarafında, **İlkeler**bölümünde, ilgili günlük dosyaları da dahil olmak üzere sorunun alt düzey ayrıntılarını araştırmak için sorun iletisini (bu örnekte: **İlke Doğrulama başarısız oldu)** tıklatın.  Hatalar genellikle günlük dosyalarının alt kısmında görüntülenir.

3. Ana sayfanın sol tarafındaki menüde, bu teklifin teknik varlıklarını oluşturan liste dosyalarını görüntülemek için **Dosyalar'ı** seçin.  Microsoft gözden geçirenler, keşfedilen kritik sorunları açıklayan yorumlar eklemeli.  Aşağıdaki örnekte, iki sorun keşfedilmiştir. 

    [![İstek giriş sayfasını çekme](./media/pr-files-page-thumb.png)](./media/pr-files-page.png)
    <br/> *Genişletmek için resmin üzerine tıklayın.*

4. Çevreleyen kod bağlamında yoruma gitmek için sol ağaçtaki her yorum düğümüne tıklayın.  Yorumtarafından açıklanan sorunu düzeltmek için ekibinizin projesinde kaynak kodunuzu düzeltin.

> [!Note]
> Teklifinizin teknik varlıklarını inceleme ekibinin Azure DevOps ortamında yapamazsınız.  Yayımcılar için bu, içerdiği kaynak kodu için salt okunur bir ortamdır.  Ancak, Microsoft inceleme ekibinin yararına yorumlara yanıt bırakabilirsiniz.

   Aşağıdaki örnekte, yayımcı ilk sorunu gözden geçirdi, düzeltmiş ve yanıtlamıştır.

   ![İlk düzeltme ve yorum yanıtı](./media/first-comment-reply.png)


## <a name="next-steps"></a>Sonraki adımlar

İnceleme PR(leri) belgelenen kritik sorunları düzelttinsonra, [Azure uygulama teklifinizi yeniden](./cpp-publish-offer.md)yayımlamalısınız.

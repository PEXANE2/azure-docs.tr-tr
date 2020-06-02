---
title: Portalı kullanarak rota sunucusu ile Exchange iş ortakları için eşleme bağlantısı
titleSuffix: Azure
description: Azure portal kullanarak yönlendirme sunucusu ile bir Exchange eşlemesi oluşturun veya değiştirin
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: article
ms.date: 5/19/2020
ms.author: derekol
ms.openlocfilehash: 004479d7aa3ec46da4790c0d8dfa698c6d9fb986
ms.sourcegitcommit: 8017209cc9d8a825cc404df852c8dc02f74d584b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84249497"
---
# <a name="create-or-modify-an-exchange-peering-with-route-server-in-azure-portal"></a>Azure portal 'de rota sunucusuyla bir Exchange eşlemesi oluşturun veya değiştirin

Bu makalede, Azure portal kullanarak bir rota sunucusuyla Microsoft Exchange eşlemesi oluşturma açıklanmaktadır. Bu makalede ayrıca kaynağın durumunu denetleme, güncelleştirme veya silme ve sağlamayı kaldırma işlemlerinin nasıl yapılacağı gösterilir.


## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [Exchange eşleme](walkthrough-exchange-all.md) Kılavuzu ' nu gözden geçirin.
* Azure kaynaklarına Dönüştürülmeyen Microsoft ile Exchange eşlemeleri zaten varsa, bkz. [portalı kullanarak eski Exchange eşlemesini bir Azure kaynağına dönüştürme](howto-legacy-exchange-portal.md).

## <a name="create-and-provision-an-exchange-peering"></a>Bir Exchange eşlemesi oluşturma ve sağlama

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-an-exchange-peering-with-route-server"></a><a name=create></a>Rota sunucusuyla bir Exchange eşlemesi oluşturma


Bir Internet Exchange sağlayıcısı olarak, [eşleme oluşturarak]( https://go.microsoft.com/fwlink/?linkid=2129593)bir Exchange eşleme isteği oluşturabilirsiniz.

1. **Eşleme oluştur** sayfasında, **temel bilgiler** sekmesinde, kutulara aşağıda gösterildiği gibi kutuyu girin:

    > [!div class="mx-imgBorder"] 
    > ![Eşleme Hizmeti'ni kaydetme](./media/setup-basics-tab.png)

* Azure aboneliğinizi seçin.

* Kaynak grubu için, açılan listeden var olan bir kaynak grubunu seçebilir veya yeni oluştur ' u seçerek yeni bir grup oluşturabilirsiniz. Bu örnek için yeni bir kaynak grubu oluşturacağız.

* Ad, kaynak adına karşılık gelir ve seçtiğiniz herhangi bir şey olabilir.

* Mevcut bir kaynak grubunu seçerseniz bölge otomatik olarak seçilir. Yeni bir kaynak grubu oluşturmayı seçerseniz, kaynağın bulunmasını istediğiniz Azure bölgesini de seçmeniz gerekir.

    >[!NOTE]
    >Kaynak grubunun bulunduğu bölge, Microsoft ile eşleme oluşturmak istediğiniz konumdan bağımsızdır. Ancak, en yakın Azure bölgelerinde bulunan kaynak grupları içindeki eşleme kaynaklarınızı düzenlemek en iyi uygulamadır. Örneğin, Ashburn içindeki eşlemeler için Doğu ABD veya Doğu ABD2 içinde bir kaynak grubu oluşturabilirsiniz.

* **Peerasn** kutusunda ASN 'nizi seçin.

    >[!IMPORTANT] 
    >Bir eşleme isteği göndermeden önce, yalnızca onay durumu ile bir ASN seçebilirsiniz. PeerAsn isteğinizi gönderdikten sonra, ASN ilişkisinin onaylanabilmesi için 12 saat bekleyin. Seçtiğiniz ASN, doğrulama bekliyor ise, bir hata iletisi görürsünüz. Seçmeniz gereken ASN 'yi görmüyorsanız doğru aboneliği seçtiğinizden emin olun. Bu durumda, **[EŞDÜZEY ASN 'Yi Azure aboneliğine ilişkilendir](https://go.microsoft.com/fwlink/?linkid=2129592)**' i kullanarak peerasn 'yi zaten oluşturmuş olup olmadığınızı kontrol edin.

* **İleri ' yi seçin:** devam etmek için yapılandırma.

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme

1. Eşleme Oluştur sayfasında, yapılandırma sekmesinde, kutulara aşağıda gösterildiği gibi kutuyu girin:

    > [!div class="mx-imgBorder"]
    > ![Rota sunucusunu yapılandırma](./media/setup-exchange-conf-tab-routeserver.png)
 
    * Eşleme türü için **doğrudan** ' yi seçin
    * Microsoft ağı için **Exchange Route Server Ile AS8075**' yi seçin. 
    * SKU 'YU **temel ücretsiz**olarak seçin. Özel uygulamalar için ayrıldığından Premium ücretsiz olarak seçmeyin.
    * Eşlemeyi ayarlamak istediğiniz **Metro** konumunu seçin.

1. **Eşleme bağlantıları**altında **Yeni oluştur** ' u seçin.

1.  **Doğrudan eşleme bağlantısı**altında aşağıdaki BGP oturumu ayrıntılarını girin:

    > [!div class="mx-imgBorder"]
    > ![Doğrudan eşleme yolu sunucusunu yapılandırma](./media/setup-exchange-conf-tab-direct-route.png)


     * Eşleme tesisi, eşleme için uygun fiziksel konumu seçin
     * Oturum adresi sağlayıcısı, eşdüzey seçin
     * Oturum IPv4 öneki, Exchange sağlayıcısı eşi tarafından sağlanacak
     * Eş oturum IPv4 adresi, yönlendirme sunucusunun IP öneki aralığından Exchange eşi tarafından seçilir.
     * Microsoft oturum IPv4 adresi, IP öneki aralığından ayrılan yönlendirici IP 'si olacaktır.
     * Oturum IPv6 Şu anda isteğe bağlıdır.
     * En fazla tanıtılan IPv4 öneki 20000 kadar olabilir. 
     * Eşleme hizmeti için kullanım varsayılan olarak devre dışıdır. Exchange sağlayıcısı, Microsoft ile bir eşleme hizmeti sözleşmesi imzalandıktan sonra etkinleştirilebilir.

1. Tamamlandıktan sonra **Kaydet**' e tıklayın. 

1. Eşleme Oluştur altında, doğrulama geçildi ' i görürsünüz. Doğrulama başarılı olduktan sonra **Oluştur** ' a tıklayın.

    > [!div class="mx-imgBorder"]
    > ![Ayarların doğrulanması](./media/setup-exchange-conf-tab-validation.png)

    >[!NOTE]
    >Microsoft eşleme hizmeti ortağı olan normal Internet hizmet sağlayıcıları (ISS) için müşteri IP ön eki kaydı gereklidir. Ancak, bir rota sunucusuyla Exchange iş ortakları söz konusu olduğunda, ön ekleri değil, müşteri güvenlerine kaydolmanız gerekir. Müşterinin ön ek kaydı için aynı ASN anahtarı geçerli olacaktır.

1. Ayarlar bölümü altında **kayıtlı ASNs** ' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Doğrudan eşleme yolu sunucusunu yapılandırma](./media/setup-exchange-registered-asn.png)

1. Exchange Aboneliğiniz kapsamında yeni bir müşteri ASN oluşturmak için **KAYıTLı ASN Ekle** ' yi seçin.

    > [!div class="mx-imgBorder"]
    > ![Doğrudan eşleme yolu sunucusunu yapılandırma](./media/setup-exchange-register-new-asn.png)

1. ASN Kaydet altında bir ad seçin, müşteri ASN 'yi doldurun ve Kaydet ' e tıklayın.

1. Kayıtlı ASNs altında, her ASN 'ye atanan ilişkili bir önek anahtarı olacaktır. Bir Exchange sağlayıcısı olarak, bu önek anahtarını, abonelikleri kapsamında eşleme hizmetini kaydedebilmeleri için müşterinize sağlamanız gerekir.

    > [!div class="mx-imgBorder"]
    > ![Doğrudan eşleme yolu sunucusunu yapılandırma](./media/setup-exchange-register-asn-prefixkey.png)




### <a name="verify-an-exchange-peering"></a><a name=get></a>Bir Exchange eşlemesini doğrulama
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="modify-an-exchange-peering"></a><a name="modify"></a>Bir Exchange eşlemesini değiştirme
[!INCLUDE [peering-exchange-modify-portal](./includes/exchange-portal-modify.md)]

## <a name="deprovision-an-exchange-peering"></a><a name="delete"></a>Bir Exchange eşlemesi sağlamayı kaldırma
[!INCLUDE [peering-exchange-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak doğrudan eşleme oluşturma veya değiştirme](howto-direct-portal.md)
* [Portalı kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için bkz. [Internet eşlemesi SSS](faqs.md).

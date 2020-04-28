---
title: Portalı kullanarak eşdüzey ASN’yi Azure aboneliğiyle ilişkilendirme
titleSuffix: Azure
description: Portalı kullanarak eşdüzey ASN’yi Azure aboneliğiyle ilişkilendirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: cee548aff49cd5e4a57eed994b8ade2d157c6313
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75912216"
---
# <a name="associate-peer-asn-to-azure-subscription-using-the-portal"></a>Portalı kullanarak eşdüzey ASN’yi Azure aboneliğiyle ilişkilendirme

Bir eşleme isteği göndermeden önce, aşağıdaki adımları kullanarak ASN 'nizi Azure aboneliğiyle ilişkilendirmeniz gerekir.

İsterseniz, [PowerShell](howto-subscription-association-powershell.md)'i kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="create-peerasn-to-associate-your-asn-with-azure-subscription"></a>ASN 'nizi Azure aboneliğiyle ilişkilendirmek için PeerAsn oluşturun

### <a name="sign-in-to-the-portal"></a>Portalda oturum açın
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="register-for-peering-resource-provider"></a>Eşleme kaynak sağlayıcısı için kaydolun
Aşağıdaki adımları izleyerek aboneliğinizde eşleme kaynak sağlayıcısına kaydolun. Bunu yürütütemezsiniz, eşlemeyi ayarlamak için gereken Azure kaynaklarına erişilemez.

1. Portalın sol üst köşesindeki **abonelikler** ' e tıklayın. Bunu görmüyorsanız, **daha fazla hizmet** ' e tıklayın ve arama yapın.

    > [!div class="mx-imgBorder"]
    > ![Açık abonelikler](./media/rp-subscriptions-open.png)

1. Eşleme için kullanmak istediğiniz aboneliğe tıklayın.

    > [!div class="mx-imgBorder"]
    > ![Aboneliği Başlat](./media/rp-subscriptions-launch.png)

1. Abonelik açıldıktan sonra, sol taraftaki **kaynak sağlayıcıları**' na tıklayın. Sonra sağ bölmede, ara penceresinde *eşleme* araması yapın veya **Microsoft. eşleme** ' yi bulmak için kaydırma çubuğunu kullanın ve **duruma**bakın. Durum ***kayıtlıysa***, aşağıdaki adımları atlayın ve bölüm **peerasn oluşturun**bölümüne ilerleyin. Durum ***Notregistered***Ise, **Microsoft. eşleme** ' yi seçin ve **Kaydet**' e tıklayın.

    > [!div class="mx-imgBorder"]
    > ![Kayıt başlangıcı](./media/rp-register-start.png)

1. Durumunun ***kayıt***olarak değiştiğini gözlemleyin.

    > [!div class="mx-imgBorder"]
    > ![Kayıt devam ediyor](./media/rp-register-progress.png)

1. Kayıt işleminin tamamlanabilmesi için en az bir dakika bekleyin. Ardından **Yenile** ' ye tıklayın ve durumun ***kayıtlı***olduğunu doğrulayın.

    > [!div class="mx-imgBorder"]
    > ![Kayıt tamamlandı](./media/rp-register-completed.png)

### <a name="create-peerasn"></a>PeerAsn oluştur
Bir otonom sistem numarası 'nı (ASN) Azure aboneliğiyle ilişkilendirmek için yeni bir PeerAsn kaynağı oluşturabilirsiniz. İlişkilendirmeniz gereken her ASN için bir **Peerasn** oluşturarak, birden fazla ASNs 'yi bir abonelikle ilişkilendirebilirsiniz.

1. **Kaynak** > oluştur ' a tıklayarak**tümünü görüntüleyin**.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn 'de ara](./media/peerasn-seeall.png)

1. Arama kutusunda *Peerasn* araması yapın ve klavyenizde *ENTER* tuşuna basın. Sonuçlardan **Peerasn** kaynağı ' na tıklayın.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn 'yi Başlat](./media/peerasn-launch.png)

1. **Peerasn** başlatıldıktan sonra **Oluştur**' a tıklayın.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn oluştur](./media/peerasn-create.png)

1. **Eş ASN 'Yi ilişkilendir** sayfasında, **temel bilgiler** sekmesinde, alanları aşağıda gösterildiği gibi doldurun.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn temelleri sekmesi](./media/peerasn-basics-tab.png)

    * **Ad** , kaynak adına karşılık gelir ve seçtiğiniz herhangi bir şey olabilir.  
    * ASN 'yi ilişkilendirmek için ihtiyacınız olan **aboneliği** seçin.
    * **Eş adı** , şirketinizin adına karşılık gelir ve PeeringDB profilinize olabildiğince yakın olması gerekir. Değerin yalnızca a-z, A-Z ve boşluk karakterlerini desteklediğini unutmayın
    * **EŞDÜZEY ASN** alanına ASN 'nizi girin.
    * **Yeni oluştur** ' a tıklayın ve ağ işlemleri merkezinize yönelik **e-posta adresı** ve **telefon numarası** girin (NOC)
1. Ardından, **gözden geçir + oluştur** ' a tıklayın ve bu portalın, girdiğiniz bilgilerin temel doğrulamasını çalıştırmasını sağlayabilirsiniz. Bu, *son doğrulama çalıştırılırken*en üstteki şeritte görüntülenir....

    > [!div class="mx-imgBorder"]
    > ![PeerAsn Inceleme sekmesi](./media/peerasn-review-tab-validation.png)

1. Şeritteki ileti, *doğrulama başarılı*olduktan sonra, **Oluştur**' a tıklayarak bilgilerinizi doğrulayın ve isteği iletin. Doğrulama geçmezse, daha sonra **geri** ' ye tıklayın ve isteğinizi değiştirmek için yukarıdaki adımları tekrarlayın ve girdiğiniz değerlerin hata olmadığından emin olun.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn Inceleme sekmesi](./media/peerasn-review-tab.png)

1. İsteği gönderdikten sonra, dağıtımın tamamlanmasını bekleyin. Dağıtım başarısız olursa, [Microsoft eşleme](mailto:peering@microsoft.com)ile iletişime geçin. Başarılı bir dağıtım aşağıda gösterildiği gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn başarısı](./media/peerasn-success.png)

### <a name="view-status-of-a-peerasn"></a>PeerAsn durumunu görüntüleme
PeerAsn kaynağı başarıyla dağıtıldıktan sonra Microsoft 'un ilişkilendirme isteğini onaylaması için beklemeniz gerekir. Onay için 12 saate kadar zaman alabilir. Onaylandığında, yukarıdaki bölümde girilen e-posta adresine bir bildirim alırsınız.

> [!IMPORTANT]
> Bir eşleme isteği göndermeden önce ValidationState durumunun "Onaylandı" olmasını bekleyin. Bu onay için 12 saate kadar zaman alabilir.

## <a name="modify-peerasn"></a>PeerAsn 'yi Değiştir
PeerAsn değiştirme Şu anda desteklenmiyor. Değiştirmeniz gerekiyorsa, [Microsoft eşleme](mailto:peering@microsoft.com)ile iletişim kurun.

## <a name="delete-peerasn"></a>PeerAsn 'yi Sil
PeerAsn silme işlemi şu anda desteklenmiyor. PeerAsn 'yi silmeniz gerekiyorsa, [Microsoft eşleme](mailto:peering@microsoft.com)ile iletişim kurun.

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Doğrudan eşleme oluşturma veya değiştirme](howto-direct-portal.md)
* [Portalı kullanarak eski bir Doğrudan eşlemeyi bir Azure kaynağına dönüştürme](howto-legacy-direct-portal.md)
* [Portalı kullanarak Exchange eşlemesi oluşturma veya değiştirme](howto-exchange-portal.md)
* [Portalı kullanarak eski bir Exchange eşlemesini bir Azure kaynağına dönüştürme](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet eşlemesi SSS](faqs.md) ' yi ziyaret edin
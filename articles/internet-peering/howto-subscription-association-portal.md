---
title: Portalı kullanarak eşdüzey ASN 'yi Azure aboneliğine ilişkilendirin
titleSuffix: Azure
description: Portalı kullanarak eşdüzey ASN 'yi Azure aboneliğine ilişkilendirin
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: d3737be5a3186774f230aef9d932464a27a764f4
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75775647"
---
# <a name="associate-peer-asn-to-azure-subscription-using-the-portal"></a>Portalı kullanarak eşdüzey ASN 'yi Azure aboneliğine ilişkilendirin

Bir eşleme isteği göndermeden önce, aşağıdaki adımları kullanarak ASN 'nizi Azure aboneliğiyle ilişkilendirmeniz gerekir.

İsterseniz, [PowerShell](howto-subscription-association-powershell.md)'i kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="create-peerasn-to-associate-your-asn-with-azure-subscription"></a>ASN 'nizi Azure aboneliğiyle ilişkilendirmek için PeerAsn oluşturun

### <a name="sign-in-to-the-portal"></a>Portalda oturum açın
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-peerasn"></a>PeerAsn oluştur
Bir otonom sistem numarası 'nı (ASN) Azure aboneliğiyle ilişkilendirmek için yeni bir PeerAsn kaynağı oluşturabilirsiniz. İlişkilendirmeniz gereken her ASN için bir **Peerasn** oluşturarak, birden fazla ASNs 'yi bir abonelikle ilişkilendirebilirsiniz.

1. **Kaynak oluştur** > **Tümünü gör**' e tıklayın.

    > [!div class="mx-imgBorder"]
    > ![arama PeerAsn](./media/peerasn-seeall.png)

1. Arama kutusunda *Peerasn* araması yapın ve klavyenizde *ENTER* tuşuna basın. Sonuçlardan **Peerasn** kaynağı ' na tıklayın.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn](./media/peerasn-launch.png) Başlat

1. **Peerasn** başlatıldıktan sonra **Oluştur**' a tıklayın.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn](./media/peerasn-create.png) oluştur

1. **Eş ASN 'Yi ilişkilendir** sayfasında, **temel bilgiler** sekmesinde, alanları aşağıda gösterildiği gibi doldurun.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn temel sekmesi](./media/peerasn-basics-tab.png)

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
    > ![PeerAsn başarılı](./media/peerasn-success.png)

### <a name="view-status-of-a-peerasn"></a>PeerAsn durumunu görüntüleme
PeerAsn kaynağı başarıyla dağıtıldıktan sonra Microsoft 'un ilişkilendirme isteğini onaylaması için beklemeniz gerekir. Onay için 12 saate kadar zaman alabilir. Onaylandığında, yukarıdaki bölümde girilen e-posta adresine bir bildirim alırsınız.

> [!IMPORTANT]
> Bir eşleme isteği göndermeden önce ValidationState durumunun "Onaylandı" olmasını bekleyin. Bu onay için 12 saate kadar zaman alabilir.

## <a name="modify-peerasn"></a>PeerAsn 'yi Değiştir
PeerAsn değiştirme Şu anda desteklenmiyor. Değiştirmeniz gerekiyorsa, [Microsoft eşleme](mailto:peering@microsoft.com)ile iletişim kurun.

## <a name="delete-peerasn"></a>PeerAsn 'yi Sil
PeerAsn silme işlemi şu anda desteklenmiyor. PeerAsn 'yi silmeniz gerekiyorsa, [Microsoft eşleme](mailto:peering@microsoft.com)ile iletişim kurun.

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak doğrudan eşleme oluşturma veya değiştirme](howto-direct-portal.md)
* [Portalı kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-portal.md)
* [Portalı kullanarak Exchange eşlemesi oluşturma veya değiştirme](howto-exchange-portal.md)
* [Portalı kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet eşlemesi SSS](faqs.md) ' yi ziyaret edin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75912216"
---
# <a name="associate-peer-asn-to-azure-subscription-using-the-portal"></a>Portalı kullanarak eşdüzey ASN’yi Azure aboneliğiyle ilişkilendirme

Bir eşleme isteği göndermeden önce, asn aboneliğinizi aşağıdaki adımları kullanarak Azure aboneliğiyle ilişkilendirmelisiniz.

İsterseniz, [PowerShell](howto-subscription-association-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="create-peerasn-to-associate-your-asn-with-azure-subscription"></a>ASN'nizi Azure Aboneliği ile ilişkilendirmek için PeerAsn'ı oluşturun

### <a name="sign-in-to-the-portal"></a>Portalda oturum açın
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="register-for-peering-resource-provider"></a>Eşleyen kaynak sağlayıcısına kaydolun
Aşağıdaki adımları izleyerek aboneliğinizde eşleyen kaynak sağlayıcısına kaydolun. Bunu yürütmezseniz, eşleme ayarlamak için gereken Azure kaynaklarına erişilemez.

1. Portalın sol üst köşesindeki **Abonelikler'e** tıklayın. Görmüyorsanız, **Daha Fazla hizmete** tıklayın ve arayın.

    > [!div class="mx-imgBorder"]
    > ![Açık abonelikler](./media/rp-subscriptions-open.png)

1. Bakmak için kullanmak istediğiniz aboneliği tıklatın.

    > [!div class="mx-imgBorder"]
    > ![Başlatma aboneliği](./media/rp-subscriptions-launch.png)

1. Abonelik açıldıktan sonra, solda Kaynak **sağlayıcıları**tıklatın. Daha sonra, sağ bölmede, arama penceresinde *bakma* yı arayın veya **Microsoft.Peering'i** bulmak ve **Durum'a**bakmak için kaydırma çubuğunu kullanın. Durum ***Kayıtlıysa,*** aşağıdaki adımları atlayın ve **PeerAsn oluştur**bölümüne gidin. Durum Kayıtlı ***değilse,*** **Microsoft.Peering'i** seçin ve **Kaydol'a**tıklayın.

    > [!div class="mx-imgBorder"]
    > ![Kayıt başlangıcı](./media/rp-register-start.png)

1. Durum'un ***Kayıt'ta***değiştiğini gözlemleyin.

    > [!div class="mx-imgBorder"]
    > ![Kayıt devam ediyor](./media/rp-register-progress.png)

1. Kayıt tamamlamak için bir dakika kadar bekleyin. Ardından, **Yenile'yi** tıklatın ve durumun ***Kayıtlı***olduğunu doğrulayın.

    > [!div class="mx-imgBorder"]
    > ![Kayıt tamamlandı](./media/rp-register-completed.png)

### <a name="create-peerasn"></a>PeerAsn oluştur
Azure aboneliğiyle Özerk Sistem Numarası (ASN) ilişkilendirme için yeni bir PeerAsn kaynağı oluşturabilirsiniz. Ilişkilendirmeniz gereken her ASN için bir **PeerAsn** oluşturarak birden çok ASN'yi bir abonelikle ilişkilendirebilirsiniz.

1. **Kaynak** > Oluştur'u tıklatın**Tümlerini Görün**.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn'ı ara](./media/peerasn-seeall.png)

1. Arama kutusunda *PeerAsn'ı* arayın ve klavyenizde *Enter* tuşuna basın. Sonuçlardan **PeerAsn** kaynağını tıklatın.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn'ı başlat](./media/peerasn-launch.png)

1. **PeerAsn** başlatıldıktan sonra **Oluştur'a**tıklayın.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn oluştur](./media/peerasn-create.png)

1. Eş **Leş atla ilişkilendir** sayfasında, **Temel Bilgiler** sekmesinin altında, aşağıda gösterildiği gibi alanları doldurun.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn Temelleri Sekmesi](./media/peerasn-basics-tab.png)

    * **Ad** kaynak adına karşılık gelir ve seçtiğiniz herhangi bir şey olabilir.  
    * ASN ile ilişkilendirmeniz gereken **Aboneliği** seçin.
    * **Eş adı** şirketinizin adına karşılık gelir ve PeeringDB profilinize mümkün olduğunca yakın olması gerekir. Değerin yalnızca a-z, A-Z ve boşluk karakterlerini desteklediğini unutmayın
    * AsN'nizi **Peer ASN** alanına girin.
    * Yeni **Oluştur'a** tıklayın ve Ağ Operasyon Merkeziniz (NOC) için **E-POSTA ADRESİ** ve **TELEFON NUMARASI** girin
1. Ardından, **Gözden Geçir +'ya** tıklayın ve portalın girdiğiniz bilgilerin temel doğrulamasını çalıştırdığını gözlemleyin. Bu, *son doğrulamayı çalıştırırken*üstteki şeritte görüntülenir... .

    > [!div class="mx-imgBorder"]
    > ![PeerAsn İnceleme Sekmesi](./media/peerasn-review-tab-validation.png)

1. Şeritteki ileti *Validation Geçti'ye*döndükten sonra, bilgilerinizi doğrulayın ve **Oluştur'u**tıklatarak isteğinizi gönderin. Doğrulama geçmezse, sonra **Önceki'ye** tıklayın ve isteğinizi değiştirmek ve girdiğiniz değerlerin hata olmadığından emin olmak için yukarıdaki adımları yineleyin.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn İnceleme Sekmesi](./media/peerasn-review-tab.png)

1. İsteği gönderdikten sonra, dağıtımı niçin tamamlamasını bekleyin. Dağıtım başarısız olursa, [Microsoft'a başvurun.](mailto:peering@microsoft.com) Başarılı bir dağıtım aşağıdaki gibi görünür.

    > [!div class="mx-imgBorder"]
    > ![PeerAsn Başarı](./media/peerasn-success.png)

### <a name="view-status-of-a-peerasn"></a>PeerAsn'ın durumunu görüntüleme
PeerAsn kaynağı başarıyla dağıtıldıktan sonra, Microsoft'un ilişkilendirme isteğini onaylamasını beklemeniz gerekir. Onay 12 saat kadar sürebilir. Onaylandıktan sonra, yukarıdaki bölümde girilen e-posta adresine bir bildirim alacaksınız.

> [!IMPORTANT]
> Bir eşleme isteği göndermeden önce Doğrulama Durumunun "Onaylandı"yı döndürmesini bekleyin. Bu onay 12 saat kadar sürebilir.

## <a name="modify-peerasn"></a>PeerAsn değiştir
PeerAsn'ı değiştirmek şu anda desteklenmiyor. Değiştirmeniz gerekiyorsa, [Microsoft'a başvurun.](mailto:peering@microsoft.com)

## <a name="delete-peerasn"></a>PeerAsn'ı sil
PeerAsn silme şu anda desteklenmez. PeerAsn'ı silmeniz gerekiyorsa, [Microsoft'a başvurun.](mailto:peering@microsoft.com)

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Doğrudan eşleme oluşturma veya değiştirme](howto-direct-portal.md)
* [Portalı kullanarak eski bir Doğrudan eşlemeyi bir Azure kaynağına dönüştürme](howto-legacy-direct-portal.md)
* [Portalı kullanarak Exchange eşlemi oluşturma veya değiştirme](howto-exchange-portal.md)
* [Portalı kullanarak eski bir Exchange eşlemesini bir Azure kaynağına dönüştürme](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet'e bakan SSS'leri](faqs.md) ziyaret edin
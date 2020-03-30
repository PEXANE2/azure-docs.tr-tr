---
title: SSL IP adres değişikliğine hazırlanın
description: SSL IP adresiniz değişecekse, uygulamanızın değişiklikten sonra çalışmaya devam etmesi için ne yapmanız gerektiğini öğrenin.
ms.topic: article
ms.date: 06/28/2018
ms.custom: seodec18
ms.openlocfilehash: 34f35eb67cada6066e35227fcd6a0eaf425ac007
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74672400"
---
# <a name="how-to-prepare-for-an-ssl-ip-address-change"></a>SSL IP adresi değişikliğine nasıl hazırlanım

Azure Uygulama Hizmeti uygulamanızın SSL IP adresinin değiştiğine dair bir bildirim aldıysanız, mevcut SSL IP adresini serbest bırakmak ve yeni bir adres atamak için bu makaledeki yönergeleri izleyin.

## <a name="release-ssl-ip-addresses-and-assign-new-ones"></a>SSL IP adreslerini serbest bırakın ve yenilerini atayın

1.  Azure [portalını](https://portal.azure.com)açın.

2.  Sol daki gezinme menüsünde **Uygulama Hizmetleri'ni**seçin.

3.  Uygulama Hizmeti uygulamanızı listeden seçin.

4.  **Ayarlar** üstbilgisinin altında, sol gezintideki **SSL ayarlarını** tıklatın.

1. SSL bağlamaları bölümünde, ana bilgisayar ad kaydını seçin. Açılan düzenleyicide, **SSL Türü** açılır menüsünde **SNI SSL'yi** seçin ve **Bağlama Ekle'yi**tıklatın. İşlem başarı iletisini gördüğünüzde, varolan IP adresi serbest bırakıldı.

6.  **SSL bağlamaları** bölümünde, yine sertifikayla aynı ana bilgisayar ad kaydını seçin. Açılan düzenleyicide, bu kez **SSL Type** açılır menüsünde **IP Tabanlı SSL'yi** seçin ve **Bağlayıcıekle'yi**tıklatın. İşlem başarı iletisini gördüğünüzde, yeni bir IP adresi edinilmiş olursunuz.

7.  Etki Alanı Kayıt Portalınızda (üçüncü taraf DNS Sağlayıcısı veya Azure DNS) bir A kaydı (doğrudan IP adresinizi gösteren DNS kaydı) yapılandırılırsa, varolan IP adresini yeni oluşturulan la değiştirin. Yeni IP adresini sonraki bölümdeki talimatları izleyerek bulabilirsiniz.

## <a name="find-the-new-ssl-ip-address-in-the-azure-portal"></a>Azure Portalı'nda yeni SSL IP adresini bulun

1.  Birkaç dakika bekleyin ve ardından [Azure portalını](https://portal.azure.com)açın.

2.  Sol daki gezinme menüsünde **Uygulama Hizmetleri'ni**seçin.

3.  Uygulama Hizmeti uygulamanızı listeden seçin.

4.  **Ayarlar** üstbilgisinin altında, sol gezintideki **Özellikler'i** tıklatın ve Sanal **IP adresi**etiketli bölümü bulun.

5. IP adresini kopyalayın ve etki alanı kaydınızı veya IP mekanizmanızı yeniden yapılandırın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure tarafından başlatılan bir IP adresi değişikliğine nasıl hazırlanacağı açıklanmıştır. Azure Uygulama Hizmeti'ndeki IP adresleri hakkında daha fazla bilgi için [Azure Uygulama Hizmeti'ndeki SSL ve SSL IP adreslerine](overview-inbound-outbound-ips.md)bakın.

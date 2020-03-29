---
title: Azure portal tercihlerinizi ayarlayın | Microsoft Dokümanlar
description: Azure portalı varsayılan ayarlarını kendi tercihlerinizi karşılamak için değiştirebilirsiniz. Ayarlar etkin olmayan oturum zaman sonu, varsayılan görünüm, menü modu, kontrast, tema, bildirimler ve dil ve bölgesel biçimleri içerir
services: azure-portal
keywords: ayarlar, zaman sonu, dil, bölgesel
author: mgblythe
ms.author: mblythe
ms.date: 12/19/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 7bcfdeec832b14eb53c0dab6cb2f53970d85c804
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76310688"
---
# <a name="set-your-azure-portal-preferences"></a>Azure portalı tercihlerinizi ayarlama

Azure portalının varsayılan ayarlarını kendi tercihlerinizi karşılamak için değiştirebilirsiniz. Aşağıda listelenen ayarların her biri değiştirilebilir:

* [Etkin olmayan oturum zaman ayarı](#change-the-idle-duration-for-inactive-sign-out)
* [Varsayılan görünüm](#choose-your-default-view)
* [Portal menü modu](#choose-a-portal-menu-mode)
* [Renk ve yüksek kontrast teması](#choose-a-theme)
* [Açılır bildirimler](#enable-or-disable-pop-up-notifications)
* [Dil ve bölgesel biçim](#change-language-and-regional-settings)

## <a name="change-general-portal-settings"></a>Genel portal ayarlarını değiştirme

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Genel sayfa üstbilgisinden **Ayarlar'ı** seçin.

    ![Ayarları vurgulanmış olan genel sayfa üstbilgi simgelerini gösteren ekran görüntüsü](./media/set-preferences/header-settings.png)

### <a name="change-the-idle-duration-for-inactive-sign-out"></a>Etkin olmayan oturum açma süresini değiştirme

İş istasyonunuzu güvenli hale almayı unutursanız, hareketsizzaman ayarı kaynakları yetkisiz erişimden korumaya yardımcı olur. Bir süre boşta kaldıktan sonra Azure portal oturumunuzotomatik olarak oturumunuza dahil oluyorsunuz.

Etkin olmadığında Beni **Kaydet'in**altındaki açılır açılır dosyayı seçin. Boştaysanız Azure portal oturumuzun oturumunun tamamlandığı süreyi seçin.

   ![Etkin olmayan zaman sonu ayarları vurgulanmış portal ayarlarını gösteren ekran görüntüsü](./media/set-preferences/inactive-signout-user.png)

Değişiklik otomatik olarak kaydedilir. Boştaysanız, Azure portal oturumunuz belirlediğiniz süre den sonra oturumu tamamlar.

Bu ayar, en fazla boşta kalma süresini zorlamak için dizin düzeyindeki bir yönetici tarafından da yapılabilir. Bir yönetici dizin düzeyinde bir zaman aşım ayarı yapmışsa, yine de kendi etkin olmayan oturum açma sürenizi ayarlayabilirsiniz. Dizin düzeyinde ayarlanandan daha az bir saat ayarı seçin.

Yöneticiniz bir etkinlik dışı zaman ödeme ilkesi **etkinleştirmişse, dizin inaktivite zaman aşmasını** denetleme yi seçin. İlke ayarından daha az bir zaman aralığı ayarlayın.

   ![Vurgulanan dizin etkinlik zaman sonu ilkesi ayarını geçersiz kılacak portal ayarlarını gösteren ekran görüntüsü](./media/set-preferences/inactive-signout-override.png)


> [!NOTE]
> Yöneticiyseniz ve Azure portalının tüm kullanıcıları için etkin olmayan bir zaman dilimi ayarını uygulamak istiyorsanız, [Azure portalı kullanıcıları için dizin düzeyinde etkinlik zaman arasını ayarlama](admin-timeout.md) bölümüne bakın
>

### <a name="choose-your-default-view"></a>Varsayılan görünümünüzü seçin 

Oturum açtığınızda Azure portalında varsayılan olarak açılan sayfayı değiştirebilirsiniz.

   ![Varsayılan görünüm vurgulanmış Azure portal ayarlarını gösteren ekran görüntüsü](./media/set-preferences/default-view.png)

Oturum açken azure portal görünümünün gösterildiği varsayılan görünüm ayarı denetler. Varsayılan olarak Azure Home'u veya Pano görünümünü açmayı seçebilirsiniz.

* **Ev** özelleştirici olamaz.  Popüler Azure hizmetlerinin kısayollarını görüntüler ve en son kullandığınız kaynakları listeler. Ayrıca, Microsoft Learn ve Azure yol haritası gibi kaynaklara yararlı bağlantılar da savuruyoruz.
* Panolar, sadece sizin için tasarlanmış bir çalışma alanı oluşturmak için özelleştirilebilir. Örneğin, proje, görev veya rol odaklı bir pano oluşturabilirsiniz. **Pano'yı**seçerseniz, varsayılan görünümünüz en son kullanılan panonuza gider.

### <a name="choose-a-portal-menu-mode"></a>Portal menü modu seçin

Portal menüsünün varsayılan modu, portal menüsünün sayfada ne kadar yer kaplar olduğunu denetler.

* Portal menüsü **uçuş** modundayken, ihtiyacınız olana kadar gizlenir. Menüyü açmak veya kapatmak için menü simgesini seçin.
* Portal menüsü için **sabitlenmiş** modu seçerseniz, her zaman görünür. Daha fazla çalışma alanı sağlamak için menüyü daraltabilirsiniz. 

### <a name="choose-a-theme"></a>Tema seç

Seçtiğiniz tema, Azure portalında görünen arka plan ve yazı tipi renklerini etkiler. Önceden ayarlanmış dört renk temalı temadan birini seçebilirsiniz. Size en uygun temayı bulmak için her küçük resmi seçin.

   ![Vurgulanan temalarla Azure portal ayarlarını gösteren ekran görüntüsü](./media/set-preferences/theme.png)

Bunun yerine yüksek kontrastlı temalardan birini seçebilirsiniz. Yüksek kontrast ayarları, Azure portalının görme engelli kullanıcılar için okunmasını kolaylaştırır ve diğer tüm tema seçimlerini geçersiz kılar. Daha fazla bilgi için bkz: [Yüksek karşıtlığı aç veya teonu değiştir.](azure-portal-change-theme-high-contrast.md)

### <a name="enable-or-disable-pop-up-notifications"></a>Açılır pencere bildirimlerini etkinleştirme veya devre dışı etme

Bildirimler, geçerli oturumunuzla ilgili sistem iletileridir. Oluşturduğunuz kaynaklar kullanılabilir olduğunda mevcut kredi bakiyeniz gibi bilgiler sağlar veya örneğin son eyleminizi onaylar. Açılır bildirimler açık olduğunda, iletiler kısa bir süre için ekranınızın üst köşesinde görüntülenir. 

Açılır pencerelerbildirimlerini etkinleştirmek veya devre dışı kalmak **için, açılan bildirimleri etkinleştir** onay kutusunu seçin veya seçin.

   ![Açılır bildirimler vurgulanmış Azure portal ayarlarını gösteren ekran görüntüsü](./media/set-preferences/popup-notifications.png)

Geçerli oturumunuz sırasında alınan tüm bildirimleri okumak için genel üstbilgiden **Bildirimler'i** seçin.

   ![Vurgulanan bildirimlerle Azure portalı genel üstbilgigösteren ekran görüntüsü](./media/set-preferences/read-notifications.png)

Önceki oturumlardan gelen bildirimleri okumak istiyorsanız, Etkinlik günlüğündeki olayları arayın. Daha fazla bilgi edinmek için [Azure Etkinliği günlük etkinliklerini görüntüle ve alın.](/azure/azure-monitor/platform/activity-log-view)

### <a name="settings-under-useful-links"></a>Yararlı bağlantılar altında ayarlar

Azure portal ayarlarında değişiklik yaptıysanız ve bunları atmak istiyorsanız, **varsayılan ayarları geri yükle'yi**seçin. Portal ayarlarında yaptığınız tüm değişiklikler kaybolur. Bu seçenek pano özelleştirmelerini etkilemez.

Tüm ayarları **Dışa Aktar** veya **Tüm ayarları ve özel panoları sil**hakkında daha fazla bilgi için kullanıcı [ayarlarını dışa](azure-portal-export-delete-settings.md)aktar veya silmek için bkz.

## <a name="change-language-and-regional-settings"></a>Dil ve bölgesel ayarları değiştirme

Azure portalındaki metnin nasıl görününden denetleyen iki ayar vardır. **Dil** ayarı, Azure portalında metin için gördüğünüz dili denetler. **Bölgesel biçim,** tarihlerin, zamanın, sayıların ve para biriminin gösterilme biçimini denetler.

Azure portalında kullanılan dili değiştirmek için, kullanılabilir diller listesinden seçim yapmak için açılır açılır bölümü kullanın.

Bölgesel biçim seçimi, yalnızca seçtiğiniz dil için bölgesel seçenekleri görüntülemek için değişir. Bu otomatik seçimi değiştirmek için, istediğiniz bölgesel biçimi seçmek için açılır açılır ı kullanın.

Örneğin, diliniz olarak İngilizce'yi seçerseniz ve ardından bölgesel biçim olarak ABD'yi seçerseniz, para birimi ABD doları cinsinden gösterilir. Dil olarak İngilizce'yi seçerseniz ve bölgesel biçim olarak Avrupa'yı seçerseniz, para birimi euro cinsinden gösterilir.

Dil ve bölgesel biçim ayarlarınızı güncellemek için **Uygula'yı** seçin.

   ![Dil ve bölgesel biçim ayarlarını gösteren ekran görüntüsü](./media/set-preferences/language.png)

>[!NOTE]
>Bu dil ve bölgesel ayarlar yalnızca Azure portalını etkiler. Yeni bir sekme veya pencerede açılan dokümantasyon bağlantıları, görüntülenecek dili belirlemek için tarayıcınızın dil ayarlarını kullanır.
>

## <a name="next-steps"></a>Sonraki adımlar

* [Özel panoları oluşturma ve paylaşma](azure-portal-dashboards.md)
* [Azure portalı nasıl yapılır video serisi](azure-portal-video-series.md)

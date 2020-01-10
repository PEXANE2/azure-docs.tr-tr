---
title: Azure portal tercihlerinizi ayarlayın | Microsoft Docs
description: Kendi tercihlerinizi karşılamak için Azure portal varsayılan ayarlarını değiştirebilirsiniz. Etkin olmayan oturum zaman aşımı, varsayılan görünüm, menü modu, karşıtlık, tema, bildirimler ve dil ve bölge biçimlerini içeren ayarlar
services: azure-portal
keywords: ayarlar, zaman aşımı, dil, bölgesel
author: mblythe
ms.author: mblythe
ms.date: 12/19/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 0c18ccbf5d9e4884af46e088f1a4ead67f50c3f5
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75641375"
---
# <a name="set-your-azure-portal-preferences"></a>Azure portalı tercihlerinizi ayarlama

Kendi tercihlerinizi karşılamak için Azure portal varsayılan ayarlarını değiştirebilirsiniz. Aşağıda listelenen ayarların her biri değiştirilebilir:

* [Etkin olmayan oturum zaman aşımı](#change-the-idle-duration-for-inactive-sign-out)
* [Varsayılan görünüm](#choose-your-default-view)
* [Portal menü modu](#choose-a-portal-menu-mode)
* [Renk ve yüksek karşıtlık teması](#choose-a-theme)
* [Açılır bildirimler](#enable-or-disable-pop-up-notifications)
* [Dil ve bölgesel biçim](#change-language-and-regional-settings)

## <a name="change-general-portal-settings"></a>Genel Portal ayarlarını değiştir

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. Genel sayfa üstbilgisindeki **ayarları** seçin.

    ![Ayarlar vurgulanmış olarak genel sayfa üst bilgi simgelerini gösteren ekran görüntüsü](./media/set-preferences/header-settings.png)

### <a name="change-the-idle-duration-for-inactive-sign-out"></a>Etkin olmayan oturum kapatma için boşta kalma süresini değiştirme

İşlem yapılmayan zaman aşımı ayarı, iş istasyonunuzun güvenliğini unutursanız kaynakları yetkisiz erişimden korumanıza yardımcı olur. Bir süredir boşta kaldıktan sonra, Azure portal oturumunuz oturumunuzu otomatik olarak açtınız.

**Devre dışı bırakıldığında Oturumumu**Kapat ' ın altındaki açılır seçimi seçin. Boşta kalırsa Azure portal oturumunuz kapatıldıktan sonraki süreyi seçin.

   ![Etkin olmayan zaman aşımı ayarları vurgulanmış şekilde Portal ayarlarını gösteren ekran görüntüsü](./media/set-preferences/inactive-signout-user.png)

Değişiklik otomatik olarak kaydedilir. Boşta değilseniz, Azure portal oturumunuz, ayarladığınız süreden sonra oturumunuzu açacaktır.

Bu ayar, dizin düzeyinde bir yönetici tarafından en fazla boş kalma süresi zorlamak için de yapılabilir. Bir yönetici dizin düzeyinde bir zaman aşımı ayarı yapmışsa, devre dışı bırakılmış oturum kapatma sürenize yine de ayarlama yapabilirsiniz. Dizin düzeyinde ayarlandıklardan daha az bir zaman ayarı seçin.

Yöneticiniz, etkin olmama zaman aşımı ilkesini etkinleştirmişse **Dizin eylemsizlik zaman aşımı ilkesi onay kutusunu geçersiz kıl** seçeneğini belirleyin. İlke ayarından daha az bir zaman aralığı ayarlayın.

   ![Portal ayarlarını geçersiz kılma ile gösteren ekran görüntüsü dizin eylemsizlik zaman aşımı ilkesi ayarını vurguladı](./media/set-preferences/inactive-signout-override.png)


> [!NOTE]
> Yönetici değilseniz ve Azure portal tüm kullanıcıları için etkin olmayan bir zaman aşımı ayarını zorlamak istiyorsanız, bkz [. Azure Portal kullanıcıları için Dizin düzeyi eylemsizlik zaman aşımını ayarlama](admin-timeout.md)
>

### <a name="choose-your-default-view"></a>Varsayılan görünümünüzü seçin 

Azure portal oturum açtığınızda varsayılan olarak açılan sayfayı değiştirebilirsiniz.

   ![Varsayılan görünüm vurgulanmış Azure portal ayarlarını gösteren ekran görüntüsü](./media/set-preferences/default-view.png)

Varsayılan Görünüm ayarı, oturum açtığınızda hangi Azure portal görünümünün gösterildiğini denetler. Azure Home 'ı varsayılan olarak veya Pano görünümünde açmayı seçebilirsiniz.

* **Ana sayfa** özelleştirilemiyor.  Popüler Azure hizmetlerinin kısayollarını görüntüler ve en son kullandığınız kaynakları listeler. Ayrıca, Microsoft Learn ve Azure yol haritası gibi kaynaklara yönelik faydalı bağlantılar sunuyoruz.
* Panolar, yalnızca sizin için tasarlanmış bir çalışma alanı oluşturmak üzere özelleştirilebilir. Örneğin, proje, görev veya rol odaklı bir pano oluşturabilirsiniz. **Pano**' yı seçerseniz, varsayılan görünümlerinizin en son kullanılan panoya gitmesi gerekir.

### <a name="choose-a-portal-menu-mode"></a>Bir portal menü modu seçin

Portal menüsünün varsayılan modu, Portal menüsünün sayfada ne kadar alan kapladiğini denetler.

* Portal menüsü **açılır** modda olduğunda, bu, ihtiyacınız olana kadar gizlenir. Menüyü açmak veya kapatmak için menü simgesini seçin.
* Portal menüsü için **yerleşik** mod ' u seçerseniz, her zaman görünür olur. Daha fazla çalışma alanı sağlamak için menüyü daraltabilirsiniz. 

### <a name="choose-a-theme"></a>Tema seç

Seçtiğiniz tema, Azure portal görüntülenen arka plan ve yazı tipi renklerini etkiler. Önceden ayarlanmış dört renk teması arasından birini seçebilirsiniz. Sizi en iyi şekilde karşılayacak temayı bulmak için her bir küçük resmi seçin.

   ![Temalar vurgulanmış Azure portal ayarlarını gösteren ekran görüntüsü](./media/set-preferences/theme.png)

Bunun yerine yüksek karşıtlıklı temalardan birini seçebilirsiniz. Yüksek karşıtlık ayarları, görme engelli kullanıcılar için Azure portal okumayı daha kolay hale getirir ve diğer tüm tema seçimlerini geçersiz kılar. Daha fazla bilgi için bkz. [yüksek karşıtlığı açma veya temayı değiştirme](azure-portal-change-theme-high-contrast.md).

### <a name="enable-or-disable-pop-up-notifications"></a>Açılır bildirimleri etkinleştirme veya devre dışı bırakma

Bildirimler, geçerli oturumunuzla ilgili sistem iletilerdir. Bunlar, yeni oluşturduğunuz kaynaklar kullanılabilir hale geldiğinde geçerli kredi bakiyeniz gibi bilgiler sağlar veya Son eyleminizi (örneğin,) onaylayın. Açılır bildirimler açıldığında, iletiler ekranın üst köşesinde kısaca görüntülenir. 

Açılır bildirimleri etkinleştirmek veya devre dışı bırakmak için **açılır bildirimleri etkinleştir** onay kutusunu seçin veya kaldırın.

   ![Açılır bildirimler vurgulanmış Azure portal ayarlarını gösteren ekran görüntüsü](./media/set-preferences/popup-notifications.png)

Geçerli oturumunuz sırasında alınan tüm bildirimleri okumak için genel üst bilgiden **Bildirimler** ' i seçin.

   ![Bildirimleri vurgulanan Azure portal genel üstbilgiyi gösteren ekran görüntüsü](./media/set-preferences/read-notifications.png)

Önceki oturumlardaki bildirimleri okumak istiyorsanız, etkinlik günlüğündeki olayları arayın. Daha fazla bilgi edinmek için bkz. [Azure etkinlik günlüğü olaylarını görüntüleme ve alma](/azure/azure-monitor/platform/activity-log-view).

### <a name="settings-under-useful-links"></a>Yararlı bağlantılar altındaki ayarlar

Azure portal ayarlarında değişiklik yaptıysanız ve bunları atmak istiyorsanız **varsayılan ayarları geri yükle**' yi seçin. Portal ayarlarında yaptığınız tüm değişiklikler kaybedilir. Bu seçenek Pano özelleştirmelerini etkilemez.

**Tüm ayarları dışarı aktarma** veya **tüm ayarları ve özel panoları silme**hakkında daha fazla bilgi için bkz. [Kullanıcı ayarlarını dışa aktarma veya silme](azure-portal-export-delete-settings.md).

## <a name="change-language-and-regional-settings"></a>Dil ve bölge ayarlarını değiştirme

Azure portal metnin nasıl göründüğünü denetleyen iki ayar vardır. **Dil** ayarı, Azure Portal metin için gördüğünüz dili denetler. **Bölgesel biçim** tarihler, saat, sayılar ve para biriminin gösterilme biçimini denetler.

Azure portal kullanılan dili değiştirmek için, açılan listeyi kullanarak kullanılabilir diller listesinden seçin.

Bölgesel biçim seçimi yalnızca seçtiğiniz dilin bölgesel seçeneklerini görüntüleyecek şekilde değişir. Bu otomatik seçimi değiştirmek için, açılan eklentiyi kullanarak istediğiniz bölgesel biçimi seçin.

Örneğin, diliniz olarak Ingilizce seçeneğini belirleyip bölgesel biçim olarak Birleşik Devletler ' yi seçerseniz para birimi ABD Doları cinsinden gösterilir. Dil olarak Ingilizce ' yi ve ardından bölgesel biçim olarak Avrupa ' yı seçerseniz para birimi Euro olarak gösterilir.

Dilinizi ve bölgesel biçim ayarlarınızı güncelleştirmek için **Uygula** ' yı seçin.

   ![Dil ve bölgesel biçim ayarlarını gösteren ekran görüntüsü](./media/set-preferences/language.png)

>[!NOTE]
>Bu dil ve bölgesel ayarlar yalnızca Azure portal etkiler. Yeni bir sekmede veya pencerede açık olan belge bağlantıları, görüntülenecek dili belirlemekte tarayıcınızın dil ayarlarını kullanacaktır.
>

## <a name="next-steps"></a>Sonraki adımlar

* [Özel panolar oluşturma ve paylaşma](azure-portal-dashboards.md)
* [Azure portal nasıl yapılır video serisi](azure-portal-video-series.md)

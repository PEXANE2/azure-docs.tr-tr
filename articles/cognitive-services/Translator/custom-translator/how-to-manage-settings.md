---
title: Ayarları nasıl yönetebilirsiniz? - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Özel Çevirmen'de ayarları yönetme, çalışma alanı oluşturma, çalışma alanını paylaşma ve abonelik anahtarını yönetme.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: f90ada6b6cd1760b3b779010625f252533fa611d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219516"
---
# <a name="how-to-manage-settings"></a>Ayarları yönetme

Özel Çevirmen ayarları sayfasında yeni bir çalışma alanı oluşturabilir, çalışma alanınızı paylaşabilir ve Microsoft Çeviri abonelik anahtarınızı ekleyebilir veya değiştirebilirsiniz.

Ayarlar sayfasına erişmek için:

1. [Özel Çevirmen](https://portal.customtranslator.azure.ai/) portalında oturum açın.
2. Özel Çevirmen portalında kenar çubuğundaki vites simgesine tıklayın.

    ![Bağlantıyı Ayarlama](media/how-to/how-to-settings.png)

## <a name="associating-microsoft-translator-subscription"></a>Microsoft Translator Aboneliğini İlişkilendirme

Modelleri eğitmek veya dağıtmak için çalışma alanınızla ilişkili bir Microsoft Translator Text API abonelik anahtarına sahip olmanız gerekir.

Aboneliğiniz yoksa aşağıdaki adımları izleyin:

1. Microsoft Translator Text API'ye abone olun. Bu makalede, Microsoft Translator Text API'ye nasıl abone olunur gösterilmektedir.
2. Çevirmen aboneliğinizin anahtarına dikkat edin. Key1 veya Key2'den biri kabul edilebilir.
3. Özel Çevirmen portalına geri dönün.

### <a name="add-existing-key"></a>Varolan anahtarı ekleme

1.  Çalışma alanınız için "Ayarlar" sayfasına gidin.
2.  Anahtar Ekle'yi tıklatın

    ![Abonelik anahtarı ekleme](media/how-to/how-to-add-subscription-key.png)

3. İletişim günlüğüne, çevirmen aboneliğiniz için anahtarı girin ve ardından "Ekle" düğmesini tıklatın.

    ![Abonelik anahtarı ekleme](media/how-to/how-to-add-subscription-key-dialog.png)
4.  Bir anahtar ekledikten sonra, istediğiniz zaman anahtarı değiştirebilir veya silebilirsiniz.

    ![Add'den sonra abonelik anahtarı](media/how-to/subscription-key-after-add.png)

## <a name="manage-your-workspace"></a>Çalışma alanınızı yönetme

Çalışma alanı, özel çeviri sisteminizi oluşturmak ve oluşturmak için bir çalışma alanıdır. Çalışma alanı birden çok proje, model ve belge içerebilir.

Çalışmanızın farklı bir bölümünün farklı kişilerle paylaşılması gerekiyorsa, birden çok çalışma alanı oluşturmak yararlı olabilir.

## <a name="create-a-new-workspace"></a>Yeni çalışma alanı oluşturma

1.  Çalışma alanı "Ayarlar" sayfasına gidin.
2.  "Yeni Çalışma Alanı Oluştur" bölümündeki "Yeni çalışma alanı" düğmesine tıklayın.

    ![Yeni çalışma alanı oluşturma](media/how-to/create-new-workspace.png)

4.  İletişim kutusunda, yeni çalışma alanının adını girin.
5.  "Oluştur"u tıklatın.

    ![Yeni çalışma alanı iletişim kutusu oluşturma](media/how-to/create-new-workspace-dialog.png)

## <a name="share-your-workspace"></a>Çalışma alanınızı paylaşın

Özel Çevirmen'de çalışma alanınızı başkalarıyla paylaşabilirsiniz, çalışmanızın farklı bir bölümünün farklı kişilerle paylaşılması gerekiyorsa.

1.  Çalışma alanı "Ayarlar" sayfasına gidin.
2.  "Paylaşım Ayarları" bölümündeki "Paylaş" düğmesine tıklayın.

    ![Çalışma alanını paylaşma](media/how-to/share-workspace.png)

3.  İletişim günlüğünde, bu çalışma alanının paylaşılbilmesini istediğiniz e-posta adreslerinin virgülle ayrılmış bir listesini girin. Kişinin Custom Translator'da oturum açmak için kullandığı e-posta adresiyle paylaştığınızdan emin olun. Ardından, uygun paylaşım izni düzeyini seçin.

4.  Çalışma alanınızın varsayılan adı hala "Çalışma alanım" ise, çalışma alanınızı paylaşmadan önce değiştirmeniz gerekir.
5.  "Kaydet"i tıklatın.

## <a name="sharing-permissions"></a>İzinleri paylaşma

1.  **Okuyucu:** Çalışma alanındaki bir okuyucu çalışma alanındaki tüm bilgileri görüntüleyebilir.

2.  **Editör:** Çalışma alanındaki bir düzenleyici belge ekleyebilir, modelleri eğitebilir ve belgeleri ve projeleri silebilir. Bir abonelik anahtarı ekleyebilirler, ancak çalışma alanının kiminle paylaşıldığını değiştiremezler, çalışma alanını silemez veya çalışma alanı adını değiştiremezler.

3.  **Sahibi:** Bir sahibiçalışma alanı için tam izinleri vardır.

## <a name="change-sharing-permission"></a>Paylaşım iznini değiştirme

Bir çalışma alanı paylaşıldığında, "Paylaşım Ayarları" bölümü bu çalışma alanının paylaşılabilen tüm e-posta adreslerini gösterir. Çalışma alanına sahip erişiminiz varsa, her e-posta adresi için varolan paylaşım iznini değiştirebilirsiniz.

1.  Her e-posta için "Paylaşım Ayarları" bölümünde açılır menü geçerli izin düzeyini gösterir.

2.  Açılan menüyü tıklatın ve bu e-posta adresine atamak istediğiniz yeni izin düzeyini seçin.

    ![İzin ayarlarını paylaşma](media/how-to/sharing-permission-settings.png)

## <a name="next-steps"></a>Sonraki adımlar

- Çalışma alanınızı ve projenizi [Microsoft Translator Hub'dan](https://hub.microsofttranslator.com) [nasıl geçirteceklerinizi](how-to-migrate.md) öğrenin

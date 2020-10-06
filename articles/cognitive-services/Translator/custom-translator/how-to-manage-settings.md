---
title: Ayarları yönetme -Özel çevirici
titleSuffix: Azure Cognitive Services
description: Özel çevirmende ayarları yönetme, çalışma alanı oluşturma, çalışma alanı paylaşma ve abonelik anahtarını yönetme.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 08/17/2020
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 184073ef3ee1b6a67c844754f7da81f88bbbdf62
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91757036"
---
# <a name="how-to-manage-settings"></a>Ayarları yönetme

Özel çevirici ayarları sayfasında, çalışma alanınızı paylaşabilir, Translator abonelik anahtarınızı değiştirebilir ve çalışma alanını silebilirsiniz.

Ayarlar sayfasına erişmek için:

1. [Özel çevirmen](https://portal.customtranslator.azure.ai/) portalında oturum açın.
2. Özel çevirmen portalında kenar çubuğundaki dişli simgesine tıklayın.

    ![Ayar bağlantısı](media/how-to/how-to-settings.png)

## <a name="associating-translator-subscription"></a>Translator aboneliğini ilişkilendirme

Modelleri eğitmek veya dağıtmak için çalışma alanınız ile ilişkili bir Translator abonelik anahtarına sahip olmanız gerekir.

Aboneliğiniz yoksa, aşağıdaki adımları izleyin:

1. Bir çevirmen kaynağı oluşturmak için abone olun. Nasıl abone olunacağı ve bir çevirmen anahtarı elde edebilmek için [çevirmene kaydolma](https://docs.microsoft.com/azure/cognitive-services/translator/translator-how-to-signup) konusuna uyun.
2. Çevirmen aboneliğinizin anahtarını aklınızda edin. KEY1 veya key2 seçeneklerinden biri kabul edilebilir.
3. Özel çevirmen portalına geri gidin.

## <a name="create-a-new-workspace"></a>Yeni çalışma alanı oluşturma

1. "Özel çevirici" kenar çubuğunda "+ çalışma alanı oluştur" düğmesine tıklayın.

    ![Yeni çalışma alanı oluştur](media/how-to/create-new-workspace.png)

2. İletişim kutusunda yeni çalışma alanının adını girin.
3. "Ileri" ye tıklayın.
4. Abonelik türünü seçin.
5. Abonelik Bölgesi ' ni seçin. Çevirmen kaynak anahtarı oluşturulurken bölgenin Seçili bölge ile eşleşmesi gerekir.
6. Translator aboneliğiniz için anahtarı girin ve ardından "Kaydet" düğmesine tıklayın.

    ![Yeni çalışma alanı oluştur iletişim kutusu](media/how-to/create-new-workspace-dialog.png)

>[!Note]
>Özel çevirmen Translator Metin Çevirisi API'si kaynak için çalışma alanı oluşturulmasını desteklemiyor (deyişle Azure abonelik anahtarı) [ETKIN VNET](https://docs.microsoft.com/azure/api-management/api-management-using-with-vnet)içinde oluşturulmuş.

### <a name="modify-existing-key"></a>Mevcut anahtarı Değiştir

1. Çalışma alanınızın "Ayarlar" sayfasına gidin.
2. Anahtarı Değiştir 'e tıklayın

    ![Abonelik anahtarı ekleme](media/how-to/how-to-add-subscription-key.png)

3. İletişim kutusunda, Translator aboneliğiniz için anahtarı girin ve ardından "Kaydet" düğmesine tıklayın.

    ![Abonelik anahtarı ekleme iletişim kutusu](media/how-to/how-to-add-subscription-key-dialog.png)

## <a name="manage-your-workspace"></a>Çalışma alanınızı yönetme

Çalışma alanı, özel çeviri sisteminizi oluşturmaya ve oluşturmaya yönelik bir çalışma alanıdır. Bir çalışma alanı birden fazla proje, model ve belge içerebilir.

Çalışmanızın farklı bir kısmının farklı kişilerle paylaşılması gerekiyorsa, birden çok çalışma alanı oluşturmak yararlı olabilir.

## <a name="share-your-workspace"></a>Çalışma alanınızı paylaşma

Özel çevirmende, çalışmanızın farklı bir kısmının farklı kişilerle paylaşılması gerekiyorsa, çalışma alanınızı başkalarıyla paylaşabilirsiniz.

1. Çalışma alanı "ayarları" sayfasına gidin.
2. "Paylaşım ayarları" bölümünde "kişi ekle" düğmesine tıklayın.

    ![Çalışma alanını paylaşma](media/how-to/share-workspace.png)

3. İletişim kutusunda, bu çalışma alanının paylaşılacağını istediğiniz, virgülle ayrılmış bir e-posta adresi listesi girin. İle özel çevirmende oturum açmak için kişinin kullandığı e-posta adresiyle paylaşımda olduğunuzdan emin olun. Ardından uygun paylaşım düzeyini seçin ve "Kaydet" düğmesine tıklayın.

    ![Çalışma alanını paylaşma iletişim kutusu](media/how-to/share-workspace-dialog.png)

4. Çalışma alanınız "My Workspace" varsayılan adına sahipse, çalışma alanınızı paylaşmadan önce değiştirmeniz gerekecektir.
5. "Kaydet" e tıklayın.

## <a name="sharing-permissions"></a>Paylaşım izinleri

1. **Okuyucu:** Çalışma alanındaki bir okuyucu, çalışma alanındaki tüm bilgileri görüntüleyebilecektir.

2. **Düzenleyici:** Çalışma alanındaki bir düzenleyici belge ekleyebilir, modelleri eğitebilir ve belge ve projeleri silebilir. Abonelik anahtarı ekleyebilirler, ancak çalışma alanının kimlerle paylaşıldığını değiştiremezler, çalışma alanını silebilir veya çalışma alanı adını değiştirebilirsiniz.

3. **Sahip:** Bir sahibin çalışma alanı üzerinde tam izinleri vardır.

## <a name="change-sharing-permission"></a>Paylaşım iznini Değiştir

Bir çalışma alanı paylaşıldığında, "paylaşım ayarları" bölümü, bu çalışma alanının paylaşıldığı tüm e-posta adreslerini gösterir. Çalışma alanına sahip erişiminiz varsa, her bir e-posta adresi için varolan paylaşım iznini değiştirebilirsiniz.

1. Her e-posta için "paylaşım ayarları" bölümünde, bir açılan menü, geçerli izin düzeyini gösterir.

2. Açılır menüye tıklayın ve bu e-posta adresine atamak istediğiniz yeni izin düzeyini seçin.

    ![İzin ayarlarını paylaşma](media/how-to/sharing-permission-settings.png)

## <a name="pin-your-workspace"></a>Çalışma alanınızı sabitleme

Oluşturulan ilk çalışma alanınız varsayılan olarak sabitlenmiştir. Her oturum açışınızda, sabitlenmiş çalışma alanınız site yüküne göre görüntülenir. Birden çok çalışma alanı oluşturduysanız ve oturum açtığınızda bunlardan birini varsayılan yapmanız istenirse, onu sabitetmeniz gerekir.

1. Kenar çubuğunda, sabitlemek istediğiniz çalışma alanının adına tıklayın.
2. Çalışma alanınızın "Ayarlar" sayfasına gidin.
3. Sabitleme simgesine tıklayın.

    ![Çalışma alanını sabitle](media/how-to/how-to-pin-workspace.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Çalışma alanınızı ve projelerinizi oluşturmayı](workspace-and-project.md) öğrenin

---
title: Azure portal ayarlarını dışarı veya silme | Microsoft Docs
description: Azure portal Kullanıcı ayarlarınızı, Özel panoları ve özel ayarları nasıl dışarı aktarıp silebileceğinizi öğrenin.
services: azure-portal
keywords: ''
author: santhoshsomayajula
ms.date: 04/08/2019
ms.topic: conceptual
ms.service: azure-portal
ms.custom: ''
manager: mtillman
ms.author: mblythe
ms.openlocfilehash: f033af37985077f4d8df9d541b55764df0c75eda
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75640185"
---
# <a name="export-or-delete-user-settings"></a>Kullanıcı ayarlarını dışarı aktarma veya silme

Özel bir deneyim oluşturmak için Azure portal ayarları ve özellikleri kullanabilirsiniz. Özel ayarlarınızla ilgili bilgiler Azure 'da depolanır. Aşağıdaki kullanıcı verilerini dışa aktarabilir veya silebilirsiniz:

* Azure portal özel panolar
* Sık kullanılan abonelikler veya dizinler gibi Kullanıcı ayarları ve oturum açan son dizin
* Temalar ve diğer özel portal ayarları

Ayarlarınızı silmeden önce dışa ve gözden geçirmek iyi bir fikirdir. Panoları yeniden derleme veya özel ayarları yeniden oluşturma zaman alabilir.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="export-or-delete-your-portal-settings"></a>Portal ayarlarınızı dışarı veya silme

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
2. Portalın üst bilgisinde **Ayarlar**' ı seçin.

    ![Portal ayarlarını dişli olarak gösteren ekran görüntüsü](media/azure-portal-export-delete-settings/azure-portal-settings-icon.png)

3. **Tüm ayarları dışarı aktar** veya **tüm ayarları ve özel panoları Sil '** i seçin.

    ![Portalın dışarı ve silme ayarlarını gösteren ekran görüntüsü](media/azure-portal-export-delete-settings/azure-portal-export-delete-settings.png)

      Aşağıdaki tabloda bu eylemler açıklanmaktadır.

      | Eylem | Açıklama |
      | --- | --- |
      | **Tüm ayarları dışarı aktar** | Renk teması, Sık Kullanılanlar ve özel panolarınız gibi kullanıcı ayarlarınızı içeren bir. JSON dosyası oluşturur.|
      | **Tüm ayarları ve özel panoları silme** | Özel panolar ve portalda yaptığınız diğer özel ayarlar için tüm bağlantıları siler. |

> [!NOTE]
> Kullanıcı ayarlarının dinamik doğası ve veri bozulması riski nedeniyle,. json dosyasından ayarları içeri aktaramazsınız.
>
>


## <a name="next-steps"></a>Sonraki adımlar

* [Azure panoları oluşturma ve paylaşma](azure-portal-dashboard-share-access.md)
* [Sık kullanılanları ekleme, kaldırma ve sıralama](azure-portal-add-remove-sort-favorites.md)

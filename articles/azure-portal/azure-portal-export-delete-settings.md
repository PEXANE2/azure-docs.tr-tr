---
title: Azure portal ayarlarını dışarı veya silme
description: Azure portal Kullanıcı ayarlarınızı, Özel panoları ve özel ayarları nasıl dışarı aktarıp silebileceğinizi öğrenin.
services: azure-portal
keywords: ''
author: santhoshsomayajula
ms.date: 01/29/2020
ms.topic: conceptual
ms.service: azure-portal
ms.custom: ''
manager: mtillman
ms.author: mblythe
ms.openlocfilehash: accdfbd939fad73ca7d008450c358d366e7f8b70
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76900772"
---
# <a name="export-or-delete-user-settings"></a>Kullanıcı ayarlarını dışarı aktarma veya silme

Özel bir deneyim oluşturmak için Azure portal ayarları ve özellikleri kullanabilirsiniz. Özel ayarlarınızla ilgili bilgiler Azure 'da depolanır. Aşağıdaki kullanıcı verilerini dışa aktarabilir veya silebilirsiniz:

* Azure portal özel panolar
* Sık kullanılan abonelikler veya dizinler gibi Kullanıcı ayarları ve oturum açan son dizin
* Temalar ve diğer özel portal ayarları

Ayarlarınızı silmeden önce dışa ve gözden geçirmek iyi bir fikirdir. Panoları yeniden derleme veya özel ayarları yeniden oluşturma zaman alabilir.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="export-or-delete-your-portal-settings"></a>Portal ayarlarınızı dışarı veya silme

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Portalın üst bilgisinde ayarlar simgesi ![](media/azure-portal-export-delete-settings/settings-icon.png) **Ayarlar**' ı seçin.

1. **Tüm ayarları dışarı aktar** veya **tüm ayarları ve özel panoları Sil '** i seçin.

    ![Azure portal ayarları ve ayarları seçenekleri](media/azure-portal-export-delete-settings/azure-portal-settings-with-export-delete.png)

      Aşağıdaki tabloda bu eylemler açıklanmaktadır.

      | Eylem | Açıklama |
      | --- | --- |
      | **Tüm ayarları dışarı aktar** | Renk teması, Sık Kullanılanlar ve özel panolarınız gibi kullanıcı ayarlarınızı içeren bir *. JSON* dosyası oluşturur.|
      | **Tüm ayarları ve özel panoları silme** | Özel panolar ve portalda yaptığınız diğer özel ayarlar için tüm bağlantıları siler. |

> [!NOTE]
> Kullanıcı ayarlarının dinamik doğası ve veri bozulması riski nedeniyle, *. JSON* dosyasından ayarları içeri aktaramazsınız.
>
>

## <a name="next-steps"></a>Sonraki adımlar

* [Rol Tabanlı Erişim Denetimi kullanarak Azure panolarını paylaşma](azure-portal-dashboard-share-access.md)
* [Sık kullanılanları ekleme, kaldırma ve yeniden düzenleme](azure-portal-add-remove-sort-favorites.md)

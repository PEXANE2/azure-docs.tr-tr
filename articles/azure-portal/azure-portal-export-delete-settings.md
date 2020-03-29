---
title: Azure portal ayarlarını dışa aktarma veya silme
description: Azure portalında kullanıcı ayarlarınızı, özel panolarınızı ve özel ayarlarınızı nasıl dışa aktarabileceğinizi veya silebileceğinizi öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76900772"
---
# <a name="export-or-delete-user-settings"></a>Kullanıcı ayarlarını dışarı aktarma veya silme

Azure portalındaki ayarları ve özellikleri özel bir deneyim oluşturmak için kullanabilirsiniz. Özel ayarlarınızla ilgili bilgiler Azure'da depolanır. Aşağıdaki kullanıcı verilerini dışa aktarabilir veya silebilirsiniz:

* Azure portalındaki özel panolar
* Sık kullanılan abonelikler veya dizinler gibi kullanıcı ayarları ve son oturum açmış dizini
* Temalar ve diğer özel portal ayarları

Ayarlarınızı silmeden önce dışa aktarmave gözden geçirmeniz iyi bir fikirdir. Panoları yeniden oluşturmak veya özel ayarları yeniden yapmak zaman alabilir.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="export-or-delete-your-portal-settings"></a>Portal ayarlarınızı dışa aktarma veya silme

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Portalın üstbilgisinde ayarlar ![simgesi](media/azure-portal-export-delete-settings/settings-icon.png) **Ayarları'nı**seçin.

1. **Tüm ayarları Dışa Aktar'ı** seçin veya tüm ayarları ve özel panoları **silin.**

    ![Azure portal ayarları ve ayarları seçenekleri](media/azure-portal-export-delete-settings/azure-portal-settings-with-export-delete.png)

      Aşağıdaki tabloda bu eylemleri açıklanır.

      | Eylem | Açıklama |
      | --- | --- |
      | **Tüm ayarları dışa aktarma** | Renk temanız, sık kullanılanlarınız ve özel panolarınız gibi kullanıcı ayarlarınızı içeren bir *.json* dosyası oluşturur.|
      | **Tüm ayarları ve özel panoları silme** | Portala yaptığınız özel panolara ve diğer özel ayarlara yapılan tüm bağlantıları siler. |

> [!NOTE]
> Kullanıcı ayarlarının dinamik yapısı ve veri bozulması riski nedeniyle *,.json* dosyasından ayarları içe aktaramazsınız.
>
>

## <a name="next-steps"></a>Sonraki adımlar

* [Rol Tabanlı Erişim Denetimi kullanarak Azure panolarını paylaşma](azure-portal-dashboard-share-access.md)
* [Sık kullanılanları ekleme, kaldırma ve yeniden düzenleme](azure-portal-add-remove-sort-favorites.md)

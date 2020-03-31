---
title: Dropbox'a bağlan
description: Azure Mantık Uygulamaları'nı kullanarak Dropbox'ta dosyaları yükleyen ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/01/2019
tags: connectors
ms.openlocfilehash: 8f54f832884b172761f62b16db29d2f0abd0dd46
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75665760"
---
# <a name="upload-and-manage-files-in-dropbox-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak Dropbox'a dosya yükleme ve yönetme

Dropbox bağlayıcısı ve Azure Mantık Uygulamaları ile Dropbox hesabınıza dosya yükleyen ve yöneten otomatik iş akışları oluşturabilirsiniz. 

Bu makalede, mantık uygulamanızdan Dropbox'a nasıl bağlanışveriz ve **bir dosya tetiklendiğinde** Dropbox'ı ve yol eylemini kullanarak Dropbox **Get dosya içeriğini** ekleyin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Ücretsiz olarak kaydolabileceğiniz [dropbox hesabı.](https://www.dropbox.com/) Mantık uygulamanız ile Dropbox hesabınız arasında bağlantı oluşturmak için hesap kimlik bilgileriniz gereklidir.

* [Mantık uygulamaları oluşturmak için nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md)temel bilgi . Bu örnekte, boş bir mantık uygulamasına ihtiyacınız var.

## <a name="add-trigger"></a>Tetikleyici ekleme

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Arama kutusunun **altında, Tümü'ni**seçin. Arama kutusuna filtreniz olarak "dropbox" girin.
Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Dosya oluşturulduğunda**

   ![Dropbox tetikleyicisi seçin](media/connectors-create-api-dropbox/select-dropbox-trigger.png)

1. Dropbox hesap kimlik bilgilerinizle oturum açın ve Azure Logic Apps için Dropbox verilerinize erişim yetkisi verin.

1. Tetikleyiciniz için gerekli bilgileri sağlayın. 

   Bu örnekte, dosya oluşturmayı izlemek istediğiniz klasörü seçin. Klasörlerinize göz atmak için **Klasör** kutusunun yanındaki klasör simgesini seçin.

## <a name="add-action"></a>Eylem ekleme

Şimdi herhangi bir yeni dosyadan içerik alır bir eylem ekleyin.

1. Tetikleyicinin altında Sonraki **adımı**seçin. 

1. Arama kutusunun **altında, Tümü'ni**seçin. Arama kutusuna filtreniz olarak "dropbox" girin.
Eylemler listesinden şu eylemi seçin: **Yol kullanarak dosya içeriğini alın**

1. Azure Logic Apps'a Dropbox'a erişmek için yetki vermediyseniz, şimdi erişim yetkisi verin.

1. Kullanmak istediğiniz dosya yoluna göz atmak için **Dosya Yolu** kutusunun yanındaki elips (**...**) düğmesini seçin. 

   **Dosya Yolu** kutusunun içine de tıklayabilirsiniz ve dinamik içerik listesinden, değeri önceki bölümde eklediğiniz tetikleyiciden çıktı olarak kullanılabilen **Dosya yolunu**seçin.

1. Işiniz bittiğinde, mantık uygulamanızı kaydedin.

1. Mantık uygulamanızı tetiklemek için Dropbox'ta yeni bir dosya oluşturun.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler, eylemler ve sınırlar gibi teknik ayrıntılar için, bağlayıcının Swagger dosyasında açıklandığı gibi, [bağlayıcının başvuru sayfasına](/connectors/dropbox/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin

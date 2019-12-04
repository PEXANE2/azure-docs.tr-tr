---
title: Dropbox 'a Bağlan
description: Dropbox REST API 'Leri ve Azure Logic Apps dosya yükleme ve yönetme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/01/2019
tags: connectors
ms.openlocfilehash: b38f9b9626e5eec0272256aacc71f7503a006dd6
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789823"
---
# <a name="upload-and-manage-files-in-dropbox-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak Dropbox 'ta dosya yükleme ve yönetme

Dropbox Bağlayıcısı ve Azure Logic Apps ile Dropbox hesabınızdaki dosyaları karşıya yükleyen ve yöneten otomatik iş akışları oluşturabilirsiniz. 

Bu makalede, mantıksal uygulamanızdan Dropbox 'a nasıl bağlanacağınız ve sonra **bir dosya oluşturulduğunda** ve açılan kutu **yolu kullanarak dosya Içeriğini alma** işlemi olduğunda Dropbox nasıl ekleneceği gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Ücretsiz olarak kaydolabilir bir [Dropbox hesabı](https://www.dropbox.com/). Hesap kimlik bilgileriniz, mantıksal uygulamanız ve Dropbox hesabınız arasında bağlantı oluşturmak için gereklidir.

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)hakkında temel bilgi. Bu örnekte, boş bir mantıksal uygulama gerekir.

## <a name="add-trigger"></a>Tetikleyici Ekle

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Arama kutusunda **Tümü**' ni seçin. Arama kutusuna filtreniz olarak "Dropbox" yazın.
Tetikleyiciler listesinde, bu tetikleyiciyi seçin: **bir dosya oluşturulduğunda**

   ![Dropbox tetikleyicisi Seç](media/connectors-create-api-dropbox/select-dropbox-trigger.png)

1. Dropbox hesabınızın kimlik bilgilerinizle oturum açın ve Azure Logic Apps için Dropbox verilerinize erişim yetkisi verin.

1. Tetikleyiciniz için gerekli bilgileri sağlayın. 

   Bu örnekte, dosya oluşturmayı izlemek istediğiniz klasörü seçin. Klasörlerinize gözatıp **klasör** kutusunun yanındaki klasör simgesini seçin.

## <a name="add-action"></a>Eylem Ekle

Şimdi herhangi bir yeni dosyadan içerik alan bir eylem ekleyin.

1. Tetikleyici altında, **İleri adım**' ı seçin. 

1. Arama kutusunda **Tümü**' ni seçin. Arama kutusuna filtreniz olarak "Dropbox" yazın.
Eylemler listesinden şu eylemi seçin: **yolu kullanarak dosya Içeriğini al**

1. Dropbox 'a erişme Azure Logic Apps henüz yetkiniz yoksa, erişimi Şimdi Yetkilendir.

1. Kullanmak istediğiniz dosya yoluna gitmek için **dosya yolu** kutusunun yanındaki üç nokta ( **...** ) düğmesini seçin. 

   Ayrıca, **dosya yolu** kutusunun içine tıklayabilir ve dinamik içerik listesinden, değeri önceki bölümde eklediğiniz tetikleyiciden çıktı olarak kullanılabilir olan **dosya yolu**' nu seçin.

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin.

1. Mantıksal uygulamanızı tetiklemek için Dropbox 'ta yeni bir dosya oluşturun.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Openapı (eski adıyla Swagger) dosyasında açıklandığı gibi Tetikleyiciler, Eylemler ve sınırlar gibi teknik ayrıntılar için [bağlayıcının başvuru sayfasına](/connectors/dropbox/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin

---
title: Excel Online'da verileri, çalışma sayfalarını ve tabloları yönetme
description: Azure Mantık Uygulamalarını kullanarak Excel Online for Business veya Excel Online for OneDrive'daki çalışma sayfalarındaki ve tablolardaki verileri yönetme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 08/23/2018
tags: connectors
ms.openlocfilehash: b1df1219590aa94331a4404b1315eb7231c59699
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75445876"
---
# <a name="manage-excel-online-data-with-azure-logic-apps"></a>Azure Logic Apps ile Excel Online verilerini yönetme

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve İş için Excel [Çevrimiçi](/connectors/excelonlinebusiness/) bağlayıcısı veya OneDrive bağlayıcısı için Excel Online ile, Excel Online for Business veya [OneDrive'da](/connectors/excelonline/) verilerinize dayalı olarak otomatik görevler ve iş akışları oluşturabilirsiniz. Bu bağlayıcı, verilerinizle çalışmanıza ve elektronik tabloları yönetmenize yardımcı olan eylemler sağlar( örneğin:

* Yeni çalışma sayfaları ve tablolar oluşturun.
* Çalışma sayfalarını, tabloları ve satırları alın ve yönetin.
* Tek satırlar ve anahtar sütunları ekleyin.

Daha sonra, bu eylemlerden elde edilen çıktıları diğer hizmetler için eylemlerle kullanabilirsiniz. Örneğin, her hafta çalışma sayfaları oluşturan bir eylem kullanırsanız, Office 365 Outlook bağlayıcısını kullanarak onay e-postası gönderen başka bir eylem kullanabilirsiniz.

Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

> [!NOTE]
> [OneDrive](/connectors/excelonlinebusiness/) bağlayıcıları için Excel Online ve OneDrive bağlayıcıları [için Excel Çevrimiçi,](/connectors/excelonline/) Azure Mantık Uygulamaları ile çalışır ve [PowerApps için Excel bağlayıcısından](/connectors/excel/)farklıdır.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* İş hesabınız veya kişisel Microsoft hesabınız için bir [Office 365](https://www.office.com/) hesabı

  Excel verileriniz, örneğin OneDrive'daki bir depolama klasöründe virgülle ayrılmış değer (CSV) dosyası olarak bulunabilir. 
  Ayrıca [düz dosya bağlayıcısı](../logic-apps/logic-apps-enterprise-integration-flatfile.md)ile aynı CSV dosyasını kullanabilirsiniz.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Excel Online verilerinize erişmek istediğiniz mantık uygulaması. Bu bağlayıcı yalnızca eylemler sağlar, bu nedenle mantık uygulamanızı başlatmak için, örneğin **Yinelenen** Tetikleyici gibi ayrı bir tetikleyici seçin.

## <a name="add-excel-action"></a>Excel eylemi ekleme

1. Azure [portalında,](https://portal.azure.com)mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Tetikleyicinin altında **Yeni adımı**seçin.

1. Arama kutusuna filtreniz olarak "excel" girin. Eylemler listesinin altında, istediğiniz eylemi seçin.

   > [!NOTE]
   > Mantık Uygulama Tasarımcısı, 100 veya daha fazla sütunu olan tabloları yükleyebilir. Mümkünse, tasarımcının tabloyu yükleyebilmeleri için seçili tablodaki sütun sayısını azaltın.

1. İstenirse, Office 365 hesabınızda oturum açın.

   Kimlik bilgileriniz, Excel Online'a bağlantı oluşturmak ve verilerinize erişmek için mantık uygulamanıza yetki verebolur.

1. Seçili eylem için gerekli ayrıntıları sağlamaya ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) dosyaları tarafından açıklandığı gibi tetikleyiciler, eylemler ve sınırlar gibi teknik ayrıntılar için aşağıdaki bağlayıcı başvuru sayfalarına bakın:

* [İş için Excel Online](/connectors/excelonlinebusiness/)
* [OneDrive için Excel Online](/connectors/excelonline/)

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin

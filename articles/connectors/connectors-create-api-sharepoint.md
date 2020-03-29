---
title: Azure Logic Apps'tan SharePoint'e bağlanma
description: Azure Logic Apps'ı kullanarak SharePoint Online veya SharePoint Server'daki kaynakları şirket içinde izleyen ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
tags: connectors
ms.openlocfilehash: bb82ef2d6fb83c2e1b0fa81aa9504c9bb7d8234b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789265"
---
# <a name="monitor-and-manage-sharepoint-resources-with-azure-logic-apps"></a>Azure Logic Apps ile SharePoint kaynaklarını izleme ve yönetme

Azure Logic Apps ve SharePoint bağlayıcısı ile, örneğin SharePoint Online'da veya şirket içinde SharePoint Server'da dosyalar, klasörler, listeler, öğeler, kişiler gibi kaynakları izleyen ve yöneten otomatik görevler ve iş akışları oluşturabilirsiniz:

* Dosyalar veya öğeler oluşturulduğunda, değiştirildiğinde veya silindiğinde izleyin.
* Öğeleri oluşturun, alın, güncelleyin veya silin.
* Ekler ekleme, alma veya silme. İçeriği ekleri alın.
* Dosyaları oluşturun, kopyalayın, güncelleyin veya silin. 
* Dosya özelliklerini güncelleştirin. Bir dosyanın içeriğini, meta verilerini veya özelliklerini alın.
* Klasörleri listeleyin veya ayıklayın.
* Liste veya liste görünümleri alın.
* İçerik onay durumunu ayarlayın.
* Kişileri çözün.
* HTTP isteklerini SharePoint'e gönderin.
* Varlık değerlerini alın.

SharePoint'ten yanıt alan ve çıktıyı diğer eylemler için kullanılabilir hale getiren tetikleyiciler kullanabilirsiniz. SharePoint'te görevleri gerçekleştirmek için mantık uygulamalarınızdaki eylemleri kullanabilirsiniz. SharePoint eylemlerinden çıktıyı başka eylemlerin de kullanmasını sağlayabilirsiniz. Örneğin, SharePoint'ten düzenli olarak dosya getiriyorsanız, Slack bağlayıcısını kullanarak ekibinize ileti gönderebilirsiniz.
Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* SharePoint site adresiniz ve kullanıcı kimlik bilgileriniz

  Kimlik bilgileriniz, bir bağlantı oluşturmak ve SharePoint hesabınıza erişmek için mantık uygulamanıza yetki vetir. 

* Mantık uygulamalarını SharePoint Server gibi şirket içi sistemlere bağlamadan önce [şirket içi bir veri ağ geçidi yüklemeniz ve kurmanız](../logic-apps/logic-apps-gateway-install.md)gerekir. Bu şekilde, mantık uygulamanız için SharePoint Server bağlantısını oluştururken ağ geçidi yüklemenizi kullanacağınızı belirtebilirsiniz.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* SharePoint hesabınıza erişmek istediğiniz mantık uygulaması. SharePoint tetikleyicisiyle başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) SharePoint eylemini kullanmak için, bir Salesforce hesabınız varsa, mantık uygulamanızı Salesforce tetikleyicisi gibi bir tetikleyiciyle başlatın.

  Örneğin, mantık uygulamanızı **Salesforce** tetikleyicisi ne zaman bir kayıt oluşturulursa ile başlatabilirsiniz. 
  Bu tetikleyici, Salesforce'ta müşteri adayı gibi yeni bir kaydın oluşturulduğu her seferde devreye sayılsa. 
  Daha sonra SharePoint **Oluştur dosya** eylemi ile bu tetikleyiciyi izleyebilirsiniz. Bu şekilde, yeni kayıt oluşturulduğunda, mantık uygulamanız SharePoint'te yeni kayıt la ilgili bilgileri içeren bir dosya oluşturur.

## <a name="connect-to-sharepoint"></a>SharePoint’e bağlanma

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Boş mantık uygulamaları için, arama kutusuna filtreniz olarak "sharepoint"i girin. Tetikleyiciler listesinin altında, istediğiniz tetikleyiciyi seçin. 

   -veya-

   Varolan mantık uygulamaları için, SharePoint eylemi eklemek istediğiniz son adımaltında **Yeni adım'ı**seçin. 
   Arama kutusuna filtreniz olarak "sharepoint" girin. 
   Eylemler listesinin altında, istediğiniz eylemi seçin.

   Adımlar arasında eylem eklemek için işaretçinizin üzerine adımların arasında ilerleyin. 
   Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Oturum açmanız istendiğinde, gerekli bağlantı bilgilerini sağlayın. SharePoint Server kullanıyorsanız, şirket içi **veri ağ geçidi üzerinden Bağlan'ı**seçtiğinizden emin olun. İşiniz bittiğinde **Oluştur**’u seçin.

1. Seçtiğiniz tetikleyici veya eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/sharepoint/)inceleyin.

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin

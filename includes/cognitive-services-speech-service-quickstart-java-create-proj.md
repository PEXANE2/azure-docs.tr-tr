---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 10/15/2020
ms.author: erhopf
ms.openlocfilehash: 68e83ca0fc92247a31e840e76fc8019736b71dd9
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99214511"
---
1. Eclipse’i başlatın.

1. Eclipse Başlatıcısı’nda **Çalışma Alanı** alanına yeni bir çalışma alanı dizininin adını girin. Ardından **Başlat**’ı seçin.

   ![Eclipse Başlatıcısı ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-01-create-new-eclipse-workspace.png)

1. Çok geçmeden Eclipse IDE ana penceresi görüntülenir. Varsa, **hoş geldiniz** ekranını kapatın.

1. Çakışan Küreler menü çubuğunda **Dosya**  >  **Yeni**  >  **Proje**' yi seçerek yeni bir proje oluşturun.

1. **Yeni Proje** iletişim kutusu görünür. **Java Projesi**’ni ve sonra **İleri**’yi seçin.

   ![Java Projesi vurgulanmış şekilde, Yeni Proje iletişim kutusunun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-02-select-wizard.png)

1. **Yeni Java proje** Sihirbazı başlatılır. **Proje adı** alanına **hızlı başlangıç** yazın ve yürütme ortamı olarak **JavaSE-1.8** seçeneğini belirleyin. **Son**'u seçin.

   ![Yeni Java Projesi sihirbazının ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-03-create-java-project.png)

1. **İlişkili Perspektif Açılsın mı?** penceresi görüntülenirse **Perspektifi Aç**’ı seçin.

1. **Paket gezgini**’nde **hızlı başlangıç** projesine sağ tıklayın.   >  Bağlam menüsünden **Maven projesine dönüştürmeyi** Yapılandır ' ı seçin.

   ![Paket gezgininin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-04-convert-to-maven-project.png)

1. **Yeni POM Oluştur** penceresi görüntülenir. **Grup Kimliği** alanına *com. Microsoft. biliveservices. Speech. Samples* girin ve **yapıt kimliği** alanına *hızlı başlangıç* girin. Ardından **son**' u seçin.

   ![Yeni POM Oluştur penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-05-configure-maven-pom.png)

1. *pom.xml* dosyasını açıp düzenleyin.

   * Dosyanın sonunda, `</project>` kapanış etiketinden önce burada gösterildiği gibi Konuşma SDK’sı için Maven deposuna başvuru içeren bir `repositories` öğesi oluşturun:

     [!code-xml[POM Repositories](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-microphone/pom.xml#repositories)]

   * Ayrıca, bir `dependencies` bağımlılık olarak konuşma SDK sürümü 1.15.0 ile bir öğesi ekleyin:

     [!code-xml[POM Dependencies](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/from-microphone/pom.xml#dependencies)]

   * Değişiklikleri kaydedin.

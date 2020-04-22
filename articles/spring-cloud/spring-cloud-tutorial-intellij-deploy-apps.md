---
title: Öğretici - Azure Bahar Bulutu uygulamalarını dağıtmak için IntelliJ'i kullanın
description: Uygulamaları Azure Bahar Bulutu'na dağıtmak için IntelliJ'i kullanın.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 03/26/2020
ms.openlocfilehash: 8e473a5692c3fa2be3b0f2d823d2a36ba768d661
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81731367"
---
# <a name="use-intellij-to-deploy-azure-spring-cloud-applications"></a>IntelliJ kullanarak Azure Spring Cloud uygulamalarını dağıtma
Azure Bahar Bulutu için IntelliJ eklentisi, IntelliJ IDEA'dan uygulama dağıtımını destekler.  

## <a name="prerequisites"></a>Ön koşullar
* [JDK 8 Azul Zulu](https://docs.microsoft.com/java/azure/jdk/java-jdk-install?view=azure-java-stable)
* [Maven 3.5.0+](https://maven.apache.org/download.cgi)
* [IntelliJ IDEA, Topluluk/Ultimate Edition, sürüm 2020.1/2019.3](https://www.jetbrains.com/idea/download/#section=windows)

## <a name="install-the-plug-in"></a>Eklentiyi yükleme
IntelliJ **Plugins** UI'den IntelliJ IDEA 3.35.0 için Azure Araç Seti'ni ekleyebilirsiniz.

1. IntelliJ'i başlatın.  Daha önce bir proje açtıysanız, hoş geldiniz iletişim kutusunu göstermek için projeyi kapatın. Sağ alttaki bağlantıdan **Yapıla'yı** seçin ve eklenti yapılandırma iletişim kutusunu açmak için **Eklentiler'i** tıklatın ve **diskten Eklentileri Yükle'yi**seçin.

    ![Yapılandırma'yı seçin](media/spring-cloud-intellij-howto/configure-plugin-1.png)

1. IntelliJ için Azure Araç Kiti'ni arayın.  **Yükle'yi**tıklatın.

    ![Eklentiyi yükleyin](media/spring-cloud-intellij-howto/install-plugin.png)

1. **IDE'yi Yeniden Başlat'ı**tıklatın.

## <a name="tutorial-procedures"></a>Öğretici prosedürler
Aşağıdaki yordamlar IntelliJ IDEA kullanarak bir Hello World uygulaması dağıtmak.

* Açık gs-bahar-önyükleme projesi
* Azure İlkbahar Bulutuna Dağıl
* Akış günlüklerini göster

## <a name="open-gs-spring-boot-project"></a>Açık gs-bahar-önyükleme projesi

1. Bu öğretici için kaynak deposunu indirin ve indirin veya unzip'ini yapın veya Git: git klonunu kullanarak klonlahttps://github.com/spring-guides/gs-spring-boot.git 
1. cd içine gs-spring-boot\complete.
1. IntelliJ **Hoşgeldiniz** iletişim kutusunu açın, alma sihirbazını açmak için **Project'i aktar'ı** seçin.
1. Klasörü seçin. `gs-spring-boot\complete`

    ![İthalat Projesi](media/spring-cloud-intellij-howto/import-project-1.png)

## <a name="deploy-to-azure-spring-cloud"></a>Azure İlkbahar Bulutuna Dağıl
Azure'a dağıtmak için Azure hesabınızla oturum açmanız ve aboneliğinizi seçmeniz gerekir.  Oturum açma ayrıntıları için [Yükleme ve oturum açma'ya](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in)bakın.

1. IntelliJ proje gezgininde projenizi sağ tıklatın ve **Azure** -> **Bulutu'na Azure Dağıt'ı**seçin.

    ![Azure 1'e Dağıt](media/spring-cloud-intellij-howto/deploy-to-azure-1.png)

1. **Ad** alanında uygulamanın adını kabul edin. **Ad,** uygulama adını değil yapılandırmayı ifade eder. Kullanıcıların genellikle değiştirmesi gerekmez.
1. Artifon için projeden tanımlayıcıyı kabul **edin.**
1. **Uygulamayı Seçiniz:** ardından **Uygulamayı Oluştur'a tıklayın...**.

    ![Azure 2'ye Dağıt](media/spring-cloud-intellij-howto/deploy-to-azure-2.png)

1. **Uygulama adını**girin, ardından **Tamam'ı**tıklatın.

    ![Azure Ok'a Dağıt](media/spring-cloud-intellij-howto/deploy-to-azure-2a.png)

1. Çalıştır düğmesini tıklatarak dağıtımı **başlatın.** 

    ![Azure 3'e Dağıt](media/spring-cloud-intellij-howto/deploy-to-azure-3.png)

1. Eklenti projedeki komutu `mvn package` çalıştırır ve ardından yeni uygulamayı oluşturur ve `package` komut tarafından oluşturulan kavanozu dağıtır.

1. Uygulama URL'si çıkış penceresinde görünmüyorsa, azure portalından alın. Kaynak grubunuzdan Azure İlkbahar Bulutu örneğine gidin.  Ardından **Uygulamalar'ı**tıklatın.  Çalışan uygulama listelenir.

    ![Test URL'si alın](media/spring-cloud-intellij-howto/get-test-url.png)

1. Tarayıcıdaki URL'ye gidin.

    ![Tarayıcı 2'de Gezinme](media/spring-cloud-intellij-howto/navigate-in-browser-2.png)

## <a name="show-streaming-logs"></a>Akış günlüklerini göster
Günlükleri almak için:
1. **Azure Gezgini'ni**seçin, ardından **Yay Bulutu' nun.**
1. Çalışan uygulamaya sağ tıklayın.
1. Açılan listeden **Akış Günlükleri'ni** seçin.

    ![Akış günlüklerini seçin](media/spring-cloud-intellij-howto/streaming-logs.png)

1. Örneği seçin.

    ![Örnek seçin](media/spring-cloud-intellij-howto/select-instance.png)

1. Akış günlüğü çıkış penceresinde görünür olacaktır.

    ![Akış günlüğü çıktısı](media/spring-cloud-intellij-howto/streaming-log-output.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Bahar Bulutu için Bahar uygulamasını hazırlayın](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment)
* [IntelliJ için Azure Araç Seti hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/)

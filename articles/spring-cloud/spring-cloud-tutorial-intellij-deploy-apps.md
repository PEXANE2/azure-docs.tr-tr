---
title: Öğretici-Azure yay bulut uygulamalarını dağıtmak için IntelliJ kullanma
description: Azure Spring Cloud 'a uygulama dağıtmak için IntelliJ kullanın.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 03/26/2020
ms.openlocfilehash: 8e473a5692c3fa2be3b0f2d823d2a36ba768d661
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81731367"
---
# <a name="use-intellij-to-deploy-azure-spring-cloud-applications"></a>IntelliJ kullanarak Azure Spring Cloud uygulamalarını dağıtma
Azure yay bulutu için IntelliJ eklentisi, IntelliJ FIKRINDEN uygulama dağıtımını destekler.  

## <a name="prerequisites"></a>Ön koşullar
* [JDK 8 Azul Zulu](https://docs.microsoft.com/java/azure/jdk/java-jdk-install?view=azure-java-stable)
* [Maven 3.5.0 +](https://maven.apache.org/download.cgi)
* [IntelliJ fıkır, topluluk/Ultimate Edition, sürüm 2020.1/2019.3](https://www.jetbrains.com/idea/download/#section=windows)

## <a name="install-the-plug-in"></a>Eklentiyi yükler
IntelliJ **eklentileri** kullanıcı ARABIRIMINDEN Azure Toolkit for IntelliJ fikir 3.35.0 ekleyebilirsiniz.

1. IntelliJ 'yi başlatın.  Daha önce bir proje açtıysanız, hoş geldiniz iletişim kutusunu görüntülemek için projeyi kapatın. Sağ alt bağlantıdan **Yapılandır** ' ı seçin ve sonra eklenti yapılandırması iletişim kutusunu **açmak için eklentiler** ' i tıklatın ve **eklentileri diskten yüklemeyi**seçin.

    ![Yapılandır 'ı seçin](media/spring-cloud-intellij-howto/configure-plugin-1.png)

1. Azure Toolkit for IntelliJ arayın.  **Install**'a tıklayın.

    ![Eklentiyi yükler](media/spring-cloud-intellij-howto/install-plugin.png)

1. **IDE 'Yi yeniden Başlat**seçeneğine tıklayın.

## <a name="tutorial-procedures"></a>Öğretici yordamları
Aşağıdaki yordamlar IntelliJ FIKRINI kullanarak bir Merhaba Dünya uygulaması dağıtır.

* Açık GS-yay-önyükleme projesi
* Azure yay bulutuna dağıtın
* Akış günlüklerini göster

## <a name="open-gs-spring-boot-project"></a>Açık GS-yay-önyükleme projesi

1. Bu öğretici için kaynak depoyu indirip sıkıştırmasını açın veya git 'i kullanarak kopyalayın: git Clonehttps://github.com/spring-guides/gs-spring-boot.git 
1. CD 'ye GS-Spring-boot\complete.
1. IntelliJ **karşılama** iletişim kutusunu açın, içeri aktar sihirbazını açmak Için **projeyi içeri aktar** ' ı seçin.
1. Klasör `gs-spring-boot\complete` seçin.

    ![Projeyi içeri aktar](media/spring-cloud-intellij-howto/import-project-1.png)

## <a name="deploy-to-azure-spring-cloud"></a>Azure yay bulutuna dağıtın
Azure 'a dağıtmak için Azure hesabınızla oturum açmanız ve aboneliğinizi seçmeniz gerekir.  Oturum açma ayrıntıları için bkz. [yükleme ve oturum açma](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/create-hello-world-web-app#installation-and-sign-in).

1. IntelliJ Proje Gezgini ' nde projenize sağ tıklayın ve **Azure** -> **yay bulutu 'na Azure dağıtımı**' nı seçin.

    ![Azure 'a dağıtma 1](media/spring-cloud-intellij-howto/deploy-to-azure-1.png)

1. **Ad** alanındaki uygulamanın adını kabul edin. **Ad** , uygulama adı değil, yapılandırmaya başvurur. Kullanıcıların genellikle bunu değiştirmesi gerekmez.
1. **Yapıt**için projeden tanımlayıcıyı kabul edin.
1. **Uygulama seçin:** ardından **uygulama oluştur...** öğesine tıklayın.

    ![Azure 2 ' ye dağıtma](media/spring-cloud-intellij-howto/deploy-to-azure-2.png)

1. **Uygulama adı**girin ve ardından **Tamam**' a tıklayın.

    ![Azure 'a dağıtma Tamam](media/spring-cloud-intellij-howto/deploy-to-azure-2a.png)

1. **Çalıştır** düğmesine tıklayarak dağıtımı başlatın. 

    ![Azure 'a dağıtma 3](media/spring-cloud-intellij-howto/deploy-to-azure-3.png)

1. Eklenti, projede komutunu `mvn package` çalıştırır ve ardından yeni uygulamayı oluşturur ve `package` komut tarafından oluşturulan jar 'yi dağıtır.

1. Uygulama URL 'SI çıkış penceresinde gösterilmezse, Azure portal alın. Kaynak grupınızdan Azure Spring Cloud örneğine gidin.  Ardından **uygulamalar**' a tıklayın.  Çalışan uygulama listelenecektir.

    ![Test URL 'SI al](media/spring-cloud-intellij-howto/get-test-url.png)

1. Tarayıcıdaki URL 'ye gidin.

    ![Tarayıcıda gezin 2](media/spring-cloud-intellij-howto/navigate-in-browser-2.png)

## <a name="show-streaming-logs"></a>Akış günlüklerini göster
Günlükleri almak için:
1. **Azure Gezgini**' ni ve ardından **yay bulutu**' nı seçin.
1. Çalışan uygulamaya sağ tıklayın.
1. Açılan listeden **akış günlükleri** ' ni seçin.

    ![Akış günlüklerini seçme](media/spring-cloud-intellij-howto/streaming-logs.png)

1. Örnek seçin.

    ![Örnek seçin](media/spring-cloud-intellij-howto/select-instance.png)

1. Akış günlüğü, çıkış penceresinde görünür olur.

    ![Akış günlüğü çıkışı](media/spring-cloud-intellij-howto/streaming-log-output.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure yay bulutu için yay uygulamasını hazırlama](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment)
* [Azure Toolkit for IntelliJ hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/developer/java/toolkit-for-intellij/)

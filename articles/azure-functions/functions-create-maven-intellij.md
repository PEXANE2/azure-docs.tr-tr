---
title: Java ve IntelliJ ile Azure işlevi oluşturma
description: Java ve IntelliJ ile Azure 'da basit bir HTTP ile tetiklenen, sunucusuz bir uygulama oluşturmayı ve yayımlamayı öğrenin.
author: jeffhollan
ms.topic: how-to
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: b1eb430f94ef545fa74ed225c427a121a47f7cf8
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87055794"
---
# <a name="create-your-first-azure-function-with-java-and-intellij"></a>Java ve IntelliJ ile ilk Azure işlevinizi oluşturma

Bu makale:
- IntelliJ FIKRIYLE [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) bir işlev projesi oluşturma
- Kendi bilgisayarınızdaki tümleşik geliştirme ortamında (IDE) işlevi test etme ve hata ayıklama adımları
- İşlev projesini Azure Işlevlerine dağıtmaya yönelik yönergeler

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Java ve IntelliJ ile bir işlev geliştirmek için aşağıdaki yazılımı yüklemelisiniz:

+ Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
+ Java 8 için [desteklenen bir Azure Java Geliştirme Seti (JDK)](https://aka.ms/azure-jdks)
+ Yüklü [IntelliJ IDEA](https://www.jetbrains.com/idea/download/) Ultimate Edition veya Community Edition
+ [Maven 3.5.0 +](https://maven.apache.org/download.cgi)
+ En son [Işlev çekirdek araçları](https://github.com/Azure/azure-functions-core-tools)


## <a name="installation-and-sign-in"></a>Yükleme ve Oturum Açma

1. IntelliJ IDEA'nın Ayarlar/Tercihler iletişim kutusunda (Ctrl+Alt+S) **Eklentiler**’i seçin. Sonra **Marketplace**’de **Azure Toolkit for IntelliJ**’yi bulun ve **Yükle**’ye tıklayın. Yüklendikten sonra **Yeniden Başlat**’a tıklayarak eklentiyi etkinleştirin. 

    ![Marketplace’de Azure Toolkit for IntelliJ eklentisi][marketplace]

2. Azure hesabınızda oturum açmak için kenar çubuğunda **Azure Gezgini**’ni açın ve sonra üstteki çubukta **Azure Oturum Açma** simgesine (veya IDEA menüsünden **Araçlar/Azure/Azure Oturum Açma**) tıklayın.
    ![IntelliJ Azure Oturum Açma komutu][intellij-azure-login]

3. **Azure Oturum Açma** penceresinde **Cihaz Oturumu**’nu seçin ve ardından **Oturum aç**’a tıklayın ([diğer oturum açma seçenekleri](/azure/developer/java/toolkit-for-intellij/sign-in-instructions)).

   ![Cihaz oturumu’nun seçili olduğu Azure Oturum Açma penceresi][intellij-azure-popup]

4. **Azure Cihaz Oturum Açma** iletişim kutusunda **Kopyala ve Aç**’a tıklayın.

   ![Azure Oturum Açma İletişim Kutusu penceresi][intellij-azure-copycode]

5. Tarayıcıda cihaz kodunuzu yapıştırın (son adımda **Kopyala ve Aç**’a tıkladığınızda kopyalanmış olan kod) ve ardından **İleri**’ye tıklayın.

   ![Cihaz oturumu tarayıcısı][intellij-azure-link-ms-account]

6. **Abonelik Seç** iletişim kutusunda kullanmak istediğiniz abonelikleri seçin ve sonra **Tamam**’a tıklayın.

   ![Abonelik Seç iletişim kutusu][intellij-azure-login-select-subs]
   
## <a name="create-your-local-project"></a>Yerel projenizi oluşturma

Bu bölümde, yerel bir Azure Işlevleri projesi oluşturmak için Azure Toolkit for IntelliJ kullanırsınız. Bu makalenin ilerleyen kısımlarında, işlev kodunuzu Azure 'a yayımlayacaksınız. 

1. IntelliJ karşılama iletişim kutusunu açın, yeni *proje oluştur* ' u seçerek yeni bir proje Sihirbazı açın, *Azure işlevleri*' ni seçin.

    ![İşlevler projesi oluştur](media/functions-create-first-java-intellij/create-functions-project.png)

1. *Http tetikleyicisi*' ni seçin ve ardından *İleri* ' ye tıklayın ve aşağıdaki sayfalardaki tüm yapılandırmalara gitmek için Sihirbazı izleyin; projenizin konumunu doğrulayın ve *son*' a tıklayın. Intellj fıkrı daha sonra yeni projenizi açacak.

    ![İşlev oluşturma proje sonu](media/functions-create-first-java-intellij/create-functions-project-finish.png)

## <a name="run-the-function-app-locally"></a>İşlev Uygulaması yerel olarak çalıştırma

1. `src/main/java/org/example/functions/HttpTriggerFunction.java`Oluşturulan kodu görmek için bölümüne gidin. *17*. satırın yanında, bir yeşil *çalışma* düğmesi olduğunu fark edeceksiniz ve *' Azure-Function-sınavu Çalıştır... '* seçeneğini belirlediğinizde, işlev uygulamanızın bir kaç günlükle yerel olarak çalıştığını görürsünüz.

    ![Yerel çalıştırma işlevleri projesi](media/functions-create-first-java-intellij/local-run-functions-project.png)

    ![Yerel çalıştırma işlevleri çıkışı](media/functions-create-first-java-intellij/local-run-functions-output.png)

1. Yazdırılmış uç noktaya, gibi tarayıcıdan erişerek işlevi deneyebilirsiniz `http://localhost:7071/api/HttpTrigger-Java?name=Azure` .

    ![Yerel çalıştırma işlevleri test sonucu](media/functions-create-first-java-intellij/local-run-functions-test.png)

1. Günlük Ayrıca fıkır olarak da yazdırılır, şimdi *Durdur* düğmesine tıklayarak işlevi durdurun.

    ![Yerel çalıştırma işlevleri test günlüğü](media/functions-create-first-java-intellij/local-run-functions-log.png)

## <a name="debug-the-function-app-locally"></a>İşlev Uygulaması yerel olarak hata ayıklama

1. Şimdi İşlev Uygulaması yerel olarak hata ayıklamanın ardından araç çubuğunda *Hata Ayıkla* düğmesine tıklayın (görmüyorsanız, araç çubuğunu etkinleştirmek için görüntüle-> görünüm *-> araç çubuğuna* tıklayın).

    ![Yerel hata ayıklama işlevleri düğmesi](media/functions-create-first-java-intellij/local-debug-functions-button.png)

1. Bir kesme noktası eklemek için dosyanın *20* . satırına tıklayın, `src/main/java/org/example/functions/HttpTriggerFunction.java` uç noktaya yeniden erişin, `http://localhost:7071/api/HttpTrigger-Java?name=Azure` kesme noktasının isabet olduğunu fark edersiniz. *adım*, *izleme*, *değerlendirme*gibi daha fazla hata ayıklama özelliğini deneyebilirsiniz. Durdur düğmesine tıklayarak hata ayıklama oturumunu durdurun.

    ![Yerel hata ayıklama işlevleri sonu](media/functions-create-first-java-intellij/local-debug-functions-break.png)

## <a name="deploy-your-function-app-to-azure"></a>İşlev Uygulaması Azure 'a dağıtın

1. IntelliJ Proje Gezgini ' nde projenize sağ tıklayın, Azure *-> dağıtım işlevleri* ' ni seçin.

    ![İşlevleri Azure 'a dağıtma](media/functions-create-first-java-intellij/deploy-functions-to-azure.png)

1. Henüz bir İşlev Uygulaması yoksa, *kullanılabilir bir Işlev yok ' a tıklayın, yeni bir tane oluşturmak için tıklayın*.

    ![Azure uygulama oluşturmak için işlevleri dağıtma](media/functions-create-first-java-intellij/deploy-functions-create-app.png)

1. Işlev uygulaması adı ' nı yazın ve uygun abonelik/platform/kaynak grubu/App Service planı ' nı seçin, buradan kaynak grubu/App Service planı da oluşturabilirsiniz. Ardından, uygulama ayarlarını değişmeden tutun, *Tamam* ' a tıklayın ve yeni işlevin oluşturulması için birkaç dakika bekleyin. *Yeni işlev uygulaması* oluşturulduktan sonra... ilerleme çubuğu kaybolur.

    ![Azure uygulama oluşturma sihirbazına işlevleri dağıtma](media/functions-create-first-java-intellij/deploy-functions-create-app-wizard.png)

1. Dağıtmak istediğiniz işlev uygulamasını seçin (az önce oluşturduğunuz yeni işlev uygulaması otomatik olarak seçilir). İşlevlerinizi dağıtmak için *Çalıştır* ' a tıklayın.

    ![Azure çalıştırmasına işlevleri dağıtma](media/functions-create-first-java-intellij/deploy-functions-run.png)

    ![Azure günlüğüne işlevleri dağıtma](media/functions-create-first-java-intellij/deploy-functions-log.png)

## <a name="manage-azure-functions-from-idea"></a>Azure Işlevlerini fıkır olarak yönetme

1. İşlevlerinizi *Azure Explorer* ile yönetebilirsiniz. *işlev uygulaması*' ye tıkladığınızda, tüm işlevlerinizi burada görürsünüz.

    ![Gezgin 'de işlevleri görüntüleme](media/functions-create-first-java-intellij/explorer-view-functions.png)

1. İşlevlerinizin birini seçin ve sağ tıklayın, *özellikleri göster* ' i seçerek ayrıntı sayfasını açın. 

    ![İşlev özelliklerini göster](media/functions-create-first-java-intellij/explorer-functions-show-properties.png)

1. *Httptrigger-Java*işlevinizi sağ tıklatın ve *tetikleyici işlevi*' ni SEÇIN, tarayıcının tetikleyici URL 'si ile açıldığını görürsünüz.

    ![Azure çalıştırmasına işlevleri dağıtma](media/functions-create-first-java-intellij/explorer-trigger-functions.png)

## <a name="add-more-functions-to-the-project"></a>Projeye daha fazla Işlev ekleyin

1. *Org. example. Functions* paketine sağ tıklayın ve *New-> Azure Function Class*' ı seçin. 

    ![Proje girişine işlevler ekleyin](media/functions-create-first-java-intellij/add-functions-entry.png)

1. *Httptest* sınıf adı ' nı doldurup *httptrigger* ' ı seçin işlev sınıfı oluşturma sihirbazında, oluşturmak için *Tamam* ' a tıklayın, bu şekilde, istediğiniz gibi yeni işlevler oluşturabilirsiniz.

    ![Projeye işlev Ekle tetikleyiciyi seçin](media/functions-create-first-java-intellij/add-functions-trigger.png)
    
    ![Proje çıkışına işlevler ekleyin](media/functions-create-first-java-intellij/add-functions-output.png)

## <a name="cleaning-up-functions"></a>Işlevleri Temizleme

1. Azure Explorer 'da Azure Işlevlerini silme
      
      ![Projeye işlev Ekle tetikleyiciyi seçin](media/functions-create-first-java-intellij/delete-function.png)
      

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen bir işlev içeren bir Java işlevleri projesi oluşturdunuz, yerel makinenizde çalıştırın ve Azure 'a dağıttınız. Şimdi, işlevinizi genişletin...

> [!div class="nextstepaction"]
> [Azure depolama kuyruğu çıkış bağlaması ekleme](./functions-add-output-binding-storage-queue-java.md)


[marketplace]:./media/functions-create-first-java-intellij/marketplace.png
[intellij-azure-login]: media/functions-create-first-java-intellij/intellij-azure-login.png
[intellij-azure-popup]: media/functions-create-first-java-intellij/intellij-azure-login-popup.png
[intellij-azure-copycode]: media/functions-create-first-java-intellij/intellij-azure-login-copyopen.png
[intellij-azure-link-ms-account]: media/functions-create-first-java-intellij/intellij-azure-login-linkms-account.png
[intellij-azure-login-select-subs]: media/functions-create-first-java-intellij/intellij-azure-login-selectsubs.png

---
title: IntelliJ kullanarak Azure Işlevlerinde bir Kotlin işlevi oluşturma
description: IntelliJ kullanarak, daha sonra Azure 'da sunucusuz bir ortamda çalışmak üzere yayımladığınız basit bir HTTP ile tetiklenen bir Kotlin işlevi oluşturma hakkında bilgi edinin.
author: dglover
ms.service: azure-functions
ms.topic: quickstart
ms.date: 03/25/2020
ms.author: dglover
ms.openlocfilehash: 0207e4af9f845343866714ec207ca306cb327b36
ms.sourcegitcommit: c4c554db636f829d7abe70e2c433d27281b35183
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98035181"
---
# <a name="create-your-first-kotlin-function-in-azure-using-intellij"></a>IntelliJ kullanarak Azure 'da ilk Kotlin işlevinizi oluşturma

Bu makalede, bir IntelliJ fıkır projesinde HTTP ile tetiklenen bir Java işlevi oluşturma, tümleşik geliştirme ortamında (IDE) projeyi çalıştırma ve hata ayıklama ve son olarak işlev projesini Azure 'da bir işlev uygulamasına dağıtma işlemlerinin nasıl yapılacağı gösterilmektedir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

IntelliJ kullanarak Azure 'da Kotlin işlevleri oluşturup yayımlamak için aşağıdaki yazılımı yükle:

- [Java geliştirici seti](/azure/developer/java/fundamentals/java-jdk-long-term-support) (JDK), sürüm 8
- [Apache Maven](https://maven.apache.org), sürüm 3,0 veya üzeri
- [IntelliJ fikir](https://www.jetbrains.com/idea/download), Maven ile topluluk veya Ultimate sürümleri
- [Azure CLI](/cli/azure)
- Azure Functions Core Tools [sürüm 2. x](functions-run-local.md#v2) . Azure Işlevleri yazmak, çalıştırmak ve hata ayıklamak için yerel bir geliştirme ortamı sağlar.

> [!IMPORTANT]
> JAVA_HOME ortam değişkeni, bu makaledeki adımları tamamlayabilmeniz için JDK 'nin Install konumuna ayarlanmalıdır.

## <a name="create-a-function-project"></a>İşlev projesi oluşturma

1. IntelliJ fıkır bölümünde **Yeni proje oluştur**' u seçin.  
1. **Yeni proje** penceresinde sol bölmeden **Maven** ' ı seçin.
1. Bir arşiv **ETYPE oluştur** onay kutusunu seçin ve ardından [Azure-Functions-Kotlin-ızetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-kotlin-archetype)için arşiv **ETYPE Ekle** ' yi seçin.
1. Arşiv **Ekle** penceresinde, alanları aşağıdaki gibi doldurun:
    - _GroupID_: com. Microsoft. Azure
    - _ArtifactId_: Azure-Functions-Kotlin-arşiv Etype
    - _Sürüm_: [Orta depodan](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-kotlin-archetype)en son sürümü kullanın 
     ![ IntelliJ IDEA bir Maven projesi oluşturun](media/functions-create-first-kotlin-intellij/functions-create-intellij.png)  
1. **Tamam**' ı ve ardından **İleri**' yi seçin.
1. Geçerli proje için ayrıntılarınızı girin ve **son**' u seçin.

Maven, proje dosyalarını _ArtifactId_ değeriyle aynı ada sahip yeni bir klasörde oluşturur. Projenin oluşturulan kodu, tetikleme HTTP isteğinin gövdesini yansıtan basit bir [http ile tetiklenen](./functions-bindings-http-webhook.md) bir işlevdir.

## <a name="run-project-locally-in-the-ide"></a>Projeyi IDE 'de yerel olarak çalıştır

> [!NOTE]
> Projeyi yerel olarak çalıştırmak ve hatalarını ayıklamak için [Azure Functions Core Tools, sürüm 2 ' yi](functions-run-local.md#v2)yüklediğinizden emin olun.

1. Değişiklikleri el ile içeri aktarın veya [otomatik içeri aktarmayı](https://www.jetbrains.com/help/idea/creating-and-optimizing-imports.html)etkinleştirin.
1. **Maven projeleri** araç çubuğunu açın.
1. **Yaşam döngüsü**' ni genişletin ve ardından **paketi** açın. Çözüm yeni oluşturulan bir hedef dizinde oluşturulup paketlenmiştir.
1. **Eklentiler**  >  **Azure-Functions** ' i genişletin ve Azure işlevleri 'ni açın. Azure işlevleri yerel çalışma zamanına başlamak için **çalıştırın** .  
  ![Azure Işlevleri için Maven araç çubuğu](media/functions-create-first-kotlin-intellij/functions-intellij-kotlin-maven-toolbar.png)  

1. İşlevinizi test etmeyi bitirdiğinizde Çalıştır iletişim kutusunu kapatın. Tek seferde yalnızca bir işlev Konağı etkin ve yerel olarak çalıştırılabilir.

## <a name="debug-the-project-in-intellij"></a>IntelliJ 'de projede hata ayıkla

1. İşlev konağını hata ayıklama modunda başlatmak için, işlevinizi çalıştırdığınızda bağımsız değişken olarak ' **denabledebug** ' ekleyin. [Maven hedeflerde](https://www.jetbrains.com/help/idea/maven-support.html#run_goal) yapılandırmayı değiştirebilir veya bir Terminal penceresinde aşağıdaki komutu çalıştırabilirsiniz:  

   ```
   mvn azure-functions:run -DenableDebug
   ```

   Bu komut, işlev ana bilgisayarının 5005 adresinden bir hata ayıklama bağlantı noktası açmasına neden olur.

1. **Çalıştır** menüsünde, **konfigürasyonları Düzenle**' yi seçin.
1. **Uzaktan** eklemek için **(+)** seçeneğini belirleyin.
1. _Ad_ ve _Ayarlar_ alanlarını doldurun ve ardından yapılandırmayı kaydetmek için **Tamam** ' ı seçin.
1. Kurulumdan sonra, hata ayıklamayı başlatmak için **< uzak yapılandırma adı >hata ayıkla** ' yı seçin veya klavyenizde SHIFT + F9 tuşlarına basın.

   ![IntelliJ 'de hata ayıklama projesi](media/functions-create-first-kotlin-intellij/debug-configuration-intellij.PNG)

1. İşiniz bittiğinde, hata ayıklayıcıyı ve çalışan işlemi durdurun. Tek seferde yalnızca bir işlev Konağı etkin ve yerel olarak çalıştırılabilir.

## <a name="deploy-the-project-to-azure"></a>Projeyi Azure’a dağıtma

1. Projenizi Azure 'da bir işlev uygulamasına dağıtabilmeniz için önce [Azure CLI 'yi kullanarak oturum](/cli/azure/authenticate-azure-cli?view=azure-cli-latest)açmalısınız.

   ``` azurecli
   az login
   ```

1. Maven hedefini kullanarak kodunuzu yeni bir işlev uygulamasına dağıtın `azure-functions:deploy` . Ayrıca Maven projeleri penceresinde **Azure-Functions: dağıt** seçeneğini de belirleyebilirsiniz.

   ```
   mvn azure-functions:deploy
   ```

1. İşlev uygulaması başarıyla dağıtıldıktan sonra, Azure CLı çıktısında HTTP tetikleyici işlevinizin URL 'sini bulun.

   ``` output
   [INFO] Successfully deployed Function App with package.
   [INFO] Deleting deployment package from Azure Storage...
   [INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
   [INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   ```

## <a name="next-steps"></a>Sonraki adımlar

İlk Kotlin işlev uygulamanızı Azure 'a dağıttığınıza göre, Java ve Kotlin işlevleri geliştirme hakkında daha fazla bilgi için [Azure Işlevleri Java geliştirici kılavuzu](functions-reference-java.md) ' nu gözden geçirin.
- Maven hedefini kullanarak projenize farklı tetikleyicilerle ek işlev uygulamaları ekleyin `azure-functions:add` .

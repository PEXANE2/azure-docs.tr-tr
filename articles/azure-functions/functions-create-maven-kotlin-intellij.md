---
title: Kotlin ve IntelliJ ile bir Azure işlevi oluşturma
description: Kotlin ve IntelliJ ile Azure 'da basit bir HTTP ile tetiklenen, sunucusuz bir uygulama oluşturmayı ve yayımlamayı öğrenin.
author: dglover
ms.service: azure-functions
ms.topic: quickstart
ms.date: 03/25/2020
ms.author: dglover
ms.openlocfilehash: fa834e3ac4946c4f617e857342f850445eebfd30
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87055447"
---
# <a name="quickstart-create-your-first-http-triggered-function-with-kotlin-and-intellij"></a>Hızlı başlangıç: Kotlin ve IntelliJ ile ilk HTTP tetiklenen işlevinizi oluşturma

Bu makalede, IntelliJ ıDEA ve Apache Maven ile [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) bir işlev projesi oluşturma işlemi gösterilir. Ayrıca, işlev kodunuzda tümleşik geliştirme ortamında (IDE) yerel olarak hata ayıklamanın nasıl yapılacağını ve ardından işlev projesini Azure 'a dağıtmayı gösterir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Kotlin ve IntelliJ ile bir işlev geliştirmek için aşağıdaki yazılımı yükle:

- [Java geliştirici seti](https://aka.ms/azure-jdks) (JDK), sürüm 8
- [Apache Maven](https://maven.apache.org), sürüm 3,0 veya üzeri
- [IntelliJ fikir](https://www.jetbrains.com/idea/download), Maven ile topluluk veya Ultimate sürümleri
- [Azure CLI](/cli/azure)
- Azure Functions Core Tools [sürüm 2. x](functions-run-local.md#v2) . Azure Işlevleri yazmak, çalıştırmak ve hata ayıklamak için yerel bir geliştirme ortamı sağlar.

> [!IMPORTANT]
> JAVA_HOME ortam değişkeni, bu makaledeki adımları tamamlayabilmeniz için JDK 'nin Install konumuna ayarlanmalıdır.

## <a name="create-a-functions-project"></a>Işlevler projesi oluşturma

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

## <a name="run-functions-locally-in-the-ide"></a>İşlevleri IDE 'de yerel olarak çalıştırma

> [!NOTE]
> İşlevleri yerel olarak çalıştırmak ve hatalarını ayıklamak için [Azure Functions Core Tools, sürüm 2 ' yi](functions-run-local.md#v2)yüklediğinizden emin olun.

1. Değişiklikleri el ile içeri aktarın veya [otomatik içeri aktarmayı](https://www.jetbrains.com/help/idea/creating-and-optimizing-imports.html)etkinleştirin.
1. **Maven projeleri** araç çubuğunu açın.
1. **Yaşam döngüsü**' ni genişletin ve ardından **paketi**açın. Çözüm yeni oluşturulan bir hedef dizinde oluşturulup paketlenmiştir.
1. **Eklentiler**  >  **Azure-Functions** ' i genişletin ve Azure işlevleri 'ni açın. Azure işlevleri yerel çalışma zamanına başlamak için **çalıştırın** .  
  ![Azure Işlevleri için Maven araç çubuğu](media/functions-create-first-kotlin-intellij/functions-intellij-kotlin-maven-toolbar.png)  

1. İşlevinizi test etmeyi bitirdiğinizde Çalıştır iletişim kutusunu kapatın. Tek seferde yalnızca bir işlev Konağı etkin ve yerel olarak çalıştırılabilir.

## <a name="debug-the-function-in-intellij"></a>IntelliJ 'de işlevde hata ayıkla

1. İşlev konağını hata ayıklama modunda başlatmak için, işlevinizi çalıştırdığınızda bağımsız değişken olarak ' **denabledebug** ' ekleyin. [Maven hedeflerde](https://www.jetbrains.com/help/idea/maven-support.html#run_goal) yapılandırmayı değiştirebilir veya bir Terminal penceresinde aşağıdaki komutu çalıştırabilirsiniz:  

   ```
   mvn azure-functions:run -DenableDebug
   ```

   Bu komut, işlev ana bilgisayarının 5005 adresinden bir hata ayıklama bağlantı noktası açmasına neden olur.

1. **Çalıştır** menüsünde, **konfigürasyonları Düzenle**' yi seçin.
1. **Uzaktan**eklemek için **(+)** seçeneğini belirleyin.
1. _Ad_ ve _Ayarlar_ alanlarını doldurun ve ardından yapılandırmayı kaydetmek için **Tamam** ' ı seçin.
1. Kurulumdan sonra, hata ayıklamayı başlatmak için **< uzak yapılandırma adı >hata ayıkla** ' yı seçin veya klavyenizde SHIFT + F9 tuşlarına basın.

   ![IntelliJ 'de hata ayıklama işlevleri](media/functions-create-first-kotlin-intellij/debug-configuration-intellij.PNG)

1. İşiniz bittiğinde, hata ayıklayıcıyı ve çalışan işlemi durdurun. Tek seferde yalnızca bir işlev Konağı etkin ve yerel olarak çalıştırılabilir.

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

1. İşlevinizi Azure 'a dağıtabilmeniz için önce [Azure CLI 'yi kullanarak oturum](/cli/azure/authenticate-azure-cli?view=azure-cli-latest)açmalısınız.

   ``` azurecli
   az login
   ```

1. Maven hedefini kullanarak kodunuzu yeni bir işleve dağıtın `azure-functions:deploy` . Ayrıca Maven projeleri penceresinde **Azure-Functions: dağıt** seçeneğini de belirleyebilirsiniz.

   ```
   mvn azure-functions:deploy
   ```

1. İşlev başarıyla dağıtıldıktan sonra, Azure CLı çıktısında işlevinizin URL 'sini bulun.

   ``` output
   [INFO] Successfully deployed Function App with package.
   [INFO] Deleting deployment package from Azure Storage...
   [INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
   [INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   ```

## <a name="next-steps"></a>Sonraki adımlar

İlk Kotlin işlevinizi Azure 'a dağıttığınıza göre, Java ve Kotlin işlevleri geliştirme hakkında daha fazla bilgi için [Java işlevleri Geliştirici Kılavuzu](functions-reference-java.md) ' nu gözden geçirin.
- Maven hedefini kullanarak projenize farklı tetikleyicilerle ek işlevler ekleyin `azure-functions:add` .

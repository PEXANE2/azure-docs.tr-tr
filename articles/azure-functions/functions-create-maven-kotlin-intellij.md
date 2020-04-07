---
title: Kotlin ve IntelliJ ile Azure işlevi oluşturun
description: Kotlin ve IntelliJ ile Azure'da HTTP tarafından tetiklenen basit bir uygulamayı nasıl oluşturup yayınlayacağınızı öğrenin.
author: dglover
ms.service: azure-functions
ms.topic: quickstart
ms.date: 03/25/2020
ms.author: dglover
ms.openlocfilehash: 2eb1a016e04a4150a76112c68683926810f5c66d
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80674100"
---
# <a name="quickstart-create-your-first-http-triggered-function-with-kotlin-and-intellij"></a>Quickstart: Kotlin ve IntelliJ ile ilk HTTP tetiklenen işlevi oluşturun

Bu makalede, IntelliJ IDEA ve Apache Maven ile [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) bir işlev projesi nasıl oluşturulacağını gösterir. Ayrıca, tümleşik geliştirme ortamında (IDE) işlev kodunuzu yerel olarak nasıl ayıklandığınızı ve ardından işlev projenizi Azure'a nasıl dağıttığınızı da gösterir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Kotlin ve IntelliJ ile bir işlev geliştirmek için aşağıdaki yazılımı yükleyin:

- [Java Geliştirici Kiti](https://aka.ms/azure-jdks) (JDK), sürüm 8
- [Apache Maven](https://maven.apache.org), sürüm 3.0 veya daha yüksek
- [IntelliJ IDEA](https://www.jetbrains.com/idea/download), Maven ile Topluluk veya Ultimate sürümleri
- [Azure CLI](https://docs.microsoft.com/cli/azure)
- Azure İşlevler Temel Araçları sürümü [2.x.](functions-run-local.md#v2) Azure İşlevlerini yazmak, çalıştırmak ve hata ayıklama için yerel bir geliştirme ortamı sağlar.

> [!IMPORTANT]
> Bu makaledeki adımları tamamlamak için JAVA_HOME ortamı değişkeni JDK'nın yükleme konumuna ayarlanmalıdır.

## <a name="create-a-functions-project"></a>İşlevler projesi oluşturma

1. IntelliJ IDEA'da **Yeni Proje Oluştur'u**seçin.  
1. Yeni **Proje** penceresinde, sol bölmeden **Maven'i** seçin.
1. **Arketip onay kutusundan Oluştur'u** seçin ve ardından [azure fonksiyonları-kotlin-archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-kotlin-archetype)için **Arketip Ekle'yi** seçin.
1. **Arketip Ekle** penceresinde, alanları aşağıdaki gibi tamamlayın:
    - _GroupId_: com.microsoft.azure
    - _ArtifactId_: azure-functions-kotlin-archetype
    - _Sürüm_:
    ![ [IntelliJ IDEA'da](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-kotlin-archetype)arketipten bir Maven projesi oluşturma merkezi deposundan en son sürümü kullanın](media/functions-create-first-kotlin-intellij/functions-create-intellij.png)  
1. **Tamam'ı**seçin ve sonra **İleri'yi**seçin.
1. Geçerli proje için bilgilerinizi girin ve **Finish'i**seçin.

Maven, proje dosyalarını _ArtifactId_ değeriyle aynı ada sahip yeni bir klasörde oluşturur. Projenin oluşturulan kodu, tetikleyen HTTP isteğinin gövdesini yansıtan basit bir [HTTP tetikleme](/azure/azure-functions/functions-bindings-http-webhook) işlevidir.

## <a name="run-functions-locally-in-the-ide"></a>IDE'de işlevleri yerel olarak çalıştırın

> [!NOTE]
> İşlevleri yerel olarak çalıştırmak ve hata ayıklamak için [Azure İşlevler Temel Araçları'nı sürüm 2'ye](functions-run-local.md#v2)yüklediğinizden emin olun.

1. Değişiklikleri el ile içe aktarın veya [otomatik içe aktarmayı etkinleştirin.](https://www.jetbrains.com/help/idea/creating-and-optimizing-imports.html)
1. **Maven Projeleri** araç çubuğunu açın.
1. **Yaşam Döngüsünü**genişletin ve **ardından paketi**açın. Çözüm, yeni oluşturulan hedef dizinde oluşturulur ve paketlenir.
1. **Eklentileri** > **azure işlevlerini** genişletin ve **azure işlevlerini açın:Azure** İşlerini yerel çalışma saatini başlatmak için çalıştırın.  
  ![Azure İşlevler için Maven araç çubuğu](media/functions-create-first-kotlin-intellij/functions-intellij-kotlin-maven-toolbar.png)  

1. İşlevinizi test etmeniz bittiğinde çalıştır iletişim kutusunu kapatın. Aynı anda yalnızca bir işlev ana bilgisayarı etkin olabilir ve yerel olarak çalışabilir.

## <a name="debug-the-function-in-intellij"></a>IntelliJ işlevi hata ayıklama

1. İşlev ana bilgisayarını hata ayıklama modunda başlatmak için, işlevinizi çalıştırdığınızda bağımsız değişken olarak **-DenableDebug'ı** ekleyin. [Maven hedeflerinde](https://www.jetbrains.com/help/idea/maven-support.html#run_goal) yapılandırmayı değiştirebilir veya terminal penceresinde aşağıdaki komutu çalıştırabilirsiniz:  

   ```
   mvn azure-functions:run -DenableDebug
   ```

   Bu komut, işlev ana bilgisayarının 5005'te hata ayıklama bağlantı noktasını açmasına neden olur.

1. **Çalıştır** menüsünde **Yapılandırmaları Edit'i**seçin.
1. **Uzaktan Kumanda**eklemek için **(+)** seçeneğini belirleyin.
1. _Ad_ ve _Ayarlar_ alanlarını tamamlayın ve yapılandırmayı kaydetmek için **Tamam'ı** seçin.
1. Kurulumdan sonra Hata **Ayıklama < Remote Configuration Name >'yi** seçin veya hata ayıklamaya başlamak için klavyenizdeki Shift+F9 tuşuna basın.

   ![IntelliJ'de Hata Ayıklama işlevleri](media/functions-create-first-kotlin-intellij/debug-configuration-intellij.PNG)

1. İşi bittiğinde hata ayıklama işlemini ve çalıştırma işlemini durdurun. Aynı anda yalnızca bir işlev ana bilgisayarı etkin olabilir ve yerel olarak çalışabilir.

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

1. İşlevinizi Azure'a dağıtmadan önce, [Azure CLI'yi kullanarak oturum açmanız](/cli/azure/authenticate-azure-cli?view=azure-cli-latest)gerekir.

   ``` azurecli
   az login
   ```

1. `azure-functions:deploy` Maven hedefini kullanarak kodunuzu yeni bir işleve dağıtın. Maven Projeleri penceresinde **azure işlevleri:dağıt** seçeneğini de seçebilirsiniz.

   ```
   mvn azure-functions:deploy
   ```

1. İşlev başarıyla dağıtıldıktan sonra Azure CLI çıktısında işlevinizin URL'sini bulun.

   ``` output
   [INFO] Successfully deployed Function App with package.
   [INFO] Deleting deployment package from Azure Storage...
   [INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
   [INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   ```

## <a name="next-steps"></a>Sonraki adımlar

İlk Kotlin işlevinizi Azure'a dağıttığınıza göre, Java ve Kotlin işlevleri geliştirme hakkında daha fazla bilgi için [Java Fonksiyonları geliştirici kılavuzunu](functions-reference-java.md) inceleyin.
- `azure-functions:add` Maven hedefini kullanarak projenize farklı tetikleyicilerle ek işlevler ekleyin.

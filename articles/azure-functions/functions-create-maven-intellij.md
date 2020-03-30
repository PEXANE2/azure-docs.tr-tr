---
title: Java ve IntelliJ ile bir Azure işlevi oluşturun
description: Java ve IntelliJ ile Azure'da HTTP tarafından tetiklenen basit bir uygulamayı nasıl oluşturup yayınlayacağınızı öğrenin.
author: jeffhollan
ms.topic: conceptual
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: 7003dc19a7bfc405809de91534028aba8e0416c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136859"
---
# <a name="create-your-first-azure-function-with-java-and-intellij"></a>Java ve IntelliJ ile ilk Azure işlevinizi oluşturun

Bu makale:
- Nasıl IntelliJ IDEA ve Apache Maven ile [bir sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) fonksiyon projesi oluşturmak için
- Tümleşik geliştirme ortamındaki (IDE) işlevi kendi bilgisayarınızda sınama ve hata ayıklama adımları
- İşlev projesini Azure İşlevleri'ne dağıtma yönergeleri

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Java ve IntelliJ ile bir işlev geliştirmek için aşağıdaki yazılımı yükleyin:

- [Java Geliştirici Kiti](https://www.azul.com/downloads/zulu/) (JDK), sürüm 8
- [Apache Maven](https://maven.apache.org), sürüm 3.0 veya daha yüksek
- [IntelliJ IDEA](https://www.jetbrains.com/idea/download), Maven ile Topluluk veya Ultimate sürümleri
- [Azure CLI](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT]
> Bu makaledeki adımları tamamlamak için JAVA_HOME ortamı değişkeni JDK'nın yükleme konumuna ayarlanmalıdır.

 [Azure İşlevler Temel Araçları'nı, sürüm 2'yi](functions-run-local.md#v2)yüklemenizi öneririz. Azure İşlevlerini yazmak, çalıştırmak ve hata ayıklama için yerel bir geliştirme ortamı sağlar.

## <a name="create-a-functions-project"></a>İşlevler projesi oluşturma

1. IntelliJ IDEA'da **Yeni Proje Oluştur'u**seçin.  
1. Yeni **Proje** penceresinde, sol bölmeden **Maven'i** seçin.
1. **Arketip onay kutusundan Oluştur'u** seçin ve ardından [azure işlevleri-arketipi](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)için **Arketip Ekle'yi** seçin.
1. **Arketip Ekle** penceresinde, alanları aşağıdaki gibi tamamlayın:
    - _GroupId_: com.microsoft.azure
    - _ArtifactId_: azure-fonksiyonlar-arketip
    - _Sürüm_:
    ![IntelliJ IDEA'da arketipten bir Maven projesi oluşturun [merkezi deposundan](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)en son sürümü kontrol edin ve kullanın](media/functions-create-first-java-intellij/functions-create-intellij.png)  
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
  ![Azure İşlevler için Maven araç çubuğu](media/functions-create-first-java-intellij/functions-intellij-java-maven-toolbar.png)  

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

- Java işlevleri geliştirme hakkında daha fazla bilgi edinmek için [Java İşlevleri geliştirici kılavuzunu](functions-reference-java.md) gözden geçirin.
- `azure-functions:add` Maven hedefini kullanarak projenize farklı tetikleyicilerle ek işlevler ekleyin.

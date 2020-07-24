---
title: Java ve tutulma ile bir Azure işlev uygulaması oluşturma
description: Java kullanarak basit bir HTTP ile tetiklenen sunucusuz uygulama oluşturma ve Yayımlama Kılavuzu ve Azure Işlevleri ile çakışan küreler.
author: jeffhollan
ms.topic: how-to
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: 6f521cccdc8814fad8605460bf87c0d0fca20378
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87055617"
---
# <a name="create-your-first-function-with-java-and-eclipse"></a>Java ve tutulma ile ilk işlevinizi oluşturma 

Bu makalede, tutulma IDE ve Apache Maven ile [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir işlev projesi oluşturma, test etme ve hata ayıklama ve Azure Işlevlerine dağıtma işlemleri gösterilir. 

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Java ve tutulma ile bir işlevler uygulaması geliştirmek için aşağıdakilerin yüklü olması gerekir:

-  [Java geliştirici seti](https://www.azul.com/downloads/zulu/), sürüm 8.
-  [Apache Maven](https://maven.apache.org), sürüm 3,0 veya üzeri.
-  Java ve Maven desteğiyle [Çakışan Küreler](https://www.eclipse.org/downloads/packages/).
-  [Azure CLI](/cli/azure)

> [!IMPORTANT] 
> Bu hızlı başlangıcın tamamlanabilmesi için JAVA_HOME ortam değişkeni JDK’nin yükleme konumu olarak ayarlanmalıdır.

Azure Işlevleri 'ni çalıştırmaya ve hata ayıklamaya yönelik yerel bir ortam sağlayan [Azure Functions Core Tools, sürüm 2](functions-run-local.md#v2)' yi da yüklemek kesinlikle önerilir. 

## <a name="create-a-functions-project"></a>Işlevler projesi oluşturma

1. Tutulma ' da **Dosya** menüsünü seçin ve ardından **Yeni- &gt; Maven projesi**' ni seçin. 
1. **Yeni Maven proje** iletişim kutusunda varsayılan değerleri kabul edin ve **İleri ' yi**seçin.
1. [Azure-Functions-arşiv ETYPE](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype) bulun ve seçin ve **İleri**' ye tıklayın.
1. , Ve dahil tüm alanların değerlerini doldurduğunuzdan emin olun `resourceGroup` `appName` `appRegion` (lütfen **fabrikam-Function-20170920120101928**dışında farklı bir AppName kullanın) ve sonunda **son**' u kullanın.
    ![Tutulma Maven Create2](media/functions-create-first-java-eclipse/functions-create-eclipse2.png)  

Maven, _artifactId_ adlı yeni bir dosyada proje dosyalarını oluşturur. Projedeki oluşturulan kod, tetikleme HTTP isteğinin gövdesini yansıtan basit bir [http ile tetiklenen](./functions-bindings-http-webhook.md) bir işlevdir.

## <a name="run-functions-locally-in-the-ide"></a>İşlevleri IDE 'de yerel olarak çalıştırma

> [!NOTE]
> Azure Functions Core Tools, yerel olarak işlevleri çalıştırmak ve hatalarını ayıklamak için [sürüm 2 '](functions-run-local.md#v2) nin yüklü olması gerekir.

1. Oluşturulan projeye sağ tıklayın ve ardından **Farklı Çalıştır** ve **Maven Build**öğesini seçin.
1. **Yapılandırma Düzenle** Iletişim kutusunda `package` **hedefler** ve **ad** alanlarını girip **Çalıştır**' ı seçin. Bu işlem işlev kodunu derleyip paketleyebilir.
1. Oluşturma işlemi tamamlandıktan sonra, `azure-functions:run` hedef ve ad olarak kullanarak yukarıdaki gibi başka bir çalıştırma yapılandırması oluşturun. İşlevi IDE 'de çalıştırmak için **Çalıştır** ' ı seçin.

İşlevinizi test etmeyi bitirdiğinizde konsol penceresindeki çalışma zamanını sonlandırın. Tek seferde yalnızca bir işlev Konağı etkin ve yerel olarak çalıştırılabilir.

### <a name="debug-the-function-in-eclipse"></a>Çakışan Küreler işlevinde hata ayıklama

Önceki adımda bulunan **Farklı Çalıştır** yapılandırmanızda, `azure-functions:run` `azure-functions:run -DenableDebug` işlev uygulamasını hata ayıklama modunda başlatmak için olarak değiştirin ve güncelleştirilmiş yapılandırmayı çalıştırın.

**Çalıştır** menüsünü seçin ve **hata ayıklama yapılandırması**' nı açın. **Uzak Java uygulaması** ' nı seçin ve yeni bir tane oluşturun. Yapılandırmanıza bir ad verin ve ayarları girin. Bağlantı noktası, varsayılan olarak olan işlev Konağı tarafından açılan hata ayıklama bağlantı noktasıyla tutarlı olmalıdır `5005` . Kurulumdan sonra, `Debug` hata ayıklamayı başlatmak için Aç ' a tıklayın.

![Tutulma 'daki hata ayıklama işlevleri](media/functions-create-first-java-eclipse/debug-configuration-eclipse.PNG)

IDE 'yi kullanarak kesme noktaları ayarlayın ve işlevinizdeki nesneleri inceleyin. İşiniz bittiğinde, hata ayıklayıcıyı ve çalışan işlev konağını durdurun. Tek seferde yalnızca bir işlev Konağı etkin ve yerel olarak çalıştırılabilir.

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

Azure İşlevleri’ne dağıtım işlemi, Azure CLI’dan hesap kimlik bilgilerini kullanır. Bilgisayarınızın komut istemi ile devam etmeden önce [Azure CLI Ile oturum açın](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) .

```azurecli
az login
```

`azure-functions:deploy`Yeni bir **Farklı Çalıştır** yapılandırmasında Maven hedefini kullanarak kodunuzu yeni bir işlev uygulamasına dağıtın.

Dağıtım tamamlandığında, Azure işlev uygulamanıza erişmek için kullanabileceğiniz URL’yi görürsünüz:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

## <a name="next-steps"></a>Sonraki adımlar

- Java işlevleri geliştirme hakkında daha fazla bilgi edinmek için [Java İşlevleri geliştirici kılavuzunu](functions-reference-java.md) gözden geçirin.
- `azure-functions:add` Maven hedefini kullanarak projenize farklı tetikleyicilere sahip ek işlevler ekleyin.

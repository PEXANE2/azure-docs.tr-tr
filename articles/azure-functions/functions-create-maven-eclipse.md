---
title: Java ve Eclipse ile bir Azure işlev uygulaması oluşturun
description: Java ve Eclipse'i Kullanarak Azure İşlevlerini kullanarak basit bir HTTP tetiklenen sunucusuz uygulama oluşturmak ve yayınlamak için nasıl kullanılır kılavuzu.
author: jeffhollan
ms.topic: conceptual
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: c82432a3d76a84eba1ad921d936b2f3ba064e2ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79136842"
---
# <a name="create-your-first-function-with-java-and-eclipse"></a>Java ve Eclipse ile ilk işlevinizi oluşturun 

Bu makalede, Eclipse IDE ve Apache Maven ile [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir işlev projesi oluşturma, test etme ve hata ayıklama ve ardından Azure İşlevler'e nasıl dağıtılacaştırış gösterilmektedir. 

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Java ve Eclipse ile bir işlev uygulaması geliştirmek için aşağıdakileri yüklemiş olmalısınız:

-  [Java Geliştirici Kiti](https://www.azul.com/downloads/zulu/), sürüm 8.
-  [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üzeri.
-  [Eclipse](https://www.eclipse.org/downloads/packages/), Java ve Maven desteği ile.
-  [Azure CLI](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> Bu hızlı başlangıcın tamamlanabilmesi için JAVA_HOME ortam değişkeni JDK’nin yükleme konumu olarak ayarlanmalıdır.

Azure İşlevlerini çalıştırmak ve hata ayıklamak için yerel bir ortam sağlayan [Azure İşlevler Temel Araçları sürüm 2'yi](functions-run-local.md#v2)de yüklemeniz önerilir. 

## <a name="create-a-functions-project"></a>İşlevler projesi oluşturma

1. Eclipse'de **Dosya** menüsünü seçin ve **ardından Yeni -&gt; Maven Project'i**seçin. 
1. **Yeni Maven Project** diyaloğundaki varsayılanları kabul edin ve **İleri'yi**seçin.
1. **Archetype Ekle'yi** seçin ve [azure-functions-archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)için girişleri ekleyin.
    - Archetype Grup Kimliği: com.microsoft.azure
    - Archetype Artifakı Kimliği: azure-functions-archetype
    - Sürüm: [Merkezi depo](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![Eclipse Maven oluşturmak en son sürümünü kontrol edin ve kullanın](media/functions-create-first-java-eclipse/functions-create-eclipse.png)  
1. **Tamam'ı** tıklatın ve sonra **İleri'yi**tıklatın.  `resourceGroup`Dahil olmak üzere `appName`tüm alanlar için değerleri doldurduğunuzdan emin olun , ve `appRegion` (lütfen **fabrikam-function-20170920120101928**dışında farklı bir uygulama Adı kullanın ) ve sonunda **Bitirin**.
    ![Eclipse Maven oluşturmak2](media/functions-create-first-java-eclipse/functions-create-eclipse2.png)  

Maven, _artifactId_ adlı yeni bir dosyada proje dosyalarını oluşturur. Projede oluşturulan kod, tetikleyen HTTP isteğinin gövdesini yansıtan basit bir [HTTP tetikleme](/azure/azure-functions/functions-bindings-http-webhook) işlevidir.

## <a name="run-functions-locally-in-the-ide"></a>IDE'de işlevleri yerel olarak çalıştırın

> [!NOTE]
> [Azure İşlevler Temel Araçları, sürüm 2](functions-run-local.md#v2) yerel olarak işlevleri çalıştırmak ve hata ayıklamak için yüklü olmalıdır.

1. Oluşturulan projeye sağ tıklayın, ardından **Çalıştır As** ve **Maven oluşturma'yı**seçin.
1. **Yapılandırmayı Edit** iletişim `package` kutusunda, **Hedefler** ve **Ad** alanlarına girin ve ardından **Çalıştır'ı**seçin. Bu, işlev kodunu oluşturur ve paketler.
1. Yapı tamamlandıktan sonra, hedef ve ad `azure-functions:run` olarak kullanarak yukarıdaki gibi başka bir Çalıştır yapılandırması oluşturun. IDE'deki işlevi çalıştırmak için **Çalıştır'ı** seçin.

İşlevinizi test etmeyi bitirdiğinizde konsol penceresindeçalışma süresini sonlandırın. Aynı anda yalnızca bir işlev ana bilgisayarı etkin olabilir ve yerel olarak çalışabilir.

### <a name="debug-the-function-in-eclipse"></a>Eclipse işlevi hata ayıklama

Önceki adımda ayarlanan Çalıştır Yapılandırması'nda, işlev uygulamasını hata ayıklama modunda başlatmak için güncelleştirilmiş yapılandırmayı değiştirin **Run As** `azure-functions:run` `azure-functions:run -DenableDebug` ve çalıştırın.

**Çalıştır** menüsünü seçin ve **Hata Ayıklama Yapılandırmaları'nı**açın. **Uzak Java Uygulamasını** seçin ve yeni bir uygulama oluşturun. Yapılandırmanıza bir ad verin ve ayarları doldurun. Bağlantı noktası, varsayılan olarak `5005`işlev ana bilgisayarı tarafından açılan hata ayıklama bağlantı noktasıyla tutarlı olmalıdır. Kurulumdan sonra `Debug` hata ayıklamaya başlamak için tıklayın.

![Eclipse'de hata ayıklama işlevleri](media/functions-create-first-java-eclipse/debug-configuration-eclipse.PNG)

Kesme noktaları ayarlayın ve IDE'yi kullanarak işlevinizdeki nesneleri inceleyin. Tamamlandığında, hata ayıklama ve çalışan işlev ana bilgisayarı durdurun. Aynı anda yalnızca bir işlev ana bilgisayarı etkin olabilir ve yerel olarak çalışabilir.

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

Azure İşlevleri’ne dağıtım işlemi, Azure CLI’dan hesap kimlik bilgilerini kullanır. Bilgisayarınızın komut istemini kullanmaya devam etmeden önce Azure CLI ile [oturum açın.](/cli/azure/authenticate-azure-cli?view=azure-cli-latest)

```azurecli
az login
```

`azure-functions:deploy` Yeni Bir **Çalıştır Yapılandırmasında** Maven hedefini kullanarak kodunuzu yeni bir İşlev uygulamasına dağıtın.

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

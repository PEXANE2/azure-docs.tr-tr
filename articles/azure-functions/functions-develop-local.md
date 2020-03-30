---
title: Azure işlevlerini yerel olarak geliştirme ve çalıştırma
description: Azure İşlevlerinde çalıştırmadan önce Azure işlevlerini yerel bilgisayarınızda nasıl kodlayacağınızı ve test edin.
ms.topic: conceptual
ms.date: 09/04/2018
ms.openlocfilehash: 835edcb94b294d93cab41ea51b88ac38db71d95e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74230641"
---
# <a name="code-and-test-azure-functions-locally"></a>Azure İşlevleri’ni yerel olarak kodlama ve test etme

[Azure portalında]Azure İşlevlerini geliştirip test edebilseniz de, birçok geliştirici yerel bir geliştirme deneyimini tercih eder. İşlevler, yerel bilgisayarınızda işlevler oluşturmak ve test etmek için en sevdiğiniz kod düzenleyicisini ve geliştirme araçlarını kullanmayı kolaylaştırır. Yerel işlevleriniz canlı Azure hizmetlerine bağlanabilir ve tam İşlevler çalışma süresini kullanarak bunları yerel bilgisayarınızda hata ayıklayabilirsiniz.

## <a name="local-development-environments"></a>Yerel gelişim ortamları

Yerel bilgisayarınızda işlev geliştirme şekliniz [dilve](supported-languages.md) araç tercihlerinize bağlıdır. Aşağıdaki tablodaki ortamlar yerel gelişimi destekler:

|Ortam                              |Diller         |Açıklama|
|-----------------------------------------|------------|---|
|[Visual Studio Code](functions-develop-vs-code.md)| [C# (sınıf kitaplığı)](functions-dotnet-class-library.md), [C# komut dosyası (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md), [PowerShell](functions-create-first-function-powershell.md), [Python](functions-reference-python.md) | [VS Kodu için Azure İşlevler uzantısı,](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) VS Kodu'na Fonksiyon desteği ekler. Çekirdek Araçları gerektirir. Çekirdek Araçları'nın 2.x sürümünü kullanırken Linux, MacOS ve Windows'da geliştirmeyi destekler. Daha fazla bilgi için visual [studio kodunu kullanarak ilk işlevinizi oluşturun'e](functions-create-first-function-vs-code.md)bakın. |
| [Komut istemi veya terminal](functions-run-local.md) | [C# (sınıf kitaplığı)](functions-dotnet-class-library.md), [C# komut dosyası (.csx)](functions-reference-csharp.md), [JavaScript](functions-reference-node.md), [PowerShell](functions-reference-powershell.md), [Python](functions-reference-python.md) | [Azure İşlevler Çekirdek Araçları,] yerel geliştirmeyi sağlayan işlevler oluşturmak için temel çalışma süresini ve şablonları sağlar. Sürüm 2.x Linux, MacOS ve Windows'daki geliştirmeyi destekler. Tüm ortamlar, yerel Işlevler için Çekirdek Araçları'na güvenir. |
| [Visual Studio 2019](functions-develop-vs.md) | [C# (sınıf kitaplığı)](functions-dotnet-class-library.md) | Azure İşlevleri araçları Visual [Studio 2019](https://www.visualstudio.com/vs/) ve sonraki sürümlerinin **Azure geliştirme** iş yüküne dahildir. Sınıf kitaplığında işlevleri derlemenize ve .dll'yi Azure'da yayımlamanızı sağlar. Yerel sınama için Çekirdek Araçları içerir. Daha fazla bilgi için bkz: [Visual Studio'u kullanarak Azure İşlevlerini Geliştirin.](functions-develop-vs.md) |
| [Maven](functions-create-first-java-maven.md) (çeşitli) | [Java](functions-reference-java.md) | Java işlevlerinin geliştirilmesini sağlamak için Core Tools ile tümleştirir. Sürüm 2.x Linux, MacOS ve Windows'daki geliştirmeyi destekler. Daha fazla bilgi için java [ve Maven ile ilk işlevinizi oluşturun'e](functions-create-first-java-maven.md)bakın. Ayrıca [Eclipse](functions-create-maven-eclipse.md) ve [IntelliJ IDEA](functions-create-maven-intellij.md) kullanarak geliştirme destekler |

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Bu yerel geliştirme ortamlarının her biri, işlev uygulaması projeleri oluşturmanıza ve yeni işlevler oluşturmak için önceden tanımlanmış İşlev şablonları kullanmanıza olanak tanır. Her biri, diğer tüm uygulamada olduğu gibi kendi makinenizdeki gerçek Fonksiyonlar çalışma süresine karşı işlevlerinizi sınamak ve hata ayıklamak için Temel Araçları kullanır. İşlev uygulama projenizi bu ortamlardan herhangi birinden Azure'a da yayınlayabilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar

+ Visual Studio 2019'u kullanarak derlenmiş C# işlevlerinin yerel gelişimi hakkında daha fazla bilgi edinmek için [Visual Studio'yı kullanarak Azure İşlevlerini Geliştir'e](functions-develop-vs.md)bakın.
+ Mac, Linux veya Windows bilgisayarda VS Kodu kullanarak işlevlerin yerel gelişimi hakkında daha fazla bilgi edinmek için, [Bkz. VS Kodu'ndan Azure İşlerini Dağıt.](/azure/javascript/tutorial-vscode-serverless-node-01)
+ Komut istemi veya terminalden işlev geliştirme hakkında daha fazla bilgi edinmek için Azure [İşi Temel Araçlarıyla Çalışma](functions-run-local.md)'ya bakın.

<!-- LINKS -->

[Azure İşlevler Temel Araçları]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure portalında]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows

---
title: Azure İşlevleri’ni kullanmaya başlama
description: Azure Işlevleri ile çalışmaya yönelik ilk adımları uygulayın.
author: craigshoemaker
ms.topic: overview
ms.date: 11/19/2020
ms.author: cshoe
zone_pivot_groups: programming-languages-set-functions-lang-workers
ms.openlocfilehash: 6aba58dad9853714bf26442592f74fc77e39765c
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94975030"
---
# <a name="getting-started-with-azure-functions"></a>Azure İşlevleri’ni kullanmaya başlama

## <a name="introduction"></a>Giriş

[Azure işlevleri](./functions-overview.md) , sisteminizin mantığını kolayca kullanılabilir kod bloklarına uygulamanıza olanak tanır. Bu kod blokları "işlevler" olarak adlandırılır.

Başlamak için aşağıdaki kaynakları kullanın.

::: zone pivot="programming-language-csharp"

| Eylem | Kaynaklar |
| --- | --- |
| **İlk uygulamanızı oluşturma** | Aşağıdaki araçlardan birini kullanarak:<br><br><li>[Visual Studio Code](./functions-create-your-first-function-visual-studio.md)<li>[Visual Studio](./create-first-function-vs-code-csharp.md)<li>[Komut satırı](./create-first-function-cli-csharp.md) |
| **Çalışan bir işlev görüntüle** | <li>[Azure örnekleri tarayıcısı](https://docs.microsoft.com/samples/browse/?languages=csharp&expanded=azure&products=azure-functions)<li>[Azure Community kitaplığı](https://www.serverlesslibrary.net/?technology=Functions%202.x&language=C%23) |
| **Etkileşimli öğreticiyi keşfet**| <li>[İş senaryonuz için en uygun Azure sunucusuz teknolojisini seçme](https://docs.microsoft.com/learn/modules/serverless-fundamentals/)<li>[İyi tasarlanmış çerçeve-performans verimliliği](https://docs.microsoft.com/learn/modules/azure-well-architected-performance-efficiency/)<li>[Tetikleyicilerle bir Azure işlevi yürütme](https://docs.microsoft.com/learn/modules/execute-azure-function-with-triggers/) <br><br>[Etkileşimli öğreticilerin tam listesi](https://docs.microsoft.com/learn/browse/?expanded=azure&products=azure-functions)için bkz. Microsoft Learn.|
| **Daha ayrıntılı bilgi edinin** | <li>İşlevlerin talebe uyacak şekilde örnekleri [otomatik olarak nasıl artıracağınızı veya azalmasını](./functions-scale.md) öğrenin<li>Kullanılabilir farklı [dağıtım yöntemlerini](./functions-deployment-technologies.md) keşfet<li>İşlevlerinizi çözümlemeye yardımcı olması için yerleşik [izleme araçlarını](./functions-monitoring.md) kullanın<li>[C# dil başvurusunu](./functions-dotnet-class-library.md) okuyun|

::: zone-end

::: zone pivot="programming-language-java"
| Eylem | Kaynaklar |
| --- | --- |
| **İlk uygulamanızı oluşturma** | Aşağıdaki araçlardan birini kullanarak:<br><br><li>[Visual Studio Code](./create-first-function-vs-code-java.md)<li>[Terminal/komut istemiyle Java/Maven işlevi](./create-first-function-cli-java.md)<li>[Gradle](./functions-create-first-java-gradle.md)<li>[Eclipse](./functions-create-maven-eclipse.md)<li>[IntelliJ fıkrı](./functions-create-maven-intellij.md) |
| **Çalışan bir işlev görüntüle** | <li>[Azure örnekleri tarayıcısı](https://docs.microsoft.com/samples/browse/?expanded=azure&products=azure-functions&languages=java)<li>[Azure Community kitaplığı](https://www.serverlesslibrary.net/?technology=Functions%202.x&language=Java) |
| **Etkileşimli öğreticiyi keşfet**| <li>[İş senaryonuz için en uygun Azure sunucusuz teknolojisini seçme](https://docs.microsoft.com/learn/modules/serverless-fundamentals/)<li>[İyi tasarlanmış çerçeve-performans verimliliği](https://docs.microsoft.com/learn/modules/azure-well-architected-performance-efficiency/)<li>[Azure İşlevleri için Maven Eklentisini kullanarak Uygulama geliştirme](https://docs.microsoft.com/learn/modules/develop-azure-functions-app-with-maven-plugin/) <br><br>[Etkileşimli öğreticilerin tam listesi](https://docs.microsoft.com/learn/browse/?expanded=azure&products=azure-functions)için bkz. Microsoft Learn.|
| **Daha ayrıntılı bilgi edinin** | <li>İşlevlerin talebe uyacak şekilde örnekleri [otomatik olarak nasıl artıracağınızı veya azalmasını](./functions-scale.md) öğrenin<li>Kullanılabilir farklı [dağıtım yöntemlerini](./functions-deployment-technologies.md) keşfet<li>İşlevlerinizi çözümlemeye yardımcı olması için yerleşik [izleme araçlarını](./functions-monitoring.md) kullanın<li>[Java dil başvurusunu](./functions-reference-java.md) okuyun|
::: zone-end

::: zone pivot="programming-language-javascript"
| Eylem | Kaynaklar |
| --- | --- |
| **İlk uygulamanızı oluşturma** | Aşağıdaki araçlardan birini kullanarak:<br><br><li>[Visual Studio Code](./create-first-function-vs-code-node.md)<li>[ Terminal/komut istemiNode.js](./create-first-function-cli-java.md) |
| **Çalışan bir işlev görüntüle** | <li>[Azure örnekleri tarayıcısı](https://docs.microsoft.com/samples/browse/?expanded=azure&products=azure-functions&languages=javascript%2Ctypescript)<li>[Azure Community kitaplığı](https://www.serverlesslibrary.net/?technology=Functions%202.x&language=JavaScript%2CTypeScript) |
| **Etkileşimli öğreticiyi keşfet** | <li>[İş senaryonuz için en uygun Azure sunucusuz teknolojisini seçme](https://docs.microsoft.com/learn/modules/serverless-fundamentals/)<li>[İyi tasarlanmış çerçeve-performans verimliliği](https://docs.microsoft.com/learn/modules/azure-well-architected-performance-efficiency/)<li>[Azure İşlevleri ile Sunucusuz API’ler oluşturma](https://docs.microsoft.com/learn/modules/build-api-azure-functions/)<li>[Azure İşlevleri ile sunucusuz mantık oluşturma](https://docs.microsoft.com/learn/modules/create-serverless-logic-with-azure-functions/)<li>[Azure İşlevleri ile Node.js ve Express API’lerini Sunucusuz API’lere Yeniden Düzenleme](https://docs.microsoft.com/learn/modules/shift-nodejs-express-apis-serverless/) <br><br>[Etkileşimli öğreticilerin tam listesi](https://docs.microsoft.com/learn/browse/?expanded=azure&products=azure-functions)için bkz. Microsoft Learn.|
| **Daha ayrıntılı bilgi edinin** | <li>İşlevlerin talebe uyacak şekilde örnekleri [otomatik olarak nasıl artıracağınızı veya azalmasını](./functions-scale.md) öğrenin<li>Kullanılabilir farklı [dağıtım yöntemlerini](./functions-deployment-technologies.md) keşfet<li>İşlevlerinizi çözümlemeye yardımcı olması için yerleşik [izleme araçlarını](./functions-monitoring.md) kullanın<li>[JavaScript](./functions-reference-node.md) veya [TypeScript](./functions-reference-node.md#typescript) dil başvurusunu okuyun|
::: zone-end

::: zone pivot="programming-language-powershell"
| Eylem | Kaynaklar |
| --- | --- |
| **İlk uygulamanızı oluşturma** | <li>[Visual Studio Code](./create-first-function-vs-code-powershell.md) kullanma |
| **Çalışan bir işlev görüntüle** | <li>[Azure örnekleri tarayıcısı](https://docs.microsoft.com/samples/browse/?expanded=azure&products=azure-functions&languages=powershell)<li>[Azure Community kitaplığı](https://www.serverlesslibrary.net/?technology=Functions%202.x&language=PowerShell) |
| **Etkileşimli öğreticiyi keşfet** | <li>[İş senaryonuz için en uygun Azure sunucusuz teknolojisini seçme](https://docs.microsoft.com/learn/modules/serverless-fundamentals/)<li>[İyi tasarlanmış çerçeve-performans verimliliği](https://docs.microsoft.com/learn/modules/azure-well-architected-performance-efficiency/)<li>[Azure İşlevleri ile Sunucusuz API’ler oluşturma](https://docs.microsoft.com/learn/modules/build-api-azure-functions/)<li>[Azure İşlevleri ile sunucusuz mantık oluşturma](https://docs.microsoft.com/learn/modules/create-serverless-logic-with-azure-functions/)<li>[Tetikleyicilerle bir Azure işlevi yürütme](https://docs.microsoft.com/learn/modules/execute-azure-function-with-triggers/) <br><br>[Etkileşimli öğreticilerin tam listesi](https://docs.microsoft.com/learn/browse/?expanded=azure&products=azure-functions)için bkz. Microsoft Learn.|
| **Daha ayrıntılı bilgi edinin** | <li>İşlevlerin talebe uyacak şekilde örnekleri [otomatik olarak nasıl artıracağınızı veya azalmasını](./functions-scale.md) öğrenin<li>Kullanılabilir farklı [dağıtım yöntemlerini](./functions-deployment-technologies.md) keşfet<li>İşlevlerinizi çözümlemeye yardımcı olması için yerleşik [izleme araçlarını](./functions-monitoring.md) kullanın<li>[PowerShell dil başvurusunu](./functions-reference-powershell.md)okuyun)|
::: zone-end

::: zone pivot="programming-language-python"
| Eylem | Kaynaklar |
| --- | --- |
| **İlk uygulamanızı oluşturma** | Aşağıdaki araçlardan birini kullanarak:<br><br><li>[Visual Studio Code](./functions-create-first-function-vs-code.md?pivots=programming-language-python)<li>[Terminal/komut istemi](./functions-create-first-azure-function-azure-cli.md?pivots=programming-language-python) |
| **Çalışan bir işlev görüntüle** | <li>[Azure örnekleri tarayıcısı](https://docs.microsoft.com/samples/browse/?expanded=azure&products=azure-functions&languages=python)<li>[Azure Community kitaplığı](https://www.serverlesslibrary.net/?technology=Functions%202.x&language=Python) |
| **Etkileşimli öğreticiyi keşfet** | <li>[İş senaryonuz için en uygun Azure sunucusuz teknolojisini seçme](https://docs.microsoft.com/learn/modules/serverless-fundamentals/)<li>[İyi tasarlanmış çerçeve-performans verimliliği](https://docs.microsoft.com/learn/modules/azure-well-architected-performance-efficiency/)<li>[Azure İşlevleri ile Sunucusuz API’ler oluşturma](https://docs.microsoft.com/learn/modules/build-api-azure-functions/)<li>[Azure İşlevleri ile sunucusuz mantık oluşturma](https://docs.microsoft.com/learn/modules/create-serverless-logic-with-azure-functions/) <br><br>[Etkileşimli öğreticilerin tam listesi](https://docs.microsoft.com/learn/browse/?expanded=azure&products=azure-functions)için bkz. Microsoft Learn.|
| **Daha ayrıntılı bilgi edinin** | <li>İşlevlerin talebe uyacak şekilde örnekleri [otomatik olarak nasıl artıracağınızı veya azalmasını](./functions-scale.md) öğrenin<li>Kullanılabilir farklı [dağıtım yöntemlerini](./functions-deployment-technologies.md) keşfet<li>İşlevlerinizi çözümlemeye yardımcı olması için yerleşik [izleme araçlarını](./functions-monitoring.md) kullanın<li>[Python dil başvurusunu](./functions-reference-python.md) okuyun|
::: zone-end

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Işlevleri uygulamasının anatomumu hakkında bilgi edinin](./functions-reference.md)

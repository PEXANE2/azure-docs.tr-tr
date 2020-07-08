---
title: Azure Stream Analytics 'da Kullanıcı tanımlı işlevlerde hata ayıkla
description: Bu makalede Azure Stream Analytics içindeki kullanıcı tanımlı işlevlerin nasıl ayıklanacağı açıklanır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/24/2020
ms.openlocfilehash: a6c2e390af39b496a871ae9b5799288ea3609bca
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85981513"
---
# <a name="debug-user-defined-functions-in-azure-stream-analytics"></a>Azure Stream Analytics 'da Kullanıcı tanımlı işlevlerde hata ayıkla 

Kullanıcı tanımlı işlevler (UDF), beklendiği gibi çalışmazsa, sorunu bulmak için hata ayıklaması yapmanız gerekir. [Visual Studio Code](visual-studio-code-local-run-live-input.md) veya [Visual Studio](stream-analytics-vs-tools-local-run.md)kullanarak işlerinizi yerel olarak çalıştırdığınızda Stream Analytics işleriniz için UDF 'leri hata ayıklaması yapabilirsiniz.

İşinizi yerel olarak canlı bir giriş akışına çalıştırırken, yalnızca bir düğümdeki bulut Azure Stream Analytics altyapısının yürütülmesine taklit eder. Canlı veri yerel testi, bulutta yaptığınız performans ve ölçeklenebilirlik testini değiştirmez, ancak test etmek istediğiniz her seferinde işinizi buluta göndermesi gerekmeden işlevsel test sırasında zamandan tasarruf edersiniz. Ayrıca, yerel veya örnek verilerle yerel çalıştırma için zaman ilkeleri devre dışıdır, ancak canlı veri testi için zaman ilkeleri desteklenir.

## <a name="pick-your-language"></a>Dilinizi seçin

.NET (C#) veya JavaScript kullanarak Azure Stream Analytics için UDF 'ler yazabilirsiniz. 

### <a name="functions-in-c"></a>C içindeki işlevler # 

[Visual Studio kullanarak .net UDF 'leri yazdığınızda](stream-analytics-edge-csharp-udf-methods.md), her türlü .NET sınıfı projesinde yaptığınız gibi aynı destek düzeyine sahip olursunuz. Bu destek şunları içerir:

* Sözdizimi denetimi ve derleyici desteği gibi derleme desteği.

* Stream Analytics çözümünüzde bir C# projesi ve yapıtları ekleme, derleme ve başvuru yapma özelliği. 

* Paylaşılabilir bir projede kapsüllenmiş kodun kolay yeniden kullanılması. 

* Visual Studio 'da doğrudan hata ayıklama desteği. Stream Analytics projeyi başlangıç projesi olarak ayarlayın ve C# kodunda kesme noktaları ayarlayın. Daha sonra, C# kodunda hata ayıklamak için **F5** tuşuna basın, diğer c# projeleri için yaptığınız gibi. 

### <a name="functions-in-javascript"></a>JavaScript 'teki işlevler

JavaScript, Stream Analytics işlevleri oluşturmaya yönelik başka bir seçenektir. JavaScript kodu doğrudan Stream Analytics projesinin işlev alanına yerleştirilir ve bu, projeler arasında paylaşımı daha zor hale getirir.

Derleme, Stream Analytics Projesi derlendiğinde veya yürütüldüğünde gerçekleşir. Yalnızca çalışma zamanında bir sorun bulma şansı daha yüksektir. Stream Analytics doğrudan JavaScript işlevleri için hata ayıklama desteği yoktur.

## <a name="debug-options-for-javascript"></a>JavaScript için hata ayıklama seçenekleri

Stream Analytics doğrudan JavaScript işlevleri için hata ayıklama desteği olmadığından, işlevi bir HTML sitesinde kapsülleyerek ve çıktıyı buradan alarak hata ayıklaması yapabilirsiniz.

Aşağıdaki örnek, [Visual Studio Code](quick-create-vs-code.md)' deki tümleşik çalışma zamanı ortamında bazı sınırlamalara sahip JavaScript UDF 'leri hata ayıklamanın nasıl yapılacağını gösterir.

### <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, Azure Stream Analytics projenizin aşağıdaki öğelere sahip olduğundan emin olun:

* Bir giriş 
* Bir çıkış 
* Bir Stream Analytics sorgusu (. aşama QL) 
* Stream Analytics iş yapılandırması (JobConfig.jsaçık)
* JavaScript UDF

### <a name="prepare-files"></a>Dosyaları hazırlama

Aşağıdaki görüntüde, *. aşama QL* sorgu dosyası yalnızca UDF çağrısı olan *fxcharcount*' u içerir. Bu değişiklik, değişiklikleri yapıldıktan sonra projeyi derleyebilmenizi sağlar.

> [!div class="mx-imgBorder"]
> ![Test sorgu dosyası Stream Analytics](./media/debug-user-defined-functions/asaql-file.png)

Testi JavaScript işleviyle yürütmek için çağrılan test dosyasını barındırmak için **testlerde** ek bir klasör oluşturun. Bu örnekte, klasörün adı *Fxcharcount* , testin adı ise *Test_UDF.js*. 

Aşağıdaki görüntüde, işlev dosyasını yükleyen ve işlevi yürüten test dosyasındaki kod gösterilmektedir. Bu örnek basittir, ancak daha fazla test verisi dosyası yükleyebilir ve daha fazla testi kullanarak çıktıyı alabilir. İşlev çağrısının gösterimi ortak çağrılardan biraz farklıdır, çünkü dosyaya başvuruluyor ve çalışma zamanında yüklenmez ve hata ayıklaması mümkündür. 

> [!div class="mx-imgBorder"]
> ![Test dosyası Stream Analytics](./media/debug-user-defined-functions/test-file.png)

İşlevinde, yöntemleri göstermek için aşağıdaki kod satırlarını dosyaya ekleyin. Visual Studio Code kodu derleme özelliğini etkilemez.

```javascript
var methods = {};
methods.fxchartCount = fxchartCount;
exports.data = methods;
``` 

> [!div class="mx-imgBorder"]
> ![JavaScript UDF Stream Analytics](./media/debug-user-defined-functions/udf-file.png)
  
## <a name="install-debug-support"></a>Hata ayıklama desteğini yükler

Hata ayıklamak için **node.js** [indirmeniz](https://nodejs.org/en/download/) ve kurmanız gerekir. Kullanmakta olduğunuz platforma göre doğru sürümü yükleyebilirsiniz. node.js çalışma zamanını yükledikten sonra, değişiklikleri uygulamak için Visual Studio Code yeniden başlatın. 

Hata ayıklamayı başlatmak için **Çalıştır ve hata ayıkla '** yı veya **CTRL + SHIFT + D** tuşlarına basın. Çalışma zamanı olarak **node.js** seçebileceğiniz bir Birleşik giriş kutusu açılır. Yalnızca node.js yüklüyse, varsayılan olarak kullanılır. F11 ile gerekirse, kodda ve uydu dosyasında adım adım ilerlemenbilmelisiniz. 

> [!div class="mx-imgBorder"]
> ![Stream Analytics Çalıştır ve hata ayıkla UDF](./media/debug-user-defined-functions/run-debug-udf.png)

### <a name="debug-user-defined-aggregates"></a>Kullanıcı tanımlı toplamalarda hata ayıkla 

Kullanıcı tanımlı toplamaların (UDA) hatalarını ayıklamak için JavaScript UDF 'Leri için hata ayıklama yöntemini kullanabilirsiniz. Bu örnekte, *. aşama QL* sorgu dosyasına ve test DOSYASıNA bir uda eklenir.

UDF 'de olduğu gibi, projenin değişiklikler yapıldıktan sonra derlendiğinden emin olmak için UDA 'ya bir çağrı dahil edersiniz. 

> [!div class="mx-imgBorder"]
> ![Asaql 'e uda ekleyin](./media/debug-user-defined-functions/asaql-uda.png)

*Test_UDA.js* dosyasında, UDF ile YAPTıĞıNıZ gibi uda dosyasına başvurdunuz. Ayrıca,, ve ' i çağırır `main()` `init()` `accumulate()` . `accumulate()`Yöntemi, değerleri durum yığınına yerleştirmek için bir döngüde çağrılır. `computeresult()`Yöntemi, son sorguyu oluşturmak için çağrılır. 

> [!div class="mx-imgBorder"]
> ![UDA test dosyası](./media/debug-user-defined-functions/uda-test.png)

UDF örneğinde olduğu gibi, ilgili yöntemleri göstermek için bazı kodların UDA 'ya eklenmesi gerekir.

```javascript
var methods = {};
methods.main = main;
methods.init = main.init;
methods.accumulate = main.accumulate;
methods.computeResult = main.computeResult;
exports.data = methods;
``` 

> [!div class="mx-imgBorder"]
> ![Kod UDA 'ya eklendi](./media/debug-user-defined-functions/uda-expose-methods.png)

Hata ayıklamayı başlatmak için **Çalıştır ve hata ayıkla '** yı veya **CTRL + SHIFT + D** tuşlarına basın. Çalışma zamanı olarak **node.js** seçebileceğiniz bir Birleşik giriş kutusu açılır. Yalnızca node.js yüklüyse, varsayılan olarak kullanılır. F11 ile gerekirse, kodda ve uydu dosyasında adım adım ilerlemenbilmelisiniz.

> [!div class="mx-imgBorder"]
> ![Stream Analytics Çalıştır ve hata ayıkla uda](./media/debug-user-defined-functions/run-debug-uda.png)


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics işleri yerel olarak geliştirme ve hata ayıklama](develop-locally.md)
* [Visual Studio 'da iş diyagramını kullanarak Azure Stream Analytics sorguları yerel olarak ayıklayın](debug-locally-using-job-diagram.md)
* [Azure Stream Analytics 'de Kullanıcı tanımlı işlevler](functions-overview.md)
 

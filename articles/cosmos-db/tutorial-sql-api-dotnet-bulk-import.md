---
title: .NET SDK kullanarak verileri Azure Cosmos DB SQL API hesabına toplu olarak içeri aktarma
description: Verileri içeri aktarmak için gereken üretilen iş üretimini (RU/s) optimize eden bir .NET konsol uygulaması oluşturarak Azure Cosmos DB verileri içeri aktarmayı veya almayı öğrenin
author: ealsur
ms.author: maquaran
ms.service: cosmos-db
ms.topic: tutorial
ms.date: 11/04/2019
ms.reviewer: sngun
ms.openlocfilehash: 79771e082a4a6ffae15f33f636b0300e93bcdaba
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "77587575"
---
# <a name="bulk-import-data-to-azure-cosmos-db-sql-api-account-by-using-the-net-sdk"></a>.NET SDK kullanarak verileri Azure Cosmos DB SQL API hesabına toplu olarak içeri aktarma

Bu öğreticide, verileri Azure Cosmos DB aktarmak için gereken üretilen iş üretimini (RU/s) optimize eden bir .NET konsol uygulamasının nasıl oluşturulacağı gösterilmektedir. Bu makalede, bir örnek veri kaynağından verileri okuyacaksınız ve Azure Cosmos kapsayıcısına aktarırsınız.
Bu öğretici, .NET Framework veya .NET Core 'a hedeflenmiş Azure Cosmos DB .NET SDK 'sının [3.0 + sürümünü](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) kullanır.

Bu öğreticinin içindekiler:

> [!div class="checklist"]
> * Azure Cosmos hesabı oluşturma
> * Projenizi yapılandırma
> * Toplu destek etkin olan bir Azure Cosmos hesabına bağlanma
> * Eşzamanlı oluşturma işlemleri aracılığıyla veri içeri aktarma gerçekleştirme

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki yönergeleri izleyerek önce aşağıdaki kaynaklara sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [NET Core 3 SDK](https://dotnet.microsoft.com/download/dotnet-core). Çalıştırıp ortamınızda hangi sürümün kullanılabilir olduğunu doğrulayabilirsiniz `dotnet --version` .

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma

Azure portal [Azure Cosmos DB BIR SQL API hesabı oluşturun](create-cosmosdb-resources-portal.md) veya [Azure Cosmos DB öykünücüsünü](local-emulator.md)kullanarak hesabı oluşturabilirsiniz.

## <a name="step-2-set-up-your-net-project"></a>2. Adım: .NET projenizi ayarlama

Yerel bilgisayarınızdan Windows komut istemi veya bir Terminal penceresi açın. Komut istemi veya terminalden sonraki bölümlerde bulunan tüm komutları çalıştıracaksınız. *Toplu içeri aktarma tanıtımı*adlı yeni bir uygulama oluşturmak için aşağıdaki DotNet New komutunu çalıştırın. `--langVersion`Parametresi, oluşturulan proje dosyasındaki *langversion* özelliğini ayarlar.

   ```bash
   dotnet new console –langVersion:8 -n bulk-import-demo
   ```

Dizininizi yeni oluşturulan uygulama klasörüyle değiştirin. Uygulamayı ile oluşturabilirsiniz:

   ```bash
   cd bulk-import-demo
   dotnet build
   ```

Derlemeden beklenen çıktı şuna benzemelidir:

   ```bash
   Restore completed in 100.37 ms for C:\Users\user1\Downloads\CosmosDB_Samples\bulk-import-demo\bulk-import-demo.csproj.
     bulk -> C:\Users\user1\Downloads\CosmosDB_Samples\bulk-import-demo \bin\Debug\netcoreapp2.2\bulk-import-demo.dll

   Build succeeded.
       0 Warning(s)
       0 Error(s)

   Time Elapsed 00:00:34.17
   ```

## <a name="step-3-add-the-azure-cosmos-db-package"></a>3. Adım: Azure Cosmos DB paketini ekleme

Hala uygulama dizininde, DotNet Add Package komutunu kullanarak .NET Core için Azure Cosmos DB istemci kitaplığını yükleyebilirsiniz.

   ```bash
   dotnet add package Microsoft.Azure.Cosmos
   ```

## <a name="step-4-get-your-azure-cosmos-account-credentials"></a>4. Adım: Azure Cosmos hesabı kimlik bilgilerinizi alın

Örnek uygulamanın Azure Cosmos hesabınızda kimlik doğrulaması yapması gerekir. Kimlik doğrulaması yapmak için Azure Cosmos hesabı kimlik bilgilerini uygulamaya geçirmeniz gerekir. Aşağıdaki adımları izleyerek Azure Cosmos hesabı kimlik bilgilerinizi alın:

1.  [Azure portalında](https://portal.azure.com/) oturum açın.
1.  Azure Cosmos hesabınıza gidin.
1.  **Anahtarlar** bölmesini açın ve hesabınızın **URI** ve **birincil anahtarını** kopyalayın.

Azure Cosmos DB öykünücüsü kullanıyorsanız, [Bu makaledeki öykünücü kimlik bilgilerini](local-emulator.md#authenticating-requests)alın.

## <a name="step-5-initialize-the-cosmosclient-object-with-bulk-execution-support"></a>5. Adım: CosmosClient nesnesini toplu yürütme desteğiyle başlatın

Oluşturulan `Program.cs` dosyayı bir kod düzenleyicisinde açın. Toplu yürütme etkin olan yeni bir CosmosClient örneği oluşturacaksınız ve Azure Cosmos DB karşı işlemleri yapmak için bunu kullanacaksınız. 

Varsayılan metodun üzerine yazarak `Main` ve genel değişkenleri tanımlayarak başlayalım. Bu genel değişkenler uç nokta ve yetkilendirme anahtarlarını, veritabanının adını, oluşturacağınız kapsayıcıyı ve toplu olarak eklediğiniz öğelerin sayısını içerir. EndpointURL ve yetkilendirme anahtarı değerlerini ortamınıza göre değiştirdiğinizden emin olun. 


   ```csharp
   using System;
   using System.Collections.Generic;
   using System.Diagnostics;
   using System.IO;
   using System.Text.Json;
   using System.Threading.Tasks;
   using Microsoft.Azure.Cosmos;

   public class Program
   {
        private const string EndpointUrl = "https://<your-account>.documents.azure.com:443/";
        private const string AuthorizationKey = "<your-account-key>";
        private const string DatabaseName = "bulk-tutorial";
        private const string ContainerName = "items";
        private const int ItemsToInsert = 300000;

        static async Task Main(string[] args)
        {

        }
   }
   ```

Yöntemi içinde `Main` , CosmosClient nesnesini başlatmak için aşağıdaki kodu ekleyin:

[!code-csharp[Main](~/cosmos-dotnet-bulk-import/src/Program.cs?name=CreateClient)]

Toplu yürütme etkinleştirildikten sonra, CosmosClient eş zamanlı işlemleri dahili olarak tek hizmet çağrılarına gruplandırır. Bu sayede, bölümler arasında hizmet çağrıları dağıtarak ve son olarak özgün çağıranlara ayrı sonuçlar atayarak üretilen iş kullanımını iyileştirir.

Daha sonra tüm öğelerimizi depolamak için bir kapsayıcı oluşturabilirsiniz.  `/pk`Bölüm anahtarı olarak, sağlanan verimlilik olarak 50000 ru/sn ve yazma verimini iyileştirmek için tüm alanları hariç tutacak özel bir dizin oluşturma ilkesi olarak tanımlayın. CosmosClient başlatma ifadesinden sonra aşağıdaki kodu ekleyin:

[!code-csharp[Main](~/cosmos-dotnet-bulk-import/src/Program.cs?name=Initialize)]

## <a name="step-6-populate-a-list-of-concurrent-tasks"></a>6. Adım: eşzamanlı görevlerin bir listesini doldurma

Toplu yürütme desteğinden yararlanmak için, verilerin kaynağına ve gerçekleştirmek istediğiniz işlemlere göre zaman uyumsuz görevlerin bir listesini oluşturun ve `Task.WhenAll` bunları eşzamanlı olarak yürütmek için kullanın.
Veri modelinizdeki öğelerin bir listesini oluşturmak için "sahte" verileri kullanarak başlayalım. Gerçek dünyada bir uygulamada, öğeler istenen veri kaynağından gelir.

İlk olarak, DotNet paket Ekle komutunu kullanarak yapay paketi çözüme ekleyin.

   ```bash
   dotnet add package Bogus
   ```

Kaydetmek istediğiniz öğelerin tanımını tanımlayın. `Item`Dosya içinde sınıfı tanımlamanız gerekir `Program.cs` :

[!code-csharp[Main](~/cosmos-dotnet-bulk-import/src/Program.cs?name=Model)]

Sonra, sınıfı içinde bir yardımcı işlevi oluşturun `Program` . Bu yardımcı işlev, eklemek ve rastgele veri oluştururken tanımladığınız öğelerin sayısını alır:

[!code-csharp[Main](~/cosmos-dotnet-bulk-import/src/Program.cs?name=Bogus)]

Öğeleri okuyun ve sınıfını kullanarak akış örneklerine serileştirin `System.Text.Json` . Otomatik olarak oluşturulan verilerin doğası nedeniyle, verileri akışlar olarak serileştirdide olursunuz. Öğe örneğini doğrudan da kullanabilirsiniz, ancak bunları akışlara dönüştürerek CosmosClient içinde Stream API 'lerinin performansından yararlanabilirsiniz. Genellikle, verileri Bölüm anahtarını bildiğiniz sürece doğrudan kullanabilirsiniz. 


Verileri akış örneklerine dönüştürmek için, yöntemi içinde, `Main` kapsayıcıyı oluşturduktan hemen sonra aşağıdaki kodu ekleyin:

[!code-csharp[Main](~/cosmos-dotnet-bulk-import/src/Program.cs?name=Operations)]

Ardından, eşzamanlı görevler oluşturmak için veri akışlarını kullanın ve öğeleri kapsayıcıya eklemek için görev listesini doldurun. Bu işlemi gerçekleştirmek için aşağıdaki kodu `Program` sınıfına ekleyin:

[!code-csharp[Main](~/cosmos-dotnet-bulk-import/src/Program.cs?name=ConcurrentTasks)]

Tüm bu eşzamanlı nokta işlemleri, giriş bölümünde açıklanan şekilde birlikte yürütülür (toplu).

## <a name="step-7-run-the-sample"></a>7. Adım: örneği çalıştırma

Örneği çalıştırmak için, bunu yalnızca `dotnet` komutla yapabilirsiniz:

   ```bash
   dotnet run
   ```

## <a name="get-the-complete-sample"></a>Tüm örneği alın

Bu öğreticideki adımları tamamlamaya yönelik bir zaman yoksa veya yalnızca kod örneklerini indirmek istiyorsanız, [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-bulk-import-throughput-optimizer)' dan edinebilirsiniz.

Projeyi kopyaladıktan sonra, istenen kimlik bilgilerini [program.cs](https://github.com/Azure-Samples/cosmos-dotnet-bulk-import-throughput-optimizer/blob/master/src/Program.cs#L25)içinde güncelleştirdiğinizden emin olun.

Örnek, depo dizinine göre değiştirilerek ve kullanılarak çalıştırılabilir `dotnet` :

   ```bash
   cd cosmos-dotnet-bulk-import-throughput-optimizer
   dotnet run
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki adımları tamamladınız:

> [!div class="checklist"]
> * Azure Cosmos hesabı oluşturma
> * Projenizi yapılandırma
> * Toplu destek etkin olan bir Azure Cosmos hesabına bağlanma
> * Eşzamanlı oluşturma işlemleri aracılığıyla veri içeri aktarma gerçekleştirme

Şimdi de sonraki öğreticiye geçebilirsiniz:

> [!div class="nextstepaction"]
>[SQL API 'sini kullanarak Azure Cosmos DB sorgulama](tutorial-query-sql-api.md)
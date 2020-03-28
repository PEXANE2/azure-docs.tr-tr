---
title: .Net SDK kullanarak Azure Cosmos DB SQL API hesabına toplu veri alma
description: Veri almak için gerekli olan sağlanan iş ortasını (RU/s) optimize eden bir .NET konsol uygulaması oluşturarak Azure Cosmos DB'ye nasıl veri aktarılamayı veya almamayı öğrenin
author: ealsur
ms.author: maquaran
ms.service: cosmos-db
ms.topic: tutorial
ms.date: 11/04/2019
ms.reviewer: sngun
ms.openlocfilehash: 79771e082a4a6ffae15f33f636b0300e93bcdaba
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77587575"
---
# <a name="bulk-import-data-to-azure-cosmos-db-sql-api-account-by-using-the-net-sdk"></a>.NET SDK kullanarak Azure Cosmos DB SQL API hesabına toplu veri alma

Bu öğretici, Azure Cosmos DB'ye veri almak için gereken sağlanan iş ortasını (RU/s) optimize eden bir .NET konsol uygulamasının nasıl oluşturilebildiğini gösterir. Bu makalede, örnek bir veri kaynağından gelen verileri okuyacak ve bir Azure Cosmos kapsayıcısına aktarın.
Bu öğretici, .NET Framework veya .NET Core'a hedeflenebilecek Azure Cosmos DB .NET SDK Sürümü [3.0+'yı](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) kullanır.

Bu öğreticinin içindekiler:

> [!div class="checklist"]
> * Azure Cosmos hesabı oluşturma
> * Projenizi yapılandırma
> * Toplu destek etkinleştirilmiş bir Azure Cosmos hesabına bağlanma
> * Eşzamanlı oluşturma işlemleri yoluyla veri alma gerçekleştirme

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki yönergeleri takip etmeden önce, aşağıdaki kaynaklara sahip olduğundan emin olun:

* Etkin bir Azure hesabı. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [NET Çekirdek 3 SDK](https://dotnet.microsoft.com/download/dotnet-core). Ortamınızda hangi sürümün kullanılabildiği çalıştırılarak `dotnet --version`doğrulayabilirsiniz.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma

Azure portalından [bir Azure Cosmos DB SQL API hesabı oluşturun](create-cosmosdb-resources-portal.md) veya Azure [Cosmos DB Emülatörü'ni](local-emulator.md)kullanarak hesabı oluşturabilirsiniz.

## <a name="step-2-set-up-your-net-project"></a>Adım 2: .NET projenizi ayarlama

Yerel bilgisayarınızdan Windows komut istemini veya Terminal penceresini açın. Komut istemi veya terminalsonraki bölümlerdeki tüm komutları çalıştırın. *Toplu alma-demo*adı ile yeni bir uygulama oluşturmak için aşağıdaki dotnet yeni komutu çalıştırın. `--langVersion` Parametre, oluşturulan proje dosyasındaki *LangVersion* özelliğini ayarlar.

   ```bash
   dotnet new console –langVersion:8 -n bulk-import-demo
   ```

Diziniyeniyeniyeni oluşturulan uygulama klasörüne değiştirin. Uygulamayı aşağıdakilerle oluşturabilirsiniz:

   ```bash
   cd bulk-import-demo
   dotnet build
   ```

Yapıdan beklenen çıktı şuna benzer:

   ```bash
   Restore completed in 100.37 ms for C:\Users\user1\Downloads\CosmosDB_Samples\bulk-import-demo\bulk-import-demo.csproj.
     bulk -> C:\Users\user1\Downloads\CosmosDB_Samples\bulk-import-demo \bin\Debug\netcoreapp2.2\bulk-import-demo.dll

   Build succeeded.
       0 Warning(s)
       0 Error(s)

   Time Elapsed 00:00:34.17
   ```

## <a name="step-3-add-the-azure-cosmos-db-package"></a>Adım 3: Azure Cosmos DB paketini ekleyin

Uygulama dizinindeyken, dotnet ekle paket komutunu kullanarak .NET Core için Azure Cosmos DB istemci kitaplığını yükleyin.

   ```bash
   dotnet add package Microsoft.Azure.Cosmos
   ```

## <a name="step-4-get-your-azure-cosmos-account-credentials"></a>Adım 4: Azure Cosmos hesap kimlik bilgilerinizi alın

Örnek uygulamanın Azure Cosmos hesabınızda kimliğinin doğrulanması gerekir. Kimlik doğrulaması yapmak için Azure Cosmos hesap kimlik bilgilerini uygulamaya geçirmeniz gerekir. Aşağıdaki adımları izleyerek Azure Cosmos hesap kimlik bilgilerinizi alın:

1.  [Azure portalında](https://portal.azure.com/)oturum açın.
1.  Azure Cosmos hesabınıza gidin.
1.  **Keys** bölmesini açın ve hesabınızın **URI** ve **BIRINCIL ANAHTARINI** kopyalayın.

Azure Cosmos DB Emülatörü kullanıyorsanız, [bu makaleden emülatör kimlik bilgilerini](local-emulator.md#authenticating-requests)edinin.

## <a name="step-5-initialize-the-cosmosclient-object-with-bulk-execution-support"></a>Adım 5: CosmosClient nesnesini toplu yürütme desteğiyle başlatma

Oluşturulan `Program.cs` dosyayı bir kod düzenleyicisinde açın. Toplu yürütme etkin leştirilmiş yeni bir CosmosClient örneği oluşturur ve Azure Cosmos DB'ye karşı işlem yapmak için kullanırsınız. 

Varsayılan `Main` yöntemin üzerine yazarak ve genel değişkenleri tanımlayarak başlayalım. Bu genel değişkenler bitiş noktası ve yetkilendirme anahtarlarını, veritabanının adını, oluşturacağınız kapsayıcıyı ve toplu olarak ekeceğiniz öğe sayısını içerir. Ortamınıza göre endpointURL ve yetkilendirme anahtar değerlerini değiştirdiğinizden emin olun. 


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

Yöntemin `Main` içinde, CosmosClient nesnesini başlatmak için aşağıdaki kodu ekleyin:

[!code-csharp[Main](~/cosmos-dotnet-bulk-import/src/Program.cs?name=CreateClient)]

Toplu yürütme etkinleştirildikten sonra, CosmosClient dahili olarak eşzamanlı işlemleri tek hizmet çağrılarına gruplandırır. Bu şekilde, hizmet çağrılarını bölümler arasında dağıtarak ve son olarak özgün arayanlara tek tek sonuçlar atayarak iş sonu kullanımını en iyi duruma sokar.

Daha sonra tüm öğeleri saklamak için bir kapsayıcı oluşturabilirsiniz.  Bölüm `/pk` anahtarı, 50000 RU/s'yi sağlanan iş tükenir olarak ve yazma işakışını optimize etmek için tüm alanları hariç tutan özel bir dizin oluşturma ilkesi olarak tanımlayın. CosmosClient başlatma deyiminden sonra aşağıdaki kodu ekleyin:

[!code-csharp[Main](~/cosmos-dotnet-bulk-import/src/Program.cs?name=Initialize)]

## <a name="step-6-populate-a-list-of-concurrent-tasks"></a>Adım 6: Eşzamanlı görevlerin listesini doldurma

Toplu yürütme desteğinden yararlanmak için, veri kaynağına ve gerçekleştirmek istediğiniz işlemlere dayalı eşzamanlı görevlerin bir `Task.WhenAll` listesini oluşturun ve bunları aynı anda yürütmek için kullanın.
Veri modelimizden öğelerin bir listesini oluşturmak için "Sahte" verileri kullanarak başlayalım. Gerçek bir uygulamada, öğeler istediğiniz veri kaynağından gelir.

İlk olarak, dotnet ekle paket komutunu kullanarak çözüme Sahte paket ekleyin.

   ```bash
   dotnet add package Bogus
   ```

Kaydetmek istediğiniz öğelerin tanımını tanımlayın. Dosya içindeki `Item` sınıfı tanımlamanız `Program.cs` gerekir:

[!code-csharp[Main](~/cosmos-dotnet-bulk-import/src/Program.cs?name=Model)]

Ardından, `Program` sınıf içinde bir yardımcı işlev oluşturun. Bu yardımcı işlevi eklemek için tanımladığınız ve rasgele veri üreten öğe sayısını alır:

[!code-csharp[Main](~/cosmos-dotnet-bulk-import/src/Program.cs?name=Bogus)]

Öğeleri okuyun ve sınıfı kullanarak akış örneklerine `System.Text.Json` serileştirin. Otomatik oluşturulan verilerin doğası nedeniyle, verileri akış olarak seri hale getirmeniz gerekir. Öğe örneğini doğrudan da kullanabilirsiniz, ancak bunları akışlara dönüştürerek CosmosClient'daki akış API'lerinin performansından yararlanabilirsiniz. Genellikle, bölüm anahtarını bildiğiniz sürece verileri doğrudan kullanabilirsiniz. 


Verileri akış örneklerine dönüştürmek için, `Main` yöntem içinde, kapsayıcıyı oluşturduktan hemen sonra aşağıdaki kodu ekleyin:

[!code-csharp[Main](~/cosmos-dotnet-bulk-import/src/Program.cs?name=Operations)]

Daha sonra eşzamanlı görevler oluşturmak ve öğeleri kapsayıcıya eklemek için görev listesini doldurmak için veri akışlarını kullanın. Bu işlemi gerçekleştirmek için `Program` sınıfa aşağıdaki kodu ekleyin:

[!code-csharp[Main](~/cosmos-dotnet-bulk-import/src/Program.cs?name=ConcurrentTasks)]

Tüm bu eşzamanlı nokta işlemleri, giriş bölümünde açıklandığı gibi birlikte (yani toplu olarak) yürütülecektir.

## <a name="step-7-run-the-sample"></a>Adım 7: Örneği çalıştırma

Örneği çalıştırmak için, bunu basitçe komutla `dotnet` yapabilirsiniz:

   ```bash
   dotnet run
   ```

## <a name="get-the-complete-sample"></a>Tam örneği alın

Bu öğreticideki adımları tamamlamak için zamanın yoksa veya sadece kod örneklerini indirmek istiyorsanız, [Bunu GitHub'dan](https://github.com/Azure-Samples/cosmos-dotnet-bulk-import-throughput-optimizer)alabilirsiniz.

Projeyi klonladıktan sonra, [Program.cs](https://github.com/Azure-Samples/cosmos-dotnet-bulk-import-throughput-optimizer/blob/master/src/Program.cs#L25)içinde istenen kimlik bilgilerini güncelleştirildiğinden emin olun.

Örnek depo dizinine değiştirilerek ve aşağıdakiler kullanılarak `dotnet`çalıştırılabilir:

   ```bash
   cd cosmos-dotnet-bulk-import-throughput-optimizer
   dotnet run
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki adımları yaptınız:

> [!div class="checklist"]
> * Azure Cosmos hesabı oluşturma
> * Projenizi yapılandırma
> * Toplu destek etkinleştirilmiş bir Azure Cosmos hesabına bağlanma
> * Eşzamanlı oluşturma işlemleri yoluyla veri alma gerçekleştirme

Şimdi bir sonraki öğretici devam edebilirsiniz:

> [!div class="nextstepaction"]
>[SQL API'sini kullanarak Azure Cosmos DB'yi sorgula](tutorial-query-sql-api.md)
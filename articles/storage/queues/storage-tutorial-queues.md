---
title: Öğretici-.NET 'te Azure depolama kuyrukları ile çalışma
description: Azure Kuyruk hizmeti kullanarak kuyruklar oluşturma, .NET kodu kullanarak ileti ekleme, alma ve silme hakkında bir öğretici.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 06/09/2020
ms.service: storage
ms.subservice: queues
ms.topic: tutorial
ms.reviewer: dineshm
ms.openlocfilehash: 73bc21307ff0648b7e0aab7611e57f6fa60a806b
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "84809571"
---
# <a name="tutorial-work-with-azure-storage-queues-in-net"></a>Öğretici: .NET 'te Azure depolama kuyrukları ile çalışma

Azure kuyruk depolama, dağıtılmış bir uygulamanın bileşenleri arasında iletişimi etkinleştirmek için bulut tabanlı kuyruklar uygular. Her kuyruk, bir gönderen bileşeni tarafından eklenebilen ve bir alıcı bileşeni tarafından işlenen iletilerin bir listesini tutar. Bir kuyruk ile uygulamanız, talebi karşılamak için hemen ölçeklendirebilir. Bu makalede, Azure depolama kuyruğu ile çalışmaya yönelik temel adımlar gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> - Azure depolama hesabı oluşturma
> - Uygulama oluşturma
> - Azure istemci kitaplıklarını ekleme
> - Zaman uyumsuz kod desteği ekle
> - Bir kuyruk oluşturma
> - Bir kuyruğa ileti ekleme
> - İletileri sıradan çıkarma
> - Boş bir kuyruğu silme
> - Komut satırı bağımsız değişkenlerini denetle
> - Uygulamayı derleyin ve çalıştırın

## <a name="prerequisites"></a>Ön koşullar

- Platformlar arası [Visual Studio Code](https://code.visualstudio.com/download) düzenleyicisinin ücretsiz kopyasını alın.
- [.NET Core SDK](https://dotnet.microsoft.com/download) sürüm 3,1 veya üstünü indirin ve yükleyin.
- Geçerli bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-an-azure-storage-account"></a>Azure depolama hesabı oluşturma

İlk olarak, bir Azure depolama hesabı oluşturun. Depolama hesabı oluşturmaya yönelik adım adım kılavuz için bkz. [depolama hesabı oluşturma](../common/storage-account-create.md?toc=%2Fazure%2Fstorage%2Fqueues%2Ftoc.json) hızlı başlangıcı. Bu, önkoşullardan ücretsiz bir Azure hesabı oluşturduktan sonra gerçekleştirdiğiniz ayrı bir adımdır.

## <a name="create-the-app"></a>Uygulama oluşturma

**Queueapp**adlı bir .NET Core uygulaması oluşturun. Kolaylık olması için, bu uygulama sıra aracılığıyla ileti gönderir ve alır.

1. Konsol penceresinde (CMD, PowerShell veya Azure CLı gibi), `dotnet new` **queueapp**adlı yeni bir konsol uygulaması oluşturmak için komutunu kullanın. Bu komut, tek bir kaynak dosyası olan basit bir "Merhaba Dünya" C# projesi oluşturur: **program.cs**.

   ```console
   dotnet new console -n QueueApp
   ```

2. Yeni oluşturulan **Queueapp** klasörüne geçin ve bunların tümünün iyi olduğunu doğrulamak için uygulamayı derleyin.

   ```console
   cd QueueApp
   ```

   ```console
   dotnet build
   ```

   Aşağıdaki çıktıya benzer sonuçlar görmeniz gerekir:

   ```output
   C:\Tutorials>dotnet new console -n QueueApp
   The template "Console Application" was created successfully.

   Processing post-creation actions...
   Running 'dotnet restore' on QueueApp\QueueApp.csproj...
     Restore completed in 155.63 ms for C:\Tutorials\QueueApp\QueueApp.csproj.

   Restore succeeded.

   C:\Tutorials>cd QueueApp

   C:\Tutorials\QueueApp>dotnet build
   Microsoft (R) Build Engine version 16.0.450+ga8dc7f1d34 for .NET Core
   Copyright (C) Microsoft Corporation. All rights reserved.

     Restore completed in 40.87 ms for C:\Tutorials\QueueApp\QueueApp.csproj.
     QueueApp -> C:\Tutorials\QueueApp\bin\Debug\netcoreapp3.1\QueueApp.dll

   Build succeeded.
       0 Warning(s)
       0 Error(s)

   Time Elapsed 00:00:02.40

   C:\Tutorials\QueueApp>_
   ```

## <a name="add-the-azure-client-libraries"></a>Azure istemci kitaplıklarını ekleme

1. Komutunu kullanarak Azure Storage istemci kitaplıklarını projeye ekleyin `dotnet add package` .

   # <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

   Konsol penceresindeki proje klasöründen aşağıdaki komutu yürütün.

   ```console
   dotnet add package Azure.Storage.Queues
   ```

   # <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

   Konsol penceresindeki proje klasöründen aşağıdaki komutları yürütün.

   ```console
   dotnet add package Microsoft.Azure.Storage.Common
   ```

   ```console
   dotnet add package Microsoft.Azure.Storage.Queue
   ```
   ---

### <a name="add-using-statements"></a>Using deyimleri Ekle

1. Proje dizinindeki komut satırından, `code .` geçerli dizinde Visual Studio Code açmak için yazın. Komut satırı penceresini açık tutun. Daha sonra yürütülecek daha fazla komut olacaktır. Derleme ve hata ayıklama için gereken C# varlıkları eklemeniz istenirse, **Evet** düğmesine tıklayın.

1. **Program.cs** kaynak dosyasını açın ve deyimden hemen sonra aşağıdaki ad alanlarını ekleyin `using System;` . Bu uygulama, Azure depolama 'ya bağlanmak ve kuyruklarla çalışmak için bu ad alanlarındaki türleri kullanır.

   # <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_UsingStatements":::

   # <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_UsingStatements":::

1. **Program.cs** dosyasını kaydedin.

## <a name="add-support-for-asynchronous-code"></a>Zaman uyumsuz kod desteği ekle

Uygulama bulut kaynaklarını kullandığından, kod zaman uyumsuz olarak çalışır.

1. **Ana** yöntemi zaman uyumsuz olarak çalışacak şekilde güncelleştirin. **Void** değerini **zaman uyumsuz bir görev** dönüş değeri ile değiştirin.

   ```csharp
   static async Task Main(string[] args)
   ```

1. **Program.cs** dosyasını kaydedin.

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Azure API 'Lerine hiçbir çağrı yapmadan önce, Azure portal kimlik bilgilerinizi almalısınız.

[!INCLUDE [storage-quickstart-credentials-include](../../../includes/storage-quickstart-credentials-include.md)]

### <a name="add-the-connection-string-to-the-app"></a>Bağlantı dizesini uygulamaya ekleme

Depolama hesabına erişebilmeleri için bağlantı dizesini uygulamaya ekleyin.

1. Visual Studio Code 'e geri dönün.

1. **Main** yönteminde, `Console.WriteLine("Hello World!");` kodu ortam değişkeninden bağlantı dizesini alan aşağıdaki satırla değiştirin.

   # <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_DeclareConnectionString":::

   # <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_DeclareConnectionString":::

1. Daha sonra gönderme ve alma yöntemlerine geçirilen bir kuyruk nesnesi oluşturmak için aşağıdaki kodu **Main** öğesine ekleyin.

   # <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_CreateQueueClient":::

   # <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_CreateQueueClient":::

1. Dosyayı kaydedin.

## <a name="insert-messages-into-the-queue"></a>Kuyruğa ileti ekleme

Kuyruğa ileti göndermek için yeni bir yöntem oluşturun.

1. Aşağıdaki **ınsertmessageasync** yöntemini **Program** sınıfınıza ekleyin.

   # <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

   Bu yönteme bir kuyruk başvurusu geçirilir. Zaten mevcut değilse, [Createifnotexistsasync](/dotnet/api/azure.storage.queues.queueclient.createifnotexistsasync)çağırarak yeni bir kuyruk oluşturulur. Ardından, [SendMessageAsync](/dotnet/api/azure.storage.queues.queueclient.sendmessageasync)çağırarak *newMessage* kuyruğa eklenir.

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_InsertMessage":::

   # <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

   Bu yönteme bir kuyruk başvurusu geçirilir. Zaten mevcut değilse, [Createifnotexistsasync](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.createifnotexistsasync)çağırarak yeni bir kuyruk oluşturulur. Ardından, [Addmessageasync](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.addmessageasync)çağırarak *newMessage* kuyruğa eklenir.

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_InsertMessage":::

1. **Isteğe bağlı** Varsayılan olarak, bir ileti için en uzun yaşam süresi yedi güne ayarlanır. İletinin yaşam süresi için herhangi bir pozitif sayı belirtebilirsiniz. Aşağıdaki kod parçacığı, süresi *dolmayacak* bir ileti ekler.

   # <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

    Süreleri dolan bir ileti eklemek için `Timespan.FromSeconds(-1)` **SendMessageAsync**çağrısında kullanın.

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Initial.cs" id="snippet_SendNonExpiringMessage":::

   # <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

    Kullanım süreleri dolan bir ileti eklemek için `Timespan.FromSeconds(-1)` **Addmessageasync**çağrısında kullanın.

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Initial.cs" id="snippet_SendNonExpiringMessage":::

1. Dosyayı kaydedin.

Kuyruk iletisi UTF-8 kodlaması kullanan bir XML isteğiyle uyumlu biçimde olmalıdır. İleti boyutu 64 KB 'ye kadar olabilir. Bir ileti ikili veri içeriyorsa, iletiyi [Base64 olarak kodlayın](/dotnet/api/system.convert.tobase64string) .

## <a name="dequeue-messages"></a>İletileri sıradan çıkarma

Kuyruktan ileti almak için yeni bir yöntem oluşturun. İleti başarıyla alındıktan sonra, bir kereden fazla işlenmemesi için kuyruktan silinmesi önemlidir.

1. **Program** sınıfınız için, **ıneveneventextmessageasync** adlı yeni bir yöntem ekleyin.

   # <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

   Bu yöntem, sıradaki bir sonraki iletiyi almak için birinci parametreye 1 geçirerek [Receivemessagesasync](/dotnet/api/azure.storage.queues.queueclient.receivemessagesasync)çağırarak kuyruktan bir ileti alır. İleti alındıktan sonra [Deletemessageasync](/dotnet/api/azure.storage.queues.queueclient.deletemessageasync)çağırarak kuyruktan silin.

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Initial.cs" id="snippet_InitialRetrieveMessage":::

   # <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

   Bu yöntem, [Getmessageasync](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.getmessageasync)çağırarak kuyruktan bir ileti alır. İleti alındıktan sonra [Deletemessageasync](/dotnet/api/microsoft.azure.storage.queue.cloudqueue.deletemessageasync)çağırarak kuyruktan silin.

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Initial.cs" id="snippet_InitialRetrieveMessage":::

1. Dosyayı kaydedin.

## <a name="delete-an-empty-queue"></a>Boş bir kuyruğu silme

Projenin sonunda oluşturduğunuz kaynaklara ihtiyacınız olup olmadığını belirlemek için en iyi uygulamadır. Çalışır durumda bırakılan kaynaklar maliyetlerin artmasına neden olabilir. Sıra varsa ancak boşsa, kullanıcıyı silmek isteyip istemediğini sorar.

1. Boş kuyruğu silmek için bir istem eklemek **üzere, yeniden** alma yöntemini genişletin.

   # <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_RetrieveMessage":::

   # <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_RetrieveMessage":::

1. Dosyayı kaydedin.

## <a name="check-for-command-line-arguments"></a>Komut satırı bağımsız değişkenlerini denetle

Uygulamaya geçirilen herhangi bir komut satırı bağımsız değişkeni varsa, bu, kuyruğa eklenecek bir ileti olduğunu varsayın. Bir dize oluşturmak için bağımsız değişkenleri birleştirin. Daha önce eklediğimiz **ınsertmessageasync** yöntemini çağırarak bu dizeyi ileti kuyruğuna ekleyin.

Komut satırı bağımsız değişkeni yoksa alma işlemini deneyin. Sıradaki bir sonraki iletiyi almak için, **Alaevenextmessageasync** metodunu çağırın.

Son olarak, **Console. ReadLine**'u çağırarak çıkmadan önce Kullanıcı girişini bekleyin.

1. Komut satırı bağımsız değişkenlerini denetlemek ve Kullanıcı girişini beklemek için **Main** yöntemini genişletin.

   # <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_Main":::

   # <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_Main":::

1. Dosyayı kaydedin.

## <a name="complete-code"></a>Kodu tamamla

Bu proje için kod listesinin tamamı aşağıda verilmiştir.

   # <a name="net-v12"></a>[\.NET V12](#tab/dotnet)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v12/QueueApp/Program.cs" id="snippet_AllCode":::

   # <a name="net-v11"></a>[\.NET v11](#tab/dotnetv11)

   :::code language="csharp" source="~/azure-storage-snippets/queues/tutorial/dotnet/dotnet-v11/QueueApp/Program.cs" id="snippet_AllCode":::
   ---

## <a name="build-and-run-the-app"></a>Uygulamayı derleyin ve çalıştırın

1. Proje dizinindeki komut satırından, projeyi derlemek için aşağıdaki DotNet komutunu çalıştırın.

   ```console
   dotnet build
   ```

1. Proje başarıyla yapılandırıldıktan sonra, sıraya ilk iletiyi eklemek için aşağıdaki komutu çalıştırın.

   ```console
   dotnet run First queue message
   ```

   Şu çıkışı görmelisiniz:

   ```output
   C:\Tutorials\QueueApp>dotnet run First queue message
   The queue was created.
   Sent: First queue message
   Press Enter..._
   ```

1. Kuyruktaki ilk iletiyi almak ve kaldırmak için komut satırı bağımsız değişkeni olmadan uygulamayı çalıştırın.

   ```console
   dotnet run
   ```

1. Tüm iletiler kaldırılana kadar uygulamayı çalıştırmaya devam edin. Daha sonra bir kez çalıştırırsanız, sıranın boş olduğunu belirten bir ileti ve kuyruğu silmek için bir istem alırsınız.

   ```output
   C:\Tutorials\QueueApp>dotnet run First queue message
   The queue was created.
   Sent: First queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run Second queue message
   Sent: Second queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run Third queue message
   Sent: Third queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: First queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: Second queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   Received: Third queue message
   Press Enter...

   C:\Tutorials\QueueApp>dotnet run
   The queue is empty. Attempt to delete it? (Y/N) Y
   Received: The queue was deleted.
   Press Enter...

   C:\Tutorials\QueueApp>_
   ```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

1. Bir kuyruk oluşturma
1. Kuyruktaki iletileri ekleme ve kaldırma
1. Azure depolama kuyruğunu silme

Daha fazla bilgi için Azure Kuyrukları hızlı başlangıçlarına göz atın.

> [!div class="nextstepaction"]
> [Portal için kuyruklar hızlı başlangıcı](storage-quickstart-queues-portal.md)

- [.NET için kuyruklar hızlı başlangıcı](storage-quickstart-queues-dotnet.md)
- [Java için kuyruklar hızlı başlangıcı](storage-quickstart-queues-java.md)
- [Python için kuyruklar hızlı başlangıcı](storage-quickstart-queues-python.md)
- [JavaScript için kuyruklar hızlı başlangıcı](storage-quickstart-queues-nodejs.md)

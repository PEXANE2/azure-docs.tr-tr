---
title: Küme düğümlerini başlat ve durdur
description: Küme düğümlerini başlatıp durdurarak Service Fabric uygulamasını test etmek için hata enjeksiyonu nasıl kullanılacağını öğrenin.
author: LMWF
ms.topic: conceptual
ms.date: 6/12/2017
ms.author: lemai
ms.openlocfilehash: 8f2eefec94ad4763a054ee089b17232c41e642dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75609800"
---
# <a name="replacing-the-start-node-and-stop-node-apis-with-the-node-transition-api"></a>Başlangıç Düğümü ve Durdur düğümü API'lerini Düğüm Geçişi API'siyle değiştirme

## <a name="what-do-the-stop-node-and-start-node-apis-do"></a>Stop Node ve Start Node API'leri ne işe yyapar?

Stop Node API (yönetilen: [StopNodeAsync()][stopnode], PowerShell: [Stop-ServiceFabricNode][stopnodeps]) bir Servis Kumaş düğümü durdurur.  Servis Kumaş düğümü bir VM veya makine değil, işlemdir – VM veya makine çalışmaya devam edecektir.  Belgenin geri kalanı için "düğüm" Hizmet Kumaş düğümü anlamına gelecektir.  Düğümü durdurmak, düğümü kümenin bir üyesi olmadığı ve hizmetleri barındıramadığı *durmuş* bir duruma sokar ve böylece *aşağı* düğümü simüle edilir.  Bu, uygulamanızı test etmek için sisteme hata enjekte etmek için yararlıdır.  Başlangıç Düğümü API'si (yönetilen: [StartNodeAsync()][startnode], PowerShell: [Start-ServiceFabricNode][startnodeps]]) Düğüm API'sını tersine çevirir ve bu da düğümü normal bir duruma geri getirir.

## <a name="why-are-we-replacing-these"></a>Neden bunların yerine geçtiğimizi?

Daha önce açıklandığı gibi, *durdurulan* Hizmet Kumaşdüğümü, Stop Node API kullanılarak kasıtlı olarak hedeflenen bir düğümdür.  *Aşağı* düğüm, başka bir nedenle kapalı olan bir düğümdür (örneğin, VM veya makine kapalıdır).  Stop Node API ile *sistem, durdurulan* düğümler ile *aşağı* düğümler arasında ayrım yapmak için bilgileri açığa çıkarmaz.

Buna ek olarak, bu API'ler tarafından döndürülen bazı hatalar olabildiğince açıklayıcı değildir.  Örneğin, *zaten durmuş* bir düğümüzerinde Stop Düğümü API'yi çağırmak *Geçersiz Adres*hatasını döndürür.  Bu deneyim geliştirilebilir.

Ayrıca, Başlangıç Düğümü API çağrılana kadar bir düğümün durdurulduğu süre "sonsuz"dur.  Bunun sorunlara yol açabileceğini ve hataya yatkın olabileceğini bulduk.  Örneğin, bir kullanıcının düğüm üzerinde Stop Node API'yi çağırıp sonra da bunu unuttuğu sorunlar gördük.  Daha sonra, düğüm *aşağı* veya *durduruldu*olup olmadığı belli değildi.


## <a name="introducing-the-node-transition-apis"></a>Düğüm Geçiş API'leri Tanıtımı

Bu sorunları yeni bir API setinde ele aldık.  Yeni Düğüm Geçiş API 'si (yönetilen: [StartNodeTransitionAsync()][snt]bir Hizmet Dokusu düğümini *durmuş* duruma geçiş yapmak veya *durmuş* bir durumdan normal bir duruma geçiş yapmak için kullanılabilir.  API adına "Başlangıç" düğümü başlatma anlamına gelmez.  Bu, sistemin düğümü *durdurulmuş* veya başlatılan duruma geçiş için yürüteceği eşzamanlı bir işlem başlatmayı ifade eder.

**Kullanım**

Düğüm Geçiş API çağrıldığınızda bir özel durum atmazsa, sistem eşzamanlı işlemi kabul etti ve yürütecek.  Başarılı bir arama, işlemin henüz tamamlanacağı anlamına gelmez.  İşlemin geçerli durumu hakkında bilgi almak için, düğüm geçiş ilerleme API'sini (yönetilen: [GetNodeTransitionProgressAsync()][gntp]bu işlem için Düğüm Geçiş API'sını çağırırken kullanılan guidile birlikte arayın.  Düğüm Geçişi İlerleme API'si bir DüğümGeçişi İlerleme nesnesi döndürür.  Bu nesnenin Durum özelliği, işlemin geçerli durumunu belirtir.  Durum "Çalışıyor" ise, işlem yürütülme.  Tamamlanırsa, işlem hatasız olarak tamamlanır.  Hatalı ise, işlemi yürütmede bir sorun vardı.  Sonuç özelliğinin Özel Durum özelliği sorunun ne olduğunu gösterir.  Devlet https://docs.microsoft.com/dotnet/api/system.fabric.testcommandprogressstate özelliği hakkında daha fazla bilgi ve kod örnekleri için aşağıdaki "Örnek Kullanım" bölümüne bakın.


**Durmuş bir düğüm ile aşağı düğüm arasında ayrım** Düğüm Geçiş API'si kullanılarak bir düğüm *durdurulursa,* düğüm sorgusunun çıktısı (yönetilen: [GetNodeListAsync()][nodequery], PowerShell: [Get-ServiceFabricNode][nodequeryps]) bu düğümün gerçek bir *Durdurulmuş* özellik değerine sahip olduğunu gösterir.  Bu *Down* *diyeceknodeStatus* özelliği, değerinden farklı olduğunu unutmayın.  Düğüm *Durumu* özelliği *Down*değerine sahipse, ancak *Durduruldu* yanlışsa, düğüm Düğüm Geçiş API'sını kullanarak durdurulmadı ve başka bir nedenle *Aşağılandı.*  *IsStop* özelliği doğruysa ve *Düğüm Durumu* özelliği *Aşağı*ise, Düğüm Geçiş API'sını kullanarak durduruldu.

Düğüm Geçiş API'ni kullanarak *durdurulan* bir düğüm başlatmak, onu kümenin normal bir üyesi olarak yeniden çalışmaya döndürür.  Düğüm sorgusu API çıktısı Yanlış olarak *Durduruldu* ve *NodeStatus* Aşağı olmayan bir şey olarak (örneğin, Yukarı) gösterir.


**Sınırlı Süre** Düğüm geçiş API'sını kullanarak bir düğümü durdurur, gerekli parametrelerden biri olan *stopNodeDurationInSeconds,* düğümü *durdurmak*için saniyeler içinde süreyi temsil eder.  Bu değer, en az 600 ve en fazla 14400 olan izin verilen aralıkta olmalıdır.  Bu süre dolduktan sonra, düğüm kendisini otomatik olarak Yukarı durumuna yeniden başlatacak.  Bir kullanım örneği için aşağıdaki Örnek 1'e bakın.

> [!WARNING]
> Düğüm Geçiş API'lerini ve Stop Node ve Start Node API'lerini karıştırmaktan kaçının.  Öneri, yalnızca Düğüm Geçiş API'sını kullanmaktır.  > Bir düğüm Zaten Stop Node API kullanılarak durduruldu, önce > Düğüm Geçiş API'lerini kullanmadan önce Başlat Düğümü API'si kullanılarak başlanmalıdır.

> [!WARNING]
> Aynı düğümüzerinde paralel olarak birden çok Düğüm Geçiş API'si çağrı yapılamaz.  Böyle bir durumda, Düğüm Geçiş API > NodeTransitionInProgress bir ErrorCode özellik değeri ile FabricException atmak >.  Belirli bir düğümüzerinde düğüm geçişi > başlatıldıktan sonra, aynı düğümde yeni bir geçiş > başlatmadan önce işlemin bir terminal durumuna (Tamamlandı, Hatalı veya Zorla İptal Edildi) ulaşmasını beklemeniz gerekir.  Farklı düğümlerde paralel düğüm geçiş çağrılarına izin verilir.


#### <a name="sample-usage"></a>Örnek Kullanım


**Örnek 1** - Aşağıdaki örnek, düğümü durdurmak için Düğüm Geçiş API'sını kullanır.

```csharp
        // Helper function to get information about a node
        static Node GetNodeInfo(FabricClient fc, string node)
        {
            NodeList n = null;
            while (n == null)
            {
                n = fc.QueryManager.GetNodeListAsync(node).GetAwaiter().GetResult();
                Task.Delay(TimeSpan.FromSeconds(1)).GetAwaiter();
            };

            return n.FirstOrDefault();
        }

        static async Task WaitForStateAsync(FabricClient fc, Guid operationId, TestCommandProgressState targetState)
        {
            NodeTransitionProgress progress = null;

            do
            {
                bool exceptionObserved = false;
                try
                {
                    progress = await fc.TestManager.GetNodeTransitionProgressAsync(operationId, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                    exceptionObserved = true;
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                    exceptionObserved = true;
                }

                if (!exceptionObserved)
                {
                    Console.WriteLine("Current state of operation '{0}': {1}", operationId, progress.State);

                    if (progress.State == TestCommandProgressState.Faulted)
                    {
                        // Inspect the progress object's Result.Exception.HResult to get the error code.
                        Console.WriteLine("'{0}' failed with: {1}, HResult: {2}", operationId, progress.Result.Exception, progress.Result.Exception.HResult);

                        // ...additional logic as required
                    }

                    if (progress.State == targetState)
                    {
                        Console.WriteLine("Target state '{0}' has been reached", targetState);
                        break;
                    }
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (true);
        }

        static async Task StopNodeAsync(FabricClient fc, string nodeName, int durationInSeconds)
        {
            // Uses the GetNodeListAsync() API to get information about the target node
            Node n = GetNodeInfo(fc, nodeName);

            // Create a Guid
            Guid guid = Guid.NewGuid();

            // Create a NodeStopDescription object, which will be used as a parameter into StartNodeTransition
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, durationInSeconds);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with the NodeStopDescription from above, which will stop the target node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    // This is retryable
                }
                catch (FabricTransientException fte)
                {
                    // This is retryable
                }

                // Backoff
                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);
            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until the desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Örnek 2** - Aşağıdaki örnek *durmuş* bir düğüm başlatır.  İlk örnekten bazı yardımcı yöntemler kullanır.

```csharp
        static async Task StartNodeAsync(FabricClient fc, string nodeName)
        {
            // Uses the GetNodeListAsync() API to get information about the target node
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = n.NodeInstanceId;

            // Create a NodeStartDescription object, which will be used as a parameter into StartNodeTransition
            NodeStartDescription description = new NodeStartDescription(guid, n.NodeName, nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with the NodeStartDescription from above, which will start the target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until the desired state is reached.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Completed).ConfigureAwait(false);
        }
```

**Örnek 3** - Aşağıdaki örnekyanlış kullanımı gösterir.  Sağladığı *stopDurationInSeconds* izin verilen aralıktan daha büyük olduğundan bu kullanım yanlıştır.  StartNodeTransitionAsync() önemli bir hata ile başarısız olacağıiçin, işlem kabul edilmedi ve ilerleme API çağrılmamalıdır.  Bu örnek, ilk örnekten bazı yardımcı yöntemleri kullanır.

```csharp
        static async Task StopNodeWithOutOfRangeDurationAsync(FabricClient fc, string nodeName)
        {
            Node n = GetNodeInfo(fc, nodeName);

            Guid guid = Guid.NewGuid();

            // Use an out of range value for stopDurationInSeconds to demonstrate error
            NodeStopDescription description = new NodeStopDescription(guid, n.NodeName, n.NodeInstanceId, 99999);

            try
            {
                await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
            }

            catch (FabricException e)
            {
                Console.WriteLine("Caught {0}", e);
                Console.WriteLine("ErrorCode {0}", e.ErrorCode);
                // Output:
                // Caught System.Fabric.FabricException: System.Runtime.InteropServices.COMException (-2147017629)
                // StopDurationInSeconds is out of range ---> System.Runtime.InteropServices.COMException: Exception from HRESULT: 0x80071C63
                // << Parts of exception omitted>>
                //
                // ErrorCode InvalidDuration
            }
        }
```

**Örnek 4** - Aşağıdaki örnek, Düğüm Geçiş API'sı tarafından başlatılan işlem kabul edildiğinde Düğüm Geçiş İlerleme API'sinden döndürülecek hata bilgilerini gösterir, ancak daha sonra yürütülürken başarısız olur.  Düğüm Geçiş API var olmayan bir düğüm başlatmaya çalıştığından, bu durumda başarısız olur.  Bu örnek, ilk örnekten bazı yardımcı yöntemleri kullanır.

```csharp
        static async Task StartNodeWithNonexistentNodeAsync(FabricClient fc)
        {
            Guid guid = Guid.NewGuid();
            BigInteger nodeInstanceId = 12345;

            // Intentionally use a nonexistent node
            NodeStartDescription description = new NodeStartDescription(guid, "NonexistentNode", nodeInstanceId);

            bool wasSuccessful = false;

            do
            {
                try
                {
                    // Invoke StartNodeTransitionAsync with the NodeStartDescription from above, which will start the target stopped node.  Retry transient errors.
                    await fc.TestManager.StartNodeTransitionAsync(description, TimeSpan.FromMinutes(1), CancellationToken.None).ConfigureAwait(false);
                    wasSuccessful = true;
                }
                catch (OperationCanceledException oce)
                {
                    Console.WriteLine("Caught exception '{0}'", oce);
                }
                catch (FabricTransientException fte)
                {
                    Console.WriteLine("Caught exception '{0}'", fte);
                }

                await Task.Delay(TimeSpan.FromSeconds(5)).ConfigureAwait(false);

            }
            while (!wasSuccessful);

            // Now call StartNodeTransitionProgressAsync() until the desired state is reached.  In this case, it will end up in the Faulted state since the node does not exist.
            // When StartNodeTransitionProgressAsync()'s returned progress object has a State if Faulted, inspect the progress object's Result.Exception.HResult to get the error code.
            // In this case, it will be NodeNotFound.
            await WaitForStateAsync(fc, guid, TestCommandProgressState.Faulted).ConfigureAwait(false);
        }
```

[stopnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN
[stopnodeps]: https://msdn.microsoft.com/library/mt125982.aspx
[startnode]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.faultmanagementclient?redirectedfrom=MSDN
[startnodeps]: https://msdn.microsoft.com/library/mt163520.aspx
[nodequery]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient
[nodequeryps]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricnode
[snt]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient
[gntp]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.testmanagementclient

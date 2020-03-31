---
title: Azure-SSIS tümleştirme çalışma zamanı için lisanslı bileşenler yükleme
description: Bir ISV'nin Azure-SSIS tümleştirme çalışma süresi için ücretli veya lisanslı özel bileşenleri nasıl geliştirip yükleyebileceğini öğrenin
services: data-factory
ms.service: data-factory
ms.workload: data-services
author: swinarko
ms.author: sawinark
manager: mflasko
ms.reviewer: douglasl
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/01/2019
ms.openlocfilehash: 599b54f8a5d97ee5ed29ce4df16980f456ffb919
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74914574"
---
# <a name="install-paid-or-licensed-custom-components-for-the-azure-ssis-integration-runtime"></a>Azure-SSIS Integration Runtime için ücretli veya lisanslı özel bileşenleri yükleme

Bu makalede, Bir ISV'nin Azure-SSIS tümleştirme çalışma zamanında Azure'da çalışan SQL Server Tümleştirme Hizmetleri (SSIS) paketleri için ücretli veya lisanslı özel bileşenler nasıl geliştirebileceği ve yükleyebileceği açıklanmaktadır.

## <a name="the-problem"></a>Sorun

Azure-SSIS tümleştirme çalışma zamanının doğası, özel bileşenlerin şirket içi yüklenmesi için kullanılan tipik lisanslama yöntemlerini yetersiz hale getiren çeşitli zorluklar sunar. Sonuç olarak, Azure-SSIS IR farklı bir yaklaşım gerektirir.

-   Azure-SSIS IR düğümleri geçicidir ve herhangi bir zamanda ayrılabilir veya serbest bırakılabilir. Örneğin, maliyeti yönetmek için düğümleri başlatabilir veya durdurabilir veya çeşitli düğüm boyutları nda yukarı ve aşağı ölçeklendirebilirsiniz. Sonuç olarak, mac adresi veya CPU Kimliği gibi makineye özgü bilgileri kullanarak üçüncü taraf bileşen lisansLarını belirli bir düğüme bağlamak artık geçerli değildir.

-   Düğüm sayısının istediğiniz zaman küçülmesi veya genişlemesi için Azure-SSIS IR'yi içinde veya dışında da ölçeklendirebilirsiniz.

## <a name="the-solution"></a>Çözüm

Önceki bölümde açıklanan geleneksel lisanslama yöntemlerinin sınırlamaları sonucunda, Azure-SSIS IR yeni bir çözüm sağlar. Bu çözüm, lisans bağlama ve üçüncü taraf bileşenlerin doğrulanması için Windows ortam değişkenleri ve SSIS sistem değişkenlerini kullanır. ISV'ler, Küme Kimliği ve Küme Düğüm Sayısı gibi bir Azure-SSIS IR için benzersiz ve kalıcı bilgiler elde etmek için bu değişkenleri kullanabilir. Bu bilgilerle, ISV'ler daha sonra kendi bileşeni için lisans bir Azure-SSIS IR *bir küme olarak*bağlayabilirsiniz. Bu bağlama, müşteriler Azure-SSIS IR'yi herhangi bir şekilde başlattıklarında veya durdurduklarında, küçülttterken veya küçültttün, ölçeklendirdiğinde veya yeniden yapılandırdığında değişmeyen bir kimlik kullanır.

Aşağıdaki diyagram, bu yeni değişkenleri kullanan üçüncü taraf bileşenler için tipik yükleme, etkinleştirme ve lisans bağlama ve doğrulama akışlarını gösterir:

![Lisanslı bileşenlerin montajı](media/how-to-configure-azure-ssis-ir-licensed-components/licensed-component-installation.png)

## <a name="instructions"></a>Yönergeler
1. ISV'ler lisanslı bileşenlerini çeşitli SDO'larda veya katmanlarda sunabilir (örneğin, tek düğüm, en fazla 5 düğüm, 10 düğüm vb.). ISV, müşteriler bir ürünü satın aldığında ilgili Ürün Anahtarını sağlar. ISV ayrıca, ISV Kurulum komut dosyası ve ilişkili dosyalar içeren bir Azure Depolama blob kapsayıcısı da sağlayabilir. Müşteriler bu dosyaları kendi depolama kaplarına kopyalayabilir ve kendi Ürün Anahtarıyla `IsvSetup.exe -pid xxxx-xxxx-xxxx`değiştirebilir (örneğin, çalıştırarak). Müşteriler daha sonra Azure-SSIS IR'yi konteynerlerinin SAS URI'si ile parametre olarak sağlayabilir veya yeniden yapılandırabilir. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirmesi çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md).

2. Azure-SSIS IR sağlandığında veya yeniden yapılandırıldığında, ISV Kurulumu Windows ortam değişkenlerini `SSIS_CLUSTERID` sorgulamak için her düğümde çalışır ve `SSIS_CLUSTERNODECOUNT`. Ardından Azure-SSIS IR, bir Etkinleştirme Anahtarı oluşturmak için lisanslı ürünün Küme Kimliğini ve Ürün Anahtarını ISV Etkinleştirme Sunucusuna gönderir.

3. Etkinleştirme Anahtarı'nı aldıktan sonra ISV Kurulumu anahtarı her düğümde (örneğin, Kayıt Defteri'nde) yerel olarak depolayabilir.

4. Müşteriler, AZURE-SSIS IR düğümünde ISV'nin lisanslı bileşenini kullanan bir paket çalıştırdığında, paket yerel olarak depolanan Etkinleştirme Anahtarını okur ve düğümün Küme Kimliği'ne göre doğrular. Paket ayrıca isteğe bağlı olarak Küme Düğümü Sayısını ISV etkinleştirme sunucusuna bildirebilir.

    Etkinleştirme anahtarını doğrulayan ve küme düğümü sayısını bildiren bir kod örneği aşağıda verilmiştir:

    ```csharp
    public override DTSExecResult Validate(Connections, VariableDispenser, IDTSComponentEvents componentEvents, IDTSLogging log) 
                                                                                                                               
    {                                                                                                                             
                                                                                                                               
    Variables vars = null;                                                                                                        
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterID");                                                                           
                                                                                                                               
    variableDispenser.LockForRead("System::ClusterNodeCount");                                                                    
                                                                                                                               
    variableDispenser.GetVariables(ref vars);                                                                                     
                                                                                                                               
    // Validate Activation Key with ClusterID                                                                                     
                                                                                                                               
    // Report on ClusterNodeCount                                                                                                 
                                                                                                                               
    vars.Unlock();                                                                                                                
                                                                                                                               
    return base.Validate(connections, variableDispenser, componentEvents, log);                                                   
                                                                                                                               
    }
    ```

## <a name="isv-partners"></a>ISV ortakları

Bu blog yazısının sonunda Azure-SSIS IR için bileşenlerini ve uzantılarını uyarlamış ISV iş ortaklarının bir listesini bulabilirsiniz - [Enterprise Edition, Custom Setup ve ADF'de SSIS için 3.](https://techcommunity.microsoft.com/t5/SQL-Server-Integration-Services/Enterprise-Edition-Custom-Setup-and-3rd-Party-Extensibility-for/ba-p/388360)

## <a name="next-steps"></a>Sonraki adımlar

-   [Azure-SSIS tümleştirme çalışma zamanı için özel kurulum](how-to-configure-azure-ssis-ir-custom-setup.md)

-   [Azure-SSIS Tümleştirme Çalışma Zamanı'nın Kurumsal Sürümü](how-to-configure-azure-ssis-ir-enterprise-edition.md)

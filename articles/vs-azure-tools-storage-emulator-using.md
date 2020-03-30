---
title: Visual Studio ile Depolama Emülatör'unun yapılandırılması ve kullanılması | Microsoft Dokümanlar
description: Yerel geliştirme makinenizde Azure'da bulunan Blob, Queue ve Tablo depolama hizmetlerini taklit eden bir yardımcı program olan depolama emülatörü yapılandırma ve kullanma.
services: visual-studio-online
author: ghogen
manager: jillfra
assetId: c8e7996f-6027-4762-806e-614b93131867
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 8/17/2017
ms.author: ghogen
ms.openlocfilehash: a6f853924416cce2440ca15767044029b20e651f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75450738"
---
# <a name="configuring-and-using-the-storage-emulator-with-visual-studio"></a>Visual Studio ile Depolama Öykünücüsünü Yapılandırma ve Kullanma

[!INCLUDE [storage-try-azure-tools](../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Genel Bakış

Azure SDK geliştirme ortamı, yerel geliştirme makinenizde Azure'da bulunan Blob, Kuyruk ve Tablo depolama hizmetlerini taklit eden bir yardımcı program olan depolama emülatörü içerir. Azure depolama hizmetlerini kullanan bir bulut hizmeti oluşturuyorsanız veya depolama hizmetlerini çağıran herhangi bir harici uygulama yazıyorsanız, kodunuzu yerel olarak depolama emülatörüne karşı test edebilirsiniz. Microsoft Visual Studio için Azure Araçları, depolama emülatörü yönetimini Visual Studio'ya entegre edin. Azure Araçları ilk kullanımda depolama emülatörü veritabanını başlatın, Visual Studio'dan kodunuzu çalıştırdığınızda veya hata ayıkladiğinizde depolama emülatör hizmetini başlatır ve Azure Depolama Gezgini aracılığıyla yalnızca okuma yayımlayıcı verilere erişim sağlar.

Sistem gereksinimleri ve özel yapılandırma yönergeleri de dahil olmak üzere depolama emülatörü hakkında ayrıntılı bilgi için [bkz.](storage/common/storage-use-emulator.md)

> [!NOTE]
> Depolama emülatör simülasyonu ile Azure depolama hizmetleri arasında bazı işlevsellik farklılıkları vardır. Belirli farklar hakkında bilgi için Azure SDK belgelerinde [Depolama Emülatörü ve Azure Depolama Hizmetleri arasındaki farklara](storage/common/storage-use-emulator.md) bakın.

## <a name="configuring-a-connection-string-for-the-storage-emulator"></a>Depolama emülatörü için bağlantı dizesi yapılandırma

Bir rol içindeki koddan depolama emülatörüne erişmek için, depolama emülatörüne işaret eden ve daha sonra bir Azure depolama hesabına işaret edecek şekilde değiştirilebilen bir bağlantı dizesi yapılandırmak istersiniz. Bağlantı dizesi, bir depolama hesabına bağlanmak için rolünüz çalışma zamanında okuyabileceği bir yapılandırma ayarıdır. Bağlantı dizeleri oluşturma hakkında daha fazla bilgi için Azure [Depolama bağlantı dizelerini yapılandırma](/azure/storage/common/storage-configure-connection-string)bilgisine bakın.

> [!NOTE]
> **GeliştirmeStorageAccount** özelliğini kullanarak kodunuzdan depolama emülatör hesabına bir başvuru döndürebilirsiniz. Bu yaklaşım, kodunuzdan depolama emülatörüne erişmek istiyorsanız doğru çalışır, ancak uygulamanızı Azure'da yayımlamayı planlıyorsanız, Azure depolama hesabınıza erişmek ve bu bağlantıyı kullanmak için kodunuzu değiştirmek için bir bağlantı dizesi oluşturmanız gerekir yayımlamadan önce dize. Depolama emülatör hesabı ile Azure depolama hesabı arasında sık sık geçiş yapıyorsunuzsa, bağlantı dizesi bu işlemi basitleştirir.

## <a name="initializing-and-running-the-storage-emulator"></a>Depolama emülatörlerini başlatma ve çalıştırma

Visual Studio'da hizmetinizi çalıştırdığınızda veya hata ayıkladiğinizde Visual Studio'nun depolama emülatörü otomatik olarak başlatdığını belirtebilirsiniz. Çözüm Gezgini'nde, **Azure** projeniziçin kısayol menüsünü açın ve **Özellikler'i**seçin. **Geliştirme** sekmesinde, **Azure Depolama Emülatörlerini Başlat** listesinde **True'yu** seçin (bu değere ayarlanmışsa).  Bazı proje türlerinde **Geliştirme** sekmesi yoktur. Bu durumda, proje dosyasındaki öğeyi `StartDevelopmentStorage` ayarlayarak depolama emülatörü başlatılmasını etkinleştirebilir veya devre dışı kullanabilirsiniz. Etkinleştirmek için **True** veya devre dışı bırakabilmek için **False** olarak ayarlayın.  Örneğin, bir Azure İşlevler projesinde, proje dosyasını düzenlemek için açın ve XML kodunu aşağıdaki gibi değiştirin:

```xml
  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
    <AzureFunctionsVersion>v2</AzureFunctionsVersion>
    <StartDevelopmentStorage>True</StartDevelopmentStorage>
  </PropertyGroup>
```

Visual Studio'dan ilk kez hizmetinizi çalıştırdığınızda veya hata ayıklamanızda, depolama emülatörü bir başlatma işlemi başlatir. Bu işlem depolama emülatörü için yerel bağlantı noktalarını ayırır ve depolama emülatörü veritabanını oluşturur. Tamamlandıktan sonra, depolama emülatörü veritabanı silinmedikçe bu işlemin yeniden çalışması gerekmez.

> [!NOTE]
> Azure Araçları'nın Haziran 2012 sürümünden başlayarak, depolama emülatörü varsayılan olarak SQL Express LocalDB'de çalışır. Azure Araçları'nın önceki sürümlerinde, depolama emülatörü Azure SDK'sını yüklemeden önce yüklemeniz gereken varsayılan SQL Express 2005 veya 2008 örneğine karşı çalışır. Depolama emülatörünün adını taşıyan bir SQL Express örneğine veya Microsoft SQL Server'ın adlandırılmış veya varsayılan örneğine karşı da çalıştırabilirsiniz. Depolama emülatörü varsayılan örnek dışında bir örneğe karşı çalışacak şekilde yapılandırmanız gerekiyorsa, [bkz.](storage/common/storage-use-emulator.md)

Depolama emülatörü, yerel depolama hizmetlerinin durumunu görüntülemek ve bunları başlatmak, durdurmak ve sıfırlamak için bir kullanıcı arabirimi sağlar. Depolama emülatör hizmeti başlatıldıktan sonra, Windows görev çubuğundaki Microsoft Azure Emülatör'ün bildirim alanı simgesini sağ tıklayarak kullanıcı arabirimini görüntüleyebilir veya hizmeti başlatabilir veya durdurabilirsiniz.

## <a name="viewing-storage-emulator-data-in-server-explorer"></a>Depolama emülatör verilerini Server Explorer'da görüntüleme

Server Explorer'daki Azure Depolama düğümü, depolama emülatörü de dahil olmak üzere depolama hesaplarınızdaki tablo ve tablo verilerinin verilerini görüntülemenize ve ayarları değiştirmenize olanak tanır. Daha fazla bilgi için [Storage Explorer ile Azure Blob Depolama kaynaklarını yönet'e](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-blobs) bakın.


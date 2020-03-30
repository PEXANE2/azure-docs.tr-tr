---
title: include dosyası
description: include dosyası
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 06/27/2019
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 4d77cb8128105a40143a40e48ebe450115f7cf1e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164322"
---
## <a name="prerequisites"></a>Ön koşullar

### <a name="azure-subscription"></a>Azure aboneliği
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

### <a name="azure-roles"></a>Azure rolleri
Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, *katkıda bulunan*, *sahip* veya *yönetici* rollerinin üyesi ya da bir Azure aboneliğinin yöneticisi olmalıdır. Abonelikteki izinleri görüntülemek için [Azure portalına](https://portal.azure.com)gidin , sağ üst köşedeki kullanıcı adınızı seçin, daha fazla seçenek için "**...**" simgesini seçin ve ardından **İzinlerim'i**seçin. Birden çok aboneliğe erişiminiz varsa uygun aboneliği seçin.

Data Factory için veri kümeleri, bağlı hizmetler, işlem hatları, tetikleyiciler ve tümleştirme çalışma zamanları gibi alt kaynaklar oluşturup yönetmek için aşağıdaki gereksinimlerin karşılanması gerekir:

- Azure portalda alt öğe oluşturup yönetebilmek için kaynak grubu düzeyinde **Data Factory Katılımcısı** rolüne veya daha üst düzey bir role sahip olmanız gerekir.
- PowerShell veya SDK ile alt öğe oluşturup yönetebilmek için kaynak düzeyinde **katkıda bulunan** rolü veya daha üst düzey bir rol yeterli olacaktır.

Kullanıcı rolü eklemeye ilişkin örnek yönergeler için [Rol ekleme](../articles/cost-management-billing/manage/add-change-subscription-administrator.md) makalesine bakın.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Data Factory Katılımcısı rolü](../articles/role-based-access-control/built-in-roles.md#data-factory-contributor)
- [Azure Data Factory için roller ve izinler](../articles/data-factory/concepts-roles-permissions.md)

### <a name="azure-storage-account"></a>Azure Depolama hesabınızın
Bu hızlı başlangıçta hem *kaynak* hem de *hedef* veri depoları olarak genel amaçlı bir Azure Depolama hesabı (özellikle Blob depolama alanı) kullanırsınız. Genel amaçlı bir Azure Depolama hesabınız yoksa, oluşturmak için [bir depolama hesabı oluşturma'ya](../articles/storage/common/storage-account-create.md) bakın. 

#### <a name="get-the-storage-account-name"></a>Depolama hesabı adını alma
Bu hızlı başlangıç için Azure Depolama hesabınızın adını almanız gerekir. Aşağıdaki yordam, depolama hesabınızın adını almak için adımlar sağlar: 

1. Bir web tarayıcısında Azure [portalına](https://portal.azure.com) gidin ve Azure kullanıcı adınızı ve parolanızı kullanarak oturum açın.
2. Azure portalı menüsünden **Tüm hizmetler'i**seçin ve ardından **Depolama** > **Alanı hesaplarını**seçin. Ayrıca herhangi bir sayfadan *Depolama hesaplarını* arayabilir ve seçebilirsiniz.
3. Depolama **hesapları** sayfasında, depolama hesabınıza filtre uygulayın (gerekirse) ve ardından depolama hesabınızı seçin. 

Ayrıca herhangi bir sayfadan *Depolama hesaplarını* arayabilir ve seçebilirsiniz.

#### <a name="create-a-blob-container"></a>Blob kapsayıcısı oluşturma
Bu bölümde, Azure Blob depolama alanında **adftutorial** adlı bir blob kapsayıcısı oluşturursunuz.

1. Depolama hesabı sayfasından **Genel Bakış** > **Kapsayıcıları'nı**seçin.
2. Kapsayıcılar sayfasının araç * \<çubuğunu>*  -  **Containers** Hesap adında **Kapsayıcı'yı**seçin.
3. **Yeni kapsayıcı** iletişim kutusunda ad olarak **adftutorial** girin ve ardından **Tamam**’ı seçin. **Kapsayıcılar**>Hesap  - adı, kapsayıcılar listesine **adftutorial** içerecek şekilde güncelleştirilir. * \< *

   ![Kapsayıcılar listesi](media/data-factory-quickstart-prerequisites/list-of-containers.png)

#### <a name="add-an-input-folder-and-file-for-the-blob-container"></a>Blob kapsayıcısı için giriş klasörü ve dosya ekleme
Bu bölümde, oluşturduğunuz kapsayıcıda **giriş** adlı bir klasör oluşturur sunuz ve ardından giriş klasörüne örnek bir dosya yüklersiniz. Başlamadan **önce, Not Defteri**gibi bir metin düzenleyicisi açın ve aşağıdaki içeriği içeren **emp.txt** adlı bir dosya oluşturun:

```emp.txt
John, Doe
Jane, Doe
```

Dosyayı **C:\ADFv2QuickStartPSH** klasörüne kaydedin. (Klasör zaten yoksa, oluşturun.) Ardından Azure portalına dönün ve aşağıdaki adımları izleyin:

1. Kaldığınız * \<Kapsayıcılar *  -  **>** Hesap adı sayfasında, güncelleştirilmiş kapsayıcılar listesinden **adftutorial'i** seçin.

   1. Pencereyi kapattıysanız veya başka bir sayfaya gittiyseniz, [Azure portalında](https://portal.azure.com) yeniden oturum açın.
   1. Azure portalı menüsünden **Tüm hizmetler'i**seçin ve ardından **Depolama** > **Alanı hesaplarını**seçin. Ayrıca herhangi bir sayfadan *Depolama hesaplarını* arayabilir ve seçebilirsiniz.
   1. Depolama hesabınızı seçin ve ardından **Kapsayıcılar** > **adftutorial'i**seçin.

2. **adftutorial** kapsayıcı sayfasının araç çubuğunda **Yükle'yi**seçin.
3. Yükleme **blob** sayfasında, **Dosyalar** kutusunu seçin ve ardından **emp.txt** dosyasına göz atın ve seçin.
4. **Gelişmiş** başlığı genişletin. Sayfa şimdi gösterildiği gibi görüntüler:

   ![Gelişmiş bağlantı seçeneğini belirleme](media/data-factory-quickstart-prerequisites/upload-blob-advanced.png)
5. **Klasöre Yükle kutusuna** **giriş girin.**
6. Ardından **Yükle** düğmesini seçin. Listede **emp.txt** dosyasını ve karşıya yükleme durumunu görmeniz gerekir.
7. **Upload blob** sayfasını kapatmak için **Kapat** simgesini **(X)** seçin.

**adftutorial** kapsayıcı sayfasını açık tutun. Bu hızlı başlangıcın sonundaki çıktıyı doğrulamak için bu sayfayı kullanırsınız.
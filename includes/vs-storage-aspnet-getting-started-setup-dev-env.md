---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a7c696870e22e1692ca5ed778e47f8e4cc00615a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188272"
---
## <a name="set-up-the-development-environment"></a>Geliştirme ortamını ayarlama

Bu bölüm, geliştirme ortamını kurma yoluyla yürür. Buna ASP.NET bir MVC uygulaması oluşturmak, bağlı bir hizmet bağlantısı eklemek, bir denetleyici eklemek ve gerekli ad alanı yönergelerini belirtmek dahildir.

### <a name="create-an-aspnet-mvc-app-project"></a>ASP.NET bir MVC uygulama projesi oluşturma

1. Visual Studio'yu açın.

1. Ana menüden**Yeni** >  **Dosya** > **Projesi'ni**seçin.

1. Yeni **Proje** iletişim kutusunda **Web** > **ASP.NET Web Uygulaması (.NET Framework) seçeneğini belirleyin.** **Ad** alanında **StorageAspNet'i**belirtin. **Tamam'ı**seçin.

    ![Yeni Proje iletişim kutusunun ekran görüntüsü](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. Yeni **ASP.NET Web Uygulaması** iletişim kutusunda **MVC'yi**seçin ve ardından **Tamam'ı**seçin.

    ![Yeni ASP.NET Web Uygulaması iletişim kutusu ekran görüntüsü](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Azure depolama hesabına bağlanmak için bağlı hizmetleri kullanma

1. **Çözüm Gezgini'nde**projeyi sağ tıklatın.

1. Bağlam menüsünden**Bağlı Hizmet** **Ekle'yi** > seçin.

1. Bağlı **Hizmetler** iletişim kutusunda, **Azure Depolama ile Bulut Depolama'yı**seçin.

    ![Bağlı Hizmetler iletişim kutusunun ekran görüntüsü](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. Azure **Depolama** iletişim kutusunda, bu öğretici için kullanılacak Azure depolama hesabını seçin. Yeni bir Azure depolama hesabı oluşturmak için **Yeni Depolama Hesabı Oluştur'u**seçin ve formu doldurun. Varolan bir depolama hesabı nı seçtikten veya yeni bir hesap oluşturduktan sonra **Ekle'yi**seçin. Visual Studio, Azure Depolama için NuGet paketini ve **Web.config'e**bir depolama bağlantı dizesini yükler.

1. **Solution**Explorer'da, **Bağımlılıklar'a**sağ tıklayın, **NuGet Paketlerini Yönet'i**seçin ve Microsoft.Azure.ConfigurationManager'un en son sürümüne bir NuGet paketi başvurusu ekleyin.

> [!TIP]
> [Azure portalında](https://portal.azure.com)nasıl bir depolama hesabı oluşturabilirsiniz öğrenmek için [bkz.](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
>
> [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [Azure CLI](../articles/storage/common/storage-azure-cli.md)veya Azure Bulut [BulutU'nu](../articles/cloud-shell/overview.md)kullanarak bir depolama hesabı da oluşturabilirsiniz.

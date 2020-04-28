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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67188272"
---
## <a name="set-up-the-development-environment"></a>Geliştirme ortamını ayarlama

Bu bölümde, geliştirme ortamının kurulması gösterilmektedir. Buna bir ASP.NET MVC uygulaması oluşturma, bağlı hizmetler bağlantısı ekleme, denetleyici ekleme ve gerekli ad alanı yönergelerini belirtme dahildir.

### <a name="create-an-aspnet-mvc-app-project"></a>ASP.NET MVC uygulama projesi oluşturma

1. Visual Studio'yu açın.

1. Ana menüden **Dosya** > **Yeni** > **Proje**' yi seçin.

1. **Yeni proje** iletişim kutusunda **Web** > **ASP.NET Web uygulaması (.NET Framework)** öğesini seçin. **Ad** alanında, **storageaspnet**' i belirtin. **Tamam**’ı seçin.

    ![Yeni Proje iletişim kutusunun ekran görüntüsü](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. **Yeni ASP.NET Web uygulaması** Iletişim kutusunda **MVC**' yi seçin ve ardından **Tamam**' ı seçin.

    ![Yeni ASP.NET Web uygulaması iletişim kutusunun ekran görüntüsü](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Azure depolama hesabına bağlanmak için bağlı hizmetleri kullanma

1. **Çözüm Gezgini**, projeye sağ tıklayın.

1. Bağlam menüsünden**bağlı hizmet** **Ekle** > ' yi seçin.

1. **Bağlı hizmetler** iletişim kutusunda, **Azure depolama ile bulut depolama**' yı seçin.

    ![Bağlı hizmetler iletişim kutusunun ekran görüntüsü](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. **Azure depolama** iletişim kutusunda, bu öğretici Için kullanılacak Azure Depolama hesabını seçin. Yeni bir Azure depolama hesabı oluşturmak için **Yeni depolama hesabı oluştur**' u seçin ve formu doldurun. Mevcut bir depolama hesabı seçtikten veya yeni bir tane oluşturduktan sonra **Ekle**' yi seçin. Visual Studio, Azure depolama için NuGet paketini ve **Web. config**'e bir depolama bağlantı dizesi yükleme.

1. **Çözüm Gezgini**' de **Bağımlılıklar**' a sağ tıklayın, **NuGet Paketlerini Yönet**' i seçin ve Microsoft. Azure. ConfigurationManager ' nin en son sürümüne bir NuGet paket başvurusu ekleyin.

> [!TIP]
> [Azure Portal](https://portal.azure.com)bir depolama hesabı oluşturmayı öğrenmek için bkz. [depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).
>
> [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [Azure CLI](../articles/storage/common/storage-azure-cli.md)veya [Azure Cloud Shell](../articles/cloud-shell/overview.md)kullanarak da bir depolama hesabı oluşturabilirsiniz.

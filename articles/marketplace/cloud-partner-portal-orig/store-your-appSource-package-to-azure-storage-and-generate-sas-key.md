---
title: AppSource Paketinizi Azure depolama alanına saklayın ve SAS tuşu ile bir URL oluşturun
description: Bir AppSource paketini yüklemek ve güvenli hale getirmek için gereken adımları ayrıntıları.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: dsindona
ms.openlocfilehash: 57bc370fd160b8b3d6d7941ea28cd460c99f3d96
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80285376"
---
<a name="store-your-appsource-package-to-azure-storage-and-generate-a-url-with-sas-key"></a>AppSource Paketinizi Azure depolama alanına saklayın ve SAS tuşu ile bir URL oluşturun
=============================================================================

Dosyalarınızın güvenliğini sağlamak için tüm iş ortaklarının AppSource paket dosyalarını azure blob depolama hesabında saklaması ve paylaşmak için bir SAS anahtarı kullanması gerekir. Paket dosyasını sertifikalandırma ve AppSource denemeleri için kullanmak üzere Azure depolama konumunuzdan alacağız.

Paketinizi blob depolama alanına yüklemek için aşağıdaki adımları kullanın:

1. Ücretsiz <https://azure.microsoft.com> deneme veya faturalı bir hesaba gidin ve oluşturun.

2. [Azure Portalı'nda](https://portal.azure.com/)oturum açın.

3. **+ Yeni'yi** tıklayarak ve Veri **+ Depolama** hesabına giderek yeni bir Depolama Hesabı oluşturun.

   ![Microsoft Azure Portalı'nda Veri + Depolama bıçağı](media/CRMScreenShot7.png)

4. **Ad** ve **Kaynak Grubu** adı girin ve **Oluştur** düğmesini tıklatın.

   ![Microsoft Azure Portalı'nda depolama hesabı oluşturma](media/CRMScreenShot8.png)

5. Yeni oluşturduğunuz kaynak grubuna gidin ve yeni bir blob kapsayıcısı oluşturun.

   ![Microsoft Azure Portal'ı kullanarak paketi blob olarak yükleyin](media/CRMScreenShot9.png)

6. Bunu daha önce yapmadıysanız, Microsoft [Azure Depolama Gezgini'ni](https://storageexplorer.com/)indirin ve yükleyin.

7. Depolama Gezgini'ni açın ve Azure depolama hesabınıza bağlanmak için simgeyi kullanın.

8. Oluşturduğunuz blob kapsayıcısına gidin ve paket zip dosyanızı eklemek için **Yükle'yi** tıklatın.

   ![Microsoft Storage Explorer'ı kullanarak paket yükleme](media/CRMScreenShot10.png)

9. Dosyanıza sağ tıklayın ve **Paylaşılan Erişim İmzası alın'ı**seçin.

   ![Azure dosyasının paylaşılan erişim imzalarını alma](media/CRMScreenShot11.png)

10. SAS'ı bir ay boyunca etkin kılmak için **Son Kullanma süresini** değiştirin ve ardından **Oluştur'u**tıklatın.

    ![Azure dosyasının SAS son kullanma tarihini değiştirme](media/CRMScreenShot12.png)

11. URL alanını kopyalayın ve daha sonraya kaydedin. İlişkili teklifi oluştururken bu URL'yi girmeniz gerekir. 

    ![Azure dosyasının SAS URL'sini kopyalama](media/CRMScreenShot13.png)


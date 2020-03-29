---
title: Kullanıcıları Azure Veri Gölü Analizi hesabına ekleme
description: Kullanıcı Ekle Sihirbazı ve Azure PowerShell'i kullanarak verilerinizi Veri Gölü Analizi hesabınıza nasıl doğru şekilde ekleyeceğinizi öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: db35f16e-1565-4873-a851-bd987accdc58
ms.topic: conceptual
ms.date: 05/24/2018
ms.openlocfilehash: 16c503fe2d584d5f8256c65bfc49825b300f6a36
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71672723"
---
# <a name="adding-a-user-in-the-azure-portal"></a>Azure portalında kullanıcı ekleme

## <a name="start-the-add-user-wizard"></a>Kullanıcı Ekle Sihirbazı'nı Başlat
1. Azure Veri Gölü Analizinizi https://portal.azure.com' den açın.
2. **Kullanıcı Sihirbazı Ekle'yi**tıklatın.
3. Kullanıcı **seç** adımında, eklemek istediğiniz kullanıcıyı bulun. **Seç'i**tıklatın.
4. **rol adımını seçin,** **Data Lake Analytics Developer'ı**seçin. Bu rol, U-SQL işlerini göndermek/izlemek/yönetmek için gereken en az izin kümesine sahiptir. Grup Azure hizmetlerini yönetmek için tasarlanmamışsa bu role atayın.
5. Katalog **İzinleri Seç** adımında, kullanıcının erişması gereken ek veritabanlarını seçin. İş göndermek için ana veritabanına Erişim Oku ve Yaz gereklidir. Bitirdiğinizde, **Tamam**’a tıklayın.
6. **Seçili izinleri ata** adlı son adımda sihirbazın yapacağı değişiklikleri gözden geçirin. **Tamam**'a tıklayın.


## <a name="configure-acls-for-data-folders"></a>Veri klasörleri için Aç'ları yapılandırma
"R-X" veya "RWX"i gerektiği gibi giriş verileri ve çıktı verileri içeren klasörlerde verin.


## <a name="optionally-add-the-user-to-the-azure-data-lake-storage-gen1-role-reader-role"></a>İsteğe bağlı olarak, kullanıcıyı Azure Veri Gölü Depolama Gen1 **rolüNe** ekleyin.
1.  Azure Veri Gölü Depolama Gen1 hesabınızı bulun.
2.  **Kullanıcılar**’a tıklayın.
3. **Ekle**’ye tıklayın.
4.  Bu grubu atamak için bir Azure RBAC Rolü seçin.
5.  Reader rolüne atayın. Bu rol, ADLSGen1'de depolanan verilere göz atmak/yönetmek için gereken en az izin kümesine sahiptir. Grup Azure hizmetlerini yönetmeye yönelik değilse bu role atayın.
6.  Grubun adını yazın.
7.  **Tamam**'a tıklayın.

## <a name="adding-a-user-using-powershell"></a>PowerShell kullanarak kullanıcı ekleme

1. Bu kılavuzdaki yönergeleri izleyin: [Azure PowerShell nasıl yüklenir ve yapılandırılır.](https://azure.microsoft.com/documentation/articles/powershell-install-configure/)
2. [Add-AdlaJobUser.ps1](https://github.com/Azure/AzureDataLake/blob/master/Samples/PowerShell/ADLAUsers/Add-AdlaJobUser.ps1) PowerShell komut dosyasını indirin.
3. PowerShell komut dosyasını çalıştırın. 

Kullanıcıya iş gönderme, yeni iş meta verilerini görüntüleme ve eski meta verileri görüntüleme erişimi sağlayan örnek komutu şuşekildedir:

`Add-AdlaJobUser.ps1 -Account myadlsaccount -EntityToAdd 546e153e-0ecf-417b-ab7f-aa01ce4a7bff -EntityType User -FullReplication`


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics’e Genel Bakış](data-lake-analytics-overview.md)
* [Azure portalını kullanarak Data Lake Analytics'e başlayın](data-lake-analytics-get-started-portal.md)
* [Azure PowerShell'i kullanarak Azure Veri Gölü Analizini yönetin](data-lake-analytics-manage-use-powershell.md)


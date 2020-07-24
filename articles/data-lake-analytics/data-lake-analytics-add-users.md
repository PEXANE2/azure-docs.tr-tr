---
title: Azure Data Lake Analytics hesaba Kullanıcı ekleme
description: Kullanıcı Ekleme Sihirbazı 'Nı ve Azure PowerShell kullanarak Data Lake Analytics hesabınıza nasıl doğru şekilde kullanıcı ekleneceğini öğrenin.
services: data-lake-analytics
ms.service: data-lake-analytics
ms.reviewer: jasonh
ms.assetid: db35f16e-1565-4873-a851-bd987accdc58
ms.topic: how-to
ms.date: 05/24/2018
ms.openlocfilehash: 09f866d7ed72e82770a7f3a0fe5da24bb864851f
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87126083"
---
# <a name="adding-a-user-in-the-azure-portal"></a>Azure portalında kullanıcı ekleme

## <a name="start-the-add-user-wizard"></a>Kullanıcı Ekleme Sihirbazı 'Nı başlatın
1. Aracılığıyla Azure Data Lake Analytics açın https://portal.azure.com .
2. **Kullanıcı Ekleme Sihirbazı**' na tıklayın.
3. **Kullanıcı Seç** adımında, eklemek istediğiniz kullanıcıyı bulun. **Seç**’e tıklayın.
4. **Rol Seç** adımı **Data Lake Analytics geliştirici**' yi seçin. Bu rol, U-SQL işlerini göndermek/izlemek/yönetmek için gereken en düşük izin kümesini içerir. Grup, Azure hizmetlerini yönetmeye yönelik değilse, bu role atayın.
5. **Katalog Izinlerini seçin** adımında, kullanıcının erişmesi gereken diğer veritabanlarını seçin. İşleri göndermek için ana veritabanına okuma ve yazma erişimi gereklidir. Bitirdiğinizde, **Tamam**’a tıklayın.
6. **Seçili Izinleri ata** adlı son adımda, sihirbazın yapabileceği değişiklikleri gözden geçirin. **Tamam** düğmesine tıklayın.


## <a name="configure-acls-for-data-folders"></a>Veri klasörleri için ACL 'Leri yapılandırma
Giriş verilerini ve çıkış verilerini içeren klasörlerde gerektiğinde "R-X" veya "RWX" verin.


## <a name="optionally-add-the-user-to-the-azure-data-lake-storage-gen1-role-reader-role"></a>İsteğe bağlı olarak, kullanıcıyı Azure Data Lake Storage 1. rol **okuyucu** rolüne ekleyin.
1.  Azure Data Lake Storage 1. hesabınızı bulun.
2.  **Kullanıcılar**’a tıklayın.
3. **Ekle**'ye tıklayın.
4.  Bu grubu atamak için bir Azure RBAC rolü seçin.
5.  Okuyucu rolüne ata. Bu rol, ADLSGen1 içinde depolanan verileri taramak/yönetmek için gereken en düşük izin kümesine sahiptir. Grup, Azure hizmetlerini yönetmeye yönelik değilse, bu role atayın.
6.  Grubun adını yazın.
7.  **Tamam** düğmesine tıklayın.

## <a name="adding-a-user-using-powershell"></a>PowerShell kullanarak Kullanıcı ekleme

1. Bu kılavuzdaki yönergeleri izleyin: [nasıl yüklenir ve yapılandırılır Azure PowerShell](/powershell/azure/).
2. [Add-AdlaJobUser.ps1](https://github.com/Azure/AzureDataLake/blob/master/Samples/PowerShell/ADLAUsers/Add-AdlaJobUser.ps1) PowerShell betiğini indirin.
3. PowerShell betiğini çalıştırın. 

Kullanıcı erişimini iş göndermeye, yeni iş meta verilerini görüntülemeye ve eski meta verileri görüntülemeye yönelik örnek komut:

`Add-AdlaJobUser.ps1 -Account myadlsaccount -EntityToAdd 546e153e-0ecf-417b-ab7f-aa01ce4a7bff -EntityType User -FullReplication`


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Data Lake Analytics’e Genel Bakış](data-lake-analytics-overview.md)
* [Azure portal kullanarak Data Lake Analytics kullanmaya başlayın](data-lake-analytics-get-started-portal.md)
* [Azure PowerShell kullanarak Azure Data Lake Analytics yönetme](data-lake-analytics-manage-use-powershell.md)


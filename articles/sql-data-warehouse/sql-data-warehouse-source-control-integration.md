---
title: Kaynak denetimi tümleştirmesi | Microsoft Docs
description: Azure Repos (git ve GitHub) kullanarak yerel kaynak denetimi tümleştirmesiyle SQL veri ambarı için kurumsal sınıf veritabanı DevOps deneyimi.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: integration
ms.date: 08/23/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 10a10882efe05ef9e6bb86e54fcfcf8c5d73d225
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70098754"
---
# <a name="source-control-integration-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için kaynak denetimi tümleştirmesi

Bu öğreticide, SQL Server veri araçları (SSDT) veritabanı projenizi kaynak denetimiyle tümleştirme özetlenmektedir.  Kaynak denetimi tümleştirmesi, SQL veri ambarı ile sürekli tümleştirme ve dağıtım işlem hattınızı oluşturmanın ilk adımıdır. 

## <a name="before-you-begin"></a>Başlamadan önce

- [Azure DevOps organizasyonu](https://azure.microsoft.com/services/devops/) için kaydolun
- [Oluşturma ve bağlanma](https://docs.microsoft.com/azure/sql-data-warehouse/create-data-warehouse-portal) öğreticisine gidin
-  [Visual Studio 2019 'yi yükler](https://visualstudio.microsoft.com/vs/older-downloads/) 

## <a name="set-up-and-connect-to-azure-devops"></a>Azure DevOps 'ı ayarlama ve bu sunucuya bağlanma

1. Azure DevOps kuruluşunuzda, bir Azure depo deposu aracılığıyla SSDT veritabanı projenizi barındıracak bir proje oluşturun

   ![Proje oluştur](media/sql-data-warehouse-source-control-integration/1-create-project-azure-devops.png "Proje oluştur")

2. "Bağlantıları Yönet" i seçerek Visual Studio 'Yu açın ve adım 1 ' deki Azure DevOps kuruluşunuza ve projenize bağlanın

   ![Bağlantıları Yönet](media/sql-data-warehouse-source-control-integration/2-manage-connections.png "Bağlantıları Yönet")

   ![Bağlan](media/sql-data-warehouse-source-control-integration/3-connect.png "Bağlan")

3. Azure depoyu deponuzu projenizden yerel makinenize kopyalayın

   ![Depoyu Kopyala](media/sql-data-warehouse-source-control-integration/4-clone-repo.png "Depoyu Kopyala")

## <a name="create-and-connect-your-project"></a>Projenizi oluşturun ve bağlayın

1. Visual Studio 'da **Yerel kopyalanmış deponuzda** bir dizin ve yerel Git deposu ile yeni bir SQL Server veritabanı projesi oluşturun

   ![Yeni proje oluştur](media/sql-data-warehouse-source-control-integration/5-create-new-project.png "Yeni proje oluştur")  

2. Boş sqlprojenize sağ tıklayın ve veri Ambarınızı veritabanı projesine aktarın

   ![Projeyi Içeri aktar](media/sql-data-warehouse-source-control-integration/6-import-new-project.png "Projeyi Içeri aktar")  

3. Visual Studio 'da Takım Gezgini 'nde, tüm değişikliklerinizi yerel git deponuza işleyin 

   ![Yürüt](media/sql-data-warehouse-source-control-integration/6.5-commit-push-changes.png "Yürüt")  

4. Kopyalanan depoda yerel olarak işlenen değişikliklere sahip olduğunuza göre, Azure DevOps projenizdeki Azure depo deponuzda yaptığınız değişiklikleri eşitleyin ve yeniden gönderin.

   ![Eşitleme ve itme hazırlama](media/sql-data-warehouse-source-control-integration/7-commit-push-changes.png "Eşitleme ve itme hazırlama")

   ![Eşitle ve Gönder](media/sql-data-warehouse-source-control-integration/7.5-commit-push-changes.png "Eşitle ve Gönder")  

## <a name="validation"></a>Doğrulama

1. Visual Studio SQL Server Veri Araçları (SSDT) veritabanı projenizdeki tablo sütununu güncelleştirerek Azure deponuzda yapılan değişiklikleri doğrulayın

   ![Güncelleştirme sütununu doğrula](media/sql-data-warehouse-source-control-integration/8-validation-update-column.png "Güncelleştirme sütununu doğrula")

2. Yerel deponuzdaki değişikliği Azure deponuza işleyin ve gönderin

   ![Değişiklikleri Gönder](media/sql-data-warehouse-source-control-integration/9-push-column-change.png "Değişiklikleri Gönder")

3. Azure depo deponuzda değişikliğin yapıldığını doğrulayın

   ![Doğrula](media/sql-data-warehouse-source-control-integration/10-verify-column-change-pushed.png "Değişiklikleri doğrula")

4. (**Isteğe bağlı**) Azure depo deponuzdaki nesne tanımlarının ve yerel deponun veri Ambarınızı yansıttığından emin olmak için, SSDT kullanarak hedef veri ambarınızdaki değişiklikleri güncelleştirme, şema karşılaştırması kullanın

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL veri ambarı için geliştirme](sql-data-warehouse-overview-develop.md)

<!--Image references-->

<!--Article references-->


<!--MSDN references-->

<!--Other Web references-->


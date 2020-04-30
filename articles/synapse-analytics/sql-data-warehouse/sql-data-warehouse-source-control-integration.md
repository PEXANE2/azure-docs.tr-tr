---
title: Kaynak Denetimi Tümleştirmesi
description: Azure Repos (git ve GitHub) kullanarak yerel kaynak denetimi tümleştirmesiyle SQL havuzu için kurumsal sınıf veritabanı DevOps deneyimi.
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 08/23/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 3ec52c5274891619cf7976e99b5241bfc67a4076
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81415093"
---
# <a name="source-control-integration-for-sql-pool"></a>SQL havuzu için kaynak denetimi tümleştirmesi

Bu öğreticide, SQL Server veri araçları (SSDT) veritabanı projenizi kaynak denetimiyle tümleştirme özetlenmektedir.  Kaynak denetimi tümleştirmesi, Azure SYNAPSE Analytics 'te SQL havuzu kaynağıyla sürekli tümleştirme ve dağıtım işlem hattınızı oluşturmanın ilk adımıdır.

## <a name="before-you-begin"></a>Başlamadan önce

- [Azure DevOps organizasyonu](https://azure.microsoft.com/services/devops/) için kaydolun
- [Oluşturma ve bağlanma](create-data-warehouse-portal.md) öğreticisine gidin
- [Visual Studio 2019 'yi yükler](https://visualstudio.microsoft.com/vs/older-downloads/)

## <a name="set-up-and-connect-to-azure-devops"></a>Azure DevOps 'ı ayarlama ve bu sunucuya bağlanma

1. Azure DevOps kuruluşunuzda, bir Azure depo deposu aracılığıyla SSDT veritabanı projenizi barındıracak bir proje oluşturun

   ![Proje oluştur](./media/sql-data-warehouse-source-control-integration/1-create-project-azure-devops.png "Proje oluştur")

2. "Bağlantıları Yönet" i seçerek Visual Studio 'Yu açın ve adım 1 ' deki Azure DevOps kuruluşunuza ve projenize bağlanın

   ![Bağlantıları Yönet](./media/sql-data-warehouse-source-control-integration/2-manage-connections.png "Bağlantıları Yönet")

   ![Bağlan](./media/sql-data-warehouse-source-control-integration/3-connect.png "Bağlan")

3. Azure depoyu deponuzu projenizden yerel makinenize kopyalayın

   ![Depoyu Kopyala](./media/sql-data-warehouse-source-control-integration/4-clone-repo.png "Depoyu Kopyala")

## <a name="create-and-connect-your-project"></a>Projenizi oluşturun ve bağlayın

1. Visual Studio 'da **Yerel kopyalanmış deponuzda** bir dizin ve yerel Git deposu ile yeni bir SQL Server veritabanı projesi oluşturun

   ![Yeni proje oluştur](./media/sql-data-warehouse-source-control-integration/5-create-new-project.png "Yeni proje oluşturma")  

2. Boş sqlprojenize sağ tıklayın ve veri Ambarınızı veritabanı projesine aktarın

   ![Projeyi içeri aktar](./media/sql-data-warehouse-source-control-integration/6-import-new-project.png "Projeyi içeri aktar")  

3. Visual Studio 'da Takım Gezgini 'nde, tüm değişikliklerinizi yerel git deponuza işleyin

   ![İşleme](./media/sql-data-warehouse-source-control-integration/6.5-commit-push-changes.png "İşleme")  

4. Kopyalanan depoda yerel olarak işlenen değişikliklere sahip olduğunuza göre, Azure DevOps projenizdeki Azure depo deponuzda yaptığınız değişiklikleri eşitleyin ve yeniden gönderin.

   ![Eşitleme ve Itme hazırlama](./media/sql-data-warehouse-source-control-integration/7-commit-push-changes.png "Eşitleme ve itme hazırlama")

   ![Eşitle ve Gönder](./media/sql-data-warehouse-source-control-integration/7.5-commit-push-changes.png "Eşitle ve Gönder")  

## <a name="validation"></a>Doğrulama

1. Visual Studio SQL Server Veri Araçları (SSDT) veritabanı projenizdeki tablo sütununu güncelleştirerek Azure deponuzda yapılan değişiklikleri doğrulayın

   ![Güncelleştirme sütununu doğrula](./media/sql-data-warehouse-source-control-integration/8-validation-update-column.png "Güncelleştirme sütununu doğrula")

2. Yerel deponuzdaki değişikliği Azure deponuza işleyin ve gönderin

   ![Değişiklikleri gönderme](./media/sql-data-warehouse-source-control-integration/9-push-column-change.png "Değişiklikleri gönderme")

3. Azure depo deponuzda değişikliğin yapıldığını doğrulayın

   ![Doğrula](./media/sql-data-warehouse-source-control-integration/10-verify-column-change-pushed.png "Değişiklikleri doğrula")

4. (**Isteğe bağlı**) Azure depo deponuzdaki nesne tanımlarının ve yerel deponun veri Ambarınızı yansıttığından emin olmak için, SSDT kullanarak hedef veri ambarınızdaki değişiklikleri güncelleştirme, şema karşılaştırması kullanın

## <a name="next-steps"></a>Sonraki adımlar

- [SQL havuzu için geliştirme geliştirme](sql-data-warehouse-overview-develop.md)

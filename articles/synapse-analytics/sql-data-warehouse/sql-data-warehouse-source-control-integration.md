---
title: Kaynak Denetimi Tümleştirmesi
description: Azure Repos (Git ve GitHub) kullanarak yerel kaynak denetimi tümleştirmesi ile SQL havuzu için kurumsal sınıf Veritabanı DevOps deneyimi.
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
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81415093"
---
# <a name="source-control-integration-for-sql-pool"></a>SQL havuzu için Kaynak Denetim Entegrasyonu

Bu öğretici, SQL Server Data araçları (SSDT) veritabanı projenizi kaynak denetimiyle nasıl tümleştirilene özetler.  Kaynak denetimi tümleştirmesi, Azure Synapse Analytics'teki SQL havuz kaynağıyla sürekli tümleştirme ve dağıtım ardışık alan ınızın oluşturulmasının ilk adımıdır.

## <a name="before-you-begin"></a>Başlamadan önce

- [Azure DevOps kuruluşuna](https://azure.microsoft.com/services/devops/) kaydolun
- [Oluştur ve Bağla](create-data-warehouse-portal.md) öğreticisini gözden geçirin
- [Visual Studio 2019'u Yükle](https://visualstudio.microsoft.com/vs/older-downloads/)

## <a name="set-up-and-connect-to-azure-devops"></a>Azure DevOps'leri ayarlama ve bu tür lere bağlanma

1. Azure DevOps Kuruluşunuzda, Bir Azure Repo deposu aracılığıyla SSDT veritabanı projenizi barındıracak bir proje oluşturun

   ![Proje Oluştur](./media/sql-data-warehouse-source-control-integration/1-create-project-azure-devops.png "Proje Oluştur")

2. Visual Studio'yu açın ve "Bağlantıları Yönet" seçeneğini seçerek 1.

   ![Bağlantıları Yönetme](./media/sql-data-warehouse-source-control-integration/2-manage-connections.png "Bağlantıları Yönetme")

   ![Bağlan](./media/sql-data-warehouse-source-control-integration/3-connect.png "Bağlan")

3. Azure Repo deponuzu projenizden yerel makinenize klonlama

   ![Klon repo](./media/sql-data-warehouse-source-control-integration/4-clone-repo.png "Klon repo")

## <a name="create-and-connect-your-project"></a>Projenizi oluşturun ve bağlayın

1. Visual Studio'da, **yerel klonlanmış deponuzda** hem dizin hem de yerel Git deposu içeren yeni bir SQL Server Veritabanı Projesi oluşturun

   ![Yeni proje oluşturma](./media/sql-data-warehouse-source-control-integration/5-create-new-project.png "Yeni proje oluşturma")  

2. Boş sqlprojenize sağ tıklayın ve veri ambarınızı veritabanı projesine aktarın

   ![İthalat Projesi](./media/sql-data-warehouse-source-control-integration/6-import-new-project.png "İthalat Projesi")  

3. Visual Studio'daki takım gezgininde, tüm değişikliklerinizi yerel Git deponuzda gerçekleştirin

   ![İşleme](./media/sql-data-warehouse-source-control-integration/6.5-commit-push-changes.png "İşleme")  

4. Artık klonlanmış depoda yerel olarak işlenen değişikliklere sahip olduğunuzda, Değişikliklerinizi Azure DevOps projenizde Azure Repo deposunuzda eşitle ve itin.

   ![Eşitleme ve İtme - evreleme](./media/sql-data-warehouse-source-control-integration/7-commit-push-changes.png "Senkronize ve itme - evreleme")

   ![Eşitleme ve İtme](./media/sql-data-warehouse-source-control-integration/7.5-commit-push-changes.png "Senkronize etme ve itme")  

## <a name="validation"></a>Doğrulama

1. Visual Studio SQL Server Data Tools 'dan (SSDT) veritabanı projenizdeki bir tablo sütununu güncelleştirerek değişiklikleri Azure Repo'nuza doğrulayın

   ![Güncelleştirme sütunu doğrulama](./media/sql-data-warehouse-source-control-integration/8-validation-update-column.png "Güncelleştirme sütunu doğrulama")

2. Yerel deponuzdaki değişikliği Azure Repo'nuza işlemeve itme

   ![Değişiklikleri gönderme](./media/sql-data-warehouse-source-control-integration/9-push-column-change.png "Değişiklikleri gönderme")

3. Değişikliğin Azure Repo deponuzda itildiğini doğrulama

   ![Doğrula](./media/sql-data-warehouse-source-control-integration/10-verify-column-change-pushed.png "Değişiklikleri doğrulama")

4. (**İsteğe bağlı**) Azure Repo deposunuzdaki ve yerel deponuzdaki nesne tanımlarının veri ambarınızı yansıttığından emin olmak için SSDT kullanarak Hedef Veri ambarınızdaki değişiklikleri karşılaştır'ı ve güncelleştirin

## <a name="next-steps"></a>Sonraki adımlar

- [SQL havuzu için geliştirme](sql-data-warehouse-overview-develop.md)

---
title: Azure 'da dağıtılmış SSIS paketleriyle dosya açma ve kaydetme
description: Azure 'da yerel dosya sistemlerini kullanan SSIS paketlerini kaldırdığınızda ve bu paketleri Azure 'da nasıl açıp kaydedeceğiniz hakkında bilgi edinin
ms.date: 06/27/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 630a8ead409ad60b9146838f062210a3301b43b5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81760205"
---
# <a name="open-and-save-files-on-premises-and-in-azure-with-ssis-packages-deployed-in-azure"></a>Azure 'da dağıtılmış SSIS paketleri ile şirket içinde ve Azure 'da dosya açma ve kaydetme

Bu makalede, Azure 'da yerel dosya sistemlerini kullanan SSIS paketlerini kaldırdığınızda ve bu paketleri Azure 'da nasıl açıp kaydedeceğiniz açıklanır.

## <a name="save-temporary-files"></a>Geçici dosyaları Kaydet

Tek bir paket yürütmesi sırasında geçici dosyaları depolamanız ve işleyebilmeniz gerekiyorsa, paketler geçerli çalışma dizinini (`.`) veya Azure-SSIS Integration Runtime düğümlerinizin geçici klasörünü (`%TEMP%`) kullanabilir.

## <a name="use-on-premises-file-shares"></a>Şirket içi dosya paylaşımlarını kullanma

Yerel dosya sistemlerini kullanan paketleri Azure 'da yüklerken ve kaydıradığınızda **Şirket içi dosya paylaşımlarını** kullanmaya devam etmek için aşağıdaki işlemleri yapın:

1. Dosyaları yerel dosya sistemlerinden şirket içi dosya paylaşımlarına aktarma.

2. Şirket içi dosya paylaşımlarını bir Azure sanal ağına katın.

3. Azure-SSIS IR aynı sanal ağa katın. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa ekleme](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

4. Windows kimlik doğrulaması kullanan erişim kimlik bilgilerini ayarlayarak Azure-SSIS IR, aynı sanal ağ içindeki şirket içi dosya paylaşımlarına bağlayın. Daha fazla bilgi için bkz. [Windows kimlik doğrulamasıyla veri ve dosya paylaşımlarına bağlanma](ssis-azure-connect-with-windows-auth.md).

5. Paketinizdeki yerel dosya yollarını, şirket içi dosya paylaşımlarını gösteren UNC yollarına güncelleştirin. Örneğin, ' a `C:\abc.txt` güncelleştirin `\\<on-prem-server-name>\<share-name>\abc.txt`.

## <a name="use-azure-file-shares"></a>Azure dosya paylaşımlarını kullanma

Yerel dosya sistemlerini kullanan paketleri Azure 'da yüklerken ve kaydıradığınızda **Azure dosyalarını** kullanmak için aşağıdaki işlemleri yapın:

1. Yerel dosya sistemlerinden Azure dosyalarına dosya aktarma. Daha fazla bilgi için bkz. [Azure dosyaları](https://azure.microsoft.com/services/storage/files/).

2. Windows kimlik doğrulaması kullanan erişim kimlik bilgilerini ayarlayarak Azure-SSIS IR Azure dosyalarına bağlayın. Daha fazla bilgi için bkz. [Windows kimlik doğrulamasıyla veri ve dosya paylaşımlarına bağlanma](ssis-azure-connect-with-windows-auth.md).

3. Paketinizdeki yerel dosya yollarını Azure dosyalarını gösteren UNC yollarına güncelleştirin. Örneğin, ' a `C:\abc.txt` güncelleştirin `\\<storage-account-name>.file.core.windows.net\<share-name>\abc.txt`.

## <a name="next-steps"></a>Sonraki adımlar

- Paketlerinizi dağıtın. Daha fazla bilgi için bkz. [SSMS Ile Azure 'a BIR SSIS projesi dağıtma](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-deploy-ssms).
- Paketlerinizi çalıştırın. Daha fazla bilgi için bkz. [SSMS Ile Azure 'DA SSIS paketleri çalıştırma](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-run-ssms).
- Paketlerinizi zamanlayın. Daha fazla bilgi için bkz. [Azure 'DA SSIS paketlerini zamanlama](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms?view=sql-server-ver15).

---
title: Azure'da dağıtılan SSIS paketleriyle dosyaları açın ve kaydedin
description: Azure'da yerel dosya sistemlerini kullanan SSIS paketlerini kaldırdığınızda ve kaydırdığınızda dosyaları şirket içinde ve Azure'da nasıl açacağınızı ve kaydedin
ms.date: 06/27/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 630a8ead409ad60b9146838f062210a3301b43b5
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81760205"
---
# <a name="open-and-save-files-on-premises-and-in-azure-with-ssis-packages-deployed-in-azure"></a>Azure'da dağıtılan SSIS paketleriyle dosyaları şirket içinde ve Azure'da açın ve kaydedin

Bu makalede, azure'da yerel dosya sistemlerini kullanan SSIS paketlerini kaldırdığınızda ve kaydırdığınızda, dosyaları şirket içinde ve Azure'da nasıl açacağınızı ve kaydedilen açıklar.

## <a name="save-temporary-files"></a>Geçici dosyaları kaydetme

Tek bir paket yürütme sırasında geçici dosyaları depolamanız ve işlemeniz gerekiyorsa,`.`paketler Azure-SSIS Tümleştirme Çalışma zamanı düğümlerinizin geçerli çalışma dizinini ( ) veya geçici klasörünü ()`%TEMP%`kullanabilir.

## <a name="use-on-premises-file-shares"></a>Şirket içi dosya paylaşımlarını kullanma

Azure'da yerel dosya sistemlerini kullanan paketleri kaldırıp kaydırırken **şirket içi dosya paylaşımlarını** kullanmaya devam etmek için aşağıdaki leri yapın:

1. Dosyaları yerel dosya sistemlerinden şirket içi dosya paylaşımlarına aktarın.

2. Bir Azure sanal ağında şirket içi dosya paylaşımlarına katılın.

3. Azure-SSIS IR'nize aynı sanal ağda katılın. Daha fazla bilgi için bkz: [Sanal ağa Azure-SSIS tümleştirme çalışma zamanı katılın.](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)

4. Windows kimlik doğrulamasını kullanan erişim kimlik bilgilerini ayarlayarak Azure-SSIS IR'nizi aynı sanal ağ daki şirket içi dosya paylaşımlarına bağlayın. Daha fazla bilgi için bkz: [Verilere bağlan ve Windows Kimlik Doğrulama ile dosya paylaşımları.](ssis-azure-connect-with-windows-auth.md)

5. Paketlerinizdeki yerel dosya yollarını şirket içi dosya paylaşımlarını gösteren UNC yollarla güncelleştirin. Örneğin, '' `\\<on-prem-server-name>\<share-name>\abc.txt`için güncelleştirin. `C:\abc.txt`

## <a name="use-azure-file-shares"></a>Azure dosya paylaşımlarını kullanma

Azure'da yerel dosya sistemlerini kullanan paketleri kaldırdığınızda ve kaydırırken **Azure Dosyalarını** kullanmak için aşağıdaki leri yapın:

1. Dosyaları yerel dosya sistemlerinden Azure Dosyalarına aktarın. Daha fazla bilgi için [Azure Dosyaları'na](https://azure.microsoft.com/services/storage/files/)bakın.

2. Windows kimlik doğrulamasını kullanan erişim kimlik bilgilerini ayarlayarak Azure-SSIS IR'nizi Azure Dosyalarına bağlayın. Daha fazla bilgi için bkz: [Verilere bağlan ve Windows Kimlik Doğrulama ile dosya paylaşımları.](ssis-azure-connect-with-windows-auth.md)

3. Paketlerinizdeki yerel dosya yollarını Azure Dosyaları'nı işaret eden UNC yollarına güncelleştirin. Örneğin, '' `\\<storage-account-name>.file.core.windows.net\<share-name>\abc.txt`için güncelleştirin. `C:\abc.txt`

## <a name="next-steps"></a>Sonraki adımlar

- Paketlerinizi dağıtın. Daha fazla bilgi için bkz: [SSMS ile Azure'a bir SSIS projesini dağıtın.](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-deploy-ssms)
- Paketlerinizi çalıştırın. Daha fazla bilgi için [Azure'da SSMS ile SSIS paketlerini çalıştır'a](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-run-ssms)bakın.
- Paketlerinizi planlayın. Daha fazla bilgi için [Azure'daki SSIS paketlerini zamanlayın.](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms?view=sql-server-ver15)

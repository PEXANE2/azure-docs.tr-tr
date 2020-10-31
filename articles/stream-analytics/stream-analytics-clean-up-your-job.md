---
title: Azure Stream Analytics işinizi Temizleme
description: Bu makalede Azure Stream Analytics işlerinizi silmeye yönelik farklı yöntemler gösterilmektedir.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 06/21/2019
ms.custom: seodec18
ms.openlocfilehash: 124d2cf4380153bd3ea68de9233f6a1b5eb6ba78
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93130706"
---
# <a name="stop-or-delete-your-azure-stream-analytics-job"></a>Azure Stream Analytics işinizi durdurma veya silme

Azure Stream Analytics işleri Azure portal, Azure PowerShell, .NET için Azure SDK veya REST API aracılığıyla kolayca durdurulabilir veya silinebilir. Bir Stream Analytics işi silindikten sonra kurtarılamaz.

>[!NOTE] 
>Stream Analytics işinizi durdurduğunuzda, veriler yalnızca giriş ve çıkış depolamada (örneğin, Event Hubs veya Azure SQL veritabanı) devam ediyor. Verileri Azure 'dan kaldırmanız gerekiyorsa, Stream Analytics işinizin giriş ve çıkış kaynakları için kaldırma işlemini izlediğinizden emin olun.

## <a name="stop-a-job-in-azure-portal"></a>Azure portal bir işi durdurma

Bir işi durdurduğunuzda, kaynakların sağlanması kaldırılır ve olayları işlemeyi durdurur. Bu işle ilgili ücretler de durdurulur. Ancak tüm yapılandırmanız korunur ve işi daha sonra yeniden başlatabilirsiniz 

1. [Azure portalında](https://portal.azure.com) oturum açın. 

2. Çalışan Stream Analytics işinizi bulun ve seçin.

3. İşi durdurmak için Stream Analytics işi sayfasında **Durdur** ' u seçin. 

   ![Azure Stream Analytics işini durdur](./media/stream-analytics-clean-up-your-job/stop-stream-analytics-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Azure portal bir işi silme

>[!WARNING] 
>Bir Stream Analytics işi silindikten sonra kurtarılamaz.

1. Azure portalında oturum açın. 

2. Mevcut Stream Analytics işinizi bulun ve seçin.

3. İşi silmek için Stream Analytics işi sayfasında **Sil** ' i seçin. 

   ![Azure Stream Analytics Işini Sil](./media/stream-analytics-clean-up-your-job/delete-stream-analytics-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>PowerShell 'i kullanarak bir işi durdurma veya silme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell 'i kullanarak bir işi durdurmak için [stop-AzStreamAnalyticsJob](/powershell/module/az.streamanalytics/stop-azstreamanalyticsjob) cmdlet 'ini kullanın. PowerShell 'i kullanarak bir işi silmek için [Remove-AzStreamAnalyticsJob](/powershell/module/az.streamanalytics/Remove-azStreamAnalyticsJob) cmdlet 'ini kullanın.

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>.NET için Azure SDK 'Yı kullanarak bir işi durdurma veya silme

.NET için Azure SDK 'yı kullanarak bir işi durdurmak için [Streamingjobsoperationsextensions. BeginStop](/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet) metodunu kullanın. .NET için Azure SDK, [Streamingjobsoperationsextensions. BeginDelete](/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet) yöntemi kullanarak bir işi silmek için.

## <a name="stop-or-delete-a-job-using-rest-api"></a>REST API kullanarak bir işi durdurma veya silme

REST API kullanarak bir işi durdurmak için [stop](/powershell/module/azurerm.streamanalytics/stop-azurermstreamanalyticsjob#stop) yöntemine bakın. REST API kullanarak bir işi silmek için, [Delete](/powershell/module/azurerm.streamanalytics/stop-azurermstreamanalyticsjob#delete) yöntemine bakın.
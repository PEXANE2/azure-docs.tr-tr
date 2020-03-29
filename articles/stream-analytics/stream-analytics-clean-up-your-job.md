---
title: Azure Akış Analizi işinizi temizleyin
description: Bu makalede, Azure Akış Analizi işlerinizi silemeniz için farklı yöntemler gösterilmektedir.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.custom: seodec18
ms.openlocfilehash: d99920417f20034da1001a821c02376ac19274d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426480"
---
# <a name="stop-or-delete-your-azure-stream-analytics-job"></a>Azure Akış Analizi işinizi durdurun veya silin

Azure Akışı Analizi işleri, Azure portalı, Azure PowerShell, .Net için Azure SDK veya REST API üzerinden kolayca durdurulabilir veya silinebilir. Bir Stream Analytics işi silindikten sonra kurtarılamaz.

>[!NOTE] 
>Akış Analizi işinizi durdurduğunuzda, veriler yalnızca Olay Hub'ları veya Azure SQL Veritabanı gibi giriş ve çıktı depolamasında kalır. Azure'dan veri kaldırmanız gerekiyorsa, Akış Analizi işinizin giriş ve çıktı kaynakları için kaldırma işlemini izlediğinizden emin olun.

## <a name="stop-a-job-in-azure-portal"></a>Azure portalındaki bir işi durdurma

Bir işi durdurduğınızda, kaynaklar deprovisioned ve olayları işleme durur. Bu işle ilgili ücretler de durdurulur. Ancak tüm yapılandırma tutulur ve daha sonra işi yeniden başlatabilirsiniz 

1. [Azure portalında](https://portal.azure.com)oturum açın. 

2. Çalışan Stream Analytics işinizi bulun ve seçin.

3. Akış Analizi iş sayfasında, işi durdurmak için **Durdur'u** seçin. 

   ![Azure Akışı Analizi işini durdurun](./media/stream-analytics-clean-up-your-job/stop-stream-analytics-job.png)


## <a name="delete-a-job-in-azure-portal"></a>Azure portalındaki bir işi silme

>[!WARNING] 
>Bir Stream Analytics işi silindikten sonra kurtarılamaz.

1. Azure Portal’da oturum açın. 

2. Mevcut Stream Analytics işinizi bulun ve seçin.

3. Akış Analizi iş sayfasında, işi silmek için **Sil'i** seçin. 

   ![Azure Akışı Analizi İşini Sil](./media/stream-analytics-clean-up-your-job/delete-stream-analytics-job.png)


## <a name="stop-or-delete-a-job-using-powershell"></a>PowerShell'i kullanarak bir işi durdurma veya silme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

PowerShell'i kullanarak bir işi durdurmak için [Stop-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/stop-azstreamanalyticsjob) cmdlet'i kullanın. PowerShell kullanarak bir işi silmek için [Remove-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/Remove-azStreamAnalyticsJob) cmdlet'ini kullanın.

## <a name="stop-or-delete-a-job-using-azure-sdk-for-net"></a>.NET için Azure SDK kullanarak bir işi durdurma veya silme

.NET için Azure SDK'yı kullanan bir işi durdurmak için [StreamingJobsOperationsExtensions.BeginStop](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.beginstop?view=azure-dotnet) yöntemini kullanın. .NET için Azure SDK kullanarak bir işi silmek [için, StreamingJobsOperationsExtensions.BeginDelete](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.streamingjobsoperationsextensions.begindelete?view=azure-dotnet) yöntemi.

## <a name="stop-or-delete-a-job-using-rest-api"></a>REST API'yi kullanarak bir işi durdurma veya silme

REST API'yi kullanarak bir işi durdurmak için [Durdur](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#stop) metoduna bakın. REST API kullanarak bir işi silmek için [Sil](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job#delete) yöntemine bakın.

---
title: Azure Stream Analytics için sürekli tümleştirme ve dağıtım
description: Bu makale, Azure Stream Analytics için bir sürekli tümleştirme ve dağıtım (CI/CD) işlem hattına genel bir bakış sunar.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 9/22/2020
ms.openlocfilehash: b5057eb8c84e839f504060228986ea759c8bdc3d
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93123176"
---
# <a name="continuous-integration-and-deployment-cicd-for-azure-stream-analytics"></a>Azure Stream Analytics için sürekli tümleştirme ve dağıtım (CI/CD)

Azure Stream Analytics işinizi, kaynak denetimi tümleştirmesini kullanarak sürekli olarak dağıtabilirsiniz. Kaynak denetimi tümleştirmesi, bir kod güncelleştirmesinin Azure 'a kaynak dağıtımını tetiklediği bir iş akışını sağlar. Bu makalede bir sürekli tümleştirme ve dağıtım (CI/CD) işlem hattı oluşturmaya yönelik temel adımlar özetlenmektedir.

Azure Stream Analytics yeni başladıysanız [Azure Stream Analytics hızlı başlangıç](stream-analytics-quick-create-portal.md)ile çalışmaya başlayın.

## <a name="create-a-cicd-pipeline"></a>CI/CD işlem hattı oluşturma

Stream Analytics için bir CI/CD işlem hattı oluşturmak için bu kılavuzdaki adımları izleyin.

1. Azure Stream Analytics bir sorgu geliştirin.

   [Sorguları yerel olarak geliştirmek ve test](develop-locally.md)etmek için [Visual Studio Code](./quick-create-visual-studio-code.md) veya [Visual Studio](stream-analytics-quick-create-vs.md) için Azure Stream Analytics araçları kullanın. Ayrıca, [var olan bir işi](visual-studio-code-explore-jobs.md#export-a-job-to-a-local-project) yerel bir projeye dışarı aktarabilirsiniz.

2. Azure Stream Analytics projelerinizi bir git deposu gibi kaynak denetim sisteminize işleyin.

3. Projeleri derlemek ve dağıtım için Azure Kaynak Yönetimi şablonları oluşturmak üzere [Azure Stream ANALYTICS CI/CD araçları](cicd-tools.md) kullanın.

4. Kalite gerileme için [Otomatikleştirilmiş betik testlerini](cicd-tools.md#automated-test) çalıştırın.

5. [İşi](cicd-tools.md#deploy-to-azure) otomatik olarak Azure 'a dağıtın.

## <a name="auto-build-test-and-deploy"></a>Otomatik derleme, test etme ve dağıtma

Otomatik olarak derlemek, test etmek ve dağıtmak için komut satırı ve [Azure Stream ANALYTICS CI/CD araçları](cicd-tools.md) ' nı kullanabilirsiniz. Ayrıca, [Azure Pipelines](set-up-cicd-pipeline.md)' de bir CI/CD işlem hattı da ayarlayabilirsiniz. İşlem hattı yönetimi, görselleştirme ve tetikleyiciler gibi daha gelişmiş özellikleri etkinleştirmek için Azure Pipelines.

## <a name="next-steps"></a>Sonraki adımlar

* [CI/CD araçları kullanarak Azure Stream Analytics işin derlemelerini, testlerini ve dağıtımlarını otomatikleştirin](cicd-tools.md)
* [Azure Pipelines kullanarak Stream Analytics işi için bir CI/CD işlem hattı ayarlama](set-up-cicd-pipeline.md)
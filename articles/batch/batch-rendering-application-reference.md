---
title: Render uygulamalarını kullanma - Azure Toplu İş
description: Azure Toplu İşile oluşturma uygulamaları nasıl kullanılır? Bu makalede, her render uygulaması çalıştırmak için nasıl kısa bir açıklama sağlar.
services: batch
ms.service: batch
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: conceptual
ms.openlocfilehash: dc0ce23c90a4ba6575ba26b37d97f94ba8fa1f63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75390477"
---
# <a name="rendering-applications"></a>Uygulamaları oluşturma

Renderuygulamaları Toplu iş ve görevler oluşturarak kullanılır. Görev komut satırı özelliği, uygun komut satırı ve parametreleri belirtir.  İş görevlerini oluşturmanın en kolay [yolu, bu makalede](https://docs.microsoft.com/azure/batch/batch-rendering-using#using-batch-explorer)belirtildiği gibi Toplu İş Gezgini şablonlarını kullanmaktır.  Şablonlar görüntülenebilir ve gerekirse değiştirilmiş sürümler oluşturulabilir.

Bu makalede, her render uygulaması çalıştırmak için nasıl kısa bir açıklama sağlar.

## <a name="rendering-with-autodesk-3ds-max"></a>Autodesk 3ds Max ile oluşturma

### <a name="renderer-support"></a>Renderer desteği

3ds Max yerleşik render ek olarak, aşağıdaki render vm görüntüleri üzerinde kullanılabilir ve 3ds Max sahne dosyası tarafından başvurulabilir:

* Autodesk Arnold
* Kaos Grubu V-Ray

### <a name="task-command-line"></a>Görev komut satırı

Bir `3dsmaxcmdio.exe` havuz düğümünde komut satırı oluşturma gerçekleştirmek için uygulamayı çağırın.  Bu uygulama, görev çalıştırıldığında yoldadır. Uygulama `3dsmaxcmdio.exe` `3dsmaxcmd.exe` [3ds Max yardım belgelerinde](https://help.autodesk.com/view/3DSMAX/2018/ENU/) belgelenen uygulama ile aynı kullanılabilir parametrelere sahiptir (Rendering | Komut Satırı Oluşturma bölümü).

Örnek:

```
3dsmaxcmdio.exe -v:5 -rfw:0 -start:{0} -end:{0} -bitmapPath:"%AZ_BATCH_JOB_PREP_WORKING_DIR%\sceneassets\images" -outputName:dragon.jpg -w:1280 -h:720 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scenes\dragon.max"
```

Notlar:

* Varlık dosyalarının bulunduğundan emin olmak için büyük özen gerekir.  **Varlıkların İzleme** penceresini kullanarak yolların doğru ve göreceli `-bitmapPath` olduğundan emin olun veya komut satırındaki parametreyi kullanın.
* Görev çalıştırıldığında 3ds Max tarafından yazılan dosyayı `stdout.txt` denetleyerek varlıkları bulameme gibi işleme yle ilgili sorunlar olup olmadığına bakın.

### <a name="batch-explorer-templates"></a>Toplu Gezgin şablonları

Havuz ve iş şablonlarına Toplu Gezgin'deki **Galeri'den** erişilebilir.  Şablon kaynak dosyaları [GitHub'daki Toplu Gezgin veri deposunda](https://github.com/Azure/BatchExplorer-data/tree/master/ncj/3dsmax)mevcuttur.

## <a name="rendering-with-autodesk-maya"></a>Autodesk Maya ile Rendering

### <a name="renderer-support"></a>Renderer desteği

Maya yerleşik render ek olarak, aşağıdaki render vm görüntüleri üzerinde kullanılabilir ve 3ds Max sahne dosyası tarafından başvurulabilir:

* Autodesk Arnold
* Kaos Grubu V-Ray

### <a name="task-command-line"></a>Görev komut satırı

`renderer.exe` Komut satırı işleyicisi görev komut satırında kullanılır. Komut satırı [işleyicisi Maya yardım](https://help.autodesk.com/view/MAYAUL/2018/ENU/?guid=GUID-EB558BC0-5C2B-439C-9B00-F97BCB9688E4)belgelenmiştir.

Aşağıdaki örnekte, sahne dosyalarını ve varlıklarını iş hazırlama çalışma dizinine kopyalamak için bir iş hazırlama görevi kullanılır, işoluşturma görüntüsünü depolamak için bir çıktı klasörü kullanılır ve çerçeve 10 işlenir.

```
render -renderer sw -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

V-Ray işleme için, Maya sahne dosyası normalde renderer olarak V-Ray belirtecek.  Komut satırında da belirtilebilir:

```
render -renderer vray -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

Arnold render için, Maya sahne dosyası normalde renderer olarak Arnold belirtecek.  Komut satırında da belirtilebilir:

```
render -renderer arnold -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

### <a name="batch-explorer-templates"></a>Toplu Gezgin şablonları

Havuz ve iş şablonlarına Toplu Gezgin'deki **Galeri'den** erişilebilir.  Şablon kaynak dosyaları [GitHub'daki Toplu Gezgin veri deposunda](https://github.com/Azure/BatchExplorer-data/tree/master/ncj/maya)mevcuttur.

## <a name="next-steps"></a>Sonraki adımlar

Toplu Gezgini'ni kullanarak [GitHub'daki veri deposundaki](https://github.com/Azure/BatchExplorer-data/tree/master/ncj) havuz ve iş şablonlarını kullanın.  Gerektiğinde, yeni şablonlar oluşturun veya sağlanan şablonlardan birini değiştirin.
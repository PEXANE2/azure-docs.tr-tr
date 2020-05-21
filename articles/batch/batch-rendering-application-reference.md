---
title: İşleme uygulamaları kullanma
description: Azure Batch ile uygulama işleme nasıl kullanılır. Bu makalede, her bir işleme uygulamasının nasıl çalıştırılacağı hakkında kısa bir açıklama sunulmaktadır.
author: mscurrell
ms.author: markscu
ms.date: 08/02/2018
ms.topic: how-to
ms.openlocfilehash: 21ae6e0a190875e3e541eb858ec38658ce191ee6
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83726511"
---
# <a name="rendering-applications"></a>Uygulamaları işleme

İşleme uygulamaları toplu işler ve görevler oluşturularak kullanılır. Görev komut satırı özelliği uygun komut satırını ve parametreleri belirtir.  İş görevlerini oluşturmanın en kolay yolu, [Bu makalede](https://docs.microsoft.com/azure/batch/batch-rendering-using#using-batch-explorer)belirtildiği gibi Batch Explorer şablonlarını kullanmaktır.  Şablonlar görüntülenebilir ve gerekirse oluşturulan sürümler değiştirilebilir.

Bu makalede, her bir işleme uygulamasının nasıl çalıştırılacağı hakkında kısa bir açıklama sunulmaktadır.

## <a name="rendering-with-autodesk-3ds-max"></a>Autodesk 3ds Max ile işleme

### <a name="renderer-support"></a>İşleyici desteği

3ds Max ile oluşturulan oluşturuculara ek olarak, aşağıdaki işleyiciler işleme VM görüntülerinde kullanılabilir ve 3ds Max sahne dosyası tarafından başvurulabilirler:

* Autodesk Arnold
* Chaos Group V-Ray

### <a name="task-command-line"></a>Görev komut satırı

`3dsmaxcmdio.exe`Bir havuz düğümünde komut satırı oluşturmayı gerçekleştirmek için uygulamayı çağırın.  Bu uygulama, görev çalıştırıldığında yol üzerinde bulunur. `3dsmaxcmdio.exe`Uygulama, uygulama ile aynı kullanılabilir parametrelere sahiptir `3dsmaxcmd.exe` ve bu, [3ds Max yardım belgelerinde](https://help.autodesk.com/view/3DSMAX/2018/ENU/) (işleme |) belgelenmiştir. Komut satırı Işleme bölümü).

Örnek:

```
3dsmaxcmdio.exe -v:5 -rfw:0 -start:{0} -end:{0} -bitmapPath:"%AZ_BATCH_JOB_PREP_WORKING_DIR%\sceneassets\images" -outputName:dragon.jpg -w:1280 -h:720 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scenes\dragon.max"
```

Notlar:

* Varlık dosyalarının bulunduğundan emin olmak için harika bir ilginin alınması gerekir.  Yolların doğru ve göreli olduğundan, **varlık izleme** penceresini kullanarak veya `-bitmapPath` komut satırındaki parametresini kullandığınızdan emin olun.
* İşle ilgili bir sorun olup olmadığını, örneğin varlık bulma gibi, bir `stdout.txt` görev çalıştırıldığında 3ds Max ile yazılmış dosyayı denetleyerek, bkz..

### <a name="batch-explorer-templates"></a>Batch Explorer şablonları

Havuz ve iş şablonlarına Batch Explorer **galerisinden** bir şekilde erişilebilir.  Şablon kaynak dosyaları, [GitHub 'daki Batch Explorer veri deposunda](https://github.com/Azure/BatchExplorer-data/tree/master/ncj/3dsmax)bulunur.

## <a name="rendering-with-autodesk-maya"></a>Autodesk Maya ile işleme

### <a name="renderer-support"></a>İşleyici desteği

Maya içinde yerleşik olan oluşturuculara ek olarak, aşağıdaki işleyiciler işleme VM görüntülerinde kullanılabilir ve 3ds Max sahne dosyası tarafından başvurulabilirler:

* Autodesk Arnold
* Chaos Group V-Ray

### <a name="task-command-line"></a>Görev komut satırı

`renderer.exe`Komut satırı işleyicisi görev komut satırında kullanılır. Komut satırı işleyicisi [Maya yardımı](https://help.autodesk.com/view/MAYAUL/2018/ENU/?guid=GUID-EB558BC0-5C2B-439C-9B00-F97BCB9688E4)'nda belgelenmiştir.

Aşağıdaki örnekte, sahne dosyalarını ve varlıklarını iş hazırlama çalışma dizinine kopyalamak için bir iş hazırlama görevi kullanılır, işleme görüntüsünü depolamak için bir çıkış klasörü kullanılır ve kare 10 işlenir.

```
render -renderer sw -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

V-Ray işleme için, Maya sahne dosyası normalde işleyici olarak V-Ray ' i belirtir.  Komut satırında de belirlenebilir:

```
render -renderer vray -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

Arnold işleme için, Maya sahne dosyası normalde işleyici olarak Arnold olarak belirtilebilir.  Komut satırında de belirlenebilir:

```
render -renderer arnold -proj "%AZ_BATCH_JOB_PREP_WORKING_DIR%" -verb -rd "%AZ_BATCH_TASK_WORKING_DIR%\output" -s 10 -e 10 -x 1920 -y 1080 "%AZ_BATCH_JOB_PREP_WORKING_DIR%\scene-file.ma"
```

### <a name="batch-explorer-templates"></a>Batch Explorer şablonları

Havuz ve iş şablonlarına Batch Explorer **galerisinden** bir şekilde erişilebilir.  Şablon kaynak dosyaları, [GitHub 'daki Batch Explorer veri deposunda](https://github.com/Azure/BatchExplorer-data/tree/master/ncj/maya)bulunur.

## <a name="next-steps"></a>Sonraki adımlar

Batch Explorer kullanarak [GitHub 'daki veri deposundan](https://github.com/Azure/BatchExplorer-data/tree/master/ncj) havuz ve iş şablonlarını kullanın.  Gerektiğinde yeni şablonlar oluşturun veya sağlanan şablonlardan birini değiştirin.
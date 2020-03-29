---
title: PowerShell'i kullanarak verileri bulutta dönüştürme
description: Bu PowerShell komut dosyası, Bir Azure HDInsight Spark kümesinde Spark programını çalıştırarak buluttaki verileri dönüştürür.
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 09/12/2017
ms.openlocfilehash: c09d0532b845472d0ccaac1ad57e3772630bb5c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74932060"
---
# <a name="powershell-script---transform-data-in-cloud-using-azure-data-factory"></a>PowerShell komut dosyası - Azure Veri Fabrikası'nı kullanarak verileri bulutta dönüştürme

Bu örnek PowerShell komut dosyası, Bir Azure HDInsight Spark kümesinde Spark programını çalıştırarak buluttaki verileri dönüştüren bir ardışık işlem hattı oluşturur. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

## <a name="prerequisites"></a>Ön koşullar
* **Azure Depolama hesabı.** Bir python komut dosyası ve bir giriş dosyası oluşturun ve bunları Azure depolama alanına yükleyin. Spark programının çıktısı bu depolama hesabında depolanır. İsteğe bağlı Spark kümesi, birincil depolama alanıyla aynı depolama hesabını kullanır.  

### <a name="upload-python-script-to-your-blob-storage-account"></a>Python betiğini Blob Depolama hesabınıza yükleme
1. aşağıdaki içeriği içeren **WordCount_Spark.py** adlı bir python dosyası oluşturun: 

    ```python
    import sys
    from operator import add
    
    from pyspark.sql import SparkSession
    
    def main():
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
            
        lines = spark.read.text("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/inputfiles/minecraftstory.txt").rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' ')) \
            .map(lambda x: (x, 1)) \
            .reduceByKey(add)
        counts.saveAsTextFile("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/outputfiles/wordcount")
        
        spark.stop()
    
    if __name__ == "__main__":
        main()
    ```
2. ** &lt;&gt; storageAccountName'i** Azure Depolama hesabınızın adıyla değiştirin. Ardından dosyayı kaydedin. 
3. Azure Blob depolama alanınızda henüz yoksa **adftutorial** adlı bir kapsayıcı oluşturun. 
4. **Spark** adlı bir klasör oluşturun.
5. **Spark** klasörünün altında **script** adlı bir alt klasör oluşturun. 
6. **WordCount_Spark.py** dosyasını **script** alt klasörüne yükleyin. 


### <a name="upload-the-input-file"></a>Girdi dosyasını yükleme
1. Bazı metinlerle **minecraftstory.txt** adlı bir dosya oluşturun. Spark programı bu metindeki sözcükleri sayar. 
2. Blob kapsayıcısının `inputfiles` klasöründe `spark` adında bir alt klasör oluşturun. 
3. `minecraftstory.txt` dosyasını `inputfiles` alt klasörüne yükleyin. 

## <a name="sample-script"></a>Örnek betik
> [!IMPORTANT]
> Bu komut dosyası, c:\ 'daki sabit diskinizde Veri Fabrikası varlıklarını (bağlantılı hizmet, veri kümesi ve ardışık) tanımlayan JSON dosyaları oluşturur: \ Klasör.

[!code-powershell[main](../../../powershell_scripts/data-factory/transform-data-using-spark/transform-data-using-spark.ps1 "Transform data using Spark")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

Örnek komut dosyasını çalıştırdıktan sonra, kaynak grubunu ve onunla ilişkili tüm kaynakları kaldırmak için aşağıdaki komutu kullanabilirsiniz:

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
```
Veri fabrikasını kaynak grubundan kaldırmak için aşağıdaki komutu çalıştırın: 

```powershell
Remove-AzDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="script-explanation"></a>Betik açıklaması

Bu betik şu komutları kullanır:

| Komut | Notlar |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Tüm kaynakların depolandığı bir kaynak grubu oluşturur. |
| [Set-AzDataFactoryV2](/powershell/module/az.datafactory/set-Azdatafactoryv2) | Veri fabrikası oluşturma. |
| [Set-AzDataFactoryV2LinkedService](/powershell/module/az.datafactory/set-Azdatafactoryv2linkedservice) | Veri fabrikasında bağlantılı bir hizmet oluşturur. Bağlantılı bir hizmet, bir veri deposuveya bilgi işlem bağlantısını bir veri fabrikasına bağlar. |
| [Set-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/set-Azdatafactoryv2pipeline) | Veri fabrikasında bir ardışık hat lar oluşturur. Bir ardışık işlem, belirli bir işlemi gerçekleştiren bir veya daha fazla etkinlik içerir. Bu ardışık işlemde, bir kıvılcım etkinliği, bir Azure HDInsight Spark kümesinde bir program çalıştırarak verileri dönüştürür. |
| [Invoke-AzDataFactoryV2Pipeline](/powershell/module/az.datafactory/invoke-Azdatafactoryv2pipeline) | Boru hattı için bir çalışma oluşturur. Başka bir deyişle, boru hattını çalıştırın. |
| [Get-AzDataFactoryV2ActivityRun](/powershell/module/az.datafactory/get-Azdatafactoryv2activityrun) | Ardışık ardışık ardışık alanda etkinlik (etkinlik çalışması) çalıştırıla ilgili ayrıntıları alır. 
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Bir kaynak grubunu tüm iç içe geçmiş kaynaklar dahil siler. |
|||

## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](https://docs.microsoft.com/powershell/).

Ek Azure Veri Fabrikası PowerShell komut dosyası örnekleri [Azure Veri Fabrikası PowerShell örneklerinde](../samples-powershell.md)bulunabilir.

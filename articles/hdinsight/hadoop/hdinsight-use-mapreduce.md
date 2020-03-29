---
title: HDInsight'ta Apache Hadoop ile MapReduce
description: HDInsight kümelerinde Apache Hadoop'taki Apache MapReduce işlerini nasıl çalıştıracağınızhakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/06/2019
ms.openlocfilehash: 238f37cc1e8cbc8d3260693181d6e54ac5f592f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435716"
---
# <a name="use-mapreduce-in-apache-hadoop-on-hdinsight"></a>HDInsight üzerinde Apache Hadoop ile MapReduce'u kullanma

HDInsight kümelerinde MapReduce işlerini nasıl çalıştırılayarıştiz öğrenin.

## <a name="example-data"></a>Örnek veriler

HDInsight, dizinde `/example/data` ve `/HdiSamples` dizinde depolanan çeşitli örnek veri kümeleri sağlar. Bu dizinler kümeniz için varsayılan depolama alanındadır. Bu belgede, dosyayı `/example/data/gutenberg/davinci.txt` kullanırız. Bu dosyada Leonardo da Vinci'nin defterleri var.

## <a name="example-mapreduce"></a>Örnek MapReduce

Örnek bir MapReduce kelime sayısı uygulaması HDInsight kümenize dahildir. Bu örnek, kümeniz için varsayılan depolama `/example/jars/hadoop-mapreduce-examples.jar` yerinde bulunur.

Aşağıdaki Java kodu, dosyada bulunan MapReduce `hadoop-mapreduce-examples.jar` uygulamasının kaynağıdır:

```java
package org.apache.hadoop.examples;

import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class WordCount {

    public static class TokenizerMapper
        extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
        StringTokenizer itr = new StringTokenizer(value.toString());
        while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
    }
    }

    public static class IntSumReducer
        extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                        Context context
                        ) throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
        sum += val.get();
        }
        result.set(sum);
        context.write(key, result);
    }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
        System.err.println("Usage: wordcount <in> <out>");
        System.exit(2);
    }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

Kendi MapReduce uygulamalarınızı yazmak için talimatlar [için, HDInsight için Java MapReduce uygulamalarını geliştir'e](apache-hadoop-develop-deploy-java-mapreduce-linux.md)bakın.

## <a name="run-the-mapreduce"></a>MapReduce çalıştırın

HDInsight çeşitli yöntemler kullanarak HiveQL işleri çalıştırabilirsiniz. Hangi yöntemin sizin için doğru olduğuna karar vermek için aşağıdaki tabloyu kullanın ve ardından bir gözden geçirme için bağlantıyı izleyin.

| **Bunu kullanın**... | **... bunu yapmak için** |  ... bu **istemci işletim sisteminden** |
|:--- |:--- |:--- |:--- |
| [Ssh](apache-hadoop-use-mapreduce-ssh.md) |**SSH** üzerinden Hadoop komutunu kullanın |Linux, Unix, Mac OS X veya Windows |
| [Curl](apache-hadoop-use-mapreduce-curl.md) |**REST'i** kullanarak işi uzaktan gönderme |Linux, Unix, Mac OS X veya Windows |
| [Windows PowerShell](apache-hadoop-use-mapreduce-powershell.md) |**Windows PowerShell'i** kullanarak işi uzaktan gönderme  |Windows |

## <a name="next-steps"></a>Sonraki adımlar

HDInsight'ta verilerle çalışma hakkında daha fazla bilgi edinmek için aşağıdaki belgelere bakın:

* [HDInsight için Java MapReduce programları geliştirin](apache-hadoop-develop-deploy-java-mapreduce-linux.md)

* [HDInsight ile Apache Hive'ı kullanma](./hdinsight-use-hive.md)

---
title: FASTQ dosya girişlerini kullanarak iş akışı gönderme
titleSuffix: Microsoft Genomics
description: Bu makalede, giriş dosyalarınız tek bir FASTQ dosyası çiftinde olduğunda Microsoft Genomiks hizmetine bir iş akışı gönderme gösterilmektedir.
services: genomics
author: grhuynh
manager: cgronlun
ms.author: grhuynh
ms.service: genomics
ms.topic: conceptual
ms.date: 12/07/2017
ms.openlocfilehash: 3806b165e5abb661e53c6a315650d025fd42e17f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "72248555"
---
# <a name="submit-a-workflow-using-fastq-file-inputs-in-microsoft-genomics"></a>Microsoft Genomiks’te FASTQ dosyası girişlerini kullanarak iş akışı gönderme

Bu makalede, giriş dosyalarınız tek bir FASTQ dosyası çiftinde olduğunda Microsoft Genomiks hizmetine bir iş akışı gönderme gösterilmektedir. Bu konu başlığında `msgen` istemcisini yükleyip çalıştırdığınız ve Azure Depolama konusunda bilgi sahibi olduğunuz kabul edilmektedir. Belirtilen örnek verileri kullanarak bir iş akışını başarıyla gönderdiyseniz, bu makaleye devam etmeye hazırsınızdır. 

## <a name="set-up-upload-your-fastq-files-to-azure-storage"></a>Kurulum: FASTQ dosyalarınızı Azure depolamaya yükleme
*reads_1.fq.gz* ve *reads_2.fq.gz* olmak üzere iki dosyaya sahip olduğunuzu ve bunları *myaccount* adlı Azure depolama hesabınıza **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/inputs/reads_1<span></span>.fq<span></span>.gz<span></span>** ve **https://<span></span>myaccount.blob.core.<span></span>windows<span></span>.net/<span></span>inputs/<span></span>reads_2.fq<span></span>.gz<span></span>** olarak yüklediğinizi düşünelim. API URL'sine ve erişim anahtarına sahipsiniz. **https://<span></span>myaccount.blob.core<span></span>.windows<span></span>.net<span></span>/outputs<span></span>** içinde iki çıkış olmasını istiyorsunuz.


## <a name="submit-your-job-to-the-msgen-client"></a>İşinizi `msgen` istemcisine gönderme 

Burada `msgen` istemcisine sağlamanız gereken minimum bağımsız değişkenler verilmiştir; kodun daha anlaşılır olması için satır sonları eklenmiştir:

Windows için:

```
msgen submit ^
  --api-url-base <Genomics API URL> ^
  --access-key <Genomics access key> ^
  --process-args R=b37m1 ^
  --input-storage-account-name myaccount ^
  --input-storage-account-key <storage access key to "myaccount"> ^
  --input-storage-account-container inputs ^
  --input-blob-name-1 reads_1.fq.gz ^
  --input-blob-name-2 reads_2.fq.gz ^
  --output-storage-account-name myaccount ^
  --output-storage-account-key <storage access key to "myaccount"> ^
  --output-storage-account-container outputs
```

Unix için:

```
msgen submit \
  --api-url-base <Genomics API URL> \
  --access-key <Genomics access key> \
  --process-args R=b37m1 \
  --input-storage-account-name myaccount \
  --input-storage-account-key <storage access key to "myaccount"> \
  --input-storage-account-container inputs \
  --input-blob-name-1 reads_1.fq.gz \
  --input-blob-name-2 reads_2.fq.gz \
  --output-storage-account-name myaccount \
  --output-storage-account-key <storage access key to "myaccount"> \
  --output-storage-account-container outputs
```


Yapılandırma dosyası kullanmayı tercih ediyorsanız şu bileşenleri dahil etmeniz gerekir:

```
api_url_base:                     <Genomics API URL>
access_key:                       <Genomics access key>
process_args:                     R=b37m1
input_storage_account_name:       myaccount
input_storage_account_key:        <storage access key to "myaccount">
input_storage_account_container:  inputs
input_blob_name_1:                reads_1.fq.gz
input_blob_name_2:                reads_2.fq.gz
output_storage_account_name:      myaccount
output_storage_account_key:       <storage access key to "myaccount">
output_storage_account_container: outputs
```

`config.txt` dosyasını şu çağrıyla gönderin: `msgen submit -f config.txt`

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede FASTQ dosyaları çiftini Azure Depolama'ya yükleyip `msgen` python istemcisi üzerinden Microsoft Genomiks hizmetine bir iş akışı gönderdiniz. İş akışı gönderme ve Microsoft Genomiks hizmetiyle kullanabileceğiniz diğer komutlar hakkında daha fazla bilgi edinmek için bkz. [SSS](frequently-asked-questions-genomics.md). 

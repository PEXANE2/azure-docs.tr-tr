---
title: include dosyası
description: include dosyası
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/31/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: ebcbe7e8af7e01a9b7e75282be4e4f4ceb80b806
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76279561"
---
## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar.

1. İşlevinizi test etmek için işlev kodunda bir kesme noktası ayarlayın ve işlev uygulaması projesini başlatmak için F5 tuşuna basın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

1. **Terminal** panelinde, HTTP ile tetiklenen işlevinizin URL uç noktasını kopyalayın. 

    ![Azure yerel çıktısı](./media/functions-run-function-test-local-vs-code/functions-vscode-f5.png)

1. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. `?name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütün. Kesme noktasına gelindiğinde yürütme duraklatılır.

1. Yürütmeyi sürdürdüğünüzde GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir:

    ![Tarayıcıdaki işlev localhost yanıtı](./media/functions-run-function-test-local-vs-code/functions-test-local-browser.png)

1. Hata ayıklamayı durdurmak için Shift + F5 tuşuna basın.

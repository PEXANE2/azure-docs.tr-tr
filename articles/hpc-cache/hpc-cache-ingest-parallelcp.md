---
title: Azure HPC önbelleği veri alma-paralel kopya betiği
description: Azure HPC önbelleğinde bir BLOB depolama hedefine veri taşımak için paralel kopyalama betiği kullanma
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 10/30/2019
ms.author: v-erkel
ms.openlocfilehash: ff7b15a36c5ef19a1fa6ffdca7697dd6ba97c29f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87092375"
---
# <a name="azure-hpc-cache-data-ingest---parallel-copy-script-method"></a>Azure HPC önbellek verileri alma-paralel kopyalama betiği yöntemi

Bu makalede, ``parallelcp`` betiği oluşturma ve Azure HPC önbelleğiyle kullanılmak üzere verileri bir BLOB depolama kapsayıcısına taşımak için kullanılan yönergeler sunulmaktadır.

Azure HPC önbelleğiniz için verileri blob depolamaya taşıma hakkında daha fazla bilgi edinmek için [Azure Blob depolama 'ya veri taşıma](hpc-cache-ingest.md)makalesini okuyun.

## <a name="create-the-parallelcp-script"></a>Parallelcp betiği oluşturma

Aşağıdaki komut dosyası yürütülebilir dosyayı ekleyecek `parallelcp` . (Bu betik Ubuntu için tasarlanmıştır; başka bir dağıtım kullanılıyorsa ayrı olarak ' yi yüklemelisiniz ``parallel`` .)

```bash
sudo touch /usr/bin/parallelcp && sudo chmod 755 /usr/bin/parallelcp && sudo sh -c "/bin/cat >/usr/bin/parallelcp" <<EOM
#!/bin/bash

display_usage() {
    echo -e "\nUsage: \$0 SOURCE_DIR DEST_DIR\n"
}

if [  \$# -le 1 ] ; then
    display_usage
    exit 1
fi

if [[ ( \$# == "--help") ||  \$# == "-h" ]] ; then
    display_usage
    exit 0
fi

SOURCE_DIR="\$1"
DEST_DIR="\$2"

if [ ! -d "\$SOURCE_DIR" ] ; then
    echo "Source directory \$SOURCE_DIR does not exist, or is not a directory"
    display_usage
    exit 2
fi

if [ ! -d "\$DEST_DIR" ] && ! mkdir -p \$DEST_DIR ; then
    echo "Destination directory \$DEST_DIR does not exist, or is not a directory"
    display_usage
    exit 2
fi

if [ ! -w "\$DEST_DIR" ] ; then
    echo "Destination directory \$DEST_DIR is not writeable, or is not a directory"
    display_usage
    exit 3
fi

if ! which parallel > /dev/null ; then
    sudo apt-get update && sudo apt install -y parallel
fi

DIRJOBS=225
JOBS=225
find \$SOURCE_DIR -mindepth 1 -type d -print0 | sed -z "s/\$SOURCE_DIR\///" | parallel --will-cite -j\$DIRJOBS -0 "mkdir -p \$DEST_DIR/{}"
find \$SOURCE_DIR -mindepth 1 ! -type d -print0 | sed -z "s/\$SOURCE_DIR\///" | parallel --will-cite -j\$JOBS -0 "cp -P \$SOURCE_DIR/{} \$DEST_DIR/{}"
EOM
```

## <a name="parallel-copy-example"></a>Paralel kopya örneği

Bu örnekte, ``glibc`` Azure HPC önbelleğinde kaynak dosyaları kullanarak derlemek için paralel kopyalama betiği kullanılmaktadır.

Kaynak dosyalar Azure HPC önbellek bağlama noktasında önbelleğe alınır ve nesne dosyaları yerel sabit sürücüde depolanır.

Bu örnek, paralel kopyalama betiğini seçeneğiyle ``-j`` ve ``make`` paralelleştirme sağlamak için kullanır.

```bash
sudo apt-get update
sudo apt install -y gcc bison gcc binutils make parallel
cd
wget https://mirrors.kernel.org/gnu/libc/glibc-2.27.tar.bz2
tar jxf glibc-2.27.tar.bz2
ln -s /nfs/cache1 hpccache
time parallelcp glibc-2.27 avere/glibc-2.27
cd
mkdir obj
mkdir usr
cd obj
/home/azureuser/avere/glibc-2.27/configure --prefix=/home/azureuser/usr
time make -j
```

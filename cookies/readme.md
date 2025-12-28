# Печеньки
```shell
fdisk -l SD.dd
```

```log
Disk SD.dd: 1.86 GiB, 2000020480 bytes, 3906290 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 500906BD-DA74-4F82-ADC4-804BEC708564

Device       Start     End Sectors  Size Type
SD.dd1          40 2929479 2929440  1.4G Windows recovery environment
SD.dd2     2929536 3482495  552960  270M Microsoft basic data
SD.dd3     3483648 3903487  419840  205M Microsoft basic data
```

Просмотр содержимого разделов
```shell
sudo fsstat -o 40 SD.dd # apfs
sudo fsstat -o 2929536 SD.dd # fat32
sudo fsstat -o 3483648 SD.dd # ext3
```

## Поиск по всему образу
* Поиск последовательности байтов, которая в кодировке cp1251 значит 'еченьк'
```shell
grep -aF "$(echo -n 'еченьк' | iconv -t cp1251)" SD.dd 
```
Найденые вхождения:
* �UR� �N�T�vgWNj������������ � 8
* for <��������4@mail.ru>; Thu, 5 Aug 2021 11:51:19 +0000 
* Adobe Photoshop CC 2019 (Windows)2019:01:31 14:59:37�������� �14ࠠ@
* Я Печенька № 15 и я последняя в списке!

### Печенька №7
* Найдена в свободном пространстве
```shell
echo -n 'печенька № 7' | iconv -t cp1251 
```
* Закодированная строка
```log
efe5f7e5edfceae020b92037
```
* Она же в свободном пространстве
```dump
77200000  ef e5 f7 e5 ed fc ea e0  20 b9 20 37 00 00 00 00  |........ . 7....|
77200010  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|
```


## APFS
Извлечение раздела в отдельный образ
```shell
dd if=SD.dd of=sd1.img bs=512 skip=40 count=2929440
file sd2.img
```

```log
sd1.img: Apple File System (APFS), blocksize 4096
```

### Печенька №2
* Примонтировать ФС с линукса неполучилось
* Открыл через R-Studio
* Поврежденный rar архив восстанавливается и открывается при помощи WinRar
* Внутри zip архив с паролем 321
* Внутри txt с печенькой

## FAT32
Извлечение раздела в отдельный образ
```shell
dd if=SD.dd of=sd2.img bs=512 skip=2929536 count=552960
file sd2.img
```

```log
sd2.img: DOS/MBR boot sector, code offset 0x58+2, OEM-ID "MSDOS5.0", sectors/cluster 8, reserved sectors 7126, Media descriptor 0xf8, sectors/track 63, heads 255, hidden sectors 2929536, sectors 552960 (volumes > 32 MB), FAT (32 bit), sectors/FAT 533, serial number 0x68887223, unlabeled
```

Монтирование системы
```shell
sudo mkdir -p /mnt/sd2
sudo mount sd2.img /mnt/sd2
sudo umount /mnt/sd2
```

### Печенька №1
* Белый текст в файле '????????????? ??????.doc'
* Найден поиском по тексту в LibreOffice Writer

### Печенька №4
* Ранее была найдена в образе
* Откроем файл сообщения в этой кодировке
* выберем строки с 'еченьк'
```shell
iconv -f cp1251 -t
```
* Печенька в адресе электронной почты
```log
Delivered-To: печенька№4@mail.ru
        for печенька№4@mail.ru; Thu, 05 Aug 2021 14:51:42 +0300
Received: by smtp66.emlone.com id h1f8cs2erpkt for <печенька№4@
5481@emluni.com>)
To: печенька№4@mail.ru
```

### Печенька №6
* Через R-Studio восстановлен удаленный файл "Вот я был и вот меня не стало"
* Содержимое - печенька

## EXT3
Извлечение раздела в отдельный образ
```shell
dd if=SD.dd of=sd3.img bs=512 skip=3483648 count=419840
file sd3.img
```

```log
sd3.img: Linux rev 1.0 ext3 filesystem data, UUID=07022dc5-653b-c47a-1308-1c784d0080a8, volume name "apfs"
```

Монтирование системы
```shell
sudo mkdir -p /mnt/sd3
sudo mount sd3.img /mnt/sd3
sudo umount /mnt/sd3
```

### Печенька №15
```shell
grep -r 'ече' /mnt/sd3
```

```log
Эгегей! Я здесь.txt:1:Я Печенька № 15 и я последняя в списке!
```

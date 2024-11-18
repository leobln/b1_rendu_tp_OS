# I. Programme minimal



🌞 **Retrouvez à l'aide de `readelf` l'architecture pour laquelle le programme est compilé**

- vous devriez voir que c'est un ELF destiné à être exécuté par un *CPU* 32-bit

```
leobln@testtoto:~/work$ readelf -h hello1
ELF Header:
  Magic:   7f 45 4c 46 01 01 01 00 00 00 00 00 00 00 00 00
  Class:                             ELF32
  Data:                              2's complement, little endian
  Version:                           1 (current)
  OS/ABI:                            UNIX - System V
  ABI Version:                       0
  Type:                              DYN (Position-Independent Executable file)
  Machine:                           Intel 80386
  Version:                           0x1
  Entry point address:               0x1000
  Start of program headers:          52 (bytes into file)
  Start of section headers:          13308 (bytes into file)
  Flags:                             0x0
  Size of this header:               52 (bytes)
  Size of program headers:           32 (bytes)
  Number of program headers:         11
  Size of section headers:           40 (bytes)
  Number of section headers:         21
  Section header string table index: 20
```

🌞 **Afficher la liste des *sections* contenues dans le *programme***

- on devrait notamment y voir une section `.text` qui est celle qui contient le code assembleur du *programme*

```
leobln@testtoto:~/work$ readelf -S hello1
There are 21 section headers, starting at offset 0x33fc:

Section Headers:
  [Nr] Name              Type            Addr     Off    Size   ES Flg Lk Inf Al
  [ 0]                   NULL            00000000 000000 000000 00      0   0  0
  [ 1] .interp           PROGBITS        00000194 000194 000013 00   A  0   0  1
  [ 2] .note.gnu.bu[...] NOTE            000001a8 0001a8 000024 00   A  0   0  4
  [ 3] .gnu.hash         GNU_HASH        000001cc 0001cc 000018 04   A  4   0  4
  [ 4] .dynsym           DYNSYM          000001e4 0001e4 000010 10   A  5   1  4
  [ 5] .dynstr           STRTAB          000001f4 0001f4 000001 00   A  0   0  1
  [ 6] .text             PROGBITS        00001000 001000 00005d 00  AX  0   0  1
  [ 7] .eh_frame_hdr     PROGBITS        00002000 002000 00001c 00   A  0   0  4
  [ 8] .eh_frame         PROGBITS        0000201c 00201c 000058 00   A  0   0  4
  [ 9] .dynamic          DYNAMIC         00003f8c 002f8c 000068 08  WA  5   0  4
  [10] .got.plt          PROGBITS        00003ff4 002ff4 00000c 04  WA  0   0  4
  [11] .comment          PROGBITS        00000000 003000 00001f 01  MS  0   0  1
  [12] .debug_aranges    PROGBITS        00000000 00301f 000020 00      0   0  1
  [13] .debug_info       PROGBITS        00000000 00303f 000070 00      0   0  1
  [14] .debug_abbrev     PROGBITS        00000000 0030af 000062 00      0   0  1
  [15] .debug_line       PROGBITS        00000000 003111 000054 00      0   0  1
  [16] .debug_str        PROGBITS        00000000 003165 000085 01  MS  0   0  1
  [17] .debug_line_str   PROGBITS        00000000 0031ea 00001b 01  MS  0   0  1
  [18] .symtab           SYMTAB          00000000 003208 0000b0 10     19   6  4
  [19] .strtab           STRTAB          00000000 0032b8 00006a 00      0   0  1
  [20] .shstrtab         STRTAB          00000000 003322 0000d9 00      0   0  1
Key to Flags:
  W (write), A (alloc), X (execute), M (merge), S (strings), I (info),
  L (link order), O (extra OS processing required), G (group), T (TLS),
  C (compressed), x (unknown), o (OS specific), E (exclude),
  D (mbind), p (processor specific)
  ```

🌞 **Affichez le code *assembleur* de la section `.text` à l'aide d'`objdump`**

```
leobln@testtoto:~/work$ objdump -d hello1

hello1:     file format elf32-i386


Disassembly of section .text:

00001000 <_start>:
    1000:       55                      push   %ebp
    1001:       89 e5                   mov    %esp,%ebp
    1003:       57                      push   %edi
    1004:       56                      push   %esi
    1005:       53                      push   %ebx
    1006:       83 ec 10                sub    $0x10,%esp
    1009:       e8 4b 00 00 00          call   1059 <__x86.get_pc_thunk.ax>
    100e:       05 e6 2f 00 00          add    $0x2fe6,%eax
    1013:       c7 45 e5 48 65 6c 6c    movl   $0x6c6c6548,-0x1b(%ebp)
    101a:       c7 45 e9 6f 2c 20 57    movl   $0x57202c6f,-0x17(%ebp)
    1021:       c7 45 ed 6f 72 6c 64    movl   $0x646c726f,-0x13(%ebp)
    1028:       c7 45 f0 64 21 0a 00    movl   $0xa2164,-0x10(%ebp)
    102f:       8d 75 e5                lea    -0x1b(%ebp),%esi
    1032:       bf 0e 00 00 00          mov    $0xe,%edi
    1037:       b8 04 00 00 00          mov    $0x4,%eax
    103c:       bb 01 00 00 00          mov    $0x1,%ebx
    1041:       89 f1                   mov    %esi,%ecx
    1043:       89 fa                   mov    %edi,%edx
    1045:       cd 80                   int    $0x80
    1047:       b8 01 00 00 00          mov    $0x1,%eax
    104c:       31 db                   xor    %ebx,%ebx
    104e:       cd 80                   int    $0x80
    1050:       90                      nop
    1051:       83 c4 10                add    $0x10,%esp
    1054:       5b                      pop    %ebx
    1055:       5e                      pop    %esi
    1056:       5f                      pop    %edi
    1057:       5d                      pop    %ebp
    1058:       c3                      ret

00001059 <__x86.get_pc_thunk.ax>:
    1059:       8b 04 24                mov    (%esp),%eax
    105c:       c3                      ret
```

## 2. Librairie et compilation dynamique



🌞 **Tracez à l'aide de la commande `ldd` les *librairies* appelées par...**

- le programme `hello2`

```
leobln@testtoto:~/work$ ldd hello2
        linux-gate.so.1 (0xf7f59000)
        libc.so.6 => /lib32/libc.so.6 (0xf7d16000)
        /lib/ld-linux.so.2 (0xf7f5b000)
```

- puis le programme `hello1`

```
leobln@testtoto:~/work$ ldd hello1
        statically linked
```

- puis `ls`
  - vérifiez que le fichier `ls` que vous analysez est bien un *programme* ELF
  - toujours la *commande* `file` pour voir le type d'un fichier

```
leobln@testtoto:~/work$ file /bin/ls
/bin/ls: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=15dfff3239aa7c3b16a71e6b2e3b6e4009dab998, for GNU/Linux 3.2.0, stripped
leobln@testtoto:~/work$ ldd /bin/ls
        linux-vdso.so.1 (0x00007ffd9ab9c000)
        libselinux.so.1 => /lib/x86_64-linux-gnu/libselinux.so.1 (0x00007fbc35c9b000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fbc35aba000)
        libpcre2-8.so.0 => /lib/x86_64-linux-gnu/libpcre2-8.so.0 (0x00007fbc35a20000)
        /lib64/ld-linux-x86-64.so.2 (0x00007fbc35d01000)
```

- puis `firefox` (vous l'avez installé au TP1 normalement !)
  - vérifiez que le fichier `firefox` que vous analysez est bien un un *programme* ELF

```
leobln@testtoto:~/work$ file /usr/bin/firefox
/usr/bin/firefox: POSIX shell script, ASCII text executable
leobln@testtoto:~/work$ ldd /usr/bin/firefox
        not a dynamic executable
```

🌞 **Parmi les *librairies* appelées par *hello2*, déterminer le type du fichier nommé `libc.so.X`**


## 3. Compilation statique

🌞 **Affichez le type des fichiers `hello2` et `hello3`**

- vous devriez voir une différence explicite
- une différence qui indique que l'un des deux est compilé statiquement
- et l'autre dynamiquement

```
leobln@testtoto:~/work$ file hello2
hello2: ELF 32-bit LSB pie executable, Intel 80386, version 1 (SYSV), **dynamically linked**, interpreter /lib/ld-linux.so.2, BuildID[sha1]=7bf77b8a069e05d26038ab8df5b253337dadd579, for GNU/Linux 3.2.0, with debug_info, not stripped
leobln@testtoto:~/work$ file hello3
hello3: ELF 32-bit LSB executable, Intel 80386, version 1 (GNU/Linux), **statically linked**, BuildID[sha1]=7fe0673e0ea721050851b32434cfbee5af8b0fdc, for GNU/Linux 3.2.0, with debug_info, not stripped
```

🌞 **Affichez leurs tailles**

- avec une *commande* `du`
- vous pouvez ajouter l'option `-h` pour afficher les tailles avec des K, M, G etc (`-h` pour *Human readable*)

```
leobln@testtoto:~/work$ du -h hello2
16K     hello2
leobln@testtoto:~/work$ du -h hello3
728K    hello3
```

## 4. Compilation cross-platform

Comme vu dans le cours, chaque *CPU* (ou chaque *architecture* de *CPU*) a son propre langage : on appelle ça l'*architecture* du *CPU*.

Quand vous compilez avec GCC sur votre *machine*, vous compilez en réalité pour l'*architecture* de votre *CPU* (celui de la *machine* depuis laquelle vous compilez).

➜ **On va maintenant compiler notre *programme* pour une autre *architecture*.**

Pour cela, on va d'abord vérifier l'*architecture* de votre *machine* Linux.

🌞 **Affichez l'*architecture* de votre *CPU***

- ça se fait avec la commande `lscpu`
- mais en vrai, il existe un fichier dédié qui contient toutes les infos relatives à votre *CPU*, c'est le fichier `/proc/cpuinfo`
- il suffit de le `cat` !
- s'il y a plusieurs blocs, c'est pour chacun des coeurs de votre *CPU*

Pour compiler un *programme* pour d'autres *architectures*, il va nous falloir utiliser d'autres versions de GCC, vous pouvez vérifier si vous avez la *commande* `x86-64-linux-gnu-gcc`. Celle-ci permet de compiler en *architecture* *x86_64*, l'*architecture* la plus courante.

🌞 **Vérifiez que vous avez la commande `x86-64-linux-gnu-gcc`**

- prouvez-le avec la *commande* de votre choix

🌞 **Compilez votre fichier `hello3.c` dans un fichier cible nommé `hello4` vers une autre *architecture* que la vôtre**

- vous devez donc trouver une autre commande `gcc` que `x86-64-linux-gnu-gcc` qui est en réalité celle que vous utilisez depuis le début
- par exemple, trouvez une *commande* `gcc` qui permet de compiler vers une *architecture* *ARM*

> *ARM* c'est l'*architecture* utilisée par les CPU des téléphones, des aspirateurs connectés et des Mac M1, M2 et M3, des tests de grossesse, entre un million d'autres exemples. Du moment que ça doit être petit, embarqué, et pas consommer beaucoup d'énergie, ce sera souvent une *architecture* *ARM*.

🌞 **[Désassemblez](../../cours/memo/glossary.md#désassembler) `hello3` et `hello4` à l'aide d'`objdump`**

- vous devriez constater que malgré le même *programme* C d'entrée, le contenu des deux *programmes* une fois compilés est bien différent

🌞 **Essayez d'exécuter le *programme* `hello4`**

- sproutch !
- should NOT work


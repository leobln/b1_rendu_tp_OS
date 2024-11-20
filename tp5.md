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

```
leobln@testtoto:~/work$ du -h hello2
16K     hello2
leobln@testtoto:~/work$ du -h hello3
728K    hello3
```

## 4. Compilation cross-platform

🌞 **Affichez l'*architecture* de votre *CPU***

- ça se fait avec la commande `lscpu`
- mais en vrai, il existe un fichier dédié qui contient toutes les infos relatives à votre *CPU*, c'est le fichier `/proc/cpuinfo`
- il suffit de le `cat` !
- s'il y a plusieurs blocs, c'est pour chacun des coeurs de votre *CPU*

```
leobln@testtoto:~/work$ lscpu
Architecture:             x86_64
  CPU op-mode(s):         32-bit, 64-bit
  Address sizes:          39 bits physical, 48 bits virtual
  Byte Order:             Little Endian
CPU(s):                   1
  On-line CPU(s) list:    0
Vendor ID:                GenuineIntel
  Model name:             13th Gen Intel(R) Core(TM) i7-1355U
    CPU family:           6
    Model:                186
    Thread(s) per core:   1
    Core(s) per socket:   1
    Socket(s):            1
    Stepping:             3
    BogoMIPS:             5222.39
    Flags:                fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca
                           cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx
                          rdtscp lm constant_tsc rep_good nopl xtopology nonstop_t
                          sc cpuid tsc_known_freq pni pclmulqdq ssse3 cx16 sse4_1
                          sse4_2 movbe popcnt aes rdrand hypervisor lahf_lm abm 3d
                          nowprefetch ibrs_enhanced fsgsbase bmi1 bmi2 invpcid rds
                          eed adx clflushopt sha_ni arat md_clear flush_l1d arch_c
                          apabilities
Virtualization features:
  Hypervisor vendor:      KVM
  Virtualization type:    full
Caches (sum of all):
  L1d:                    48 KiB (1 instance)
  L1i:                    32 KiB (1 instance)
  L2:                     1.3 MiB (1 instance)
  L3:                     12 MiB (1 instance)
NUMA:
  NUMA node(s):           1
  NUMA node0 CPU(s):      0
Vulnerabilities:
  Gather data sampling:   Not affected
  Itlb multihit:          Not affected
  L1tf:                   Not affected
  Mds:                    Not affected
  Meltdown:               Not affected
  Mmio stale data:        Not affected
  Reg file data sampling: Vulnerable: No microcode
  Retbleed:               Mitigation; Enhanced IBRS
  Spec rstack overflow:   Not affected
  Spec store bypass:      Vulnerable
  Spectre v1:             Mitigation; usercopy/swapgs barriers and __user pointer
                          sanitization
  Spectre v2:             Mitigation; Enhanced / Automatic IBRS; RSB filling; PBRS
                          B-eIBRS SW sequence; BHI SW loop, KVM SW loop
  Srbds:                  Not affected
  Tsx async abort:        Not affected
leobln@testtoto:~/work$ cat /proc/cpuinfo
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 186
model name      : 13th Gen Intel(R) Core(TM) i7-1355U
stepping        : 3
microcode       : 0xffffffff
cpu MHz         : 2611.196
cache size      : 12288 KB
physical id     : 0
siblings        : 1
core id         : 0
cpu cores       : 1
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 22
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq ssse3 cx16 sse4_1 sse4_2 movbe popcnt aes rdrand hypervisor lahf_lm abm 3dnowprefetch ibrs_enhanced fsgsbase bmi1 bmi2 invpcid rdseed adx clflushopt sha_ni arat md_clear flush_l1d arch_capabilities
bugs            : spectre_v1 spectre_v2 spec_store_bypass swapgs retbleed eibrs_pbrsb rfds bhi
bogomips        : 5222.39
clflush size    : 64
cache_alignment : 64
address sizes   : 39 bits physical, 48 bits virtual
power management:
```

🌞 **Vérifiez que vous avez la commande `x86-64-linux-gnu-gcc`**

- prouvez-le avec la *commande* de votre choix

```
leobln@testtoto:~/work$ which x86_64-linux-gnu-gcc
/usr/bin/x86_64-linux-gnu-gcc
```

🌞 **Compilez votre fichier `hello3.c` dans un fichier cible nommé `hello4` vers une autre *architecture* que la vôtre**

- vous devez donc trouver une autre commande `gcc` que `x86-64-linux-gnu-gcc` qui est en réalité celle que vous utilisez depuis le début
- par exemple, trouvez une *commande* `gcc` qui permet de compiler vers une *architecture* *ARM*

```
leobln@testtoto:~/work$ arm-linux-gnueabihf-gcc hello3.c -o hello4
```

🌞 **[Désassemblez](../../cours/memo/glossary.md#désassembler) `hello3` et `hello4` à l'aide d'`objdump`**

- vous devriez constater que malgré le même *programme* C d'entrée, le contenu des deux *programmes* une fois compilés est bien différent

```
leobln@testtoto:~/work$ objdump -d hello3
[...]
leobln@testtoto:~/work$ arm-linux-gnueabihf-objdump -d hello4
```

🌞 **Essayez d'exécuter le *programme* `hello4`**

- sproutch !
- should NOT work

```
leobln@testtoto:~/work$ ./hello4
-bash: ./hello4: cannot execute binary file: Exec format error
```


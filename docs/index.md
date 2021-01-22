# 이것은 docs/ 아래의 index.md 파일입니다.

## 도입

Linux 0.01 소스 코드를 살펴보자.
이유는 아래 링크에서 커널 입문을 위해 추천하는 방식이기 때문이다.
<https://kernelnewbies.org/CompleteNewbiesClickHere>

## Linux 0.01 소스 코드

아래 링크에서 소스 코드 다운로드가 가능하다. 
<https://mirrors.edge.kernel.org/pub/linux/kernel/Historic/>

## 참고 자료

* Linux 0.01 source code browse
  * <https://elixir.bootlin.com/linux/0.01/source>
* Linux 0.01 Release Notes
  * <http://gunkies.org/wiki/Linux_0.01#Release_Notes>
* Intel 80386 programmer's reference manual
  * <https://css.csail.mit.edu/6.858/2014/readings/i386.pdf>
* Intel 80386 wiki
  * <https://en.wikipedia.org/wiki/Intel_80386>

## boot/boot.s

```
  1 |
  2 |	boot.s
  3 |
  4 | boot.s is loaded at 0x7c00 by the bios-startup routines, and moves itself
  5 | out of the way to address 0x90000, and jumps there.
  6 |
  7 | It then loads the system at 0x10000, using BIOS interrupts. Thereafter
  8 | it disables all interrupts, moves the system down to 0x0000, changes
  9 | to protected mode, and calls the start of system. System then must
 10 | RE-initialize the protected mode in it's own tables, and enable
 11 | interrupts as needed.
```

위 내용을 정리하면 아래와 같다.

* BIOS 시작 루틴에 의해 boot.s는 0x7C00에 로드된다.
* boot.s 스스로가 0x90000 번지로 복사되고 그곳으로 jump 한다.
* 그리고 BIOS interrupt를 사용하여 시스템이 0x10000에 로드한다.
* 모든 interrupt를 disable하고, 시스템을 0x0000으로 옮기고 protected mode로 바꾼다. 그리고 시스템의 시작을 call한다.
* 시스템은 protected mode를 자신의 테이블에 RE-initialize 해야 한다.
* 필요에 따라 interrupt를 enable한다.

<br>

```
 12 |
 13 | NOTE! currently system is at most 8*65536 bytes long. This should be no
 14 | problem, even in the future. I want to keep it simple. This 512 kB
 15 | kernel size should be enough - in fact more would mean we'd have to move
 16 | not just these start-up routines, but also do something about the cache-
 17 | memory (block IO devices). The area left over in the lower 640 kB is meant
 18 | for these. No other memory is assumed to be "physical", ie all memory
 19 | over 1Mb is demand-paging. All addresses under 1Mb are guaranteed to match
 20 | their physical addresses.
 21 |
```
???

<br>

```
 22 | NOTE1 abouve is no longer valid in it's entirety. cache-memory is allocated
 23 | above the 1Mb mark as well as below. Otherwise it is mainly correct.
 24 |
 25 | NOTE 2! The boot disk type must be set at compile-time, by setting
 26 | the following equ. Having the boot-up procedure hunt for the right
 27 | disk type is severe brain-damage.
 28 | The loader has been made as simple as possible (had to, to get it
 29 | in 512 bytes with the code to move to protected mode), and continuos
 30 | read errors will result in a unbreakable loop. Reboot by hand. It
 31 | loads pretty fast by getting whole sectors at a time whenever possible.
 32
```



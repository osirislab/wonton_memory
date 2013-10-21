A vdb script usefull for finding connections in memory.
The script will write a .dot file which you can use to draw
the graph of everything touchable from a given pointer.

example usage:

wont@wont-virtual-machine:~/wonton_memory$ ls
readme  wonton_memory

wont@wont-virtual-machine:~/wonton_memory$ cat /home/wont/Shellcode/tcb_research/gs.s 
;; Evan Jensen
;; 101413

;; usefull for enumerating runtime datastructures
;; use in conjuction with the enumeration vdb script
BITS 32
extern printf
global main


					   
main:

	mov eax,DWORD [gs:0]
	int3

wont@wont-virtual-machine:~/wonton_memory$ vdb 

vdb > exec /home/wont/Shellcode/tcb_research/tcb

vdb > Attached to : 7040
Loading Binary: /home/wont/Shellcode/tcb_research/tcb
Loading Binary: /lib/i386-linux-gnu/ld-2.15.so
Loading Binary: [vdso]
Thread: 7040 NOTIFY_BREAK
go
Running Tracer (use 'break' to stop it)
vdb > Process Recieved Signal 5 (0x00000005) (Thread: 7040 (0x00001b80))


basics:
EOF         bp      detach  guid    memdiff     quit     server    struct  
alias       bpedit  dis     help    memdump     recon    signal    suspend 
alloc       bpfile  dope    ignore  memload     reg      snapshot  syms    
attach      break   eval    lm      memprotect  restart  stalker   threads 
autocont    bt      exec    maps    meta        resume   status    var     
autoscript  call    fds     mem     mode        saveout  stepi     vstruct 
bestname    clear   go      memcmp  ps          script   stepo     waitlib 
binstr      config  gui     memcpy  python      search   stepout   writemem

amd64:
eflags


vdb > eval rax
rax = 0xf76e1900

vdb > script wonton_memory
usage: wonton_memory [-h] [--search Root_Addr Size Output_prefix]
                     [--path Start_addr End_Addr]

Look for connections in memory

optional arguments:
  -h, --help            show this help message and exit
  --search Root_Addr Size Output_prefix, -s Root_Addr Size Output_prefix
                        Search memory starting from root with each "struct"
                        being n bytes long
  --path Start_addr End_Addr, -p Start_addr End_Addr
                        Find a path between the two nodes. Run after search

Written by wont for your pleasure.

vdb > script wonton_memory -s 0xf76e1900 0x88 example
Caught exception: reading from invalid memory 0xffbb9f7dL (131 returned)
makePicture
writing example.1382386112.06.dot
writing example.1382386112.06.pickle
finished in 47.137748003 seconds

vdb > script wonton_memory -p 0xf76e1900 0x8048000
5 paths:
Path 1
========================================
0xf76e1900 0x0
0xffbb7914 0x80
0x41020ff4 0x38
0x41021a74 0x34
0x8048000 0x50
Path 2
========================================
0xf76e1900 0x0
0xffbb7914 0x80
0x41020ff4 0x38
0x41021a74 0x34
0x8048000 0x50
0x8048000 0x7c
Path 3
========================================
0xf76e1900 0x0
0xffbb7914 0x80
0x41020ff4 0x38
0x41021a74 0x34
0x8048000 0x50
0x8048000 0x80
Path 4
========================================
0xf76e1900 0x0
0xffbb7914 0x80
0x41020ff4 0x38
0x41021a74 0x34
0x8048000 0x50
0x8048034 0x3c
0x8048000 0x48
Path 5
========================================
0xf76e1900 0x0
0xffbb7914 0x80
0x41020ff4 0x38
0x41021a74 0x34
0x8048000 0x50
0x8048034 0x3c
0x8048000 0x4c
vdb > quit

wont@wont-virtual-machine:~/wonton_memory$ dot -Tjpeg -O *dot
dot: graph is too large for cairo-renderer bitmaps. Scaling by 0.396134 to fit

wont@wont-virtual-machine:~/wonton_memory$ ls
example.1382386112.06.dot       example.1382386112.06.pickle  wonton_memory
example.1382386112.06.dot.jpeg  readme



